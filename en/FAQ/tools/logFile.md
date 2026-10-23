# FAQ on Modules underlying log

### **How to capture the module’s underlying logs?**

*   The underlying logs of the module include debug log, ap log, cp log and etc.

*   The debug log is output via the module debug port. For Unisoc platform, it will encrypt the log and decrypt with the tool form original manufacturer. For other platforms, the serial port tool call be applied to output.

*   Unisoc platform: Different tools are required to capture ap log and cp log. The cooltools and Armtracer will be used to capture AP log and CP log respectively. 

*   ASR platform: The CATStudio will be used to capture no matter AP log or CP log.

*   Qualcomm platform: The Qwinlog will be used to capture no matter AP log or CP log.

### **How to grab the log when it occurs dump in module?**

*   When a dump occurs in the module, the underlying watchdog will reset the module. Therefore, for sake of capturing the dump log,  please issue an AT command to turn off the watchdog first.

| Platform | Modules                     | AT commands                                           |
| -------- | --------------------------- | ----------------------------------------------------- |
| ASR      | EC600N/EC600M/EC800N/EC800M | AT+QDUMPCFG=0,0<br />AT+QDUMPCFG=1,0<br />AT+log=19,1 |
| ASR      | EC200A                      | AT+QDUMPCFG=0,0<br />AT+QDUMPCFG=2,1<br />AT+log=19,1 |
| Unisoc   | EC200U/EC600U/EC600G/EC800G | AT+QDBGCFG="DUMPCFG",0,1                              |
| Qualcomm | BG95/BG600L                 | at+qcfgext="dump",1                                   |

*   Tool to capture dump log on ASR platform: teraterm

*   Tool to capture dump log on Unisoc platform: cooltools

*   Too to capture dump log on Qualcomm platform: QPST

> All tools mentioned above can be provided by consulting Quectel technical support.

