# class OneWire – One-Wire Protocol

This class provides One-Wire communication functionality. This feature is currently only supported on the EC200U/EC600U/EG912U/EG915U series.

## Constructor

### `machine.OneWire` 

```python
class machine.OneWire(GPIO)
```

**Parameter:**

- `GPIO` - The gpio used, int type.


**Example:**

```python
from machine import OneWire
obj=OneWire(OneWire.GPIO19)# Create a One-Wire object using GPIO19
```

## Methods  

### `OneWire.reset`

```python 
OneWire.reset()
```

This method is used to reset the bus and detect if a device responds. It needs to be called before calling read/write methods.  

**Return Value:**

Returns int value `0` indicating correct response, returns int value `-1` indicating no response.

### `OneWire.read`

```python
OneWire.read(len)  
```

This method is used to read data.

**Parameter:**

- `len` - The length of data to read, int type.  

**Return Value:**

Returns the read bytes object.

### `OneWire.write`

```python
OneWire.write(data)
```

This method is used to write data.  

**Parameter:**

- `data` - The data to write, bytes type.

**Return Value:** 

Returns int value `0`.

**Example:** Read DS18B20 data  

> Needs to work with DS18B20!

```python
from machine import OneWire  

obj=OneWire(OneWire.GPIO19)  
data1=bytearray([0xcc,0x44])
data2=bytearray([0xcc,0xbe])

obj.reset()#First reset bus and detect if slave device responds, then read/write
obj.write(data1) 
obj.reset()
obj.write(data2)
obj.read(2)#The read data is the raw DS18B20 data, needs to be converted to actual temperature based on DS18B20 data format
```
