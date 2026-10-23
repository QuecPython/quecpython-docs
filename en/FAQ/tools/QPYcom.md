# FAQ on QPYcom 

### **After the module is connected to the PC, QPYcom does not display the COM port**

1.  Make sure the module is powered on. Turn on the development board by pressing the PWRKEY button (The startup can be determined by whether the VDD\_EXT pin of the module outputs 1V8 voltage.  If it does, which means the module is on);

2.  Make sure the driver corresponding to the module is installed. The module drivers of different platforms are varied. If you cannot confirm, please consult FAE;

3.  Check that the USB connection cable is indeed a 4-wire USB cable and can perform data exchange;

4.  When drawing PCB, please check whether the USB\_VBUS pin has been pulled up since pin controls USB recognition enable (Note: It will be a failure to enter low power consumption mode if the module is pulled up. Thus,  It is recommended to connect it directly to the PC\_USB).

### **After QPYcom opens the interactive port, it shows that the device is not connected or there is no response to the operation.**

1.  Confirm whether Python firmware has been burnt into module. which can be queried by ATI command  via USB\_AT\_Port. If the firmware name does not contain the characters of QPY, it proves that it is not Python firmware. Please download Python firmware again;

2.  Check whether the selected port is correct and on;

3.  Confirm whether the **main.py** file already exists in the module.

### **Are the source code files uploaded to the module safe?**&#x20;

Files uploaded to the module can be read through specific methods (such as HTTP\_Post. However,  the client source files can be safe. The QPYcom owns a feature of code obfuscation encryption. Thus, relevant codes can be encrypted during the mass production to ensure that user programs are not directly exposed. For detailed operations, please refer to the relevant chapters of "Advanced Development-Mass Production".

###  **Whether each module driver supports Windows7/WindowsXP system**

The drivers of different platforms are inconsistent. Most of them can support Windows10/Windows8/Windows7 instead of WindowsXP.

### **Does QPYcom support Windows7/WindowsXP system?**

The Windows released on the official website is 64-bit. It  can operate in 64-bit Windows 7/8/10. While for 32-bit, it is not supported currently.

### **Is the QPYcom available in Mac/Linux system?**

 Windows system and Linux system are available, while the Mac systems is to be developed.

### **Which serial port should be used to download firmware via QPYcom?**

The QPYcom does not need to select/enable the serial port to download the firmware since it will automatically identify the port. The ports enumerated by the module's USB driver will be endowed with unique and fixed ID, and QPYcom will identify the burning ports based on these IDs.

### **How to investigate if it fails to  download firmware via QPYcom?**

1.  Check whether the firmware version matches the module model;

2.  Check whether the selected firmware file is correct;

3.  Whether the firmware burning is successful after entering the force download mode.

### **It reminds syntax error when dragging files to the module tool**

1.  Check whether the ''.py'' file is UTF-8 coding. QuecPython only supports UTF-8 encoding;

2.  Check whether the indentation of the ".py" file complies with Python syntax. The indentation in Python is similar to "{}" in C language with definite range.

### **Can the QPYcom burn Python firmware only?**

The QPYcom is designed for debugging QuecPython. The firmware download function is oriented to python firmware without supporting burning other firmware.

### **Can QPYcom export the files from the module? If so, how to ensure the script security?**

Up to 2023/07/05, it is still unavailable in QPYcom. The internal files of the module can be read through the  Python exchange port in module.

To ensure the security of customer scripts, it is recommended for customers to encrypt scripts when publishing products and disable or encrypt the interactive port via the `system.replSetEnable(flag, **kw_args)` interface or the `system.replChangPswd(old_password, new_password)` interface.

### **Does the QPYcom support adding/deleting/modifying/checking module partitions?**

To adapt to different customer needs, up to current QPYcom\_V3.1.1 version, it is capable to adjust the customer partition on RDA8910 series modules and all modules on ASR platform, mainly on the proportion of the customer\_fs partition (usr partition where to store codes by customer) and customer\_backup\_s partition (bak partition where to store the backup codes of customer) ,

It is also available for customers to add external partitions (ext partitions) through external storage devices. However, the addition of this partition requires the external respond to HW device. 

### **The QuecPython script is in clear text. How to ensure the security of the script?**

QPYcom supports script encryption function and uses hash algorithm to encrypt clear text scripts into binary files. Protect script security and prevent important information from being exposed.

The script encryption and Hash Algorithm in QPYcom will encrypt the clear text into binary file, guard script and prevent important information from being exposed.

Besides that, a backup restoration is embedded in QPYcom, which will be triggered if it occurs exception in script located in usr partition,  avoiding mis-operation in script during program running.

### **Do firmware and script codes have to be burnt separately during mass production?**

Unnecessary actually. The QPYcom can not only merge the customer scripts into the firmware but also burn in batch, which can improve the efficiency greatly. For specific operations, please refer to the `QuecPython official website-Document Center-Advanced Development-Mass Production` in detail.

### **How to make the upgrade package required for FOTA upgrade? **

Up to QPYcom\_V3.1.1, QPYcom has already supported making upgrade packages for ASR\_CAT-1/RDA\_CAT-1/EC200A. The operation process is simple: Click the "FOTA Upgrade" button on the QPYcom, select different options according to different models and then carry out step by step. 
