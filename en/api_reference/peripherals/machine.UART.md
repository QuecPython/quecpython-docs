# UART - Duplex Serial Communication Bus

This class transmits data through the UART.

## Constructor

### `machine.UART`

<div class="content-QuecPython">
<div class="EC600U|EC200U|EG915U|EG915N">
<pre class="language-python"><code class="language-python">class machine.UART(UART.UARTn, baudrate, databits, parity, stopbits, flowctl)
</code></pre>
<p><strong>Parametric Description:</strong></p>
<ul>
<li><p><code>UARTn</code> - UART number, int type, UARTn description is as follows:<br/><code>UART0</code> - DEBUG PORT <br/><code>UART1</code> - BT PORT <br/><code>UART2</code> - MAIN PORT <br/><code>UART3</code> - USB CDC PORT <br/><code>UART4</code> - STDOUT PORT <br/></p>
</li>
<li><p><code>baudratec</code> - Baud rate, int type, supports common baud rates such as<code>4800</code>、<code>9600</code>、<code>19200</code>、<code>38400</code>、<code>57600</code>、<code>115200</code>、<code>230400</code>.<br/></p></li>
<li><p><code>databits</code> - The data bit only supports 8 bits, int type.<br/></p></li>
<li><p><code>parity</code> - Parity (0-None, 1-even, 2-odd), int type.<br/></p></li>
<li><p><code>stopbits</code> - Stop bit [1 ~ 2], int type.<br/></p></li>
<li><p><code>flowctl</code> - Hardware control flow (0-FC_NONE, 1-FC_HW), int type.<br/></p>
</li>
</ul>
</div>

<div class="EC200A|EC200A-V1|UC200A|EC800N|BC25PA|EG912N|EC600E|EC800E|EC600G|EC800G|EC600K|EG800P|BC92|BC32|FCM360W|FCM362K|EG912U">
<pre class="language-python"><code class="language-python">class machine.UART(UART.UARTn, baudrate, databits, parity, stopbits, flowctl)
</code></pre>
<p><strong>Parametric Description:</strong></p>
<ul>
<li><p><code>UARTn</code> - UART number, int type, UARTn description is as follows:<br/><code>UART0</code> - DEBUG PORT <br/><code>UART1</code> - BT PORT <br/><code>UART2</code> - MAIN PORT <br/><code>UART3</code> - USB CDC PORT <br/></p>
</li>
<li><p><code>baudratec</code> - Baud rate, int type, supports common baud rates such as<code>4800</code>、<code>9600</code>、<code>19200</code>、<code>38400</code>、<code>57600</code>、<code>115200</code>、<code>230400</code>.<br/></p></li>
<li><p><code>databits</code> - Data bits [5-8], int type.<br/></p></li>
<li><p><code>parity</code> - Parity (0-None, 1-even, 2-odd), int type.<br/></p></li>
<li><p><code>stopbits</code> - Stop bit [1 ~ 2], int type.<br/></p></li>
<li><p><code>flowctl</code> - Hardware control flow (0-FC_NONE, 1-FC_HW), int type.<br/></p>
</li>
</ul>
</div>

<div class="EC800Z|EG800Z">
<pre class="language-python"><code class="language-python">class machine.UART(UART.UARTn, baudrate, databits, parity, stopbits, flowctl, [group])
</code></pre>
<p><strong>Parametric Description:</strong></p>
<ul>
<li><p><code>UARTn</code> - UART number, int type, UARTn description is as follows:<br/><code>UART0</code> - DEBUG PORT <br/><code>UART1</code> - BT PORT <br/><code>UART2</code> - MAIN PORT <br/><code>UART3</code> - USB CDC PORT <br/></p>
</li>
<li><p><code>baudratec</code> - Baud rate, int type, supports common baud rates such as<code>4800</code>、<code>9600</code>、<code>19200</code>、<code>38400</code>、<code>57600</code>、<code>115200</code>、<code>230400</code>.<br/></p></li>
<li><p><code>databits</code> - Data bits [8], int type.<br/></p></li>
<li><p><code>parity</code> - Parity (0-None, 1-even, 2-odd), int type.<br/></p></li>
<li><p><code>stopbits</code> - Stop bit [1 ~ 2], int type.<br/></p></li>
<li><p><code>flowctl</code> - Hardware control flow (0-FC_NONE, 1-FC_HW), int type.<br/></p>
</li>
<li><p><code>[group]</code> - The UART is selected to be used on different pins, with the default value being 0, int type.<br/></p>
</li>
</ul>
</div>

<div class="EG800K|EC800K|EG800AK|EC600M|EC800M|EG810M">
<pre class="language-python"><code class="language-python">class machine.UART(UART.UARTn, baudrate, databits, parity, stopbits, flowctl, [group])
</code></pre>
<p><strong>Parametric Description:</strong></p>
<ul>
<li><p><code>UARTn</code> - UART number, int type, UARTn description is as follows:<br/><code>UART0</code> - DEBUG PORT <br/><code>UART1</code> - BT PORT <br/><code>UART2</code> - MAIN PORT <br/><code>UART3</code> - USB CDC PORT <br/></p>
</li>
<li><p><code>baudratec</code> - Baud rate, int type, supports common baud rates such as<code>4800</code>、<code>9600</code>、<code>19200</code>、<code>38400</code>、<code>57600</code>、<code>115200</code>、<code>230400</code>.<br/></p></li>
<li><p><code>databits</code> - Data bits [5-8], int type.<br/></p></li>
<li><p><code>parity</code> - Parity (0-None, 1-even, 2-odd), int type.<br/></p></li>
<li><p><code>stopbits</code> - Stop bit [1 ~ 2], int type.<br/></p></li>
<li><p><code>flowctl</code> - Hardware control flow (0-FC_NONE, 1-FC_HW), int type.<br/></p>
<li><p><code>[group]</code> - The UART is selected to be used on different pins, with the default value being 0, int type.<br/></p>
</li>
</ul>
</div>

<div class="BG95">
<pre class="language-python"><code class="language-python">class machine.UART(UART.UARTn, baudrate, databits, parity, stopbits, flowctl)
</code></pre>
<p><strong>Parametric Description:</strong></p>
<ul>
<li><p><code>UARTn</code> - UART number, int type, UARTn description is as follows:<br/><code>UART0</code> - DEBUG PORT <br/><code>UART1</code> - BT PORT <br/><code>UART2</code> - MAIN PORT <br/><code>UART3</code> - USB CDC PORT (BG95M3 is not supported.) <br/></p>
</li>
<li><p><code>baudratec</code> - Baud rate, int type, supports common baud rates such as<code>4800</code>、<code>9600</code>、<code>19200</code>、<code>38400</code>、<code>57600</code>、<code>115200</code>、<code>230400</code>.<br/></p></li>
<li><p><code>databits</code> - Data bits [5-8], int type.<br/></p></li>
<li><p><code>parity</code> - Parity (0-None, 1-even, 2-odd), int type.<br/></p></li>
<li><p><code>stopbits</code> - Stop bit [1 ~ 2], int type.<br/></p></li>
<li><p><code>flowctl</code> - Hardware control flow (0-FC_NONE, 1-FC_HW), int type.<br/></p>
</li>
</ul>
</div>

<div class="BG950S">
<pre class="language-python"><code class="language-python">class machine.UART(UART.UARTn, baudrate, databits, parity, stopbits, flowctl)
</code></pre>
<p><strong>Parametric Description:</strong></p>
<ul>
<li><p><code>UARTn</code> - UART number, int type, UARTn description is as follows:<br/><code>UART0</code> - DEBUG PORT <br/><code>UART1</code> - BT PORT <br/><code>UART2</code> - MAIN PORT <br/><code>UART3</code> - USB CDC PORT <br/></p>
</li>
<li><p><code>baudratec</code> - Baud rate, int type, supports common baud rates such as<code>4800</code>、<code>9600</code>、<code>19200</code>、<code>38400</code>、<code>57600</code>、<code>115200</code>、<code>230400</code>.<br/></p></li>
<li><p><code>databits</code> - Data bits [5-8], int type.<br/></p></li>
<li><p><code>parity</code> - Parity (0-None, 1-even, 2-odd), int type.<br/></p></li>
<li><p><code>stopbits</code> - Stop bit [1 ~ 2], int type.<br/></p></li>
<li><p><code>flowctl</code> - Hardware control flow (0-FC_NONE, 1-FC_HW), int type.<br/></p>
</li>
</ul>
</div>

</div>

**UART Pin Correspondences:**

<div class="table-QuecPython">
<table class="EC600U">
<thead>
  <th>UART number</th>
  <th>TX pin</th>
  <th>RX pin</th>
  <th>RTS pin</th>
  <th>CTS pin</th>
</tr>
</thead>
<tbody>
<tr>
  <td>UART1</td>
  <td>pin124</td>
  <td>pin123</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
<tr>
  <td>UART2</td>
  <td>pin32</td>
  <td>pin31</td>
  <td>pin34</td>
  <td>pin33</td>
</tr>
<tr>
  <td>UART4</td>
  <td>pin103</td>
  <td>pin104</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
</tbody>
</table>

<table class="EC200U">
<thead>
  <th>UART number</th>
  <th>TX pin</th>
  <th>RX pin</th>
  <th>RTS pin</th>
  <th>CTS pin</th>
</tr>
</thead>
<tbody>
<tr>
  <td>UART1</td>
  <td>pin138</td>
  <td>pin137</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
<tr>
  <td>UART2</td>
  <td>pin67</td>
  <td>pin68</td>
  <td>pin65</td>
  <td>pin64</td>
</tr>
<tr>
  <td>UART4</td>
  <td>pin82</td>
  <td>pin81</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
</tbody>
</table>

<table class="EC200A">
<thead>
  <th>UART number</th>
  <th>TX pin</th>
  <th>RX pin</th>
  <th>RTS pin</th>
  <th>CTS pin</th>
</tr>
</thead>
<tbody>
<tr>
  <td>UART0</td>
  <td>pin12</td>
  <td>pin11</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
<tr>
  <td>UART1</td>
  <td>pin63</td>
  <td>pin66</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
<tr>
  <td>UART2</td>
  <td>pin67</td>
  <td>pin68</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
</tbody>
</table>

<table class="EC200A-V1">
<thead>
  <th>UART number</th>
  <th>TX pin</th>
  <th>RX pin</th>
  <th>RTS pin</th>
  <th>CTS pin</th>
</tr>
</thead>
<tbody>
<tr>
  <td>UART0</td>
  <td>pin12</td>
  <td>pin11</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
<tr>
  <td>UART1</td>
  <td>pin26</td>
  <td>pin27</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
<tr>
  <td>UART2</td>
  <td>pin67</td>
  <td>pin68</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
</tbody>
</table>

<table class="UC200A">
<thead>
  <th>UART number</th>
  <th>TX pin</th>
  <th>RX pin</th>
  <th>RTS pin</th>
  <th>CTS pin</th>
</tr>
</thead>
<tbody>
<tr>
  <td>UART0</td>
  <td>pin12</td>
  <td>pin11</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
<tr>
  <td>UART1</td>
  <td>pin63</td>
  <td>pin66</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
<tr>
  <td>UART2</td>
  <td>pin67</td>
  <td>pin68</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
</tbody>
</table>

<table class="EC800N">
<thead>
  <th>UART number</th>
  <th>TX pin</th>
  <th>RX pin</th>
  <th>RTS pin</th>
  <th>CTS pin</th>
</tr>
</thead>
<tbody>
<tr>
  <td>UART0</td>
  <td>pin39</td>
  <td>pin38</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
<tr>
  <td>UART1</td>
  <td>pin50</td>
  <td>pin51</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
<tr>
  <td>UART2</td>
  <td>pin18</td>
  <td>pin17</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
</tbody>
</table>

<table class="BC25PA">
<thead>
  <th>UART number</th>
  <th>TX pin</th>
  <th>RX pin</th>
  <th>RTS pin</th>
  <th>CTS pin</th>
</tr>
</thead>
<tbody>
<tr>
  <td>UART1</td>
  <td>pin29</td>
  <td>pin28</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
</tbody>
</table>

<table class="BG95">
<thead>
  <th>UART number</th>
  <th>TX pin</th>
  <th>RX pin</th>
  <th>RTS pin</th>
  <th>CTS pin</th>
</tr>
</thead>
<tbody>
<tr>
  <td>UART0</td>
  <td>pin23</td>
  <td>pin22</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
<tr>
  <td>UART1</td>
  <td>pin27</td>
  <td>pin28</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
<tr>
  <td>UART2</td>
  <td>pin64</td>
  <td>pin65</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
<tr>
  <td>UART4</td>
  <td>pin35</td>
  <td>pin34</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
</tbody>
</table>

<table class="EC600M">
<thead>
  <th>UART number</th>
  <th>TX pin</th>
  <th>RX pin</th>
  <th>RTS pin</th>
  <th>CTS pin</th>
</tr>
</thead>
<tbody>
<tr>
  <td>UART0</td>
  <td>pin71</td>
  <td>pin72</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
<tr>
  <td>UART1(group = 0)</td>
  <td>pin3</td>
  <td>pin2</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
<tr>
  <td>UART1(group = 1)</td>
  <td>pin33</td>
  <td>pin34</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
<tr>
  <td>UART2</td>
  <td>pin32</td>
  <td>pin31</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
</tbody>
</table>

<table class="EG915U">
<thead>
  <th>UART number</th>
  <th>TX pin</th>
  <th>RX pin</th>
  <th>RTS pin</th>
  <th>CTS pin</th>
</tr>
</thead>
<tbody>
<tr>
  <td>UART1</td>
  <td>pin27</td>
  <td>pin28</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
<tr>
  <td>UART2</td>
  <td>pin35</td>
  <td>pin34</td>
  <td>pin37</td>
  <td>pin36</td>
</tr>
<tr>
  <td>UART4</td>
  <td>pin19</td>
  <td>pin18</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
</tbody>
</table>

<table class="EC800M">
<thead>
  <th>UART number</th>
  <th>TX pin</th>
  <th>RX pin</th>
  <th>RTS pin</th>
  <th>CTS pin</th>
</tr>
</thead>
<tbody>
<tr>
  <td>UART0</td>
  <td>pin39</td>
  <td>pin38</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
<tr>
  <td>UART1(group = 0)</td>
  <td>pin50</td>
  <td>pin51</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
<tr>
  <td>UART1(group = 1)</td>
  <td>pin22</td>
  <td>pin23</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
<tr>
  <td>UART2</td>
  <td>pin18</td>
  <td>pin17</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
<tr>
  <td>UART4</td>
  <td>pin29</td>
  <td>pin28</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
</tbody>
</table>

<table class="EG810M">
<thead>
  <th>UART number</th>
  <th>TX pin</th>
  <th>RX pin</th>
  <th>RTS pin</th>
  <th>CTS pin</th>
</tr>
</thead>
<tbody>
<tr>
  <td>UART0</td>
  <td>pin39</td>
  <td>pin38</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
<tr>
  <td>UART1(group = 0)</td>
  <td>pin50</td>
  <td>pin51</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
<tr>
  <td>UART1(group = 1)</td>
  <td>pin22</td>
  <td>pin23</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
<tr>
  <td>UART2</td>
  <td>pin18</td>
  <td>pin17</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
<tr>
  <td>UART4</td>
  <td>pin29</td>
  <td>pin28</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
</tbody>
</table>

<table class="EG912N">
<thead>
  <th>UART number</th>
  <th>TX pin</th>
  <th>RX pin</th>
  <th>RTS pin</th>
  <th>CTS pin</th>
</tr>
</thead>
<tbody>
<tr>
  <td>UART0</td>
  <td>pin23</td>
  <td>pin22</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
<tr>
  <td>UART1(flowctl = 0)</td>
  <td>pin27</td>
  <td>pin28</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
<tr>
  <td>UART1(flowctl = 1)</td>
  <td>pin36</td>
  <td>pin37</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
<tr>
  <td>UART2</td>
  <td>pin35</td>
  <td>pin34</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
</tbody>
</table>

<table class="EC600E">
<thead>
  <th>UART number</th>
  <th>TX pin</th>
  <th>RX pin</th>
  <th>RTS pin</th>
  <th>CTS pin</th>
</tr>
</thead>
<tbody>
<tr>
  <td>UART0</td>
  <td>pin71</td>
  <td>pin72</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
<tr>
  <td>UART1</td>
  <td>pin70</td>
  <td>pin69</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
<tr>
  <td>UART2</td>
  <td>pin32</td>
  <td>pin31</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
</tbody>
</table>

<table class="EC800E">
<thead>
  <th>UART number</th>
  <th>TX pin</th>
  <th>RX pin</th>
  <th>RTS pin</th>
  <th>CTS pin</th>
</tr>
</thead>
<tbody>
<tr>
  <td>UART0</td>
  <td>pin39</td>
  <td>pin38</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
<tr>
  <td>UART1</td>
  <td>pin29</td>
  <td>pin28</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
<tr>
  <td>UART2</td>
  <td>pin18</td>
  <td>pin17</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
</tbody>
</table>

<table class="EC600G">
<thead>
  <th>UART number</th>
  <th>TX pin</th>
  <th>RX pin</th>
  <th>RTS pin</th>
  <th>CTS pin</th>
</tr>
</thead>
<tbody>
<tr>
  <td>UART0</td>
  <td>pin124</td>
  <td>pin123</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
<tr>
  <td>UART2</td>
  <td>pin32</td>
  <td>pin31</td>
  <td>pin34</td>
  <td>pin33</td>
</tr>
<tr>
  <td>UART4</td>
  <td>pin116</td>
  <td>pin9</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
<tr>
  <td>UART5</td>
  <td>pin125</td>
  <td>pin126</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
<tr>
  <td>UART6</td>
  <td>pin106</td>
  <td>pin105</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
</tbody>
</table>

<table class="EC800G">
<thead>
  <th>UART number</th>
  <th>TX pin</th>
  <th>RX pin</th>
  <th>RTS pin</th>
  <th>CTS pin</th>
</tr>
</thead>
<tbody>
<tr>
  <td>UART1</td>
  <td>pin29</td>
  <td>pin28</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
<tr>
  <td>UART2</td>
  <td>pin18</td>
  <td>pin17</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
<tr>
  <td>UART5</td>
  <td>pin23</td>
  <td>pin22</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
<tr>
  <td>UART6</td>
  <td>pin86</td>
  <td>pin83</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
</tbody>
</table>

<table class="EG912U">
<thead>
  <th>UART number</th>
  <th>TX pin</th>
  <th>RX pin</th>
  <th>RTS pin</th>
  <th>CTS pin</th>
</tr>
</thead>
<tbody>
<tr>
  <td>UART1</td>
  <td>pin27</td>
  <td>pin28</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
<tr>
  <td>UART2</td>
  <td>pin35</td>
  <td>pin34</td>
  <td>pin37</td>
  <td>pin36</td>
</tr>
<tr>
  <td>UART4</td>
  <td>pin19</td>
  <td>pin18</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
</tbody>
</table>

<table class="EC600K">
<thead>
  <th>UART number</th>
  <th>TX pin</th>
  <th>RX pin</th>
  <th>RTS pin</th>
  <th>CTS pin</th>
</tr>
</thead>
<tbody>
<tr>
  <td>UART0</td>
  <td>pin71</td>
  <td>pin72</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
<tr>
  <td>UART1(flowctl = 0)</td>
  <td>pin3</td>
  <td>pin2</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
<tr>
  <td>UART1(flowctl = 1)</td>
  <td>pin33</td>
  <td>pin34</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
<tr>
  <td>UART2</td>
  <td>pin32</td>
  <td>pin31</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
</tbody>
</table>

<table class="EC800K">
<thead>
  <th>UART number</th>
  <th>TX pin</th>
  <th>RX pin</th>
  <th>RTS pin</th>
  <th>CTS pin</th>
</tr>
</thead>
<tbody>
<tr>
  <td>UART0</td>
  <td>pin39</td>
  <td>pin38</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
<tr>
  <td>UART1(group = 0)</td>
  <td>pin50</td>
  <td>pin51</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
<tr>
  <td>UART1(group = 1)</td>
  <td>pin22</td>
  <td>pin23</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
<tr>
  <td>uart1(group = 2)</td>
  <td>pin29</td>
  <td>pin28</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
<tr>
  <td>UART2</td>
  <td>pin18</td>
  <td>pin17</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
</tbody>
</table>

<table class="EG800K">
<thead>
  <th>UART number</th>
  <th>TX pin</th>
  <th>RX pin</th>
  <th>RTS pin</th>
  <th>CTS pin</th>
</tr>
</thead>
<tbody>
<tr>
  <td>UART0</td>
  <td>pin39</td>
  <td>pin38</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
<tr>
  <td>UART1(group = 0)</td>
  <td>pin50</td>
  <td>pin51</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
<tr>
  <td>UART1(group = 1)</td>
  <td>pin22</td>
  <td>pin23</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
<tr>
  <td>uart1(group = 2)</td>
  <td>pin29</td>
  <td>pin28</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
<tr>
  <td>UART2</td>
  <td>pin18</td>
  <td>pin17</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
</tbody>
</table>

<table class="EG800AK">
<thead>
  <th>UART number</th>
  <th>TX pin</th>
  <th>RX pin</th>
  <th>RTS pin</th>
  <th>CTS pin</th>
</tr>
</thead>
<tbody>
<tr>
  <td>UART0</td>
  <td>pin39</td>
  <td>pin38</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
<tr>
  <td>UART1(group = 0)</td>
  <td>pin50</td>
  <td>pin51</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
<tr>
  <td>UART1(group = 1)</td>
  <td>pin22</td>
  <td>pin23</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
<tr>
  <td>uart1(group = 2)</td>
  <td>pin29</td>
  <td>pin28</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
<tr>
  <td>UART2</td>
  <td>pin18</td>
  <td>pin17</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
</tbody>
</table>

<table class="EG800Z">
<thead>
  <th>UART number</th>
  <th>TX pin</th>
  <th>RX pin</th>
  <th>RTS pin</th>
  <th>CTS pin</th>
</tr>
</thead>
<tbody>
<tr>
  <td>uart0</td>
  <td>pin39</td>
  <td>pin38</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
<tr>
  <td>uart1(group = 0)</td>
  <td>pin29</td>
  <td>pin28</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
<tr>
  <td>uart1(group = 1)</td>
  <td>pin63</td>
  <td>pin64</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
<tr>
  <td>uart2</td>
  <td>pin18</td>
  <td>pin17</td>
  <td>pin23</td>
  <td>pin22</td>
</tr>
</tbody>
</table>

<table class="EG800P">
<thead>
  <th>UART number</th>
  <th>TX pin</th>
  <th>RX pin</th>
  <th>RTS pin</th>
  <th>CTS pin</th>
</tr>
</thead>
<tbody>
<tr>
  <td>UART0</td>
  <td>pin39</td>
  <td>pin38</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
<tr>
  <td>UART1</td>
  <td>pin22</td>
  <td>pin23</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
<tr>
  <td>UART2</td>
  <td>pin18</td>
  <td>pin17</td>
  <td>pin23</td>
  <td>pin22</td>
</tr>
<tr>
  <td>UART3</td>
  <td>pin29</td>
  <td>pin28</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
</tbody>
</table>

<table class="FCM360W">
<thead>
  <th>UART number</th>
  <th>TX pin</th>
  <th>RX pin</th>
  <th>RTS pin</th>
  <th>CTS pin</th>
</tr>
</thead>
<tbody>
<tr>
  <td>UART0</td>
  <td>pin27</td>
  <td>pin26</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
<tr>
  <td>UART1</td>
  <td>pin20</td>
  <td>pin19</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
</tbody>
</table>

<table class="FCM362K">
<thead>
  <th>UART number</th>
  <th>TX pin</th>
  <th>RX pin</th>
  <th>RTS pin</th>
  <th>CTS pin</th>
</tr>
</thead>
<tbody>
<tr>
  <td>UART1</td>
  <td>pin35</td>
  <td>pin34</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
<tr>
  <td>UART2</td>
  <td>pin28</td>
  <td>pin27</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
</tbody>
</table>

<table class="BC32">
<thead>
  <th>UART number</th>
  <th>TX pin</th>
  <th>RX pin</th>
  <th>RTS pin</th>
  <th>CTS pin</th>
</tr>
</thead>
<tbody>
<tr>
  <td>UART0</td>
  <td>pin21</td>
  <td>pin22</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
</tbody>
</table>

<table class="BC92">
<thead>
  <th>UART number</th>
  <th>TX pin</th>
  <th>RX pin</th>
  <th>RTS pin</th>
  <th>CTS pin</th>
</tr>
</thead>
<tbody>
<tr>
  <td>UART0</td>
  <td>pin22</td>
  <td>pin21</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
</tbody>
</table>

<table class="EG915N">
<thead>
  <th>UART number</th>
  <th>TX pin</th>
  <th>RX pin</th>
  <th>RTS pin</th>
  <th>CTS pin</th>
</tr>
</thead>
<tbody>
<tr>
  <td>UART0</td>
  <td>pin23</td>
  <td>pin22</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
<tr>
  <td>UART1</td>
  <td>pin27</td>
  <td>pin28</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
<tr>
  <td>UART2</td>
  <td>pin35</td>
  <td>pin34</td>
  <td>pin37</td>
  <td>pin38</td>
</tr>
<tr>
  <td>UART4</td>
  <td>pin36</td>
  <td>pin37</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
</tbody>
</table>

<table class="EC800Z">
<thead>
  <th>UART number</th>
  <th>TX pin</th>
  <th>RX pin</th>
  <th>RTS pin</th>
  <th>CTS pin</th>
</tr>
</thead>
<tbody>
<tr>
  <td>UART0</td>
  <td>pin39</td>
  <td>pin38</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
<tr>
  <td>UART1</td>
  <td>pin29</td>
  <td>pin28</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
<tr>
  <td>UART2</td>
  <td>pin18</td>
  <td>pin17</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
</tbody>
</table>

<table class="BG950S">
<thead>
  <th>UART number</th>
  <th>TX pin</th>
  <th>RX pin</th>
  <th>RTS pin</th>
  <th>CTS pin</th>
</tr>
</thead>
<tbody>
<tr>
  <td>UART2</td>
  <td>pin35</td>
  <td>pin34</td>
  <td>NULL</td>
  <td>NULL</td>
</tr>
</tbody>
</table>

</div>

<div class="content-QuecPython">
<div class="EC200A">
<blockquote>
<p>Attention：</p>
        <p>Uart0 is not recommended</p>
        <p>EC200ACN_LA,EC200AEU_V1HA,EC200AAU_V1HA,EC200ACN_V1DA module uart1 pin number is different from other models:</p>
        <p>EC200A already occupies UART1 and has an internal GNSS integrated.</p>
        <p>TX: Pin26</p>
        <p>RX: Pin27</p>
</blockquote>
</div>

<div class="EC200A-V1">
<blockquote>
<p>Attention：</p>
        <p>Uart0 is not recommended
        <p>EC200AXX_V1 comes with a built-in GNSS series serial port 1 that is not functional.</p>
</blockquote>
</div>

<div class="UC200A">
<blockquote>
<p>Attention：</p>
        <p>Uart0 is not recommended</p>
</blockquote>
</div>

<div class="EC800M">
<blockquote>
<p>Attention：</p>
        <p>Uart1 for EC800MCNGA, CNGB, and CNGD modules is not available</p>
</blockquote>
</div>

<div class="EG800AK">
<blockquote>
<p>注意：</p>
        <p>Uart1 for EG800AKXX_11LG series、EG800AKXX_11LN series modules is not available</p>
</blockquote>
</div>

<div class="EG800Z">
<blockquote>
<p>注意：</p>
        <p>Uart1 for EG800ZCN_LG modules is not available</p>
</blockquote>
</div>

<div class="EG810M">
<blockquote>
<p>Attention：</p>
        <p>Uart1 for EG810MCNGA, CNGB modules is not available</p>
</blockquote>
</div>

<div class="EG800K">
<blockquote>
<p>Attention：</p>
        <p>The uart1 of the EG800KCN module is not available</p>
</blockquote>
</div>

<div class="EG915N">
<blockquote>
<p>Attention：</p>
        <p>The uart4 of the EG915NEU_AG module is not available</p>
</blockquote>
</div>

<div class="EC200U">
<blockquote>
<p>Attention：</p>
        <p>EC200UXXAA module uart4 is not available</p>
</blockquote>
</div>

<div class="EC600E">
<blockquote>
<p>Attention：</p>
        <p>EC600ECN_LE&LQ module uart1 is not available</p>
</blockquote>
</div>

<div class="EG912U">
<blockquote>
<p>Attention：</p>
        <p>EG912UGL_AA module uart4 is not available</p>
</blockquote>
</div>

</div>

<div class="content-QuecPython">
<div class="BG95">
<blockquote>
<p> To use UART4, you need to call modem.main_uart_enable_set(1) to enable UART4, and restart it as follows </p>
</blockquote>
<ul>
<pre class="language-python"><code class="language-python">import modem
# Get Main UART enabled status 1-enabled, 0-not enabled
modem.main_uart_enable_get()
# Set Main_UART enable state 1- enabled, 0- Not enabled, restart to take effect
modem.main_uart_enable_set(1)
</code></pre>
</ul>
</div>

<div class="EG912N|EC600K">
<blockquote>
<p> When flowctl = 1, uart1 only maps uart1 to different pins and does not turn on the flow control function. </p>
</blockquote>
</div>


<div class="FCM360W">
<blockquote>
<p> UART2 is initialized after the interaction port will not be used, you need to execute uart2.close() in the code or restart the module to continue to use the interaction port. </p>
</blockquote>
</div>

<div class="FCM362K">
<blockquote>
<p>  UART1 is initialized after the interaction port will not be used, you need to execute uart1.close() in the code or restart the module to continue to use the interaction port. </p>
</blockquote>
</div>
</div>

**Example:**

```python
>>> # Creates a UART object
>>> from machine import UART
>>> uart1 = UART(UART.UART1, 115200, 8, 0, 1, 0)
>>> uartx = UART(UART.UARTx, 115200, 8, 0, 1, 0, group = 1) # Available only when UARTx supports the group parameter
```

## Methods

### `uart.any`

```python
uart.any()
```

This method gets the size of the unread data in the receiving cache.

**Return Value:**

Size of data that is unread in the receiving cache.

**Example:**

```python
>>> uart.any()
20 # It indicates that there is 20 bytes of unread data in the receiving cache.
```

### `uart.read`

```python
uart.read(nbytes)
```

This method reads data from the UART.

**Parameter:**

- `nbytes` - Integer type. Size of data to be read.  

**Return Value:**

Size of data that has been read.

### `uart.write`

```python
uart.write(data)
```

This method sends data to the UART.

**Parameter:**

- `data` - Bytes type. Data to be sent.  

**Return Value:**

Size of data that has been sent. 

### `uart.close`

```python
uart.close()
```

This method disables the UART.

**Return Value:**

`0` - Successful execution

`-1` - Failed execution

### `uart.control_485`

<div class="content-QuecPython">
<div class="EC600U|EC200U|EC200A|EC200A-V1|UC200A|EC800N|EC600M|EC800M|EG810M|EG912N|EC600E|EC800E|EC600G|EC800G|EC600K|EC800K|EG800K|EG800P|FCM362K|BC32|BC92|EG915N|EC800Z|EG912U|EG800Z">
<pre class="language-python"><code class="language-python">uart.control_485(UART.GPIOn, direction)
</code></pre>
<p>This method is used to control the 485 communication direction, and the specified GPIO is pulled up and down before and after the serial port sends data to indicate the 485 communication direction.</p>
<p><strong>Parametric Description:</strong></p>
<ul>
<li><p><code>GPIOn</code> - GPIO Pin number to control, refer to pin definition in <a href="machine.Pin.html">Pin module</a>, int type.<br/></p>
</li>
<li><p><code>direction</code> - Pin level change, int type, described as follows:<br/><code>1</code>It indicates that the pin level change is: the serial port is pulled up by low before sending data, and then pulled down by high after sending data<br/><code>0</code>It indicates that the pin level change is: the serial port is pulled down by high before sending data, and then pulled up by low after sending data<br/></p>
</li>
</ul>
<p><strong>Return value Description:</strong></p>
<p>Returns an integer value on success<code>0</code>，Returns an integer value on failure<code>-1</code>。</p>
<p><strong>Example:</strong></p>
<pre class="language-python"><code class="language-python">from machine import UART<br/>uart1 = UART(UART.UART1, 115200, 8, 0, 1, 0)<br/>art1.control_485(UART.GPIO24, 1)
</code></pre>
</div>

<div class="BC25PA|BG95|FCM360W">
<p><strong>This API is not supported by this model</strong></p>
</div>


</div>

### `uart.set_callback`

```python
uart.set_callback(fun)
```

This method sets the callback function of the UART. This callback function will be triggered when data is received on the UART.

**Parameter:**

- `fun` - Callback function of the UART. Prototype:

  ```
  fun(result_list)
  ```

  Parameter of the callback function:

  - `result_list[0]`：Whether the data is received successfully. 

    0 - Received successfully

    Others - Receiving failed
  
  - `result_list[1]`：Port for receiving data.
  
  - `result_list[2]`：How much data is returned. 

**Return Value:**

`0` - Successful execution

`-1` - Failed execution

**Example:**

```python
>>> from machine import UART
>>> uart1 = UART(UART.UART1, 115200, 8, 0, 1, 0)
>>> 
>>> def uart_call(para):
>>>		print(para)
>>> uart1.set_callback(uart_call)
```

**Example:**

```python
"""
Runnnig this routine, you need to connect the main port on the EVB to a PC by a USB-to-Serial Port Adapter, enable the main port by a UART tool on the PC and send data to this port. Then you can see the messages sent by PC.
"""
import _thread
import utime
import log
from machine import UART


'''
The following two global variables are necessary. You can modify the values of these two global variables based on your project requirements.
'''
PROJECT_NAME = "QuecPython_UART_example"
PROJECT_VERSION = "1.0.1"

'''
 * Parameter1: Port
        Note: For EC100Y-CN and EC600S-CN modules, descriptions of UARTn are as follows:
        UART0 - DEBUG PORT
        UART1 – BT PORT
        UART2 – MAIN PORT
        UART3 – USB CDC PORT
 * Parameter2：Baud rate
 * Parameter3：Data bits  (5—8)
 * Parameter4：Parity  （0：NONE  1：EVEN  2：ODD）
 * Parameter5：Stop bits (1–2)
 * Parameter6：Flow control (0: FC_NONE  1: FC_HW)
'''


# Sets the log output level
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
    

# Examples of running results
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

## Constants

| Constant   | Description |
| ---------- | ----------- |
| UART.UART0 | UART0       |
| UART.UART1 | UART1       |
| UART.UART2 | UART2       |
| UART.UART3 | UART3       |
| UART.UART4 | UART4       |

