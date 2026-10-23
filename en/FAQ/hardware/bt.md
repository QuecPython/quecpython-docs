# FAQ on Bluetooth development 

### **Key Differences between Classic Bluetooth and Bluetooth Low Energy**

There are some major differences between Classic Bluetooth (BT) and Bluetooth Low Energy (BLE).

*   Power consumption: The most obvious difference is power consumption. Bluetooth Low Energy (BLE) is designed for low-power scenarios, and its energy consumption is much lower than Classic Bluetooth (BT). BLE is suitable for devices that require long-term operation and inconvenient battery replacement, such as health monitoring equipment, smart home applications, etc.
*   Data transmission rate: Classic Bluetooth is equipped with a high data transmission rate up to 1-3 Mbps, which is suitable for audio transmission, file transfer and other application scenarios that require higher transmission rates. While the data transmission rate of Bluetooth Low Energy is about 1 Mbps at maximum, which is suitable for scenarios where the amount of data transmission is small.
*   Transmission distance: Under the same conditions, the transmission distance of classic Bluetooth is usually slightly longer than that of low-power Bluetooth. The transmission distance of Classic Bluetooth is up to 100 meters, compared with 50 meters of BLE. The actual transmission distance is affected by signal power, transmission environment, and equipment hardware.
*   Connection time: Bluetooth Low Energy consumes less time than Classic Bluetooth in connection. BLE devices typically establish connection within a few milliseconds, while classic Bluetooth usually takes about few seconds. This makes Bluetooth Low Energy more suitable for application scenarios that require frequent connection establishment.&#x20;
*   Application scenarios: Due to the above differences, there are obvious difference in practical applications. Classic Bluetooth is suitable for audio equipment, vehicle communications, file transfer and other scenarios, while BLE is widely used in low-power scenarios such as wearable devices, IoT devices, and health monitoring.&#x20;
*   Compatibility: Compatibility between Bluetooth Classic and Bluetooth Low Energy is limited. However, since there exist differences in transmission protocols, hardware requirements,  even if modern Bluetooth devices support dual-mode (i.e. Classic Bluetooth and Bluetooth Low Energy). As a result, not all of device can be compatible with each other. 

### **What is the maximum transmission rate that can be reached in BT?**

The transmission rate of Classic Bluetooth (BT) increases with upgrade of Bluetooth version . See following maximum transmission rates of different versions of Bluetooth:

*   Bluetooth 1.x: 1 Mbps at maximum.

*   Bluetooth 2.x + EDR (Enhanced Data Rate):  3 Mbps at maximum.

*   Bluetooth 3.0 + HS (High Speed): integrating with the 802.11 Wi-Fi protocol, the theoretical maximum transmission rate will up to 24 Mbps.

*   Bluetooth 4.x: 3 Mbps at maximum, mainly on the  Bluetooth low energy technology (BLE) in introduced in Bluetooth 4,x.

*   Bluetooth 5.x: 2 Mbps at maximum. While the rate will be decreased to 500 kbps in long-distance transmission mode for wider transmission range.

Note: These transmission rates are theoretical maximum values. In actual applications, they may be affected by device hardware, distance, and environmental interference. The actual transmission rate may be lower than the theoretical value.

### **What is the maximum transmission rate of BLE?**

The maximum transmission rate of Bluetooth Low Energy (BLE, Bluetooth Low Energy) increases with the upgrade of Bluetooth version. For BLE, see following theoretical maximum transmission rates of different versions:

*   Bluetooth 4.x: BLE was first introduced in Bluetooth 4.x with a maximum transmission rate of approximately 1 Mbps.
*   Bluetooth 5.x: In Bluetooth 5.x, the maximum transmission rate is improved to 2 Mbps.

Note: These transmission rates are theoretical maximum values. In actual applications, they may be affected by device hardware, distance, and environmental interference. The actual transmission rate may be lower than the theoretical value. At the same time, when using Bluetooth Low Energy, you are usually more concerned about its low energy consumption rather than the transmission rate.

> Bluetooth 4.x technology includes two modes: Classic Bluetooth and Bluetooth Low Energy (BLE). In Classic Bluetooth, the maximum transmission rate of Bluetooth 4.x is 3 Mbps. While in Bluetooth Low Energy (BLE), the maximum transmission rate of Bluetooth 4.x is approximately 1 Mbps.
> In conclusion, as description aforementioned, the 3 Mbps data is for the classic Bluetooth mode of Bluetooth 4.x, while the 1 Mbps data is for the Bluetooth Low Energy (BLE) mode of Bluetooth 4.x.

### **What are the Bluetooth server and client?**

Server (Server) and Client (Client) refer to the party of the device in the communication process. The server is responsible for storing and serving data, while the client is responsible for connecting to the server and requesting access to its data. In Bluetooth communication, the roles of server and client can be exchanged between each other. For example, in BLE, the device can act as a GATT server to provide data, or it can also act as a GATT client to access data services of other devices.

### **What are the master and slave devices of Bluetooth?**

Master and Slave refer to the role of the device in the connection process. The master is typically responsible for issuing connection requests, managing connection parameters, and maintaining the device's connection status. While the slave is the connected device and is responsible for responding to the  request from the master. In Bluetooth communication, the roles of master and slave can be exchanged between each other during different connection processes.

> In Bluetooth communication, server/client and master/slave are two independent concepts without direct correspondence. The specific master-slave relationship and server-client relationship depend on the connection and communication requirements between devices and whether BT or BLE is used.

### **Bluetooth CCID**

In the BLE (Bluetooth Low Energy) server, the characteristic descriptor with UUID of 0x2902 is the Client Characteristic Configuration Descriptor (CCCD), which is used to manage the notification and indication functions of the characteristic value. Notifications and Indications are two mechanisms that can be used when real-time updates of characteristic values ​​(for example, changes in sensor data) need to be sent to client devices.

In order to support these functions, a feature descriptor with UUID of 0x2902 needs to be added to the corresponding feature.

The characteristic descriptor with UUID of 0x2902 needs to be added to the BLE server in following situations:

*   When real-time updates of characteristic values ​​need to be sent to the client, such as environmental sensor indications or device status changes.
*   When characteristic needs to support notifications or indications. Notification is a low-latency data update mechanism that does not require client confirmation, while instructions require client confirmation to ensure the reliability of data transmission.

In practice, after adding a characteristic descriptor with a UUID of 0x2902 for a characteristic, the client can enable or disable the notification or indication function by writing to the descriptor. For example, a client can write 0x0001 to enable notifications, 0x0002 to enable indications, or 0x0000 to disable both.

It’s important to note that not all characteristics need to support notifications and indications. Only when the characteristic value needs to be updated in real time, you need to add a descriptor with UUID of 0x2902.

### **What is the difference between public addr and random addr of BLE?**

In BLE (Bluetooth Low Energy), the device address is used to identify a Bluetooth device uniquely. There are two types of device addresses: Public Address and Random Address. The main differences between two addresses are address origin, changeability, and privacy protection.

**1. Public Address：**

A public address serves as the permanent address of one device,  which will be assigned to the device manufacturer by IEEE (such as the Bluetooth SIG). This address is used to identify the device for its global uniqueness. The public address is typically written into the device's hardware during the device's production phase and remains unchanged throughout the device's lifetime. The first 24 bits (OUI) of the public address are assigned by the IEEE to the manufacturer, and the last 24 bits are assigned by the manufacturer to the device.

**2. Random Address：**

A random address is an address generated by the device dynamically in stage of using. it provides a level of privacy protection since it can be changed as actual need. Random address are divided into three sub-types:

*   Static Random Address: A random address that generated when the device is started or reset, which can remain unchanged after the device is shut down or reset. This kind of address provides a certain level of stability and privacy protection. 
*   Private Resolvable Random Address: This random address can be resolved by other paired devices, but is random for unpaired devices. Private resolvable addresses can be changed periodically during device connection, providing a high degree of privacy protection.&#x20;
*   Private Non-Resolvable Random Address: This random address is random for all devices and cannot be resolved. Private non-resolvable addresses provide the highest degree of privacy, but may prevent the device from reconnecting after being disconnected.

In summary, the main differences between Public Address and Random Address are:

*   Assigned by the device manufacturer, the public Address is permanent and globally unique; while the Random Address is dynamically generated and can be changed during device use.
*   Public Address remains unchanged during the device life cycle; Random Address can be changed as needed, providing varying degrees of privacy protection.

### **Solution to the problem of no sound in Bluetooth calls via set\_pa() interface**

**Background**：

For customer equipment, when the audio.set\_pa() interface is vacant, it is normal to call via BT with sound output. However, after calling audio.set\_pa(),  there is no sound in BT call even if it is normal to play music and local audio. In addition,  if the customer directly uses GPIO to enable the PA control pin, there will also be sound during the Bluetooth call.

**Analysis**：

Only one speaker channel is used and and an external PA is connected to the channel in user. When a Bluetooth call came in, the user will switch the channel to the speaker in the application code. Based on former description, it can be judged that the external PA is off, resulting in no sound during Bluetooth calls;

So is it normal to play Bluetooth music and local audio? it lies in that the set\_pa function on Unisoc platform only supports audio, which means Bluetooth music and local audio. In fact, for the module, the audio-related play interface is called finally since only this processing path can trigger the callback for setting the pa. The pa is turned on and off respectively at the beginning and end of play. However, the Bluetooth call will go through a completely different logic, and the audio-related play interface will not be called, which will not trigger the callback for setting pa.

There are two scenarios for users actually：

**Scenario 1: The user calls audio.set\_pa() without playing Bluetooth music. At this time, there is no sound during the Bluetooth call.**

This is because the Unisoc audio.set\_pa() is actually an initialization interface for the pa control pin while the real control of pa is done in the callback. When the user calls audio.set\_pa(),  the external pa control pin is initialized and set it as low level by default, which is equivalent to turn off PA. However, the Bluetooth call can't trigger PA control callback, resulting in sound output. 

**Scenario 2: The user calls audio.set\_pa(), and plays Bluetooth music. At this time, the Bluetooth music plays normally, and then calls the mobile phone that has established a Bluetooth connection with the device. After answering, there is no sound in the Bluetooth call.**

There is no sound in this scene because when the mobile phone is playing music, if a call comes in, the mobile phone will pause the music first. After the module receives the pause command, the module will also stop playing. At this time, the pa setting callback will be triggered to close it, so there is no sound during Bluetooth calls;

The above two scenarios are essentially caused the PA being turned off, and the Bluetooth call itself cannot control the PA.

**Solution**：

On the module side, when receiving a ringing event of Bluetooth call, turn on pa; then when receiving a  hang-up event, turn off pa as well. To sum up, there is a principle for controlling PA. Whoever currently uses the corresponding channel will open the PA connected to the corresponding channel.
