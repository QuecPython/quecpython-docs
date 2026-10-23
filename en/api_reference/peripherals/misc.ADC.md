# ADC - Voltage Collection

This class collects voltage signals.

## Constructor

### `misc.ADC`

```python
class misc.ADC()
```

**Example:**

```python
from misc import ADC
adc = ADC()
```

## Methods

### `ADC.open`

```python
ADC.open()
```

This method initializes ADC.

**Return Value:**

 `0` - Successful execution

 `-1` - Failed execution

### `ADC.read`

```python
ADC.read(ADCn)
```

This method reads voltage values of a specified channel. Unit: `mV`.

**Parameter:**

- `ADCn` - Integer type. ADC channel. <a href="#label_pinmap">Click here to learn more</a> for supported channels and corresponding pins.

**Return Value:**

If successful, a specified channel voltage value is returned.

 `-1` - Failed execution.

**Example:**

```python
>>>adc.read(ADC.ADC0)  # Reads voltage value of ADC0
613
>>>adc.read(ADC.ADC1)  # Reads voltage value of ADC1
605
```

<span id="label_pinmap">**Mapping Relationship Between ADC Passages and Pysical Pins:**</span>

<div class="table-QuecPython">
<table class="EC600M" >
<thead>
<tr>
  <th>ADC number</th>
  <th>Pin correspondence</th>
</tr>
</thead>
<tbody>
<tr>
  <td>ADC0</td>
  <td>Pin number19</td>
</tr>
<tr>
  <td>ADC1</td>
  <td>Pin number20</td>
</tr>
</tbody>
</table>

<table class="EC800N" >
<thead>
<tr>
  <th>ADC number</th>
  <th>Pin correspondence</th>
</tr>
</thead>
<tbody>
<tr>
  <td>ADC0</td>
  <td>Pin number9</td>
</tr>
</tbody>
</table>

<table class="EC600U" >
<thead>
<tr>
  <th>ADC number</th>
  <th>Pin correspondence</th>
</tr>
</thead>
<tbody>
<tr>
  <td>ADC0</td>
  <td>Pin number19</td>
</tr>
<tr>
  <td>ADC1</td>
  <td>Pin number20</td>
</tr>
<tr>
  <td>ADC2</td>
  <td>Pin number113</td>
</tr>
<tr>
  <td>ADC3</td>
  <td>Pin number114</td>
</tr>
</tbody>
</table>

<table class="EC200U" >
<thead>
<tr>
  <th>ADC number</th>
  <th>Pin correspondence</th>
</tr>
</thead>
<tbody>
<tr>
  <td>ADC0</td>
  <td>Pin number45</td>
</tr>
<tr>
  <td>ADC1</td>
  <td>Pin number44</td>
</tr>
<tr>
  <td>ADC2</td>
  <td>Pin number43</td>
</tr>
</tbody>
</table>

<table class="EC200A" >
<thead>
<tr>
  <th>ADC number</th>
  <th>Pin correspondence</th>
</tr>
</thead>
<tbody>
<tr>
  <td>ADC0</td>
  <td>Pin number45</td>
</tr>
<tr>
  <td>ADC1</td>
  <td>Pin number44</td>
</tr>
</tbody>
</table>

<table class="EC200A-V1" >
<thead>
<tr>
  <th>ADC number</th>
  <th>Pin correspondence</th>
</tr>
</thead>
<tbody>
<tr>
  <td>ADC0</td>
  <td>Pin number45</td>
</tr>
<tr>
  <td>ADC1</td>
  <td>Pin number44</td>
</tr>
</tbody>
</table>

<table class="UC200A" >
<thead>
<tr>
  <th>ADC number</th>
  <th>Pin correspondence</th>
</tr>
</thead>
<tbody>
<tr>
  <td>ADC0</td>
  <td>Pin number45</td>
</tr>
<tr>
  <td>ADC1</td>
  <td>Pin number44</td>
</tr>
</tbody>
</table>

<table class="BG95" >
<thead>
<tr>
  <th>ADC number</th>
  <th>Pin correspondence</th>
</tr>
</thead>
<tbody>
<tr>
  <td>ADC0</td>
  <td>Pin number24</td>
</tr>
</tbody>
</table>

<table class="EG915U" >
<thead>
<tr>
  <th>ADC number</th>
  <th>Pin correspondence</th>
</tr>
</thead>
<tbody>
<tr>
  <td>ADC0</td>
  <td>Pin number24</td>
</tr>
<tr>
  <td>ADC1</td>
  <td>Pin number2</td>
</tr>
</tbody>
</table>

<table class="EC800M" >
<thead>
<tr>
  <th>ADC number</th>
  <th>Pin correspondence</th>
</tr>
</thead>
<tbody>
<tr>
  <td>ADC0</td>
  <td>Pin number9</td>
</tr>
<tr>
  <td>ADC1</td>
  <td>Pin number96</td>
</tr>
</tbody>
</table>

<table class="EG810M" >
<thead>
<tr>
  <th>ADC number</th>
  <th>Pin correspondence</th>
</tr>
</thead>
<tbody>
<tr>
  <td>ADC0</td>
  <td>Pin number9</td>
</tr>
<tr>
  <td>ADC1</td>
  <td>Pin number96</td>
</tr>
</tbody>
</table>

<table class="EG912N" >
<thead>
<tr>
  <th>ADC number</th>
  <th>Pin correspondence</th>
</tr>
</thead>
<tbody>
<tr>
  <td>ADC0</td>
  <td>Pin number24</td>
</tr>
<tr>
  <td>ADC1</td>
  <td>Pin number2</td>
</tr>
</tbody>
</table>

<table class="EC600E" >
<thead>
<tr>
  <th>ADC number</th>
  <th>Pin correspondence</th>
</tr>
</thead>
<tbody>
<tr>
  <td>ADC0</td>
  <td>Pin number19</td>
</tr>
<tr>
  <td>ADC1</td>
  <td>Pin number20</td>
</tr>
</tbody>
</table>

<table class="EC800E" >
<thead>
<tr>
  <th>ADC number</th>
  <th>Pin correspondence</th>
</tr>
</thead>
<tbody>
<tr>
  <td>ADC0</td>
  <td>Pin number9</td>
</tr>
<tr>
  <td>ADC1</td>
  <td>Pin number96</td>
</tr>
</tbody>
</table>

<table class="EC600G" >
<thead>
<tr>
  <th>ADC number</th>
  <th>Pin correspondence</th>
</tr>
</thead>
<tbody>
<tr>
  <td>ADC0</td>
  <td>Pin number19</td>
</tr>
<tr>
  <td>ADC1</td>
  <td>Pin number20</td>
</tr>
<tr>
  <td>ADC2</td>
  <td>Pin number113</td>
</tr>
<tr>
  <td>ADC3</td>
  <td>Pin number114</td>
</tr>
</tbody>
</table>

<table class="EC800G" >
<thead>
<tr>
  <th>ADC number</th>
  <th>Pin correspondence</th>
</tr>
</thead>
<tbody>
<tr>
  <td>ADC0</td>
  <td>Pin number9</td>
</tr>
<tr>
  <td>ADC1</td>
  <td>Pin number96</td>
</tr>
</tbody>
</table>

<table class="EG912U" >
<thead>
<tr>
  <th>ADC number</th>
  <th>Pin correspondence</th>
</tr>
</thead>
<tbody>
<tr>
  <td>ADC0</td>
  <td>Pin number24</td>
</tr>
<tr>
  <td>ADC1</td>
  <td>Pin number2</td>
</tr>
</tbody>
</table>

<table class="EC600K" >
<thead>
<tr>
  <th>ADC number</th>
  <th>Pin correspondence</th>
</tr>
</thead>
<tbody>
<tr>
  <td>ADC0</td>
  <td>Pin number19</td>
</tr>
<tr>
  <td>ADC1</td>
  <td>Pin number20</td>
</tr>
</tbody>
</table>

<table class="EC800K" >
<thead>
<tr>
  <th>ADC number</th>
  <th>Pin correspondence</th>
</tr>
</thead>
<tbody>
<tr>
  <td>ADC0</td>
  <td>Pin number9</td>
</tr>
<tr>
  <td>ADC1</td>
  <td>Pin number96</td>
</tr>
</tbody>
</table>

<table class="EG800K" >
<thead>
<tr>
  <th>ADC number</th>
  <th>Pin correspondence</th>
</tr>
</thead>
<tbody>
<tr>
  <td>ADC0</td>
  <td>Pin number9</td>
</tr>
<tr>
  <td>ADC1</td>
  <td>Pin number96</td>
</tr>
</tbody>
</table>

<table class="EG800AK" >
<thead>
<tr>
  <th>ADC number</th>
  <th>Pin correspondence</th>
</tr>
</thead>
<tbody>
<tr>
  <td>ADC0</td>
  <td>Pin number9</td>
</tr>
<tr>
  <td>ADC1</td>
  <td>Pin number96</td>
</tr>
</tbody>
</table>

<table class="EG800Z" >
<thead>
<tr>
  <th>ADC number</th>
  <th>Pin correspondence</th>
</tr>
</thead>
<tbody>
<tr>
  <td>ADC0</td>
  <td>Pin number9</td>
</tr>
<tr>
  <td>ADC1</td>
  <td>Pin number96</td>
</tr>
</tbody>
</table>

<table class="FCM360W" >
<thead>
<tr>
  <th>ADC number</th>
  <th>Pin correspondence</th>
</tr>
</thead>
<tbody>
<tr>
  <td>ADC0</td>
  <td>Pin number8</td>
</tr>
<tr>
  <td>ADC1</td>
  <td>Pin number7</td>
</tr>
<tr>
  <td>ADC2</td>
  <td>Pin number6</td>
</tr>
</tbody>
</table>

<table class="BC32" >
<thead>
<tr>
  <th>ADC number</th>
  <th>Pin correspondence</th>
</tr>
</thead>
<tbody>
<tr>
  <td>ADC0</td>
  <td>Pin number2</td>
</tr>
</tbody>
</table>

<table class="BC92" >
<thead>
<tr>
  <th>ADC number</th>
  <th>Pin correspondence</th>
</tr>
</thead>
<tbody>
<tr>
  <td>ADC0</td>
  <td>Pin number2</td>
</tr>
</tbody>
</table>

<table class="EG915N" >
<thead>
<tr>
  <th>ADC number</th>
  <th>Pin correspondence</th>
</tr>
</thead>
<tbody>
<tr>
  <td>ADC0</td>
  <td>Pin number24</td>
</tr>
<tr>
  <td>ADC1</td>
  <td>Pin number2</td>
</tr>
</tbody>
</table>

<table class="EC800Z" >
<thead>
<tr>
  <th>ADC number</th>
  <th>Pin correspondence</th>
</tr>
</thead>
<tbody>
<tr>
  <td>ADC0</td>
  <td>Pin number9</td>
</tr>
<tr>
  <td>ADC1</td>
  <td>Pin number96</td>
</tr>
</tbody>
</table>

<table class="BG950S" >
<thead>
<tr>
  <th>ADC number</th>
  <th>Pin correspondence</th>
</tr>
</thead>
<tbody>
<tr>
  <td>ADC0</td>
  <td>Pin number24</td>
</tr>
<tr>
  <td>ADC1</td>
  <td>Pin number2</td>
</tr>
</tbody>
</table>

</div>

### ADC.close

```python
ADC.close()
```

**Return Value:**

 `0` - Successful execution

 `-1` - Failed execution

## Constants

<div class="table-cons-QuecPython">
<table class="EC600U" >
<thead>
<tr>
  <th>Constant</th>
  <th>Description</th>
</tr>
</thead>
<tbody>
<tr>
  <td>ADC.ADC0</td>
  <td>ADC channel 0</td>
</tr>
<tr>
  <td>ADC.ADC1</td>
  <td>ADC channel 1</td>
</tr>
<tr>
  <td>ADC.ADC2</td>
  <td>ADC channel 2</td>
</tr>
<tr>
  <td>ADC.ADC3</td>
  <td>ADC channel 3</td>
</tr>
</tbody>
</table>

<table class="EC200U" >
<thead>
<tr>
  <th>Constant</th>
  <th>Description</th>
</tr>
</thead>
<tbody>
<tr>
  <td>ADC.ADC0</td>
  <td>ADC channel 0</td>
</tr>
<tr>
  <td>ADC.ADC1</td>
  <td>ADC channel 1</td>
</tr>
<tr>
  <td>ADC.ADC2</td>
  <td>ADC channel 2</td>
</tr>
</tbody>
</table>

<table class="BC25PA" >
<thead>
<tr>
  <th>Constant</th>
  <th>Description</th>
</tr>
</thead>
<tbody>
<tr>
  <td>ADC.ADC0</td>
  <td>ADC channel 0</td>
</tr>
</tbody>
</table>

<table class="EC800N" >
<thead>
<tr>
  <th>Constant</th>
  <th>Description</th>
</tr>
</thead>
<tbody>
<tr>
  <td>ADC.ADC0</td>
  <td>ADC channel 0</td>
</tr>
</tbody>
</table>

<table class="BG95" >
<thead>
<tr>
  <th>Constant</th>
  <th>Description</th>
</tr>
</thead>
<tbody>
<tr>
  <td>ADC.ADC0</td>
  <td>ADC channel 0</td>
</tr>
</tbody>
</table>

<table class="EC200A" >
<thead>
<tr>
  <th>Constant</th>
  <th>Description</th>
</tr>
</thead>
<tbody>
<tr>
  <td>ADC.ADC0</td>
  <td>ADC channel 0</td>
</tr>
<tr>
  <td>ADC.ADC1</td>
  <td>ADC channel 1</td>
</tr>
</tbody>
</table>

<table class="EC200A-V1" >
<thead>
<tr>
  <th>Constant</th>
  <th>Description</th>
</tr>
</thead>
<tbody>
<tr>
  <td>ADC.ADC0</td>
  <td>ADC channel 0</td>
</tr>
<tr>
  <td>ADC.ADC1</td>
  <td>ADC channel 1</td>
</tr>
</tbody>
</table>

<table class="UC200A" >
<thead>
<tr>
  <th>Constant</th>
  <th>Description</th>
</tr>
</thead>
<tbody>
<tr>
  <td>ADC.ADC0</td>
  <td>ADC channel 0</td>
</tr>
<tr>
  <td>ADC.ADC1</td>
  <td>ADC channel 1</td>
</tr>
</tbody>
</table>

<table class="EG800K" >
<thead>
<tr>
  <th>Constant</th>
  <th>Description</th>
</tr>
</thead>
<tbody>
<tr>
  <td>ADC.ADC0</td>
  <td>ADC channel 0</td>
</tr>
<tr>
  <td>ADC.ADC1</td>
  <td>ADC channel 1</td>
</tr>
</tbody>
</table>

<table class="EG800AK" >
<thead>
<tr>
  <th>Constant</th>
  <th>Description</th>
</tr>
</thead>
<tbody>
<tr>
  <td>ADC.ADC0</td>
  <td>ADC channel 0</td>
</tr>
<tr>
  <td>ADC.ADC1</td>
  <td>ADC channel 1</td>
</tr>
</tbody>
</table>

<table class="EG800Z" >
<thead>
<tr>
  <th>Constant</th>
  <th>Description</th>
</tr>
</thead>
<tbody>
<tr>
  <td>ADC.ADC0</td>
  <td>ADC channel 0</td>
</tr>
<tr>
  <td>ADC.ADC1</td>
  <td>ADC channel 1</td>
</tr>
</tbody>
</table>

<table class="EG810M" >
<thead>
<tr>
  <th>Constant</th>
  <th>Description</th>
</tr>
</thead>
<tbody>
<tr>
  <td>ADC.ADC0</td>
  <td>ADC channel 0</td>
</tr>
<tr>
  <td>ADC.ADC1</td>
  <td>ADC channel 1</td>
</tr>
</tbody>
</table>

<table class="EC600M" >
<thead>
<tr>
  <th>Constant</th>
  <th>Description</th>
</tr>
</thead>
<tbody>
<tr>
  <td>ADC.ADC0</td>
  <td>ADC channel 0</td>
</tr>
<tr>
  <td>ADC.ADC1</td>
  <td>ADC channel 1</td>
</tr>
</tbody>
</table>

<table class="EG915U" >
<thead>
<tr>
  <th>Constant</th>
  <th>Description</th>
</tr>
</thead>
<tbody>
<tr>
  <td>ADC.ADC0</td>
  <td>ADC channel 0</td>
</tr>
<tr>
  <td>ADC.ADC1</td>
  <td>ADC channel 1</td>
</tr>
</tbody>
</table>

<table class="EC800M" >
<thead>
<tr>
  <th>Constant</th>
  <th>Description</th>
</tr>
</thead>
<tbody>
<tr>
  <td>ADC.ADC0</td>
  <td>ADC channel 0</td>
</tr>
<tr>
  <td>ADC.ADC1</td>
  <td>ADC channel 1</td>
</tr>
</tbody>
</table>

<table class="EG912N" >
<thead>
<tr>
  <th>Constant</th>
  <th>Description</th>
</tr>
</thead>
<tbody>
<tr>
  <td>ADC.ADC0</td>
  <td>ADC channel 0</td>
</tr>
<tr>
  <td>ADC.ADC1</td>
  <td>ADC channel 1</td>
</tr>
</tbody>
</table>

<table class="EC600E" >
<thead>
<tr>
  <th>Constant</th>
  <th>Description</th>
</tr>
</thead>
<tbody>
<tr>
  <td>ADC.ADC0</td>
  <td>ADC channel 0</td>
</tr>
<tr>
  <td>ADC.ADC1</td>
  <td>ADC channel 1</td>
</tr>
</tbody>
</table>

<table class="EC800E" >
<thead>
<tr>
  <th>Constant</th>
  <th>Description</th>
</tr>
</thead>
<tbody>
<tr>
  <td>ADC.ADC0</td>
  <td>ADC channel 0</td>
</tr>
<tr>
  <td>ADC.ADC1</td>
  <td>ADC channel 1</td>
</tr>
</tbody>
</table>

<table class="EC600G" >
<thead>
<tr>
  <th>Constant</th>
  <th>Description</th>
</tr>
</thead>
<tbody>
<tr>
  <td>ADC.ADC0</td>
  <td>ADC channel 0</td>
</tr>
<tr>
  <td>ADC.ADC1</td>
  <td>ADC channel 1</td>
</tr>
</tbody>
</table>

<table class="EC800G" >
<thead>
<tr>
  <th>Constant</th>
  <th>Description</th>
</tr>
</thead>
<tbody>
<tr>
  <td>ADC.ADC0</td>
  <td>ADC channel 0</td>
</tr>
<tr>
  <td>ADC.ADC1</td>
  <td>ADC channel 1</td>
</tr>
</tbody>
</table>

<table class="EG912U" >
<thead>
<tr>
  <th>Constant</th>
  <th>Description</th>
</tr>
</thead>
<tbody>
<tr>
  <td>ADC.ADC0</td>
  <td>ADC channel 0</td>
</tr>
<tr>
  <td>ADC.ADC1</td>
  <td>ADC channel 1</td>
</tr>
</tbody>
</table>

<table class="EC600K" >
<thead>
<tr>
  <th>Constant</th>
  <th>Description</th>
</tr>
</thead>
<tbody>
<tr>
  <td>ADC.ADC0</td>
  <td>ADC channel 0</td>
</tr>
<tr>
  <td>ADC.ADC1</td>
  <td>ADC channel 1</td>
</tr>
</tbody>
</table>

<table class="EC800K" >
<thead>
<tr>
  <th>Constant</th>
  <th>Description</th>
</tr>
</thead>
<tbody>
<tr>
  <td>ADC.ADC0</td>
  <td>ADC channel 0</td>
</tr>
<tr>
  <td>ADC.ADC1</td>
  <td>ADC channel 1</td>
</tr>
</tbody>
</table>

<table class="FCM360W" >
<thead>
<tr>
  <th>Constant</th>
  <th>Description</th>
</tr>
</thead>
<tbody>
<tr>
  <td>ADC.ADC0</td>
  <td>ADC channel 0</td>
</tr>
<tr>
  <td>ADC.ADC1</td>
  <td>ADC channel 1</td>
</tr>
<tr>
  <td>ADC.ADC2</td>
  <td>ADC channel 2</td>
</tr>
</tbody>
</table>

<table class="BC32" >
<thead>
<tr>
  <th>Constant</th>
  <th>Description</th>
</tr>
</thead>
<tbody>
<tr>
  <td>ADC.ADC0</td>
  <td>ADC channel 0</td>
</tr>
</tbody>
</table>

<table class="BC92" >
<thead>
<tr>
  <th>Constant</th>
  <th>Description</th>
</tr>
</thead>
<tbody>
<tr>
  <td>ADC.ADC0</td>
  <td>ADC channel 0</td>
</tr>
</tbody>
</table>

<table class="EG915N" >
<thead>
<tr>
  <th>Constant</th>
  <th>Description</th>
</tr>
</thead>
<tbody>
<tr>
  <td>ADC.ADC0</td>
  <td>ADC channel 0</td>
</tr>
<tr>
  <td>ADC.ADC1</td>
  <td>ADC channel 1</td>
</tr>
</tbody>
</table>

<table class="EC800Z" >
<thead>
<tr>
  <th>Constant</th>
  <th>Description</th>
</tr>
</thead>
<tbody>
<tr>
  <td>ADC.ADC0</td>
  <td>ADC channel 0</td>
</tr>
<tr>
  <td>ADC.ADC1</td>
  <td>ADC channel 1</td>
</tr>
</tbody>
</table>

<table class="BG950S" >
<thead>
<tr>
  <th>Constant</th>
  <th>Description</th>
</tr>
</thead>
<tbody>
<tr>
  <td>ADC.ADC0</td>
  <td>ADC channel 0</td>
</tr>
<tr>
  <td>ADC.ADC1</td>
  <td>ADC channel 1</td>
</tr>
</tbody>
</table>

</div>