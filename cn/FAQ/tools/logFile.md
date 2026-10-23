# 模组底层日志相关问题

### **如何抓取模组底层日志**

- 模组底层日志有debug log、ap log和cp log等


- debug log用模组debug口输出，展锐平台会对log进行加密，需要用原厂工具解密，其他平台可用串口工具输出

- 展锐平台抓取ap log和cp log需要用到不同的工具，抓取ap log用抓cooltools，cp log用ArmTracer

- ASR平台抓取ap log和cp log都用CATStudio


- 高通平台抓取ap log和cp log都用Qwinlog


### **如何抓取模组dump死机时的日志**

- 当模组发生dump时，底层看门狗会对模组进行复位，因此抓取dump log需要先发AT指令关闭看门狗


| 平台 | 模组                        | AT指令                                              |
| ---- | --------------------------- | --------------------------------------------------- |
| ASR  | EC600N/EC600M/EC800N/EC800M | AT+QDUMPCFG=0,0<br/>AT+QDUMPCFG=1,0<br/>AT+log=19,1 |
| ASR  | EC200A                      | AT+QDUMPCFG=0,0<br/>AT+QDUMPCFG=2,1<br/>AT+log=19,1 |
| 展锐 | EC200U/EC600U/EC600G/EC800G | AT+QDBGCFG="DUMPCFG",0,1                            |
| 高通 | BG95/BG600L                 | at+qcfgext="dump",1                                 |

- ASR平台抓取dump log用teraterm


- 展锐平台抓取dump log用cooltools


- 高通平台抓取dump log用QPST




> 以上提到的工具都可咨询移远技术支持提供