## REPL

REPL stands for Read-Eval-Print-Loop (interactive interpreter). You can debug QuecPython programs in REPL.

Open **QPYcom**, select the correct serial port (no need to specify the baud rate) and open it to start Python command line interaction.

- <font color='red'>**Step1: Enter the "REPL" Tab**</font>

Open QPYcom, set "COM Port" to **Quectel USB REPL Port**, and click the "REPL" tab.

Note that the name of the interface is different on different platforms.

| module                                            | REPL Name                    | Driver Name                        |
| ----------------------------------------------- | ----------------------------- | ------------------------------- |
| EC600N/M<br>EC200A                               | **Quectel USB REPL Port** | QuecPython_USB_Driver_Win10_ASR |
| EC200U<br>EC600U/G<br>EG800G<br>EG912U<br>EG915U | **Quectel USB REPL Port** | QuecPython_USB_Driver_Win10_U_G |
| BG95/BG600L                                      | **Quectel USB REPL Port** | QuecPython_USB_Driver_Win10_BG  |
| EC600E,EC800E                                    | **Quectel USB 串行设备**      | QuecPython_USB_Driver_Win10_E   |

Note that the **Quectel USB REPL Port** is displayed only when using the latest firmware version and the new driver version

The display name of the serial port on older firmware and drivers varies depending on the platform

The name of the interface of the U and G series modules is **Quectel USB Serial-1 Port**

The name of the interface for the N/M and A series modules is **Quectel USB MI05 COM Port**

BG95, BG600L series module interface name is **Quectel USB NEMA Port**

- <font color='red'>**Step2: Open the Serial Port**</font>

Click "**Open port**", enter **print('hello world')** on the "REPL" page, and press "**Enter**" to view the execution result.

```python
>>> print('hello world')
hello world
```

![](media/readme/hello_world.png)

<font color='red'>*Note: Only English characters can be entered on the "REPL" page, and Chinese characters will be blocked.*</font>