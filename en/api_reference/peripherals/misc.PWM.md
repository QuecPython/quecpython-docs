# PWM - Pulse Width Modulation

This class provides the feature of PWM output. 

> Note: BC25 series module does not support this feature.

## Constructor

### `misc.PWM`

```python
class misc.PWM(PWM.PWMn,PWM.ABOVE_xx, highTime, cycleTime)
```

**Parameter:**

- `PWM.PWMn` - Integer type. PWM channel. <a href="#label_pwmmap">Click here to learn more</a> for supported channels and corresponding pins.

- `PWM.ABOVE_xx` - Integer type. Time range. 

  For EC200U, EC600U , EG915U and EG912U series modules:<br />PWM.ABOVE_MS				          ms-level time range: (0,10]<br/>PWM.ABOVE_1US				        us-level time range: (0,10000]<br/>PWM.ABOVE_10US				      us-level time range: (1,10000]<br/>PWM.ABOVE_BELOW_US			ns-level time range: [100,65535]
    <br>FCM360W:<br />PWM.ABOVE_MS				          ms-level time range：[0,1]<br/>PWM.ABOVE_1US				        us-level time range：[0,1638]<br/>PWM.ABOVE_10US				      us-level time range：[0,1638]<br/>PWM.ABOVE_BELOW_US			ns-level time range: [2500,65535]

- `highTime` - Integer type. High level time. 

  For ms-level time, unit: ms.<br/>For us-level time, unit: us.<br/>For ns-level: it needs to be calculated by users.<br/>               Frequency = 13Mhz / cycleTime<br/>               Duty cycle = highTime/ cycleTime

- `cycleTime` - Integer type. Cycle time.

  For ms-level time, unit: ms.<br/>For us-level time, unit: us.<br/>For ns-level: it needs to be calculated by users. <br/>              Frequency = 13Mhz / cycleTime<br/>              Duty cycle = highTime/ cycleTime

> Frequency support range: EC600N/EC800N/EG912N/EC600M/EC800M/EG810M/EC200A/EC200A-V1/EC600E/EC800E/EC600K/EC800K/EG915N: 1Hz - 1MHz, EC200U/EC600U/EG915U series: 100Hz - 1MHz, BG95 series: 293Hz - 600kHz.

**Example:**

```python
 from misc import PWM
 pwm1 = PWM(PWM.PWM1, PWM.ABOVE_MS, 1, 2)
```

### `misc.PWM_V2`

```python
class misc.PWM_V2(PWM_V2.PWMn,frequency, duty)
```

**Parameter:**

- `PWM_V2.PWMn` - Integer type. PWM channel. <a href="#label_pwmmap">Click here to learn more</a> for supported channels and corresponding pins.

- `frequency` - float type. will be in a form of float (I.e. 100.0 indicates 100Hz) . 

- `duty` - Integer type.  a input range 0~100. The 50 means the duty ratio is 50%.


> EC600U/EC200U/EG912U/EG915U para `duty` can't set to 0.

**Example:**

```python
 from misc import PWM_V2
 pwm1 = PWM_V2(PWM_V2.PWM1, 100.0, 50)#set frequence to 100Hz,duty to 50%
```

<span id="label_pwmmap">**Mapping Relationship Between PWM Channels and Pysical Pins:**</span>

The corresponding pins of EC100Y series module for PWM0-PWM3 are as follows:<br/>PWM0 – pin19<br/>PWM1 – pin18<br/>PWM2 – pin23<br/>PWM3 – pin22<br/>The corresponding pins of EC600S-CN and EC600N modules for PWM0-PWM3 are as follows:<br/>PWM0 – pin52<br/>PWM1 – pin53<br/>PWM2 – pin70<br/>PWM3 – pin69<br />The corresponding pins of EC800N module for PWM0-PWM3 are as follows:<br/>PWM0 – pin79<br/>PWM1 – pin78<br/>PWM2 – pin16<br/>PWM3 – pin49<br />The corresponding pin of EC200U series module for PWM0 is as follows:<br />PWM0 – pin135<br />The corresponding pin of EC600U series module for PWM0 is as follows:<br />PWM0 – pin70<br />The corresponding pins of EC600M module for PWM0-PWM3 are as follows:<br/>PWM0 – pin57<br/>PWM1 – pin56<br/>PWM2 – pin70<br/>PWM3 – pin69<br/>The corresponding pin of EG915U series module for PWM0 is as follows:<br/>PWM0 – pin20<br/>The corresponding pins of EC800M/EG810M module for PWM0-PWM3 are as follows:<br/>PWM0 – pin83<br/>PWM1 – pin78<br/>PWM2 – pin16<br/>PWM3 – pin49<br/>The corresponding pins of EG912N module for PWM0-PWM3 are as follows:<br/>PWM0 – pin21<br/>PWM1 – pin116<br/>PWM2 – pin107<br/>PWM3 – pin92<br/>

The corresponding pins of EG912U module for PWM0 are as follow:<br/>PWM0 – pin20<br />

The corresponding pins of BG95 module for PWM0-PWM1 are as follow:<br/>PWM0 – pin66<br/>PWM1 – pin85

The corresponding pins of EC200A/UC200A module for PWM0-PWM2 are as follow:<br/>PWM0 – pin1<br/>PWM1 – pin4<br/>PWM2 – pin6

The corresponding pins of EC200A-V1 module for PWM0-PWM2 are as follow:<br/>PWM0 – pin65<br/>PWM1 – pin64<br/>PWM2 – pin6<br/>PWM3 – pin4

The corresponding pins of EG915N module for PWM0-PWM3 are as follow:<br/>PWM0 – pin25<br/>PWM1 – pin26<br/>PWM2 – pin104<br/>PWM3 – pin18

The corresponding pins of EC800K/EG800K/EG800AK module for PWM0-PWM3 are as follow:<br/>PWM1 – pin78<br/>PWM2 – pin83

The corresponding pins of BG950S module for PWM0-PWM2 are as follow:<br/>PWM0 – pin66<br/>PWM1 – pin38<br/>PWM2 – pin30

The corresponding pins of EG800Z module for PWM0-PWM14 are as follow:<br/>PWM0 – pin20<br/>PWM1 – pin33<br/>PWM2 – pin32<br/>PWM3 – pin31<br/>PWM4 – pin30<br/>PWM7 – pin16<br/>PWM8 – pin49<br/>PWM9 – pin22<br/>PWM10 – pin26<br/>PWM11 – pin25<br/>PWM12 – pin54<br/>PWM13 – pin101<br/>PWM14 – pin100

## Methods

### `PWM.open`

```python
PWM.open()
```

This methods enables PWM output.

**Return Value:**

`0` - Successful execution

`-1` - Failed execution

### `PWM_V2.open`

```python
PWM_V2.open([frequency],[duty])
```

This methods enables PWM output.`frequency` and  `duty`  are default parameters,only allow input both or neither.

**Return Value:**

`0` - Successful execution

`-1` - Failed execution

### `PWM.close`

```
PWM.close()
```

This methods disables PWM output.

**Return Value:**

`0` - Successful execution

`-1` - Failed execution

### `PWM_V2.close`

```
PWM_V2.close()
```

This methods disables PWM output.

**Return Value:**

`0` - Successful execution

`-1` - Failed execution

**Example:**

```python
from misc import PWM
import utime
if __name__ == '__main__':
    pwm = PWM(PWM.PWM0, PWM.ABOVE_MS, 1, 2)  # Initializes a PWM object
    pwm.open()  # Enables PWM output
    utime.sleep(10)
    pwm.close()  # Disables PWM output
```

```python
from misc import PWM_V2
import utime

if __name__ == '__main__':
    pwm = PWM_V2(PWM_V2.PWM0,100.0,50)  #Initializes a PWM object,frequency is 100Hz,duty is 50%
    pwm.open()  # Enables PWM output
    utime.sleep(10)
    pwm.open(200.0,40)#Enables PWM output,and set frequecy to 200Hz,duty to 40%
    utime.sleep(10)
    pwm.close()  #Disables PWM output
```



## Constants

| Constant | Description | Module                                                       |
| -------- | ----------- | ------------------------------------------------------------ |
| PWM.PWM0 | PWM0        | EC600S / EC600N / EC100Y/EC600U/EC200U/EC800N/<br />EC600M/EG915U/EC800M/EG912N/EG912U/EG915N/BG950S/EC200A-V1/EG800Z |
| PWM.PWM1 | PWM1        | EC600S / EC600N / EC100Y/EC800N/EC600M/EC800M/EG912N/EG915N/EC800K/EG800K/EG800AK/BG950S/EC200A-V1  |
| PWM.PWM2 | PWM2        | EC600S / EC600N / EC100Y/EC800N/EC600M/EC800M/EG912N/EG915N/EC800K//EG800K/EG800AKBG950S/EC200A-V1  |
| PWM.PWM3 | PWM3        | EC600S / EC600N / EC100Y/EC800N/EC600M/EC800M/EG912N/EG915N/EC200A-V1  |
| PWM.PWM3  | PWM3  | EC600S / EC600N / EC100Y/EC800N/EC600M/EC800M/EG912N/EC600E/EC800E/EC600G/<br />EC800G/EC200A-V1/EC600K/FCM360W/EG915N/EG800Z |
| PWM.PWM4  | PWM4  | EG800Z                                 |
| PWM.PWM7  | PWM7  | EG800Z                                         |
| PWM.PWM8  | PWM8  | EG800Z                                         |
| PWM.PWM9  | PWM9  | EG800Z                                         |
| PWM.PWM10 | PWM10 | EG800Z                                         |
| PWM.PWM11 | PWM11 | EG800Z                                         |
| PWM.PWM12 | PWM12 | EG800Z                                         |
| PWM.PWM13 | PWM13 | EG800Z                                         |
| PWM.PWM14 | PWM14 | EG800Z                                         |