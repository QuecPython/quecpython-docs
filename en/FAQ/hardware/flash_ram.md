# FAQ on Flash and RAM 

###   **What Flash space that users can use and how large it is?**

1.  Query via `uos.statvfs()` interface 

2.  The size of FLASH corresponding to each model is different. 

###   **What RAM space is available to users and how large is it?**

1.  Query via `uos.statvfs()` interface

2.  The RAM size corresponding to each model is different. For details,For details, you can ask for <Quectel\_QuecPython\_Customer demand list collection> .

### **Does it support external flash extension and what interfaces and models are supported?**

Different platforms support different Flash, please see below for details:

Unisoc: Nor Flash model:

| 4-wire SPI NOR Flash model | Remark             |
| :------------------------- | :----------------- |
| w25Q64JWSSIM               | Passed stress test |
| GD25LE64E                  |                    |
| GD25LE64C                  |                    |
| XM25QU64B                  | Passed stress test |
| XM25QU128C                 |                    |
| GD25LQ64                   | Passed stress test |
| GD25LQ64E                  | Passed stress test |
| W25Q256JWFQ                |                    |
| GD25LQ128E                 |                    |
| W25Q128JWSSIQ              |                    |
| W25Q128JWSSIM              |                    |
| W25Q64JVSIQ                |                    |
| W25Q16JVSIQ                |                    |
| W25Q32                     |                    |
| ZD25Q32                    |                    |
| P25Q32L                    |                    |
| XM25QU32C                  |                    |




| 6-wire SPI NOR Flash model | Remark                    |
| -------------------------- | :------------------------ |
| w25Q128JWSIQT              |                           |
| GD25LQ128DGSIG             |                           |
| GD25LQ64ESIG               |                           |
| GD25LQ128DYIGR             |                           |
| GD25Q64C                   |                           |
| w25Q32JW                   |                           |
| GD25LQ32E                  |                           |
| XM25QU64CHIQT08s           |                           |
| GD25LQ64EWIGR              |                           |
| w25Q64JWSSIM               |                           |
| XM25QU64                   | Passed stress test        |
| GD25LE64E                  |                           |
| GD25LQ128C                 |                           |
| XM25QU128                  | Passed stress test        |
| w25Q128JW                  |                           |
| w25Q32JW                   |                           |
| w25Q64JV                   |                           |
| ZB25VQ64                   | Used in customer projects |
| MD25Q64C                   | Used in customer projects |



ASR: Flash model

| Flash model | Remark |
| ----------- | ------ |
| GD25LE64E   |        |
| XM2 5QU64C  |        |
| GD25LQ64C   |        |
| XM25QU128B  | 16M    |

