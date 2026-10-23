# class SPI – SPI通信

该类提供串行外设接口总线协议功能。

## 构造函数

### `machine.SPI`

<div class="content-QuecPython">

<div class="EC800N|EG912N|EC200A|EC200A-V1|EG915N|UC200A">
<pre class="language-python"><code class="language-python">class machine.SPI(spi, mode, clk, [group])
</code></pre>
<p><strong>参数描述：</strong></p>
<ul>
<li><p><code>spi</code> - 通道选择[0,1]，int类型。</p>
</li>
<li><p><code>mode</code> - SPI 的工作模式，int类型，说明如下：</code><br/>时钟极性CPOL：即SPI空闲时，时钟信号SCLK的电平(0:空闲时低电平; 1:空闲时高电平)<br/><code>0</code> ：<code>CPOL=0, CPHA=0</code><br/><code>1</code> ：<code>CPOL=0, CPHA=1</code></code><br/><code>2</code> ：<code>CPOL=1, CPHA=0</code></code><br/><code>3</code> ：<code>CPOL=1, CPHA=1</code></p>
</li>
<li><p><code>clk</code> - 时钟频率，说明如下：</code><br/><code>0</code> ：<code>812.5kHz</code><br/><code>1</code> ：<code>1.625MHz</code></code><br/><code>2</code> ：<code>3.25MHz</code></code><br/><code>3</code> ：<code>6.5MHz</code><br/><code>4</code> ：<code>13MHz</code><br/><code>5</code> ：<code>26MHz</code><br/><code>6</code> ：<code>52MHz</code></p>
</li>
<p><code>52MHz时钟频率对硬件要求较高。</code></p>
</ul>
</div>

<div class="BG95">
<pre class="language-python"><code class="language-python">class machine.SPI(spi, mode, clk, [group])
</code></pre>
<p><strong>参数描述：</strong></p>
<ul>
<li><p><code>spi</code> - 通道选择[0,1]，int类型。</p>
</li>
<li><p><code>mode</code> - SPI 的工作模式，int类型，说明如下：</code><br/>时钟极性CPOL：即SPI空闲时，时钟信号SCLK的电平(0:空闲时低电平; 1:空闲时高电平)<br/><code>0</code> ：<code>CPOL=0, CPHA=0</code><br/><code>1</code> ：<code>CPOL=0, CPHA=1</code></code><br/><code>2</code> ：<code>CPOL=1, CPHA=0</code></code><br/><code>3</code> ：<code>CPOL=1, CPHA=1</code></p>
</li>
<li><p><code>clk</code> - 时钟频率，说明如下：</code><br/><code>0</code> ：<code>960kHz</code><br/><code>1</code> ：<code>4.8MHz</code></code><br/><code>2</code> ：<code>9.6MHz</code></code><br/><code>3</code> ：<code>16MHz</code><br/><code>4</code> ：<code>19.2MHz</code><br/><code>5</code> ：<code>25MHz</code><br/><code>6</code> ：<code>50MHz</code></p>
</li>
<p><code>50MHz时钟频率对硬件要求较高。</code></p>
</ul>
</div>

<div class="EC800K|EC600K|EG800K|EG800AK">
<pre class="language-python"><code class="language-python">class machine.SPI(spi, mode, clk, [group])
</code></pre>
<p><strong>参数描述：</strong></p>
<ul>
<li><p><code>spi</code> - 通道选择[0,1]，int类型。</p>
</li>
<li><p><code>mode</code> - SPI 的工作模式，int类型，说明如下：</code><br/>时钟极性CPOL：即SPI空闲时，时钟信号SCLK的电平(0:空闲时低电平; 1:空闲时高电平)<br/><code>0</code> ：<code>CPOL=0, CPHA=0</code><br/><code>1</code> ：<code>CPOL=0, CPHA=1</code></code><br/><code>2</code> ：<code>CPOL=1, CPHA=0</code></code><br/><code>3</code> ：<code>CPOL=1, CPHA=1</code></p>
</li>
<li><p><code>clk</code> - 时钟频率，说明如下：</code><br/><code>0</code> ：<code>100KHz</code><br/><code>1</code> ：<code>200KHz</code></code><br/><code>2</code> ：<code>300KHz</code></code><br/><code>3</code> ：<code>400KHz</code><br/><code>4</code> ：<code>500KHz</code><br/><code>5</code> ：<code>600KHz</code><br/><code>6</code> ：<code>700KHz</code></code><br/><code>7</code> ：<code>812.5KHz</code><br/><code>8</code> ：<code>1.625MHz</code></code><br/><code>9</code> ：<code>3.25MHz</code></code><br/><code>10</code> ：<code>6.5MHz</code><br/><code>11</code> ：<code>13MHz</code><br/><code>12</code> ：<code>26MHz</code><br/><code>13</code> ：<code>52MHz</code></p>
</li>
<p><code>52MHz时钟频率对硬件要求较高。</code></p>
</ul>
</div>

<div class="EC600M|EC800M|EG810M">
<pre class="language-python"><code class="language-python">class machine.SPI(spi, mode, clk, [group])
</code></pre>
<p><strong>参数描述：</strong></p>
<ul>
<li><p><code>spi</code> - 通道选择[0,1]，int类型。</p>
</li>
<li><p><code>mode</code> - SPI 的工作模式，int类型，说明如下：</code><br/>时钟极性CPOL：即SPI空闲时，时钟信号SCLK的电平(0:空闲时低电平; 1:空闲时高电平)<br/><code>0</code> ：<code>CPOL=0, CPHA=0</code><br/><code>1</code> ：<code>CPOL=0, CPHA=1</code></code><br/><code>2</code> ：<code>CPOL=1, CPHA=0</code></code><br/><code>3</code> ：<code>CPOL=1, CPHA=1</code></p>
</li>
<li><p><code>clk</code> - 时钟频率，说明如下：</code><br/><code>0</code> ：<code>812.5kHz</code><br/><code>1</code> ：<code>1.625MHz</code></code><br/><code>2</code> ：<code>3.25MHz</code></code><br/><code>3</code> ：<code>6.5MHz</code><br/><code>4</code> ：<code>13MHz</code><br/></p>
</li>
<li><p><code>[group]</code> - 选择在不同管脚使用spi，缺省值为0。<br/><code>1</code> ：<code>该组管脚不支持挂载 nor flash</code></p>
</li>
</ul>
</div>

<div class="EC600E|EC800E|EC800Z|EG800Z">
<pre class="language-python"><code class="language-python">class machine.SPI(spi, mode, clk, [group])
</code></pre>
<p><strong>参数描述：</strong></p>
<ul>
<li><p><code>spi</code> - 通道选择[0,1]，int类型。</p>
</li>
<li><p><code>mode</code> - SPI 的工作模式，int类型，说明如下：</code><br/>时钟极性CPOL：即SPI空闲时，时钟信号SCLK的电平(0:空闲时低电平; 1:空闲时高电平)<br/><code>0</code> ：<code>CPOL=0, CPHA=0</code><br/><code>1</code> ：<code>CPOL=0, CPHA=1</code></code><br/><code>2</code> ：<code>CPOL=1, CPHA=0</code></code><br/><code>3</code> ：<code>CPOL=1, CPHA=1</code></p>
</li>
<li><p><code>clk</code> - 时钟频率，说明如下：</code><br/><code>0</code> ：<code>812.5kHz</code><br/><code>1</code> ：<code>1.625MHz</code></code><br/><code>2</code> ：<code>3.25MHz</code></code><br/><code>3</code> ：<code>6.5MHz</code><br/><code>4</code> ：<code>13MHz</code><br/></p>
</li>
<li><p><code>[group]</code> - 选择在不同管脚使用spi，缺省值为0。<br/><code>0</code> ：<code>缺省值</code><br/><code>1</code> ：<code>不支持挂载 nor flash</code></p>
</li>
</ul>
</div>

<div class="EC600U|EC200U|EG915U|EG912U">
<pre class="language-python"><code class="language-python">class machine.SPI(spi, mode, clk, [group])
</code></pre>
<p><strong>参数描述：</strong></p>
<ul>
<li><p><code>spi</code> - 通道选择[0,1]，int类型。</p>
</li>
<li><p><code>mode</code> - SPI 的工作模式，int类型，说明如下：</code><br/>时钟极性CPOL：即SPI空闲时，时钟信号SCLK的电平(0:空闲时低电平; 1:空闲时高电平)<br/><code>0</code> ：<code>CPOL=0, CPHA=0</code><br/><code>1</code> ：<code>CPOL=0, CPHA=1</code></code><br/><code>2</code> ：<code>CPOL=1, CPHA=0</code></code><br/><code>3</code> ：<code>CPOL=1, CPHA=1</code></p>
</li>
<li><p><code>clk</code> - 时钟频率，说明如下：</code><br/><code>0</code> ：<code>781.25KHz</code><br/><code>1</code> ：<code>1.5625MHz</code></code><br/><code>2</code> ：<code>3.125MHz</code></code><br/><code>3</code> ：<code>5MHz</code><br/><code>4</code> ：<code>6.25MHz</code><br/><code>5</code> ：<code>10MHz</code><br/><code>6</code> ：<code>12.5MHz</code><br/><code>7</code> ：<code>20MHz</code><br/><code>8</code> ：<code>25MHz</code><br/><code>9</code> ：<code>33.33MHz</code><br/></p>
</li>
</ul>
</div>

<div class="BC25PA|BC25||BC32|BC92">
<pre class="language-python"><code class="language-python">class machine.SPI(spi, mode, clk, [group])
</code></pre>
<p><strong>参数描述：</strong></p>
<ul>
<li><p><code>spi</code> - 通道选择[0,1]，int类型。</p>
</li>
<li><p><code>mode</code> - SPI 的工作模式，int类型，说明如下：</code><br/>时钟极性CPOL：即SPI空闲时，时钟信号SCLK的电平(0:空闲时低电平; 1:空闲时高电平)<br/><code>0</code> ：<code>CPOL=0, CPHA=0</code><br/><code>3</code> ：<code>CPOL=1, CPHA=1</code></p>
</li>
<li><p><code>clk</code> - 时钟频率，说明如下：</code><br/><code>0</code> ：<code>781.25KHz</code><br/><code>X</code> ：<code>XMHz (X in [1,39])</code></code><br/></p>
</li>
</ul>
</div>

<div class="EC600G|EC800G">
<pre class="language-python"><code class="language-python">class machine.SPI(spi, mode, clk, [group])
</code></pre>
<p><strong>参数描述：</strong></p>
<ul>
<li><p><code>spi</code> - 通道选择[0,1]，int类型。</p>
</li>
<li><p><code>mode</code> - SPI 的工作模式，int类型，说明如下：</code><br/>时钟极性CPOL：即SPI空闲时，时钟信号SCLK的电平(0:空闲时低电平; 1:空闲时高电平)<br/><code>0</code> ：<code>CPOL=0, CPHA=0</code><br/><code>1</code> ：<code>CPOL=0, CPHA=1</code></code><br/><code>2</code> ：<code>CPOL=1, CPHA=0</code></code><br/><code>3</code> ：<code>CPOL=1, CPHA=1</code></p>
</li>
<li><p><code>clk</code> - 时钟频率，说明如下：</code><br/><code>0</code> ：<code>97.656kHz</code><br/><code>1</code> ：<code>100kHz</code></code><br/><code>2</code> ：<code>812.5kHz</code></code><br/><code>3</code> ：<code>1.3MHz</code><br/><code>4</code> ：<code>1.625MHz</code><br/><code>5</code> ：<code>2MHz</code><br/><code>6</code> ：<code>3.25MHz</code><br/><code>7</code> ：<code>4.333MHz</code><br/><code>8</code> ：<code>6.6MHz</code><br/><code>9</code> ：<code>11.93MHz</code><br/><code>10</code> ：<code>13MHz</code><br/><code>11</code> ：<code>13.92MHz</code><br/><code>12</code> ：<code>16.7MHz</code><br/><code>13</code> ：<code>20.875MHz</code><br/><code>14</code> ：<code>27.83MHz</code><br/></p>
</li>
</ul>
</div>

<div class="FCM360W|FCM362K">
<pre class="language-python"><code class="language-python">class machine.SPI(spi, mode, clk, [group])
</code></pre>
<p><strong>参数描述：</strong></p>
<ul>
<li><p><code>spi</code> - 通道选择[0,1]，int类型。</p>
</li>
<li><p><code>mode</code> - SPI 的工作模式，int类型，说明如下：</code><br/>时钟极性CPOL：即SPI空闲时，时钟信号SCLK的电平(0:空闲时低电平; 1:空闲时高电平)<br/><code>0</code> ：<code>CPOL=0, CPHA=0</code><br/><code>1</code> ：<code>CPOL=0, CPHA=1</code></code><br/><code>2</code> ：<code>CPOL=1, CPHA=0</code></code><br/><code>3</code> ：<code>CPOL=1, CPHA=1</code></p>
</li>
<li><p><code>clk</code> - 时钟频率，说明如下：</code><br/><code>0</code> ：<code>500kHz</code><br/><code>1</code> ：<code>1MHz</code></code><br/><code>2</code> ：<code>5MHz</code></code><br/><code>3</code> ：<code>10MHz</code><br/><code>4</code> ：<code>20MHz</code><br/></p>
</li>
</ul>
</div>


<div class="BG950S">
<pre class="language-python"><code class="language-python">class machine.SPI(spi, mode, clk)
</code></pre>
<p><strong>参数描述：</strong></p>
<ul>
<li><p><code>spi</code> - 通道选择[0,1]，int类型。</p>
</li>
<li><p><code>mode</code> - SPI 的工作模式，int类型，说明如下：</code><br/>时钟极性CPOL：即SPI空闲时，时钟信号SCLK的电平(0:空闲时低电平; 1:空闲时高电平)<br/><code>0</code> ：<code>CPOL=0, CPHA=0</code><br/><code>1</code> ：<code>CPOL=0, CPHA=1</code></code><br/><code>2</code> ：<code>CPOL=1, CPHA=0</code></code><br/><code>3</code> ：<code>CPOL=1, CPHA=1</code></p>
</li>
<li><p><code>clk</code> - 时钟频率，说明如下：</code><br/><code>0</code> ：<code>500kHz</code><br/><code>1~30</code> ：<code>(1~30)*1MHz</code><br/></p>
</li>
</ul>
</div>

</div>


**示例：**

```python
>>> from machine import SPI
>>> # 创建SPI对象
>>> spi_obj = SPI(1, 0, 1)
```


**SPI引脚对应关系：**

<div class="table-QuecPython">
<table class="EC600U">
<thead>
<tr>
  <th>SPI通道</th>
  <th>CS引脚</th>
  <th>CLK引脚</th>
  <th>MOSI引脚</th>
  <th>MISO引脚</th>
</tr>
</thead>
<tbody>
<tr>
  <td>spi0</td>
  <td>引脚4</td>
  <td>引脚1</td>
  <td>引脚3</td>
  <td>引脚2</td>
</tr>
<tr>
  <td>spi1</td>
  <td>引脚58</td>
  <td>引脚61</td>
  <td>引脚59</td>
  <td>引脚60</td>
</tr>
</tbody>
</table>

<table class="EC200U">
<thead>
<tr>
  <th>SPI通道</th>
  <th>CS引脚</th>
  <th>CLK引脚</th>
  <th>MOSI引脚</th>
  <th>MISO引脚</th>
</tr>
</thead>
<tbody>
<tr>
  <td>spi0</td>
  <td>引脚134</td>
  <td>引脚133</td>
  <td>引脚132</td>
  <td>引脚131</td>
</tr>
<tr>
  <td>spi1</td>
  <td>引脚26</td>
  <td>引脚27</td>
  <td>引脚24</td>
  <td>引脚25</td>
</tr>
</tbody>
</table>

<table class="EC800N">
<thead>
<tr>
  <th>SPI通道</th>
  <th>CS引脚</th>
  <th>CLK引脚</th>
  <th>MOSI引脚</th>
  <th>MISO引脚</th>
</tr>
</thead>
<tbody>
<tr>
  <td>spi0</td>
  <td>引脚31</td>
  <td>引脚30</td>
  <td>引脚32</td>
  <td>引脚33</td>
</tr>
<tr>
  <td>spi1</td>
  <td>引脚52</td>
  <td>引脚53</td>
  <td>引脚50</td>
  <td>引脚51</td>
</tr>
</tbody>
</table>

<table class="BC25PA">
<thead>
<tr>
  <th>SPI通道</th>
  <th>CS引脚</th>
  <th>CLK引脚</th>
  <th>MOSI引脚</th>
  <th>MISO引脚</th>
</tr>
</thead>
<tbody>
<tr>
  <td>spi0</td>
  <td>引脚6</td>
  <td>引脚5</td>
  <td>引脚4</td>
  <td>引脚3</td>
</tr>
</tbody>
</table>

<table class="BG95">
<thead>
<tr>
  <th>SPI通道</th>
  <th>CS引脚</th>
  <th>CLK引脚</th>
  <th>MOSI引脚</th>
  <th>MISO引脚</th>
</tr>
</thead>
<tbody>
<tr>
  <td>spi0</td>
  <td>引脚25</td>
  <td>引脚26</td>
  <td>引脚27</td>
  <td>引脚28</td>
</tr>
<tr>
  <td>spi1</td>
  <td>引脚41</td>
  <td>引脚40</td>
  <td>引脚64</td>
  <td>引脚65</td>
</tr>
<tr>
  <td>spi2</td>
  <td>引脚19</td>
  <td>引脚18</td>
  <td>引脚23</td>
  <td>引脚22</td>
</tr>
</tbody>
</table>

<table class="EC600M">
<thead>
<tr>
  <th>SPI通道</th>
  <th>CS引脚</th>
  <th>CLK引脚</th>
  <th>MOSI引脚</th>
  <th>MISO引脚</th>
</tr>
</thead>
<tbody>
<tr>
  <td>spi0<br/>group0</td>
  <td>引脚58</td>
  <td>引脚61</td>
  <td>引脚59</td>
  <td>引脚60</td>
</tr>
<tr>
  <td>spi0<br/>group1</td>
  <td>引脚4</td>
  <td>引脚1</td>
  <td>引脚3</td>
  <td>引脚2</td>
</tr>
<tr>
  <td>spi1<br/>group0</td>
  <td>引脚4</td>
  <td>引脚1</td>
  <td>引脚3</td>
  <td>引脚2</td>
</tr>
<tr>
  <td>spi1<br/>group1</td>
  <td>引脚65</td>
  <td>引脚67</td>
  <td>引脚66</td>
  <td>引脚63</td>
</tr>
<tr>
  <td>spi2</td>
  <td>引脚49</td>
  <td>引脚54</td>
  <td>引脚53</td>
  <td>引脚52</td>
</tr>
</tbody>
</table>

<table class="EG915U">
<thead>
<tr>
  <th>SPI通道</th>
  <th>CS引脚</th>
  <th>CLK引脚</th>
  <th>MOSI引脚</th>
  <th>MISO引脚</th>
</tr>
</thead>
<tbody>
<tr>
  <td>spi0</td>
  <td>引脚25</td>
  <td>引脚26</td>
  <td>引脚64</td>
  <td>引脚88</td>
</tr>
<tr>
  <td>spi1</td>
  <td>引脚5</td>
  <td>引脚4</td>
  <td>引脚6</td>
  <td>引脚7</td>
</tr>
</tbody>
</table>

<table class="EC800M">
<thead>
<tr>
  <th>SPI通道</th>
  <th>CS引脚</th>
  <th>CLK引脚</th>
  <th>MOSI引脚</th>
  <th>MISO引脚</th>
</tr>
</thead>
<tbody>
<tr>
  <td>spi0<br/>group0</td>
  <td>引脚31</td>
  <td>引脚30</td>
  <td>引脚32</td>
  <td>引脚33</td>
</tr>
<tr>
  <td>spi0<br/>group1</td>
  <td>引脚52</td>
  <td>引脚53</td>
  <td>引脚50</td>
  <td>引脚51</td>
</tr>
<tr>
  <td>spi1<br/>group0</td>
  <td>引脚52</td>
  <td>引脚53</td>
  <td>引脚50</td>
  <td>引脚51</td>
</tr>
<tr>
  <td>spi1<br/>group1</td>
  <td>引脚69</td>
  <td>引脚68</td>
  <td>引脚85</td>
  <td>引脚84</td>
</tr>
<tr>
  <td>spi2</td>
  <td>引脚76</td>
  <td>引脚77</td>
  <td>引脚78</td>
  <td>引脚16</td>
</tr>
</tbody>
</table>

<table class="EG810M">
<thead>
<tr>
  <th>SPI通道</th>
  <th>CS引脚</th>
  <th>CLK引脚</th>
  <th>MOSI引脚</th>
  <th>MISO引脚</th>
</tr>
</thead>
<tbody>
<tr>
  <td>spi0<br/>group0</td>
  <td>引脚31</td>
  <td>引脚30</td>
  <td>引脚32</td>
  <td>引脚33</td>
</tr>
<tr>
  <td>spi0<br/>group1</td>
  <td>引脚52</td>
  <td>引脚53</td>
  <td>引脚50</td>
  <td>引脚51</td>
</tr>
<tr>
  <td>spi1<br/>group0</td>
  <td>引脚52</td>
  <td>引脚53</td>
  <td>引脚50</td>
  <td>引脚51</td>
</tr>
<tr>
  <td>spi1<br/>group1(EG810M_EU不支持)</td>
  <td>引脚69</td>
  <td>引脚68</td>
  <td>引脚85</td>
  <td>引脚84</td>
</tr>
<tr>
  <td>spi2</td>
  <td>引脚76</td>
  <td>引脚77</td>
  <td>引脚78</td>
  <td>引脚16</td>
</tr>
</tbody>
</table>

<table class="EG912N">
<thead>
<tr>
  <th>SPI通道</th>
  <th>CS引脚</th>
  <th>CLK引脚</th>
  <th>MOSI引脚</th>
  <th>MISO引脚</th>
</tr>
</thead>
<tbody>
<tr>
  <td>spi0</td>
  <td>引脚25</td>
  <td>引脚26</td>
  <td>引脚27</td>
  <td>引脚28</td>
</tr>
<tr>
  <td>spi1</td>
  <td>引脚5</td>
  <td>引脚4</td>
  <td>引脚6</td>
  <td>引脚7</td>
</tr>
</tbody>
</table>

<table class="UC200A">
<thead>
<tr>
  <th>SPI通道</th>
  <th>CS引脚</th>
  <th>CLK引脚</th>
  <th>MOSI引脚</th>
  <th>MISO引脚</th>
</tr>
</thead>
<tbody>
<tr>
  <td>spi0</td>
  <td>引脚37</td>
  <td>引脚40</td>
  <td>引脚38</td>
  <td>引脚39</td>
</tr>
<tr>
  <td>spi1</td>
  <td>引脚26</td>
  <td>引脚27</td>
  <td>引脚25</td>
  <td>引脚24</td>
</tr>
</tbody>
</table>

<table class="EC200A">
<thead>
<tr>
  <th>SPI通道</th>
  <th>CS引脚</th>
  <th>CLK引脚</th>
  <th>MOSI引脚</th>
  <th>MISO引脚</th>
</tr>
</thead>
<tbody>
<tr>
  <td>spi0</td>
  <td>引脚37</td>
  <td>引脚40</td>
  <td>引脚38</td>
  <td>引脚39</td>
</tr>
<tr>
  <td>spi1</td>
  <td>引脚26</td>
  <td>引脚27</td>
  <td>引脚25</td>
  <td>引脚24</td>
</tr>
</tbody>
</table>

<table class="EC200A-V1">
<thead>
<tr>
  <th>SPI通道</th>
  <th>CS引脚</th>
  <th>CLK引脚</th>
  <th>MOSI引脚</th>
  <th>MISO引脚</th>
</tr>
</thead>
<tbody>
<tr>
  <td>spi0</td>
  <td>引脚37</td>
  <td>引脚40</td>
  <td>引脚38</td>
  <td>引脚39</td>
</tr>
<tr>
  <td>spi1</td>
  <td>引脚136</td>
  <td>引脚135</td>
  <td>引脚127</td>
  <td>引脚62</td>
</tr>
</tbody>
</table>

<table class="EC600E">
<thead>
<tr>
  <th>SPI通道</th>
  <th>CS引脚</th>
  <th>CLK引脚</th>
  <th>MOSI引脚</th>
  <th>MISO引脚</th>
</tr>
</thead>
<tbody>
<tr>
  <td>spi0(EC600ECN_LE&LQ不可用)</td>
  <td>引脚65</td>
  <td>引脚67</td>
  <td>引脚66</td>
  <td>引脚64</td>
</tr>
<tr>
  <td>spi1</td>
  <td>引脚69</td>
  <td>引脚71</td>
  <td>引脚70</td>
  <td>引脚72</td>
</tr>
</tbody>
</table>

<table class="EC800E">
<thead>
<tr>
  <th>SPI通道</th>
  <th>CS引脚</th>
  <th>CLK引脚</th>
  <th>MOSI引脚</th>
  <th>MISO引脚</th>
</tr>
</thead>
<tbody>
<tr>
  <td>spi0</td>
  <td>引脚28</td>
  <td>引脚39</td>
  <td>引脚29</td>
  <td>引脚38</td>
</tr>
<tr>
  <td>spi1(EC800ECN_LE&LQ&CG不可用)</td>
  <td>引脚52</td>
  <td>引脚53</td>
  <td>引脚50</td>
  <td>引脚51</td>
</tr>
</tbody>
</table>

<table class="EC600G">
<thead>
<tr>
  <th>SPI通道</th>
  <th>CS引脚</th>
  <th>CLK引脚</th>
  <th>MOSI引脚</th>
  <th>MISO引脚</th>
</tr>
</thead>
<tbody>
<tr>
  <td>spi0</td>
  <td>引脚4</td>
  <td>引脚1</td>
  <td>引脚3</td>
  <td>引脚2</td>
</tr>
<tr>
  <td>spi1(EC800ECN_LE&LQ&CG不可用)</td>
  <td>引脚53</td>
  <td>引脚52</td>
  <td>引脚54</td>
  <td>引脚51</td>
</tr>
</tbody>
</table>

<table class="EC800G">
<thead>
<tr>
  <th>SPI通道</th>
  <th>CS引脚</th>
  <th>CLK引脚</th>
  <th>MOSI引脚</th>
  <th>MISO引脚</th>
</tr>
</thead>
<tbody>
<tr>
  <td>spi1</td>
  <td>引脚79</td>
  <td>引脚101</td>
  <td>引脚100</td>
  <td>引脚108</td>
</tr>
</tbody>
</table>

<table class="EG912U">
<thead>
<tr>
  <th>SPI通道</th>
  <th>CS引脚</th>
  <th>CLK引脚</th>
  <th>MOSI引脚</th>
  <th>MISO引脚</th>
</tr>
</thead>
<tbody>
<tr>
  <td>spi0(EG912UGLAA不可用)</td>
  <td>引脚25</td>
  <td>引脚26</td>
  <td>引脚64</td>
  <td>引脚88</td>
</tr>
<tr>
  <td>spi1</td>
  <td>引脚5</td>
  <td>引脚4</td>
  <td>引脚6</td>
  <td>引脚7</td>
</tr>
</tbody>
</table>

<table class="EC600K">
<thead>
<tr>
  <th>SPI通道</th>
  <th>CS引脚</th>
  <th>CLK引脚</th>
  <th>MOSI引脚</th>
  <th>MISO引脚</th>
</tr>
</thead>
<tbody>
<tr>
  <td>spi1</td>
  <td>引脚4</td>
  <td>引脚1</td>
  <td>引脚3</td>
  <td>引脚2</td>
</tr>
<tr>
  <td>spi2</td>
  <td>引脚58</td>
  <td>引脚61</td>
  <td>引脚59</td>
  <td>引脚60</td>
</tr>
</tbody>
</table>

<table class="EG800K">
<thead>
<tr>
  <th>SPI通道</th>
  <th>CS引脚</th>
  <th>CLK引脚</th>
  <th>MOSI引脚</th>
  <th>MISO引脚</th>
</tr>
</thead>
<tbody>
<tr>
  <td>spi0</td>
  <td>引脚31</td>
  <td>引脚30</td>
  <td>引脚32</td>
  <td>引脚33</td>
</tr>
<tr>
  <td>spi1</td>
  <td>引脚52</td>
  <td>引脚53</td>
  <td>引脚50</td>
  <td>引脚51</td>
</tr>
<tr>
  <td>spi2</td>
  <td>引脚74</td>
  <td>引脚75</td>
  <td>引脚76</td>
  <td>引脚77</td>
</tr>
</tbody>
</table>

<table class="EC800K">
<thead>
<tr>
  <th>SPI通道</th>
  <th>CS引脚</th>
  <th>CLK引脚</th>
  <th>MOSI引脚</th>
  <th>MISO引脚</th>
</tr>
</thead>
<tbody>
<tr>
  <td>spi0</td>
  <td>引脚31</td>
  <td>引脚30</td>
  <td>引脚32</td>
  <td>引脚33</td>
</tr>
<tr>
  <td>spi1</td>
  <td>引脚52</td>
  <td>引脚53</td>
  <td>引脚50</td>
  <td>引脚51</td>
</tr>
<tr>
  <td>spi2</td>
  <td>引脚74</td>
  <td>引脚75</td>
  <td>引脚76</td>
  <td>引脚77</td>
</tr>
</tbody>
</table>

<table class="EG800AK">
<thead>
<tr>
  <th>SPI通道</th>
  <th>CS引脚</th>
  <th>CLK引脚</th>
  <th>MOSI引脚</th>
  <th>MISO引脚</th>
</tr>
</thead>
<tbody>
<tr>
  <td>spi0</td>
  <td>引脚31</td>
  <td>引脚30</td>
  <td>引脚32</td>
  <td>引脚33</td>
</tr>
<tr>
  <td>spi1</td>
  <td>引脚52</td>
  <td>引脚53</td>
  <td>引脚50</td>
  <td>引脚51</td>
</tr>
<tr>
  <td>spi2</td>
  <td>引脚74</td>
  <td>引脚75</td>
  <td>引脚76</td>
  <td>引脚77</td>
</tr>
</tbody>
</table>

<table class="EG800Z">
<thead>
<tr>
  <th>SPI通道</th>
  <th>CS引脚</th>
  <th>CLK引脚</th>
  <th>MOSI引脚</th>
  <th>MISO引脚</th>
</tr>
</thead>
<tbody>
<tr>
  <td>spi0</td>
  <td>引脚66</td>
  <td>引脚29</td>
  <td>引脚67</td>
  <td>引脚28</td>
</tr>
<tr>
  <td>spi1</td>
  <td>引脚64</td>
  <td>引脚49</td>
  <td>引脚63</td>
  <td>引脚62</td>
</tr>
</tbody>
</table>

<table class="FCM360W">
<thead>
<tr>
  <th>SPI通道</th>
  <th>CS引脚</th>
  <th>CLK引脚</th>
  <th>MOSI引脚</th>
  <th>MISO引脚</th>
</tr>
</thead>
<tbody>
<tr>
  <td>spi0</td>
  <td>引脚21</td>
  <td>引脚22</td>
  <td>引脚29</td>
  <td>引脚23</td>
</tr>
</tbody>
</table>

<table class="FCM362K">
<thead>
<tr>
  <th>SPI通道</th>
  <th>CS引脚</th>
  <th>CLK引脚</th>
  <th>MOSI引脚</th>
  <th>MISO引脚</th>
</tr>
</thead>
<tbody>
<tr>
  <td>spi0</td>
  <td>引脚29</td>
  <td>引脚30</td>
  <td>引脚37</td>
  <td>引脚31</td>
</tr>
</tbody>
</table>

<table class="BC32">
<thead>
<tr>
  <th>SPI通道</th>
  <th>CS引脚</th>
  <th>CLK引脚</th>
  <th>MOSI引脚</th>
  <th>MISO引脚</th>
</tr>
</thead>
<tbody>
<tr>
  <td>spi0</td>
  <td>引脚41</td>
  <td>引脚42</td>
  <td>引脚43</td>
  <td>引脚44</td>
</tr>
</tbody>
</table>

<table class="BC92">
<thead>
<tr>
  <th>SPI通道</th>
  <th>CS引脚</th>
  <th>CLK引脚</th>
  <th>MOSI引脚</th>
  <th>MISO引脚</th>
</tr>
</thead>
<tbody>
<tr>
  <td>spi0</td>
  <td>引脚41</td>
  <td>引脚42</td>
  <td>引脚43</td>
  <td>引脚44</td>
</tr>
</tbody>
</table>

<table class="EG915N">
<thead>
<tr>
  <th>SPI通道</th>
  <th>CS引脚</th>
  <th>CLK引脚</th>
  <th>MOSI引脚</th>
  <th>MISO引脚</th>
</tr>
</thead>
<tbody>
<tr>
  <td>spi0</td>
  <td>引脚5</td>
  <td>引脚4</td>
  <td>引脚6</td>
  <td>引脚7</td>
</tr>
</tbody>
</table>

<table class="EC800Z">
<thead>
<tr>
  <th>SPI通道</th>
  <th>CS引脚</th>
  <th>CLK引脚</th>
  <th>MOSI引脚</th>
  <th>MISO引脚</th>
</tr>
</thead>
<tbody>
<tr>
  <td>spi0</td>
  <td>引脚49</td>
  <td>引脚29</td>
  <td>引脚101</td>
  <td>引脚28</td>
</tr>
<tr>
  <td>spi1</td>
  <td>引脚52</td>
  <td>引脚53</td>
  <td>引脚50</td>
  <td>引脚51</td>
</tr>
</tbody>
</table>

<table class="BG950S">
<thead>
<tr>
  <th>SPI通道</th>
  <th>CS引脚</th>
  <th>CLK引脚</th>
  <th>MOSI引脚</th>
  <th>MISO引脚</th>
</tr>
</thead>
<tbody>
<tr>
  <td>spi0</td>
  <td>引脚86</td>
  <td>引脚85</td>
  <td>引脚88</td>
  <td>引脚87</td>
</tr>
</tbody>
</table>



</div>

## 方法

### `SPI.read`

```python
SPI.read(recv_data, datalen)
```

该方法用于读取数据。

**参数描述：**

- `recv_data` - 接收读取数据的数组，bytearray类型。
- `datalen` - 读取数据的长度，int类型。

**返回值描述：**

成功返回整型值`0`，失败返回整型值`-1`。

### `SPI.write`

```python
SPI.write(data, datalen)
```

该方法用于写入数据。

**参数描述：**

- `data` - 写入的数据，bytes类型。
- `datalen` - 写入的数据长度，int类型。

**返回值描述：**

成功返回整型值`0`，失败返回整型值`-1`。

### `SPI.write_read`

```python
SPI.write_read(r_data, data, datalen)
```

该方法用于写入和读取数据。

**参数描述：**

- `r_data  ` - 接收读取数据的数组，bytearray类型。
- `data` - 发送的数据，bytes类型。
- `datalen` - 读取数据的长度，int类型。

**返回值描述：**

成功返回整型值`0`，失败返回整型值`-1`。

### `SPI.close`

```python
SPI.close()
```

该方法用于关闭spi。

**返回值描述：**

成功返回整型值`0`，失败返回整型值`-1`。

**使用示例：**

> 需要配合外设使用！

```python
import log
from machine import SPI
import utime

spi_obj = SPI(0, 0, 1)

# 设置日志输出级别
log.basicConfig(level=log.INFO)
spi_log = log.getLogger("SPI")

if __name__ == '__main__':
    r_data = bytearray(5)  # 创建接收数据的buff
    data = b"world"  # 测试数据

    ret = spi_obj.write_read(r_data, data, 5)  # 写入数据并接收
    spi_log.info(r_data)

```
