# app_fota - 用户文件升级相关功能

`app_fota`模块用于用户文件升级。

**示例**：

```python
import app_fota
from misc import Power

fota = app_fota.new()
download_list = [{'url': 'http://www.example.com/app.py', 'file_name': '/usr/app.py'}, {'url': 'http://www.example.com/test.txt', 'file_name': '/usr/text.txt'}]
fota.bulk_download(download_list) # 下载
fota.set_update_flag() # 设置升级标志
Power.powerRestart() # 重启
```

## 初始化相关功能

### `app_fota.new`

```python
app_fota.new()
```

创建app_fota对象。

**返回值描述：**

app_fota对象。

**示例**：

```python
import app_fota
fota = app_fota.new()
```

## 下载相关功能

### `fota.download`

```python
fota.download(url, file_name)
```

下载单个文件。

**参数描述：**

- `url`-待下载文件的url，类型为str。
- `file_name`-本地待升级文件的绝对路径，类型str。

**返回值描述**：

成功返回0，否则返回-1。

### `fota.bulk_download`

```python
fota.bulk_download(info=[])
```

下载批量文件。

**参数描述：**

- `info`-批量下载列表，列表的元素均为包含了`url`和`file_name`的字典，类型为list。

**返回值描述**：

下载失败时返回下载失败的列表，类型为list。下载成功时返回空。

**示例**：

```python
download_list = [{'url': 'http://www.example.com/app.py', 'file_name': '/usr/app.py'}, {'url': 'http://www.example.com/test.txt', 'file_name': '/usr/text.txt'}]
fota.bulk_download(download_list)
```

该示例中，假设`http://www.example.com/test.txt`下载失败，则该方法返回值为`[{url: 'http://www.example.com/test.txt', file_name: '/usr/text.txt'}]`。

## 设置升级标志相关功能

### `fota.set_update_flag`

```python
fota.set_update_flag(use_rename=False)
```

设置升级标志。设置完成升级标志后，调用重启接口，重启后即可启动升级工作。升级完成后会直接进入应用程序。

**参数描述：**

- `use_rename`-是否使用重命名模式进行升级，类型为bool，默认值为`False`。为`False`时使用默认模式，升级时通过拷贝方式写入目标文件；为`True`时使用重命名模式，当暂存目录与目标路径位于同一文件系统时，升级时通过重命名方式移动文件。

## 空间预留说明

`app_fota`升级时，文件会先下载到暂存目录（默认为`/usr/.updater/`，使用外部分区时为对应分区的`.updater`目录），重启后再从暂存目录写入到目标路径。因此需要为升级预留足够的可用空间，否则会导致下载或升级失败。

预留空间大小与升级模式，以及目标文件是新增文件还是更新文件有关：

- **新增文件**（目标路径原本不存在该文件，升级时新建目标文件）：
    - **默认模式**（`fota.set_update_flag()`）：暂存副本与新建的目标文件副本会同时存在，需预留待升级文件总大小的2倍。
    - **重命名模式**（`fota.set_update_flag(use_rename=True)` 且暂存目录与目标路径位于同一文件系统）：通过`rename`将暂存文件移动到目标路径，不产生重复副本，需预留待升级文件总大小的1倍。
- **更新文件**（目标路径已存在同名文件，升级时覆盖原文件）：原文件已占用的空间会被复用，两种模式均只需额外预留约为待升级文件总大小的1倍（暂存副本）。

此外，还需额外预留暂存目录文件夹（`.updater`）约 16KB 及配置文件约 4KB 的空间。

> 说明：当目标路径与下载路径位于不同分区时，例如目标为外部分区/ext，或`EC200U`、`EC600U`、`EG912U`、`EG915U`、`EC600G`、`EC800G`、`EG700G`、`EG800G`型号的暂存目录位于独立的`/fota`分区（`/fota/usr/.updater/`）而非`/usr`分区，升级属于跨分区操作，此类情况下暂存副本占用的是下载分区空间，目标分区只需为目标文件预留约为待升级文件总大小的1倍空间。

