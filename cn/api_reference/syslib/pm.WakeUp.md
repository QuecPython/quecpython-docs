# class WakeUp - 唤醒中断

EC600/EC800E/EC800Z系列的普通GPIO不支持双边沿触发中断，且不支持休眠下中断唤醒。WakeUp则没有上下拉的限制，默认支持双边沿触发中断以及在休眠状态下唤醒模组。使用方法如下：

## **构造函数**

```python
class pm.WakeUp(WakeupID, pull,[edge])
```

**参数说明：**

- `WakeupID` - 需要控制的wakeup号，<a href="#label_wakeupmap">点此查看</a>：<br />
- `pull` - 上下拉模式，int类型，说明如下：<br />`PULL_DISABLE`– 浮空模式0<br />`PULL_PU` – 上拉模式1 <br />`PULL_PD`  – 下拉模式2<br />

- `[edge_enable]` - 触发边沿设置， int类型，缺省值为双边沿触发，说明如下：<br />`IRQ_RISING`-仅使能上升沿触发<br />

  `IRQ_FALLING`-仅使能下降沿触发<br />`IRQ_RISING_FALLING`-使能双边沿触发

> `[edge_enable]`仅EC800Z支持设置。

**初始化示例：**

```python
WakeUp(WakeUp.WAKEUP5,WakeUp.PULL_PD,WakeUp.IRQ_RISING)#EC800Z 配置为上升沿触发
WakeUp(WakeUp.WAKEUP5,WakeUp.PULL_PD,WakeUp.IRQ_FALLING)#EC800Z 配置为下降沿触发
```

**示例：**

```python
from pm import WakeUp
def fun(level):
    print(str(level))

wake=WakeUp(WakeUp.WAKEUP5,WakeUp.PULL_PD)
wake.set_callback(fun)# 设置回调，回调仅返回电平
wake.enable()# 使能中断

wake.read() #读引脚电平
```

## 方法

### `wake.enable`

```python
wake.enable()
```

该方法用于使能唤醒中断，当WakeUp引脚检测到上升沿或者下降沿时，会调用callback执行。

**返回值：**

成功返回整型值`0`，失败返回整型值`-1`。

### `wake.disable`

```python
wake.disable()
```

该方法用于关闭中断，禁用与WakeUp对象关联的中断 。

**返回值：**

成功返回整型值`0`，失败返回整型值`-1`。

### `wake.read`

```python
wake.read()
```

该方法用于读取当前引脚电平。

**返回值：**

返回引脚电平，`0`表示获取到的引脚电平为低，`1` 表示获取到的引脚电平为高。

### `wake.deinit`

```python
wake.deinit()
```

关闭WakeUp功能。

**返回值：**

成功返回整型值`0`，失败返回整型值`-1`。

### `wake.set_callback`
<p>注意：</p>
<p>EG800Z系列支持比autosleep睡眠等级更高的“更深度睡眠”。进入该睡眠状态后，RAM会掉电，此时WakeUp虽然可以通过中断将模组唤醒，但Python回调无法在唤醒过程中触发。当WakeUp中断唤醒模组、Python虚拟机重新运行后，若模组不再进入更深度睡眠，回调即可正常触发。</p>

```python
wake.set_callback(fun)
```

该方法用于设置回调，触发中断后，会执行该回调，回调返回电平。

**参数描述：**

- `fun` - 回调函数，回调函数原型：

  ```
  fun(level)
  ```

  回调函数参数描述：

  - level：`0`:低电平，`1`：高电平。

<span id="label_wakeupmap">**WakeUp编号与物理引脚的映射关系：**</span>


| 模组   | WakeUp对应引脚说明                                           |
| ------ | ------------------------------------------------------------ |
| EC800E | `WAKEUP2` - 引脚号79<br/>`WAKEUP3` - 引脚号109(EC800ECN_LE&LQ&LC不可用)<br />`WAKEUP4` - 引脚号108(EC800ECN_LE&LQ&LC不可用)<br />`WAKEUP5` - 引脚号19 |
| EC600E | `WAKEUP2` - 引脚号9<br/>`WAKEUP3` -  引脚号51<br/>`WAKEUP4` -  引脚号50<br/>`WAKEUP5` -  引脚号39 |
| EC800Z | `WAKEUP0` - 引脚号87(低压模组是107脚)<br/>`WAKEUP2` - 引脚号79<br/>`WAKEUP3` - 引脚号109<br />`WAKEUP4` - 引脚号108<br />`WAKEUP5` - 引脚号19 |
| EG800Z | `WAKEUP0` - 引脚号87<br/>`WAKEUP2` - 引脚号79<br/>`WAKEUP3` - 引脚号5<br />`WAKEUP4` - 引脚号6<br />`WAKEUP5` - 引脚号19 |