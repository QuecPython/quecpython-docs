# HTTP相关问题

### **是否支持HTTPS，支持哪些认证方式**

- HTTPS需要模组支持SSL，QuecPython开发的模组8+8版本以上的支持，4+4版本以下的小内存方案模组不支持
- 支持单向认证和双向认证

### **如何POST文件**

- 以rb的形式读文件，将读取到的数据进行post

```python
import request
import uio
import ujson

url = "http://httpbin.org/post"
with uio.open('usr/dtu_config.json',mode = 'rb') as f:
    data = ujson.dumps(f.read())
    response = request.post(url, data=data)
    
print(response.status_code)  # 打印响应状态码
print(response.text)  # 打印响应内容
```



### **HTTP支持哪些协议版本**

- HTTP1.0和HTTP1.1

### **HTTP请求头包含那些信息**

HTTP请求头中的一些常见默认字段包括：

- Host：指定请求的目标主机名。

- User-Agent：标识发送请求的客户端应用程序或浏览器的信息。

- Accept：指定客户端能够接受的响应内容类型。

- Accept-Language：指定客户端能够接受的自然语言。

- Accept-Encoding：指定客户端能够接受的内容编码方式，如gzip、deflate等。

- Connection：指定客户端与服务器之间的连接类型，如keep-alive、close等。

- Cache-Control：指定客户端对缓存的处理方式，如no-cache、max-age等。

- Referer：指示请求的来源页面的URL。

- If-None-Match：用于条件GET请求，指定资源的ETag值，服务器将检查该值以确定是否返回304 Not Modified。

- If-Modified-Since：用于条件GET请求，指定如果自指定日期以来资源未被修改，则返回304 Not Modified。