# Python DLMS

该库是基于开源的C-DLMS库进行开发的，并且支持 Client / Server 模式。

支持 DLMS 功能的模组型号如下：

| 系列   | 型号                                                         |
| :----- | :----------------------------------------------------------- |
| EC200U | EC200UCN_AA_DLMS |

> 💡 **Tips**
> - 本库支持Client/Server模式，适用于智能电表等物联网设备。
> - 商业应用请走商务流程。
 
## DLMS `COSEM` Objects

### `COSEM` Objects 共有属性和方法
#### `Event hooks`
dlms 模块中的每个对象都是某个子类的实例，为每个对象添加了以下功能：
六个属性（CosemObject，基类）  
`on_before_read、on_after_read、on_before_write、on_after_write、on_before_action、on_after_action`，每个对象都可以设置为一个回调函数。

```python
import dlms
# 使用clock对象+on_before_read举例子
#on_after_read、on_before_write、on_after_write、on_before_action、on_after_action也类似
clock = dlms.Clock("0.0.1.0.0.255")

def on_before_read_clock(self, event):
    if event.index == 2:  # attr 2 = time
        print("[DLMS] Client is reading clock.time")
        # 返回 True 或 None → 允许读取（C 层已从 RTC 刷新了时间）
        return True
        
    elif event.index == 3:  # attr 3 = time_zone
        print("[DLMS] Client is reading clock.time_zone")
        return True
    
    return True  # 默认允许

# 绑定 handler 在client/server通信中才能触发
clock.on_before_read = on_before_read_clock
```

#### `Per-instance access control`
`access_dict` 属性接受一个与 `access` 构造器参数格式相同的访问字典，并会覆盖该特定对象实例的类级别默认值。

每个对象都有一个默认的访问字典，该字典由 set_default_access 函数设置。
```python
import dlms
# 使用Clock对象举例子，因为每个对象都可以设置access_dict属性，所以可以针对每个对象设置不同的访问字典
# 类级别：所有 Clock 默认客户端只读
dlms.set_default_access(dlms.Clock, {
    2: (dlms.AccessMode.READ, dlms.Authentication.NONE),   # time 只读
    3: (dlms.AccessMode.READ, dlms.Authentication.NONE),   # time_zone 只读
})

# 两个 Clock 对象
clk_a = dlms.Clock("0.0.1.0.0.255")  # 允许客户端只读
clk_b = dlms.Clock("0.0.2.0.0.255")  # 允许客户端只读

# 但 clk_b 需要允许 HLS 客户端同步时间 → 实例级别覆盖
clk_b.access_dict = {
    2: (dlms.AccessMode.READ_WRITE, dlms.Authentication.HIGH),  # time 可写
}
```
#### `obj.idx(name)+obj.attr_name(index)`
每个对象都可以使用这两种方法
```python
import dlms
#Clock对象使用obj.idx和obj.attr_name举例子，每个对象都可以这样参考clock的例子来写
# 1. 创建 Clock 对象
clock = dlms.Clock("0.0.1.0.0.255", time_zone=8, deviation=0)
print("=== COSEM Objects ===")
print(clock)
print()
# 2. idx(name) — Python 名 → DLMS 编号
print("clock.idx('time')        = {}".format(clock.idx('time')))         # 2
print("clock.idx('time_zone')   = {}".format(clock.idx('time_zone')))    # 3
print("clock.idx('status')      = {}".format(clock.idx('status')))       # 4
print("clock.idx('begin')       = {}".format(clock.idx('begin')))        # 5
print("clock.idx('end')         = {}".format(clock.idx('end')))          # 6
print("clock.idx('deviation')   = {}".format(clock.idx('deviation')))    # 7
print("clock.idx('enabled')     = {}".format(clock.idx('enabled')))      # 8
print("clock.idx('base')        = {}".format(clock.idx('base')))         # 9
# 验证：不存在的名字会抛异常
print()
try:
    clock.idx('foobar')
except Exception as e:
    print("clock.idx('foobar') throw exception: {}".format(type(e).__name__, e))

# 3. attr_name(index) — DLMS 编号 → Python 名
print()
print("=== clock.attr_name() — DLMS Number → Python attr ===")
for i in range(2, 10):
    print("clock.attr_name({}) = '{}'".format(i, clock.attr_name(i)))

# 4. attrs() — 可持久化属性列表（跳过 VOLATILE）
print()
print("=== clock.attrs() — Persistent attribute list ===")
attrs_list = clock.attrs()
print("backup {} attributes:".format(len(attrs_list)))
for index, name in attrs_list:
    print("clock.attr_name({}) = '{}'".format(index, name))

print()
print("note: (2, 'time') had been skipped, because it's DLMS_ATTR_VOLATILE")

# 5. 实际使用：用 idx() 配合 client.read()
print()
print("=== real situation ===")
print("example client.read(clock, 2):")
print("  client.read(clock, clock.idx('time'))")
print("  client.read(clock, clock.idx('time_zone'))")
print("This way, the code becomes more readable and there is no need to remember the Blue Book numbers.")
```

`obj.idx(name)+obj.attr_name(index) 实际应用`

在事件处理程序和客户端代码中使用这些方法，可以避免硬编码魔术数字，从而无需查阅蓝皮书进行解码。例如，所以clock.idx('time') 返回  client.read(clock, clock.idx('time')) 等价于  client.read(clock, 2)，但具有自文档化特性。  obj.attrs() 返回一个包含所有复杂属性（如 (attr_index, attr_name) 元组）的列表，这些属性将被 BinarySerializer 保存。不包括瞬态属性（如 Clock.time 和 ProfileGeneric.buffer）。该列表在构建自定义序列化器时尤为有用。
```python
import dlms #该例子不完整，仅供说明参考
# × 没有 idx()：必须记住每个属性的 Blue Book 编号,
client.read(clock, 2)       # 2 是什么？time？time_zone？
client.write(clock, 7, 60)  # 7 是什么？deviation？enabled？

# √ 有 idx()：用 Python 属性名，自文档化
client.read(clock, clock.idx('time'))
client.write(clock, clock.idx('deviation'), 60)
```
---
### `Data`
DLMS 中最通用的数据容器。可存储**任意类型**（整数、字符串、字节、布尔值、空值，甚至引用其他对象的属性），其**COSEM ID=1** 。

`Blue Book 属性`

| 编号 | 名称 | 类型 | 说明 |
|------|------|------|------|
| 1 | `logical_name` | octet-string(6) | OBIS 代码，只读 |
| 2 | `value` | CHOICE（任意类型） | 存储的实际数据 |

`构造函数`

```python
Data(logical_name: str, nocopy: bool = False, access: dict = None)
```
| 参数 | 类型 | 默认值 | 说明 |
|------|------|--------|------|
| `logical_name` | `str` | 必传 | OBIS 代码字符串，如 `"1.0.1.8.0.255"` |
| `nocopy` | `bool` | `False` | 若为 `True`，`value` 可设为 `(obj, attr_index)` 元组引用其他属性 |
| `access` | `dict` | `None` | 实例级权限，格式 `{attr_index: (AccessMode, Authentication)}` |


`支持的 value 类型`

```python
import dlms
#初始化Data对象
data = dlms.Data("1.0.1.8.0.255",nocopy=False,access={2: (dlms.AccessMode.READ_WRITE, dlms.Authentication.HIGH)})
#Data对象有logical_name和value属性
print(data.logical_name)  #输出“1.0.1.8.0.255”
print(data.value)         #输出None

#另一种输出Data对象的属性的方式
print(data)  # <Data ln='1.0.1.8.0.255', value=None>

#设置Data对象的value属性
data.value = 42           # 整数
print(data.value)         # 输出42

data.value = b"\x01\x02"  # 字节串
print(data.value)         # 输出b'\x01\x02' 

data.value = "hello"      # 字符串
print(data.value)         # 输出"hello"

data.value = True         # 布尔值
print(data.value)         # 输出True

data.value = None         # 设置为None
print(data.value)         # 输出None

```
**实时引用（BYREF）模式（`nocopy=True`）**

以 `nocopy=True` 构造 `Data` 对象，并将 `(target_object, attribute_index)` 元组赋值给 `value`，即可启用实时引用模式。此后当客户端读取该 `Data` 对象时，服务器会在读取的那一刻透明地从目标对象取回被引用属性的**当前值**，而不做任何复制。该模式最常见的用途是暴露位于 `SecuritySetup` 对象内部的调用计数器（invocation counter）。

```python
import dlms
data = dlms.Data("1.0.1.8.0.255",nocopy=True,access={2: (dlms.AccessMode.READ_WRITE, dlms.Authentication.HIGH)})
sec = dlms.SecuritySetup("0.0.43.0.2.255")
data.value = (sec, 6)       # 获取SecuritySetup对象的in_invocation_counter访问权
print(data.value)           # 输出sec.min_invocation_counter的值，开始未赋值，输出0

sec.min_invocation_counter = 100
print(data.value)           # 输出sec.min_invocation_counter的值，现在为100
```
on_before_write — 写前验证

`Python 属性一览`

| 属性 | 类型 | 可写 | 说明 |
|------|------|:--:|------|
| `logical_name` | `bytes` | × | OBIS 代码（6 字节） |
| `value` | `int \| bytes \| str \| bool \| None \| tuple` | √ | 数据值，支持引用模式 |
| `on_before_read` | `Callable` | √ | 读前钩子（继承自 CosemObject） |
| `on_after_read` | `Callable` | √ | 读后钩子（继承自 CosemObject） |
| `on_before_write` | `Callable` | √ | 写前钩子（继承自 CosemObject） |
| `on_after_write` | `Callable` | √ | 写后钩子（继承自 CosemObject） |
| `on_before_action` | `Callable` | √ | 动作前钩子（继承自 CosemObject） |
| `on_after_action` | `Callable` | √ | 动作后钩子（继承自 CosemObject） |
| `access_dict` | `dict` | √ | 实例级访问控制 |


`访问控制`

```python
import dlms

# 构造函数中指定
data = dlms.Data("1.0.1.8.0.255",access={2: (dlms.AccessMode.READ_WRITE, dlms.Authentication.HIGH)})
# 输出{2: (AccessMode.READ_WRITE, Authentication.HIGH)} 安全模式的返回值可看安全模型（Security）章节
print(data.access_dict)
# 或事后修改
data.access_dict = {2: (dlms.AccessMode.READ, dlms.Authentication.HIGH)}
 # 输出{2: (AccessMode.READ_WRITE, Authentication.HIGH)}  安全模式的返回值可看安全模型（Security）章节
print(data.access_dict) 

```
`典型用例`

| 场景 | 示例 |
|------|------|
| 存储电表序列号 | `data.value = 12345678` |
| 存储固件版本字符串 | `data.value = "v1.2.3"` |
| 存储自定义状态标志 | `data.value = True` |
| 事件通知的消息载体 | `data.value = "Tamper detected"` |
| BYREF 联动读写 | `data.value = (other_obj, 2)` |

---
### `Register`
DLMS 中表示**单一物理测量量**的标准对象。用于电能读数、瞬时功率、电压、电流及类似测量。内置 `scaler`（缩放因子）实现原始整数值到物理量的转换，`unit` 由 OBIS 代码自动派生，其**COSEM ID=3**。

`Blue Book 属性`

| Python 属性 | 类型 | DLMS 属性编号 | 说明 |
|------------|------|-------------|------|
| `logical_name` | str | 1 | OBIS 代码，只读 |
| `value` | int | 2 | 原始整数值，尚未应用 scaler |
| `scaler` | int | 3（部分） | 10 的幂次指数。|
| `unit` | int（只读） | 3（部分） | 物理单位，`Unit` 枚举值。由 OBIS 代码在构造时自动派生，不可修改 |

```python
import dlms
reg = dlms.Register(
    "1.0.1.8.0.255",
    0, 
    scaler=-3,
    access={2: (dlms.AccessMode.READ_WRITE, dlms.Authentication.HIGH)})
print(reg)               #输出<Register ln='1.0.1.8.0.255', value=0, scaler=-3, unit=Unit.ACTIVE_ENERGY>
#属性修改或者不可修改
print(reg.logical_name)  #输出“1.0.1.8.0.255” reg.logical_name初始化后不可修改
reg.value = 12345678
print(reg.value)         # 输出12345678
reg.scaler = -5
print(reg.scaler)        # 输出-5
print(reg.unit)          # 输出Unit.ACTIVE_ENERGY ,OBIS 代码在构造时自动派生，不可修改
```
`构造函数`

```python
Register(logical_name: str, default_value: int, scaler: int = 1, access: dict = None)
```
| 参数 | 类型 | 默认值 | 说明 |
|------|------|--------|------|
| `logical_name` | `str` | 必传 | OBIS 代码字符串，如 `"1.0.1.8.0.255"` |
| `default_value` | `int` | 必传 | 初始值，也是 `reset()` 恢复的目标值 |
| `scaler` | `int` | `1` | 10 的幂指数。`-3` 表示原始值 ÷ 1000 |
| `access` | `dict` | `None` | 实例级权限，格式 `{attr_index: (AccessMode, Authentication)}` |

`物理量转换公式`

```python
physical_value = raw_value * (10 ** scaler)
```

| scaler | 含义 | 示例 |
|--------|------|------|
| `0` | 原始值即物理值 | `value=100` → `100 kWh` |
| `-3` | 原始值 ÷ 1000 | `value=12345678` → `12345.678 kWh` |
| `3` | 原始值 × 1000 | `value=12` → `12000 kWh` |

`unit`（只读）由 OBIS 代码自动派生——例如 `1.0.1.8.0.255` 自动映射到 `Unit.ACTIVE_ENERGY`，无需手动设置。

```python
import dlms
reg = dlms.Register(
    "1.0.1.8.0.255", 
    0, 
    scaler=-3,
    access={2: (dlms.AccessMode.READ_WRITE, dlms.Authentication.HIGH)})
reg.value = 1234789
raw_value = reg.value * (10 ** reg.scaler) # 1234.789
```
| 方法 | COSEM 方法 | 说明 |
|------|-----------|------|
| `reset()` | 1 | 将 `value` 重置为构造时的 `default_value` |

如需 `reset()` 时同步清除硬件状态，使用 `on_before_action` 拦截：

```python
# 默认行为：C 层直接恢复 value → 0
# reg.reset() 该方法并未注册开到python库中，而是在通信时，触发before_reset回调执行复位
# 成功return ture则调用C层的reset方法，否则不调用C层的reset方法
import dlms
def before_reset(self, event):
    if event.index == 1:  # method 1 = reset
        # 此处可以调用硬件 API 清零外部计数器
        pass
    return True  # 允许执行

reg.on_before_action = before_reset
```

`Python 属性一览`

| 属性 | 类型 | 可写 | 说明 |
|------|------|:--:|------|
| `logical_name` | `bytes` | × | OBIS 代码（6 字节） |
| `value` | `int` | √ | 当前测量值（原始整数，未应用 scaler） |
| `scaler` | `int` | √ | 10 的幂指数，用于计算物理量 |
| `unit` | `int` | × | 物理单位（`Unit` 枚举），由 OBIS 代码自动派生，不可修改 |
| `on_before_read` | `Callable` | √ | 读前钩子（继承自 CosemObject） |
| `on_after_read` | `Callable` | √ | 读后钩子（继承自 CosemObject） |
| `on_before_write` | `Callable` | √ | 写前钩子（继承自 CosemObject） |
| `on_after_write` | `Callable` | √ | 写后钩子（继承自 CosemObject） |
| `on_before_action` | `Callable` | √ | 动作前钩子（继承自 CosemObject） |
| `on_after_action` | `Callable` | √ | 动作后钩子（继承自 CosemObject） |
| `access_dict` | `dict` | √ | 实例级访问控制 |

`访问控制`

```python
import dlms
# 构造函数中指定
reg = dlms.Register("1.0.1.8.0.255", 0, scaler=-3,
    access={2: (dlms.AccessMode.READ_WRITE, dlms.Authentication.HIGH)})

# 或事后修改
reg.access_dict = {2: (dlms.AccessMode.READ_WRITE, dlms.Authentication.HIGH)}
```

`典型用例`

| 场景 | 示例 |
|------|------|
| 总正向有功电能，scaler=-3 换算 kWh | `Register("1.0.1.8.0.255", 0, scaler=-3)` |
| 单相瞬时电压，scaler=-1 换算 V | `Register("1.0.32.7.0.255", 0, scaler=-1)` |
| 单相瞬时电流，scaler=-2 换算 A | `Register("1.0.31.7.0.255", 0, scaler=-2)` |
| 单相有功功率 | `Register("1.0.21.7.0.255", 0, scaler=0)` |


### `ExtendedRegister`
`Register` 的扩展版本。在 `value`、`scaler`、`unit` 之外增加了 `status`（状态码）和 `capture_time`（捕获时间戳）两个属性，用于记录测量值产生时的上下文信息，其**COSEM ID=4**。

`Blue Book 属性`

| 编号 | 名称 | 类型 | 说明 |
|------|------|------|------|
| 1 | `logical_name` | octet-string(6) | OBIS 代码，只读 |
| 2 | `value` | CHOICE | 当前测量值（支持多种数值类型） |
| 3 | `scaler` |  int |  10 的幂次指数。|
| 4 | `unit` | int |  物理单位，`Unit` 枚举值。由 OBIS 代码在构造时自动派生，不可修改 |
| 5 | `status` | int | 应用定义的状态码，`0`=OK，`None`=未记录 |
| 6 | `capture_time` | octet-string | 值的捕获时间戳 |

`unit`（只读）由 OBIS 代码自动派生，逻辑与 `Register` 完全一致。
```python
import dlms
ext_reg = dlms.ExtendedRegister(
        "1.0.2.8.0.255",
        value=0,
        scaler=-3,
        status=0,
        capture_time=(2025, 1, 1, 0, 0, 0),
        access={2: (dlms.AccessMode.READ_WRITE, dlms.Authentication.HIGH)}
)
print(ext_reg) 
# 输出<ExtendedRegister ln='1.0.2.8.0.255', value=0, scaler=-3, unit=Unit.ACTIVE_ENERGY, status=0, capture_time=(2025, 1, 1, 0, 0, 0)>
#同时也可以单个打印：ext_reg.logical_name...
#部分属性可进行修改，例：
print(ext_reg.capture_time) #输出(2025, 1, 1, 0, 0, 0)
ext_reg.capture_time = (2026, 1, 1, 0, 0, 0) #变更为 2026, 1, 1, 0, 0, 0)
#部分属性不可修改，例：
print(ext_reg.logical_name) #输出“1.0.2.8.0.255” ext_reg.logical_name初始化后不可修改


```
`构造函数`

```python
ExtendedRegister(logical_name: str, value: int | float | str = 0, scaler: int = 0,
                 status: int | None = None, capture_time: tuple | None = None,
                 access: dict = None)
```


| 参数 | 类型 | 默认值 | 说明 |
|------|------|--------|------|
| `logical_name` | `str` | 必传 | OBIS 代码字符串，如 `"1.0.2.8.0.255"` |
| `value` | `int \| float \| str` | `0` | 初始测量值 |
| `scaler` | `int` | `0` | 10 的幂指数。`-3` 表示原始值 ÷ 1000 |
| `status` | `int \| None` | `None` | 状态码，`0`=OK，`None`=未记录状态 |
| `capture_time` | `tuple \| None` | `None` | 捕获时间戳 `(year, month, day, hour, min, sec)` |
| `access` | `dict` | `None` | 实例级权限，格式 `{attr_index: (AccessMode, Authentication)}` |

`物理量转换公式（同 Register）`

```python
physical_value = raw_value * (10 ** scaler)
```

```python
import dlms
ext_reg = dlms.ExtendedRegister(
        "1.0.2.8.0.255",
        value=0,
        scaler=-3,
        status=0,
        capture_time=(2025, 1, 1, 0, 0, 0),
        access={2: (dlms.AccessMode.READ_WRITE, dlms.Authentication.HIGH)}
)
ext_reg.value = 1234789
raw_value = ext_reg.value * (10 ** ext_reg.scaler) # 1234.789
```

| 方法 | COSEM 方法 | 说明 |
|------|-----------|------|
| `reset()` | 1 | 将 `value` 重置为构造时的默认值；行为与 `Register` 一致 |

```python
# 默认行为：C 层直接恢复 value → 0
# reext_regg.reset() 该方法并注册开到python库中；在通信时，也能通过回调进行复位
# 成功return ture则调用C层的reset方法，否则不调用C层的reset方法
import dlms
def before_reset(self, event):
    if event.index == 1:  # method 1 = reset
        # 此处可以调用硬件 API 清零外部计数器
        pass
    return True  # 允许执行

ext_reg.on_before_action = before_reset
```

`Python 属性一览`

| 属性 | 类型 | 可写 | 说明 |
|------|------|:--:|------|
| `logical_name` | `bytes` | × | OBIS 代码（6 字节） |
| `value` | `int \| float \| str` | √ | 当前测量值（原始值，未应用 scaler） |
| `scaler` | `int` | √ | 10 的幂指数，用于计算物理量 |
| `unit` | `int` | × | 物理单位（`Unit` 枚举），由 OBIS 代码自动派生 |
| `status` | `int \| None` | √ | 状态码，`0`=OK，`None`=未记录 |
| `capture_time` | `tuple \| None` | √ | 捕获时间戳 `(year, month, day, hour, min, sec)` |
| `on_before_read` | `Callable` | √ | 读前钩子（继承自 CosemObject） |
| `on_after_read` | `Callable` | √ | 读后钩子（继承自 CosemObject） |
| `on_before_write` | `Callable` | √ | 写前钩子（继承自 CosemObject） |
| `on_after_write` | `Callable` | √ | 写后钩子（继承自 CosemObject） |
| `on_before_action` | `Callable` | √ | 动作前钩子（继承自 CosemObject） |
| `on_after_action` | `Callable` | √ | 动作后钩子（继承自 CosemObject） |
| `access_dict` | `dict` | √ | 实例级访问控制 |

`对比 Register`

| 特性 | Register | ExtendedRegister |
|------|----------|-----------------|
| `value` 类型 | `int` | `int \| float \| str` |
| `status` | 无 | √ |
| `capture_time` | 无 | √ |
| `reset()` | √ | √ |

`访问控制`

```python
import dlms
# 构造函数中指定
ext_reg = dlms.ExtendedRegister("1.0.2.8.0.255", value=0, scaler=-3,
    access={2: (dlms.AccessMode.READ_WRITE, dlms.Authentication.HIGH)})

# 或事后修改
ext_reg.access_dict = {2: (dlms.AccessMode.READ_WRITE, dlms.Authentication.HIGH)}
```

`典型用例`

| 场景 | 示例 |
|------|------|
| 总反向有功电能，带状态和捕获时间 | `ExtendedRegister("1.0.2.8.0.255", value=0, scaler=-3, status=0, capture_time=(2025, 1, 15, 10, 30, 0))` |
| 瞬时功率，浮动状态码 | `ExtendedRegister("1.0.21.7.0.255", value=0, scaler=0, status=None)` |

---

### `ProfileGeneric`
DLMS 标准对象，用于**历史数据存储**（负荷曲线、事件日志、账单记录）。内部维护一个有序行缓冲区，每行是若干捕获对象在某一时刻的快照，其**COSEM ID=7**。

| | **RAM Buffer**（C 侧管理| **外部存储**（Python 管理）|
|--|------------|---------|
| 数据存放 | `gxProfileGeneric.buffer`（堆内存）| flash / JSONL / EEPROM / 用户自定|
| 自动 capture | `cosem_invoke` → 写入 RAM buffer  | `on_before_action` → 你的 flash 写入函数  |
| 客户端 read|`cosem_getValue` → 从 RAM buffer 序列化| `on_before_read` → 你返回`list[list]`   |
| 排序（FIFO 等） | C 侧自动淘汰旧行        | 仅协议上报；**实际排序由 Python 实现**    |
| 持久化   | 掉电丢失   |  自己控制|
| BinarySerializer | N/A   | 自动跳过（buffer 标记为 COMPLEX）          |
| 适用场景  | 开发调试、小容量测试               | **生产环境 ── 必须使用**                   |


`Blue Book 属性`

| 编号 | 名称 | DLMS 类型 | Python 接口 | 说明 |
|----|------|-----------|-------------|------|
| 1 | `logical_name` | octet-string(6) | `logical_name` | OBIS 代码，只读 |
| 2 | `buffer` | ARRAY of STRUCTURE | 由 `on_before_read` 提供 | 历史数据行；Python 侧不能直接读写 通过 `buffer_size` 查看行数）|
| 3 | `capture_objects` | ARRAY of STRUCTURE | `capture_objects` | 定义每行捕获的对象、属性和数据索引 |
| 4 | `capture_period` | uint32 | `capture_period` | 自动捕获周期（秒），`0` 表示禁用 |
| 5 | `sort_method` | enum | `sort_method` | FIFO、LIFO、LARGEST 或 SMALLEST |
| 6 | `sort_object` | STRUCTURE | 见下方三个 Python 属性 | LARGEST/SMALLEST 的排序参照 |
| 7 | `entries_in_use` | uint32 | `entries_in_use` | 当前历史记录数 |
| 8 | `profile_entries` | uint32 | `profile_entries` | 最大历史记录数 |

标准属性只有 1 到 8。`sort_object_attribute_index` 和 `sort_object_data_index` 不是属性 7、8，而是标准属性 6 `sort_object` 的组成部分：

| Python 属性 | 对应 C 字段 | 默认值 |
|-------------|------------|:------:|
| `sort_object` | `sortObject` | `None` |
| `sort_object_attribute_index` | `sortObjectAttributeIndex` | `2` |
| `sort_object_data_index` | `sortObjectDataIndex` | `0` |

`排序方式常量`

| 常量 | 值 | 说明 |
|------|----|------|
| `ProfileGeneric.FIFO` | `1` | 先进先出（最常用） |
| `ProfileGeneric.LIFO` | `2` | 后进先出 |
| `ProfileGeneric.LARGEST` | `3` | 按 `sort_object` 排序，取最大值 |
| `ProfileGeneric.SMALLEST` | `4` | 按 `sort_object` 排序，取最小值 |

```python
#使用DLMS RAM Buffer 模式
import dlms
clock = dlms.Clock("0.0.1.0.0.255", time_zone=480)
reg = dlms.Register("1.0.1.8.0.255", 0, scaler=-3)

# 先创建空壳
profile = dlms.ProfileGeneric("1.0.99.1.0.255")
print(profile.logical_name)                 # 输出 "1.0.99.1.0.255"
print(profile.buffer_size)                  #输出 0 当输出等于profile_entries时，表示buffer已满
# DLMS 属性 7，当前行数。每次 capture 自动 +1，上限 = profile_entries。
# 正常只读，不要手动改。
print(profile.entries_in_use)   

# 再逐步填充属性
profile.capture_objects = [clock, reg]       #  裸对象写法
profile.capture_period = 900                 # 15 分钟 更新 capture_objects对象的值
profile.sort_method = dlms.ProfileGeneric.FIFO
profile.profile_entries = 5                  # 5 行

# 需要capture_period设置为LARGEST和SMALLEST才生效，否则不生效
profile.sort_object = reg                    
profile.sort_object_attribute_index = 2      # 默认就是 2，可改
profile.sort_object_data_index = 0           # 默认就是 0，可改
```
```
# buffer 变化（FIFO, profile_entries=5）：
# T0 (12:00):  [(2025,8,2,12, 0,0),  100]   ← 第 1 行
# T1 (12:15):  [(2025,8,2,12,15,0),  110]   ← 第 2 行
# T2 (12:30):  [(2025,8,2,12,30,0),  105]   ← 第 3 行
# T3 (12:45):  [(2025,8,2,12,45,0),   98]   ← 第 4 行
# T4 (13:00):  [(2025,8,2,13, 0,0),  130]   ← 第 5 行，buffer 满了
#──────────────────────────────────────────
# T5 (13:15):  [(2025,8,2,13,15,0),  125]   ← 踢掉 T0，T5 加末尾
# T6 (13:30):  [(2025,8,2,13,30,0),  115]   ← 踢掉 T1，T6 加末尾

```
```python
#使用DLMS RAM Buffer 模式
import dlms
clock = dlms.Clock("0.0.1.0.0.255", time_zone=480)
reg = dlms.Register("1.0.1.8.0.255", 0, scaler=-3)

# 先创建空壳
profile = dlms.ProfileGeneric("1.0.99.1.0.255")
# 再逐步填充属性
profile.capture_objects = [clock, reg]       # 裸对象写法
profile.capture_period = 900                 # 15 分钟 更新 capture_objects所有对象的值
profile.sort_method = dlms.ProfileGeneric.LARGEST
profile.profile_entries = 5                  # 3 行

# 需要capture_period设置为LARGEST和SMALLEST才生效，否则不生效
profile.sort_object = reg                    # 按 reg 的值比大小
profile.sort_object_attribute_index = 2      # reg 的属性 2 = value（默认）
profile.sort_object_data_index = 0           # 取整个值（默认，reg 是整数不需要子索引）


# sort_object_attribute_index和sort_object_data_index的用法
# 按 Clock 的时间比大小，只比"小时"字段
# profile.sort_object = clock
# profile.sort_object_attribute_index = 2      # Clock 的属性 2 = datetime 值
# profile.sort_object_data_index = 3           # datetime=(年,月,日,时,分,秒)，索引 3 = 小时
```
```
buffer 演变（LARGEST, profile_entries=3）：
T0 (12:00):  [(12, 0,0),  100]                                   reg 值: 100
T1 (12:15):  [(12,15,0),  110]                                   reg 值: 100, 110
T2 (12:30):  [(12,30,0),  105]                                   reg 值: 100, 110, 105 → 满了！
─────────────────────────────────────────────────────────────────
T3 (12:45):  [(12,45,0),   98]  ← reg=110 最大 → 踢掉 T1 那行
             结果：[(12,0,0),100]  [(12,30,0),105]  [(12,45,0),98]


#sort_object_attribute_index和sort_object_data_index的用法
#sort_method = LARGEST，sort_object = clock，data_index = 3（按"小时"比较）
#T0 (12:00):  [(12, 0,0),  100]   时=12
#T1 (14:00):  [(14, 0,0),  105]   时=14
#T2 (12:30):  [(12,30,0),  110]   时=12  → 满了！
#─────────────────────────────────────
#T3 (16:00):  [(16, 0,0),   98]   ← 时=14 最大 → 踢掉 T1
#            结果：[(12,0,0),100]  [(12,30,0),110]  [(16,0,0),98]
```
`构造函数`

```python
ProfileGeneric(logical_name: str, capture_objects: list = None,
               capture_period: int = None, sort_method: int = None,
               sort_object: object = None, profile_entries: int = None,
               entries_in_use: int = None, access: dict = None)
```

| 参数 | 类型 | 默认值 | 说明 |
|------|------|--------|------|
| `logical_name` | `str` | 必传 | OBIS 代码字符串，如 `"1.0.99.1.0.255"` |
| `capture_objects` | `list` | `None` | 每行捕获的内容，支持三种格式（见下方说明） |
| `capture_period` | `int` | `0` | 自动捕获间隔（秒），`0` 禁用，常见值 `900`（15 分钟） |
| `sort_method` | `int` | `0`（无效） | 排序方式（见下方常量表）。**默认 `0` 不是有效值**，请始终显式传入常量如 `ProfileGeneric.FIFO` |
| `sort_object` | `object` | `None` | 排序参照对象，仅 `LARGEST`/`SMALLEST` 时需要 |
| `profile_entries` | `int` | `0` | 缓冲区最大容量，如 `8928`（93 天 × 15 分钟） |
| `entries_in_use` | `int` | `0` | 当前已有行数 |
| `access` | `dict` | `None` | 实例级权限（**仅限关键字参数**），格式 `{attr_index: (AccessMode, Authentication)}` |

`重要说明`

- `buffer`（属性 2）带有 `AttributeFlag.COMPLEX` 标志，**`BinarySerializer` 不会自动持久化**。缓冲区需单独序列化。
- `capture_objects` 和 `sort_object` 也标记为 COMPLEX，序列化器同样跳过。
- **必须**设置 `on_before_read` handler 来提供历史数据，否则客户端读到空缓冲区：
```python
#使用DLMS 自定义存储 模式
import dlms

clock = dlms.Clock("0.0.1.0.0.255")
reg   = dlms.Register("1.0.1.8.0.255", 0, scaler=-3)

profile = dlms.ProfileGeneric(
    "1.0.99.1.0.255",
    capture_objects=[clock, reg],             # 裸对象 → 默认 attr=2, data=0
    capture_period=900,                       # 15 分钟自动捕获
    sort_method=dlms.ProfileGeneric.FIFO,
    profile_entries=8928,                     # 93 天 × 96 行/天
    access={
        2: (dlms.AccessMode.READ, dlms.Authentication.NONE)
    }
)

# ===== 写：自动 or 手动触发 → 存到外部 =====
def on_capture(self, event):
    if event.action != 1:                    # action=1 = capture
        return True
    row = [clock.time, reg.value]            # 拍快照
    append_row_to_flash(row)                 # 实现append_row_to_flash 写入操作
    return True

# ===== 读：客户端请求 buffer → 从外部取出 =====
def on_read_buffer(self, event):
    if event.index != self.idx('buffer'):    # 非 buffer 属性走默认
        return True
    return load_rows_from_flash()            # 实现append_row_to_flash 读取操作

profile.on_before_action = on_capture
profile.on_before_read   = on_read_buffer


```

`Python 属性一览`

| 属性 | 类型 | 可写 | 说明 |
|------|------|:--:|------|
| `logical_name` | `bytes` | × | OBIS 代码（6 字节） |
| `capture_objects` | `list` | √ | 每行捕获 `(obj, attr_index, data_index)` 三元组列表 |
| `capture_period` | `int` | √ | 自动捕获间隔（秒） |
| `sort_method` | `int` | √ | 排序方式 |
| `sort_object` | `object` | √ | 排序参照对象 |
| `sort_object_attribute_index` | `int` | √ | 排序参照属性索引 |
| `sort_object_data_index` | `int` | √ | 排序参照数据索引 |
| `profile_entries` | `int` | √ | 缓冲区最大容量 |
| `entries_in_use` | `int` | √ | 当前行数 |
| `buffer_size` | `int` | × | 缓冲区数据字节数（只读） |
| `on_before_read` | `Callable` | √ | 读前钩子（继承自 CosemObject） |
| `on_after_read` | `Callable` | √ | 读后钩子（继承自 CosemObject） |
| `on_before_write` | `Callable` | √ | 写前钩子（继承自 CosemObject） |
| `on_after_write` | `Callable` | √ | 写后钩子（继承自 CosemObject） |
| `on_before_action` | `Callable` | √ | 动作前钩子（继承自 CosemObject） |
| `on_after_action` | `Callable` | √ | 动作后钩子（继承自 CosemObject） |
| `access_dict` | `dict` | √ | 实例级访问控制 |

`方法`

| 方法 | 说明 |
|------|------|
| `deinit()` | 释放 C 侧资源，移除对象前调用 |


`访问控制`

```python
import dlms
# 构造函数中指定
profile = dlms.ProfileGeneric("1.0.99.1.0.255",
    access={
        2: (dlms.AccessMode.READ, dlms.Authentication.LOW),
        1: (dlms.AccessMode.AUTHENTICATED_WRITE, dlms.Authentication.HIGH),
    })

# 或事后修改
profile.access_dict = {2: (dlms.AccessMode.READ, dlms.Authentication.LOW)}
```


### `Clock`
设备的实时时钟对象，典型 OBIS 代码 `0.0.1.0.0.255`。包含时区偏移和夏令时（DST）配置。C 层在每次读写 `time` 属性时自动与硬件 RTC 同步，无需额外 handler 即可实现基本时间管理，其**COSEM ID=8**。

`Blue Book 属性`

| 编号 | 名称 | 类型 | 说明 |
|------|------|------|------|
| 1 | `logical_name` | octet-string(6) | OBIS 代码，只读 |
| 2 | `time` | octet-string | 当前日期和时间（⏩ VOLATILE，每读自动从 RTC 刷新） |
| 3 | `time_zone` | int16 | UTC 偏移量（分钟） |
| 4 | `status` | int8 | 时钟状态标志 （不建议手动更改） |
| 5 | `begin` | octet-string | DST 开始时间 |
| 6 | `end` | octet-string | DST 结束时间 |
| 7 | `deviation` | int8 | DST 生效时的偏移量（分钟） |
| 8 | `enabled` | bool | DST 调整是否启用 |
| 9 | `base` | enum | 时钟源 |

`默认 DST 配置：`

| 参数 | 默认值 | 含义 |
|------|--------|------|
| `begin` | `(-1, 3, -1, 2, 0, 0, 0, 0)` | 3 月最后一个周日 2:00 |
| `end` | `(-1, 10, -1, 3, 0, 0, 0, 0)` | 10 月最后一个周日 3:00 |

> 时间 tuple 中的 `-1` 表示"不指定"（wildcard），如 `(-1, -1, -1, 8, 0, 0)` 表示"每天 8:00"。

`时钟源常量（`base` 参数）`

| 常量 | 值 | 说明 |
|------|----|------|
| `Clock.BASE_NONE` | `0` | 未指定 |
| `Clock.BASE_CRYSTAL` | `1` | 晶体振荡器 |
| `Clock.BASE_FREQUENCY_50` | `2` | 50 Hz  |
| `Clock.BASE_FREQUENCY_60` | `3` | 60 Hz  |
| `Clock.BASE_GPS` | `4` | GPS 授时 |
| `Clock.BASE_RADIO` | `5` | 无线电授时 |

```python
import dlms
# 手动设置（写完自动同步到硬件RTC）
clock.time = (2025, 8, 3, 14, 30, 0)     # 年,月,日,时,分,秒

# 客户端 GET → 自动从硬件RTC刷新后回复
# 客户端每次读到的都是准确的实时值
# Python 直接读 → 读的是C struct缓存，可能是旧值
print(clock.time)   # 如果半小时前设的，还显示半小时前

# 这个值不会自动变，硬件RTC不报存时区，需要手动填写
clock.time_zone = 480    # UTC+8（东八区），单位：分钟
clock.time_zone = -300   # UTC-5（美国东部）

# bit5 由 DST 判断逻辑自动设置/清除，不建议手动改这个位
clock.status = 0         # Clock OK
clock.status = 4         # doubtful_value = 时间不可信
clock.status = 1         # clock_ok（0和1都表示OK）

# 元组: (年, 月, 日, 时, 分, 秒, 毫秒, 星期), -1表示通配
# 第9个元素是 extra_info 标志位（可选）
# 默认值：begin=2月最后一天2:00, end=10月最后一天3:00
clock.begin = (-1, 3, -1, 2, 0, 0, 0, 0)    # DST开始 3月最后一个周日 2:00
clock.end   = (-1, 10, -1, 3, 0, 0, 0, 0)   # DST结束 10月最后一个周日 3:00

clock.deviation = 60       # DST 生效时偏移60分钟（默认）
clock.deviation = 30       # 有些地区只偏移30分钟

clock.enabled = True       # 启用夏时令自动调整
clock.enabled = False      # 禁用

#仅告诉客户端时钟源，不负责同步
clock.base = dlms.Clock.BASE_FREQUENCY_50  # 时钟源：50Hz电网频率
clock.base = dlms.Clock.BASE_FREQUENCY_60  # 时钟源：60Hz电网频率
clock.base = dlms.Clock.BASE_GPS           # 时钟源：GPS授时
clock.base = dlms.Clock.BASE_RADIO         # 时钟源：无线电授时
```
`构造函数`

```python
Clock(logical_name: str, begin: tuple = ..., end: tuple = ..., time_zone: int = 0,
      deviation: int = 60, base: int = BASE_FREQUENCY_50, access: dict = None)
```

| 参数 | 类型 | 默认值 | 说明 |
|------|------|--------|------|
| `logical_name` | `str` | 必传 | OBIS 代码字符串，通常为 `"0.0.1.0.0.255"` |
| `begin` | `tuple` | 见下方 | DST 开始时间，默认 3 月最后一个周日 2:00 |
| `end` | `tuple` | 见下方 | DST 结束时间，默认 10 月最后一个周日 3:00 |
| `time_zone` | `int` | `0` | UTC 偏移量（分钟），`-480`=UTC−8（PST），`480`=UTC+8 |
| `deviation` | `int` | `60` | DST 偏移量（分钟），通常为 60 |
| `base` | `int` | `BASE_FREQUENCY_50` | 时钟源，可选 `BASE_CRYSTAL` / `BASE_FREQUENCY_50` / `BASE_FREQUENCY_60` / `BASE_GPS` / `BASE_RADIO` |
| `access` | `dict` | `None` | 实例级权限，格式 `{attr_index: (AccessMode, Authentication)}` |


`方法（COSEM 动作）`

| 方法 | COSEM 方法 | 说明 |
|------|-----------|------|
| `adjust_to_quarter()` | 1 | 四舍五入到最近 15 分钟 |
| `adjust_to_minute()` | 3 | 四舍五入到整分钟 |
| `adjust_to_preset_time()` | 4 | 应用预存的目标时间 |
| `preset_adjusting_time(tuple)` | 5 | 存储目标时间供 `adjust_to_preset_time()` 使用 |
| `shift_time(seconds)` | 6 | 按秒数偏移（正数前进，负数后退） |

> C 层在每次读写 `time` 属性时自动与硬件 RTC 双向同步，无需 handler。事件钩子可用于审计日志等额外用途。

```python
import dlms
# ============================================================
# 1. 初始化（含 DST 配置）
# ============================================================
clock = dlms.Clock(
    "0.0.1.0.0.255",
    time_zone=480,                       # UTC+8（东八区），单位：分钟
    deviation=60,                        # DST 生效时偏移 60 分钟
    begin=(-1, 3, -1, 2, 0, 0, 0, 0),   # DST start: 每年3月最后一个周日 2:00
    end=(-1, 10, -1, 3, 0, 0, 0, 0),    # DST end:   每年10月最后一个周日 3:00
    base=dlms.Clock.BASE_CRYSTAL,        # 告知客户端：内部晶振
    access={
        2: (dlms.AccessMode.READ, dlms.Authentication.LOW),
        1: (dlms.AccessMode.AUTHENTICATED_WRITE, dlms.Authentication.HIGH),
    }
)
# —— 中国无 DST，但保留代码作为参考 ——
clock.enabled = False

# 同步一次正确时间到硬件 RTC
import utime
rtc = utime.localtime()
clock.time = (rtc[0], rtc[1], rtc[2], rtc[3], rtc[4], rtc[5])
```
```python 
# 另一台设备作为 client，远程操作这台表，该例子仅供理解
client = dlms.Client(client_address=0x10, server_address=0x01, ...)
client.connect(...)
objs = client.get_objects()
clock = [o for o in objs if type(o).__name__ == "Clock"][0]

# 标准校时两步走：
client.method(clock, 5, (2026, 8, 3, 14, 30, 0))  # 先预设时间
client.method(clock, 4, None)                      # 再激活

# 或直接对齐刻钟/分钟/偏移：
client.method(clock, 1, None)   # adjust_to_quarter
client.method(clock, 3, None)   # adjust_to_minute
client.method(clock, 6, 3600)   # shift_time 快进 1 小时
```
`Python 属性一览`

| 属性 | 类型 | 可写 | 说明 |
|------|------|:--:|------|
| `logical_name` | `bytes` | × | OBIS 代码（6 字节） |
| `time` | `tuple` | √ | 当前日期时间 `(year, month, day, hour, minute, sec)`，每读自动从 RTC 刷新 |
| `time_zone` | `int` | √ | UTC 偏移量（分钟） |
| `status` | `int` | √ | 时钟状态标志 |
| `begin` | `tuple` | √ | DST 开始时间 |
| `end` | `tuple` | √ | DST 结束时间 |
| `deviation` | `int` | √ | DST 偏移量（分钟） |
| `enabled` | `bool` | √ | DST 是否启用 |
| `base` | `int` | √ | 时钟源（见上方常量表） |
| `on_before_read` | `Callable` | √ | 读前钩子（继承自 CosemObject） |
| `on_after_read` | `Callable` | √ | 读后钩子（继承自 CosemObject） |
| `on_before_write` | `Callable` | √ | 写前钩子（继承自 CosemObject） |
| `on_after_write` | `Callable` | √ | 写后钩子（继承自 CosemObject） |
| `on_before_action` | `Callable` | √ | 动作前钩子（继承自 CosemObject） |
| `on_after_action` | `Callable` | √ | 动作后钩子（继承自 CosemObject） |
| `access_dict` | `dict` | √ | 实例级访问控制 |

`访问控制`

```python
import dlms
# 构造函数中指定
clock = dlms.Clock("0.0.1.0.0.255",
    access={2: (dlms.AccessMode.READ_WRITE, dlms.Authentication.HIGH)})

# 或事后修改
clock.access_dict = {2: (dlms.AccessMode.READ_WRITE, dlms.Authentication.HIGH)}
```

`典型用例`

| 场景 | 示例 |
|------|------|
| UTC+8 时区，无 DST | `Clock("0.0.1.0.0.255", time_zone=480, deviation=0)` |
| UTC−8 (PST)，美国 DST | `Clock("0.0.1.0.0.255", time_zone=-480, deviation=60)` |
| GPS 授时 | `Clock("0.0.1.0.0.255", base=Clock.BASE_GPS)` |

---
### `CompactData`
模板驱动的紧凑编码对象。将多个捕获对象的属性值编码为一个紧凑二进制缓冲区，**省去每个值的类型标签**，显著减少窄带链路（如 PLC、SMS）上的传输字节数，其**COSEM ID=62**。

`template_description`（属性 5）由 Gurux 自动维护，描述了类型模板，客户端据此解码缓冲区。`capture_objects` 使用与 `ProfileGeneric` 相同的三元组格式。

`Blue Book 属性`

| 编号 | 名称 | 类型 | 说明 |
|------|------|------|------|
| 1 | `logical_name` | octet-string(6) | OBIS 代码，只读 |
| 2 | `buffer` | octet-string | 紧凑编码后的原始字节（可手动写入预编码 payload） |
| 3 | `capture_objects` | ARRAY of STRUCT | 定义捕获哪些对象的哪些属性，格式同 `ProfileGeneric` |
| 4 | `template_id` | uint8 | 模板标识符（0–255） |
| 5 | `template_description` | octet-string | 类型模板描述（只读，自动维护） |
| 6 | `capture_method` | enum | 捕获模式 |

`捕获模式（`capture_method`）`

| 常量 | 值 | 说明 |
|------|----|------|
| `CaptureMethod.IMPLICIT` | `0` | 每次读属性 2（buffer）时自动刷新编码 |
| `CaptureMethod.INVOKE` | `1` | 客户端调用方法 2 时才刷新编码 |

```python 
#例子仅用clock和register对象做演示
import dlms
clock = dlms.Clock("0.0.1.0.0.255", time_zone=480)
reg = dlms.Register("1.0.1.8.0.255", 0, scaler=-3)
cd = dlms.CompactData("0.0.96.60.0.255")

print(cd)                    #<CompactData ln='0.0.96.60.0.255', objs=2, buf=4, method=IMPLICIT>
cd.buffer = b'123456'
print(cd.buffer)             #b'123456'
cd.capture_objects = [(clock, 2, 0), (reg, 2, 0)]
print(cd.capture_objects)    #[(None, 2, 0), (None, 2, 0)]
cd.template_id=1
print(cd.template_id)        #1
cd.capture_method=dlms.CaptureMethod.IMPLICIT
print(cd.capture_method)     #IMPLICIT
```

`构造函数`

```python
CompactData(logical_name: str, capture_objects: list = None, template_id: int = 0,
            capture_method: int = CaptureMethod.IMPLICIT, access: dict = None)
```
| 参数 | 类型 | 默认值 | 说明 |
|------|------|--------|------|
| `logical_name` | `str` | 必传 | OBIS 代码字符串，如 `"0.0.96.60.0.255"` |
| `capture_objects` | `list` | `None` | 捕获列表，每元素为 `(obj, attr_idx, data_idx)` 三元组 |
| `template_id` | `int` | `0` | 模板标识符（0–255） |
| `capture_method` | `int` | `CaptureMethod.IMPLICIT` | `IMPLICIT`=读时自动编码，`INVOKE`=客户端方法触发 |
| `access` | `dict` | `None` | 实例级权限，格式 `{attr_index: (AccessMode, Authentication)}` |

```python
import dlms
cd = dlms.CompactData(
    "0.0.96.60.0.255",
    capture_objects=[(clock, 2, 0), (reg, 2, 0)],
    template_id=1,
    capture_method=dlms.CaptureMethod.IMPLICIT,
    access={2: (dlms.AccessMode.READ_WRITE, dlms.Authentication.HIGH)}
)
```
`Python 属性一览`

| 属性 | 类型 | 可写 | 说明 |
|------|------|:--:|------|
| `logical_name` | `bytes` | × | OBIS 代码（6 字节） |
| `buffer` | `bytes` | √ | 紧凑编码后的原始字节（也可手动写入预编码 payload） |
| `capture_objects` | `list` | √ | `(obj, attr_idx, data_idx)` 三元组列表 |
| `template_id` | `int` | √ | 模板标识符（0–255） |
| `template_description` | `bytes` | × | 类型模板描述（自动维护） |
| `capture_method` | `int` | √ | 捕获模式（`IMPLICIT`/`INVOKE`） |
| `on_before_read` | `Callable` | √ | 读前钩子（继承自 CosemObject） |
| `on_after_read` | `Callable` | √ | 读后钩子（继承自 CosemObject） |
| `on_before_write` | `Callable` | √ | 写前钩子（继承自 CosemObject） |
| `on_after_write` | `Callable` | √ | 写后钩子（继承自 CosemObject） |
| `on_before_action` | `Callable` | √ | 动作前钩子（继承自 CosemObject） |
| `on_after_action` | `Callable` | √ | 动作后钩子（继承自 CosemObject） |
| `access_dict` | `dict` | √ | 实例级访问控制 |

`方法`

| 方法 | 说明 |
|------|------|
| `capture(server)` | **手动触发捕获**（非请求上下文中）。将当前捕获对象的值编码到 buffer，并刷新 `template_description`。传入一个运行中的 `Server` 实例。 |
| `idx(name)` | 返回 Python 属性名对应的 Blue Book 属性编号（继承自 CosemObject） |

> **⚠️ 重要：** `on_before_read` / `on_before_action` handler **禁止设置 `event.handled = True`**。handler 返回后 Gurux 必须继续执行紧凑编码。

` IMPLICIT 模式 + `on_before_read`（推荐）`

```python
import dlms

cd = dlms.CompactData(
    "0.0.96.60.0.255",
    capture_objects=[(clock, 2, 0), (reg, 2, 0)],
    template_id=1,
    capture_method=dlms.CaptureMethod.IMPLICIT,
)

# 在编码前更新捕获对象的值（如从传感器读取）
def refresh(self, event):
    if event.index == self.idx('buffer'):
        reg.value = read_sensor()   # 在编码前刷新硬件读数
        # 不要 return False — 编码必须继续执行

cd.on_before_read = refresh
```

`手动捕获：启动时预填 buffer`

```python
# 在 server.run() 之前手动触发一次捕获
reg.value = 12345678
cd.capture(server)  # 将当前值编码进 buffer 并更新 template_description
```

`访问控制`

```python
# 构造函数中指定
cd = dlms.CompactData("0.0.96.60.0.255",
    access={2: (dlms.AccessMode.READ, dlms.Authentication.NONE)})

# 或事后修改
cd.access_dict = {2: (dlms.AccessMode.READ, dlms.Authentication.NONE)}
```

`典型用例`

| 场景 | 示例 |
|------|------|
| 窄带 PLC 上报电能和时间 | `CompactData("0.0.96.60.0.255", capture_objects=[(clock, 2, 0), (reg, 2, 0)], template_id=1)` |
| SMS 推送紧凑编码 | `CompactData("0.0.96.60.0.255", capture_objects=[(reg_a, 2, 0), (reg_b, 2, 0)], capture_method=CaptureMethod.INVOKE)` |

---
### `DisconnectControl`
可控供电开关继电器。用于远程断开/合上负载（如欠费断电、远程资产管理）。C 层默认行为只更新内存中的 `output_state` 和 `control_state`；如需驱动真实硬件继电器，用 `on_before_action` 拦截，其**COSEM ID=70**。

`Blue Book 属性`

| 编号 | 名称 | 类型 | 说明 |
|------|------|------|------|
| 1 | `logical_name` | octet-string(6) | OBIS 代码，只读 |
| 2 | `output_state` | bool | `True`=负载接通, `False`=负载断开 |
| 3 | `control_state` | enum | `0`=断开, `1`=接通, `2`=等待重连 |
| 4 | `control_mode` | uint8 | 操作模式（0–6，见 Blue Book） |

```python 
import dlms
disconnect_ctl = dlms.DisconnectControl("0.0.96.3.10.255")
print(disconnect_ctl.logical_name)     #输出0.0.96.3.10.255
disconnect_ctl.output_state = True     
print(disconnect_ctl.output_state)     #输出 True
disconnect_ctl.control_mode = 1 
print(disconnect_ctl.control_mode)     #输出 1
```

`构造函数`

```python
DisconnectControl(logical_name: str, output_state: bool = False,
                  control_state: int = 0, control_mode: int = 0,
                  access: dict = None)
```

| 参数 | 类型 | 默认值 | 说明 |
|------|------|--------|------|
| `logical_name` | `str` | 必传 | OBIS 代码字符串，如 `"0.0.96.3.10.255"` |
| `output_state` | `bool` | `False` | 初始继电器状态，`True`=接通, `False`=断开 |
| `control_state` | `int` | `0` | 控制状态码，`0`=断开, `1`=接通, `2`=等待重连 |
| `control_mode` | `int` | `0` | 操作模式（0–6），决定允许哪些手动/远程操作 |
| `access` | `dict` | `None` | 实例级权限，格式 `{attr_index: (AccessMode, Authentication)}` |

`控制状态码`

| 值 | 名称 | 说明 |
|----|------|------|
| `0` | DISCONNECTED | 已断开 |
| `1` | CONNECTED | 已接通 |
| `2` | READY_FOR_RECONNECTION | 等待重连 |
```python
import dlms
disconnect_ctl = dlms.DisconnectControl(
    "0.0.96.3.10.255",
    output_state=True,
    control_state=1,
    control_mode=1,
    access={
        2: (dlms.AccessMode.READ, dlms.Authentication.NONE),           
        4: (dlms.AccessMode.AUTHENTICATED_WRITE, dlms.Authentication.HIGH),
    }
    )

```
`方法（COSEM 动作）`

| 方法 | COSEM 方法 | 说明 |
|------|-----------|------|
| `remote_disconnect()` | 1 | 远程断开负载，默认行为更新 `output_state=False`、`control_state=0` |
| `remote_reconnect()` | 2 | 远程合上负载，默认行为更新 `output_state=True`、`control_state=1` |

> 默认行为只更新内存中的 C 结构体。如需驱动真实硬件 GPIO，使用 `on_before_action` 拦截。

硬件联动：`on_before_action`

```python
import dlms
from machine import Pin
# 假设 GPIO 引脚用于控制继电器
gpio_relay = Pin(Pin.GPIO1, Pin.OUT, Pin.PULL_DISABLE, 1)

def relay_handler(self, event):
    if event.index == 1:
        gpio_relay.write(0)   # remote_disconnect → 断开硬件
    elif event.index == 2:
        gpio_relay.write(1)   # remote_reconnect → 合上硬件
    return True               # 允许 C 层继续更新内存状态

disconnect_ctl = dlms.DisconnectControl(
    "0.0.96.3.10.255",
    output_state=True,
    control_state=1,
    control_mode=1,
    )
disconnect_ctl.on_before_action = relay_handler
```

`Python 属性一览`

| 属性 | 类型 | 可写 | 说明 |
|------|------|:--:|------|
| `logical_name` | `bytes` | × | OBIS 代码（6 字节） |
| `output_state` | `bool` | √ | 继电器状态，`True`=接通, `False`=断开 |
| `control_state` | `int` | √ | 控制状态，`0`=断开, `1`=接通, `2`=等待重连 |
| `control_mode` | `int` | √ | 操作模式（0–6） |
| `on_before_read` | `Callable` | √ | 读前钩子（继承自 CosemObject） |
| `on_after_read` | `Callable` | √ | 读后钩子（继承自 CosemObject） |
| `on_before_write` | `Callable` | √ | 写前钩子（继承自 CosemObject） |
| `on_after_write` | `Callable` | √ | 写后钩子（继承自 CosemObject） |
| `on_before_action` | `Callable` | √ | 动作前钩子（继承自 CosemObject） |
| `on_after_action` | `Callable` | √ | 动作后钩子（继承自 CosemObject） |
| `access_dict` | `dict` | √ | 实例级访问控制 |

`访问控制`

```python
import dlms
# 构造函数中指定：正常遥测可读，远程通断需要 HIGH 认证
disconnect_ctl = dlms.DisconnectControl("0.0.96.3.10.255",
    output_state=True,
    control_state=1,
    control_mode=1,
    access={
        2: (dlms.AccessMode.READ, dlms.Authentication.NONE),           # output_state 可读
        4: (dlms.AccessMode.AUTHENTICATED_WRITE, dlms.Authentication.HIGH),  # control_mode 需 HIGH 认证
    })

# 或事后修改
disconnect_ctl.access_dict = {4: (dlms.AccessMode.AUTHENTICATED_WRITE, dlms.Authentication.HIGH)}
```
`典型用例`

| 场景 | 示例 |
|------|------|
| 欠费断电，默认接通 | `DisconnectControl("0.0.96.3.10.255", output_state=True, control_state=1, control_mode=1)` |
| 初始断开，等待远程合闸 | `DisconnectControl("0.0.96.3.10.255", output_state=False, control_state=0, control_mode=1)` |

---
### `ScriptTable`
> COSEM Class ID = 9，用于将"一组对 DLMS 对象的操作"打包成**脚本（Script）**，
> 供远程客户端通过 `Execute` 方法一键触发，实现断连/复电、电价切换、限载等自动化场景。

---

```
ScriptTable
├── Script #1 (id=1)          ← 由 add_script() 添加
│   ├── ScriptAction (Write)   → 写目标对象属性
│   └── ScriptAction (Execute) → 调用目标对象方法
├── Script #2 (id=2)
└── ...
```

`构造函数`

**ScriptTable：**

```python
ScriptTable(logical_name: str, access: dict = None)
```


| 参数 | 类型 | 默认值 | 说明 |
|------|------|--------|------|
| `logical_name` | `str` | 必传 | OBIS 代码字符串，如 `"0.0.10.0.106.255"` |
| `access` | `dict` | `None` | 实例级权限，格式 `{attr_index: (AccessMode, Authentication)}` |

```python
import dlms
script_table = dlms.ScriptTable(
    "0.0.10.0.106.255",
    access={1:(dlms.AccessMode.AUTHENTICATED_WRITE,dlms.Authentication.HIGH),
    })
```


**ScriptAction：**

```python
ScriptAction(type: int, target: object, method: int = None,
             attribute: int = None, parameter: int | bool = None)
```

| 参数 | 类型 | 默认值 | 说明 |
|------|------|--------|------|
| `type` | `int` | 必传 | 动作类型，决定这条动作"干什么"：`ScriptAction.Write`(1) = 写目标对象的**属性**；`ScriptAction.Execute`(2) = 调用目标对象的**方法**。见下方"两种动作类型" |
| `target` | `object` | 必传 | 动作作用于的**目标 DLMS 对象实例**（`Register`、`DisconnectControl`、`Clock` 等）。必须是已创建好的对象，C 层执行时会向其发 `SET`(Write) 或 `ACTION`(Execute) 请求。传非 DLMS 对象会报 `TypeError` |
| `method` | `int` | `None` | **仅 `type=Execute` 时使用**（必填，不能为 0）。目标对象**方法的编号**，如 DisconnectControl 中 `1`=remote_disconnect（断开）、`2`=remote_reconnect（恢复）。具体编号含义取决于目标对象类 |
| `attribute` | `int` | `None` | **仅 `type=Write` 时使用**（必填，不能为 0）。目标对象**属性的编号**，如 Register 中 `2`=value（当前值）。执行时把 `parameter` 的值写入该属性 |
| `parameter` | `int \| bool` | `None` | 动作携带的数据：**Write** 时 = 要写入属性的新值；**Execute** 时 = 传给方法调用的参数。可选，不传则动作不带数据。仅支持 `int`（按大小自动转为 int8/int16/int32）和 `bool`，其它类型报 `TypeError` |

` ScriptAction 类型`

| 类型 | 值 | 说明 |
|------|----|------|
| `ScriptAction.Write` | `1` | 把目标对象的指定属性设为给定值 |
| `ScriptAction.Execute` | `2` | 调用目标对象的指定方法 |


```python
import dlms

tariff_reg  = dlms.Register("1.0.1.8.0.255", default_value=0)
tariff_reg1  = dlms.Register("1.0.1.8.0.255", default_value=0)
disconnect  = dlms.DisconnectControl("0.0.2.0.0.255")
disconnect1  = dlms.DisconnectControl("0.0.2.0.0.255")

script_table = dlms.ScriptTable("0.0.10.0.106.255")

action_a = dlms.ScriptAction(
    type=dlms.ScriptAction.Write,
    target=tariff_reg,
    attribute=2,
    parameter=2,          
)
action_b = dlms.ScriptAction(
    type=dlms.ScriptAction.Write,
    target=tariff_reg1,
    attribute=2,
    parameter=2,          
)

action_c = dlms.ScriptAction(
    type=dlms.ScriptAction.Execute,
    target=disconnect,
    method=2,
    parameter=0,
)

action_d = dlms.ScriptAction(
    type=dlms.ScriptAction.Execute,
    target=disconnect1,
    method=2,
    parameter=0,
)

script_table.add_script(id=3, actions=[action_a,action_c]) #关联组： ID:3对应action_a, action_c
script_table.add_script(id=2, actions=action_b)          #关联组： ID2对应action_b
script_table.add_script(id=4, actions=action_d)          #关联组： ID2对应action_b
print(script_table.get_scripts())                        # [3，2,4]
script_table.remove_script(3)                            #删除关联组：3
print(script_table.get_scripts())                        # [2,4]

#填写id为2是失败的，因为2不是设置为Execute，而4是成功的
script_table.execute_script(4)                           #True
```

`Python 属性一览`

| 属性 | 类型 | 可写 | 说明 |
|------|------|:--:|------|
| `logical_name` | `bytes` | × | OBIS 代码（6 字节） |
| `on_before_read` | `Callable` | √ | 读前钩子（继承自 CosemObject） |
| `on_after_read` | `Callable` | √ | 读后钩子（继承自 CosemObject） |
| `on_before_write` | `Callable` | √ | 写前钩子（继承自 CosemObject） |
| `on_after_write` | `Callable` | √ | 写后钩子（继承自 CosemObject） |
| `on_before_action` | `Callable` | √ | 动作前钩子（继承自 CosemObject） |
| `on_after_action` | `Callable` | √ | 动作后钩子（继承自 CosemObject） |
| `access_dict` | `dict` | √ | 实例级访问控制 |

| 方法 | 说明 |
|------|------|
| `add_script(id, actions)` | 添加脚本。`actions` 可以是单个 `ScriptAction` 或 `ScriptAction` 列表 |
| `get_scripts()` | 返回配置的脚本 ID 列表 |
| `remove_script(id)` | 按 ID 删除脚本，成功返回 `True` |
| `execute_script(id)` | 该接口需要在回调中进行执行，本地调用仅测试时使用 |
| `deinit()` | 释放 C 侧资源 |

> `on_before_action` 在脚本执行前触发。返回 `False` 可阻止整个脚本执行；返回 `True` 后 C 层按序迭代所有动作。
```python
import dlms
#该服务器例子仅供参考，服务器和客户端通信时，如何触发execute，不能正常运行
script_table = dlms.ScriptTable("0.0.10.0.106.255")
# ... 添加脚本 1、2、99 ...

def handle_script_execute(self, event):
    script_id = event.parameters
    print("[ScriptTable] Python handler: Execute script ID {}".format(script_id))

    # 场景 1：脚本 1 —— 做日志/校验后交给 C 执行
    if script_id == 1:
        print("[ScriptTable] Preparing to close disconnect control...")
        return True          # 放行，C 层按序执行动作

    # 场景 2：脚本 99 —— 纯 Python 实现，不走 C
    elif script_id == 99:
        print("[ScriptTable] Custom Python-only script")
        return False         # 阻止 C 执行

    # 场景 3：其它脚本 —— 默认放行
    return True

script_table.on_before_action = handle_script_execute

#客户端例子
import dlms
client = dlms.Client(
    client_address=0x41,
    server_address=1,
    authentication=dlms.Authentication.HIGH,
    password=b"your_password",
)
client.connect(serial)

# 获取 ScriptTable 对象
objs = client.get_objects()
st = next(o for o in objs if type(o).__name__ == "ScriptTable")

# 触发 script_id=1：发送 Action-Request，方法号=1，参数=script_id
result = client.method(st, 1, 1)   # (对象, 方法号=1固定, script_id)

```

---

### `ActivityCalendar`
分层费率计划表。以"季节 → 周 → 日"的三层结构组织定时脚本执行（其**COSEM ID=20**）：

```
季节 (Season)
 └── 周模板 (Week Profile)
      └── 日模板 (Day Profile)
           └── 定时动作列表: ((hour, min, sec), script_table, script_id)
```

日历采用**主备双版本**设计：`active`（活跃，对客户端可见）和 `passive`（待生效，在后台预先构建）。调用 `activate_passive_calendar()` 将 passive 版本提升为 active。

`Blue Book 属性`

| 编号 | 名称 | 类型 | 说明 |
|------|------|------|------|
| 1 | `logical_name` | octet-string(6) | OBIS 代码，只读 |
| 2 | `calendar_name_active` | visible-string | 活跃日历名称 |
| 3–5 | （season / week / day 模板，active） | complex | COSEM 复杂结构，Python 方法操作 |
| 6 | `calendar_name_passive` | visible-string | 待生效日历名称 |
| 7–9 | （season / week / day 模板，passive） | complex | COSEM 复杂结构，Python 方法操作 |
| 10 | `activate_passive_calendar_time` | octet-string | passive → active 的触发时间 |

| 方法 | 说明 |
|------|------|
| `add_season_profile(season_name, start_time, week_name, passive=False)` | 添加季节。`start_time` 为 `(month, day, hour)` 三元组 |
| `add_week_profile(name, monday, tuesday, ..., sunday, passive=False)` | 添加周模板，每天映射到一个 `day_id` |
| `add_day_profile(day_id, actions, passive=False)` | 添加日模板。`actions` 为 `((hour, min, sec), script_table, script_id)` 元组列表 |
| `get_season_profiles(passive=False)` | 获取季节列表 |
| `get_week_profiles(passive=False)` | 获取周模板列表 |
| `get_day_profiles(passive=False)` | 获取日模板列表 |
| `clear_profiles(passive=False)` | 清空所有模板 |
| `copy_active_to_passive()` | 将 active 日历复制到 passive |
| `activate_passive_calendar()` | **动作 1**：将 passive 提升为 active |
| `deinit()` | 释放 C 侧资源 |

> 所有构建方法默认修改 `passive` 日历（`passive=True`）。构建完成后调用 `activate_passive_calendar()` 使之生效。

```python
import dlms
from dlms import AccessMode, Authentication
st = dlms.ScriptTable("0.0.10.0.0.255")
st = dlms.ScriptTable("0.0.10.0.0.255")   # 准备 ScriptTable 供定时动作引用
r = dlms.Register("1.1.1.8.0.255",default_value=0)        # 要写入的目标寄存器

st.add_script(id=1, actions=[dlms.ScriptAction(
    type=dlms.ScriptAction.Write, target=r, attribute=2, parameter=150
)])
st.add_script(id=2, actions=[dlms.ScriptAction(
    type=dlms.ScriptAction.Write, target=r, attribute=2, parameter=80
)])
cal = dlms.ActivityCalendar("0.0.13.0.0.255",
    access={2: (AccessMode.READ_WRITE, Authentication.LOW)})

# ── 属性 1: logical_name ──
print(cal.logical_name)  # b'\x00\x00\r\x00\x00\xff'
# ── 属性 2: calendar_name_active ──
cal.calendar_name_active = "Summer2026"
print(cal.calendar_name_active)         # "Summer2024"
# ── 属性 3–5: season / week / day （active 侧）──
# 通过 add_* 方法 + passive=False 直接操作 active
cal.add_day_profile(1, [((8, 0, 0), st, 1)], passive=False)
cal.add_week_profile("WorkWeek", monday=1, tuesday=1, wednesday=1,
                     thursday=1, friday=1, saturday=1, sunday=1,
                     passive=False)
cal.add_season_profile("Summer", (6, 1, 0), "WorkWeek", passive=False)

# 读取 active 侧的完整的数据
for s in cal.get_season_profiles(passive=False):
    print(s["name"], s["start_time"])   # Summer, (6, 1, ...)
for w in cal.get_week_profiles(passive=False):
    print(w["name"], w["monday"])       # WorkWeek, 1

# ── 属性 6: calendar_name_passive ──
cal.calendar_name_passive = "Winter2026"
print(cal.calendar_name_passive)  # "Winter2024"

# ── 属性 7–9: season / week / day （passive 侧）──
# passive=True 是默认值，可省略
cal.add_day_profile(1, [((6, 0, 0), st, 1), ((22, 0, 0), st, 2)])
cal.add_week_profile("AllWeek", monday=1, tuesday=1, wednesday=1,
                     thursday=1, friday=1, saturday=1, sunday=1)
cal.add_season_profile("Winter", (1, 1, 0), "AllWeek")
# 遍历 passive 模板（默认 passive=True）
for d in cal.get_day_profiles():
    for a in d["actions"]:
        print(a["time"], a["selector"])  # (6,0,0) 1  /  (22,0,0) 2

# ── 属性 10: activate_passive_calendar ─
cal.activate_passive_calendar()       # passive → active
```

`构造函数`

```python
ActivityCalendar(logical_name: str, calendar_name_active: str = "",
                 calendar_name_passive: str = "", access: dict = None)
```

| 参数 | 类型 | 默认值 | 说明 |
|------|------|--------|------|
| `logical_name` | `str` | 必传 | OBIS 代码字符串，如 `"0.0.13.0.0.255"` |
| `calendar_name_active` | `str` | `""` | 活跃日历名称 |
| `calendar_name_passive` | `str` | `""` | 待生效日历名称 |
| `access` | `dict` | `None` | 实例级权限，格式 `{attr_index: (AccessMode, Authentication)}` |

`通配符`

| 常量 | 值 | 说明 |
|------|----|------|
| `dlms.ANY` | `-1` | 在时间字段中表示"任意"，如 `(ANY, ANY, ANY, 8, 0, 0)` 表示"每天 8:00" |

```python
import dlms
from dlms import AccessMode, Authentication

cal = dlms.ActivityCalendar("0.0.13.0.0.255",
    calendar_name_active="SummerRates",
    calendar_name_passive="WinterRates",
    access={2: (AccessMode.READ, Authentication.NONE)})

```
`Python 属性一览`

| 属性 | 类型 | 可写 | 说明 |
|------|------|:--:|------|
| `logical_name` | `bytes` | × | OBIS 代码（6 字节） |
| `calendar_name_active` | `str` | √ | 活跃日历名称 |
| `calendar_name_passive` | `str` | √ | 待生效日历名称 |
| `on_before_read` | `Callable` | √ | 读前钩子（继承自 CosemObject） |
| `on_after_read` | `Callable` | √ | 读后钩子（继承自 CosemObject） |
| `on_before_write` | `Callable` | √ | 写前钩子（继承自 CosemObject） |
| `on_after_write` | `Callable` | √ | 写后钩子（继承自 CosemObject） |
| `on_before_action` | `Callable` | √ | 动作前钩子（继承自 CosemObject） |
| `on_after_action` | `Callable` | √ | 动作后钩子（继承自 CosemObject） |
| `access_dict` | `dict` | √ | 实例级访问控制 |

`访问控制`

```python
import dlms
cal = dlms.ActivityCalendar("0.0.13.0.0.255",
    calendar_name_active="SummerRates",
    calendar_name_passive="WinterRates",
    access={2: (dlms.AccessMode.READ, dlms.Authentication.NONE)})

# 或事后修改
cal.access_dict = {2: (dlms.AccessMode.READ_WRITE, dlms.Authentication.HIGH)}
```

`典型用例`

| 场景 | 示例 |
|------|------|
| 分层费率 | 峰谷分时、季节切换 | 


---
### `SingleActionSchedule`
单次定时脚本调度器（其 **COSEM ID=22**）。在指定的日期/时间组合触发 `ScriptTable` 中某个脚本的执行。支持通配符 `dlms.ANY`，C 层自动将当前时间与 `execution_times` 列表匹配，命中时调用对应的 `(ScriptTable, selector)`。

`Blue Book 属性`

| 编号 | 名称 | 类型 | 说明 |
|------|------|------|------|
| 1 | `logical_name` | octet-string(6) | OBIS 代码，只读 |
| 2 | `executed_script` | STRUCT | 要执行的 `(ScriptTable, selector)` 元组；**读取**：`("A.B.C.D.E.F", int)` 元组 |
| 3 | `execution_type` | enum | 执行类型，当前固定为 `1`（Type 1：按日期/时间表达式匹配） |
| 4 | `execution_times` | ARRAY of STRUCT | 可读写，时间元组列表（6 或 7 元组；读取始终返回 7 元组） |

`构造函数`

```python
SingleActionSchedule(logical_name: str, executed_script: tuple = None,
                     execution_type: int = 1, execution_times: list = None,
                     access: dict = None)
```

| 参数 | 类型 | 默认值 | 说明 |
|------|------|--------|------|
| `logical_name` | `str` | 必传 | OBIS 代码字符串，如 `"0.0.15.0.1.255"` |
| `executed_script` | `tuple` | `None` | `(script_table_instance, script_id)` 二元组 |
| `execution_type` | `int` | `1` | 执行类型，默认为 `1`（Type 1：日期/时间表达式） |
| `execution_times` | `list` | `None` | 时间表达式列表，每项 6 或 7 元组；`-1` 表示通配 |
| `access` | `dict` | `None` | 实例级权限，格式 `{attr_index: (AccessMode, Authentication)}` |

`执行时间表达式`

```python
(year, month, day, hour, minute, second[, day_of_week])
```

| 位置 | 字段 | 范围 | 说明 |
|------|------|------|------|
| 0 | `year` | 任意整数 / `ANY` | 年份 |
| 1 | `month` | 1–12 / `ANY` | 月份 |
| 2 | `day` | 1–31 / `ANY` | 日 |
| 3 | `hour` | 0–23 / `ANY` | 小时 |
| 4 | `minute` | 0–59 / `ANY` | 分钟 |
| 5 | `second` | 0–59 / `ANY` | 秒 |
| 6 | `day_of_week` | 0–7 / `ANY` | 1 为周一，7 为周日；ANY（-1）表示不限星期 |

> 任意位置使用 `dlms.ANY`（值为 `-1`）表示通配，如 `(ANY, ANY, ANY, 2, 0, 0, ANY)` = 每天 02:00:00。6 元组（省略 `day_of_week`）等价于 7 元组末尾补 `-1`。`dlms.ANY` = `-1`。
```python
import dlms
from dlms import AccessMode, Authentication

A = dlms.ANY

# 准备脚本
st = dlms.ScriptTable("0.0.10.0.0.255")
r1 = dlms.Register("1.1.1.8.0.255",0)
r2 = dlms.Register("1.1.2.8.0.255",0)
st.add_script(id=1, actions=[dlms.ScriptAction(
    type=dlms.ScriptAction.Write, target=r1, attribute=2, parameter=150
)])
st.add_script(id=2, actions=[dlms.ScriptAction(
    type=dlms.ScriptAction.Write, target=r2, attribute=2, parameter=80
)])

#写法1：
# sas = dlms.SingleActionSchedule("0.0.15.0.1.255",
#     executed_script=(st, 1),
#     execution_times=[(dlms.ANY, dlms.ANY, dlms.ANY, 2, 0, 0, dlms.ANY)],
#     access={2: (AccessMode.READ_WRITE, Authentication.HIGH)})

#写法2（主要介绍属性）：
# ── 属性 1: logical_name ──
sas = dlms.SingleActionSchedule("0.0.15.0.0.255")
# 只读，返回 6 字节 OBIS 代码
print(sas.logical_name)  # b'\x00\x00\x0f\x00\x00\xff'

# ── 属性 2: executed_script ──
# 写入: (ScriptTable, selector=script_id)
sas.executed_script = (st, 1)
# 读取: 返回 (字符串, int) — 注意不是 (对象, int)!
print(sas.executed_script)  # ("0.0.10.0.0.255", 1)

# ── 属性 3: execution_type ──
print(sas.execution_type)  # 1（TYPE1）
sas.execution_type = 1     # 可写

# ── 属性 4: execution_times ──
# 写入：支持 多项 6 元组（省略 dow，自动补 -1）或 7 元组
sas.execution_times = [
    (A, A, A, 2, 0, 0),           # 6 元组: 每天 02:00:00
    (A, A, A, 12, 0, 0, -1),      # 7 元组: 每天 12:00:00，忽略星期
    (2026, 1, 1, 0, 0, 0, -1),    # 一次性: 2026-01-01 00:00:00
    (A, A, A, 8, 0, 0, 1),        # 每周一 08:00:00
]

# 读取：始终返回 7 元组
for t in sas.execution_times:
    print(t)  # (2020, 1, 1, 2, 0, 0, -1), ...

# 追加时间（读取 → 修改列表 → 写回）
times = sas.execution_times
times.append((A, A, A, 18, 0, 0))  # 追加每天 18:00
sas.execution_times = times

# ── 钩子（继承自 CosemObject）──
def on_before_write(sender, attr_index, value, context):
    print(f"Write to attr {attr_index}: {value}")
    return dlms.DLMSEvent.ALLOW

sas.on_before_write = on_before_write

```
`Python 属性一览`

| 属性 | 类型 | 可写 | 说明 |
|------|------|:--:|------|
| `logical_name` | `bytes` | × | OBIS 代码（6 字节） |
| `executed_script` | `tuple` | √ | `(script_table, script_id)` 要执行的脚本 |
| `execution_type` | `int` | √ | 执行类型，固定为 `1` |
| `execution_times` | `list` | √ | 执行时间表达式列表 |
| `on_before_read` | `Callable` | √ | 读前钩子（继承自 CosemObject） |
| `on_after_read` | `Callable` | √ | 读后钩子（继承自 CosemObject） |
| `on_before_write` | `Callable` | √ | 写前钩子（继承自 CosemObject） |
| `on_after_write` | `Callable` | √ | 写后钩子（继承自 CosemObject） |
| `on_before_action` | `Callable` | √ | 动作前钩子（继承自 CosemObject） |
| `on_after_action` | `Callable` | √ | 动作后钩子（继承自 CosemObject） |
| `access_dict` | `dict` | √ | 实例级访问控制 |

`方法`

| 方法 | 说明 |
|------|------|
| `deinit()` | 释放 C 侧资源 |

`访问控制`

```python
import dlms
schedule = dlms.SingleActionSchedule("0.0.15.0.1.255",
    executed_script=(script_table, 1),
    execution_times=[(dlms.ANY, dlms.ANY, dlms.ANY, 2, 0, 0, dlms.ANY)],
    access={2: (dlms.AccessMode.READ_WRITE, dlms.Authentication.HIGH)})

# 或事后修改
schedule.access_dict = {2: (dlms.AccessMode.READ_WRITE, dlms.Authentication.HIGH)}
```

`典型用例`

| 场景 | 示例 |
|------|------|
| 每天凌晨 2:00 执行抄表脚本 | `execution_times=[(ANY, ANY, ANY, 2, 0, 0, ANY)]` |
| 每月 1 号 0:00 执行结算脚本 | `execution_times=[(ANY, 1, 1, 0, 0, 0, ANY)]` |
| 每周一 8:00 上报周报 | `execution_times=[(ANY, ANY, ANY, 8, 0, 0, 1)]` |
---

### `RegisterMonitor`
阈值监控器（其 **COSEM ID=21**）。监控一个 DLMS 对象的某个属性值，当值跨越预设阈值时自动触发 `ScriptTable` 脚本。C 层后台线程每秒轮询一次；调用 `server.monitor()` 可立即手动检查。

`Blue Book 属性`

| 编号 | 名称 | 类型 | 说明 |
|------|------|------|------|
| 1 | `logical_name` | octet-string(6) | OBIS 代码，只读 |
| 2 | `thresholds` | ARRAY of int | 阈值列表，从低到高排序 |
| 3 | `monitored_value` | STRUCT | 被监视的 `(dlms_object, attribute_index)`  |
| 4 | `actions` | ARRAY of STRUCT | 每个阈值对应一对 "up"/"down" 动作脚本|

> **RegisterMonitor没有 COSEM 动作方法**，不会触发 `on_before_action` / `on_after_action`。

`构造函数`

```python
RegisterMonitor(logical_name: str, access: dict = None)
```

| 参数 | 类型 | 默认值 | 说明 |
|------|------|--------|------|
| `logical_name` | `str` | 必传 | OBIS 代码字符串，如 `"0.0.16.1.0.255"` |
| `access` | `dict` | `None` | 实例级权限，格式 `{attr_index: (AccessMode, Authentication)}` |
```python
import dlms
from dlms import AccessMode, Authentication

reg = dlms.Register("1.0.1.8.0.255",0)
st = dlms.ScriptTable("0.0.10.0.0.255")
r_relay = dlms.Register("1.1.1.8.0.255",0)

st.add_script(id=1, actions=[dlms.ScriptAction(
    type=dlms.ScriptAction.Write, target=r_relay, attribute=2, parameter=1
)])
st.add_script(id=2, actions=[dlms.ScriptAction(
    type=dlms.ScriptAction.Write, target=r_relay, attribute=2, parameter=0
)])

rm = dlms.RegisterMonitor(
    "0.0.16.1.0.255",
    access={2: (AccessMode.READ_WRITE, Authentication.HIGH)})

# ── 属性 2: thresholds ──
# 写入时自动同步 lastValues（内部数组），支持 int 和 float
rm.thresholds = [1000, 5000, 10000]
print(rm.thresholds)  # [1000, 5000, 10000]
# ── 属性 3: monitored_value ──
rm.monitored_value = (reg, reg.idx('value')) 
print(rm.monitored_value)      # (<Register ...>, 2)

# ── 属性 4: actions ──
# 长度必须与 thresholds 一致，down 可以设为 None
rm.thresholds = [1000, 5000]
rm.actions = [
    {"up": (st, 1), "down": (st, 2)},   # 阈值 1000：双向动作
    {"up": (st, 1), "down": None},      # 阈值 5000：仅 up 动作
]
# 关键步骤：注册到 server 后，find_python_object_by_c_ptr 才能工作
# 不注册server 返回值是(None, 1) (None, 2) (None, 1) (None, 0)
for a in rm.actions:
    print(a["up"], a["down"])  # (<ScriptTable...>, 1) , (<ScriptTable...>, 2)

# ── 钩子 ──
def on_before_read(sender, attr_index, context):
    if attr_index == 3:
        print("About to read monitored_value")
    return dlms.DLMSEvent.ALLOW

rm.on_before_read = on_before_read

```
`Python 属性一览`

| 属性 | 类型 | 可写 | 说明 |
|------|------|:--:|------|
| `logical_name` | `bytes` | × | OBIS 代码（6 字节） |
| `thresholds` | `list[int \| float]` | √ | 阈值列表，从低到高排序 |
| `monitored_value` | `tuple \| None` | √ | `(dlms_object, attribute_index)` 被监视的目标 |
| `actions` | `list[dict]` | √ | 每个阈值一条，`"up"`/`"down"` 键对应 `(ScriptTable, selector)` 或 `None` |
| `on_before_read` | `Callable` | √ | 读前钩子（继承自 CosemObject） |
| `on_after_read` | `Callable` | √ | 读后钩子（继承自 CosemObject） |
| `on_before_write` | `Callable` | √ | 写前钩子（继承自 CosemObject） |
| `on_after_write` | `Callable` | √ | 写后钩子（继承自 CosemObject） |
| `access_dict` | `dict` | √ | 实例级访问控制 |

`方法`

| 方法 | 说明 |
|------|------|
| `deinit()` | 释放 C 侧资源 |

`工作方式`

```
server.run() 启动后，后台线程每秒轮询一次：
  monitored_value → 当前值 = reg.value
  比较 thresholds 列表 → 找到所处的区间
  如果跨越了阈值 → 触发对应的 "up" 或 "down" 脚本

server.monitor()  → 立即手动触发一次检查（无需等待下一秒）
```

`actions 格式`

```python
#仅解释参考，程序不完整
rm.actions = [
    {"up": (script_table, 1), "down": (script_table, 2)},   # 阈值 1 的动作
    {"up": (script_table, 3), "down": None},                 # 阈值 2 只有 up 动作
]
# "up"   → 值从下方跨越此阈值时触发
# "down" → 值从上方跨越此阈值时触发
# None   → 此方向不触发任何动作
```

`访问控制`

```python
import dlms
rm = dlms.RegisterMonitor("0.0.16.1.0.255",
    access={2: (dlms.AccessMode.READ_WRITE, dlms.Authentication.HIGH)})

# 或事后修改
rm.access_dict = {2: (dlms.AccessMode.READ_WRITE, dlms.Authentication.HIGH)}
```

`典型用例`

| 场景 | 示例 |
|------|------|
| 低电量告警 + 过载断电 | `thresholds = [1000, 50000]`, 低于 1000 告警 / 高于 50000 断电 |
| 欠费跳闸（单阈值） | `thresholds = [0]`, 余额向下跨 0 时断开 |
| 多级电价切换 | `thresholds = [1000, 5000, 10000]`, 各阈值触发不同费率脚本 |
---
### `PushSetup`
数据主动推送控制器（其 **COSEM ID=40**）。定义"推送什么数据、推到哪个地址、在什么时间窗口内发送"。C 层负责将 `objectList` 中所有对象的当前值序列化为 DLMS `DATA-NOTIFICATION` PDU，Python 侧通过 `generate_pdu()` 获取字节后自行选择传输方式发送。

`Blue Book 属性`

| 编号 | 名称 | 类型 | 说明 |
|------|------|------|------|
| 1 | `logical_name` | octet-string(6) | OBIS 代码，只读 |
| 2 | `objectList` | ARRAY of STRUCT | 推送的数据项列表 |
| 3 | `destination` | visible-string | 目标地址，`"ip:port"` 格式 |
| 4 | `communicationWindow` | ARRAY of STRUCT | 推送时间窗口列表 |
| 5 | `randomisationStartInterval` | uint16 | 随机延迟区间（秒），防止多设备同时推送 |
| 6 | `retries` | uint8 | 重试次数 |
| 7 | `retryDelay` | uint32 | 重试间隔（秒） |

```python
import dlms
from dlms import AccessMode, Authentication
A = dlms.ANY

# ── 准备对象 ──
reg = dlms.Register("1.0.1.8.0.255", 12345)
clock = dlms.Clock("0.0.1.0.0.255")

ps = dlms.PushSetup("0.0.25.9.0.255")

# ── 属性 1: logical_name ──
print(ps.logical_name)  # b'\x00\x00\x19\t\x00\xff'

# ── 属性 2: objectList ──
ps.objectList = [reg, (clock, 2, 0)]  # 混合格式
# 需要先注册到 server 后才能输出 (<Register...>, 2, 0) , (<Clock...>, 2, 0)
# 否则输出的是(None, 2, 0) , (None, 2, 0)
# 没有 server 时 py_obj 为 None（C→Python 反向查找需要注册表）,
# 但 attr_idx/data_idx 始终正确，不影响 C 层数据 仅例子只展示属性如何使用
for item in ps.objectList:
    print(item) 

# ── 属性 3: destination ──
ps.destination = "10.0.0.1:4059"
print(ps.destination)  # "10.0.0.1:4059"

# ── 属性 4: communicationWindow ──
ps.communicationWindow = [
    [(-1, -1, -1,  8, 0, 0), (-1, -1, -1, 20, 0, 0)],
]

# ── 属性 5/6/7 ──
# 场景 1: 宽松（信号差，允许长时间重试）
ps.randomisationStartInterval = 10   # 最多随机延迟 10s
ps.retries = 5                        # 最多重试 5 次
ps.retryDelay = 120                   # 每次间隔 2 分钟
# 总耗时上限 ≈ 10 + 5×(120+send_time) ≈ 10 分钟

# 场景 2: 紧凑（信号好，失败快速放弃）
ps.randomisationStartInterval = 1    # 最多延迟 1s
ps.retries = 1                        # 只重试 1 次
ps.retryDelay = 10                    # 10s 后重试
# 总耗时上限 ≈ 1 + 1×(10+send_time) ≈ 11 秒

# 场景 3: 不重试（数据不重要，丢了就丢了）
ps.retries = 0                        # 不重试
# randomisationStartInterval 仍然生效

# ── generate_pdu() ──
# 触发 on_before_read 钩子后序列化所有对象值
def on_before_read(sender, attr_index, context):
    print("PushSetup reading attr {}".format(attr_index))
    return dlms.DLMSEvent.ALLOW

ps.on_before_read = on_before_read
# pdu = ps.generate_pdu()  # 返回 bytes，可用 socket.send(pdu)

# ── Push action 钩子 ──
def on_push_action(sender, event):
    if event.index == 1:
        pdu = sender.generate_pdu()
        # mobile_conn.send(pdu)  ← Python 侧传输
        print("Pushed {} bytes".format(len(pdu)))
        return True

ps.on_before_action = on_push_action

# ── 清理 ──
ps.deinit(ps)   # dest[1] bug, 需手动传 self
reg.deinit(reg)
clock.deinit(clock)
```

`构造函数`

```python
PushSetup(logical_name: str, objectList: list = None, destination: str = None,
          retries: int = 3, retryDelay: int = 60,
          randomisationStartInterval: int = 0,
          communicationWindow: list = None)
```
```python
import dlms
A = dlms.ANY

# 准备被推送的对象
reg = dlms.Register("1.0.1.8.0.255", 0)
clock = dlms.Clock("0.0.1.0.0.255")

# 创建 PushSetup
ps = dlms.PushSetup(
    "0.0.25.9.0.255",
    objectList=[(reg, 2, 0), (clock, 2, 0)],
    destination="192.168.1.100:4059",
    retries=3,
    retryDelay=60,
    randomisationStartInterval=5,
)
# 设置通信窗口
ps.communicationWindow = [
    [(-1, -1, -1,  9, 0, 0), (-1, -1, -1, 12, 0, 0)],
    [(-1, -1, -1, 14, 0, 0), (-1, -1, -1, 17, 0, 0)],
]
```

| 参数 | 类型 | 默认值 | 说明 |
|------|------|--------|------|
| `logical_name` | `str` | 必传 | OBIS 代码字符串，如 `"0.0.25.9.0.255"` |
| `objectList` | `list` | `None` | 推送的数据项，每项为 `(obj, attr_idx, data_idx)` 三元组 |
| `destination` | `str` | `None` | 目标地址，`"ip:port"` 格式 |
| `retries` | `int` | `3` | 推送失败重试次数 |
| `retryDelay` | `int` | `60` | 重试间隔（秒） |
| `randomisationStartInterval` | `int` | `0` | 随机延迟区间（秒），避免多设备集中推送 |
| `communicationWindow` | `list` | `None` | 推送允许时间窗口，`[[start_tuple, end_tuple], ...]` |

`Python 属性一览`

| 属性 | 类型 | 可写 | 说明 |
|------|------|:--:|------|
| `logical_name` | `bytes` | × | OBIS 代码（6 字节） |
| `objectList` | `list` | √ | `(obj, attr_idx, data_idx)` 三元组列表 |
| `destination` | `str` | √ | 目标 `"ip:port"` 地址 |
| `retries` | `int` | √ | 重试次数 |
| `retryDelay` | `int` | √ | 重试间隔（秒） |
| `randomisationStartInterval` | `int` | √ | 随机延迟区间（秒） |
| `communicationWindow` | `list` | √ | 推送时间窗口 `[[start, end], ...]` |
| `on_before_read` | `Callable` | √ | 读前钩子（继承自 CosemObject） |
| `on_after_read` | `Callable` | √ | 读后钩子（继承自 CosemObject） |
| `on_before_write` | `Callable` | √ | 写前钩子（继承自 CosemObject） |
| `on_after_write` | `Callable` | √ | 写后钩子（继承自 CosemObject） |
| `on_before_action` | `Callable` | √ | 动作前钩子（继承自 CosemObject） |
| `on_after_action` | `Callable` | √ | 动作后钩子（继承自 CosemObject） |
| `access_dict` | `dict` | √ | 实例级访问控制 |

`方法`

| 方法 | 说明 |
|------|------|
| `generate_pdu()` | 编码当前属性值为 DLMS DATA-NOTIFICATION PDU，返回 `bytes` |
| `deinit()` | 释放 C 侧资源 |

`生成推送流程`

```
客户端调用 COSEM 方法 1（push）
        │
        ▼
on_before_action(self, event)  ← 你的 handler
        │
        ├── self.generate_pdu() →  bytes（DATA-NOTIFICATION PDU）
        │
        └── mobile_conn.send(pdu) → 通过 relay UDP 发出
```

**`generate_pdu()` 做了 3 件事：**
1. 读取 `objectList` 中所有对象的当前属性值
2. 按 DLMS 结构格式序列化
3. 包装为 DATA-NOTIFICATION 命令，返回 `bytes`

**你的 handler 只负责传输：**调用 `generate_pdu()` 拿数据 → `mobile_conn.send(pdu)` 发出。
`访问控制`

```python
# ⚠️ 构造时不支持 access 参数
ps = dlms.PushSetup("0.0.25.9.0.255",
    objectList=[(reg, 2, 0)],
    destination="192.168.1.100:4059")

# 全局默认访问控制（推荐方式）
dlms.set_default_access(dlms.PushSetup, {
    dlms.Authentication.NONE: {
        2: dlms.AccessMode.READ,
        3: dlms.AccessMode.READ,
        4: dlms.AccessMode.READ,
    },
})
```
`典型用例`

| 场景 | 示例 |
|------|------|
| 定期上报电能和时间 | `objectList=[(reg, 2, 0), (clock, 2, 0)]`, `destination="203.0.113.1:4060"` |
| 仅在白天推送 | `communicationWindow=[[(-1,-1,-1,6,0,0), (-1,-1,-1,22,0,0)]]` |
| 告警主动推送 | `objectList=[(event_code, 2, 0)]`, `retries=5` |
---

### `GsmDiagnostic`
蜂窝网络实时诊断对象。DLMS 客户端可远程读取设备连接状态。调用 `update()` 从 QuecPython `net` 模块刷新所有字段；配合 `on_before_read` 可在每次客户端读取前自动刷新，其**COSEM ID=47，OBIS 0.0.25.6.0.255**。

`Blue Book 属性`

| 编号 | 名称 | 类型 | 说明 |
|------|------|------|------|
| 1 | `logical_name` | octet-string(6) | OBIS 代码，只读 |
| 2 | `operator_name` | visible-string | 运营商名称，如 `"A1 Srbija"`，不可用时为 `None` |
| 3 | `status` | enum | 网络注册状态（⏩ VOLATILE） |
| 4 | `circuit_switch_status` | enum | 电路交换连接状态（⏩ VOLATILE） |
| 5 | `packet_switch_status` | enum | 数据技术类型（⏩ VOLATILE） |
| 6 | `cell_info` | STRUCT | 服务小区详细信息（⏩ COMPLEX + VOLATILE） |
| 7 | `adjacentCells` | array | 只读，`adjacent_cells` 返回 `AdjacentCell` 列表副本 |
| 8 | `captureTime` | octet-string | C 端存储但 Python 未暴露 getter |

> 构造函数只有 `logical_name` 一个参数，无 `access` 参数。

`构造函数`

```python
GsmDiagnostic(logical_name: str)
```

| 参数 | 类型 | 默认值 | 说明 |
|------|------|--------|------|
| `logical_name` | `str` | 必传 | OBIS 代码字符串，如 `"0.0.25.6.0.255"` |

```python
import dlms

gsm = dlms.GsmDiagnostic("0.0.25.6.0.255")

# ── 属性 1: logical_name ──
print(gsm.logical_name)  # b'\x00\x00\x19\x06\x00\xff'

# ── 属性 2: operator_name ──
gsm.operator_name = "China Mobile"
print(gsm.operator_name)  # "China Mobile"

# ── 属性 3: status ──
print(gsm.status)  # 4 (初始默认 UNKNOWN)
gsm.status = 1      # 手动设为 HOME

# ── 属性 4/5: CS/PS 状态 ──
print(gsm.circuit_switch_status, gsm.packet_switch_status)  # 0, 0

# ── update（需 modem 在线）──
gsm.update()
print(gsm.operator_name)         # 真实运营商
print(gsm.status)                # 真实注册状态
print(gsm.packet_switch_status)  # 1=GPRS / 5=LTE

# ── 属性 6: cell_info（只读，每次新建副本）──
ci = gsm.cell_info
print(ci.cell_id, ci.location_id, ci.signal_quality)
print(ci.mobile_country_code, ci.mobile_network_code)

# ── 属性 7: adjacent_cells（只读）──
print(gsm.adjacent_cells_count)
for adj in gsm.adjacent_cells:
    print("  cell=%d, signal=%d" % (adj.cell_id, adj.signal_quality))

# ── Legacy 快捷属性 ──
print(gsm.cell_id, gsm.signal_quality)  # 同 ci.cell_id, ci.signal_quality
gsm.signal_quality = -75                # 可直接写 embedded 结构体

# ── 钩子：读前自动刷新 ──
def on_before_read(sender, attr_index, context):
    if attr_index == 3:  # status 被读取前
        sender.update()
    return dlms.DLMSEvent.ALLOW

gsm.on_before_read = on_before_read

```
`Python 属性一览`

| 属性 | 类型 | 可写 | 说明 |
|------|------|:--:|------|
| `logical_name` | `bytes` | × | OBIS 代码（6 字节） |
| `operator_name` | `str\|None` | √ | 运营商名称，`update()` 自动填充 |
| `status` | `int` | √ | 注册状态：0=未注册, 1=归属, 2=搜索中, 3=被拒, 4=未知, 5=漫游 |
| `circuit_switch_status` | `int` | √ | CS 域状态：0=非活跃, 1=活跃, 2=未知 |
| `packet_switch_status` | `int` | √ | PS 域制式：0=非活跃, 1=GPRS, 5=LTE…（`update()` 根据服务小区自动判定） |
| `cell_info` | `GsmCellInfo` | × | **只读**，每次读取 `m_new_obj` + `memcpy` 新建副本 |
| `adjacent_cells` | `list[AdjacentCell]` | × | **只读**，每次读取新建副本列表 |
| `adjacent_cells_count` | `int` | × | **只读**，邻区总数 |
| `cell_id` | `int` | √ | **Legacy**：等价 `cell_info.cell_id`，直接读写嵌入的 `cellInfo.cellId` |
| `location_id` | `int` | √ | **Legacy**：等价 `cell_info.location_id` |
| `signal_quality` | `int` | √ | **Legacy**：等价 `cell_info.signal_quality` |
| `ber` | `int` | √ | **Legacy**：等价 `cell_info.ber` |
| `mobile_country_code` | `int` | √ | **Legacy**：等价 `cell_info.mobile_country_code` |
| `mobile_network_code` | `int` | √ | **Legacy**：等价 `cell_info.mobile_network_code` |
| `channel_number` | `int` | √ | **Legacy**：等价 `cell_info.channel_number` |
| `on_before_read` | `Callable` | √ | 读前钩子（继承自 CosemObject） |
| `on_after_read` | `Callable` | √ | 读后钩子 |
| `on_before_write` | `Callable` | √ | 写前钩子 |
| `on_after_write` | `Callable` | √ | 写后钩子 |
| `access_dict` | `dict` | √ | 继承自 CosemObject，构造时不支持传入 |

> **Legacy 属性**：`cell_id`、`location_id` 等 7 个字段直接映射到嵌入的 `gxGSMCellInfo` 结构体，等价于 `cell_info.xxx` 的快捷方式。保留是为了向后兼容。

`注册状态常量（`status`）`

| 值 | 常量 | 说明 |
|----|------|------|
| `0` | NOT_REGISTERED | 未注册到任何网络 |
| `1` | HOME_NETWORK | 已注册到归属网络 |
| `2` | SEARCHING | 正在搜索网络 |
| `3` | DENIED | 注册被拒绝 |
| `4` | UNKNOWN | 状态未知 |
| `5` | ROAMING | 漫游中 |

`数据技术常量（`packet_switch_status`）`

| 值 | 说明 |
|----|------|
| `0` | INACTIVE |
| `1` | GPRS |
| `2` | EGPRS |
| `3` | UMTS |
| `4` | HSDPA |
| `5` | LTE |

| 方法 | 说明 |
|------|------|
| `update()` | 调用 QuecPython `net` 模块（`net.getCellInfo()` / `net.getSignal()` / `net.operatorName()` / `net.getState()`）刷新所有属性。网络不可用时仅打印警告，不抛异常 |

`GsmCellInfo — 服务小区详情`

| 属性 | 类型 | 说明 |
|------|------|------|
| `cell_id` | `int` | 小区标识（CID） |
| `location_id` | `int` | 位置区码（LAC）或跟踪区码（TAC for LTE） |
| `signal_quality` | `int` | 接收信号强度（dBm，通常为负值） |
| `ber` | `int` | 误码率等级（0–7，GSM 05.08） |
| `mobile_country_code` | `int` | 移动国家码（MCC，如 220 表示塞尔维亚） |
| `mobile_network_code` | `int` | 移动网络码（MNC） |
| `channel_number` | `int` | ARFCN / UARFCN / EARFCN |

`AdjacentCell — 邻小区`

| 属性 | 类型 | 说明 |
|------|------|------|
| `cell_id` | `int` | 邻小区标识 |
| `signal_quality` | `int` | 接收信号强度（dBm） |

> `GsmDiagnostic` 上同时暴露旧式快捷访问属性（`cell_id`、`location_id`、`signal_quality`、`ber`、`mobile_country_code`、`mobile_network_code`、`channel_number`），它们直接映射到 `cell_info` 的同名字段。

`访问控制`

```python
import dlms
from dlms import AccessMode, Authentication

gsm = dlms.GsmDiagnostic("0.0.25.6.0.255")

dlms.set_default_access(dlms.GsmDiagnostic, {
    dlms.Authentication.NONE: {
        2: dlms.AccessMode.READ,  # operator_name
        3: dlms.AccessMode.READ,  # status
        4: dlms.AccessMode.READ,  # circuit_switch_status
        5: dlms.AccessMode.READ,  # packet_switch_status
        6: dlms.AccessMode.READ,  # cell_info
    },
})
```
`典型用例`

| 场景 | 示例 |
|------|------|
| 远程诊断信号强度 | `gsm.update(); print(gsm.cell_info.signal_quality)` |
| 判断是否在归属网络 | `if gsm.status == 1: print("Home")` |
| 扫描邻小区数量 | `print(gsm.adjacent_cells_count)` |
---

### `SecuritySetup`
DLMS 安全配置对象（其 **COSEM ID=64**）。定义服务器端的安全策略、加密套件、系统标题、密钥和证书。每个 `AssociationLogicalName` / `AssociationShortName` 可通过 `security_setup` 属性引用一个 SecuritySetup 实例，从而为不同逻辑设备绑定独立的安全配置。

> **初始化副作用**：构造函数在 `bb_init` 后检查全局 `SERVER_SYSTEM_TITLE[8]` 数组，若非空则自动设置 `serverSystemTitle`。写入 `server_system_title` 时还会同步更新 `serverSettings.base.cipher.systemTitle`（C 层加密引擎实际使用的值）。

`Blue Book 属性`

| 编号 | 名称 | 类型 | 说明 |
|------|------|------|------|
| 1 | `logical_name` | octet-string(6) | OBIS 代码，只读 |
| 2 | `server_system_title` | octet-string(8) | 服务端系统标题（8 字节） |
| 3 | `client_system_title` | octet-string(8) | 客户端系统标题（8 字节） |
| 4 | `security_policy` | enum | 安全策略（见 SecurityPolicy 表） |
| 5 | `security_suite` | enum | 安全套件版本（0/1/2） |
| 6 | `certificates` | ARRAY of STRUCT | 证书列表（⏩ COMPLEX） |
| 7 | `min_invocation_counter` | uint32 | 最低调用计数器 |
| 8 | `gak` | octet-string | 全局认证密钥（16 或 32 字节） |
| 9 | `guek` | octet-string | 全局单播加密密钥（Block Cipher Key，16 或 32 字节） |
| 10 | `gbek` | octet-string | 全局广播加密密钥 |

> 构造函数只有 `logical_name` 一个参数，无 `access` 参数。

`构造函数`

```python
SecuritySetup(logical_name: str)
```

| 参数 | 类型 | 说明 |
|------|------|------|
| `logical_name` | `str` | OBIS 代码字符串，如 `"0.0.43.0.1.255"` |

`关联的枚举类型`

`SecurityPolicy` — 定义消息级安全保护策略（`dlms.SecurityPolicy.XXX`）：

| 常量 | 值 | 说明 |
|------|:--:|------|
| `SecurityPolicy.NOTHING` | `0` | 无保护 |
| `SecurityPolicy.AUTHENTICATED` | `1` | 仅认证（Suite V0） |
| `SecurityPolicy.ENCRYPTED` | `2` | 仅加密（Suite V0） |
| `SecurityPolicy.AUTHENTICATED_ENCRYPTED` | `3` | 认证+加密（Suite V0；HighGMac 预建立关联用此值） |
| `SecurityPolicy.AUTHENTICATED_REQUEST` | `0x4` | 请求认证（Suite V1） |
| `SecurityPolicy.ENCRYPTED_REQUEST` | `0x8` | 请求加密（Suite V1） |
| `SecurityPolicy.DIGITALLY_SIGNED_REQUEST` | `0x10` | 请求签名（Suite V1） |
| `SecurityPolicy.AUTHENTICATED_RESPONSE` | `0x20` | 响应认证（Suite V1） |
| `SecurityPolicy.ENCRYPTED_RESPONSE` | `0x40` | 响应加密（Suite V1） |
| `SecurityPolicy.DIGITALLY_SIGNED_RESPONSE` | `0x80` | 响应签名（Suite V1） |

> **V0 是互斥单选**（0–3），**V1 是位掩码组合**（0x4–0x80 可 `\|` 组合）。`SecurityPolicy` 枚举本身不区分版本，具体语义由 `security_suite` 决定。

```python
import dlms
from dlms import SecurityPolicy, CertificateEntity, CertificateType

sec = dlms.SecuritySetup("0.0.43.0.1.255")

# ── 属性 1: logical_name ──
print(sec.logical_name)  # b'\x00\x00+\x00\x01\xff'

# ── 属性 2: server_system_title ──
# 写入：同时更新 serverSettings.base.cipher.systemTitle（C 层加密引擎）
sec.server_system_title = b'GRX12345'
print(sec.server_system_title)  # b'GRX12345'

# ── 属性 3: client_system_title ──
sec.client_system_title = b'CLI12345'
print(sec.client_system_title)  # b'CLI12345'

# ── 属性 4: security_policy ──
sec.security_policy = SecurityPolicy.AUTHENTICATED_ENCRYPTED  # Suite V0: 认证+加密
# Suite V1 位掩码组合:
# sec.security_policy = SecurityPolicy.AUTHENTICATED_REQUEST | SecurityPolicy.ENCRYPTED_REQUEST

# ── 属性 5: security_suite ──
sec.security_suite = 1  # V1: AES-GCM-128 + ECDSA P-256

# ── 属性 6: certificates ──
sec.certificates = [{
    "entity":         CertificateEntity.SERVER,
    "type":           CertificateType.DIGITAL_SIGNATURE,
    "serial_number":  "123456",
    "issuer":         "CN=Test CA",
    "subject":        "CN=Test Server",
    "subject_alt_name": "",
}]
for cert in sec.certificates:
    print(cert["serial_number"])  # "123456"

# ── 属性 7: min_invocation_counter ──
sec.min_invocation_counter = 0
print(sec.min_invocation_counter)  # 0

# ── 属性 8/9/10: 密钥 ──
sec.gak  = b'\x00' * 16   # 认证密钥，16 或 32 字节
sec.guek = b'\x01' * 16   # 单播加密密钥
sec.gbek = b'\x02' * 16   # 广播加密密钥

# ── 绑定到 Association ──
assoc = dlms.AssociationLogicalName("0.0.40.0.1.255")
assoc.auth_mechanism = "HighGMac"
assoc.clientSAP = 0x10
assoc.security_setup = sec  # 一个 SecuritySetup 可被多个 Association 共享

# ── 调用计数器零拷贝暴露 ──
# 用 nocopy Data 对象直接引用 SecuritySetup 的 min_invocation_counter（属性 7→6）
inv = dlms.Data("0.0.43.1.0.255", nocopy=True,
    access={dlms.Authentication.NONE: {2: dlms.AccessMode.READ}})
inv.value = (sec, 6)  # nocopy 引用 SecuritySetup 的 min_invocation_counter

```

`Python 属性一览`

| 属性 | 类型 | 可写 | 说明 |
|------|------|:--:|------|
| `logical_name` | `bytes` | × | OBIS 代码（6 字节） |
| `security_policy` | `int` | √ | 安全策略（SecurityPolicy 枚举） |
| `security_suite` | `int` | √ | 安全套件版本（0/1/2） |
| `min_invocation_counter` | `int` | √ | 最低调用计数器 |
| `server_system_title` | `bytes` | √ | 服务端系统标题（8 字节） |
| `client_system_title` | `bytes` | √ | 客户端系统标题（8 字节） |
| `guek` | `bytes` | √ | 全局单播加密密钥 |
| `gak` | `bytes` | √ | 全局认证密钥 |
| `gbek` | `bytes` | √ | 全局广播加密密钥 |
| `certificates` | `list` | √ | 证书列表，每项为 dict |
| `on_before_read` | `Callable` | √ | 读前钩子（继承自 CosemObject） |
| `on_after_read` | `Callable` | √ | 读后钩子（继承自 CosemObject） |
| `on_before_write` | `Callable` | √ | 写前钩子（继承自 CosemObject） |
| `on_after_write` | `Callable` | √ | 写后钩子（继承自 CosemObject） |
| `access_dict` | `dict` |  | 实例级访问控制 |

`方法`

| 方法 | 说明 |
|------|------|
| `deinit()` | 释放 C 侧资源 |

`访问控制`

```python
import dlms
from dlms import AccessMode, Authentication

sec = dlms.SecuritySetup("0.0.43.0.1.255")
sec.security_policy = dlms.SecurityPolicy.AUTHENTICATED_ENCRYPTED

# 全局默认访问控制（推荐方式）
dlms.set_default_access(dlms.SecuritySetup, {
    dlms.Authentication.HIGH: {
        2: dlms.AccessMode.READ,     # server_system_title
        3: dlms.AccessMode.READ,     # client_system_title
        4: dlms.AccessMode.READ,     # security_policy
        5: dlms.AccessMode.READ,     # security_suite
        6: dlms.AccessMode.READ,     # certificates
        7: dlms.AccessMode.READ,     # min_invocation_counter
    },
})
```

### 网络传输对象

以下对象配置 DLMS 通信栈：`IecHdlcSetup`（HDLC 帧参数）、`LocalPortSetup`（光口 Mode E 协商）、`GprsSetup`（蜂窝 APN）、`IPv4Setup`（IP 地址）、`TcpUdpSetup`（TCP/UDP 端口）、`MacAddressSetup`（MAC 地址）。

---

#### `IecHdlcSetup`
IEC 62056-46 HDLC 链路层参数配置对象（其 **COSEM ID=23**）。定义 HDLC 通信的物理/链路层参数：波特率、窗口大小、帧长度、超时和设备地址。该实例通过 `dlms.set_hdlc(hdlc)` 注册为全局 HDLC 配置（驱动 `SerialConnection` 等连接类型）。

> **波特率内部转换**：`commSpeed` 在 C 层以 `DLMS_BAUD_RATE` 枚举（1 字节）存储（编译器 `-fshort-enums`），Python 读写时自动通过 `baud_int_to_enum`/`baud_enum_to_int` 在整数和枚举索引间转换。支持的速率：300, 600, 1200, 2400, 4800, 9600, 19200, 38400, 57600, 115200。

`Blue Book 属性`

| 编号 | 名称 | 类型 | 说明 |
|------|------|------|------|
| 1 | `logical_name` | octet-string(6) | OBIS 代码 只读，`bytes`|
| 2 | `comm_speed` | enum | 通信速率（baud） |
| 3 | `window_size_rx` | uint8 | 接收窗口大小 |
| 4 | `window_size_tx` | uint8 | 发送窗口大小 |
| 5 | `max_info_len_tx` | uint16 | 发送最大信息长度 |
| 6 | `max_info_len_rx` | uint16 | 接收最大信息长度 |
| 7 | `inactivity_timeout` | uint16 | 非活动超时（秒） |
| 8 | `device_address` | uint16 | 设备地址（默认 `0x10`） |
| — | `interCharachterTimeout` | uint16 |  |

```python
import dlms

hdlc = dlms.IecHdlcSetup("0.0.22.0.0.255")

# ── 属性 1: logical_name ──
print(hdlc.logical_name)  # b'\x00\x00\x16\x00\x00\xff'

# ── 属性 2: commSpeed ──
print(hdlc.commSpeed)  # 9600（默认）
hdlc.commSpeed = 19200
print(hdlc.commSpeed)  # 19200

# ── 属性 3/4: 窗口大小 ──
print(hdlc.windowSizeRx, hdlc.windowSizeTx)  # 1, 1
hdlc.windowSizeRx = 2

# ── 属性 5/6: 最大帧长度 ──
print(hdlc.maxInfoLenTx, hdlc.maxInfoLenRx)  # 128, 128
hdlc.maxInfoLenTx = 256

# ── 属性 7: timeout ──
print(hdlc.timeout)  # 120
hdlc.timeout = 60

# ── 属性 8: deviceAddr ──
print("0x%02x" % hdlc.deviceAddr)  # 0x10
hdlc.deviceAddr = 0x20

# ── 注册为全局配置 ──
dlms.set_hdlc(hdlc)

# ── 钩子 ──
def on_before_write(sender, attr_index, value, context):
    if attr_index == 2:  # commSpeed
        print("Changing baud rate to {}".format(value))
    return dlms.DLMSEvent.ALLOW

hdlc.on_before_write = on_before_write
```
`构造函数`

```python
IecHdlcSetup(logical_name: str, commSpeed: int = 9600,
             windowSizeRx: int = 1, windowSizeTx: int = 1,
             maxInfoLenTx: int = 128, maxInfoLenRx: int = 128,
             timeout: int = 120, deviceAddr: int = 0x10)
```
> 构造函数只有 `logical_name` 必传，其余参数都带默认值。无 `access` 参数。

| 参数 | 类型 | 默认值 | 说明 |
|------|------|--------|------|
| `logical_name` | `str` | 必传 | OBIS 代码，如 `"0.0.22.0.0.255"` |
| `commSpeed` | `int` | `9600` | 波特率；支持 300/600/1200/2400/4800/9600/19200/38400/57600/115200 |
| `windowSizeRx` | `int` | `1` | 接收窗口大小（HDLC 滑动窗口） |
| `windowSizeTx` | `int` | `1` | 发送窗口大小 |
| `maxInfoLenTx` | `int` | `128` | 最大发送帧信息字段长度（字节） |
| `maxInfoLenRx` | `int` | `128` | 最大接收帧信息字段长度 |
| `timeout` | `int` | `120` | 无通信超时（秒） |
| `deviceAddr` | `int` | `0x10` | HDLC 设备地址（1 字节，0x00–0xFF） |

```python
import dlms

hdlc = dlms.IecHdlcSetup(
    "0.0.22.0.0.255",
    commSpeed=9600,
    windowSizeRx=1,
    windowSizeTx=1,
    maxInfoLenTx=128,
    maxInfoLenRx=128,
    timeout=120,
    deviceAddr=0x10,
)

# 注册为全局 HDLC 配置（SerialConnection 等依赖此配置）
dlms.set_hdlc(hdlc)
```
`Python 属性一览`

| 属性 | 类型 | 可写 | 说明 |
|------|------|:--:|------|
| `logical_name` | `bytes` | × | OBIS 代码（6 字节） |
| `commSpeed` | `int` | √ | 波特率整数，读写自动转换枚举（未知值回退到 9600） |
| `windowSizeRx` | `int` | √ | 接收窗口大小 |
| `windowSizeTx` | `int` | √ | 发送窗口大小 |
| `maxInfoLenTx` | `int` | √ | 最大发送帧长度 |
| `maxInfoLenRx` | `int` | √ | 最大接收帧长度 |
| `timeout` | `int` | √ | 无通信超时（秒） |
| `deviceAddr` | `int` | √ | HDLC 设备地址（1 字节） |
| `on_before_read` | `Callable` | √ | 读前钩子 |
| `on_after_read` | `Callable` | √ | 读后钩子 |
| `on_before_write` | `Callable` | √ | 写前钩子 |
| `on_after_write` | `Callable` | √ | 写后钩子 |
| `on_before_action` | `Callable` | √ | 动作前钩子 |
| `on_after_action` | `Callable` | √ | 动作后钩子 |
| `access_dict` | `dict` | √ | 继承自 CosemObject |

> **无 `deinit`**：IecHdlcSetup 内部只有嵌入的基元字段和枚举，无动态分配的堆内存，不需要显式释放。

`波特率对照表`

| `commSpeed` | 枚举索引 | 说明 |
|:--:|:--:|------|
| `300` | 0 | |
| `600` | 1 | |
| `1200` | 2 | |
| `2400` | 3 | |
| `4800` | 4 | |
| `9600` | 5 | **默认值** |
| `19200` | 6 | |
| `38400` | 7 | |
| `57600` | 8 | |
| `115200` | 9 | |

> 传入不在上表的波特率值 → 自动回退到 9600。

`访问控制`

```python
import dlms
from dlms import AccessMode, Authentication

hdlc = dlms.IecHdlcSetup("0.0.22.0.0.255")

dlms.set_default_access(dlms.IecHdlcSetup, {
    dlms.Authentication.NONE: {
        2: dlms.AccessMode.READ,     # commSpeed: 公开读
        3: dlms.AccessMode.READ,     # windowSizeRx
        4: dlms.AccessMode.READ,     # windowSizeTx
        5: dlms.AccessMode.READ,     # maxInfoLenTx
        7: dlms.AccessMode.READ,     # timeout
    },
    dlms.Authentication.HIGH: {
        2: dlms.AccessMode.READ_WRITE,  # commSpeed: HIGH 可写
        8: dlms.AccessMode.READ,        # deviceAddr
    },
})
```

---

#### `LocalPortSetup`
IEC 62056-21 光学端口配置对象（其 **COSEM ID=19**）。定义本地光学端口（红外/串口）的 Mode E 协议协商参数：默认模式、波特率切换、响应时间、设备地址和三级密码保护。

> **与 IecHdlcSetup 的区别**：`LocalPortSetup` 配置的是**光学端口协议层**（Mode E 协商 + 波特率切换 + 密码），`IecHdlcSetup` 配置的是**HDLC 链路层**（帧格式、窗口大小、超时）。实际通信栈中两者协同：光学端口先用 Mode E 握手切换到目的波特率，然后在上层跑 HDLC。

`Blue Book 属性`

| 编号 | 名称 | 说明 |
|------|------|------|
| 1 | `logical_name` | OBIS 代码 |
| 2 | `default_mode` | 默认光口协议模式（DLMS_OPTICAL_PROTOCOL_MODE） |
| 3 | `default_baud` | 默认波特率（典型 300 bps） |
| 4 | `proposed_baud` | 协商后切换的目标波特率（9600/19200） |
| 5 | `response_time` | 响应超时（ms） |
| 6 | `device_address` | 设备地址 |
| 7 | `password_1` | P1 密码（最低安全级） |
| 8 | `password_2` | P2 密码（中等安全级） |
| 9 | `password_5` | P5 密码（最高安全级） |

```python
import dlms

# ── 创建光学端口配置 ──
port = dlms.LocalPortSetup(
    "0.0.128.0.0.255",
    default_mode=0,        # Mode E
    default_baud=0,        # 300 bps（枚举索引 0）
    proposed_baud=5,       # 9600 bps（枚举索引 5）
    response_time=0,       # 20ms
    device_address=b"MTR001",
    password_1=b"00000000",
    password_2=b"12345678",
    password_5=b"87654321",
)

# ── 属性 1: logical_name ──
print(port.logical_name)  # b'\x00\x00\x80\x00\x00\xff'

# ── 属性 2: default_mode ──
print(port.default_mode)  # 0（DEFAULT / Mode E）
port.default_mode = 1     # 切换为 NET (HDLC) 模式

# ── 属性 3/4: 波特率 ──
print(port.default_baud, port.proposed_baud)  # 0, 5
# ⚠️ 传枚举索引，不要传整数！
port.proposed_baud = 6   # 19200 bps

# ── 属性 5: response_time ──
print(port.response_time)  # 0（20ms）
port.response_time = 1     # 200ms

# ── 属性 6: device_address ──
print(port.device_address)  # b'MTR001'
port.device_address = b"ABC"  # 最大 6 字节

# ── 属性 7/8/9: 三级密码 ──
print(port.password_1)  # b'00000000'（未设置返回 None）
port.password_2 = b"newpass123"

# ── 钩子 ──
def on_before_read(sender, attr_index, context):
    # 密码属性被读取前可以拦截（安全考虑）
    if attr_index in (7, 8, 9):
        return dlms.DLMSEvent.DENY  # 禁止从 DLMS 读取密码
    return dlms.DLMSEvent.ALLOW

port.on_before_read = on_before_read
```

`构造函数`

```python
LocalPortSetup(logical_name: str, default_mode: int = 0,
               default_baud: int = 300, proposed_baud: int = 9600,
               response_time: int = 1000, device_address: bytes = None,
               password_1: bytes = None, password_2: bytes = None,
               password_5: bytes = None)
```
> 构造函数 9 个参数，仅 `logical_name` 必传。无 `access` 参数。无 `deinit`（全为 `gxByteBuffer` 嵌入字段，无 `gxmalloc` 堆分配）。

```python
import dlms
local_port = dlms.LocalPortSetup(
    "0.0.19.0.0.255",
    default_baud=300,
    proposed_baud=9600,
    device_address=b"MTR001",
    password_1=b"00000000",
)
hdlc = dlms.IecHdlcSetup("0.0.22.0.0.255", commSpeed=9600, deviceAddr=0x10)
conn = dlms.OpticalConnection(uart_port=1, local_port_setup=local_port, hdlc_setup=hdlc)
```
`访问控制`

```python
import dlms
from dlms import AccessMode, Authentication

port = dlms.LocalPortSetup("0.0.128.0.0.255")

port.access_dict = {
    2: (AccessMode.READ,        Authentication.NONE),   # default_mode
    3: (AccessMode.READ,        Authentication.NONE),   # default_baud
    4: (AccessMode.READ,        Authentication.NONE),   # proposed_baud
    5: (AccessMode.READ,        Authentication.NONE),   # response_time
    6: (AccessMode.READ,        Authentication.NONE),   # device_address
    7: (AccessMode.READ_WRITE,  Authentication.HIGH),   # password_1
    8: (AccessMode.READ_WRITE,  Authentication.HIGH),   # password_2
    9: (AccessMode.READ_WRITE,  Authentication.HIGH),   # password_5
}
```

---

#### `GprsSetup`
GPRS/蜂窝网络接入点配置对象（其 **COSEM ID=45**）。定义 APN（接入点名称）和 SIM PIN 码。通常与 `IPv4Setup` 配合使用：`IPv4Setup.datalink_reference = gprs` 将 IP 层绑定到 GPRS 数据链路层。

`Blue Book 属性`

| 编号 | 名称 | 类型 | 说明 |
|------|------|------|:--:|
| 1 | `logical_name` | octet-string(6) | 只读，`bytes` |
| 2 | `apn` | visible-string | 可读写，接入点名称 |
| 3 | `pinCode` | uint16 | 可读写，`pin_code` |
| 4 | `defaultQualityOfService` | structure | 内部属性变量 |
| 5 | `requestedQualityOfService` | structure |内部属性变量 |

```python
import dlms

gprs = dlms.GprsSetup("0.1.25.0.0.255")

# ── 属性 1: logical_name ──
print(gprs.logical_name)  # b'\x00\x01\x19\x00\x00\xff'

# ── 属性 2: apn ──
print(gprs.apn)         # None（默认未设置）
gprs.apn = "internet"
print(gprs.apn)         # "internet"
gprs.apn = "m2m.carrier.net"

# ── 属性 3: pin_code ──
print(gprs.pin_code)    # 0（默认无 PIN）
gprs.pin_code = 1234

# ── 配合 IPv4Setup 使用 ──
ipv4 = dlms.IPv4Setup(
    "0.0.25.1.0.255",
    datalink_reference=gprs,   # IP 层绑定到 GPRS
    ip_address="0.0.0.0",      # DHCP
    use_dhcp=True,
    primary_dns_address="8.8.8.8",
    secondary_dns_address="8.8.4.4",
)

# ── 钩子 ──
def on_before_write(sender, attr_index, value, context):
    if attr_index == 2:  # apn
        print("Changing APN to {}".format(value))
    return dlms.DLMSEvent.ALLOW

gprs.on_before_write = on_before_write
```

`构造函数`

```python
GprsSetup(logical_name: str, apn: str = None, pin_code: int = 0)
```

| 参数 | 类型 | 默认值 | 说明 |
|------|------|--------|------|
| `logical_name` | `str` | 必传 | OBIS 代码，如 `"0.1.25.0.0.255"` |
| `apn` | `str` | `None` | 接入点名称（如 `"internet"`、`"m2m.carrier.net"`）。**注意：默认 `None` 非 `""`** |
| `pin_code` | `int` | `0` | SIM 卡 PIN 码（0 = 无 PIN） |

```python
import dlms
gprs = dlms.GprsSetup("0.0.25.0.0.255", apn="internet", pin_code=0)
```

`Python 属性一览`

| 属性 | 类型 | 可写 | 说明 |
|------|------|:--:|------|
| `logical_name` | `bytes` | × | OBIS 代码（6 字节） |
| `apn` | `str\|None` | √ | 读取未设置时返回 `None`（不是 `""`） |
| `pin_code` | `int` | √ | SIM PIN 码 |
| `on_before_read` | `Callable` | √ | 读前钩子 |
| `on_after_read` | `Callable` | √ | 读后钩子 |
| `on_before_write` | `Callable` | √ | 写前钩子 |
| `on_after_write` | `Callable` | √ | 写后钩子 |
| `on_before_action` | `Callable` | √ | 动作前钩子 |
| `on_after_action` | `Callable` | √ | 动作后钩子 |
| `access_dict` | `dict` | √ | 继承自 CosemObject |


`访问控制`

```python
import dlms
from dlms import AccessMode, Authentication

gprs = dlms.GprsSetup("0.1.25.0.0.255")

dlms.set_default_access(dlms.GprsSetup, {
    dlms.Authentication.NONE: {
        2: dlms.AccessMode.READ,      # apn: 公开读
    },
    dlms.Authentication.HIGH: {
        2: dlms.AccessMode.READ_WRITE,  # apn: HIGH 认证可写
        3: dlms.AccessMode.READ_WRITE,  # pin_code
    },
})
```

---

#### `IPv4Setup`
IPv4 网络层配置对象（其 **COSEM ID=42**）。定义 IP 地址、子网掩码、网关、DNS 和 DHCP 开关。通过 `datalink_reference` 绑定下层数据链路对象（`GprsSetup` 或 `MacAddressSetup`），与 `TcpUdpSetup` 配合构成完整的 DLMS 网络通信栈。

> **内部转换**：IP 地址在 C 层以 `uint32_t`（网络字节序）存储，Python 读写时自动通过 `ip_str_to_uint32`/`uint32_to_ip_str` 做字符串↔整数的双向转换。传入非法 IP → 返回 `0.0.0.0`。

```
┌─────────────────────────────────────┐
│  TcpUdpSetup (COSEM 41)             │  ← 传输层
│  port=4059, ip_reference=ipv4       │
├─────────────────────────────────────┤
│  IPv4Setup (COSEM 42)               │  ← 网络层
│  datalink_reference=gprs|mac        │
├─────────────────────────────────────┤
│  GprsSetup (45) / MacAddressSetup   │  ← 数据链路层
│  apn="internet" / mac="AA:BB:..."   │
└─────────────────────────────────────┘
```

`Blue Book 属性`

| 编号 | 名称 | 类型 | 说明 |
|------|------|------|:--:|
| 1 | `logical_name` | octet-string(6) | 只读，`bytes` |
| 2 | `dataLinkLayer` | object-ref | 可读写，`datalink_reference` |
| 3 | `ipAddress` | uint32 | 可读写，`ip_address`（字符串↔uint32 自动转换） |
| 4 | `multicastIPAddress` | array |  `multicast_ip_address`**仅存 Python 引用，未解析到 C 数组** |
| 5 | `ipOptions` | array | 内部属性 |
| 6 | `subnetMask` | uint32 | 可读写，`subnet_mask` |
| 7 | `gatewayIPAddress` | uint32 | 可读写，`gateway_ip_address` |
| 8 | `useDHCP` | bool | 可读写，`use_dhcp` |
| 9 | `primaryDNSAddress` | uint32 | 可读写，`primary_dns_address` |
| 10 | `secondaryDNSAddress` | uint32 | 可读写，`secondary_dns_address` |

```python
import dlms

# ── 绑定数据链路层 ──
gprs = dlms.GprsSetup("0.1.25.0.0.255", apn="internet")

ipv4 = dlms.IPv4Setup(
    "0.0.25.1.0.255",
    datalink_reference=gprs,
    use_dhcp=True,
)

# ── 属性 1: logical_name ──
print(ipv4.logical_name)  # b'\x00\x00\x19\x01\x00\xff'

# ── 属性 2: datalink_reference ──
print(ipv4.datalink_reference)  # <GprsSetup ...>
# 可事后修改
mac = dlms.MacAddressSetup("0.0.25.4.0.255")
ipv4.datalink_reference = mac    # 切换到以太网

# ── 属性 3: ip_address ──
print(ipv4.ip_address)  # "0.0.0.0"（DHCP 模式，未分配）
ipv4.ip_address = "192.168.1.100"   # 静态 IP
ipv4.use_dhcp = False               # 关闭 DHCP 才用静态 IP

# ── 属性 6/7: 子网掩码 & 网关 ──
ipv4.subnet_mask = "255.255.255.0"
ipv4.gateway_ip_address = "192.168.1.1"
print(ipv4.subnet_mask, ipv4.gateway_ip_address)

# ── 属性 8: use_dhcp ──
print(ipv4.use_dhcp)   # False
ipv4.use_dhcp = False   # 切换为静态 IP

# ── 属性 9/10: DNS ──
ipv4.primary_dns_address = "8.8.8.8"
ipv4.secondary_dns_address = "8.8.4.4"
print(ipv4.primary_dns_address, ipv4.secondary_dns_address)

# ── 属性 4: multicast_ip_address（仅 Python 侧存储）──
ipv4.multicast_ip_address = ["224.0.0.1", "224.0.0.251"]
print(ipv4.multicast_ip_address)  # ['224.0.0.1', '224.0.0.251']

# ── 钩子 ──
def on_before_write(sender, attr_index, value, context):
    if attr_index == 3:  # ip_address
        print("Changing IP to {}".format(value))
    return dlms.DLMSEvent.ALLOW

ipv4.on_before_write = on_before_write
```


`构造函数`

```python
IPv4Setup(logical_name: str, datalink_reference: object = None,
          ip_address: str = "0.0.0.0", subnet_mask: str = "255.255.255.0",
          gateway_ip_address: str = "0.0.0.0", use_dhcp: bool = True,
          primary_dns_address: str = "0.0.0.0",
          secondary_dns_address: str = "0.0.0.0")
```

| 参数 | 类型 | 编号 | 默认值 | 说明 |
|------|------|:--:|--------|------|
| `logical_name` | `str` | 1 | 必传 | OBIS 代码，如 `"0.0.25.1.0.255"` |
| `datalink_reference` | DLMS 对象 | 2 | `None` | 数据链路层对象（`GprsSetup` 或 `MacAddressSetup`） |
| `ip_address` | `str` | 3 | `None` | IPv4 地址字符串（如 `"192.168.1.10"`） |
| `multicast_ip_address` | `list` | 4 | `None` | 组播地址列表（仅存 Python 引用，不转换到 C） |
| `subnet_mask` | `str` | 6 | `None` | 子网掩码（如 `"255.255.255.0"`） |
| `gateway_ip_address` | `str` | 7 | `None` | 网关地址 |
| `use_dhcp` | `bool` | 8 | `True` | 是否启用 DHCP |
| `primary_dns_address` | `str` | 9 | `None` | 首选 DNS 服务器 |
| `secondary_dns_address` | `str` | 10 | `None` | 备用 DNS 服务器 |

```python
import dlms

gprs = dlms.GprsSetup("0.1.25.0.0.255", apn="internet")

ipv4 = dlms.IPv4Setup(
    "0.0.25.1.0.255",
    datalink_reference=gprs,
    ip_address="0.0.0.0",
    subnet_mask="255.255.255.0",
    gateway_ip_address="192.168.1.1",
    use_dhcp=True,
    primary_dns_address="8.8.8.8",
    secondary_dns_address="8.8.4.4",
)
```
`Python 属性一览`

| 属性 | 类型 | 可写 | 说明 |
|------|------|:--:|------|
| `logical_name` | `bytes` | × | OBIS 代码（6 字节） |
| `datalink_reference` | DLMS对象\|`None` | √ | 指向 `GprsSetup` / `MacAddressSetup`。写入时自动 `gx_from_mp` 取 C 指针 |
| `ip_address` | `str` | √ | 字符串 IP，读写自动 `uint32`↔`string` 转换 |
| `multicast_ip_address` | `list` | √ |  仅存 Python 引用，**不解析到 C 语言 `multicastIPAddress` 数组** |
| `subnet_mask` | `str` | √ | 子网掩码字符串 |
| `gateway_ip_address` | `str` | √ | 网关地址字符串 |
| `use_dhcp` | `bool` | √ | DHCP 开关（`True`/`False`） |
| `primary_dns_address` | `str` | √ | 首选 DNS 地址 |
| `secondary_dns_address` | `str` | √ | 备用 DNS 地址 |
| `on_before_read` | `Callable` | √ | 读前钩子 |
| `on_after_read` | `Callable` | √ | 读后钩子 |
| `on_before_write` | `Callable` | √ | 写前钩子 |
| `on_after_write` | `Callable` | √ | 写后钩子 |
| `on_before_action` | `Callable` | √ | 动作前钩子 |
| `on_after_action` | `Callable` | √ | 动作后钩子 |
| `access_dict` | `dict` | √ | 继承自 CosemObject |

> **`multicast_ip_address` 的特殊处理**：构造函数和多线程 setter 都只把值存入 `self->multicast_ip_list`（Python 引用），**不解析为 C 语言 `gxArray multicastIPAddress` 或 `variantArray multicastIPAddress`**。C 结构体中的该字段始终为空（`arr_init`/`va_init` 后的初始状态）。
---

`访问控制`

```python
import dlms
from dlms import AccessMode, Authentication

ipv4 = dlms.IPv4Setup("0.0.25.1.0.255")

dlms.set_default_access(dlms.IPv4Setup, {
    dlms.Authentication.NONE: {
        2: dlms.AccessMode.READ,     # datalink_reference
        3: dlms.AccessMode.READ,     # ip_address
        8: dlms.AccessMode.READ,     # use_dhcp
    },
    dlms.Authentication.HIGH: {
        3: dlms.AccessMode.READ_WRITE,   # ip_address: HIGH 认证可写
        7: dlms.AccessMode.READ_WRITE,   # gateway_ip_address
        9: dlms.AccessMode.READ_WRITE,   # primary_dns_address
    },
})
```

#### `TcpUdpSetup`
TCP/UDP 传输层配置对象（其 **COSEM ID=41**）。定义 DLMS 通信的端口号、IP 层引用、最大分段大小（MSS/MTU）、最大并发连接数和无通信超时。在网络栈中位于最顶层（传输层），通过 `ip_reference` 引用下层的 `IPv4Setup`。

> **网络栈三层结构**：`GprsSetup/MacAddressSetup`（数据链路）→ `IPv4Setup`（网络）→ `TcpUdpSetup`（传输），通过各自的对象引用串联成完整通信路径。

`Blue Book 属性`

| 编号 | 名称 | 类型 | 说明 |
|------|------|------|:--:|
| 1 | `logical_name` | octet-string(6) | 只读，`bytes` |
| 2 | `port` | uint16 | 可读写，默认 4059 |
| 3 | `ipReference` | object-ref |  可读写，`ip_reference`（指向 `IPv4Setup`） |
| 4 | `maximumSegmentSize` | uint16 | 可读写，`max_segment_size`，默认 1460 |
| 5 | `maximumSimultaneousConnections` | uint8 | 可读写，`max_simultaneous_connections`，默认 1 |
| 6 | `inactivityTimeout` | uint16 | ✅读写，`inactivity_timeout`（秒），默认 120 |

```python
import dlms

# ── 构建完整网络栈 ──
gprs = dlms.GprsSetup("0.1.25.0.0.255", apn="internet")
ipv4 = dlms.IPv4Setup("0.0.25.1.0.255", datalink_reference=gprs, use_dhcp=True)
tcp  = dlms.TcpUdpSetup("0.0.25.2.0.255", ip_reference=ipv4)

# ── 属性 1: logical_name ──
print(tcp.logical_name)  # b'\x00\x00\x19\x02\x00\xff'

# ── 属性 2: port ──
print(tcp.port)    # 4059（默认 DLMS 端口）
tcp.port = 80    # 可改为其他端口

# ── 属性 3: ip_reference ──
print(tcp.ip_reference)  # <IPv4Setup ...>
tcp.ip_reference = None  # 可清除引用

# ── 属性 4: max_segment_size ──
print(tcp.max_segment_size)  # 1460（默认）
tcp.max_segment_size = 536   # 最小 MTU（IPv4 要求）

# ── 属性 5: max_simultaneous_connections ──
print(tcp.max_simultaneous_connections)  # 1
tcp.max_simultaneous_connections = 3     # 最多 3 个并发客户端

# ── 属性 6: inactivity_timeout ──
print(tcp.inactivity_timeout)  # 120 秒
tcp.inactivity_timeout = 0     # 禁用超时

# ── 钩子 ──
def on_before_write(sender, attr_index, value, context):
    if attr_index == 2:  # port
        print("Changing port to {}".format(value))
    return dlms.DLMSEvent.ALLOW

tcp.on_before_write = on_before_write
```

`构造函数`

```python
TcpUdpSetup(logical_name: str, port: int = 4059,
            ip_reference: object = None, max_simultaneous_connections: int = 1,
            inactivity_timeout: int = 0)
```

> 构造函数 6 个参数，仅 `logical_name` 必传。无 `access` 参数（与 `.pyi` 声明不同）。无 `deinit`。

| 参数 | 类型 | 编号 | 默认值 | 说明 |
|------|------|:--:|--------|------|
| `logical_name` | `str` | 1 | 必传 | OBIS 代码，如 `"0.0.25.2.0.255"` |
| `port` | `int` | 2 | `4059` | TCP/UDP 端口号（DLMS 标准端口） |
| `ip_reference` | DLMS 对象 | 3 | `None` | 下层 `IPv4Setup` 对象引用。写入时自动 `gx_from_mp` 取 C 指针 |
| `max_segment_size` | `int` | 4 | `1460` | 最大分段大小 / MTU（字节） |
| `max_simultaneous_connections` | `int` | 5 | `1` | 最大并发连接数 |
| `inactivity_timeout` | `int` | 6 | `120` | 无通信超时（秒，0 = 不超时） |

```python
import dlms

gprs = dlms.GprsSetup("0.1.25.0.0.255", apn="internet")
ipv4 = dlms.IPv4Setup("0.0.25.1.0.255", datalink_reference=gprs, use_dhcp=True)

tcp = dlms.TcpUdpSetup(
    "0.0.25.2.0.255",
    port=4059,
    ip_reference=ipv4,
    max_segment_size=1460,
    max_simultaneous_connections=1,
    inactivity_timeout=120,
)
```

`Python 属性一览`

| 属性 | 类型 | 可写 | 说明 |
|------|------|:--:|------|
| `logical_name` | `bytes` | × | OBIS 代码（6 字节） |
| `port` | `int` | √ | 端口号，默认 4059（DLMS 标准端口） |
| `ip_reference` | `IPv4Setup\|None` | √ | 下层 IP 配置引用，同时存 Python 引用（防 GC）和 C 指针 `c_obj.ipSetup` |
| `max_segment_size` | `int` | √ | MSS/MTU 大小（C 层 `uint16_t`） |
| `max_simultaneous_connections` | `int` | √ | 最大并发连接（C 层 `unsigned char`，超出 255 截断） |
| `inactivity_timeout` | `int` | √ | 无通信超时秒数（C 层 `uint16_t`） |
| `on_before_read` | `Callable` | √ | 读前钩子 |
| `on_after_read` | `Callable` | √ | 读后钩子 |
| `on_before_write` | `Callable` | √ | 写前钩子 |
| `on_after_write` | `Callable` | √ | 写后钩子 |
| `on_before_action` | `Callable` | √ | 动作前钩子 |
| `on_after_action` | `Callable` | √ | 动作后钩子 |
| `access_dict` | `dict` | √ | 继承自 CosemObject |

---

`访问控制`

```python
import dlms
from dlms import AccessMode, Authentication

tcp = dlms.TcpUdpSetup("0.0.25.2.0.255")

dlms.set_default_access(dlms.TcpUdpSetup, {
    dlms.Authentication.NONE: {
        2: dlms.AccessMode.READ,     # port: 公开读
        4: dlms.AccessMode.READ,     # max_segment_size
        5: dlms.AccessMode.READ,     # max_simultaneous_connections
        6: dlms.AccessMode.READ,     # inactivity_timeout
    },
    dlms.Authentication.HIGH: {
        2: dlms.AccessMode.READ_WRITE,   # port: HIGH 认证可写
        4: dlms.AccessMode.READ_WRITE,   # max_segment_size
        6: dlms.AccessMode.READ_WRITE,   # inactivity_timeout
    },
})
```
#### `MacAddressSetup`
以太网/蜂窝 MAC 地址配置对象（其 **COSEM ID=43**）。定义数据链路层的 MAC 地址（6 字节）。与 `GprsSetup` 同级属于数据链路层，可通过 `IPv4Setup.datalink_reference` 引用，构成以太网通信路径。

`Blue Book 属性`

| 编号 | 名称 | 类型 | 说明 |
|------|------|------|------|
|  1 |`logical_name` | `str` |  OBIS 代码，如 `"0.0.25.4.0.255"` |
|  2 | `mac_address` | `bytes` |`None`  MAC 地址，必须恰好 6 字节（如 `b"\x00\x11\x22\x33\x44\x55"`） |

`构造函数`

```python
MacAddressSetup(logical_name: str, mac_address: bytes = None)
```
```python
import dlms

mac = dlms.MacAddressSetup("0.0.25.4.0.255")

# ── 属性 1: logical_name ──
print(mac.logical_name)  # b'\x00\x00\x19\x04\x00\xff'

# ── 属性 2: mac_address ──
print(mac.mac_address)   # None（默认未设置）
mac.mac_address = b"\x00\x11\x22\x33\x44\x55"
print(mac.mac_address)   # b'\x00\x11"3DU'

# 十六进制格式化显示
print(":".join(["%02X" % b for b in mac.mac_address]))  # "00:11:22:33:44:55"

# 打印对象（自动显示 MAC）
print(mac)  # <MacAddressSetup ln='0.0.25.4.0.255', mac_address=00:11:22:33:44:55>

# ── 长度校验 ──
# mac.mac_address = b"\x00\x11"          # ValueError: must be 6 bytes
# mac.mac_address = b"\x00" * 7          # ValueError: must be 6 bytes
# mac.mac_address = "00:11:22:33:44:55"  # TypeError: must be bytes

# ── 配合 IPv4Setup 使用（以太网通信路径）──
ipv4 = dlms.IPv4Setup(
    "0.0.25.1.0.255",
    datalink_reference=mac,      # 绑定以太网 MAC
    ip_address="192.168.1.100",
    subnet_mask="255.255.255.0",
    use_dhcp=False,
)

# ── 钩子 ──
def on_before_write(sender, attr_index, value, context):
    if attr_index == 2:  # mac_address
        print(":".join(["{:02X}".format(b) for b in mac.mac_address]))  # "00:11:22:33:44:55"
    return dlms.DLMSEvent.ALLOW

mac.on_before_write = on_before_write
```
`访问控制`

```python
import dlms
from dlms import AccessMode, Authentication

mac = dlms.MacAddressSetup("0.0.25.4.0.255")

dlms.set_default_access(dlms.MacAddressSetup, {
    dlms.Authentication.NONE: {
        2: dlms.AccessMode.READ,     # mac_address: 公开读
    },
    dlms.Authentication.HIGH: {
        2: dlms.AccessMode.READ_WRITE,   # mac_address: HIGH 认证可写
    },
})
```

`典型用例`

```python
mac = dlms.MacAddressSetup("0.0.25.0.0.255", mac_address=b'\x00\x11\x22\x33\x44\x55')
```
---
###  M-Bus 对象

M-Bus 从机端口配置对象（其 **COSEM ID=25**）。当 QuecPython 设备作为 M-Bus **从机/表计**时，向 DLMS 客户端（主站/集中器）暴露自身 M-Bus 端口的物理参数：默认波特率、当前可用波特率、地址分配状态和总线地址。

---

#### `MbusSlavePortSetup`
当 QuecPython 设备**作为 M-Bus 从站/仪表**时使用。暴露设备的 M-Bus 端口参数给 DLMS 客户端（主站/集中器）读取，其**COSEM ID = 25**。

`Blue Book 属性`

| 编号 | 名称 | 类型 | 说明 |
|------|------|------|------|
| 1 | `logical_name` | octet-string(6) | OBIS 代码 |
| 2 | `default_baud` | enum | 默认 M-Bus 波特率 |
| 3 | `available_baud` | enum | 当前可用波特率 |
| 4 | `address_state` | enum | 地址分配状态 |
| 5 | `bus_address` | uint8 | M-Bus 主站地址（0–255，有效从站范围 1–250） |

`构造函数`

```python
MbusSlavePortSetup(logical_name: str, access: dict = None)
```

`AddressState 枚举`

| 常量 | 值 | 说明 |
|------|:--:|------|
| `AddressState.NONE` | `0` | 自上次上电后未分配地址 |
| `AddressState.ASSIGNED` | `1` | 地址已分配（手动或自动） |

> 类名全小写用 `dlms.AddressState.NONE`，也可直接用整数 `0`/`1`。

`波特率校验`

`MbusSlavePortSetup` 使用 `baud_int_to_enum` + 往返校验（与 `IecHdlcSetup` 相同）：
```python
import dlms
from dlms import AddressState, AccessMode, Authentication

slave = dlms.MbusSlavePortSetup("0.0.24.9.0.255",
    access={
        5: (AccessMode.READ_WRITE, Authentication.HIGH),  # bus_address 需 HIGH
    })

# ── 属性 1: logical_name ──
print(slave.logical_name)  # b'\x00\x00\x18\t\x00\xff'

# ── 属性 2: default_baud ──
print(slave.default_baud)   # 9600（构造默认）
slave.default_baud = 2400
print(slave.default_baud)   # 2400

# ── 属性 3: available_baud ──
print(slave.available_baud) # 9600
slave.available_baud = 2400
print(slave.available_baud) # 2400

# ── 属性 4: address_state ──
print(slave.address_state)  # 0（NONE，默认）
slave.address_state = AddressState.ASSIGNED  # 或直接用 1

# ── 属性 5: bus_address ──
print(slave.bus_address)    # 0（默认未设置）
slave.bus_address = 1       # 有效从机地址 1–250

# ── 完整典型配置 ──
slave.default_baud   = 9600
slave.available_baud = 9600
slave.address_state  = AddressState.ASSIGNED
slave.bus_address    = 1

# ── 钩子：客户端读前刷新状态 ──
def on_before_read(sender, attr_index, context):
    if attr_index == 4:  # address_state
        sender.address_state = AddressState.ASSIGNED
    return dlms.DLMSEvent.ALLOW

slave.on_before_read = on_before_read
```
`Python 属性一览`

| 属性 | 类型 | 可写 | 说明 |
|------|------|:--:|------|
| `logical_name` | `bytes` | × | OBIS 代码（6 字节） |
| `default_baud` | `int` | √ | 默认波特率（默认 9600）。传入非法值抛 `ValueError` |
| `available_baud` | `int` | √ | 当前可用波特率（默认 9600） |
| `address_state` | `int` | √ | `AddressState.NONE`(0) / `ASSIGNED`(1) |
| `bus_address` | `int` | √ | M-Bus 从机地址（0–255，1–250 为有效从机地址，0/251–255 保留） |
| `on_before_read` | `Callable` | √ | 读前钩子（继承自 CosemObject） |
| `on_after_read` | `Callable` | √ | 读后钩子 |
| `on_before_write` | `Callable` | √ | 写前钩子 |
| `on_after_write` | `Callable` | √ | 写后钩子 |
| `access_dict` | `dict` | √ | 实例级访问控制（构造时支持传入） |

> **M-Bus class 25 无 COSEM 动作方法**：`on_before_action` / `on_after_action` 永远不会被触发。


`访问控制`

```python
import dlms
from dlms import AccessMode, Authentication

slave = dlms.MbusSlavePortSetup("0.0.24.9.0.255")

dlms.set_default_access(dlms.MbusSlavePortSetup, {
    dlms.Authentication.NONE: {
        2: dlms.AccessMode.READ,     # default_baud: 公开读
        3: dlms.AccessMode.READ,     # available_baud
        4: dlms.AccessMode.READ,     # address_state
        5: dlms.AccessMode.READ,     # bus_address
    },
    dlms.Authentication.HIGH: {
        2: dlms.AccessMode.READ_WRITE,   # default_baud: HIGH 可写
        4: dlms.AccessMode.READ_WRITE,   # address_state
        5: dlms.AccessMode.READ_WRITE,   # bus_address
    },
})
```
---

#### `MbusMasterPortSetup`
M-Bus 主站端口配置对象（其 **COSEM ID=74**）。当 QuecPython 设备作为 M-Bus **主站/集中器**时，向 DLMS 客户端暴露主站端口的通信速率（波特率）。

`Blue Book 属性（仅一个）`

| 属性 | 类型 | 说明 |
|------|------|------|
| `logical_name` | `octet-string(6)` | OBIS 代码 |
| `comm_speed` | `int` | 通信速率（300/600/…/115200） |

`波特率校验`

使用 `baud_int_to_enum` + 往返校验（与 `MbusSlavePortSetup` 相同）：

| `comm_speed` | 枚举索引 |
|:--:|:--:|
| 300 | 0 |
| 600 | 1 |
| 1200 | 2 |
| 2400 | 3 |
| 4800 | 4 |
| 9600 | 5 |
| 19200 | 6 |
| 38400 | 7 |
| 57600 | 8 |
| 115200 | 9 |

`构造函数`

```python
MbusMasterPortSetup(logical_name: str, access: dict = None)
```
```python
import dlms
from dlms import AccessMode, Authentication

master = dlms.MbusMasterPortSetup("0.0.24.3.0.255")

# ── 属性 1: logical_name ──
print(master.logical_name)  # b'\x00\x00\x18\x03\x00\xff'

# ── 属性 2: comm_speed ──
print(master.comm_speed)    # 9600（默认）
master.comm_speed = 2400
print(master.comm_speed)    # 2400

# 非法值会被拒绝
# master.comm_speed = 12345  # ValueError

# ── 实例级访问控制 ──
master = dlms.MbusMasterPortSetup("0.0.24.3.0.255",
    access={2: (AccessMode.READ_WRITE, Authentication.HIGH)})

# ── 钩子 ──
def on_before_write(sender, attr_index, value, context):
    if attr_index == 2:
        print("Changing M-Bus master baud rate to {}".format(value))
    return dlms.DLMSEvent.ALLOW

master.on_before_write = on_before_write
```

`Python 属性一览`

| 属性 | 类型 | 可写 | 说明 |
|------|------|:--:|------|
| `logical_name` | `bytes` | × | OBIS 代码（6 字节） |
| `comm_speed` | `int` | √ | 波特率（默认 9600），传入非法值抛 `ValueError` |
| `on_before_read` | `Callable` | √ | 读前钩子（继承自 CosemObject） |
| `on_after_read` | `Callable` | √ | 读后钩子 |
| `on_before_write` | `Callable` | √ | 写前钩子 |
| `on_after_write` | `Callable` | √ | 写后钩子 |
| `access_dict` | `dict` | √ | 实例级访问控制（构造时支持传入） |


`访问控制`

```python
import dlms
from dlms import AccessMode, Authentication

master = dlms.MbusMasterPortSetup("0.0.24.3.0.255")

dlms.set_default_access(dlms.MbusMasterPortSetup, {
    dlms.Authentication.NONE: {
        2: dlms.AccessMode.READ,      # comm_speed: 公开读
    },
    dlms.Authentication.HIGH: {
        2: dlms.AccessMode.READ_WRITE,  # comm_speed: HIGH 可写
    },
})
```

---

#### `MbusPortSetup`
M-Bus 端口配置对象（其 **COSEM ID=76**）。描述 M-Bus 主站所连接的**单个从机端口**的完整通信参数：从机标识（地址/ID/厂商/版本/设备类型）、通信配置（数据头类型/PDU 大小）和监听窗口。DLMS 客户端通过它了解 M-Bus 从机的物理能力和当前状态。

> **与 `MbusSlavePortSetup`/`MbusMasterPortSetup` 的区别**：
> - `MbusSlavePortSetup`(25)：**从机自己**暴露自身端口
> - `MbusMasterPortSetup`(74)：**主站**暴露自己的通信速率
> - `MbusPortSetup`(76)：**主站视角**描述某个从机端口的完整档案（相当于主站为每个从机建一份配置记录），通常配合 `MbusClient`(72) 使用


`Blue Book 属性`

| 编号 | 名称 | 说明 |
|------|------|------|
| 1 | `logical_name` | OBIS 代码 |
| 2 | `profile_selection` | 关联的 ProfileGeneric OBIS（⏩ COMPLEX） |
| 3 | `port_communication_status` | 端口状态 |
| 4 | `data_header_type` | 数据头类型 |
| 5 | `primary_address` | 主站地址（0–255） |
| 6 | `identification_number` | 标识号（只读） |
| 7 | `manufacturer_id` | 厂家 ID（2 字节，只读） |
| 8 | `mbus_version` | 协议版本（只读） |
| 9 | `device_type` | 设备类型（只读） |
| 10 | `max_pdu_size` | 最大 PDU 大小 |
| 11 | `listening_window` | 监听窗口列表 `[[start_tuple, end_tuple], ...]`（⏩ COMPLEX） |


`构造函数`

```python
MbusPortSetup(logical_name: str, access: dict = None)
```
`枚举常量`

`port_communication_status`（`DLMS_MBUS_PORT_COMMUNICATION_STATE`）：

| 值 | 说明 |
|:--:|------|
| `0` | NO_ACCESS — 无访问权限 |
| `1` | TEMPORARY_NO_ACCESS — 临时无访问 |
| `2` | LIMITED_ACCESS — 受限访问 |
| `3` | UNLIMITED_ACCESS — 完全访问 |
| `4` | WMBUS — 无线 M-Bus |

`data_header_type`（`DLMS_MBUS_DATA_HEADER_TYPE`）：

| 值 | 说明 |
|:--:|------|
| `0` | NONE — 不使用数据头 |
| `1` | SHORT — 短数据头 |
| `2` | LONG — 长数据头 |

`device_type`（`DLMS_MBUS_METER_TYPE`）：

| 值 | 表计类型 | 值 | 表计类型 |
|:--:|------|:--:|------|
| `0` | OTHER | `8` | HEAT_COST_ALLOCATOR |
| `1` | OIL | `10` | GAS_MODE2 |
| `2` | ENERGY | `11` | HEAT_MODE2 |
| `3` | GAS | `12` | HOT_WATER_MODE2 |
| `4` | HEAT | `13` | WATER_MODE2 |
| `5` | STEAM | `14` | HEAT_COST_ALLOCATOR_MODE2 |
| `6` | HOT_WATER | `0x0F` | UNKNOWN |
| `7` | WATER | | |

```python
import dlms
A = dlms.ANY

port = dlms.MbusPortSetup("0.0.24.7.0.255")

# ── 属性 1: logical_name ──
print(port.logical_name)  # b'\x00\x00\x18\x07\x00\xff'

# ── 属性 2: profile_selection ──
port.profile_selection = "0.0.24.1.0.255"   # 关联的 profile OBIS
print(port.profile_selection)               # "0.0.24.1.0.255"

# ── 属性 3/4: 状态与头类型 ──
print(port.port_communication_status)  # 0 (NO_ACCESS)
port.port_communication_status = 3     # UNLIMITED_ACCESS
port.data_header_type = 2              # LONG 数据头

# ── 属性 5–9: 从机标识 ──
port.primary_address = 1
port.identification_number = 12345678
port.manufacturer_id = 0x1234
port.mbus_version = 1
port.device_type = 3                   # GAS 气表

# ── 属性 10: max_pdu_size ──
port.max_pdu_size = 240

# ── 属性 11: listening_window ──
port.listening_window = [
    [(A, A, A,  8, 0, 0), (A, A, A, 18, 0, 0)],   # 每天 8:00–18:00
    [(A, A, A, 20, 0, 0), (A, A, A, 22, 0, 0)],   # 每天 20:00–22:00
]
for start, end in port.listening_window:
    print(start, end)

# ── 配合 MbusClient 使用 ──
client = dlms.MbusClient("0.0.24.1.0.255", mbus_port=port)
client.capture_period = 900   # 15 分钟采集一次
```

`Python 属性一览`

| 属性 | 类型 | 可写 | 说明 |
|------|------|:--:|------|
| `logical_name` | `bytes` | × | OBIS 代码（6 字节） |
| `profile_selection` | `str` | √ | 关联 profile 的 OBIS 代码（6 字节），读写用 `"A.B.C.D.E.F"` 字符串格式 |
| `port_communication_status` | `int` | √ | 端口通信状态（见下方枚举） |
| `data_header_type` | `int` | √ | 数据头类型：0=None, 1=Short, 2=Long |
| `primary_address` | `int` | √ | 从机主地址（0–255） |
| `identification_number` | `int` | √ | 从机标识号（uint32） |
| `manufacturer_id` | `int` | √ | 2 字节厂商码（uint16） |
| `mbus_version` | `int` | √ | M-Bus 协议版本 |
| `device_type` | `int` | √ | 表计类型（见下方枚举） |
| `max_pdu_size` | `int` | √ | 最大 PDU 大小（字节） |
| `listening_window` | `list[list]` | √ | `[[start_6tuple, end_6tuple], ...]` 监听窗口列表 |
| `on_before_read` | `Callable` | √ | 读前钩子 |
| `on_after_read` | `Callable` | √ | 读后钩子 |
| `on_before_write` | `Callable` | √ | 写前钩子 |
| `on_after_write` | `Callable` | √ | 写后钩子 |
| `access_dict` | `dict` | √ | 实例级访问控制 |


`访问控制`

```python
import dlms
from dlms import AccessMode, Authentication

port = dlms.MbusPortSetup("0.0.24.7.0.255")

dlms.set_default_access(dlms.MbusPortSetup, {
    dlms.Authentication.NONE: {
        5: dlms.AccessMode.READ,      # primary_address: 公开读
        6: dlms.AccessMode.READ,      # identification_number
        9: dlms.AccessMode.READ,      # device_type
    },
    dlms.Authentication.HIGH: {
        2: dlms.AccessMode.READ_WRITE,   # profile_selection
        5: dlms.AccessMode.READ_WRITE,   # primary_address: HIGH 可写
        11: dlms.AccessMode.READ_WRITE,  # listening_window
    },
})
```

---

#### `MbusClient`
M-Bus 客户端对象（其 **COSEM ID=72**）。描述 M-Bus **主站所连接的单个从机表计**——定义从机的标识（地址/ID/厂商/设备类型）、采集配置（周期 + 采集记录定义）和当前状态（访问号/状态/告警/配置字/密钥状态）。每个从机表对应一个 `MbusClient` 实例。

```
MbusPortSetup (76)              MbusDiagnostic (77)
    ▲  mbus_port                    ▲ 每通道一个
    │  ┌─────────────────────────────────────────┐
    ├──┤  MbusClient (72) — 每个从机表一个        │
    │  │  capture_definition / capture_period    │
    │  │  identification / status / alarm        │
    │  └─────────────────────────────────────────┘
    │        8 个方法 ← on_before_action 钩子实现
    ▼
MbusMasterPortSetup (74) — 主站自身波特率
```

> **⚠️ 与 `dlms.Client` 完全是两回事**：`dlms.MbusClient` 是**数据模型对象**（CosemObject 子类，放进服务器对象模型供远程读取）；`dlms.Client` 是**通信引擎**（主动连远端 DLMS 设备执行 read/write）。

`Blue Book 属性`

| 编号 | 名称 | 类型 | 说明 |
|------|------|------|:--:|
| 1 | `logical_name` | octet-string(6) | 只读，`bytes` |
| 2 | `mBusPort` | object-ref | 可读写，`mbus_port`（指向 `MbusPortSetup`） |
| 3 | `captureDefinition` | array | 可读写，`capture_definition`（`(bytes, bytes)` 键值对列表） |
| 4 | `capturePeriod` | uint32 | 可读写，`capture_period`（秒） |
| 5 | `primaryAddress` | uint8 | 可读写，`primary_address` |
| 6 | `identificationNumber` | uint32 | 读写（attr 标记只读，但 Python setter 存在），`identification_number` |
| 7 | `manufacturerID` | uint16 | 读写，`manufacturer_id` |
| 8 | `dataHeaderVersion` | uint8 | 读写，`version` |
| 9 | `deviceType` | uint8 | 读写，`device_type` |
| 10 | `accessNumber` | uint8 | 读写，`access_number`（VOLATILE） |
| 11 | `status` | uint8 | 读写，`status`（VOLATILE） |
| 12 | `alarm` | uint8 | 读写，`alarm`（VOLATILE） |
| 13 | `configuration` | uint16 | 可读写，`configuration` |
| 14 | `encryptionKeyStatus` | enum | 可读写，`encryption_key_status` |

> 属性 6–12、14 在 attr_table 标记 `READONLY`/`VOLATILE`（供 DLMS 客户端语义使用），但 **Python setter 全部存在**——因为数据是 Python 钩子从 M-Bus 硬件采回来后写入的。


```python
import dlms
from dlms import AccessMode, Authentication

# ── 关联端口 ──
port = dlms.MbusPortSetup("0.0.24.7.0.255")
port.primary_address = 1

client = dlms.MbusClient("0.0.24.1.0.255",
    mbus_port=port,
    access={
        dlms.Authentication.NONE: {2: dlms.AccessMode.READ_WRITE,
                                    3: dlms.AccessMode.READ_WRITE,
                                    4: dlms.AccessMode.READ_WRITE},
    })

# ── 属性 2: mbus_port ──
print(client.mbus_port)         # <MbusPortSetup ...>
client.mbus_port = None         # 可解除
client.mbus_port = port         # 可恢复

# ── 属性 3: capture_definition ──
client.capture_definition = [
    (b'\x02\x04', b'\x09\x06'),   # (数据记录key, 值key)
    (b'\x01\x02\x03', b'\x01'),
]
for key, val in client.capture_definition:
    print(key, val)   # b'\x02\x04' b'\x09\x06' ...

# ── 属性 4: capture_period ──
client.capture_period = 900   # 15 分钟

# ── 属性 5: primary_address ──
client.primary_address = 1

# ── 属性 6–9: 从机标识 ──
client.identification_number = 12345678
client.manufacturer_id = 0x4D41   # 'MA'
client.version = 1
client.device_type = 3            # GAS

# ── 属性 10–12: 运行状态 ──
client.access_number = 5          # 采集后递增
client.status = 0x05
client.alarm = 0x01

# ── 属性 13/14 ──
client.configuration = 0x0100
client.encryption_key_status = 2  # KEY_INUSE

# ── 采集动作（核心）──
def on_capture(self, event):
    if event.index == 3:  # capture 方法
        # 真实 M-Bus 硬件读取从这里开始
        # data = mbus_read(client.primary_address, client.capture_definition)
        # client.status = ...
        # client.access_number += 1
        return True
    return True

client.on_before_action = on_capture

```
`构造函数`

```python
MbusClient(logical_name: str, mbus_port: object = None, access: dict = None)
```
```python
import dlms
from dlms import AccessMode, Authentication

# ── 先创建关联端口 ──
port = dlms.MbusPortSetup("0.0.24.7.0.255")
port.primary_address = 1

# ── 实例化 MbusClient ──
client = dlms.MbusClient(
    "0.0.24.1.0.255",        # logical_name: OBIS 代码（必传）
    mbus_port=port,          # 关联 MbusPortSetup（可省略，之后用 client.mbus_port = port 补）
    access={                 # 实例级权限（可省略）
        2: (AccessMode.READ,        Authentication.NONE),   # mbus_port
        3: (AccessMode.READ_WRITE,  Authentication.HIGH),   # capture_definition
        4: (AccessMode.READ_WRITE,  Authentication.HIGH),   # capture_period
    },
)
```
| 参数 | 类型 | 编号 | 默认值 | 说明 |
|------|------|:--:|--------|------|
| `logical_name` | `str` | 1 | 必传 | OBIS 代码，如 `"0.0.24.1.0.255"` |
| `mbus_port` | DLMS 对象 | 2 | `None` | 关联的 `MbusPortSetup`（或任意 DLMS 对象），写入时自动 `gx_from_mp` 取 C 指针 |
| `access` | `dict` | — | `None` | 实例级权限（无校验） |

`Python 属性一览`

| 属性 | 类型 | 可写 | 说明 |
|------|------|:--:|------|
| `logical_name` | `bytes` | × | OBIS 代码（6 字节） |
| `mbus_port` | `MbusPortSetup\|None` | √ | 端口引用，双存（Python 引用 + C 指针 `c_obj.mBusPort`）。读回时优先 Python 引用，否则 `find_python_object_by_c_ptr` 反向查找 |
| `capture_definition` | `list[(bytes,bytes)]` | √ | 采集记录定义：`(data_key_bytes, value_key_bytes)` 对列表 |
| `capture_period` | `int` | √ | 采集周期（秒） |
| `primary_address` | `int` | √ | 从机主地址 |
| `identification_number` | `int` | √ | 从机标识号 |
| `manufacturer_id` | `int` | √ | 2 字节厂商码 |
| `version` | `int` | √ | 数据头版本 |
| `device_type` | `int` | √ | 表计类型 |
| `access_number` | `int` | √ | 访问号计数器（每次通信递增） |
| `status` | `int` | √ | 从机状态字节 |
| `alarm` | `int` | √ | 从机告警字节 |
| `configuration` | `int` | √ | 2 字节配置字 |
| `encryption_key_status` | `int` | √ | 加密密钥状态（`DLMS_MBUS_ENCRYPTION_KEY_STATUS`） |
| `on_before_read` | `Callable` | √ | 读前钩子 |
| `on_after_read` | `Callable` | √ | 读后钩子 |
| `on_before_action` | `Callable` | √ | **动作前钩子——8 个方法的实现入口** |
| `on_after_action` | `Callable` | √ | 动作后钩子 |
| `access_dict` | `dict` | √ | 实例级访问控制 |

`方法（全部通过 `on_before_action` 派发）`

| 序号 | 方法名 | 说明 |
|------|--------|------|
| 1 | slave_install | 安装从站 |
| 2 | slave_deinstall | 卸载从站 |
| 3 | capture | 捕获数据（触发测量值更新） |
| 4 | reset_alarm | 清零告警 |
| 5 | synchronize_clock | 同步时钟 |
| 6 | data_send | 发送数据 |
| 7 | set_encryption_key | 设置加密密钥 |
| 8 | transfer_key | 传输密钥 |

> 所有方法需要 `on_before_action` handler 实现实际的 M-Bus 物理通信。
```python
def on_action(self, event):
    method_names = {1: "slave_install", 2: "slave_deinstall", 3: "capture",
                    4: "reset_alarm", 5: "synchronize_clock", 6: "data_send",
                    7: "set_encryption_key", 8: "transfer_key"}
    if event.index == 3:  # capture
        # TODO: 调用 M-Bus 硬件 API 读取从机数据，写入本对象属性
        return True   # 成功
    return False  # 其他方法未实现 → 拒绝

client.on_before_action = on_action
```
`访问控制`

```python
import dlms
from dlms import AccessMode, Authentication

client = dlms.MbusClient("0.0.24.1.0.255")

dlms.set_default_access(dlms.MbusClient, {
    dlms.Authentication.NONE: {
        2: dlms.AccessMode.READ,      # mbus_port
        4: dlms.AccessMode.READ,      # capture_period
    },
    dlms.Authentication.HIGH: {
        3: dlms.AccessMode.READ_WRITE,   # capture_definition
        4: dlms.AccessMode.READ_WRITE,   # capture_period
        5: dlms.AccessMode.READ_WRITE,   # primary_address
    },
    # 方法（1–8）在方法索引下配置：
    # dlms.Authentication.HIGH: {1: dlms.AccessMode.AUTHENTICATED_WRITE, ...}
})
```

---

#### `MbusDiagnostic`
M-Bus 通道诊断对象（其 **COSEM ID=77**）。监控**单个 M-Bus 通信通道**的链路质量：信号强度、通道 ID、链路状态、广播帧计数、收发帧统计和最后采集时间。通常与 `MbusClient` 搭配——每个通道一个 `MbusDiagnostic`。

> **类比 GsmDiagnostic**：`GsmDiagnostic` 是蜂窝网络的"信号仪表盘"，`MbusDiagnostic` 是 M-Bus 总线/无线通道的"信号仪表盘"——都是被动数据载体，数值靠钩子从硬件刷新。

`Blue Book 属性`

| 编号 | 名称 | 类型 | 说明 |
|------|------|------|:--:|
| 1 | `logical_name` | octet-string(6) | 只读，`bytes` |
| 2 | `receivedSignalStrength` | uint8 | 可读写，`received_signal_strength`（dBm/dBμV） |
| 3 | `channelId` | uint8 | 可读写，`channel_id` |
| 4 | `linkStatus` | enum | 可读写，`link_status`（`DLMS_MBUS_LINK_STATUS`） |
| 5 | `broadcastFrames` | array | 可读写，`broadcast_frames`（dict 列表） |
| 6 | `transmissions` | uint32 | 可读写，`transmissions` |
| 7 | `receivedFrames` | uint32 | 可读写，`received_frames`（校验正确） |
| 8 | `failedReceivedFrames` | uint32 | 可读写，`failed_received_frames`（校验错误） |
| 9 | `captureTime` | structure | 可读写，`capture_time`（dict） |

> 构造函数有 `access` 参数（无校验），CosemObject 子类，无 `deinit`（`broadcastFrames` 用 `arr_clear` 管理，`captureTime` 嵌入式）。

```python
import dlms
from dlms import AccessMode, Authentication

diag = dlms.MbusDiagnostic("0.0.24.8.0.255")

# ── 属性 1: logical_name ──
print(diag.logical_name)  # b'\x00\x00\x18\x08\x00\xff'

# ── 属性 2: received_signal_strength ──
print(diag.received_signal_strength)  # 0（默认）
diag.received_signal_strength = 100   # dBμV

# ── 属性 3: channel_id ──
diag.channel_id = 1

# ── 属性 4: link_status ──
print(diag.link_status)   # 0 (NONE)
diag.link_status = 1      # NORMAL

# ── 属性 5: broadcast_frames ──
diag.broadcast_frames = [
    {"client_id": 3, "counter": 7, "timestamp": (-1, -1, -1, 8, 0, 0)},
    {"client_id": 5, "counter": 20, "timestamp": None},
]
for f in diag.broadcast_frames:
    print(f["client_id"], f["counter"], f["timestamp"])

# ── 属性 6–8: 收发帧统计 ──
diag.transmissions = 0
diag.received_frames = 0
diag.failed_received_frames = 0

# ── 属性 9: capture_time ──
diag.capture_time = {"attribute_id": 2, "timestamp": (2026, 3, 6, 12, 0, 0)}
print(diag.capture_time["attribute_id"])  # 2
print(diag.capture_time["timestamp"])     # (2026, 3, 6, 12, 0, 0)

# ── 钩子：读前刷新信号强度（从硬件）──
def on_before_read(sender, attr_index, context):
    if attr_index == 2:  # received_signal_strength
        # sender.received_signal_strength = mbus_get_rssi()
        pass
    return dlms.DLMSEvent.ALLOW

diag.on_before_read = on_before_read

# ── 钩子：reset 动作 ──
def on_reset(sender, event):
    if event.index == 1:
        print("Reset counters requested")
        event.handled = 0   # 让 Gurux 清 C 侧计数器
        return True
    return True

diag.on_before_action = on_reset
```

`构造函数`

```python
MbusDiagnostic(logical_name: str, access: dict = None)
```
| 参数 | 类型 | 编号 | 默认值 | 说明 |
|------|------|:--:|--------|------|
| `logical_name` | `str` | 1 | 必传 | OBIS 代码，如 `"0.0.24.8.0.255"` |
| `access` | `dict` | — | `None` | 实例级权限，`{attr_index: (AccessMode, Authentication)}`（无校验） |

```python
import dlms
from dlms import AccessMode, Authentication

diag = dlms.MbusDiagnostic(
    "0.0.24.8.0.255",                       # logical_name（必传）
    access={                                 # 实例级权限（可选）
        2: (AccessMode.READ,       Authentication.NONE),   # received_signal_strength
        3: (AccessMode.READ,       Authentication.NONE),   # channel_id
        4: (AccessMode.READ,       Authentication.NONE),   # link_status
        5: (AccessMode.READ_WRITE, Authentication.HIGH),   # broadcast_frames
        6: (AccessMode.READ,       Authentication.NONE),   # transmissions
        7: (AccessMode.READ,       Authentication.NONE),   # received_frames
        8: (AccessMode.READ,       Authentication.NONE),   # failed_received_frames
        9: (AccessMode.READ,       Authentication.NONE),   # capture_time
        1: (AccessMode.AUTHENTICATED_WRITE, Authentication.HIGH),  # reset 方法
    },
)
```
`Python 属性一览`

| 属性 | 类型 | 可写 | 说明 |
|------|------|:--:|------|
| `logical_name` | `bytes` | × | OBIS 代码（6 字节） |
| `received_signal_strength` | `int` | √ | 接收信号强度（dBm / dBμV） |
| `channel_id` | `int` | √ | 当前使用的通道 ID |
| `link_status` | `int` | √ | 链路状态（见枚举表） |
| `broadcast_frames` | `list[dict]` | √ | `[{"client_id": int, "counter": int, "timestamp": 6tuple}, ...]` |
| `transmissions` | `int` | √ | 已发送帧数（uint32） |
| `received_frames` | `int` | √ | 校验正确的接收帧数（uint32） |
| `failed_received_frames` | `int` | √ | 校验错误的接收帧数（uint32） |
| `capture_time` | `dict` | √ | `{"attribute_id": int, "timestamp": 6tuple}` |
| `on_before_read` | `Callable` | √ | 读前钩子（刷新信号强度等） |
| `on_after_read` | `Callable` | √ | 读后钩子 |
| `on_before_action` | `Callable` | √ | **动作前钩子——method 1 `reset` 实现入口** |
| `on_after_action` | `Callable` | √ | 动作后钩子 |
| `access_dict` | `dict` | √ | 实例级访问控制 |

`link_status 枚举（DLMS_MBUS_LINK_STATUS）`

| 值 | 说明 |
|:--:|------|
| `0` | NONE — 从未收到数据 |
| `1` | NORMAL — 正常 |
| `2` | TEMPORARILY_INTERRUPTED — 临时中断 |
| `3` | PERMANENTLY_INTERRUPTED — 永久中断 |

`broadcast_frames 格式`

```python
# 每项为 dict，timestamp 是 6 元组（-1 表示通配）
diag.broadcast_frames = [
    {"client_id": 3, "counter": 7, "timestamp": (-1, -1, -1, 8, 0, 0)},
    {"client_id": 5, "counter": 20, "timestamp": None},   # timestamp 可省略（None）
]
```

`capture_time 格式`

```python
# 表示"最后一次状态变化的时间"
diag.capture_time = {"attribute_id": 2, "timestamp": (2026, 3, 6, 12, 0, 0)}
```
| 方法号 | 名称 | 说明 |
|:--:|------|------|
| 1 | `reset` | 复位计数器。**默认行为是 no-op**，实际清零逻辑写在 `on_before_action` 钩子中；除非你 `event.handled = 1` 阻止，否则 Gurux 也会清 C 侧计数器 |

```python
def on_reset(self, event):
    if event.index == 1:  # reset
        # 这里复位硬件计数器
        event.handled = 0   # 0=让 Gurux 也清 C 侧计数器（默认）；1=阻止默认
        return True
diag.on_before_action = on_reset
```

`访问控制`

```python
import dlms
from dlms import AccessMode, Authentication

diag = dlms.MbusDiagnostic("0.0.24.8.0.255")

dlms.set_default_access(dlms.MbusDiagnostic, {
    dlms.Authentication.NONE: {
        2: dlms.AccessMode.READ,      # received_signal_strength
        3: dlms.AccessMode.READ,      # channel_id
        4: dlms.AccessMode.READ,      # link_status
        6: dlms.AccessMode.READ,      # transmissions
        7: dlms.AccessMode.READ,      # received_frames
        8: dlms.AccessMode.READ,      # failed_received_frames
    },
    dlms.Authentication.HIGH: {
        1: dlms.AccessMode.AUTHENTICATED_WRITE,  # reset() 方法需 HIGH 认证
        5: dlms.AccessMode.READ_WRITE,           # broadcast_frames
    },
})
```

---
### G3-PLC 对象（ITU-T G.9903）

以下三个对象提供 G3-PLC 窄带电力线通信的 COSEM 属性结构。**重点：** 与 M-Bus 对象一样，这些目前为纯数据持有者——`dlms` 模块暴露 COSEM 属性结构供 DLMS 客户端读取配置和统计数据，**不实现 G3-PLC 传输本身**。应用层负责通过 `GenericConnection` 挂接 G3-PLC 调制解调器进行物理层通信，并将从传输层获取的数据填充到这些 COSEM 对象中。

> **注意：** 当前版本中 G3-PLC 动作方法没有 C 层派发，所有动作必须通过 `on_before_action` handler 实现。

---

#### `G3PlcMacCounters`

G3-PLC MAC 层计数器对象（其 **COSEM ID=90**）。记录设备在 G3-PLC（ITU-T G.9903）电力线通信网络中的 MAC 层数据包统计：收发数据包/命令包数量、CSMA 冲突失败、无 ACK、坏 CRC、广播收发计数。全部为 `uint32_t` 无符号计数器。

`Blue Book 属性`

| 编号 | 名称 | 类型 | 说明 |
|------|------|------|------|
| 1 | `logical_name` | octet-string(6) | OBIS 代码 |
| 2 | `tx_data_packet_count` | uint32 | 发送数据包总数 |
| 3 | `rx_data_packet_count` | uint32 | 接收数据包总数 |
| 4 | `tx_cmd_packet_count` | uint32 | 发送命令包总数 |
| 5 | `rx_cmd_packet_count` | uint32 | 接收命令包总数 |
| 6 | `csma_fail_count` | uint32 | CSMA 失败次数 |
| 7 | `csma_no_ack_count` | uint32 | CSMA 未收到 ACK 次数 |
| 8 | `bad_crc_count` | uint32 | 坏 CRC 帧数 |
| 9 | `tx_data_broadcast_count` | uint32 | 发送广播数据包数 |
| 10 | `rx_data_broadcast_count` | uint32 | 接收广播数据包数 |

`构造函数`

```python
G3PlcMacCounters(logical_name: str, access: dict = None)
```
```python
import dlms
from dlms import AccessMode, Authentication

counters = dlms.G3PlcMacCounters("0.0.29.1.0.255")

# ── 属性 1: logical_name ──
print(counters.logical_name)  # b'\x00\x00\x1d\x01\x00\xff'

# ── 属性 2–5: 数据/命令包统计 ──
counters.tx_data_packet_count = 100
counters.rx_data_packet_count = 98
counters.tx_cmd_packet_count = 20
counters.rx_cmd_packet_count = 19

# ── 属性 6–8: 通信质量指标 ──
counters.csma_fail_count = 3      # 信道冲突
counters.csma_no_ack_count = 1    # 无 ACK
counters.bad_crc_count = 0        # 坏帧

# ── 属性 9/10: 广播统计 ──
counters.tx_data_broadcast_count = 5
counters.rx_data_broadcast_count = 4

# ── 读前钩子：从 PHY 同步计数 ──
def on_before_read(sender, attr_index, context):
    # 从 G3-PLC 调制解调器寄存器读取并更新
    # sender.tx_data_packet_count = plc_get_counter(0x01)
    pass
    return dlms.DLMSEvent.ALLOW
counters.on_before_read = on_before_read

# ── reset 动作钩子（唯一实现）──
def do_reset(sender, event):
    if event.index == 1:
        sender.tx_data_packet_count = 0
        sender.rx_data_packet_count = 0
        sender.tx_cmd_packet_count = 0
        sender.rx_cmd_packet_count = 0
        sender.csma_fail_count = 0
        sender.csma_no_ack_count = 0
        sender.bad_crc_count = 0
        sender.tx_data_broadcast_count = 0
        sender.rx_data_broadcast_count = 0
        # plc_reset_counters()  ← 硬件清零
        return True
    return True
counters.on_before_action = do_reset

```

`Python 属性一览`

| 属性 | 类型 | 可写 | 说明 |
|------|------|:--:|------|
| `logical_name` | `bytes` | × | OBIS 代码（6 字节） |
| `tx_data_packet_count` | `int` | √ | 成功发送的数据包数 |
| `rx_data_packet_count` | `int` | √ | 成功接收的数据包数 |
| `tx_cmd_packet_count` | `int` | √ | 成功发送的命令包数 |
| `rx_cmd_packet_count` | `int` | √ | 成功接收的命令包数 |
| `csma_fail_count` | `int` | √ | CSMA 退避达到 `macMaxCSMABackoffs` 的次数（信道冲突失败） |
| `csma_no_ack_count` | `int` | √ | 发送单播数据帧未收到 ACK 的次数 |
| `bad_crc_count` | `int` | √ | 接收到的坏 CRC 帧数 |
| `tx_data_broadcast_count` | `int` | √ | 发送的广播帧数 |
| `rx_data_broadcast_count` | `int` | √ | 成功接收的广播帧数 |
| `on_before_read` | `Callable` | √ | 读前钩子（从 PHY 同步计数） |
| `on_after_read` | `Callable` | √ | 读后钩子 |
| `on_before_action` | `Callable` | √ | **动作前钩子——method 1 `reset` 的唯一实现** |
| `on_after_action` | `Callable` | √ | 动作后钩子 |
| `access_dict` | `dict` | √ | 实例级访问控制 |



`访问控制`

```python
import dlms
from dlms import AccessMode, Authentication

counters = dlms.G3PlcMacCounters("0.0.29.1.0.255")

dlms.set_default_access(dlms.G3PlcMacCounters, {
    dlms.Authentication.NONE: {
        2: dlms.AccessMode.READ,   # tx_data_packet_count
        3: dlms.AccessMode.READ,   # rx_data_packet_count
        4: dlms.AccessMode.READ,   # tx_cmd_packet_count
        5: dlms.AccessMode.READ,   # rx_cmd_packet_count
        6: dlms.AccessMode.READ,   # csma_fail_count
        7: dlms.AccessMode.READ,   # csma_no_ack_count
        8: dlms.AccessMode.READ,   # bad_crc_count
        9: dlms.AccessMode.READ,   # tx_data_broadcast_count
        10: dlms.AccessMode.READ,  # rx_data_broadcast_count
    },
    dlms.Authentication.HIGH: {
        1: dlms.AccessMode.AUTHENTICATED_WRITE,  # reset() 方法需 HIGH 认证
    },
})
```

---

####  `G3PlcMacSetup`

G3-PLC MAC 层配置对象（其 **COSEM ID=91**）。配置 G3-PLC（ITU-T G.9903）电力线通信设备的 MAC 层参数：网络身份（短地址/PAN ID/协调器）、加密密钥表、CSMA 退避参数、邻居表、MAC 位置表和帧重试等。**这是 G3-PLC 家族中属性最多的对象**（25 个属性）。

> **数据流**：G3-PLC MAC 层由调制解调器硬件实现，这些参数需要下发到 PHY/MAC 固件生效。**Gurux 只存储不执行**——真实配置生效逻辑（写寄存器）需在钩子或应用层完成。

`Blue Book 核心属性`

| 编号 | 名称 | 类型 | 说明 |
|------|------|------|------|
| 1 | `logical_name` | octet-string(6) | OBIS 代码 |
| 2 | `short_address` | uint16 | 本节点 MAC 短地址 |
| 3 | `rc_coord` | uint8 | 到协调器的路由成本 |
| 4 | `pan_id` | uint16 | PAN 标识符 |
| 5 | `key_table` | ARRAY | 密钥表 `[{"id": int, "key": bytes(16)}, ...]`（⏩ COMPLEX） |
| 6 | `frame_counter` | uint32 | 帧计数器 |
| 7 | `tone_mask` | bytes | 活跃子载波 packed bit-array |
| 8 | `tmr_ttl` | uint8 | TMR 生命周期 |
| 9 | `max_frame_retries` | uint8 | 最大帧重传次数 |
| 10 | `neighbour_table_entry_ttl` | uint8 | 邻区条目生命周期（秒） |
| 11 | `neighbour_table` | ARRAY | 邻区表 `[{short_address, lqi, valid_time, ...}, ...]`（⏩ COMPLEX） |
|12 | `high_priority_window_size` | int | 高优先级窗口大小 |
|13 |`cscm_fairness_limit` | int |CSCM 公平性限制 |
| 14|`beacon_randomization_window_length` | int | 信标随机化窗口长度 |
|15/16 | `mac_a` / `mac_k` | int| MAC 层 A/K 参数 |
| 17|`min_cw_attempts` | int | 最小竞争窗口尝试次数 |
| 18|`cenelec_legacy_mode` | int | CENELEC 兼容模式 |
| 19|`fcc_legacy_mode` | int| FCC 兼容模式 |
| 20|`max_be`  | int| 最大退避指数 |
| 21|`max_csma_backoffs` | int| 最大 CSMA 退避次数 |
| 22|`min_be` | int| 最小退避指数 |
| 23|`mac_broadcast_max_cw_enabled` | int | 广播最大竞争窗口开关（bool） |
| 24|`mac_transmit_atten` | int | 输出衰减（dB） |
| 25|`mac_pos_table` | list[dict] | `[{"short_address", "lqi", "valid_time"}]` |
| 26|`mac_duplicate_detection_ttl` | int | 重复帧检测时间（秒） |

`构造函数`

```python
G3PlcMacSetup(logical_name: str, access: dict = None)
```
```python
import dlms
from dlms import AccessMode, Authentication

mac = dlms.G3PlcMacSetup("0.0.29.0.0.255")

# ── 属性 2–4: 网络身份 ──
mac.short_address = 0x1001
mac.rc_coord = 0x0000
mac.pan_id = 0x1234

# ── 属性 5: key_table ──
mac.key_table = [
    {"id": 1, "key": b"\x01\x02\x03\x04\x05\x06\x07\x08"},
    {"id": 2, "key": b"\x11\x12\x13\x14\x15\x16\x17\x18"},
]
for entry in mac.key_table:
    print(entry["id"], entry["key"])

# ── 属性 6: frame_counter ──
mac.frame_counter = 0

# ── 属性 7: tone_mask（bytes）──
mac.tone_mask = b"\xff\xff"   # 全频段启用

# ── 属性 8–14: 时序参数 ──
mac.tmr_ttl = 60
mac.max_frame_retries = 3
mac.neighbour_table_entry_ttl = 600
mac.high_priority_window_size = 2
mac.cscm_fairness_limit = 1
mac.beacon_randomization_window_length = 2

# ── 属性 15–24: CSMA 与模式 ──
mac.mac_a = 1
mac.mac_k = 8
mac.min_cw_attempts = 4
mac.cenelec_legacy_mode = 0
mac.fcc_legacy_mode = 0
mac.max_be = 8
mac.max_csma_backoffs = 4
mac.min_be = 3
mac.mac_broadcast_max_cw_enabled = 0
mac.mac_transmit_atten = 0

# ── 属性 25: mac_pos_table ──
mac.mac_pos_table = [
    {"short_address": 0x1002, "lqi": 200, "valid_time": 300},
]

# ── 属性 26: mac_duplicate_detection_ttl ──
mac.mac_duplicate_detection_ttl = 30

# ── 属性 11: neighbour_table ──
mac.neighbour_table = [{
    "short_address": 0x1002,
    "payload_modulation_scheme": 1,
    "tone_map": b"\xff",
    "modulation": 1,
    "tx_gain": 0,
    "tx_res": 0,
    "tx_coeff": b"\x01\x02",
    "lqi": 200,
    "phase_differential": 0,
    "tmr_valid_time": 50,
    "no_data": 0,
}]
for n in mac.neighbour_table:
    print(n["short_address"], n["lqi"])

# ── get_neighbour_table 动作钩子（唯一实现）──
def on_get_neighbour(self, event):
    if event.index == 1:
        # 从 G3-PLC 硬件读取真实邻居表，写入 mac.neighbour_table
        # self.neighbour_table = plc_get_neighbours()
        return True
    return True
mac.on_before_action = on_get_neighbour

```

`Python 属性一览（钩子回调）`

| 属性 | 类型 | 可写 | 说明 |
|------|------|:--:|------|
| `on_before_read` | `Callable` | √ | 读前钩子 |
| `on_after_read` | `Callable` | √ | 读后钩子 |
| `on_before_action` | `Callable` | √ | **动作前钩子——method 1 `get_neighbour_table` 的唯一实现** |
| `on_after_action` | `Callable` | √ | 动作后钩子 |
| `access_dict` | `dict` | √ | 实例级访问控制 |

> 邻区表应在客户端读取前从 G3-PLC 调制解调器刷新。

`访问控制`

```python
import dlms
from dlms import AccessMode, Authentication

mac = dlms.G3PlcMacSetup("0.0.29.0.0.255")

dlms.set_default_access(dlms.G3PlcMacSetup, {
    dlms.Authentication.NONE: {
        2: dlms.AccessMode.READ,   # short_address
        4: dlms.AccessMode.READ,   # pan_id
        6: dlms.AccessMode.READ,   # frame_counter
    },
    dlms.Authentication.HIGH: {
        2: dlms.AccessMode.READ_WRITE,   # short_address: HIGH 可写
        5: dlms.AccessMode.READ_WRITE,   # key_table: HIGH 可写（密钥安全）
        7: dlms.AccessMode.READ_WRITE,   # tone_mask
        1: dlms.AccessMode.AUTHENTICATED_WRITE,  # get_neighbour_table 方法
    },
})
```

---

#### `G3Plc6LoWPAN`

G3-PLC 6LoWPAN 适配层配置对象（其 **COSEM ID=92**）。管理 G3-PLC（ITU-T G.9903）网络中节点的 **6LoWPAN/LOADng 路由配置**：最大跳数、弱链路阈值、安全级别、前缀表、路由配置/路由表、上下文信息表、黑名单表、广播日志表、组表、目的地址表和 LQI 阈值。

> **协议栈位置**：`G3PlcMacSetup`(91) 管 MAC 层（CSMA/帧重试），`G3Plc6LoWPAN`(92) 管其上的 **6LoWPAN 适配层**（IPv6 压缩 + LOADng 路由）。每个字段都有对应的 PIB 属性（0x02–0xF0），最终写入 G3-PLC 调制解调器寄存器生效。

`Blue Book 核心属性`

| 编号 | 名称 | 类型 | 说明 |
|------|------|------|------|
| 1 | `logical_name` | octet-string(6) | OBIS 代码 |
| 2 | `max_hops` | uint8 | 最大 LOADng 路由跳数（PIB 0x02） |
| 3 | `weak_lqi_value` | uint8 | "弱链路"的 LQI 阈值（PIB 0x04 低阈值） |
| 4 | `security_level` | uint8 | 适配帧最低安全等级 |
| 5 | `prefix_table` | bytes | PAN 前缀列表（⏩ COMPLEX） |
| 6 | `routing_configuration` | ARRAY | LOADng 路由参数，14 字段 dict（⏩ COMPLEX） |
| 7 | `broadcast_log_table_entry_ttl` | uint16 | 广播日志 TTL（分钟） |
| 8 | `routing_table` | ARRAY | LOADng 路由表 `[{destination, next_hop, cost, hop_count, weak_link_count, valid_time}]`（⏩ COMPLEX） |
| 9 | `context_information_table` | ARRAY | 6LoWPAN 上下文信息 `[{cid, context_length, context, compression, valid_lifetime}]`（⏩ COMPLEX） |
| 10 | `blacklist_table` | ARRAY | 黑名单邻区 `[{neighbour_address, valid_time}]`（⏩ COMPLEX） |
| 11 | `broadcast_log_table` | ARRAY | 广播日志 `[{source_address, sequence_number, valid_time}]`（⏩ COMPLEX） |
| 12 | `group_table` | ARRAY | 本设备注册的组地址（uint16 列表）（⏩ COMPLEX） |
| 13 | `max_join_wait_time` | uint16 | 网络加入超时（秒，LBD，PIB 0x20） |
| 14 | `path_discovery_time` | uint8 | 路径发现超时（秒，PIB 0x21） |
| 15 | `active_key_index` | uint8 | 活跃 GMK 密钥索引（PIB 0x22） |
| 16 | `metric_type` | uint8 | LOADng 路由度量类型（PIB 0x03） |
| 17 | `coord_short_address` | uint16 | 协调器短地址（PIB 0x08） |
| 18 | `disable_default_routing` | uint8 | 1=禁用 LOADng 默认路由（PIB 0xF0） |
| 19 | `device_type` | uint8 | 设备类型（`DLMS_PAN_DEVICE_TYPE`，PIB 0x10） |
| 20 | `default_coord_route_enabled` | uint8 | 1=创建到协调器的默认路由（PIB 0x24） |
| 21 | `destination_address` | ARRAY | 本路由器提供连通性的地址列表（uint16）（⏩ COMPLEX，PIB 0x23） |
| 22 | `low_lqi` | uint8 | 低 LQI 阈值（PIB 0x04） |
| 23 | `high_lqi` | uint8 | 高 LQI 阈值（PIB 0x04） |

> **22 个属性全暴露**（编号 2–23）。构造函数有 `access` 参数（无校验），CosemObject 子类。**无 COSEM 动作方法**（class 92 未定义任何 method，`on_before_action`/`on_after_action` 不触发）。
`构造函数`

```python
G3Plc6LoWPAN(logical_name: str, access: dict = None)
```
```python
import dlms
from dlms import AccessMode, Authentication

pan = dlms.G3Plc6LoWPAN("0.0.29.2.0.255")

# ── 属性 1: logical_name ──
print(pan.logical_name)  # b'\x00\x00\x1d\x02\x00\xff'

# ── 属性 2–4: 基本参数 ──
pan.max_hops = 8
pan.weak_lqi_value = 150
pan.security_level = 5

# ── 属性 5: prefix_table ──
pan.prefix_table = b"\x20\x01\x0d\xb8"  # 前缀字节

# ── 属性 6: routing_configuration（14 字段）──
pan.routing_configuration = [{
    "net_traversal_time": 30,
    "routing_table_entry_ttl": 300,
    "kr": 3, "km": 2, "kc": 3, "kq": 2, "kh": 3, "krt": 2,
    "rreq_retries": 3,
    "rreq_req_wait": 5,
    "blacklist_table_entry_ttl": 600,
    "unicast_rreq_gen_enable": 1,
    "rlc_enabled": 1,
    "add_rev_link_cost": 1,
}]

# ── 属性 7/13–18: 时序与路由 ──
pan.broadcast_log_table_entry_ttl = 30
pan.max_join_wait_time = 60
pan.path_discovery_time = 5
pan.active_key_index = 1
pan.metric_type = 0
pan.coord_short_address = 0x0000
pan.disable_default_routing = 0
pan.device_type = 2
pan.default_coord_route_enabled = 1

# ── 属性 8: routing_table（6 字段）──
pan.routing_table = [{
    "destination_address": 0x1002,
    "next_hop_address": 0x1003,
    "route_cost": 10,
    "hop_count": 2,
    "weak_link_count": 0,
    "valid_time": 300,
}]

# ── 属性 9: context_information_table（5 字段）──
pan.context_information_table = [{
    "cid": 1,
    "context_length": 8,
    "context": b"\x20\x01\x0d\xb8\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00",
    "compression": 1,
    "valid_lifetime": 600,
}]

# ── 属性 10/11: 黑名单 & 广播日志 ──
pan.blacklist_table = [{"neighbour_address": 0x2001, "valid_time": 600}]
pan.broadcast_log_table = [{"source_address": 0x1001, "sequence_number": 5, "valid_time": 30}]

# ── 属性 12/21: 组表 & 目的地址（uint16 列表）──
pan.group_table = [0x0001, 0x0002]
pan.destination_address = [0x1001, 0x1002]

# ── 属性 22/23: LQI 阈值 ──
pan.low_lqi = 100
pan.high_lqi = 200

```

`Python 属性一览`

| 属性 | 类型 | 可写 | 说明 |
|------|------|:--:|------|
| `on_before_read` | `Callable` | √ | 读前钩子 |
| `on_after_read` | `Callable` | √ | 读后钩子 |
| `access_dict` | `dict` | √ | 实例级访问控制 |

`子结构 dict 格式`

| 表 | 字段数 | 键 |
|------|:--:|------|
| `routing_configuration` | 14 | `net_traversal_time`, `routing_table_entry_ttl`, `kr`, `km`, `kc`, `kq`, `kh`, `krt`, `rreq_retries`, `rreq_req_wait`, `blacklist_table_entry_ttl`, `unicast_rreq_gen_enable`, `rlc_enabled`, `add_rev_link_cost` |
| `routing_table` | 6 | `destination_address`, `next_hop_address`, `route_cost`, `hop_count`, `weak_link_count`, `valid_time` |
| `context_information_table` | 5 | `cid`, `context_length`, `context`(bytes,16), `compression`, `valid_lifetime` |
| `blacklist_table` | 2 | `neighbour_address`, `valid_time` |
| `broadcast_log_table` | 3 | `source_address`, `sequence_number`, `valid_time` |

`访问控制`

```python
import dlms
from dlms import AccessMode, Authentication

pan = dlms.G3Plc6LoWPAN("0.0.29.2.0.255")

dlms.set_default_access(dlms.G3Plc6LoWPAN, {
    dlms.Authentication.NONE: {
        2: dlms.AccessMode.READ,   # max_hops
        8: dlms.AccessMode.READ,   # routing_table
        19: dlms.AccessMode.READ,  # device_type
    },
    dlms.Authentication.HIGH: {
        2: dlms.AccessMode.READ_WRITE,   # max_hops: HIGH 可写
        6: dlms.AccessMode.READ_WRITE,   # routing_configuration
        8: dlms.AccessMode.READ_WRITE,   # routing_table
        9: dlms.AccessMode.READ_WRITE,   # context_information_table
    },
})
```

---
### 预付费子系统（Prepayment）

预付费子系统由四个协作类组成：

| 类 ID | 类名 | OBIS | 角色 |
|-------|------|------|------|
| 111 | `Account` | `0.0.19.0.0.255` | 顶层控制器，串联 Credit / Charge / TokenGateway |
| 112 | `Credit` | `0.0.19.10.0.255` | 信用额度（余额）管理 |
| 113 | `Charge` | `0.0.19.20.0.255` | 基于消费量的费用计算 |
| 115 | `TokenGateway` | `0.0.19.40.0.255` | Token 入口、验证与执行 |

> **重要**：这四个类**均无 C 端动作调度**。所有方法（method）处理必须通过 `on_before_action` 回调在 Python 侧实现。

---

`数据流概述`

```
客户端/键盘 → TokenGateway.enter(token)
                         ↓
              on_before_action 验证 token
                         ↓
              TokenGateway.token / time / status 更新
                         ↓
              Account 根据 token_gateway_configurations
              按比例分配额度到对应 Credit
                         ↓
              Credit.current_credit_amount 增加
              Credit.status 更新
                         ↓
              Account 重算 available_credit /
              current_credit_status
                         ↓
              Charge.collect() → 根据 tariff 扣除 Credit 余额
```

---

#### `TokenGateway`

预付费（prepayment）计量中的 **Token 网关**对象，典型 OBIS 代码 `0.0.19.40.0.255`。负责充值 Token 的**录入（enter）→ 验证（verify）→ 执行（execute）**全流程管理：接收远端/本地/手动投递的充值 Token，记录最后一次处理的 Token 及其元数据，并将结果信用暴露给 `Credit` / `Account` 等对象使用。

`Blue Book 属性`

| 编号 | Python 名称 | 类型 | 说明 |
|------|-------------|------|------|
| 1 | `logical_name` | octet-string(6) | OBIS 代码，只读 |
| 2 | `token` | `bytes` | 最后一次接受的 token 原始数据 |
| 3 | `time` | `tuple(6)` | token 处理时间 `(年,月,日,时,分,秒)` |
| 4 | `descriptions` | `list[str]` | token 描述列表（每条描述是一个字符串） |
| 5 | `delivery_method` | `TokenDelivery` | Token 投递方式（REMOTE / LOCAL / MANUAL） |
| 6 | `status` | `TokenStatusCode` | Token 处理状态码 |
| 7 | `data_value` | `bytes` | Bit 数组形式的附加数据 |

` 方法（均需通过 `on_before_action` 实现）`

| 编号 | 名称 | 说明 |
|------|------|------|
| 1 | `enter` | 录入 token |
| 2 | `verify` | 验证 token |
| 3 | `execute` | 执行 token |

`构造函数`

```python
TokenGateway(logical_name: str, access: dict = None)
```
```python
import dlms
from dlms import AccessMode, Authentication

gw = dlms.TokenGateway("0.0.19.40.0.255",access={2: (AccessMode.READ_WRITE, Authentication.HIGH)})

# ── 属性 1: logical_name ──
print(gw.logical_name)  # "0.0.19.40.0.255"

# ── 属性 2: token（最后一次处理的 Token 原始数据）──
gw.token = b"\x12\x34\x56\x78"

# ── 属性 3: time（处理时间戳）──
gw.time = (2024, 1, 15, 10, 30, 0)

# ── 属性 4: descriptions（信用类型描述列表）──
gw.descriptions = ["Gas meter token", "Electric token"]

# ── 属性 5: delivery_method（投递方式）──
gw.delivery_method = dlms.TokenDelivery.REMOTE

# ── 属性 6: status（处理状态）──
gw.status = dlms.TokenStatusCode.FORMAT_OK

# ── 属性 7: data_value（处理后的位数据）──
gw.data_value = b"\x01\x02"

# ── 方法处理：enter(1) / verify(2) / execute(3) 统一入口 ──
def on_token_action(self, event):
    if event.index == 1:   # enter
        raw = self.token
        print("[TokenGateway] enter token={!r}".format(raw))
        if raw and raw[0] != 0x00:
            self.status = dlms.TokenStatusCode.VALIDATION_OK
            self.time = (2025, 6, 15, 10, 30, 0)
        else:
            self.status = dlms.TokenStatusCode.TOKEN_FORMAT_FAILURE
    elif event.index == 2: # verify
        # ...解密 / STS 校验逻辑...
        self.status = dlms.TokenStatusCode.AUTHENTICATION_OK
    elif event.index == 3: # execute
        # ...解析 Token 金额并更新对应 Credit...
        self.status = dlms.TokenStatusCode.TOKEN_EXECUTION_OK
    return True

gw.on_before_action = on_token_action
```

| 方法 | COSEM 方法 | 说明 |
|------|-----------|------|
| `enter` | 1 | 录入一个充值 Token（标准入口） |
| `verify` | 2 | 验证 Token 的有效性 |
| `execute` | 3 | 执行 Token，将信用更新到 `Credit` 对象 |

> **重要**：三个方法**全部**经由 `on_before_action` 派发，需自行实现处理逻辑。Gurux 原生实现仅处理方法 1（`enter`），对 2/3 会报错；本移植将其统一标记为已处理以保持一致行为。

枚举：`TokenStatusCode`

Token 处理结果码（对应 `DLMS_TOKEN_STATUS_CODE_*`），通过 `dlms.TokenStatusCode` 访问：

| 常量 | 值 | 说明 |
|------|----|------|
| `TokenStatusCode.FORMAT_OK` | `0` | 格式正确 |
| `TokenStatusCode.AUTHENTICATION_OK` | `1` | 认证通过 |
| `TokenStatusCode.VALIDATION_OK` | `2` | 验证通过 |
| `TokenStatusCode.TOKEN_EXECUTION_OK` | `3` | 执行成功 |
| `TokenStatusCode.TOKEN_FORMAT_FAILURE` | `4` | 格式错误 |
| `TokenStatusCode.AUTHENTICATION_FAILURE` | `5` | 认证失败 |
| `TokenStatusCode.VALIDATION_RESULT_FAILURE` | `6` | 验证结果失败 |
| `TokenStatusCode.TOKEN_EXECUTION_RESULT_FAILURE` | `7` | 执行结果失败 |
| `TokenStatusCode.TOKEN_RECEIVED` | `8` | Token 已接收（待处理） |

枚举：`TokenDelivery`

Token 投递通道（对应 `DLMS_TOKEN_DELIVERY_*`），通过 `dlms.TokenDelivery` 访问：

| 常量 | 值 | 说明 |
|------|----|------|
| `TokenDelivery.REMOTE` | `0` | 远程投递（如通过通信网络下发） |
| `TokenDelivery.LOCAL` | `1` | 本地投递（如按键/本地接口录入） |
| `TokenDelivery.MANUAL` | `2` | 手动投递（如人工抄表写入） |

| 参数 | 类型 | 默认值 | 说明 |
|------|------|--------|------|
| `logical_name` | `str` | 必传 | OBIS 代码字符串，如 `"0.0.19.40.0.255"` |
| `access` | `dict` | `None` | 实例级权限，格式 `{auth: {attr_index: AccessMode}}` |

`Python 属性一览`

| 属性 | 类型 | 可写 | 说明 |
|------|------|:--:|------|
| `on_before_read` | `Callable` | √ | 读前钩子（继承自 CosemObject） |
| `on_after_read` | `Callable` | √ | 读后钩子（继承自 CosemObject） |
| `on_before_write` | `Callable` | √ | 写前钩子（继承自 CosemObject） |
| `on_after_write` | `Callable` | √ | 写后钩子（继承自 CosemObject） |
| `on_before_action` | `Callable` | √ | 动作前钩子（**所有方法的实现入口**） |
| `on_after_action` | `Callable` | √ | 动作后钩子（继承自 CosemObject） |
| `access_dict` | `dict` | √ | 实例级访问控制 |

`访问控制`

```python
import dlms
# 构造函数中指定
gateway = dlms.TokenGateway("0.0.19.40.0.255",
    access={2: (AccessMode.READ_WRITE, Authentication.HIGH)})

# 或事后修改
gateway.access_dict = {2: (AccessMode.READ_WRITE, Authentication.HIGH)}
```

`典型用例`

| 场景 | 示例 |
|------|------|
| 远程下发充值 | `delivery_method = TokenDelivery.REMOTE` |
| 本地键盘录入 | `delivery_method = TokenDelivery.LOCAL` |
| 校验失败上报 | `status = TokenStatusCode.AUTHENTICATION_FAILURE` |

---

#### `Credit`

预付费（prepayment）计量中的**信用额度**对象，典型 OBIS 代码 `0.0.19.10.0.255`。管理单个充值信用的余额与配置：跟踪当前余额（`current_credit_amount`），定义信用类型、优先级、告警阈值、欠费上限等参数。一个 `Account` 下可同时存在多个 `Credit` 对象，配合 `TokenGateway` / `Charge` 完成充值 → 扣费全流程。

`Blue Book 属性`

| 编号 | 名称 | 类型 | 说明 |
|------|------|------|------|
| 1 | `logical_name` | octet-string(6) | OBIS 代码，只读 |
| 2 | `current_credit_amount` | int32 | 当前信用余额（有符号） |
| 3 | `type` | enum | 信用类型（`CreditType`） |
| 4 | `priority` | uint8 | 优先级（值越小越优先消耗） |
| 5 | `warning_threshold` | int32 | 低余额告警阈值 |
| 6 | `limit` | int32 | 余额下限（可为负数，表示允许的债务额度） |
| 7 | `credit_configuration` | bitmask | 信用配置位掩码（`CreditConfiguration`） |
| 8 | `status` | uint8 | 信用生命周期状态（`CreditStatus`，⏩ VOLATILE） |
| 9 | `preset_credit_amount` | int32 | 预设信用额度（下次充值时载入） |
| 10 | `credit_available_threshold` | int32 | 信用可用阈值（低于此值阻断） |
| 11 | `period` | octet-string | 周期时间 `(year, month, day, hour, min, sec)` |

`构造函数`

```python
Credit(logical_name: str, access: dict = None)
```

```python
import dlms
from dlms import AccessMode, Authentication

credit = dlms.Credit("0.0.19.10.0.255", access={2: (AccessMode.READ_WRITE, Authentication.HIGH)})

# ── 属性 1: logical_name ──
print(credit.logical_name)  # "0.0.19.10.0.255"

# ── 属性 2: current_credit_amount（当前余额）──
credit.current_credit_amount = 5000

# ── 属性 3: type（信用类型）──
credit.type = dlms.CreditType.TOKEN

# ── 属性 4: priority（优先级，越小越优先消耗）──
credit.priority = 1

# ── 属性 5: warning_threshold（告警阈值）──
credit.warning_threshold = 500

# ── 属性 6: limit（下限，允许 200 的债务）──
credit.limit = -200

# ── 属性 7: credit_configuration（配置位掩码，可组合）──
credit.credit_configuration = (dlms.CreditConfiguration.VISUAL |
                               dlms.CreditConfiguration.TOKENS)

# ── 属性 8: status（生命周期状态）──
credit.status = dlms.CreditStatus.ENABLED

# ── 属性 9: preset_credit_amount（预设充值额度）──
credit.preset_credit_amount = 10000

# ── 属性 10: credit_available_threshold（可用阈值）──
credit.credit_available_threshold = 100

# ── 属性 11: period（周期时间）──
credit.period = (2024, 1, 1, 0, 0, 0)

# ── 方法处理：update_amount(1) / set_amount_to_value(2) / invoke_credit(3) ──
def on_credit_action(self, event):
    if event.index == 1:   # update_amount：按增量调整余额
        delta = event.parameters if isinstance(event.parameters, int) else 0
        self.current_credit_amount += delta
        print("[Credit] update_amount delta={} balance={}".format(
            delta, self.current_credit_amount))
    elif event.index == 2: # set_amount_to_value：设为绝对值
        self.current_credit_amount = event.parameters
        print("[Credit] set_amount_to_value balance={}".format(
            self.current_credit_amount))
    elif event.index == 3: # invoke_credit：从预设额度充值
        self.current_credit_amount += self.preset_credit_amount
        self.status = dlms.CreditStatus.IN_USE
        print("[Credit] invoke_credit balance={}".format(
            self.current_credit_amount))
    return True

credit.on_before_action = on_credit_action
```

`方法（COSEM 动作）`

| 方法 | COSEM 方法 | 说明 |
|------|-----------|------|
| `update_amount` | 1 | 按增量调整当前信用余额（delta 可为负） |
| `set_amount_to_value` | 2 | 将余额设为指定绝对值 |
| `invoke_credit` | 3 | 激活信用：从 `preset_credit_amount` 载入充值额度 |

> **重要**：三个方法**全部**经由 `on_before_action` 派发，需自行实现处理逻辑（C 层在 events.c 中统一置 `e->handled=1`）。

枚举：`CreditType`

信用类型（对应 `DLMS_CREDIT_TYPE_*`），通过 `dlms.CreditType` 访问：

| 常量 | 值 | 说明 |
|------|----|------|
| `CreditType.TOKEN` | `0` | Token 信用（充值 Token 带来的信用） |
| `CreditType.RESERVED` | `1` | 预留信用 |
| `CreditType.EMERGENCY` | `2` | 紧急信用（欠费断电后临时供电） |
| `CreditType.TIME_BASED` | `3` | 按时间计费的信用 |
| `CreditType.CONSUMPTION_BASED` | `4` | 按用量计费的信用 |

枚举：`CreditStatus`

信用生命周期状态（对应 `DLMS_CREDIT_STATUS_*`），通过 `dlms.CreditStatus` 访问：

| 常量 | 值 | 说明 |
|------|----|------|
| `CreditStatus.ENABLED` | `0` | 已启用 |
| `CreditStatus.SELECTABLE` | `1` | 可被选择/激活 |
| `CreditStatus.INVOKED` | `2` | 已被调用/激活 |
| `CreditStatus.IN_USE` | `3` | 使用中 |
| `CreditStatus.CONSUMED` | `4` | 已消耗 |

枚举：`CreditConfiguration`

信用配置位掩码（对应 `DLMS_CREDIT_CONFIGURATION_*`），通过 `dlms.CreditConfiguration` 访问，可多值按位或：

| 常量 | 值 | 说明 |
|------|----|------|
| `CreditConfiguration.NONE` | `0x00` | 无配置 |
| `CreditConfiguration.VISUAL` | `0x01` | 需要视觉指示 |
| `CreditConfiguration.CONFIRMATION` | `0x02` | 激活前需要确认 |
| `CreditConfiguration.PAID_BACK` | `0x04` | 信用金额需要偿还 |
| `CreditConfiguration.RESETTABLE` | `0x08` | 可重置 |
| `CreditConfiguration.TOKENS` | `0x10` | 可接收 Token 充值 |

```python
credit.credit_configuration = (dlms.CreditConfiguration.VISUAL |
                               dlms.CreditConfiguration.TOKENS)
```

枚举：`CreditCollectionConfiguration`

信用回收条件位掩码（对应 `DLMS_CREDIT_COLLECTION_CONFIGURATION_*`），通过 `dlms.CreditCollectionConfiguration` 访问，可多值按位或：

| 常量 | 值 | 说明 |
|------|----|------|
| `CreditCollectionConfiguration.NONE` | `0x00` | 无 |
| `CreditCollectionConfiguration.DISCONNECTED` | `0x01` | 断电状态下回收 |
| `CreditCollectionConfiguration.LOAD_LIMITING` | `0x02` | 限载状态下回收 |
| `CreditCollectionConfiguration.FRIENDLY_CREDIT` | `0x04` | 友好信用（暂缓断电） |

`Python 属性一览`

| 属性 | 类型 | 可写 | 说明 |
|------|------|:--:|------|
| `on_before_read` | `Callable` | √ | 读前钩子（继承自 CosemObject） |
| `on_after_read` | `Callable` | √ | 读后钩子（继承自 CosemObject） |
| `on_before_write` | `Callable` | √ | 写前钩子（继承自 CosemObject） |
| `on_after_write` | `Callable` | √ | 写后钩子（继承自 CosemObject） |
| `on_before_action` | `Callable` | √ | 动作前钩子（**所有方法的实现入口**） |
| `on_after_action` | `Callable` | √ | 动作后钩子（继承自 CosemObject） |
| `access_dict` | `dict` | ✅ | 实例级访问控制 |

`访问控制`

```python
import dlms
from dlms import AccessMode, Authentication

# 构造函数中指定
credit = dlms.Credit("0.0.19.10.0.255",
    access={2: (AccessMode.READ_WRITE, Authentication.HIGH)})

# 或事后修改
credit.access_dict = {2: (AccessMode.READ_WRITE, Authentication.HIGH)}
```

`典型用例`

| 场景 | 示例 |
|------|------|
| Token 充值信用 | `type = CreditType.TOKEN`，`credit_configuration \|= TOKENS` |
| 紧急供电 | `type = CreditType.EMERGENCY` |
| 允许欠费 | `limit = -200` |
| 充值后激活 | `invoke_credit`（方法 3，从 `preset_credit_amount` 载入） |
---

#### `Charge`

预付费（prepayment）计量中的**计费**对象，典型 OBIS 代码 `0.0.19.20.0.255`。根据能量/用量消耗，通过费率表（`unit_charge_active` / `unit_charge_passive`）计算费用，并将结果累计到 `Account` 的聚合债务（`total_amount_remaining`）中。一个 `Account` 下可同时存在多个 `Charge` 对象（不同计费项），配合 `Credit` / `TokenGateway` 完成充值 → 扣费全流程。

`Blue Book 属性`

| 编号 | 名称 | 类型 | 说明 |
|------|------|------|------|
| 1 | `logical_name` | octet-string(6) | OBIS 代码，只读 |
| 2 | `total_amount_paid` | int32 | 累计已付金额（有符号） |
| 3 | `charge_type` | enum | 计费类型（`ChargeType`） |
| 4 | `priority` | uint8 | 优先级（决定计费项应用顺序） |
| 5 | `unit_charge_active` | structure | 当前生效的费率表（⏩ COMPLEX） |
| 6 | `unit_charge_passive` | structure | 待生效（下一周期）费率表（⏩ COMPLEX） |
| 7 | `unit_charge_activation_time` | octet-string | passive 变为 active 的时间 `(year, month, day, hour, min, sec)` |
| 8 | `period` | uint32 | 计费周期（秒） |
| 9 | `charge_configuration` | bitmask | 计费配置位掩码（`ChargeConfiguration`） |
| 10 | `last_collection_time` | octet-string | 上次计费回收时间 `(year, month, day, hour, min, sec)` |
| 11 | `last_collection_amount` | int32 | 上次计费回收金额 |
| 12 | `total_amount_remaining` | int32 | 尚欠余额（有符号） |
| 13 | `proportion` | uint16 | 比例因子（0~65535） |

`构造函数`

```python
Charge(logical_name: str, access: dict = None)
```
```python
import dlms
from dlms import AccessMode, Authentication
energy_register = dlms.ExtendedRegister("1.1.1.8.0.255")
charge = dlms.Charge("0.0.19.20.0.255", access={2: (AccessMode.READ_WRITE, Authentication.HIGH)})

# ── 属性 1: logical_name ──
print(charge.logical_name)  # "0.0.19.20.0.255"

# ── 属性 2: total_amount_paid（累计已付）──
charge.total_amount_paid = 5000

# ── 属性 3: charge_type（计费类型）──
charge.charge_type = dlms.ChargeType.CONSUMPTION_BASED_COLLECTION

# ── 属性 4: priority（优先级，越小越优先应用）──
charge.priority = 1

# ── 属性 5: unit_charge_active（当前生效费率表）──
charge.unit_charge_active = {
    "charge_per_unit_scaling": {"commodity_scale": 0, "price_scale": 2},
    "commodity": {"target": energy_register, "attribute_index": 2},
    "charge_tables": [
        {"index": b"\x01", "charge_per_unit": 100},
        {"index": b"\x02", "charge_per_unit": 200},
    ],
}

# ── 属性 6: unit_charge_passive（待生效费率表）──
charge.unit_charge_passive = {
    "charge_per_unit_scaling": {"commodity_scale": 0, "price_scale": 3},
    "commodity": {"target": None, "attribute_index": 0},
    "charge_tables": [
        {"index": b"\x03", "charge_per_unit": 300},
    ],
}

# ── 属性 7: unit_charge_activation_time（passive 激活时间）──
charge.unit_charge_activation_time = (2024, 7, 1, 0, 0, 0)

# ── 属性 8: period（计费周期，秒）──
charge.period = 3600

# ── 属性 9: charge_configuration（配置位掩码，可组合）──
charge.charge_configuration = dlms.ChargeConfiguration.CONTINUOUS_COLLECTION

# ── 属性 10: last_collection_time（上次回收时间）──
charge.last_collection_time = (2024, 6, 30, 23, 59, 59)

# ── 属性 11: last_collection_amount（上次回收金额）──
charge.last_collection_amount = 350

# ── 属性 12: total_amount_remaining（尚欠余额）──
charge.total_amount_remaining = 1200

# ── 属性 13: proportion（比例因子）──
charge.proportion = 100

# ── 方法处理：update_unit_charge(1) / activate(2) / collect(3) ──
def on_charge_action(self, event):
    if event.index == 1:   # update_unit_charge：passive 复制为 active
        self.unit_charge_active = self.unit_charge_passive
        print("[Charge] update_unit_charge")
    elif event.index == 2: # activate：立即激活 passive
        self.unit_charge_active = self.unit_charge_passive
        print("[Charge] activate")
    elif event.index == 3: # collect：执行一次计费回收
        # ...从 commodity.target 读取用量并按费率表计算...
        # self.total_amount_remaining += collected
        # self.last_collection_amount = collected
        print("[Charge] collect")
    elif event.index == 4: # update_last_collection_time
        self.last_collection_time = (2024, 7, 1, 0, 0, 0)
        print("[Charge] update_last_collection_time")
    elif event.index == 5: # update_total_amount_remaining
        # self.total_amount_remaining = ...
        print("[Charge] update_total_amount_remaining")
    elif event.index == 6: # set_total_amount_paid
        self.total_amount_paid = 0
        print("[Charge] set_total_amount_paid")
    return True

charge.on_before_action = on_charge_action

```

`方法（均需通过 `on_before_action` 实现）`
| 方法 | COSEM 方法 | 说明 |
|------|-----------|------|
| `update_unit_charge` | 1 | 将 passive 费率表复制为 active |
| `activate` | 2 | 立即激活 passive 费率表 |
| `collect` | 3 | 执行一次计费回收周期 |
| `update_last_collection_time` | 4 | 更新上次回收时间 |
| `update_total_amount_remaining` | 5 | 重新计算剩余欠款 |
| `set_total_amount_paid` | 6 | 重置已付金额计数 |

> **重要**：全部方法**均**经由 `on_before_action` 派发，需自行实现处理逻辑（C 层在 events.c 中统一置 `e->handled=1`）。

枚举：`ChargeType`

计费回收方式（对应 `DLMS_CHARGE_TYPE_*`），通过 `dlms.ChargeType` 访问：

| 常量 | 值 | 说明 |
|------|----|------|
| `ChargeType.CONSUMPTION_BASED_COLLECTION` | `0` | 按用量计费 |
| `ChargeType.TIME_BASED_COLLECTION` | `1` | 按时间计费 |
| `ChargeType.PAYMENT_EVENT_BASED_COLLECTION` | `2` | 按缴费事件计费 |

枚举：`ChargeConfiguration`

计费配置位掩码（对应 `DLMS_CHARGE_CONFIGURATION_*`），通过 `dlms.ChargeConfiguration` 访问，可多值按位或：

| 常量 | 值 | 说明 |
|------|----|------|
| `ChargeConfiguration.NONE` | `0x00` | 无配置 |
| `ChargeConfiguration.PERCENTAGE_BASED_COLLECTION` | `0x01` | 按百分比计费 |
| `ChargeConfiguration.CONTINUOUS_COLLECTION` | `0x02` | 连续计费 |

`unit_charge_active` / `unit_charge_passive` 字典结构

两个费率表属性使用相同的 dict 结构：

```python
{
    "charge_per_unit_scaling": {"commodity_scale": int, "price_scale": int},
    "commodity":               {"target": dlms_obj_or_none, "attribute_index": int},
    "charge_tables":           [{"index": bytes, "charge_per_unit": int}, ...]
}
```

| 键 | 类型 | 说明 |
|----|------|------|
| `charge_per_unit_scaling` | `dict` | 计量刻度：`commodity_scale`（商品缩放，int8）、`price_scale`（价格缩放，int8） |
| `commodity` | `dict` | 关联的商品对象：`target`（DLMS 对象引用，如能量寄存器，可 `None`）、`attribute_index`（读取的属性编号） |
| `charge_tables` | `list[dict]` | 费率表条目：`index`（费率标识 bytes）、`charge_per_unit`（单位费率 int16） |



`Python 属性一览`

| 属性 | 类型 | 可写 | 说明 |
|------|------|:--:|------|
| `on_before_read` | `Callable` | √ | 读前钩子（继承自 CosemObject） |
| `on_after_read` | `Callable` | √ | 读后钩子（继承自 CosemObject） |
| `on_before_write` | `Callable` | √ | 写前钩子（继承自 CosemObject） |
| `on_after_write` | `Callable` | √ | 写后钩子（继承自 CosemObject） |
| `on_before_action` | `Callable` | √ | 动作前钩子（**所有方法的实现入口**） |
| `on_after_action` | `Callable` | √ | 动作后钩子（继承自 CosemObject） |
| `access_dict` | `dict` | √ | 实例级访问控制 |

`访问控制`

```python
import dlms
from dlms import AccessMode, Authentication

# 构造函数中指定
charge = dlms.Charge("0.0.19.20.0.255",
    access={2: (AccessMode.READ_WRITE, Authentication.HIGH)})

# 或事后修改
charge.access_dict = {2: (AccessMode.READ_WRITE, Authentication.HIGH)}
```


`典型用例`

| 场景 | 示例 |
|------|------|
| 按用量计费 | `charge_type = ChargeType.CONSUMPTION_BASED_COLLECTION` |
| 按时间计费 | `charge_type = ChargeType.TIME_BASED_COLLECTION` |
| 阶梯费率 | `charge_tables` 配置多档 `index` / `charge_per_unit` |
| 周期回收 | `period = 3600`，`collect`（方法 3） |

---
#### `Account`

预付费（prepayment）计量中的**账户**对象，典型 OBIS 代码 `0.0.19.0.0.255`。作为预付费业务的顶层控制器：维护付费模式与账户状态，关联其下的 `Credit`（信用）与 `Charge`（计费）对象，聚合可用信用（`available_credit`）与债务（`aggregated_debt`），并通过 `credit_charge_configurations` / `token_gateway_configurations` 配置「信用 → 计费」「信用 → 令牌」的映射关系。一个 `Account` 可同时挂接多个 `Credit` / `Charge`，配合 `TokenGateway` 完成充值 → 扣费 → 结算全流程。

`Blue Book 属性`

| 编号 | Python 名称 | 类型 | 说明 |
|------|-------------|------|------|
| 1 | `logical_name` | octet-string(6) | OBIS 代码，只读 |
| 2 | `payment_mode` | `AccountPaymentMode` | 支付模式（CREDIT / PREPAYMENT） |
| 3 | `account_status` | `AccountStatus` | 账户状态 |
| 4 | `current_credit_in_use` | `uint8` | 当前使用的信用索引 |
| 5 | `current_credit_status` | `AccountCreditStatus` | 当前信用状态（位掩码） |
| 6 | `available_credit` | `int32` | 可用信用总额 |
| 7 | `amount_to_clear` | `int32` | 需清偿金额 |
| 8 | `clearance_threshold` | `int32` | 清偿阈值 |
| 9 | `aggregated_debt` | `int32` | 累计债务 |
| 10 | `credit_references` | `list[str]` | 关联的 Credit 对象的 OBIS 列表 |
| 11 | `charge_references` | `list[str]` | 关联的 Charge 对象的 OBIS 列表 |
| 12 | `credit_charge_configurations` | `list[dict]` | Credit↔Charge 映射配置 |
| 13 | `token_gateway_configurations` | `list[dict]` | TokenGateway↔Credit 映射配置 |
| 14 | `account_activation_time` | `tuple(6)` | 账户激活时间 |
| 15 | `account_closure_time` | `tuple(6)` | 账户关闭时间 |
| 16 | `currency` | `dict` | 货币信息 `{name, scale, unit}` |
| 17 | `low_credit_threshold` | `int32` | 低信用阈值 |
| 18 | `next_credit_available_threshold` | `int32` | 下一信用可用阈值 |
| 19 | `max_provision` | `uint16` | 最大预授权 |
| 20 | `max_provision_period` | `int32` | 最大预授权周期 |

```python
import dlms
from dlms import AccessMode, Authentication

account = dlms.Account("0.0.19.0.0.255")

# ── 属性 1: logical_name ──
print(account.logical_name)  # "0.0.19.0.0.255"

# ── 属性 2: payment_mode（付费模式）──
account.payment_mode = dlms.AccountPaymentMode.PREPAYMENT

# ── 属性 3: account_status（账户状态）──
account.account_status = dlms.AccountStatus.ACTIVE

# ── 属性 4: current_credit_in_use（当前使用的 Credit 索引）──
account.current_credit_in_use = 0

# ── 属性 5: current_credit_status（信用状态位掩码）──
account.current_credit_status = dlms.AccountCreditStatus.IN_CREDIT

# ── 属性 6: available_credit（可用信用）──
account.available_credit = 7500

# ── 属性 7: amount_to_clear（需清偿债务）──
account.amount_to_clear = 0

# ── 属性 8: clearance_threshold（清偿阈值）──
account.clearance_threshold = 100

# ── 属性 9: aggregated_debt（聚合债务）──
account.aggregated_debt = 0

# ── 属性 10: credit_references（关联 Credit 的 OBIS 列表）──
account.credit_references = ["0.0.19.10.0.255"]

# ── 属性 11: charge_references（关联 Charge 的 OBIS 列表）──
account.charge_references = ["0.0.19.20.0.255"]

# ── 属性 12: credit_charge_configurations（信用-计费关联配置）──
account.credit_charge_configurations = [
    {
        "credit_reference": "0.0.19.10.0.255",
        "charge_reference": "0.0.19.20.0.255",
        "collection_configuration": 1,
    },
]

# ── 属性 13: token_gateway_configurations（令牌网关配置）──
account.token_gateway_configurations = [
    {"credit_reference": "0.0.19.10.0.255", "token_proportion": 100},
]

# ── 属性 14: account_activation_time（账户激活时间）──
account.account_activation_time = (2024, 1, 1, 0, 0, 0)

# ── 属性 15: account_closure_time（账户关闭时间）──
account.account_closure_time = (2030, 12, 31, 23, 59, 59)

# ── 属性 16: currency（货币）──
account.currency = {"name": "EUR", "scale": -2, "unit": 8}

# ── 属性 17: low_credit_threshold（低信用告警阈值）──
account.low_credit_threshold = 500

# ── 属性 18: next_credit_available_threshold（下一信用可用阈值）──
account.next_credit_available_threshold = 1000

# ── 属性 19: max_provision（最大预充额度）──
account.max_provision = 2000

# ── 属性 20: max_provision_period（最大预充周期，秒）──
account.max_provision_period = 86400

# ── 方法处理：方法 1~18 全部派发至 on_before_action，C 层无派发 ──
# 方法编号与语义由业务逻辑约定（常见语义：activate / deactivate /
# update_credit / clear_debt / set_payment_mode 等），以下仅为示例。
def on_account_action(self, event):
    if event.index == 1:   # 例如 set_payment_mode：切换付费模式
        self.payment_mode = dlms.AccountPaymentMode.PREPAYMENT
        print("[Account] set_payment_mode -> PREPAYMENT")
    elif event.index == 2: # 例如 update_credit：从关联 Credit 对象同步可用信用
        self.available_credit = credit_obj.current_credit_amount
        print("[Account] update_credit available_credit={}".format(self.available_credit))
    elif event.index == 3: # 例如 clear_debt：清偿债务
        self.aggregated_debt = 0
        self.amount_to_clear = 0
        print("[Account] clear_debt")
    elif event.index == 4: # 例如 activate / deactivate：激活 / 关闭账户
        self.account_status = dlms.AccountStatus.ACTIVE
        print("[Account] activate")
    else:
        # 其余方法：保持 available_credit 与关联 Credit 同步
        self.available_credit = max(0, credit_obj.current_credit_amount)
        print("[Account] Action method={} available_credit={}".format(
              event.index, self.available_credit))
    return True

account.on_before_action = on_account_action

```

`方法`

支持 **18 个动作方法**，编号 1–18。应用须在 `on_before_action` 中实现全部 18 个方法。

| 编号 | 说明 |
|------|------|
| 1–18 | 由应用自行定义（如激活、充值、冻结、结算等） |

`构造函数`

```python
Account(logical_name: str, access: dict = None)
```

| 参数 | 类型 | 默认值 | 说明 |
|------|------|--------|------|
| `logical_name` | `str` | 必传 | OBIS 代码字符串，如 `"0.0.19.0.0.255"` |
| `access` | `dict` | `None` | 实例级权限 |


**`AccountStatus` — 账户状态**
| Python 名称 | 值 | 说明 |
|-------------|:--:|------|
| `AccountStatus.NEW_INACTIVE_ACCOUNT` | 1 | 新建未激活 |
| `AccountStatus.ACTIVE` | 2 | 已激活 |
| `AccountStatus.CLOSED` | 3 | 已关闭 |

**`AccountPaymentMode` — 付费模式**

| Python 名称 | 值 | 说明 |
|-------------|:--:|------|
| `AccountPaymentMode.CREDIT` | 1 | 后付费（先消费后付费） |
| `AccountPaymentMode.PREPAYMENT` | 2 | 预付费（先充值后消费） |


**`AccountCreditStatus` — 信用状态（位掩码，可位或组合）**

| Python 名称 | 值 | 说明 |
|-------------|:--:|------|
| `AccountCreditStatus.NONE` | 0x0 | 无特殊状态 |
| `AccountCreditStatus.IN_CREDIT` | 0x1 | 有可用信用 |
| `AccountCreditStatus.LOW_CREDIT` | 0x2 | 低信用 |
| `AccountCreditStatus.NEXT_CREDIT_ENABLED` | 0x4 | 下一信用已启用 |
| `AccountCreditStatus.NEXT_CREDIT_SELECTABLE` | 0x8 | 下一信用可选 |
| `AccountCreditStatus.CREDIT_REFERENCE_LIST` | 0x10 | 使用信用引用列表 |
| `AccountCreditStatus.SELECTABLE_CREDIT_IN_USE` | 0x20 | 可选信用正在使用 |
| `AccountCreditStatus.OUT_OF_CREDIT` | 0x40 | 信用耗尽 |
| `AccountCreditStatus.RESERVED` | 0x80 | 保留 |

取值见上方 `AccountCreditStatus` 枚举表（位掩码，可位或组合）。

**`Currency` — 货币单位**

| Python 名称 | 值 | 说明 |
|-------------|:--:|------|
| `Currency.TIME` | 0 | 时间单位 |
| `Currency.CONSUMPTION` | 1 | 消费量（用量）单位 |
| `Currency.MONETARY` | 2 | 货币单位 |


`Python 属性一览`

| 属性 | 类型 | 可写 | 说明 |
|------|------|:--:|------|
| `logical_name` | `bytes` | × | OBIS 代码（6 字节） |
| `payment_mode` | `AccountPaymentMode` | √ | 支付模式 |
| `account_status` | `AccountStatus` | √ | 账户状态 |
| `current_credit_in_use` | `int` | √ | 当前使用信用索引 |
| `current_credit_status` | `AccountCreditStatus` | √ | 当前信用状态位掩码 |
| `available_credit` | `int` | √ | 可用信用总额 |
| `amount_to_clear` | `int` | √ | 需清偿金额 |
| `clearance_threshold` | `int` | √ | 清偿阈值 |
| `aggregated_debt` | `int` | √ | 累计债务 |
| `credit_references` | `list[str]` | √ | Credit OBIS 引用列表 |
| `charge_references` | `list[str]` | √ | Charge OBIS 引用列表 |
| `credit_charge_configurations` | `list[dict]` | √ | Credit→Charge 映射列表 |
| `token_gateway_configurations` | `list[dict]` | √ | TokenGateway→Credit 映射列表 |
| `account_activation_time` | `tuple(6)` | √ | 激活时间 |
| `account_closure_time` | `tuple(6)` | √ | 关闭时间 |
| `currency` | `dict` | √ | 货币信息 `{name, scale, unit}` |
| `low_credit_threshold` | `int` | √ | 低信用阈值 |
| `next_credit_available_threshold` | `int` | √ | 下一信用可用阈值 |
| `max_provision` | `int` | √ | 最大预授权 |
| `max_provision_period` | `int` | √ | 最大预授权周期 |
| `on_before_action` | `Callable` | √ | 动作前钩子 |

`访问控制`

```python
import dlms
from dlms import AccessMode, Authentication


account = dlms.Account("0.0.19.0.0.255",access={6: (dlms.AccessMode.READ_WRITE, dlms.Authentication.HIGH)}

# 或事后修改
account.access_dict = {6: (dlms.AccessMode.READ_WRITE, dlms.Authentication.HIGH)}
```
---

### 其他辅助类

---

#### `StatusMapping`

**状态映射**对象，典型 OBIS 代码 `0.0.96.5.4.255`。将状态字（`status_word`）中的各个位映射到关联 COSEM 对象的条目（`mapping_table`），用于把设备运行状态（如计量状态、报警位）以位图形式呈现给客户端。属性 2 为动态值（随设备状态变化），属性 3 为静态配置（由应用在初始化时设定）。

`Blue Book 属性`

| 编号 | Python 名称 | 类型 | 说明 |
|------|-------------|------|------|
| 1 | `logical_name` | octet-string(6) | OBIS 代码，只读 |
| 2 | `status_word` | `tuple(2)` | `(dlms_type, value)` — 类型标签选择编码格式，`value` 为 `int` 或 `bytes` |
| 3 | `mapping_table` | `tuple(2)` | `(ref_table_id, mapping)` — `mapping` 为单个起始条目整数或每比特条目索引列表 |

> **注意**：客户端 SET 请求会拒绝属性 2 和 3 的写入。需直接从 Python 更新值。

`status_word` 支持的类型标签

| 标签值 | 含义 |
|:------:|------|
| `4` | BIT_STRING（位串） |
| `6` | UINT32（32 位无符号整数） |
| `9` | OCTET_STRING（八位位组串） |
| `10` | STRING（字符串） |
| `12` | STRING_UTF8（UTF-8 字符串） |
| `17` | UINT8（8 位无符号整数） |
| `18` | UINT16（16 位无符号整数 — **默认**） |
| `21` | UINT64（64 位无符号整数） |

`mapping_table` 格式

- **长-无符号选择**（`mapping` 为 `int`）：表示引用表中的起始条目索引
- **数组选择**（`mapping` 为 `list[int]`）：显式指定每个比特位对应的条目索引列表

`构造函数`

```python
StatusMapping(logical_name: str, access: dict = None)
```

| 参数 | 类型 | 默认值 | 说明 |
|------|------|--------|------|
| `logical_name` | `str` | 必传 | OBIS 代码字符串 |
| `access` | `dict` | `None` | 实例级权限 |

```python
import dlms

sm = dlms.StatusMapping("0.0.96.5.4.255",access={6: (dlms.AccessMode.READ_WRITE, dlms.Authentication.HIGH)})

# ── 属性 1: logical_name ──
print(sm.logical_name)  # "0.0.96.5.4.255"

# ── 属性 2: status_word（状态字，UINT16，全部位清零）──
sm.status_word = (18, 0x0000)

# ── 属性 3: mapping_table（映射表，8 位状态字逐位映射到条目 0~7）──
sm.mapping_table = (0, [0, 1, 2, 3, 4, 5, 6, 7])

# ── 运行时更新状态字（模拟某状态位置位）──
sm.status_word = (18, 0x0002)   # bit1 置位

# ── 方法处理：动作派发至 on_before_action ──
def on_sm_action(self, event):
    print("[StatusMapping] Action method={}".format(event.index))
    return True

sm.on_before_action = on_sm_action

```


`Python 属性一览`

| 属性 | 类型 | 可写 | 说明 |
|------|------|:--:|------|
| `on_before_read` | `Callable` | √ | 读前钩子（继承 CosemObject） |
| `on_after_read` | `Callable` | √ | 读后钩子 |
| `on_before_write` | `Callable` | √ | 写前钩子 |
| `on_after_write` | `Callable` | √ | 写后钩子 |
| `on_before_action` | `Callable` | √ | 动作前钩子 |
| `on_after_action` | `Callable` | √ | 动作后钩子 |
| `access_dict` | `dict` | √ | 实例级访问控制 |

`访问控制`

```python
import dlms
from dlms import AccessMode, Authentication


sm = dlms.StatusMapping("0.0.96.5.4.255",access={6: (dlms.AccessMode.READ_WRITE, dlms.Authentication.HIGH)})

# 或事后修改
sm.access_dict = {6: (dlms.AccessMode.READ_WRITE, dlms.Authentication.HIGH)}
```

---

#### `Arbitrator`

**仲裁器**对象，典型 OBIS 代码 `0.0.96.5.5.255`。解决多个参与者（actor）对同一组动作（action）的**并发请求冲突**：每个参与者拥有权限位集（`permissions_table`）与权重（`weightings_table`），仲裁器根据参与者最近请求（`most_recent_requests_table`）与权重选出获胜动作，结果记录在 `last_outcome`。常用于多主站/多控制源场景（如本地按键与远程指令争用同一执行器）。


`Blue Book 属性`

| 编号 | Python 名称 | 类型 | 说明 |
|------|-------------|------|------|
| 1 | `logical_name` | octet-string(6) | OBIS 代码，只读 |
| 2 | `actions` | `list[tuple]` | `(ScriptTable_or_None, selector)` 列表 |
| 3 | `permissions_table` | `list[bytes]` | 权限位集，每参与方一行 |
| 4 | `weightings_table` | `list[list[int]]` | 权重表，每参与方×每动作的 uint16 权重 |
| 5 | `most_recent_requests_table` | `list[bytes]` | 最近请求位集，每参与方一行 |
| 6 | `last_outcome` | `uint8` | 上次仲裁结果（0–255） |

`方法（需通过 `on_before_action` 实现）`

| 编号 | 名称 | 说明 |
|------|------|------|
| 1 | `push` | 提交请求 bytes（由应用实现仲裁逻辑） |

`构造函数`

```python
Arbitrator(logical_name: str, access: dict = None)
```

| 参数 | 类型 | 默认值 | 说明 |
|------|------|--------|------|
| `logical_name` | `str` | 必传 | OBIS 代码字符串 |
| `access` | `dict` | `None` | 实例级权限 |

`基本用法`

```python
import dlms

arb = dlms.Arbitrator("0.0.96.5.5.255", access={6: (dlms.AccessMode.READ_WRITE, dlms.Authentication.HIGH)})

# ── 属性 1: logical_name ──
print(arb.logical_name)  # "0.0.96.5.5.255"

# ── 属性 2: actions（动作列表，关联脚本表）──
# 先创建脚本表
disconnect_ctl = dlms.DisconnectControl("0.0.96.3.10.255")

action_close = dlms.ScriptAction(
    type=dlms.ScriptAction.Execute,
    target=disconnect_ctl,
    method=1,           # remote_disconnect
    parameter=0,
)
action_open = dlms.ScriptAction(
    type=dlms.ScriptAction.Execute,
    target=disconnect_ctl,
    method=2,           # remote_reconnect
    parameter=0,
)
script_table = dlms.ScriptTable("0.0.10.0.106.255")
script_table.add_script(id=1, actions=action_close)   # selector=1：关闭负载
script_table.add_script(id=3, actions=action_open)    # selector=3：打开负载

arb.actions = [
    (script_table, 1),   # 动作 0：执行脚本表 selector=1
    (None, 0),           # 动作 1：无脚本
    (script_table, 3),   # 动作 2：执行脚本表 selector=3
]

# ── 属性 3: permissions_table（权限位集：2 个参与者，各 4 位权限）──
arb.permissions_table = [b'\xF0', b'\x0F']

# ── 属性 4: weightings_table（权重表：2 参与者 × 3 动作）──
arb.weightings_table = [[10, 20, 30], [5, 15, 25]]

# ── 属性 5: most_recent_requests_table（最近请求位集）──
arb.most_recent_requests_table = [b'\x00', b'\x00']

# ── 属性 6: last_outcome（上次仲裁结果）──
arb.last_outcome = 0

# ── 方法处理：push（方法 1）──
def on_arb_action(self, event):
    if event.index == 1:   # push：接收参与者请求字节并执行仲裁
        # 业务逻辑：
        #   1. 解析请求字节，更新 most_recent_requests_table
        #   2. 结合 permissions_table 过滤无权限请求
        #   3. 按 weightings_table 加权比较，选出获胜动作
        #   4. self.last_outcome = winning_action_id
        #   5. 执行 self.actions[winning_action_id] 对应脚本
        print("[Arbitrator] push() invoked")
    return True

arb.on_before_action = on_arb_action
```

`Python 属性一览`

| 属性 | 类型 | 可写 | 说明 |
| `on_before_read` | `Callable` | √ | 读前钩子（继承自 CosemObject） |
| `on_after_read` | `Callable` | √ | 读后钩子（继承自 CosemObject） |
| `on_before_write` | `Callable` | √ | 写前钩子（继承自 CosemObject） |
| `on_after_write` | `Callable` | √ | 写后钩子（继承自 CosemObject） |
| `on_before_action` | `Callable` | √ | 动作前钩子（**方法 1 push 的实现入口**） |
| `on_after_action` | `Callable` | √ | 动作后钩子（继承自 CosemObject） |
| `access_dict` | `dict` | √ | 实例级访问控制 |

```python
import dlms
from dlms import AccessMode, Authentication


arb = dlms.Arbitrator("0.0.96.5.5.255", access={6: (dlms.AccessMode.READ_WRITE, dlms.Authentication.HIGH)})

# 或事后修改
arb.access_dict = {6: (dlms.AccessMode.READ_WRITE, dlms.Authentication.HIGH)}
```


---

#### `DataProtection`

**数据保护**对象，典型 OBIS 代码 `0.0.29.0.0.255`。为 DLMS 通信数据提供**密码学保护**：通过 `required_protection` 声明请求/响应必须满足的保护级别（认证、加密、数字签名），通过 `protection_buffer` 保存加密数据，通过 `protection_object_list` 声明受保护的对象列表，通过 `protection_parameters_get` / `protection_parameters_set` 配置读写操作的保护参数（保护类型、密钥类型等）。配合 `protect()` / `unprotect()` 方法可直接对数据块进行加密/解密。

> 本类在 Gurux 中为 **stub**（`cosem_getDataProtection()` 返回 `NOT_IMPLEMENTED` 且 `assert(0)`，`gxinvoke.c` 对类 30 无动作派发）。**所有属性读取与动作派发均由 Python 回调驱动**：`protection_object_list` / `protection_parameters_get` / `protection_parameters_set` 直接以 Python 对象形式存储在 Python 侧。

`Blue Book 属性`

| 编号 | Python 名称 | 类型 | 说明 |
|------|-------------|------|------|
| 1 | `logical_name` | octet-string(6) | OBIS 代码，只读 |
| 2 | `protection_buffer` | `bytes` | 保护缓冲区（加密/认证后的数据） |
| 3 | `required_protection` | `RequiredProtection` | 所需保护类型位掩码 |
| 4 | `protection_object_list` | `list[dict]` | 受保护对象列表（Python 侧存储） |
| 5 | `protection_parameters_get` | `list[dict]` | GET 操作的保护参数（Python 侧存储） |
| 6 | `protection_parameters_set` | `list[dict]` | SET 操作的保护参数（Python 侧存储） |

`构造函数`

```python
DataProtection(logical_name: str, access: dict = None)
```

| 参数 | 类型 | 默认值 | 说明 |
|------|------|--------|------|
| `logical_name` | `str` | 必传 | OBIS 代码字符串 |
| `access` | `dict` | `None` | 实例级权限 |


`方法`

| 方法 | 说明 |
|------|------|
| `protect(plaintext_bytes)` → `bytes` | 使用当前会话密码（AES-GCM）加密数据，自动更新 `protection_buffer` |
| `unprotect(ciphertext_bytes)` → `bytes` | 解密之前由 `protect()` 产生的密文包 |
| `deinit()` | 释放 C 堆内存 |

> `protect()` / `unprotect()` 仅在启用 HIGH GMAC 加密会话时可用。无密码密钥时将抛出 `ValueError`。

`protection_object_list` 字典结构


```python
import dlms

dp = dlms.DataProtection("0.0.29.0.0.255", access={6: (dlms.AccessMode.READ_WRITE, dlms.Authentication.HIGH)})

# ── 属性 1: logical_name ──
print(dp.logical_name)  # "0.0.29.0.0.255"

# ── 属性 2: protection_buffer（保护缓冲）──
dp.protection_buffer = b'\x01\x02\x03\x04'

# ── 属性 3: protection_object_list（受保护对象列表）──
data = dlms.Data("1.0.1.8.0.255",nocopy=False,access={2: (dlms.AccessMode.READ_WRITE, dlms.Authentication.HIGH)})
dp.protection_object_list = [
    (data, 2, 0),   # (DLMS 对象, 属性编号, 数据下标)
]

# ── 属性 4: protection_parameters_get（GET 保护参数）──
dp.protection_parameters_get = [
    # (protection_type, id, originator, recipient, information, key_info)
    (dlms.ProtectionType.AUTHENTICATION, b'', b'', b'', b'',
     (dlms.DataProtectionKeyType.IDENTIFIED,
      dlms.IdentifiedKeyType.UNICAST_ENCRYPTION)),
]

# ── 属性 5: protection_parameters_set（SET 保护参数）──
dp.protection_parameters_set = []

# ── 属性 6: required_protection（必需保护位掩码）──
dp.required_protection = dlms.RequiredProtection.AUTHENTICATED_REQUEST

# ── 方法处理：动作派发至 on_before_action ──
def on_dp_action(self, event):
    print("[DataProtection] Action method={}".format(event.index))
    return True

dp.on_before_action = on_dp_action

# ── protect / unprotect（需加密会话，AES-GCM）──
# cipher = dp.protect(b'\x01\x02\x03\x04')   # 加密，同时写入 protection_buffer
# plain  = dp.unprotect(cipher)              # 解密

```


**`RequiredProtection` — 必需保护（位掩码，可位或组合）**

| Python 名称 | 值 | 说明 |
|-------------|:--:|------|
| `RequiredProtection.NONE` | 0x0 | 无需保护 |
| `RequiredProtection.AUTHENTICATED_REQUEST` | 0x4 | 请求必须认证 |
| `RequiredProtection.ENCRYPTED_REQUEST` | 0x8 | 请求必须加密 |
| `RequiredProtection.DIGITALLY_SIGNED_REQUEST` | 0x10 | 请求必须数字签名 |
| `RequiredProtection.AUTHENTICATED_RESPONSE` | 0x20 | 响应必须认证 |
| `RequiredProtection.ENCRYPTED_RESPONSE` | 0x40 | 响应必须加密 |
| `RequiredProtection.DIGITALLY_SIGNED_RESPONSE` | 0x80 | 响应必须数字签名 |

**`ProtectionType` — 保护类型**

| Python 名称 | 值 | 说明 |
|-------------|:--:|------|
| `ProtectionType.AUTHENTICATION` | 1 | 仅认证 |
| `ProtectionType.ENCRYPTION` | 2 | 仅加密 |
| `ProtectionType.AUTHENTICATION_ENCRYPTION` | 3 | 认证 + 加密 |

**`DataProtectionKeyType` — 密钥类型**

| Python 名称 | 值 | 说明 |
|-------------|:--:|------|
| `DataProtectionKeyType.IDENTIFIED` | 0 | 预共享标识密钥（配 `IdentifiedKeyType`） |
| `DataProtectionKeyType.WRAPPED` | 1 | 包装（加密传输）密钥（配 `WrappedKeyType` + 密钥字节） |
| `DataProtectionKeyType.AGREED` | 2 | 协商密钥（配参数 + 数据字节） |

**`IdentifiedKeyType` — 标识密钥子类型**

| Python 名称 | 值 | 说明 |
|-------------|:--:|------|
| `IdentifiedKeyType.UNICAST_ENCRYPTION` | 0 | 全局单播加密密钥 |
| `IdentifiedKeyType.BROADCAST_ENCRYPTION` | 1 | 全局广播加密密钥 |

**`WrappedKeyType` — 包装密钥子类型**

| Python 名称 | 值 | 说明 |
|-------------|:--:|------|
| `WrappedKeyType.MASTER_KEY` | 0 | 主密钥 |

`Python 属性一览`

| 属性 | 类型 | 可写 | 说明 |
|------|------|:--:|------|
| `on_before_read` | `Callable` | √ | 读前钩子（**需实现复杂属性读取**） |
| `on_before_action` | `Callable` | √ | 动作前钩子 |
| `on_after_read` | `Callable` | √ | 读后钩子 |
| `on_after_write` | `Callable` | √ | 写后钩子 |
| `on_before_write` | `Callable` | √ | 写前钩子 |
| `on_after_action` | `Callable` | √ | 动作后钩子 |
| `access_dict` | `dict` | √ | 实例级访问控制 |

`访问权限`

```python
import dlms
from dlms import AccessMode, Authentication

dp = dlms.DataProtection("0.0.29.0.0.255", access={6: (dlms.AccessMode.READ_WRITE, dlms.Authentication.HIGH)})

# 或事后修改
dp.access_dict = {6: (dlms.AccessMode.READ_WRITE, dlms.Authentication.HIGH)}
```

---
#### `SapAssignment`

**SAP（服务接入点）分配**对象，典型 OBIS 代码 `0.0.41.0.0.255`。维护逻辑设备列表及其 SAP 地址（`sap_id`）与逻辑设备名（LDN）的映射关系。用于多逻辑设备通信场景：客户端通过 SAP 地址区分不同的逻辑设备。

`Blue Book 属性`

| 编号 | Python 名称 | 类型 | 说明 |
|------|-------------|------|------|
| 1 | `logical_name` | octet-string(6) | OBIS 代码，只读 |
| 2 | `sap_assignment_list` | `list[tuple]` | `[(sap_id, device_name), ...]`，`device_name` 为 `bytes` |


`构造函数`

```python
SapAssignment(logical_name: str, sap_assignment_list: list = None)
```
```python
import dlms

sap = dlms.SapAssignment(
    logical_name='0.0.41.0.0.255',
    sap_assignment_list=[(1, b'GRX0000000012345')],
)

# ── 属性 1: logical_name ──
print(sap.logical_name)  # "0.0.41.0.0.255"

# ── 属性 2: sap_assignment_list（SAP 分配列表）──
print(sap.sap_assignment_list)   # [(1, b'GRX0000000012345')]
sap.sap_assignment_list = [
    (1, b'GRX0000000012345'),
    (2, "GRX0000000012346"),
]

# ── 方法：connect_logical_device（新增/更新 SAP 分配，本地调用）──
sap.connect_logical_device(3, b'GRX0000000012347')


```

| 参数 | 类型 | 默认值 | 说明 |
|------|------|--------|------|
| `logical_name` | `str` | 必传 | OBIS 代码字符串，如 `"0.0.41.0.0.255"` |
| `sap_assignment_list` | `list` | `None` | 初始 SAP 分配列表，格式 `[(sap_id, device_name), ...]` |

`方法`

| 方法 | 签名 | 说明 |
|------|------|------|
| `connect_logical_device` | `(sap_id, device_name)` | 添加或更新单个 SAP 条目。若 `sap_id` 已存在则更新设备名称，否则新增 |
| `deinit` | `()` | 释放 C 堆内存 |


`Python 属性一览`

| 属性 | 类型 | 可写 | 说明 |
|------|------|:--:|------|
| `on_before_read` | `Callable` | √ | 读前钩子（继承 CosemObject） |
| `on_after_read` | `Callable` | √ | 读后钩子 |
| `on_before_write` | `Callable` | √ | 写前钩子 |
| `on_after_write` | `Callable` | √ | 写后钩子 |
| `on_before_action` | `Callable` | √ | 动作前钩子 |
| `on_after_action` | `Callable` | √ | 动作后钩子 |
| `access_dict` | `dict` | √ | 实例级访问控制 |

---
## 事件处理（Event Handling）

每个 COSEM 对象暴露六个回调属性，在客户端请求处理的不同阶段被调用。回调是普通的 Python 函数，直接在对象上赋值即可。

---

### `DLMSEvent` — 事件上下文

`DLMSEvent` 是服务器为每个客户端请求创建的事件对象，传递给所有回调。

| 属性 | 类型 | 说明 |
|------|------|------|
| `index` | `int` | 属性编号（GET/SET）或方法编号（ACTION），1-based |
| `selector` | `int` | 选择器类型：`0`=无, `1`=范围, `2`=条目 |
| `is_action` | `bool` | `True` 为 ACTION（方法调用），`False` 为 GET/SET |
| `selector_params` | `dict` / `None` | 选择器参数（见下表） |
| `parameters` | `int` / `None` | ACTION 请求的方法参数 |

**selector_params**

| `selector` | 含义 | 字典内容 |
|:----------:|------|----------|
| `0` | 无选择器 | `None` |
| `1` | 范围选择器 | `{"from_time": int, "to_time": int}` |
| `2` | 条目选择器 | `{"from_entry": int, "to_entry": int}` |

**辅助方法**

```python
obj.idx("attr_name")      # 属性名 → 索引，如 obj.idx('buffer') → 2
obj.attr_name(index)      # 索引 → 属性名，如 obj.attr_name(2) → 'buffer'
```

---

### 六大回调

所有回调签名为 `handler(self, event)`。默认均为 `None`（禁用），赋值为可调用对象即启用。

| 回调 | 触发时机 | 用途 |
|------|----------|------|
| `on_before_read` | GET 响应序列化前 | 刷新值、返回自定义数据、拒绝访问 |
| `on_after_read` | GET 响应发送后 | 日志、审计 |
| `on_before_write` | SET 请求应用前 | 验证、拒绝写入 |
| `on_after_write` | SET 请求应用后 | 同步硬件、记录变更 |
| `on_before_action` | ACTION 分发前 | 执行方法逻辑 |
| `on_after_action` | ACTION 分发后 | 日志、审计 |

**on_before_read 返回值**

| 返回值 | 效果 |
|--------|------|
| `None` / `True` | 默认处理，序列化当前属性值 |
| `False` | 拒绝请求，客户端收到 Access Violation |
| `list` | 替代默认值，将返回的列表编码后发送给客户端（用于 ProfileGeneric） |

---

### 常见模式

**读前刷新传感器值**

```python
import dlms

energy_reg = dlms.Register("1.0.1.8.0.255", scaler=-3)

def refresh_value(self, event):
    if event.index == self.idx('value'):
        self.value = read_energy_sensor()  # 应用自定义
    return True

energy_reg.on_before_read = refresh_value
```

**写前校验**

```python
def validate_write(self, event):
    if event.index == 2:  # value
        # event 中可获取待写入值
        pass  # return False 可拒绝写入
    return True

data.on_before_write = validate_write
```

**驱动硬件（ACTION）**

```python
def relay_handler(self, event):
    if event.index == 1:      # remote_disconnect
        gpio_relay.value(0)
    elif event.index == 2:    # remote_reconnect
        gpio_relay.value(1)
    return True               # 允许内存状态同步

disconnect_ctl.on_before_action = relay_handler
```

**ProfileGeneric 从外部存储提供数据**

```python
def provide_buffer(self, event):
    if event.index != self.idx('buffer'):
        return True
    if event.selector == 1:
        t_from = event.selector_params['from_time']
        t_to   = event.selector_params['to_time']
        return load_flash_rows(t_from, t_to)  # 返回 list
    return load_flash_rows(None, None)

profile.on_before_read = provide_buffer
```

**写后审计日志**

```python
def audit_write(self, event):
    attr = self.attr_name(event.index)
    now  = utime.localtime()
    msg  = "WRITE {}.{} at {}-{:02d}-{:02d} {:02d}:{:02d}:{:02d}".format(
        self.logical_name, attr,
        now[0], now[1], now[2], now[3], now[4], now[5])
    append_audit_log(msg)

clock.on_after_write = audit_write
```

---

### 线程安全要点

- 每个连接在独立的后台线程中运行，回调在所属连接的线程中执行
- `SerialConnection` / `OpticalConnection` / `MobileConnection` 在 C 系统线程中运行
- `GenericConnection` 在调用 `connect()` 的 Python 线程中运行
- Helios RTOS 无 GIL，共享可变对象需用 `_thread.allocate_lock()` 保护
- **不要在回调中调用 `server.stop()`**，会导致死锁

## 安全模型（Security）

DLMS/COSEM 定义了分层安全模型：访问控制按属性/方法实施，认证决定客户端的信任级别，加密保护传输中的 PDU 内容。

---

### 访问控制（Access Dictionaries）

每个 COSEM 对象构造函数接受可选的 `access` 参数。字典以 `Authentication` 级别为键，内层字典以属性索引（1-based）映射到 `AccessMode` 常量。

```python
import dlms

reg = dlms.Register("1.0.1.8.0.255", access={
    dlms.Authentication.NONE: {2: dlms.AccessMode.READ},
    dlms.Authentication.HIGH: {3: dlms.AccessMode.AUTHENTICATED_WRITE},
})
```

**AccessMode 常量**

| 常量 | 值 | 含义 |
|------|:--:|------|
| `AccessMode.NONE` | 0 | 无访问权限（客户端不可见） |
| `AccessMode.READ` | 1 | 允许 GET，拒绝 SET |
| `AccessMode.WRITE` | 2 | 允许 SET，拒绝 GET |
| `AccessMode.READ_WRITE` | 3 | 允许 GET 和 SET |
| `AccessMode.AUTHENTICATED_READ` | 4 | 仅认证客户端可 GET |
| `AccessMode.AUTHENTICATED_WRITE` | 5 | 仅认证客户端可 SET |
| `AccessMode.AUTHENTICATED_READ_WRITE` | 6 | GET 和 SET 均需认证 |

**Authentication 常量**

| 常量 | 值 | 说明 |
|------|:--:|------|
| `Authentication.NONE` | 0 | 公开，无需密码 |
| `Authentication.LOW` | 1 | 明文密码认证 |
| `Authentication.HIGH` | 2 | 挑战-响应（HLS） |
| `Authentication.HIGH_MD5` | 3 | 挑战-响应 + MD5 |
| `Authentication.HIGH_SHA1` | 4 | 挑战-响应 + SHA-1 |
| `Authentication.HIGH_GMAC` | 5 | AES-GCM 认证+可选加密 |
| `Authentication.HIGH_SHA256` | 6 | 挑战-响应 + SHA-256 |
| `Authentication.HIGH_ECDSA` | 7 | ECDSA 认证 |

**类级默认值**

`dlms.set_default_access()` 可为整个类设置默认权限，避免在每个对象上重复编写。优先级：**实例 `access_dict` > 构造函数 `access` > `set_default_access` > 默认（无权限）**。

```python
# 所有 Register 对象默认公开可读
dlms.set_default_access(dlms.Register, {
    dlms.Authentication.NONE: {2: dlms.AccessMode.READ},
})

# 个别对象可覆盖
sensitive_reg = dlms.Register("1.0.1.8.1.255")
sensitive_reg.access_dict = {
    dlms.Authentication.HIGH: {2: dlms.AccessMode.AUTHENTICATED_READ_WRITE},
}
```

---

### 认证机制

`AssociationLogicalName.auth_mechanism` 选择该关联所需的认证级别。

| 值 | 说明 |
|------|------|
| `'None'` | 公开关联，无密码。适用于初始化阶段的逻辑设备，配合 `access_dict` 限制敏感属性 |
| `'Low'` | 明文密码认证。客户端在 AA-open 握手中提交明文密码，服务器与 `secret` 比对。仅适合光口等物理安全接口 |
| `'High'` | 挑战-响应（HLS）。双方交换随机挑战，用共享密钥哈希后返回摘要。需要 `SecuritySetup` 且 `security_policy = SecurityPolicy.NOTHING` |
| `'HighGMac'` | AES-GCM 认证（可选加密通信）。需要配置 `SecuritySetup` 的系统标题、GUEK 和 GAK。所有 APDU 受完整性保护 |

---

### SecuritySetup 配置

`dlms.SecuritySetup` 是接口类 64（OBIS `0.0.43.0.X.255`），每个安全上下文创建一个对象。

**关键属性**

| 属性 | 类型 | 说明 |
|------|------|------|
| `security_policy` | `int` | `SecurityPolicy` 值（见下表） |
| `security_suite` | `int` | 密码套件：`0`=AES-128 GCM, `1`=ECDH/AES, `2`=ECDH/AES 变体 |
| `server_system_title` | `bytes` | 服务器 8 字节系统标题（标识符 3 字节 + 序列号 5 字节） |
| `client_system_title` | `bytes` | 客户端 8 字节系统标题 |
| `guek` | `bytes` | Global Unicast Encryption Key（16 或 32 字节） |
| `gak` | `bytes` | Global Authentication Key（16 或 32 字节） |
| `min_invocation_counter` | `int` | 最小调用计数器，防重放攻击 |

**SecurityPolicy 值**

| 常量 | 值 | 含义 |
|------|:--:|------|
| `SecurityPolicy.NOTHING` | 0 | 无密码层保护（用于 High HLS） |
| `SecurityPolicy.AUTHENTICATED` | 1 | 认证所有 APDU |
| `SecurityPolicy.ENCRYPTED` | 2 | 加密所有 APDU |
| `SecurityPolicy.AUTHENTICATED_ENCRYPTED` | 3 | 认证+加密所有 APDU |

**最小 HighGMac 配置**

```python
import dlms

sec = dlms.SecuritySetup("0.0.43.0.2.255")
sec.security_policy = dlms.SecurityPolicy.AUTHENTICATED_ENCRYPTED
sec.security_suite = 0
sec.server_system_title = b'GRX12345'
sec.client_system_title = b'GRX00001'
sec.guek = b'\x00\x01\x02\x03\x04\x05\x06\x07\x08\x09\x0A\x0B\x0C\x0D\x0E\x0F'
sec.gak  = b'\xD0\xD1\xD2\xD3\xD4\xD5\xD6\xD7\xD8\xD9\xDA\xDB\xDC\xDD\xDE\xDF'
```

**调用计数器持久化**

GCM 调用计数器随每个加密帧递增。重启后必须恢复，否则客户端会拒绝重放帧。推荐方案：使用 `nocopy=True` 的 `Data` 对象引用 `SecuritySetup` 属性 6。

```python
import dlms

# nocopy=True 表示 .value 持有对 (sec, 6) 的实时引用
inv_ctr = dlms.Data("0.0.43.1.2.255", nocopy=True)
inv_ctr.value = (sec, 6)  # 指向 SecuritySetup 属性 6（调用计数器）
inv_ctr.access_dict = {dlms.Authentication.HIGH_GMAC: {2: dlms.AccessMode.READ}}
```

启动时，反序列化后将 `sec.min_invocation_counter` 设为恢复值加上安全余量（如 +100）。

---

### 密码处理

- `AssociationLogicalName.secret` 和 `AssociationShortName.secret` 接受 `bytes` 值
- 生产环境建议从安全存储读取密码和密钥，而非硬编码在固件中
- 切勿在日志或调试输出中打印 `secret` 或密钥值
- 每台设备的密钥应从根密钥和设备序列号派生，而非全设备相同

**KEK（Key Encryption Key）**

当管理客户端可能使用 Global-Key-Transfer 服务时，需通过 `dlms.set_kek()` 设置 16 字节 KEK。KEK 必须保存在安全存储中。

```python
import dlms
import SecureData

KEK_INDEX = 1
buf = bytearray(16)
length = SecureData.Read(KEK_INDEX, buf, 16)
if length != 16:
    raise RuntimeError("KEK not provisioned (SecureData index {})".format(KEK_INDEX))
dlms.set_kek(bytes(buf))
```

首次写入 KEK：

```python
import SecureData

kek = b'\x00\x01\x02\x03\x04\x05\x06\x07\x08\x09\x0A\x0B\x0C\x0D\x0E\x0F'
SecureData.Store(KEK_INDEX, kek, 16)
```

---

### AssociationLogicalName 配置

接口类 15（OBIS `0.0.40.0.X.255`），每个客户端关联创建一个对象。

**关键属性**

| 属性 | 类型 | 说明 |
|------|------|------|
| `auth_mechanism` | `str` | `'None'` / `'Low'` / `'High'` / `'HighGMac'` |
| `secret` | `bytes` / `None` | Low 或 High 认证的共享密码；`None` 用于 None 和 HighGMac |
| `objects` | `list` | 该关联可访问的 COSEM 对象列表 |
| `clientSAP` | `int` | 客户端 SAP 号（管理用 SAP 1，公开用 SAP 16） |
| `security_setup` | `SecuritySetup` / `None` | High 和 HighGMac 必需；None 和 Low 为 `None` |
| `context` | `DLMSContext` / `None` | 可选，控制协商的 PDU 大小和一致性块 |

**DLMSContext**

| 属性 | 说明 |
|------|------|
| `conformance` | `Conformance` 常量位掩码（默认协商） |
| `maxReceivePduSize` | 最大接收 PDU 大小（字节；0=使用连接默认） |
| `maxSendPduSize` | 最大发送 PDU 大小（字节；0=使用连接默认） |
| `dlmsVersionNumber` | DLMS 版本（6=DLMS/COSEM Ed. 7+） |

**双关联示例**

```python
import dlms

# --- 公开只读关联 ---
assoc_pub = dlms.AssociationLogicalName("0.0.40.0.1.255")
assoc_pub.auth_mechanism = 'None'
assoc_pub.clientSAP = 16
assoc_pub.objects = [clock, energy_reg, profile]

# --- 管理关联（HighGMac）---
assoc_mgmt = dlms.AssociationLogicalName("0.0.40.0.2.255")
assoc_mgmt.auth_mechanism = 'HighGMac'
assoc_mgmt.clientSAP = 1
assoc_mgmt.objects = [clock, energy_reg, profile, inv_ctr, sec]
assoc_mgmt.security_setup = sec
assoc_mgmt.context = dlms.DLMSContext(dlmsVersionNumber=6)

# 两个关联对象都需通过 server.add_object() 注册
server.add_object(assoc_pub)
server.add_object(assoc_mgmt)
```

---

**Short Name 关联（AssociationShortName）**

接口类 12（OBIS `0.0.40.0.0.255`）。SN 关联使用 16 位短名引用属性，减少帧开销。仅支持 Low 认证。

```python
import dlms

assoc_sn = dlms.AssociationShortName("0.0.40.0.0.255")
assoc_sn.secret = b"PASSword"
assoc_sn.objects = [clock, energy_reg]

server.add_object(assoc_sn)

conn = dlms.MobileConnection(
    tcp_udp_setup=tcp_udp,
    gprs_setup=gprs,
    gsm_diag=gsm,
    recv_buffer=recv_buf,
    use_logical_name=False,  # SN 模式
)
server.add_connection(conn)
```

SN 和 LN 关联可在同一服务器上共存，互不冲突。

### 安全模式总示例

服务器
```python
import dlms
from dlms import Conformance, AccessMode, Authentication
import utime

try:
    import SecureData
except ImportError:
    SecureData = None   # 未开启模块时给提示，见 main()

# ======================= 配置区（非机密，可硬编码） =======================
UART_PORT     = 2                 # 串口连接所在 UART
HDLC_BAUD     = 9600              # HDLC 波特率
HDLC_DEV_ADDR = 0x10              # HDLC 设备地址
SERIAL_NUMBER = 12345             # 电表序列号
FLAG_ID       = "QCT"             # 厂商代码（3 字符）

# 演示便利开关：机密未预置时自动写入默认值（生产必须 False！）
AUTO_PROVISION = True

# SecureData 槽位规划（16 槽）
SLOT_KEK         = 1    # 16B 主密钥 KEK
SLOT_PWD_LOW     = 2    # LOW  密码（8B）
SLOT_PWD_HIGH    = 3    # HIGH 密码（8B）
SLOT_GMAC_SERVER = 4    # 8B  服务器系统标题
SLOT_GMAC_CLIENT = 5    # 8B  客户端系统标题
SLOT_GMAC_GUEK   = 6    # 16B 单播加密密钥
SLOT_GMAC_GAK    = 7    # 16B 认证密钥
# =====================================================================


# ======================= 1) SecureData 工具（机密处理） =======================
def _store(index, data):
    if SecureData is None:
        raise RuntimeError("SecureData 模块未启用")
    if SecureData.Store(index, data, len(data)) < 0:
        raise RuntimeError("SecureData.Store(slot=%d) failed" % index)


def _read_exact(index, expect_len, name):
    if SecureData is None:
        raise RuntimeError("SecureData 模块未启用")
    buf = bytearray(expect_len)
    n = SecureData.Read(index, buf, expect_len)
    if n != expect_len:
        raise RuntimeError("%s 未预置 (slot=%d, 读到 %d/%d)"
                           % (name, index, n, expect_len))
    return bytes(buf)


def provision_secrets():
    """产线/首次启动执行一次，把默认机密写入安全存储。
    注意：每台设备的密钥应从根密钥 + 序列号派生，不要全设备相同。"""
    _store(SLOT_KEK,         bytes([0x00,0x01,0x02,0x03,0x04,0x05,0x06,0x07,
                                    0x08,0x09,0x0A,0x0B,0x0C,0x0D,0x0E,0x0F]))
    _store(SLOT_PWD_LOW,     b"low12345")
    _store(SLOT_PWD_HIGH,    b"high12345")
    _store(SLOT_GMAC_SERVER, b"GRX12345")
    _store(SLOT_GMAC_CLIENT, b"GRX54321")
    _store(SLOT_GMAC_GUEK,   bytes([0x10,0x11,0x12,0x13,0x14,0x15,0x16,0x17,
                                    0x18,0x19,0x1A,0x1B,0x1C,0x1D,0x1E,0x1F]))
    _store(SLOT_GMAC_GAK,    bytes([0xD0,0xD1,0xD2,0xD3,0xD4,0xD5,0xD6,0xD7,
                                    0xD8,0xD9,0xDA,0xDB,0xDC,0xDD,0xDE,0xDF]))
    print("[provision] 机密已写入 SecureData（生产部署后请删除本调用）")


def load_secrets():
    """从安全存储读取全部机密并配置。绝不 print 这些值。"""
    kek = _read_exact(SLOT_KEK, 16, "KEK")
    dlms.set_kek(kek)                       # 全局 KEK，必须 server.run() 前调用

    pwd_low  = _read_exact(SLOT_PWD_LOW,  8, "LOW password")
    pwd_high = _read_exact(SLOT_PWD_HIGH, 8, "HIGH password")
    st_server = _read_exact(SLOT_GMAC_SERVER, 8, "server system title")
    st_client = _read_exact(SLOT_GMAC_CLIENT, 8, "client system title")
    guek = _read_exact(SLOT_GMAC_GUEK, 16, "GUEK")
    gak  = _read_exact(SLOT_GMAC_GAK,  16, "GAK")
    return dict(pwd_low=pwd_low, pwd_high=pwd_high,
                st_server=st_server, st_client=st_client, guek=guek, gak=gak)


# ======================= 2) 业务对象（按认证等级控制访问） =======================
FULL_CONF = (
    Conformance.BLOCK_TRANSFER_WITH_ACTION | Conformance.BLOCK_TRANSFER_WITH_SET_OR_WRITE |
    Conformance.BLOCK_TRANSFER_WITH_GET_OR_READ | Conformance.SET |
    Conformance.SELECTIVE_ACCESS | Conformance.ACTION |
    Conformance.MULTIPLE_REFERENCES | Conformance.GET
)


def build_business():
    """业务对象：energy/voltage/config/low_read/high_read/ldn。"""
    energy = dlms.Register(
        "1.0.1.8.0.255", 12345, scaler=1,
        access={Authentication.NONE: {2: AccessMode.READ, 3: AccessMode.READ},
                Authentication.HIGH: {2: AccessMode.READ_WRITE, 3: AccessMode.READ}},
    )
    voltage = dlms.Register(
        "1.0.32.7.0.255", 230, scaler=1,
        access={Authentication.NONE: {2: AccessMode.READ}},
    )
    config = dlms.Register(
        "1.0.25.1.0.255", 0, scaler=0,
        access={Authentication.NONE: {2: AccessMode.NONE},
                Authentication.HIGH: {2: AccessMode.READ_WRITE}},
    )
    low_read = dlms.Register(
        "1.0.11.1.0.255", 200, scaler=0,
        access={Authentication.NONE: {2: AccessMode.NONE},
                Authentication.LOW:  {2: AccessMode.READ},
                Authentication.HIGH: {2: AccessMode.READ}},
    )
    high_read = dlms.Register(
        "1.0.12.1.0.255", 300, scaler=0,
        access={Authentication.NONE: {2: AccessMode.NONE},
                Authentication.LOW:  {2: AccessMode.NONE},
                Authentication.HIGH: {2: AccessMode.READ}},
    )
    ldn = dlms.Data("0.0.42.0.0.255",
                    access={Authentication.NONE: {2: AccessMode.READ}})
    ldn.value = b"SN12345"
    return [energy, voltage, config, low_read, high_read, ldn]


# ======================= 3) SecuritySetup =======================
def build_security(sec):
    """创建密码认证 + GMAC 用的 SecuritySetup 对象。"""
    # HIGH 密码认证用的 SecuritySetup：仅密码，不做 GMac/加密
    sec_high = dlms.SecuritySetup("0.0.43.0.1.255")
    sec_high.security_policy = dlms.SecurityPolicy.NOTHING

    # High GMAC 认证/加密用的 SecuritySetup
    sec_gmac = dlms.SecuritySetup("0.0.43.0.2.255")
    sec_gmac.security_policy     = dlms.SecurityPolicy.AUTHENTICATED_ENCRYPTED
    sec_gmac.security_suite      = 0
    sec_gmac.server_system_title = sec["st_server"]
    sec_gmac.client_system_title = sec["st_client"]
    sec_gmac.guek                = sec["guek"]
    sec_gmac.gak                 = sec["gak"]
    return sec_high, sec_gmac


# ======================= 4) 关联对象（密码/安全挂载点） =======================
def build_assocs(sec, business, sec_high, sec_gmac):
    """None / Low / High / HighGMac 四级关联对象。"""
    def ctx():
        return dlms.DLMSContext(maxSendPduSize=128, maxReceivePduSize=128,
                                conformance=FULL_CONF)

    assoc_none = dlms.AssociationLogicalName("0.0.40.0.1.255")
    assoc_none.auth_mechanism = "None"
    assoc_none.clientSAP      = 0x10
    assoc_none.objects        = list(business)
    assoc_none.context        = ctx()

    assoc_low = dlms.AssociationLogicalName("0.0.40.0.2.255")
    assoc_low.auth_mechanism  = "Low"
    assoc_low.secret          = sec["pwd_low"]      # <- LOW 密码
    assoc_low.clientSAP       = 2
    assoc_low.objects         = list(business)
    assoc_low.security_setup  = sec_high
    assoc_low.context         = ctx()

    assoc_high = dlms.AssociationLogicalName("0.0.40.0.3.255")
    assoc_high.auth_mechanism = "High"
    assoc_high.secret         = sec["pwd_high"]     # <- HIGH 密码
    assoc_high.clientSAP      = 5
    assoc_high.objects        = list(business)
    assoc_high.security_setup = sec_high
    assoc_high.context        = ctx()

    assoc_gmac = dlms.AssociationLogicalName("0.0.40.0.4.255")
    assoc_gmac.auth_mechanism = "HighGMac"
    assoc_gmac.clientSAP      = 4
    assoc_gmac.objects        = list(business)
    assoc_gmac.security_setup = sec_gmac
    assoc_gmac.context        = ctx()

    return assoc_none, assoc_low, assoc_high, assoc_gmac


# ======================= 5) 服务器主流程 =======================
def build_server():
    """组装服务器并返回（含连接）。"""
    # 读取机密 + 设置 KEK
    try:
        sec = load_secrets()
    except RuntimeError:
        if not AUTO_PROVISION:
            raise
        print("[!] 机密未预置，演示模式自动写入默认值（生产请关闭 AUTO_PROVISION）")
        provision_secrets()
        sec = load_secrets()

    # SecuritySetup + 业务对象 + 关联对象
    business = build_business()
    sec_high, sec_gmac = build_security(sec)
    assocs = build_assocs(sec, business, sec_high, sec_gmac)

    # 连接：串口（HDLC）。光学/移动/通用连接同理换类型即可
    hdlc = dlms.IecHdlcSetup(
        "0.0.22.0.0.255",
        commSpeed=HDLC_BAUD, windowSizeRx=1, windowSizeTx=1,
        maxInfoLenTx=128, maxInfoLenRx=128, timeout=120, deviceAddr=HDLC_DEV_ADDR,
    )
    conn = dlms.SerialConnection(uart_port=UART_PORT, hdlc_setup=hdlc,
                                 use_logical_name=True)

    # 注册对象 + 连接
    server = dlms.Server(serial_number=SERIAL_NUMBER, flag_id=FLAG_ID)
    for obj in business + [sec_high, sec_gmac]:
        server.add_object(obj)
    for a in assocs:
        server.add_object(a)
    server.add_connection(conn)
    return server


def main():
    print("=" * 60)
    print("[Server] DLMS 安全服务器 Demo（密码 + GMAC + KEK）")
    print("=" * 60)
    if SecureData is None:
        print("[Error] SecureData 模块未启用（需 MICROPY_QPY_MODULE_SECUREDATA）")
        return -1

    server = build_server()
    server.run()
    print("[Server] 已启动，等待客户端（None/Low/High/HighGMac）接入...")
    try:
        while True:
            utime.sleep(1)
    except KeyboardInterrupt:
        server.stop()
        print("[Server] 已停止")
    return 0


if __name__ == "__main__":
    main()


```
客户端 
```python
import dlms
from dlms import Authentication

try:
    import SecureData
except ImportError:
    SecureData = None

# ======================= 配置区（非机密，可硬编码） =======================
UART_PORT     = 2                  # 客户端所在 UART（与服务器 UART2 对连）
HDLC_BAUD     = 9600               # 与服务器 IecHdlcSetup.commSpeed 一致
HDLC_DEV_ADDR = 0x10
SERVER_SERIAL = 12345              # 服务器序列号

# 演示便利开关：机密未预置时自动写入默认值（生产必须 False！）
AUTO_PROVISION = True

# SecureData 槽位规划（与服务器完全一致）
SLOT_KEK         = 1    # 16B 主密钥 KEK
SLOT_PWD_LOW     = 2    # LOW  密码（8B）
SLOT_PWD_HIGH    = 3    # HIGH 密码（8B）
SLOT_GMAC_SERVER = 4    # 8B  服务器系统标题
SLOT_GMAC_CLIENT = 5    # 8B  客户端系统标题
SLOT_GMAC_GUEK   = 6    # 16B 单播加密密钥
SLOT_GMAC_GAK    = 7    # 16B 认证密钥
# =====================================================================


# ======================= 1) SecureData 工具（与服务器同款） =======================
def _store(index, data):
    if SecureData is None:
        raise RuntimeError("SecureData 模块未启用")
    if SecureData.Store(index, data, len(data)) < 0:
        raise RuntimeError("SecureData.Store(slot=%d) failed" % index)


def _read_exact(index, expect_len, name):
    if SecureData is None:
        raise RuntimeError("SecureData 模块未启用")
    buf = bytearray(expect_len)
    n = SecureData.Read(index, buf, expect_len)
    if n != expect_len:
        raise RuntimeError("%s 未预置 (slot=%d, 读到 %d/%d)"
                           % (name, index, n, expect_len))
    return bytes(buf)


def provision_secrets():
    """产线/首次启动执行一次：客户端侧也写入同一套默认机密。"""
    _store(SLOT_KEK,         bytes([0x00,0x01,0x02,0x03,0x04,0x05,0x06,0x07,
                                    0x08,0x09,0x0A,0x0B,0x0C,0x0D,0x0E,0x0F]))
    _store(SLOT_PWD_LOW,     b"low12345")
    _store(SLOT_PWD_HIGH,    b"high12345")
    _store(SLOT_GMAC_SERVER, b"GRX12345")
    _store(SLOT_GMAC_CLIENT, b"GRX54321")
    _store(SLOT_GMAC_GUEK,   bytes([0x10,0x11,0x12,0x13,0x14,0x15,0x16,0x17,
                                    0x18,0x19,0x1A,0x1B,0x1C,0x1D,0x1E,0x1F]))
    _store(SLOT_GMAC_GAK,    bytes([0xD0,0xD1,0xD2,0xD3,0xD4,0xD5,0xD6,0xD7,
                                    0xD8,0xD9,0xDA,0xDB,0xDC,0xDD,0xDE,0xDF]))
    print("[provision] 客户端机密已写入 SecureData（生产部署后请删除）")


def load_secrets():
    """从安全存储读取客户端所需机密。绝不 print 这些值。"""
    return dict(
        pwd_low  = _read_exact(SLOT_PWD_LOW,  8, "LOW password"),
        pwd_high = _read_exact(SLOT_PWD_HIGH, 8, "HIGH password"),
        st_client = _read_exact(SLOT_GMAC_CLIENT, 8, "client system title"),
        guek = _read_exact(SLOT_GMAC_GUEK, 16, "GUEK"),
        gak  = _read_exact(SLOT_GMAC_GAK,  16, "GAK"),
    )


# ======================= 2) 串口连接（C 层驱动） =======================
def _build_serial_conn():
    """构造 C 层驱动的 SerialConnection（8N1 HDLC，客户端模式）。

    dlms_client_connect() 识别到 SerialConnection 后会自动：
      - 用 IecHdlcSetup.commSpeed 打开 UART（8N1）
      - 收 0x7E..0x7E HDLC 帧（按长度字段定位）
    无需 Python 开 UART / 解析帧。
    """
    hdlc = dlms.IecHdlcSetup("0.0.22.0.0.255",
                             commSpeed=HDLC_BAUD, deviceAddr=HDLC_DEV_ADDR)
    return dlms.SerialConnection(uart_port=UART_PORT, hdlc_setup=hdlc,
                                 use_logical_name=True)


# ======================= 3) 业务对象（与服务器 OBIS 一致） =======================
LN_ENERGY  = "1.0.1.8.0.255"
LN_VOLT    = "1.0.32.7.0.255"
LN_CONFIG  = "1.0.25.1.0.255"
LN_LOWREAD = "1.0.11.1.0.255"
LN_HIGHREAD = "1.0.12.1.0.255"


def _read_attr(client, ln, attr, name):
    obj = dlms.Register(ln, 0)
    try:
        client.read(obj, attr)
        print("[OK  ] {} attr{} = {}".format(name, attr, getattr(obj, "value", None)))
    except Exception as e:
        print("[FAIL] {} attr{} : {}".format(name, attr, e))


def _write_attr(client, ln, attr, value, name):
    obj = dlms.Register(ln, 0)
    obj.value = value
    try:
        client.write(obj, attr)
        print("[OK  ] {} attr{} 写入 {} 成功".format(name, attr, value))
    except Exception as e:
        print("[FAIL] {} attr{} 写入 : {}".format(name, attr, e))


# ======================= 4) HIGH 密码认证 Demo =======================
def demo_high(sec):
    print("--- HIGH 密码认证（HLS 挑战-响应） ---")
    conn = _build_serial_conn()
    try:
        client = dlms.Client(
            client_address=5,                              # 对应 assoc_high.clientSAP
            server_address=dlms.hdlc_server_address(SERVER_SERIAL),
            authentication=dlms.Authentication.HIGH,
            password=sec["pwd_high"].decode(),             # str，与 assoc_high.secret 一致
            security=0x00,                                 # 密码认证不加密
        )
        client.connect(conn)                               # C 层开 UART(8N1) + HDLC 收发
        print("[HIGH] 关联成功")
        _read_attr(client, LN_ENERGY, 2, "energy")
        _read_attr(client, LN_CONFIG, 2, "config")
        _write_attr(client, LN_CONFIG, 2, 999, "config")   # HIGH 可写
        try:
            client.disconnect()                            # C 层 deinit UART
        except Exception:
            pass
    except Exception as e:
        print("[HIGH] 失败: {}".format(e))


# ======================= 5) High GMAC 认证/加密 Demo =======================
def demo_gmac(sec):
    print("--- High GMAC 认证/加密 ---")
    conn = _build_serial_conn()
    try:
        client = dlms.Client(
            client_address=4,                              # 对应 assoc_gmac.clientSAP
            server_address=dlms.hdlc_server_address(SERVER_SERIAL),
            authentication=dlms.Authentication.HIGH_GMAC,
            system_title=sec["st_client"],                 # 8B，与服务器 client_system_title 一致
            authentication_key=sec["gak"],                 # 16B，与 sec_gmac.gak 一致
            block_cipher_key=sec["guek"],                  # 16B，与 sec_gmac.guek 一致
            security=0x30,                                 # AUTHENTICATION_ENCRYPTION
        )
        client.connect(conn)                               # C 层开 UART(8N1) + HDLC 收发
        print("[GMAC] 关联成功（GMac 认证 + 加密）")
        _read_attr(client, LN_ENERGY, 2, "energy")
        try:
            client.disconnect()                            # C 层 deinit UART
        except Exception:
            pass
    except Exception as e:
        print("[GMAC] 失败: {}".format(e))


# ======================= 6) 主流程 =======================
def main():
    print("=" * 60)
    print("[Client] DLMS 安全客户端 Demo（HIGH 密码 + High GMAC）")
    print("[Client] 服务器序列号 {}  UART{}  {} bps".format(
        SERVER_SERIAL, UART_PORT, HDLC_BAUD))
    print("=" * 60)
    if SecureData is None:
        print("[Error] SecureData 模块未启用（需 MICROPY_QPY_MODULE_SECUREDATA）")
        return -1

    try:
        sec = load_secrets()
    except RuntimeError:
        if not AUTO_PROVISION:
            raise
        print("[!] 客户端机密未预置，演示模式自动写入默认值（生产请关闭 AUTO_PROVISION）")
        provision_secrets()
        sec = load_secrets()

    demo_high(sec)
    print()
    demo_gmac(sec)
    return 0


if __name__ == "__main__":
    main()

```

## 连接（Connection）

连接表示 DLMS 客户端与服务器通信的物理或逻辑传输通道。每种连接类型封装不同的底层介质（UART、光口、蜂窝 UDP 或自定义 Python I/O），均通过 `server.add_connection(conn)` 添加到服务器。调用 `server.start()` 后，服务器为每个连接启动一个独立线程。

---


| 连接方式 | 直接必需对象 | 可选/间接对象 | 典型场景 |
|---|---|---|---|
| `SerialConnection` | `IecHdlcSetup` | 无 | UART 上跑 HDLC |
| `OpticalConnection` | `LocalPortSetup` + `IecHdlcSetup` | 无 | Mode E 协商后跑 HDLC |
| `MobileConnection` | `TcpUdpSetup` + `GprsSetup` + `GsmDiagnostic` + `recv_buffer` | `relay_tcp_setup` 可选，`IPv4Setup` 常经 `TcpUdpSetup` 间接关联 | 蜂窝 TCP/UDP |
| `GenericConnection` (`HDLC`) | `IecHdlcSetup` | 无 | 自定义承载上的 HDLC |
| `GenericConnection` (`WRAPPER`) | `TcpUdpSetup` | `IPv4Setup` 间接 | 自定义承载上的 WRAPPER |
| `GenericConnection` (`HDLC_WITH_MODE_E`) | `IecHdlcSetup` + `LocalPortSetup` | 无 | 自定义承载上的 Mode E + HDLC |

---

### SerialConnection

串口连接。内部在 C 线程中管理 UART。适用于直连场景，无需自定义传输逻辑。

服务器示例
```python
# -*- coding: utf-8 -*-
"""
DLMS 服务器端脚本（模组 A / 电表）- 统一 NONE/LOW/HIGH 认证版
==================================================================
通过 SerialConnection（HDLC over UART）对外提供 DLMS/COSEM 服务。
一个服务器同时注册三个关联对象，分别对应三种认证级别，
客户端按需选择连接

三种认证级别（客户端 SAP 各不相同，用于区分连接）:
  * NONE (0x10): 无认证，直接关联
  * LOW  (0x20): 密码认证（LOW），AARE 阶段校验 assoc.secret
  * HIGH (0x30): HLS 挑战-响应认证，secret 作为挑战密钥

对象访问控制（利用 access_control.c 的级联语义）:
  * energy / voltage / ext_energy / param / ldn:
      配置 {Authentication.NONE: {...}} —— 所有认证级别（含 LOW/HIGH）
      都能命中该配置，属性2 读写权限按配置生效。
  * config_reg (1.0.25.1.0.255):
      仅配置 {Authentication.HIGH: {2: READ_WRITE}} ——
      NONE / LOW 连接下级联匹配不到，落到 events.c 的 C 兜底（只读），
      写入被拒（READ_WRITE_DENIED）；仅 HIGH 连接可写。

接线（模组 A  <->  模组 B）:
    A.UART_TX  ----> B.UART_RX
    A.UART_RX  <---- B.UART_TX
    A.GND      ----  B.GND      （共地是必须的，否则电平无法判定）

关键点:
  * SerialConnection 的客户端侧强制使用 HDLC 帧格式，
    因此服务器端 interface_type 必须保持默认 HDLC（不要用 WRAPPER）。
  * 两端波特率（commSpeed）必须一致。
  * 客户端 client_address 必须等于对应关联对象的 clientSAP。
"""

import dlms
from dlms import Conformance
import utime

# ============================ 配置区 ============================
UART_PORT   = 2        # 模组 A 使用的 UART 口
BAUD        = 9600     # 波特率，必须与客户端一致
FLAG_ID     = "GRX"    # 厂商代码（3 字符）
SERIAL_NUM  = 12345    # 电表序列号（≤5 位，Python 模式由 set_serial_number 设置）
PASSWORD    = "Quectel"  # LOW/HIGH 认证密码（LOW 直接比对，HIGH 用作挑战密钥）

# 三种认证级别各自的客户端 SAP（必须互不相同，客户端按此选路）
SAP_NONE = 0x10    # NONE 认证关联对象
SAP_LOW  = 0x20    # LOW  认证关联对象
SAP_HIGH = 0x30    # HIGH 认证关联对象

# 服务器地址 = 序列号 % 10000 + 1000
#   Python 服务器模式：12345 -> 2345 + 1000 = 3345
SERVER_ADDR = SERIAL_NUM % 10000 + 1000   # = 3345，同时作为 IecHdlcSetup 设备地址
# ================================================================

# Python 服务器模式（CONFIG_DLMS_BUILTIN_SERVER 关闭）：events.c 生效，
# svr_isTarget 读取 SRV_SERIAL_NUMBER（由本调用设置）,
# 客户端 server_address 必须 == 该值 % 10000 + 1000。
dlms.set_serial_number(SERIAL_NUM)

# ============================ COSEM 对象 ============================
# 电能寄存器：属性2(value) 可读可写 —— 所有认证级别可写
energy = dlms.Register(
    "1.0.1.8.0.255",
    default_value=12345,
    scaler=1,
    access={
        dlms.Authentication.NONE: {
            2: dlms.AccessMode.READ_WRITE,   # value：客户端可写
            3: dlms.AccessMode.READ,         # scaler/unit
        }
    }
)

# 电压寄存器：只读 —— 所有认证级别只读
voltage = dlms.Register(
    "1.0.32.7.0.255",
    default_value=230,
    scaler=1,
    access={
        dlms.Authentication.NONE: {
            2: dlms.AccessMode.READ,
            3: dlms.AccessMode.READ,
        }
    }
)

# 高权限寄存器：仅 HIGH 认证可写 —— 演示"部分对象需要更高权限"
# 只配置 Authentication.HIGH 级，NONE/LOW 连接写会被拒（READ_WRITE_DENIED）。
config_reg = dlms.Register(
    "1.0.25.1.0.255",          # 费率/参数配置寄存器（示意）
    default_value=0,
    scaler=0,
    access={
        dlms.Authentication.HIGH: {
            2: dlms.AccessMode.READ_WRITE,   # value：仅 HIGH 可写
            3: dlms.AccessMode.READ,
        }
    }
)

# 扩展寄存器（ExtendedRegister，COSEM class 4）：value 可读写
ext_energy = dlms.ExtendedRegister(
    "1.0.1.8.1.255",
    value=500,
    scaler=0,
    unit=dlms.Unit.ACTIVE_ENERGY,
    status=0,
    access={
        dlms.Authentication.NONE: {
            2: dlms.AccessMode.READ_WRITE,   # value：客户端可写
            3: dlms.AccessMode.READ,         # scaler/unit
            4: dlms.AccessMode.READ,         # status
            5: dlms.AccessMode.READ,         # capture_time
        }
    }
)
ext_energy.capture_time = (2026, 8, 20, 12, 0, 0)

# 参数对象：可读可写
param = dlms.Data(
    "0.0.1.1.0.255",
    access={dlms.Authentication.NONE: {2: dlms.AccessMode.READ_WRITE}}
)
param.value = 42

# 逻辑设备名 LDN：只读
ldn = dlms.Data(
    "0.0.42.0.0.255",
    access={dlms.Authentication.NONE: {2: dlms.AccessMode.READ}}
)
ldn.value = b"SN12345"

# ------------------------------------------------------------------
# 读取权限分级演示对象（仅演示 attr2 的读权限）
# ------------------------------------------------------------------

# 公开只读寄存器：NONE 认证即可读（所有认证级别都能读）
public_read_reg = dlms.Register(
    "1.0.10.1.0.255",          # 公开读数寄存器（示意）
    default_value=100,
    scaler=0,
    access={
        dlms.Authentication.NONE: {
            2: dlms.AccessMode.READ,   # NONE 可读（级联后 LOW/HIGH 也可读）
        }
    }
)

# LOW 级可读寄存器：只有 LOW(及以上) 认证才可读。
# 关键：必须显式把 NONE 级配置为 AccessMode.NONE（封死），否则 NONE 连接会
# 级联不到任何配置、落到 events.c 的 C 兜底（NONE 默认 READ，反而可读）。
low_read_reg = dlms.Register(
    "1.0.11.1.0.255",          # 需 LOW 权限的读数寄存器（示意）
    default_value=200,
    scaler=0,
    access={
        dlms.Authentication.NONE: {
            2: dlms.AccessMode.NONE,   # NONE 连接：不可读
        },
        dlms.Authentication.LOW: {
            2: dlms.AccessMode.READ,   # LOW 连接：可读
        },
        dlms.Authentication.HIGH: {
            2: dlms.AccessMode.READ,   # HIGH 连接：可读（级联命中 HIGH 级）
        },
    }
)

# HIGH 级可读寄存器：只有 HIGH 认证才可读。
# NONE / LOW 级都显式配置为 AccessMode.NONE 封死。
high_read_reg = dlms.Register(
    "1.0.12.1.0.255",          # 需 HIGH 权限的读数寄存器（示意）
    default_value=300,
    scaler=0,
    access={
        dlms.Authentication.NONE: {
            2: dlms.AccessMode.NONE,   # NONE 连接：不可读
        },
        dlms.Authentication.LOW: {
            2: dlms.AccessMode.NONE,   # LOW 连接：不可读
        },
        dlms.Authentication.HIGH: {
            2: dlms.AccessMode.READ,   # HIGH 连接：可读
        },
    }
)

# ------------------------------------------------------------------
# 类型级兜底访问控制（确保写权限一定生效）
# ------------------------------------------------------------------
dlms.set_default_access(
    dlms.Register,
    {
        dlms.Authentication.NONE: {
            2: dlms.AccessMode.READ_WRITE,
            3: dlms.AccessMode.READ,
        }
    }
)
dlms.set_default_access(
    dlms.Data,
    {
        dlms.Authentication.NONE: {
            2: dlms.AccessMode.READ_WRITE,
        }
    }
)

# ------------------ HDLC 链路层配置 ------------------
hdlc = dlms.IecHdlcSetup(
    "0.0.22.0.0.255",
    commSpeed=BAUD,
    windowSizeRx=1,
    windowSizeTx=1,
    maxInfoLenTx=128,
    maxInfoLenRx=128,
    timeout=120,
    deviceAddr=SERVER_ADDR,
)

# 服务器上注册的全部业务对象（关联对象各自持有自己的对象列表视图）
all_objects = [
    energy, voltage, config_reg,
    public_read_reg, low_read_reg, high_read_reg,
    param, ldn, hdlc, ext_energy,
]

# ------------------ 关联对象 1：NONE 认证 ------------------
assoc_none = dlms.AssociationLogicalName("0.0.40.0.1.255")
assoc_none.auth_mechanism = "None"
assoc_none.clientSAP = SAP_NONE
assoc_none.objects = list(all_objects)
assoc_none.context = dlms.DLMSContext(
    maxSendPduSize=128,
    maxReceivePduSize=128,
    conformance=Conformance.BLOCK_TRANSFER_WITH_ACTION | Conformance.BLOCK_TRANSFER_WITH_SET_OR_WRITE |
                Conformance.BLOCK_TRANSFER_WITH_GET_OR_READ | Conformance.SET |
                Conformance.SELECTIVE_ACCESS | Conformance.ACTION |
                Conformance.MULTIPLE_REFERENCES | Conformance.GET,
)

# ------------------ 关联对象 2：LOW 认证 ------------------
assoc_low = dlms.AssociationLogicalName("0.0.40.0.2.255")
assoc_low.auth_mechanism = "Low"
assoc_low.secret = b"Quectel"        # 密码，必须与 LOW 客户端 password 一致
assoc_low.clientSAP = SAP_LOW
assoc_low.objects = list(all_objects)
assoc_low.context = dlms.DLMSContext(
    maxSendPduSize=128,
    maxReceivePduSize=128,
    conformance=Conformance.BLOCK_TRANSFER_WITH_ACTION | Conformance.BLOCK_TRANSFER_WITH_SET_OR_WRITE |
                Conformance.BLOCK_TRANSFER_WITH_GET_OR_READ | Conformance.SET |
                Conformance.SELECTIVE_ACCESS | Conformance.ACTION |
                Conformance.MULTIPLE_REFERENCES | Conformance.GET,
)

# ------------------ 关联对象 3：HIGH 认证 ------------------
assoc_high = dlms.AssociationLogicalName("0.0.40.0.3.255")
assoc_high.auth_mechanism = "High"
assoc_high.secret = b"Quectel"       # HLS 挑战密钥，必须与 HIGH 客户端 password 一致
assoc_high.clientSAP = SAP_HIGH
assoc_high.objects = list(all_objects)
assoc_high.context = dlms.DLMSContext(
    maxSendPduSize=128,
    maxReceivePduSize=128,
    conformance=Conformance.BLOCK_TRANSFER_WITH_ACTION | Conformance.BLOCK_TRANSFER_WITH_SET_OR_WRITE |
                Conformance.BLOCK_TRANSFER_WITH_GET_OR_READ | Conformance.SET |
                Conformance.SELECTIVE_ACCESS | Conformance.ACTION |
                Conformance.MULTIPLE_REFERENCES | Conformance.GET,
)

# ============================ 服务器 ============================
server = dlms.Server(serial_number=SERIAL_NUM, flag_id=FLAG_ID)
for obj in all_objects + [assoc_none, assoc_low, assoc_high]:
    server.add_object(obj)

# SerialConnection：C 驱动 UART，服务器端自动监听并回包
serial_conn = dlms.SerialConnection(
    uart_port=UART_PORT,
    hdlc_setup=hdlc,
    flowcontrol=0,
    interface_type=dlms.InterfaceType.HDLC,  # 必须 HDLC（与客户端一致）
    use_logical_name=True,
)

server.add_connection(serial_conn)
server.run()   # 非阻塞，连接在后台线程中监听
print("[Server] DLMS server (NONE/LOW/HIGH) running on UART{} @ {} baud".format(UART_PORT, BAUD))
print("[Server] Serial num={}, Server addr={}, password={}".format(SERIAL_NUM, SERVER_ADDR, PASSWORD))
print("[Server] SAPs: NONE=0x{:02X} LOW=0x{:02X} HIGH=0x{:02X}".format(SAP_NONE, SAP_LOW, SAP_HIGH))
print("[Server] Objects: energy(1.0.1.8.0.255) voltage(1.0.32.7.0.255) config(HIGH-w) ext(1.0.1.8.1.255) param")
print("[Server] Read levels: public(1.0.10.1.0.255,NONE+) low(1.0.11.1.0.255,LOW+) high(1.0.12.1.0.255,HIGH)")

# ============================ 主循环 ============================
# 模拟电表周期采集，更新寄存器值（客户端可读到变化的电压）
try:
    seq = 0
    while True:
        seq += 1
        v = 228 + (seq % 7)          # 230/229/... 波动，模拟真实采样
        voltage.value = v
        print("[Server] sample: voltage={} V, energy={}".format(v, energy.value))
        utime.sleep(5)
except KeyboardInterrupt:
    server.stop()
    print("[Server] stopped")

```

客户端(NONE)
```python
# -*- coding: utf-8 -*-
"""
DLMS 客户端脚本（模组 B / 集中器）- NONE 认证版
==================================================================
通过 SerialConnection（HDLC over UART）读取/写入服务器（模组 A），

本脚本使用 NONE 认证连接（client_address = 0x10，对应服务器 NONE 关联）:
  * energy / ext_energy / param: 可读可写（服务器配置了 NONE 级 READ_WRITE）
  * voltage:                      只读
  * config_reg (1.0.25.1.0.255): 可读，写入会被拒绝（仅 HIGH 可写）

接线（模组 A  <->  模组 B）:
    B.UART_RX  <---- A.UART_TX
    B.UART_TX  ----> A.UART_RX
    B.GND      ----  A.GND      （共地是必须的）

关键点:
  * client_address 必须等于对应认证级别关联对象的 clientSAP（0x10）。
  * server_address 必须等于 服务器序列号 % 10000 + 1000。
  * 波特率必须与服务器端一致（commSpeed）。
"""

import dlms
import utime

# ============================ 配置区 ============================
UART_PORT = 2        # 模组 B 使用的 UART 口
BAUD      = 9600     # 波特率，必须与服务器端一致

# 服务器端序列号 —— 必须与服务器脚本中的 SERIAL_NUM 一致！
SERVER_SERIAL = 12345

CLIENT_ADDR = 0x10   # NONE 认证关联对象的 clientSAP
# 服务器地址 = 序列号 % 10000 + 1000
SERVER_ADDR = SERVER_SERIAL % 10000 + 1000   # = 3345
AUTH        = dlms.Authentication.NONE       # NONE 认证

# 演示用对象的逻辑名（均为服务器上真实存在的对象）
LN_ENERGY = "1.0.1.8.0.255"    # 电能寄存器（Register，可写）
LN_VOLT   = "1.0.32.7.0.255"   # 电压寄存器（Register，只读）
LN_EXT    = "1.0.1.8.1.255"    # 扩展电能寄存器（ExtendedRegister，可写）
LN_PARAM  = "0.0.1.1.0.255"    # 参数 Data（可写）
LN_CONFIG = "1.0.25.1.0.255"   # 高权限寄存器（Register，仅 HIGH 认证可写）
LN_PUBLIC = "1.0.10.1.0.255"   # 公开只读寄存器（NONE+ 可读）
LN_LOWREAD = "1.0.11.1.0.255"  # LOW+ 可读寄存器（NONE 下不可读）
LN_HIGHREAD = "1.0.12.1.0.255" # HIGH 才可读寄存器（NONE/LOW 下不可读）
# ================================================================

# ------------------ HDLC 链路层配置（必须与服务器一致） ------------------
hdlc = dlms.IecHdlcSetup(
    "0.0.22.0.0.255",
    commSpeed=BAUD,
    windowSizeRx=1,
    windowSizeTx=1,
    maxInfoLenTx=128,
    maxInfoLenRx=128,
    timeout=120,
    deviceAddr=0x10,
)

serial_conn = dlms.SerialConnection(
    uart_port=UART_PORT,
    hdlc_setup=hdlc,
    flowcontrol=0,
    interface_type=dlms.InterfaceType.HDLC,  # 必须 HDLC
    use_logical_name=True,
)

client = dlms.Client(
    client_address=CLIENT_ADDR,
    server_address=SERVER_ADDR,
    authentication=AUTH,
    use_logical_name=True,
)


def read_attr(ln, attr):
    """读取单个属性，失败返回 None 并打印错误。"""
    try:
        val = client.read(ln, attr)
        print("[Read ] {} attr{} = {}".format(ln, attr, val))
        return val
    except Exception as e:
        print("[Read ] {} attr{} FAILED: {}".format(ln, attr, e))
        return None


def write_attr(ln, attr, value):
    """写入单个属性，返回是否成功。"""
    try:
        client.write(ln, attr, value)
        print("[Write] {} attr{} = {} OK".format(ln, attr, value))
        return True
    except Exception as e:
        print("[Write] {} attr{} = {} FAILED: {}".format(ln, attr, value, e))
        return False


def run_once():
    """单轮测试：读取所有对象 -> 写入 -> 回读验证。"""
    print("-" * 50)

    # 1) 读取
    volt = read_attr(LN_VOLT, 2)          # 电压寄存器值（只读）
    read_attr(LN_ENERGY, 2)               # 电能寄存器值
    read_attr(LN_EXT, 2)                  # 扩展电能寄存器值
    read_attr(LN_PARAM, 2)                # 参数

    # 2) 写电能寄存器（属性2 value）—— NONE 下可写
    new_energy = (volt or 0) + 1000
    write_attr(LN_ENERGY, 2, new_energy)

    # 3) 写扩展电能寄存器 + 参数对象 —— NONE 下可写
    write_attr(LN_EXT, 2, 888)
    write_attr(LN_PARAM, 2, 100)

    # 4) 高权限寄存器：NONE 连接下可读但不可写（期望 READ_WRITE_DENIED）
    read_attr(LN_CONFIG, 2)
    write_attr(LN_CONFIG, 2, 9999)

    # 5) 读取权限分级验证（NONE 连接）
    read_attr(LN_PUBLIC, 2)     # 期望：OK（NONE 可读）
    read_attr(LN_LOWREAD, 2)    # 期望：FAILED（仅 LOW+ 可读）
    read_attr(LN_HIGHREAD, 2)   # 期望：FAILED（仅 HIGH 可读）

    # 6) 回读验证
    read_attr(LN_ENERGY, 2)
    read_attr(LN_EXT, 2)
    read_attr(LN_PARAM, 2)


def main():
    print("[Client] Connecting to server via UART{} @ {} baud (NONE auth)...".format(UART_PORT, BAUD))
    print("[Client] client_addr=0x{:02X}, server_addr={}".format(CLIENT_ADDR, SERVER_ADDR))

    while True:
        try:
            client.connect(serial_conn)     # 阻塞直到关联（AARQ/AARE）完成
            print("[Client] connected & associated! (NONE auth)")
            run_once()
            break
        except Exception as e:
            print("[Client] connect/run failed: {}".format(e))
            utime.sleep(3)

    try:
        while True:
            utime.sleep(10)
            read_attr(LN_VOLT, 2)          # 周期性读取服务器采样的电压
            read_attr(LN_ENERGY, 2)
    except KeyboardInterrupt:
        pass
    finally:
        try:
            client.disconnect()
            print("[Client] disconnected")
        except Exception:
            pass


main()

```

客户端(LOW)
```python
# -*- coding: utf-8 -*-
"""
DLMS 客户端脚本（模组 B / 集中器）- LOW 认证版
==================================================================
通过 SerialConnection（HDLC over UART）读取/写入服务器（模组 A）

本脚本使用 LOW 认证连接（client_address = 0x20，对应服务器 LOW 关联）:
  * 密码认证：password 必须与服务器 assoc_low.secret 一致，
    否则 AARE 以 AUTHENTICATION_FAILURE 拒绝关联。
  * energy / ext_energy / param: 可读可写（服务器 NONE 级 READ_WRITE
    通过 access_control.c 级联语义在 LOW 连接下依然生效）
  * voltage:                      只读
  * config_reg (1.0.25.1.0.255): 可读，写入会被拒绝（仅 HIGH 可写）

接线（模组 A  <->  模组 B）:
    B.UART_RX  <---- A.UART_TX
    B.UART_TX  ----> A.UART_RX
    B.GND      ----  A.GND      （共地是必须的）

关键点:
  * client_address 必须等于对应认证级别关联对象的 clientSAP（0x20）。
  * server_address 必须等于 服务器序列号 % 10000 + 1000。
  * 波特率必须与服务器端一致（commSpeed）。
"""

import dlms
import utime

# ============================ 配置区 ============================
UART_PORT = 2        # 模组 B 使用的 UART 口
BAUD      = 9600     # 波特率，必须与服务器端一致

# 服务器端序列号 —— 必须与服务器脚本中的 SERIAL_NUM 一致！
SERVER_SERIAL = 12345

CLIENT_ADDR = 0x20   # LOW 认证关联对象的 clientSAP（dlms_server.py 中 SAP_LOW）
# 服务器地址 = 序列号 % 10000 + 1000
SERVER_ADDR = SERVER_SERIAL % 10000 + 1000   # = 3345
AUTH        = dlms.Authentication.LOW        # LOW 认证（密码）
PASSWORD    = "Quectel"                      # 必须与服务器 assoc_low.secret 一致

# 演示用对象的逻辑名（均为服务器上真实存在的对象）
LN_ENERGY = "1.0.1.8.0.255"    # 电能寄存器（Register，可写）
LN_VOLT   = "1.0.32.7.0.255"   # 电压寄存器（Register，只读）
LN_EXT    = "1.0.1.8.1.255"    # 扩展电能寄存器（ExtendedRegister，可写）
LN_PARAM  = "0.0.1.1.0.255"    # 参数 Data（可写）
LN_CONFIG = "1.0.25.1.0.255"   # 高权限寄存器（Register，仅 HIGH 认证可写）
LN_PUBLIC = "1.0.10.1.0.255"   # 公开只读寄存器（NONE+ 可读）
LN_LOWREAD = "1.0.11.1.0.255"  # LOW+ 可读寄存器（NONE 下不可读）
LN_HIGHREAD = "1.0.12.1.0.255" # HIGH 才可读寄存器（NONE/LOW 下不可读）
# ================================================================

# ------------------ HDLC 链路层配置（必须与服务器一致） ------------------
hdlc = dlms.IecHdlcSetup(
    "0.0.22.0.0.255",
    commSpeed=BAUD,
    windowSizeRx=1,
    windowSizeTx=1,
    maxInfoLenTx=128,
    maxInfoLenRx=128,
    timeout=120,
    deviceAddr=0x10,
)

serial_conn = dlms.SerialConnection(
    uart_port=UART_PORT,
    hdlc_setup=hdlc,
    flowcontrol=0,
    interface_type=dlms.InterfaceType.HDLC,  # 必须 HDLC
    use_logical_name=True,
)

client = dlms.Client(
    client_address=CLIENT_ADDR,
    server_address=SERVER_ADDR,
    authentication=AUTH,
    password=PASSWORD,       # LOW 认证密码
    use_logical_name=True,
)


def read_attr(ln, attr):
    """读取单个属性，失败返回 None 并打印错误。"""
    try:
        val = client.read(ln, attr)
        print("[Read ] {} attr{} = {}".format(ln, attr, val))
        return val
    except Exception as e:
        print("[Read ] {} attr{} FAILED: {}".format(ln, attr, e))
        return None


def write_attr(ln, attr, value):
    """写入单个属性，返回是否成功。"""
    try:
        client.write(ln, attr, value)
        print("[Write] {} attr{} = {} OK".format(ln, attr, value))
        return True
    except Exception as e:
        print("[Write] {} attr{} = {} FAILED: {}".format(ln, attr, value, e))
        return False


def run_once():
    """单轮测试：读取所有对象 -> 写入 -> 回读验证。"""
    print("-" * 50)

    # 1) 读取
    volt = read_attr(LN_VOLT, 2)          # 电压寄存器值（只读）
    read_attr(LN_ENERGY, 2)               # 电能寄存器值
    read_attr(LN_EXT, 2)                  # 扩展电能寄存器值
    read_attr(LN_PARAM, 2)                # 参数

    # 2) 写电能寄存器（属性2 value）—— LOW 下可写
    new_energy = (volt or 0) + 1000
    write_attr(LN_ENERGY, 2, new_energy)

    # 3) 写扩展电能寄存器 + 参数对象 —— LOW 下可写
    write_attr(LN_EXT, 2, 888)
    write_attr(LN_PARAM, 2, 100)

    # 4) 高权限寄存器：LOW 连接下可读但不可写（期望 READ_WRITE_DENIED）
    read_attr(LN_CONFIG, 2)
    write_attr(LN_CONFIG, 2, 9999)

    # 5) 读取权限分级验证（LOW 连接）
    read_attr(LN_PUBLIC, 2)     # 期望：OK（NONE 可读）
    read_attr(LN_LOWREAD, 2)    # 期望：OK（LOW+ 可读）
    read_attr(LN_HIGHREAD, 2)   # 期望：FAILED（仅 HIGH 可读）

    # 6) 回读验证
    read_attr(LN_ENERGY, 2)
    read_attr(LN_EXT, 2)
    read_attr(LN_PARAM, 2)


def main():
    print("[Client] Connecting to server via UART{} @ {} baud (LOW auth)...".format(UART_PORT, BAUD))
    print("[Client] client_addr=0x{:02X}, server_addr={}, password={}".format(
        CLIENT_ADDR, SERVER_ADDR, PASSWORD))

    while True:
        try:
            client.connect(serial_conn)     # 阻塞直到关联（AARQ/AARE）完成
            print("[Client] connected & associated! (LOW auth)")
            run_once()
            break
        except Exception as e:
            print("[Client] connect/run failed: {}".format(e))
            utime.sleep(3)

    try:
        while True:
            utime.sleep(10)
            read_attr(LN_VOLT, 2)          # 周期性读取服务器采样的电压
            read_attr(LN_ENERGY, 2)
    except KeyboardInterrupt:
        pass
    finally:
        try:
            client.disconnect()
            print("[Client] disconnected")
        except Exception:
            pass


main()

```

客户端(HIGH)
```python
# -*- coding: utf-8 -*-
"""
DLMS 客户端脚本（模组 B / 集中器）- HIGH 认证版
==================================================================
通过 SerialConnection（HDLC over UART）读取/写入服务器（模组 A）

本脚本使用 HIGH 认证连接（client_address = 0x30，对应服务器 HIGH 关联）:
  * HLS 挑战-响应认证：client.c 在 connect() 中自动完成 AARQ + 挑战响应，
    password 作为挑战密钥，必须与服务器 assoc_high.secret 一致。
  * energy / ext_energy / param: 可读可写
  * voltage:                      只读
  * config_reg (1.0.25.1.0.255): 可读可写 —— 这是唯一能写该对象的认证级别
    （服务器仅配置了 Authentication.HIGH: {2: READ_WRITE}）

接线（模组 A  <->  模组 B）:
    B.UART_RX  <---- A.UART_TX
    B.UART_TX  ----> A.UART_RX
    B.GND      ----  A.GND      （共地是必须的）

关键点:
  * client_address 必须等于对应认证级别关联对象的 clientSAP（0x30）。
  * server_address 必须等于 服务器序列号 % 10000 + 1000。
  * 波特率必须与服务器端一致（commSpeed）。
"""

import dlms
import utime

# ============================ 配置区 ============================
UART_PORT = 2        # 模组 B 使用的 UART 口
BAUD      = 9600     # 波特率，必须与服务器端一致

# 服务器端序列号 —— 必须与服务器脚本中的 SERIAL_NUM 一致！
SERVER_SERIAL = 12345

CLIENT_ADDR = 0x30   # HIGH 认证关联对象的 clientSAP（dlms_server.py 中 SAP_HIGH）
# 服务器地址 = 序列号 % 10000 + 1000
SERVER_ADDR = SERVER_SERIAL % 10000 + 1000   # = 3345
AUTH        = dlms.Authentication.HIGH       # HIGH 认证（HLS 挑战-响应）
PASSWORD    = "Quectel"                      # 必须与服务器 assoc_high.secret 一致

# 演示用对象的逻辑名（均为服务器上真实存在的对象）
LN_ENERGY = "1.0.1.8.0.255"    # 电能寄存器（Register，可写）
LN_VOLT   = "1.0.32.7.0.255"   # 电压寄存器（Register，只读）
LN_EXT    = "1.0.1.8.1.255"    # 扩展电能寄存器（ExtendedRegister，可写）
LN_PARAM  = "0.0.1.1.0.255"    # 参数 Data（可写）
LN_CONFIG = "1.0.25.1.0.255"   # 高权限寄存器（Register，仅 HIGH 认证可写）
LN_PUBLIC = "1.0.10.1.0.255"   # 公开只读寄存器（NONE+ 可读）
LN_LOWREAD = "1.0.11.1.0.255"  # LOW+ 可读寄存器（NONE 下不可读）
LN_HIGHREAD = "1.0.12.1.0.255" # HIGH 才可读寄存器（NONE/LOW 下不可读）
# ================================================================

# ------------------ HDLC 链路层配置（必须与服务器一致） ------------------
hdlc = dlms.IecHdlcSetup(
    "0.0.22.0.0.255",
    commSpeed=BAUD,
    windowSizeRx=1,
    windowSizeTx=1,
    maxInfoLenTx=128,
    maxInfoLenRx=128,
    timeout=120,
    deviceAddr=0x10,
)

serial_conn = dlms.SerialConnection(
    uart_port=UART_PORT,
    hdlc_setup=hdlc,
    flowcontrol=0,
    interface_type=dlms.InterfaceType.HDLC,  # 必须 HDLC
    use_logical_name=True,
)

client = dlms.Client(
    client_address=CLIENT_ADDR,
    server_address=SERVER_ADDR,
    authentication=AUTH,
    password=PASSWORD,       # HIGH 认证挑战密钥
    use_logical_name=True,
)


def read_attr(ln, attr):
    """读取单个属性，失败返回 None 并打印错误。"""
    try:
        val = client.read(ln, attr)
        print("[Read ] {} attr{} = {}".format(ln, attr, val))
        return val
    except Exception as e:
        print("[Read ] {} attr{} FAILED: {}".format(ln, attr, e))
        return None


def write_attr(ln, attr, value):
    """写入单个属性，返回是否成功。"""
    try:
        client.write(ln, attr, value)
        print("[Write] {} attr{} = {} OK".format(ln, attr, value))
        return True
    except Exception as e:
        print("[Write] {} attr{} = {} FAILED: {}".format(ln, attr, value, e))
        return False


def run_once():
    """单轮测试：读取所有对象 -> 写入 -> 回读验证。"""
    print("-" * 50)

    # 1) 读取
    volt = read_attr(LN_VOLT, 2)          # 电压寄存器值（只读）
    read_attr(LN_ENERGY, 2)               # 电能寄存器值
    read_attr(LN_EXT, 2)                  # 扩展电能寄存器值
    read_attr(LN_PARAM, 2)                # 参数

    # 2) 写电能寄存器（属性2 value）—— HIGH 下可写
    new_energy = (volt or 0) + 1000
    write_attr(LN_ENERGY, 2, new_energy)

    # 3) 写扩展电能寄存器 + 参数对象 —— HIGH 下可写
    write_attr(LN_EXT, 2, 888)
    write_attr(LN_PARAM, 2, 100)

    # 4) 高权限寄存器：HIGH 连接下可读也可写（这是唯一的可写认证级别）
    read_attr(LN_CONFIG, 2)
    write_attr(LN_CONFIG, 2, 9999)

    # 5) 读取权限分级验证（HIGH 连接）
    read_attr(LN_PUBLIC, 2)     # 期望：OK（NONE 可读）
    read_attr(LN_LOWREAD, 2)    # 期望：OK（LOW+ 可读）
    read_attr(LN_HIGHREAD, 2)   # 期望：OK（仅 HIGH 可读）

    # 6) 回读验证（重点确认 config_reg 写入成功）
    read_attr(LN_ENERGY, 2)
    read_attr(LN_EXT, 2)
    read_attr(LN_PARAM, 2)
    read_attr(LN_CONFIG, 2)


def main():
    print("[Client] Connecting to server via UART{} @ {} baud (HIGH auth)...".format(UART_PORT, BAUD))
    print("[Client] client_addr=0x{:02X}, server_addr={}, password={}".format(
        CLIENT_ADDR, SERVER_ADDR, PASSWORD))

    while True:
        try:
            client.connect(serial_conn)     # 阻塞直到关联（AARQ/AARE + HLS）完成
            print("[Client] connected & associated! (HIGH auth)")
            run_once()
            break
        except Exception as e:
            print("[Client] connect/run failed: {}".format(e))
            utime.sleep(3)

    try:
        while True:
            utime.sleep(10)
            read_attr(LN_VOLT, 2)          # 周期性读取服务器采样的电压
            read_attr(LN_ENERGY, 2)
    except KeyboardInterrupt:
        pass
    finally:
        try:
            client.disconnect()
            print("[Client] disconnected")
        except Exception:
            pass


main()


```

**构造函数参数**

| 参数 | 类型 | 默认值 | 说明 |
|------|------|--------|------|
| `uart_port` | `int` | 必传 | UART 端口号 |
| `hdlc_setup` | `IecHdlcSetup` | 必传 | HDLC 参数块 |
| `flowcontrol` | `int` | `0` | 流控制：`0`=无, `1`=RTS/CTS |
| `use_logical_name` | `bool` | `True` | LN 关联；`False` 为 SN 关联 |

---

### OpticalConnection (暂不开发)

光口连接。实现 IEC 62056-21 Mode E 协商协议。

**协商流程：**
1. 服务器在 300 bps (7E1) 等待客户端签到序列 `/?...\r\n`
2. 服务器以 300 bps 回复标识字符串 `/<FLAG><BAUD><MODE>\r\n`
3. 等待客户端 ACK 字节 (`0x06`) 确认波特率
4. 双方切换至协商后的波特率 (8N1)，继续 HDLC 帧通信

需要以下两个支持对象：

**LocalPortSetup**

接口类 19（OBIS `0.0.20.0.0.255`），配置光口参数。

| 属性 | 类型 | 说明 |
|------|------|------|
| `default_mode` | `int` | 光口协议模式（`0`=Mode E） |
| `default_baud` | `int` | 初始波特率（Mode E 必须为 `300`） |
| `proposed_baud` | `int` | 协商后的目标波特率（通常 `9600` 或 `19200`） |
| `response_time` | `int` | 签到与响应间的等待时间（毫秒，默认 `1000`） |
| `device_address` | `bytes` / `None` | 设备标识（最多 6 字节） |
| `password_1` | `bytes` / `None` | P1 级别（只读）光口密码 |
| `password_2` | `bytes` / `None` | P2 级别（读写）光口密码 |
| `password_5` | `bytes` / `None` | P5 级别（完全访问）光口密码 |

```python
import dlms

local_port = dlms.LocalPortSetup(
    "0.0.20.0.0.255",
    default_mode=0,
    default_baud=300,
    proposed_baud=9600,
    response_time=200,
    device_address=b"MTR001",
    password_1=b"00000000",
    password_2=b"11111111",
    password_5=b"AAAAAAAA",
)

hdlc = dlms.IecHdlcSetup("0.0.22.0.0.255", commSpeed=9600, deviceAddr=0x10)

conn = dlms.OpticalConnection(
    uart_port=1,
    local_port_setup=local_port,
    hdlc_setup=hdlc,
)
server.add_connection(conn)
```

**构造函数签名**

```python
OpticalConnection(uart_port, local_port_setup, hdlc_setup, *, use_logical_name=True)
```

---

### MobileConnection

蜂窝网络连接。在后台线程中运行 C 管理 UDP 套接字。支持两种模式：

| 模式 | 条件 | 说明 |
|------|------|------|
| **直连模式** | 不提供 `relay_tcp_setup` | 监听 `tcp_udp_setup` 定义的 UDP 端口，与知道设备公网 IP 的客户端直接通信 |
| **中继模式** | 提供 `relay_tcp_setup` | 向 UDP 中继服务器注册，中继按 HDLC 地址路由；支持 `connection.send(data)` 主动推送 |

**需要的支持对象**

| 对象 | 作用 |
|------|------|
| `TcpUdpSetup` | 本地 UDP 端口（默认 `4059`），可关联 `IPv4Setup` 报告 IP |
| `GprsSetup` | APN 配置 |
| `GsmDiagnostic` | 蜂窝信号质量属性 |
| `recv_buffer` | 预分配的 `bytearray`（推荐 `4096` 字节） |
| `relay_tcp_setup` (可选) | 中继模式的第二组 `TcpUdpSetup` |


**构造函数签名**

```python
MobileConnection(tcp_udp_setup, gprs_setup, gsm_diag, recv_buffer,
                 relay_tcp_setup=None, *, use_logical_name=True)
```
服务器
```python
# -*- coding: utf-8 -*-
"""
DLMS MobileConnection 服务器端 Demo —— 统一 NONE / LOW / HIGH 认证
==================================================================
传输：MobileConnection（UDP + HDLC 帧，IEC 62056-46），direct 模式（不经中继）。

配套客户端：
  client_address=0x10  NONE 认证
  client_address=2     LOW  认证
  client_address=5     HIGH 认证

地址约定：
  HDLC 服务器地址 = dlms.hdlc_server_address(SERIAL)
  客户端地址：NONE=0x10 / LOW=2 / HIGH=5（= 各关联对象 clientSAP）

注意：
  - IPv6 拨号后地址会变，脚本启动时自动从 dataCall 获取，无需手填。
  - 请确保模组已联网（必要时先 dataCall.activate(1)）。
"""

import dlms
from dlms import Conformance
import utime

# ============================ 配置区 ============================
USE_IPV6   = True            # True=IPv6；False=IPv4（与客户端一致）
SERIAL     = 12345           # 电表序列号 -> HDLC 服务器地址
FLAG_ID    = "QCT"           # 厂商代码（3 字符）
APN        = "vipmobile"     # 运营商 APN
PIN_CODE   = 0               # SIM PIN（0 = 无）
LOCAL_PORT = 4059            # 本机 UDP 监听端口（direct 模式）

SERVER_IP   = "10.152.151.79"          # IPv4 模式：本机 IPv4
SERVER_IPV6 = "240E:453:DD7D:2479::1"  # IPv6 模式：兜底值，实际自动从 dataCall 获取

# 三种认证级别各自的客户端地址（与客户端 CLIENT_ADDRESS 一致）
CLIENT_ADDR_NONE = 0x10     # NONE 认证关联对象
CLIENT_ADDR_LOW  = 2        # LOW  认证关联对象
CLIENT_ADDR_HIGH = 5        # HIGH 认证关联对象
PASSWORD = "12345678"       # LOW/HIGH 认证密码（= 各 assoc.secret）
# ================================================================


def get_local_ipv6():
    """自动获取本机 IPv6（拨号会变）。"""
    try:
        import dataCall
        info = dataCall.getInfo(1, 1)  # (profile, ip_version, [state, recon, ip, dns1, dns2])
        if info[2][0] == 1 and info[2][2]:
            return info[2][2].upper()
    except Exception as e:
        print("[Net] 获取 IPv6 失败：{}".format(e))
    return None


# ============================ COSEM 业务对象 ============================
energy = dlms.Register(
    "1.0.1.8.0.255", default_value=12345, scaler=1,
    access={dlms.Authentication.NONE: {2: dlms.AccessMode.READ_WRITE, 3: dlms.AccessMode.READ}},
)

voltage = dlms.Register(
    "1.0.32.7.0.255", default_value=230, scaler=1,
    access={dlms.Authentication.NONE: {2: dlms.AccessMode.READ, 3: dlms.AccessMode.READ}},
)

config_reg = dlms.Register(
    "1.0.25.1.0.255", default_value=0, scaler=0,
    access={dlms.Authentication.HIGH: {2: dlms.AccessMode.READ_WRITE, 3: dlms.AccessMode.READ}},
)

public_read_reg = dlms.Register(
    "1.0.10.1.0.255", default_value=100, scaler=0,
    access={dlms.Authentication.NONE: {2: dlms.AccessMode.READ}},
)

low_read_reg = dlms.Register(
    "1.0.11.1.0.255", default_value=200, scaler=0,
    access={
        dlms.Authentication.NONE: {2: dlms.AccessMode.NONE},
        dlms.Authentication.LOW:  {2: dlms.AccessMode.READ},
        dlms.Authentication.HIGH: {2: dlms.AccessMode.READ},
    },
)

high_read_reg = dlms.Register(
    "1.0.12.1.0.255", default_value=300, scaler=0,
    access={
        dlms.Authentication.NONE: {2: dlms.AccessMode.NONE},
        dlms.Authentication.LOW:  {2: dlms.AccessMode.NONE},
        dlms.Authentication.HIGH: {2: dlms.AccessMode.READ},
    },
)

param = dlms.Data(
    "0.0.1.1.0.255",
    access={dlms.Authentication.NONE: {2: dlms.AccessMode.READ_WRITE}},
)
param.value = 42

ldn = dlms.Data(
    "0.0.42.0.0.255",
    access={dlms.Authentication.NONE: {2: dlms.AccessMode.READ}},
)
ldn.value = b"SN12345"

# 类型级兜底：未显式授权时 NONE 默认只读
dlms.set_default_access(dlms.Register, {dlms.Authentication.NONE: {2: dlms.AccessMode.READ, 3: dlms.AccessMode.READ}})
dlms.set_default_access(dlms.Data, {dlms.Authentication.NONE: {2: dlms.AccessMode.READ}})

business = [energy, voltage, config_reg, public_read_reg, low_read_reg, high_read_reg, param, ldn]


def build_network(local_ip):
    """创建网络对象：GprsSetup / IP / TcpUdpSetup / GsmDiagnostic。"""
    gprs = dlms.GprsSetup("0.1.25.0.0.255", apn=APN, pin_code=PIN_CODE)
    if USE_IPV6:
        ip_setup = dlms.IPv6Setup(
            "0.0.25.7.0.255",
            datalink_reference=gprs,
            address_config_mode=2,              # MANUAL
            unicast_ip_address=[local_ip],      # 本机 IPv6
            primary_dns_address="2001:4860:4860::8888",
            secondary_dns_address="2001:4860:4860::8844",
        )
    else:
        ip_setup = dlms.IPv4Setup(
            "0.0.25.1.0.255",
            datalink_reference=gprs,
            ip_address=local_ip,
            subnet_mask="255.255.255.0",
            gateway_ip_address="0.0.0.0",
            use_dhcp=False,
        )
    tcp_udp = dlms.TcpUdpSetup(
        "0.0.25.2.0.255",
        port=LOCAL_PORT,
        ip_reference=ip_setup,
        max_segment_size=1460,
        max_simultaneous_connections=1,
        inactivity_timeout=120,
    )
    gsm_diag = dlms.GsmDiagnostic("0.0.25.6.0.255")
    return gprs, ip_setup, tcp_udp, gsm_diag


def build_assocs(objects):
    """创建 3 个认证等级的关联对象。"""
    FULL_CONF = (
        Conformance.BLOCK_TRANSFER_WITH_ACTION | Conformance.BLOCK_TRANSFER_WITH_SET_OR_WRITE |
        Conformance.BLOCK_TRANSFER_WITH_GET_OR_READ | Conformance.SET |
        Conformance.SELECTIVE_ACCESS | Conformance.ACTION |
        Conformance.MULTIPLE_REFERENCES | Conformance.GET
    )

    assoc_none = dlms.AssociationLogicalName("0.0.40.0.1.255")
    assoc_none.auth_mechanism = "None"
    assoc_none.clientSAP = CLIENT_ADDR_NONE
    assoc_none.objects = list(objects)
    assoc_none.context = dlms.DLMSContext(maxSendPduSize=128, maxReceivePduSize=128, conformance=FULL_CONF)

    assoc_low = dlms.AssociationLogicalName("0.0.40.0.2.255")
    assoc_low.auth_mechanism = "Low"
    assoc_low.secret = b"12345678"
    assoc_low.clientSAP = CLIENT_ADDR_LOW
    assoc_low.objects = list(objects)
    assoc_low.context = dlms.DLMSContext(maxSendPduSize=128, maxReceivePduSize=128, conformance=FULL_CONF)

    assoc_high = dlms.AssociationLogicalName("0.0.40.0.3.255")
    assoc_high.auth_mechanism = "High"
    assoc_high.secret = b"12345678"
    assoc_high.clientSAP = CLIENT_ADDR_HIGH
    assoc_high.objects = list(objects)
    assoc_high.context = dlms.DLMSContext(maxSendPduSize=128, maxReceivePduSize=128, conformance=FULL_CONF)

    return assoc_none, assoc_low, assoc_high


def main():
    # 1. 确定本机 IP（IPv6 自动获取，拨号会变）
    local_ip = SERVER_IPV6 if USE_IPV6 else SERVER_IP
    if USE_IPV6:
        auto = get_local_ipv6()
        if auto:
            local_ip = auto

    print("=" * 60)
    print("[Server] MobileConnection (NONE/LOW/HIGH) {}".format("IPv6" if USE_IPV6 else "IPv4"))
    print("[Server] 本机 {} = {}".format("IPv6" if USE_IPV6 else "IP", local_ip))
    print("[Server] 端口 = {}  服务器地址 = {} (0x{:02X})".format(LOCAL_PORT, dlms.hdlc_server_address(SERIAL), dlms.hdlc_server_address(SERIAL)))
    print("=" * 60)

    # 2. 网络对象 + 业务对象
    gprs, ip_setup, tcp_udp, gsm_diag = build_network(local_ip)
    objs = list(business)

    # 3. Server + 注册对象
    server = dlms.Server(serial_number=SERIAL, flag_id=FLAG_ID)
    for obj in objs + [gprs, ip_setup, tcp_udp, gsm_diag]:
        server.add_object(obj)
    for assoc in build_assocs(objs):
        server.add_object(assoc)

    # 4. MobileConnection（direct 模式，UDP + HDLC）
    mobile = dlms.MobileConnection(
        tcp_udp_setup=tcp_udp,
        gprs_setup=gprs,
        gsm_diag=gsm_diag,
        recv_buffer=bytearray(4096),
        relay_tcp_setup=None,
    )
    mobile.on_connected = lambda: print("[Server] UDP 通道已建立")
    mobile.on_disconnected = lambda: print("[Server] UDP 通道断开")
    server.add_connection(mobile)

    # 5. 启动
    server.run()
    print("[Server] 已启动，等待客户端连接...")
    if USE_IPV6:
        print("[Server] 客户端配置：SERVER_IPV6={}  PORT={}  SERIAL={}".format(local_ip, LOCAL_PORT, SERIAL))
    else:
        print("[Server] 客户端配置：SERVER_IP={}  PORT={}  SERIAL={}".format(local_ip, LOCAL_PORT, SERIAL))

    try:
        while True:
            utime.sleep(1)
    except KeyboardInterrupt:
        server.stop()
        print("[Server] 已停止")


if __name__ == "__main__":
    main()

```
客户端(NONE)
```python
# -*- coding: utf-8 -*-
"""
DLMS MobileConnection 客户端 Demo —— NONE 认证
==================================================================
传输：MobileConnection（UDP + HDLC 帧），direct 模式。

配套服务器：
  客户端地址 = 0x10 -> 服务器 NONE 关联对象（clientSAP=0x10）

预期（NONE 认证）：
  可读写：energy(1.0.1.8.0.255)、voltage、param
  不可读：low_read(1.0.11.1.0.255)、high_read(1.0.12.1.0.255)
  不可写：config_reg(1.0.25.1.0.255，仅 HIGH)
"""

import dlms
import utime

# ============================ 配置区 ============================
USE_IPV6       = True            # 与服务器一致
SERVER_SERIAL  = 12345           # 服务器序列号
SERVER_IP      = "10.152.151.79"          # IPv4：服务器 IPv4
SERVER_IPV6    = "240E:453:DD7D:2479::1"  # IPv6：服务器 IPv6（从服务器打印复制）
SERVER_PORT    = 4059
APN            = "vipmobile"

CLIENT_ADDRESS = 0x10                       # NONE
AUTHENTICATION = dlms.Authentication.NONE
PASSWORD       = None
# ================================================================


def build_mobile():
    """创建客户端 MobileConnection（目标 = 服务器）。"""
    gprs = dlms.GprsSetup("0.1.25.0.0.255", apn=APN, pin_code=0)
    if USE_IPV6:
        target_ip = dlms.IPv6Setup(
            "0.0.25.7.0.255",
            datalink_reference=gprs,
            address_config_mode=2,
            unicast_ip_address=[SERVER_IPV6],   # 服务器 IPv6
        )
    else:
        target_ip = dlms.IPv4Setup(
            "0.0.25.1.0.255",
            datalink_reference=gprs,
            ip_address=SERVER_IP,
            use_dhcp=False,
        )
    tcp_udp = dlms.TcpUdpSetup("0.0.25.2.0.255", port=SERVER_PORT, ip_reference=target_ip)
    gsm_diag = dlms.GsmDiagnostic("0.0.25.6.0.255")
    return dlms.MobileConnection(
        tcp_udp_setup=tcp_udp,
        gprs_setup=gprs,
        gsm_diag=gsm_diag,
        recv_buffer=bytearray(4096),
        relay_tcp_setup=None,
    )


def read_attr(client, obj, attr, name):
    try:
        client.read(obj, attr)
        print("[OK  ] {} attr{} = {}".format(name, attr, getattr(obj, "value", None)))
    except Exception as e:
        print("[FAIL] {} attr{} : {}".format(name, attr, e))


def main():
    target = SERVER_IPV6 if USE_IPV6 else SERVER_IP
    print("=" * 60)
    print("[Client] MobileConnection NONE auth ({})".format("IPv6" if USE_IPV6 else "IPv4"))
    print("[Client] 目标 = {}:{}".format(target, SERVER_PORT))
    print("=" * 60)

    mobile = build_mobile()
    client = dlms.Client(
        client_address=CLIENT_ADDRESS,
        server_address=dlms.hdlc_server_address(SERVER_SERIAL),
        authentication=AUTHENTICATION,
        password=PASSWORD,
    )

    # 连接（服务器冷启动首次回包可能失败，重试几次兜底）
    ok = False
    for attempt in range(1, 4):
        try:
            client.connect(mobile)
            print("[Client] 已连接并完成 DLMS 关联")
            ok = True
            break
        except Exception as e:
            print("[Client] 第 {} 次连接失败: {}".format(attempt, e))
            utime.sleep(attempt * 2)
    if not ok:
        return -1

    # NONE 等级验证
    energy = dlms.Register("1.0.1.8.0.255", 0)
    voltage = dlms.Register("1.0.32.7.0.255", 0)
    low_read = dlms.Register("1.0.11.1.0.255", 0)
    high_read = dlms.Register("1.0.12.1.0.255", 0)
    config_reg = dlms.Register("1.0.25.1.0.255", 0)

    read_attr(client, energy, 2, "energy")
    read_attr(client, voltage, 2, "voltage")
    read_attr(client, low_read, 2, "low_read")     # 期望 FAIL（NONE 无权限）
    read_attr(client, high_read, 2, "high_read")   # 期望 FAIL
    try:
        config_reg.value = 999
        client.write(config_reg, 2)                # 期望 FAIL（仅 HIGH）
        print("[OK  ] config_reg 写成功（意外）")
    except Exception as e:
        print("[FAIL] config_reg 写 : {}".format(e))

    try:
        client.disconnect()
    except Exception:
        pass
    return 0


if __name__ == "__main__":
    main()

```
客户端(LOW)
```python
# -*- coding: utf-8 -*-
"""
DLMS MobileConnection 客户端 Demo —— LOW 认证
==================================================================
传输：MobileConnection（UDP + HDLC 帧），direct 模式。

配套服务器：
  客户端地址 = 2 -> 服务器 LOW 关联对象（clientSAP=2），密码 = "12345678"

预期（LOW 认证）：
  可读写：energy(1.0.1.8.0.255)、voltage、param
  可读  ：low_read(1.0.11.1.0.255)
  不可读：high_read(1.0.12.1.0.255，仅 HIGH)
  不可写：config_reg(1.0.25.1.0.255，仅 HIGH)
"""

import dlms
import utime

# ============================ 配置区 ============================
USE_IPV6       = True            # 与服务器一致
SERVER_SERIAL  = 12345           # 服务器序列号
SERVER_IP      = "10.152.151.79"          # IPv4：服务器 IPv4
SERVER_IPV6    = "240E:453:DD7D:2479::1"  # IPv6：服务器 IPv6（从服务器打印复制）
SERVER_PORT    = 4059
APN            = "vipmobile"

CLIENT_ADDRESS = 2                          # LOW
AUTHENTICATION = dlms.Authentication.LOW
PASSWORD       = "12345678"
# ================================================================


def build_mobile():
    """创建客户端 MobileConnection（目标 = 服务器）。"""
    gprs = dlms.GprsSetup("0.1.25.0.0.255", apn=APN, pin_code=0)
    if USE_IPV6:
        target_ip = dlms.IPv6Setup(
            "0.0.25.7.0.255",
            datalink_reference=gprs,
            address_config_mode=2,
            unicast_ip_address=[SERVER_IPV6],   # 服务器 IPv6
        )
    else:
        target_ip = dlms.IPv4Setup(
            "0.0.25.1.0.255",
            datalink_reference=gprs,
            ip_address=SERVER_IP,
            use_dhcp=False,
        )
    tcp_udp = dlms.TcpUdpSetup("0.0.25.2.0.255", port=SERVER_PORT, ip_reference=target_ip)
    gsm_diag = dlms.GsmDiagnostic("0.0.25.6.0.255")
    return dlms.MobileConnection(
        tcp_udp_setup=tcp_udp,
        gprs_setup=gprs,
        gsm_diag=gsm_diag,
        recv_buffer=bytearray(4096),
        relay_tcp_setup=None,
    )


def read_attr(client, obj, attr, name):
    try:
        client.read(obj, attr)
        print("[OK  ] {} attr{} = {}".format(name, attr, getattr(obj, "value", None)))
    except Exception as e:
        print("[FAIL] {} attr{} : {}".format(name, attr, e))


def main():
    target = SERVER_IPV6 if USE_IPV6 else SERVER_IP
    print("=" * 60)
    print("[Client] MobileConnection LOW auth ({})".format("IPv6" if USE_IPV6 else "IPv4"))
    print("[Client] 目标 = {}:{}".format(target, SERVER_PORT))
    print("=" * 60)

    mobile = build_mobile()
    client = dlms.Client(
        client_address=CLIENT_ADDRESS,
        server_address=dlms.hdlc_server_address(SERVER_SERIAL),
        authentication=AUTHENTICATION,
        password=PASSWORD,
    )

    # 连接（服务器冷启动首次回包可能失败，重试几次兜底）
    ok = False
    for attempt in range(1, 4):
        try:
            client.connect(mobile)
            print("[Client] 已连接并完成 DLMS 关联")
            ok = True
            break
        except Exception as e:
            print("[Client] 第 {} 次连接失败: {}".format(attempt, e))
            utime.sleep(attempt * 2)
    if not ok:
        return -1

    # LOW 等级验证
    energy = dlms.Register("1.0.1.8.0.255", 0)
    voltage = dlms.Register("1.0.32.7.0.255", 0)
    low_read = dlms.Register("1.0.11.1.0.255", 0)
    high_read = dlms.Register("1.0.12.1.0.255", 0)
    config_reg = dlms.Register("1.0.25.1.0.255", 0)

    read_attr(client, energy, 2, "energy")
    read_attr(client, voltage, 2, "voltage")
    read_attr(client, low_read, 2, "low_read")     # 期望 OK（LOW 可读）
    read_attr(client, high_read, 2, "high_read")   # 期望 FAIL（仅 HIGH）
    try:
        config_reg.value = 999
        client.write(config_reg, 2)                # 期望 FAIL（仅 HIGH）
        print("[OK  ] config_reg 写成功（意外）")
    except Exception as e:
        print("[FAIL] config_reg 写 : {}".format(e))

    try:
        client.disconnect()
    except Exception:
        pass
    return 0


if __name__ == "__main__":
    main()

```
客户端(HIGH)
```python
# -*- coding: utf-8 -*-
"""
DLMS MobileConnection 客户端 Demo —— HIGH 认证
==================================================================
传输：MobileConnection（UDP + HDLC 帧），direct 模式。

配套服务器：
  客户端地址 = 5 -> 服务器 HIGH 关联对象（clientSAP=5），密码 = "12345678"

预期（HIGH 认证，最高权限）：
  可读写：energy(1.0.1.8.0.255)、voltage、param、config_reg(1.0.25.1.0.255)
  可读  ：low_read(1.0.11.1.0.255)、high_read(1.0.12.1.0.255)
"""

import dlms
import utime

# ============================ 配置区 ============================
USE_IPV6       = True            # 与服务器一致
SERVER_SERIAL  = 12345           # 服务器序列号
SERVER_IP      = "10.152.151.79"          # IPv4：服务器 IPv4
SERVER_IPV6    = "240E:453:DD7D:2479::1"  # IPv6：服务器 IPv6（从服务器打印复制）
SERVER_PORT    = 4059
APN            = "vipmobile"

CLIENT_ADDRESS = 5                          # HIGH
AUTHENTICATION = dlms.Authentication.HIGH
PASSWORD       = "12345678"
# ================================================================


def build_mobile():
    """创建客户端 MobileConnection（目标 = 服务器）。"""
    gprs = dlms.GprsSetup("0.1.25.0.0.255", apn=APN, pin_code=0)
    if USE_IPV6:
        target_ip = dlms.IPv6Setup(
            "0.0.25.7.0.255",
            datalink_reference=gprs,
            address_config_mode=2,
            unicast_ip_address=[SERVER_IPV6],   # 服务器 IPv6
        )
    else:
        target_ip = dlms.IPv4Setup(
            "0.0.25.1.0.255",
            datalink_reference=gprs,
            ip_address=SERVER_IP,
            use_dhcp=False,
        )
    tcp_udp = dlms.TcpUdpSetup("0.0.25.2.0.255", port=SERVER_PORT, ip_reference=target_ip)
    gsm_diag = dlms.GsmDiagnostic("0.0.25.6.0.255")
    return dlms.MobileConnection(
        tcp_udp_setup=tcp_udp,
        gprs_setup=gprs,
        gsm_diag=gsm_diag,
        recv_buffer=bytearray(4096),
        relay_tcp_setup=None,
    )


def read_attr(client, obj, attr, name):
    try:
        client.read(obj, attr)
        print("[OK  ] {} attr{} = {}".format(name, attr, getattr(obj, "value", None)))
    except Exception as e:
        print("[FAIL] {} attr{} : {}".format(name, attr, e))


def main():
    target = SERVER_IPV6 if USE_IPV6 else SERVER_IP
    print("=" * 60)
    print("[Client] MobileConnection HIGH auth ({})".format("IPv6" if USE_IPV6 else "IPv4"))
    print("[Client] 目标 = {}:{}".format(target, SERVER_PORT))
    print("=" * 60)

    mobile = build_mobile()
    client = dlms.Client(
        client_address=CLIENT_ADDRESS,
        server_address=dlms.hdlc_server_address(SERVER_SERIAL),
        authentication=AUTHENTICATION,
        password=PASSWORD,
    )

    # 连接（服务器冷启动首次回包可能失败，重试几次兜底）
    ok = False
    for attempt in range(1, 4):
        try:
            client.connect(mobile)
            print("[Client] 已连接并完成 DLMS 关联")
            ok = True
            break
        except Exception as e:
            print("[Client] 第 {} 次连接失败: {}".format(attempt, e))
            utime.sleep(attempt * 2)
    if not ok:
        return -1

    # HIGH 等级验证（全部应成功）
    energy = dlms.Register("1.0.1.8.0.255", 0)
    voltage = dlms.Register("1.0.32.7.0.255", 0)
    low_read = dlms.Register("1.0.11.1.0.255", 0)
    high_read = dlms.Register("1.0.12.1.0.255", 0)
    config_reg = dlms.Register("1.0.25.1.0.255", 0)

    read_attr(client, energy, 2, "energy")
    read_attr(client, voltage, 2, "voltage")
    read_attr(client, low_read, 2, "low_read")     # 期望 OK
    read_attr(client, high_read, 2, "high_read")   # 期望 OK
    try:
        config_reg.value = 999
        client.write(config_reg, 2)                # 期望 OK（HIGH 可写）
        client.read(config_reg, 2)
        print("[OK  ] config_reg 写后读回 = {}".format(config_reg.value))
    except Exception as e:
        print("[FAIL] config_reg 写 : {}".format(e))

    try:
        client.disconnect()
    except Exception:
        pass
    return 0


if __name__ == "__main__":
    main()

```
---

### GenericConnection

通用连接。适用于自定义传输场景（如特殊成帧的 UART、SPI、MQTT、G3-PLC 等）。

**特性：**
- 不创建 C 线程，由 Python 驱动 I/O 循环
- 通过 `interface_type` 选择成帧类型
- 调用 `conn.process_msg(data)` 处理收到的数据，返回响应字节或 `None`

**成帧类型**

| 常量 | 适用场景 | 所需参数 |
|------|----------|----------|
| `InterfaceType.HDLC` | 字节流传输：UART、SPI、RS485 | `hdlc_setup` |
| `InterfaceType.WRAPPER` | 数据包传输：UDP、MQTT、G3-PLC | `tcp_udp_setup` |
| `InterfaceType.HDLC_WITH_MODE_E` | 光口手动 Mode E | `hdlc_setup` + `local_port_setup` |


**构造函数签名**

```python
GenericConnection(interface_type, frame_size=1024, pdu_size=512,
                  *, hdlc_setup=None, tcp_udp_setup=None,
                  local_port_setup=None, use_logical_name=True)
```

因该模式可自定义传输模式，以下仅提供UART和TCP的示例

UART 模式
服务器
```python
# -*- coding: utf-8 -*-
"""
DLMS 服务器端脚本（简洁版）- GenericConnection UART/HDLC，统一 NONE/LOW/HIGH 认证
==================================================================
  * NONE (clientSAP=0x10) / LOW (clientSAP=2) / HIGH (clientSAP=5)
"""

import dlms
from dlms import Conformance
import utime
import _thread

try:
    from machine import UART  # QuecPython / MicroPython
except ImportError:
    UART = None  # PC 上无法跑 UART 传输

# ============================ 配置区 ============================
UART_PORT   = 2        # 模组 A 使用的 UART 口
BAUD        = 19200    # 波特率，必须与客户端一致（Mk7 客户端为 19200）
FLAG_ID     = "GRX"    # 厂商代码（3 字符）
SERIAL_NUM  = 12345    # 电表序列号（Python 模式由 set_serial_number 设置）
PASSWORD    = "12345678"  # LOW/HIGH 认证密码（对齐 Mk7 LLS 的 -P 12345678）

# ---- HDLC 地址（与 Mk7 客户端脚本保持一致）----
SERVER_ADDRESS   = 144      # 电表（服务器）HDLC 地址（逻辑=1, 物理=16 -> 1*128+16）
HDLC_DEVICE_ADDR = 16       # 物理设备地址（GXDLMSDirector 中的 0x10）

# 三种认证级别各自的客户端 HDLC 地址（互不相同；对应客户端 CLIENT_ADDRESS）
CLIENT_ADDR_NONE = 0x10     # NONE 认证关联对象（本 SDK dlms_client_none.py 约定 0x10）
CLIENT_ADDR_LOW  = 2        # LOW  认证关联对象（对齐 Mk7 LLS: -c 2）
CLIENT_ADDR_HIGH = 5        # HIGH 认证关联对象（对齐 Mk7 HLS 的 -c 5 习惯）

LOG_SAMPLE = False          # True 时打印周期采样值（默认关闭，避免刷屏）
# ================================================================

# Python 服务器模式（CONFIG_DLMS_BUILTIN_SERVER 关闭）：events.c 生效。
dlms.set_serial_number(SERIAL_NUM)

# ============================ COSEM 对象 ============================
energy = dlms.Register(
    "1.0.1.8.0.255",
    default_value=12345,
    scaler=1,
    access={
        dlms.Authentication.NONE: {
            2: dlms.AccessMode.READ_WRITE,   # value：客户端可写
            3: dlms.AccessMode.READ,         # scaler/unit
        }
    }
)

voltage = dlms.Register(
    "1.0.32.7.0.255",
    default_value=230,
    scaler=1,
    access={
        dlms.Authentication.NONE: {
            2: dlms.AccessMode.READ,
            3: dlms.AccessMode.READ,
        }
    }
)

# 高权限寄存器：仅 HIGH 认证可写
config_reg = dlms.Register(
    "1.0.25.1.0.255",          # 费率/参数配置寄存器（示意）
    default_value=0,
    scaler=0,
    access={
        dlms.Authentication.HIGH: {
            2: dlms.AccessMode.READ_WRITE,   # value：仅 HIGH 可写
            3: dlms.AccessMode.READ,
        }
    }
)

ext_energy = dlms.ExtendedRegister(
    "1.0.1.8.1.255",
    value=500,
    scaler=0,
    unit=dlms.Unit.ACTIVE_ENERGY,
    status=0,
    access={
        dlms.Authentication.NONE: {
            2: dlms.AccessMode.READ_WRITE,   # value：客户端可写
            3: dlms.AccessMode.READ,         # scaler/unit
            4: dlms.AccessMode.READ,         # status
            5: dlms.AccessMode.READ,         # capture_time
        }
    }
)
ext_energy.capture_time = (2026, 8, 20, 12, 0, 0)

param = dlms.Data(
    "0.0.1.1.0.255",
    access={dlms.Authentication.NONE: {2: dlms.AccessMode.READ_WRITE}}
)
param.value = 42

ldn = dlms.Data(
    "0.0.42.0.0.255",
    access={dlms.Authentication.NONE: {2: dlms.AccessMode.READ}}
)
ldn.value = b"SN12345"

public_read_reg = dlms.Register(
    "1.0.10.1.0.255",
    default_value=100,
    scaler=0,
    access={
        dlms.Authentication.NONE: {
            2: dlms.AccessMode.READ,
        }
    }
)

low_read_reg = dlms.Register(
    "1.0.11.1.0.255",
    default_value=200,
    scaler=0,
    access={
        dlms.Authentication.NONE: {
            2: dlms.AccessMode.NONE,   # NONE 连接：不可读
        },
        dlms.Authentication.LOW: {
            2: dlms.AccessMode.READ,   # LOW 连接：可读
        },
        dlms.Authentication.HIGH: {
            2: dlms.AccessMode.READ,   # HIGH 连接：可读
        },
    }
)

high_read_reg = dlms.Register(
    "1.0.12.1.0.255",
    default_value=300,
    scaler=0,
    access={
        dlms.Authentication.NONE: {
            2: dlms.AccessMode.NONE,
        },
        dlms.Authentication.LOW: {
            2: dlms.AccessMode.NONE,
        },
        dlms.Authentication.HIGH: {
            2: dlms.AccessMode.READ,
        },
    }
)

# 类型级兜底访问控制（确保写权限一定生效）
dlms.set_default_access(
    dlms.Register,
    {
        dlms.Authentication.NONE: {
            2: dlms.AccessMode.READ_WRITE,
            3: dlms.AccessMode.READ,
        }
    }
)
dlms.set_default_access(
    dlms.Data,
    {
        dlms.Authentication.NONE: {
            2: dlms.AccessMode.READ_WRITE,
        }
    }
)

# ------------------ HDLC 链路层配置 ------------------
hdlc = dlms.IecHdlcSetup(
    "0.0.22.0.0.255",
    commSpeed=BAUD,
    windowSizeRx=1,
    windowSizeTx=1,
    maxInfoLenTx=128,
    maxInfoLenRx=128,
    timeout=120,
    deviceAddr=HDLC_DEVICE_ADDR,   # 物理设备地址 0x10，与客户端 HDLC_DEVICE_ADDR 一致
)

all_objects = [
    energy, voltage, config_reg,
    public_read_reg, low_read_reg, high_read_reg,
    param, ldn, hdlc, ext_energy,
]

# 完整 conformance：GET/SET/ACTION + 块传输（HIGH 认证的 HLS 挑战-响应依赖 ACTION）
FULL_CONF = (
    Conformance.BLOCK_TRANSFER_WITH_ACTION | Conformance.BLOCK_TRANSFER_WITH_SET_OR_WRITE |
    Conformance.BLOCK_TRANSFER_WITH_GET_OR_READ | Conformance.SET |
    Conformance.SELECTIVE_ACCESS | Conformance.ACTION |
    Conformance.MULTIPLE_REFERENCES | Conformance.GET
)

# ------------------ 关联对象 1：NONE 认证 ------------------
assoc_none = dlms.AssociationLogicalName("0.0.40.0.1.255")
assoc_none.auth_mechanism = "None"
assoc_none.clientSAP = CLIENT_ADDR_NONE
assoc_none.objects = list(all_objects)
assoc_none.context = dlms.DLMSContext(
    maxSendPduSize=128,
    maxReceivePduSize=128,
    conformance=FULL_CONF,
)

# ------------------ 关联对象 2：LOW 认证 ------------------
assoc_low = dlms.AssociationLogicalName("0.0.40.0.2.255")
assoc_low.auth_mechanism = "Low"
assoc_low.secret = b"12345678"        # 密码，必须与 LOW 客户端 password 一致
assoc_low.clientSAP = CLIENT_ADDR_LOW
assoc_low.objects = list(all_objects)
assoc_low.context = dlms.DLMSContext(
    maxSendPduSize=128,
    maxReceivePduSize=128,
    conformance=FULL_CONF,
)

# ------------------ 关联对象 3：HIGH 认证 ------------------
assoc_high = dlms.AssociationLogicalName("0.0.40.0.3.255")
assoc_high.auth_mechanism = "High"
assoc_high.secret = b"12345678"       # HLS 挑战密钥，必须与 HIGH 客户端 password 一致
assoc_high.clientSAP = CLIENT_ADDR_HIGH
assoc_high.objects = list(all_objects)
assoc_high.context = dlms.DLMSContext(
    maxSendPduSize=128,
    maxReceivePduSize=128,
    conformance=FULL_CONF,
)

# ============================ 服务器 ============================
server = dlms.Server(serial_number=SERIAL_NUM, flag_id=FLAG_ID)
for obj in all_objects + [assoc_none, assoc_low, assoc_high]:
    server.add_object(obj)

generic_conn = dlms.GenericConnection(
    interface_type=dlms.InterfaceType.HDLC,  # 必须 HDLC（与客户端一致）
    frame_size=1024,
    pdu_size=512,
    hdlc_setup=hdlc,
    use_logical_name=True,
)

server.add_connection(generic_conn)
server.run()   # 设置 active registry + 注入对象；GenericConnection 不建 C 线程

print("[Server] DLMS server (NONE/LOW/HIGH) running via GenericConnection on UART{} @ {} baud".format(
    UART_PORT, BAUD))
print("[Server] Server addr={} (logical=1, physical={}), password={}".format(
    SERVER_ADDRESS, HDLC_DEVICE_ADDR, PASSWORD))


# ============================ Python UART 传输线程 ============================
def uart_loop(conn, uart_port, baud_rate):
    """后台线程：UART 读帧 -> process_msg -> 回包写回。"""
    try:
        if UART is None:
            print("[UART] machine.UART unavailable on PC")
            return
        uart = UART(uart_port, baud_rate, 8, 0, 1, 0)   # bits/parity/stop/flow
        conn.connect()    # process_msg 依赖 connected 状态
        print("[UART] Listening for DLMS frames on UART{} @ {} baud...".format(uart_port, baud_rate))
        buf = bytearray(1024)
        while True:
            n = uart.readinto(buf)
            if n and n > 0:
                resp = conn.process_msg(buf[:n])
                if resp:
                    uart.write(resp)
            utime.sleep(0.01)
    except Exception as e:
        print("[UART] Fatal error: {}".format(e))
    finally:
        try:
            conn.disconnect()
        except Exception:
            pass
        print("[UART] Thread stopped")


_thread.start_new_thread(uart_loop, (generic_conn, UART_PORT, BAUD))

# ============================ 主循环 ============================
# 模拟电表周期采集，更新寄存器值（客户端可读到变化的电压）
try:
    seq = 0
    while True:
        seq += 1
        v = 228 + (seq % 7)          # 230/229/... 波动，模拟真实采样
        voltage.value = v
        if LOG_SAMPLE:
            print("[Server] sample: voltage={} V, energy={}".format(v, energy.value))
        utime.sleep(5)
except KeyboardInterrupt:
    server.stop()
    print("[Server] stopped")

```

客户端(NONE)
```python
# -*- coding: utf-8 -*-
"""
DLMS 客户端脚本（简洁版）- GenericConnection UART/HDLC，NONE 认证
==================================================================
  * client_address = 0x10 -> 服务器 NONE 关联对象（clientSAP=0x10）
  * server_address = 144  -> 服务器地址（逻辑=1, 物理=16）
  * 波特率 19200，与服务器一致
"""

import utime

try:
    from machine import UART  # QuecPython / MicroPython
except ImportError:
    UART = None  # PC 上无法跑 UART 传输

import dlms

# ============================ 配置区 ============================
UART_ID     = UART.UART2 if UART else 2   # 模组 B 使用的 UART 口
BAUDRATE    = 19200                       # 必须与服务器端一致
DATABITS    = 8
PARITY      = 0
STOPBITS    = 1

CLIENT_ADDRESS = 0x10          # 对应服务器 NONE 关联对象的 clientSAP
SERVER_ADDRESS = 144           # 电表（服务器）HDLC 地址（逻辑=1, 物理=16）
HDLC_DEVICE_ADDR = 16          # 物理设备地址（GXDLMSDirector 中的 0x10）

USE_LOGICAL_NAME = True
AUTHENTICATION   = dlms.Authentication.NONE   # NONE 认证：无密码
PASSWORD         = None
SECURITY         = 0x00
# ================================================================

# 演示对象（服务器 dlms_server_generic_lite.py 上注册）
LN_ENERGY  = "1.0.1.8.0.255"     # 电能寄存器（Register，所有级别可写）
LN_VOLT    = "1.0.32.7.0.255"    # 电压寄存器（Register，只读）
LN_CONFIG  = "1.0.25.1.0.255"    # 高权限寄存器（Register，仅 HIGH 可写）
LN_PUBLIC  = "1.0.10.1.0.255"    # 公开只读寄存器（NONE+ 可读）
LN_LOWREAD = "1.0.11.1.0.255"    # LOW+ 可读寄存器（NONE 下不可读）
LN_HIGHREAD = "1.0.12.1.0.255"   # HIGH 才可读寄存器（NONE 下不可读）


class UartTransport:
    """Drive HDLC frames over a QuecPython UART（帧级接收，无回显）。"""

    def __init__(self, uart_id, baudrate, databits, parity, stopbits):
        self._uart_id = uart_id
        self._baudrate = baudrate
        if UART is not None:
            self._uart = UART(uart_id, baudrate, databits, parity, stopbits, 0)
            self._uart.set_callback(None)  # 避免回调与 read() 竞争
        else:
            self._uart = None

    def open(self):
        pass

    def close(self):
        pass

    def send(self, data):
        if self._uart is None:
            raise RuntimeError("UART not available")
        return self._uart.write(data)

    def receive(self, timeout_ms):
        """读取恰好一帧 HDLC（0x7E ... 0x7E），按帧长字段收齐；超时返回 None。"""
        if self._uart is None:
            return None
        deadline = utime.ticks_ms() + timeout_ms
        buf = bytearray()

        # Phase 1: 扫描起始 0x7E 标志（跳过垃圾/回显字节）
        while utime.ticks_ms() < deadline:
            if self._uart.any() > 0:
                b = self._uart.read(1)
                if b and b[0] == 0x7E:
                    buf.append(0x7E)
                    break
            else:
                utime.sleep_ms(5)
        if not buf:
            return None

        # Phase 2: 读 2 字节帧头（帧类型 + 长度）
        while len(buf) < 3 and utime.ticks_ms() < deadline:
            if self._uart.any() > 0:
                b = self._uart.read(1)
                if b:
                    buf.append(b[0])
            else:
                utime.sleep_ms(5)
        if len(buf) < 3:
            return None

        frame_len = ((buf[1] & 0x07) << 8) | buf[2]
        if frame_len < 4:
            return None

        # Phase 3: 读帧体
        need = frame_len - 1
        while len(buf) < 3 + need and utime.ticks_ms() < deadline:
            if self._uart.any() > 0:
                b = self._uart.read(1)
                if b:
                    buf.append(b[0])
            else:
                utime.sleep_ms(5)
        if len(buf) < 3 + need:
            return None
        return bytes(buf)


def build_hdlc_setup(baudrate=None):
    return dlms.IecHdlcSetup(
        "0.0.22.0.0.255",
        commSpeed=baudrate if baudrate else BAUDRATE,
        windowSizeRx=1,
        windowSizeTx=1,
        maxInfoLenTx=128,
        maxInfoLenRx=128,
        timeout=10,
        deviceAddr=HDLC_DEVICE_ADDR,
    )


def build_connection(transport, baudrate=None):
    conn = dlms.GenericConnection(
        interface_type=dlms.InterfaceType.HDLC,
        frame_size=1024,
        pdu_size=1024,
        hdlc_setup=build_hdlc_setup(baudrate),
        use_logical_name=USE_LOGICAL_NAME,
    )
    conn.on_send = lambda data: transport.send(data)
    conn.on_receive = lambda timeout_ms: transport.receive(timeout_ms)
    return conn


def build_client():
    return dlms.Client(
        client_address=CLIENT_ADDRESS,
        server_address=SERVER_ADDRESS,
        authentication=AUTHENTICATION,
        password=PASSWORD,
        security=SECURITY,
        use_logical_name=USE_LOGICAL_NAME,
    )


def read_attr(client, ln, attr):
    try:
        val = client.read(ln, attr)
        print("[Read ] {} attr{} = {}".format(ln, attr, val))
        return val
    except Exception as e:
        print("[Read ] {} attr{} FAILED: {}".format(ln, attr, e))
        return None


def write_attr(client, ln, attr, value):
    try:
        client.write(ln, attr, value)
        print("[Write] {} attr{} = {} OK".format(ln, attr, value))
        return True
    except Exception as e:
        print("[Write] {} attr{} = {} FAILED: {}".format(ln, attr, value, e))
        return False


def run_once(client):
    """单轮测试：读 -> 权限验证 -> 写。"""
    # 1) NONE 可读对象
    read_attr(client, LN_VOLT, 2)
    read_attr(client, LN_ENERGY, 2)
    read_attr(client, LN_PUBLIC, 2)

    # 2) 权限分级验证：LOW+ / HIGH 对象应被拒
    read_attr(client, LN_LOWREAD, 2)   # 期望 FAILED（READ_WRITE_DENIED）
    read_attr(client, LN_HIGHREAD, 2)  # 期望 FAILED（READ_WRITE_DENIED）

    # 3) 写电能寄存器：NONE 下可写
    write_attr(client, LN_ENERGY, 2, 2222)

    # 4) 写高权限寄存器：仅 HIGH 可写，NONE 下应被拒
    write_attr(client, LN_CONFIG, 2, 9999)   # 期望 FAILED（READ_WRITE_DENIED）


def main():
    print("[Client] UART{} @ {} baud, NONE auth, client=0x{:02X}, server={}".format(
        UART_ID, BAUDRATE, CLIENT_ADDRESS, SERVER_ADDRESS))

    transport = UartTransport(UART_ID, BAUDRATE, DATABITS, PARITY, STOPBITS)
    client = build_client()

    while True:
        try:
            conn = build_connection(transport, BAUDRATE)
            client.connect(conn)     # SNRM (HDLC) + AARQ（NONE 直接关联）
            print("[Client] connected & associated! (NONE auth)")
            run_once(client)
            break
        except Exception as e:
            print("[Client] connect/run failed: {}".format(e))
            utime.sleep(3)

    try:
        while True:
            utime.sleep(10)
            read_attr(client, LN_VOLT, 2)     # 周期性读服务器采样的电压
            read_attr(client, LN_ENERGY, 2)
    except KeyboardInterrupt:
        pass
    finally:
        try:
            client.disconnect()
        except Exception:
            pass


if __name__ == "__main__":
    main()

```
客户端(LOW)
```python
# -*- coding: utf-8 -*-
"""
DLMS 客户端脚本（简洁版）- GenericConnection UART/HDLC，LOW 认证
==================================================================
  * client_address = 2    -> 服务器 LOW 关联对象（clientSAP=2，对齐 Mk7 LLS -c 2）
  * server_address = 144  -> 服务器地址（逻辑=1, 物理=16）
  * 波特率 19200；LOW 认证：密码 "12345678"
"""

import utime

try:
    from machine import UART  # QuecPython / MicroPython
except ImportError:
    UART = None  # PC 上无法跑 UART 传输

import dlms

# ============================ 配置区 ============================
UART_ID     = UART.UART2 if UART else 2   # 模组 B 使用的 UART 口
BAUDRATE    = 19200                       # 必须与服务器端一致
DATABITS    = 8
PARITY      = 0
STOPBITS    = 1

CLIENT_ADDRESS = 2               # 对应服务器 LOW 关联对象的 clientSAP（Mk7 LLS -c 2）
SERVER_ADDRESS = 144             # 电表（服务器）HDLC 地址（逻辑=1, 物理=16）
HDLC_DEVICE_ADDR = 16            # 物理设备地址（GXDLMSDirector 中的 0x10）

USE_LOGICAL_NAME = True
AUTHENTICATION   = dlms.Authentication.LOW    # LOW 认证：密码方式
PASSWORD         = "12345678"                 # 必须与服务器 assoc_low.secret 一致
SECURITY         = 0x00
# ================================================================

# 演示对象（服务器 dlms_server_generic_lite.py 上注册）
LN_ENERGY  = "1.0.1.8.0.255"     # 电能寄存器（Register，所有级别可写）
LN_VOLT    = "1.0.32.7.0.255"    # 电压寄存器（Register，只读）
LN_CONFIG  = "1.0.25.1.0.255"    # 高权限寄存器（Register，仅 HIGH 可写）
LN_PUBLIC  = "1.0.10.1.0.255"    # 公开只读寄存器（NONE+ 可读）
LN_LOWREAD = "1.0.11.1.0.255"    # LOW+ 可读寄存器（LOW 可读）
LN_HIGHREAD = "1.0.12.1.0.255"   # HIGH 才可读寄存器（LOW 下不可读）


class UartTransport:
    """Drive HDLC frames over a QuecPython UART（帧级接收，无回显）。"""

    def __init__(self, uart_id, baudrate, databits, parity, stopbits):
        self._uart_id = uart_id
        self._baudrate = baudrate
        if UART is not None:
            self._uart = UART(uart_id, baudrate, databits, parity, stopbits, 0)
            self._uart.set_callback(None)  # 避免回调与 read() 竞争
        else:
            self._uart = None

    def open(self):
        pass

    def close(self):
        pass

    def send(self, data):
        if self._uart is None:
            raise RuntimeError("UART not available")
        return self._uart.write(data)

    def receive(self, timeout_ms):
        """读取恰好一帧 HDLC（0x7E ... 0x7E），按帧长字段收齐；超时返回 None。"""
        if self._uart is None:
            return None
        deadline = utime.ticks_ms() + timeout_ms
        buf = bytearray()

        # Phase 1: 扫描起始 0x7E 标志（跳过垃圾/回显字节）
        while utime.ticks_ms() < deadline:
            if self._uart.any() > 0:
                b = self._uart.read(1)
                if b and b[0] == 0x7E:
                    buf.append(0x7E)
                    break
            else:
                utime.sleep_ms(5)
        if not buf:
            return None

        # Phase 2: 读 2 字节帧头（帧类型 + 长度）
        while len(buf) < 3 and utime.ticks_ms() < deadline:
            if self._uart.any() > 0:
                b = self._uart.read(1)
                if b:
                    buf.append(b[0])
            else:
                utime.sleep_ms(5)
        if len(buf) < 3:
            return None

        frame_len = ((buf[1] & 0x07) << 8) | buf[2]
        if frame_len < 4:
            return None

        # Phase 3: 读帧体
        need = frame_len - 1
        while len(buf) < 3 + need and utime.ticks_ms() < deadline:
            if self._uart.any() > 0:
                b = self._uart.read(1)
                if b:
                    buf.append(b[0])
            else:
                utime.sleep_ms(5)
        if len(buf) < 3 + need:
            return None
        return bytes(buf)


def build_hdlc_setup(baudrate=None):
    return dlms.IecHdlcSetup(
        "0.0.22.0.0.255",
        commSpeed=baudrate if baudrate else BAUDRATE,
        windowSizeRx=1,
        windowSizeTx=1,
        maxInfoLenTx=128,
        maxInfoLenRx=128,
        timeout=10,
        deviceAddr=HDLC_DEVICE_ADDR,
    )


def build_connection(transport, baudrate=None):
    conn = dlms.GenericConnection(
        interface_type=dlms.InterfaceType.HDLC,
        frame_size=1024,
        pdu_size=1024,
        hdlc_setup=build_hdlc_setup(baudrate),
        use_logical_name=USE_LOGICAL_NAME,
    )
    conn.on_send = lambda data: transport.send(data)
    conn.on_receive = lambda timeout_ms: transport.receive(timeout_ms)
    return conn


def build_client():
    return dlms.Client(
        client_address=CLIENT_ADDRESS,
        server_address=SERVER_ADDRESS,
        authentication=AUTHENTICATION,
        password=PASSWORD,
        security=SECURITY,
        use_logical_name=USE_LOGICAL_NAME,
    )


def read_attr(client, ln, attr):
    try:
        val = client.read(ln, attr)
        print("[Read ] {} attr{} = {}".format(ln, attr, val))
        return val
    except Exception as e:
        print("[Read ] {} attr{} FAILED: {}".format(ln, attr, e))
        return None


def write_attr(client, ln, attr, value):
    try:
        client.write(ln, attr, value)
        print("[Write] {} attr{} = {} OK".format(ln, attr, value))
        return True
    except Exception as e:
        print("[Write] {} attr{} = {} FAILED: {}".format(ln, attr, value, e))
        return False


def run_once(client):
    """单轮测试：读 -> 权限验证 -> 写。"""
    # 1) LOW 可读对象
    read_attr(client, LN_VOLT, 2)
    read_attr(client, LN_ENERGY, 2)
    read_attr(client, LN_PUBLIC, 2)
    read_attr(client, LN_LOWREAD, 2)   # LOW 可读

    # 2) 权限分级验证：HIGH 对象应被拒
    read_attr(client, LN_HIGHREAD, 2)  # 期望 FAILED（READ_WRITE_DENIED）

    # 3) 写电能寄存器：LOW 下可写
    write_attr(client, LN_ENERGY, 2, 3333)

    # 4) 写高权限寄存器：仅 HIGH 可写，LOW 下应被拒
    write_attr(client, LN_CONFIG, 2, 9999)   # 期望 FAILED（READ_WRITE_DENIED）


def main():
    print("[Client] UART{} @ {} baud, LOW auth, client={}, server={}, pwd='{}'".format(
        UART_ID, BAUDRATE, CLIENT_ADDRESS, SERVER_ADDRESS, PASSWORD))

    transport = UartTransport(UART_ID, BAUDRATE, DATABITS, PARITY, STOPBITS)
    client = build_client()

    while True:
        try:
            conn = build_connection(transport, BAUDRATE)
            client.connect(conn)     # SNRM (HDLC) + AARQ（LOW：密码随 AARQ）
            print("[Client] connected & associated! (LOW auth)")
            run_once(client)
            break
        except Exception as e:
            print("[Client] connect/run failed: {}".format(e))
            utime.sleep(3)

    try:
        while True:
            utime.sleep(10)
            read_attr(client, LN_VOLT, 2)     # 周期性读服务器采样的电压
            read_attr(client, LN_ENERGY, 2)
    except KeyboardInterrupt:
        pass
    finally:
        try:
            client.disconnect()
        except Exception:
            pass


if __name__ == "__main__":
    main()

```
客户端(HIGH)
```python
# -*- coding: utf-8 -*-
"""
DLMS 客户端脚本（简洁版）- GenericConnection UART/HDLC，HIGH 认证
==================================================================
  * client_address = 5    -> 服务器 HIGH 关联对象（clientSAP=5，对齐 Mk7 HLS -c 5）
  * server_address = 144  -> 服务器地址（逻辑=1, 物理=16）
  * 波特率 19200；HIGH 认证：HLS 挑战-响应，密码 "12345678"
"""

import utime

try:
    from machine import UART  # QuecPython / MicroPython
except ImportError:
    UART = None  # PC 上无法跑 UART 传输

import dlms

# ============================ 配置区 ============================
UART_ID     = UART.UART2 if UART else 2   # 模组 B 使用的 UART 口
BAUDRATE    = 19200                       # 必须与服务器端一致
DATABITS    = 8
PARITY      = 0
STOPBITS    = 1

CLIENT_ADDRESS = 5               # 对应服务器 HIGH 关联对象的 clientSAP（Mk7 HLS -c 5）
SERVER_ADDRESS = 144             # 电表（服务器）HDLC 地址（逻辑=1, 物理=16）
HDLC_DEVICE_ADDR = 16            # 物理设备地址（GXDLMSDirector 中的 0x10）

USE_LOGICAL_NAME = True
AUTHENTICATION   = dlms.Authentication.HIGH   # HIGH 认证：HLS 挑战-响应
PASSWORD         = "12345678"                 # 挑战密钥，必须与服务器 assoc_high.secret 一致
SECURITY         = 0x00
# ================================================================

# 演示对象（服务器 dlms_server_generic_lite.py 上注册）
LN_ENERGY  = "1.0.1.8.0.255"     # 电能寄存器（Register，所有级别可写）
LN_VOLT    = "1.0.32.7.0.255"    # 电压寄存器（Register，只读）
LN_CONFIG  = "1.0.25.1.0.255"    # 高权限寄存器（Register，仅 HIGH 可写）
LN_PUBLIC  = "1.0.10.1.0.255"    # 公开只读寄存器（NONE+ 可读）
LN_LOWREAD = "1.0.11.1.0.255"    # LOW+ 可读寄存器（HIGH 可读）
LN_HIGHREAD = "1.0.12.1.0.255"   # HIGH 才可读寄存器（HIGH 可读）


class UartTransport:
    """Drive HDLC frames over a QuecPython UART（帧级接收，无回显）。"""

    def __init__(self, uart_id, baudrate, databits, parity, stopbits):
        self._uart_id = uart_id
        self._baudrate = baudrate
        if UART is not None:
            self._uart = UART(uart_id, baudrate, databits, parity, stopbits, 0)
            self._uart.set_callback(None)  # 避免回调与 read() 竞争
        else:
            self._uart = None

    def open(self):
        pass

    def close(self):
        pass

    def send(self, data):
        if self._uart is None:
            raise RuntimeError("UART not available")
        return self._uart.write(data)

    def receive(self, timeout_ms):
        """读取恰好一帧 HDLC（0x7E ... 0x7E），按帧长字段收齐；超时返回 None。"""
        if self._uart is None:
            return None
        deadline = utime.ticks_ms() + timeout_ms
        buf = bytearray()

        # Phase 1: 扫描起始 0x7E 标志（跳过垃圾/回显字节）
        while utime.ticks_ms() < deadline:
            if self._uart.any() > 0:
                b = self._uart.read(1)
                if b and b[0] == 0x7E:
                    buf.append(0x7E)
                    break
            else:
                utime.sleep_ms(5)
        if not buf:
            return None

        # Phase 2: 读 2 字节帧头（帧类型 + 长度）
        while len(buf) < 3 and utime.ticks_ms() < deadline:
            if self._uart.any() > 0:
                b = self._uart.read(1)
                if b:
                    buf.append(b[0])
            else:
                utime.sleep_ms(5)
        if len(buf) < 3:
            return None

        frame_len = ((buf[1] & 0x07) << 8) | buf[2]
        if frame_len < 4:
            return None

        # Phase 3: 读帧体
        need = frame_len - 1
        while len(buf) < 3 + need and utime.ticks_ms() < deadline:
            if self._uart.any() > 0:
                b = self._uart.read(1)
                if b:
                    buf.append(b[0])
            else:
                utime.sleep_ms(5)
        if len(buf) < 3 + need:
            return None
        return bytes(buf)


def build_hdlc_setup(baudrate=None):
    return dlms.IecHdlcSetup(
        "0.0.22.0.0.255",
        commSpeed=baudrate if baudrate else BAUDRATE,
        windowSizeRx=1,
        windowSizeTx=1,
        maxInfoLenTx=128,
        maxInfoLenRx=128,
        timeout=10,
        deviceAddr=HDLC_DEVICE_ADDR,
    )


def build_connection(transport, baudrate=None):
    conn = dlms.GenericConnection(
        interface_type=dlms.InterfaceType.HDLC,
        frame_size=1024,
        pdu_size=1024,
        hdlc_setup=build_hdlc_setup(baudrate),
        use_logical_name=USE_LOGICAL_NAME,
    )
    conn.on_send = lambda data: transport.send(data)
    conn.on_receive = lambda timeout_ms: transport.receive(timeout_ms)
    return conn


def build_client():
    return dlms.Client(
        client_address=CLIENT_ADDRESS,
        server_address=SERVER_ADDRESS,
        authentication=AUTHENTICATION,
        password=PASSWORD,
        security=SECURITY,
        use_logical_name=USE_LOGICAL_NAME,
    )


def read_attr(client, ln, attr):
    try:
        val = client.read(ln, attr)
        print("[Read ] {} attr{} = {}".format(ln, attr, val))
        return val
    except Exception as e:
        print("[Read ] {} attr{} FAILED: {}".format(ln, attr, e))
        return None


def write_attr(client, ln, attr, value):
    try:
        client.write(ln, attr, value)
        print("[Write] {} attr{} = {} OK".format(ln, attr, value))
        return True
    except Exception as e:
        print("[Write] {} attr{} = {} FAILED: {}".format(ln, attr, value, e))
        return False


def run_once(client):
    """单轮测试：读 -> 权限验证 -> 写。"""
    # 1) 全部对象可读（HIGH 是最高权限）
    read_attr(client, LN_VOLT, 2)
    read_attr(client, LN_ENERGY, 2)
    read_attr(client, LN_PUBLIC, 2)
    read_attr(client, LN_LOWREAD, 2)
    read_attr(client, LN_HIGHREAD, 2)   # 仅 HIGH 可读
    read_attr(client, LN_CONFIG, 2)

    # 2) 写电能寄存器：HIGH 下可写
    write_attr(client, LN_ENERGY, 2, 4444)

    # 3) 写高权限寄存器：仅 HIGH 可写
    write_attr(client, LN_CONFIG, 2, 9999)   # 期望 OK
    read_attr(client, LN_CONFIG, 2)          # 回读验证


def main():
    print("[Client] UART{} @ {} baud, HIGH auth, client={}, server={}, pwd='{}'".format(
        UART_ID, BAUDRATE, CLIENT_ADDRESS, SERVER_ADDRESS, PASSWORD))

    transport = UartTransport(UART_ID, BAUDRATE, DATABITS, PARITY, STOPBITS)
    client = build_client()

    while True:
        try:
            conn = build_connection(transport, BAUDRATE)
            client.connect(conn)     # SNRM (HDLC) + AARQ + HLS（自动）
            print("[Client] connected & associated! (HIGH auth)")
            run_once(client)
            break
        except Exception as e:
            print("[Client] connect/run failed: {}".format(e))
            utime.sleep(3)

    try:
        while True:
            utime.sleep(10)
            read_attr(client, LN_VOLT, 2)     # 周期性读服务器采样的电压
            read_attr(client, LN_ENERGY, 2)
    except KeyboardInterrupt:
        pass
    finally:
        try:
            client.disconnect()
        except Exception:
            pass


if __name__ == "__main__":
    main()

```
---

TCP 模式
服务器
```python
# -*- coding: utf-8 -*-
"""
DLMS 服务器端脚本（简洁版）- GenericConnection TCP/WRAPPER，统一 NONE/LOW/HIGH 认证
==================================================================
  * NONE (clientSAP=0x10) / LOW (clientSAP=2) / HIGH (clientSAP=5)
  * 配 _lite 客户端脚本使用（dlms_client_*_generic_tcp_lite.py）
"""

import dlms
from dlms import Conformance
import utime

try:
    import usocket as socket  # QuecPython / MicroPython
except ImportError:
    try:
        import socket  # 标准名兼容
    except ImportError:
        socket = None  # 无 socket 模块（纯 PC 环境）

# ============================ 配置区 ============================
TCP_PORT    = 4020    # TCP 端口，客户端必须一致
# IPv4/IPv6 开关（与客户端一致）：False = IPv4；True = IPv6
USE_IPV6    = True
# 服务器监听地址：IPv4 填 SIM IPv4；IPv6 填模组自身 IPv6（拨号会变！）
# 绑定失败会自动回退 "0.0.0.0"（IPv4）/ "::"（IPv6，监听所有网卡）
SERVER_IP   = "240E:452:DDAB:928D::1"

FLAG_ID     = "GRX"    # 厂商代码（3 字符）
SERIAL_NUM  = 12345    # 电表序列号
PASSWORD    = "12345678"  # LOW/HIGH 认证密码

# ---- 地址（与客户端脚本保持一致）----
SERVER_ADDRESS   = 144      # 电表（服务器）地址（逻辑=1, 物理=16 -> 1*128+16）
HDLC_DEVICE_ADDR = 16       # 物理设备地址

# 三种认证级别各自的客户端地址（互不相同；对应客户端 CLIENT_ADDRESS）
CLIENT_ADDR_NONE = 0x10     # NONE 认证关联对象
CLIENT_ADDR_LOW  = 2        # LOW  认证关联对象
CLIENT_ADDR_HIGH = 5        # HIGH 认证关联对象

LOG_SAMPLE = True           # False 时不打印周期性采样值
# ================================================================

print("[Server] TCP:{}:{} addr={} pwd='{}' client NONE=0x{:02X} LOW={} HIGH={}".format(
    SERVER_IP, TCP_PORT, SERVER_ADDRESS, PASSWORD,
    CLIENT_ADDR_NONE, CLIENT_ADDR_LOW, CLIENT_ADDR_HIGH))

dlms.set_serial_number(SERIAL_NUM)

# ============================ COSEM 对象 ============================
energy = dlms.Register(
    "1.0.1.8.0.255",
    default_value=12345,
    scaler=1,
    access={
        dlms.Authentication.NONE: {
            2: dlms.AccessMode.READ_WRITE,
            3: dlms.AccessMode.READ,
        }
    }
)

voltage = dlms.Register(
    "1.0.32.7.0.255",
    default_value=230,
    scaler=1,
    access={
        dlms.Authentication.NONE: {
            2: dlms.AccessMode.READ,
            3: dlms.AccessMode.READ,
        }
    }
)

# 高权限寄存器：仅 HIGH 认证可写
config_reg = dlms.Register(
    "1.0.25.1.0.255",
    default_value=0,
    scaler=0,
    access={
        dlms.Authentication.HIGH: {
            2: dlms.AccessMode.READ_WRITE,
            3: dlms.AccessMode.READ,
        }
    }
)

ext_energy = dlms.ExtendedRegister(
    "1.0.1.8.1.255",
    value=500,
    scaler=0,
    unit=dlms.Unit.ACTIVE_ENERGY,
    status=0,
    access={
        dlms.Authentication.NONE: {
            2: dlms.AccessMode.READ_WRITE,
            3: dlms.AccessMode.READ,
            4: dlms.AccessMode.READ,
            5: dlms.AccessMode.READ,
        }
    }
)
ext_energy.capture_time = (2026, 8, 20, 12, 0, 0)

param = dlms.Data(
    "0.0.1.1.0.255",
    access={dlms.Authentication.NONE: {2: dlms.AccessMode.READ_WRITE}}
)
param.value = 42

ldn = dlms.Data(
    "0.0.42.0.0.255",
    access={dlms.Authentication.NONE: {2: dlms.AccessMode.READ}}
)
ldn.value = b"SN12345"

public_read_reg = dlms.Register(
    "1.0.10.1.0.255",
    default_value=100,
    scaler=0,
    access={
        dlms.Authentication.NONE: {2: dlms.AccessMode.READ},
    }
)

low_read_reg = dlms.Register(
    "1.0.11.1.0.255",
    default_value=200,
    scaler=0,
    access={
        dlms.Authentication.NONE: {2: dlms.AccessMode.NONE},
        dlms.Authentication.LOW:  {2: dlms.AccessMode.READ},
        dlms.Authentication.HIGH: {2: dlms.AccessMode.READ},
    }
)

high_read_reg = dlms.Register(
    "1.0.12.1.0.255",
    default_value=300,
    scaler=0,
    access={
        dlms.Authentication.NONE: {2: dlms.AccessMode.NONE},
        dlms.Authentication.LOW:  {2: dlms.AccessMode.NONE},
        dlms.Authentication.HIGH: {2: dlms.AccessMode.READ},
    }
)

# 类型级兜底访问控制
dlms.set_default_access(
    dlms.Register,
    {
        dlms.Authentication.NONE: {
            2: dlms.AccessMode.READ_WRITE,
            3: dlms.AccessMode.READ,
        }
    }
)
dlms.set_default_access(
    dlms.Data,
    {
        dlms.Authentication.NONE: {
            2: dlms.AccessMode.READ_WRITE,
        }
    }
)

# ------------------ 网络设置对象（TCP，与 USE_IPV6 一致） ------------------
if USE_IPV6:
    ip_setup = dlms.IPv6Setup(
        "0.0.25.7.0.255",
        address_config_mode=2,                    # MANUAL
        unicast_ip_address=[SERVER_IP],
        primary_dns_address="2001:4860:4860::8888",
        secondary_dns_address="2001:4860:4860::8844",
    )
else:
    ip_setup = dlms.IPv4Setup(
        "0.0.25.1.0.255",
        ip_address=SERVER_IP if SERVER_IP else "0.0.0.0",
        subnet_mask="255.255.255.0",
        gateway_ip_address="0.0.0.0",
        use_dhcp=(not SERVER_IP or SERVER_IP == "0.0.0.0"),
    )
tcp_udp = dlms.TcpUdpSetup(
    "0.0.25.2.0.255",
    port=TCP_PORT,
    ip_reference=ip_setup,
    max_segment_size=1460,
    max_simultaneous_connections=1,
    inactivity_timeout=120,
)

# ------------------ HDLC 链路层配置（对象用） ------------------
hdlc = dlms.IecHdlcSetup(
    "0.0.22.0.0.255",
    commSpeed=9600,
    windowSizeRx=1,
    windowSizeTx=1,
    maxInfoLenTx=128,
    maxInfoLenRx=128,
    timeout=120,
    deviceAddr=HDLC_DEVICE_ADDR,
)

all_objects = [
    energy, voltage, config_reg,
    public_read_reg, low_read_reg, high_read_reg,
    param, ldn, hdlc, ext_energy, ip_setup, tcp_udp,
]

# 完整 conformance（HIGH 的 HLS 需要 ACTION，长响应需要块传输）
FULL_CONF = (
    Conformance.BLOCK_TRANSFER_WITH_ACTION | Conformance.BLOCK_TRANSFER_WITH_SET_OR_WRITE |
    Conformance.BLOCK_TRANSFER_WITH_GET_OR_READ | Conformance.SET |
    Conformance.SELECTIVE_ACCESS | Conformance.ACTION |
    Conformance.MULTIPLE_REFERENCES | Conformance.GET
)

assoc_none = dlms.AssociationLogicalName("0.0.40.0.1.255")
assoc_none.auth_mechanism = "None"
assoc_none.clientSAP = CLIENT_ADDR_NONE
assoc_none.objects = list(all_objects)
assoc_none.context = dlms.DLMSContext(
    maxSendPduSize=128,
    maxReceivePduSize=128,
    conformance=FULL_CONF,
)

assoc_low = dlms.AssociationLogicalName("0.0.40.0.2.255")
assoc_low.auth_mechanism = "Low"
assoc_low.secret = b"12345678"
assoc_low.clientSAP = CLIENT_ADDR_LOW
assoc_low.objects = list(all_objects)
assoc_low.context = dlms.DLMSContext(
    maxSendPduSize=128,
    maxReceivePduSize=128,
    conformance=FULL_CONF,
)

assoc_high = dlms.AssociationLogicalName("0.0.40.0.3.255")
assoc_high.auth_mechanism = "High"
assoc_high.secret = b"12345678"
assoc_high.clientSAP = CLIENT_ADDR_HIGH
assoc_high.objects = list(all_objects)
assoc_high.context = dlms.DLMSContext(
    maxSendPduSize=128,
    maxReceivePduSize=128,
    conformance=FULL_CONF,
)

# ============================ 服务器 ============================
server = dlms.Server(serial_number=SERIAL_NUM, flag_id=FLAG_ID)
for obj in all_objects + [assoc_none, assoc_low, assoc_high]:
    server.add_object(obj)

generic_conn = dlms.GenericConnection(
    interface_type=dlms.InterfaceType.WRAPPER,  # TCP/IP WRAPPER
    frame_size=1024,
    pdu_size=512,
    tcp_udp_setup=tcp_udp,
    use_logical_name=True,
)

server.add_connection(generic_conn)
server.run()

print("[Server] DLMS server (NONE/LOW/HIGH) WRAPPER/TCP on port {}".format(TCP_PORT))
print("[Server] Server addr={}, password={}".format(SERVER_ADDRESS, PASSWORD))


# ============================ TCP 监听（单线程非阻塞轮询） ============================
_clients = []   # 每个元素: [sock, recv_buf(bytearray)]


def server_socket():
    """创建并绑定监听 socket（非阻塞）。支持 IPv4 / IPv6（由 USE_IPV6 决定）。"""
    if USE_IPV6:
        family   = socket.AF_INET6
        any_addr = "::"
        fam_name = "IPv6"
    else:
        family   = socket.AF_INET
        any_addr = "0.0.0.0"
        fam_name = "IPv4"
    srv = socket.socket(family, socket.SOCK_STREAM, socket.IPPROTO_TCP_SER)
    try:
        srv.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
    except Exception:
        pass
    srv.setblocking(False)
    # IPv6：直接监听所有接口 [::]；IPv4：先试 SERVER_IP，失败回退 0.0.0.0
    if USE_IPV6:
        srv.bind(("::", TCP_PORT))
    else:
        try:
            srv.bind((SERVER_IP, TCP_PORT))
        except Exception:
            try:
                srv.bind(("0.0.0.0", TCP_PORT))
            except Exception as e2:
                print("[Server] bind 0.0.0.0 failed: {}".format(e2))
                raise
    srv.listen(1)
    print("[Server] listening on {}:{} [{}]".format(
        "::" if USE_IPV6 else (SERVER_IP or "0.0.0.0"), TCP_PORT, fam_name))
    return srv


def pump_client(conn, entry):
    """非阻塞处理一个客户端连接：累积收帧 -> process_msg -> 回包。
    返回 False 表示连接已结束/出错，需要移除。"""
    sock, buf = entry
    try:
        while True:
            try:
                chunk = sock.recv(1024)
            except OSError:
                break   # 非阻塞：暂无数据
            if not chunk:
                print("[Server] client closed")
                sock.close()
                return False
            buf += chunk
            # 尝试解析出 1 个或多个完整 WRAPPER 帧
            while len(buf) >= 8:
                length = (buf[6] << 8) | buf[7]
                if length <= 0 or len(buf) < 8 + length:
                    break   # 帧未收齐，等待更多数据
                frame = bytes(buf[:8 + length])
                # QuecPython bytearray 不支持 del buf[:n]，用切片重建
                buf = bytearray(buf[8 + length:])
                entry[1] = buf
                resp = conn.process_msg(frame)
                if resp:
                    sock.send(resp)
    except Exception as e:
        print("[Server] client error: {}".format(e))
        sock.close()
        return False
    return True


def poll_once(conn, srv):
    """轮询一次：accept 新客户端 + 处理现有客户端的所有待收数据。"""
    try:
        # 注意：QuecPython 的 accept() 返回 3 元组 (sock, ip_str, port)，不是标准 2 元组！
        res = srv.accept()
        c = res[0]
        addr = res[1]
        c.setblocking(False)
        _clients.append([c, bytearray()])
        print("[Server] client connected: {}".format(addr))
    except OSError:
        pass   # 无新连接
    for entry in _clients[:]:
        if not pump_client(conn, entry):
            try:
                _clients.remove(entry)
            except Exception:
                pass


# 建立监听 socket（单线程轮询，不依赖 _thread 后台线程）
srv = server_socket()
generic_conn.connect()

# ============================ 主循环（TCP 轮询 + 采样） ============================
try:
    seq = 0
    while True:
        seq += 1
        poll_once(generic_conn, srv)         # 非阻塞处理 TCP 收发
        v = 228 + (seq % 7)                  # 模拟电压采样波动
        voltage.value = v
        if LOG_SAMPLE and seq % 10 == 0:     # 每 10 轮（约 2 秒）打印一次采样
            print("[Server] sample: voltage={} V, energy={}".format(v, energy.value))
        utime.sleep(0.2)                     # 短 sleep，让出 GIL 供 DLMS monitor 线程运行
except KeyboardInterrupt:
    server.stop()
    print("[Server] stopped")

```
客户端(NONE)
```python
# -*- coding: utf-8 -*-
"""
DLMS 客户端脚本（简洁版）- GenericConnection TCP/WRAPPER，NONE 认证
==================================================================
  * client_address = 0x10 -> 服务器 NONE 关联对象（clientSAP=0x10）
  * server_address = 144  -> 服务器地址（逻辑=1, 物理=16）
"""

import utime

try:
    import usocket as socket  # QuecPython / MicroPython
except ImportError:
    try:
        import socket  # 标准名兼容
    except ImportError:
        socket = None  # 无 socket 模块

import dlms

# ============================ 配置区 ============================
# IPv4/IPv6 开关（与服务器一致）
USE_IPV6    = True        # False = IPv4；True = IPv6
SERVER_IP   = "240E:452:DDAB:928D::1"   # 模组 A 当前 IPv6
TCP_PORT    = 4020             # 必须与服务器一致

CLIENT_ADDRESS = 0x10           # 对应服务器 NONE 关联对象 clientSAP
SERVER_ADDRESS = 144            # 电表（服务器）地址（逻辑=1, 物理=16）
HDLC_DEVICE_ADDR = 16

USE_LOGICAL_NAME = True
AUTHENTICATION   = dlms.Authentication.NONE   # NONE 认证：无密码
PASSWORD         = None
SECURITY         = 0x00
# ================================================================

LN_ENERGY  = "1.0.1.8.0.255"
LN_VOLT    = "1.0.32.7.0.255"
LN_CONFIG  = "1.0.25.1.0.255"
LN_PUBLIC  = "1.0.10.1.0.255"
LN_LOWREAD = "1.0.11.1.0.255"
LN_HIGHREAD = "1.0.12.1.0.255"


def build_tcp_setup():
    # 按 IP 版本创建正确的 IP 设置对象（IPv6 用 IPv6Setup，IPv4 用 IPv4Setup）
    if USE_IPV6:
        ip_setup = dlms.IPv6Setup(
            "0.0.25.7.0.255",
            address_config_mode=2,               # MANUAL
            unicast_ip_address=[SERVER_IP],      # 模组 A 的 IPv6（对象数据与实际一致）
        )
    else:
        ip_setup = dlms.IPv4Setup(
            "0.0.25.1.0.255",
            ip_address=SERVER_IP,
            subnet_mask="255.255.255.0",
            use_dhcp=False,
        )
    tcp_udp = dlms.TcpUdpSetup(
        "0.0.25.2.0.255",
        port=TCP_PORT,
        ip_reference=ip_setup,
        max_segment_size=1460,
        max_simultaneous_connections=1,
        inactivity_timeout=120,
    )
    return ip_setup, tcp_udp


def _recv(sock, timeout_ms):
    """直接 recv 一段，超时返回 None（无 TX/RX 回显）。"""
    try:
        sock.settimeout(max(0.001, float(timeout_ms) / 1000.0))
        return sock.recv(4096)
    except Exception:
        return None


def build_connection(sock, tcp_udp):
    conn = dlms.GenericConnection(
        interface_type=dlms.InterfaceType.WRAPPER,   # TCP/IP WRAPPER
        frame_size=1024,
        pdu_size=1024,
        tcp_udp_setup=tcp_udp,
        use_logical_name=USE_LOGICAL_NAME,
    )
    conn.on_send = lambda data: sock.send(data)
    conn.on_receive = lambda timeout_ms: _recv(sock, timeout_ms)
    return conn


def build_client():
    return dlms.Client(
        client_address=CLIENT_ADDRESS,
        server_address=SERVER_ADDRESS,
        authentication=AUTHENTICATION,
        password=PASSWORD,
        security=SECURITY,
        use_logical_name=USE_LOGICAL_NAME,
    )


def read_attr(client, ln, attr):
    try:
        val = client.read(ln, attr)
        print("[Read ] {} attr{} = {}".format(ln, attr, val))
        return val
    except Exception as e:
        print("[Read ] {} attr{} FAILED: {}".format(ln, attr, e))
        return None


def write_attr(client, ln, attr, value):
    try:
        client.write(ln, attr, value)
        print("[Write] {} attr{} = {} OK".format(ln, attr, value))
        return True
    except Exception as e:
        print("[Write] {} attr{} = {} FAILED: {}".format(ln, attr, value, e))
        return False


def run_once(client):
    read_attr(client, LN_VOLT, 2)
    read_attr(client, LN_ENERGY, 2)
    read_attr(client, LN_PUBLIC, 2)
    # 权限验证：LOW+ / HIGH 对象应被拒
    read_attr(client, LN_LOWREAD, 2)    # 期望 FAILED
    read_attr(client, LN_HIGHREAD, 2)   # 期望 FAILED
    # 写
    write_attr(client, LN_ENERGY, 2, 2222)         # OK
    write_attr(client, LN_CONFIG, 2, 9999)         # 期望 FAILED



def main():
    ipver = "IPv6" if USE_IPV6 else "IPv4"
    print("[Client] TCP {}:{} [{}] NONE auth, client=0x{:02X}, server={}".format(
        SERVER_IP, TCP_PORT, ipver, CLIENT_ADDRESS, SERVER_ADDRESS))

    if socket is None:
        print("[Client] socket unavailable on PC")
        return
    client = build_client()

    while True:
        try:
            fam = socket.AF_INET6 if USE_IPV6 else socket.AF_INET
            sock = socket.socket(fam, socket.SOCK_STREAM)
            # 用 getaddrinfo 解析出 QuecPython 认可的 sockaddr（IPv6 为 4 元组）
            addr = socket.getaddrinfo(SERVER_IP, TCP_PORT, fam)[0][-1]
            sock.connect(addr)
            _, tcp_udp = build_tcp_setup()
            conn = build_connection(sock, tcp_udp)
            client.connect(conn)     # WRAPPER 无 SNRM，直接 AARQ
            print("[Client] connected & associated! (NONE auth)")
            run_once(client)
            break
        except Exception as e:
            print("[Client] connect/run failed: {}".format(e))
            utime.sleep(3)

    try:
        while True:
            utime.sleep(10)
            read_attr(client, LN_VOLT, 2)
            read_attr(client, LN_ENERGY, 2)
    except KeyboardInterrupt:
        pass
    finally:
        try:
            client.disconnect()
        except Exception:
            pass


if __name__ == "__main__":
    main()

```
客户端(LOW)
```python
# -*- coding: utf-8 -*-
"""
DLMS 客户端脚本（简洁版）- GenericConnection TCP/WRAPPER，LOW 认证
==================================================================
  * client_address = 2    -> 服务器 LOW 关联对象（clientSAP=2）
  * server_address = 144  -> 服务器地址（逻辑=1, 物理=16）
  * LOW 认证：密码 "12345678"（AARQ 阶段比对，服务器 assoc_low.secret）
"""

import utime

try:
    import usocket as socket  # QuecPython / MicroPython
except ImportError:
    try:
        import socket  # 标准名兼容
    except ImportError:
        socket = None  # 无 socket 模块

import dlms

# ============================ 配置区 ============================
# IPv4/IPv6 开关（与服务器一致）：False = IPv4；True = IPv6
USE_IPV6    = True
SERVER_IP   = "240E:452:DEBE:566B::1"
TCP_PORT    = 4020              # 必须与服务器一致

CLIENT_ADDRESS = 2              # 对应服务器 LOW 关联对象 clientSAP
SERVER_ADDRESS = 144            # 电表（服务器）地址（逻辑=1, 物理=16）
HDLC_DEVICE_ADDR = 16

USE_LOGICAL_NAME = True
AUTHENTICATION   = dlms.Authentication.LOW    # LOW 认证：密码方式
PASSWORD         = "12345678"                 # 必须与服务器 assoc_low.secret 一致
SECURITY         = 0x00
# ================================================================

LN_ENERGY  = "1.0.1.8.0.255"
LN_VOLT    = "1.0.32.7.0.255"
LN_CONFIG  = "1.0.25.1.0.255"
LN_PUBLIC  = "1.0.10.1.0.255"
LN_LOWREAD = "1.0.11.1.0.255"
LN_HIGHREAD = "1.0.12.1.0.255"


def read_wrapper_frame(sock, timeout_ms):
    """读取一个完整 WRAPPER 帧；超时/断开返回 None。"""
    sock.settimeout(timeout_ms / 1000.0)
    try:
        hdr = b""
        while len(hdr) < 8:
            chunk = sock.recv(8 - len(hdr))
            if not chunk:
                return None
            hdr += chunk
        length = (hdr[6] << 8) | hdr[7]
        body = b""
        while len(body) < length:
            chunk = sock.recv(length - len(body))
            if not chunk:
                return None
            body += chunk
        return hdr + body
    except OSError:
        return None


def build_tcp_setup():
    # 按 IP 版本创建正确的 IP 设置对象（IPv6 用 IPv6Setup，IPv4 用 IPv4Setup）
    if USE_IPV6:
        ip_setup = dlms.IPv6Setup(
            "0.0.25.7.0.255",
            address_config_mode=2,               # MANUAL
            unicast_ip_address=[SERVER_IP],      # 模组 A 的 IPv6（对象数据与实际一致）
        )
    else:
        ip_setup = dlms.IPv4Setup(
            "0.0.25.1.0.255",
            ip_address=SERVER_IP,
            subnet_mask="255.255.255.0",
            use_dhcp=False,
        )
    tcp_udp = dlms.TcpUdpSetup(
        "0.0.25.2.0.255",
        port=TCP_PORT,
        ip_reference=ip_setup,
        max_segment_size=1460,
        max_simultaneous_connections=1,
        inactivity_timeout=120,
    )
    return ip_setup, tcp_udp


def _recv(sock, timeout_ms):
    """直接 recv 一段，超时返回 None（无 TX/RX 回显）。"""
    try:
        sock.settimeout(max(0.001, float(timeout_ms) / 1000.0))
        return sock.recv(4096)
    except Exception:
        return None


def build_connection(sock, tcp_udp):
    conn = dlms.GenericConnection(
        interface_type=dlms.InterfaceType.WRAPPER,   # TCP/IP WRAPPER
        frame_size=1024,
        pdu_size=1024,
        tcp_udp_setup=tcp_udp,
        use_logical_name=USE_LOGICAL_NAME,
    )
    conn.on_send = lambda data: sock.send(data)
    conn.on_receive = lambda timeout_ms: _recv(sock, timeout_ms)
    return conn


def build_client():
    return dlms.Client(
        client_address=CLIENT_ADDRESS,
        server_address=SERVER_ADDRESS,
        authentication=AUTHENTICATION,
        password=PASSWORD,
        security=SECURITY,
        use_logical_name=USE_LOGICAL_NAME,
    )


def read_attr(client, ln, attr):
    try:
        val = client.read(ln, attr)
        print("[Read ] {} attr{} = {}".format(ln, attr, val))
        return val
    except Exception as e:
        print("[Read ] {} attr{} FAILED: {}".format(ln, attr, e))
        return None


def write_attr(client, ln, attr, value):
    try:
        client.write(ln, attr, value)
        print("[Write] {} attr{} = {} OK".format(ln, attr, value))
        return True
    except Exception as e:
        print("[Write] {} attr{} = {} FAILED: {}".format(ln, attr, value, e))
        return False


def run_once(client):
    read_attr(client, LN_VOLT, 2)
    read_attr(client, LN_ENERGY, 2)
    read_attr(client, LN_PUBLIC, 2)
    read_attr(client, LN_LOWREAD, 2)    # LOW 可读
    # 权限验证：HIGH 对象应被拒
    read_attr(client, LN_HIGHREAD, 2)   # 期望 FAILED
    # 写
    write_attr(client, LN_ENERGY, 2, 3333)         # OK
    write_attr(client, LN_CONFIG, 2, 9999)         # 期望 FAILED


def main():
    print("[Client] TCP {}:{} LOW auth, client={}, server={}, pwd='{}'".format(
        SERVER_IP, TCP_PORT, CLIENT_ADDRESS, SERVER_ADDRESS, PASSWORD))

    if socket is None:
        print("[Client] socket unavailable on PC")
        return
    client = build_client()

    while True:
        try:
            fam = socket.AF_INET6 if USE_IPV6 else socket.AF_INET
            sock = socket.socket(fam, socket.SOCK_STREAM)
            # 用 getaddrinfo 解析出 QuecPython 认可的 sockaddr（IPv6 为 4 元组）
            addr = socket.getaddrinfo(SERVER_IP, TCP_PORT, fam)[0][-1]
            sock.connect(addr)
            _, tcp_udp = build_tcp_setup()
            conn = build_connection(sock, tcp_udp)
            client.connect(conn)     # WRAPPER 无 SNRM，直接 AARQ
            print("[Client] connected & associated! (LOW auth)")
            run_once(client)
            break
        except Exception as e:
            print("[Client] connect/run failed: {}".format(e))
            utime.sleep(3)

    try:
        while True:
            utime.sleep(10)
            read_attr(client, LN_VOLT, 2)
            read_attr(client, LN_ENERGY, 2)
    except KeyboardInterrupt:
        pass
    finally:
        try:
            client.disconnect()
        except Exception:
            pass


if __name__ == "__main__":
    main()

```
客户端(HIGH)
```python
# -*- coding: utf-8 -*-
"""
DLMS 客户端脚本（简洁版）- GenericConnection TCP/WRAPPER，HIGH 认证
==================================================================
  * client_address = 5    -> 服务器 HIGH 关联对象（clientSAP=5）
  * server_address = 144  -> 服务器地址（逻辑=1, 物理=16）
  * HIGH 认证：HLS 挑战-响应，密码 "12345678" 作为挑战密钥
"""

import utime

try:
    import usocket as socket  # QuecPython / MicroPython
except ImportError:
    try:
        import socket  # 标准名兼容
    except ImportError:
        socket = None  # 无 socket 模块

import dlms

# ============================ 配置区 ============================
# IPv4/IPv6 开关（与服务器一致）
USE_IPV6    = True        # False = IPv4；True = IPv6
SERVER_IP   = "240E:452:DEBE:566B::1"   # 模组 A 当前 IPv6
TCP_PORT    = 4020             # 必须与服务器一致

CLIENT_ADDRESS = 5              # 对应服务器 HIGH 关联对象 clientSAP
SERVER_ADDRESS = 144            # 电表（服务器）地址（逻辑=1, 物理=16）
HDLC_DEVICE_ADDR = 16

USE_LOGICAL_NAME = True
AUTHENTICATION   = dlms.Authentication.HIGH   # HIGH 认证：HLS 挑战-响应
PASSWORD         = "12345678"                 # 挑战密钥，必须与服务器 assoc_high.secret 一致
SECURITY         = 0x00
# ================================================================

LN_ENERGY  = "1.0.1.8.0.255"
LN_VOLT    = "1.0.32.7.0.255"
LN_CONFIG  = "1.0.25.1.0.255"
LN_PUBLIC  = "1.0.10.1.0.255"
LN_LOWREAD = "1.0.11.1.0.255"
LN_HIGHREAD = "1.0.12.1.0.255"


def read_wrapper_frame(sock, timeout_ms):
    """读取一个完整 WRAPPER 帧；超时/断开返回 None。"""
    sock.settimeout(timeout_ms / 1000.0)
    try:
        hdr = b""
        while len(hdr) < 8:
            chunk = sock.recv(8 - len(hdr))
            if not chunk:
                return None
            hdr += chunk
        length = (hdr[6] << 8) | hdr[7]
        body = b""
        while len(body) < length:
            chunk = sock.recv(length - len(body))
            if not chunk:
                return None
            body += chunk
        return hdr + body
    except OSError:
        return None


def build_tcp_setup():
    # 按 IP 版本创建正确的 IP 设置对象（IPv6 用 IPv6Setup，IPv4 用 IPv4Setup）
    if USE_IPV6:
        ip_setup = dlms.IPv6Setup(
            "0.0.25.7.0.255",
            address_config_mode=2,               # MANUAL
            unicast_ip_address=[SERVER_IP],      # 模组 A 的 IPv6（对象数据与实际一致）
        )
        print("[Client] TCP {}:{} [{}] HIGH auth, client={}, server={}, pwd='{}'".format(
            SERVER_IP, TCP_PORT, "IPv6", CLIENT_ADDRESS, SERVER_ADDRESS, PASSWORD))
    else:
        ip_setup = dlms.IPv4Setup(
            "0.0.25.1.0.255",
            ip_address=SERVER_IP,
            subnet_mask="255.255.255.0",
            use_dhcp=False,
        )
    tcp_udp = dlms.TcpUdpSetup(
        "0.0.25.2.0.255",
        port=TCP_PORT,
        ip_reference=ip_setup,
        max_segment_size=1460,
        max_simultaneous_connections=1,
        inactivity_timeout=120,
    )
    return ip_setup, tcp_udp


def _recv(sock, timeout_ms):
    """直接 recv 一段，超时返回 None（无 TX/RX 回显）。"""
    try:
        sock.settimeout(max(0.001, float(timeout_ms) / 1000.0))
        return sock.recv(4096)
    except Exception:
        return None


def build_connection(sock, tcp_udp):
    conn = dlms.GenericConnection(
        interface_type=dlms.InterfaceType.WRAPPER,   # TCP/IP WRAPPER
        frame_size=1024,
        pdu_size=1024,
        tcp_udp_setup=tcp_udp,
        use_logical_name=USE_LOGICAL_NAME,
    )
    conn.on_send = lambda data: sock.send(data)
    conn.on_receive = lambda timeout_ms: _recv(sock, timeout_ms)
    return conn


def build_client():
    return dlms.Client(
        client_address=CLIENT_ADDRESS,
        server_address=SERVER_ADDRESS,
        authentication=AUTHENTICATION,
        password=PASSWORD,
        security=SECURITY,
        use_logical_name=USE_LOGICAL_NAME,
    )


def read_attr(client, ln, attr):
    try:
        val = client.read(ln, attr)
        print("[Read ] {} attr{} = {}".format(ln, attr, val))
        return val
    except Exception as e:
        print("[Read ] {} attr{} FAILED: {}".format(ln, attr, e))
        return None


def write_attr(client, ln, attr, value):
    try:
        client.write(ln, attr, value)
        print("[Write] {} attr{} = {} OK".format(ln, attr, value))
        return True
    except Exception as e:
        print("[Write] {} attr{} = {} FAILED: {}".format(ln, attr, value, e))
        return False


def run_once(client):
    read_attr(client, LN_VOLT, 2)
    read_attr(client, LN_ENERGY, 2)
    read_attr(client, LN_PUBLIC, 2)
    read_attr(client, LN_LOWREAD, 2)
    read_attr(client, LN_HIGHREAD, 2)   # 仅 HIGH 可读
    read_attr(client, LN_CONFIG, 2)
    # 写
    write_attr(client, LN_ENERGY, 2, 4444)         # OK
    write_attr(client, LN_CONFIG, 2, 9999)         # OK（仅 HIGH 可写）
    read_attr(client, LN_CONFIG, 2)                # 回读验证


def main():
    ipver = "IPv6" if USE_IPV6 else "IPv4"
    print("[Client] TCP {}:{} [{}] HIGH auth, client={}, server={}, pwd='{}'".format(
        SERVER_IP, TCP_PORT, ipver, CLIENT_ADDRESS, SERVER_ADDRESS, PASSWORD))

    if socket is None:
        print("[Client] socket unavailable on PC")
        return
    client = build_client()

    while True:
        try:
            fam = socket.AF_INET6 if USE_IPV6 else socket.AF_INET
            sock = socket.socket(fam, socket.SOCK_STREAM)
            # 用 getaddrinfo 解析出 QuecPython 认可的 sockaddr（IPv6 为 4 元组）
            addr = socket.getaddrinfo(SERVER_IP, TCP_PORT, fam)[0][-1]
            sock.connect(addr)
            _, tcp_udp = build_tcp_setup()
            conn = build_connection(sock, tcp_udp)
            client.connect(conn)     # WRAPPER 无 SNRM，AARQ + HLS（自动）
            print("[Client] connected & associated! (HIGH auth)")
            run_once(client)
            break
        except Exception as e:
            print("[Client] connect/run failed: {}".format(e))
            utime.sleep(3)

    try:
        while True:
            utime.sleep(10)
            read_attr(client, LN_VOLT, 2)
            read_attr(client, LN_ENERGY, 2)
    except KeyboardInterrupt:
        pass
    finally:
        try:
            client.disconnect()
        except Exception:
            pass


if __name__ == "__main__":
    main()

```

### 连接生命周期回调

所有连接类型共享以下回调：

| 回调 | 触发时机 |
|------|----------|
| `on_connected` | 物理链路建立（HDLC 链接 up、UDP 套接字绑定、或 `GenericConnection.connect()` 调用） |
| `on_disconnected` | 链路断开（超时、传输错误、或 `disconnect()` 调用） |
| `on_send` | 原始字节即将发送（C 驱动连接上为监控钩子；`GenericConnection` 客户端模式需接入发送） |
| `on_receive` | 收到原始字节（功能同 `on_send`，方向相反） |

- C 驱动连接（`SerialConnection` / `OpticalConnection` / `MobileConnection`）的回调在 C 监听线程中触发
- `GenericConnection` 的回调在调用 `connect()` 的 Python 线程中触发
- 保持回调简短，避免阻塞操作

## 服务器配置（Server）

本章介绍如何将 COSEM 对象、安全关联对象、连接和 `Server` 实例组装成一个运行中的 DLMS 服务器。

---

### 示例

**最小骨架**

```python
import dlms
from dlms import Conformance
import utime

# ============================ 配置区 ============================
UART_PORT   = 2        # 模组 A 使用的 UART 口
BAUD        = 9600     # 波特率，必须与客户端一致
CLIENT_SAP  = 0x10     # 允许接入的客户端 SAP（None 认证关联）
FLAG_ID     = "GRX"    # 厂商代码（3 字符）
SERIAL_NUM  = 12345    # 电表序列号（≤5 位，Python 模式由 set_serial_number 设置）
# 服务器地址 = 序列号 % 10000 + 1000
#   Python 服务器模式：12345 -> 2345 + 1000 = 3345
#   （注意：不再是内置模式的 123456 -> 4456）
SERVER_ADDR = dlms.hdlc_server_address(SERIAL_NUM)   # = 3345，同时作为 IecHdlcSetup 设备地址
# ================================================================

# Python 服务器模式（CONFIG_DLMS_BUILTIN_SERVER 关闭）：events.c 生效，
# svr_isTarget 读取 SRV_SERIAL_NUMBER（由本调用设置）,
# 客户端 server_address 必须 == 该值 % 10000 + 1000。
dlms.set_serial_number(SERIAL_NUM)

# ============================ COSEM 对象 ============================
# 电能寄存器：属性2(value) 可读可写 —— 用于演示 client.write()
energy = dlms.Register(
    "1.0.1.8.0.255",
    default_value=12345,
    scaler=1,
    access={
        dlms.Authentication.NONE: {
            2: dlms.AccessMode.READ_WRITE,   # value：客户端可写
            3: dlms.AccessMode.READ,         # scaler/unit
        }
    }
)

# 电压寄存器：只读 —— 用于演示 client.read()
voltage = dlms.Register(
    "1.0.32.7.0.255",
    default_value=230,
    scaler=1,
    access={
        dlms.Authentication.NONE: {
            2: dlms.AccessMode.READ,
            3: dlms.AccessMode.READ,
        }
    }
)

# 扩展寄存器（ExtendedRegister，COSEM class 4）：
#   value(2) / scaler-unit(3) / status(4) / capture_time(5)
# 带状态与采集时间，常用于最大需量、费率电量等。value 可读写。
ext_energy = dlms.ExtendedRegister(
    "1.0.1.8.1.255",
    value=500,
    scaler=0,
    unit=dlms.Unit.ACTIVE_ENERGY,
    status=0,
    access={
        dlms.Authentication.NONE: {
            2: dlms.AccessMode.READ_WRITE,   # value：客户端可写
            3: dlms.AccessMode.READ,         # scaler/unit
            4: dlms.AccessMode.READ,         # status
            5: dlms.AccessMode.READ,         # capture_time
        }
    }
)
# 设置采集时间（6 元组：年/月/日/时/分/秒）
ext_energy.capture_time = (2026, 8, 19, 12, 0, 0)

# 参数对象：可读可写
param = dlms.Data(
    "0.0.1.1.0.255",
    access={dlms.Authentication.NONE: {2: dlms.AccessMode.READ_WRITE}}
)
param.value = 42

# 逻辑设备名 LDN：只读
ldn = dlms.Data(
    "0.0.42.0.0.255",
    access={dlms.Authentication.NONE: {2: dlms.AccessMode.READ}}
)
ldn.value = b"SN12345"

# ------------------ HDLC 链路层配置 ------------------
hdlc = dlms.IecHdlcSetup(
    "0.0.22.0.0.255",
    commSpeed=BAUD,          # 直接写实际波特率数值
    windowSizeRx=1,
    windowSizeTx=1,
    maxInfoLenTx=128,
    maxInfoLenRx=128,
    timeout=120,             # 空闲超时（秒）
    deviceAddr=SERVER_ADDR,  # 与客户端 server_address 推导规则一致
)

# ------------------ 关联对象（None 认证） ------------------
assoc = dlms.AssociationLogicalName("0.0.40.0.1.255")
assoc.auth_mechanism = "None"
assoc.clientSAP = CLIENT_SAP
assoc.objects = [energy, voltage, param, ldn, hdlc, ext_energy]
assoc.context = dlms.DLMSContext(
    maxSendPduSize=128,
    maxReceivePduSize=128,
    # 包含 SET，客户端才能写入；若只读可去掉 SET
    conformance=Conformance.GET | Conformance.SET,
)

# ============================ 服务器 ============================
server = dlms.Server(serial_number=SERIAL_NUM, flag_id=FLAG_ID)
for obj in [energy, voltage, param, ldn, hdlc, ext_energy, assoc]:
    server.add_object(obj)

# SerialConnection：C 驱动 UART，服务器端自动监听并回包
serial_conn = dlms.SerialConnection(
    uart_port=UART_PORT,
    hdlc_setup=hdlc,
    flowcontrol=0,
    interface_type=dlms.InterfaceType.HDLC,  # 必须 HDLC（与客户端一致）
    use_logical_name=True,
)

server.add_connection(serial_conn)
server.run()   # 非阻塞，连接在后台线程中监听
print("[Server] DLMS server running on UART{} @ {} baud".format(UART_PORT, BAUD))
print("[Server] Serial num={}, Server addr={} (serial%10000+1000), Client SAP=0x{:02X}".format(
    SERIAL_NUM, SERVER_ADDR, CLIENT_SAP))
print("[Server] Objects: energy(1.0.1.8.0.255) voltage(1.0.32.7.0.255) param(0.0.1.1.0.255)")

# ============================ 主循环 ============================
# 模拟电表周期采集，更新寄存器值（客户端可读到变化的电压）
try:
    seq = 0
    while True:
        seq += 1
        v = 228 + (seq % 7)          # 230/229/... 波动，模拟真实采样
        voltage.value = v
        print("[Server] sample: voltage={} V".format(v))
        utime.sleep(5)
except KeyboardInterrupt:
    server.stop()
    print("[Server] stopped")


```

---

### 将对象组织到模块中

按子系统分组（能源、安全、网络、预付费等），便于条件编译。若某个板型不支持预付费，只需省略 `setup_prepayment_objects()` 调用，相关对象永远不会进入 `add_object()`。启动序列的其他部分无需改动。

---

### 多连接

服务器最多支持 **8 个** 同时连接，每个在独立线程中运行。SN 和 LN 关联可在同一服务器上共存：

- 一个 `SerialConnection` 使用 `use_logical_name=True` 服务 LN 客户端
- 另一个 `SerialConnection` 使用 `use_logical_name=False`（不同 UART 端口）服务 SN 客户端
- 一个 `MobileConnection` 可与两者同时运行

每个连接独占其 UART。两个 `SerialConnection`（或 `GenericConnection`）实例**不能共享**同一 UART 端口号。

---

### 事件代码和事件日志

`server.set_event_code()` 将 C 层的内部事件代码跟踪连接到两个 Python 对象，使 C 层检测到的事件代码变化自动反映到对象中，无需 Python 侧轮询。

```python
import dlms

event_code = dlms.Data("0.0.96.11.0.255")
event_code.value = 0

event_log = dlms.ProfileGeneric(
    "0.0.99.98.0.255",
    capture_objects=[(clock, 2, 0), (event_code, 2, 0)],
    profile_entries=200,
)

server.set_event_code(event_code, event_log)
```

启动后，写入 `event_code.value` 且值发生变化时，服务器自动向 `event_log` 捕获一行：

```python
event_code.value = 255  # "power fail" — 自动捕获到 event_log
```

`server.set_event_code()` **必须**在 `server.run()` 之前调用，之后调用无效。

---

### RegisterMonitor 与后台监控

`server.run()` 启动后，服务器创建一个后台监控线程，每秒唤醒一次检查所有 `RegisterMonitor` 阈值。阈值被触发时执行对应的 `ScriptTable` 动作。

若应用更新了受监控寄存器的值并希望立即检查阈值（无需等待最多 1 秒），可调用 `server.monitor()`：

```python
energy_reg.value = read_energy_sensor()
server.monitor()  # 立即检查；若阈值被触发则执行 ScriptTable 动作
```

- `server.monitor()` 返回已检查的连接数
- 若监控返回错误则抛出 `RuntimeError`
- 可从主应用线程安全调用，与后台监控线程并发执行

## 客户端（Client）

`dlms.Client` 提供同步接口，用于读取属性、写入属性和调用远程 DLMS/COSEM 服务器的方法。支持与服务器端相同的所有连接类型：串口、光口和蜂窝。所有客户端方法在通信失败时抛出 `RuntimeError`，生产代码应将调用包裹在 `try/except` 中。

---

### 客户端地址和服务端地址

DLMS 会话由一对地址标识：**客户端地址** 和 **服务端地址**。

**客户端地址** 是 DLMS SAP，必须与目标服务器 `AssociationLogicalName` 上配置的 `clientSAP` 一致。常用约定值：

| 值 | 对应关联 |
|:--:|----------|
| `16` (`0x10`) | 公开（无认证）关联 |
| `18` (`0x12`) | High（挑战-响应）认证关联 |
| `1` | HighGMac（AES-GCM）关联 |

任何一致的值均可；上述仅匹配常见 DLMS 测试工具的默认值。

**服务端地址** 由设备序列号派生。HDLC 连接（串口和光口）的计算公式为 `serial % 10000 + 1000`。辅助函数 `dlms.hdlc_server_address(serial)` 执行此计算：

```python
import dlms

addr = dlms.hdlc_server_address(12345)  # 序列号 12345 → 服务端地址 3345
```

蜂窝中继连接同样适用此公式：中继服务器使用帧中嵌入的 HDLC 服务端地址将数据包路由到正确的设备。

---

### 构造客户端

`Client` 构造函数参数：

| 参数 | 类型 | 默认值 | 说明 |
|------|------|--------|------|
| `client_address` | `int` | `16` | DLMS SAP |
| `server_address` | `int` | `1` | HDLC 服务端地址 |
| `authentication` | `int` | `Authentication.NONE` | 认证级别常量 |
| `password` | `str` / `None` | `None` | Low 或 High 认证的密码；HighGMac 不需要 |
| `use_logical_name` | `bool` | `True` | `True`=LN 引用, `False`=SN 引用 |
| `system_title` | `bytes` / `None` | `None` | 客户端 8 字节系统标题（HighGMac 必需） |
| `authentication_key` | `bytes` / `None` | `None` | GAK（HighGMac 必需） |
| `block_cipher_key` | `bytes` / `None` | `None` | GUEK（HighGMac 必需） |
| `security` | `int` | `0` | `SecurityPolicy` 值（HighGMac 必需） |

**公开（无认证）客户端**

```python
import dlms

client = dlms.Client(
    client_address=16,
    server_address=dlms.hdlc_server_address(12345),
)
```

**HighGMac 客户端**

```python
import dlms

client = dlms.Client(
    client_address=1,
    server_address=dlms.hdlc_server_address(12345),
    authentication=dlms.Authentication.HIGH_GMAC,
    system_title=b'GRX00001',
    authentication_key=b'\xD0\xD1\xD2\xD3\xD4\xD5\xD6\xD7\xD8\xD9\xDA\xDB\xDC\xDD\xDE\xDF',
    block_cipher_key=b'\x00\x01\x02\x03\x04\x05\x06\x07\x08\x09\x0A\x0B\x0C\x0D\x0E\x0F',
    security=dlms.SecurityPolicy.AUTHENTICATED_ENCRYPTED,
)
```

---

### 客户端最小骨架示例


```python
import dlms
import utime

# ============================ 配置区 ============================
UART_PORT = 2        # 模组 B 使用的 UART 口
BAUD      = 9600     # 波特率，必须与服务器端一致

# 服务器端序列号 —— 必须与服务器脚本中的 SERIAL_NUM 一致！
# 当前配置为 Python 服务器模式（CONFIG_DLMS_BUILTIN_SERVER 关闭）,
# 服务器地址由 dlms.set_serial_number(12345) 设置：
#   server_address = 12345 % 10000 + 1000 = 3345
SERVER_SERIAL = 12345

CLIENT_ADDR = 0x10   # 16，必须匹配服务器关联对象 clientSAP
# 服务器地址 = 序列号 % 10000 + 1000
#   Python 服务器 (CONFIG_DLMS_BUILTIN_SERVER 关闭，当前):
#     12345 -> 2345 + 1000 = 3345   ← 用这个
#   内置 C 服务器 (CONFIG_DLMS_BUILTIN_SERVER=y):
#     123456 -> 3456 + 1000 = 4456  （C 硬编码，Python 不可改）
SERVER_ADDR = dlms.hdlc_server_address(SERVER_SERIAL)
AUTH        = dlms.Authentication.NONE

# 演示用对象的逻辑名
LN_ENERGY  = "1.0.1.8.0.255"    # 电能寄存器（可写）
LN_VOLTAGE = "1.0.32.7.0.255"   # 电压寄存器（只读）
LN_PARAM   = "0.0.1.1.0.255"    # 参数 Data（可写）
LN_ext_energy = "1.0.1.8.1.255"
# ================================================================

# ------------------ HDLC 链路层配置（必须与服务器一致） ------------------
hdlc = dlms.IecHdlcSetup(
    "0.0.22.0.0.255",
    commSpeed=BAUD,
    windowSizeRx=1,
    windowSizeTx=1,
    maxInfoLenTx=128,
    maxInfoLenRx=128,
    timeout=120,
    deviceAddr=0x10,
)

serial_conn = dlms.SerialConnection(
    uart_port=UART_PORT,
    hdlc_setup=hdlc,
    flowcontrol=0,
    interface_type=dlms.InterfaceType.HDLC,  # 必须 HDLC
    use_logical_name=True,
)

client = dlms.Client(
    client_address=CLIENT_ADDR,
    server_address=SERVER_ADDR,
    authentication=AUTH,
    use_logical_name=True,
)


def read_attr(ln, attr):
    """读取单个属性，失败返回 None 并打印错误。"""
    try:
        val = client.read(ln, attr)
        print("[Read ] {} attr{} = {}".format(ln, attr, val))
        return val
    except Exception as e:
        print("[Read ] {} attr{} FAILED: {}".format(ln, attr, e))
        return None


def write_attr(ln, attr, value):
    """写入单个属性，返回是否成功。"""
    try:
        client.write(ln, attr, value)
        print("[Write] {} attr{} = {} OK".format(ln, attr, value))
        return True
    except Exception as e:
        print("[Write] {} attr{} = {} FAILED: {}".format(ln, attr, value, e))
        return False


def run_once():
    """单轮测试：读取所有对象 -> 写电能 -> 回读验证。"""
    print("-" * 50)

    # 1) 读取（简单模式：逻辑名 + 属性号）
    voltage = read_attr(LN_VOLTAGE, 2)      # 电压寄存器值
    read_attr(LN_ENERGY, 2)                 # 电能寄存器值
    read_attr(LN_ENERGY, 3)                 # scaler/unit
    read_attr(LN_PARAM, 2)                  # 参数

    # 2) 写入电能寄存器（属性2 value）
    new_energy = (voltage or 0) + 1000      # 演示：基于读到的电压拼一个值
    write_attr(LN_ENERGY, 2, new_energy)
    write_attr(LN_ext_energy, 2, new_energy)
    # 3) 写参数对象
    write_attr(LN_PARAM, 2, 100)

    # 4) 回读验证
    read_attr(LN_ENERGY, 2)
    read_attr(LN_PARAM, 2)


def main():
    print("[Client] Connecting to server via UART{} @ {} baud ...".format(UART_PORT, BAUD))
    print("[Client] client_addr=0x{:02X}, server_addr={}".format(CLIENT_ADDR, SERVER_ADDR))

    while True:
        try:
            client.connect(serial_conn)     # 阻塞直到关联（AARQ/AARE）完成
            print("[Client] connected & associated!")
            run_once()
            break
        except Exception as e:
            print("[Client] connect/run failed: {}".format(e))
            utime.sleep(3)

    try:
        while True:
            utime.sleep(10)
            read_attr(LN_VOLTAGE, 2)        # 周期性读取服务器采样的电压
            read_attr(LN_ENERGY, 2)
    except KeyboardInterrupt:
        pass
    finally:
        try:
            client.disconnect()
            print("[Client] disconnected")
        except Exception:
            pass


main()

```

**会话状态属性**

| 属性 | 说明 |
|------|------|
| `client.connected` | 物理传输打开后为 `True` |
| `client.associated` | AARQ/AARE 握手成功后为 `True`，DLMS 会话激活 |
| `client.currentAssociation` | 当前活动会话的 `AssociationLogicalName` 对象；未连接时为 `None`，可用于检查协商的一致性块和 PDU 大小 |

`client.disconnect()` 发送 RLRQ（释放请求），等待 RLRE，然后关闭物理传输。

---

### 读写操作

所有读写方法均为同步：阻塞直到服务器响应或抛出 `RuntimeError`。

#### 单属性读取

```python
import dlms

clock = dlms.Clock("0.0.1.0.0.255")
t = client.read(clock, clock.idx('time'))  # 读取属性 2 (time)
print("Time: {}-{:02d}-{:02d} {:02d}:{:02d}:{:02d}".format(*t))
```

使用 `obj.idx('attr_name')` 代替裸整数，使意图清晰易懂。

#### 批量读取

传递 `None` 作为属性索引，在一次请求中读取所有持久属性：

```python
reg = dlms.Register("1.0.1.8.0.255", 0, scaler=-3)
client.read(reg, None)  # 读取 value 和 scaler
print("Energy: {} Wh".format(reg.value * (10 ** reg.scaler)))
```

#### 单属性写入

```python
client.write(clock, clock.idx('time_zone'), 60)  # 设置 UTC+1
```

#### 批量写入

传递 `None` 将所有当前存储在对象上的属性写回服务器：

```python
clock.time_zone = 120
client.write(clock, None)
```

#### 方法调用

```python
client.method(disconnect_ctl, 1)         # 调用 remote_disconnect（方法 1）
client.method(clock, 6, 30)              # shift_time(30 秒)
```

#### 多属性批量读取

`read_multiple` 发送单个 Get-Request-With-List，按输入顺序返回值列表：

```python
objs = [(clock, clock.idx('time')), (reg, reg.idx('value'))]
vals = client.read_multiple(objs)
print("Time: {}, Energy: {}".format(vals[0], vals[1]))
```

---

### 读取负荷曲线

`client.read_profile(profile, start_index, count)` 从 `ProfileGeneric` 对象读取缓冲行。返回值是行列表，每行本身是一个列表，元素与 `capture_objects` 声明匹配。

```python
import dlms

profile = dlms.ProfileGeneric(
    "1.0.99.1.0.255",
    capture_objects=[(clock, 2, 0), (reg, 2, 0)],
)

# 读取第 1–10 行
rows = client.read_profile(profile, start_index=1, count=10)
for row in rows:
    timestamp, energy = row[0], row[1]
    print("{}: {} Wh".format(timestamp, energy))

# 读取所有可用行
all_rows = client.read_profile(profile, start_index=1, count=0)
```

---

### 关联视图发现

当目标服务器未知或其对象列表可能变化时（如调试或互操作性测试），客户端可查询服务器的关联视图，而非本地构造对象。

#### `get_object_info()`

获取关联视图，返回 `(class_id, version, logical_name_str)` 元组列表：

```python
info = client.get_object_info()
for class_id, version, ln in info:
    print("class={:3d}  ver={}  ln={}".format(class_id, version, ln))
```

#### `get_objects()`

执行关联视图查询，返回活的、带类型的 DLMS 对象列表。`dlms` 模块中未实现的类 ID 会被静默跳过。

```python
objects = client.get_objects()
print("Association contains {} objects".format(len(objects)))

# 找到第一个 Clock 并填充
for obj in objects:
    if isinstance(obj, dlms.Clock):
        client.read(obj)  # 批量读取所有持久属性
        print("Clock {}  time={}".format(obj.logical_name, obj.time))
        break
```

`get_objects()` 执行一次网络往返，应在每个会话中调用一次并复用结果。

---

### 蜂窝中继客户端

当服务器在 CGNAT 后使用 `relay_tcp_setup` 时，客户端也通过中继连接。中继根据帧中嵌入的 HDLC 服务端地址识别目标设备，因此客户端侧无需特殊配置，只需使用正确的服务端地址：

```python
import dlms

relay_tcp = dlms.TcpUdpSetup("0.0.25.0.0.254", port=4060)
relay_tcp.ip_reference = ipv4_setup  # ipv4_setup.ipAddress = 中继服务器 IP

mobile = dlms.MobileConnection(
    tcp_udp_setup=dlms.TcpUdpSetup("0.0.25.0.0.255", port=4059),
    gprs_setup=gprs,
    gsm_diag=gsm,
    recv_buffer=bytearray(4096),
    relay_tcp_setup=relay_tcp,
)

client = dlms.Client(
    client_address=16,
    server_address=dlms.hdlc_server_address(12345),
)
client.connect(mobile)
value = client.read(reg, reg.idx('value'))
client.disconnect()
```

中继使用公式 `serial % 10000 + 1000` 将帧映射到已注册的设备。两个序列号在取模 10000 后结果相同的设备会发生冲突；分配序列号时应避免此情况。

## 持久化（Persistence）

嵌入式设备上的 DLMS 服务器必须在断电重启后保持对象状态。`dlms` 模块提供 `BinarySerializer` 作为主要持久化机制，同时还有 `JsonSerializer` 和可自定义的 `Serializer` 基类。

---

### BinarySerializer

每个 DLMS 对象保存为独立二进制文件，文件名来自 OBIS 代码（如 `1.1.33.25.0.255.bin`）。目录必须预先创建。

```python
import dlms

ser = dlms.BinarySerializer("/usr/dlms")
```

**核心方法**

| 方法 | 说明 |
|------|------|
| `ser.save_all(server)` | 序列化服务器上所有已注册的对象 |
| `ser.load_all(server)` | 恢复所有对象；`.bin` 文件不存在时静默跳过（首次启动安全） |
| `ser.save(obj)` | 保存单个对象 |
| `ser.load(obj)` | 恢复单个对象 |
| `ser.get_size(server)` | 返回已保存文件占用的总字节数（不修改文件） |

**典型启动流程**

```python
import dlms

# 创建对象
reg   = dlms.Register("1.0.1.8.0.255", 0, scaler=-3)
clock = dlms.Clock("0.0.1.0.0.255")

server = dlms.Server(serial_number=12345, flag_id="GRX")
server.add_object(reg)
server.add_object(clock)

# 恢复持久化状态（首次启动无效果）
ser = dlms.BinarySerializer("/usr/dlms")
ser.load_all(server)

# 启动服务器
dlms.set_serial_number(12345)
dlms.set_flag_id("GRX")
server.run()
```

---

### 序列化规则

`BinarySerializer` 根据属性标志决定序列化内容。可以用 `obj.attrs()` 查看实际会被保存的属性：

```python
reg = dlms.Register("1.0.1.8.0.255", 0)
for index, name in reg.attrs():
    print("attr {}: {}".format(index, name))
```

**自动排除的标志：**

| 标志 | 说明 |
|------|------|
| `AttributeFlag.VOLATILE` | 持续变化的属性（如 `Clock.time`、`GsmDiagnostic.status`），自动排除 |
| `AttributeFlag.COMPLEX` | 结构化属性（如 `ProfileGeneric.buffer`），自动排除，需自定持久化 |
| 只读属性 | 如 Unit/Scaler，同样自动排除 |

---

### 忽略特定属性

`ser.ignore()` 可从序列化中排除特定属性。可以按类排除（所有实例）或按实例排除。

```python
import dlms

ser = dlms.BinarySerializer("/usr/dlms")

# 对所有 ProfileGeneric 对象跳过 buffer 属性（属性 2）
ser.ignore(dlms.ProfileGeneric, 2)

# 对特定 Register 跳过 scaler/unit（属性 3）
ser.ignore(my_special_reg, 3)
```

---

### ProfileGeneric 缓冲区持久化

`ProfileGeneric.buffer` 因标记为 COMPLEX 被自动排除。推荐模式：新行捕获时追加到文件，而非一次性序列化整个缓冲区。

```python
import dlms
import utime

BUFFER_PATH = "/usr/dlms/1.0.99.1.0.255.buf"

def _encode_row(ts, energy):
    return "{},{}\n".format(ts, energy).encode()

def on_capture(profile, event):
    if event.index == 2:  # capture
        with open(BUFFER_PATH, "ab") as f:
            f.write(_encode_row(utime.time(), energy_reg.value))
    return True
```

启动时按行计数恢复 `entries_in_use`：

```python
def _count_entries(path):
    count = 0
    try:
        with open(path, "rb") as f:
            while f.readline():
                count += 1
    except OSError:
        pass  # 文件尚不存在
    return count
```

> **注意**：切勿将大缓冲区文件整个加载到 Python 列表中计数或遍历。应逐行读取，否则可能超出堆内存。

---

### JsonSerializer

`JsonSerializer` 定义在 `dlms_serializer.py` 中，使用 JSON 格式保存，便于调试查看。

```python
from dlms_serializer import JsonSerializer

ser = JsonSerializer("/usr/dlms")
ser.save_all(server)
```

**限制：** 二进制属性（安全密钥、OCTET STRING 等）无法在标准 JSON 中无损表示。生产环境使用 `BinarySerializer`，`JsonSerializer` 仅限开发和测试。

---

### 自定义序列化器

可继承 `Serializer` 基类实现自定义后端（EEPROM、SD 卡等）。

```python
from dlms_serializer import Serializer

class EepromSerializer(Serializer):
    def save_all(self, server):
        for obj in server.object_registry:
            self.save(obj)

    def load_all(self, server):
        for obj in server.object_registry:
            self.load(obj)

    def save(self, obj):
        for index, name in obj.attrs():
            if self._is_ignored(obj, index):
                continue
            value = getattr(obj, name)
            self._dev.write(obj.logical_name, index, value)

    def load(self, obj):
        for index, name in obj.attrs():
            if self._is_ignored(obj, index):
                continue
            value = self._dev.read(obj.logical_name, index)
            if value is not None:
                setattr(obj, name, value)

    def _is_ignored(self, obj, index):
        for target, attr in self._ignored:
            if attr == index and (target is type(obj) or target is obj):
                return True
        return False
```

---

### 文件系统路径与存储

| 路径 | 说明 |
|------|------|
| `/usr/` | 内部闪存，始终可用，适用于配置和小型对象状态 |
| `/bak/` | 备份分区，由 FOTA/OTA 工具链管理，**不可写入** |
| `/ext/` | SPI NOR 闪存（需硬件支持并通过 QPyCOM 启用） |
| `/sd/` | SD 卡，运行时挂载 |

目录需在首次保存前创建：

```python
import uos

def _ensure_dirs():
    for path in ("/usr/dlms",):
        try:
            uos.mkdir(path)
        except OSError:
            pass  # 已存在

_ensure_dirs()
```

#### 挂载 SD 卡（SPI，EC600N/EC800N 系列）

```python
import uos

cdev = uos.VfsFat(1, 0, 4, 1)  # SPI port 1, mode 0, 13 MHz, CS=GPIO1
uos.mount(cdev, '/sd')

with open('/sd/test.txt', 'w+') as f:
    f.write('hello')
uos.listdir('/sd')
```

#### 挂载 SD 卡（SDIO，EC600U/EC200U/EC200A 系列）

```python
from uos import VfsSd
import uos

udev = VfsSd("sd_fs")
uos.mount(udev, '/sd')
udev.set_det(udev.GPIO10, 0)  # 可选：配置卡检测引脚

with open('/sd/dlms/lp.jsonl', 'a') as f:
    f.write('[1714000000,12345]\n')
```

> **容量警告：** `/usr/` 是内部闪存文件系统，与所有应用文件和固件脚本共享。保存大量 COSEM 对象（尤其是 ProfileGeneric 日志）可能耗尽空间。建议使用外部存储。

## DLMS UDP 路由器（Router）

用途与 CGNAT 穿透

蜂窝模块由运营商 CGNAT 分配私有 IP 地址，头端系统无法向设备的 IP 发起入站连接。DLMS UDP 路由器作为一个公网 IP 的会合点解决此问题：

- 设备向路由器发起**出站** UDP 连接并注册自身
- 头端系统连接到路由器的独立端口
- 路由器检查帧中嵌入的 HDLC 目标地址，在头端和设备间转发数据报

---

### 架构和地址映射

路由器监听两个独立的 UDP 套接字：

| 套接字 | 默认端口 | 连接方 | 用途 |
|--------|:--------:|--------|------|
| Board socket | `4059` | DLMS 服务器设备 | 设备注册、设备→客户端回复 |
| Client socket | `4060` | 头端/DLMS 客户端 | 客户端→设备请求 |

**设备注册：** 设备发送 `BOARD:<serial>\n` 作为第一个 UDP 数据报到端口 4059。例如设备序列号 `METER-12345` 发送：

```
BOARD:METER-12345\n
```

路由器从序列号中提取数字部分（去掉非数字字符），然后计算 HDLC 目标地址：

```
address = (numeric_serial % 10000) + 1000
```

`METER-12345` 的数字部分为 `12345`，因此 `(12345 % 10000) + 1000 = 3345`。有效地址范围 1000–10999。

**路由客户端数据报：** 数据报到 4060 端口时，路由器解析帧头中的 HDLC 目标地址，查找已注册的匹配设备，转发数据报到该设备的 UDP 端点。

**地址冲突：** 两个序列号模 10000 结果相同的设备（如 `12345` 和 `22345`）会被分配相同 HDLC 地址。路由器会记录警告并将流量转发给最近注册的设备。确保所有部署的序列号后四位唯一。

---

### 运行路由器

路由器是独立的 Python 3.8+ 包，位于 `dlms_udp_router/`。

```bash
# 本地运行
cd dlms_udp_router
python -m pip install .
dlms-udp-router --host 0.0.0.0 --board-port 4059 --client-port 4060 --log-level INFO

# Docker
cd dlms_udp_router/docker
docker compose up --build

# systemd（Linux 服务器）
sudo cp dlms_udp_router/etc/dlms_udp_router.service /etc/systemd/system/
sudo systemctl daemon-reload
sudo systemctl enable --now dlms_udp_router.service
```

**环境变量配置**

| 变量 | 默认值 | 说明 |
|------|--------|------|
| `ROUTER_HOST` | `0.0.0.0` | 绑定地址 |
| `ROUTER_BOARD_PORT` | `4059` | 设备注册 UDP 端口 |
| `ROUTER_CLIENT_PORT` | `4060` | DLMS 客户端 UDP 端口 |
| `ROUTER_LOG_LEVEL` | `INFO` | 日志级别 |
| `ROUTER_SYSLOG_HOST` | (无) | 远程 syslog 主机(UDP) |
| `ROUTER_SYSLOG_PORT` | (无) | 远程 syslog 端口 |

---

### 设备侧配置

设备通过 `MobileConnection` 的 `relay_tcp_setup` 参数向路由器注册。`relay_tcp_setup` 指向路由器的 board 端口（4059）。

```python
import dlms

SERIAL    = 12345678
RELAY_IP  = "203.0.113.10"  # 路由器服务器公网 IP
RELAY_PORT = 4059            # 设备注册端口

tcp_udp = dlms.TcpUdpSetup("0.0.25.0.0.255", port=4059)

relay_setup = dlms.TcpUdpSetup("0.0.25.0.0.254", port=RELAY_PORT)
relay_setup.ipReference = ipv4_obj  # ipv4_obj.ipAddress = RELAY_IP

gprs     = dlms.GprsSetup("0.0.2.0.0.255")
gsm      = dlms.GsmDiagnostic("0.0.25.6.0.255")
recv_buf = bytearray(4096)

mobile = dlms.MobileConnection(
    tcp_udp_setup=tcp_udp,
    gprs_setup=gprs,
    gsm_diag=gsm,
    recv_buffer=recv_buf,
    relay_tcp_setup=relay_setup,
)
mobile.on_connected    = lambda: print("relay: connected")
mobile.on_disconnected = lambda: print("relay: disconnected")
server.add_connection(mobile)
```

`server.run()` 启动后，运行时在每次（重）连接时自动发送 `BOARD:<serial>\n` 注册数据报。

**主动推送**

中继模式下，设备可主动推送帧到当前连接的客户端：

```python
mobile.send(raw_dlms_frame_bytes)
```

用于 `PushSetup` 通知。若中继连接未激活则抛出 `RuntimeError`。

---

### 客户端侧使用

路由器对标准 DLMS/HDLC 流量透明。客户端发送正常 HDLC 请求帧到路由器的 client 端口（4060）；路由器读取帧中的 HDLC 目标地址并转发给匹配的设备。

#### 计算目标 HDLC 地址

```python
import dlms

BOARD_SERIAL = 12345678
server_address = dlms.hdlc_server_address(BOARD_SERIAL % 10000)
```

#### QuecPython 客户端通过中继连接

```python
import dlms

BOARD_SERIAL = 12345678
RELAY_IP     = "203.0.113.10"
CLIENT_PORT  = 4060

tcp_udp = dlms.TcpUdpSetup("0.0.25.0.0.255", port=4060)
relay   = dlms.TcpUdpSetup("0.0.25.0.0.254", port=CLIENT_PORT)
relay.ipReference = ipv4_obj  # ipv4_obj.ipAddress = RELAY_IP

gprs     = dlms.GprsSetup("0.0.2.0.0.255")
gsm      = dlms.GsmDiagnostic("0.0.25.6.0.255")
recv_buf = bytearray(4096)

mobile = dlms.MobileConnection(
    tcp_udp_setup=tcp_udp,
    gprs_setup=gprs,
    gsm_diag=gsm,
    recv_buffer=recv_buf,
    relay_tcp_setup=relay,
)

client = dlms.Client(
    client_address=16,
    server_address=dlms.hdlc_server_address(BOARD_SERIAL % 10000),
    conn=mobile,
)
```

#### PC/头端客户端

任何标准 DLMS 客户端库（如 Gurux DLMS.Net、gurux-dlms-python）均可与中继配合使用。将传输指向路由器的公网 IP 和 client 端口（4060）。出站帧中的 HDLC 目标地址必须等于 `(BOARD_SERIAL % 10000) + 1000`。

> **防火墙注意：** 确保路由器服务器上的 UDP 端口 4059 和 4060 已开放，入站（来自设备和客户端）和出站（回复）流量均需允许。


## 最佳实践（Best Practices）

---

### 内存管理

内存是 QuecPython 开发板上最受限的资源，通常可用堆仅 200–400 KB。

**预分配接收缓冲区。** `MobileConnection` 需要构造时传入 `recv_buffer` bytearray。一次性分配，切勿重复创建：

```python
recv_buf = bytearray(4096)  # 在模块级别分配，任何线程启动之前
mobile = dlms.MobileConnection(
    tcp_udp_setup=tcp_udp,
    gprs_setup=gprs,
    gsm_diag=gsm,
    recv_buffer=recv_buf,
)
```

避免在回调或循环内分配 `bytearray` 对象，重复分配会使堆碎片化，长期运行可能导致 `MemoryError`。

**对持有 C 端定时器的对象调用 `deinit()`。** 以下类分配了后台资源，在服务器关闭或对象不再需要时必须显式释放：`ProfileGeneric`、`PushSetup`、`ActivityCalendar`、`ScriptTable`、`SingleActionSchedule`。不调用 `deinit()` 会泄漏定时器并阻止干净重启。

**使用 `BinarySerializer` 持久化状态**，而非内存结构。大型字典、历史读数列表或完整 `ProfileGeneric` 缓冲区应保存在文件系统上，而非 Python 堆中。按需读取请求所需的数据（参见第 9.4 节的逐行模式）。

**切勿将完整捕获缓冲区加载到 Python 列表。** 即使一个适中的 1440 条负荷曲线，一次全部反序列化也可能超出可用堆内存。在 `on_before_read` 中逐行从文件流式读取。

---

### 安全加固

**部署前更改所有默认密钥。** 运行时会初始化密码学密钥为已知测试值。使用这些默认值的设备可被知晓这些值的任何人读取或控制。必须更改的密钥：

- **GMAC_GUEK** — Global Unicast Encryption Key（加密数据）
- **GMAC_GAK** — Global Authentication Key（认证帧）
- **GMAC_KEK** — Key Encryption Key（保护密钥更新包裹）

在 `SecuritySetup` 对象上设置：

```python
security_setup.global_unicast_encryption_key = bytes.fromhex("YOUR_32_HEX_CHARS_GUEK")
security_setup.global_authentication_key     = bytes.fromhex("YOUR_32_HEX_CHARS_GAK")
```

KEK 通过 `dlms.set_kek()` 在启动时一次性传递给运行时。

**将 KEK 存储在受保护存储中。** KEK 是最敏感的密钥，因为它在密钥更新过程中包裹所有其他密钥。它**不能**以字符串字面量出现在 `config.py` 中。使用 Quectel 的保护闪存分区或硬件安全元件。至少，将其存储在一个 USB 大容量存储模式不可访问的分区文件中。

**对承载真实计量数据的连接使用 HighGMac 认证。** `AuthenticationLevel.HIGH_GMAC` 提供双向密码学认证和每帧重放保护。较低的认证级别（NONE、LOW）仅保留给对象列表经过严格限制的只读诊断关联。

**将未认证访问限制为最小对象集。** 任何 `auth_mechanism = 'None'` 的 `AssociationLogicalName` 在无凭证情况下公开可读。其访问字典最多应暴露少量标识对象（如 clock、固件版本）。**切勿**在未认证关联中包含可写对象或安全相关对象。

---

### 线程安全

`SerialConnection` 和 `MobileConnection` 在 C 管理的后台线程中运行 I/O 循环。COSEM 对象上注册的事件处理程序从这些线程调用。主 Python 线程同时运行应用循环。这会产生共享状态风险。

**保持事件处理程序简短且非阻塞。** 阻塞（sleep、等待锁、慢速文件系统写入）的事件处理程序会延迟连接线程，可能导致客户端超时。若需在事件响应中执行重要工作，设置标志并在主循环中处理：

```python
_capture_pending = [False]

def on_before_action_capture(profile, event):
    if event.index == 2:
        _capture_pending[0] = True
    return True

# 在主应用循环中：
while True:
    if _capture_pending[0]:
        _capture_pending[0] = False
        # ... 在此处理慢速工作 ...
    utime.sleep_ms(100)
```

**当处理程序和主循环都写入同一对象时，用锁保护共享状态。** MicroPython 的 `_thread.allocate_lock()` 提供简单互斥锁：

```python
import _thread

_lock = _thread.allocate_lock()
_shared_value = [0]

def on_before_write(obj, event):
    with _lock:
        _shared_value[0] = event.value
    return True

# 在主循环中：
with _lock:
    v = _shared_value[0]
```

**在紧循环中使用 `utime.sleep_ms(0)` 出让 CPU。** MicroPython 调度器对 Python 线程是协作式的。主循环迭代超过几毫秒不让出可能导致连接线程饥饿。在任何紧轮询循环中插入 `utime.sleep_ms(0)`（或一个小的正值）。

---

### 连接可靠性

**通过 `on_disconnected` 响应断连。** 每个连接类都暴露 `on_disconnected` 回调。注册一个至少记录事件的处理程序，并可选地重启受影响的连接或触发 modem 重置：

```python
def _on_disconnected():
    print("connection lost; scheduling reconnect")
    _reconnect_pending[0] = True

mobile.on_disconnected = _on_disconnected
```

**在 `IecHdlcSetup` 上设置 `inactivity_timeout` 以检测静默断连。** 未发送 DISC 帧即消失的客户端会使服务器无限等待。设置非零的 `inactivity_timeout`（秒）使运行时在指定时间内无流量后关闭并重新打开连接：

```python
hdlc = dlms.IecHdlcSetup(
    "0.0.22.0.0.255",
    commSpeed=9600,
    deviceAddr=0x10,
    inactivity_timeout=120,  # 静默 2 分钟后关闭
)
```

**监控 `GsmDiagnostic.status` 以检查蜂窝链路。** 在主循环中定期调用 `gsm_diag.update()` 并检查 `gsm_diag.status`。值 `1`（HOME_NETWORK）或 `5`（ROAMING）表示注册激活；其他值表示设备不可达：

```python
import utime

STATUS_REGISTERED = (1, 5)  # HOME_NETWORK, ROAMING

while True:
    gsm_diag.update()
    if gsm_diag.status not in STATUS_REGISTERED:
        print("network lost; status={}".format(gsm_diag.status))
        # 等待重新注册后再尝试重连
    utime.sleep_ms(60000)  # 每 60 秒检查一次
```

---

### 无硬件测试

DLMS 服务器逻辑的端到端测试通常需要物理开发板和 DLMS 客户端探头。`GenericConnection` 提供了替代方案：由于其传输完全由 Python 驱动，可以在单个 Python 进程中构建回环，无需任何硬件即可执行请求-响应周期。

模式是创建一个带 `GenericConnection` 的服务器和一个客户端，将 `on_send` 和 `on_receive` 钩子反向连接到 `process_msg`：

```python
import dlms

# 最小服务器
hdlc = dlms.IecHdlcSetup("0.0.22.0.0.255")
conn = dlms.GenericConnection(
    interface_type=dlms.InterfaceType.HDLC,
    hdlc_setup=hdlc,
)
server = dlms.Server(serial=99999, use_logical_name=True)
data_obj = dlms.Data("1.0.1.8.0.255")
data_obj.value = 42
server.add_object(data_obj)
server.add_connection(conn)
conn.connect()
server.run()

# 将帧直接发送到 process_msg 的客户端
client = dlms.Client(
    client_address=16,
    server_address=dlms.hdlc_server_address(99999),
    conn=conn,
)

def loopback_send(data):
    resp = conn.process_msg(data)
    if resp:
        conn.process_msg(resp)  # 将响应反馈回去

client.on_send = loopback_send
client.connect()
value = client.read(data_obj, 2)  # 读取属性 2
print(value)                       # 42
client.disconnect()
```

这种技术对于验证属性访问控制规则、确认 `on_before_read` / `on_before_write` 处理程序返回正确值，以及检查序列化往返都非常有用——全部在 PC 侧 Python 环境中完成。

**客户端方法**（`connect`、`read`、`write`、`action`）是同步的，失败时抛出 `RuntimeError`。在生产代码中，始终将它们包裹在 `try/except` 块中，以便优雅处理断连或意外响应，而非崩溃调用线程。

---

### 代码组织

大型部署受益于将应用拆分为集中的模块，而非将所有代码放在 `main.py` 中。`server_example/objects/` 目录展示了一种有效的布局：

```
config.py
    所有部署特定常量：OBIS 地址、数字序列号、APN、密码学密钥、端口号。
    这是不同开发板或客户的固件构建之间唯一不同的地方。

objects/data_objects.py
    Data、Register 和 ExtendedRegister 实例及其初始值。

objects/profile_objects.py
    ProfileGeneric 实例、捕获对象列表、缓冲区持久化辅助函数。

objects/security_objects.py
    SecuritySetup、AssociationLogicalName、AssociationShortName、访问字典。

objects/connection_objects.py
    IecHdlcSetup、TcpUdpSetup、GprsSetup、MobileConnection、SerialConnection。

main.py
    导入以上所有模块，调用 server.add_object() 和 server.add_connection()，
    调用 server.run()，然后进入应用循环。
```

**启动时创建存储目录。** 切勿假设 `/usr/dlms/` 或 `/sd/dlms/` 已存在。在实例化任何序列化器之前的启动阶段调用一次辅助函数，可防止首次写入时出现 `OSError`：

```python
import uos

DIRS = ["/usr/dlms", "/usr/dlms/objects"]

def _ensure_dirs():
    for path in DIRS:
        try:
            uos.mkdir(path)
        except OSError:
            pass  # 已存在

_ensure_dirs()
```

**保持 `config.py` 不含逻辑。** 配置常量应为简单赋值。若某个值需要计算（如从序列号派生 HDLC 地址），计算一次——若开销小则在导入时计算，否则在 `main.py` 调用的 `init()` 函数中计算。避免 `config.py` 中出现依赖运行时状态的条件逻辑；该逻辑属于 `main.py` 或相关模块。

## 术语表（Glossary）



**APDU（Application Protocol Data Unit）** — DLMS 栈中最顶层的 PDU。APDU 携带 COSEM 服务请求或响应，并传递给传输层进行成帧。



**Association（关联）** — DLMS 客户端和服务器之间建立的逻辑会话。每个关联具有协商的认证级别、密码套件和一致性位集。在 `dlms` 模块中，关联由 `AssociationLogicalName`（LN 引用）和 `AssociationShortName`（SN 引用）对象表示。


**Authentication（认证）** — 验证连接客户端身份的过程。`dlms` 模块支持的级别从 `NONE`（开放访问）到 `HIGH_GMAC`（AES-GCM 双向认证）。参见第 5 章。


**BinarySerializer** — `dlms` 序列化辅助工具，将 COSEM 对象属性状态保存到设备文件系统上的二进制文件并恢复。标记为 `AttributeFlag.COMPLEX`（如 `ProfileGeneric.buffer`）的属性被排除。参见第 9 章。


**CGNAT（Carrier-Grade NAT）** — 移动运营商运行的网络地址转换层，为蜂窝设备分配私有 IP 地址，使设备无法接受直接入站 TCP 连接。DLMS UDP 路由器提供了绕过此限制的中继方案。参见第 10 章。


**COSEM（Companion Specification for Energy Metering）** — IEC 62056 标准的数据模型部分，定义了接口类目录及其属性。`dlms` 模块中的每个 Python 类对应一个 COSEM 接口类。

**Default behaviour（默认行为）** — 当无事件处理程序拦截请求时，服务器 C 层自动执行的处理。对于 GET 请求意味着序列化当前属性值；对于 ACTION 请求意味着分发内置方法实现。事件处理程序可以观察或替换默认行为。参见第 4 章。

**DLMS（Device Language Message Specification）** — IEC 62056 标准的协议部分，定义 COSEM 对象如何序列化为 APDU 以及 APDU 如何传输。"DLMS" 通常非正式地指代 DLMS/COSEM 组合标准。


**DLMSEvent** — 传递给每个事件处理程序的上下文对象。携带当前请求的属性或方法索引、选择器类型、选择器参数和动作标志。参见第 4.1 节。

**GAK（Global Authentication Key）** — 16 或 32 字节的 AES 密钥，用于在 HighGMac 安全模式下计算 DLMS 帧的 GMAC 认证标签。服务器和客户端必须相同。


**GenericConnection** — `dlms` 连接类，其传输 I/O 循环由 Python 编写。调用者打开物理通道，将接收到的字节传递给 `process_msg()`，并将返回的字节写回通道。用于 UART、SPI、MQTT、G3-PLC 以及无需硬件的单元测试。参见第 6.4 节和第 11.5 节。

**GUEK（Global Unicast Encryption Key）** — 16 或 32 字节的 AES 密钥，用于在 HighGMac 安全模式下加密 DLMS APDU。有时写作 GMAC_GUEK。服务器和客户端必须相同。

**HDLC（High-Level Data Link Control）** — DLMS 在串行传输（RS-232、RS-485、光口）上使用的数据链路成帧层。提供寻址、成帧和流控制。HDLC 服务器地址由 `dlms.hdlc_server_address()` 从设备序列号派生。


**HighGMac** — `dlms` 模块中最高的认证级别（`Authentication.HIGH_GMAC`）。使用 AES-GCM 提供每帧认证和可选加密。需要配置了 `guek` 和 `gak` 密钥的 `SecuritySetup` 对象。参见第 5.2 节。


**IEC 62056** — DLMS/COSEM 的国际标准系列。蓝皮书（IEC 62056-62）定义 COSEM 接口类；绿皮书（IEC 62056-5-3）定义安全；黄皮书涵盖一致性。


**Interface class（接口类）** — COSEM 中对对象类型的术语。每个接口类具有数字标识符（类 ID）并定义一组固定的属性和方法。例如，`Register` 是接口类 3，`Clock` 是接口类 8。


**Invocation counter（调用计数器）** — 每个 GMAC 保护帧中包含的单调递增的 32 位整数。服务器拒绝计数器不大于上次接受值的帧，防止重放攻击。计数器必须在断电重启间持久化。参见第 5.3 节。

**KEK（Key Encryption Key）** — 用于在密钥更新过程中包裹（加密）GUEK 和 GAK 的主密钥。在启动服务器前通过 `dlms.set_kek()` 设置。必须存储在设备的受保护存储中。参见第 5.4 节。


**LN referencing（逻辑名引用）** — COSEM 属性通过 OBIS 代码和属性索引标识的寻址模式。由 `AssociationLogicalName` 使用。这是 `dlms` 模块中的默认模式。

**MobileConnection** — `dlms` 连接类，管理 C 驱动的蜂窝 UDP 套接字。支持直连模式（来自任何客户端的入站 UDP）和中继模式（向 DLMS UDP 路由器的出站注册）。需要预分配的 `recv_buffer`。参见第 6.3 节和第 10.4 节。

---

**OBIS code（对象标识系统）** — 六组数字标识符 `A.B.C.D.E.F`，唯一标识设备上的 COSEM 对象实例。每个 `dlms` 对象构造函数接受 OBIS 字符串作为其第一个参数。

**PDU（Protocol Data Unit）** — 特定协议层上的结构化数据单元。在 DLMS 应用层为 APDU；在 HDLC 层为 HDLC 帧。

**ProfileGeneric** — COSEM 接口类 7。存储历史时间序列数据（负荷曲线、事件日志）的标准对象。其 `buffer` 属性标记为 COMPLEX，必须与 `BinarySerializer` 分开持久化。参见第 3.6 节和第 9.4 节。
**QuecPython** — 嵌入 Quectel 蜂窝模组中的 MicroPython 1.13 运行时。`dlms` 模块是该运行时的编译 C 扩展。

**SAP（Service Access Point）** — DLMS 系统中逻辑设备或客户端的数字标识符。标准管理客户端 SAP 为 16。SAP 到逻辑设备的映射保存在 `SapAssignment` 中。

**SecuritySetup** — COSEM 接口类 64。持有一个安全上下文的密码学密钥、安全策略和系统标题。每个使用 HighGMac 的关联需要一个 SecuritySetup 对象。参见第 5.3 节。

**SN referencing（短名引用）** — 使用 16 位短名代替 OBIS 代码的替代 COSEM 寻址模式。由 `AssociationShortName` 使用。在现代部署中较少见；LN 引用更受青睐。

**System title（系统标题）** — DLMS 实体的 8 字节标识符，用作 GCM 初始化向量的一部分。常规格式：3 字节 ASCII 标志标识符后跟 5 字节序列号。存储在 `SecuritySetup.server_system_title` 和 `client_system_title` 中。

**WRAPPER** — 用于在 UDP 或 TCP 上承载 DLMS APDU 的薄成帧层（IEC 62056-47）。与 HDLC 不同，它不添加寻址；路由由 IP 层处理。通过 `GenericConnection` 上的 `InterfaceType.WRAPPER` 选择。
