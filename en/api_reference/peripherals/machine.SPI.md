# SPI – Serial Peripheral Interface Bus Protocol

This class provides bus protocol of serial peripheral interface (SPI) .

## Constructor

### `machine.SPI`

<div class="content-QuecPython">

<div class="EC800N|EG912N|EC200A|EC200A-V1|EG915N|UC200A">
<pre class="language-python"><code class="language-python">class machine.SPI(spi, mode, clk, [group])
</code></pre>
<p><strong>Parameter:</strong></p>
<ul>
<li><p><code>port</code> - Integer type. Channel selection: [0,1].</p>
</li>
<li><p><code>mode</code> - SPI working mode.</code><br/>Clock polarity CPOL: The pin level of clock signal SCLK when SPI is idle (0: low level; 1: high level)<br/><code>0</code> ：<code>CPOL=0, CPHA=0</code><br/><code>1</code> ：<code>CPOL=0, CPHA=1</code></code><br/><code>2</code> ：<code>CPOL=1, CPHA=0</code></code><br/><code>3</code> ：<code>CPOL=1, CPHA=1</code></p>
</li>
<li><p><code>clk</code> - Clock frequency.</code><br/><code>0</code> ：<code>812.5kHz</code><br/><code>1</code> ：<code>1.625MHz</code></code><br/><code>2</code> ：<code>3.25MHz</code></code><br/><code>3</code> ：<code>6.5MHz</code><br/><code>4</code> ：<code>13MHz</code><br/><code>5</code> ：<code>26MHz</code><br/><code>6</code> ：<code>52MHz</code></p>
</li>
<p><code>The 52MHz clock frequency has high requirements for hardware.</code></p>
</ul>
</div>

<div class="BG95">
<pre class="language-python"><code class="language-python">class machine.SPI(spi, mode, clk, [group])
</code></pre>
<p><strong>Parameter:</strong></p>
<ul>
<li><p><code>port</code> - Integer type. Channel selection: [0,1].</p>
</li>
<li><p><code>mode</code> - SPI working mode.</code><br/>Clock polarity CPOL: The pin level of clock signal SCLK when SPI is idle (0: low level; 1: high level)<br/><code>0</code> ：<code>CPOL=0, CPHA=0</code><br/><code>1</code> ：<code>CPOL=0, CPHA=1</code></code><br/><code>2</code> ：<code>CPOL=1, CPHA=0</code></code><br/><code>3</code> ：<code>CPOL=1, CPHA=1</code></p>
</li>
<li><p><code>clk</code> - Clock frequency.</code><br/><code>0</code> ：<code>960kHz</code><br/><code>1</code> ：<code>4.8MHz</code></code><br/><code>2</code> ：<code>9.6MHz</code></code><br/><code>3</code> ：<code>16MHz</code><br/><code>4</code> ：<code>19.2MHz</code><br/><code>5</code> ：<code>25MHz</code><br/><code>6</code> ：<code>50MHz</code></p>
</li>
<p><code>The 50MHz clock frequency has high requirements for hardware.</code></p>
</ul>
</div>

<div class="EC800K|EC600K|EG800K|EG800AK">
<pre class="language-python"><code class="language-python">class machine.SPI(spi, mode, clk, [group])
</code></pre>
<p><strong>Parameter:</strong></p>
<ul>
<li><p><code>port</code> - Integer type. Channel selection: [0,1].</p>
</li>
<li><p><code>mode</code> - SPI working mode.</code><br/>Clock polarity CPOL: The pin level of clock signal SCLK when SPI is idle (0: low level; 1: high level)<br/><code>0</code> ：<code>CPOL=0, CPHA=0</code><br/><code>1</code> ：<code>CPOL=0, CPHA=1</code></code><br/><code>2</code> ：<code>CPOL=1, CPHA=0</code></code><br/><code>3</code> ：<code>CPOL=1, CPHA=1</code></p>
</li>
<li><p><code>clk</code> - Clock frequency.</code><br/><code>0</code> ：<code>100KHz</code><br/><code>1</code> ：<code>200KHz</code></code><br/><code>2</code> ：<code>300KHz</code></code><br/><code>3</code> ：<code>400KHz</code><br/><code>4</code> ：<code>500KHz</code><br/><code>5</code> ：<code>600KHz</code><br/><code>6</code> ：<code>700KHz</code><code>7</code> ：<code>812.5KHz</code><br/><code>8</code> ：<code>1.625MHz</code></code><br/><code>9</code> ：<code>3.25MHz</code></code><br/><code>10</code> ：<code>6.5MHz</code><br/><code>11</code> ：<code>13MHz</code><br/><code>12</code> ：<code>26MHz</code><br/><code>13</code> ：<code>52MHz</code></p>
</li>
<p><code>The 52MHz clock frequency has high requirements for hardware.</code></p>
</ul>
</div>

<div class="EC600M|EC800M|EG810M">
<pre class="language-python"><code class="language-python">class machine.SPI(spi, mode, clk, [group])
</code></pre>
<p><strong>Parameter:</strong></p>
<ul>
<li><p><code>port</code> - Integer type. Channel selection: [0,1].</p>
</li>
<li><p><code>mode</code> - SPI working mode.</code><br/>Clock polarity CPOL: The pin level of clock signal SCLK when SPI is idle (0: low level; 1: high level)<br/><code>0</code> ：<code>CPOL=0, CPHA=0</code><br/><code>1</code> ：<code>CPOL=0, CPHA=1</code></code><br/><code>2</code> ：<code>CPOL=1, CPHA=0</code></code><br/><code>3</code> ：<code>CPOL=1, CPHA=1</code></p>
</li>
<li><p><code>clk</code> - Clock frequency.</code><br/><code>0</code> ：<code>812.5kHz</code><br/><code>1</code> ：<code>1.625MHz</code></code><br/><code>2</code> ：<code>3.25MHz</code></code><br/><code>3</code> ：<code>6.5MHz</code><br/><code>4</code> ：<code>13MHz</code><br/></p>
<li><p><code>[group]</code> - Select to use SPI on different pins, with a default value of 0.<br/><code>1</code> ：<code>This group of pins does not support mounting nor flash.</code></p>
</li>
</ul>
</div>

<div class="EC600E|EC800E|EC800Z|EG800Z">
<pre class="language-python"><code class="language-python">class machine.SPI(spi, mode, clk, [group])
</code></pre>
<p><strong>Parameter:</strong></p>
<ul>
<li><p><code>port</code> - Integer type. Channel selection: [0,1].</p>
</li>
<li><p><code>mode</code> - SPI working mode.</code><br/>Clock polarity CPOL: The pin level of clock signal SCLK when SPI is idle (0: low level; 1: high level)<br/><code>0</code> ：<code>CPOL=0, CPHA=0</code><br/><code>1</code> ：<code>CPOL=0, CPHA=1</code></code><br/><code>2</code> ：<code>CPOL=1, CPHA=0</code></code><br/><code>3</code> ：<code>CPOL=1, CPHA=1</code></p>
</li>
<li><p><code>clk</code> - Clock frequency.</code><br/><code>0</code> ：<code>812.5kHz</code><br/><code>1</code> ：<code>1.625MHz</code></code><br/><code>2</code> ：<code>3.25MHz</code></code><br/><code>3</code> ：<code>6.5MHz</code><br/><code>4</code> ：<code>13MHz</code><br/></p>
</ul>
</div>

<div class="EC600U|EC200U|EG915U">
<pre class="language-python"><code class="language-python">class machine.SPI(spi, mode, clk, [group])
</code></pre>
<p><strong>Parameter:</strong></p>
<ul>
<li><p><code>port</code> - Integer type. Channel selection: [0,1].</p>
</li>
<li><p><code>mode</code> - SPI working mode.</code><br/>Clock polarity CPOL: The pin level of clock signal SCLK when SPI is idle (0: low level; 1: high level)<br/><code>0</code> ：<code>CPOL=0, CPHA=0</code><br/><code>1</code> ：<code>CPOL=0, CPHA=1</code></code><br/><code>2</code> ：<code>CPOL=1, CPHA=0</code></code><br/><code>3</code> ：<code>CPOL=1, CPHA=1</code></p>
</li>
<li><p><code>clk</code> - Clock frequency.</code><br/><code>0</code> ：<code>781.25KHz</code><br/><code>1</code> ：<code>1.5625MHz</code></code><br/><code>2</code> ：<code>3.125MHz</code></code><br/><code>3</code> ：<code>5MHz</code><br/><code>4</code> ：<code>6.25MHz</code><br/><code>5</code> ：<code>10MHz</code><br/><code>6</code> ：<code>12.5MHz</code><br/><code>7</code> ：<code>20MHz</code><br/><code>8</code> ：<code>25MHz</code><br/><code>9</code> ：<code>33.33MHz</code><br/></p>
</li>
</ul>
</div>

<div class="BC25PA|BC25||BC32|BC92">
<pre class="language-python"><code class="language-python">class machine.SPI(spi, mode, clk, [group])
</code></pre>
<p><strong>Parameter:</strong></p>
<ul>
<li><p><code>port</code> - Integer type. Channel selection: [0,1].</p>
</li>
<li><p><code>mode</code> - SPI working mode.</code><br/>Clock polarity CPOL: The pin level of clock signal SCLK when SPI is idle (0: low level; 1: high level)<br/><code>0</code> ：<code>CPOL=0, CPHA=0</code><br/><code>3</code> ：<code>CPOL=1, CPHA=1</code></p>
</li>
<li><p><code>clk</code> - Clock frequency.</code><br/><code>0</code> ：<code>781.25KHz</code><br/><code>X</code> ：<code>XMHz (X in [1,39])</code></code><br/></p>
</ul>
</div>

<div class="EC600G|EC800G">
<pre class="language-python"><code class="language-python">class machine.SPI(spi, mode, clk, [group])
</code></pre>
<p><strong>Parameter:</strong></p>
<ul>
<li><p><code>port</code> - Integer type. Channel selection: [0,1].</p>
</li>
<li><p><code>mode</code> - SPI working mode.</code><br/><code>0</code> ：<code>97.656kHz</code><br/><code>1</code> ：<code>100kHz</code></code><br/><code>2</code> ：<code>812.5kHz</code></code><br/><code>3</code> ：<code>1.3MHz</code><br/><code>4</code> ：<code>1.625MHz</code><br/><code>5</code> ：<code>2MHz</code><br/><code>6</code> ：<code>3.25MHz</code><br/><code>7</code> ：<code>4.333MHz</code><br/><code>8</code> ：<code>6.6MHz</code><br/><code>9</code> ：<code>11.93MHz</code><br/><code>10</code> ：<code>13MHz</code><br/><code>11</code> ：<code>13.92MHz</code><br/><code>12</code> ：<code>16.7MHz</code><br/><code>13</code> ：<code>20.875MHz</code><br/><code>14</code> ：<code>27.83MHz</code><br/></p>
</ul>
</div>

<div class="FCM360W|FCM362K">
<pre class="language-python"><code class="language-python">class machine.SPI(spi, mode, clk, [group])
</code></pre>
<p><strong>Parameter:</strong></p>
<ul>
<li><p><code>port</code> - Integer type. Channel selection: [0,1].</p>
</li>
<li><p><code>mode</code> - SPI working mode.</code><br/>Clock polarity CPOL: The pin level of clock signal SCLK when SPI is idle (0: low level; 1: high level)<br/><code>0</code> ：<code>CPOL=0, CPHA=0</code><br/><code>1</code> ：<code>CPOL=0, CPHA=1</code></code><br/><code>2</code> ：<code>CPOL=1, CPHA=0</code></code><br/><code>3</code> ：<code>CPOL=1, CPHA=1</code></p>
</li>
<li><p><code>clk</code> - Clock frequency.</code><br/><code>0</code> ：<code>500kHz</code><br/><code>1</code> ：<code>1MHz</code></code><br/><code>2</code> ：<code>5MHz</code></code><br/><code>3</code> ：<code>10MHz</code><br/><code>4</code> ：<code>20MHz</code><br/></p>
</li>
</ul>
</div>

<div class="BG950S">
<pre class="language-python"><code class="language-python">class machine.SPI(spi, mode, clk, [group])
</code></pre>
<p><strong>Parameter:</strong></p>
<ul>
<li><p><code>port</code> - Integer type. Channel selection: [0,1].</p>
</li>
<li><p><code>mode</code> - SPI working mode.</code><br/>Clock polarity CPOL: The pin level of clock signal SCLK when SPI is idle (0: low level; 1: high level)<br/><code>0</code> ：<code>CPOL=0, CPHA=0</code><br/><code>1</code> ：<code>CPOL=0, CPHA=1</code></code><br/><code>2</code> ：<code>CPOL=1, CPHA=0</code></code><br/><code>3</code> ：<code>CPOL=1, CPHA=1</code></p>
</li>
<li><p><code>clk</code> - Clock frequency.</code><br/><code>0</code> ：<code>500kHz</code><br/><code>1~30</code> ：<code>(1~30)*1MHz</code><br/></p>
</li>
</ul>
</div>

</div>

**Example:**

```python
>>> from machine import SPI
>>> # Creates a SPI object
>>> spi_obj = SPI(1, 0, 1)
```

**SPI Pin Correspondences:**

<div class="table-QuecPython">
<table class="EC600U">
<thead>
<tr>
  <th>SPI</th>
  <th>CS</th>
  <th>CLK</th>
  <th>MOSI</th>
  <th>MISO</th>
</tr>
</thead>
<tbody>
<tr>
  <td>spi0</td>
  <td> pin4</td>
  <td> pin1</td>
  <td> pin3</td>
  <td> pin2</td>
</tr>
<tr>
  <td>spi1</td>
  <td> pin58</td>
  <td> pin61</td>
  <td> pin59</td>
  <td> pin60</td>
</tr>
</tbody>
</table>

<table class="EC200U">
<thead>
<tr>
  <th>SPI</th>
  <th>CS</th>
  <th>CLK</th>
  <th>MOSI</th>
  <th>MISO</th>
</tr>
</thead>
<tbody>
<tr>
  <td>spi0</td>
  <td> pin134</td>
  <td> pin133</td>
  <td> pin132</td>
  <td> pin131</td>
</tr>
<tr>
  <td>spi1</td>
  <td> pin26</td>
  <td> pin27</td>
  <td> pin24</td>
  <td> pin25</td>
</tr>
</tbody>
</table>

<table class="EC800N">
<thead>
<tr>
  <th>SPI</th>
  <th>CS</th>
  <th>CLK</th>
  <th>MOSI</th>
  <th>MISO</th>
</tr>
</thead>
<tbody>
<tr>
  <td>spi0</td>
  <td> pin31</td>
  <td> pin30</td>
  <td> pin32</td>
  <td> pin33</td>
</tr>
<tr>
  <td>spi1</td>
  <td> pin52</td>
  <td> pin53</td>
  <td> pin50</td>
  <td> pin51</td>
</tr>
</tbody>
</table>

<table class="BC25PA">
<thead>
<tr>
  <th>SPI</th>
  <th>CS</th>
  <th>CLK</th>
  <th>MOSI</th>
  <th>MISO</th>
</tr>
</thead>
<tbody>
<tr>
  <td>spi0</td>
  <td> pin6</td>
  <td> pin5</td>
  <td> pin4</td>
  <td> pin3</td>
</tr>
</tbody>
</table>

<table class="BG95">
<thead>
<tr>
  <th>SPI</th>
  <th>CS</th>
  <th>CLK</th>
  <th>MOSI</th>
  <th>MISO</th>
</tr>
</thead>
<tbody>
<tr>
  <td>spi0</td>
  <td> pin25</td>
  <td> pin26</td>
  <td> pin27</td>
  <td> pin28</td>
</tr>
<tr>
  <td>spi1</td>
  <td> pin41</td>
  <td> pin40</td>
  <td> pin64</td>
  <td> pin65</td>
</tr>
<tr>
  <td>spi2</td>
  <td> pin19</td>
  <td> pin18</td>
  <td> pin23</td>
  <td> pin22</td>
</tr>
</tbody>
</table>

<table class="EC600M">
<thead>
<tr>
  <th>SPI</th>
  <th>CS</th>
  <th>CLK</th>
  <th>MOSI</th>
  <th>MISO</th>
</tr>
</thead>
<tbody>
<tr>
  <td>spi0<br/>group0</td>
  <td> pin58</td>
  <td> pin61</td>
  <td> pin59</td>
  <td> pin60</td>
</tr>
<tr>
  <td>spi0<br/>group1</td>
  <td> pin4</td>
  <td> pin1</td>
  <td> pin3</td>
  <td> pin2</td>
</tr>
<tr>
  <td>spi1<br/>group0</td>
  <td> pin4</td>
  <td> pin1</td>
  <td> pin3</td>
  <td> pin2</td>
</tr>
<tr>
  <td>spi1<br/>group1</td>
  <td> pin65</td>
  <td> pin67</td>
  <td> pin66</td>
  <td> pin63</td>
</tr>
<tr>
  <td>spi2</td>
  <td> pin49</td>
  <td> pin54</td>
  <td> pin53</td>
  <td> pin52</td>
</tr>
</tbody>
</table>

<table class="EG915U">
<thead>
<tr>
  <th>SPI</th>
  <th>CS</th>
  <th>CLK</th>
  <th>MOSI</th>
  <th>MISO</th>
</tr>
</thead>
<tbody>
<tr>
  <td>spi0</td>
  <td> pin25</td>
  <td> pin26</td>
  <td> pin64</td>
  <td> pin88</td>
</tr>
<tr>
  <td>spi1</td>
  <td> pin5</td>
  <td> pin4</td>
  <td> pin6</td>
  <td> pin7</td>
</tr>
</tbody>
</table>

<table class="EC800M">
<thead>
<tr>
  <th>SPI</th>
  <th>CS</th>
  <th>CLK</th>
  <th>MOSI</th>
  <th>MISO</th>
</tr>
</thead>
<tbody>
<tr>
  <td>spi0<br/>group0</td>
  <td> pin31</td>
  <td> pin30</td>
  <td> pin32</td>
  <td> pin33</td>
</tr>
<tr>
  <td>spi0<br/>group1</td>
  <td> pin52</td>
  <td> pin53</td>
  <td> pin50</td>
  <td> pin51</td>
</tr>
<tr>
  <td>spi1<br/>group0</td>
  <td> pin52</td>
  <td> pin53</td>
  <td> pin50</td>
  <td> pin51</td>
</tr>
<tr>
  <td>spi1<br/>group1</td>
  <td> pin69</td>
  <td> pin68</td>
  <td> pin85</td>
  <td> pin84</td>
</tr>
<tr>
  <td>spi2</td>
  <td> pin76</td>
  <td> pin77</td>
  <td> pin78</td>
  <td> pin16</td>
</tr>
</tbody>
</table>

<table class="EG810M">
<thead>
<tr>
  <th>SPI</th>
  <th>CS</th>
  <th>CLK</th>
  <th>MOSI</th>
  <th>MISO</th>
</tr>
</thead>
<tbody>
<tr>
  <td>spi0<br/>group0</td>
  <td> pin31</td>
  <td> pin30</td>
  <td> pin32</td>
  <td> pin33</td>
</tr>
<tr>
  <td>spi0<br/>group1</td>
  <td> pin52</td>
  <td> pin53</td>
  <td> pin50</td>
  <td> pin51</td>
</tr>
<tr>
  <td>spi1<br/>group0</td>
  <td> pin52</td>
  <td> pin53</td>
  <td> pin50</td>
  <td> pin51</td>
</tr>
<tr>
  <td>spi1<br/>group1(EG810M_EU unsupported)</td>
  <td> pin69</td>
  <td> pin68</td>
  <td> pin85</td>
  <td> pin84</td>
</tr>
<tr>
  <td>spi2</td>
  <td> pin76</td>
  <td> pin77</td>
  <td> pin78</td>
  <td> pin16</td>
</tr>
</tbody>
</table>

<table class="EG912N">
<thead>
<tr>
  <th>SPI</th>
  <th>CS</th>
  <th>CLK</th>
  <th>MOSI</th>
  <th>MISO</th>
</tr>
</thead>
<tbody>
<tr>
  <td>spi0</td>
  <td> pin25</td>
  <td> pin26</td>
  <td> pin27</td>
  <td> pin28</td>
</tr>
<tr>
  <td>spi1</td>
  <td> pin5</td>
  <td> pin4</td>
  <td> pin6</td>
  <td> pin7</td>
</tr>
</tbody>
</table>

<table class="UC200A">
<thead>
<tr>
  <th>SPI</th>
  <th>CS</th>
  <th>CLK</th>
  <th>MOSI</th>
  <th>MISO</th>
</tr>
</thead>
<tbody>
<tr>
  <td>spi0</td>
  <td> pin37</td>
  <td> pin40</td>
  <td> pin38</td>
  <td> pin39</td>
</tr>
<tr>
  <td>spi1</td>
  <td> pin26</td>
  <td> pin27</td>
  <td> pin25</td>
  <td> pin24</td>
</tr>
</tbody>
</table>

<table class="EC200A">
<thead>
<tr>
  <th>SPI</th>
  <th>CS</th>
  <th>CLK</th>
  <th>MOSI</th>
  <th>MISO</th>
</tr>
</thead>
<tbody>
<tr>
  <td>spi0</td>
  <td> pin37</td>
  <td> pin40</td>
  <td> pin38</td>
  <td> pin39</td>
</tr>
<tr>
  <td>spi1</td>
  <td> pin26</td>
  <td> pin27</td>
  <td> pin25</td>
  <td> pin24</td>
</tr>
</tbody>
</table>

<table class="EC200A-V1">
<thead>
<tr>
  <th>SPI</th>
  <th>CS</th>
  <th>CLK</th>
  <th>MOSI</th>
  <th>MISO</th>
</tr>
</thead>
<tbody>
<tr>
  <td>spi0</td>
  <td> pin37</td>
  <td> pin40</td>
  <td> pin38</td>
  <td> pin39</td>
</tr>
<tr>
  <td>spi1</td>
  <td> pin136</td>
  <td> pin135</td>
  <td> pin127</td>
  <td> pin62</td>
</tr>
</tbody>
</table>

<table class="EC600E">
<thead>
<tr>
  <th>SPI</th>
  <th>CS</th>
  <th>CLK</th>
  <th>MOSI</th>
  <th>MISO</th>
</tr>
</thead>
<tbody>
<tr>
  <td>spi0(EC600ECN_LE&LQ unsupported)</td>
  <td> pin65</td>
  <td> pin67</td>
  <td> pin66</td>
  <td> pin64</td>
</tr>
<tr>
  <td>spi1</td>
  <td> pin69</td>
  <td> pin71</td>
  <td> pin70</td>
  <td> pin72</td>
</tr>
</tbody>
</table>

<table class="EC800E">
<thead>
<tr>
  <th>SPI</th>
  <th>CS</th>
  <th>CLK</th>
  <th>MOSI</th>
  <th>MISO</th>
</tr>
</thead>
<tbody>
<tr>
  <td>spi0</td>
  <td> pin28</td>
  <td> pin39</td>
  <td> pin29</td>
  <td> pin38</td>
</tr>
<tr>
  <td>spi1(EC800ECN_LE&LQ&CG unsupported)</td>
  <td> pin52</td>
  <td> pin53</td>
  <td> pin50</td>
  <td> pin51</td>
</tr>
</tbody>
</table>

<table class="EC600G">
<thead>
<tr>
  <th>SPI</th>
  <th>CS</th>
  <th>CLK</th>
  <th>MOSI</th>
  <th>MISO</th>
</tr>
</thead>
<tbody>
<tr>
  <td>spi0</td>
  <td> pin4</td>
  <td> pin1</td>
  <td> pin3</td>
  <td> pin2</td>
</tr>
<tr>
  <td>spi1(EC800ECN_LE&LQ&CG unsupported)</td>
  <td> pin53</td>
  <td> pin52</td>
  <td> pin54</td>
  <td> pin51</td>
</tr>
</tbody>
</table>

<table class="EC800G">
<thead>
<tr>
  <th>SPI</th>
  <th>CS</th>
  <th>CLK</th>
  <th>MOSI</th>
  <th>MISO</th>
</tr>
</thead>
<tbody>
<tr>
  <td>spi1</td>
  <td> pin79</td>
  <td> pin101</td>
  <td> pin100</td>
  <td> pin108</td>
</tr>
</tbody>
</table>

<table class="EG912U">
<thead>
<tr>
  <th>SPI</th>
  <th>CS</th>
  <th>CLK</th>
  <th>MOSI</th>
  <th>MISO</th>
</tr>
</thead>
<tbody>
<tr>
  <td>spi0(EG912UGLAA unsupported)</td>
  <td> pin25</td>
  <td> pin26</td>
  <td> pin64</td>
  <td> pin88</td>
</tr>
<tr>
  <td>spi1</td>
  <td> pin5</td>
  <td> pin4</td>
  <td> pin6</td>
  <td> pin7</td>
</tr>
</tbody>
</table>

<table class="EC600K">
<thead>
<tr>
  <th>SPI</th>
  <th>CS</th>
  <th>CLK</th>
  <th>MOSI</th>
  <th>MISO</th>
</tr>
</thead>
<tbody>
<tr>
  <td>spi1</td>
  <td> pin4</td>
  <td> pin1</td>
  <td> pin3</td>
  <td> pin2</td>
</tr>
<tr>
  <td>spi2</td>
  <td> pin58</td>
  <td> pin61</td>
  <td> pin59</td>
  <td> pin60</td>
</tr>
</tbody>
</table>

<table class="EG800K">
<thead>
<tr>
  <th>SPI</th>
  <th>CS</th>
  <th>CLK</th>
  <th>MOSI</th>
  <th>MISO</th>
</tr>
</thead>
<tbody>
<tr>
  <td>spi0</td>
  <td> pin31</td>
  <td> pin30</td>
  <td> pin32</td>
  <td> pin33</td>
</tr>
<tr>
  <td>spi1</td>
  <td> pin52</td>
  <td> pin53</td>
  <td> pin50</td>
  <td> pin51</td>
</tr>
<tr>
  <td>spi2</td>
  <td> pin74</td>
  <td> pin75</td>
  <td> pin76</td>
  <td> pin77</td>
</tr>
</tbody>
</table>

<table class="EC800K">
<thead>
<tr>
  <th>SPI</th>
  <th>CS</th>
  <th>CLK</th>
  <th>MOSI</th>
  <th>MISO</th>
</tr>
</thead>
<tbody>
<tr>
  <td>spi0</td>
  <td> pin31</td>
  <td> pin30</td>
  <td> pin32</td>
  <td> pin33</td>
</tr>
<tr>
  <td>spi1</td>
  <td> pin52</td>
  <td> pin53</td>
  <td> pin50</td>
  <td> pin51</td>
</tr>
<tr>
  <td>spi2</td>
  <td> pin74</td>
  <td> pin75</td>
  <td> pin76</td>
  <td> pin77</td>
</tr>
</tbody>
</table>

<table class="EG800AK">
<thead>
<tr>
  <th>SPI</th>
  <th>CS</th>
  <th>CLK</th>
  <th>MOSI</th>
  <th>MISO</th>
</tr>
</thead>
<tbody>
<tr>
  <td>spi0</td>
  <td> pin31</td>
  <td> pin30</td>
  <td> pin32</td>
  <td> pin33</td>
</tr>
<tr>
  <td>spi1</td>
  <td> pin52</td>
  <td> pin53</td>
  <td> pin50</td>
  <td> pin51</td>
</tr>
<tr>
  <td>spi2</td>
  <td> pin74</td>
  <td> pin75</td>
  <td> pin76</td>
  <td> pin77</td>
</tr>
</tbody>
</table>

<table class="EG800Z">
<thead>
<tr>
  <th>SPI</th>
  <th>CS</th>
  <th>CLK</th>
  <th>MOSI</th>
  <th>MISO</th>
</tr>
</thead>
<tbody>
<tr>
  <td>spi0</td>
  <td> pin66</td>
  <td> pin29</td>
  <td> pin67</td>
  <td> pin28</td>
</tr>
<tr>
  <td>spi1</td>
  <td> pin64</td>
  <td> pin49</td>
  <td> pin63</td>
  <td> pin62</td>
</tr>
</tbody>
</table>

<table class="FCM360W">
<thead>
<tr>
  <th>SPI</th>
  <th>CS</th>
  <th>CLK</th>
  <th>MOSI</th>
  <th>MISO</th>
</tr>
</thead>
<tbody>
<tr>
  <td>spi0</td>
  <td> pin21</td>
  <td> pin22</td>
  <td> pin29</td>
  <td> pin23</td>
</tr>
</tbody>
</table>

<table class="FCM362K">
<thead>
<tr>
  <th>SPI</th>
  <th>CS</th>
  <th>CLK</th>
  <th>MOSI</th>
  <th>MISO</th>
</tr>
</thead>
<tbody>
<tr>
  <td>spi0</td>
  <td> pin29</td>
  <td> pin30</td>
  <td> pin37</td>
  <td> pin31</td>
</tr>
</tbody>
</table>

<table class="BC32">
<thead>
<tr>
  <th>SPI</th>
  <th>CS</th>
  <th>CLK</th>
  <th>MOSI</th>
  <th>MISO</th>
</tr>
</thead>
<tbody>
<tr>
  <td>spi0</td>
  <td> pin41</td>
  <td> pin42</td>
  <td> pin43</td>
  <td> pin44</td>
</tr>
</tbody>
</table>

<table class="BC92">
<thead>
<tr>
  <th>SPI</th>
  <th>CS</th>
  <th>CLK</th>
  <th>MOSI</th>
  <th>MISO</th>
</tr>
</thead>
<tbody>
<tr>
  <td>spi0</td>
  <td> pin41</td>
  <td> pin42</td>
  <td> pin43</td>
  <td> pin44</td>
</tr>
</tbody>
</table>

<table class="EG915N">
<thead>
<tr>
  <th>SPI </th>
  <th>CS</th>
  <th>CLK</th>
  <th>MOSI</th>
  <th>MISO</th>
</tr>
</thead>
<tbody>
<tr>
  <td>spi0</td>
  <td> pin5</td>
  <td> pin4</td>
  <td> pin6</td>
  <td> pin7</td>
</tr>
</tbody>
</table>

<table class="EC800Z">
<thead>
<tr>
  <th>SPI</th>
  <th>CS</th>
  <th>CLK</th>
  <th>MOSI</th>
  <th>MISO</th>
</tr>
</thead>
<tbody>
<tr>
  <td>spi0</td>
  <td> pin49</td>
  <td> pin29</td>
  <td> pin101</td>
  <td> pin28</td>
</tr>
<tr>
  <td>spi1</td>
  <td> pin52</td>
  <td> pin53</td>
  <td> pin50</td>
  <td> pin51</td>
</tr>
</tbody>
</table>

<table class="BG950S">
<thead>
<tr>
  <th>SPI</th>
  <th>CS</th>
  <th>CLK</th>
  <th>MOSI</th>
  <th>MISO</th>
</tr>
</thead>
<tbody>
<tr>
  <td>spi0</td>
  <td> pin86</td>
  <td> pin85</td>
  <td> pin88</td>
  <td> pin87</td>
</tr>
</tbody>
</table>

</div>

## Methods

### `SPI.read`

```python
SPI.read(recv_data, datalen)
```

This method reads data.

**Parameter:**

- `recv_data` - Bytearray type. An array used to receive data.
- `datalen` - Integer type. Length of the data to be read.

**Return Value:**

`0` - Successful execution

`-1` - Failed execution

### `SPI.write`

```python
SPI.write(data, datalen)
```

This method writes data.

**Parameter:**

- `data` - Bytearray type. Data to be written.
- `datalen` - Integer type. Length of data to be written.

**Return Value:**

`0` - Successful execution

`-1` - Failed execution

### `SPI.write_read`

```python
SPI.write_read(r_data, data, datalen)
```

This method writes and reads data.

**Parameter:**

- `r_data  ` - Bytearray type. An array used to receive data.
- `data` - Bytearray type. Data to be sent.
- `datalen` - Integer type. Length of data to be read.

**Return Value:**

`0` - Successful execution

`-1` - Failed execution

### `SPI.close`

```python
SPI.close()
```

This method close SPI.

**Return Value:**

`0` - Successful execution

`-1` - Failed execution

**Example:**

> Please use this function with the peripherals.

```python
import log
from machine import SPI
import utime



spi_obj = SPI(0, 0, 1)

# Sets the log output level 
log.basicConfig(level=log.INFO)
spi_log = log.getLogger("SPI")


if __name__ == '__main__':
    r_data = bytearray(5)  # Creates a buff for receiving data
    data = b"world"  # Tests data

    ret = spi_obj.write_read(r_data, data, 5)  # Writes data and receives data to r_data
    spi_log.info(r_data)

```
