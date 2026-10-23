# audio - Audio Playback

Class feature: Audio playback.

> Currently supported modules：EC600N Series, EC800N Series, EC600M-CN(LA/LE), EC800M-CN(LA/LE/GA), EC600U Series, EC200U Series, EG912U, EG915U and EG915N-EUAG.

**Example:**

```python
# -*- coding: UTF-8 -*-
import audio
from machine import Pin
import utime

def audio_cb(event):
    if event == 0:
        print('audio-play start.')
    elif event == 7:
        print('audio-play finish.')

aud = audio.Audio(0)
aud.setCallback(audio_cb)
# Sets pa
aud.set_pa(Pin.GPIO15,2)
# Plays MP3
aud.play(2, 1, 'U:/music.mp3')
aud.stop()

# Audio stream playback
size = 10*1024 # Ensures that the audio data filled at once is large enough for continuous playback on the underlying layer
format = 4

def play_from_fs():
    file_size = uos.stat("/usr/test.amr")[6]  # Gets the total number of bytes of the file
    print(file_size)
    with open("/usr/test.amr", "rb")as f:   
        while 1:
            b = f.read(size)   # read
            if not b:
                break
            aud.playStream(format, b)
            utime.sleep_ms(20)


play_from_fs()
# Waits for the playback to finish
utime.sleep_ms(5000) 
# Stops the playback so that it does not affect the next playback
aud.stopPlayStream() 
```

## Constructor

### `audio.Audio`

```python
class audio.Audio(device)
```

Creates an audio object.

**Parameter:**

- `device` - Integer type. The output channel. 0 indicates earpiece, 1 indicates headphone and 2 indicates speaker. See the table below for the specific channels supported by each module.

**Channels Supported by the Module:**

| Module Series        | Earpiece    | Headphone   | Speaker     |
| -------------------- | ----------- | ----------- | ----------- |
| EC200N/EC600N/EC800N | Supported   | Unsupported | Unsupported |
| EC600M-CN(LA/LE)     | Supported   | Unsupported | Unsupported |
| EC800M-CN(LA/LE/GA)  | Supported   | Unsupported | Unsupported |
| EG810M               | Supported   | Unsupported | Unsupported |
| EG915N/EG912N        | Supported   | Unsupported | Unsupported |
| EG912U               | Supported   | Unsupported | Unsupported |
| EC200U               | Unsupported | Unsupported | Supported   |
| EC600U               | Supported   | Supported   | Supported   |
| EG915U               | Supported   | Supported   | Unsupported |
| EC600S               | Supported   | Unsupported | Unsupported |
| EC600K/EC800K        | Supported   | Unsupported | Unsupported |
| EC800G-CN(GA/TT/LA)  | Supported   | Supported   | Supported   |
| EC600G-CN(LA)        | Supported   | Supported   | Supported   |

## Methods

### Audio.set_pa

```python
Audio.set_pa(gpio,num)
```

This method sets the GPIO of the output PA.

**Parameter:**

- `gpio` - Integer type. The output GPIO. Refer to [Pin](./machine.Pin.md).
- `num` - Integer type. Number of power-on pulses.

**Return Value**

`1`- Successful execution; `0`- Failed execution.

### Audio.setSpeakerpaSwitch

```python
Audio.setSpeakerpaSwitch(on_off)
```

This method manually switches the PA on or off.

> **Note:** `setSpeakerpaSwitch` takes effect only after the PA has been set up with `set_pa`.

**Parameter**

- `on_off` - Integer type. PA switch state. `1` turns on the PA and `0` turns off the PA.

**Return Value**

`0` - Successful execution; `-1` - Failed execution.

**Example**

```python
from audio import Audio
aud = Audio(0)
aud.setSpeakerpaSwitch(1)  # Turn on the PA
aud.setSpeakerpaSwitch(0)  # Turn off the PA
```

### Audio.setSpeakerpaCallback

```python
Audio.setSpeakerpaCallback(cb)
```

This method sets the PA event notification callback.

**Parameter**

- `cb` - Function type. User callback function. The prototype is as follows:

  ```
  cb(event)
  ```

  **Parameter of the Callback Function**：

  - `event` - Integer type. Event. `0` - PA turned off; `1` - PA turned on.

**Return Value**

`0` - Successful execution; `-1` - Failed execution.

### Audio.set_close_pa_delay

```python
Audio.set_close_pa_delay(time)
```

This method sets the delayed close of the PA to avoid the pop noise at the end of audio playback. The default value is 0 if not set.

**Parameter**

- `time` - Integer type. The time for the delayed close of the PA, in ms.

**Return Value**

`1` - Successful execution; `-1` - Failed execution.

**Example**

```python
from audio import Audio
aud = Audio(0)
aud.set_close_pa_delay(100)  # Close the PA after a delay of 100 ms
```

### Audio.play

```python
Audio.play(priority, breakin, filename)
```

This method plays audio files.

<span id="label_Audio_play_priority">**Description of the Playback Priority:**</span>

It supports the playback of MP3, AMR, and WAV format files. Priorities from 0 to 4 are supported, with a higher number indicating a higher priority. Each priority group supports up to 10 playback tasks simultaneously, and the same playback queue is shared with TTS playback.

> **Note:** Since the TTS and audio file playback share the same playback queue, the playback priority and interruption mode set in the TTS are not only compared with those set in other TTS playback tasks, but also with those set in audio file playback tasks. Similarly, the playback priority and interruption mode set in audio file playback are also valid for those set in TTS tasks.

**Parameter:**

- `priority` - Integer type. Playback priority. Supports priorities from 0 to 4, with a higher number indicating a higher priority.
- `breakin` - Integer type. Interruption mode. 0 indicates the playback is not allowed to be interrupted and 1 indicates the playback is allowed to be interrupted.
- `filename` - File name. String type. The name of the file to be played, including the file storage path. <a href="#label_Audio_desc1">Click here</a> for the description of the playback path.

**Return Value:**

`0` - Successful execution

`-1`- Failed execution

`1` - The task cannot be played immediately and is added to the playback queue.

`-2`- The task can neither be played immediately nor added to the playback queue because the task queue of the priority group for the request has reached its limit.

<span id="label_Audio_desc1">**Description of the Playback Path:**</span>

The user partition path is fixed to start with ’U:/‘, representing the root directory of the user partition. If the user creates an "audio" directory in the root directory and stores the audio files in the "audio" directory, then the path parameter passed in the playback interface should be 'U:/audio/music.mp3'.

### Audio.stop

```python
Audio.stop()
```

This method stops the audio that is currently playing.

**Return Value:**

`0` - Successful execution; `-1`- Failed execution.

### Audio.stopAll

```python
Audio.stopAll()
```

This method stops the playback of the entire queue. That is, if an audio file is currently being played and there are other audio files waiting to be played in the queue, calling this method will not only stop the current playback but also clear the entire queue and no more content is played. If an audio file is currently being played and the playback queue is empty, calling this method is as same as calling Audio.stop().

**Return Value:**

`0` - Successful execution; `-1`- Failed execution.

### Audio.setCallback

```python
Audio.setCallback(cb)
```

This method registers the user's callback function to notify the user of the playback status of the audio file. 

> Note: It is recommended to only perform simple and short operations instead of time-consuming or blocking operations in this callback function.

**Parameter**

- `cb` - Function type. User callback function. The prototype is as follows:

  ```
  cb(event)
  ```

  **Parameter of the Callback Function**：

  -  `event` - Integer type. Playback status. <a href="#label_Audio_map2">click here</a> for the description of this parameter.

**Return Value**

`0` - Successful execution; `-1` - Failed execution.

<span id="label_Audio_map2">**Description of Parameter `event` **</span>

| event | Status                                                       |
| ----- | ------------------------------------------------------------ |
| 0     | Start playback                                               |
| 5     | Pause playback. The playback buffer is empty. The codec is not closed; writing data promptly will allow playback to resume |
| 6     | Playback Resumed                                             |
| 7     | Playback ended. The codec is closed                          |

### Audio.getState

```python
Audio.getState()
```

This method gets the audio initialization state.

**Return Value:**

`0`  - Successful initialization;  `-1` - Failed initialization.

### Audio.getPlayState

```python
Audio.getPlayState()
```

This method gets the audio playback state.

**Parameter:**

None

**Return Value:**

`0` - The audio is idle, with no playback task currently; `1` - The audio is playing.

### Audio.getPlayFile

```python
Audio.getPlayFile()
```

This method gets the audio playback file path and the playback queue.

**Parameter:**

None

**Return Value:**

Returns integer `-1` when the audio is not playing;

On success, returns a tuple containing 3 elements:

- The first element is an integer, indicating the number of files waiting to be played;
- The second element is a string, indicating the path of the file currently being played;
- The third element is a list, containing the paths of all files waiting to be played (arranged in playback order).

**Example:**

```python
from audio import Audio
aud = Audio(0)
aud.play(1,0,'U:/pwron.mp3')
aud.play(2,0,'U:/pwron.wav')
aud.play(3,0,'U:/test.mp3')
file_tup = aud.getPlayFile()
if(file_tup != -1):
    wait_play_num  = file_tup[0]  # number of files waiting to be played
    cur_play_file  = file_tup[1]  # current playback file path
    wait_play_list = file_tup[2]  # all file paths waiting to be played
    str = 'Playing:{},waiting for play={},{}'.format(wait_play_num, cur_play_file, wait_play_list)
    print(str)
```

### Audio.getVolume

```python
Audio.getVolume()
```

This method gets the current playback volume. Range: 0–11. 0 indicates mute. 

**Return Value:**

Volume value in integer type.

### Audio.setVolume

```python
Audio.setVolume(vol)
```

This method sets the playback volume. Range: 0–11. 0 indicates mute.

**Parameter:**

- `vol` - Integer type. Playback volume. Range: 0–11.

**Return Value:**

`0` - Successful execution; `-1` - Failed execution.

### Audio.playStream

```python
Audio.playStream(format, buf)
```

This method plays audio stream. It supports audio stream in MP3, AMR, and WAV format.

> Note: When playing an AMR format audio stream, if the audio comes from a file, it is recommended to skip the first 6 bytes (i.e., the file header) after starting to read the file; otherwise, playback may fail.

**Parameter:**

- `format` - Integer type. The audio stream format. `2` - `WAVPCM`，`3` - `MP3`，`4` - `AMRNB`.
- `buf` - Binary file. The content of the audio stream.

**Return value:**

`0` - Successful execution; `-1` - Failed execution.

### Audio.stopPlayStream

```python
Audio.stopPlayStream()
```

This method stops playing the audio stream.

**Parameter:**

None

**Return Value:**

`0` - Successful execution; `-1` - Failed execution.

### Audio.aud_tone_play

```python
Audio.aud_tone_play(tone, time)
```

This method plays tone, and automatically stops playing after playing for a period of time.

> For EC600N/EC800N series module, the value is immediately returned after calling this method. For EC600U/EC200U series module, the return value is in blocked and wait status.

**Parameter:**

- `tone` - Integer type. The type of tone. Range: `0–15`. Key tone (0~9, A, B, C, D, #, *)，`16`: dialing tone.
- `time` - Integer type. Playback time. Unit: ms. `0` indicates always playing without stopping.

**Return Value:**

`0` - Successful execution; `-1` - Failed execution.

### Audio.aud_tone_play_stop

```python
Audio.aud_tone_play_stop()
```

This method actively stops playing the key tones.

**Return Value:**

`0` - Successful execution; `-1`- Failed execution.

### Audio.montage_play

```python
Audio.montage_play(audio_pack, priority, breakin, play_file_str)
```

This method plays spliced audio files.

> Note: All modules supporting audio support this method, but the firmware must support AMR or MP3 audio decoding. WAV format is currently not supported.

**Parameter:**

- `audio_pack` - String type. The audio pack. The path of the audio pack file. <a href="#label_Audio_montage_desc1">Click here</a> for the description of the audio pack.
- `priority` - Integer type. Playback priority. Supports priorities from 0 to 4, with a higher number indicating a higher priority. <a href="#label_Audio_play_priority">Click here</a> for the description of the playback priority.
- `breakin` - Integer type. Interruption mode. 0 indicates the playback is not allowed to be interrupted and 1 indicates the playback is allowed to be interrupted.
- `play_file_str` - String type. The description of the spliced audio. Plays the audio files in the pack in order. The format is `mp3files=a.mp3+b.mp3`. <a href="#label_Audio_montage_desc2">Click here</a> for the description of the spliced audio.

**Return Value:**

`0` - Successful execution.

`-1` - Failed execution.

`1` - The task cannot be played immediately and is added to the playback queue.

`-2` - The task can neither be played immediately nor added to the playback queue because the task queue of the priority group for the request has reached its limit.

<span id="label_Audio_montage_desc1">**Description of the Audio Pack:**</span>

For the tools and usage of making the audio pack, please contact **Quectel Technical Support**.

<span id="label_Audio_montage_desc2">**Description of the Spliced Audio:**</span>

The spliced audio refers to the audio files that are actually to be played. These files must be included in the audio pack. Declare the audio format at the beginning (i.e., write `mp3files` or `amrfiles` before `=`), and then add the audio files to be played in order, using `+` to separate files.

For example: `mp3files=a.mp3+b.mp3+a.mp3`. `a.mp3` and `b.mp3` are integrated into the audio pack. The audio format added after `=` must be consistent with the format declared at the beginning. The file name does not need to include the path because the audio files have been packed into the audio pack.

**Example:**

```python
import audio
aud = audio.Audio(0)
# test.bin is the audio pack, which contains pwron1.amr and pwron2.amr audio data
aud.montage_play('U:/test.bin',3,0,'amrfiles=pwron1.amr+pwron2.amr')

# para.bin is the audio pack, which contains payment collection and digit audio data
aud.montage_play('U:/para.bin',3,0,'mp3files=payment.mp3+received.mp3+ninety.mp3+six.mp3+yuan.mp3') # Plays "Payment received, ninety-six yuan"
```