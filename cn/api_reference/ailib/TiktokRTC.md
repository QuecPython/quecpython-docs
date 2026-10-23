# TiktokRTC - 火山实时对话式 AI

火山实时对话式 AI，在实时音视频场景中，若你希望启动智能体提供语音聊天服务，可以通过调用此类实现。

> 支持模组型号：EC600M系列、EC800M系列、EG810M系列。

## 构造函数

### `TiktokRTC`

```python
class TiktokRTC(time, callback=None)
```

初始化 TiktokRTC，并返回 tiktok 火山大模型对话对象。

**参数描述：**
- `time` - 房间无对话时，自动退出房间的超时时间。
- `callback` - 回调函数，用于大模型连接状态改变通知。

**callback参数描述：**

tuple类型，格式（event，msg）  

- `event` - 整性数值。
    - `1` - 大模型启动。
    - `2` - 大模型关闭。
    - `3` - TTS内容，播放声音内容的文本。
    - `4` - ASR内容，返回当前识别文本。
    - `5` - 异常事件，返回异常信息。
- `msg` - 消息内容。


**示例：**

```python
import TiktokRTC

tiktok = TiktokRTC(3000000)
```

## 方法

### `TiktokRTC.active([is_active])`

```python
tiktok.active()
```

大模型连接激活状态配置/查询。

若传参则表示火山大模型连接状态配置，传参True/False开启/关闭火山大模型。  
若不传参则表示查询火山大模型连接状态，返回值True/False表示火山大模型开启/关闭。

**可选参数描述：**

- `is_active` - bool类型，填True时表示启动火山大模型，为False则代表关闭火山大模型。返回0表示成功，返回其他值表示失败。


### `TiktokRTC.config`

```python
TiktokRTC.config('param')
TiktokRTC.config(param=value)
```

火山大模型参数查询/配置。

当只传参关键字时，接口查询，每次只能查询一个值。
当传参赋值时，接口设置，返回True/False表示成功/失败，可以同时设置多个值。

**可选参数描述：**

|  参数   | 类型  | 说明 |
| ---- | ---- |---------- |
| `StartVoiceChat`    | `str` | 启动智能体url |
| `UpdateVoiceChat`| `str` | 更新智能体url |
| `StopVoiceChat`  | `str` | 关闭智能体url |
| `BotId`  | `str` | 智能体 ID |
| `VoiceId` | `str` | 音色编号。当前仅支持系统音色。|
| `volume` | `int` | 音量大小，0-11 默认 5。 |

**示例：**  
配置信息查询：
```python
# 查询音量大小
TiktokRTC.config('volume')
```
配置信息设置：
```python
# 配置音量大小
TiktokRTC.config(volume=6)
```

### `TiktokRTC.interrupt()`

```python
tiktok.interrupt()
```

打断智能体，语音过程中打断智能体讲话。
