# class PCM - Audio Coding

This class provides read and write interfaces for PCM audio data, the PCM data can be used for audio encoding.

> All models that support audio also support PCM.

## Create Object

### `audio.Audio.PCM`

```python
class audio.Audio.PCM(mic_dev, channel, samplerate, flag，mode， periodcnt)
```

Create an Audio object.

**Parameter Description:**

* For detailed parameter descriptions, please refer to the table below

| Parameter    | Parameter Type | Description                                                     |
| ------------ | -------------- | ------------------------------------------------------------ |
| `mic_dev`    | int      | mic channel:<br>`0` - earpiece<br/>`1` - headset<br/>`2` - auto        |
| `channel`    | int      | Number of channels:<br />`1`:mono( PCM.MONO)<br />`2`:stereo (PCM.STEREO) |
| `samplerate` | int      | Sampling rate:<br />8000, 16000                                    |
| `flag`       | int      | Mode:<br />`0`:read only(PCM.READONLY)<br />`1`:write only(PCM.WRITEONLY)<br />`2`:read/write(PCM.WRITEREAD) |
| `mode`       | int      | Blocking mode (default = 1):<br />`1`:blocking(PCM.BLOCK)<br />`2`:non‑blocking(PCM.NOBLOCK) |
| `periodcnt`  | int      | Buffer size in frames.Range:`[2,25]`<br />Unit: frame. 20 ms per frame. Default = 3,(meaning up to 3 frames of data are stored by default.)|


**example:**

```python
from audio import Audio
pcm = Audio.PCM(1, Audio.PCM.MONO, 8000, Audio.PCM.WRITEREAD, Audio.PCM.BLOCK)
```

## Method

### `PCM.read`

```python
PCM.read(size)
```

This method is used to read the PCM data of the module's MIC recording. 

**Parameter Description:**

- `size` - Maximum amount of data read, of type int, with the unit of ByteDance. (The amount of data read in a single operation does not exceed the size of the set buffer buf)

> Note: The recording time can be calculated based on the sampling rate, number of channels, and sampling precision. The specific calculation formula is:
>
> Time(s) = Amount of data to be obtained (byte) / (Sampling rate (hz) * Number of channels (channel) * Sampling precision (byte/sample) )
>
> For example: Set the sampling rate to 8000, mono, and want to read data with size = 320. The PCM sampling precision of the module is 2, so the recording time of this interface is 0.02s

**Return value description:**

Successfully returned data buffer;
Returns `-1` if it fails. 

### `PCM.write`

```python
PCM.write(buff)
```

This method is used to write PCM data that will be played. 

**Parameter Description:**

- `buff` - PCM data to be written, of type bytearray.

**Return value description:** 

Successfully returns the amount of data written;
Returns `-1` if it fails. 

**Example:**

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

This method is used to close PCM and release the corresponding resources. 

**Return value description:**

`0` indicates success, `-1` indicates failure. 

### `PCM.setVolume`

```python
PCM.setVolume(vol)
```

This method is used to set the playback volume, with the volume value in the range [0, 11], where 0 indicates mute. 

**Parameter Description:**
- `vol` - Volume level, int type, range [0, 11]. 

**Return value description:** 

`0` indicates success, `-1` indicates failure. 

> Note: This method sets the audio playback volume, which has the same effect as Audio.setVolume in setting the volume.

### `PCM.getVolume`

```python
PCM.getVolume()
```

This method is used to obtain the current playback volume level, with the volume value in the range [0, 11], where 0 represents mute. 

**Return value description:** 

Integer volume value. 

## Audio Encoding - G711
This module is used for recording and audio playback in G711 format. 

### Constructor function

```python
class G711(pcm)
```

Create a G711 object.

**Parameter Description:**
- `device` - PCM object, obj type, see [PCM](#Create Object) . (G711 only supports 8K sampling rate by default, please note this issue when creating a PCM object)

**Example:**

```python
import G711
import audio

pcm = audio.Audio.PCM(0, 1, 8000, 2)
g711 = G711(pcm)
```

### `G711.read` 

This method is used to read G711 format recording data, outputting a specified number of frames of recording data at a time (one frame of recording time is 20ms, and the data length is 160 bytes at 8k sampling rate). 

```python
G711.read(format, frame_cnt)
```

**Parameter Description:**

- `format` - Encoding format, int type, `0` : A-LAW, `1` : U-LAW.
- `frame_cnt` - Number of frames to read, int type, optional, default value is 1.

**Return value description:** 

If successful, return the audio buffer; 

Returns `-1`  if it fails. 

> Note: Usually, after PCM data is encoded in G711 format, the data volume is only 1/2 of the original; conversely, after G711 is decoded to PCM, the data volume is twice the original.

### `G711.write`

This method is used to decode G711 audio data and play it. 

```python
G711.write(buffer, format)
```

**Parameter Description:**

- `format` - Encoding format of audio data, int type, `0`: A-LAW, `1` : U-LAW.
- `buff` - Audio data to be decoded, of type bytearray.

**Return value description:**

Successfully returns the decoded data volume written;
Returns `-1`  if it fails. 

**Example:**

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


## Audio Encoding - G722

This module is used for recording and audio playback in G722 format. 

### Constructor function

```python
class G722(pcm, bitrate)
```

Create a G722 object.

**Parameter Description:**

- `device` - PCM object, obj type, see [PCM](#Create Object). (G722 supports 16K sampling rate by default, please note this issue when creating a PCM object)
- `bitrate` - Bitrate, int type, optional values: `64000` , `56000` , `48000` . 

**Example:**

```python
import G722
import audio

pcm = audio.Audio.PCM(0, 1, 16000, 2)
g722 = G722(pcm, 64000)
```

### G722.read

This method is used to read G722 format recording data, outputting one frame of recording data at a time (one frame of recording time is 20ms, and the data length is 160 bytes at a 16k sampling rate). 

```python
G722.read()
```

**Return value description:**
 
If successful, return the audio buffer; 

If it fails, an error will be reported directly. 

> Note: Usually, after PCM data is encoded using G722, the data volume is only 1/4 of the original; conversely, after G722 is decoded to PCM, the data volume is 4 times the original.

### `G722.write`

This method is used to decode G722 audio data into PCM and place it in the playback buffer for playback. 

```python
G722.write(buffer)
```

**Parameter Description:**

- `buff` - Audio data to be decoded, of type bytearray.

**Return value description:**
 
Successfully returns the amount of data written;
Returns `-1` if it fails. 

**Example:**

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

## Audio Encoding - G729

This module is used for recording and audio playback in G729 format. 

### Constructor function

```python
class G729(pcm)
```

Create a G729 object. (G729 supports the 8K sampling rate by default, so please note this issue when creating a PCM object)

**Parameter Description:**

- `device` - PCM object, obj type, see [PCM](#Create Object) . 

**Example:**

```python
import G729
import audio

pcm = audio.Audio.PCM(0, 1, 8000, 2)
g729 = G729(pcm)
```


### `G729.read`

This method is used to read G722 format recording data, outputting one frame of recording data at a time (the length of one frame of recording data is 20 bytes). 

```python
G729.read()
```

**Return value description:**
 
If successful, return the audio buffer; 

If it fails, an error will be reported directly. 

> Note: Usually, after PCM data is encoded using G729, the data volume is only 1/16 of the original; conversely, after G729 is decoded into PCM, the data volume is 16 times the original.

### `G729.write`

This method is used to decode G729 audio data into PCM and place it in the playback buffer for playback. 

```python
G729.write(buffer)
```

**Parameter Description:**

- `buff` - Audio data to be decoded, of type bytearray.

**Return value description:**

Successfully returns the amount of data written;

Returns `-1`  if it fails.

**Example:**

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