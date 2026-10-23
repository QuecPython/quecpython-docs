# WLAN - 无线网络控制

该类主要用于内置 Wi-Fi 网卡控制。

> 支持模组型号：FCM360W

**示例：**

```python
# 本例程演示了 Wi-Fi 网卡连接热点并进行网络通信的功能。
import network
import utime
nic = network.WLAN(network.STA_MODE)
while nic.status("station_if") != 1011:
    print("Waiting for connection...")
    nic.connect(ssid="your_ssid", password="your_password")
    utime.sleep(1)
print(nic.ifconfig())

# now use socket as usual
import usocket
addr = usocket.getaddrinfo("python.quectel.com", 80)[0][-1]
s = usocket.socket()
s.connect(addr)
s.send(b"GET / HTTP/1.1\r\nHost: python.quectel.com\r\n\r\n")
data = s.recv(1000)
print(data)
s.close()
```

## 构造函数

### `network.WLAN`

```python
class network.WLAN(mode)
```

初始化 Wi-Fi 网卡信息，并返回 Wi-Fi 网卡对象。

**参数描述：**

- `mode` - 无线网卡工作模式，int类型。
  - `network.NONE_MODE` : `0` - 空闲模式。
  - `network.AP_MODE` : `1` - AP模式，只支持无线客户端接入。
  - `network.STA_MODE` : `2` - Station模式，只接入无线网络，比如路由器、手机热点。
  - `network.AP_STA_MODE` : `3` - AP+Station模式，既支持无线客户端接入，同时也支持接入无线网络。

> 注意：
>
> 1. WLAN网卡对象只能创建一个，如果重复创建将会抛出异常：`ERR_WLAN_NETIF_IS_BUSY`。
> 2. WLAN网卡总共有以上四种工作模式，这四种工作模式实际上都是在对两个网络接口进行操作：`softap网口`和 `station网口`。当模式为 `AP_STA_MODE`时，此时 `softap`和 `station`两种网口并存，所以在调用一些有网口指向性的接口时需要指定 `interface`参数，否则我们无法知道您预期操作的网口。

**示例：**

```python
# 创建Station模式，并且连接指定AP
import network
nic = network.WLAN(network.STA_MODE)
nic.connect(ssid='your-ssid', password='your-password')
nic.status("station_if")
```

## 方法

### `WLAN.mode`

```python
WLAN.mode([mode])
```

设置或查询无线网卡工作模式。

- 查询无线网卡工作模式

  不传参时，为查询无线网卡工作模式。
- 设置无线网卡工作模式

  传参时，为设置无线网卡工作模式。模式切换后，无线网卡处于停止工作的状态。

**参数描述：**

- `mode` - 无线网卡工作模式，`int`类型。
  - `network.NONE_MODE` : `0` - 空闲模式。
  - `network.AP_MODE` : `1` - AP模式，只支持无线客户端接入。
  - `network.STA_MODE` : `2` - Station模式，只接入无线网络，比如路由器、手机热点。
  - `network.AP_STA_MODE` : `3` - AP+Station模式，既支持无线客户端接入，同时也支持接入无线网络。

**返回值描述：**

- 查询无线网卡工作模式

  返回无线网卡当前的工作模式，`int`类型。
- 设置无线网卡工作模式

  返回错误码，`int`类型。

  - `ERR_OK` : `2000` - 无错误。
  - `ERR_WIFI_BUSY` : `2003` - 已经处于该工作模式。
  - `ERR_INTERNAL` : `2006` - 内部错误，底层接口运行出错。
  - `ERR_NOT_SUPPORT` : `2004` - 不支持该模式。

**示例：**

```python
# 查询当前模式
>>> nic.mode()
2
```

```python
# 设置当前模式为AP模式
>>> nic.mode(network.AP_MODE)
2000
```

### `WLAN.config`

```python
WLAN.config('param')
WLAN.config(param=value)
```

设置或查询配置参数

- 查询配置参数

  输入相关的关键字进行参数查询，每次只能查询一个参数，`str`类型。
- 设置配置参数

  当参数为关键字参数时，进行参数设置；可同时设置多个参数。

**参数描述：**

- `ap_ssid` - softap的无线接入点名称，`str`类型，1~32 个字符，默认值：`'quecpython'`。
- `ap_password` - softap的无线接入点密码，`str`类型，0~63 个字符，默认值：`'quecpython'`。

  - `AUTH_OPEN` : `0`个字符 - 无需密码。
  - `AUTH_WEP` : `5`个字符 - 固定长度为5。
  - `AUTH_WPA_PSK` : `8~63`个字符。
  - `AUTH_WPA2_PSK` : `8~63`个字符。
  - `AUTH_WPA_WPA2_PSK` : `8~63`个字符。
- `ap_auth` - softap的认证方式，`int`类型，默认值：`AUTH_WPA_WPA2_PSK`。

  - `AUTH_OPEN` : `0` - 开放式身份认证。
  - `AUTH_WEP` : `1` - 有线等效加密身份认证。
  - `AUTH_WPA_PSK` : `2` - WPA预共享密钥身份认证。
  - `AUTH_WPA2_PSK` : `3` - WPA2预共享密钥身份认证。
  - `AUTH_WPA_WPA2_PSK` : `4` - 混合模式，结合WPA和WPA2的PSK身份认证。
- `ap_channel` - softap的信道，`int`类型。可设置的范围为：`[1,13]`，默认值：`3`。
- `ap_hidden` - softap的无线接入点名称是否隐藏，`bool`类型，`True`为隐藏，默认值：`False`。
- `ap_max_clients` - softap的最大client连接数，`int`类型，默认值：`4`。
- `ap_mac` - softap的MAC地址，`str`类型。
- `sta_ssid` - station待连接的无线接入点名称，`str`类型，1~32 个字符。
- `sta_password` - station待连接的无线接入点密码，`str`类型，0~63 个字符。
- `sta_bssid` - station目标无线接入点的MAC地址，`str`类型。
- `sta_channel` - station的信道，`int`类型。可设置的范围为：`[1,13]`。
- `sta_mac` - station的MAC地址，`str`类型。
- `block` - 是否为阻塞模式，`bool`类型，默认为 `True`，即阻塞模式。非阻塞模式请配合事件回调使用。
- `timeout` - 连接超时时间，`int`类型，单位：s，默认值：`15`s。
- `country` - 国家码，`str`类型。
- `event_callback` - WLAN状态回调函数，当WLAN状态发生改变时会产生回调，回调函数原型：

  ```
  fun(event)
  ```

  回调函数参数描述：

  - `event`：事件信息，`dict`类型，携带的参数如下：
    - `type`：事件类型，`int`类型，详见[事件码.事件类型](#事件码)。
    - `id`：具体的事件，`int`类型，详见[事件码.事件ID](#事件码)。
    - `msg`：传递给用户的消息，详见[事件码.事件回调传递给用户的消息](#事件码)。

**返回值描述：**

- 查询配置参数：

  返回查询结果。
  如果执行该接口失败，将返回错误码。

  - `ERR_NO_AP_IF` : `2020` - 当前不处于 `AP`或 `AP_STA`模式，无法获取当前查询的信息。
  - `ERR_NO_STA_IF` : `2017` - 当前不处于 `STA`或 `AP_STA`模式，无法获取当前查询的信息。
  - `ERR_INTERNAL` : `2006` - 内部错误，底层接口运行出错。
  - `ERR_NOT_SUPPORT` : `2004` - 不支持的参数。
- 设置配置参数：

  返回错误码。

  - `ERR_OK` : `2000` - 无错误。
  - `ERR_PARAM_INVALID` : `2002` - 参数错误。
  - `ERR_INTERNAL` : `2006` - 内部错误，底层接口运行出错。

**示例：**

```python
# 查询AP ssid名称配置
>>> nic.config('ap_ssid')
'quecpython'
```

```python
# 创建AP模式，并且使用阻塞连接
>>> import network
>>> nic = network.WLAN(network.AP_MODE)
>>> nic.config(ap_ssid="FCM360W-TEST", ap_password="12345678", ap_channel=5, ap_auth=nic.AUTH_WPA_WPA2_PSK, block=True, timeout=15)
>>> nic.active(True)
2000
```

```python
# 创建AP+STA模式，并且使用非阻塞连接
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

网卡激活状态配置/查询

- 网卡状态查询

  若不传参则表示查询网卡状态。
- 网卡激活

  若传参为 `True`/`False`，则表示激活/停用网卡。

**参数描述：**

- `enable` - bool类型，`True`/`False`分别表示激活/停用网卡。

**返回值描述：**

- 网卡状态查询

  返回 `True`/`False`，表示已激活/未激活，`bool`类型。
- 网卡激活

  返回错误码，`int`类型。

  - `ERR_OK` : `2000` - 无错误。
  - `ERR_WOULDBLOCK` : `2015` - 在非阻塞模式下，符合网卡激活的条件后，接口返回 `ERR_WOULDBLOCK`，并进行异步的网卡激活。
  - `ERR_PARAM_INVALID` : `2002` - 参数错误。
  - `ERR_TIMEOUT` : `2023` - 执行超时。

**示例：**

```python
# 查询网卡状态
>>> nic.active()
False
```

```python
# 激活网卡
>>> nic.active(True)
2000
```

### `WLAN.connect`

```python
WLAN.connect([ssid, password, bssid, timeout])
```

连接指定无线网络。仅支持Station模式，支持阻塞/非阻塞式连接，默认阻塞式连接，超时时间15s。
如果没有指定ssid和password，则会自动连接上一次连接成功的AP。

**参数描述：**

- `ssid` - 填写所要连接的 Wi-Fi 的名称，`str`类型，1~32 个字符。
- `password` - 填写所要连接的 Wi-Fi 的密码，`str`类型，0~63 个字符。
  - `AUTH_OPEN` : `0`个字符 - 无需密码。
  - `AUTH_WEP` : `5`个字符 - 固定长度为5。
  - `AUTH_WPA_PSK` : `8~63`个字符。
  - `AUTH_WPA2_PSK` : `8~63`个字符。
  - `AUTH_WPA_WPA2_PSK` : `8~63`个字符。
- `bssid` - 填写所要连接的 Wi-Fi 的bssid信息，该参数是目标AP的MAC地址，`str`类型。
- `timeout` - 连接超时时间，`int`类型，单位：s，默认 `15`s。

**返回值描述：**

返回错误码，`int`类型。

- `ERR_OK` : `2000` - 无错误。
- `ERR_WOULDBLOCK` : `2015` - 在非阻塞模式下，符合建立网络连接的条件后，接口返回 `ERR_WOULDBLOCK`，并进行异步的网络连接。
- `ERR_NO_STA_IF` : `2017` - 当前不处于 `STA`或 `AP_STA`模式，无法连接网络。
- `ERR_PARAM_INVALID` : `2002` - 参数错误。
- `ERR_TIMEOUT` : `2023` - 执行超时。

示例：

```python
# 连接AP，并设置超时时间为10s
>>> import network
>>> nic = network.WLAN(network.STA_MODE)
>>> nic.connect(ssid='your-ssid', password='your-password', timeout=10)
```

### `WLAN.disconnect`

```python
WLAN.disconnect([interface, mac, ip])
```

断开连接

- 处于 AP 模式时

  传入对应station的mac或ip可以断开与对应station的连接。
  不传参表示断开与所有 station 的连接。
- 处于 STA 模式时

  断开与热点的连接。
- 处于 AP_STA 模式时

  通过 `interface`参数指定需要操作的 WLAN 网口。

**参数描述：**

- `interface` - AP_STA模式必选，用于指定需要操作的WLAN网口，`int`类型。

  - `SOFTAP_IF` : `0` - softap网口
  - `STATION_IF` : `1` - station网口。
- `mac` - AP或者AP_STA模式下断开指定MAC的station连接，如果MAC为'FF:XX:XX:XX:XX:XX'，则表示与所有station断开连接，`str`类型。
- `ip` - AP或者AP_STA模式下断开指定IP的station连接，`str`类型。

> 如果同时传入了 `mac` 和 `ip` 两个参数，则位置靠后的参数生效。

**返回值描述：**

返回错误码。

- `ERR_OK` : `2000` - 无错误。
- `ERR_NO_AP_IF` : `2020` - 当前不处于 `AP`或 `AP_STA`模式，无法对softap网口进行操作。
- `ERR_NO_STA_IF` : `2017` - 当前不处于 `STA`或 `AP_STA`模式，无法对station网口进行操作。
- `ERR_PARAM_INVALID` : `2002` - 参数错误。
- `ERR_INTERNAL` : `2006` - 内部错误，底层接口运行出错。

**示例：**

```python
# AP模式下断开MAC为'7e:d0:3f:21:5d:34'的station
>>> nic.disconnect(mac="7e:d0:3f:21:5d:34")
2000
```

```python
# STA模式下断开与AP的连接，关闭station网口
>>> nic.disconnect()
2000
```

```python
# AP_STA模式下断开与AP的连接，关闭station网口
>>> nic.disconnect(interface=nic.STATION_IF)
2000
```

```python
# AP_STA模式下断开IP为'10.10.10.2'的station
>>> nic.disconnect(interface=nic.SOFTAP_IF, ip="10.10.10.2")
2000
```

### `WLAN.status`

```python
WLAN.status('param')
```

查询网卡状态。

**参数描述：**

- `stations_list` - 仅 `AP`或 `AP_STA`模式下使用，查询与当前AP连接的所有station列表。
- `ap_list` - 仅 `STA`或 `AP_STA`模式下使用，获取当前连接的AP的信息。
- `rf` - 获取射频状态，见[状态码](#状态码)。
- `softap_if` - 获取当前 `softap`网口状态，见[状态码](#状态码)。
- `station_if` - 获取当前 `station`网口状态，见[状态码](#状态码)。
- `netcfg` - 获取当前配网状态，见[状态码](#状态码)。

**返回值描述：**

- `stations_list` - 返回与当前AP连接的所有station列表，`list`类型，格式：`[(aid, ip, mac),...]`。
- `ap_list` - 仅 `STA`或 `AP_STA`模式下生效，获取当前连接的AP的信息，`list`类型，格式：`[ssid, password, mac, rssi, channel, auth, cipher]`。

  - `auth` - 认证方式，`int`类型。

    - `AUTH_OPEN` : `0` - 开放式身份认证。
    - `AUTH_WEP` : `1` - 有线等效加密身份认证
    - `AUTH_WPA_PSK` : `2` - WPA预共享密钥身份认证。
    - `AUTH_WPA2_PSK` : `3` - WPA2预共享密钥身份认证。
    - `AUTH_WPA_WPA2_PSK` : `4` - 混合模式，结合WPA和WPA2的PSK身份认证。
  - `cipher` - 加密方式，`int`类型

    - `CIPHER_NONE` : `0` - 无加密。
    - `CIPHER_WEP40` : `1` - WEP40加密。
    - `CIPHER_WEP104` : `2` - WEP104加密。
    - `CIPHER_TKIP` : `3` - TKIP加密。
    - `CIPHER_CCMP` : `4` - CCMP加密。
- `rf` - 获取射频状态，`int`类型，见[状态码](#状态码)。
- `softap_if` - 获取当前 `softap`网口状态，`int`类型，见[状态码](#状态码)。
- `station_if` - 获取当前 `station`网口状态，`int`类型，见[状态码](#状态码)。
- `netcfg` - 获取当前配网状态，`int`类型，见[状态码](#状态码)。

如果执行该接口失败，将返回错误码：

- `ERR_NO_AP_IF` : `2020` - 当前不处于 `AP`或 `AP_STA`模式，无法执行与softap网口相关的操作。
- `ERR_AP_UNSTART` : `2019` - softap未开启，无法查询stations列表。
- `ERR_NO_STA_IF` : `2017` - 当前不处于 `STA`或 `AP_STA`模式，无法执行与station网口相关的操作。
- `ERR_UNCONNECTED_AP` : `2018` - 未连接AP，无法查询AP信息。
- `ERR_PARAM_INVALID` : `2002` - 参数错误。
- `ERR_INTERNAL` : `2006` - 内部错误，底层接口运行出错。

**示例：**

```python
# STA模式下获取当前连接的AP信息
>>> nic.status("ap_list")
['Tenda_996', '12345678', '50:2b:73:08:55:d1', -38, 11, 3, 4]
```

### `WLAN.scan`

```python
WLAN.scan([ssid, bssid, channel, passive, max_item, scan_time])
```

无线网络扫描。

**参数描述：**

- `ssid` - 指定扫描的ssid，`str`类型，默认不配置，即扫描当前区域内的所有可达的AP。
- `bssid` - 指定扫描的bssid，`str`类型，默认不配置，即扫描当前区域内的所有可达的AP。
- `channel` - 指定扫描的信道，`int`类型，默认不配置，即扫描所有信道。
- `passive` - 是否为被动扫描，`bool`类型，`True`为被动扫描，默认值为 `False`。
- `max_item` - 指定扫描的最大AP数，`int`类型，默认值为 `128`。
- `scan_time` - 每个通道的扫描时间，单位：ms，`int`类型，默认不配置。

**返回值描述：**

阻塞模式扫描成功返回扫描到的可接入点信息，`list`类型，`[(ssid, bssid, channel, rssi, auth, hidden)，...]`

| 参数        | 类型     | 说明              |
| ----------- | -------- | ----------------- |
| `ssid`    | `str`  | 可接入点 名称     |
| `bssid`   | `str`  | 可接入点 MAC      |
| `channel` | `int`  | 可接入点 所在信道 |
| `rssi`    | `int`  | 可接入点 信号强度 |
| `auth`    | `int`  | 可接入点 认证方式 |
| `hidden`  | `bool` | 可接入点 是否隐藏 |

非阻塞模式请配合事件回调使用，在回调中返回扫描结果。

扫描失败返回对应错误码，`int`类型。

- `ERR_OK` : `2000` - 无错误。
- `ERR_WOULDBLOCK` : `2015` - 在非阻塞模式下，符合热点扫描的条件后，接口返回 `ERR_WOULDBLOCK`，并进行异步的热点扫描。
- `ERR_NO_STA_IF` : `2017` - 当前不处于 `STA`或 `AP_STA`模式，无法使用该方法。
- `ERR_PARAM_INVALID` : `2002` - 参数错误。
- `ERR_INTERNAL` : `2006` - 内部错误，底层接口运行出错。

**示例：**

```python
# STA模式下扫描ssid名为'Tenda_996'的AP
>>> nic.scan(ssid="Tenda_996")
[('Tenda_996', '50:2b:73:08:55:d1', 11, -42, 3, False)]
```

### `WLAN.ifconfig`

```python
WLAN.ifconfig([interface, config])
```

配置或查询网络信息。

**参数描述：**

- `interface` - AP_STA 模式下必须指定需要配置/查询的网口，`int`类型。

  - `SOFTAP_IF` : `0` - softap网口
  - `STATION_IF` : `1` - station网口。
- `config` - ip配置，`tuple`类型，格式：`(ip, netmask, gateway, primary_dns, secondary_dns)`。

  | 参数              | 类型    | 说明            |
  | ----------------- | ------- | --------------- |
  | `ip`            | `str` | ip地址          |
  | `netmask`       | `str` | 子网掩码        |
  | `gateway`       | `str` | 网关            |
  | `primary_dns`   | `str` | DNS服务器主地址 |
  | `secondary_dns` | `str` | DNS服务器辅地址 |

**返回值描述：**

参数查询返回对应的查询结果。

参数配置返回对应错误码，`int`类型。

- `ERR_OK` : `2000` - 无错误。
- `ERR_NO_AP_IF` : `2020` - 当前不处于 `AP`或 `AP_STA`模式，无法使用查询softap网口参数。
- `ERR_NO_STA_IF` : `2017` - 当前不处于 `STA`或 `AP_STA`模式，无法查询station网口参数。
- `ERR_PARAM_INVALID` : `2002` - 参数错误。
- `ERR_INTERNAL` : `2006` - 内部错误，底层接口运行出错。

**示例：**

查询station网口的ip信息：

```python
nic.ifconfig(interface=nic.STATION_IF)
```

设置station网口的ip信息：

```python
nic.ifconfig(interface=nic.STATION_IF, config=('192.168.0.4', '255.255.255.0', '192.168.0.1', '8.8.8.8', '114.114.114.114'))
```

### `WLAN.netcfg`

```python
WLAN.netcfg(enable=True, type=nic.NETCFG_SMARTCONFIG, timeout=120)
```

开启或关闭配网。

**参数描述：**

- `enable` - 开启/关闭配网，`bool`类型，`True`为开启，默认为 `True`。
- `type` - 配网方式，`int`类型。

  - `NETCFG_SMARTCONFIG` : `0` - smartconfig配网，由于该配网模式需要开启混杂模式，所以启动后会关闭已开启的AP和STA。
  - `NETCFG_WEBCONFIG` : `1` - web配网，该配网模式依赖AP，所有请先开启softap才能使用该配网模式进行配网，成功开启后，请使用浏览器访问该地址：http://`IP`:8080/setting，输入ssid和密码后即可完成配网。此处 `IP`为模组开启的softap的IP，可以通过 `nic.ifconfig(interface=nic.SOFTAP_IF)`来查询。
  - `NETCFG_BLECONFIG` : `2` - ble配网。
- `timeout` - 配网超时时间，`int`类型，单位：s，默认120s。

**返回值描述：**

返回错误码，`int`类型。

- `ERR_OK` : `2000` - 无错误。
- `ERR_WOULDBLOCK` : `2015` - 在非阻塞模式下，符合配网的条件后，接口返回 `ERR_WOULDBLOCK`，并进行异步的配网流程。
- `ERR_NETCFG_BUSY` : `2024` - 重复开启配网。
- `ERR_AP_UNSTART` : `2019` - 使用webconfig前需开启AP，webconfig业务逻辑依赖于此AP。
- `ERR_TIMEOUT` : `2023` - 执行超时。
- `ERR_PARAM_INVALID` : `2002` - 参数错误。
- `ERR_INTERNAL` : `2006` - 内部错误，底层接口运行出错。
- `ERR_NOT_SUPPORT` : `2004` - 不支持的操作。

**示例：**

```python
# 开启AP模式并开始web配网
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
# 关闭web配网
>>> nic.netcfg(enable=False)
2000
{'msg': None, 'type': 3400, 'id': 3403}
```

## 常量

### 模式

| 模式        | 值 | 说明           |
| ----------- | -- | -------------- |
| NONE_MODE   | 0  | 空闲模式       |
| AP_MODE     | 1  | AP模式         |
| STA_MODE    | 2  | Station模式    |
| AP_STA_MODE | 3  | AP+Station模式 |

### 网口

| 网口       | 值 | 说明        |
| ---------- | -- | ----------- |
| SOFTAP_IF  | 0  | softap网口  |
| STATION_IF | 1  | station网口 |

### 认证方式

| 认证方式          | 值 | 说明                                 |
| ----------------- | -- | ------------------------------------ |
| AUTH_OPEN         | 0  | 开放式身份认证                       |
| AUTH_WEP          | 1  | 有线等效加密身份认证                 |
| AUTH_WPA_PSK      | 2  | WPA预共享密钥身份认证                |
| AUTH_WPA2_PSK     | 3  | WPA2预共享密钥身份认证               |
| AUTH_WPA_WPA2_PSK | 4  | 混合模式，结合WPA和WPA2的PSK身份认证 |
| AUTH_WPA3_PSK      | 5  | WPA3预共享密钥身份认证                |
| AUTH_WPA2_WPA3_PSK | 6  | 混合模式，结合WPA2和WPA3的PSK身份认证 |

### 加密方式

| 加密方式      | 值 | 说明       |
| ------------- | -- | ---------- |
| CIPHER_NONE   | 0  | 无加密     |
| CIPHER_WEP40  | 1  | WEP40加密  |
| CIPHER_WEP104 | 2  | WEP104加密 |
| CIPHER_TKIP   | 3  | TKIP加密   |
| CIPHER_CCMP   | 4  | CCMP加密   |

### 配网方式

| 配网方式           | 值 | 说明            |
| ------------------ | -- | --------------- |
| NETCFG_SMARTCONFIG | 0  | smartconfig配网 |
| NETCFG_WEBCONFIG   | 1  | web配网         |
| NETCFG_BLECONFIG   | 2  | ble配网         |

### 错误码

| 错误码                        | 说明                                  | 值   |
| ----------------------------- | ------------------------------------- | ---- |
| ERR_OK                        | 无错误                                | 2000 |
| ERR_NO_MEM                    | 内存申请失败                          | 2001 |
| ERR_PARAM_INVALID             | 无效参数                              | 2002 |
| ERR_WIFI_BUSY                 | 网卡忙                                | 2003 |
| ERR_NOT_SUPPORT               | 不支持该功能或操作                    | 2004 |
| ERR_WIFI_ALREADY_CUR_IF       | 该模式已经使能                        | 2005 |
| ERR_INTERNAL                  | 内部异常                              | 2006 |
| ERR_WOULDCONNECT              | 需要调用connect接口连接AP             | 2007 |
| ERR_NO_AP_FOUND               | 未找到热点                            | 2008 |
| ERR_PWD_WRONG                 | 密码错误                              | 2009 |
| ERR_AUTHMODE_WRONG            | 身份验证错误                          | 2010 |
| ERR_CHANNEL_INVALID           | 信道错误                              | 2011 |
| ERR_NONAUTH                   | WPA  收到未认证station的class-2 frame | 2012 |
| ERR_NONASSOC                  | WPA  收到未认证station的class-3 frame | 2013 |
| ERR_HANDSHAKE_TIMEOUT         | 握手超时                              | 2014 |
| ERR_WOULDBLOCK                | 当前处于非阻塞模式                    | 2015 |
| ERR_NOT_ACTIVE                | 未处于激活状态                        | 2016 |
| ERR_NO_STA_IF                 | 未处于STA模式                         | 2017 |
| ERR_UNCONNECTED_AP            | 未连接热点                            | 2018 |
| ERR_AP_UNSTART                | softap网口未启动                      | 2019 |
| ERR_NO_AP_IF                  | 未处于AP模式                          | 2020 |
| ERR_NO_SCAN_MODE              | 未处于扫描模式                        | 2021 |
| ERR_NO_AP_STA_IF              | 未处于STA或AP模式                     | 2022 |
| ERR_TIMEOUT                   | 超时                                  | 2023 |
| ERR_NETCFG_BUSY               | 重复开启配网                          | 2024 |
| ERR_NETCFG_SOFTAP_START       | AP启动失败                            | 2025 |
| ERR_NETCFG_SOCKET_CREATE      | Socket创建失败                        | 2026 |
| ERR_NETCFG_HTTP_SERVER_CREATE | http  server启动失败                  | 2027 |
| ERR_NETCFG_BLE_SERVER_CREATE  | ble启动失败                           | 2028 |
| ERR_NETCFG_GATT_SERVER_CREATE | GATT服务启动失败                      | 2029 |
| ERR_NETCFG_NOT_IN_NETCFG      | 未处于配网模式                        | 2030 |
| ERR_NETCFG_HTTP_SERVER_CLOSE  | web配网http关闭失败                   | 2031 |
| ERR_NETCFG_SOCKET_CLOSE       | web配网SOCKET关闭失                   | 2032 |
| ERR_NETCFG_SOFTAP_CLOSE       | web配网AP关闭失败                     | 2033 |
| ERR_NETCFG_BLE_SERVER_CLOSE   | ble配网服务关闭失败                   | 2034 |
| ERR_STA_KICKED_OFFLINE_BY_AP  | 被AP踢下线                            | 2035 |
| ERR_SER_DISCONNECT            | 用户主动断开                          | 2036 |
| ERR_AUTHMODE_CHANGED          | 连接策略改变                          | 2037 |
| ERR_IFMODE_CHANGED            | 模式改变                              | 2038 |
| ERR_DHCP_LEASE_EXPIRED        | DHCP租期到期                          | 2039 |
| ERR_INACTIVE_TIMEOUT          | 在指定的超时时间内，未进行任何交互    | 2040 |
| ERR_IP_DEL_ARTIFICIALLY       | 人为删除IP地址                        | 2041 |

### 事件码

| 事件类型 | 事件名                           | 事件ID | 事件说明           | 事件回调传递给用户的消息                                                                                                                                                                                                                                                                                                                                 |
| -------- | -------------------------------- | ------ | ------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 3000     | EVENT_RF_WAKEUP                  | 3001   | 射频唤醒           | None                                                                                                                                                                                                                                                                                                                                                     |
| 3000     | EVENT_RF_SLEEP                   | 3002   | 射频休眠           | None                                                                                                                                                                                                                                                                                                                                                     |
| 3100     | EVENT_SCAN_DONE                  | 3101   | 扫描结束           | 返回扫描结果，`list`类型，格式：`[(ssid, bssid, channel, rssi, auth, hidden)，...]`                                                                                                                                                                                                                                                                  |
| 3200     | EVENT_AP_START_SUCCEED           | 3201   | AP创建成功         | None                                                                                                                                                                                                                                                                                                                                                     |
| 3200     | EVENT_AP_START_FAILED            | 3202   | AP创建失败         | None                                                                                                                                                                                                                                                                                                                                                     |
| 3200     | EVENT_STA_CONNECTED              | 3203   | 与Station建立连接  | 返回Station的信息，`dict`类型。`<br>aid`: AP为Station分配的id。`<br>mac`: Station的MAC地址。                                                                                                                                                                                                                                                       |
| 3200     | EVENT_STA_DISCONNECTED           | 3204   | 与Station断开连接  | 返回Station的信息，`dict`类型。`<br>aid`: AP为Station分配的id。`<br>ip`: Station的IP地址。`<br>mac`: Station的MAC地址。                                                                                                                                                                                                                          |
| 3200     | EVENT_STA_IP_ASSIGNED            | 3205   | 已为Station分配IP  | 返回Station的信息，`dict`类型。`<br>aid`: AP为Station分配的id。`<br>ip`: Station的IP地址。`<br>mac`: Station的MAC地址。                                                                                                                                                                                                                          |
| 3200     | EVENT_AP_STOP                    | 3206   | 停止AP模式         | None                                                                                                                                                                                                                                                                                                                                                     |
| 3300     | EVENT_STA_START_CONN             | 3301   | 开始连接AP         | None                                                                                                                                                                                                                                                                                                                                                     |
| 3300     | EVENT_CONN_SUCCEED               | 3302   | 连接AP成功         | 返回已连接的AP信息，`dict`类型。 `<br>ssid`: 无线接入点名称，`str`类型。`<br>password`: 无线接入点密码，`str`类型。 `<br>bssid`: 无线接入点MAC地址，`str`类型。 `<br>channel`: 当前所处信道，`int`类型。 `<br>rssi`: 无线接入点信号强度，`int`类型。 `<br>auth`: 认证方式，`int`类型。 `<br>cipher`: 加密方式，`int`类型。 |
| 3300     | EVENT_CONN_FAILED                | 3303   | 连接AP失败         | None                                                                                                                                                                                                                                                                                                                                                     |
| 3300     | EVENT_CONN_LOST                  | 3304   | 与AP断开连接       | 返回断开连接的AP信息，`dict`类型。 `<br>ssid`: 无线接入点名称，`str`类型。`<br>bssid`: 无线接入点MAC地址，`str`类型。`<br>reason`: 断连原因，`int`类型（0、1：正常断开，其他：异常）。                                                                                                                                                                                   |
| 3300     | EVENT_GOT_IP                     | 3305   | 获取到IP           | 返回获取到的IP信息，`tuple`类型，格式：`(ip, netmask, gateway, primary_dns, secondary_dns)`                                                                                                                                                                                                                                                          |
| 3300     | EVENT_LOST_IP                    | 3306   | IP丢失             | None                                                                                                                                                                                                                                                                                                                                                     |
| 3400     | EVENT_NETCFG_START_SUCCEED       | 3401   | 配网启动成功       | None                                                                                                                                                                                                                                                                                                                                                     |
| 3400     | EVENT_NETCFG_START_FAILED        | 3402   | 配网启动失败       | None                                                                                                                                                                                                                                                                                                                                                     |
| 3400     | EVENT_NETCFG_STOP_SUCCEED        | 3403   | 配网停止成功       | None                                                                                                                                                                                                                                                                                                                                                     |
| 3400     | EVENT_NETCFG_STOP_FAILED         | 3404   | 配网停止失败       | None                                                                                                                                                                                                                                                                                                                                                     |
| 3400     | EVENT_NETCFG_STAT_CLIENT_CONNECT | 3405   | 配网有客户端连接   | None                                                                                                                                                                                                                                                                                                                                                     |
| 3400     | EVENT_NETCFG_GOT_KEY             | 3406   | 配网获取到账号密码 | 返回获取到的账号密码，`tuple`类型，格式：`(ssid, password)`                                                                                                                                                                                                                                                                                          |
| 3400     | EVENT_NETCFG_TIMEOUT             | 3407   | 配网超时           | None                                                                                                                                                                                                                                                                                                                                                     |

### 状态码

| 状态码                  | 值   | 说明                     |
| ----------------------- | ---- | ------------------------ |
| STATUS_RF_NOT_ACTIVATED | 1000 | 射频未激活               |
| STATUS_RF_ACTIVATED     | 1001 | 射频已激活               |
| STATUS_RF_SLEEP         | 1002 | 射频已休眠               |
| STATUS_RF_WAKEUP        | 1003 | 射频已唤醒               |
| STATUS_AP_IDLE          | 1004 | softap网口空闲，未启动   |
| STATUS_AP_START         | 1005 | softap网口已启动         |
| STATUS_AP_STACONNECTED  | 1006 | 已有设备接入             |
| STATUS_STA_IDLE         | 1007 | station网口空闲，未启动  |
| STATUS_STA_SCANNING     | 1008 | 正在扫描AP               |
| STATUS_STA_CONNECTING   | 1009 | 正在连接AP               |
| STATUS_STA_CONNECTED    | 1010 | AP连接成功               |
| STATUS_STA_GOT_IP       | 1011 | 已获取到IP               |
| STATUS_STA_LOST_IP      | 1012 | 丢失IP                   |
| STATUS_NETCFG_NOT_INTO  | 1013 | 配网模式未启动           |
| STATUS_NETCFG_INTO      | 1014 | 进入配网模式             |
| STATUS_NETCFG_GOT_KEY   | 1015 | 已获取到  ssid 和 passwd |
