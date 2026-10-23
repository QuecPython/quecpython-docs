# Aws - 亚马逊 IoT 平台

提供一个可接入亚马逊 Iot 平台的客户端，用于管理亚马逊 MQTT 连接和影子设备。

## 初始化客户端

### `Aws`
```python
class Aws(client_id,server,port,keep_alive,ssl,ssl_params)
```
**参数：**

- `client_id` (str) - 客户端唯一标识。
- `server` (str) - 亚马逊 Iot 平台服务器地址。
- `port` (int) - MQTT 服务端口（默认端口为 1883，TLS 默认端口 为 8883）。
- `keep_alive` (int) - 客户端 keep-alive 超时值，单位：秒（默认 60 秒）。
- `ssl` (bool) - 是否使能 SSL/TLS（默认：False。如果设置为True，则必须设置 `ssl_params` 参数）。
- `ssl_params` (dict) - SSL/TLS 连接参数。

**示例：**

```python
>>> # Create Aws object
>>> import aws
>>> aws = aws.Aws(client_id, server, port, keep_alive=60,ssl=True,ssl_params={"cert": certificate_content,"key": private_content})
```

## **MQTT 接入**

使用 `umqtt` 模块接口来建立 AWS 云平台连接。

### `aws.connect`

```python
connect()
```
该方法用于建立 AWS 平台连接。

**返回值：** 

`None`

### `aws.disconnect`

```python
disconnect()
```
改方法用于断开 AWS 平台连接。

**返回值：**

`None`

### `aws.subscribe`

```python
subscribe(topic) 
```
该方法用于订阅 mqtt 主题。

**参数：**

- `topic` (str) - 订阅主题。

**返回值：** 

`None`

### `aws.publish`

```python
publish(topic, payload)
```
该方法用于发布消息到对应topic。

**参数：**

- `topic` (str) - 发布主题。
- `payload` (dict) - 需要发送的数据。

**返回值：**

`None`

## **影子设备管理**

### `aws.create_shadow`

```python
create_shadow(shadow_name="", state="")
```
该方法用于创建影子设备。

**参数：**

- `shadow_name` (str) - 影子设备名称（可选）。
- `state` (dict) - 影子设备的初始状态参数（可选）。

**返回值：**

`None`

### `aws.update_shadow`

```python
update_shadow(shadow_name="", state="")
```
该方法用于更新影子设备名称和状态。

**参数：**

- `shadow_name` (str) - 影子设备名称。
- `state` (dict) -  影子设备的状态参数（可选）。

**返回值：**

 `None`

### `aws.get_shadow`

```python
get_shadow(shadow_name="")
```
该方法用于获取影子设备状态。

**参数：**

- `shadow_name` (str) - 影子设备名称。

**返回值：**

当前影子设备的状态数据。

### `aws.delete_shadow`

```python
delete_shadow(shadow_name="")
```
该方法用于删除一个影子设备。

**参数：**

- `shadow_name` (str) - 影子设备的名称。

**返回值：**

`None`

### `aws.connect_shadow`

```python
connect_shadow(shadow_name="", topics=None)
```
该方法用于连接影子设备。

**参数：**

- `shadow_name` (str) - 影子设备名称（可选）。
- `topics` (list) - 关联 影子设备的 MQTT 主题列表（可选）。

**返回值：**

`None`

### `aws.set_callback`

```python
set_callback(topic_name, callback)
```
**参数：**

- `topic_name` (str) - MQTT 主题。
- `callback` (function) - `topic_name` 主题对应的回调函数。 函数原型：

        callback_function(msg)        
    - 回调函数参数:
    
        - `msg`: 字典类型，接收到的消息。

**返回值：**

`None`

## **Python Example**
```python
import usr.aws as aws
import modem
import ujson
import sim  # Check if PIN verification is needed for your SIM card
import net

# AWS IoT credentials
certificate_content = """
-----BEGIN CERTIFICATE-----

-----END CERTIFICATE-----
"""

private_content = """
-----BEGIN RSA PRIVATE KEY-----

-----END RSA PRIVATE KEY-----
"""

client_id = 'qpthing'
server = 'abgka7vzgjoa0-ats.iot.eu-west-3.amazonaws.com'
port = 8883

def aws_callback(data):
    print("HELLO from 1234 topic callback")

def shadow_callback_get(data):
    print("HELLO from get accepted callback")

def shadow_callback_update(data):
    print("HELLO from update accepted callback")

def shadow_callback_delta(data):
    print("HELLO from update delta callback")

# Create AWS object
aws_obj = aws.Aws(client_id, server, port, keep_alive=60, ssl=True,
                  ssl_params={"cert": certificate_content, "key": private_content})
print("Created AWS object")

# Connect to AWS IoT
aws_obj.connect()
print("Connected to AWS IoT")

# Subscribe and publish
aws_obj.set_callback("1234", aws_callback)
aws_obj.subscribe("1234")
aws_obj.publish("7777", "Hello from QuecPython")
aws_obj.start()

# Shadow operations
aws_obj.create_shadow()
aws_obj.connect_shadow()
aws_obj.set_callback("$aws/things/qpthing/shadow/get/accepted", shadow_callback_get)
aws_obj.set_callback("$aws/things/qpthing/shadow/update/accepted", shadow_callback_update)
aws_obj.set_callback("$aws/things/qpthing/shadow/update/delta", shadow_callback_delta)
aws_obj.get_shadow()
aws_obj.update_shadow(state={"state": {"reported": {"welcome": "change reported"}}})
```
