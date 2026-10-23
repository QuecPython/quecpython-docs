# power - 关机以及软件重启

提供关机、软件重启、获取开机原因、获取上次关机原因、获取电池电压功能。

## 关机功能

### `Power.powerDown`

```python
Power.powerDown()
```

模块关机。

**示例：**

```python
from misc import Power

Power.powerDown()
```

## 重启功能

### `Power.powerRestart`

```python
Power.powerRestart()
```

模块重启。

## 获取开机原因功能

### `Power.powerOnReason`

```python
Power.powerOnReason()
```

获取开机原因。

**返回值描述：**

| 值   | 说明                             |
| ---- | -------------------------------- |
| 0    | 获取开机原因失败或者开机原因未知 |
| 1    | 按 PWRKEY 开机                   |
| 2    | 按 RESET 重启                    |
| 3    | VBAT 触发的开机                  |
| 4    | RTC 定时开机                     |
| 5    | watchdog 触发重启或异常开机      |
| 6    | VBUS 触发的开机                  |
| 7    | 充电开机                         |
| 8    | PSM 唤醒开机                     |
| 9    | 发生 Dump 后重启                 |

## 获取上次关机原因

### `Power.powerDownReason`

```
Power.powerDownReason()
```

获取关机原因。

**返回值描述：**

| 值   | 说明                  |
| ---- | --------------------- |
| 0    | 原因未知              |
| 1    | 正常关机              |
| 2    | 供电电压过高导致关机  |
| 3    | 供电电压过低导致关机  |
| 4    | 温度过高导致关机      |
| 5    | 看门狗触发的关机      |
| 6    | VRTC 电压过低触发关机 |

> BC25PA系列和EC200U/EC600U系列不支持此方法。

## 获取电池电压

### `Power.getVbatt`

```python
Power.getVbatt()
```

获取电池电压，单位mV。

**返回值描述：**

返回整型电压值。

**示例：**

```python
Power.getVbatt()
3590
```

## CAM_VDD电源输出

### `Power.camVDDEnable`

```python
Power.camVDDEnable(enable,[vol],[lp_enable])
```

CAM_VDD管脚电源输出控制。

> 仅EC600U/EG912U/EG915U系列支持。

**参数描述：**

- `enable` int类型,范围0/1，0：关闭输出，1：打开输出；
- `[vol]`- int类型，可缺省，范围：VDD_1V80，VDD_2V50，VDD_2V80，VDD_3V00，分别设置输出电压为1.8v，2.5v，2.8v，3.0v，缺省为输出2.8v。
- `[lp_enable]`-int类型，可缺省，LP_ENABLE：低功耗状态可以输出，LP_DISABLE:低功耗状态不输出，缺省为低功耗状态不输出。

**返回值描述：**

0表示成功，-1表示失败。

**示例：**

```python
from misc import Power
Power.camVDDEnable(1,Power.VDD_1V80，Power.LP_ENABLE)#打开cam_vdd电源输出，电压1.8v，在低功耗下继续输出
Power.camVDDEnable(0)#关闭输出
```

