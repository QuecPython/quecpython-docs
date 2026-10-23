# class WDT – 看门狗定时器

该类提供APP应用程序发生异常不执行时进行系统重启操作。

## 构造函数

### `machine.WDT`

```python
class machine.WDT(period)
```

创建软狗对象。

**参数描述：**

- `period` - 设置软狗检测时间，单位(s)，int类型。

**返回值描述：**

返回软狗对象。

>FCM360W实际超时时间生效值与设置值不一样，具体如下：<br>
1024s>设置时间>=256s,生效时间为256s<br>256s>设置时间>=64s,生效时间为64s<br>64s>设置时间>=16s,生效时间为16s<br>16s>设置时间>=4.5s,生效时间为4.5s<br>4.5s>设置时间>=1.5s,生效时间为1.5s<br>1.5s>设置时间>=1.004s,生效时间为1.004s<br>1.004s>设置时间>=0.502s,生效时间为0.502s<br>0.502s>设置时间>=0.252s,生效时间为0.252s<br>

## 方法

### `wdt.feed`

```python
wdt.feed()
```

该方法用于喂狗。

**返回值描述：**

成功返回整型值`0`。

### `wdt.stop`

```python
wdt.stop()
```

该方法用于关闭软狗功能。

**返回值描述：**

成功返回整型值`0`。

**使用示例：**

```python
from machine import WDT
from machine import Timer


timer1 = Timer(Timer.Timer1)

def feed(t):
    wdt.feed()


if __name__ == '__main__':
    wdt = WDT(20)  # 启动看门狗，设置超时时间
    timer1.start(period=15000, mode=timer1.PERIODIC, callback=feed)  # 使用定时器喂狗

    # wdt.stop()

```
