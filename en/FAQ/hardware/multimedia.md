# FAQ on Multimedia Function Development

### **What screen models are currently supported or debugged?**&#x20;

*   LCD black and white screen: SSD1306 & UC1628
*   LCD color screen: ILI9225, ST7735 and ST7789
*   MIPI: ST7701S

### **The relationship between the SPI interface of ECxxxA/ECxxxM/ECxxxN series modules and the LCD and camera drivers**

The LCD\_SPI in the three series modules in the title can be used as an ordinary SPI, while the CAM\_SPI is specially used to drive the camera and cannot be used as an ordinary SPI interface.

### **What are the supported camera models?**

*   30W pixel: GC032A
*   8W pixels: BF3901

### **What is the highest pixel count supported by the camera?**&#x20;

*   30W pixels.