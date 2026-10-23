# FAQ on Power Consumption 

### **What are the conditions for a module to enter low power consumption?**

The following conditions must be met：

*    Unplugged USB, USB\_VBUS has no external voltage.

*   All system wake locks are released (the number of wake locks can be obtained through the `pm.get\_wakelock\_num()` ).

*   No external pin interrupt interference.

*   Enable the system to sleep automatically (specific interface: `pm.autosleep(sleep\_flag)`).

*   CFUN=0 or normal network registration status.

### **Partial wake-up sources of low power consumption on Unisoc platform module**

| Wake-up source     | Type        | Wake-up conditions & processes                               |
| ------------------ | ----------- | ------------------------------------------------------------ |
| UART               | Peripherals | 1. The receiving and sending threads of all serial ports will block sleep. It is needed to stop sending and receiving. <br />2. MAIN_UART is included in low power consumption management by the open side and wakes up automatically when sending AT command. |
| RTC                | Peripherals | Wake up when it expires and use it appropriately.            |
| External Interrupt | Peripherals | Wake up when triggered. Keep the pin set as a interrupt in a non-wake-up state |
| USB                | Peripherals | Wake up when the USB is plugged in and disconnect when sleeping |
| Wakeup pin         | Peripherals | Hardware wake-up pin. it will wake up when pulled low and sleep when pulled high. |
| Timer              | Peripherals | Turn off unnecessary timers during sleep and only keep timers for necessary tasks |
| thread             | Software    | Threads other than IDEL will block the sleep function to enter the judgement. Sleep or block all of them. |
| Wakeup lock        | Software    | wake-up source set artificially, shut down when sleep is required |

###  **Sleep level and behavior of the Eigencomm platform**

| Sleep level | Description                                                  |
| ----------- | ------------------------------------------------------------ |
| IDLE        | Turn off the core clock and stop the large loop that keeps the bottom layer active, but any system interrupt can restart the core clock and re-enter the active state. |
| SLEEP1      | Most of the peripheral devices are powered off (except for some pins marked "Always on" in the specifications that still retain their functions). The external interrupts, main serial port and USB\_VBUS that retain their functions can wake up the module from this mode (approximately equivalent to deep-sleep on other platforms, the current version of the main serial port wake-up is invalid) |
| SLEEP2      | Close part of sram (MSMB part),  and leave the sram (ASMB) of 64KB in waiting mode |
| HIBERNATE   | Turn off the ASMB, that is, turn off all SRAMs.              |

### **What are the working modes of the BC25 module and the power consumption performance in each mode?**

The working modes of BC25 can be divided into following three types:

*   Active status:

    The network is active and data can be sent and received. All module functions are available normally, and the module can be switched to Idle mode in this mode.
*   Idle status:

    The network is in DRX state or eDRX state and can receive paging messages. All module functions are available normally, and the module can be switched to Active or sleep mode in this mode.
*   Sleep state:

    The network is in eDRX state or PSM state and cannot receive downlink data. After entering sleep, in order to minimize the power consumption of the module, the relevant peripherals will be turned off, the serial port will no longer respond to AT commands and the CPU will stop running. In this situation, the module status will be close to the shutdown state and only RTC works inside the module.

Power consumption in each mode：

*   Active mode: The network is active and the power consumption is high, which is necessary. There are three low-power consumption network application modes: DRX, eDRX and PSM. Among them, DRX is oriented to Idle state; eDRX can either be Idle state or sleep state; while the PSM is oriented to sleep state.

*   DRX mode: the module monitors the paging channel once in each DRX cycle, and the power consumption is higher than eDRX and PSM.

*   eDRX means that the module continuously turns on and off the receiver. The supported configuration time is 20.48s \~ 2.92h. The power consumption of eDRX is lower than that of DRX.

*   The PSM turns on and off the receiver occasionally, which will turn on the receiver once every few days. In PSM mode, the power consumption in entering sleep is only microampere level. Only in this working mode can the terminal achieve extremely low power consumption.

