# ftplib - FTP Client

FTP (File Transfer Protocol) is a standard protocol for transferring files over a computer network. It is based on a client-server model and uses control and data connections to enable file upload, download, and management operations.  

> ftplib is implemented in Python source code. If the corresponding firmware version does not support it by default, users can use the source code [ftplib.py](https://github.com/QuecPython/ftplib) and put it in the default directory /usr. When using it, change from ftplib import FTP to from usr.ftplib import FTP.

## Constructor

### `ftplib.FTP`

```python
class ftplib.FTP(host=None, port=None, user=None, passwd=None, acct=None,timeout=None)
```

Construct an FTP connection object.  

> 1. If `host` and `port` are actively passed when instantiating parameters, the `connect` method will be actively called after the object is successfully constructed to connect to the server. Connection failure will trigger an exception. If not passed, you need to call the `connect` method yourself.
> 2. On the basis of active connection, entering `user` and `port` will call the `login` method to log in to the server after the connection is successful. If not entered, you need to call the `login` method yourself.

**Parameter**  

- `host` - Optional parameter, FTP server address, string type, need to call `connect` method to connect server if not passed.
- `port` - Optional parameter, FTP server port, integer, default port is 21.  
- `user` - Optional parameter, client login username, string type, anonymous access if not specified, default is `'anonymous'`.
- `passwd`- Optional parameter, client login password, string type, default is `'anonymous@'` if `user` is `'anonymous'`.
- `acct`- Optional parameter, account, string type, empty by default, generally not set.
- `timeout`- Optional parameter, connection timeout, integer, unit second.  

**Return Value**  

- Return FTP object.

**Example**  

```python
# The feature is included if no exception is thrown
from ftplib import FTP

# Create FTP object without passing parameters  
ftp = FTP()

# Create FTP object by passing parameters, will connect and login server based on passed host and user
host = 'xxx.xxx.xxx.xxx'
port = 21 
ftp = FTP(host='xxx.xxx.xxx.xxx', port=21, user='xxx', passwd='xxx')
```

## Set Debug Level  

### `ftp.set_debuglevel`  

```python
ftp.set_debuglevel(level)
```

Set the debug level of the instance, which controls the amount of debug information. The default value 0 does not generate debug information. The value 1 generates a medium amount of debug information, usually one line per request. Values greater than or equal to 2 generate the most debug information, and every line sent and received on the FTP control connection will be logged.  

**Parameter**  

- `level` - Optional parameter, debug level, integer, default is 0.  

**Return Value**  

- None.  

**Example**

```python  
ftp.set_debuglevel(1)
```

## Connect to FTP Server  

### `ftp.connect`  

```python
ftp.connect(host, port, timeout)  
```

The FTP client initiates a connection request to the server.  

**Parameter**   

- `host` - Required parameter, FTP server address, string type, need to call `connect` method if not passed.
- `port` - Optional parameter, FTP server port, integer, default port is 21.   
- `timeout` - Optional parameter, connection timeout, integer, unit second.  

**Return Value**   

- Return string starting with `220`, indicating successful connection (e.g. `'220 (vsFTPd 3.0.3)'`), otherwise connection failed.

**Example**  

```python
host = 'xxx.xxx.xxx.xxx'  
port = 21

ftp.connect(host=host, port=port)
# 220 (vsFTPd 3.0.3)
```

## Login to FTP Server  

### `ftp.login`  

```python 
ftp.login(user, passwd)
```

The FTP client initiates a login request to the server. The login request needs to be made after the connection is established successfully.  

**Parameter**  

- `user` - Optional parameter, client login username, string type, anonymous access if not specified, default is `'anonymous'`.  
- `passwd` - Optional parameter, client login password, string type, default is `'anonymous@'` if `user` is `'anonymous'`.

**Return Value**   

- Return string starting with `230`, indicating successful login (e.g. `'230 Login successful.'`), otherwise login failed.  

**Example**  

```python
user = 'xxx'  
passwd = 'xxx'

ftp.login(user=user, passwd=passwd)  
# 230 Login successful.
```

## FTP Operation Commands  

### `ftp.nlst`  

```python  
ftp.nlst(*args)
```

Return file name list, default is current server directory.  

**Parameter**   

- `args` - Optional parameter, directory to be listed (default is current server directory), list type.  

**Return Value**   

- File name list.  

**Example**  

```python
ftp.nlst()
# ['FTP-TEST', 'system_config.json']  
```

### `ftp.dir`  

```python  
ftp.dir(*args)
```

Get content list in current directory.  

**Parameter**   

- `args` - Optional parameter, directory to return, list type.   

**Return Value**   

- Content list in the directory.   

**Example**  

```python 
ftp.dir()
# -rw-r--r--    1 1002     1002           12 Jul 26 10:07 FTP-TEST  
# -rw-------    1 1002     1002           17 Jul 28 16:40
```

### `ftp.pwd`  

```python  
ftp.pwd()
```

 Get current FTP server directory.  

**Return Value**  

- Current FTP server directory.   

**Example**   

```python  
ftp.pwd()  
# /home/ftpuser
```

### `ftp.rename`  

```python
ftp.rename(fromname, toname) 
```

Rename a file on the FTP server.  

**Parameter**   

- `fromname` - Required parameter, filename to be modified, string type.  
- `toname` - Required parameter, renamed filename, string type.   

**Return Value**   

- Return string starting with `250`, indicating successful renaming (e.g. `'250 Rename successful.'`), otherwise renaming failed.  

**Example**  

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

Delete a file on the FTP server.  

**Parameter**   

- `file_name` - Required parameter, filename to delete, string type.   

**Return Value**   

- Return string starting with `250`, indicating successful deletion (e.g. `'250 Delete operation successful.'`), otherwise deletion failed.   

**Example**  

```python  
file_name = ftp.pwd() + '/system.json' 
ftp.delete(file_name)  
# 250 Delete operation successful.
```

### `ftp.mkd`  

```python  
ftp.mkd(pathname)  
```

Create a directory on the FTP server.  

**Parameter**   

- `pathname` - Required parameter, file path of directory to be created, string type.   

**Return Value**   

- Return created path information, string format.  

**Example**  

```python
new_dir = ftp.pwd() + '/test_dir'  
ftp.mkd(new_dir)
# /home/ftpuser/test_dir  
```

### `ftp.cwd`

```python  
ftp.cwd(pathname)  
```

Set current directory on server side.  

**Parameter**  

- `pathname` - Required parameter, path to set, string type.   

**Return Value**   

- Return string starting with `250`, indicating successful setting (e.g. `'250 Directory successfully changed.'`), otherwise setting failed.  

**Example**

```python  
target_dir = ftp.pwd() + '/test_dir'  
ftp.cwd(target_dir)   
# 250 Directory successfully changed.
```

### `ftp.quit`  

```python  
ftp.quit()  
```

Close FTP server connection.   

**Return Value**   

- Return string starting with `221`, indicating successful disconnection (e.g. `'221 Goodbye.'`), otherwise disconnection failed.

**Example**   

```python  
ftp.quit()   
# 221 Goodbye.
```

## File Download  

### `ftp.retrbinary`   

```python
ftp.retrbinary(cmd, callback, blocksize)  
```

Use `RETR filename` FTP command to download files from FTP server to local in binary mode. It is recommended when handling binary files (such as images, videos, audios, etc.).  

**Parameter**   

- `cmd` - Required parameter, transfer command, consist of `RETR + filename` (separated by spaces), string type.  
- `callback` - Required parameter, callback function to receive remote file data.
  - **Function prototype:** `function(msg)`
  - **Parameter:**  
    - `msg`: Bytes type, used to receive file data transferred from FTP server to local.  
- `blocksize` - Optional parameter, set maximum bytes per transfer, default is `2048K`.   

**Return Value**   

- Return string starting with `226`, indicating successful file download (e.g. `226 Transfer complete`), otherwise download failed.  

**Example**  

```python
path = 'usr/' # Root dir of module user partition is 'usr'  
filename = 'ftp_file.bin' # Filename to download  

# Open and create a file  
save_fp = open(path + filename, 'wb+')
# Get current path on server  
server_path = ftp.pwd()
# Establish download connection, save_fp.write is write file operation, passed as callback function  
res = ftp.retrbinary('RETR ' + server_path + '/' + filename, save_fp.write)
# Download complete  
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

Use `RETR filename` FTP command to download files in text mode, usually used to download text files.  

**Parameter**  

- `cmd` - Required parameter, transfer command, consist of `RETR + filename` (separated by spaces), string type.  
- `callback` - Required parameter, callback function to receive remote file data.   
  - **Function prototype:** `function(msg)`
  - **Parameter:**   
    - `msg`: Bytes type, used to receive file data transferred from FTP server to local.  

**Return Value**   

- Return string starting with `226`, indicating successful file download (e.g. `226 Transfer complete`), otherwise download failed.  

**Example**  

```python  
path = 'usr/' # Root dir of module user partition is 'usr'  
filename = 'ftp_file.txt' # Filename to download  

# Open and create a file  
save_fp = open(path + filename, 'wb+') 
# Get current path on server
server_path = ftp_obj.pwd()  
# Establish download connection, save_fp.write is write file operation, passed as callback function
res = ftp_obj.retrlines('RETR ' + server_path + '/' + filename, save_fp.write)
# Download complete
msg = 'Down %s to device %s.'  
if res.startswith('226 Transfer complete'):
    print(msg % (filename, 'success')) 
    return True  
else:  
    print(msg % (filename, 'falied'))
    return False     
```

## File Upload  

### `ftp.storbinary`  

```python  
ftp.storbinary(cmd, fp, blocksize)
```

Upload file to FTP server in binary format.  

**Parameter**   

- `cmd` - Required parameter, transfer command, consist of `STOR + filename` (separated by spaces), string type.   
- `fp` - Required parameter, local file handle.  
- `blocksize` - Optional parameter, maximum bytes to read per file each time, default is `2048K`.  

**Return Value**   

- Return string starting with `226`, indicating successful file upload (e.g. `226 Transfer complete`), otherwise upload failed.  

**Example**  

```python  
path = 'usr/' # Root dir of module user partition is 'usr'  
filename = 'ftp_file.bin'  

# Check if file exists  
if filename in uos.listdir('usr/'):
    print('File exists') 
    with open(path + filename, 'rb') as fp:
    # cmd should be STOR command. fp is a file object (opened in binary mode). 
    res = ftp.storbinary('STOR ' + filename, fp)  
    msg = 'Upload %s to FTP Server %s.'
    if res.startswith('226 Transfer complete'):
        print(msg % (filename, 'success'))
        return True
    else:
        print(msg % (filename, 'falied')) 
        return False  
else:
    print('File path does not exist')  
```

### `ftp.storlines`   

```python  
ftp.storlines(cmd, fp)
```

Upload file to FTP server in ASCII format.  

**Parameter**   

- `cmd` - Required parameter, transfer command, consist of `STOR + filename` (separated by spaces), string type.
- `fp` - Required parameter, local file handle.  

**Return Value**   

- Return string starting with `226`, indicating successful file upload (e.g. `226 Transfer complete`), otherwise upload failed.   

**Example**  

```python  
path = 'usr/' # Root dir of module user partition is 'usr'  
filename = 'ftp_file.txt'  

# Check if file exists  
if filename in uos.listdir('usr/'):
    print('File exists')
    with open(path + filename, 'rb') as fp:
    # cmd should be STOR command.  
    res = ftp.storlines('STOR ' + filename, fp)
    msg = 'Upload %s to FTP Server %s.'
    if res.startswith('226 Transfer complete'):
        print(msg % (filename, 'success'))
        return True  
    else:
        print(msg % (filename, 'falied'))
        return False
else:
    print('File path does not exist') 
```

## Introduction to FTP Client Commands  

Following are commonly used FTP client commands:  

- CONNECT: Establish connection with FTP server.  
- USER: Specify login username.   
- PASS: Specify login password.    
- LIST: List files and subdirectories in current directory on server.    
- CWD: Change current working directory.   
- PWD: Show path of current working directory.    
- RETR: Download files from server to local computer.    
- STOR: Upload local files to server.   
- DELE: Delete files on server.   
- MKD: Create new directories on server.  
- RMD: Delete directories on server.  
- RNFR: Specify file or directory to rename.   
- RNTO: Specify renamed file or directory name.    
- PASV: Enter passive mode for data transfer.    
- TYPE: Specify data transfer type, e.g. ASCII or binary.   
- SIZE: Get file size information on server. 
- SYST: Get operating system type of server.   
- NOOP: Null operation to keep control connection active.  
- QUIT: Disconnect from server.