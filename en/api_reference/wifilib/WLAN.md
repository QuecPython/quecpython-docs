# WLAN - Wireless Local Area Network

This class `WLAN` is primarily used for controlling the Wi-Fi network card.

> Supported module model: FCM360W

**Example:**

```python
# This example demonstrates the functionality of connecting the Wi-Fi network card to a hotspot and performing network communication.
import network
import utime
nic = network.WLAN(network.STA_MODE)
while nic.status("station_if") != 1011:
    print("Waiting for connection...")
    nic.connect(ssid="your_ssid", password="your_password")
    utime.sleep(1)
print(nic.ifconfig())

# Now use socket as usual
import usocket
addr = usocket.getaddrinfo("python.quectel.com", 80)[0][-1]
s = usocket.socket()
s.connect(addr)
s.send(b"GET / HTTP/1.1\r\nHost: python.quectel.com\r\n\r\n")
data = s.recv(1000)
print(data)
s.close()
```

## Constructor

### `network.WLAN`

```python
class network.WLAN(mode)
```

Initialize WLAN network card and return a WLAN object.

**Parameter:**

- `mode` - Wireless network card operating mode, `int` type.
  - `network.NONE_MODE` : `0` - Idle mode.
  - `network.AP_MODE` : `1` - AP (Access Point) mode, supports only wireless client connections.
  - `network.STA_MODE` : `2` - Station mode, connects to a wireless network, such as a router or mobile hotspot.
  - `network.AP_STA_MODE` : `3` - AP+Station mode, supports both wireless client connections and connection to a wireless network.

> Note:
>
> 1. Only one WLAN network card object can be created. Attempting to create more will raise an exception: `ERR_WLAN_NETIF_IS_BUSY`.
> 2. The WLAN network card has a total of four working modes as mentioned above. These four modes are actually operations on two network interfaces: the `softap interface` and the `station interface`. When the mode is `AP_STA_MODE`, both `softap` and `station` interfaces coexist. Therefore, when calling some interface-specific functions, the `interface` parameter needs to be specified to indicate the intended interface, or else it won't be clear which interface you are targeting.

**Example:**

```python
# Create Station mode and connect to a specified AP
import network
nic = network.WLAN(network.STA_MODE)
nic.connect(ssid='your-ssid', password='your-password')
nic.status("station_if")
```

## Methods

### `WLAN.mode`

```python
WLAN.mode([mode])
```

Set or query the wireless network card operating mode.

- Query the wireless network card operating mode

  When called without parameters, it queries the wireless network card operating mode.
- Set the wireless network card operating mode

  When a parameter is provided, it sets the wireless network card operating mode. After mode switching, the wireless network card is in a stopped state.

**Parameter:**

- `mode` - Wireless network card operating mode, `int` type.
  - `network.NONE_MODE` : `0` - Idle mode.
  - `network.AP_MODE` : `1` - AP (Access Point) mode, supports only wireless client connections.
  - `network.STA_MODE` : `2` - Station mode, connects to a wireless network, such as a router or mobile hotspot.
  - `network.AP_STA_MODE` : `3` - AP+Station mode, supports both wireless client connections and connection to a wireless network.

**Return:**

- Query the wireless network card operating mode

  Returns the current operating mode of the wireless network card, `int` type.
- Set the wireless network card operating mode

  Returns an error code, `int` type.

  - `ERR_OK` : `2000` - No error.
  - `ERR_WIFI_BUSY` : `2003` - Already in this operating mode.
  - `ERR_INTERNAL` : `2006` - Internal error, the underlying interface encountered an error.
  - `ERR_NOT_SUPPORT` : `2004` - This mode is not supported.

**Examples:**

```python
# Query the current mode
>>> nic.mode()
2
```

```python
# Set the current mode to AP mode
>>> nic.mode(network.AP_MODE)
2000
```

### `WLAN.config`

```python
WLAN.config('param')
WLAN.config(param=value)
```

Set or query configuration parameters.

- Query configuration parameters

  Input relevant keywords to query parameters, each time only one parameter can be queried, `str` type.
- Set configuration parameters

  When parameters are keyword arguments, it sets the parameters; multiple parameters can be set simultaneously.

**Parameter:**

- `ap_ssid` - SoftAP's wireless access point name, `str` type, 1~32 characters, default value: `'quecpython'`.
- `ap_password` - SoftAP's wireless access point password, `str` type, 0~63 characters, default value: `'quecpython'`.

  - `AUTH_OPEN` : `0` characters - No password required.
  - `AUTH_WEP` : `5` characters - Fixed length of 5.
  - `AUTH_WPA_PSK` : `8~63` characters.
  - `AUTH_WPA2_PSK` : `8~63` characters.
  - `AUTH_WPA_WPA2_PSK` : `8~63` characters.
- `ap_auth` - SoftAP's authentication method, `int` type, default value: `AUTH_WPA_WPA2_PSK`.

  - `AUTH_OPEN` : `0` - Open system authentication.
  - `AUTH_WEP` : `1` - Wired Equivalent Privacy (WEP) authentication.
  - `AUTH_WPA_PSK` : `2` - WPA Pre-Shared Key (PSK) authentication.
  - `AUTH_WPA2_PSK` : `3` - WPA2 PSK authentication.
  - `AUTH_WPA_WPA2_PSK` : `4` - Mixed mode, combining WPA and WPA2 PSK authentication.
- `ap_channel` - SoftAP's channel, `int` type. The allowable range is: `[1,13]`, default value: `3`.
- `ap_hidden` - Whether the wireless access point name of SoftAP is hidden, `bool` type, `True` for hidden, default value: `False`.
- `ap_max_clients` - Maximum number of client connections for SoftAP, `int` type, default value: `4`.
- `ap_mac` - MAC address of SoftAP, `str` type.
- `sta_ssid` - Wireless access point name to be connected by the station, `str` type, 1~32 characters.
- `sta_password` - Password of the wireless access point to be connected by the station, `str` type, 0~63 characters.
- `sta_bssid` - MAC address of the target wireless access point for the station, `str` type.
- `sta_channel` - Channel of the station, `int` type. The allowable range is: `[1,13]`.
- `sta_mac` - MAC address of the station, `str` type.
- `block` - Whether it is in blocking mode,  `bool` type, default is `True`, i.e., blocking mode. Non-blocking mode should be used with event callbacks.
- `timeout` - Connection timeout period, `int` type, unit: s, default value: `15`s.
- `country` - Country code, `str` type.
- `event_callback` - WLAN status callback function, called when WLAN status changes, callback function prototype:

  ```python
  fun(event)
  ```

  Callback function parameter description:

  - `event`: Event information, `dict` type, carrying the parameters as follows:
    - `type`: Event type, `int` type, see `Event Type` in [Event Codes](#Event-Codes) for details.
    - `id`: Specific event, `int` type, see  `Event ID` in [Event Codes](#Event-Codes) for details.
    - `msg`: Message passed to the user, see  `Message Passed to User in Event Callback` in [Event Codes](#Event-Codes) for details.

**Return Value:**

- Query configuration parameters:

  Returns the query result.
  If this interface fails, an error code will be returned.

  - `ERR_NO_AP_IF` : `2020` - Cannot obtain the current query information as the device is not in `AP` or `AP_STA` mode.
  - `ERR_NO_STA_IF` : `2017` - Cannot obtain the current query information as the device is not in `STA` or `AP_STA` mode.
  - `ERR_INTERNAL` : `2006` - Internal error, the underlying interface encountered an error.
  - `ERR_NOT_SUPPORT` : `2004` - Unsupported parameter.
- Set configuration parameters:

  Returns an error code.

  - `ERR_OK` : `2000` - No error.
  - `ERR_PARAM_INVALID` : `2002` - Parameter error.
  - `ERR_INTERNAL` : `2006` - Internal error, the underlying interface encountered an error.

**Examples:**

```python
# Query AP SSID configuration
>>> nic.config('ap_ssid')
'quecpython'
```

```python
# Set to AP mode and connect with blocking
>>> import network
>>> nic = network.WLAN(network.AP_MODE)
>>> nic.config(ap_ssid="FCM360W-TEST", ap_password="12345678", ap_channel=5, ap_auth=nic.AUTH_WPA_WPA2_PSK, block=True, timeout=15)
>>> nic.active(True)
2000
```

```python
# Set to AP+STA mode and connect with non-blocking
>>> import network
>>> 
>>> def fun(event):
>>>     print(event)
>>> 
>>> nic = network.WLAN(network.AP_STA_MODE)
>>> nic.config(ap_ssid="FCM360W-TEST", ap_password="12345678", ap_channel=5, ap_auth=nic.AUTH_WPA_WPA2_PSK, sta_ssid="Tenda_996", sta_password="12345678", country="CN", block=False, timeout=15, event_callback=fun)
>>> nic.active(True)
2015
>>> {'msg': {'password': '12345678', 'ssid': 'Tenda_996', 'rssi': -35, 'channel': 3, 'bssid': '50:2b:73:08:55:d1', 'auth': 3, 'cipher': 4}, 'type': 3200, 'id': 3201}
{'msg': ('192.168.137.206', '255.255.255.0', '192.168.137.1', '0.0.0.0', '0.0.0.0'), 'type': 3200, 'id': 3204}
```

### `WLAN.active`

```python
WLAN.active([enable])
```

Configure/Query the activation status of the network card.

- Query Network Card Status

  It indicates querying the network card status without passing the parameter.
- Activate the Network Card

  It represents activating/disabling the network card when the parameter is passed as `True`/`False`.

**Parameter:**

- `enable` - `bool `type, `True`/`False` respectively represent activate/deactivate the network card.

**Return Value:**

- Network card status query

  Returns `True`/`False`, indicating activated/deactivated, `bool` type.
- Network card activation

  Returns error code, `int` type.

  - `ERR_OK`: `2000` - No error.
  - `ERR_WOULDBLOCK`: `2015` - In non-blocking mode, when the conditions for activating the network card are met, the interface returns `ERR_WOULDBLOCK` and performs asynchronous network card activation.
  - `ERR_PARAM_INVALID`: `2002` - Invalid parameter.
  - `ERR_TIMEOUT`: `2023` - Execution timeout.

**Example:**

```python
# Query network card status
>>> nic.active()
False
```

```python
# Activate network card
>>> nic.active(True)
2000
```

### `WLAN.connect`

```python
WLAN.connect([ssid, password, bssid, timeout])
```

Connects to a wireless network. Only supports Station mode and can be used for both blocking and non-blocking connections, with the  default being blocking. The timeout is set to 15 seconds by default. If  ssid and password are not specified, it will automatically connect to  the last successfully connected AP.

**Parameter:**

- `ssid` - Specify the name of the Wi-Fi to connect to, `str` type, 1 to 32 characters.
- `password` - Specify the password for the Wi-Fi to connect to, `str` type, 0 to 63 characters.
  - `AUTH_OPEN`: `0` characters - No password required.
  - `AUTH_WEP`: `5` characters - Fixed length of 5.
  - `AUTH_WPA_PSK`: `8~63` characters.
  - `AUTH_WPA2_PSK`: `8~63` characters.
  - `AUTH_WPA_WPA2_PSK`: `8~63` characters.
- `bssid` - Specify the BSSID information of the Wi-Fi to connect to; this parameter is the MAC address of the target AP, `str` type.
- `timeout` - Connection timeout in seconds, `int` type, default is `15` seconds.

**Return Value:**

Returns error code, `int` type.

- `ERR_OK`: `2000` - No error.
- `ERR_WOULDBLOCK`: `2015` - In non-blocking mode, when the conditions for establishing a network connection are met, the interface returns `ERR_WOULDBLOCK` and performs an asynchronous network connection.
- `ERR_NO_STA_IF`: `2017` - Cannot connect to the Wi-Fi network, as the device is not in `STA` or `AP_STA` mode.
- `ERR_PARAM_INVALID`: `2002` - Invalid parameter.
- `ERR_TIMEOUT`: `2023` - Execution timeout.

**Example:**

```python
# Connect to an AP and set the timeout to 10 seconds
>>> import network
>>> nic = network.WLAN(network.STA_MODE)
>>> nic.connect(ssid='your-ssid', password='your-password', timeout=10)
```

### `WLAN.disconnect`

```python
WLAN.disconnect([interface, mac, ip])
```

Disconnect from the network.

- In AP mode

  Passing the corresponding MAC or IP of the station can disconnect from  that specific station.

  If no parameters are passed, it disconnects from  all stations.
- In STA mode

  Disconnect from the AP network.
- In AP_STA mode

  Specify the WLAN interface to be operated on through the `interface` parameter.

**Parameter:**

- `interface` - Mandatory for AP_STA Mode, used to specify the WLAN interface to operate on, `int` type.

  - `SOFTAP_IF` : `0` - SoftAP interface.
  - `STATION_IF` : `1` - Station interface.
- `mac` - Disconnect from the specified station with the given MAC address in AP  or AP_STA mode. If MAC is `'FF:XX:XX:XX:XX:XX'`, it disconnects from all  stations, `str` type.
- `ip` - Disconnect from the specified station with the given IP address in AP or AP_STA mode, `str` type.

> If both the `mac` and `ip` parameters are passed simultaneously, the one specified later takes effect.

**Return Value:**

Returns an error code.

- `ERR_OK` : `2000` - No error.
- `ERR_NO_AP_IF` : `2020` - Unable to operate on the softap interface as the current mode is not `AP` or `AP_STA`.
- `ERR_NO_STA_IF` : `2017` - Unable to operate on the station interface as the current mode is not `STA` or `AP_STA`.
- `ERR_PARAM_INVALID` : `2002` - Parameter error.
- `ERR_INTERNAL` : `2006` - Internal error, underlying interface operation failed.

**Examples:**

```python
# Disconnect a station with MAC '7e:d0:3f:21:5d:34' in AP mode
>>> nic.disconnect(mac="7e:d0:3f:21:5d:34")
2000
```

```python
# Disconnect from the AP and close the station interface in STA mode
>>> nic.disconnect()
2000
```

```python
# Disconnect from the AP and close the station interface in AP_STA mode
>>> nic.disconnect(interface=nic.STATION_IF)
2000
```

```python
# Disconnect a station with IP '10.10.10.2' in AP_STA mode
>>> nic.disconnect(interface=nic.SOFTAP_IF, ip="10.10.10.2")
2000
```

### `WLAN.status`

```python
WLAN.status('param')
```

Query the status of the WLAN interface.

**Parameter:**

- `stations_list` - Only for `AP` or `AP_STA` mode, retrieves a list of all stations currently connected to the AP.
- `ap_list` - Only for `STA` or `AP_STA` mode, retrieves information about the currently connected AP.

  - `rf` - Retrieves the RF status, see [Status Codes](#Status-Codes).
- `softap_if` - Retrieves the current state of the `softap` interface, see [Status Codes](#Status-Codes).
- `station_if` - Retrieves the current state of the `station` interface, see [Status Codes](#Status-Codes).
- `netcfg` - Retrieves the current network configuration status, see [Status Codes](#Status-Codes).

**Return Value:**

- `stations_list` - Returns a list of all stations currently connected to the AP, `list` type, format: `[(aid, ip, mac), ...]`.
- `ap_list` - Only for `STA` or `AP_STA` mode, retrieves information about the currently connected AP, `list` type, format: `[ssid, password, mac, rssi, channel, auth, cipher]`.

  - `auth` - Authentication method, `int` type.

    - `AUTH_OPEN` : `0` - Open system authentication.
    - `AUTH_WEP` : `1` - Wired Equivalent Privacy (WEP) authentication.
    - `AUTH_WPA_PSK` : `2` - WPA Pre-Shared Key (PSK) authentication.
    - `AUTH_WPA2_PSK` : `3` - WPA2 Pre-Shared Key (PSK) authentication.
    - `AUTH_WPA_WPA2_PSK` : `4` - Mixed mode, a combination of WPA and WPA2 PSK authentication.
  - `cipher` - Encryption method, `int` type.

    - `CIPHER_NONE` : `0` - No encryption.
    - `CIPHER_WEP40` : `1` - WEP with 40-bit encryption.
    - `CIPHER_WEP104` : `2` - WEP with 104-bit encryption.
    - `CIPHER_TKIP` : `3` - TKIP encryption.
    - `CIPHER_CCMP` : `4` - CCMP encryption.
- `rf` - Retrieves the RF status, `int` type, see [Status Codes](#Status-Codes).
- `softap_if` - Retrieves the current state of the `softap` interface, `int` type, see [Status Codes](#Status-Codes).
- `station_if` - Retrieves the current state of the `station` interface, `int` type, see [Status Codes](#Status-Codes).
- `netcfg` - Retrieves the current network configuration status, `int` type, see [Status Codes](#Status-Codes).

If the execution of this interface fails, it will return an error code:

- `ERR_NO_AP_IF` : `2020` - Unable to perform operations related to the softap interface as the current mode is not `AP` or `AP_STA`.
- `ERR_AP_UNSTART` : `2019` - Softap is not started, unable to query the stations list.
- `ERR_NO_STA_IF` : `2017` - Unable to perform operations related to the station interface as the current mode is not `STA` or `AP_STA`.
- `ERR_UNCONNECTED_AP` : `2018` - Not connected to an AP, unable to query AP information.
- `ERR_PARAM_INVALID` : `2002` - Parameter error.
- `ERR_INTERNAL` : `2006` - Internal error, underlying interface operation failed.

**Example:**

```python
# Get information about the currently connected AP in STA mode
>>> nic.status("ap_list")
['Tenda_996', '12345678', '50:2b:73:08:55:d1', -38, 11, 3, 4]
```

### `WLAN.scan`

```python
WLAN.scan([ssid, bssid, channel, passive, max_item, scan_time])
```

Wireless network scanning.

**Parameter:**

- `ssid` - Specify the ssid to scan, `str` type, default is not configured, meaning scan all reachable APs in the current area.
- `bssid` - Specify the bssid to scan, `str` type, default is not configured, meaning scan all reachable APs in the current area.
- `channel` - Specify the channel to scan, `int` type, default is not configured, meaning scan all channels.
- `passive` - Whether it is a passive scan, `bool` type, `True` for passive scan, default is `False`.
- `max_item` - Specify the maximum number of APs to scan, `int` type, default is `128`.
- `scan_time` - Scan time for each channel, unit: ms, `int` type, default is not configured.

**Return Value:**

In blocking mode, returns the information of the scanned access points, `list` type, `[(ssid, bssid, channel, rssi, auth, hidden), ...]`

| Parameter   | Type     | Description                        |
| ----------- | -------- | ---------------------------------- |
| `ssid`    | `str`  | Access point name                  |
| `bssid`   | `str`  | Access point MAC                   |
| `channel` | `int`  | Access point channel               |
| `rssi`    | `int`  | Access point signal strength       |
| `auth`    | `int`  | Access point authentication method |
| `hidden`  | `bool` | Access point hidden status         |

For non-blocking mode, please use it with event callbacks to receive the scan results in the callback.

If scanning fails, it returns the corresponding error code, `int` type.

- `ERR_OK` : `2000` - No error.
- `ERR_WOULDBLOCK` : `2015` - `2015` - In non-blocking mode, when the conditions for hotspot scanning are met, the interface returns `ERR_WOULDBLOCK` and performs asynchronous hotspot scanning.
- `ERR_NO_STA_IF` : `2017` - Unable to use this method as the current mode is not `STA` or `AP_STA`.
- `ERR_PARAM_INVALID` : `2002` - Parameter error.
- `ERR_INTERNAL` : `2006` - Internal error, underlying interface operation failed.

**Example:**

```python
# Scan for AP with ssid 'Tenda_996' in STA mode
>>> nic.scan(ssid="Tenda_996")
[('Tenda_996', '50:2b:73:08:55:d1', 11, -42, 3, False)]
```

### `WLAN.ifconfig`

```python
WLAN.ifconfig([interface, config])
```

Configure or query network information.

**Parameter:**

- `interface` - Must specify the interface to configure/query in AP_STA mode, `int` type.

  - `SOFTAP_IF` : `0` - SoftAP interface.
  - `STATION_IF` : `1` - Station interface.
- `config` - IP configuration, `tuple` type, format: `(ip, netmask, gateway, primary_dns, secondary_dns)`.

  | Parameter         | Type    | Description                  |
  | ----------------- | ------- | ---------------------------- |
  | `ip`            | `str` | IP address                   |
  | `netmask`       | `str` | Subnet mask                  |
  | `gateway`       | `str` | Gateway                      |
  | `primary_dns`   | `str` | DNS server primary address   |
  | `secondary_dns` | `str` | DNS server secondary address |

**Return Value:**

For parameter query, returns the corresponding query result.

For parameter configuration, returns the corresponding error code, `int` type.

- `ERR_OK` : `2000` - No error.
- `ERR_NO_AP_IF` : `2020` - Unable to query softAP interface parameters as the current mode is not `AP` or `AP_STA`.
- `ERR_NO_STA_IF` : `2017` - Unable to query station interface parameters as the current mode is not `STA` or `AP_STA`.
- `ERR_PARAM_INVALID` : `2002` - Parameter error.
- `ERR_INTERNAL` : `2006` - Internal error, underlying interface operation failed.

**Example:**

Query the IP information of the station interface:

```python
nic.ifconfig(interface=nic.STATION_IF)
```

Set the IP information of the station interface:

```python
nic.ifconfig(interface=nic.STATION_IF, config=('192.168.0.4', '255.255.255.0', '192.168.0.1', '8.8.8.8', '114.114.114.114'))
```

### `WLAN.netcfg`

```python
WLAN.netcfg(enable=True, type=nic.NETCFG_SMARTCONFIG, timeout=120)
```

Enable or disable network configuration.

**Parameter:**

- `enable` - Enable/disable network configuration, `bool` type, `True` to enable, default is `True`.
- `type` - Network configuration method, `int` type.

  - `NETCFG_SMARTCONFIG` : `0` - SmartConfig, as this configuration mode requires enabling promiscuous mode, it will turn off the already enabled AP and STA.
  - `NETCFG_WEBCONFIG` : `1` - Web configuration, this configuration mode depends on AP, so please enable softAP first to use this configuration mode for network configuration. After successful activation, please access this address in the browser: http://`IP`:8080/setting, enter the ssid and password to complete the network configuration. Here, `IP` is the IP of the softAP opened by the module, which can be queried through `nic.ifconfig(interface=nic.SOFTAP_IF)`.
  - `NETCFG_BLECONFIG` : `2` - BLE configuration.
- `timeout` - Configuration timeout, `int` type, unit: s, default is 120s.

**Return Value:**

Returns an error code, `int` type.

- `ERR_OK` : `2000` - No error.
- `ERR_WOULDBLOCK` : `2015` - In non-blocking mode, after meeting the conditions for network configuration, the interface returns `ERR_WOULDBLOCK` and performs an asynchronous network configuration process.
- `ERR_NETCFG_BUSY` : `2024` - Repeatedly enabling network configuration.
- `ERR_AP_UNSTART` : `2019` - AP must be enabled before using web configuration, and web configuration logic depends on this AP.
- `ERR_TIMEOUT` : `2023` - Execution timeout.
- `ERR_PARAM_INVALID` : `2002` - Parameter error.
- `ERR_INTERNAL` : `2006` - Internal error, underlying interface operation failed.
- `ERR_NOT_SUPPORT` : `2004` - Unsupported operation.

**Example:**

```python
# Enable AP mode and start web configuration
>>> import network
>>> nic=network.WLAN(network.AP_MODE)
>>> nic.active(True)
2000
>>> def fun(event):
...     print(event)
>>> nic.config(block=False, event_callback=fun)
2000
>>> nic.netcfg(enable=True, type=nic.NETCFG_WEBCONFIG, timeout=120)
2015
{'msg': None, 'type': 3400, 'id': 3401}
```

```python
# Disable web configuration
>>> nic.netcfg(enable=False)
2000
{'msg': None, 'type': 3400, 'id': 3403}
```

## Constants

### Modes

| Mode        | Value | Description    |
| ----------- | ----- | -------------- |
| NONE_MODE   | 0     |  IDLE Mode         |
| AP_MODE     | 1     | Access Point Mode  |
| STA_MODE    | 2     | Station Mode       |
| AP_STA_MODE | 3     | Access Point and Station Mode |

### Interfaces

| Interface  | Value | Description       |
| ---------- | ----- | ----------------- |
| SOFTAP_IF  | 0     | SoftAP interface  |
| STATION_IF | 1     | Station interface |

### Authentication Modes

| Authentication Mode | Value | Description                                     |
| ------------------- | ----- | ----------------------------------------------- |
| AUTH_OPEN           | 0     | Open system authentication                      |
| AUTH_WEP            | 1     | Wired Equivalent Privacy (WEP) authentication   |
| AUTH_WPA_PSK        | 2     | WPA pre-shared key (PSK) authentication         |
| AUTH_WPA2_PSK       | 3     | WPA2 pre-shared key (PSK) authentication        |
| AUTH_WPA_WPA2_PSK   | 4     | Mixed mode with WPA and WPA2 PSK authentication |
| AUTH_WPA3_PSK       | 5     | WPA3 pre-shared key (PSK) authentication          |
| AUTH_WPA2_WPA3_PSK  | 6     | Mixed mode with WPA2 and WPA3 PSK authentication |

### Encryption Modes

| Encryption Mode | Value | Description       |
| --------------- | ----- | ----------------- |
| CIPHER_NONE     | 0     | No encryption     |
| CIPHER_WEP40    | 1     | WEP40 encryption  |
| CIPHER_WEP104   | 2     | WEP104 encryption |
| CIPHER_TKIP     | 3     | TKIP encryption   |
| CIPHER_CCMP     | 4     | CCMP encryption   |

### Configuration Modes

| Configuration Mode | Value | Description       |
| ------------------ | ----- | ----------------- |
| NETCFG_SMARTCONFIG | 0     | SmartConfig       |
| NETCFG_WEBCONFIG   | 1     | Web configuration |
| NETCFG_BLECONFIG   | 2     | BLE configuration |

### Error Codes

| Error Code                    | Value | Description                                             |
| ----------------------------- | ----- | ------------------------------------------------------- |
| ERR_OK                        | 2000  | No error                                                |
| ERR_NO_MEM                    | 2001  | Memory allocation failed                                |
| ERR_PARAM_INVALID             | 2002  | Invalid parameter                                       |
| ERR_WIFI_BUSY                 | 2003  | Wi-Fi card busy                                         |
| ERR_NOT_SUPPORT               | 2004  | Unsupported function or operation                       |
| ERR_WIFI_ALREADY_CUR_IF       | 2005  | The mode is already enabled                             |
| ERR_INTERNAL                  | 2006  | Internal exception                                      |
| ERR_WOULDCONNECT              | 2007  | Connect to AP needs to be called                        |
| ERR_NO_AP_FOUND               | 2008  | Hotspot not found                                       |
| ERR_PWD_WRONG                 | 2009  | Incorrect password                                      |
| ERR_AUTHMODE_WRONG            | 2010  | Authentication error                                    |
| ERR_CHANNEL_INVALID           | 2011  | Invalid channel                                         |
| ERR_NONAUTH                   | 2012  | WPA received class-2 frame from unauthenticated station |
| ERR_NONASSOC                  | 2013  | WPA received class-3 frame from unauthenticated station |
| ERR_HANDSHAKE_TIMEOUT         | 2014  | Handshake timeout                                       |
| ERR_WOULDBLOCK                | 2015  | Currently in non-blocking mode                          |
| ERR_NOT_ACTIVE                | 2016  | Not in active state                                     |
| ERR_NO_STA_IF                 | 2017  | Not in STA mode                                         |
| ERR_UNCONNECTED_AP            | 2018  | Not connected to AP                                     |
| ERR_AP_UNSTART                | 2019  | SoftAP interface not started                            |
| ERR_NO_AP_IF                  | 2020  | Not in AP mode                                          |
| ERR_NO_SCAN_MODE              | 2021  | Not in scan mode                                        |
| ERR_NO_AP_STA_IF              | 2022  | Not in STA or AP mode                                   |
| ERR_TIMEOUT                   | 2023  | Timeout                                                 |
| ERR_NETCFG_BUSY               | 2024  | Repeatedly enabling network configuration               |
| ERR_NETCFG_SOFTAP_START       | 2025  | AP start failed                                         |
| ERR_NETCFG_SOCKET_CREATE      | 2026  | Socket creation failed                                  |
| ERR_NETCFG_HTTP_SERVER_CREATE | 2027  | HTTP server start failed                                |
| ERR_NETCFG_BLE_SERVER_CREATE  | 2028  | BLE start failed                                        |
| ERR_NETCFG_GATT_SERVER_CREATE | 2029  | GATT server start failed                                |
| ERR_NETCFG_NOT_IN_NETCFG      | 2030  | Not in network configuration mode                       |
| ERR_NETCFG_HTTP_SERVER_CLOSE  | 2031  | Web configuration HTTP close failed                     |
| ERR_NETCFG_SOCKET_CLOSE       | 2032  | Web configuration SOCKET close failed                   |
| ERR_NETCFG_SOFTAP_CLOSE       | 2033  | Web configuration AP close failed                       |
| ERR_NETCFG_BLE_SERVER_CLOSE   | 2034  | BLE configuration server close failed                   |
| ERR_STA_KICKED_OFFLINE_BY_AP  | 2035  | Kicked offline by AP                                    |
| ERR_SER_DISCONNECT            | 2036  | User-initiated disconnection                            |
| ERR_AUTHMODE_CHANGED          | 2037  | Connection strategy changed                             |
| ERR_IFMODE_CHANGED            | 2038  | Mode changed                                            |
| ERR_DHCP_LEASE_EXPIRED        | 2039  | DHCP lease expired                                      |
| ERR_INACTIVE_TIMEOUT          | 2040  | No interaction within the specified timeout             |
| ERR_IP_DEL_ARTIFICIALLY       | 2041  | IP address deleted artificially                         |

### Event Codes

| Event Type | Event Name                       | Event ID | Event Description                          | Message Passed to User in Event Callback                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| ---------- | -------------------------------- | -------- | ------------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 3000       | EVENT_RF_WAKEUP                  | 3001     | RF Wakeup                                  | None                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| 3000       | EVENT_RF_SLEEP                   | 3002     | RF Sleep                                   | None                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| 3100       | EVENT_SCAN_DONE                  | 3101     | Scan Done                                  | Returns scan results,`list` type, format: `[(ssid, bssid, channel, rssi, auth, hidden),...]`                                                                                                                                                                                                                                                                                                                                                                           |
| 3200       | EVENT_AP_START_SUCCEED           | 3201     | AP Creation Successful                     | None                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| 3200       | EVENT_AP_START_FAILED            | 3202     | AP Creation Failed                         | None                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| 3200       | EVENT_STA_CONNECTED              | 3203     | Connected to Station                       | Returns information about the Station,`dict` type. `<br>aid`: ID assigned by AP to Station. `<br>mac`: Station's MAC address.                                                                                                                                                                                                                                                                                                                                        |
| 3200       | EVENT_STA_DISCONNECTED           | 3204     | Disconnected from Station                  | Returns information about the Station,`dict` type. `<br>aid`: ID assigned by AP to Station. `<br>ip`: Station's IP address. `<br>mac`: Station's MAC address.                                                                                                                                                                                                                                                                                                      |
| 3200       | EVENT_STA_IP_ASSIGNED            | 3205     | IP Assigned to Station                     | Returns information about the Station,`dict` type. `<br>aid`: ID assigned by AP to Station. `<br>ip`: Station's IP address. `<br>mac`: Station's MAC address.                                                                                                                                                                                                                                                                                                      |
| 3200       | EVENT_AP_STOP                    | 3206     | AP Mode Stop                               | None                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| 3300       | EVENT_STA_START_CONN             | 3301     | Start Connecting to AP                     | None                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| 3300       | EVENT_CONN_SUCCEED               | 3302     | Connection to AP Successful                | Returns information about the connected AP,`dict` type. `<br>ssid`: Wireless access point name, `str` type.`<br>password`: Wireless access point password, `str` type. `<br>bssid`: Wireless access point MAC address, `str` type. `<br>channel`: Current channel, `int` type. `<br>rssi`: Signal strength of the wireless access point, `int` type. `<br>auth`: Authentication mode, `int` type. `<br>cipher`: Encryption mode, `int` type. |
| 3300       | EVENT_CONN_FAILED                | 3303     | Connection to AP Failed                    | None                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| 3300       | EVENT_CONN_LOST                  | 3304     | Connection to AP Lost                      | Returns information about the disconnected AP,`dict` type. `<br>ssid`: Wireless access point name, `str` type.`<br>bssid`: Wireless access point MAC address, `str` type.`<br>reason`: Disconnection reason, `int` type(0 or 1:normal,other:error).                                                                                                                                                                                                                                     |
| 3300       | EVENT_GOT_IP                     | 3305     | IP Obtained                                | Returns obtained IP information,`tuple` type, format: `(ip, netmask, gateway, primary_dns, secondary_dns)`                                                                                                                                                                                                                                                                                                                                                             |
| 3300       | EVENT_LOST_IP                    | 3306     | IP Lost                                    | None                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| 3400       | EVENT_NETCFG_START_SUCCEED       | 3401     | Network Configuration Start Successful     | None                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| 3400       | EVENT_NETCFG_START_FAILED        | 3402     | Network Configuration Start Failed         | None                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| 3400       | EVENT_NETCFG_STOP_SUCCEED        | 3403     | Network Configuration Stop Successful      | None                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| 3400       | EVENT_NETCFG_STOP_FAILED         | 3404     | Network Configuration Stop Failed          | None                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| 3400       | EVENT_NETCFG_STAT_CLIENT_CONNECT | 3405     | Network Configuration Client Connected     | None                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| 3400       | EVENT_NETCFG_GOT_KEY             | 3406     | Network Configuration Got Account Password | Returns the obtained account and password,`tuple` type, format: `(ssid, password)`                                                                                                                                                                                                                                                                                                                                                                                     |
| 3400       | EVENT_NETCFG_TIMEOUT             | 3407     | Network Configuration Timeout              | None                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |

### Status Codes

| Status Code             | Value | Description                            |
| ----------------------- | ----- | -------------------------------------- |
| STATUS_RF_NOT_ACTIVATED | 1000  | RF not activated                       |
| STATUS_RF_ACTIVATED     | 1001  | RF activated                           |
| STATUS_RF_SLEEP         | 1002  | RF in sleep mode                       |
| STATUS_RF_WAKEUP        | 1003  | RF wake up                             |
| STATUS_AP_IDLE          | 1004  | SoftAP interface idle, not started     |
| STATUS_AP_START         | 1005  | SoftAP interface started               |
| STATUS_AP_STACONNECTED  | 1006  | Devices connected to SoftAP            |
| STATUS_STA_IDLE         | 1007  | Station interface idle, not started    |
| STATUS_STA_SCANNING     | 1008  | Scanning for APs                       |
| STATUS_STA_CONNECTING   | 1009  | Connecting to AP                       |
| STATUS_STA_CONNECTED    | 1010  | Connected to AP                        |
| STATUS_STA_GOT_IP       | 1011  | Obtained IP                            |
| STATUS_STA_LOST_IP      | 1012  | Lost IP                                |
| STATUS_NETCFG_NOT_INTO  | 1013  | Network configuration mode not started |
| STATUS_NETCFG_INTO      | 1014  | Entered network configuration mode     |
| STATUS_NETCFG_GOT_KEY   | 1015  | Obtained SSID and Password             |
