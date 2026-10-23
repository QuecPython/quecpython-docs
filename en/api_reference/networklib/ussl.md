# ussl – SSL/TLS Protocol

ussl realizes the encrypted communication using TLS/SSL protocol, mainly for unidirectional and bidirectional authentication.



## Create Secure Channel over SSL

### `ussl.wrap_socket`

```python
ussl.wrap_socket(sock,server_hostname=None,cert=None,key=None)
```

**Parameter**

- `sock` – The usocket.socket object to be wrapped. Required parameter.

- `server_hostname` – String type. Server IP address. Optional parameter. 

- `cert` – String type. Digital certificate. Optional parameter. 

- `key` – String type. Private key. Optional parameter. 

- `sslversion` - Integer type, optional parameter, default value is 4. Used to specify the SSL version: 0:SSL3.0, 1:TLS1.0, 2:TLS1.1, 3:TLS1.2, 4:ALL (all supported versions).

- `ciphersuites` - List type, optional parameter. Supports configuration of the algorithm suites to be used.

**Return Value**

A wrapped `usocket.socket` object.



**Example**

```python
# Import ussl
# -*- coding: UTF-8 -*-
import ussl
import usocket
import log
import utime
import checkNet

'''
The following two global variables are required. You can modify the values of the following two global variables according to your actual projects.
'''
PROJECT_NAME = "QuecPython_Socket_example"
PROJECT_VERSION = "1.0.0"

checknet = checkNet.CheckNetwork(PROJECT_NAME, PROJECT_VERSION)

# Set the log output level.
log.basicConfig(level=log.INFO)
socket_log = log.getLogger("SOCKET")

if __name__ == '__main__':
    stagecode, subcode = checknet.wait_network_connected(30)
    if stagecode == 3 and subcode == 1:
        socket_log.info('Network connection successful!')
        # 1. Unidirectional-authentication description
        # Create a socket instance.
        sock = usocket.socket(usocket.AF_INET, usocket.SOCK_STREAM)
        # Resolve a domain name.
        sockaddr=usocket.getaddrinfo('myssl.com', 443)[0][-1]
        # Set up a connection.
        sock.connect(sockaddr)
        # Create Secure Channel over SSL, supported by the server.
        sock = ussl.wrap_socket(sock, server_hostname="myssl.com")
        # Send messages to the server.
        ret = sock.write('GET / HTTP/1.0\r\nHost: myssl.com\r\nAccept-Encoding: deflate\r\n\r\n')
        socket_log.info('write %d bytes' % ret)
        # Receive the messages from the server.
        data=sock.read(256)
        socket_log.info('read %s bytes:' % len(data))
        socket_log.info(data.decode())

        # Close the connection.
        sock.close()
        socket_log.info('--------------------Socket Ussl End-------------------')
    else:
        socket_log.info('Network connection failed! stagecode = {}, subcode = {}'.format(stagecode, subcode))

'''
# 2. Bidirectional-authentication description(If two-way authentication is used, the certificate and private key need to be configured manually. That is, when using ussl.wrap_socket(), the cert and key parameters should be passed in.)
cert = "Certificate"
key = "Private Key"
sock = ussl.wrap_socket(sock, server_hostname="myssl.com", cert=cert, key=key)
'''
```



## List of Supported Cipher Suites

Configure SSL version for communication via ussl.wrap_socket's sslversion parameter.

```
SSL version description:
ussl supports SSL 3.0 up to TLS 1.2 and DTLS communication.
import ussl
ussl.wrap_socket(sock,sslversion=4)
#sslversion设置为0:SSL3.0,1:TLS1.0,2:TLS1.1,3:TLS1.2,4:ALL
```

The cipher suites to be used can be configured via the ciphersuites parameter of ussl.wrap_socket.

```
Config Description:
import ussl
list = [ussl.TLS_ECDHE_ECDSA_WITH_CHACHA20_POLY1305_SHA256,ussl.TLS_DHE_RSA_WITH_CHACHA20_POLY1305_SHA256, ussl.TLS_ECDHE_RSA_WITH_CHACHA20_POLY1305_SHA256]
ussl.wrap_socket(sock,ciphersuites=list)
```

| Cipher Suite                                                |
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