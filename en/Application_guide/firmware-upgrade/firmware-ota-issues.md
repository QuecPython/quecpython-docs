# Frequently Asked Questions

## Is it necessary to make an upgrade package for FOTA upgrades?

Yes. The firmware file in the firmware package cannot be used directly as an upgrade package.

## How to make an upgrade package?

The process of making an upgrade package may vary with module models. Please refer to [FOTA Upgrade Package Making](./firmware-ota-delta-package-making.md) for more information.

## Reasons for failure in making an upgrade package

Firmware with different partition structures cannot be used in one OTA upgrade. Therefore, it is important to ensure that the partition structures of the firmware used for making the upgrade package are consistent. The official firmware for the same module model will have the same firmware partition structure. If there is any special reason for the OTA upgrade failure, it will be specified.

## How to resolve unexpected power loss during a FOTA upgrade?

If a power loss occurs during the download phase, the upgrade needs to be triggered again and the upgrade package needs to be downloaded again after the module is powered on again. If a power loss occurs during the upgrade phase, the upgrade progress and interrupt node will be recorded. After the module is powered on again, the upgrade will continue from the interrupt node until the upgrade ends and the device is rebooted.

## How to resolve unexpected power loss during an application OTA upgrade?

If a power loss occurs during the download phase, the upgrade needs to be triggered again after the module is powered on again. The files that have already been downloaded will not be downloaded again, but the files that were interrupted during the previous download will be downloaded again. If a power loss occurs during the upgrade phase, the upgrade progress and interrupt node will be recorded. After the module is powered on again, the upgrade will continue from the interrupt node until the upgrade ends and the device is rebooted.

## How to handle unstable network conditions?

If the network response times out, the indicator of an upgrade failure will be returned from the upgrade API. You need to call the upgrade API again to trigger a FOTA upgrade. For the MiniFota upgrade solution, the upgrade will be suspended until the network is restored.

## What is a MiniFOTA upgrade?

MiniFOTA upgrade solution is a customized solution that combines both DFOTA upgrade and FullFOTA upgrade solutions. It is designed for devices with limited flash size that do not have enough space to store the upgrade package file. When making the upgrade package, two upgrade package files will be generated, and multiple automatic reboots will be required.

## How to call upgrade API for automatic reboots in the MiniFOTA upgrade solution?

The MiniFota upgrade solution is a customized solution that requires the module to enter the minimum system mode before performing an OTA upgrade. Therefore, after calling the OTA upgrade API, the device will immediately reboot into the minimum system mode. In the minimum system mode, the device will automatically connect to the network, download the upgrade package, and perform the upgrade process until the upgrade is successfully completed. No user intervention is required in the minimum system mode, just wait for the upgrade to be completed. This process may involve multiple device reboots.

## Why can't user scripts be DFOTA upgraded?

User scripts are stored in the file system partition, and the content of the file system partition will change after the device is powered on, making it impossible to use the delta algorithm for a DFOTA upgrade.

## How to upgrade the file system partition by using the MiniFOTA upgrade solution?

The MiniFOTA upgrade supports upgrading the file system partition, and user script files and other files are also stored in the file system partition, which enables both the firmware and user files to be upgraded together in the MiniFOTA upgrade solution. When you make an upgrade package, the *fs* partition must be included. Please refer to [FOTA Upgrade Package Making](./firmware-ota-delta-package-making.md) for more information.

## Does the application OTA upgrade include integrity verification during download?

The OTA upgrade process does not include integrity verification, but you can add the integrity verification step. The method is to pass the pre-calculated MD5 checksum value during the download and compare it with the calculated MD5 checksum value of the downloaded content on the device to check if they are consistent.

## How much memory size should be reserved for the file system during application OTA upgrades?

Sufficient memory size needs to be reserved, which should be larger than the size occupied by all the files to be upgraded. Note that even if the content of a file is less than 4 KB, the file will occupy 4 KB, and a directory will occupy 8 KB. Lack of memory size will lead to an upgrade failure.

## Is it necessary to reserve memory size for firmware OTA upgrades?

The FOTA upgrade memory size is guaranteed by the device's underlying system. If the size of the upgrade package exceeds the memory limit, the upgrade will fail.

## Watchdog

During the upgrade process, the device is in a special mode and cannot run applications, so it cannot feed the external watchdog circuit. In this case, the timeout period of the watchdog should not be shorter than the maximum time required for a successful upgrade.

## Download protocols

The download protocols supported include HTTP, HTTPS, and FTP. The specific support for each model is shown in the following table.

| Model                                | HTTP | HTTPS  | FTP    |
| ------------------------------------ | ---- | ------ | ------ |
| EG91xN series | Support | Support | Not support |
| EC200U&EG915U series     | Support | Support | Support |
| EC200A series                          | Support | Support | Support |
| BG95&BG600L series                     | Support | Support | Support |

## Troubleshooting application OTA upgrade failures

### Possible reasons

1. Insufficient memory of file system. Sufficient memory size needs to be reserved, which should be larger than the size occupied by all the files to be upgraded. Note that even if the content of a file is less than 4 KB, the file will occupy 4 KB, and a directory will occupy 8 KB. Lack of memory size will lead to an upgrade failure.

2. Network interruption during download. The download API needs to be called again to continue downloading the remaining files.

3. Residual intermediate files from previous interrupted upgrades due to power loss. Before executing an OTA upgrade, it is recommended to check if there are any residual upgrade files from the previous upgrade. If there is any, it needs to be deleted.

### Solution

In case of a failed application OTA upgrade due to power loss or other reasons, the file system partition may contain downloaded upgrade files and intermediate files. To avoid affecting the next normal upgrade, it is recommended to delete the residual upgrade files. The following code can be added before the upgrade:

```python
if ql_fs.path_exists("usr/.updater"):
	ql_fs.rmdirs('usr/.updater')
```

## Troubleshooting firmware OTA upgrade failures

### DFOTA upgrade solution

#### -1 is returned after the upgrade API is called without upgrade progress

1. Check whether the network connection is normal, whether the SIM card is inserted, whether the antenna is connected, and whether the data call is successful.

2. Check whether HTTPS is used as the download protocol. Only some models support the HTTPS protocol. If a device that does not support HTTPS protocol uses a URL of HTTPS protocol, the download will fail. In such cases, the HTTPS URL needs to be changed to an HTTP URL. The models that support HTTPS protocol are EG91xN series, EC200U series, EC200A series, and BG95&BG600L series when in the DFOTA upgrade solution.

3. Check whether the URL is correct. You can enter the URL in a browser and check whether the package can be downloaded successfully.

#### -1 is returned after the upgrade API is called with upgrade progress

1. Version mismatch. The current firmware version downloaded into the device must be consistent with the old version used to make the upgrade package to ensure a correct upgrade package.

2. The difference between the old and target firmware versions is too large, resulting in an upgrade package that is too big to be stored in the storage space. Check the size of the upgrade package. The maximum size limit for an upgrade package for each model is as follows.

| Model                | Maximum Size of an Upgrade Package |
| -------------------- | ---------------------------------- |
| EG91xN series        | 3 MB                               |
| EC200U&EG915U series | 700 KB                             |
| EC200A series        | 3.5 MB                             |
| BG95&BG600L series   | 7 MB                               |

#### 0 is returned after the upgrade API is called, but the version number remains unchanged after the device reboots.

Version mismatch. The current firmware version downloaded into the device must be consistent with the old version used to make the upgrade package to ensure a correct upgrade package.

### MiniFOTA Upgrade Solution

#### The device remains in the upgrade state without any response.

1. Check whether the network connection is normal, whether the SIM card is inserted, whether the antenna is connected, and whether the data call is successful.

2. Check whether the URL is correct. You can enter the URL in a browser and check whether the package can be downloaded successfully.

3. Check whether HTTPS is used as the download protocol. MiniFOTA upgrade solution does not support HTTPS download protocol. The HTTPS URL needs to be changed to an HTTP URL.

#### The device keeps rebooting and cannot recover to a normal state.

Capture the debug UART log and catstudio log and provide them to Quectel Technical Support for analysis.

#### After the device reboots and recovers to the normal state, the version number is still the old one and the upgrade is not successful.

Version mismatch. The current firmware version downloaded into the device must be consistent with the old version used to make the upgrade package to ensure a correct upgrade package.