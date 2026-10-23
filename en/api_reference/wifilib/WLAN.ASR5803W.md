# class ASR5803W - ASR5803W Driver

This class controls the Wi-Fi NIC: `ASR5803W`.

> Supported module models: EC200A series.

## Constructor

### `network.ASR5803W`

```python
class network.ASR5803W(mode, wlan_ldo_en_pin, wlan_reset_pin, host_wake_wlan_pin, wlan_wake_host_pin)
```

Initialize the Wi-Fi NIC information and return the Wi-Fi NIC object.

**Parameter**

- `mode` - Integer type. Wi-Fi NIC working mode. 0: AP mode. 1: STA mode (not supported currently).
- `wlan_ldo_en_pin` - Integer type. EN pin of the Wi-Fi NIC.
- `wlan_reset_pin` - Integer type. Reset pin of the Wi-Fi NIC.
- `host_wake_wlan_pin` - Integer type. Pin of module waking up the Wi-Fi NIC.
- `wlan_wake_host_pin` - Integer type. Pin of Wi-Fi NIC waking up the module.

**Example**

```python
import network
from machine import Pin

nic = network.ASR5803W(0, Pin.GPIO7, Pin.GPIO36, Pin.GPIO34, Pin.GPIO6)
```

## Method

### `ASR5803W.active`

```python
ASR5803W.active([is_active])
```

Configure/query the activation status of ASR5803W. If **is_active** is specified, configure the NIC status. If **is_active** is not specified, query the NIC status. 

**Parameter**

- `is_active` - Boolean type. 

  True: Enable the NIC and activate the NIC configuration

  False: Disable the NIC

**Return Value**

0: Successful execution

Other value: Failed execution


### `ASR5803W.isconnected`

```python
ASR5803W.isconnected()
```

Query whether there is a client connected to the Wi-Fi NIC.

**Return Value**

True: A client is connected to the Wi-Fi NIC

False: No client is connected to the Wi-Fi NIC


### `ASR5803W.status`

```python
ASR5803W.status([param])
```

Query the NIC status. **param** can be `stations/node` or `speed`. You can only query one parameter value at a time. If **param** is not specified, query the current connection status. If **param** is `stations/node` or `speed`, query the corresponding parameter value.

**Parameter**

- `stations/node` - List of devices currently connected to the client
- `speed` - Network speed of the NIC.

**Return Value** 

If **param** is not specified

- `True` - The NIC is connected.
- `False` - The NIC is not connected.

If **param** is `stations/node`
- List type. List of devices currently connected to the client. 

  - 0: No device is connected. 

  - A list containing two elements: At least one device is connected. 

    - Element 1: Integer type. The number of currently connected devices.

    - Element 2: Tuple type. Information of the currently connected devices. The information in the tuple includes the device MAC address (byte type), the device IP address (string type), and the device connection time (Integer type. Unit: second).

If **param** is `speed`

- Tuple type. The current network speed of the NIC. 
  - Element 1: Integer type. Downlink network speed. Unit: bps.
  - Element 2: Integer type. Uplink network speed. Unit: bps.


### `ASR5803W.config`

```python
ASR5803W.config('param')
ASR5803W.config(param=value)
```

Query/configure network information. 

If only the parameter name is specified, *ASR5803W.config* is used to query the current parameter value. Only one value can be queried at a time. 

If the parameter value is specified, *ASR5803W.config* is used to configure the parameter. When the return value is True, it indicates a successful setting; when the return value is False, it indicates a failed setting. Multiple values can be configured at the same time.

**Parameter**

|  Parameter   | Type  | Description |
| ---- | ---- |---------- |
| `ssid`    | `str` | Wi-Fi network name |
| `authmode`| `enum` | Authentication type |
| `security`  | `enum` | Encryption method |
| `password`  | `str` | Password |
| `protocol` | `enum` | Network protocol |
| `channel` | `enum` | Wi-Fi network channel number |
| `b40acs` | `bool` | Channel adaptation in LTE band 40 |
| `mac` | `str` | MAC address |

**Example**
Query network information:

```python
# Query SSID configuration
ASR5803W.config('ssid')
```
Configure network information:
```python
# Configure Wi-Fi name and channel
ASR5803W.config(ssid='ssid', channel=1)
```