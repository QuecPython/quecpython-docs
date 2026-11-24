# ussl - SSL加密通信

ussl模块实现了TLS/SSL加密通信功能, 主要用于单向和双向认证使用。



## 创建SSL连接通道

### `ussl.wrap_socket`

```python
ussl.wrap_socket(sock,server_hostname=None,cert=None,key=None,ca=None,ignorelevel=0)
```

**参数描述：**

- `sock` - usocket.socket对象, 必须参数,要包装的套接字（usocket.socket）对象

- `server_hostname` - 字符串类型, 可选参数, 服务器IP地址

- `cert` - 字符串类型,  可选参数, 数字证书数据

- `key` - 字符串类型, 可选参数, 私钥数据

- `ca` - 字符串类型, 可选参数, CA证书数据, 用于验证服务器证书。传入后会启用服务器证书校验。

- `ignorelevel` - 整数类型, 可选参数, 默认值为 0, 用于设置证书校验时要忽略的错误标志（位掩码）。仅在设置 `ca` 时生效, 设置非 0 值可能放宽证书校验, 请谨慎使用。

- `sslversion` - 整数类型, 可选参数, 默认值为 4,用于指定 SSL 版本：0:SSL3.0,1:TLS1.0,2:TLS1.1,3:TLS1.2,4:ALL(所有支持的版本)

- `ciphersuites` - 列表类型, 可选参数, 支持配置要使用的算法套件。

**返回值描述：**

返回一个被包装的`usocket.socket`对象



**示例：**

```python
# 导入ussl模块
# -*- coding: UTF-8 -*-
import ussl
import usocket
import log
import utime
import checkNet

'''
下面两个全局变量是必须有的，用户可以根据自己的实际项目修改下面两个全局变量的值
'''
PROJECT_NAME = "QuecPython_Socket_example"
PROJECT_VERSION = "1.0.0"

checknet = checkNet.CheckNetwork(PROJECT_NAME, PROJECT_VERSION)

# 设置日志输出级别
log.basicConfig(level=log.INFO)
socket_log = log.getLogger("SOCKET")

if __name__ == '__main__':
    stagecode, subcode = checknet.wait_network_connected(30)
    if stagecode == 3 and subcode == 1:
        socket_log.info('Network connection successful!')
        # 1. 单向认证说明
        # 创建一个socket实例
        sock = usocket.socket(usocket.AF_INET, usocket.SOCK_STREAM)
        # 解析域名
        sockaddr=usocket.getaddrinfo('myssl.com', 443)[0][-1]
        # 建立连接
        sock.connect(sockaddr)
        # SSL连接. 前提需要服务器支持
        sock = ussl.wrap_socket(sock, server_hostname="myssl.com")
        # 向服务端发送消息
        ret = sock.write('GET / HTTP/1.0\r\nHost: myssl.com\r\nAccept-Encoding: deflate\r\n\r\n')
        socket_log.info('write %d bytes' % ret)
        #接收服务端消息
        data=sock.read(256)
        socket_log.info('read %s bytes:' % len(data))
        socket_log.info(data.decode())

        # 关闭连接
        sock.close()
        socket_log.info('--------------------Socket Ussl End-------------------')
    else:
        socket_log.info('Network connection failed! stagecode = {}, subcode = {}'.format(stagecode, subcode))

'''
# 2. 双向认证说明（如果使用双向认证，需主动配置证书和私钥，即ussl.wrap_socket（）传入cert和key参数）
cert = "数据证书"
key = "私钥"
sock = ussl.wrap_socket(sock, server_hostname="myssl.com", cert=cert, key=key)
'''

```



## ssl加密算法套件支持

通过 ussl.wrap_socket sslversion参数, 可以配置通信使用的SSL版本.

```
ssl版本说明：
ussl 支持 SSL 3.0 至 TLS 1.2 和 DTLS通信。
import ussl
ussl.wrap_socket(sock,sslversion=4)
#sslversion设置为0:SSL3.0,1:TLS1.0,2:TLS1.1,3:TLS1.2,4:ALL
```

通过 ussl.wrap_socket ciphersuites参数, 可以配置要使用的算法套件.

```
配置说明：
import ussl
list = [ussl.TLS_ECDHE_ECDSA_WITH_CHACHA20_POLY1305_SHA256,ussl.TLS_DHE_RSA_WITH_CHACHA20_POLY1305_SHA256, ussl.TLS_ECDHE_RSA_WITH_CHACHA20_POLY1305_SHA256]
ussl.wrap_socket(sock,ciphersuites=list)
```

| 算法套件                                                    |
| ----------------------------------------------------------- |
| ussl.TLS_ECDHE_ECDSA_WITH_CHACHA20_POLY1305_SHA256 (0xcca9) |
| ussl.TLS_DHE_RSA_WITH_CHACHA20_POLY1305_SHA256 (0xccaa)     |
| ussl.TLS_ECDHE_RSA_WITH_CHACHA20_POLY1305_SHA256 (0xcca8)   |
| ussl.TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384 (0xc02c)       |
| ussl.TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384 (0xc030)         |
| ussl.TLS_DHE_RSA_WITH_AES_256_GCM_SHA384 (0x009f)           |
| ussl.TLS_ECDHE_ECDSA_WITH_AES_256_CCM (0xc0ad)              |
| ussl.TLS_DHE_RSA_WITH_AES_256_CCM (0xc09f)                  |
| ussl.TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA384 (0xc024)       |
| ussl.TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384 (0xc028)         |
| ussl.TLS_DHE_RSA_WITH_AES_256_CBC_SHA256 (0x006b)           |
| ussl.TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA (0xc00a)          |
| ussl.TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA (0xc014)            |
| ussl.TLS_DHE_RSA_WITH_AES_256_CBC_SHA (0x0039)              |
| ussl.TLS_ECDHE_ECDSA_WITH_AES_256_CCM_8 (0xc0af)            |
| ussl.TLS_DHE_RSA_WITH_AES_256_CCM_8 (0xc0a3)                |
| ussl.TLS_ECDHE_ECDSA_WITH_CAMELLIA_256_GCM_SHA384 (0xc087)  |
| ussl.TLS_ECDHE_RSA_WITH_CAMELLIA_256_GCM_SHA384 (0xc08b)    |
| ussl.TLS_DHE_RSA_WITH_CAMELLIA_256_GCM_SHA384 (0xc07d)      |
| ussl.TLS_ECDHE_ECDSA_WITH_CAMELLIA_256_CBC_SHA384 (0xc073)  |
| ussl.TLS_ECDHE_RSA_WITH_CAMELLIA_256_CBC_SHA384 (0xc077)    |
| ussl.TLS_DHE_RSA_WITH_CAMELLIA_256_CBC_SHA256 (0x00c4)      |
| ussl.TLS_DHE_RSA_WITH_CAMELLIA_256_CBC_SHA (0x0088)         |
| ussl.TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256 (0xc02b)       |
| ussl.TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256 (0xc02f)         |
| ussl.TLS_DHE_RSA_WITH_AES_128_GCM_SHA256 (0x009e)           |
| ussl.TLS_ECDHE_ECDSA_WITH_AES_128_CCM (0xc0ac)              |
| ussl.TLS_DHE_RSA_WITH_AES_128_CCM (0xc09e)                  |
| ussl.TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA256 (0xc023)       |
| ussl.TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256 (0xc027)         |
| ussl.TLS_DHE_RSA_WITH_AES_128_CBC_SHA256 (0x0067)           |
| ussl.TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA (0xc009)          |
| ussl.TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA (0xc013)            |
| ussl.TLS_DHE_RSA_WITH_AES_128_CBC_SHA (0x0033)              |
| ussl.TLS_ECDHE_ECDSA_WITH_AES_128_CCM_8 (0xc0ae)            |
| ussl.TLS_DHE_RSA_WITH_AES_128_CCM_8 (0xc0a2)                |
| ussl.TLS_ECDHE_ECDSA_WITH_CAMELLIA_128_GCM_SHA256 (0xc086)  |
| ussl.TLS_ECDHE_RSA_WITH_CAMELLIA_128_GCM_SHA256 (0xc08a)    |
| ussl.TLS_DHE_RSA_WITH_CAMELLIA_128_GCM_SHA256 (0xc07c)      |
| ussl.TLS_ECDHE_ECDSA_WITH_CAMELLIA_128_CBC_SHA256 (0xc072)  |
| ussl.TLS_ECDHE_RSA_WITH_CAMELLIA_128_CBC_SHA256 (0xc076)    |
| ussl.TLS_DHE_RSA_WITH_CAMELLIA_128_CBC_SHA256 (0x00be)      |
| ussl.TLS_DHE_RSA_WITH_CAMELLIA_128_CBC_SHA (0x0045)         |
| ussl.TLS_RSA_WITH_AES_256_GCM_SHA384 (0x009d)               |
| ussl.TLS_RSA_WITH_AES_256_CCM (0xc09d)                      |
| ussl.TLS_RSA_WITH_AES_256_CBC_SHA256 (0x003d)               |
| ussl.TLS_RSA_WITH_AES_256_CBC_SHA (0x0035)                  |
| ussl.TLS_ECDH_RSA_WITH_AES_256_GCM_SHA384 (0xc032)          |
| ussl.TLS_ECDH_RSA_WITH_AES_256_CBC_SHA384 (0xc02a)          |
| ussl.TLS_ECDH_RSA_WITH_AES_256_CBC_SHA (0xc00f)             |
| ussl.TLS_ECDH_ECDSA_WITH_AES_256_GCM_SHA384 (0xc02e)        |
| ussl.TLS_ECDH_ECDSA_WITH_AES_256_CBC_SHA384 (0xc026)        |
| ussl.TLS_ECDH_ECDSA_WITH_AES_256_CBC_SHA (0xc005)           |
| ussl.TLS_RSA_WITH_AES_256_CCM_8 (0xc0a1)                    |
| ussl.TLS_RSA_WITH_CAMELLIA_256_GCM_SHA384 (0xc07b)          |
| ussl.TLS_RSA_WITH_CAMELLIA_256_CBC_SHA256 (0x00c0)          |
| ussl.TLS_RSA_WITH_CAMELLIA_256_CBC_SHA (0x0084)             |
| ussl.TLS_ECDH_RSA_WITH_CAMELLIA_256_GCM_SHA384 (0xc08d)     |
| ussl.TLS_ECDH_RSA_WITH_CAMELLIA_256_CBC_SHA384 (0xc079)     |
| ussl.TLS_ECDH_ECDSA_WITH_CAMELLIA_256_GCM_SHA384 (0xc089)   |
| ussl.TLS_ECDH_ECDSA_WITH_CAMELLIA_256_CBC_SHA384 (0xc075)   |
| ussl.TLS_RSA_WITH_AES_128_GCM_SHA256 (0x009c)               |
| ussl.TLS_RSA_WITH_AES_128_CCM (0xc09c)                      |
| ussl.TLS_RSA_WITH_AES_128_CBC_SHA256 (0x003c)               |
| ussl.TLS_RSA_WITH_AES_128_CBC_SHA (0x002f)                  |
| ussl.TLS_ECDH_RSA_WITH_AES_128_GCM_SHA256 (0xc031)          |
| ussl.TLS_ECDH_RSA_WITH_AES_128_CBC_SHA256 (0xc029)          |
| ussl.TLS_ECDH_RSA_WITH_AES_128_CBC_SHA (0xc00e)             |
| ussl.TLS_ECDH_ECDSA_WITH_AES_128_GCM_SHA256 (0xc02d)        |
| ussl.TLS_ECDH_ECDSA_WITH_AES_128_CBC_SHA256 (0xc025)        |
| ussl.TLS_ECDH_ECDSA_WITH_AES_128_CBC_SHA (0xc004)           |
| ussl.TLS_RSA_WITH_AES_128_CCM_8 (0xc0a0)                    |
| ussl.TLS_RSA_WITH_CAMELLIA_128_GCM_SHA256 (0xc07a)          |
| ussl.TLS_RSA_WITH_CAMELLIA_128_CBC_SHA256 (0x00ba)          |
| ussl.TLS_RSA_WITH_CAMELLIA_128_CBC_SHA (0x0041)             |
| ussl.TLS_ECDH_RSA_WITH_CAMELLIA_128_GCM_SHA256 (0xc08c)     |
| ussl.TLS_ECDH_RSA_WITH_CAMELLIA_128_CBC_SHA256 (0xc078)     |
| ussl.TLS_ECDH_ECDSA_WITH_CAMELLIA_128_GCM_SHA256 (0xc088)   |
| ussl.TLS_ECDH_ECDSA_WITH_CAMELLIA_128_CBC_SHA256 (0xc074)   |
| ussl.TLS_EMPTY_RENEGOTIATION_INFO_SCSV (0x00ff)             |