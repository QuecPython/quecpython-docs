# KeyPad - Matrix Keyboard

This class provides the matrix keyboard interface.



## Constructor

### `machine.KeyPad`

```python
class machine.KeyPad(row,col)
```

**Parameter:**

- `row` - Integer type. Row number. It shall be greater than 0 and cannot exceed the maximum value supported by the module.
- `col` - Integer type. Column number. It shall be greater than 0. The value cannot exceed the maximum value supported by the module.

> If you do not set the row and column value, the default value is 4X4.

<div class="table-cons-QuecPython">
<table class="EC200U">
<thead>
<tr>
  <th>Maximum Row</th>
  <th>Maximum Column</th>
</tr>
</thead>
<tbody>
<tr>
  <td>6（series only suprrort 4 Row,see Pin Correspondences）</td>
  <td>4</td>
</tr>
</tbody>
</table>

<table class="EC600U">
<thead>
<tr>
  <th>Maximum Row</th>
  <th>Maximum Column</th>
</tr>
</thead>
<tbody>
<tr>
  <td>6</td>
  <td>6</td>
</tr>
</tbody>
</table>

<table class="EC600M">
<thead>
<tr>
  <th>Maximum Row</th>
  <th>Maximum Column</th>
</tr>
</thead>
<tbody>
<tr>
  <td>5</td>
  <td>5</td>
</tr>
</tbody>
</table>

<table class="EC800M">
<thead>
<tr>
  <th>Maximum Row</th>
  <th>Maximum Column</th>
</tr>
</thead>
<tbody>
<tr>
  <td>5</td>
  <td>5</td>
</tr>
</tbody>
</table>

<table class="EG810M">
<thead>
<tr>
  <th>Maximum Row</th>
  <th>Maximum Column</th>
</tr>
</thead>
<tbody>
<tr>
  <td>5（EG810MEU series only suprrort 3 Row,see Pin Correspondences）</td>
  <td>5（EG810MEU series only suprrort 3 Row,see Pin Correspondences）</td>
</tr>
</tbody>
</table>

<table class="EG912N">
<thead>
<tr>
  <th>Maximum Row</th>
  <th>Maximum Column</th>
</tr>
</thead>
<tbody>
<tr>
  <td>3</td>
  <td>3</td>
</tr>
</tbody>
</table>

<table class="EG915N">
<thead>
<tr>
  <th>Maximum Row</th>
  <th>Maximum Column</th>
</tr>
</thead>
<tbody>
<tr>
  <td>4</td>
  <td>4</td>
</tr>
</tbody>
</table>

<table class="EC600K">
<thead>
<tr>
  <th>Maximum Row</th>
  <th>Maximum Column</th>
</tr>
</thead>
<tbody>
<tr>
  <td>3</td>
  <td>3</td>
</tr>
</tbody>
</table>

<table class="EC800K">
<thead>
<tr>
  <th>Maximum Row</th>
  <th>Maximum Column</th>
</tr>
</thead>
<tbody>
<tr>
  <td>5</td>
  <td>5</td>
</tr>
</tbody>
</table>

<table class="EG800K">
<thead>
<tr>
  <th>Maximum Row</th>
  <th>Maximum Column</th>
</tr>
</thead>
<tbody>
<tr>
  <td>4</td>
  <td>4</td>
</tr>
</tbody>
</table>

<table class="EG800AK">
<thead>
<tr>
  <th>Maximum Row</th>
  <th>Maximum Column</th>
</tr>
</thead>
<tbody>
<tr>
  <td>5</td>
  <td>5</td>
</tr>
</tbody>
</table>

<table class="EC600G">
<thead>
<tr>
  <th>Maximum Row</th>
  <th>Maximum Column</th>
</tr>
</thead>
<tbody>
<tr>
  <td>6</td>
  <td>5</td>
</tr>
</tbody>
</table>

<table class="EC800G">
<thead>
<tr>
  <th>Maximum Row</th>
  <th>Maximum Column</th>
</tr>
</thead>
<tbody>
<tr>
  <td>5</td>
  <td>5</td>
</tr>
</tbody>
</table>

<table class="EG800P">
<thead>
<tr>
  <th>Maximum Row</th>
  <th>Maximum Column</th>
</tr>
</thead>
<tbody>
<tr>
  <td>3</td>
  <td>3</td>
</tr>
</tbody>
</table>

</div>

**KeyPad Pin Correspondences:**

> When part of pins are used, you shall connect the keyboard and the pin according to row and column numbers in ascending order. For example, for EC600M, when a 2x2 matrix keyboard is used, the hardware will use 49, 51 and 48, 50 pins.  

<div class="table-QuecPython">
<table class="EC600M">
<thead>
<tr>
  <th>Row number</th>
  <th>Row PIN</th>
  <th>Column number</th>
  <th>Column PIN</th>
</tr>
</thead>
<tbody>
<tr>
  <td>Row number0</td>
  <td>pin49</td>
  <td>Column number0</td>
  <td>pin48</td>
</tr>
<tr>
  <td>Row number1</td>
  <td>pin51</td>
  <td>Column number1</td>
  <td>pin50</td>
</tr>
<tr>
  <td>Row number2</td>
  <td>pin53</td>
  <td>Column number2</td>
  <td>pin52</td>
</tr>
<tr>
  <td>Row number3</td>
  <td>pin55</td>
  <td>Column number3</td>
  <td>pin54</td>
</tr>
<tr>
  <td>Row number4</td>
  <td>pin56</td>
  <td>Column number4</td>
  <td>pin57</td>
</tr>
</tbody>
</table>

<table class="EC800M">
<thead>
<tr>
  <th>Row number</th>
  <th>Row PIN</th>
  <th>Column number</th>
  <th>Column PIN</th>
</tr>
</thead>
<tbody>
<tr>
  <td>Row number0</td>
  <td>pin86</td>
  <td>Column number0</td>
  <td>pin87</td>
</tr>
<tr>
  <td>Row number1</td>
  <td>pin76</td>
  <td>Column number1</td>
  <td>pin77</td>
</tr>
<tr>
  <td>Row number2</td>
  <td>pin85</td>
  <td>Column number2</td>
  <td>pin84</td>
</tr>
<tr>
  <td>Row number3</td>
  <td>pin82</td>
  <td>Column number3</td>
  <td>pin83</td>
</tr>
<tr>
  <td>Row number4</td>
  <td>pin74</td>
  <td>Column number4</td>
  <td>pin75</td>
</tr>
</tbody>
</table>

<table class="EG810M">
<thead>
<tr>
  <th>Row number</th>
  <th>Row PIN</th>
  <th>Column number</th>
  <th>Column PIN</th>
</tr>
</thead>
<tbody>
<tr>
  <td>Row number0</td>
  <td>pin86(EG810MEU series not support)</td>
  <td>Column number0</td>
  <td>pin87(EG810MEU series not support)</td>
</tr>
<tr>
  <td>Row number1</td>
  <td>pin76</td>
  <td>Column number1</td>
  <td>pin77</td>
</tr>
<tr>
  <td>Row number2</td>
  <td>pin85(EG810MEU series not support)</td>
  <td>Column number2</td>
  <td>pin84(EG810MEU series not support)</td>
</tr>
<tr>
  <td>Row number3</td>
  <td>pin82</td>
  <td>Column number3</td>
  <td>pin83</td>
</tr>
<tr>
  <td>Row number4</td>
  <td>pin74</td>
  <td>Column number4</td>
  <td>pin75</td>
</tr>
</tbody>
</table>

<table class="EG912N">
<thead>
<tr>
  <th>Row number</th>
  <th>Row PIN</th>
  <th>Column number</th>
  <th>Column PIN</th>
</tr>
</thead>
<tbody>
<tr>
  <td>Row number1</td>
  <td>pin20</td>
  <td>Column number2</td>
  <td>pin105</td>
</tr>
<tr>
  <td>Row number2</td>
  <td>pin16</td>
  <td>Column number3</td>
  <td>pin21</td>
</tr>
<tr>
  <td>Row number3</td>
  <td>pin116</td>
  <td>Column number4</td>
  <td>pin1</td>
</tr>
</tr>
</tbody>
</table>

<table class="EC200U">
<thead>
<tr>
  <th>Row number</th>
  <th>Row PIN</th>
  <th>Column number</th>
  <th>Column PIN</th>
</tr>
</thead>
<tbody>
<tr>
  <td>Row number0</td>
  <td>pin83</td>
  <td>Column number0</td>
  <td>pin115</td>
</tr>
<tr>
  <td>Row number1</td>
  <td>pin84</td>
  <td>Column number1</td>
  <td>pin78</td>
</tr>
<tr>
  <td>Row number2</td>
  <td>pin113</td>
  <td>Column number2</td>
  <td>pin79</td>
</tr>
<tr>
  <td>Row number3</td>
  <td>pin114</td>
  <td>Column number3</td>
  <td>pin80</td>
</tr>
<tr>
  <td>Row number4</td>
  <td>pin81(EC200UXXAA series not support)</td>
  <td>Column number4</td>
  <td>/</td>
</tr>
<tr>
  <td>Row number5</td>
  <td>pin82(EC200UXXAA series not support)</td>
  <td>Column number5</td>
  <td>/</td>
</tr>
</tbody>
</table>

<table class="EC600U">
<thead>
<tr>
  <th>Row number</th>
  <th>Row PIN</th>
  <th>Column number</th>
  <th>Column PIN</th>
</tr>
</thead>
<tbody>
<tr>
  <td>Row number0</td>
  <td>pin105</td>
  <td>Column number0</td>
  <td>pin55</td>
</tr>
<tr>
  <td>Row number1</td>
  <td>pin106</td>
  <td>Column number1</td>
  <td>pin129</td>
</tr>
<tr>
  <td>Row number2</td>
  <td>pin107</td>
  <td>Column number2</td>
  <td>pin128</td>
</tr>
<tr>
  <td>Row number3</td>
  <td>pin108</td>
  <td>Column number3</td>
  <td>pin127</td>
</tr>
<tr>
  <td>Row number4</td>
  <td>pin104</td>
  <td>Column number4</td>
  <td>pin126</td>
</tr>
<tr>
  <td>Row number5</td>
  <td>pin103</td>
  <td>Column number5</td>
  <td>pin125</td>
</tr>
</tbody>
</table>

<table class="EG915N">
<thead>
<tr>
  <th>Row number</th>
  <th>Row PIN</th>
  <th>Column number</th>
  <th>Column PIN</th>
</tr>
</thead>
<tbody>
<tr>
  <td>Row number0</td>
  <td>pin39</td>
  <td>Column number0</td>
  <td>pin83</td>
</tr>
<tr>
  <td>Row number1</td>
  <td>pin20</td>
  <td>Column number2</td>
  <td>pin28</td>
</tr>
<tr>
  <td>Row number2</td>
  <td>pin27</td>
  <td>Column number3</td>
  <td>pin25</td>
</tr>
<tr>
  <td>Row number3</td>
  <td>pin26</td>
  <td>Column number4</td>
  <td>pin1</td>
</tr>
</tbody>
</table>

<table class="EC600K">
<thead>
<tr>
  <th>Row number</th>
  <th>Row PIN</th>
  <th>Column number</th>
  <th>Column PIN</th>
</tr>
</thead>
<tbody>
<tr>
  <td>Row number0</td>
  <td>pin53</td>
  <td>Column number0</td>
  <td>pin52</td>
</tr>
<tr>
  <td>Row number1</td>
  <td>pin49</td>
  <td>Column number1</td>
  <td>pin54</td>
</tr>
<tr>
  <td>Row number2</td>
  <td>pin55</td>
  <td>Column number2</td>
  <td>pin50</td>
</tr>
</tbody>
</table>

<table class="EC800K">
<thead>
<tr>
  <th>Row number</th>
  <th>Row PIN</th>
  <th>Column number</th>
  <th>Column PIN</th>
</tr>
</thead>
<tbody>
<tr>
  <td>Row number0</td>
  <td>pin74</td>
  <td>Column number0</td>
  <td>pin75</td>
</tr>
<tr>
  <td>Row number1</td>
  <td>pin76</td>
  <td>Column number1</td>
  <td>pin77</td>
</tr>
<tr>
  <td>Row number2</td>
  <td>pin86</td>
  <td>Column number2</td>
  <td>pin87</td>
</tr>
<tr>
  <td>Row number3</td>
  <td>pin82</td>
  <td>Column number3</td>
  <td>pin81</td>
</tr>
<tr>
  <td>Row number4</td>
  <td>pin29</td>
  <td>Column number4</td>
  <td>pin28</td>
</tr>
</tbody>
</table>

<table class="EG800K">
<thead>
<tr>
  <th>Row number</th>
  <th>Row PIN</th>
  <th>Column number</th>
  <th>Column PIN</th>
</tr>
</thead>
<tbody>
<tr>
  <td>Row number0</td>
  <td>pin74</td>
  <td>Column number0</td>
  <td>pin75</td>
</tr>
<tr>
  <td>Row number1</td>
  <td>pin76</td>
  <td>Column number1</td>
  <td>pin77</td>
</tr>
<tr>
  <td>Row number2</td>
  <td>pin86</td>
  <td>Column number2</td>
  <td>pin87</td>
</tr>
<tr>
  <td>Row number3</td>
  <td>pin82</td>
  <td>Column number3</td>
  <td>pin81</td>
</tr>
</tbody>
</table>

<table class="EG800AK">
<thead>
<tr>
  <th>Row number</th>
  <th>Row PIN</th>
  <th>Column number</th>
  <th>Column PIN</th>
</tr>
</thead>
<tbody>
<tr>
  <td>Row number0</td>
  <td>pin74</td>
  <td>Column number0</td>
  <td>pin75</td>
</tr>
<tr>
  <td>Row number1</td>
  <td>pin76</td>
  <td>Column number1</td>
  <td>pin77</td>
</tr>
<tr>
  <td>Row number2</td>
  <td>pin86</td>
  <td>Column number2</td>
  <td>pin87</td>
</tr>
<tr>
  <td>Row number3</td>
  <td>pin82</td>
  <td>Column number3</td>
  <td>pin81</td>
</tr>
<tr>
  <td>Row number4</td>
  <td>pin29</td>
  <td>Column number4</td>
  <td>pin28</td>
</tr>
</tbody>
</table>

<table class="EC600G">
<thead>
<tr>
  <th>Row number</th>
  <th>Row PIN</th>
  <th>Column number</th>
  <th>Column PIN</th>
</tr>
</thead>
<tbody>
<tr>
  <td>Row number0</td>
  <td>pin105</td>
  <td>Column number0</td>
  <td>pin129</td>
</tr>
<tr>
  <td>Row number1</td>
  <td>pin106</td>
  <td>Column number1</td>
  <td>pin128</td>
</tr>
<tr>
  <td>Row number2</td>
  <td>pin107</td>
  <td>Column number2</td>
  <td>pin127</td>
</tr>
<tr>
  <td>Row number3</td>
  <td>pin108</td>
  <td>Column number3</td>
  <td>pin126</td>
</tr>
<tr>
  <td>Row number4</td>
  <td>pin104</td>
  <td>Column number4</td>
  <td>pin125</td>
</tr>
<tr>
  <td>Row number5</td>
  <td>pin103</td>
  <td>Column number5</td>
  <td>/</td>
</tr>
</tbody>
</table>

<table class="EC800G">
<thead>
<tr>
  <th>Row number</th>
  <th>Row PIN</th>
  <th>Column number</th>
  <th>Column PIN</th>
</tr>
</thead>
<tbody>
<tr>
  <td>Row number0</td>
  <td>pin83</td>
  <td>Column number0</td>
  <td>pin82</td>
</tr>
<tr>
  <td>Row number1</td>
  <td>pin86</td>
  <td>Column number1</td>
  <td>pin87</td>
</tr>
<tr>
  <td>Row number2</td>
  <td>pin76</td>
  <td>Column number2</td>
  <td>pin77</td>
</tr>
<tr>
  <td>Row number3</td>
  <td>pin74</td>
  <td>Column number3</td>
  <td>pin75</td>
</tr>
<tr>
  <td>Row number4</td>
  <td>pin85</td>
  <td>Column number4</td>
  <td>pin84</td>
</tr>
</tbody>
</table>

<table class="EG800P">
<thead>
<tr>
  <th>Row number</th>
  <th>Row PIN</th>
  <th>Column number</th>
  <th>Column PIN</th>
</tr>
</thead>
<tbody>
<tr>
  <td>Row number0</td>
  <td>pin86</td>
  <td>Column number0</td>
  <td>pin87</td>
</tr>
<tr>
  <td>Row number1</td>
  <td>pin85</td>
  <td>Column number1</td>
  <td>pin84</td>
</tr>
<tr>
  <td>Row number2</td>
  <td>pin82</td>
  <td>Column number2</td>
  <td>pin83</td>
</tr>
</tbody>
</table>

</div>

**Example:**

```python
>>> # Creates a keypad object
>>> import machine
>>> keypad=machine.KeyPad(2,3)  # Sets a matrix keyboard with 2 rows and 3 columes
>>> keypad=machine.KeyPad()     # Default parameter. The default setting is a matrix keyboard with 4 rows and 4 columns
>>> keypad=machine.KeyPad(2)    # Sets the row value to 2. Default column Value. The default column value is 4. A matrix keyboard with 2 rows and 4 columes is initialized. 
```

## Methods

### `keypad.init`

```python
keypad.init()
```

This method initializes keypad settings.

**Return Value:**

`0` - Successful execution

`-1` - Failed execution

### `keypad.set_callback`

```python
keypad.set_callback(usrFun)
```

This method sets callback function. After the external keyboard button is connected to the module, this callback function will be triggered when the external keyboard button is pressed and released.

**Parameter:**

- `usrFun` - Matrix keyboard callback function. Prototype:

  ```
  usrFun(result_list)
  ```

  Parameter of callback function:

  - `result_list[0]`: Key status (1 indicates the button is pressed and 0 indicates the button is released).

  - `result_list[1]`: Row number 

  - `result_list[2]`: Column number

> for example,the EC600M module, suppose the key connect pin 49 and pin 52,if press the key, the parameter of callback is [1,0,2],if release the key, the parameter of callback is [0,0,2].

**Return Value:**

`0` - Successful execution

`-1` - Failed execution

### `keypad.deinit`

```python
keypad.deinit()
```

This method deinitializes to release initialized resources and callback function settings. 

**Return Value:**

`0` - Successful execution

`-1` - Failed execution

**Example:**

```python
import machine
import utime
is_loop = 1
keypad=machine.KeyPad()  
keypad.init()
def userfun(l_list):
    global is_loop 
    if  l_list[0] != 1 :
        is_loop = 0
        print('will exit')
    print(l_list)
keypad.set_callback(userfun)
loop_num = 0
while is_loop == 1 and loop_num < 10:
    utime.sleep(5)
    loop_num = loop_num +1
    print(" running..... ",is_loop,loop_num)
keypad.deinit()
print('exit!')
```
