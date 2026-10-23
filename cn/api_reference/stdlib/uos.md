# uos - 基本系统服务

`uos`模块包含文件系统访问和挂载构建，该模块实现了CPython模块相应模块的子集。更多信息请参阅CPython文档：[os](https://docs.python.org/3.5/library/os.html#module-os)

## 删除文件

### `uos.remove`

```python
uos.remove(path)
```

删除文件。

**参数描述**

* `path`，字符串，表示文件名。

## 改变当前目录

### `uos.chdir`

```python
uos.chdir(path)
```

改变当前目录。

**参数描述**

* `path`，字符串，表示目录名。

## 获取当前路径

### `uos.getcwd`

```python
uos.getcwd()
```

获取当前路径。

**返回值描述**

字符串，当前路径

## 列出指定目录文件

### `uos.listdir`

```python
uos.listdir( [dir] )
```

没有参数列出当前目录文件，否则列出给定目录的文件。

**参数描述**

* `dir`为字符串，可选参数，表示目录名，默认为 ‘/’ 目录。

**返回值描述**

元组，列出路径下所有存在的对象（目录&文件）

**示例：**

```python
>>> uos.listdir()
[‘file1’, ‘read.txt’, ‘demo.py’]
```

## 创建新目录

### `uos.mkdir`

```
uos.mkdir(path)
```

创建一个新的目录。

**参数描述**

* `path`表示准备创建的目录名，为所在目录的相对路径。

**示例：**

```python
>>> uos.mkdir('testdir')
>>> uos.listdir()
[‘file1’, ‘read.txt’, ‘demo.py’, 'testdir']
```

## 重命名文件

### `uos.rename`

```python
uos.rename(old_path, new_path)
```

重命名文件。

**参数描述**

* `old_path`，字符串，表示旧文件或目录名，
* `new_path`，字符串，表示新文件或目录名。

**示例：**

```python
>>> uos.rename('testdir', 'testdir1')
```

## 删除指定目录

### `uos.rmdir`

```python
uos.rmdir(path)
```

删除指定目录。

**参数描述**

* `path`，字符串，表示目录名，为所在目录的相对路径。

**示例：**

```python
>>> uos.rmdir('testdir')
>>> uos.listdir()
[‘file1’, ‘read.txt’, ‘demo.py’]
```

## 列出当前目录参数

### `uos.ilistdir`

```python
uos.ilistdir( [dir] )
```

该函数返回一个迭代器，该迭代器会生成所列出条目对应的3元组。

**参数描述**

* `dir`为可选参数，字符串，表示目录名，没有参数时，默认列出当前目录，有参数时，则列出dir参数指定的目录。

**返回值描述**

返回一个迭代器，该迭代器会生成所列出条目对应的3元组

元组的形式为 `(name, type, inode[, size])`:

* `name` 是条目的名称，字符串类型，如果dir是字节对象，则名称为字节;
* `type` 是条目的类型，整型数，0x4000表示目录，0x8000表示常规文件；
* `inode`是一个与文件的索引节点相对应的整数，对于没有这种概念的文件系统来说，可能为0；
* 一些平台可能会返回一个4元组，其中包含条目的size。对于文件条目，size表示文件大小的整数，如果未知，则为-1。对于目录项，其含义目前尚未定义。

## 获取文件或目录的状态

### `uos.stat`

```python
uos.stat(path)
```

获取文件或目录的状态。

**参数描述**

* `path`，字符串，表示文件或目录名。

**返回值描述**

返回值是一个元组，返回值形式为：

`(mode, ino, dev, nlink, uid, gid, size, atime, mtime, ctime)`

* `mode` – inode保护模式
* `ino` – inode节点号
* `dev`  – inode驻留的设备
* `nlink`  – inode的链接数
* `uid ` – 所有者的用户ID
* `gid`  – 所有者的组ID
* `size`  – 文件大小，单位字节
* `atime`  – 上次访问的时间
* `mtime`  – 最后一次修改的时间
* `ctime`  – 操作系统报告的“ctime”，在某些系统上是最新的元数据更改的时间，在其它系统上是创建时间，详细信息参见平台文档

## 获取文件系统状态信息

### `uos.statvfs`

```python
uos.statvfs(path)
```

获取文件系统状态信息。

**参数描述**

* `path`，字符串，表示文件或目录名。

**返回值描述**

返回一个包含文件系统信息的元组：

`(f_bsize, f_frsize, f_blocks, f_bfree, f_bavail, f_files, f_ffree, f_favail, f_flag, f_namemax)`

* `f_bsize` – 文件系统块大小，单位字节
* `f_frsize` – 分栈大小，单位字节
* `f_blocks` – 文件系统数据块总数
* `f_bfree` – 可用块数
* `f_bavai` – 非超级用户可获取的块数
* `f_files`  – 文件结点总数
* `f_ffree` – 可用文件结点数
* `f_favail` – 超级用户的可用文件结点数
* `f_flag` – 挂载标记
* `f_namemax` – 最大文件长度，单位字节

**示例：**

```python
>>> import uos
>>> res = uos.statvfs("main.py")
>>> print(res)
(4096, 4096, 256, 249, 249, 0, 0, 0, 0, 255)
```

## 获取关于底层信息或其操作系统的信息

### `uos.uname`

```python
uos.uname()
```

获取关于底层信息或其操作系统的信息。

**返回值描述**

该接口与micropython官方接口返回值形式有所区别，返回一个元组，形式为：

`(sysname, nodename, release, version, machine)`

* `sysname` – 底层系统的名称，string类型
* `nodename` – 网络名称(可以与 sysname 相同) ，string类型
* `release` – 底层系统的版本，string类型
* `version` – MicroPython版本和构建日期，string类型
* `machine` – 底层硬件(如主板、CPU)的标识符，string类型
* `qpyver` – QuecPython 短版本号，string类型

**示例：**

```python
>>> import uos
>>> uos.uname()
('sysname=EC600S-CNLB', 'nodename=EC600S', 'release=1.12.0', 'version=v1.12 on 2020-06-23', 'machine=EC600S with QUECTEL', 'qpyver=V0001')
>>> uos.uname()[0].split('=')[1] # 可通过这种方式来获取sysname的值
'EC600S-CNLB'
```

### `uos.uname2`

```python
uos.uname2()
```

获取关于底层信息或其操作系统的信息。

**返回值描述**

该接口与micropython官方接口返回值形式一致。注意与上面uos.uname()接口返回值的区别，返回值形式为：

`(sysname, nodename, release, version, machine, qpyver)`

* `sysname` – 底层系统的名称，string类型
* `nodename` – 网络名称(可以与 sysname 相同) ，string类型
* `release` – 底层系统的版本，string类型
* `version` – MicroPython版本和构建日期，string类型
* `machine` – 底层硬件(如主板、CPU)的标识符，string类型
* `qpyver` – QuecPython 短版本号，string类型

**示例：**

```python
>>> import uos
>>> uos.uname2()
(sysname='EC600S-CNLB', nodename='EC600S', release='1.12.0', version='v1.12 on 2020-06-23', machine='EC600S with QUECTEL', qpyver='V0001')
>>> uos.uname2().sysname  # 可通过这种方式直接获取sysname的值
'EC600S-CNLB'
>>> uos.uname2().machine
'EC600S with QUECTEL'
```

## 返回具有*n个*随机字节的bytes对象

### `uos.urandom`

```python
uos.urandom(n)
```

返回具有*n个*随机字节的bytes对象，如果模组搭载了硬件随机数生成器，它就会由硬件随机数生成器生成。

**参数描述**

* `n`，整型，随机字节的个数

**返回值描述**

具有*n个*随机字节的bytes对象

**示例：**

```python
>>> import uos
>>> uos.urandom(5)
b'\xb3\xc9Y\x1b\xe9'
```

## 注册存储设备 - SPI - SD卡

> 目前仅ECx00N&EG91xN平台支持。

### `uos.VfsFat`

```python
uos.VfsFat(spi_port, spimode, spiclk, spics)
```

初始化SD卡，和SD卡通信。使用SPI通信方式。

**参数描述**

* `spi_port`，int，通道选择[0,1]
* `spimode`，int，SPI 的工作模式(模式0最常用):<br />
|参数|工作模式|
| ---- | ---- |
|   0   |CPOL=0, CPHA=0|
|   1   | CPOL=0, CPHA=1|
|   2  |CPOL=1, CPHA=0|
|   3  |CPOL=1, CPHA=1|
> 时钟极性CPOL: 即SPI空闲时，时钟信号SCLK的电平（0:空闲时低电平; 1:空闲时高电平）

* `spiclk`， int

|参数|时钟频率|
| ---- | ---- |
|   0   |812.5kHz|
|   1   |1.625MHz|
|   2  |3.25MHz|
|   3  |6.5MHz|
|   4  |13MHz|

* `spics`，int，指定CS片选引脚为任意GPIO，硬件CS可以接这里指定的脚，也可以接默认的SPI CS脚

> 1-n:指定Pin.GPIO1-Pin.GPIOn为CS脚

**返回值描述**

成功则返回VfsFat object，失败则会报错。

**示例:**

```python
>>> cdev = uos.VfsFat(1, 0, 4, 1)
>>> uos.mount(cdev, '/sd')
>>> f = open('/sd/test.txt','w+')
>>> f.write('0123456')
>>> f.close()
>>> uos.listdir('/sd')
>>> f = open('/sd/test.txt','r')
>>> f.read()
>>> f.close()
```

## 注册存储设备 - SDIO - SD卡

> 目前仅EC600U/EC200U/EC200A平台支持。

### `uos.VfsSd`

```python
uos.VfsSd(str)
```

初始化SD卡，使用SDIO通信方式。

**参数描述**

* `str`，str类型，固定为"sd_fs"，后续扩展。

**返回值描述**

成功则返回vfs object，失败则会报错。

**引脚说明**

| 平台   | 引脚                                                                                                            |
| ------ | --------------------------------------------------------------------------------------------------------------- |
| EC600U | CMD:引脚号48<br />DATA0:引脚号39<br />DATA1:引脚号40<br />DATA2:引脚号49<br />DATA3:引脚号50<br />CLK:引脚号132 |
| EC200U | CMD:引脚号33<br />DATA0:引脚号31<br />DATA1:引脚号30<br />DATA2:引脚号29<br />DATA3:引脚号28<br />CLK:引脚号32  |
| EC200A | CMD:引脚号33<br />DATA0:引脚号31<br />DATA1:引脚号30<br />DATA2:引脚号29<br />DATA3:引脚号28<br />CLK:引脚号32  |

**示例：**

```python
>>> from uos import VfsSd
>>> udev = VfsSd("sd_fs")
```

## 设置SD卡检测管脚

### `uos.set_det`

```python
uos.set_det(vfs_obj.GPIOn,mode)
```

指定sd卡插拔卡的检测管脚和模式。

**参数描述**

* `vfs_obj.GPIOn`，int类型，用于sd卡插拔卡检测的GPIO引脚号，参照[Pin](../peripherals/machine.Pin.md)模块的定义
* `mode`，int类型 <br />0:sd卡插上后，检测口为低电平；sd卡取出后，检测口为高电平 <br />1:sd卡插上后，检测口为高电平；sd卡取出后，检测口为低电平

**返回值描述**

成功返回 `0`，失败返回 `-1`。

**示例：**

```python
>>> from uos import VfsSd
>>> udev = VfsSd("sd_fs")
>>> uos.mount(udev, '/sd')
>>> udev.set_det(udev.GPIO10,0)#使用GPIO10作为卡检测管脚，sd卡插上，检测口为低电平，sd卡取出，检测口为高电平（实际使用根据硬件）
```

## 设置插拔SD卡回调函数

### `uos.set_callback`

```python
uos.set_callback(fun)
```

设定发生插拔卡事件时的用户回调函数。

**参数描述**

* `fun`，插拔卡的回调函数，function类型，原型为fun(args),参数`args`为int类型，0表示拔卡，1表示插卡

**返回值描述**

成功返回 `0`，失败返回 `-1`。

**SD卡使用示例（SDIO接口）**

> 目前仅EC600U/EC200U/EC200A平台支持。

**示例：**

```python
from uos import VfsSd
import ql_fs
udev = VfsSd("sd_fs")
uos.mount(udev, '/sd')
udev.set_det(udev.GPIO10,0)
#文件读写
f = open('/sd/test.txt','w+')
f.write('1234567890abcdefghijkl')
f.close()
uos.listdir('/sd')
f = open('/sd/test.txt','r')
f.read()
f.close()
#插拔卡回调函数
def call_back(para):
    if(para == 1):
        print("insert")
        print(uos.listdir('/usr'))  
        print(ql_fs.file_copy('/usr/1.txt','/sd/test.txt'))#复制sd卡里的test.txt内容到usr下的1.txt中
        print(uos.listdir('/usr'))
    elif(para == 0):
        print("plug out")   
  
udev.set_callback(call_back)
```

## 注册存储设备 - SPI NOR FLASH

使用SPI通信方式将外挂 SPI NOR FLASH 挂载为 littleFS 文件系统。根据平台所用 littleFS 版本的不同，接口名称分为 `uos.VfsLfs1`（LFS1 平台）和 `uos.VfsLfs2`（LFS2 平台），参数完全相同。且只支持4线spi nor flash。

### `uos.VfsLfs1`

> 支持 `uos.VfsLfs1` 的公版平台及型号：
>
> ASR平台：
>
> - ASR 旧平台：EC100YCN_AA/EC600SCN_LA/EC600SCN_LB/EC600NCN_LA/EC600NCN_LC/EC600NCN_LF/EC600NCN_LD/EC800NCN_LA/EC600NCN_LE/EC600NCN_AA/EG915NEU_AC/EG915NEU_AG/EG915NEU_AP/EG915NLA_AP/EG915NLA_AC/EG915NEA_AC/EC200NCN_LA/EC200NCN_AA/EC200NCN_AC/EG912NEN_AA/EG200NLM_EN。
> - ASR_1606_R06 平台：EC600MCN_LA/EC600MCN_LE/EC600MEU_LA/EC600MLA_LA/EC800MCN_LA/EC800MCN_GA/EC800MCN_GB/EC800MCN_LE/EG810MCN_LA/EG810MEU_LA/EG810MCN_GA/EG810MCN_GB。
> - ASR_1803s/ASR_1803s_R02/ASR_1803sc 平台：EC200AEU_HA/EC200AAU_HA/EC200ACN_DA/EC200ACN_LA/EC200ACN_HA/EC200AEL_LA/UC200AGL_AA/EC200ACN_GA/EG950AEL_LG/EC200AEU_V1HA/EC200ACN_V1DA/EC200AAU_V1HA/EC200AEU_V1GA/EC200AAU_V1GA。
>
> Unisoc 平台：
>
> - Unisoc 平台：EC200UCN_AA/EC200UCN_LA/EC200UCN_LB/EC600UCN_LB/EC600UCN_LC/EC200UEU_AA。
> - Unisoc_8910_R05/R06 平台：EC600UCN_LB/EC200UEU_AA/EC200UCN_LA/EC200UEU_DC/EG915ULA_DC/EC600UEC_DC/EC600UEU_DC/EG915UEU_DC。
> - Unisoc_8850/8850_R02 平台：EC800GCN_GA/EC600GCN_LA/EC600GCN_LD/EC800GCN_LD/EC800GCN_LB/EC800GCN_TT/EG700GCN_LC/EG800GLA_LD/EG800GEU_LD；不建议使用 `spi_clk = 3`。
>
> Qualcomm 平台：BG95M6/BG95M8。

```python
uos.VfsLfs1(readsize,progsize,lookahead,pname,spi_port,spi_clk)
```

初始化spi nor flash，和外挂nor flash通信。适用于使用 **LFS1** 文件系统版本的平台（见上方支持说明）。

**参数描述**

* `readsize`，int类型，预留，暂未使用
* `progsize`，int类型，预留，暂未使用
* `lookahead`，int类型，预留，暂未使用
* `pname`，str类型，固定为"ext_fs"。后续扩展
* `spi_port`，int类型，支持的端口参照SPI章节说明
* `spi_clk`，int类型 <br />
|参数|时钟频率|
| ---- | ---- |
|   0   |6.25MHz|
|   1   |13MHz|
|   2  |26MHz|
|   3  |52MHz|
|   4  |3.25MHz|
|   5  |1.625MHz|
|  6  |812.5KHz|

> 52MHz时钟频率对硬件要求较高。

**返回值描述**

成功则返回VfsLfs1 object,失败则返回 OSError 19。
<details>
  <summary><span id="BG95spi nor flash"></span>BG95平台注册储存设备接口clk参数频率对应关系<br /></summary>
0 – 960KHz<br />1 – 4.8MHz<br />2 – 9.6MHz<br />3 – 16MHz<br />4 – 19.2MHz<br />5 – 25MHz<br />6 – 50MHz<br />50MHz时钟频率对硬件要求较高
</details>
**示例：**

```python
>>>ldev = uos.VfsLfs1(32, 32, 32, "ext_fs",1,0)
>>>uos.mount(ldev,'/ext')
>>>f = open('/ext/test.txt','w+')
>>>f.write('hello world!!!')
>>>f.close()
  
>>>uos.listdir('ext')
  
>>>f = open('/ext/test.txt','r')
>>>f.read()
>>>f.close()
  
```

### `uos.VfsLfs2`

> 支持 `uos.VfsLfs2` 的公版平台及型号：
>
> ASR平台：
>
> - ASR_1602 平台：EC600KCN_LC/EC800KCN_CC/EC800KCN_LC/EC800KEU_LC/EG800KCN_GC/EG800KEU_CC/EG800KEU_LC/EG800KLA_LC。
> - ASR_1603c 平台：EG915NEA_AG/EG915NEA_AK/EG915NEA_AN/EG915NLA_AK。
> - ASR_1605 平台：EG800AKCN_91LC/EG800AKCN_11LG/EG800AKCN_11LN/EG800AKGL_11LC/EG800AKLA_11LC/EG800AKJP_11LC/EG800AKLA_11LG/EG800AKEU_11LC/EG800AKEU_11LG。
> - ASR_1606_R07 平台：EC600MCN_LF/EC800MCN_GC/EC800MCN_GD/EC800MCN_LC/EC800MCN_LF/EC600MCN_LC/EC600MCN_CC/EC800MCN_CC。
> - ASR_1609 平台：EG800PCN_LA。
>
> AIC 平台：
>
> - aic8800m40 平台：FCM362K。

```python
uos.VfsLfs2(readsize,progsize,lookahead,pname,spi_port,spi_clk)
```

初始化spi nor flash，和外挂nor flash通信。适用于使用 **LFS2** 文件系统版本的平台（见上方支持说明），参数与 `uos.VfsLfs1` 完全相同。

**参数描述**

* `readsize`，int类型，预留，暂未使用
* `progsize`，int类型，预留，暂未使用
* `lookahead`，int类型，预留，暂未使用
* `pname`，str类型，固定为"ext_fs"。后续扩展
* `spi_port`，int类型，支持的端口参照SPI章节说明
* `spi_clk`，int类型 <br />
|参数|时钟频率|
| ---- | ---- |
|   0   |6.5MHz|
|   1   |13MHz|
|   2  |26MHz|
|   3  |52MHz|
|   4  |3.25MHz|
|   5  |1.625MHz|
|  6  |812.5KHz|

> 52MHz时钟频率对硬件要求较高。

**返回值描述**

成功则返回VfsLfs2 object,失败则返回 OSError 19。

**示例：**

```python
>>>ldev = uos.VfsLfs2(32, 32, 32, "ext_fs",1,0)
>>>uos.mount(ldev,'/ext')
>>>f = open('/ext/test.txt','w+')
>>>f.write('hello world!!!')
>>>f.close()
  
>>>uos.listdir('ext')
  
>>>f = open('/ext/test.txt','r')
>>>f.read()
>>>f.close()
  
```
## 注册存储设备 - EMMC

> 目前仅EC200ACNLA平台支持

### `uos.VfsEmmc`

```python
uos.VfsEmmc(str)
```

初始化EMMC存储，使用SDIO通信方式。

**参数描述**

* `str`，str类型，预留，暂未使用。

**返回值描述**

成功则返回vfs object，失败则会报错。

**示例：**

```python
edev = uos.VfsEmmc('emmc')
uos.mount(edev,'/emmc')
f = open('/emmc/test.txt','w+')
f.write('hello world!!!')
f.close()

uos.listdir('emmc')
f = open('/emmc/test.txt','r')
f.read()
f.close()
```

## 挂载文件系统

### `uos.mount`

```python
uos.mount(vfs_obj, path, mkfs=)
```

挂载实体文件系统(如littleFS/FATFS等)到虚拟文件系统(VFS)。

**参数描述**

* `vfs_obj`，vfs object，文件系统对象
* `path`，str类型，文件系统的根目录
* `mkfs=`，int类型，可选参数，是否格式化存储设备。传入1执行格式化，不传入该参数或传入0不执行格式化。

**示例：**

```python
import uos

#SPI NOR FLASH存储场景
ldev = uos.VfsLfs1(32, 32, 32, "ext_fs", 1, 0)
uos.mount(ldev, '/ext') #不传入mkfs=1只挂载不格式化
#uos.mount(ldev, '/ext', mkfs=1) #传入mkfs=1进行格式化

#SPI SD卡存储场景
cdev = uos.VfsFat(1, 0, 4, 1)
uos.mount(cdev, '/sd') #不传入mkfs=1只挂载不格式化
#uos.mount(cdev, '/sd', mkfs=1) #传入mkfs=1进行格式化

#SDIO SD卡存储场景
from uos import VfsSd
udev = VfsSd("sd_fs")
uos.mount(udev, '/sd') #不传入mkfs=1只挂载不格式化
#uos.mount(udev, '/sd', mkfs=1) #传入mkfs=1进行格式化
```

> 格式化功能适用于SD卡存储及EC600MCN_CC_EXT&ECx00U&EGx00U系列平台外置SPI NOR flash存储场景。

