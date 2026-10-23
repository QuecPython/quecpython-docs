# I2C – Two-wire Serial Protocol

This class is designed for the two-wire serial protocol for communication between devices.

## Constructor

### `machine.I2C`

<div class="content-QuecPython">
<div class="EC600U|EC200U|BC25PA|EG915U|EC600G|BC32|BC92|EC800Z|EC200A|EC200A-V1|UC200A|EC800N|BG95|EC600M|EG912N|EC800G|EG912U|EC600K|EC800K|EG800K|EG915N|EG800AK">
<pre class="language-python"><code class="language-python">class machine.I2C(I2Cn, MODE)
</code></pre>
<p><strong>Parameter:</strong></p>
<ul>
<li><p><code>I2Cn</code> - Integer type. I2C channel index number.<br/><code>I2C0</code> : <code>0</code> - Channel 0 <br/><code>I2C1</code> : <code>1</code> - Channel 1 <br/><code>I2C2</code> : <code>2</code> - Channel 2<br/></p>
</li>
<li><p><code>MODE</code> - Integer type. I2C working mode.<br/><code>STANDARD_MODE</code> : <code>0</code> - Standard mode<br/><code>FAST_MODE</code> ：<code>1</code> - Fast mode</p>
</li>
</li>
</ul>
</div>


<div class="EC800M|EG810M">
<pre class="language-python"><code class="language-python">class machine.I2C(I2Cn, MODE)
</code></pre>
<p><strong>Parameter:</strong></p>
<ul>
<li><p><code>I2Cn</code> - Integer type. I2C channel index number.<br/><code>I2C0</code> : <code>0</code> - Channel 0 <br/><code>I2C1</code> : <code>1</code> - Channel 1 <br/><code>I2C2</code> : <code>2</code> - Channel 2<br/><code>I2C3</code> : <code>3</code> - Channel 3<br/></p>
</li>
<li><p><code>MODE</code> - Integer type. I2C working mode.<br/><code>STANDARD_MODE</code> : <code>0</code> - Standard mode<br/><code>FAST_MODE</code> ：<code>1</code> - Fast mode</p>
</li>
</li>
</ul>
</div>

<div class="EC600E">
<pre class="language-python"><code class="language-python">class machine.I2C(I2Cn, MODE)
</code></pre>
<p><strong>Parameter:</strong></p>
<ul>
<li><p><code>I2Cn</code> - Integer type. I2C channel index number.<br/><code>I2C0</code> : <code>0</code> - Channel 0 <br/><code>I2C1</code> : <code>1</code> - Channel 1 <br/><code>I2C2</code> : <code>2</code> - Channel 2<br/></p>
</li>
<li><p><code>MODE</code> - Integer type. I2C working mode.<br/><code>STANDARD_MODE</code> : <code>0</code> - Standard mode<br/><code>FAST_MODE</code> ：<code>1</code> - Fast mode</p>
</li>
<li><code>Note: </code>
<br/><code>1.Inside the module, Pin57 is the same as Pin11, and Pin56 is the same as Pin12.</code>
</li>
</ul>
</div>

<div class="EC800E">
<pre class="language-python"><code class="language-python">class machine.I2C(I2Cn, MODE)
</code></pre>
<p><strong>Parameter:</strong></p>
<ul>
<li><p><code>I2Cn</code> - Integer type. I2C channel index number.<br/><code>I2C0</code> : <code>0</code> - Channel 0 <br/><code>I2C1</code> : <code>1</code> - Channel 1 <br/><code>I2C2</code> : <code>2</code> - Channel 2<br/></p>
</li>
<li><p><code>MODE</code> - Integer type. I2C working mode.<br/><code>STANDARD_MODE</code> : <code>0</code> - Standard mode<br/><code>FAST_MODE</code> ：<code>1</code> - Fast mode</p>
</li>
<li><code>Note: </code>
<br/><code>1.Inside the module, Pin67 is the same as Pin57, and Pin66 is the same as Pin58.</code>
<br/><code>2.The 57th and 58th pins of EC800ECN_LE&LQ&CG are not available.</code>
</li>
</ul>
</div>

<div class="FCM360W">
<pre class="language-python"><code class="language-python">class machine.I2C(I2Cn, MODE)
</code></pre>
<p><strong>Parameter:</strong></p>
<ul>
<li><p><code>I2Cn</code> - Integer type. I2C channel index number.<br/><code>I2C0</code> : <code>0</code> - Channel 0 <br/><code>I2C1</code> : <code>1</code> - Channel 1 <br/><code>I2C2</code> : <code>2</code> - Channel 2<br/></p>
</li>
<li><p><code>MODE</code> - Integer type. I2C working mode.<br/><code>STANDARD_MODE</code> : <code>0</code> - Standard mode<br/><code>FAST_MODE</code> ：<code>1</code> - Fast mode<br/><code>ENHANCED_FAST_MODE</code> ：<code>2</code> - Enhanced Fast mode</p>
</li>
</ul>
</div>

<div class="FCM362K">
<pre class="language-python"><code class="language-python">class machine.I2C(I2Cn, MODE, [group])
</code></pre>
<p><strong>Parameter:</strong></p>
<ul>
<li><p><code>I2Cn</code> - Integer type. I2C channel index number.<br/><code>I2C0</code> : <code>0</code> - Channel 0 <br/><code>I2C1</code> : <code>1</code> - Channel 1 <br/><code>I2C2</code> : <code>2</code> - Channel 2<br/></p>
</li>
<li><p><code>MODE</code> - Integer type. I2C working mode.<br/><code>STANDARD_MODE</code> : <code>0</code> - Standard mode<br/><code>FAST_MODE</code> ：<code>1</code> - Fast mode</p>
</li>
<li><p><code>[group]</code> - Select to use I2C in different pins. Currently, only FCM362K supports Iics. The default value is 0<br/></p>
</li>
</ul>
</div>
<div class="EG800Z">
<pre class="language-python"><code class="language-python">class machine.I2C(I2Cn, MODE, [group])
</code></pre>
<p><strong>Parameter:</strong></p>
<ul>
<li><p><code>I2Cn</code> - Integer type. I2C channel index number.<br/><code>I2C0</code> : <code>0</code> - Channel 0 <br/><code>I2C1</code> : <code>1</code> - Channel 1 <br/></p>
</li>
<li><p><code>MODE</code> - Integer type. I2C working mode.<br/><code>STANDARD_MODE</code> : <code>0</code> - Standard mode<br/><code>FAST_MODE</code> ：<code>1</code> - Fast mode</p>
</li>
<li><p><code>[group]</code> - Select to use I2C in different pins. Currently, only FCM362K supports Iics. The default value is 0<br/></p>
</li>
</ul>
</div>


<div class="BG950S">
<pre class="language-python"><code class="language-python">class machine.I2C(I2Cn, MODE)
</code></pre>
<p><strong>Parameter:</strong></p>
<ul>
<li><p><code>I2Cn</code> - Integer type. I2C channel index number.<br/><code>I2C0</code> : <code>0</code> - Channel 0 <br/><code>I2C1</code> : <code>1</code> - Channel 1 <br/><code>I2C2</code> : <code>2</code> - Channel 2<br/></p>
</li>
<li><p><code>MODE</code> - Integer type. I2C working mode.<br/><code>STANDARD_MODE</code> : <code>0</code> - Standard mode<br/><code>FAST_MODE</code> ：<code>1</code> - Fast mode</p>
</li>
</ul>
</div>

</div>

<div class="content-QuecPython">

<div class="EC600U|EC200U|BC25PA|EG915U|EC600G|BC32|BC92|EC800Z|EC200A|EC200A-V1|UC200A|EC800N|BG95|EC600M|EC800M|EG810M|EG912N|EC800G|EG912U|EC600K|EC800K|EG800K|EG915N|EC600E|EC800E|FCM360W|BG950S|EG800AK">
<p><strong>Example:</strong></p>
<pre class="language-python"><code class="language-python">
>>> from machine import I2C
>>> # Creates an I2C object
>>> i2c_obj = I2C(I2C.I2C0, I2C.STANDARD_MODE)  # Returns an I2C object
</code></pre>
</div>
<div class="FCM362K|EG800Z">
<p><strong>Example:</strong></p>
<pre class="language-python"><code class="language-python">
>>> from machine import I2C
>>> # Creates an I2C object
>>> i2c_obj = I2C(I2C.I2C0, I2C.STANDARD_MODE,0)  # Returns an I2C object
</code></pre>
</div>

</div>

**I2C Pin Correspondences:**

<div class="table-QuecPython">
<table class="EC600U">
<thead>
<tr>
  <th>I2C</th>
  <th>SCL</th>
  <th>SDA</th>
</tr>
</thead>
<tbody>
<tr>
  <td>I2C0</td>
  <td>pin11</td>
  <td>pin12</td>
</tr>
<tr>
  <td>I2C1</td>
  <td>pin57</td>
  <td>pin56</td>
</tr>
</tbody>
</table>

<table class="EC200U">
<thead>
<tr>
  <th>I2C</th>
  <th>SCL</th>
  <th>SDA</th>
</tr>
</thead>
<tbody>
<tr>
  <td>I2C0</td>
  <td>pin41</td>
  <td>pin42</td>
</tr>
<tr>
  <td>I2C1</td>
  <td>pin141</td>
  <td>引142</td>
</tr>
</tbody>
</table>

<table class="EC200A" >
<thead>
<tr>
  <th>I2C</th>
  <th>SCL</th>
  <th>SDA</th>
</tr>
</thead>
<tbody>
<tr>
  <td>I2C0</td>
  <td>pin41</td>
  <td>pin42</td>
</tr>
</tbody>
</table>

<table class="EC200A-V1" >
<thead>
<tr>
  <th>I2C</th>
  <th>SCL</th>
  <th>SDA</th>
</tr>
</thead>
<tbody>
<tr>
  <td>I2C0</td>
  <td>pin41</td>
  <td>pin42</td>
</tr>
<tr>
  <td>I2C1</td>
  <td>pin143</td>
  <td>pin144</td>
</tr>
</tbody>
</table>

<table class="UC200A" >
<thead>
<tr>
  <th>I2C</th>
  <th>SCL</th>
  <th>SDA</th>
</tr>
</thead>
<tbody>
<tr>
  <td>I2C0</td>
  <td>pin41</td>
  <td>pin42</td>
</tr>
</tbody>
</table>

<table class="BC25PA" >
<thead>
<tr>
  <th>I2C</th>
  <th>SCL</th>
  <th>SDA</th>
</tr>
</thead>
<tbody>
<tr>
  <td>I2C0</td>
  <td>pin23</td>
  <td>pin22</td>
</tr>
<tr>
  <td>I2C1</td>
  <td>pin20</td>
  <td>pin21</td>
</tr>
</tbody>
</table>

<table class="EC800N" >
<thead>
<tr>
  <th>I2C</th>
  <th>SCL</th>
  <th>SDA</th>
</tr>
</thead>
<tbody>
<tr>
  <td>I2C0</td>
  <td>pin67</td>
  <td>pin66</td>
</tr>
</tbody>
</table>

<table class="BG95" >
<thead>
<tr>
  <th>I2C</th>
  <th>SCL</th>
  <th>SDA</th>
</tr>
</thead>
<tbody>
<tr>
  <td>I2C0</td>
  <td>pin18</td>
  <td>pin19</td>
</tr>
<tr>
  <td>I2C1</td>
  <td>pin40</td>
  <td>pin41</td>
</tr>
<tr>
  <td>I2C2</td>
  <td>pin26</td>
  <td>pin25</td>
</tr>
</tbody>
</table>

<table class="EC600M" >
<thead>
<tr>
  <th>I2C</th>
  <th>SCL</th>
  <th>SDA</th>
</tr>
</thead>
<tbody>
<tr>
  <td>I2C0</td>
  <td>pin9</td>
  <td>pin64</td>
</tr>
<tr>
  <td>I2C1</td>
  <td>pin57</td>
  <td>pin56</td>
</tr>
<tr>
  <td>I2C2</td>
  <td>pin67</td>
  <td>pin65</td>
</tr>
</tbody>
</table>

<table class="EG915U" >
<thead>
<tr>
  <th>I2C</th>
  <th>SCL</th>
  <th>SDA</th>
</tr>
</thead>
<tbody>
<tr>
  <td>I2C0</td>
  <td>pin103</td>
  <td>pin114</td>
</tr>
<tr>
  <td>I2C1</td>
  <td>pin40</td>
  <td>pin41</td>
</tr>
</tbody>
</table>


<table class="EC800M" >
<thead>
<tr>
  <th>I2C</th>
  <th>SCL</th>
  <th>SDA</th>
</tr>
</thead>
<tbody>
<tr>
  <td>I2C0</td>
  <td>pin67</td>
  <td>pin66</td>
</tr>
<tr>
  <td>I2C1</td>
  <td>pin75</td>
  <td>pin74</td>
</tr>
<tr>
  <td>I2C2</td>
  <td>pin68</td>
  <td>pin69</td>
</tr>
<tr>
  <td>I2C3</td>
  <td>pin57</td>
  <td>pin58</td>
</tr>
</tbody>
</table>

<table class="EG810M" >
<thead>
<tr>
  <th>I2C</th>
  <th>SCL</th>
  <th>SDA</th>
</tr>
</thead>
<tbody>
<tr>
  <td>I2C0</td>
  <td>pin67</td>
  <td>pin66</td>
</tr>
<tr>
  <td>I2C1</td>
  <td>pin75</td>
  <td>pin74</td>
</tr>
<tr>
  <td>I2C2</td>
  <td>pin68</td>
  <td>pin69</td>
</tr>
<tr>
  <td>I2C3</td>
  <td>pin57</td>
  <td>pin58</td>
</tr>
</tbody>
</table>


<table class="EG912N" >
<thead>
<tr>
  <th>I2C</th>
  <th>SCL</th>
  <th>SDA</th>
</tr>
</thead>
<tbody>
<tr>
  <td>I2C1</td>
  <td>pin40</td>
  <td>pin41</td>
</tr>
</tbody>
</table>

<table class="EC600E" >
<thead>
<tr>
  <th>I2C</th>
  <th>SCL</th>
  <th>SDA</th>
</tr>
</thead>
<tbody>
<tr>
  <td>I2C1</td>
  <td>pin57(11)</td>
  <td>pin56(12)</td>
</tr>
</tbody>
</table>

<table class="EC800E" >
<thead>
<tr>
  <th>I2C</th>
  <th>SCL</th>
  <th>SDA</th>
</tr>
</thead>
<tbody>
<tr>
  <td>I2C0</td>
  <td>pin67(57)</td>
  <td>pin66(58)</td>
</tr>
</tbody>
</table>

<table class="EC600G" >
<thead>
<tr>
  <th>I2C</th>
  <th>SCL</th>
  <th>SDA</th>
</tr>
</thead>
<tbody>
<tr>
  <td>I2C0</td>
  <td>pin57</td>
  <td>pin56</td>
</tr>
<tr>
  <td>I2C1</td>
  <td>pin11</td>
  <td>pin12</td>
</tr>
</tbody>
</table>

<table class="EC800G" >
<thead>
<tr>
  <th>I2C</th>
  <th>SCL</th>
  <th>SDA</th>
</tr>
</thead>
<tbody>
<tr>
  <td>I2C0</td>
  <td>pin67</td>
  <td>pin66</td>
</tr>
<tr>
  <td>I2C1</td>
  <td>pin57</td>
  <td>pin58</td>
</tr>
<tr>
  <td>I2C2</td>
  <td>pin68</td>
  <td>pin69</td>
</tr>
</tbody>
</table>

<table class="EG912U" >
<thead>
<tr>
  <th>I2C</th>
  <th>SCL</th>
  <th>SDA</th>
</tr>
</thead>
<tbody>
<tr>
  <td>I2C1</td>
  <td>pin40</td>
  <td>pin41</td>
</tr>
</tbody>
</table>

<table class="EC600K" >
<thead>
<tr>
  <th>I2C</th>
  <th>SCL</th>
  <th>SDA</th>
</tr>
</thead>
<tbody>
<tr>
  <td>I2C1</td>
  <td>pin57</td>
  <td>pin56</td>
</tr>
<tr>
  <td>I2C3</td>
  <td>pin11</td>
  <td>pin12</td>
</tr>
</tbody>
</table>

<table class="EC800K" >
<thead>
<tr>
  <th>I2C</th>
  <th>SCL</th>
  <th>SDA</th>
</tr>
</thead>
<tbody>
<tr>
  <td>I2C0</td>
  <td>pin67</td>
  <td>pin66</td>
</tr>
<tr>
  <td>I2C2</td>
  <td>pin68</td>
  <td>pin69</td>
</tr>
</tbody>
</table>

<table class="EG800K" >
<thead>
<tr>
  <th>I2C</th>
  <th>SCL</th>
  <th>SDA</th>
</tr>
</thead>
<tbody>
<tr>
  <td>I2C0</td>
  <td>pin67</td>
  <td>pin66</td>
</tr>
<tr>
  <td>I2C2</td>
  <td>pin68</td>
  <td>pin69</td>
</tr>
</tbody>
</table>

<table class="EG800AK" >
<thead>
<tr>
  <th>I2C</th>
  <th>SCL</th>
  <th>SDA</th>
</tr>
</thead>
<tbody>
<tr>
  <td>I2C0</td>
  <td>pin67</td>
  <td>pin66</td>
</tr>
<tr>
  <td>I2C2</td>
  <td>pin68</td>
  <td>pin69</td>
</tr>
</tbody>
</table>

<table class="FCM360W" >
<thead>
<tr>
  <th>I2C</th>
  <th>SCL</th>
  <th>SDA</th>
</tr>
</thead>
<tbody>
<tr>
  <td>I2C0</td>
  <td>pin29</td>
  <td>pin23</td>
</tr>
</tbody>
</table>

<table class="FCM362K" >
<thead>
<tr>
  <th>I2C</th>
  <th>SCL</th>
  <th>SDA</th>
</tr>
</thead>
<tbody>
<tr>
  <td>I2C0</td>
  <td>pin30</td>
  <td>pin29</td>
</tr>
</tbody>
</table>
<table class="EG800Z" >
<thead>
<tr>
  <th>I2C</th>
  <th>SCL</th>
  <th>SDA</th>
</tr>
</thead>
<tbody>
<tr>
  <td>I2C0</td>
  <td>pin81(group=0)</td>
  <td>pin80(group=0)</td>
</tr>
<tr>
  <td>I2C0</td>
  <td>pin67(group=1)</td>
  <td>pin66(group=1)</td>
</tr>
<tr>
  <td>I2C1</td>
  <td>pin57</td>
  <td>pin58</td>
</tr>
</tbody>
</table>


<table class="BC32" >
<thead>
<tr>
  <th>I2C</th>
  <th>SCL</th>
  <th>SDA</th>
</tr>
</thead>
<tbody>
<tr>
  <td>I2C0</td>
  <td>pin12</td>
  <td>pin26</td>
</tr>
<tr>
  <td>I2C1</td>
  <td>pin43</td>
  <td>pin44</td>
</tr>
</tbody>
</table>

<table class="BC92" >
<thead>
<tr>
  <th>I2C</th>
  <th>SCL</th>
  <th>SDA</th>
</tr>
</thead>
<tbody>
<tr>
  <td>I2C0</td>
  <td>pin12</td>
  <td>pin26</td>
</tr>
<tr>
  <td>I2C2</td>
  <td>pin43</td>
  <td>pin44</td>
</tr>
</tbody>
</table>

<table class="EG915N" >
<thead>
<tr>
  <th>I2C</th>
  <th>SCL</th>
  <th>SDA</th>
</tr>
</thead>
<tbody>
<tr>
  <td>I2C1</td>
  <td>pin40</td>
  <td>pin41</td>
</tr>
</tbody>
</table>

<table class="EC800Z" >
<thead>
<tr>
  <th>I2C</th>
  <th>SCL</th>
  <th>SDA</th>
</tr>
</thead>
<tbody>
<tr>
  <td>I2C0</td>
  <td>pin67</td>
  <td>pin66</td>
</tr>
<tr>
  <td>I2C1</td>
  <td>pin57</td>
  <td>pin58</td>
</tr>
</tbody>
</table>

<table class="BG950S" >
<thead>
<tr>
  <th>I2C</th>
  <th>SCL</th>
  <th>SDA</th>
</tr>
</thead>
<tbody>
<tr>
  <td>I2C0</td>
  <td>pin40</td>
  <td>pin41</td>
</tr>
</tbody>
</table>

</div>

## Methods

### `I2C.read`

```python
I2C.read(slaveaddress, addr,addr_len, r_data, datalen, delay)
```

This method reads data to I2C bus.

**Parameter:**

- `slaveaddress` - Integer type. I2C device address.
- `addr` - Bytearray type. I2C register address.
- `addr_len` - Integer type. Register address length.
- `r_data` - Bytearray type. Byte array for receiving data.
- `datalen` - Integer type. Length of byte array.
- `delay` - Integer type. Delay. Data conversion buffer time (unit: ms).

**Return Value:**

`0` - Successful execution

`-1` - Failed execution

### `I2C.write`

```python
I2C.write(slaveaddress, addr, addr_len, data, datalen)
```

This method writes data to I2C bus. 

**Parameter:**

- `slaveaddress` - Integer type. I2C device address.
- `addr` - Bytearray type. I2C register address.
- `addr_len` - Integer type. Register address length.
- `data` - Bytearray type. Data to be written.
- `datalen` - Integer type. Length of data to be written.

**Return Value:**

`0` - Successful execution

`-1` - Failed execution

**Example:**

> Please connect the device.

```python
import log
from machine import I2C
import utime


'''
The following two global variables are necessary. You can modify the values of these two global variables based on project requirement.
'''
PROJECT_NAME = "QuecPython_I2C_example"
PROJECT_VERSION = "1.0.0"

'''
I2C usage example
'''

# Sets log output level
log.basicConfig(level=log.INFO)
i2c_log = log.getLogger("I2C")


if __name__ == '__main__':
    I2C_SLAVE_ADDR = 0x1B  # I2C device address
    WHO_AM_I = bytearray([0x02, 0])   # I2C register address. It is passed in as a buff. Take the first value and calculate the length of a value  

    data = bytearray([0x12, 0])   # Inputs the corresponding command
    i2c_obj = I2C(I2C.I2C0, I2C.STANDARD_MODE)  # Returns an I2C object
    i2c_obj.write(I2C_SLAVE_ADDR, WHO_AM_I, 1, data, 2) # Writes data

    r_data = bytearray(2)  # Creates a byte array with the length of 2 bytes for receiving
    i2c_obj.read(I2C_SLAVE_ADDR, WHO_AM_I, 1, r_data, 2, 0)   # read
    i2c_log.info(r_data[0])
    i2c_log.info(r_data[1])

```

## Constants

<div class="table-cons-QuecPython">
<table class="EC600U" style="width: 100%;">
<thead>
<tr>
  <th>Constant</th>
  <th>Description</th>
</tr>
</thead>
<tbody>
<tr>
  <td>I2C.I2C0</td>
  <td>I2C passage index number: 0</td>
</tr>
<tr>
  <td>I2C.I2C1</td>
  <td>I2C passage index number: 1</td>
</tr>
<tr>
  <td>I2C.STANDARD_MODE</td>
  <td>Standard mode</td>
</tr>
<tr>
  <td>I2C.FAST_MODE</td>
  <td>Fast mode</td>
</tr>
</tbody>
</table>

<table class="EC600K" style="width: 100%;">
<thead>
<tr>
  <th>Constant</th>
  <th>Description</th>
</tr>
</thead>
<tbody>
<tr>
  <td>I2C.I2C1</td>
  <td>I2C passage index number: 1</td>
</tr>
<tr>
  <td>I2C.I2C3</td>
  <td>I2C passage index number: 3</td>
</tr>
<tr>
  <td>I2C.STANDARD_MODE</td>
  <td>Standard mode</td>
</tr>
<tr>
  <td>I2C.FAST_MODE</td>
  <td>Fast mode</td>
</tr>
</tbody>
</table>

<table class="EC600E" style="width: 100%;">
<thead>
<tr>
  <th>Constant</th>
  <th>Description</th>
</tr>
</thead>
<tbody>
<tr>
  <td>I2C.I2C1</td>
  <td>I2C passage index number: 1</td>
</tr>
<tr>
  <td>I2C.STANDARD_MODE</td>
  <td>Standard mode</td>
</tr>
<tr>
  <td>I2C.FAST_MODE</td>
  <td>Fast mode</td>
</tr>
</tbody>
</table>

<table class="EG915N" style="width: 100%;">
<thead>
<tr>
  <th>Constant</th>
  <th>Description</th>
</tr>
</thead>
<tbody>
<tr>
  <td>I2C.I2C1</td>
  <td>I2C passage index number: 1</td>
</tr>
<tr>
  <td>I2C.STANDARD_MODE</td>
  <td>Standard mode</td>
</tr>
<tr>
  <td>I2C.FAST_MODE</td>
  <td>Fast mode</td>
</tr>
</tbody>
</table>

<table class="EC200U" style="width: 100%;">
<thead>
<tr>
  <th>Constant</th>
  <th>Description</th>
</tr>
</thead>
<tbody>
<tr>
  <td>I2C.I2C0</td>
  <td>I2C passage index number: 0</td>
</tr>
<tr>
  <td>I2C.I2C1</td>
  <td>I2C passage index number: 1</td>
</tr>
<tr>
  <td>I2C.STANDARD_MODE</td>
  <td>Standard mode</td>
</tr>
<tr>
  <td>I2C.FAST_MODE</td>
  <td>Fast mode</td>
</tr>
</tbody>
</table>

<table class="BC25PA" style="width: 100%;">
<thead>
<tr>
  <th>Constant</th>
  <th>Description</th>
</tr>
</thead>
<tbody>
<tr>
  <td>I2C.I2C0</td>
  <td>I2C passage index number: 0</td>
</tr>
<tr>
  <td>I2C.I2C1</td>
  <td>I2C passage index number: 1</td>
</tr>
<tr>
  <td>I2C.STANDARD_MODE</td>
  <td>Standard mode</td>
</tr>
<tr>
  <td>I2C.FAST_MODE</td>
  <td>Fast mode</td>
</tr>
</tbody>
</table>

<table class="BC25" style="width: 100%;">
<thead>
<tr>
  <th>Constant</th>
  <th>Description</th>
</tr>
</thead>
<tbody>
<tr>
  <td>I2C.I2C0</td>
  <td>I2C passage index number: 0</td>
</tr>
<tr>
  <td>I2C.I2C1</td>
  <td>I2C passage index number: 1</td>
</tr>
<tr>
  <td>I2C.STANDARD_MODE</td>
  <td>Standard mode</td>
</tr>
<tr>
  <td>I2C.FAST_MODE</td>
  <td>Fast mode</td>
</tr>
</tbody>
</table>

<table class="EG915U" style="width: 100%;">
<thead>
<tr>
  <th>Constant</th>
  <th>Description</th>
</tr>
</thead>
<tbody>
<tr>
  <td>I2C.I2C0</td>
  <td>I2C passage index number: 0</td>
</tr>
<tr>
  <td>I2C.I2C1</td>
  <td>I2C passage index number: 1</td>
</tr>
<tr>
  <td>I2C.STANDARD_MODE</td>
  <td>Standard mode</td>
</tr>
<tr>
  <td>I2C.FAST_MODE</td>
  <td>Fast mode</td>
</tr>
</tbody>
</table>

<table class="EC800M" style="width: 100%;">
<thead>
<tr>
  <th>Constant</th>
  <th>Description</th>
</tr>
</thead>
<tbody>
<tr>
  <td>I2C.I2C0</td>
  <td>I2C passage index number: 0</td>
</tr>
<tr>
  <td>I2C.I2C1</td>
  <td>I2C passage index number: 1</td>
</tr>
<tr>
  <td>I2C.I2C2</td>
  <td>I2C passage index number: 2</td>
</tr>
<tr>
  <td>I2C.I2C3</td>
  <td>I2C passage index number: 3</td>
</tr>
<tr>
  <td>I2C.STANDARD_MODE</td>
  <td>Standard mode</td>
</tr>
<tr>
  <td>I2C.FAST_MODE</td>
  <td>Fast mode</td>
</tr>
</tbody>
</table>

<table class="EG810M" style="width: 100%;">
<thead>
<tr>
  <th>Constant</th>
  <th>Description</th>
</tr>
</thead>
<tbody>
<tr>
  <td>I2C.I2C0</td>
  <td>I2C passage index number: 0</td>
</tr>
<tr>
  <td>I2C.I2C1</td>
  <td>I2C passage index number: 1</td>
</tr>
<tr>
  <td>I2C.I2C2</td>
  <td>I2C passage index number: 2</td>
</tr>
<tr>
  <td>I2C.I2C3</td>
  <td>I2C passage index number: 3</td>
</tr>
<tr>
  <td>I2C.STANDARD_MODE</td>
  <td>Standard mode</td>
</tr>
<tr>
  <td>I2C.FAST_MODE</td>
  <td>Fast mode</td>
</tr>
</tbody>
</table>

<table class="BC32" style="width: 100%;">
<thead>
<tr>
  <th>Constant</th>
  <th>Description</th>
</tr>
</thead>
<tbody>
<tr>
  <td>I2C.I2C0</td>
  <td>I2C passage index number: 0</td>
</tr>
<tr>
  <td>I2C.I2C1</td>
  <td>I2C passage index number: 1</td>
</tr>
<tr>
  <td>I2C.STANDARD_MODE</td>
  <td>Standard mode</td>
</tr>
<tr>
  <td>I2C.FAST_MODE</td>
  <td>Fast mode</td>
</tr>
</tbody>
</table>

<table class="BC92" style="width: 100%;">
<thead>
<tr>
  <th>Constant</th>
  <th>Description</th>
</tr>
</thead>
<tbody>
<tr>
  <td>I2C.I2C0</td>
  <td>I2C passage index number: 0</td>
</tr>
<tr>
  <td>I2C.I2C1</td>
  <td>I2C passage index number: 1</td>
</tr>
<tr>
  <td>I2C.STANDARD_MODE</td>
  <td>Standard mode</td>
</tr>
<tr>
  <td>I2C.FAST_MODE</td>
  <td>Fast mode</td>
</tr>
</tbody>
</table>
<table class="EC800Z|EG800Z" style="width: 100%;">
<thead>
<tr>
  <th>Constant</th>
  <th>Description</th>
</tr>
</thead>
<tbody>
<tr>
  <td>I2C.I2C0</td>
  <td>I2C passage index number: 0</td>
</tr>
<tr>
  <td>I2C.I2C1</td>
  <td>I2C passage index number: 1</td>
</tr>
<tr>
  <td>I2C.STANDARD_MODE</td>
  <td>Standard mode</td>
</tr>
<tr>
  <td>I2C.FAST_MODE</td>
  <td>Fast mode</td>
</tr>
</tbody>
</table>



<table class="BG95M3" style="width: 100%;">
<thead>
<tr>
  <th>Constant</th>
  <th>Description</th>
</tr>
</thead>
<tbody>
<tr>
  <td>I2C.I2C0</td>
  <td>I2C passage index number: 0</td>
</tr>
<tr>
  <td>I2C.I2C1</td>
  <td>I2C passage index number: 1</td>
</tr>
<tr>
  <td>I2C.I2C2</td>
  <td>I2C passage index number: 2</td>
</tr>
<tr>
  <td>I2C.STANDARD_MODE</td>
  <td>Standard mode</td>
</tr>
<tr>
  <td>I2C.FAST_MODE</td>
  <td>Fast mode</td>
</tr>
</tbody>
</table>


<table class="EC600M" style="width: 100%;">
<thead>
<tr>
  <th>Constant</th>
  <th>Description</th>
</tr>
</thead>
<tbody>
<tr>
  <td>I2C.I2C0</td>
  <td>I2C passage index number: 0</td>
</tr>
<tr>
  <td>I2C.I2C1</td>
  <td>I2C passage index number: 1</td>
</tr>
<tr>
  <td>I2C.I2C2</td>
  <td>I2C passage index number: 2</td>
</tr>
<tr>
  <td>I2C.STANDARD_MODE</td>
  <td>Standard mode</td>
</tr>
<tr>
  <td>I2C.FAST_MODE</td>
  <td>Fast mode</td>
</tr>
</tbody>
</table>


<table class="EC800K" style="width: 100%;">
<thead>
<tr>
  <th>Constant</th>
  <th>Description</th>
</tr>
</thead>
<tbody>
<tr>
  <td>I2C.I2C0</td>
  <td>I2C passage index number: 0</td>
</tr>
<tr>
  <td>I2C.I2C1</td>
  <td>I2C passage index number: 1</td>
</tr>
<tr>
  <td>I2C.I2C2</td>
  <td>I2C passage index number: 2</td>
</tr>
<tr>
  <td>I2C.STANDARD_MODE</td>
  <td>Standard mode</td>
</tr>
<tr>
  <td>I2C.FAST_MODE</td>
  <td>Fast mode</td>
</tr>
</tbody>
</table>

<table class="EG800K" style="width: 100%;">
<thead>
<tr>
  <th>Constant</th>
  <th>Description</th>
</tr>
</thead>
<tbody>
<tr>
  <td>I2C.I2C0</td>
  <td>I2C passage index number: 0</td>
</tr>
<tr>
  <td>I2C.I2C1</td>
  <td>I2C passage index number: 1</td>
</tr>
<tr>
  <td>I2C.I2C2</td>
  <td>I2C passage index number: 2</td>
</tr>
<tr>
  <td>I2C.STANDARD_MODE</td>
  <td>Standard mode</td>
</tr>
<tr>
  <td>I2C.FAST_MODE</td>
  <td>Fast mode</td>
</tr>
</tbody>
</table>

<table class="EG800AK" style="width: 100%;">
<thead>
<tr>
  <th>Constant</th>
  <th>Description</th>
</tr>
</thead>
<tbody>
<tr>
  <td>I2C.I2C0</td>
  <td>I2C passage index number: 0</td>
</tr>
<tr>
  <td>I2C.I2C1</td>
  <td>I2C passage index number: 1</td>
</tr>
<tr>
  <td>I2C.I2C2</td>
  <td>I2C passage index number: 2</td>
</tr>
<tr>
  <td>I2C.STANDARD_MODE</td>
  <td>Standard mode</td>
</tr>
<tr>
  <td>I2C.FAST_MODE</td>
  <td>Fast mode</td>
</tr>
</tbody>
</table>

<table class="EC800N" style="width: 100%;">
<thead>
<tr>
  <th>Constant</th>
  <th>Description</th>
</tr>
</thead>
<tbody>
<tr>
  <td>I2C.I2C0</td>
  <td>I2C passage index number: 0</td>
</tr>
<tr>
  <td>I2C.STANDARD_MODE</td>
  <td>Standard mode</td>
</tr>
<tr>
  <td>I2C.FAST_MODE</td>
  <td>Fast mode</td>
</tr>
</tbody>
</table>


<table class="EC800E" style="width: 100%;">
<thead>
<tr>
  <th>Constant</th>
  <th>Description</th>
</tr>
</thead>
<tbody>
<tr>
  <td>I2C.I2C0</td>
  <td>I2C passage index number: 0</td>
</tr>
<tr>
  <td>I2C.STANDARD_MODE</td>
  <td>Standard mode</td>
</tr>
<tr>
  <td>I2C.FAST_MODE</td>
  <td>Fast mode</td>
</tr>
</tbody>
</table>


<table class="FCM362K" style="width: 100%;">
<thead>
<tr>
  <th>Constant</th>
  <th>Description</th>
</tr>
</thead>
<tbody>
<tr>
  <td>I2C.I2C0</td>
  <td>I2C passage index number: 0</td>
</tr>
<tr>
  <td>I2C.STANDARD_MODE</td>
  <td>Standard mode</td>
</tr>
<tr>
  <td>I2C.FAST_MODE</td>
  <td>Fast mode</td>
</tr>
</tbody>
</table>


<table class="EC200A" style="width: 100%;">
<thead>
<tr>
  <th>Constant</th>
  <th>Description</th>
</tr>
</thead>
<tbody>
<tr>
  <td>I2C.I2C0</td>
  <td>I2C passage index number: 0</td>
</tr>
<tr>
  <td>I2C.STANDARD_MODE</td>
  <td>Standard mode</td>
</tr>
<tr>
  <td>I2C.FAST_MODE</td>
  <td>Fast mode</td>
</tr>
</tbody>
</table>

<table class="EC200A-V1" style="width: 100%;">
<thead>
<tr>
  <th>Constant</th>
  <th>Description</th>
</tr>
</thead>
<tbody>
<tr>
  <td>I2C.I2C0</td>
  <td>I2C passage index number: 0</td>
</tr>
<tr>
  <td>I2C.STANDARD_MODE</td>
  <td>Standard mode</td>
</tr>
<tr>
  <td>I2C.FAST_MODE</td>
  <td>Fast mode</td>
</tr>
</tbody>
</table>

<table class="FCM360W" style="width: 100%;">
<thead>
<tr>
  <th>Constant</th>
  <th>Description</th>
</tr>
</thead>
<tbody>
<tr>
  <td>I2C.I2C0</td>
  <td>I2C passage index number: 0</td>
</tr>
<tr>
  <td>I2C.STANDARD_MODE</td>
  <td>Standard mode</td>
</tr>
<tr>
  <td>I2C.FAST_MODE</td>
  <td>Fast mode</td>
</tr>
<tr>
  <td>I2C.ENHANCED_FAST_MODE</td>
  <td>Enhanced fast mode</td>
</tr>
</tbody>
</table>

<table class="BG950S" style="width: 100%;">
<thead>
<tr>
  <th>Constant</th>
  <th>Description</th>
</tr>
</thead>
<tbody>
<tr>
  <td>I2C.I2C0</td>
  <td>I2C passage index number: 0</td>
</tr>
<tr>
  <td>I2C.STANDARD_MODE</td>
  <td>Standard mode</td>
</tr>
<tr>
  <td>I2C.FAST_MODE</td>
  <td>Fast mode</td>
</tr>
</tbody>
</table>

</div>