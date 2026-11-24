# Introduction to EC200U/A Evaluation Board

## Supported Module List

- [EC200U-CN](https://www.quectel.com.cn/product/ec200u-series)
- [EC200U-EU](https://www.quectel.com.cn/product/ec200u-series)
- [EC200A-CN](https://www.quectel.com.cn/product/ec200a-series)
- [EC200A-AU](https://www.quectel.com.cn/product/ec200a-series)
- [EC200A-EU](https://www.quectel.com.cn/product/ec200a-series)

## Feature List

### Basic Overview

The QuecPython_EC200U/A C4-P01 evaluation board is a compact and portable board designed specifically for QuecPython.

The main equipped modules are the EC200U/A series LTE Cat 1/LTE Cat 4 wireless communication modules.

The evaluation board has a USB Type-C interface, making it convenient for development. Developers only need a USB Type-C cable to easily use the evaluation board.

Additionally, the evaluation board is compatible with the expansion board of Raspberry Pi Zero, which can be directly used on the evaluation board.


## Evaluation Board Resources

### Function Description

The main component and interface placement of the evaluation board is shown in the following figure:

![img](./media/ec200x-evb/200U-1.png)

Evaluation board dimensions:

![img](./media/ec200x-evb/200U-chicun.png)

The main pin placement of the evaluation board is shown in the following figure:

![img](./media/ec200x-evb/EC200X_EVB.png)

>**Tip**
>
>The function multiplexing pins beginning with A/U in the figure indicate the functional difference of EC200A and EC200U series modules, that is, the function multiplexing pins beginning with U will take effect only when the module is EC200U. The function multiplexing pins starting with A takes effect only when the module is EC200A.
>
>For more information about the evaluation board, please visit <https://www.quectel.com.cn/download-zone>.

### Resource Download

- [EC200U_A_C4-P01-Series-EVB_SCH](https://developer.quectel.com/en/wp-content/uploads/sites/2/2024/11/EC200U_A_C4-P01-Series-EVB_SCH.pdf)
- [EC200U_A_C4-P01-Series-EVB_Silkscreen](https://developer.quectel.com/en/wp-content/uploads/sites/2/2024/11/EC200U_A_C4-P01-Series-EVB_Silkscreen.pdf)


### Evaluation Board Configuration

The detailed assignment of the peripheral interfaces on the evaluation board is as follows:

| No.  | Name           | Model        | Silkscreen | Comment                                               |
| ---- | -------------- | ------------ | ---------- | ------------------------------------------------------------ |
| 1    | USB Type-C Interface | -            | -          | -                                                            |
| 2    | SIM Card Slot 1| SMN-315-ARP7 | SIM1  | Nano-SIM                                                     |
| 3    | SIM Card Slot 2| MUP-C7809-1  | SIM2    | -                                                        |
| 4    | SD Card Slot   | TF-101A-P3   | SD    | Before using the SD card, enable SD to provide power to the SD card in the 10-Pin Header. |
| 5    | USB Power Supply Switch | -       | USB_ON/OFF | When the switch is normally closed, Type-C supplies power to the module and evaluation board, and USB can be connected normally. When the USB switch is normally open, Type-C supplies power only to the evaluation board. In this case, the module is not powered. |
| 6    | MIPI Interface | -            | MIPI_DSI   | When the main module is EC200U-XX, it supports MIPI LCD and is fully compatible with Raspberry Pi peripherals. |
| 7    | Camera Interface | -            | -          | The evaluation board supports customized SPI cameras with a maximum of 300,000 pixels. |
| 8    | Matrix Keyboard  Interface | -            | KO&KI      | The matrix keyboard function is reserved. Users can customize the soldering or connection of the keyboard keys. No matrix keyboard on the board by default. |
| 9    | MIC&SPK Interface | AW8733A      | +MIC-/-SPK+ | -                                                            |
| 10 | 40-Pin Header | - | - | Onboard 40-Pin header; For details, see the above figure and table. |
| 11 | 10-Pin Header | - | - | Onboard 10-Pin header; For details, see the above figure and table. |
| 12 | ADC Interface | - | ADC0_IN/ADC1_IN | ADC testing interface |
| 13 | PWK Botton | - | PWK | Turn-on button |
| 14 | RST Button | - | RST | Reset button |
| 15 | BOOT Button | - | BOOT | Firmware burning button |
| 16 | Antenna Interfaces | - | LTE<br/>DIV/WIFI/BT<br/>GNSS | LTE antenna connector<br/>DIV/Wi-Fi/Bluetooth antenna connector<br/>GNSS antenna connector |

**The evaluation board has 5 functional indication LEDs, as follows:**

- **P01**: Module Pin 1, default as EC200A-XX PWM0.
- **P05**: Module Pin 5, NET_MODE indication LED.
- **SCK1**: SIM1 detection indication LED, lights up when SIM1 is inserted.
- **SCK2**: SIM2 detection indication LED, lights up when SIM2 is inserted. Default connection to Pin 128. EC200A-XX does not have SIM2, this LED can be ignored.
- **PWR**: Power indication LED.

>The positions of the above indication LEDs refer to the silkscreen on the top of the evaluation board mentioned in the previous text (the side where the module is located is the top).

### Evaluation Board Interfaces

This evaluation board provides 40 pins compatible with Raspberry Pi, as well as 10 pins with enabling function. The functions of the evaluation board are described as follows:

**Pin Assignment of 10-Pin Header **

| **Header** | **No.** | **Silk**screen | **Function** |
| ---------- | ------- | -------------- | ------------ |
| 10-Pin     | 1       | PWK_AUTO       | POWERKEY   Auto turn on  |
| 10-Pin     | 2       | PWK_AUTO       | GND     Auto turn on     |
| 10-Pin     | 3       | VDD_EXT        | 1.8 V    Provide power externally when connected and test power consumption when disconnected    |
| 10-Pin     | 4       | VDD_EXT        | VDD_EXT    Provide power externally when connected and test power consumption when disconnected  |
| 10-Pin     | 5       | MODULE_EN      | 3.8 V   Connect DCDC to power the module     |
| 10-Pin     | 6       | MODULE_EN      | 3.8 V   Connect DCDC to power the module     |
| 10-Pin     | 7       | SD_EN          | SD_EN   Enable SD power supply     |
| 10-Pin     | 8       | SD_EN          | 3.3 V   Enable SD power supply     |
| 10-Pin     | 9       | GNSS_EN        | 3.3 V   Enable GNSS active power supply     |
| 10-Pin     | 10      | GNSS_EN        | GNSS_EN  Enable GNSS active power supply    |

>When testing power consumption, make sure to switch off the USB power supply switch (USB_ON/OFF) on the back of the evaluation board.

**Singular-pin Assignment of 40-Pin Header**

| **Header** | No. | **Name** | **Function Multiplexing** | **Function** |
| ---------- | -------------- | -------- | ------------------------ | ------------ |
| 40-Pin | 1              | 3V3      | -                        | 3.3 V output |
| 40-Pin | 3              | SDA      | I2C0_SDA/GPIO35          | I2C0 serial data/General-purpose input/output |
| 40-Pin | 5              | SCL      | I2C0_SCL                 | I2C0 serial clock |
| 40-Pin | 7              | P06      | GPIO46/A_PWM2            | General-purpose input/output/PWM2 output (When the module is EC200A) |
| 40-Pin | 9              | GND      | -                        | Ground       |
| 40-Pin | 11             | P64      | GPIO19                   | General-purpose input/output |
| 40-Pin | 13             | P136     | GPIO7                    | General-purpose input/output |
| 40-Pin | 15             | P03      | GPIO9                    | General-purpose input/output |
| 40-Pin | 17             | 3V3      | -                        | 3.3 V output |
| 40-Pin | 19             | MO1      | SPI1_MOSI/GPIO3          | SPI1 master-out slave-in/General-purpose input/output |
| 40-Pin | 21             | MI1      | SPI1_MISO/GPIO4          | SPI1 master-in slave-out/General-purpose input/output |
| 40-Pin | 23             | CLK1     | SPI1_CLK/GPIO1           | SPI1 clock/General-purpose input/output |
| 40-Pin | 25             | GND      | -                        | Ground       |
| 40-Pin | 27             | P63      | GPIO44/A_UART1_TXD       | General-purpose input/output/UART1 transmit (When the module is EC200A) |
| 40-Pin | 29             | P01      | GPIO28/A_PWM0            | General-purpose input/output/PWM0 output (When the module is EC200A) |
| 40-Pin | 31             | P02      | GPIO29                   | General-purpose input/output |
| 40-Pin | 33             | P04      | GPIO30/A_PWM1            | General-purpose input/output/PWM1 output (When the module is EC200A) |
| 40-Pin | 35             | P39      | GPIO13/A_SPI0_MISO       | General-purpose input/output/SPI0 master-in slave-out (When the module is EC200A) |
| 40-Pin | 37             | P134     | GPIO37                   | General-purpose input/output |
| 40-Pin | 39             | GND      |                          | Ground       |

**Double-pin Assignment of 40-Pin Header**

| Header | No.  | **Name** | **Function Multiplexing** | **Function**                                                 |
| ------ | ---- | -------- | ------------------------- | ------------------------------------------------------------ |
| 40-Pin | 2    | 5V       | -                         | 5 V output                                                   |
| 40-Pin | 4    | 5V       | -                         | 5 V output                                                   |
| 40-Pin | 6    | GND      | -                         | Ground                                                       |
| 40-Pin | 8    | TXD      | UART2_TXD                 | UART2 transmit                                               |
| 40-Pin | 10   | RXD      | UART2_RXD                 | UART2 receive                                                |
| 40-Pin | 12   | P37      | GPIO11/A_SPI0_CS          | General-purpose input/output/SPI0 chip select (When the module is EC200A) |
| 40-Pin | 14   | GND      | -                         | Ground                                                       |
| 40-Pin | 16   | P133     | GPIO8                     | General-purpose input/output                                 |
| 40-Pin | 18   | P65      | GPIO18                    | General-purpose input/output                                 |
| 40-Pin | 20   | GND      | -                         | Ground                                                       |
| 40-Pin | 22   | P127     | GPIO22                    | General-purpose input/output                                 |
| 40-Pin | 24   | CS1      | SPI1_CS/GPIO2             | SPI1 chip select/General-purpose input/output                |
| 40-Pin | 26   | P139     | GPIO20                    | General-purpose input/output                                 |
| 40-Pin | 28   | P66      | GPIO45/A_UART1_RXD        | General-purpose input/output/UART1 receive (When the module is EC200A) |
| 40-Pin | 30   | GND      | -                         | Ground                                                       |
| 40-Pin | 32   | P135     | GPIO6/U_PWM0              | General-purpose input/output/PWM0 output (When the module is EC200U) |
| 40-Pin | 34   | GND      | -                         | Ground                                                       |
| 40-Pin | 36   | P62      | GPIO43                    | General-purpose input/output                                 |
| 40-Pin | 38   | P38      | GPIO12/A_SPI0_MOSI        | General-purpose input/output/SPI0 master-out slave-in (When the module is EC200A) |
| 40-Pin | 40   | P40      | GPIO10/A_SPI0_CLK         | General-purpose input/output/SPI0 clock (When the module is EC200A) |

- ## Quick Start Guide

### Hardware Preparation

- <font color='red'>**Step 1: Required Items**</font>

- Development board
- USB-C cable (A-to-C)
- Windows 10 PC
- Nano-SIM card
- 4G antenna

- <font color='red'>**Step 2: Antenna & SIM Installation**</font>

- Attach the provided antenna to the MAIN antenna port.
- Insert the SIM card into the Nano-SIM slot.

- <font color='red'>**Step 3: Connect the Board**</font>

- Power the board via USB-C.

- <font color='red'>**Step 4: Power On**</font>

- Press and hold PWK until the power indicator light on the mainboard  (labeled as POW) lights up. 
- If you short-circuited PWK_ON, there's no need to press PWK; the board will power on  automatically.

><font color='red'>**If the PWR indicator is constantly on, the EVB is successfully turned on.**</font>

### Driver Installation

1. Download the **QuecPython USB Driver** from [QuecPython Official Drivers](https://www.quectel.com.cn/download-zone).
2. Extract and run `setup.exe` or `setup.bat`.
3. Verify installation in **Device Manager** (look for "Quectel USB" ports). Ignore unrelated devices like "Mobile ECM Network Adapter."

### Tool Installation

1. **QPYcom**: Download from [QuecPython Tools](https://www.quectel.com.cn/download-zone). Extract and use directly.
2. **VSCode Plugin**: Search for "QuecPython" in the Visual Studio Code marketplace.![](./media/readme/VScode_1.png)

### Firmware Burning

1. **Firmware Download**: Get the latest QuecPython firmware  from [QuecPython Firmware](https://developer.quectel.com/en/resource-download?cid=2) Page.

2. **Burning Steps**:
- Open QPYcom, select the USB REPL port, and create a new project.

- Choose the `.bin` firmware file 
- Set mode to "Download FW" and click **Download**.
- For BG95, enable DM serial port before burning.

![download_firmware1](./media/readme/download_firmware1.png)

![download_firmware2](./media/readme/download_firmware2.png)

<img src="./media/readme/download_firmware3.png" alt="download_firmware3" style="zoom:97%;" />

### REPL Debugging

1. Connect via QPYcom and open the REPL interface.
2. Test with `print("Hello World!")`.

![hello](./media/readme/hello.png)

## First Script Development

### Writing a Script

Create `helloworld.py`:

```python
print("Hello World!")
```

### File Transfer

- **Method 1**: Drag-and-drop files via QPYcom’s GUI.

![download_script1](./media/readme/download_script1.jpg)

- **Method 2**: Use the "Download Script" feature in QPYcom.

![download_script1](./media/readme/download_script2.jpg)

### Running Scripts

Execute via REPL:

```python
import example
example.exec("/usr/helloworld.py")
```

### Stopping Program Execution

How to stop a running program. The following methods are provided according to the type of the running script file: 

| Is the program name main.py? | Does the program contain a dead loop? | Does the program use multiple threads? | Stopping Steps |
| :--------------------------: | :-----------------------------------: | :------------------------------------: | :------------------------------------ |
|              ✓               |                   ✓                   |                   ✓                    | (1) Press Ctrl + A to enter RAW mode <br />(2) Press Ctrl + D to restart the QuePython virtual machine <br />(3) Press Ctrl + B to return to the normal interactive mode<br />(4) If the above methods fail, re-flash the firmware |
|              ✓               |                   ✓                   |                   ✗                    | (1) Press Ctrl + C to interrupt the program execution <br />(2) If the above method fails, re-flash the firmware |
|              ✓               |                   ✗                   |                   ✓                    | (1) Press Ctrl + A to enter RAW mode <br />(2) Press Ctrl + D to restart the QuePython virtual machine <br />(3) Press Ctrl + B to return to the normal interactive mode <br />(4) If the above methods fail, wait for the program to finish running |
|              ✓               |                   ✗                   |                   ✗                    | (1) Press Ctrl + C to interrupt the program execution <br />(2) If the above method fails, re-flash the firmware |
|              ✗               |                   ✓                   |                   ✓                    | (1) Press Ctrl + D to restart the QuePython virtual machine <br />(2) If the above method fails, directly restart the module |
|              ✗               |                   ✓                   |                   ✗                    | (1) Press Ctrl + D to restart the QuePython virtual machine <br />(2) If the above method fails, directly restart the module |
|              ✗               |                   ✗                   |                   ✓                    | (1) Press Ctrl + C to interrupt the program execution <br />(2) If the above method fails, re-flash the firmware or directly restart the module |
|              ✗               |                   ✗                   |                   ✗                    | (1) Press Ctrl + C to interrupt the program execution <br />(2) If the above method fails, re-flash the firmware or directly restart the module |


## Low Power Consumption Testing Guide

TheEC200X module supports multiple operating modes, each with distinct power consumption characteristics. The common operating modes are as follows:

**ACTIVE**: The module is engaged in LTE data transmission, GSM calls, or executing RTOS logic. Power consumption varies significantly depending on specific tasks and network communication standards, as both CPU power usage and RF power levels differ across scenarios.

**IDLE**: The module is in an idle state with hardware fully powered and RTOS running, but no active threads. It resumes operation immediately upon task initiation or incoming network activity. ECX00U series modules reduce clock frequency in IDLE mode, entering a light sleep state (disabling high-speed clocks while keeping the CPU active).

**Sleep**: Sleep mode requires the module to be idle with autosleep enabled. In this mode, RTOS pauses, clock frequency slows, and peripheral controllers (UART, SPI, etc.) are powered down, retaining only essential interrupt controllers to minimize power consumption.

**PSM**: PSM (Power Saving Mode) is a 3GPP-defined low-power mode where the module periodically wakes to perform tasks and remains in PSM sleep otherwise. During PSM sleep, behavior and power consumption resemble a powered-off state.

**Power Off**: The module is fully powered down, with baseband chips and peripheral controllers disabled (PMIC remains active). It can be awakened via Powerkey or RTC alarm.



| Module Model         | ECX00U   |
| :------------------- | :------- |
| IDLE (LTE FDD@64ms)  | 12.34 mA |
| SLEEP (LTE FDD@64ms) | 2.05 mA  |
| SLEEP (CFUN0)        | 1.29 mA  |
| Power Off            | 40 μA    |



### Test Preparation

- <font color='red'>**Step 1: Hardware Setup**</font>

Prepare the following components before starting:

1. **Development Board**  
2. **USB Cable** (USB-A to USB-TypeC)  
3. **PC** (Windows 10)  
4. **SIM Card**  
5. **4G Antenna**  
6. **Power Monitor Mini**  

- <font color='red'>**Step 2: Antenna & SIM Card Installation**</font>

Install the provided antenna into the MAIN antenna socket and insert the SIM card into the development board's SIM slot.

- <font color='red'>**Step 3: Board Connection**</font>

Connect the development board's Type-C port to the PC using a USB Type-C cable. Attach the power monitor's positive lead to MODULE_EN's Vbat (refer to [BG95 User Guide](https://developer.quectel.com/wp-content/uploads/2024/09/BG95-Pi-Zero%E7%B3%BB%E5%88%97%E5%BC%80%E5%8F%91%E6%9D%BF%E4%BA%A7%E5%93%81%E8%A7%84%E6%A0%BC%E5%8F%8A%E7%94%A8%E6%88%B7%E6%8C%87%E5%AF%BC.pdf)) and the negative lead to the module's GND. Connection diagram:

<img src="./media/ec200x-evb/PowerMonitor_connect.png" />

>**Important!**  
>Remove all jumper caps during measurements to avoid external interference.  

### Power Consumption Measurement

#### Power-Off Current

With the board connected via USB Type-C, power off the module by disabling USB_VBus (set USB switch to "off" on the board's rear). Measure the current while the module is inactive (output pins pulled low/floating). Average current remains stable at microampere levels.

<img src="./media/ec200x-evb/close_power.png" alt="Power-off current measurement" />

#### Idle Current

After power-off measurement, press and hold Powerkey to boot the module (default: IDLE mode). The module remains idle with RTOS running but no active threads, ready to resume instantly for tasks or calls.

<img src="./media/ec200x-evb/free_power.png" alt="Idle current measurement" />

#### Sleep Current

With autosleep enabled during idle, the module enters sleep mode by deactivating non-essential IP cores (peripherals/interrupts) and reducing clock frequency.

1. Re-enable USB_VBus.  

2. Open [QPYcom Tool](https://developer.quectel.com/wp-content/uploads/2024/09/QPYcom_V3.8.0.zip) and execute:  

```python
import pm
pm.autosleep(1)  # Enable autosleep
```

<img src="./media/ec200x-evb/open_auto_sleep.png" alt="Sleep current waveform" />

3. Disable USB_VBus again (switch to "off").
[API Reference](https://developer.quectel.com/doc/quecpython/API_reference/en/syslib/pm.html) | [Details](https://developer.quectel.com/doc/quecpython/Application_guide/en/system/power-consumption.html)

4. Sleep current shows periodic spikes:
<img src="./media/ec200x-evb/sleep_power.png" alt="Sleep current waveform" />

>**Notes:**
>
>- [Additional guidance](https://developer.quectel.com/doc/quecpython/Application_guide/en/system/power-consumption.html).
