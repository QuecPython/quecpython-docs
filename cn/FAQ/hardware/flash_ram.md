# Flash和RAM问题

### 用户可以使用的Flash空间有哪些、有多大

1. uos.statvfs()接口查询
2. 每个型号对应的FLASH大小都不相同，具体可以在移远QuecPython交流群咨询要一份需求表：”Quectel_QuecPython_客户需求表搜集“，即可查询。

### 用户可以使用的RAM空间有哪些、有多大

1. uos.statvfs()接口查询
2. 每个型号对应的RAM大小都不相同，具体可以在移远QuecPython交流群咨询要一份需求表：”Quectel_QuecPython_客户需求表搜集“，即可查询。


### 是否支持外扩flash，支持什么接口及什么型号

不同平台支持的Flash不同，详情请看下面：

展锐：Nor Flash型号：

| 4线Nor Flash型号 | 备注   |
| :--------------- | :----- |
| w25Q64JWSSIM     | 已压测 |
| GD25LE64E        |        |
| GD25LE64C        |        |
| XM25QU64B        | 已压测 |
| XM25QU128C       |        |
| GD25LQ64C        | 已压测 |
| GD25LQ64E        | 已压测 |
| W25Q256JWFQ      |        |
| GD25LQ128E       |        |
| W25Q128JWSSIQ    |        |
| W25Q128JWSSIM    |        |
| W25Q64JVSIQ      |        |
| W25Q16JVSIQ      |        |
| W25Q32           |        |
| ZD25Q32          |        |
| P25Q32L          |        |
| XM25QU32C        |        |



| 6线Nor Flash型号 | 备注           |
| ---------------- | :------------- |
| w25Q128JWSIQT    |                |
| GD25LQ128DGSIG   |                |
| GD25LQ64ESIG     |                |
| GD25LQ128DYIGR   |                |
| GD25Q64C         |                |
| w25Q32JW         |                |
| GD25LQ32E        |                |
| XM25QU64CHIQT08s |                |
| GD25LQ64EWIGR    |                |
| w25Q64JWSSIM     |                |
| XM25QU64         | 已压测         |
| GD25LE64E        |                |
| GD25LQ128C       |                |
| XM25QU128        | 已压测         |
| w25Q128JW        |                |
| w25Q32JW         |                |
| w25Q64JV         |                |
| ZB25VQ64         | 客户项目中使用 |
| MD25Q64C         | 客户项目中使用 |



ASR：Flash型号

| Flash型号  | 备注 |
| ---------- | ---- |
| GD25LE64E  |      |
| XM2 5QU64C |      |
| GD25LQ64C  |      |
| XM25QU128B | 16M  |
| ZB25LQ16   |      |
| ZB25LQ32   |      |
| ZB25LQ64   |      |
| P25Q16L    |      |
| P25Q32L    |      |
| P25Q64L    |      |
| P25Q128L   |      |
| MX25U3235F |      |
| GD25LE/LQ16|      |
| GD25LE/LQ32|      |
| GD25LE/LQ64|      |
| GD25LE/LQ128|      |
| XM25QU16   |      |
| XM25QU32   |      |
| XM25QU64   |      |
| XM25QU128  |      |
| W25Q32JW   |      |
| W25Q64JW   |      |
| W25Q128JW  |      |
