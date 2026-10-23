# Aws S3 – AWS S3 Storage Client

## Overview

The `S3Client` class provides a convenient interface for interacting with AWS S3 storage. It enables operations such as file uploads, downloads, and deletions, while ensuring secure communication through HTTP SigV4 authentication.

## Constructor

### `S3Client`
```python
class S3Client(region=None, access_key=None, secret_key=None, bucket=None, config_path="config.json", download_path="")
```

**Parameters:**

- **region** *(str, optional)*: Specifies the AWS region where the S3 bucket resides.
- **access_key** *(str, optional)*: AWS Access Key ID for authentication.
- **secret_key** *(str, optional)*: AWS Secret Access Key for authentication.
- **bucket** *(str, optional)*: Name of the S3 bucket to interact with.
- **config_path** *(str, optional)*: Path to a JSON configuration file. Defaults to `"config.json"`.
- **download_path** *(str, optional)*: Local directory for storing downloaded files. Defaults to an empty string.

**Example:**

- Using Explicit Parameters

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

- Using a Configuration File

```python
from s3client import S3Client

client = S3Client(config_path="usr/config.json")
```

## S3 Communication

The `S3Client` uses HTTP SigV4 signed requests to perform operations such as uploading, downloading, and deleting files from AWS S3 storage.

### `upload_file`

```python
upload_file(filepath, object_key, content_type='application/octet-stream')
```

Uploads a local file to the specified AWS S3 bucket.

**Parameters:**

- **filepath** *(str)*: Local path to the file you want to upload.
- **object_key** *(str)*: The key (path) under which to store the file in the S3 bucket.
- **content_type** *(str, optional)*: The MIME type of the file (default: `application/octet-stream`).

**Return Value:**

- A **response object** containing all information returned by the server, such as response status codes, response headers, and response bodies.

- **None** if an exception occurs.

### `download_to_file`

```python
download_to_file(object_key)
```

Downloads a file from the S3 bucket and saves it locally in the `download_path` defined in the configuration or constructor.

**Parameters:**

- **object_key** *(str)*: The key (path) of the file you want to download from the S3 bucket.

**Return Value:**

- A **response object** containing all information returned by the server, such as response status codes, response headers, and response bodies.

- **None** if an exception occurs.

### `delete_object`

```python
delete_object(object_key)
```

Deletes a file (object) from the S3 bucket.

**Parameters:**

- **object_key** *(str)*: The key (path) of the file you want to delete from the S3 bucket.

**Return Value:**

- A **response object** containing all information returned by the server, such as response status codes, response headers, and response bodies.

- **None** if an exception occurs.

## Example code 

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