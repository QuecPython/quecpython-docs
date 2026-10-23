# USSD - 非结构化补充数据业务

USSD（Unstructured Supplementary Service Data,非结构化补充数据业务）是一种基于全球移动通信系统GSM（Global System for Mobile Communications）网络的、实时在线的新型交互会话数据业务;它基于SIM卡,利用GSM网络的信令通道传送数据,是在GSM的短消息系统技术基础上推出的新业务,在业务开拓方面的能力远远强于SMS系统。

USSD使用字符组成代码,最长可支持182个字符.用户可以通过从各种菜单中进行选择来直接通过手机进行交互.与SMS消息不同,在USSD会话期间,USSD消息会创建实时连接.这意味着只要通信线路保持畅通,USSD就可以进行双向信息通信.因此,查询和回答几乎是即时的.

USSD在会话过程中一直保持通话连接,提供透明通道,不进行存储转发;而SMS在物理承载层没有会话通道,是一个存储转发系统,用户完成一次查询需要进行多次会话过程.由于USSD与GPRS类似,在交互中保持一个会话过程,每次数据发送不需要重新建立信道,所以USSD系统对用户的呼叫请求是即时响应,使响应时间大大加快,主要的响应时延已转移为应用服务器一端,响应时间比短消息快.



> 要支持USSD有3个条件:
> 1. 终端设备要支持USSD功能;
> 2. SIM卡支持；
> 3. 网络要支持,需要有USSD服务器;
>
> 中国境内目前运营商均不支持USSD功能，其它地区请咨询当地运营商。
>
> 目前仅EC200UEU_AA和EG915ULA_AB_VOLTE支持USSD功能。



**示例：**

```Python
>>> import ussd

>>> def ussdCallback(args):     
...  	print(args)
...
>>> ussd.setCallback(ussdCallback)   # 注册回调函数

>>> ussd.send("*101#")    # 向网络发送USSD指令 *101#
0
>>>
(4, '', 0, 4)  # 回调收到网络端回复的内容

>>> ussd.getSessionState()   #查询是否处于连续会话状态
False

```

## 回调注册功能

### `ussd.setCallback`

```
ussd.setCallback(fun)
```

注册回调函数。在回调函数中接收针对网络端或网络发起操作的 USSD 响应，只要收到网络端的USSD消息，均会触发此回调。

**参数描述：**

* `fun` - 回调函数名，回调函数格式以及回调函数的参数说明如下：

```python
def ussdCallback(args):
	pass
```

| 参数      | 类型   | 含义                                                         |
| --------- | ------ | ------------------------------------------------------------ |
| `args[0]` | 整型   | 网络响应状态。<br>`0`响应成功，返回网络回复的字符串类型内容，见`args[1]`；<br>`1`响应成功，结束会话；<br>`2`、`3`响应成功，无回复内容；<br>`4`本地错误/网络拒绝/网络返回错误。 |
| `args[1]` | 字符串 | 收到的网络端响应USSD消息内容。只有`args[0]`为0时为正常字符串，其它为空字符串； |
| `args[2]` | 整型   | 网络下发USSD的 DCS。 DCS： 小区广播数据编码方案。            |
| `args[4]` | 整型   | 异常状态错误码。                                             |

**示例：**

```python
import ussd

def ussdCallback(args):
    resp_type = args[0]
    ussd_str = args[1]
    dcs = args[2]
    err_code = args[3]
    if resp_type == 0:
        print('### USSD Network successful return:',ussd_str)
    elif resp_type == 1:
        print('### USSD end of session.'
    elif resp_type in [2,3]:
        print('### USSD Network successful return None')
    else:
        print('### USSD Network return error,err_code:',err_code) 
ussd.setCallback(ussdCallback)
```



## 发送USSD指令

### `ussd.send`

```
ussd.send(reqstr)
```

该方法用于向网络发送USSD指令，默认DCS等于15。网络回复的结果直接在回调函数中展示。

**参数描述：**

* `reqstr`- 发送至网络的 USSD，字符串类型，不可为空，范围1~159字节。

**返回值描述：**

返回一个整型值，`0`表示发送成功，`-1`表示发送失败。

**示例：**

```python
>>> ussd
>>> ussd.send("*101#")    # 向网络发送USSD指令。
0
```



## 查询会话状态

### `ussd.getSessionState`

```
ussd.getSessionState()
```

该方法用于查询USSD的会话状态，当处于连续会话状态时，进一步的用户操作。

**返回值描述：**

  返回布尔类型，`False`非连续会话状态，无需进一步的用户操作（网络发起的 USSD 通知，或设备发起的操作后无需进一步的信息）；

`True`连续会话状态，需要进一步的用户操作(网络发起的 USSD 请求或 设备发起操作后需要进一步信息)。



> 当`ussd.getSessionState()`返回值为`True`时，网络端会一直保存与用户交互的会话通道，需要用户进一步操作，有两种操作方式：
>
> 1. 继续使用`ussd.send`发送USSD指令，与网络端交互；
> 2. 使用`ussd.stopSession`结束连续会话状态；

**示例：**

```python
>>>	import ussd
>>> ussd.getSessionState()
False
```



## 结束连续会话状态

### ``ussd.stopSession``

```
ussd.stopSession()
```

该方法用于结束连续会话状态，释放网络端保存的USSD会话通道，会触发结束会话的USSD回调函数，`args[0]`等于1。

**返回值描述：**

返回一个整型值，`0`表示结束会话成功，`-1`表示失败或无需结束会话状态。

```python
>>>	import ussd
>>> ussd.getSessionState()
True
>>> ussd.stopSession()
0
```



## DCS配置与查询

### `ussd.getDcs`

```
ussd.getDcs()
```

该方法用于查询发送USSD指令的小区广播数据编码方案。

**返回值描述：**

  返回发送USSD指令小区广播数据编码方案DCS，整型，范围 1~ 17, 68, 72, 240。



>DCS: 3GPP TS 23.038 小区广播数据编码方案。默认值：15，一般无需设置。



**示例：**

```python
>>> import ussd
>>> ussd.getDcs()
15
```



### `ussd.setDcs`

```
ussd.getDcs(Dcs)
```

该方法用于设置发送USSD指令的DCS。

**参数描述：**

* `Dcs`- 小区广播数据编码方案，整型，范围 `0~ 17, 68, 72, 240`；当为0时，表示恢复默认值。

**返回值描述：**

  返回一个整型值，`0`表示成功，`-1`表示失败。

**示例：**

```python
>>> import ussd
>>> ussd.getDcs()
15
>>> import ussd
>>> ussd.setDcs(1)
0
>>> ussd.getDcs()
1
```



