# class PowerKey - PowerKey Callback and Registration

This class provides the feature of triggering the callback function when registering the powerkey event. 

## Constructors

### `misc.PowerKey`

```python
class misc.PowerKey()
```

**Return Value:**

Returns the created object. 

**Example：**

```python
from misc import PowerKey
pk = PowerKey()
```

## Methods

### `PowerKey.powerKeyEventRegister`

```python
PowerKey.powerKeyEventRegister(usrFun, pwk_mode) # ECX00U/EG91XU series supports the optional 'pwk_mode' parameter

PowerKey.powerKeyEventRegister(usrFun) # Other platforms only support 'usrFun' parameter
```

This method registers the callback function for the powerkey event.

**Parameter:**

- `usrfun` - Callback function whose prototype is usrfun (status). The parameter is status with `0` indicating to release and `1` indicating to press. The callback will be triggered when pressing or releasing the powerkey.

- `pwk_mode` - (Supported only by ECX00U/EG91XU series) This parameter is optional. powerkey working mode, int type: 0 by default, 0 indicates that the callback function is triggered only when the key is released, and the key must be held down for at least 500ms. '1' means that when pressed and released, both trigger the callback function registered by the user.

**Return Value:**

`0` - Successful registration

`-1 ` - Failed registration


> Note:<br>
> For ECX00N/ECX00M/EG810M series modules: The callback function will be triggered when pressing and releasing the powerkey. 
>
> For ECX00U/EG91XU series: Optional parameters are used to control the powerkey working mode and switch between the two features: When the working mode is 0, the callback function will be triggered only when releasing the powerkey and the key have been pressed for at least 500 ms. When the working mode is 1, the callback function will be triggered when pressing and releasing the powerkey. 
>
> For all the above platforms, powerkey long press no longer triggers shutdown after registering the user's callback function.

**Example:**

For ECX00N/ECX00M/EG810M series modules:

```python
from misc import PowerKey

pk = PowerKey()

def pwk_callback(status):
	if status == 0:
		print('powerkey release.')
	elif status == 1:
		print('powerkey press.')
        
pk.powerKeyEventRegister(pwk_callback)
```

For ECX00U/EG91XU series modules：

```python
from misc import PowerKey

pk = PowerKey()

def pwk_callback(status):
	if status == 0:
		print('powerkey release.')
	elif status == 1:
		print('powerkey press.')
      
pk.powerKeyEventRegister(pwk_callback, 0) # The callback will be triggered only when the power key is released

pk.powerKeyEventRegister(pwk_callback, 1) # The callback function will be triggered when pressing and releasing the powerkey. 
```