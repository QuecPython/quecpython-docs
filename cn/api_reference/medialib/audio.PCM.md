# class PCM - PCM音频数据编码

该类提供PCM音频数据的读写接口，PCM数据可用于音频编码。

> 所有支持audio的型号均支持PCM。

## 创建对象

### `audio.Audio.PCM`

```python
class audio.Audio.PCM(mic_dev, channel, samplerate, flag，mode， periodcnt)
```

创建Audio对象。

**参数描述：**


* 参数说明详见下表

| 参数         | 参数类型 | 参数说明                                                     |
| ------------ | -------- | ------------------------------------------------------------ |
| `mic_dev`    | int      | mic通道：<br>`0` - 听筒<br/>`1` - 耳机<br/>`2` - 自动        |
| `channel`    | int      | 通道数：<br />`1`：单声道( PCM.MONO)<br />`2`：立体音(PCM.STEREO) |
| `samplerate` | int      | 采样率：<br />8000, 16000                                    |
| `flag`       | int      | 模式：<br />`0`：只读(PCM.READONLY)<br />`1`：只写(PCM.WRITEONLY)<br />`2`：读写(PCM.WRITEREAD) |
| `mode`       | int      | 阻塞（缺省值为1）：<br />`1`：阻塞式(PCM.BLOCK)<br />`2`：非阻塞式(PCM.NOBLOCK) |
| `periodcnt`  | int      | 缓存buf大小，取值范围：`[2,25]`<br />单位为帧。 一帧20ms，缺省值为3（即默认最多储存3帧数据） |

**示例：**

```python
from audio import Audio
pcm = Audio.PCM(1, Audio.PCM.MONO, 8000, Audio.PCM.WRITEREAD, Audio.PCM.BLOCK)
```

## 方法

### `PCM.read`

```python
PCM.read(size)
```

该方法用于读取模组MIC录音的PCM数据。

**参数描述：**

- `size` - 最大读取数据量，int类型，单位字节。（单次读取的数据量不超过设置的缓存buf大小）

> 注：录音时间可根据采样率，通道数，采样精度计算，具体计算公式为：
>
> 时间(s)=想要获取的数据量(byte)  /  (采样率(hz) * 通道数(channel) * 采样精度(byte/sample) )
>
> 例如：设置采样率8000，单声道，想读取数据size=320。模组PCM采样精度为2，所以该接口录音时间为0.02s

**返回值描述：**

成功返回数据buffer;

失败则返回`-1`。

### `PCM.write`

```python
PCM.write(buff)
```

该方法用于写入将要进行播放的PCM数据。

**参数描述：**

- `buff` - 需要写入的PCM数据，bytearray类型。

**返回值描述：**

成功返回写入的数据量;

失败则返回`-1`。

**示例：**

```python
from audio import Audio
pcm = Audio.PCM(1, Audio.PCM.MONO, 8000, Audio.PCM.WRITEREAD, Audio.PCM.BLOCK)

pcm_buf = pcm.read(320)
pcm.write(pcm_buf)
```

### `PCM.close`

```python
PCM.close()
```

该方法用于关闭PCM，释放相应资源。

**返回值描述：**

`0` 表示成功，`-1`表示失败。

### `PCM.setVolume`

```python
PCM.setVolume(vol)
```

该方法用于设置播放音量大小，音量值在区间[0 ~ 11]，0表示静音。

**参数描述：**

- `vol` - 音量大小，int类型，区间[0 ~ 11]。

**返回值描述：**

`0` 表示成功，`-1`表示失败。

> 注：该方法设置的是audio播放音量，与Audio.setVolume设置音量效果相同

### `PCM.getVolume`

```python
PCM.getVolume()
```

该方法用于获取当前播放音量大小，音量值在区间[0 ~ 11]，0表示静音。

**返回值描述：**

整型音量大小值。



## 音频编码-G711

该模块用于G711格式的录音和音频播放。

### 构造函数

```python
class G711(pcm)
```

创建G711对象。

**参数描述：**

- `device` - PCM对象，obj类型，见[PCM](#创建对象)。（G711默认只支持8K采样率，创建PCM对象时请注意此问题）

**示例：**

```python
import G711
import audio

pcm = audio.Audio.PCM(0, 1, 8000, 2)
g711 = G711(pcm)
```

### `G711.read`

该方法用于读取G711格式录音数据，单次输出指定帧数的录音数据（一帧录音时间为20ms，8k采样率下数据长度为160字节）。

```python
G711.read(format, frame_cnt)
```

**参数描述：**

- `format` - 编码格式，int类型，`0`：A-LAW，`1`：U-LAW。
- `frame_cnt` - 读取的帧数，int类型，可缺省，默认值为1。

**返回值描述：**

成功则返回音频buffer；

失败则返回`-1`。

> 注：通常PCM数据按G711格式编码后数据量只有原先的1/2，反之G711解码为PCM后数据量为原先的2倍。


### `G711.write`

该方法用于将G711音频数据解码，并播放。

```python
G711.write(buffer, format)
```

**参数描述：**

- `format` - 音频数据的编码格式，int类型，`0`：A-LAW，`1`：U-LAW。
- `buff` - 需要解码的音频数据，bytearray类型。

**返回值描述：**

成功返回写入的解码后的数据量;

失败则返回`-1`。

**示例：**

```python
import G711
import audio
import uos

pcm = audio.Audio.PCM(0, 1, 8000, 2)
g711 = G711(pcm)
#g711 code
def g711_compress(frame):
    f = open("/usr/test.g711", "wb")
    count = frame
    while count:
        count-=1
        g711_buf = g711.read(0)
        print("len:",len(g711_buf))
        if(len(g711_buf) > 0):
            f.write(g711_buf)
    f.close()
#g711 decode
def g711_decompression():
    file_size = uos.stat("/usr/test.g711")[6]  # get file size
    print(file_size)
    with open("/usr/test.g711", "rb")as f:   
        while 1:
            b = f.read(320)
            if not b:
                break
            write_len = g711.write(b, 0)
        f.close()

if __name__ == "__main__":
    g711_compress(200)
    g711_decompression()
```



## 音频编码-G722

该模块用于G722格式的录音和音频播放。

### 构造函数

```python
class G722(pcm, bitrate)
```

创建G722对象。

**参数描述：**

- `device` - PCM对象，obj类型，见[PCM](#创建对象)。（G722默认支持16K采样率，创建PCM对象时请注意此问题）
- `bitrate` - 比特率，int类型，可选值：`64000` , `56000`，`48000`。

**示例：**

```python
import G722
import audio

pcm = audio.Audio.PCM(0, 1, 16000, 2)
g722 = G722(pcm, 64000)
```

### `G722.read`

该方法用于读取G722格式录音数据，单次输出一帧录音数据（一帧录音时间为20ms，16k采样率下数据长度为160字节）。

```python
G722.read()
```

**返回值描述：**

成功则返回音频buffer；

失败则直接报错。

> 注：通常PCM数据按G722编码后数据量只有原先的1/4，反之G722解码为PCM后数据量为原先的4倍。

### `G722.write`

该方法用于将G722音频数据解码成PCM，并放入播放缓存中进行播放。

```python
G722.write(buffer)
```

**参数描述：**

- `buff` - 需要解码的音频数据，bytearray类型。

**返回值描述：**

成功返回写入的数据量;

失败则返回`-1`。

**示例：**

```python
import audio
import G722
import uos

pcm = audio.Audio.PCM(0, 1, 16000, 2, 1)
g722 = G722(pcm, 64000)
#g722 code
def g722_compress(frame):
    f = open("/usr/test.g722", "wb")
    count = frame
    while count:
        count-=1
        g722_buf = g722.read()
        print("len:",len(g722_buf))
        if(len(g722_buf) > 0):
            f.write(g722_buf)
    f.close()
#g722 decode
def g722_decompression():
    file_size = uos.stat("/usr/test.g722")[6]  # get file size
    print("size=",file_size)
    with open("/usr/test.g722", "rb")as f:   
        while 1:
            b = f.read(160)
            if not b:
                break
            write_len = g722.write(b)
        f.close()

if __name__ == "__main__":
    g722_compress(200)
    g722_decompression()
```



## 音频编码-G729

该模块用于G729格式的录音和音频播放。

### 构造函数

```python
class G729(pcm)
```

创建G729对象。（G729默认支持8K采样率，创建PCM对象时请注意此问题）

**参数描述：**

- `device` - PCM对象，obj类型，见[PCM](#创建对象)。

**示例：**

```python
import G729
import audio

pcm = audio.Audio.PCM(0, 1, 8000, 2)
g729 = G729(pcm)
```

### `G729.read`

该方法用于读取G722格式录音数据，单次输出一帧录音数据（一帧录音数据长度为20字节）。

```python
G729.read()
```

**返回值描述：**

成功则返回音频buffer；

失败则直接报错。

> 注：通常PCM数据按G729编码后数据量只有原先的1/16，反之G729解码为PCM后数据量为原先的16倍。


### `G729.write`

该方法用于将G729音频数据解码成PCM，并放入播放缓存中进行播放。

```python
G729.write(buffer)
```

**参数描述：**

- `buff` - 需要解码的音频数据，bytearray类型。

**返回值描述：**

成功返回写入的数据量;

失败则返回`-1`。

**示例：**

```python
import audio
import G729
import uos

pcm = audio.Audio.PCM(0, 1, 8000, 2, 1)
g729 = G729(pcm)
#g729 code
def g729_compress(frame):
    f = open("/usr/test.g729", "wb")
    count = frame
    while count:
        count-=1
        g729_buf = g729.read()
        print("len:",len(g729_buf))
        if(len(g729_buf) > 0):
            f.write(g729_buf)
    f.close()
#g729 decode
def g729_decompression():
    file_size = uos.stat("/usr/test.g729")[6]  # get file size
    print("size=",file_size)
    with open("/usr/test.g729", "rb")as f:   
        while 1:
            b = f.read(20)
            if not b:
                break
            write_len = g729.write(b)
        f.close()

if __name__ == "__main__":
    g729_compress(200)
    g729_decompression()
```

