# FAQ on Power Supply and Switch

### **Why does it fail to boot when clicking the power button?**
Ensure that the power supply voltage is not pulled down and the peak current can reach more than 2A.

Check whether the boot pin level corresponds to the requirements of the hardware manual.

Use a multimeter to test whether VDD_EXT reaches 1.8V. If it does, it means it is powered on.

### **Why does the code restart abnormally after running for a few minutes?**
Generally, abnormal restart is a dump phenomenon, which is necessary to capture the dump log for R&D analysis in accordance with corresponding methods applied in specific module .

Another possibility is that the `Power.powerRestart()` is called by application code to restart the software. In this situation, the code logic needs to be checked.

Use `Power.powerOnReason()` to see the reason for the restart.

###  **How to get the boot reason?**


The `Power.powerOnReason()` will be valid. 

### **Can it be optimized if the boot time is long? **
When the module is powered on, it starts from the bootloader to RTOS, then to the file system, and finally mounts main.py. This process takes a certain amount of time, and the startup time of each stage has been optimized.

### **Can the logo be displayed immediately after booting?**
It is needed to make pictures, interface information, and LCD drivers into logo.bin using the tools provided on the official website to, and replace them in the module firmware package.

### **How to get the battery power and charging status?**
The `Power.getVbatt()` will be available to acquire the battery voltage.

The QPY firmware does not have a charging management function. Those who need to charge lithium batteries can use a charging IC.

