# FAQ on Mass Production

###  **How to solve when it is a failure to burn firmware?**

This failure is commonly caused by following reasons:

1. The firmware version does not match the module model. Just change the corresponding firmware version of the module.
2. The module failed to enter download mode. Some modules need to manually enter the download mode before starting the firmware download, such as EC800E; it is also possible that a software failure fails to enter the download mode by command. encountering with such issue,  manually pull the USB_BOOT pin to enter the download mode.
3. The firmware or tool storage path contains Chinese characters, causing a failure to download. Change firmware or tool path.

The last but the least note: Since the firmware format uploaded by the website is in `.zip` , the suffix will be as same as the that in ASR firmware. In order to distinguish firmwares and compressed packages, the ASR firmware suffix is changed to `.bin` format. Pay special attention when downloading the ASR firmware.

### **Can firmware be burned in batches after the project is developed?**
Of course. After the actual project development is completed, the project script can be directly merged into the firmware, and then the mass production tool can be used to burn the firmware one by one. For specific operations, please refer to the chapter `‘QuecPython Official Website-Document Center-Advanced Development-Mass Production’` for details.

### **Is it possible to burn only part of the partition data when burning firmware?**

Yes. However, the QPYcom tools and mass production tools cannot be selected, and full-chip burning is simple and easy to operate. Unless special demand, it is not recommended to burn part of partition.
