# 快速上手



## 硬件准备

- Windows 电脑一台，建议 `Win10` 系统。

- 一块 [QuecPython U-235 开发板](https://developer.quectel.com/doc/quecpython/Dev_board_guide/zh/ec600u-235.html)

- 一根TypeC数据线




## 环境搭建

- 下载并安装 EC600U 系列模组驱动：[QuecPython_USB_DriverU_V1.0.19](https://developer.quectel.com/wp-content/uploads/2024/09/Quectel_Windows_USB_DriverU_V1.0.19.zip)
- 下载并安装[VSCode](https://code.visualstudio.com/)
- 下载并解压 [QPYCom](https://developer.quectel.com/wp-content/uploads/2024/09/QPYcom_V4.1.0.zip) 工具到电脑的合适位置。
- 下载[固件包](https://github.com/QuecPython/solution-Agriculture-Control-Panel/tree/master/8915DM_cat1_open_EC600UEUAB_R03A01M8_OCPU_QPY_BETA1107)，从实验源码目录EC600UEUAB_R03A01M8_OCPU_QPY_BETA1107中获取固件
- 下载[实验源码](https://codeload.github.com/QuecPython/solution-Agriculture-Control-Panel/zip/refs/tags/v1.0.1)



## 硬件连接

按照下图进行硬件连接：

<img src="../../media/solutions/Agriculture-Control-Panel/connection.png" style="zoom: 100%;display: block; margin: 0 auto;" />	

使用 Type-C 数据线连接开发板和电脑。



## 设备开发



### 开机


完成硬件连接的工作后，长按开发板上标识为`PWR`的按键，直到网络灯`net`闪烁，或电脑设备管理器的端口列表中出现包含`Quectel USB` 字样的 COM 口，表示开机成功。

![](../../media/solutions/Agriculture-Control-Panel/comport.png)	



### 烧录固件包

参考[此章节](https://developer.quectel.com/doc/quecpython/Application_guide/zh/dev-tools/QPYcom/qpycom-dw.html#下载固件)，烧录固件包 [EC600UEUAB_R03A01M8_OCPU_QPY_BETA1107.zip](https://objects.githubusercontent.com/github-production-release-asset-2e65be/718904250/839d5c8d-81fb-49d5-982f-73c45ceb5f9b?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=releaseassetproduction%2F20250220%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20250220T034410Z&X-Amz-Expires=300&X-Amz-Signature=ba55b3e1f47a0e7b842d315dc04b1e20b975a9f6a2fe76239989ffbbf24e42d5&X-Amz-SignedHeaders=host&response-content-disposition=attachment%3B%20filename%3D8915DM_cat1_open_EC600UEUAB_R03A01M8_OCPU_QPY_BETA1107_merge_20241108-1415_20250205-1611.zip&response-content-type=application%2Foctet-stream) 至开发板。



### 脚本导入与运行


1. 参考[此章节](https://developer.quectel.com/doc/quecpython/Getting_started/zh/4G/first_python.html#%E4%BC%A0%E8%BE%93%E8%84%9A%E6%9C%AC%E6%96%87%E4%BB%B6%E5%88%B0%E6%A8%A1%E7%BB%84)，将源码目录下 `code`文件夹中的所有文件导入到模组文件系统，如下图所示：

   <img src="../../media/solutions/Agriculture-Control-Panel/Downloadfile.png" style="zoom:80%;" />·

2. 参考[此章节](https://developer.quectel.com/doc/quecpython/Getting_started/zh/4G/first_python.html)，执行主程序文件`main.py`。(正常开机时，模组会自动执行`main.py`文件)

3. 参考[此章节](https://developer.quectel.com/doc/quecpython/Getting_started/zh/4G/first_python.html#停止程序运行)，停止程序运行。



## 界面介绍



### 程序启动

执行`main.py`之后，程序启动成功会进入`welcomescreen`界面,等待2秒后进入`main_screen`界面，

<img src="../../media/solutions/Agriculture-Control-Panel/UI_Welcomescreen.png" style="zoom: 105%;display: block; margin: 0 auto;" />	



### 主界面

主界面是农业管理的信息中心。其设计理念为直观易用，集成多种功能，以支持有效的农业管理。                                                                                                         

<img src="../../media/solutions/Agriculture-Control-Panel/UI_Main_interface.png" style="zoom: 105%;display: block; margin: 0 auto;" />	

组成部分：

1. **天气信息**：

   - 当前天气状况：包括温度、天气状况（晴、雨、云等），风速和风向。
   - 未来天气预报：提供未来几天或一周的天气预测，包括气温变化、降水概率等。

   点击后，即可进入完整的天气界面

2. **温度和湿度历史曲线**：

   - 实时更新的温度和湿度数据，以曲线图形式展现，方便观察短期和长期的变化趋势。
   - 可能包括日、周、月甚至年的数据对比。

3. **常见环境参数**：

   - 土壤湿度：实时显示土壤的湿度水平
   - 土壤PH值：监测土壤酸碱度。
   - O2浓度：显示当前环境氧气浓度。
   - 温度：实时显示当前室温，对温室作物生长尤其关键。

4. **报警信息**：

   - 系统实时监测并提醒任何异常情况，如温度或湿度突然偏离正常范围，未授权入侵，设备故障等。
   - 告警历史记录，方便回溯和分析过去的事件。



### 天气详细界面

点击主界面的`weather`进入详细天气界面

<img src="../../media/solutions/Agriculture-Control-Panel/UI_Weather.png" style="zoom: 100%;display: block; margin: 0 auto;" />	

直观显示当前城市的当前温度，最高温度，最低温度，城市的风速和风向，以及未来五天天气的详细信息



### 环境数据显示界面

设备状态显示界面用于监测和显示各个设备和传感器的运行状态， 以便于快速识别任何潜在问题，并采取相应措施，确保农业操作的顺畅和效率。通过这样的中控面板，农业管理者能够有效监控和维护农场的关键设备和传感器，提高整体的生产力和可靠性。

<img src="../../media/solutions/Agriculture-Control-Panel/UI_Detection_interface.png" style="zoom: 117%;display: block; margin: 0 auto;" />	

该界面包括了农业中常见的环境传感器状态。 包括温湿度、关照、土壤PH值、土壤EC、二氧化碳浓度、氧气浓度、风速风向和土壤的温湿度。



### 阈值设置界面

阈值设置界面允许用户自定义和调整各种环境和设备操作参数。这些设置帮助确保农业生产环境与作物的需求相匹配，从而优化生长条件和提高生产效率。

<img src="../../media/solutions/Agriculture-Control-Panel/UI_Parameter_Setting_Interface.png" style="zoom: 100%;display: block; margin: 0 auto;" />	

用户可以为各种环境参数设定阈值，如温度、湿度、光照度、土壤湿度、二氧化碳浓度等，当这些参数达到或超过设定阈值时，系统可以自动调整设备运行或发送警报。

通过这个界面，用户可以根据具体情况和需求精细调整农业操作的各个方面，确保系统的高效运行和作物的最佳生长条件。这种高度的可定制性是智慧农业系统的一个主要优势，它允许农业生产者根据变化的环境和市场需求灵活调整操作策略。



### 设备控制界面

设备控制界面是一个用于管理和操作农场各种设备的关键部分。其提供直观的控制选项和实时反馈，确保用户可以轻松而有效地管理农场的关键设备。通过这种智能化的设备控制，农场管理者可以优化生产条件，提高效率，同时减少资源浪费。

<img src="../../media/solutions/Agriculture-Control-Panel/UI_Device_Control_Interfacd.png" style="zoom: 130%;display: block; margin: 0 auto;" />	

该界面包括了农业中常见的设备控制。 包括通风设备、灌溉设备、加湿设备、驱虫设备、升温设备和报警设备。



### 系统设置页面

系统设置界面是用户配置和个性化其农业管理系统的关键部分，允许用户调整各种系统参数以满足特定的需求和偏好 。 通过这些设置，用户可以确保系统的高效运行，同时符合其个人偏好和操作习惯。

<img src="../../media/solutions/Agriculture-Control-Panel/UI_Local _Settings_Interface.png" style="zoom: 112%;display: block; margin: 0 auto;" />	

该界面包括如下功能：

1. 关于本机
   - 提供系统的基本信息，如制造商、型号、操作系统版本。
   - 显示系统的硬件配置，如处理器类型、内存大小、存储空间等。
2. 音量设置
   - 允许用户调整系统的音量，包括警报声、通知声等。
   - 可以设定静音模式，特别是在需要减少干扰的环境中。
3. 检查更新
   - 用于检查系统软件的更新，确保所有功能都是最新版本。
   - 可能包括自动更新的选项，允许系统在有新版本时自动下载和安装。
4. 低功耗模式
   - 这个选项可以减少系统的能源消耗，特别是在电源有限的环境中非常有用。
   - 低功耗模式可能会限制某些功能或降低系统的性能。
5. 息屏时间设置
   - 允许用户设定屏幕在无操作时自动关闭的时间，有助于节能。
   - 用户可以根据需要设置不同的时间间隔。
6. 模式选择
   - 用户可以选择不同的操作模式，例如日常模式、节能模式、夜间模式等。
   - 每种模式可能会调整屏幕亮度、音量、能源使用等设置。

### 设备状态显示页面

设备状态显示界面用于监测和显示各个设备和传感器的运行状态， 以便于快速识别任何潜在问题，并采取相应措施，确保农业操作的顺畅和效率。通过这样的中控面板，农业管理者能够有效监控和维护农场的关键设备和传感器，提高整体的生产力和可靠性

<img src="../../media/solutions/Agriculture-Control-Panel/UI_Device_Status_Interface.png" style="zoom: 130%;display: block; margin: 0 auto;" />	

该界面包括了农业中常见的环境传感器状态。 包括温湿度、关照、土壤PH值、土壤EC、二氧化碳浓度、氧气浓度、风速风向和土壤的温湿度。