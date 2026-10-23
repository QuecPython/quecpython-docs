# class KeyPad - 矩阵键盘

该类提供矩阵键盘接口。


## 构造函数

### `machine.KeyPad`

```python
class machine.KeyPad(row,col)
```

**参数描述：**

- `row` - 行，int类型，大于0，不超过平台支持最大值。
- `col` - 列，int类型，大于0，不超过平台支持最大值。

> 如果row和col均不设置,默认为4X4。

<div class="table-cons-QuecPython">
<table class="EC200U">
<thead>
<tr>
  <th>最大行</th>
  <th>最大列</th>
</tr>
</thead>
<tbody>
<tr>
  <td>6（EC200UXXAA系列只支持4行，见管脚说明）</td>
  <td>4</td>
</tr>
</tbody>
</table>

<table class="EC600U">
<thead>
<tr>
  <th>最大行</th>
  <th>最大列</th>
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
  <th>最大行</th>
  <th>最大列</th>
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
  <th>最大行</th>
  <th>最大列</th>
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
  <th>最大行</th>
  <th>最大列</th>
</tr>
</thead>
<tbody>
<tr>
  <td>5（EG810MEU只支持3行）</td>
  <td>5（EG810MEU只支持3行）</td>
</tr>
</tbody>
</table>

<table class="EG912N">
<thead>
<tr>
  <th>最大行</th>
  <th>最大列</th>
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
  <th>最大行</th>
  <th>最大列</th>
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
  <th>最大行</th>
  <th>最大列</th>
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
  <th>最大行</th>
  <th>最大列</th>
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
  <th>最大行</th>
  <th>最大列</th>
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
  <th>最大行</th>
  <th>最大列</th>
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
  <th>最大行</th>
  <th>最大列</th>
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
  <th>最大行</th>
  <th>最大列</th>
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
  <th>最大行</th>
  <th>最大列</th>
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

**KeyPad引脚对应关系：**

> 当不使用全部引脚时，接线按行列号从小到大顺序接线，比如EC600M使用2x2矩阵键盘时，硬件使用49、51和48、50引脚。

<div class="table-QuecPython">
<table class="EC600M">
<thead>
<tr>
  <th>行号</th>
  <th>行号引脚</th>
  <th>列号</th>
  <th>列号引脚</th>
</tr>
</thead>
<tbody>
<tr>
  <td>行号0</td>
  <td>引脚号49</td>
  <td>列号0</td>
  <td>引脚号48</td>
</tr>
<tr>
  <td>行号1</td>
  <td>引脚号51</td>
  <td>列号1</td>
  <td>引脚号50</td>
</tr>
<tr>
  <td>行号2</td>
  <td>引脚号53</td>
  <td>列号2</td>
  <td>引脚号52</td>
</tr>
<tr>
  <td>行号3</td>
  <td>引脚号55</td>
  <td>列号3</td>
  <td>引脚号54</td>
</tr>
<tr>
  <td>行号4</td>
  <td>引脚号56</td>
  <td>列号4</td>
  <td>引脚号57</td>
</tr>
</tbody>
</table>

<table class="EC800M">
<thead>
<tr>
  <th>行号</th>
  <th>行号引脚</th>
  <th>列号</th>
  <th>列号引脚</th>
</tr>
</thead>
<tbody>
<tr>
  <td>行号0</td>
  <td>引脚号86</td>
  <td>列号0</td>
  <td>引脚号87</td>
</tr>
<tr>
  <td>行号1</td>
  <td>引脚号76</td>
  <td>列号1</td>
  <td>引脚号77</td>
</tr>
<tr>
  <td>行号2</td>
  <td>引脚号85</td>
  <td>列号2</td>
  <td>引脚号84</td>
</tr>
<tr>
  <td>行号3</td>
  <td>引脚号82</td>
  <td>列号3</td>
  <td>引脚号83</td>
</tr>
<tr>
  <td>行号4</td>
  <td>引脚号74</td>
  <td>列号4</td>
  <td>引脚号75</td>
</tr>
</tbody>
</table>

<table class="EG810M">
<thead>
<tr>
  <th>行号</th>
  <th>行号引脚</th>
  <th>列号</th>
  <th>列号引脚</th>
</tr>
</thead>
<tbody>
<tr>
  <td>行号0</td>
  <td>引脚号86(EG810MEU不支持)</td>
  <td>列号0</td>
  <td>引脚号87(EG810MEU不支持)</td>
</tr>
<tr>
  <td>行号1</td>
  <td>引脚号76</td>
  <td>列号1</td>
  <td>引脚号77</td>
</tr>
<tr>
  <td>行号2</td>
  <td>引脚号85(EG810MEU不支持)</td>
  <td>列号2</td>
  <td>引脚号84(EG810MEU不支持)</td>
</tr>
<tr>
  <td>行号3</td>
  <td>引脚号82</td>
  <td>列号3</td>
  <td>引脚号83</td>
</tr>
<tr>
  <td>行号4</td>
  <td>引脚号74</td>
  <td>列号4</td>
  <td>引脚号75</td>
</tr>
</tbody>
</table>

<table class="EG912N">
<thead>
<tr>
  <th>行号</th>
  <th>行号引脚</th>
  <th>列号</th>
  <th>列号引脚</th>
</tr>
</thead>
<tbody>
<tr>
  <td>行号1</td>
  <td>引脚号20</td>
  <td>列号2</td>
  <td>引脚号105</td>
</tr>
<tr>
  <td>行号2</td>
  <td>引脚号16</td>
  <td>列号3</td>
  <td>引脚号21</td>
</tr>
<tr>
  <td>行号3</td>
  <td>引脚号116</td>
  <td>列号4</td>
  <td>引脚号1</td>
</tr>
</tr>
</tbody>
</table>

<table class="EC200U">
<thead>
<tr>
  <th>行号</th>
  <th>行号引脚</th>
  <th>列号</th>
  <th>列号引脚</th>
</tr>
</thead>
<tbody>
<tr>
  <td>行号0</td>
  <td>引脚号83</td>
  <td>列号0</td>
  <td>引脚号115</td>
</tr>
<tr>
  <td>行号1</td>
  <td>引脚号84</td>
  <td>列号1</td>
  <td>引脚号78</td>
</tr>
<tr>
  <td>行号2</td>
  <td>引脚号113</td>
  <td>列号2</td>
  <td>引脚号79</td>
</tr>
<tr>
  <td>行号3</td>
  <td>引脚号114</td>
  <td>列号3</td>
  <td>引脚号80</td>
</tr>
<tr>
  <td>行号4</td>
  <td>引脚号81(EC200UXXAA系列不支持)</td>
  <td>列号4</td>
  <td>/</td>
</tr>
<tr>
  <td>行号5</td>
  <td>引脚号82(EC200UXXAA系列不支持)</td>
  <td>列号5</td>
  <td>/</td>
</tr>
</tbody>
</table>

<table class="EC600U">
<thead>
<tr>
  <th>行号</th>
  <th>行号引脚</th>
  <th>列号</th>
  <th>列号引脚</th>
</tr>
</thead>
<tbody>
<tr>
  <td>行号0</td>
  <td>引脚号105</td>
  <td>列号0</td>
  <td>引脚号55</td>
</tr>
<tr>
  <td>行号1</td>
  <td>引脚号106</td>
  <td>列号1</td>
  <td>引脚号129</td>
</tr>
<tr>
  <td>行号2</td>
  <td>引脚号107</td>
  <td>列号2</td>
  <td>引脚号128</td>
</tr>
<tr>
  <td>行号3</td>
  <td>引脚号108</td>
  <td>列号3</td>
  <td>引脚号127</td>
</tr>
<tr>
  <td>行号4</td>
  <td>引脚号104</td>
  <td>列号4</td>
  <td>引脚号126</td>
</tr>
<tr>
  <td>行号5</td>
  <td>引脚号103</td>
  <td>列号5</td>
  <td>引脚号125</td>
</tr>
</tbody>
</table>

<table class="EG915N">
<thead>
<tr>
  <th>行号</th>
  <th>行号引脚</th>
  <th>列号</th>
  <th>列号引脚</th>
</tr>
</thead>
<tbody>
<tr>
  <td>行号0</td>
  <td>引脚号39</td>
  <td>列号0</td>
  <td>引脚号83</td>
</tr>
<tr>
  <td>行号1</td>
  <td>引脚号20</td>
  <td>列号2</td>
  <td>引脚号28</td>
</tr>
<tr>
  <td>行号2</td>
  <td>引脚号27</td>
  <td>列号3</td>
  <td>引脚号25</td>
</tr>
<tr>
  <td>行号3</td>
  <td>引脚号26</td>
  <td>列号4</td>
  <td>引脚号1</td>
</tr>
</tbody>
</table>

<table class="EC600K">
<thead>
<tr>
  <th>行号</th>
  <th>行号引脚</th>
  <th>列号</th>
  <th>列号引脚</th>
</tr>
</thead>
<tbody>
<tr>
  <td>行号0</td>
  <td>引脚号53</td>
  <td>列号0</td>
  <td>引脚号52</td>
</tr>
<tr>
  <td>行号1</td>
  <td>引脚号49</td>
  <td>列号1</td>
  <td>引脚号54</td>
</tr>
<tr>
  <td>行号2</td>
  <td>引脚号55</td>
  <td>列号2</td>
  <td>引脚号50</td>
</tr>
</tbody>
</table>

<table class="EC800K">
<thead>
<tr>
  <th>行号</th>
  <th>行号引脚</th>
  <th>列号</th>
  <th>列号引脚</th>
</tr>
</thead>
<tbody>
<tr>
  <td>行号0</td>
  <td>引脚号74</td>
  <td>列号0</td>
  <td>引脚号75</td>
</tr>
<tr>
  <td>行号1</td>
  <td>引脚号76</td>
  <td>列号1</td>
  <td>引脚号77</td>
</tr>
<tr>
  <td>行号2</td>
  <td>引脚号86</td>
  <td>列号2</td>
  <td>引脚号87</td>
</tr>
<tr>
  <td>行号3</td>
  <td>引脚号82</td>
  <td>列号3</td>
  <td>引脚号81</td>
</tr>
<tr>
  <td>行号4</td>
  <td>引脚号29</td>
  <td>列号4</td>
  <td>引脚号28</td>
</tr>
</tbody>
</table>

<table class="EG800K">
<thead>
<tr>
  <th>行号</th>
  <th>行号引脚</th>
  <th>列号</th>
  <th>列号引脚</th>
</tr>
</thead>
<tbody>
<tr>
  <td>行号0</td>
  <td>引脚号74</td>
  <td>列号0</td>
  <td>引脚号75</td>
</tr>
<tr>
  <td>行号1</td>
  <td>引脚号76</td>
  <td>列号1</td>
  <td>引脚号77</td>
</tr>
<tr>
  <td>行号2</td>
  <td>引脚号86</td>
  <td>列号2</td>
  <td>引脚号87</td>
</tr>
<tr>
  <td>行号3</td>
  <td>引脚号82</td>
  <td>列号3</td>
  <td>引脚号81</td>
</tr>
</tbody>
</table>

<table class="EG800AK">
<thead>
<tr>
  <th>行号</th>
  <th>行号引脚</th>
  <th>列号</th>
  <th>列号引脚</th>
</tr>
</thead>
<tbody>
<tr>
  <td>行号0</td>
  <td>引脚号74</td>
  <td>列号0</td>
  <td>引脚号75</td>
</tr>
<tr>
  <td>行号1</td>
  <td>引脚号76</td>
  <td>列号1</td>
  <td>引脚号77</td>
</tr>
<tr>
  <td>行号2</td>
  <td>引脚号86</td>
  <td>列号2</td>
  <td>引脚号87</td>
</tr>
<tr>
  <td>行号3</td>
  <td>引脚号82</td>
  <td>列号3</td>
  <td>引脚号81</td>
</tr>
<tr>
  <td>行号4</td>
  <td>引脚号29</td>
  <td>列号4</td>
  <td>引脚号28</td>
</tr>
</tbody>
</table>

<table class="EC600G">
<thead>
<tr>
  <th>行号</th>
  <th>行号引脚</th>
  <th>列号</th>
  <th>列号引脚</th>
</tr>
</thead>
<tbody>
<tr>
  <td>行号0</td>
  <td>引脚号105</td>
  <td>列号0</td>
  <td>引脚号129</td>
</tr>
<tr>
  <td>行号1</td>
  <td>引脚号106</td>
  <td>列号1</td>
  <td>引脚号128</td>
</tr>
<tr>
  <td>行号2</td>
  <td>引脚号107</td>
  <td>列号2</td>
  <td>引脚号127</td>
</tr>
<tr>
  <td>行号3</td>
  <td>引脚号108</td>
  <td>列号3</td>
  <td>引脚号126</td>
</tr>
<tr>
  <td>行号4</td>
  <td>引脚号104</td>
  <td>列号4</td>
  <td>引脚号125</td>
</tr>
<tr>
  <td>行号5</td>
  <td>引脚号103</td>
  <td>列号5</td>
  <td>/</td>
</tr>
</tbody>
</table>

<table class="EC800G">
<thead>
<tr>
  <th>行号</th>
  <th>行号引脚</th>
  <th>列号</th>
  <th>列号引脚</th>
</tr>
</thead>
<tbody>
<tr>
  <td>行号0</td>
  <td>引脚号83</td>
  <td>列号0</td>
  <td>引脚号82</td>
</tr>
<tr>
  <td>行号1</td>
  <td>引脚号86</td>
  <td>列号1</td>
  <td>引脚号87</td>
</tr>
<tr>
  <td>行号2</td>
  <td>引脚号76</td>
  <td>列号2</td>
  <td>引脚号77</td>
</tr>
<tr>
  <td>行号3</td>
  <td>引脚号74</td>
  <td>列号3</td>
  <td>引脚号75</td>
</tr>
<tr>
  <td>行号4</td>
  <td>引脚号85</td>
  <td>列号4</td>
  <td>引脚号84</td>
</tr>
</tbody>
</table>

<table class="EG800P">
<thead>
<tr>
  <th>行号</th>
  <th>行号引脚</th>
  <th>列号</th>
  <th>列号引脚</th>
</tr>
</thead>
<tbody>
<tr>
  <td>行号0</td>
  <td>引脚号86</td>
  <td>列号0</td>
  <td>引脚号87</td>
</tr>
<tr>
  <td>行号1</td>
  <td>引脚号85</td>
  <td>列号1</td>
  <td>引脚号84</td>
</tr>
<tr>
  <td>行号2</td>
  <td>引脚号82</td>
  <td>列号2</td>
  <td>引脚号83</td>
</tr>
</tbody>
</table>

</div>

**示例：**

```python
>>> # 创建keypad对象
>>> import machine
>>> keypad=machine.KeyPad(2,3)  # 设置为2行3列矩阵键盘
>>> keypad=machine.KeyPad()     # 参数缺省,默认设置为4行4列矩阵键盘
>>> keypad=machine.KeyPad(2)    # 行值设置为2,列值缺省,列值默认为4,初始化为2行4列矩阵键盘
```

## 方法

### `keypad.init`

```python
keypad.init()
```

该方法用于初始化keypad设置。

**返回值描述：**

设置成功返回整型值`0`，设置失败返回整型值`-1` 。

### `keypad.set_callback`

```python
keypad.set_callback(usrFun)
```

该方法用于设置回调函数，外接按键接入模组后，当外接键盘按键按放时会触发此回调函数。

**参数描述：**

- `usrFun` - 矩阵键盘回调函数，回调函数原型：

  ```python
  usrFun(result_list)
  ```

  回调函数参数描述：

  - `result_list[0]`：按键状态(1表示按下，0表示抬起)

  - `result_list[1]`：行号

  - `result_list[2]`：列号

> 以EC600M为例，假设按键连接的管脚为49,52脚，按下后产生的回调的参数为[1,0,2]，松开后产生的回调的参数为[0,0,2]。

**返回值描述：**

设置成功返回整型值`0`，设置失败返回整型值`-1` 。

### `keypad.deinit`

```python
keypad.deinit()
```

该方法用于解除初始化，释放初始化的资源和回调函数设置。

**返回值描述：**

设置成功返回整型值`0`，设置失败返回整型值`-1` 。

**使用示例：**

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
