# 驱动准备

##  <font color='red'>**驱动下载**</font>

驱动程序（device driver）全称为“设备驱动程序”，是一种可以使计算机和设备通信的特殊程序，操作系统只能通过这个接口，才能控制硬件设备的工作。

- 打开[QuecPython官网驱动下载链接](https://developer.quectel.com/resource-download?cid=5)
- 进入页面后先选择驱动栏，然后在驱动栏选择与自己模组型号和电脑系统匹配的驱动，不同平台的模组所需要的驱动程序不一致，需要根据模组型号去下载对应的驱动包
- 模组型号可以通过模组上的镭雕获取,也可以通过AT指令获取，镭雕如下图所示，镭雕上型号为EG912U

<img src="media/EG912u-GL.png" style="zoom: 80%;" />

-  左侧筛选栏选择`EG912U`，可以筛选出该型号的USB驱动[QuecPython_USB Driver Win10_U_G](https://images.quectel.com/python/2023/04/Quectel_Windows_USB_DriverU_V1.0.19.zip))点击下载按钮下载驱动:

<img src="media/driver_eg912u.png" style="zoom: 80%;" />

| 模组型号                                                     | 操作系统                                              | 驱动名称                                                     |
| ------------------------------------------------------------ | ----------------------------------------------------- | ------------------------------------------------------------ |
| [BG95](https://developer.quectel.com/modules-cat/bg95-series) &nbsp;&nbsp;&nbsp;&nbsp; [BG600L](https://developer.quectel.com/modules-cat/bg600l-series) | &nbsp;&nbsp;&nbsp;&nbsp;Win10&nbsp;&nbsp;&nbsp;&nbsp; | &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**QuecPython_USB_Driver_Win10_BG**](https://developer.quectel.com/wp-content/uploads/2024/09/Quectel_Windows_USB_DriverQ_NDIS_V2.7.14.zip) |
| [EC800E](https://developer.quectel.com/modules-cat/ec800e-series) &nbsp;&nbsp;&nbsp;&nbsp;[EC600E](https://developer.quectel.com/modules-cat/ec600e-series) &nbsp;&nbsp;&nbsp;&nbsp; [EC800Z](https://developer.quectel.com/modules-cat/ec800z-series) | &nbsp;&nbsp;&nbsp;&nbsp;Win10&nbsp;&nbsp;&nbsp;&nbsp; | &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**QuecPython_USB_Driver_Win10_E**](https://developer.quectel.com/wp-content/uploads/2024/09/Quectel_Windows_USB_DriverY_V1.0.2.zip) |
| [EG912N](https://developer.quectel.com/en/modules-cat/eg912n-series)&nbsp;&nbsp;&nbsp;&nbsp;[EG915N](https://developer.quectel.com/en/modules-cat/eg915n-series) &nbsp;&nbsp;&nbsp;&nbsp;[UC200A](https://developer.quectel.com/modules-cat/uc200a-series)<br/>[EC600K](https://developer.quectel.com/modules-cat/ec600k-series) &nbsp;&nbsp;&nbsp;&nbsp;[EG810M](https://developer.quectel.com/modules-cat/eg810m-series) &nbsp;&nbsp;&nbsp;&nbsp;[EC800K](https://developer.quectel.com/modules-cat/ec800k-series) &nbsp;&nbsp;&nbsp;&nbsp;[EG800K](https://developer.quectel.com/modules-cat/eg800k-series) &nbsp;&nbsp;&nbsp;&nbsp;<br/>[EC200N](https://developer.quectel.com/modules-cat/ec200n-series) &nbsp;&nbsp;&nbsp;&nbsp;[EC800M](https://developer.quectel.com/modules-cat/ec800m-series) &nbsp;&nbsp;&nbsp;&nbsp;[EC600N](https://developer.quectel.com/modules-cat/ec600n-series) &nbsp;&nbsp;&nbsp;&nbsp;[EC600M](https://developer.quectel.com/modules-cat/ec600m-series) &nbsp;&nbsp;&nbsp;&nbsp;[EC200A](https://developer.quectel.com/modules-cat/ec200a-series) | &nbsp;&nbsp;&nbsp;&nbsp;Win10&nbsp;&nbsp;&nbsp;&nbsp; | &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**QuecPython_USB_Driver_Win10_ASR**](https://developer.quectel.com/wp-content/uploads/2024/09/Quectel_Windows_USB_DriverA_Customer_V1.1.13.zip) |
| [EG915U](https://developer.quectel.com/en/modules-cat/eg915u-series)&nbsp;&nbsp;&nbsp;&nbsp;[EC200U](https://developer.quectel.com/modules-cat/ec200u-series)&nbsp;&nbsp;&nbsp;&nbsp;[EC800G](https://developer.quectel.com/modules-cat/ec800g-series)<br/>[EC600U](https://developer.quectel.com/modules-cat/ec600u-series) &nbsp;&nbsp;&nbsp;&nbsp;[EC600G](https://developer.quectel.com/modules-cat/ec600g-series) &nbsp;&nbsp;&nbsp;&nbsp;[EC800G](https://developer.quectel.com/modules-cat/ec800g-series) | &nbsp;&nbsp;&nbsp;&nbsp;Win10&nbsp;&nbsp;&nbsp;&nbsp; | &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**QuecPython_USB_Driver_Win10_U_G**](https://developer.quectel.com/wp-content/uploads/2024/09/Quectel_Windows_USB_DriverU_V1.0.19.zip) |

## <font color='red'>**驱动安装**</font>

安装前请检查设备是否开机，如果设备管理器中没有任何新增端口，说明设备没有开机，可尝试长按PWRKEY开机。

下载后解压驱动压缩包，找到**"setup.exe"** 或者是**"setup.bat"**，双击运行即可，安装完之后打开设备管理器就可以看到设备管理器中端口的黄色感叹号消失了，说明安装成功，能够正常通信。

*出现 <font color='red'>Mobile ECM Network Adapter </font>或 <font color='red'>CDC Ethernet Control Modle (ECM) </font>等设备未被识别属于正常现象，不影响固件烧录和后续开发，无需理会。*

右键打开【**我的电脑**】——选择【**管理**】——选择【**设备管理器**】,然后在设备管理器中选择 【**端口**】

按照步骤打开页面后如图所示能刷新出**Quectel USB** 名称开头的串口则USB驱动安装成功

**安装驱动后:**

驱动安装常见问题参考: [开发环境与工具相关问题](https://developer.quectel.com/doc/quecpython/FAQ/zh/tools/index.html)

<img src="media/driver_after.jpg"  style="zoom: 130%;"/>
