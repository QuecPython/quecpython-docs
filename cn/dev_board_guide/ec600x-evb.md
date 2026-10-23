# EC600X 开发板介绍

## 快速开始

支持的模组列表

- [EC600N-CN](https://developer.quectel.com/modules-cat/ec600n-series)

- [EC600U-CN](https://developer.quectel.com/modules-cat/ec600u-series)

- [EC600M-CN](https://developer.quectel.com/modules-cat/ec600m-series)

## 功能列表

### 基本概述

QuecPython_EC600X_EVB 开发板是专门针对 QuecPython 制造的、小巧便携的“口袋型”开发板。其体型虽小，但是功能丰富，拥有温湿度传感器、SIM卡座、LCD接口、光敏电阻、MIC等元件。

开发板当前以EC600U作为主控模组，搭载Type-C接口，开发者仅需一条USB Type-C 数据线即可轻松玩转开发板。

### 功能说明

开发板的主要组件、接口布局见下图

![](./media/ec600x-evb/600Ufunction.png)

## 资料下载

- [EC600X_EVB_SCH](https://developer.quectel.com/wp-content/uploads/2024/09/EC600X_EVB_V3.2-SCH.pdf)

## 开发板资源

### 开发板接口

**J5排针管脚分配表**

| 排针 | 编号 | 名称 | 引脚 | 功能              |
| ---- | ---- | ---- | ---- | ----------------- |
| J5   | 1    | GND  | -    | 接地              |
| J5   | 2    | GND  | -    | 接地              |
| J5   | 3    | P60  | 60   | GPIO4，SPI1 MISO  |
| J5   | 4    | P59  | 59   | GPIO30，SPI1 MOSI |
| J5   | 5    | P58  | 58   | GPIO2，SPI1 CS    |
| J5   | 6    | TX0  | 71   | UART0             |
| J5   | 7    | RX0  | 72   | UART0             |
| J5   | 8    | TX2  | 32   | UART2             |
| J5   | 9    | RX2  | 31   | UART2             |
| J5   | 10   | SDA  | 56   | GPIO16，I2C1      |
| J5   | 11   | SCL  | 57   | GPIO15，I2C1      |
| J5   | 12   | CLK  | 1    | GPIO10，SPI0 CLK  |
| J5   | 13   | MISO | 2    | GPIO13，SPI0 MISO |
| J5   | 14   | MOSI | 3    | GPIO12，SPI0 MOSI |
| J5   | 15   | CS   | 4    | GPIO11，SPI0 CS   |
| J5   | 16   | RX1  | 123  | GPIO7，UART1      |
| J5   | 17   | TX1  | 124  | GPIO19，UART1     |
| J5   | 18   | GND  | -    | 接地              |

**J6排针管脚分配表**

| 排针 | 编号 | 名称  | 引脚   | 功能            |
| ---- | ---- | ----- | ------ | --------------- |
| J6   | 1    | 5V    | -      | 5V              |
| J6   | 2    | 3V3   | -      | 3.3V            |
| J6   | 3    | 1V8   | 76     | 1.8V            |
| J6   | 4    | ADC0  | 19     | ADC0            |
| J6   | 5    | ADC1  | 20     | ADC1            |
| J6   | 6    | ADC2  | 113    | ADC2            |
| J6   | 7    | ADC3  | 114    | ADC3            |
| J6   | 8    | PWM3  | 69     | GPIO5           |
| J6   | 9    | PWM2  | 70     | GPIO6，PWM0     |
| J6   | 10   | P40   | 40     | GPIO24          |
| J6   | 11   | P39   | 39     | GPIO23          |
| J6   | 12   | P61   | 61     | GPIO1，SPI1 CLK |
| J6   | 13   | P48   | 48     | GPIO22          |
| J6   | 14   | LSPKN | 110    | 喇叭通道        |
| J6   | 15   | LSPKP | 109    | 喇叭通道        |
| J6   | 16   | VRTC  | 115    | 实时时钟电源    |
| J6   | 17   | GND   | -      | 接地            |
| J6   | 18   | VBAT  | 36、37 | 3.8V            |



开发板主要管脚布局见下图

![](./media/ec600x-evb/600Uperipheral.png)

> **小提示**
> 开发板的更多资料，请访问 <https://developer.quectel.com/resource-download?cid=23>

### 开发板配置

开发板配备了多种传感器，以及其他外设。外设资源管脚分配表明细如下：

| 序号 | 名称                         | 型号          | 是否支持 | 接口类型 | 引脚  |
| ---- | ---------------------------- | ------------- | -------- | -------- | ----- |
| 1    | 温湿度传感器                 | AHT20         | 是       | I2C      | 56,57 |
| 2    | 光敏电阻                     | GT36528       | 是       | ADC      | 19    |
| 3    | 麦克风                       | GMI6050P-66DB | 是       | SPK      | 23,24 |
| 4    | 功放芯片                     | NS4160        | 是       | SPK      | 21,22 |
| 5    | LCD 显示屏（需选择含屏套餐） | ST7789        | 是       | SPI      | 63~67 |

## 快速入门

### 硬件准备

以下是开发板上手之后的硬件准备步骤

- <font color='red'>**Step1：硬件准备**</font>

    开始之前需要准备以下配件

    1. **开发板**
    2. **USB 数据线** (USB-A TO USB-TypeC)
    3. **PC** (Windows10)
    4. **SIM 卡**
    5. **4G 天线**

- <font color='red'>**Step2：天线&SIM卡安装**</font>

安装开发板配套的天线,安装位置为LTE天线座位置,并将SIM卡插入开发板上的SIM卡座。

- <font color='red'>**Step3：开发板连接**</font>

使用 USB Type-C 数据线连接开发板的 Type-C 和电脑 USB 口即可完成供电。

- <font color='red'>**Step4: 开发板开机**</font>

开发板上USB和DC的电源选择开关拨到USB处

长按PWRKEY开机，在按下PWRKEY按键之前，需保证供电电压稳定。建议从上电到按下PWRKEY按键之间的时间间隔不少于30 ms。开机后开发板NET灯闪烁，开发板已正常开机。

### 驱动安装

- 打开[QuecPython官网驱动下载链接](https://developer.quectel.com/resource-download?cid=5)

- 选择与模组型号匹配的驱动，该开发板搭载的模组有EC600U，以EC600U为例，在下载页面右侧选择该型号的USB驱动[QuecPython_USB Driver Win10_U_G](https://developer.quectel.com/wp-content/uploads/2024/09/Quectel_Windows_USB_DriverU_V1.0.19.zip)点击下载，如果开发板搭载的是其他型号同样按照以上操作即可。

- 下载后解压驱动压缩包，找到**"setup.exe"** 或者是**"setup.bat"**，双击运行即可，安装完之后打开设备管理器就可以看到设备管理器中端口的黄色感叹号消失了，说明安装成功，能够正常通信。

>  出现 <font color='red'>Mobile ECM Network Adapter </font>或 <font color='red'>CDC Ethernet Control Modle (ECM) </font>等设备未被识别属于正常现象，不影响固件烧录和后续开发，无需理会。

- 右键打开【**我的电脑**】——选择【**管理**】——选择【**设备管理器**】,然后在设备管理器中选择 【**端口**】，按照步骤打开页面后如图所示能刷新出**Quectel USB** 名称开头的串口则USB驱动安装成功。


### 工具安装

- QPYcom获取

使用QuecPython进行开发需要用到专用的开发调试工具——**QPYcom**，包括但不限于调试代码、分析日志、文件传输、烧录固件、合并固件等。

打开[QuecPython工具下载页面](https://developer.quectel.com/resource-download?cid=8)，根据操作系统选择对应的工具进行下载。

*注意:该工具无需安装，解压即用。*

- VSCode插件

编辑Python代码一般会用到专用于Python或者兼容多种语言的IDE，可以有效提升开发效率，这里推荐VSCode

针对VScode，QuecPython推出专用插件实现代码提示、代码补全和串口调试等功能，安装方法见下图

在VSCode中点击侧边栏插件市场，在插件市场中搜索 “QuecPython”，根据搜索结果下载该插件即可

![](media/readme/VScode_1.png)

### 固件烧录

**固件获取**

模组在出厂时通常烧录有标准 AT 固件或 QuecOpen 固件，如需基于 QuecPython 对模块进行开发，需要手动为其重新烧录专门的 QuecPython 固件。

[官网固件下载地址](https://developer.quectel.com/resource-download?cid=15)

![](media/readme/Firmware.png)

面对官网种类众多的固件，如何选择合适的固件包，首先需要知道使用的模组的型号，模组型号可以通过模组的镭雕或者发送AT指令来获得。

在获取到模组型号之后根据模组的型号去官网下载该模组对应的固件即可,固件的名称一般为`QPY_OCPU_版本号_模组型号_FW`，没有特殊需求默认下载最新版本即可。

点击下载列的下载按钮即可下载，下载成功后会得到一个固件名称命名的压缩包文件(zip包)

从官网下载的固件包为压缩包格式，固件压缩包下载到本地后，需进行解压。解压后可获得两个文件，其中 .bin 格式的是 QuecPython 固件本体，.md 格式的是更新日志。

![](media/readme/firmware_zip.png)

> 请务必将压缩包内容解压至一个**不包含中文、空格和其他特殊字符**的路径下，否则下载工具可能无法正常识别到固件，同时下载工具路径也**不可包含中文、空格和其他特殊字符**。

**固件烧录**

- <font color='red'>**Step1：创建项目**</font>

首先确保模组连接正常并已开机，打开QPYcom工具进入下载页面，点击**"创建"**项目，新建要下载的固件项目，项目名称自定义，配置完成后持久化保存

- <font color='red'>**Step2：选择固件**</font>

选择要下载到模组的固件(根据要下载的模组型号选择对应的固件，若为新格式固件则所有模组型号都选择.bin文件即可）

如果是老版本固件，固件文件格式不一定以bin结尾，其中EC600U系列模组选择解压后的pac文件

- <font color='red'>**Step3：设置下载模式**</font>

单击**“Download script”**右侧的下拉选择箭头，选择**"Download FW"**

- <font color='red'>**Step4：开始烧录固件**</font>

点击**"Download FW"**后，开始下载固件,下载过程会有进度条和进度百分比显示，等待下载完毕会有弹窗提示下载成功

![](media/readme/firmware.gif)

### REPL调试

REPL全称为<font color='red'>**Read-Eval-Print-Loop (交互式解释器)**</font>，可以在REPL中进行QuecPython程序的调试

运行 **QPYcom** 工具后，选择正确的串口（波特率无需指定）并打开，即可开始 Python 命令行交互。

- <font color='red'>**Step1：进入交互页面**</font>

打开QPYcom工具，端口选择连接**Quectel USB REPL Port**，选择“交互”界面

- <font color='red'>**Step2：打开串口**</font>

点击“打开串口”按钮，在交互界面输入**print(‘hello world’)**，按回车后可以看到执行的结果信息

```python
>>> print('hello world')
hello world
```

![](media/readme/hello_world.png)

<font color='red'>*注意：工具交互页面输入时需要输入英文字符，中文字符将会被屏蔽*</font>

## 开发第一个脚本

### 编写第一个脚本文件

创建*helloworld.py*文件输出“hello world”，打印“hello world”，编写脚本如下所示：

```python
print("hello world")
```

通过QPYcom将上面编辑好的文件下载到模组中去并运行

![](media/readme/QPYcom_hello.jpg)

### PC与模组间的文件传输

**下载方法一:**

<img src="media/readme/QPYcom_drag.jpg" alt="image-2021081301" style="zoom:150%;" />

- <font color='red'>**Step1：打开串口**</font>

首先选择模组的交互口,点击"打开串口"按钮

- <font color='red'>**Step2：通过工具按钮下载**</font>

可以通过文件页面右侧上面的 "**+**","**-**" 按钮来上传和删除文件

- <font color='red'>**Step3：通过拖拽形式下载**</font>

也可以通过拖拽的方式将文件页面左侧显示的本地文件直接拖拽到右侧模组中去（也可以拖拽文件夹）

- <font color='red'>**Step4：下载进度和结果**</font>

下载过程中会在状态栏显示下载文件名和下载进度

**下载方法二:**

<img src="media/readme/QPYcom_sc_1.jpg" alt="image-2021081301" style="zoom:150%;" />

- <font color='red'>**Step1：创建项目**</font>

根据需求，创建用户项目（点击"创建"按钮）,步骤同上文烧录固件

- <font color='red'>**Step2：配置要下载的文件**</font>

选择需要下载到模块的用户脚本(在"用户脚本"区域通过右键菜单添加)

- <font color='red'>**Step3：设置下载模式**</font>

左击下拉选择箭头，选择"下载脚本"，即"Download Script"

- <font color='red'>**Step4：开始下载脚本**</font>

点击"下载脚本"开始下载脚本，下载过程中有进度条提示

### 运行脚本文件

- <font color='red'>**Step1：下载脚本**</font>

将要执行的脚本文件下载到模组中去,具体步骤参考上文

- <font color='red'>**Step2：通过repl执行脚本**</font>

打开串口之后，在QPYcom交互页面输入以下代码执行脚本文件

```python
import example
example.exec("/usr/helloworld.py") # filePath为要执行的脚本文件路径
```

- <font color='red'>**Step3：通过GUI工具执行脚本**</font>

或者通过QPYcom文件页面 **执行** 按钮执行脚本文件,在工具的文件页面选择要执行的脚本文件然后点击 "**▷**"按钮

执行结果如图

![](media/readme/QPYcom_repl_hello.png)

### 停止程序运行

如何停止正在运行的程序,根据运行的脚本文件类型有以下方法:

<table valign="center">
	<thead>
    <tr>
		<th>程序名是否</br>为main.py</th>
		<th>程序中是否</br>包含了死循环</th>
		<th>程序中是否</br>使用了多线程</th>
		<th valign="middle">停止步骤</th>
	</tr>
	 </thead>
    <tr>
		<td rowspan="4">✓</td>
		<td rowspan="2">✓</td>
		<td rowspan="1">✓</td>
		<td>（1）按 Ctrl + A 键进入 RAW 模式</br>（2）按 Ctrl + D 键重启 QuecPython 虚拟机</br>（3）按 Ctrl + B 键回到普通交互模式</br>（4）若以上方法无效，请重新烧录固件</td>
	</tr>
	<tr>
		<td rowspan="1">✗</td>
		<td>（1）按 Ctrl + C 键打断程序运行</br>（2）若以上方法无效，请重新烧录固件</td>
	</tr>
	<tr>
		<td rowspan="2">✗</td>
		<td rowspan="1">✓</td>
		<td>（1）按 Ctrl + A 键进入 RAW 模式</br>（2）按 Ctrl + D 键重启 QuecPython 虚拟机</br>（3）按 Ctrl + B 键回到普通交互模式</br>（4）若以上方法无效，请耐心等待程序运行结束</td>
	</tr>
	<tr>
		<td rowspan="1">✗</td>
		<td>（1）按 Ctrl + C 键打断程序运行</br>（2）若以上方法无效，请重新烧录固件</td>
	</tr>
	<tr>
		<td rowspan="4">✗</td>
		<td rowspan="2">✓</td>
		<td rowspan="1">✓</td>
		<td>（1）按 Ctrl + D 键重启 QuecPython 虚拟机</br>（2）若以上方法无效，请直接重启模块</td>
	</tr>
	<tr>
		<td rowspan="1">✗</td>
		<td>（1）按 Ctrl + D 键重启 QuecPython 虚拟机</br>（2）若以上方法无效，请直接重启模块</td>
	</tr>
	<tr>
		<td rowspan="2">✗</td>
		<td rowspan="1">✓</td>
		<td>（1）按 Ctrl + C 键打断程序运行</br>（2）若以上方法无效，请重新烧录固件或直接重启模块</td>
	</tr>
	<tr>
		<td rowspan="1">✗</td>
		<td>（1）按 Ctrl + C 键打断程序运行</br>（2）若以上方法无效，请重新烧录固件或直接重启模块</td>
	</tr>
</table>
