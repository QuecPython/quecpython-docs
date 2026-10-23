# lwm2m-轻型物联网
## 创建对象
轻型物联网协议，提供在云平台管理设备的功能，例如检查设备在线状态，观测当前信号情况，重启设备等。

>以下型号支持此模块功能，对于定制的功能，根据实际功能为准：
>
>EC200UEU_AB_SANX/BG95M8_SANX/EC200AAU_HA_SANX/EG912NEN_AA。

### `lwm2m`

```python
lw=lwm2m()
```

创建lwm2m对象。

**参数描述：**

无参数。

**返回值描述：**

返回lwm2m对象。


## 初始化对象

### `lwm2m.init`

```python
lwm2m.init()
```

初始化对象。

**参数描述：**

无参数。

**返回值描述：**

成功返回整型数值0，失败返回整型数值-1。

## 配置对象

### `lwm2m.config`

```python
lwm2m.config(config_type,config_list)
```

配置功能模块参数。

**参数描述：**

* `config_type` -  配置类型 ，枚举型，lwm2m对象下的枚举数值，详见下表。

| 配置类型      | 对应的config_list参数                                                    | 备注                |
| ---------- | ------------------------------------------------------------------- | ----------------- |
| lwm2m.Epnamemode | [mode]                                                              |                   |
| lwm2m.Epname | [epname]                                                              |                   |
| lwm2m.Reset      | []                                                                  | 空list             |
| lwm2m.Security   | 当security_mode = 0：[serverID,SSID,server_addr,bootstrap,security_mode,psk_id,psk_key] <br/> 当security_mode = 2：[serverID,SSID,server_addr,bootstrap,security_mode,ca_cert,client_cert,client_key] <br/> 当security_mode = 3：<br/>[serverID,SSID,server_addr,bootstrap,security_mode] | 不同安全模式config_list元素不同|
| lwm2m.Server     | [serverID,life_time,pmin,pmax,disable_timeout,storing,binding_mode] |                   |
| lwm2m.Urc        | [URC_onoff]                                                         | 如果要抓取URC，必须配置开启   |
| lwm2m.Fota       | [download,update]                                                   | 如果要抓取URC，配置成均不自动 |

* `config_list` -  配置参数列表 ，列表数据类型，列表元素值详见下表。

| 字段名             | 字段类型 | 字段说明                                                                                                                                                |
| --------------- | ---- | --------------------------------------------------------------------------------------------------------------------------------------------------- |    
| serverID        | 整型   | 0 - Bootstrap server <br/>1 - DM server <br/>                                                                                                           |
| SSID            | 整型   | 100 - Bootstrap server <br/>1000 - Diagnostics server <br/>                                                                                             |
| server_addr     | 字符串  | 服务器地址，格式为："address:port"，最长支持256                                                                                                                   |
| bootstrap       | 整型   | 0 - 不用bootstrap(仅当serverID不为0)<br/>1 - 使用bootstrap (仅当serverID为0)                                                                                       |
| security_mode   | 整型   | 加密方法:<br/>0 - Pre-share key mode <br/>2 - Certificate mode <br/>3 - No security mode                                                                                              |
| psk_id          | 字符串  | security_mode为0时有用                                                                                                                                  |
| psk_key         | 字符串  | security_mode为0时有用                                                                                                                                  |
| ca_cert         | 字符串  | security_mode为2时有用，CA证书（用于校验服务端证书链）                                                                                                                                  |
| client_cert     | 字符串  | security_mode为2时有用，客户端证书                                                                                                                                  |
| client_key      | 字符串  | security_mode为2时有用，客户端私钥                                                                                                                                  |
| life_time       | 整型   | 生存时间，取值范围1~86400，默认值86400，单位秒                                                                                                                       |
| pmin            | 整型   | pmin默认周期，取值范围1~86400，默认值86400，单位秒                                                                                                                   |
| pmax            | 整型   | pmax默认周期，取值范围1~86400，默认值86400，单位秒                                                                                                                   |
| disable_timeout | 整型   | 与 LwM2M 服务器断开连接后到下一个连接的时间间隔。范围为 1-86400，默认值为 86400，单位为秒<br />实测断开后可以立刻启动连接，功能无效(上送给服务端的，服务端未限制)                                                        |
| storing         | 整型   | 是否保存服务器信息 <br/>0 - 不保存<br/>1 - 保存                                                                                                                       |
| binding_mode    | 字符串 | 和服务器连接方式，目前仅支持基于UDP的连接方式<br/>"U"     - UDP方式<br/>"UQ"  - UDP有队列模式方式<br/>"S"      - SMS方式<br/>"SQ"   - SMS有队列模式方式<br/>"US"    - UDP和SMS方式<br/>"UQS" - UDP和SMS有队列模式方式 |
| mode            | 整型   | 终端设备名字格式 <br/>  1 - 使用 lwm2m.Epname 配置的自定义名称  <br/>3 - 使用 psk_id 配置epname (通常配合 PSK 模式使用)                                                                   |
| epname          | 字符串 | 终端设备名字(Endpoint Name)                                                                    |
| download        | 整型   | 下载模式<br />0 - 手动下载<br />1 - 自动下载                                                                                                                      |
| update          | 整型   | 更新模式<br />0 - 手动更新<br />1 - 自动更新                                                                                                                        |
| URC_onoff       | 整型   | 是否开启lwm2m的urc上报<br />0 - 禁用URC上报<br />1 - 启用URC上报                                                                                                       |


**返回值描述：**

成功返回整型数值0，失败返回整型数值-1，不支持返回整型数值-2。

## 注册

### `lwm2m.register`

```python
lwm2m.register()
```

向lwm2m服务器发送登录请求，登录成功或失败结果会通过回调函数上送登录成功消息。

**参数描述：**

无参数

**返回值描述：**

执行成功返回0，执行失败返回-1。注册结果需要调用查询接口进行查询。

## 注销

### `lwm2m.unregister`

```python
lwm2m.unregister()
```

向lwm2m服务器发送退出登录请求，成功或失败结果通过回调函数进行通知。

**参数描述：**

无参数。

**返回值描述：**

执行成功返回0，执行失败返回-1。注销结果需要调用查询接口进行查询。

## 查询注册状态

### `lwm2m.stat`

```python
lwm2m.stat()
```

查询当前登录状态。

**参数描述：**

无参数。

**返回值描述：**

未注册状态返回整型值0，注册中状态返回整型值1，已注册状态返回整型值2，注销中状态返回整型值3，登录失败状态返回整型值4。

## 发送心跳

### `lwm2m.update`

```python
lwm2m.update(ssid)
```

向lwm2m服务器发送心跳包，内部会有自动心跳流程，不建议主动调用此接口。

**参数描述：**

* `ssid` -  配置的ssid或者是BS登录阶段BS下发的ssid，整型值。

**返回值描述：**

成功返回整型值0，失败返回整型值-1。update结果通过回调函数通知。

## 注册回调函数

### `lwm2m.register_call`

```python
lwm2m.register_call(usrfun)
```

注册回调函数，此回调函数会通知上层应用登录结果，以及服务器下发的消息。

**参数描述：**

* `usrfun` -  设置消息回调函数，function类型，当服务端响应时触发该方法。

**回调函数参数**

* `list` -  回调函数通知消息数据，list类型，详细介绍如下:

| 元素      | 数据类型 | 备注                                                      |
| ------- | ---- | ------------------------------------------------------- |
| list[0] | 整型   | 事件类型(可以不处理忽略)                                           |
| list[1] | 整型   | 事件编码(可以不处理忽略)                                           |
| list[2] | 字符串   | 抓取到的URC字符串数据(具体事件说明请参考[事件说明](#事件说明)，以此参数判断Lwm2m的连接事件为准) |

**返回值描述：**

成功返回0，失败返回-1。注意如果要删除已经注册的回调函数，传入参数None。

## 事件说明

事件类型不同的平台可能稍有不同，例如APN某些平台没有，不影响功能，不影响使用。

| 事件类型                                                        | 说明                                      | 备注                                                                                               |
| -------------------------------------------------------------- | --------------------------------------- | ------------------------------------------------------------------------------------------------ |
| +QLWURC: "pdp active",result,apn                               | PDP的激活结果。在向lwm2m发送注册请求之前，应激活 PDP 服务器 | result:string类型<br/>"successfully" <br/>"failed"<br />apn:string类型<br />APN值                     |
| +QLWURC: "initial",result,ssid                                 | 客户端与lwm2m服务器连接的初始化结果                   | result:string类型<br/>"successfully" <br/>"failed"<br />ssid:整型类型<br />0-所有的服务器 <br />其他值-其他的特殊服务器 |
| +QLWURC: "dtls",result,ssid                                    | 使用加密方法时的DTLS握手结果                     | 同上                                                                                               |
| +QLWURC: "bootstraping"                                        | 当 Bootstrap 工作时，将报告此 URC               |                                                                                                  |
| +QLWURC: "bootstrap",result,ssid                               | Bootstrap 的工作结果                        | 同上                                                                                               |
| +QLWURC: "registering"                                         | 当客户端在lwm2m服务器上注册时将报告此 URC            | 同上                                                                                               |
| +QLWURC: "ready",result,ssid                                   | 向lwm2m服务器发送注册请求后的注册结果                  | 同上                                                                                               |
| +QLWURC: "update",result,ssid                                  | 向lwm2m服务器发送更新请求后的更新结果                  | 同上                                                                                               |
| +QLWURC: "deregister",ssid,code<br/>+QLWURC: "deregister",code | 向lwm2m服务器发送注销请求后的注销结果                  | ssid:整型类型<br />0-所有的服务器 <br />其他值-其他的特殊服务器<br />code:整型<br /> 0 - 成功 <br /> 1 - 失败<br /> 3 - 未知错误         |
| +QLWURC: "fota/pkgurl",url                                     | lwm2m服务器下发的url地址                          |                                                                                                  |
| +QLWURC: "apn_changed",APN                                     | lwm2m服务器下发的apn值                           |                                                                                                  |
| +QLWURC: "user_name_changed",user                              | lwm2m服务器下发的用户名                         |                                                                                                  |
| +QLWURC: "secret_changed",password                             | lwm2m服务器下发的密码                      |                                                                                                  |
| +QLWURC: "authentication_type_changed",<auth>                  | lwm2m服务器下发的鉴权方式                 |                                                                                                  |
| +QLWURC: "lifetime_changed",lifetime                           | lwm2m服务器下发的存活周期（心跳周期）                       |                                                                                                  |
| +QLWURC: "current_time_changed",data_time                      | lwm2m服务器下发的时间戳          |                                                                                                  |

---

## 获取示例代码

*[点击查看示例代码](https://github.com/QuecPython/examples/tree/main/network-comm/net-protocols/lwm2m/example_lwm2m.py)*
