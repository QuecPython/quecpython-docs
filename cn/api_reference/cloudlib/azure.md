# Azure - Azure IoT 平台

用于连接到 Azure IoT Hub、管理MQTT连接、设备孪生、直接方法以及使用 SAS 令牌和 x.509 证书进行身份验证的客户端。

## 初始化客户端

### `Azure`
```python
class Azure(client_id, server, port, keep_alive=60, user=None, password=None, ssl=False, ssl_params=None)
```
**参数：**

- `client_id` (str) – 设备的唯一标识符。
- `server` (str) – Azure IoT Hub 服务器域名 (e.g. `your-hub.azure-devices.net`)。
- `port` (int) – MQTT 端口 (通常 8883 作为 TLS 端口)。
- `keep_alive` (int) – 保活包心跳周期单位秒。 (默认： 60秒)。
- `user` (str) – MQTT 用户名 (格式: `hostname/deviceId/?api-version=2020-09-30`)。
- `password` (str) – SAS 认证的 token 值。
- `ssl` (bool) – 是否采用 SSL/TLS 连接。(默认： False)。
- `ssl_params` (dict) – SSL 连接参数。

**示例**

```python
>>> import azure
>>> az = azure.Azure(client_id, server, port, keep_alive=60, user=username, password=sas_token, ssl=True, ssl_params={"cert": cert, "key": key})
```

---

## 认证

### `azure.generate_sas_token`
```python
generate_sas_token(uri, key, policy_name, expiry=3600)
```
生成共享访问签名（SAS）令牌。

**参数：**

- `uri` - 统一资源标识符（URI），指定Azure IoT Hub中的目标设备。它应该遵循以下格式： `"{}/devices/{}".format(server, client_id)`, where `server` is the IoT Hub hostname and `client_id` is the unique device identifier. For example: `"your-hub.azure-devices.net/devices/myDeviceId"`。
- `key` - 共享访问密钥。
- `policy_name` - 您正在使用的策略的名称。如果不使用任何值，则 policy_name 值为None。

**返回值：**  

一个有效的 SAS 令牌字符串。


## MQTT 通信

### `azure.connect`
```python
connect()
```
建立 Azure IoT Hub 的 MQTT 连接。

**返回值：** 

None

### `azure.disconnect`
```python
disconnect()
```
断开与服务器的 MQTT 连接。

**返回值：** 

None

### `azure.subscribe`
```python
subscribe(topic, qos=0)
```
订阅主题。

**参数：**

- `topic` (str) - 订阅主题。Azure IoT Hub已为订阅和发布保留了主题。
- `qos` (str) - QoS 0或1。IoT Hub最多支持QoS 1，并将QoS 2订阅降级为QoS 1。

**返回值：** 

None

### `azure.publish`
```python
publish(topic, payload, qos=0)
```
指定主题发布消息。

**参数：**

- `topic` (str) - 发布的主题。
- `qos` (str) - QoS ，0 或 1。

**返回值：** 

None

### `azure.set_callback`
```python
set_callback(topic_name, callback)
```
为某个主题的传入消息设置回调。

**参数：**

- `topic_name` (str) - 要为其设置回调的主题的名称。
- `callback` (function) - 收到指定主题的回调函数，原型如下：

        callback_function(msg)        
    - 回调函数参数：
    
        - `msg`: 字典类型的消息。已从JSON字符串转换为字典格式。

**返回值：** 

None

### `azure.loop`
```python
loop()
```
处理传入的MQTT消息（非阻塞循环）。

**返回值：** 

None

### `azure.start`
```python
start()
```
启动MQTT客户端的主业务循环。

**返回值：** 

None

---

## 管理设备孪生

### `azure.retrieve_twin`
```python
retrieve_twin(request_id=None, qos=0)
```
从Azure IoT Hub检索当前孪生设备。它将打印 json 文件。

**参数：**

- `request_id` (str) - 用于跟踪请求的唯一标识符。
- `qos` (str) - level 0 或 1.

**返回值：** 

None. 

### `azure.update_twin`
```python
update_twin(payload, request_id=None, qos=0)
```
发送有关此主题的已报告属性更新：$iothub/twin/PATCH/properties/reported/?$rid={request_id}

**参数：**

- `payload`- json格式属性值。
- `request_id` (str) - 用于跟踪请求的唯一标识符。
- `qos` (str) - level 0 或 1.

**返回值：** 

None

### `azure.subscribe_to_desired_updates`

```python
subscribe_to_desired_updates(qos=0)
```
从云端订阅所需的属性更新。当所需属性从云端更改为此主题时，设备应订阅以接收实时更新：$iothub/twin/PATCH/properties/desired/#

**参数：**

- `qos` (str) - level 0 or 1.

**返回值：** 

None

---
## Direct Methods

### `azure.init_direct_method_handler`
```python
init_direct_method_handler(method_handler)
```
初始化传入方法调用的直接方法处理程序。

**参数：**

- `method_handler` - 回调函数，将在收到直接方法消息后调用。

**返回值：** 

None

### `azure.send_method_response`
```python
send_method_response(status, message, request_id)
```
向直接方法调用发送响应。当您收到直接方法消息时，您必须在预定义的时间段内做出响应。

**参数：**

- `status` - 如果方法被识别，则为200；如果方法未被识别，则为400。
- `message` - 响应字符串。
- `request_id` - 唯一标识。

**返回值：** 

None

---

## 示例代码

*Using SAS token authentication.*

```python
import usr.azure as azure
import modem
import ujson
from usr.config import CERT,PRIVATE_KEY,SHARED_ACCESS_KEY, PASSWORD


# device name
client_id = 'device_name'
# server address
server = 'hub_name.azure-devices.net'
# port MQTT
port = 8883

username = '{}/{}/?api-version=2021-04-12'.format(server, client_id)

uri = "{}/devices/{}".format(server, client_id)
SharedAccessKey = SHARED_ACCESS_KEY


# create azure obj
azure_obj = azure.Azure(client_id, server, port, keep_alive=60, user=username, password=PASSWORD, ssl=True, ssl_params={"cert": 
CERT, "key": PRIVATE_KEY})
print("create azure obj")

#generate token 
token = azure_obj.generate_sas_token(uri, SharedAccessKey, None)
azure_obj.mqtt_client.password = token

# connect mqtt server
print("azure connect start")
azure_obj.connect()
print("azure connect end")

'''
direct methods example
'''
#direct method handler
def handle_method(method_name, msg, request_id):
    if method_name == "turn_on_the_light":
        print("Light on!")
        azure_obj.send_method_response(200, "Light on", request_id)
    else:
        azure_obj.send_method_response(404, "Method not found", request_id)

#initializing direct method handler
azure_obj.init_direct_method_handler(handle_method)

'''
device twins example
'''
#getting the twin json payload
azure_obj.retrieve_twin()

#new payload to be added
payload = {
    "status": "active",
    "temperature": 24.5
}

#updating twin json payload
azure_obj.update_twin(payload)

#subscribing to recieve updates on desired field in device twin 
azure_obj.subscribe_to_desired_updates()

azure_obj.start()
```