# Aws - AWS IoT Core Connection

A client for connecting to AWS IoT Core, managing MQTT connections, and handling device shadows.

## Constructor

### `Aws`
```python
class Aws(client_id,server,port,keep_alive,ssl,ssl_params)
```
**Parameters:**

- `client_id` (str) - The unique identifier for the IoT Thing.
- `server` (str) - The AWS IoT Core endpoint.
- `port` (int) - The MQTT port (default 8883 for TLS, 1883 for non-TLS).
- `keep_alive` (int) - The keep-alive interval in seconds (default: 60).
- `ssl` (bool) - Whether to use SSL/TLS (default: False. If True, encription is enabled, so we have to provide ssl_params).
- `ssl_params` (dict) - SSL parameters for secure connection.

**Example**

```python
>>> # Create Aws object
>>> import aws
>>> aws = aws.Aws(client_id, server, port, keep_alive=60,ssl=True,ssl_params={"cert": 
certificate_content,"key": private_content})
```

## **MQTT Communication**

Uses umqtt library methods to establish connection and communication with cloud.

### `aws.connect`

```python
connect()
```
This method establishes a connection to AWS IoT Core.

**Return Value:** 

None

### `aws.disconnect`

```python
disconnect()
```
This method disconnects from AWS IoT Core.

**Return Value:** 

None

### `aws.subscribe`

```python
subscribe(topic)
```
Method for mqtt subscribing to a topic. 

**Parameters:**

- `topic` (str) - The MQTT topic to subscribe to.

**Return Value:** 

None

### `aws.publish`

```python
publish(topic, payload)
```
Method for mqtt publishing to a topic.

**Parameters:**

- `topic` (str) - The MQTT topic to publish to.
- `payload` (dict) - The message payload.

**Return Value:** 

None

## **Shadow Device Management**

### `aws.create_shadow`

```python
create_shadow(shadow_name="", state="")
```
**Parameters:**

- `shadow_name` (str) - The name of the shadow (optional).
- `state` (dict) - The initial state of the shadow (optional).

**Return Value:** 

None

### `aws.update_shadow`

```python
update_shadow(shadow_name="", state="")
```
**Parameters:**

- `shadow_name` (str) - The name of the shadow.
- `state` (dict) - The new state of the shadow.

**Return Value:**

 None

### `aws.get_shadow`

```python
get_shadow(shadow_name="")
```
**Parameters:**

- `shadow_name` (str) - The name of the shadow.

**Return Value:** 

The current shadow state.

### `aws.delete_shadow`

```python
delete_shadow(shadow_name="")
```
**Parameters:**

- `shadow_name` (str) - The name of the shadow.

**Return Value:** 

None

### `aws.connect_shadow`

```python
connect_shadow(shadow_name="", topics=None)
```
**Parameters:**

- `shadow_name` (str) - The name of the shadow (optional).
- `topics` (list) - List of MQTT topics related to the shadow (optional).

**Return Value:** 

None

### `aws.set_callback`

```python
set_callback(topic_name, callback)
```
**Parameters:**

- `topic_name` (str) - The MQTT topic for which the callback is set.
- `callback` (function) - The function to be executed when a message is received. Prototype:

        callback_function(msg)        
    - Callback function parameter:
    
        - `msg`: dictionary type message. Already converted from JSON string to dictionary format.

**Return Value:** 

None

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
