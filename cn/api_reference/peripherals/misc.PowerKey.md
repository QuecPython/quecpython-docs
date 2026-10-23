# class PowerKey - power key按键回调注册功能

提供power key事件回调注册功能。

## 构造函数

### `misc.PowerKey`

```python
class misc.PowerKey()
```

**返回值描述：**

返回创建的对象。

**示例：**

```python
from misc import PowerKey
pk = PowerKey()
```

## 方法

### `PowerKey.powerKeyEventRegister`

```python
PowerKey.powerKeyEventRegister(usrFun, pwk_mode) #ECX00U/EG91XU系列支持可选参数pwk_mode

PowerKey.powerKeyEventRegister(usrFun) #其他平台仅支持设置usrFun
```

该方法用于注册powerkey按键的回调函数。

**参数描述：**

- `usrfun`-回调函数，原型usrfun(status)，参数status，int类型：`0`表示松开，`1`表示按下；按下或松开powerkey按键时触发回调。

- `pwk_mode`-（仅ECX00U/EG91XU系列支持）可选参数，powerkey工作模式，int类型：默认为 `0`，`0`表示只在按键松开时才会触发回调函数，并且按键按下的时间需要维持500ms以上；`1`表示按下和松开时，都会触发用户注册的回调函数。

**返回值描述：**

`0`表示注册成功，`-1`表示注册失败。

> ECX00S/ECX00N/ECX00M/EG810M/ECX00E/ECX00G系列，对于powerkey，按下和松开时，都会触发用户注册的回调函数；
>
> ECX00U/EG91XU系列，通过可选参数控制powerkey工作模式，在两种特性间切换：当工作模式为0时，只在按键松开时才会触发回调函数，并且按键按下的时间需要维持500ms以上；工作模式为1时，按下和松开时，都会触发用户注册的回调函数；
>
> 上述所有平台在注册用户的回调函数后，powerkey长按不再触发关机。

### `PowerKey.getpowerKeyStatus`

```python
PowerKey.getpowerKeyStatus()
```

该方法仅EC600M/EC800M系列支持，用于获取powerkey按键的状态。

**返回值描述：**

`0`表示松开，`1`表示按下。

**示例：**

ECX00S/ECX00N/ECX00M/EG810M/ECX00E/ECX00G系列：

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

ECX00U/EG91XU系列：

```python
from misc import PowerKey

pk = PowerKey()

def pwk_callback(status):
	if status == 0:
		print('powerkey release.')
	elif status == 1:
		print('powerkey press.')
      
pk.powerKeyEventRegister(pwk_callback, 0) # 只有按键释放时才会触发回调

pk.powerKeyEventRegister(pwk_callback, 1) # 按键按下和释放时都触发回调
```
