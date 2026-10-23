# class UART - 串口通信

该类提供uart串口数据传输功能。

## 构造函数

### `machine.UART`

<div class="content-QuecPython">
<div class="EC600U|EC200U|EG915U|EC800G">
<pre class="language-python"><code class="language-python">class machine.UART(UART.UARTn, baudrate, databits, parity, stopbits, flowctl)
</code></pre>
<p><strong>参数描述：</strong></p>
<ul>
<li><p><code>UARTn</code> - UART编号，int类型，UARTn说明如下：<br/><code>UART0</code> - DEBUG PORT <br/><code>UART1</code> - BT PORT <br/><code>UART2</code> - MAIN PORT <br/><code>UART3</code> - USB CDC PORT <br/><code>UART4</code> - STDOUT PORT <br/></p>
</li>
<li><p><code>baudratec</code> - 波特率，int类型，支持常用波特率，如<code>4800</code>、<code>9600</code>、<code>19200</code>、<code>38400</code>、<code>57600</code>、<code>115200</code>、<code>230400</code>等;<br/></p></li>
<li><p><code>databits</code> - 数据位仅支持8位，int类型。<br/></p></li>
<li><p><code>parity</code> - 奇偶校验(0 – NONE，1 – EVEN，2 – ODD)，int类型。<br/></p></li>
<li><p><code>stopbits</code> - 停止位[1 ~ 2]，int类型。<br/></p></li>
<li><p><code>flowctl</code> - 硬件控制流(0 – FC_NONE， 1 – FC_HW)，int类型。<br/></p>
</li>
</ul>
</div>

<div class="EG915N">
<pre class="language-python"><code class="language-python">class machine.UART(UART.UARTn, baudrate, databits, parity, stopbits, flowctl)
</code></pre>
<p><strong>参数描述：</strong></p>
<ul>
<li><p><code>UARTn</code> - UART编号，int类型，UARTn说明如下：<br/><code>UART0</code> - DEBUG PORT <br/><code>UART1</code> - BT PORT <br/><code>UART2</code> - MAIN PORT <br/><code>UART3</code> - USB CDC PORT <br/><code>UART4</code> - STDOUT PORT <br/></p>
</li>
<li><p><code>baudratec</code> - 波特率，int类型，支持常用波特率，如<code>4800</code>、<code>9600</code>、<code>19200</code>、<code>38400</code>、<code>57600</code>、<code>115200</code>、<code>230400</code>等;<br/></p></li>
<li><p><code>databits</code> - 数据位[5 ~ 8]，int类型。<br/></p></li>
<li><p><code>parity</code> - 奇偶校验(0 – NONE，1 – EVEN，2 – ODD)，int类型。<br/></p></li>
<li><p><code>stopbits</code> - 停止位[1 ~ 2]，int类型。<br/></p></li>
<li><p><code>flowctl</code> - 硬件控制流(0 – FC_NONE， 1 – FC_HW)，int类型。<br/></p>
</li>
</ul>
</div>

<div class="EC800G">
<pre class="language-python"><code class="language-python">class machine.UART(UART.UARTn, baudrate, databits, parity, stopbits, flowctl)
</code></pre>
<p><strong>参数描述：</strong></p>
<ul>
<li><p><code>UARTn</code> - UART编号，int类型，UARTn说明如下：<br/><code>UART0</code> - DEBUG PORT <br/><code>UART1</code> - BT PORT <br/><code>UART2</code> - MAIN PORT <br/><code>UART3</code> - USB CDC PORT <br/></p>
</li>
<li><p><code>baudratec</code> - 波特率，int类型，支持常用波特率，如<code>4800</code>、<code>9600</code>、<code>19200</code>、<code>38400</code>、<code>57600</code>、<code>115200</code>、<code>230400</code>等;<br/></p></li>
<li><p><code>databits</code> - 数据位仅支持8位，int类型。<br/></p></li>
<li><p><code>parity</code> - 奇偶校验(0 – NONE，1 – EVEN，2 – ODD)，int类型。<br/></p></li>
<li><p><code>stopbits</code> - 停止位[1 ~ 2]，int类型。<br/></p></li>
<li><p><code>flowctl</code> - 硬件控制流(0 – FC_NONE， 1 – FC_HW)，int类型。<br/></p>
</li>
</ul>
</div>

<div class="EC200A|EC200A-V1|UC200A|EC800N|BC25PA|EG912N|EC600E|EC800E|EC600G|EC600K|EG800P|BC92|BC32|FCM360W|FCM362K|EG912U">
<pre class="language-python"><code class="language-python">class machine.UART(UART.UARTn, baudrate, databits, parity, stopbits, flowctl)
</code></pre>
<p><strong>参数描述：</strong></p>
<ul>
<li><p><code>UARTn</code> - UART编号，int类型，UARTn说明如下：<br/><code>UART0</code> - DEBUG PORT <br/><code>UART1</code> - BT PORT <br/><code>UART2</code> - MAIN PORT <br/><code>UART3</code> - USB CDC PORT <br/></p>
</li>
<li><p><code>baudratec</code> - 波特率，int类型，支持常用波特率，如<code>4800</code>、<code>9600</code>、<code>19200</code>、<code>38400</code>、<code>57600</code>、<code>115200</code>、<code>230400</code>等;<br/></p></li>
<li><p><code>databits</code> - 数据位[5 ~ 8]，int类型。<br/></p></li>
<li><p><code>parity</code> - 奇偶校验(0 – NONE，1 – EVEN，2 – ODD)，int类型。<br/></p></li>
<li><p><code>stopbits</code> - 停止位[1 ~ 2]，int类型。<br/></p></li>
<li><p><code>flowctl</code> - 硬件控制流(0 – FC_NONE， 1 – FC_HW)，int类型。<br/></p>
</li>
</ul>
</div>

<div class="EC800Z|EG800Z">
<pre class="language-python"><code class="language-python">class machine.UART(UART.UARTn, baudrate, databits, parity, stopbits, flowctl, [group])
</code></pre>
<p><strong>参数描述：</strong></p>
<ul>
<li><p><code>UARTn</code> - UART编号，int类型，UARTn说明如下：<br/><code>UART0</code> - DEBUG PORT <br/><code>UART1</code> - BT PORT <br/><code>UART2</code> - MAIN PORT <br/><code>UART3</code> - USB CDC PORT <br/></p>
</li>
<li><p><code>baudratec</code> - 波特率，int类型，支持常用波特率，如<code>4800</code>、<code>9600</code>、<code>19200</code>、<code>38400</code>、<code>57600</code>、<code>115200</code>、<code>230400</code>等;<br/></p></li>
<li><p><code>databits</code> - 数据位[8]，int类型。<br/></p></li>
<li><p><code>parity</code> - 奇偶校验(0 – NONE，1 – EVEN，2 – ODD)，int类型。<br/></p></li>
<li><p><code>stopbits</code> - 停止位[1 ~ 2]，int类型。<br/></p></li>
<li><p><code>flowctl</code> - 硬件控制流(0 – FC_NONE， 1 – FC_HW)，int类型。<br/></p>
</li>
<li><p><code>[group]</code> - 选择在不同管脚使用UART，缺省值为0。int类型。<br/></p>
</li>
</ul>
</div>

<div class="EC800K|EG800K|EG800AK|EC600M|EC800M|EG810M">
<pre class="language-python"><code class="language-python">class machine.UART(UART.UARTn, baudrate, databits, parity, stopbits, flowctl, [group])
</code></pre>
<p><strong>参数描述：</strong></p>
<ul>
<li><p><code>UARTn</code> - UART编号，int类型，UARTn说明如下：<br/><code>UART0</code> - DEBUG PORT <br/><code>UART1</code> - BT PORT <br/><code>UART2</code> - MAIN PORT <br/><code>UART3</code> - USB CDC PORT <br/></p>
</li>
<li><p><code>baudratec</code> - 波特率，int类型，支持常用波特率，如<code>4800</code>、<code>9600</code>、<code>19200</code>、<code>38400</code>、<code>57600</code>、<code>115200</code>、<code>230400</code>等;<br/></p></li>
<li><p><code>databits</code> - 数据位[5 ~ 8]，int类型。<br/></p></li>
<li><p><code>parity</code> - 奇偶校验(0 – NONE，1 – EVEN，2 – ODD)，int类型。<br/></p></li>
<li><p><code>stopbits</code> - 停止位[1 ~ 2]，int类型。<br/></p></li>
<li><p><code>flowctl</code> - 硬件控制流(0 – FC_NONE， 1 – FC_HW)，int类型。<br/></p>
<li><p><code>[group]</code> - 选择在不同管脚使用UART，缺省值为0。int类型。<br/></p>
</li>
</ul>
</div>
<div class="BG95">
<pre class="language-python"><code class="language-python">class machine.UART(UART.UARTn, baudrate, databits, parity, stopbits, flowctl)
</code></pre>
<p><strong>参数描述：</strong></p>
<ul>
<li><p><code>UARTn</code> - UART编号，int类型，UARTn说明如下：<br/><code>UART0</code> - DEBUG PORT <br/><code>UART1</code> - BT PORT <br/><code>UART2</code> - MAIN PORT <br/><code>UART3</code> - USB CDC PORT (不支持BG95M3) <br/></p>
</li>
<li><p><code>baudratec</code> - 波特率，int类型，支持常用波特率，如<code>4800</code>、<code>9600</code>、<code>19200</code>、<code>38400</code>、<code>57600</code>、<code>115200</code>、<code>230400</code>等;<br/></p></li>
<li><p><code>databits</code> - 数据位[5 ~ 8]，int类型。<br/></p></li>
<li><p><code>parity</code> - 奇偶校验(0 – NONE，1 – EVEN，2 – ODD)，int类型。<br/></p></li>
<li><p><code>stopbits</code> - 停止位[1 ~ 2]，int类型。<br/></p></li>
<li><p><code>flowctl</code> - 硬件控制流(0 – FC_NONE， 1 – FC_HW)，int类型。<br/></p>
</li>
</ul>
</div>
<div class="BG950S">
<pre class="language-python"><code class="language-python">class machine.UART(UART.UARTn, baudrate, databits, parity, stopbits, flowctl)
</code></pre>
<p><strong>参数描述：</strong></p>
<ul>
<li><p><code>UARTn</code> - UART编号，int类型，UARTn说明如下：<br/><code>UART0</code> - DEBUG PORT <br/><code>UART1</code> - BT PORT <br/><code>UART2</code> - MAIN PORT <br/><code>UART3</code> - USB CDC PORT <br/></p>
</li>
<li><p><code>baudratec</code> - 波特率，int类型，支持常用波特率，如<code>4800</code>、<code>9600</code>、<code>19200</code>、<code>38400</code>、<code>57600</code>、<code>115200</code>、<code>230400</code>等;<br/></p></li>
<li><p><code>databits</code> - 数据位[5 ~ 8]，int类型。<br/></p></li>
<li><p><code>parity</code> - 奇偶校验(0 – NONE，1 – EVEN，2 – ODD)，int类型。<br/></p></li>
<li><p><code>stopbits</code> - 停止位[1 ~ 2]，int类型。<br/></p></li>
<li><p><code>flowctl</code> - 硬件控制流(0 – FC_NONE， 1 – FC_HW)，int类型。<br/></p>
</li>
</ul>
</div>



</div>

**UART引脚对应关系 ：**

<div class="table-QuecPython">
<table class="EC600U">
<thead>
  <th>uart编号</th>
  <th>TX引脚</th>
  <th>RX引脚</th>
  <th>RTS引脚</th>
  <th>CTS引脚</th>
</tr>
</thead>
<tbody>
<tr>
  <td>uart1</td>
  <td>引脚号124</td>
  <td>引脚号123</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
<tr>
  <td>uart2</td>
  <td>引脚号32</td>
  <td>引脚号31</td>
  <td>引脚号34</td>
  <td>引脚号33</td>
</tr>
<tr>
  <td>uart4</td>
  <td>引脚号103</td>
  <td>引脚号104</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
</tbody>
</table>

<table class="EC200U">
<thead>
  <th>uart编号</th>
  <th>TX引脚</th>
  <th>RX引脚</th>
  <th>RTS引脚</th>
  <th>CTS引脚</th>
</tr>
</thead>
<tbody>
<tr>
  <td>uart1</td>
  <td>引脚号138</td>
  <td>引脚号137</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
<tr>
  <td>uart2</td>
  <td>引脚号67</td>
  <td>引脚号68</td>
  <td>引脚号65</td>
  <td>引脚号64</td>
</tr>
<tr>
  <td>uart4</td>
  <td>引脚号82</td>
  <td>引脚号81</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
</tbody>
</table>

<table class="EC200A">
<thead>
  <th>uart编号</th>
  <th>TX引脚</th>
  <th>RX引脚</th>
  <th>RTS引脚</th>
  <th>CTS引脚</th>
</tr>
</thead>
<tbody>
<tr>
  <td>uart0</td>
  <td>引脚号12</td>
  <td>引脚号11</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
<tr>
  <td>uart1</td>
  <td>引脚号63</td>
  <td>引脚号66</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
<tr>
  <td>uart2</td>
  <td>引脚号67</td>
  <td>引脚号68</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
</tbody>
</table>

<table class="EC200A-V1">
<thead>
  <th>uart编号</th>
  <th>TX引脚</th>
  <th>RX引脚</th>
  <th>RTS引脚</th>
  <th>CTS引脚</th>
</tr>
</thead>
<tbody>
<tr>
  <td>uart0</td>
  <td>引脚号12</td>
  <td>引脚号11</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
<tr>
  <td>uart1</td>
  <td>引脚号26</td>
  <td>引脚号27</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
<tr>
  <td>uart2</td>
  <td>引脚号67</td>
  <td>引脚号68</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
</tbody>
</table>

<table class="UC200A">
<thead>
  <th>uart编号</th>
  <th>TX引脚</th>
  <th>RX引脚</th>
  <th>RTS引脚</th>
  <th>CTS引脚</th>
</tr>
</thead>
<tbody>
<tr>
  <td>uart0</td>
  <td>引脚号12</td>
  <td>引脚号11</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
<tr>
  <td>uart1</td>
  <td>引脚号63</td>
  <td>引脚号66</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
<tr>
  <td>uart2</td>
  <td>引脚号67</td>
  <td>引脚号68</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
</tbody>
</table>

<table class="EC800N">
<thead>
  <th>uart编号</th>
  <th>TX引脚</th>
  <th>RX引脚</th>
  <th>RTS引脚</th>
  <th>CTS引脚</th>
</tr>
</thead>
<tbody>
<tr>
  <td>uart0</td>
  <td>引脚号39</td>
  <td>引脚号38</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
<tr>
  <td>uart1</td>
  <td>引脚号50</td>
  <td>引脚号51</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
<tr>
  <td>uart2</td>
  <td>引脚号18</td>
  <td>引脚号17</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
</tbody>
</table>

<table class="BC25PA">
<thead>
  <th>uart编号</th>
  <th>TX引脚</th>
  <th>RX引脚</th>
  <th>RTS引脚</th>
  <th>CTS引脚</th>
</tr>
</thead>
<tbody>
<tr>
  <td>uart1</td>
  <td>引脚号29</td>
  <td>引脚号28</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
</tbody>
</table>

<table class="BG95">
<thead>
  <th>uart编号</th>
  <th>TX引脚</th>
  <th>RX引脚</th>
  <th>RTS引脚</th>
  <th>CTS引脚</th>
</tr>
</thead>
<tbody>
<tr>
  <td>uart0</td>
  <td>引脚号23</td>
  <td>引脚号22</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
<tr>
  <td>uart1</td>
  <td>引脚号27</td>
  <td>引脚号28</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
<tr>
  <td>uart2</td>
  <td>引脚号64</td>
  <td>引脚号65</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
<tr>
  <td>uart4</td>
  <td>引脚号35</td>
  <td>引脚号34</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
</tbody>
</table>

<table class="EC600M">
<thead>
  <th>uart编号</th>
  <th>TX引脚</th>
  <th>RX引脚</th>
  <th>RTS引脚</th>
  <th>CTS引脚</th>
</tr>
</thead>
<tbody>
<tr>
  <td>uart0</td>
  <td>引脚号71</td>
  <td>引脚号72</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
<tr>
  <td>uart1(group = 0)</td>
  <td>引脚号3</td>
  <td>引脚号2</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
<tr>
  <td>uart1(group = 1)</td>
  <td>引脚号33</td>
  <td>引脚号34</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
<tr>
  <td>uart2</td>
  <td>引脚号32</td>
  <td>引脚号31</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
</tbody>
</table>

<table class="EG915U">
<thead>
  <th>uart编号</th>
  <th>TX引脚</th>
  <th>RX引脚</th>
  <th>RTS引脚</th>
  <th>CTS引脚</th>
</tr>
</thead>
<tbody>
<tr>
  <td>uart1</td>
  <td>引脚号27</td>
  <td>引脚号28</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
<tr>
  <td>uart2</td>
  <td>引脚号35</td>
  <td>引脚号34</td>
  <td>引脚号37</td>
  <td>引脚号36</td>
</tr>
<tr>
  <td>uart4</td>
  <td>引脚号19</td>
  <td>引脚号18</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
</tbody>
</table>

<table class="EC800M">
<thead>
  <th>uart编号</th>
  <th>TX引脚</th>
  <th>RX引脚</th>
  <th>RTS引脚</th>
  <th>CTS引脚</th>
</tr>
</thead>
<tbody>
<tr>
  <td>uart0</td>
  <td>引脚号39</td>
  <td>引脚号38</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
<tr>
  <td>uart1(group = 0)</td>
  <td>引脚号50</td>
  <td>引脚号51</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
<tr>
  <td>uart1(group = 1)</td>
  <td>引脚号22</td>
  <td>引脚号23</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
<tr>
  <td>uart2</td>
  <td>引脚号18</td>
  <td>引脚号17</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
<tr>
  <td>uart4</td>
  <td>引脚号29</td>
  <td>引脚号28</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
</tbody>
</table>

<table class="EG810M">
<thead>
  <th>uart编号</th>
  <th>TX引脚</th>
  <th>RX引脚</th>
  <th>RTS引脚</th>
  <th>CTS引脚</th>
</tr>
</thead>
<tbody>
<tr>
  <td>uart0</td>
  <td>引脚号39</td>
  <td>引脚号38</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
<tr>
  <td>uart1(group = 0)</td>
  <td>引脚号50</td>
  <td>引脚号51</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
<tr>
  <td>uart1(group = 1)</td>
  <td>引脚号22</td>
  <td>引脚号23</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
<tr>
  <td>uart2</td>
  <td>引脚号18</td>
  <td>引脚号17</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
<tr>
  <td>uart4</td>
  <td>引脚号29</td>
  <td>引脚号28</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
</tbody>
</table>

<table class="EG912N">
<thead>
  <th>uart编号</th>
  <th>TX引脚</th>
  <th>RX引脚</th>
  <th>RTS引脚</th>
  <th>CTS引脚</th>
</tr>
</thead>
<tbody>
<tr>
  <td>uart0</td>
  <td>引脚号23</td>
  <td>引脚号22</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
<tr>
  <td>uart1(flowctl = 0)</td>
  <td>引脚号27</td>
  <td>引脚号28</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
<tr>
  <td>uart1(flowctl = 1)</td>
  <td>引脚号36</td>
  <td>引脚号37</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
<tr>
  <td>uart2</td>
  <td>引脚号35</td>
  <td>引脚号34</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
</tbody>
</table>

<table class="EC600E">
<thead>
  <th>uart编号</th>
  <th>TX引脚</th>
  <th>RX引脚</th>
  <th>RTS引脚</th>
  <th>CTS引脚</th>
</tr>
</thead>
<tbody>
<tr>
  <td>uart0</td>
  <td>引脚号71</td>
  <td>引脚号72</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
<tr>
  <td>uart1</td>
  <td>引脚号70</td>
  <td>引脚号69</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
<tr>
  <td>uart2</td>
  <td>引脚号32</td>
  <td>引脚号31</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
</tbody>
</table>

<table class="EC800E">
<thead>
  <th>uart编号</th>
  <th>TX引脚</th>
  <th>RX引脚</th>
  <th>RTS引脚</th>
  <th>CTS引脚</th>
</tr>
</thead>
<tbody>
<tr>
  <td>uart0</td>
  <td>引脚号39</td>
  <td>引脚号38</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
<tr>
  <td>uart1</td>
  <td>引脚号29</td>
  <td>引脚号28</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
<tr>
  <td>uart2</td>
  <td>引脚号18</td>
  <td>引脚号17</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
</tbody>
</table>

<table class="EC600G">
<thead>
  <th>uart编号</th>
  <th>TX引脚</th>
  <th>RX引脚</th>
  <th>RTS引脚</th>
  <th>CTS引脚</th>
</tr>
</thead>
<tbody>
<tr>
  <td>uart0</td>
  <td>引脚号124</td>
  <td>引脚号123</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
<tr>
  <td>uart2</td>
  <td>引脚号32</td>
  <td>引脚号31</td>
  <td>引脚34</td>
  <td>引脚33</td>
</tr>
<tr>
  <td>uart4</td>
  <td>引脚号116</td>
  <td>引脚号9</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
<tr>
  <td>uart5</td>
  <td>引脚号125</td>
  <td>引脚号126</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
<tr>
  <td>uart6</td>
  <td>引脚号106</td>
  <td>引脚号105</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
</tbody>
</table>

<table class="EC800G">
<thead>
  <th>uart编号</th>
  <th>TX引脚</th>
  <th>RX引脚</th>
  <th>RTS引脚</th>
  <th>CTS引脚</th>
</tr>
</thead>
<tbody>
<tr>
  <td>uart1</td>
  <td>引脚号29</td>
  <td>引脚号28</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
<tr>
  <td>uart2</td>
  <td>引脚号18</td>
  <td>引脚号17</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
<tr>
  <td>uart5</td>
  <td>引脚号23</td>
  <td>引脚号22</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
<tr>
  <td>uart6</td>
  <td>引脚号86</td>
  <td>引脚号83</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
</tbody>
</table>

<table class="EG912U">
<thead>
  <th>uart编号</th>
  <th>TX引脚</th>
  <th>RX引脚</th>
  <th>RTS引脚</th>
  <th>CTS引脚</th>
</tr>
</thead>
<tbody>
<tr>
  <td>uart1</td>
  <td>引脚号27</td>
  <td>引脚号28</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
<tr>
  <td>uart2</td>
  <td>引脚号35</td>
  <td>引脚号34</td>
  <td>引脚号37</td>
  <td>引脚号36</td>
</tr>
<tr>
  <td>uart4</td>
  <td>引脚号19</td>
  <td>引脚号18</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
</tbody>
</table>

<table class="EC600K">
<thead>
  <th>uart编号</th>
  <th>TX引脚</th>
  <th>RX引脚</th>
  <th>RTS引脚</th>
  <th>CTS引脚</th>
</tr>
</thead>
<tbody>
<tr>
  <td>uart0</td>
  <td>引脚号71</td>
  <td>引脚号72</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
<tr>
  <td>uart1(flowctl = 0)</td>
  <td>引脚号3</td>
  <td>引脚号2</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
<tr>
  <td>uart1(flowctl = 1)</td>
  <td>引脚号33</td>
  <td>引脚号34</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
<tr>
  <td>uart2</td>
  <td>引脚号32</td>
  <td>引脚号31</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
</tbody>
</table>

<table class="EC800K">
<thead>
  <th>uart编号</th>
  <th>TX引脚</th>
  <th>RX引脚</th>
  <th>RTS引脚</th>
  <th>CTS引脚</th>
</tr>
</thead>
<tbody>
<tr>
  <td>uart0</td>
  <td>引脚号39</td>
  <td>引脚号38</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
<tr>
  <td>uart1(group = 0)</td>
  <td>引脚号50</td>
  <td>引脚号51</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
<tr>
  <td>uart1(group = 1)</td>
  <td>引脚号22</td>
  <td>引脚号23</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
<tr>
  <td>uart1(group = 2)</td>
  <td>引脚号29</td>
  <td>引脚号28</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
<tr>
  <td>uart2</td>
  <td>引脚号18</td>
  <td>引脚号17</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
</tbody>
</table>

<table class="EG800K">
<thead>
  <th>uart编号</th>
  <th>TX引脚</th>
  <th>RX引脚</th>
  <th>RTS引脚</th>
  <th>CTS引脚</th>
</tr>
</thead>
<tbody>
<tr>
  <td>uart0</td>
  <td>引脚号39</td>
  <td>引脚号38</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
<tr>
  <td>uart1(group = 0)</td>
  <td>引脚号50</td>
  <td>引脚号51</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
<tr>
  <td>uart1(group = 1)</td>
  <td>引脚号22</td>
  <td>引脚号23</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
<tr>
  <td>uart1(group = 2)</td>
  <td>引脚号29</td>
  <td>引脚号28</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
<tr>
  <td>uart2</td>
  <td>引脚号18</td>
  <td>引脚号17</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
</tbody>
</table>

<table class="EG800AK">
<thead>
  <th>uart编号</th>
  <th>TX引脚</th>
  <th>RX引脚</th>
  <th>RTS引脚</th>
  <th>CTS引脚</th>
</tr>
</thead>
<tbody>
<tr>
  <td>uart0</td>
  <td>引脚号39</td>
  <td>引脚号38</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
<tr>
  <td>uart1(group = 0)</td>
  <td>引脚号50</td>
  <td>引脚号51</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
<tr>
  <td>uart1(group = 1)</td>
  <td>引脚号22</td>
  <td>引脚号23</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
<tr>
  <td>uart1(group = 2)</td>
  <td>引脚号29</td>
  <td>引脚号28</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
<tr>
  <td>uart2</td>
  <td>引脚号18</td>
  <td>引脚号17</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
</tbody>
</table>

<table class="EG800Z">
<thead>
  <th>uart编号</th>
  <th>TX引脚</th>
  <th>RX引脚</th>
  <th>RTS引脚</th>
  <th>CTS引脚</th>
</tr>
</thead>
<tbody>
<tr>
  <td>uart0</td>
  <td>引脚号39</td>
  <td>引脚号38</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
<tr>
  <td>uart1(group = 0)</td>
  <td>引脚号29</td>
  <td>引脚号28</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
<tr>
  <td>uart1(group = 1)</td>
  <td>引脚号63</td>
  <td>引脚号64</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
<tr>
  <td>uart2</td>
  <td>引脚号18</td>
  <td>引脚号17</td>
  <td>引脚号23</td>
  <td>引脚号22</td>
</tr>
</tbody>
</table>

<table class="EG800P">
<thead>
  <th>uart编号</th>
  <th>TX引脚</th>
  <th>RX引脚</th>
  <th>RTS引脚</th>
  <th>CTS引脚</th>
</tr>
</thead>
<tbody>
<tr>
  <td>uart0</td>
  <td>引脚号39</td>
  <td>引脚号38</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
<tr>
  <td>uart1</td>
  <td>引脚号22</td>
  <td>引脚号23</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
<tr>
  <td>uart2</td>
  <td>引脚号18</td>
  <td>引脚号17</td>
  <td>引脚23</td>
  <td>引脚22</td>
</tr>
<tr>
  <td>uart3</td>
  <td>引脚号29</td>
  <td>引脚号28</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
</tbody>
</table>

<table class="FCM360W">
<thead>
  <th>uart编号</th>
  <th>TX引脚</th>
  <th>RX引脚</th>
  <th>RTS引脚</th>
  <th>CTS引脚</th>
</tr>
</thead>
<tbody>
<tr>
  <td>uart0</td>
  <td>引脚号27</td>
  <td>引脚号26</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
<tr>
  <td>uart1</td>
  <td>引脚号20</td>
  <td>引脚号19</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
</tbody>
</table>

<table class="FCM362K">
<thead>
  <th>uart编号</th>
  <th>TX引脚</th>
  <th>RX引脚</th>
  <th>RTS引脚</th>
  <th>CTS引脚</th>
</tr>
</thead>
<tbody>
<tr>
  <td>uart1</td>
  <td>引脚号35</td>
  <td>引脚号34</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
<tr>
  <td>uart2</td>
  <td>引脚号28</td>
  <td>引脚号27</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
</tbody>
</table>

<table class="BC32">
<thead>
  <th>uart编号</th>
  <th>TX引脚</th>
  <th>RX引脚</th>
  <th>RTS引脚</th>
  <th>CTS引脚</th>
</tr>
</thead>
<tbody>
<tr>
  <td>uart0</td>
  <td>引脚号21</td>
  <td>引脚号22</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
</tbody>
</table>

<table class="BC92">
<thead>
  <th>uart编号</th>
  <th>TX引脚</th>
  <th>RX引脚</th>
  <th>RTS引脚</th>
  <th>CTS引脚</th>
</tr>
</thead>
<tbody>
<tr>
  <td>uart0</td>
  <td>引脚号22</td>
  <td>引脚号21</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
</tbody>
</table>

<table class="EG915N">
<thead>
  <th>uart编号</th>
  <th>TX引脚</th>
  <th>RX引脚</th>
  <th>RTS引脚</th>
  <th>CTS引脚</th>
</tr>
</thead>
<tbody>
<tr>
  <td>uart0</td>
  <td>引脚号23</td>
  <td>引脚号22</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
<tr>
  <td>uart1</td>
  <td>引脚号27</td>
  <td>引脚号28</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
<tr>
  <td>uart2</td>
  <td>引脚号35</td>
  <td>引脚号34</td>
  <td>引脚号37</td>
  <td>引脚号36</td>
</tr>
<tr>
  <td>uart4</td>
  <td>引脚号36</td>
  <td>引脚号37</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
</tbody>
</table>

<table class="EC800Z">
<thead>
  <th>uart编号</th>
  <th>TX引脚</th>
  <th>RX引脚</th>
  <th>RTS引脚</th>
  <th>CTS引脚</th>
</tr>
</thead>
<tbody>
<tr>
  <td>uart0</td>
  <td>引脚号39</td>
  <td>引脚号38</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
<tr>
  <td>uart1</td>
  <td>引脚号29</td>
  <td>引脚号28</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
<tr>
  <td>uart2</td>
  <td>引脚号18</td>
  <td>引脚号17</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
</tbody>
</table>

<table class="BG950S">
<thead>
  <th>uart编号</th>
  <th>TX引脚</th>
  <th>RX引脚</th>
  <th>RTS引脚</th>
  <th>CTS引脚</th>
</tr>
</thead>
<tbody>
<tr>
  <td>uart2</td>
  <td>引脚号35</td>
  <td>引脚号34</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
</tbody>
</table>

</div>

<div class="content-QuecPython">
<div class="EC200A">
<blockquote>
<p>注意：</p>
        <p>uart0不建议使用</p>
        <p>EC200ACN_LA,EC200AEU_V1HA,EC200AAU_V1HA,EC200ACN_V1DA 模组uart1引脚号与其他型号不同：</p>
        <p>EC200A含有内置GNSS的已经占用UART1</p>
        <p>TX:引脚号26</p>
        <p>RX:引脚号27</p>
</blockquote>
</div>

<div class="EC200A-V1">
<blockquote>
<p>注意：</p>
        <p>uart0不建议使用
        <p>EC200AXX_V1含有内置GNSS系列串口1不可使用</p>
</blockquote>
</div>

<div class="UC200A">
<blockquote>
<p>注意：</p>
        <p>uart0不建议使用</p>
</blockquote>
</div>

<div class="EC800M">
<blockquote>
<p>注意：</p>
        <p>EC800MCNGA 、CNGB、CNGD模块的uart1 不可用</p>
</blockquote>
</div>

<div class="EG800AK">
<blockquote>
<p>注意：</p>
        <p>EG800AKXX_11LG系列、EG800AKXX_11LN系列 模块的uart1 不可用</p>
</blockquote>
</div>

<div class="EG800Z">
<blockquote>
<p>注意：</p>
        <p>EG800ZCN_LG模块的uart1 不可用</p>
</blockquote>
</div>

<div class="EG810M">
<blockquote>
<p>注意：</p>
        <p>EG810MCNGA、CNGB模块的uart1 不可用</p>
</blockquote>
</div>

<div class="EG800K">
<blockquote>
<p>注意：</p>
        <p>EG800KCN模块的uart1不可用</p>
</blockquote>
</div>

<div class="EG915N">
<blockquote>
<p>注意：</p>
        <p>EG915NEU_AG模块的uart4不可用</p>
</blockquote>
</div>

<div class="EC200U">
<blockquote>
<p>注意：</p>
        <p>EC200UXXAA模组uart4不可用</p>
</blockquote>
</div>

<div class="EC600E">
<blockquote>
<p>注意：</p>
        <p>EC600ECN_LE&LQ模组uart1不可用</p>
</blockquote>
</div>

<div class="EG912U">
<blockquote>
<p>注意：</p>
        <p>EG912UGL_AA模组uart4不可用</p>
</blockquote>
</div>
</div>

<div class="content-QuecPython">
<div class="BG95">
<blockquote>
<p> 使用UART4需要先调用modem.main_uart_enable_set(1)使能UART4，重启生效，如下 </p>
</blockquote>
<ul>
<pre class="language-python"><code class="language-python">import modem
#获取Main_UART使能状态 1-使能，0-不使能
modem.main_uart_enable_get()
#设置Main_UART使能状态 1-使能，0-不使能,重启生效
modem.main_uart_enable_set(1)
</code></pre>
</ul>
</div>

<div class="EG912N|EC600K">
<blockquote>
<p> uart1在flowctl = 1时，仅将uart1映射到不同的引脚，未开启流控功能。 </p>
</blockquote>
</div>


<div class="FCM360W">
<blockquote>
<p> UART2被初始化后交互口将无法使用，需要在代码中执行uart2.close()或者重启模组才可继续使用交互口。 </p>
</blockquote>
</div>

<div class="FCM362K">
<blockquote>
<p> UART1被初始化后交互口将无法使用，需要在代码中执行uart1.close()或者重启模组才可继续使用交互口。 </p>
</blockquote>
</div>
</div>

**示例：**

```python
>>> # 创建uart对象
>>> from machine import UART
>>> uart1 = UART(UART.UART1, 115200, 8, 0, 1, 0)
>>> uartx = UART(UART.UARTx, 115200, 8, 0, 1, 0, group = 1) # 仅当UARTx支持group参数时可用
```

## 方法

### `uart.any`

```python
uart.any()
```

该方法用于获取接收缓存未读数据大小。

**返回值描述：**

返回接收缓存器中有多少字节的数据未读。

**示例：**

```python
>>> uart1.any()
20 #表示接收缓冲区中有20字节数据未读
```

### `uart.read`

```python
uart.read(nbytes)
```

该方法用于从串口读取数据。

**参数描述：**

- `nbytes` - 要读取的字节数，int类型。

**返回值描述：**

返回读取的数据。

### `uart.write`

```python
uart.write(data)
```

该方法用于发送数据到串口。

**参数描述：**

- `data` - 发送的数据，bytes类型。

**返回值描述：**

返回发送的字节数。

### `uart.close`

```python
uart.close()
```

该方法用于关闭串口。

**返回值描述：**

成功返回整型值`0`，失败返回整型值`-1`。

### `uart.control_485`

<div class="content-QuecPython">
<div class="EC600U|EC200U|EC200A|EC200A-V1|UC200A|EC800N|EC600M|EG810M|EC800M|EG912N|EC600E|EC800E|EC600G|EC800G|EC600K|EC800K|EG800K|EG800P|FCM362K|BC32|BC92|EG915N|EC800Z|EG912U|EG800AK|EG800Z">

<pre class="language-python"><code class="language-python">uart.control_485(UART.GPIOn, direction)
</code></pre>
<p>该方法用于控制485通信方向，串口发送数据之前和之后进行拉高拉低指定GPIO，用来指示485通信的方向。</p>
<p><strong>参数描述：</strong></p>
<ul>
<li><p><code>GPIOn</code> - 需要控制的GPIO引脚号，参照<a href="machine.Pin.html">Pin模块</a>的引脚定义，int类型。<br/></p>
</li>
<li><p><code>direction</code> - 引脚电平变化，int类型，说明如下：<br/><code>1</code>表示引脚电平变化为：串口发送数据之前由低拉高、发送数据之后再由高拉低<br/><code>0</code>表示引脚电平变化为：串口发送数据之前由高拉低、发送数据之后再由低拉高<br/></p>
</li>
</ul>
<p><strong>返回值描述：</strong></p>
<p>成功返回整型值<code>0</code>，失败返回整型值<code>-1</code>。</p>
<p><strong>示例：</strong></p>
<pre class="language-python"><code class="language-python">from machine import UART
uart1 = UART(UART.UART1, 115200, 8, 0, 1, 0)
uart1.control_485(UART.GPIO24, 1)
</code></pre>

</div>

<div class="BC25PA|BG95|FCM360W">
<p><strong>该型号不支持此API</strong></p>
</div>
</div>

### `uart.set_callback`

```python
uart.set_callback(fun)
```

该方法用于设置串口数据回调，串口收到数据后，会执行该回调。

**参数描述：**

- `fun` - 串口回调函数，回调函数原型：

  ```
  fun(result_list)
  ```

  回调函数参数描述：

  - `result_list[0]`：接收是否成功(0：成功，其它：失败)

  - `result_list[1]`：接收端口

  - `result_list[2]`：返回有多少数据

**返回值描述：**

成功返回整型值`0`，失败返回整型值`-1`。

**示例：**

```python
>>> from machine import UART
>>> uart1 = UART(UART.UART1, 115200, 8, 0, 1, 0)
>>> 
>>> def uart_call(para):
>>>		print(para)
>>> uart1.set_callback(uart_call)
```

**使用示例：**

```python
"""
运行本例程，需要通过串口线连接开发板的 MAIN 口和PC，在PC上通过串口工具
打开 MAIN 口，并向该端口发送数据，即可看到 PC 发送过来的消息。
"""
import _thread
import utime
import log
from machine import UART

'''
 * 参数1：端口
        注：EC100YCN平台与EC600SCN平台，UARTn作用如下
        UART0 - DEBUG PORT
        UART1 – BT PORT
        UART2 – MAIN PORT
        UART3 – USB CDC PORT
 * 参数2：波特率
 * 参数3：data bits  （5~8）
 * 参数4：Parity  （0：NONE  1：EVEN  2：ODD）
 * 参数5：stop bits （1~2）
 * 参数6：flow control （0: FC_NONE  1：FC_HW）
'''


# 设置日志输出级别
log.basicConfig(level=log.INFO)
uart_log = log.getLogger("UART")

class Example_uart(object):
    def __init__(self, no=UART.UART2, bate=115200, data_bits=8, parity=0, stop_bits=1, flow_control=0):
        self.uart = UART(no, bate, data_bits, parity, stop_bits, flow_control)
        self.uart.set_callback(self.callback)


    def callback(self, para):
        uart_log.info("call para:{}".format(para))
        if(0 == para[0]):
            self.uartRead(para[2])

    
    def uartWrite(self, msg):
        uart_log.info("write msg:{}".format(msg))
        self.uart.write(msg)

    def uartRead(self, len):
        msg = self.uart.read(len)
        utf8_msg = msg.decode()
        uart_log.info("UartRead msg: {}".format(utf8_msg))
        return utf8_msg

    def uartWrite_test(self):
        for i in range(10):
            write_msg = "Hello count={}".format(i)
            self.uartWrite(write_msg)
            utime.sleep(1)

if __name__ == "__main__":
    uart_test = Example_uart()
    uart_test.uartWrite_test()
    

# 运行结果示例
'''
INFO:UART:write msg:Hello count=0
INFO:UART:write msg:Hello count=1
INFO:UART:write msg:Hello count=2
INFO:UART:write msg:Hello count=3
INFO:UART:write msg:Hello count=4
INFO:UART:write msg:Hello count=5
INFO:UART:write msg:Hello count=6
INFO:UART:write msg:Hello count=7
INFO:UART:write msg:Hello count=8
INFO:UART:write msg:Hello count=9

INFO:UART:call para:[0, 2, 15]
INFO:UART:UartRead msg: my name is XXX


'''

```

## 常量

| 常量       | 说明  |
| ---------- | ----- |
| UART.UART0 | UART0 |
| UART.UART1 | UART1 |
| UART.UART2 | UART2 |
| UART.UART3 | UART3 |
| UART.UART4 | UART4 |

