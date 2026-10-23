# class I2C – I2C通信

该类用于设备之间通信的双线协议。


## 构造函数

### `machine.I2C`

<div class="content-QuecPython">
<div class="EC600U|EC200U|BC25PA|EG915U|EC600G|BC32|BC92|EC800Z|EC200A|EC200A-V1|UC200A|EC800N|BG95|EC600M|EG912N|EC800G|EG912U|EC600K|EC800K|EG800K|EG915N|EG800AK">
<pre class="language-python"><code class="language-python">class machine.I2C(I2Cn, MODE)
</code></pre>
<p><strong>参数描述：</strong></p>
<ul>
<li><p><code>I2Cn</code> - I2C 通路索引号，int类型，说明如下：<br/><code>I2C0</code> : <code>0</code> - 通道0 <br/><code>I2C1</code> : <code>1</code> - 通道1 <br/><code>I2C2</code> : <code>2</code> - 通道2<br/></p>
</li>
<li><p><code>MODE</code> - I2C 的工作模式，int类型，说明如下：<br/><code>STANDARD_MODE</code> : <code>0</code> - 标准模式<br/><code>FAST_MODE</code> ：<code>1</code> - 快速模式</p>
</li>
</ul>
</div>

<div class="EC800M|EG810M">
<pre class="language-python"><code class="language-python">class machine.I2C(I2Cn, MODE)
</code></pre>
<p><strong>参数描述：</strong></p>
<ul>
<li><p><code>I2Cn</code> - I2C 通路索引号，int类型，说明如下：<br/><code>I2C0</code> : <code>0</code> - 通道0 <br/><code>I2C1</code> : <code>1</code> - 通道1 <br/><code>I2C2</code> : <code>2</code> - 通道2<br/><code>I2C3</code> : <code>3</code> - 通道3<br/></p>
</li>
<li><p><code>MODE</code> - I2C 的工作模式，int类型，说明如下：<br/><code>STANDARD_MODE</code> : <code>0</code> - 标准模式<br/><code>FAST_MODE</code> ：<code>1</code> - 快速模式</p>
</li>
</ul>
</div>

<div class="EC600E">
<pre class="language-python"><code class="language-python">class machine.I2C(I2Cn, MODE)
</code></pre>
<p><strong>参数描述：</strong></p>
<ul>
<li><p><code>I2Cn</code> - I2C 通路索引号，int类型，说明如下：<br/><code>I2C0</code> : <code>0</code> - 通道0 <br/><code>I2C1</code> : <code>1</code> - 通道1 <br/><code>I2C2</code> : <code>2</code> - 通道2<br/></p>
</li>
<li><p><code>MODE</code> - I2C 的工作模式，int类型，说明如下：<br/><code>STANDARD_MODE</code> : <code>0</code> - 标准模式<br/><code>FAST_MODE</code> ：<code>1</code> - 快速模式</p>
</li>
<li><code>注：</code>
<br/><code>1.模块的Pin57与PIN11、Pin56 与Pin12 内部是同一个管脚。</code>
</li>
</ul>
</div>

<div class="EC800E">
<pre class="language-python"><code class="language-python">class machine.I2C(I2Cn, MODE)
</code></pre>
<p><strong>参数描述：</strong></p>
<ul>
<li><p><code>I2Cn</code> - I2C 通路索引号，int类型，说明如下：<br/><code>I2C0</code> : <code>0</code> - 通道0 <br/><code>I2C1</code> : <code>1</code> - 通道1 <br/><code>I2C2</code> : <code>2</code> - 通道2<br/></p>
</li>
<li><p><code>MODE</code> - I2C 的工作模式，int类型，说明如下：<br/><code>STANDARD_MODE</code> : <code>0</code> - 标准模式<br/><code>FAST_MODE</code> ：<code>1</code> - 快速模式</p>
</li>
<li><code>注：</code>
<br/><code>1.模块的Pin67与Pin57、Pin66 与Pin58 内部是同一个管脚。</code>
<br/><code>2.EC800ECN_LE&LQ&CG 的57/58引脚不可用。</code>
</li>
</ul>
</div>


<div class="FCM360W">
<pre class="language-python"><code class="language-python">class machine.I2C(I2Cn, MODE)
</code></pre>
<p><strong>参数描述：</strong></p>
<ul>
<li><p><code>I2Cn</code> - I2C 通路索引号，int类型，说明如下：<br/><code>I2C0</code> : <code>0</code> - 通道0 <br/><code>I2C1</code> : <code>1</code> - 通道1 <br/><code>I2C2</code> : <code>2</code> - 通道2<br/></p>
</li>
<li><p><code>MODE</code> - I2C 的工作模式，int类型，说明如下：<br/><code>STANDARD_MODE</code> : <code>0</code> - 标准模式<br/><code>FAST_MODE</code> ：<code>1</code> - 快速模式<br/><code>ENHANCED_FAST_MODE</code> ：<code>2</code> - 快速模式增强</p>
</li>
</ul>
</div>

<div class="FCM362K">
<pre class="language-python"><code class="language-python">class machine.I2C(I2Cn, MODE, [group])
</code></pre>
<p><strong>参数描述：</strong></p>
<ul>
<li><p><code>I2Cn</code> - I2C 通路索引号，int类型，说明如下：<br/><code>I2C0</code> : <code>0</code> - 通道0 <br/><code>I2C1</code> : <code>1</code> - 通道1 <br/><code>I2C2</code> : <code>2</code> - 通道2<br/></p>
</li>
<li><p><code>MODE</code> - I2C 的工作模式，int类型，说明如下：<br/><code>STANDARD_MODE</code> : <code>0</code> - 标准模式<br/><code>FAST_MODE</code> ：<code>1</code> - 快速模式</p>
</li>
<li><p><code>[group]</code> - 选择在不同管脚使用IIC，缺省值为0<br/></p>
</li>
</ul>
</div>

<div class="EG800Z">
<pre class="language-python"><code class="language-python">class machine.I2C(I2Cn, MODE, [group])
</code></pre>
<p><strong>参数描述：</strong></p>
<ul>
<li><p><code>I2Cn</code> - I2C 通路索引号，int类型，说明如下：<br/><code>I2C0</code> : <code>0</code> - 通道0 <br/><code>I2C1</code> : <code>1</code> - 通道1 <br/></p>
</li>
<li><p><code>MODE</code> - I2C 的工作模式，int类型，说明如下：<br/><code>STANDARD_MODE</code> : <code>0</code> - 标准模式<br/><code>FAST_MODE</code> ：<code>1</code> - 快速模式</p>
</li>
<li><p><code>[group]</code> - 选择在不同管脚使用IIC，缺省值为0<br/></p>
</li>
</ul>
</div>

<div class="BG950S">
<pre class="language-python"><code class="language-python">class machine.I2C(I2Cn, MODE)
</code></pre>
<p><strong>参数描述：</strong></p>
<ul>
<li><p><code>I2Cn</code> - I2C 通路索引号，int类型，说明如下：<br/><code>I2C0</code> : <code>0</code> - 通道0 <br/><code>I2C1</code> : <code>1</code> - 通道1 <br/><code>I2C2</code> : <code>2</code> - 通道2<br/><code>I2C3</code> : <code>3</code> - 通道3<br/></p>
</li>
<li><p><code>MODE</code> - I2C 的工作模式，int类型，说明如下：<br/><code>STANDARD_MODE</code> : <code>0</code> - 标准模式<br/><code>FAST_MODE</code> ：<code>1</code> - 快速模式</p>
</li>
</ul>
</div>

</div>

<div class="content-QuecPython">
<div class="EC600U|EC200U|BC25PA|EG915U|EC600G|BC32|BC92|EC800Z|EC200A|EC200A-V1|UC200A|EC800N|BG95|EC600M|EC800M|EG810M|EG912N|EC800G|EG912U|EC600K|EC800K|EG800K|EG915N|EC600E|EC800E|FCM360W|BG950S|EG800AK">
<p><strong>示例：</strong></p>
<pre class="language-python"><code class="language-python">>>> from machine import I2C
>>> # 创建I2C对象
>>> i2c_obj = I2C(I2C.I2C0, I2C.STANDARD_MODE)  # 返回I2C对象
</code></pre>
</div>

<div class="FCM362K|EG800Z">
<p><strong>示例：</strong></p>
<pre class="language-python"><code class="language-python">>>> from machine import I2C
>>> # 创建I2C对象
>>> i2c_obj = I2C(I2C.I2C0, I2C.STANDARD_MODE,0)  # 返回I2C对象
</code></pre>
</div>
</div>

**I2C引脚对应关系：**

<div class="table-QuecPython">
<table class="EC600U">
<thead>
<tr>
  <th>I2C编号</th>
  <th>SCL引脚</th>
  <th>SDA引脚</th>
</tr>
</thead>
<tbody>
<tr>
  <td>I2C0</td>
  <td>引脚11</td>
  <td>引脚12</td>
</tr>
<tr>
  <td>I2C1</td>
  <td>引脚57</td>
  <td>引56</td>
</tr>
</tbody>
</table>

<table class="EC200U">
<thead>
<tr>
  <th>I2C编号</th>
  <th>SCL引脚</th>
  <th>SDA引脚</th>
</tr>
</thead>
<tbody>
<tr>
  <td>I2C0</td>
  <td>引脚41</td>
  <td>引脚42</td>
</tr>
<tr>
  <td>I2C1</td>
  <td>引脚141</td>
  <td>引142</td>
</tr>
</tbody>
</table>

<table class="EC200A" >
<thead>
<tr>
  <th>I2C编号</th>
  <th>SCL引脚</th>
  <th>SDA引脚</th>
</tr>
</thead>
<tbody>
<tr>
  <td>I2C0</td>
  <td>引脚41</td>
  <td>引脚42</td>
</tr>
</tbody>
</table>

<table class="EC200A-V1" >
<thead>
<tr>
  <th>I2C编号</th>
  <th>SCL引脚</th>
  <th>SDA引脚</th>
</tr>
</thead>
<tbody>
<tr>
  <td>I2C0</td>
  <td>引脚41</td>
  <td>引脚42</td>
</tr>
<tr>
  <td>I2C1</td>
  <td>引脚143</td>
  <td>引脚144</td>
</tr>
</tbody>
</table>

<table class="UC200A" >
<thead>
<tr>
  <th>I2C编号</th>
  <th>SCL引脚</th>
  <th>SDA引脚</th>
</tr>
</thead>
<tbody>
<tr>
  <td>I2C0</td>
  <td>引脚41</td>
  <td>引脚42</td>
</tr>
</tbody>
</table>

<table class="BC25PA" >
<thead>
<tr>
  <th>I2C编号</th>
  <th>SCL引脚</th>
  <th>SDA引脚</th>
</tr>
</thead>
<tbody>
<tr>
  <td>I2C0</td>
  <td>引脚23</td>
  <td>引脚22</td>
</tr>
<tr>
  <td>I2C1</td>
  <td>引脚20</td>
  <td>引脚21</td>
</tr>
</tbody>
</table>

<table class="EC800N" >
<thead>
<tr>
  <th>I2C编号</th>
  <th>SCL引脚</th>
  <th>SDA引脚</th>
</tr>
</thead>
<tbody>
<tr>
  <td>I2C0</td>
  <td>引脚67</td>
  <td>引脚66</td>
</tr>
</tbody>
</table>

<table class="BG95" >
<thead>
<tr>
  <th>I2C编号</th>
  <th>SCL引脚</th>
  <th>SDA引脚</th>
</tr>
</thead>
<tbody>
<tr>
  <td>I2C0</td>
  <td>引脚18</td>
  <td>引脚19</td>
</tr>
<tr>
  <td>I2C1</td>
  <td>引脚40</td>
  <td>引脚41</td>
</tr>
<tr>
  <td>I2C2</td>
  <td>引脚26</td>
  <td>引脚25</td>
</tr>
</tbody>
</table>

<table class="EC600M" >
<thead>
<tr>
  <th>I2C编号</th>
  <th>SCL引脚</th>
  <th>SDA引脚</th>
</tr>
</thead>
<tbody>
<tr>
  <td>I2C0</td>
  <td>引脚9</td>
  <td>引脚64</td>
</tr>
<tr>
  <td>I2C1</td>
  <td>引脚57</td>
  <td>引脚56</td>
</tr>
<tr>
  <td>I2C2</td>
  <td>引脚67</td>
  <td>引脚65</td>
</tr>
</tbody>
</table>

<table class="EG915U" >
<thead>
<tr>
  <th>I2C编号</th>
  <th>SCL引脚</th>
  <th>SDA引脚</th>
</tr>
</thead>
<tbody>
<tr>
  <td>I2C0</td>
  <td>引脚103</td>
  <td>引脚114</td>
</tr>
<tr>
  <td>I2C1</td>
  <td>引脚40</td>
  <td>引脚41</td>
</tr>
</tbody>
</table>


<table class="EC800M" >
<thead>
<tr>
  <th>I2C编号</th>
  <th>SCL引脚</th>
  <th>SDA引脚</th>
</tr>
</thead>
<tbody>
<tr>
  <td>I2C0</td>
  <td>引脚67</td>
  <td>引脚66</td>
</tr>
<tr>
  <td>I2C1</td>
  <td>引脚75</td>
  <td>引脚74</td>
</tr>
<tr>
  <td>I2C2</td>
  <td>引脚68</td>
  <td>引脚69</td>
</tr>
<tr>
  <td>I2C3</td>
  <td>引脚57</td>
  <td>引脚58</td>
</tr>
</tbody>
</table>

<table class="EG810M" >
<thead>
<tr>
  <th>I2C编号</th>
  <th>SCL引脚</th>
  <th>SDA引脚</th>
</tr>
</thead>
<tbody>
<tr>
  <td>I2C0</td>
  <td>引脚67</td>
  <td>引脚66</td>
</tr>
<tr>
  <td>I2C1</td>
  <td>引脚75</td>
  <td>引脚74</td>
</tr>
<tr>
  <td>I2C2</td>
  <td>引脚68</td>
  <td>引脚69</td>
</tr>
<tr>
  <td>I2C3</td>
  <td>引脚57</td>
  <td>引脚58</td>
</tr>
</tbody>
</table>


<table class="EG912N" >
<thead>
<tr>
  <th>I2C编号</th>
  <th>SCL引脚</th>
  <th>SDA引脚</th>
</tr>
</thead>
<tbody>
<tr>
  <td>I2C1</td>
  <td>引脚40</td>
  <td>引脚41</td>
</tr>
</tbody>
</table>

<table class="EC600E" >
<thead>
<tr>
  <th>I2C编号</th>
  <th>SCL引脚</th>
  <th>SDA引脚</th>
</tr>
</thead>
<tbody>
<tr>
  <td>I2C1</td>
  <td>引脚57(11)</td>
  <td>引脚56(12)</td>
</tr>
</tbody>
</table>

<table class="EC800E" >
<thead>
<tr>
  <th>I2C编号</th>
  <th>SCL引脚</th>
  <th>SDA引脚</th>
</tr>
</thead>
<tbody>
<tr>
  <td>I2C0</td>
  <td>引脚67(57)</td>
  <td>引脚66(58)</td>
</tr>
</tbody>
</table>

<table class="EC600G" >
<thead>
<tr>
  <th>I2C编号</th>
  <th>SCL引脚</th>
  <th>SDA引脚</th>
</tr>
</thead>
<tbody>
<tr>
  <td>I2C0</td>
  <td>引脚57</td>
  <td>引脚56</td>
</tr>
<tr>
  <td>I2C1</td>
  <td>引脚11</td>
  <td>引脚12</td>
</tr>
</tbody>
</table>

<table class="EC800G" >
<thead>
<tr>
  <th>I2C编号</th>
  <th>SCL引脚</th>
  <th>SDA引脚</th>
</tr>
</thead>
<tbody>
<tr>
  <td>I2C0</td>
  <td>引脚67</td>
  <td>引脚66</td>
</tr>
<tr>
  <td>I2C1</td>
  <td>引脚57</td>
  <td>引脚58</td>
</tr>
<tr>
  <td>I2C2</td>
  <td>引脚68</td>
  <td>引脚69</td>
</tr>
</tbody>
</table>

<table class="EG912U" >
<thead>
<tr>
  <th>I2C编号</th>
  <th>SCL引脚</th>
  <th>SDA引脚</th>
</tr>
</thead>
<tbody>
<tr>
  <td>I2C1</td>
  <td>引脚40</td>
  <td>引脚41</td>
</tr>
</tbody>
</table>

<table class="EC600K" >
<thead>
<tr>
  <th>I2C编号</th>
  <th>SCL引脚</th>
  <th>SDA引脚</th>
</tr>
</thead>
<tbody>
<tr>
  <td>I2C1</td>
  <td>引脚57</td>
  <td>引脚56</td>
</tr>
<tr>
  <td>I2C3</td>
  <td>引脚11</td>
  <td>引脚12</td>
</tr>
</tbody>
</table>

<table class="EC800K" >
<thead>
<tr>
  <th>I2C编号</th>
  <th>SCL引脚</th>
  <th>SDA引脚</th>
</tr>
</thead>
<tbody>
<tr>
  <td>I2C0</td>
  <td>引脚67</td>
  <td>引脚66</td>
</tr>
<tr>
  <td>I2C2</td>
  <td>引脚68</td>
  <td>引脚69</td>
</tr>
</tbody>
</table>

<table class="EG800K" >
<thead>
<tr>
  <th>I2C编号</th>
  <th>SCL引脚</th>
  <th>SDA引脚</th>
</tr>
</thead>
<tbody>
<tr>
  <td>I2C0</td>
  <td>引脚67</td>
  <td>引脚66</td>
</tr>
<tr>
  <td>I2C2</td>
  <td>引脚68</td>
  <td>引脚69</td>
</tr>
</tbody>
</table>

<table class="EG800AK" >
<thead>
<tr>
  <th>I2C编号</th>
  <th>SCL引脚</th>
  <th>SDA引脚</th>
</tr>
</thead>
<tbody>
<tr>
  <td>I2C0</td>
  <td>引脚67</td>
  <td>引脚66</td>
</tr>
<tr>
  <td>I2C2</td>
  <td>引脚68</td>
  <td>引脚69</td>
</tr>
</tbody>
</table>

<table class="FCM360W" >
<thead>
<tr>
  <th>I2C编号</th>
  <th>SCL引脚</th>
  <th>SDA引脚</th>
</tr>
</thead>
<tbody>
<tr>
  <td>I2C0</td>
  <td>引脚29</td>
  <td>引脚23</td>
</tr>
</tbody>
</table>

<table class="FCM362K" >
<thead>
<tr>
  <th>I2C编号</th>
  <th>SCL引脚</th>
  <th>SDA引脚</th>
</tr>
</thead>
<tbody>
<tr>
  <td>I2C0</td>
  <td>引脚30</td>
  <td>引脚29</td>
</tr>
</tbody>
</table>

<table class="EG800Z" >
<thead>
<tr>
  <th>I2C编号</th>
  <th>SCL引脚</th>
  <th>SDA引脚</th>
</tr>
</thead>
<tbody>
<tr>
  <td>I2C0</td>
  <td>引脚81(group=0)</td>
  <td>引脚80(group=0)</td>
</tr>
<tr>
  <td>I2C0</td>
  <td>引脚67(group=1)</td>
  <td>引脚66(group=1)</td>
</tr>
<tr>
  <td>I2C1</td>
  <td>引脚57</td>
  <td>引脚58</td>
</tr>
</tbody>
</table>


<table class="BC32" >
<thead>
<tr>
  <th>I2C编号</th>
  <th>SCL引脚</th>
  <th>SDA引脚</th>
</tr>
</thead>
<tbody>
<tr>
  <td>I2C0</td>
  <td>引脚12</td>
  <td>引脚26</td>
</tr>
<tr>
  <td>I2C1</td>
  <td>引脚43</td>
  <td>引脚44</td>
</tr>
</tbody>
</table>

<table class="BC92" >
<thead>
<tr>
  <th>I2C编号</th>
  <th>SCL引脚</th>
  <th>SDA引脚</th>
</tr>
</thead>
<tbody>
<tr>
  <td>I2C0</td>
  <td>引脚12</td>
  <td>引脚26</td>
</tr>
<tr>
  <td>I2C2</td>
  <td>引脚43</td>
  <td>引脚44</td>
</tr>
</tbody>
</table>

<table class="EG915N" >
<thead>
<tr>
  <th>I2C编号</th>
  <th>SCL引脚</th>
  <th>SDA引脚</th>
</tr>
</thead>
<tbody>
<tr>
  <td>I2C1</td>
  <td>引脚40</td>
  <td>引脚41</td>
</tr>
</tbody>
</table>

<table class="EC800Z" >
<thead>
<tr>
  <th>I2C编号</th>
  <th>SCL引脚</th>
  <th>SDA引脚</th>
</tr>
</thead>
<tbody>
<tr>
  <td>I2C0</td>
  <td>引脚67</td>
  <td>引脚66</td>
</tr>
<tr>
  <td>I2C1</td>
  <td>引脚57</td>
  <td>引脚58</td>
</tr>
</tbody>
</table>

<table class="BG950S" >
<thead>
<tr>
  <th>I2C编号</th>
  <th>SCL引脚</th>
  <th>SDA引脚</th>
</tr>
</thead>
<tbody>
<tr>
  <td>I2C0</td>
  <td>引脚40</td>
  <td>引脚41</td>
</tr>
</tbody>
</table>

</div>




## 方法

### `I2C.read`

```python
I2C.read(slaveaddress, addr,addr_len, r_data, datalen, delay)
```

该方法用于从 I2C 总线中读取数据。

**参数描述：**

- `slaveaddress` - I2C 设备地址，int类型，传入七位设备地址即可，低位自动补1。
- `addr` - I2C 寄存器地址，bytearray类型。
- `addr_len` - 寄存器地址长度，int类型。
- `r_data` - 接收数据的字节数组，bytearray类型。
- `datalen` - 字节数组的长度，int类型。
- `delay` - 延时，数据转换缓冲时间(单位ms)，int类型。

**返回值描述：**

成功返回整型值`0`，失败返回整型值`-1`。

### `I2C.write`

```python
I2C.write(slaveaddress, addr, addr_len, data, datalen)
```

该方法用于从 I2C 总线中写入数据。

**参数描述：**

- `slaveaddress` - I2C 设备地址，int类型，传入七位设备地址即可，低位自动补0。
- `addr` - I2C 寄存器地址，bytearray类型。
- `addr_len` - 寄存器地址长度，int类型。
- `data` - 写入的数据，bytearray类型。
- `datalen` - 写入数据的长度，int类型。

**返回值描述：**

成功返回整型值`0`，失败返回整型值`-1`。

**使用示例：**

> 需要连接设备使用。

```python
import log
from machine import I2C
import utime

'''
I2C使用示例
'''

# 设置日志输出级别
log.basicConfig(level=log.INFO)
i2c_log = log.getLogger("I2C")


if __name__ == '__main__':
    I2C_SLAVE_ADDR = 0x1B  # I2C  设备地址
    WHO_AM_I = bytearray([0x02, 0])   # I2C  寄存器地址，以buff的方式传入，取第一个值，计算一个值的长度

    data = bytearray([0x12, 0])   # 输入对应指令
    i2c_obj = I2C(I2C.I2C0, I2C.STANDARD_MODE)  # 返回I2C对象
    i2c_obj.write(I2C_SLAVE_ADDR, WHO_AM_I, 1, data, 2) # 写入data

    r_data = bytearray(2)  # 创建长度为2的字节数组接收
    i2c_obj.read(I2C_SLAVE_ADDR, WHO_AM_I, 1, r_data, 2, 0)   # read
    i2c_log.info(r_data[0])
    i2c_log.info(r_data[1])

```

## 常量

<div class="table-cons-QuecPython">
<table class="EC600U" style="width: 100%;">
<thead>
<tr>
  <th>常量</th>
  <th>说明</th>
</tr>
</thead>
<tbody>
<tr>
  <td>I2C.I2C0</td>
  <td>I2C通路索引号: 0</td>
</tr>
<tr>
  <td>I2C.I2C1</td>
  <td>I2C通路索引号: 1</td>
</tr>
<tr>
  <td>I2C.STANDARD_MODE</td>
  <td>标准模式</td>
</tr>
<tr>
  <td>I2C.FAST_MODE</td>
  <td>快速模式</td>
</tr>
</tbody>
</table>

<table class="EC600K" style="width: 100%;">
<thead>
<tr>
  <th>常量</th>
  <th>说明</th>
</tr>
</thead>
<tbody>
<tr>
  <td>I2C.I2C1</td>
  <td>I2C通路索引号: 1</td>
</tr>
<tr>
  <td>I2C.I2C3</td>
  <td>I2C通路索引号: 3</td>
</tr>
<tr>
  <td>I2C.STANDARD_MODE</td>
  <td>标准模式</td>
</tr>
<tr>
  <td>I2C.FAST_MODE</td>
  <td>快速模式</td>
</tr>
</tbody>
</table>

<table class="EC600E" style="width: 100%;">
<thead>
<tr>
  <th>常量</th>
  <th>说明</th>
</tr>
</thead>
<tbody>
<tr>
  <td>I2C.I2C1</td>
  <td>I2C通路索引号: 1</td>
</tr>
<tr>
  <td>I2C.STANDARD_MODE</td>
  <td>标准模式</td>
</tr>
<tr>
  <td>I2C.FAST_MODE</td>
  <td>快速模式</td>
</tr>
</tbody>
</table>

<table class="EG915N" style="width: 100%;">
<thead>
<tr>
  <th>常量</th>
  <th>说明</th>
</tr>
</thead>
<tbody>
<tr>
  <td>I2C.I2C1</td>
  <td>I2C通路索引号: 1</td>
</tr>
<tr>
  <td>I2C.STANDARD_MODE</td>
  <td>标准模式</td>
</tr>
<tr>
  <td>I2C.FAST_MODE</td>
  <td>快速模式</td>
</tr>
</tbody>
</table>

<table class="EC200U" style="width: 100%;">
<thead>
<tr>
  <th>常量</th>
  <th>说明</th>
</tr>
</thead>
<tbody>
<tr>
  <td>I2C.I2C0</td>
  <td>I2C通路索引号: 0</td>
</tr>
<tr>
  <td>I2C.I2C1</td>
  <td>I2C通路索引号: 1</td>
</tr>
<tr>
  <td>I2C.STANDARD_MODE</td>
  <td>标准模式</td>
</tr>
<tr>
  <td>I2C.FAST_MODE</td>
  <td>快速模式</td>
</tr>
</tbody>
</table>

<table class="BC25PA" style="width: 100%;">
<thead>
<tr>
  <th>常量</th>
  <th>说明</th>
</tr>
</thead>
<tbody>
<tr>
  <td>I2C.I2C0</td>
  <td>I2C通路索引号: 0</td>
</tr>
<tr>
  <td>I2C.I2C1</td>
  <td>I2C通路索引号: 1</td>
</tr>
<tr>
  <td>I2C.STANDARD_MODE</td>
  <td>标准模式</td>
</tr>
<tr>
  <td>I2C.FAST_MODE</td>
  <td>快速模式</td>
</tr>
</tbody>
</table>

<table class="BC25" style="width: 100%;">
<thead>
<tr>
  <th>常量</th>
  <th>说明</th>
</tr>
</thead>
<tbody>
<tr>
  <td>I2C.I2C0</td>
  <td>I2C通路索引号: 0</td>
</tr>
<tr>
  <td>I2C.I2C1</td>
  <td>I2C通路索引号: 1</td>
</tr>
<tr>
  <td>I2C.STANDARD_MODE</td>
  <td>标准模式</td>
</tr>
<tr>
  <td>I2C.FAST_MODE</td>
  <td>快速模式</td>
</tr>
</tbody>
</table>

<table class="EG915U" style="width: 100%;">
<thead>
<tr>
  <th>常量</th>
  <th>说明</th>
</tr>
</thead>
<tbody>
<tr>
  <td>I2C.I2C0</td>
  <td>I2C通路索引号: 0</td>
</tr>
<tr>
  <td>I2C.I2C1</td>
  <td>I2C通路索引号: 1</td>
</tr>
<tr>
  <td>I2C.STANDARD_MODE</td>
  <td>标准模式</td>
</tr>
<tr>
  <td>I2C.FAST_MODE</td>
  <td>快速模式</td>
</tr>
</tbody>
</table>

<table class="EC800M" style="width: 100%;">
<thead>
<tr>
  <th>常量</th>
  <th>说明</th>
</tr>
</thead>
<tbody>
<tr>
  <td>I2C.I2C0</td>
  <td>I2C通路索引号: 0</td>
</tr>
<tr>
  <td>I2C.I2C1</td>
  <td>I2C通路索引号: 1</td>
</tr>
<tr>
  <td>I2C.I2C2</td>
  <td>I2C通路索引号: 2</td>
</tr>
<tr>
  <td>I2C.I2C3</td>
  <td>I2C通路索引号: 3</td>
</tr>
<tr>
  <td>I2C.STANDARD_MODE</td>
  <td>标准模式</td>
</tr>
<tr>
  <td>I2C.FAST_MODE</td>
  <td>快速模式</td>
</tr>
</tbody>
</table>

<table class="EG810M" style="width: 100%;">
<thead>
<tr>
  <th>常量</th>
  <th>说明</th>
</tr>
</thead>
<tbody>
<tr>
  <td>I2C.I2C0</td>
  <td>I2C通路索引号: 0</td>
</tr>
<tr>
  <td>I2C.I2C1</td>
  <td>I2C通路索引号: 1</td>
</tr>
<tr>
  <td>I2C.I2C2</td>
  <td>I2C通路索引号: 2</td>
</tr>
<tr>
  <td>I2C.I2C3</td>
  <td>I2C通路索引号: 3</td>
</tr>
<tr>
  <td>I2C.STANDARD_MODE</td>
  <td>标准模式</td>
</tr>
<tr>
  <td>I2C.FAST_MODE</td>
  <td>快速模式</td>
</tr>
</tbody>
</table>

<table class="BC32" style="width: 100%;">
<thead>
<tr>
  <th>常量</th>
  <th>说明</th>
</tr>
</thead>
<tbody>
<tr>
  <td>I2C.I2C0</td>
  <td>I2C通路索引号: 0</td>
</tr>
<tr>
  <td>I2C.I2C1</td>
  <td>I2C通路索引号: 1</td>
</tr>
<tr>
  <td>I2C.STANDARD_MODE</td>
  <td>标准模式</td>
</tr>
<tr>
  <td>I2C.FAST_MODE</td>
  <td>快速模式</td>
</tr>
</tbody>
</table>

<table class="BC92" style="width: 100%;">
<thead>
<tr>
  <th>常量</th>
  <th>说明</th>
</tr>
</thead>
<tbody>
<tr>
  <td>I2C.I2C0</td>
  <td>I2C通路索引号: 0</td>
</tr>
<tr>
  <td>I2C.I2C1</td>
  <td>I2C通路索引号: 1</td>
</tr>
<tr>
  <td>I2C.STANDARD_MODE</td>
  <td>标准模式</td>
</tr>
<tr>
  <td>I2C.FAST_MODE</td>
  <td>快速模式</td>
</tr>
</tbody>
</table>

<table class="EC800Z" style="width: 100%;">
<thead>
<tr>
  <th>常量</th>
  <th>说明</th>
</tr>
</thead>
<tbody>
<tr>
  <td>I2C.I2C0</td>
  <td>I2C通路索引号: 0</td>
</tr>
<tr>
  <td>I2C.I2C1</td>
  <td>I2C通路索引号: 1</td>
</tr>
<tr>
  <td>I2C.STANDARD_MODE</td>
  <td>标准模式</td>
</tr>
<tr>
  <td>I2C.FAST_MODE</td>
  <td>快速模式</td>
</tr>
</tbody>
</table>

<table class="BG95M3" style="width: 100%;">
<thead>
<tr>
  <th>常量</th>
  <th>说明</th>
</tr>
</thead>
<tbody>
<tr>
  <td>I2C.I2C0</td>
  <td>I2C通路索引号: 0</td>
</tr>
<tr>
  <td>I2C.I2C1</td>
  <td>I2C通路索引号: 1</td>
</tr>
<tr>
  <td>I2C.I2C2</td>
  <td>I2C通路索引号: 2</td>
</tr>
<tr>
  <td>I2C.STANDARD_MODE</td>
  <td>标准模式</td>
</tr>
<tr>
  <td>I2C.FAST_MODE</td>
  <td>快速模式</td>
</tr>
</tbody>
</table>

<table class="EC600M" style="width: 100%;">
<thead>
<tr>
  <th>常量</th>
  <th>说明</th>
</tr>
</thead>
<tbody>
<tr>
  <td>I2C.I2C0</td>
  <td>I2C通路索引号: 0</td>
</tr>
<tr>
  <td>I2C.I2C1</td>
  <td>I2C通路索引号: 1</td>
</tr>
<tr>
  <td>I2C.I2C2</td>
  <td>I2C通路索引号: 2</td>
</tr>
<tr>
  <td>I2C.STANDARD_MODE</td>
  <td>标准模式</td>
</tr>
<tr>
  <td>I2C.FAST_MODE</td>
  <td>快速模式</td>
</tr>
</tbody>
</table>

<table class="EC800K" style="width: 100%;">
<thead>
<tr>
  <th>常量</th>
  <th>说明</th>
</tr>
</thead>
<tbody>
<tr>
  <td>I2C.I2C0</td>
  <td>I2C通路索引号: 0</td>
</tr>
<tr>
  <td>I2C.I2C1</td>
  <td>I2C通路索引号: 1</td>
</tr>
<tr>
  <td>I2C.I2C2</td>
  <td>I2C通路索引号: 2</td>
</tr>
<tr>
  <td>I2C.STANDARD_MODE</td>
  <td>标准模式</td>
</tr>
<tr>
  <td>I2C.FAST_MODE</td>
  <td>快速模式</td>
</tr>
</tbody>
</table>

<table class="EG800K" style="width: 100%;">
<thead>
<tr>
  <th>常量</th>
  <th>说明</th>
</tr>
</thead>
<tbody>
<tr>
  <td>I2C.I2C0</td>
  <td>I2C通路索引号: 0</td>
</tr>
<tr>
  <td>I2C.I2C1</td>
  <td>I2C通路索引号: 1</td>
</tr>
<tr>
  <td>I2C.I2C2</td>
  <td>I2C通路索引号: 2</td>
</tr>
<tr>
  <td>I2C.STANDARD_MODE</td>
  <td>标准模式</td>
</tr>
<tr>
  <td>I2C.FAST_MODE</td>
  <td>快速模式</td>
</tr>
</tbody>
</table>

<table class="EG800AK" style="width: 100%;">
<thead>
<tr>
  <th>常量</th>
  <th>说明</th>
</tr>
</thead>
<tbody>
<tr>
  <td>I2C.I2C0</td>
  <td>I2C通路索引号: 0</td>
</tr>
<tr>
  <td>I2C.I2C1</td>
  <td>I2C通路索引号: 1</td>
</tr>
<tr>
  <td>I2C.I2C2</td>
  <td>I2C通路索引号: 2</td>
</tr>
<tr>
  <td>I2C.STANDARD_MODE</td>
  <td>标准模式</td>
</tr>
<tr>
  <td>I2C.FAST_MODE</td>
  <td>快速模式</td>
</tr>
</tbody>
</table>

<table class="EC800N" style="width: 100%;">
<thead>
<tr>
  <th>常量</th>
  <th>说明</th>
</tr>
</thead>
<tbody>
<tr>
  <td>I2C.I2C0</td>
  <td>I2C通路索引号: 0</td>
</tr>
<tr>
  <td>I2C.STANDARD_MODE</td>
  <td>标准模式</td>
</tr>
<tr>
  <td>I2C.FAST_MODE</td>
  <td>快速模式</td>
</tr>
</tbody>
</table>

<table class="EC800E" style="width: 100%;">
<thead>
<tr>
  <th>常量</th>
  <th>说明</th>
</tr>
</thead>
<tbody>
<tr>
  <td>I2C.I2C0</td>
  <td>I2C通路索引号: 0</td>
</tr>
<tr>
  <td>I2C.STANDARD_MODE</td>
  <td>标准模式</td>
</tr>
<tr>
  <td>I2C.FAST_MODE</td>
  <td>快速模式</td>
</tr>
</tbody>
</table>

<table class="FCM362K" style="width: 100%;">
<thead>
<tr>
  <th>常量</th>
  <th>说明</th>
</tr>
</thead>
<tbody>
<tr>
  <td>I2C.I2C0</td>
  <td>I2C通路索引号: 0</td>
</tr>
<tr>
  <td>I2C.STANDARD_MODE</td>
  <td>标准模式</td>
</tr>
<tr>
  <td>I2C.FAST_MODE</td>
  <td>快速模式</td>
</tr>
</tbody>
</table>

<table class="EG800Z" style="width: 100%;">
<thead>
<tr>
  <th>常量</th>
  <th>说明</th>
</tr>
</thead>
<tbody>
<tr>
  <td>I2C.I2C0</td>
  <td>I2C通路索引号: 0</td>
</tr>
<tr>
  <td>I2C.I2C1</td>
  <td>I2C通路索引号: 1</td>
</tr>
<tr>
  <td>I2C.STANDARD_MODE</td>
  <td>标准模式</td>
</tr>
<tr>
  <td>I2C.FAST_MODE</td>
  <td>快速模式</td>
</tr>
</tbody>
</table>


<table class="EC200A" style="width: 100%;">
<thead>
<tr>
  <th>常量</th>
  <th>说明</th>
</tr>
</thead>
<tbody>
<tr>
  <td>I2C.I2C0</td>
  <td>I2C通路索引号: 0</td>
</tr>
<tr>
  <td>I2C.STANDARD_MODE</td>
  <td>标准模式</td>
</tr>
<tr>
  <td>I2C.FAST_MODE</td>
  <td>快速模式</td>
</tr>
</tbody>
</table>

<table class="EC200A-V1" style="width: 100%;">
<thead>
<tr>
  <th>常量</th>
  <th>说明</th>
</tr>
</thead>
<tbody>
<tr>
  <td>I2C.I2C0</td>
  <td>I2C通路索引号: 0</td>
</tr>
<tr>
  <td>I2C.STANDARD_MODE</td>
  <td>标准模式</td>
</tr>
<tr>
  <td>I2C.FAST_MODE</td>
  <td>快速模式</td>
</tr>
</tbody>
</table>

<table class="FCM360W" style="width: 100%;">
<thead>
<tr>
  <th>常量</th>
  <th>说明</th>
</tr>
</thead>
<tbody>
<tr>
  <td>I2C.I2C0</td>
  <td>I2C通路索引号: 0</td>
</tr>
<tr>
  <td>I2C.STANDARD_MODE</td>
  <td>标准模式</td>
</tr>
<tr>
  <td>I2C.FAST_MODE</td>
  <td>快速模式</td>
</tr>
<tr>
  <td>I2C.ENHANCED_FAST_MODE</td>
  <td>快速模式增强</td>
</tr>
</tbody>
</table>

<table class="BG950S" style="width: 100%;">
<thead>
<tr>
  <th>常量</th>
  <th>说明</th>
</tr>
</thead>
<tbody>
<tr>
  <td>I2C.I2C0</td>
  <td>I2C通路索引号: 0</td>
</tr>
<tr>
  <td>I2C.STANDARD_MODE</td>
  <td>标准模式</td>
</tr>
<tr>
  <td>I2C.FAST_MODE</td>
  <td>快速模式</td>
</tr>
</tbody>
</table>


</div>