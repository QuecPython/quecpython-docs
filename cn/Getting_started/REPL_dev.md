# REPL调试

REPL全称为<font color='red'>**Read-Eval-Print-Loop (交互式解释器)**</font>，可以在REPL中进行QuecPython程序的调试

运行 **QPYcom** 工具后，选择正确的串口（波特率无需指定）并打开，即可开始 Python 命令行交互。

- <font color='red'>**Step1：进入交互页面**</font>

进入交互页面首先需要打开USB交互口，注意不同平台交互口名称有差异

以EC600N/M系列模组举例，打开QPYcom工具，端口选择连接**Quectel USB REPL Port**，选择“交互”界面

| 模组                                             | 交互口名称                    | 驱动名称                        |
| ----------------------------------------------- | ----------------------------- | ------------------------------- |
| EC600N/M<br>EC200A                               | **Quectel USB REPL Port** | QuecPython_USB_Driver_Win10_ASR |
| EC200U<br>EC600U/G<br>EG800G<br>EG912U<br>EG915U | **Quectel USB REPL Port** | QuecPython_USB_Driver_Win10_U_G |
| BG95/BG600L                                      | **Quectel USB REPL Port** | QuecPython_USB_Driver_Win10_BG  |
| EC600E,EC800E                                    | **Quectel USB 串行设备**      | QuecPython_USB_Driver_Win10_E   |

注意只有使用最新版本的固件和新版本的驱动才会显示 Quectel USB REPL Port

在老版本的固件和驱动上串口的显示名称会根据平台的不同有所区别

其中U、G系列模组交互口名称为**Quectel USB Serial-1 Port**

N/M、A系列模组交互口名称为**Quectel USB MI05 COM Port**

BG95、BG600L系列模组交互口名称为**Quectel USB NEMA Port**

- <font color='red'>**Step2：打开串口**</font>

点击“打开串口”按钮，在交互界面输入**print(‘hello world’)**，按回车后可以看到执行的结果信息

```python
>>> print('hello world')
hello world
```

![](media/readme/hello_world.png)

<font color='red'>*注意：工具交互页面输入时需要输入英文字符，中文字符将会被屏蔽*</font>