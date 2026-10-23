# 阿里 IoT 平台问题

### 连接阿里 IoT 平台需要哪些参数

填入三个主要参数：

一型一密参数信息：productKey、productSecret、DeviceName

一机一密参数信息：productKey、DeviceName、DeviceSecret

> 注意：
>
> - 选择一型一密，DeviceSecret填入None。
> - 选择一机一密，productSecret填入None。
>

### 一型一密和一机一密有什么区别

一型一密：同一产品下所有设备可以烧录相同产品证书（即 ProductKey 和 ProductSecret ），设备发送激					活请求时，物联网平台进行产品身份确认，认证通过，下发该设备对应的 DeviceSecret。
一机一密：每个设备烧录其唯一的设备证书（ProductKey、DeviceName 和 DeviceSecret），设备发送激				   活请求时，物联网平台进行产品身份确认，认证通过，下发该设备对应的 DeviceSecret。

### 如何批量写入三元组

使用QPY_FacTools工具把三元组信息逐步写入到模块里，使用步骤如下：

1. 制作一个execl表格存放三元组信息，例如命名为QQ.xlsx。
2. 确保交互口不要被占用。
3. 在QPY_FacTools文件目录下cmd显示终端。
4. cmd输入命令行：QPY_FacTools.exe QQ.xlsx。
5. 等待写入成功，如写入失败请咨询联系FAE。

### 除了阿里 IoT 平台，支持腾讯 IoT 平台、OneNet等其他 IoT 平台吗

还支持腾讯 IoT 平台、OneNet IoT 平台、亚马逊 IoT 平台



