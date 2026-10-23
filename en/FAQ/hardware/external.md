# FAQ on External Components Development

### **What Ethernet chips and PHY chips are currently supported?**

*   Ethernet Chips: W5500, DM9051 and CH395
*   PHY Chips: YT8512, SZ18201 and JL1101

### **What peripheral drivers are supported?**

Some supported models are shown in the table below. For more models, please contact FAE.

| Dev               | Interface     | Module Type                                      |
| ----------------- | ------------- | ------------------------------------------------ |
| LCD Screen        | SPI           | ST7735/ST7789/ST7567/HX8347GC9305/SSD1306/UC1628 |
| Camera            | SPI           | GC032A/BF3901                                    |
| Memory  extension | SDIO/SPI      | SDCARD/Norflash                                  |
| IO  extension     | SPI/I2C       | CH423S/MCP23017                                  |
| G Sensor          | I2C           | ADXL346/LIS2DH12TR/SCA7A20/BMA25x                |
| NFC               | SPI/UART      | PL51NF001/RC522/PN532                            |
| Wi-Fi             | UART          | ESP8266                                          |
| Meter chip        | UART          | BL0939/HLW8110                                   |
| Ethernet Chip     | SPI           | W5500/DM9051/CH395                               |
| Ethernet Phy Chip | SPI           | YT8512/SZ18201/JL1101                            |
| Nixie Tub         | I2C           | TM1605                                           |
| Humiture          | I2C           | AHT10/SHT10/SHT20                                |
| Lightness Sensor  | I2C           | GY320/BH1750/GL5528                              |
| Audio PA          | Analog signal | AW8733ATQR                                       |

