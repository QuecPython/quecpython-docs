# 短信问题

### LTEonly是否支持短信功能，是否需要依赖VOLTE

看具体型号，大多数型号会支持短信功能，不一定需要依赖VOLTE。

注意：如果遇到用移动和联通的卡能发短信，用电信的卡不能的话，那么是固件没有内置volte，因为当没有volte时，移动和联通的卡走的是NAS短信，所以不用volte也可以发短信，但是电信卡不支持走NAS短信。

如何验证是否支持：help('modules')查看是否有sms，有则支持。

| 模组                | 平台 | sms  |
| ------------------- | ---- | ---- |
| EC600U-CNLB         | RDA  | ✓    |
| EC600U-CNLC         | RDA  | ✓    |
| EC600U-CNLB_EXTFS8M | RDA  | ✓    |
| EC600U-CNLC_EXTFS8M | RDA  | ✓    |
| EC600U-EUAB         | RDA  | ✓    |
| EC200U-EUAB         | RDA  | ✓    |
| EC200U-CNLB         | RDA  | ✓    |
| EC200U-CNLA         | RDA  | ✓    |
| EC200U-EUAA         | RDA  | ✓    |
| EC200U-CNAA         | RDA  | ✓    |
| EC200U-AUAA         | RDA  | ✓    |
| EG912U-GLAA         | RDA  | ✓    |
| EG915U-EUAB         | RDA  | ✓    |
| EC800G-CNGA         | RDA  | ✓    |
| EC600M-CNLA_VOLTE   | ASR  | ✓    |
| EC600M-CNLE_VOLTE   | ASR  | ✓    |
| EC800M-CNLE_VOLTE   | ASR  | ✓    |
| EC200A-EUHA         | ASR  | ✓    |
| EC200A-AUHA         | ASR  | ✓    |
| EC200A-CNLA         | ASR  | ✓    |
| EC200A-CNDA         | ASR  | ✓    |
| EC600N-CNLC         | ASR  | ✓    |
| EC600N-CNLF         | ASR  | ✓    |
| EC600N-CNLD         | ASR  | ✓    |
| EG915N-EUAG         | ASR  | ✓    |
| EG912N-ENAA         | ASR  | ✓    |

