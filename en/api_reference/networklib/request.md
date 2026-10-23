# request - HTTP Client

The `request` feature is used for sending an HTTP request to a server, fetching data from a server, or submitting data to a server. Multiple request methods, including GET, POST, and PUT are supported. 

## HTTP Request

### `request.get`

```python
response = request.get(url)
```

Sends a GET request.

**Parameter**

- `url` - Required parameter. The server IP address in the request(Automatically identify http/https requests, and enable ssl on https).
- `data` - Optional parameter. Parameters to be carried in JSON format in the request.
- `headers` - Dictionary type. Optional parameter. The header information in the request.
- `decode` - Boolean type. Optional parameter. Decode the response result with UTF-8 after the request is successful. True: Decode. False: Not decode. Default value: True. Bytes will be returned if False is entered. This parameter is only used for *response.content*. 
- `timeout` - Optional parameter. Timeout, Default value: 20. Unit: s.
- `ipvtype` - Optional parameter. Timeout, Default value: 20. Unit: s.
- `username` - Optional parameter. User name for user authentication, string type.
- `password` - Optional parameter. Password for user authentication, string type.
- `ssl_params` - Optional parameter. When the URL is an https request, if the key needs to be passed in during SSL authentication, Format: {"cert": certificate_content, "key": private_content}, content is a string type.

**Return Value**

- A response object containing all information returned by the server, such as response status codes, response headers, and response bodies.

**Example**

```python
import request
import utime

url = "http://www.example.com"
response = request.get(url)

for i in response.text:
    print(i);utime.sleep_ms(10)

url = "https://www.example.com"
response = request.get(url)

for i in response.text:
    print(i);utime.sleep_ms(10)

```

### `request.post`

```python
response = request.post(url,data)
```

Sends a POST request.

**Parameter**

- `url` - Required parameter. The server IP address in the request(Automatically identify http/https requests, and enable ssl on https).
- `data` - Optional parameter. Parameters to be carried in JSON format in the request.
- `headers` - Dictionary type. Optional parameter. The header information in the request.
- `decode` - Boolean type. Optional parameter. Decode the response result with UTF-8 after the request is successful. True: Decode. False: Not decode. Default value: True. Bytes will be returned if False is entered. This parameter is only used for *response.content*.
- `timeout` - Optional parameter. Timeout, Default value: 20. Unit: s.
- `ipvtype` - Optional parameter. Timeout, Default value: 20. Unit: s.
- `username` - Optional parameter. User name for user authentication, string type.
- `password` - Optional parameter. Password for user authentication, string type.
- `ssl_params` - Optional parameter. When the URL is an https request, if the key needs to be passed in during SSL authentication, Format: {"cert": certificate_content, "key": private_content}, content is a string type.

**Return Value**

- A response object containing all information returned by the server, such as response status codes, response headers, and response bodies.

**Content-Type Introduction:**

There are four types of data submitted in the POST method:

- application/x-www-form-urlencoded: The form data is encoded in key/value pairs and is sent to the server. It is the default type for the data submission in the form.
- multipart/form-data: This type is required for uploading files in the form.
- application/json: JSON format.
- application/octet-stream: Binary data stream. This type is used for downloading files.

**Example**

```python
import request

url = "http://httpbin.org/post"
data = {"key1": "value1", "key2": "value2", "key3": "value3"}
header = {'key1': 'value1', "key2": "value2", "key3": "value3"}
response = request.post(url, data=ujson.dumps(data), header)

for i in response.text:
    print(i)

```

### `request.put`

```python
response = request.put(url)
```

Sends a PUT request.

**Parameter**

- `url` - Required parameter. The server IP address in the request(Automatically identify http/https requests, and enable ssl on https).
- `data` - Optional parameter. Parameters to be carried in JSON format in the request.
- `headers` - Dictionary type. Optional parameter. The header information in the request.
- `decode` - Boolean type. Optional parameter. Decode the response result with UTF-8 after the request is successful. True: Decode. False: Not decode. Default value: True. Bytes will be returned if False is entered. This parameter is only used for *response.content*.
- `timeout` - Optional parameter. Timeout, Default value: 20. Unit: s.
- `ipvtype` - Optional parameter. Timeout, Default value: 20. Unit: s.
- `username` - Optional parameter. User name for user authentication, string type.
- `password` - Optional parameter. Password for user authentication, string type.
- `ssl_params` - Optional parameter. When the URL is an https request, if the key needs to be passed in during SSL authentication, Format: {"cert": certificate_content, "key": private_content}, content is a string type.

**Return Value**

- A response object containing all information returned by the server, such as response status codes, response headers, and response bodies.

### `request.head`

```python
response = request.head(url)
```

Sends a HEAD request.

**Parameter**

- `url` - Required parameter. The server IP address in the request(Automatically identify http/https requests, and enable ssl on https).
- `data` - Optional parameter. Parameters to be carried in JSON format in the request.
- `headers` - Dictionary type. Optional parameter. The header information in the request.
- `decode `- Boolean type. Optional parameter. Decode the response result with UTF-8 after the request is successful. True: Decode. False: Not decode. Default value: True. Bytes will be returned if False is entered. This parameter is only used for *response.content*.
- `timeout` - Optional parameter. Timeout, Default value: 20. Unit: s.
- `ipvtype` - Optional parameter. Timeout, Default value: 20. Unit: s.
- `username` - Optional parameter. User name for user authentication, string type.
- `password` - Optional parameter. Password for user authentication, string type.
- `ssl_params` - Optional parameter. When the URL is an https request, if the key needs to be passed in during SSL authentication, Format: {"cert": certificate_content, "key": private_content}, content is a string type.

**Return Value**

- A response object containing all information returned by the server, such as response status codes, response headers, and response bodies.

## Get Response

After the `request` library sends a request, a response object will be returned which contains all information sent by the server, such as response status codes, response headers, and response bodies.

### `response.status_code`

Gets the request status codes.

```python
response.status_code
```

**Return Value**

- Integer type. The request status codes.

**Example**

```python
import request

response = request.get("http://httpbin.org/get")
print(response.status_code)
```

### `response.headers`

Gets the request header.

```python
response.headers
```

**Return Value**

- Dict type. The request header.

**Example**

```python
import request

response = request.get("http://httpbin.org/get")
print(response.headers)
```

### `response.text`

Gets the text data of the response body,You can directly release resources using `response.close` without reading the data.

```python
response.text
```

**Return Value**

- A generator object reading all returned text data through a for loop.

**Example**

```python
import request

response = request.get("http://httpbin.org/get")
if True:
        for i in response.text:
            print(i)
else:
	    response.close()
```

### `response.content`

Gets the response body. 

```python
response.content
```

**Return Value**

- A generator object reading all returned response body data through a for loop.

**Example**

```python
import request

response = request.get("http://httpbin.org/get")
for i in response.content:
    print(i)
```

### `response.json`

Gets the response body in JSON format.

```python
response.json()
```

**Return Value**

- The response data in dictionary type.

**Example**

```python
import request

response = request.get("http://httpbin.org/get")
data = response.json()
print(data)
```

### `respnse.close`

Proactively close the HTTP connection without reading data from the server to release resources.

```python
response.close()
```
**Return Value**

- No return value

**Example**
```python
import request

response = request.get("http://httpbin.org/get")
response.close()
```
