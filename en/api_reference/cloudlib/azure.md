# Azure - Azure IoT Core Connection

A client for connecting to Azure IoT Hub, managing MQTT connections, device twins, direct methods, and authentication using SAS tokens and x.509 cerfificates.

## Constructor

### `Azure`
```python
class Azure(client_id, server, port, keep_alive=60, user=None, password=None, ssl=False, ssl_params=None)
```
**Parameters:**

- `client_id` (str) – Unique identifier for the device.
- `server` (str) – Azure IoT Hub hostname (e.g. `your-hub.azure-devices.net`).
- `port` (int) – MQTT port (usually 8883 for TLS).
- `keep_alive` (int) – Keep-alive interval in seconds (default: 60).
- `user` (str) – MQTT username (format: `hostname/deviceId/?api-version=2020-09-30`).
- `password` (str) – SAS token for authentication.
- `ssl` (bool) – Whether to use SSL/TLS (default: False).
- `ssl_params` (dict) – SSL parameters for secure connection.

**Example**
```python
>>> import azure
>>> az = azure.Azure(client_id, server, port, keep_alive=60, user=username, password=sas_token, ssl=True, ssl_params={"cert": cert, "key": key})
```

---

## Authentication

### `azure.generate_sas_token`
```python
generate_sas_token(uri, key, policy_name, expiry=3600)
```
Generates a Shared Access Signature (SAS) token.

**Parameters:**

- `uri` - Uniform Resource Identifier (URI) that specifies the target device in the Azure IoT Hub. It should follow the format: `"{}/devices/{}".format(server, client_id)`, where `server` is the IoT Hub hostname and `client_id` is the unique device identifier. For example: `"your-hub.azure-devices.net/devices/myDeviceId"`.
- `key` - Shared access key.
- `policy_name` - name of the policy you are using. If you dont use any then the policy_name value is None.

**Return Value:**  

A valid SAS token string.


## MQTT Communication

### `azure.connect`
```python
connect()
```
Establishes a connection to Azure IoT Hub.

**Return Value:** 

None

### `azure.disconnect`
```python
disconnect()
```
Disconnects from the IoT Hub server.

**Return Value:** 

None

### `azure.subscribe`
```python
subscribe(topic, qos=0)
```
Subscribes to a given MQTT topic.

**Parameters:**

- `topic` (str) - Subscribe topic. Azure IoT Hub has reserved topics for subscribing and publishing.
- `qos` (str) - QoS 0 or 1. IoT Hub supports up to QoS 1 and will downgrade QoS 2 subscriptions to QoS 1.

**Return Value:** 

None

### `azure.publish`
```python
publish(topic, payload, qos=0)
```
Publishes a message to the specified topic.

**Parameters:**

- `topic` (str) - Publish topic.
- `qos` (str) - QoS levels 0 or 1.

**Return Value:** 

None

### `azure.set_callback`
```python
set_callback(topic_name, callback)
```
Sets a callback for incoming messages on a topic.

**Parameters:**

- `topic_name` (str) - name of the topic you want to set callback for
- `callback` (function) - The function to be executed when a message is received. Prototype:

        callback_function(msg)        
    - Callback function parameter:
    
        - `msg`: dictionary type message. Already converted from JSON string to dictionary format.

**Return Value:** 

None

### `azure.loop`
```python
loop()
```
Handles incoming MQTT messages (non-blocking loop).

**Return Value:** 

None

### `azure.start`
```python
start()
```
Starts the MQTT client's internal loop.

**Return Value:** 

None

---

## Device Twins Management

### `azure.retrieve_twin`
```python
retrieve_twin(request_id=None, qos=0)
```
Retrieves the current device twin from Azure IoT Hub. It will print twin json file.

**Parameters:**

- `request_id` (str) - unique identifier for tracking the request. 
- `qos` (str) - level 0 or 1.

**Return Value:** 

None. 

### `azure.update_twin`
```python
update_twin(payload, request_id=None, qos=0)
```
Sends a reported properties update on this topic: $iothub/twin/PATCH/properties/reported/?$rid={request_id}. 

**Parameters:**

- `payload`- json payload.
- `request_id` (str) - unique identifier for tracking the request.
- `qos` (str) - level 0 or 1.

**Return Value:** 

None

### `azure.subscribe_to_desired_updates`

```python
subscribe_to_desired_updates(qos=0)
```
Subscribes to desired property updates from the cloud. The device should subscribe to receive real-time updates when the Desired Properties are changed from the cloud to this topic: $iothub/twin/PATCH/properties/desired/#

**Parameters:**

- `qos` (str) - level 0 or 1.

**Return Value:** 

None

---
## Direct Methods

### `azure.init_direct_method_handler`
```python
init_direct_method_handler(method_handler)
```
Initializes a direct method handler for incoming method calls.

**Parameters:**

- `method_handler` - callback function which will be invoked after recieving direct method message.

**Return Value:** 

None

### `azure.send_method_response`
```python
send_method_response(status, message, request_id)
```
Sends a response to a direct method invocation. When you recieve direct method message, you have to respond in pre-defined time period.

**Parameters:**

- `status` - 200 if method is recognized; 400 if method is not recognized.
- `message` - response string.
- `request_id` - unique identifier.

**Return Value:** 

None

---

## Example code

*Using SAS token authentication.

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