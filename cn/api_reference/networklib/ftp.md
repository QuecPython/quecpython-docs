# ftplib - FTP 客户端

FTP（File Transfer Protocol）是一种用于在计算机网络中传输文件的标准协议，它基于客户端-服务器模型，通过控制连接和数据连接实现文件的上传、下载和管理操作。

> ftplib是python源码实现，如对应固件版本未默认支持。用户可使用源码[ftplib.py](https://github.com/QuecPython/ftplib)，放默认目录/usr下，使用时，将from ftplib import FTP 改为from usr.ftplib import FTP.

## 构造函数

### `ftplib.FTP`

```python
class ftplib.FTP(host=None, port=None, user=None, passwd=None, acct=None,timeout=None)
```

构建 FTP 连接对象。

> 1. 实例化参数时如主动传入 `host` 与 `port`，将在对象构建成功后主动调用 `connect` 方法连接服务端，连接失败会触发异常，未传入时需自行调用 `connect` 方法。
> 2. 在主动连接基础上传入 `user` 与 `port` 会在连接成功后调用 `login` 方法登录服务端，未传入需自行调用 `login` 方法。

**参数描述：**

- `host` - 可选参数，FTP 服务端地址，字符串类型，未传入时需自行调用 `connect` 方法连接服务端。
- `port` - 可选参数，FTP 服务端端口，整型，默认端口为 21。
- `user` - 可选参数，客户端登录用户名，字符串类型，若未指定则匿名访问，默认为 `'anonymous'`。
- `passwd`- 可选参数，客户端登录密码，字符串类型，如果 `user` 为 `'anonymous'`，那么默认的 `passwd` 是 `'anonymous@'`。
- `acct`- 可选参数，账户，字符串类型，默认为空，一般不做设置。
- `timeout`- 可选参数，连接超时，整型，单位为秒。

**返回值描述：**

- 返回 FTP 对象。

**示例：**

```python
# 未抛出异常则包含该功能
from ftplib import FTP

# 不传入参数创建FTP对象
ftp = FTP()

# 传入参数创建 FTP 对象，会根据参数 host 和 user 来进行服务段连接和登录，登录需在连接成功之后
host = 'xxx.xxx.xxx.xxx'
port = 21
ftp = FTP(host='xxx.xxx.xxx.xxx', port=21, user='xxx', passwd='xxx')
```

## 设置调试输出级别

### `ftp.set_debuglevel`

```python
ftp.set_debuglevel(level)
```

设置实例的调试级别，它控制着调试信息的数量。默认值 0 不产生调试信息。值 1 产生中等数量的调试信息，通常每个请求产生一行。大于或等于 2 的值产生的调试信息最多，FTP 控制连接上发送和接收的每一行都将被记录下来。

**参数描述：**

- `level` - 可选参数，调试级别，整型，默认为 0。

**返回值描述：**

- 无。

**示例：**

```python
ftp.set_debuglevel(1)
```

## 连接FTP服务器

### `ftp.connect`

```python
ftp.connect(host, port, timeout)
```

FTP 客户端发起连接服务端请求。

**参数描述：**

- `host` - 必填参数，FTP 服务端地址，字符串类型，未传入时需自行调用 `connect` 方法连接服务端。
- `port` - 可选参数，FTP 服务端端口，整型，默认端口为 21。
- `timeout` - 可选参数，连接超时，整型，单位为秒。

**返回值描述：**

- 返回以 `220` 开头的字符串，则表示连接成功（如  `'220 (vsFTPd 3.0.3)'`），否则表示连接失败。

**示例：**

```python
host = 'xxx.xxx.xxx.xxx'
port = 21

ftp.connect(host=host, port=port)
# 220 (vsFTPd 3.0.3)
```

## 登录 FTP 服务器

### `ftp.login`

```python
ftp.login(user, passwd)
```

FTP客户端发起登录服务端请求，登录请求需在及连接建立成功后进行。

**参数描述：**

- `user` - 可选参数，客户端登录用户名，字符串类型，若未指定则匿名访问，默认为 `'anonymous'`。
- `passwd `- 可选参数，客户端登录密码，字符串类型，如果 `user` 为 `'anonymous'`，那么默认的 `passwd` 是 `'anonymous@'`。

**返回值描述：**

- 返回以 `230` 开头的字符串表示登录成功（如  `'230 Login successful.'`），否则表示登录失败。

**示例：**

```python
user = 'xxx'
passwd = 'xxx'

ftp.login(user=user, passwd=passwd)
# 230 Login successful.
```

## FTP 操作指令

### `ftp.nlst`

```python
ftp.nlst(*args)
```

返回文件名列表，默认返回当前服务器目录。

**参数描述：**

- `args` - 可选参数，待列出的目录（默认为当前服务器目录），列表类型。

**返回值描述：**

- 文件名列表。

**示例：**

```python
ftp.nlst()
# ['FTP-TEST', 'system_config.json']
```

### `ftp.dir`

```python
ftp.dir(*args)
```

获取当前目录中的内容列表。

**参数描述：**

- `args` - 可选参数，需要返回的目录，列表类型。

**返回值描述：**

- 目录下的内容列表。

**示例：**

```python
ftp.dir()
# -rw-r--r--    1 1002     1002           12 Jul 26 10:07 FTP-TEST
# -rw-------    1 1002     1002           17 Jul 28 16:40 
```

### `ftp.pwd`

```python
ftp.pwd()
```

 获取当前 FTP 服务器目录。

**返回值描述：**

- 当前 FTP 服务器目录。

**示例：**

```python
ftp.pwd()
# /home/ftpuser
```

### `ftp.rename`

```python
ftp.rename(fromname, toname)
```

重命名 FTP 服务器上的文件。

**参数描述：**

- `fromname` - 必填参数，待修改的文件名，字符串类型。
- `toname` - 必填参数，重命名后的文件名，字符串类型。

**返回值描述：**

- 返回以 `250` 打头的字符串，表示重命名成功（如 `'250 Rename successful.'`），否则表示重命名失败。

**示例：**

```python
from_name = ftp.pwd() + '/system_config.json'
to_name = ftp.pwd() + '/system.json'
ftp.rename(from_name, to_name)
# 250 Rename successful.
```

### `ftp.delete`

```python
ftp.delete(file_name)
```

删除 FTP 服务器上的文件。

**参数描述：**

- `file_name` - 必填参数，待删除的文件名，字符串类型。

**返回值描述：**

- 返回以 `250` 打头的字符串，表示删除成功（如 `'250 Delete operation successful.'`），否则表示删除失败。

**示例：**

```python
file_name = ftp.pwd() + '/system.json'
ftp.delete(file_name)
# 250 Delete operation successful.
```

### `ftp.mkd`

```python
ftp.mkd(pathname)
```

在 FTP 服务器上创建目录。

**参数描述：**

- `pathname` - 必填参数，待创建的的目录文件路径，字符串类型。

**返回值描述：**

- 返回创建成功的路径信息，字符串格式。

**示例：**

```python
new_dir = ftp.pwd() + '/test_dir'
ftp.mkd(new_dir)
# /home/ftpuser/test_dir
```

### `ftp.cwd`

```python
ftp.cwd(pathname)
```

设置服务器端的当前目录。

**参数描述：**

- `pathname` - 必填参数，设置的目录路径，字符串类型。

**返回值描述：**

- 返回以 `250` 打头的字符串，表示设置成功（如 `'250 Directory successfully changed.'`），否则表示设置失败。

**示例：**

```python
target_dir = ftp.pwd() + '/test_dir'
ftp.cwd(target_dir)
# 250 Directory successfully changed.
```

### `ftp.quit`

```python
ftp.quit()
```

关闭 FTP 服务器连接。

**返回值描述：**

- 返回以 `221` 打头的字符串，表示关闭连接成功（如 `'221 Goodbye.'`），否则表示关闭连接失败。

**示例：**

```python
ftp.quit()
# 221 Goodbye.
```

## 文件下载

### `ftp.retrbinary`

```python
ftp.retrbinary(cmd, callback, blocksize)
```

使用`RETR filename` FTP 命令，以二进制模式将文件从 FTP 服务器下载到本地，处理二进制文件（如图片、视频、音频等）时建议使用。

**参数描述：**

- `cmd` - 必填参数，传输命令，由 `RETR +文件名`组成（中间有空格），字符串类型。
- `callback` - 必填参数，接收远端文件数据的回调函数。
  - **函数原型：** `function(msg)` 
  - **参数描述：**
    - `msg` ：字节类型，用于接收 FTP 服务端传输到本地的文件数据。
- `blocksize` - 可选参数，设置每次传输的最大字节数，默认为 `2048K` 。

**返回值描述：**

- 返回以 `226` 打头的字符串，表示下载文件成功（如 `226 Transfer complete`），否则表示下载文件失败。

**示例：**

```python
path = 'usr/' # 模组用户分区的根目录为'usr'
filename = 'ftp_file.bin' # 待下载的文件名

# 打开并创建一个文件
save_fp = open(path + filename, 'wb+')
# 查询服务端当前路径
server_path = ftp.pwd() 
# 建立下载连接，save_fp.write为写文件操作,作为回调函数传入
res = ftp.retrbinary('RETR ' + server_path + '/' + filename, save_fp.write)
# 完成下载
msg = 'Down %s to device %s.'
if res.startswith('226 Transfer complete'):
    print(msg % (filename, 'success'))
    return True
else:
    print(msg % (filename, 'falied'))
    return False   
```

### `ftp.retrlines`

```python
ftp.retrlines(cmd, callback)
```

使用 `RETR filename` FTP命令，以文本模式下载文件，通常用于下载文本文件。

**参数描述：**

- `cmd` - 必填参数，传输命令,由 `RETR +文件名` 组成（中间有空格），字符串类型。
- `callback` - 必填参数，接收远端文件数据的回调函数。
  - **函数原型：** `function(msg)` 
  - **参数描述：**
    - `msg` ：字节类型，用于接收 FTP 服务端传输到本地的文件数据。

**返回值描述：**

- 返回以 `226` 打头的字符串，表示下载文件成功（如 `226 Transfer complete`），否则表示下载文件失败。

**示例：**

```python
path = 'usr/' # 模组用户分区的根目录为'usr'
filename = 'ftp_file.txt' # 待下载的文件名

# 打开并创建一个文件
save_fp = open(path + filename, 'wb+')
# 查询服务端当前路径
server_path = ftp_obj.pwd() 
# 建立下载连接，save_fp.write为写文件操作,作为回调函数传入
res = ftp_obj.retrlines('RETR ' + server_path + '/' + filename, save_fp.write)
# 完成下载
msg = 'Down %s to device %s.'
if res.startswith('226 Transfer complete'):
    print(msg % (filename, 'success'))
    return True
else:
    print(msg % (filename, 'falied'))
    return False    
```

## 文件上传

### `ftp.storbinary`

```python
ftp.storbinary(cmd, fp, blocksize)
```

以二进制形式向 FTP 服务器上传文件。

**参数描述：**

- `cmd` - 必填参数，传输命令，由 `STOR +文件名` 组成（中间有空格），字符串类型。
- `fp` - 必填参数，本地文件句柄。
- `blocksize` - 可选参数，每次读取文件的最大字节数，默认为 `2048K` 。

**返回值描述：**

- 返回以 `226` 打头的字符串，表示上传文件成功（如 `226 Transfer complete`），否则表示上传文件失败。

**示例：**

```python
path = 'usr/' # 模组用户分区的根目录为'usr'
filename = 'ftp_file.bin'

# 判断文件是否存在
if filename in uos.listdir('usr/'):
    print('文件存在')
    with open(path + filename, 'rb') as fp:
    # cmd应为STOR 命令。fp是一个文件对象 (以二进制模式打开)。
    res = ftp.storbinary('STOR ' + filename, fp)
    msg = 'Upload %s to FTP Server %s.'
    if res.startswith('226 Transfer complete'):
        print(msg % (filename, 'success'))
        return True
    else:
        print(msg % (filename, 'falied'))
        return False
else:
    print('文件路径不存在')
```

### `ftp.storlines`

```python
ftp.storlines(cmd, fp)
```

以 ASCII 码形式向 FTP 服务器上传文件。

**参数描述：**

- `cmd` - 必填参数，传输命令，由 `STOR +文件名` 组成（中间有空格），字符串类型。
- `fp` - 必填参数，本地文件句柄。

**返回值描述：**

- 返回以 `226` 打头的字符串，表示上传文件成功（如 `226 Transfer complete`），否则表示上传文件失败。

**示例：**

```python
path = 'usr/' # 模组用户分区的根目录为'usr'
filename = 'ftp_file.txt'

# 判断文件是否存在
if filename in uos.listdir('usr/'):
    print('文件存在')
    with open(path + filename, 'rb') as fp:
    # cmd应为STOR 命令。
    res = ftp.storlines('STOR ' + filename, fp)
    msg = 'Upload %s to FTP Server %s.'
    if res.startswith('226 Transfer complete'):
        print(msg % (filename, 'success'))
        return True
    else:
        print(msg % (filename, 'falied'))
        return False
else:
    print('文件路径不存在')
```

## FTP客户端命令介绍

以下是常用的 FTP 客户端命令：

- CONNECT：建立与 FTP 服务器的连接。
- USER：指定登录用户名。
- PASS：指定登录密码。
- LIST：列出服务器上当前目录的文件和子目录。
- CWD：改变当前工作目录。
- PWD：显示当前工作目录的路径。
- RETR：从服务器下载文件到本地计算机。
- STOR：将本地文件上传到服务器。
- DELE：删除服务器上的文件。
- MKD：在服务器上创建新的目录。
- RMD：删除服务器上的目录。
- RNFR：指定要重命名的文件或目录。
- RNTO：指定重命名后的文件或目录名。
- PASV：进入被动模式，用于数据传输。
- TYPE：指定数据传输的类型，如 ASCII 或二进制。
- SIZE：获取服务器上文件的大小信息。
- SYST：获取服务器的操作系统类型。
- NOOP：空操作，用于保持控制连接的活动状态。
- QUIT：断开与服务器的连接。