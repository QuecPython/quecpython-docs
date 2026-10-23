# Firmware Upgrade

As we all know, Python script runtime environment is necessary for QuecPython script software development, and the runtime environment is compiled into the firmware of the module that supports QuecPython. Therefore, when you get Quectel modules, you must first download the firmware provided by the QuecPython team, which supports the Python script runtime environment, so that you can develop the script software.

When you need to upgrade the firmware for development (such as feature updates or bug fixes), you can re-download the target firmware with the download tool, or upgrade the firmware over the air.

For products that have already been encapsulated (no download port reserved) or have been sold, it is not possible to upgrade the firmware using the burning tool, and only the OTA upgrade is available.

This series of documents will focus on the principles and user guide of firmware download and OTA upgrade.

- [Firmware Download](./firmware-burning.md)
- [Firmware OTA Upgrade](./firmware-ota.md)