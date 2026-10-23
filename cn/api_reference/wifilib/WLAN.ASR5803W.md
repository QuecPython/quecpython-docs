
# class ASR5803W - ASR5803W驱动

该类用于控制`ASR5803W`型号无线网卡设备。

> 支持模组型号：EC200A系列

## 构造函数

### `network.ASR5803W`

```python
class network.ASR5803W(mode, wlan_ldo_en_pin, wlan_reset_pin, host_wake_wlan_pin, wlan_wake_host_pin)
```

初始化WiFi网卡信息，并返回WiFi网卡对象。

**参数描述：**
- `mode` - int类型，无线网卡工作模式，0 为 ap 模式，1 为 sta 模式(目前仅支持 ap 模式)。
- `wlan_ldo_en_pin` - int类型，WiFi ldo 使能脚。
- `wlan_reset_pin` - int类型，WiFi reset 脚。
- `host_wake_wlan_pin` - int类型，模组唤醒 WiFi 脚。
- `wlan_wake_host_pin` - int类型，WiFi 唤醒模组脚。

**示例：**

```python
import network
from machine import Pin

nic = network.ASR5803W(0, Pin.GPIO7, Pin.GPIO36, Pin.GPIO34, Pin.GPIO6)
```

## 方法

### `ASR5803W.active`

```python
ASR5803W.active([is_active])
```

网卡激活状态配置/查询。

若传参则表示进行网卡状态配置。  
若不传参则表示查询网卡状态，返回值True/False表示网卡开启/关闭。

**可选参数描述：**

- `is_active` - bool类型，填True时表示启动网卡并激活config中网卡的配置，为False则代表停用网卡。返回0表示成功，返回其他值表示失败


### `ASR5803W.isconnected`

```python
ASR5803W.isconnected()
```

查询是否连接

**返回值描述：**

- 返回值表示是否有客户端连接，True/False分别表示有连接/无连接。


### `ASR5803W.status`

```python
ASR5803W.status([param])
```

查询网卡状态。当param不填写时返回当前连接状态，填入对应的状态信息名称时返回该状态的信息。

**可选参数描述：**

- `stations/node` - 获取当前连接客户端设备列表。
- `speed` - 获取当前网卡网络速率。

**返回值描述：**  
无传参：
- `True` - 网卡已被连接。
- `False` - 网卡未被连接。

有传参：
- `stations/node` - 返回list类型，当前连接客户端设备列表。若无设备连接返回0；若有设备连接返回一个包含两个元素的列表，元素 1 为当前连接设备数量(int 类型)，元素 2 为当前连接设备的信息元组，元组中的信息分别是设备 mac 地址(byte 类型)、设备 IP 地址(string 类型)、设备连接时长(int 类型，单位秒)。
- `speed` - 返回tuple类型，当前网卡网络速率，元素 1 是下行速率(int 类型)，元素 2 是上传速率(int 类型)，单位为 bps。


### `ASR5803W.config`

```python
ASR5803W.config('param')
ASR5803W.config(param=value)
```

网络参数查询/配置。

当只传参关键字时，接口查询，每次只能查询一个值。
当传参赋值时，接口设置，返回True/False表示成功/失败，可以同时设置多个值。

**可选参数描述：**

|  参数   | 类型  | 说明 |
| ---- | ---- |---------- |
| `ssid`    | `str` | 无线接入点名称 |
| `authmode`| `enum` | 鉴权类型 |
| `security`  | `enum` | 加密方式 |
| `password`  | `str` | 密码 |
| `protocol` | `enum` | 网络协议 |
| `channel` | `enum` | 无线信道号 |
| `b40acs` | `bool` | LTE band 40 下信道自适应 |
| `mac` | `str` | MAC地址 |

**示例：**  
配置信息查询：
```python
# 查询ssid名称配置
ASR5803W.config('ssid')
```
配置信息设置：
```python
# 设置无线名称及信号
ASR5803W.config(ssid='ssid', channel=1)
```
