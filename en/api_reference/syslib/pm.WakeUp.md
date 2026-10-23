# class WakeUp - Wake-Up Interrupt

The ordinary GPIOs of the EC600/EC800E series do not support double-edge trigger interrupts, and do not support interrupts to wake up the module during sleep. WakeUp has no pull-up/pull-down restrictions, supports double-edge trigger interrupts and waking up the module during sleep by default. The usage is as follows:

## **Constructor** 

```python
class pm.WakeUp(WakeupID, pull)
```

**Parameter:**

- `WakeupID` - The wakeup number to control, described as follows:<br />EC800E:<br />`WAKEUP2` - Pin 79<br/>`WAKEUP3` - Pin 109 (unavailable for EC800ECN_LE&LQ&LC)<br />`WAKEUP4` - Pin 108 (unavailable for EC800ECN_LE&LQ&LC)<br />`WAKEUP5` - Pin 19

  EC600E:<br />`WAKEUP2` - Pin 9<br/>`WAKEUP3` - Pin 51<br/>`WAKEUP4` - Pin 50<br/>`WAKEUP5` - Pin 39

- `pull` - Pull-up/pull-down mode, int type, described as follows:<br />`PULL_DISABLE` – Floating mode 0<br />`PULL_PU` – Pull-up mode 1<br />`PULL_PD` – Pull-down mode 2<br />

**Example:**

```python
from pm import WakeUp  
def fun(level):
    print(str(level))

wake=WakeUp(WakeUp.WAKEUP5,WakeUp.PULL_PD) 
wake.set_callback(fun)# Set callback, callback only returns level  
wake.enable()# Enable interrupt  

wake.read() #Read pin level
```

## Methods  

### `wake.enable`  

```python
wake.enable() 
```

This method is used to enable the WakeUp interrupt. When a rising or falling edge is detected on the WakeUp pin, the callback will be executed.  

**Return Value:**  

Returns integer value `0` for success, `-1` for failure.

### `wake.disable`  

```python  
wake.disable()
```

This method is used to disable the interrupt and disable the interrupt associated with the WakeUp object.   

**Return Value:**

Returns integer value `0` for success, `-1` for failure. 

### `wake.read`

```python
wake.read()  
```

This method is used to read the current pin level.  

**Return Value:**   

Returns pin level, `0` means low pin level, `1` means high pin level.  

### `wake.deinit`  

```python  
wake.deinit()
```

Disable WakeUp function.  

**Return Value:**

Returns integer value `0` for success, `-1` for failure.  

### `wake.set_callback`  
<p>Note:</p>
<p>The EG800Z series supports a "deeper sleep" state, which is at a higher sleep level than autosleep. When the module enters this state, the RAM loses power. In this case, although WakeUp can wake up the module through an interrupt, the Python callback cannot be triggered during the wake-up process. After WakeUp wakes up the module through the interrupt and the Python virtual machine restarts, the callback can be triggered normally as long as the module does not enter the deeper sleep state again.</p>


```python  
wake.set_callback(fun)
```

This method is used to set the callback. After the interrupt is triggered, this callback will be executed and return the level.  

**Parameter:**   

- `fun` - Callback function with prototype: 

  ```
  fun(level)
  ```

  Callback function parameter description:  

  - level:`0`: low level, `1`: high level.  

| Module | WakeUp Pin Description                                       |
| ------ | ------------------------------------------------------------ |
| EC800E | `WAKEUP2` - Pin 79<br/>`WAKEUP3` - Pin 109 (unavailable for EC800ECN_LE&LQ&LC)<br />`WAKEUP4` - Pin 108 (unavailable for EC800ECN_LE&LQ&LC)<br />`WAKEUP5` - Pin 19 |
| EC600E | `WAKEUP2` - Pin 9<br/>`WAKEUP3` - Pin 51<br/>`WAKEUP4` - Pin 50<br/>`WAKEUP5` - Pin 39 |
| EC800Z | `WAKEUP0` - Pin 87(The low-voltage module is pin 107)<br/>`WAKEUP2` - Pin 79<br/>`WAKEUP3` - Pin 109<br/>`WAKEUP4` - Pin 108<br/>`WAKEUP5` - Pin 19 |
| EG800Z | `WAKEUP0` - Pin 87<br/>`WAKEUP2` - Pin 79<br/>`WAKEUP3` - Pin 5<br/>`WAKEUP4` - Pin 6<br/>`WAKEUP5` - Pin 19 |