# FAQ on HTTP

### **Whether HTTPS is supported and which authentication methods are supported?**


- HTTPS requires the module to support SSL. The module developed by QuecPython is supported by version 8+8 or above. Modules with small memory solutions below version 4+4 are not supported.
- Supports one-way authentication and two-way authentication

### **How to POST file?**
- Read the file in rb format and post the read data

```python
import request
import uio
import ujson

url = "http://httpbin.org/post"
with uio.open('usr/dtu_config.json',mode = 'rb') as f:
    data = ujson.dumps(f.read())
    response = request.post(url, data=data)
    
print(response.status_code)  # Print response status code
print(response.text)  # Print response content
```



### **What protocol versions does HTTP support?**
- HTTP1.0 & HTTP1.1

### **What information does the HTTP request header contain?**


Some common default fields in HTTP request headers include:

- Host: Specifies the target host name of the request.

- User-Agent: indicates the information of the client application or browser that sends the request.

- Accept: Specifies the response content type can be accepted by client.

- Accept-Language: Specifies the natural language can be accepted by client.

- Accept-Encoding: Specifies the acceptable content encoding method in client terminal, such as gzip, deflate, etc.

- Connection: Specifies the connection type between the client and the server, such as keep-alive, close, etc.

- Cache-Control: Specifies the process to cache in client terminal such as no-cache, max-age and etc.

- Referer: Indicates the URL of the source page of the request.

- If-None-Match: Used for conditional GET requests, which specifies the ETag value of the resource. The server will check and determine whether 304 Not Modified will be returned.

- If-Modified-Since: Used for conditional GET requests, which specifies that if the resource has not been modified since the specified date, 304 Not Modified will be returned.