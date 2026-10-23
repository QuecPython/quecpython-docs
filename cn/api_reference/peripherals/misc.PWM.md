# class PWM - 脉宽调制

提供脉宽调制输出功能。

> BC25系列不支持此模块。

## 构造函数

### `misc.PWM`

```python
class misc.PWM(PWM.PWMn,PWM.ABOVE_xx, highTime, cycleTime)
```

**参数描述：**

- `PWM.PWMn`-PWM通道，int类型，<a href="#label_pwmmap">点此查看</a>支持的通道与对应引脚；

- `PWM.ABOVE_xx`-时间取值范围，int类型,说明如下：

  EC200U/EC600U/EG915U/EG912U系列:<br />PWM.ABOVE_MS				          ms级取值范围：(0,10]<br/>PWM.ABOVE_1US				        us级取值范围：(0,10000]<br/>PWM.ABOVE_10US				      us级取值范围：(1,10000]<br/>PWM.ABOVE_BELOW_US			ns级取值范围[100,65535]
  <br>FCM360W:<br />PWM.ABOVE_MS				          ms级取值范围：[0,1]<br/>PWM.ABOVE_1US				        us级取值范围：[0,1638]<br/>PWM.ABOVE_10US				      us级取值范围：[0,1638]<br/>PWM.ABOVE_BELOW_US			ns级取值范围: [2500,65535]

- `highTime`-高电平时间，int类型，说明如下：

  ms级时，单位为ms<br/>us级时，单位为us<br/>ns级别：需要使用者计算<br/>               频率 = 1 / cycleTime<br/>               占空比 = highTime/ cycleTime

- `cycleTime`-周期时间，int类型，说明如下：

  ms级时，单位为ms<br/>us级时，单位为us<br/>ns级别：需要使用者计算<br/>             频率 = 1 / cycleTime<br/>             占空比 = highTime/ cycleTime

> 频率支持范围：EC600N/EC800N/EG912N/EC600M/EC800M/EG810M/EC200A/EC200A-V1/EC600E/EC800E/EC600K/EC800K/EG915N:1Hz-1MHz，EC200U/EC600U/EG915U系列：100Hz-1MHz，BG95系列：293Hz-600kHz。

**示例：**

```python
 from misc import PWM
 pwm1 = PWM(PWM.PWM1, PWM.ABOVE_MS, 1, 2)
```

### `misc.PWM_V2`

```python
class misc.PWM_V2(PWM_V2.PWMn,frequency, duty)
```

**参数描述**：

- PWM_V2.PWMn-PWM通道，int类型，<a href="#label_pwmmap">点此查看</a>支持的通道与对应引脚；
- frequency，频率，float类型，注意要输入浮点数据：100.0表示100Hz；
- duty，占空比，int类型，输入范围0~100，输入50代表占空比50%。

> 1、EC600U/EC200U/EG912U/EG915U不支持占空比设置为0。
> 
> 2、FCM360W支持的frequency范围为610Hz至40MHz

**示例：**

```python
from misc import PWM_V2
pwm1 = PWM_V2(PWM_V2.PWM1, 100.0, 50)#频率100Hz,占空比50%
```

<span id="label_pwmmap">**PWM通道与物理引脚的映射关系：**</span>

| 系列   | 对应引脚                                                     |
| ------ | ------------------------------------------------------------ |
| EC600N | PWM0 – 引脚号52<br/>PWM1 – 引脚号53<br/>PWM2 – 引脚号70<br/>PWM3 – 引脚号69 |
| EC800N | PWM0 – 引脚号79<br/>PWM1 – 引脚号78<br/>PWM2 – 引脚号16<br/>PWM3 – 引脚号49 |
| EC200U | PWM0 – 引脚号135                                             |
| EC600U | PWM0 – 引脚号70                                              |
| EC600M | PWM0 – 引脚号57<br/>PWM1 – 引脚号56<br/>PWM2 – 引脚号70<br/>PWM3 – 引脚号69 |
| EG915U | PWM0 – 引脚号20                                              |
| EC800M/EG810M | PWM0 – 引脚号83<br/>PWM1 – 引脚号78<br/>PWM2 – 引脚号16<br/>PWM3 – 引脚号49 |
| EG912N | PWM0 – 引脚号21<br/>PWM1 – 引脚号116<br/>PWM2 – 引脚号107<br/>PWM3 – 引脚号92 |
| EC200A/UC200A | PWM0 – 引脚号1<br/>PWM1 – 引脚号4<br/>PWM2 – 引脚号6         |
| EC200A-V1 | PWM0 – 引脚号65<br/>PWM1 – 引脚号64<br/>PWM2 – 引脚号6<br/>PWM3 – 引脚号4    |
| EC600E | PWM0 – 引脚号52(不可与PWM6/12同时为PWM)<br/>PWM1 – 引脚号53(不可与PWM7/13同时为PWM)<br/>PWM2 – 引脚号54(不可与PWM8/14同时为PWM)<br/>PWM3 – 引脚号63(不可与PWM9/15/16同时为PWM)(EC600ECN_LE&LQ不可用)<br/>PWM5 – 引脚号48(不可与PWM11同时为PWM)<br/>PWM6 – 引脚号31(不可与PWM0/12同时为PWM)<br/>PWM7 – 引脚号33(不可与PWM1/13同时为PWM)<br/>PWM8 – 引脚号34(不可与PWM2/14同时为PWM)<br/>PWM9 – 引脚号71(不可与PWM3/15/16同时为PWM)<br/>PWM11 – 引脚号72(不可与PWM5同时为PWM)<br/>PWM12 – 引脚号60(不可与PWM0/6同时为PWM)<br/>PWM13 – 引脚号58(不可与PWM1/7同时为PWM)<br/>PWM14 – 引脚号59(不可与PWM2/8同时为PWM)<br/>PWM15 – 引脚号62(不可与PWM3/9/16同时为PWM)(EC600ECN_LE&LQ不可用)<br/>PWM16 – 引脚号61(不可与PWM3/9/15同时为PWM) |
| EC800E | PWM0 – 引脚号25(不可与PWM6/12同时使用)<br/>PWM1 – 引脚号78(不可与PWM7/13同时使用)(EC800ECN_LE&LQ&CG不可用)<br/>PWM2 – 引脚号16(不可与PWM8/14同时使用)<br/>PWM3 – 引脚号49(不可与PWM9/15同时使用)(EC800ECN_LE&LQ&CG不可用)<br/>PWM4 – 引脚号31(不可与PWM10/16同时使用)<br/>PWM6 – 引脚号32(不可与PWM0/12同时使用)<br/>PWM7 – 引脚号30(不可与PWM1/13同时使用)<br/>PWM8 – 引脚号17(不可与PWM2/14同时使用)<br/>PWM9 – 引脚号21(不可与PWM3/15同时使用)<br/>PWM10 – 引脚号85(不可与PWM4/16同时使用)(EC800ECN_LE&LQ&LC&CG不可用)<br/>PWM12 – 引脚号23(不可与PWM0/6同时使用)<br/>PWM13 – 引脚号39(不可与PWM1/7同时使用)<br/>PWM14 – 引脚号33(不可与PWM2/8同时使用)<br/>PWM15 – 引脚号38(不可与PWM3/9同时使用)<br/>PWM16 – 引脚号22(不可与PWM4/10同时使用) |
| BG95   | PWM0 – 引脚号66<br/>PWM1 – 引脚号85                          |
| EC600G | PWM0 – 引脚号54<br/>PWM1 – 引脚号9(不可与PWM2/3同时使用)<br/>PWM2 – 引脚号126(不可与PWM1/3同时使用)<br/>PWM3 – 引脚号125(不可与PWM1/2同时使用) |
| EC800G | PWM0 – 引脚号79(不可与PWM20同时使用)<br/>PWM1 – 引脚号25(不可与PWM4/8/17/18同时使用)<br/>PWM2 – 引脚号16<br/>PWM3 – 引脚号21(不可与PWM5同时使用)<br/>PWM4– 引脚号20(不可与PWM1/8/17/18同时使用)<br/>PWM5 – 引脚号22(不可与PWM3同时使用)<br/>PWM6 – 引脚号28(不可与PWM22同时使用)<br/>PWM7 – 引脚号29(不可与PWM23同时使用)<br/>PWM8 – 引脚号54(不可与PWM1/4/17/18同时使用)<br/>PWM9 – 引脚号57(不可与PWM10/27同时使用)<br/>PWM10 – 引脚号58(不可与PWM9/27同时使用)<br/>PWM11 – 引脚号62(不可与PWM15同时使用)<br/>PWM12 – 引脚号63(不可与PWM16/19/21/24/28同时使用)<br/>PWM13 – 引脚号64(不可与PWM14同时使用)<br/>PWM14 – 引脚号66(不可与PWM13同时使用)<br/>PWM15 – 引脚号67(不可与PWM11同时使用)<br/>PWM16 – 引脚号68(不可与PWM12/19/21/24/28同时使用)<br/>PWM17 – 引脚号69(不可与PWM1/4/8/18同时使用)<br/>PWM18 – 引脚号80(不可与PWM1/4/8/17同时使用)<br/>PWM19 – 引脚号81(不可与PWM12/16/21/24/28同时使用)<br/>PWM20 – 引脚号82(不可与PWM0同时使用)<br/>PWM21 – 引脚号83(不可与PWM12/16/19/24/28同时使用)<br/>PWM22 – 引脚号84(不可与PWM6同时使用)<br/>PWM23 – 引脚号85(不可与PWM7同时使用)<br/>PWM24 – 引脚号86(不可与PWM12/16/19/21/28同时使用)<br/>PWM25 – 引脚号87(不可与PWM26同时使用)<br/>PWM26 – 引脚号100(不可与PWM25同时使用)<br/>PWM27 – 引脚号101(不可与PWM9/10同时使用)<br/>PWM28 – 引脚号103(不可与PWM12/16/19/21/24同时使用) |
| EG912U | PWM0 – 引脚号20                                              |
| EC600K | PWM0 – 引脚号10<br/>PWM2 – 引脚号70<br/>PWM3 – 引脚号69      |
| EC800K/EG800K/EG800AK | PWM1 – 引脚号78<br/>PWM2 – 引脚号83                                             |
| FCM360W | PWM0 – 引脚号13<br/>PWM1 – 引脚号14<br/>PWM2 – 引脚号15<br/>PWM3 – 引脚号16<br/>PWM4 – 引脚号8<br/>PWM5 – 引脚号7 |
| EG915N | PWM0 – 引脚号25<br/>PWM1 – 引脚号26<br/>PWM2 – 引脚号104<br/>PWM3 – 引脚号18 |
| BG950S | PWM0 – 引脚号66<br/>PWM1 – 引脚号38<br/>PWM2 – 引脚号30 |
| EG800Z | PWM0 – 引脚号20<br/>PWM1 – 引脚号33<br/>PWM2 – 引脚号32<br/>PWM3 – 引脚号31<br/>PWM4 – 引脚号30<br/>PWM7 – 引脚号16<br/>PWM8 – 引脚号49<br/>PWM9 – 引脚号22<br/>PWM10 – 引脚号26<br/>PWM11 – 引脚号25<br/>PWM12 – 引脚号54<br/>PWM13 – 引脚号101<br/>PWM14 – 引脚号100 |

## 方法

### `PWM.open`

```python
PWM.open([PWM.ABOVE_xx],[highTime],[cycleTime])
```

该方法用于开启PWM输出，PWM.ABOVE_xx，highTime，cycleTime为可缺省参数，只允许参数都缺省或传入3个参数；若缺省，按照初始化时的参数输出；若传参，按照传入的参数输出。

**参数描述：**

- 参照构造函数说明。

**返回值描述：**

`0`表示开启成功，`-1`表示开启失败。

### `PWM_V2.open`

```python
PWM_V2.open([frequency],[duty])
```

该方法用于开启PWM输出，frequency，duty为可缺省参数，只允许参数都缺省或传入2个参数；若缺省，按照初始化时的参数输出；若传参，按照传入的参数输出。

**参数描述：**

- 参照构造函数说明。

**返回值描述：**

`0`表示开启成功，`-1`表示开启失败。

### `PWM.close`

```
PWM.close()
```

该方法用于关闭PWM输出。

**返回值描述：**

`0`表示关闭成功，`-1`表示关闭失败。

### `PWM_V2.close`

```
PWM_V2.close()
```

该方法用于关闭PWM输出。

**返回值描述：**

`0`表示关闭成功，`-1`表示关闭失败。

**示例：**

```python
from misc import PWM
import utime

if __name__ == '__main__':
    pwm = PWM(PWM.PWM0, PWM.ABOVE_MS, 1, 2) #初始化一个pwm对象,频率500Hz,占空比50%
    pwm.open()  #开启PWM输出
    utime.sleep(10)
    pwm.open(PWM.ABOVE_MS, 5, 10) #开启PWM输出,并设置频率为100Hz,占空比50%
    utime.sleep(10)
    pwm.close()  #关闭pwm输出
```

```python
from misc import PWM_V2
import utime

if __name__ == '__main__':
    pwm = PWM_V2(PWM_V2.PWM0,100.0,50)  #初始化一个pwm对象,频率100Hz,占空比50%
    pwm.open()  # 开启PWM输出
    utime.sleep(10)
    pwm.open(200.0,40)#开启PWM输出,并设置pwm频率为200Hz,占空比40%
    utime.sleep(10)
    pwm.close()  #关闭pwm输出
```



## 常量

| 常量      | 说明  | 使用平台                                                     |
| --------- | ----- | ------------------------------------------------------------ |
| PWM.PWM0  | PWM0  | EC600S / EC600N / EC100Y/EC600U/EC200U/EC800N/EC600M/EG915U/EC800M/EG912N/BG95/EC600E/EC800E/<br />EC600G/EC800G/EC200A/EC200A-V1/EG912U/EC600K/FCM360W/EG915N/BG950S/EG800Z |
| PWM.PWM1  | PWM1  | EC600S / EC600N / EC100Y/EC800N/EC600M/EC800M/EG912N/BG95/EC600E/EC800E/<br />EC600G/EC800G/EC200A/EC200A-V1/EC800K/EG800K/EG800AK/EG800Z/FCM360W/EG915N/BG950S |
| PWM.PWM2  | PWM2  | EC600S / EC600N / EC100Y/EC800N/EC600M/EC800M/EG912N/EC600E/EC800E/EC600G/<br />EC800G/EC200A/EC200A-V1/EC600K/FCM360W/EG915N/EC800K/EG800K/EG800AK/EG800Z/BG950S|
| PWM.PWM3  | PWM3  | EC600S / EC600N / EC100Y/EC800N/EC600M/EC800M/EG912N/EC600E/EC800E/EC600G/<br />EC800G/EC200A-V1/EC600K/FCM360W/EG915N/EG800Z |
| PWM.PWM4  | PWM4  | EC600E/EC800E/EC800G/FCM360W/EG800Z                                 |
| PWM.PWM5  | PWM5  | EC600E/EC800E/EC800G/FCM360W                                 |
| PWM.PWM6  | PWM6  | EC600E/EC800E/EC800G                                         |
| PWM.PWM7  | PWM7  | EC600E/EC800E/EC800G/EG800Z                                         |
| PWM.PWM8  | PWM8  | EC600E/EC800E/EC800G/EG800Z                                         |
| PWM.PWM9  | PWM9  | EC600E/EC800E/EC800G/EG800Z                                         |
| PWM.PWM10 | PWM10 | EC600E/EC800E/EC800G/EG800Z                                         |
| PWM.PWM11 | PWM11 | EC600E/EC800E/EC800G/EG800Z                                         |
| PWM.PWM12 | PWM12 | EC600E/EC800E/EC800G/EG800Z                                         |
| PWM.PWM13 | PWM13 | EC600E/EC800E/EC800G/EG800Z                                         |
| PWM.PWM14 | PWM14 | EC600E/EC800E/EC800G/EG800Z                                         |
| PWM.PWM15 | PWM15 | EC600E/EC800E/EC800G                                         |
| PWM.PWM16 | PWM16 | EC600E/EC800E/EC800G                                         |
| PWM.PWM17 | PWM17 | EC800G                                                       |
| PWM.PWM18 | PWM18 | EC800G                                                       |
| PWM.PWM19 | PWM19 | EC800G                                                       |
| PWM.PWM20 | PWM20 | EC800G                                                       |
| PWM.PWM21 | PWM21 | EC800G                                                       |
| PWM.PWM22 | PWM22 | EC800G                                                       |
| PWM.PWM23 | PWM23 | EC800G                                                       |
| PWM.PWM24 | PWM24 | EC800G                                                       |
| PWM.PWM25 | PWM25 | EC800G                                                       |
| PWM.PWM26 | PWM26 | EC800G                                                       |
| PWM.PWM27 | PWM27 | EC800G                                                       |
| PWM.PWM28 | PWM28 | EC800G                                                       |

