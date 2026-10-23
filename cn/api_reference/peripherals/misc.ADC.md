# class ADC - 模数转换

用于采集电压信号。

## 构造函数

### `misc.ADC`

```python
class misc.ADC()
```

**示例：**

```python
from misc import ADC
adc = ADC()
```

## 方法

### `ADC.open`

```python
ADC.open()
```

ADC功能初始化。

**返回值描述：**

`0`表示初始化成功，`-1`表示初始化失败。

### `ADC.read`

```python
ADC.read(ADCn)
```

读取指定通道的电压值，单位`mV`。

**参数描述：**

- `ADCn`-ADC通道,int类型,<a href="#label_pinmap">点此查看</a>支持的通道与对应引脚

**返回值描述：**

成功返回指定通道电压值，错误返回整型`-1`。

**示例：**

```python
>>>adc.read(ADC.ADC0)  #读取ADC通道0电压值
613
>>>adc.read(ADC.ADC1)  #读取ADC通道1电压值
605
```

### `ADC.close`

```python
ADC.close()
```

**返回值描述：**

`0`表示关闭成功，`-1`表示关闭失败。

<span id="label_pinmap">**ADC通道与物理引脚的映射关系：**</span>

<div class="table-QuecPython">
<table class="EC600M" >
<thead>
<tr>
  <th>ADC编号</th>
  <th>引脚对应</th>
</tr>
</thead>
<tbody>
<tr>
  <td>ADC0</td>
  <td>引脚号19</td>
</tr>
<tr>
  <td>ADC1</td>
  <td>引脚号20</td>
</tr>
</tbody>
</table>

<table class="EC800N" >
<thead>
<tr>
  <th>ADC编号</th>
  <th>引脚对应</th>
</tr>
</thead>
<tbody>
<tr>
  <td>ADC0</td>
  <td>引脚号9</td>
</tr>
</tbody>
</table>

<table class="EC600U" >
<thead>
<tr>
  <th>ADC编号</th>
  <th>引脚对应</th>
</tr>
</thead>
<tbody>
<tr>
  <td>ADC0</td>
  <td>引脚号19</td>
</tr>
<tr>
  <td>ADC1</td>
  <td>引脚号20</td>
</tr>
<tr>
  <td>ADC2</td>
  <td>引脚号113</td>
</tr>
<tr>
  <td>ADC3</td>
  <td>引脚号114</td>
</tr>
</tbody>
</table>

<table class="EC200U" >
<thead>
<tr>
  <th>ADC编号</th>
  <th>引脚对应</th>
</tr>
</thead>
<tbody>
<tr>
  <td>ADC0</td>
  <td>引脚号45</td>
</tr>
<tr>
  <td>ADC1</td>
  <td>引脚号44</td>
</tr>
<tr>
  <td>ADC2</td>
  <td>引脚号43</td>
</tr>
</tbody>
</table>

<table class="EC200A" >
<thead>
<tr>
  <th>ADC编号</th>
  <th>引脚对应</th>
</tr>
</thead>
<tbody>
<tr>
  <td>ADC0</td>
  <td>引脚号45</td>
</tr>
<tr>
  <td>ADC1</td>
  <td>引脚号44</td>
</tr>
</tbody>
</table>

<table class="EC200A-V1" >
<thead>
<tr>
  <th>ADC编号</th>
  <th>引脚对应</th>
</tr>
</thead>
<tbody>
<tr>
  <td>ADC0</td>
  <td>引脚号45</td>
</tr>
<tr>
  <td>ADC1</td>
  <td>引脚号44</td>
</tr>
</tbody>
</table>

<table class="UC200A" >
<thead>
<tr>
  <th>ADC编号</th>
  <th>引脚对应</th>
</tr>
</thead>
<tbody>
<tr>
  <td>ADC0</td>
  <td>引脚号45</td>
</tr>
<tr>
  <td>ADC1</td>
  <td>引脚号44</td>
</tr>
</tbody>
</table>

<table class="BG95" >
<thead>
<tr>
  <th>ADC编号</th>
  <th>引脚对应</th>
</tr>
</thead>
<tbody>
<tr>
  <td>ADC0</td>
  <td>引脚号24</td>
</tr>
</tbody>
</table>

<table class="EG915U" >
<thead>
<tr>
  <th>ADC编号</th>
  <th>引脚对应</th>
</tr>
</thead>
<tbody>
<tr>
  <td>ADC0</td>
  <td>引脚号24</td>
</tr>
<tr>
  <td>ADC1</td>
  <td>引脚号2</td>
</tr>
</tbody>
</table>

<table class="EC800M" >
<thead>
<tr>
  <th>ADC编号</th>
  <th>引脚对应</th>
</tr>
</thead>
<tbody>
<tr>
  <td>ADC0</td>
  <td>引脚号9</td>
</tr>
<tr>
  <td>ADC1</td>
  <td>引脚号96</td>
</tr>
</tbody>
</table>

<table class="EG810M" >
<thead>
<tr>
  <th>ADC编号</th>
  <th>引脚对应</th>
</tr>
</thead>
<tbody>
<tr>
  <td>ADC0</td>
  <td>引脚号9</td>
</tr>
<tr>
  <td>ADC1</td>
  <td>引脚号96</td>
</tr>
</tbody>
</table>

<table class="EG912N" >
<thead>
<tr>
  <th>ADC编号</th>
  <th>引脚对应</th>
</tr>
</thead>
<tbody>
<tr>
  <td>ADC0</td>
  <td>引脚号24</td>
</tr>
<tr>
  <td>ADC1</td>
  <td>引脚号2</td>
</tr>
</tbody>
</table>

<table class="EC600E" >
<thead>
<tr>
  <th>ADC编号</th>
  <th>引脚对应</th>
</tr>
</thead>
<tbody>
<tr>
  <td>ADC0</td>
  <td>引脚号19</td>
</tr>
<tr>
  <td>ADC1</td>
  <td>引脚号20</td>
</tr>
</tbody>
</table>

<table class="EC800E" >
<thead>
<tr>
  <th>ADC编号</th>
  <th>引脚对应</th>
</tr>
</thead>
<tbody>
<tr>
  <td>ADC0</td>
  <td>引脚号9</td>
</tr>
<tr>
  <td>ADC1</td>
  <td>引脚号96</td>
</tr>
</tbody>
</table>

<table class="EC600G" >
<thead>
<tr>
  <th>ADC编号</th>
  <th>引脚对应</th>
</tr>
</thead>
<tbody>
<tr>
  <td>ADC0</td>
  <td>引脚号19</td>
</tr>
<tr>
  <td>ADC1</td>
  <td>引脚号20</td>
</tr>
<tr>
  <td>ADC2</td>
  <td>引脚号113</td>
</tr>
<tr>
  <td>ADC3</td>
  <td>引脚号114</td>
</tr>
</tbody>
</table>

<table class="EC800G" >
<thead>
<tr>
  <th>ADC编号</th>
  <th>引脚对应</th>
</tr>
</thead>
<tbody>
<tr>
  <td>ADC0</td>
  <td>引脚号9</td>
</tr>
<tr>
  <td>ADC1</td>
  <td>引脚号96</td>
</tr>
</tbody>
</table>

<table class="EG912U" >
<thead>
<tr>
  <th>ADC编号</th>
  <th>引脚对应</th>
</tr>
</thead>
<tbody>
<tr>
  <td>ADC0</td>
  <td>引脚号24</td>
</tr>
<tr>
  <td>ADC1</td>
  <td>引脚号2</td>
</tr>
</tbody>
</table>

<table class="EC600K" >
<thead>
<tr>
  <th>ADC编号</th>
  <th>引脚对应</th>
</tr>
</thead>
<tbody>
<tr>
  <td>ADC0</td>
  <td>引脚号19</td>
</tr>
<tr>
  <td>ADC1</td>
  <td>引脚号20</td>
</tr>
</tbody>
</table>

<table class="EC800K" >
<thead>
<tr>
  <th>ADC编号</th>
  <th>引脚对应</th>
</tr>
</thead>
<tbody>
<tr>
  <td>ADC0</td>
  <td>引脚号9</td>
</tr>
<tr>
  <td>ADC1</td>
  <td>引脚号96</td>
</tr>
</tbody>
</table>

<table class="EG800K" >
<thead>
<tr>
  <th>ADC编号</th>
  <th>引脚对应</th>
</tr>
</thead>
<tbody>
<tr>
  <td>ADC0</td>
  <td>引脚号9</td>
</tr>
<tr>
  <td>ADC1</td>
  <td>引脚号96</td>
</tr>
</tbody>
</table>

<table class="EG800AK" >
<thead>
<tr>
  <th>ADC编号</th>
  <th>引脚对应</th>
</tr>
</thead>
<tbody>
<tr>
  <td>ADC0</td>
  <td>引脚号9</td>
</tr>
<tr>
  <td>ADC1</td>
  <td>引脚号96</td>
</tr>
</tbody>
</table>

<table class="EG800Z" >
<thead>
<tr>
  <th>ADC编号</th>
  <th>引脚对应</th>
</tr>
</thead>
<tbody>
<tr>
  <td>ADC0</td>
  <td>引脚号9</td>
</tr>
<tr>
  <td>ADC1</td>
  <td>引脚号96</td>
</tr>
</tbody>
</table>

<table class="FCM360W" >
<thead>
<tr>
  <th>ADC编号</th>
  <th>引脚对应</th>
</tr>
</thead>
<tbody>
<tr>
  <td>ADC0</td>
  <td>引脚号8</td>
</tr>
<tr>
  <td>ADC1</td>
  <td>引脚号7</td>
</tr>
<tr>
  <td>ADC2</td>
  <td>引脚号6</td>
</tr>
</tbody>
</table>

<table class="BC32" >
<thead>
<tr>
  <th>ADC编号</th>
  <th>引脚对应</th>
</tr>
</thead>
<tbody>
<tr>
  <td>ADC0</td>
  <td>引脚号2</td>
</tr>
</tbody>
</table>

<table class="BC92" >
<thead>
<tr>
  <th>ADC编号</th>
  <th>引脚对应</th>
</tr>
</thead>
<tbody>
<tr>
  <td>ADC0</td>
  <td>引脚号2</td>
</tr>
</tbody>
</table>

<table class="EG915N" >
<thead>
<tr>
  <th>ADC编号</th>
  <th>引脚对应</th>
</tr>
</thead>
<tbody>
<tr>
  <td>ADC0</td>
  <td>引脚号24</td>
</tr>
<tr>
  <td>ADC1</td>
  <td>引脚号2</td>
</tr>
</tbody>
</table>

<table class="EC800Z" >
<thead>
<tr>
  <th>ADC编号</th>
  <th>引脚对应</th>
</tr>
</thead>
<tbody>
<tr>
  <td>ADC0</td>
  <td>引脚号9</td>
</tr>
<tr>
  <td>ADC1</td>
  <td>引脚号96</td>
</tr>
</tbody>
</table>

<table class="BG950S" >
<thead>
<tr>
  <th>ADC编号</th>
  <th>引脚对应</th>
</tr>
</thead>
<tbody>
<tr>
  <td>ADC0</td>
  <td>引脚号24</td>
</tr>
<tr>
  <td>ADC1</td>
  <td>引脚号2</td>
</tr>
</tbody>
</table>

</div>

## 常量

<div class="table-cons-QuecPython">
<table class="EC600U" >
<thead>
<tr>
  <th>常量</th>
  <th>说明</th>
</tr>
</thead>
<tbody>
<tr>
  <td>ADC.ADC0</td>
  <td>ADC通道0</td>
</tr>
<tr>
  <td>ADC.ADC1</td>
  <td>ADC通道1</td>
</tr>
<tr>
  <td>ADC.ADC2</td>
  <td>ADC通道2</td>
</tr>
<tr>
  <td>ADC.ADC3</td>
  <td>ADC通道3</td>
</tr>
</tbody>
</table>

<table class="EC200U" >
<thead>
<tr>
  <th>常量</th>
  <th>说明</th>
</tr>
</thead>
<tbody>
<tr>
  <td>ADC.ADC0</td>
  <td>ADC通道0</td>
</tr>
<tr>
  <td>ADC.ADC1</td>
  <td>ADC通道1</td>
</tr>
<tr>
  <td>ADC.ADC2</td>
  <td>ADC通道2</td>
</tr>
</tbody>
</table>

<table class="BC25PA" >
<thead>
<tr>
  <th>常量</th>
  <th>说明</th>
</tr>
</thead>
<tbody>
<tr>
  <td>ADC.ADC0</td>
  <td>ADC通道0</td>
</tr>
</tbody>
</table>

<table class="EC800N" >
<thead>
<tr>
  <th>常量</th>
  <th>说明</th>
</tr>
</thead>
<tbody>
<tr>
  <td>ADC.ADC0</td>
  <td>ADC通道0</td>
</tr>
</tbody>
</table>

<table class="BG95" >
<thead>
<tr>
  <th>常量</th>
  <th>说明</th>
</tr>
</thead>
<tbody>
<tr>
  <td>ADC.ADC0</td>
  <td>ADC通道0</td>
</tr>
</tbody>
</table>

<table class="EC200A" >
<thead>
<tr>
  <th>常量</th>
  <th>说明</th>
</tr>
</thead>
<tbody>
<tr>
  <td>ADC.ADC0</td>
  <td>ADC通道0</td>
</tr>
<tr>
  <td>ADC.ADC1</td>
  <td>ADC通道1</td>
</tr>
</tbody>
</table>

<table class="EC200A-V1" >
<thead>
<tr>
  <th>常量</th>
  <th>说明</th>
</tr>
</thead>
<tbody>
<tr>
  <td>ADC.ADC0</td>
  <td>ADC通道0</td>
</tr>
<tr>
  <td>ADC.ADC1</td>
  <td>ADC通道1</td>
</tr>
</tbody>
</table>

<table class="EC600M" >
<thead>
<tr>
  <th>常量</th>
  <th>说明</th>
</tr>
</thead>
<tbody>
<tr>
  <td>ADC.ADC0</td>
  <td>ADC通道0</td>
</tr>
<tr>
  <td>ADC.ADC1</td>
  <td>ADC通道1</td>
</tr>
</tbody>
</table>

<table class="EG915U" >
<thead>
<tr>
  <th>常量</th>
  <th>说明</th>
</tr>
</thead>
<tbody>
<tr>
  <td>ADC.ADC0</td>
  <td>ADC通道0</td>
</tr>
<tr>
  <td>ADC.ADC1</td>
  <td>ADC通道1</td>
</tr>
</tbody>
</table>

<table class="EC800M" >
<thead>
<tr>
  <th>常量</th>
  <th>说明</th>
</tr>
</thead>
<tbody>
<tr>
  <td>ADC.ADC0</td>
  <td>ADC通道0</td>
</tr>
<tr>
  <td>ADC.ADC1</td>
  <td>ADC通道1</td>
</tr>
</tbody>
</table>

<table class="EG912N" >
<thead>
<tr>
  <th>常量</th>
  <th>说明</th>
</tr>
</thead>
<tbody>
<tr>
  <td>ADC.ADC0</td>
  <td>ADC通道0</td>
</tr>
<tr>
  <td>ADC.ADC1</td>
  <td>ADC通道1</td>
</tr>
</tbody>
</table>

<table class="EC600E" >
<thead>
<tr>
  <th>常量</th>
  <th>说明</th>
</tr>
</thead>
<tbody>
<tr>
  <td>ADC.ADC0</td>
  <td>ADC通道0</td>
</tr>
<tr>
  <td>ADC.ADC1</td>
  <td>ADC通道1</td>
</tr>
</tbody>
</table>

<table class="EC800E" >
<thead>
<tr>
  <th>常量</th>
  <th>说明</th>
</tr>
</thead>
<tbody>
<tr>
  <td>ADC.ADC0</td>
  <td>ADC通道0</td>
</tr>
<tr>
  <td>ADC.ADC1</td>
  <td>ADC通道1</td>
</tr>
</tbody>
</table>

<table class="EC600G" >
<thead>
<tr>
  <th>常量</th>
  <th>说明</th>
</tr>
</thead>
<tbody>
<tr>
  <td>ADC.ADC0</td>
  <td>ADC通道0</td>
</tr>
<tr>
  <td>ADC.ADC1</td>
  <td>ADC通道1</td>
</tr>
</tbody>
</table>

<table class="EC800G" >
<thead>
<tr>
  <th>常量</th>
  <th>说明</th>
</tr>
</thead>
<tbody>
<tr>
  <td>ADC.ADC0</td>
  <td>ADC通道0</td>
</tr>
<tr>
  <td>ADC.ADC1</td>
  <td>ADC通道1</td>
</tr>
</tbody>
</table>

<table class="EG912U" >
<thead>
<tr>
  <th>常量</th>
  <th>说明</th>
</tr>
</thead>
<tbody>
<tr>
  <td>ADC.ADC0</td>
  <td>ADC通道0</td>
</tr>
<tr>
  <td>ADC.ADC1</td>
  <td>ADC通道1</td>
</tr>
</tbody>
</table>

<table class="EC600K" >
<thead>
<tr>
  <th>常量</th>
  <th>说明</th>
</tr>
</thead>
<tbody>
<tr>
  <td>ADC.ADC0</td>
  <td>ADC通道0</td>
</tr>
<tr>
  <td>ADC.ADC1</td>
  <td>ADC通道1</td>
</tr>
</tbody>
</table>

<table class="EC800K" >
<thead>
<tr>
  <th>常量</th>
  <th>说明</th>
</tr>
</thead>
<tbody>
<tr>
  <td>ADC.ADC0</td>
  <td>ADC通道0</td>
</tr>
<tr>
  <td>ADC.ADC1</td>
  <td>ADC通道1</td>
</tr>
</tbody>
</table>

<table class="EG800K" >
<thead>
<tr>
  <th>常量</th>
  <th>说明</th>
</tr>
</thead>
<tbody>
<tr>
  <td>ADC.ADC0</td>
  <td>ADC通道0</td>
</tr>
<tr>
  <td>ADC.ADC1</td>
  <td>ADC通道1</td>
</tr>
</tbody>
</table>

<table class="EG800AK" >
<thead>
<tr>
  <th>常量</th>
  <th>说明</th>
</tr>
</thead>
<tbody>
<tr>
  <td>ADC.ADC0</td>
  <td>ADC通道0</td>
</tr>
<tr>
  <td>ADC.ADC1</td>
  <td>ADC通道1</td>
</tr>
</tbody>
</table>

<table class="EG800Z" >
<thead>
<tr>
  <th>常量</th>
  <th>说明</th>
</tr>
</thead>
<tbody>
<tr>
  <td>ADC.ADC0</td>
  <td>ADC通道0</td>
</tr>
<tr>
  <td>ADC.ADC1</td>
  <td>ADC通道1</td>
</tr>
</tbody>
</table>

<table class="EG810M" >
<thead>
<tr>
  <th>常量</th>
  <th>说明</th>
</tr>
</thead>
<tbody>
<tr>
  <td>ADC.ADC0</td>
  <td>ADC通道0</td>
</tr>
<tr>
  <td>ADC.ADC1</td>
  <td>ADC通道1</td>
</tr>
</tbody>
</table>

<table class="UC200A" >
<thead>
<tr>
  <th>常量</th>
  <th>说明</th>
</tr>
</thead>
<tbody>
<tr>
  <td>ADC.ADC0</td>
  <td>ADC通道0</td>
</tr>
<tr>
  <td>ADC.ADC1</td>
  <td>ADC通道1</td>
</tr>
</tbody>
</table>

<table class="FCM360W" >
<thead>
<tr>
  <th>常量</th>
  <th>说明</th>
</tr>
</thead>
<tbody>
<tr>
  <td>ADC.ADC0</td>
  <td>ADC通道0</td>
</tr>
<tr>
  <td>ADC.ADC1</td>
  <td>ADC通道1</td>
</tr>
<tr>
  <td>ADC.ADC2</td>
  <td>ADC通道2</td>
</tr>
</tbody>
</table>

<table class="BC32" >
<thead>
<tr>
  <th>常量</th>
  <th>说明</th>
</tr>
</thead>
<tbody>
<tr>
  <td>ADC.ADC0</td>
  <td>ADC通道0</td>
</tr>
</tbody>
</table>

<table class="BC92" >
<thead>
<tr>
  <th>常量</th>
  <th>说明</th>
</tr>
</thead>
<tbody>
<tr>
  <td>ADC.ADC0</td>
  <td>ADC通道0</td>
</tr>
</tbody>
</table>

<table class="EG915N" >
<thead>
<tr>
  <th>常量</th>
  <th>说明</th>
</tr>
</thead>
<tbody>
<tr>
  <td>ADC.ADC0</td>
  <td>ADC通道0</td>
</tr>
<tr>
  <td>ADC.ADC1</td>
  <td>ADC通道1</td>
</tr>
</tbody>
</table>

<table class="EC800Z" >
<thead>
<tr>
  <th>常量</th>
  <th>说明</th>
</tr>
</thead>
<tbody>
<tr>
  <td>ADC.ADC0</td>
  <td>ADC通道0</td>
</tr>
<tr>
  <td>ADC.ADC1</td>
  <td>ADC通道1</td>
</tr>
</tbody>
</table>

<table class="BG950S" >
<thead>
<tr>
  <th>常量</th>
  <th>说明</th>
</tr>
</thead>
<tbody>
<tr>
  <td>ADC.ADC0</td>
  <td>ADC通道0</td>
</tr>
<tr>
  <td>ADC.ADC1</td>
  <td>ADC通道1</td>
</tr>
</tbody>
</table>

</div>