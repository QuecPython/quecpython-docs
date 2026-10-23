# Developing the First Script

Before starting to write code, please ensure that you have completed the preparation of the hardware, firmware, and software environment.

## Writing the First Script File

Open VSCode and enter the following string:

```python
print("hello world")
```

<img src="media/VScode1.jpg" alt="image-2021081301" style="zoom:150%;" />

Next, save the written script locally and name the file `helloworld.py` (only English letters, numbers, and underscores are allowed; **Chinese characters and special characters are prohibited**).

<img src="media/VScode2.png" alt="image-2021081301" style="zoom:150%;"/>

Next, transfer the edited file to the module via QPYcom and run it.

## Transferring the Script File to the Module

Before transferring the script file, ensure that the QPYcom serial port is open.

<img src="media/QPYcom1.jpg" alt="image-2021081301" style="zoom:150%;" />

**Method 1:** Transfer via Drag-and-Drop

Open QPYcom, click `File`, then click the button shown in the figure to select the path of the `helloworld.py` script file.

<img src="media/QPYcom2.jpg" alt="image-2021081301" style="zoom:150%;" />

After selecting the path where the `helloworld.py` script file is stored, the written script file can be seen on the left side.

<img src="media/QPYcom3.jpg" alt="image-2021081301" style="zoom:150%;" />

Drag the `helloworld.py` script file from the left side to the `usr` directory on the right side for transfer. The download progress and result will be displayed in the status bar at the bottom.

<img src="media/QPYcom4.jpg" alt="image-2021081301" style="zoom:150%;" />

The display of `helloworld.py` in the `usr` directory on the right side indicates that it has been successfully transferred to the module.

<img src="media/QPYcom5.jpg" alt="image-2021081301" style="zoom:150%;" />

**Method 2:** Transfer via Tool Buttons

Click the "**+**" button on the right side to transfer the file.

<img src="media/QPYcom6.jpg" alt="image-2021081301" style="zoom:150%;" />

Locate the written `helloworld.py` file and click **Open** in the bottom right corner.

<img src="media/QPYcom7.png" alt="image-2021081301" style="zoom:150%;" />

The display of `helloworld.py` in the `usr` directory on the right side indicates that it has been successfully transferred to the module.

<img src="media/QPYcom8.jpg" alt="image-2021081301" style="zoom:150%;" />

**Method 3:** Transfer by Creating a User Project

Click the **Download** page.

Create a user project (click the "Create" button).

Enter the project name and click "OK".

<img src="media/QPYcom9.jpg" alt="image-2021081301" style="zoom:150%;" />

The created project is displayed on the left side.

<img src="media/QPYcom10.jpg" alt="image-2021081301" style="zoom:150%;" />

In the "User Scripts/Files" area, select the root directory of `usr`, right-click and select **Add File**.

<img src="media/QPYcom11.jpg" alt="image-2021081301" style="zoom:150%;" />

After successful addition, the `helloworld.py` file ready for transfer will be displayed. Click the icon in the bottom right corner <img src="media\readme\QPYcom17.png"/>, and options "Download Script" and "Download Firmware" will appear. Select **Download Script** for transfer.

<img src="media/QPYcom12.jpg" alt="image-2021081301" style="zoom:150%;" />

After successful download, a prompt "File downloaded successfully" will appear.

<img src="media/QPYcom13.jpg" alt="image-2021081301" style="zoom:150%;" />

## Executing the Script File

**Method 1**: Execute the Script via REPL

After opening the serial port, enter the following code on the QPYcom interactive page to execute the script file:

```python
import example
example.exec("/usr/hello world.py") # filePath is the path of the script file to be executed
```

**Method 2**: Execute the Script via GUI Tool

On the file page of the tool, select the `helloworld.py` script file to be executed, then click the <img src="media\readme\QPYcom18.png"/> button to run it.

<img src="media/QPYcom14.jpg" alt="image-2021081301" style="zoom:150%;" />

**Method 3**: Run the Script File via Right-Click Menu

Select the script file and right-click to run it.

<img src="media/QPYcom15.jpg" alt="image-2021081301" style="zoom:150%;" />

**The execution result is shown in the figure below**

<img src="media/QPYcom16.jpg" alt="image-2021081301" style="zoom:150%;" />

### Stop a Running Program

Depending on the type of running script file, there are the following methods to stop it:

<table valign="center">
	<thead>
    <tr>
		<th>Program </br>name is "main.py"</th>
		<th>Program </br>Contains Endless Loop</th>
		<th>Program </br>Uses Multithreading</th>
		<th valign="middle">Stop Steps</th>
	</tr>
	</thead>
    <tr>
		<td rowspan="4">✓</td>
		<td rowspan="2">✓</td>
		<td rowspan="1">✓</td>
		<td>1. Press "Ctrl + A" to enter RAW mode <br> 2. Press "Ctrl + D" to restart the QuecPython virtual machine <br> 3. Press "Ctrl + B" to return to normal interaction mode <br> 4. If the above methods are ineffective, please re-download the firmware</td>
	</tr>
	<tr>
		<td rowspan="1">✗</td>
		<td>1. Press "Ctrl + C" to interrupt the program execution <br> 2. If the above method is ineffective, please re-download the firmware</td>
	</tr>
	<tr>
		<td rowspan="2">✗</td>
		<td rowspan="1">✓</td>
		<td>1. Press "Ctrl + A" to enter RAW mode <br> 2. Press "Ctrl + D" to restart the QuecPython virtual machine <br> 3. Press "Ctrl + B" to return to normal interaction mode <br> 4. If the above methods are ineffective, please wait patiently for the program to finish running</td>
	</tr>
	<tr>
		<td rowspan="1">✗</td>
		<td>1. Press "Ctrl + C" to interrupt the program execution <br> 2. If the above method is ineffective, please re-download the firmware</td>
	</tr>
	<tr>
		<td rowspan="4">✗</td>
		<td rowspan="2">✓</td>
		<td rowspan="1">✓</td>
		<td>1. Press "Ctrl + D" to restart the QuecPython virtual machine <br> 2. If the above method is ineffective, please reboot the module</td>
	</tr>
	<tr>
		<td rowspan="1">✗</td>
		<td>1. Press "Ctrl + D" to restart the QuecPython virtual machine <br> 2. If the above method is ineffective, please reboot the module</td>
	</tr>
	<tr>
		<td rowspan="2">✗</td>
		<td rowspan="1">✓</td>
		<td>1. Press "Ctrl + C" to interrupt the program execution <br> 2. If the above method is ineffective, please re-download the firmware or reboot the module</td>
	</tr>
	<tr>
		<td rowspan="1">✗</td>
		<td>1. Press "Ctrl + C" to interrupt the program execution <br> 2. If the above method is ineffective, please re-download the firmware or reboot the module</td>
	</tr>
</table>

## Common Issues and Precautions

### 1. No Output When Executing the Script / Prompt "File Does Not Exist"

- Incorrect path spelling: When executing the `exec` function in REPL, the case/spelling of the path and file name is inconsistent with the file in the module;
- Wrong file selected for GUI execution: The target script in the `usr` directory on the right side is not selected, so no actual program runs after clicking the execute button;
- Script syntax errors: There are syntax issues in the script (such as missing parentheses, indentation errors), resulting in silent failure with no output during execution.

### 2. Failed to Download the Script File

- Serial port not ready: The serial port is not opened before download, or the corresponding firmware is not flashed to the development board / firmware flashing fails, resulting in no response from the module;
- Illegal naming/path: The file name contains Chinese characters or special characters, or the local file path is selected incorrectly;
- Improper drag-and-drop operation: The local file is not dragged to the `usr` directory area of the module on the right side.