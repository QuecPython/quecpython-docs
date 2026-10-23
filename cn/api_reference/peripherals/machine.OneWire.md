# class OneWire – 单总线

该类提供单总线通信功能。该功能目前仅EC200U/EC600U/EG912U/EG915U系列支持。

## 构造函数

### `machine.OneWire`

```python
class machine.OneWire(GPIO)
```

**参数描述：**

- `GPIO` - 使用的gpio，int类型。


**示例：**

```python
from machine import OneWire
obj=OneWire(OneWire.GPIO19)# 创建单总线对象，使用GPIO19
```

## 方法

### `OneWire.reset`

```python
OneWire.reset()
```

该方法用于复位总线并检测设备是否响应，使用时需要先调用该接口，再调用读写接口。

**返回值描述：**

返回整型值`0`表示正确响应，返回整型值`-1`表示没有响应。

### `OneWire.read`

```python
OneWire.read(len)
```

该方法用于读取数据。

**参数描述：**

- `len` - 读取数据的长度，int类型。

**返回值描述：**

返回读取到的bytes对象。

### `OneWire.write`

```python
OneWire.write(data)
```

该方法用于写入数据。

**参数描述：**

- `data` - 写入的数据，bytes类型。

**返回值描述：**

返回整型值`0`。

**使用示例：**读取DS18B20数据

> 需要配合DS18B20使用！

```python
from machine import OneWire

obj=OneWire(OneWire.GPIO19)
data1=bytearray([0xcc,0x44])
data2=bytearray([0xcc,0xbe])

obj.reset()#先进行总线复位、检测从设备是否响应，再进行读写
obj.write(data1)
obj.reset()
obj.write(data2)
obj.read(2)#读取的为DS18B20原始数据，需按照DS18B20的数据格式换算成实际温度
```
