# Aws S3 – AWS S3 客户端

`S3Client` 类为与 AWS S3 存储交互提供了一个方便的接口。它支持文件上传、下载和删除等操作，同时通过 HTTP SigV4 身份验证确保安全通信。

## 初始化客户端

### `S3Client`
```python
class S3Client(region=None, access_key=None, secret_key=None, bucket=None, config_path="config.json", download_path="")
```

**参数：**

- **region** *(str, 可选)*: 指定S3存储桶所在的AWS区域。
- **access_key** *(str, 可选)*: 用于身份验证的 Access Key。
- **secret_key** *(str, 可选)*: 用于身份验证的 Secret Key。
- **bucket** *(str, 可选)*: S3 存储桶的名称。
- **config_path** *(str, 可选)*: JSON 配置文件的路径。默认为 `config.json`。
- **download_path** *(str, 可选)*: 用于存储下载文件的本地目录。默认为空字符串。

**示例：**

- 使用显示参数初始化

```python
from s3client import S3Client

client = S3Client(
    region="eu-west-3",
    access_key="AKIA...",
    secret_key="A2HAXbc...",
    bucket="my-bucket",
    download_path="s3_downloads/"
)
```

- 使用配置文件初始化

```python
from s3client import S3Client

client = S3Client(config_path="usr/config.json")
```

## S3 通信

`S3Client` 使用 HTTP SigV4 签名的请求来执行上传、下载和从 AWS S3 存储中删除文件等操作。

### `upload_file`

```python
upload_file(filepath, object_key, content_type='application/octet-stream')
```

将本地文件上载到指定的AWS S3存储桶。

**参数：**

- **filepath** *(str)*: 要上载的文件的本地路径。
- **object_key** *(str)*: 在S3存储桶中存储文件的密钥（路径）。
- **content_type** *(str, 可选)*: 文件的 MIME 类型（默认: `application/octet-stream`）。

**返回值：**

- **响应对象**包含服务器返回的所有信息，如响应状态代码、响应头和响应体。

- 返回 **None** 如果发生任何异常。

### `download_to_file`

```python
download_to_file(object_key)
```

从 S3 存储桶下载一个文件，并将其保存在配置或构造函数中定义的`download_path`中。

**参数：**

- **object_key** *(str)*: 要从S3存储桶下载的文件的密钥（路径）。

**返回值：**

- **响应对象**包含服务器返回的所有信息，如响应状态代码、响应头和响应体。

- 返回 **None** 如果发生任何异常。

### `delete_object`

```python
delete_object(object_key)
```

从 S3 存储桶中删除一个文件。

**参数：**

- **object_key** *(str)*: 要从S3存储桶中删除的文件的密钥（路径）。

**返回值：**

- **响应对象**包含服务器返回的所有信息，如响应状态代码、响应头和响应体。

- 返回 **None** 如果发生任何异常。

## 示例代码

```python
from s3client import S3Client

s3_client = S3Client(
    region="eu-west-3",
    access_key="YOUR_ACCESS_KEY",
    secret_key="YOUR_SECRET_KEY",
    bucket="your-bucket-name",
    download_path="s3_downloads/"
)

# Upload a file
s3_client.upload_file(filepath='usr/hello.txt', object_key='test-folder/hello.txt',content_type='text/str')

# Download a file
s3_client.download_to_file(object_key='test-folder/hello.txt')

# Delete a file
s3_client.delete_file(object_key='test-folder/hello.txt')
```