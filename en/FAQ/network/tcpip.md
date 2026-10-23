# FAQ on TCP/IP

### **Why does it fail to connect to the server?**
- The server must be a public network address
- Use the TCP UDP test tool client or **mqtt.fx** on PC to connect to the server to confirm whether the connection can be successful and eliminate server failures.
- Once a SSL server is connected, confirm whether the core file supports the SSL function (for example, some core files of the 2G module do not support the SSL function)
- The SIM card of China Unicom is not suggested to use in 2G network 
- Check the module signal, network registration, network attachment, and PDP activation status
- Check whether the SIM card is in debt [For 4G module, it will fail to register to 2G network instead of 4G network]

### **Does TCP have an auto-reconnection function?**

There is no auto-reconnection in low layer and the reconnection is processed in application layer.

### **Will the TCP connection send ping packets automatically ?**

You can set the keep-alive time of **usocket.TCP_KEEPALIVE**. It is recommended to write the function of sending ping packets regularly.

### **What QoS does QuecPython MQTT support?？**

Currently both QoS0 and QoS1 are supported without QoS2. 

### **How to investigate the DNS decryption failure?**

Check whether the SIM card is successfully registered to the network and the validity of the address and try again.

### **Why does the network request fail after the SIM card is inserted and removed?**
Check whether the SIM card is successfully registered to the network and the validity of the address and try again.

### **In terms of MQTT client terminal built on myself , is there any MQTT server debugging tool?**
You can test operation status of mqtt through the **MQTT.fx**.

### **How to set APN information?**
If it is a public network SIM card, there is no need for user to set the APN. The software will query the APN and set it automatically   .

If it is a private network SIM card, please consult the SIM card carrier for APN parameters beforehand. Then set it as far forward as possible when initiating the TCP connection.

### **The same device can access to the Internet in one place, but cannot access to the Internet in another place**
Please confirm whether there is something wrong with SIM card or local network effects.

### **Why does the 4G module dedicated network card fail to connect to the server?**
- Check whether the APN parameters are set correctly
- If there are modules from other manufacturers, compare and test whether the connection is normal.
- If you cannot be 100% sure that the server configuration is correct, it is best to use wireshark to capture packets on the server side, or install a third-party tool on the server and open a server port for comparison testing.
- For IoT cards that use directed IP, you need to add the domain name or IP to the whitelist before using.

### **For same module, the network connection speed is various in different SIM cards, what is the reason? **
Please confirm any restriction in SIM card. Additionally, network mode should take into consideration.

### **How many sockets can be create at the same time for each series of modules?**
- EC200A series modules:  64 sockets at maximum
- ECxxxG/ECxxxU series modules: 15 sockets at maximum
- BG95 series modules: 20 sockets at maximum

### **What is the coding rule for the handle fd returned when creating a socket by each series of modules?**
- The handle fd returned by ECxxxA/ECxxxM/ECxxxN series modules when creating a socket starts to accumulate from 3, and  three fds, 0, 1, and 2, are reserved for internal use.
- The handle fd returned by ECxxxG/ECxxxU/BG95 series modules when creating a socket starts to accumulate from 0.

### **Large delay in data sending and receiving, slow speed, frequent failures or disconnections**
- Check whether the exception is caused by code logic error

- Check whether the exception is caused by consecutive reboots. 

- Check whether the server network is stable. Do not build the server via intranet penetration.

- Check whether the network coverage is poor in the usage environment, such as garages, underground, elevators, mountainous areas, etc.

- Check the module signal, network registration, network attachment, and PDP activation status

- Check whether it is a device antenna problem: Send the device’s antenna debugging indicator parameters to our technical support personnel

- If it occurs frequent disconnection:

     - If the tcp udp socket connection is used directly, please check the frequency of the heartbeat packet. For base station, it will close the connection if there is no data transmission for a long time. It is recommended that the frequency of the heartbeat packet should not exceed 4 minutes (2 minutes are recommended generally).

     - If the mqtt connection is used, check the mqtt keep-alive time. For base station, it will close the connection if there is no data transmission for a long time. It is recommended that the frequency of the heartbeat packet should not exceed 4 minutes (2 minutes are recommended generally).

     - If the mqtt connection is used, check whether the data is successfully sent to the server within 1.5 times of the mqtt keep-alive time. If the data is not successfully sent to the server, it will be actively disconnected by the server. This situation is generally caused by a timeout in sending data.

- Check whether it is a network problem: If all devices in the same location have problems, it may be related to the network environment; Thus, please compare and test by setting the mobile phone as the same network mode; or move the device to another location that is far away from this location for comparison.

- Check whether there is a problem with a single device: If some devices are normal and some devices are abnormal in the same location, analyze according to following aspects.

     - Analyze whether the environment of normal devices and abnormal devices are the same:

         If they are different, for example, the abnormal device is fixed on the steel wall and the normal device is placed on the table, the steel wall may interfere with the RF in antenna. Place the abnormal device and the normal device in the same usage environment, and then conduct a comparison test.

         If they are the same, refer to the steps below.

     - Analyze whether normal and abnormal devices are camped in the same cells:

         If they are the same, focus on the antenna RF of the abnormal device. If the results are failed, send the abnormal device to our R&D for analysis.

         If they are different, test it several times to confirm whether the problem is likely to occur in the abnormal community. If it is easy to occur in the abnormal community, the blocked cell will be the root. 

- Provide logs to QuecPython technical support personnel.

### **How to count flow?**
Operators count flow based on IP packet, which is composed by: IP header + TCP header + user data. Take IPv4 as an example:

- Fixed 20 bytes & optional 4 bytes in  IP header, at least 20 bytes;
- Fixed 20 bytes & optional 4 bytes in TCP header, at least 20 bytes.

The user data refers to data content that can be sensed by user. If the socket is used directly, it is the data that sensed by user; if protocols such as http, mqtt and ssl are used, such data is not the original data that can be sensed by user since the http header will be added into http, the mqtt package will be supplemented into mqtt and the data will be decrypted by ssl respectively.  

When sending and receiving data via TCP, the ACK mechanism will be implemented. For example, after sending data to the server by certain device, it will also receive a TCP ACK packet returned by the server. This TCP ACK packet (at least 40 bytes) is also counted in the flow. When receiving the data issued by the server, the device will also reply a tcp ack packet. Likewise, this ack packet is still counted in the flow.

In addition, socket connection and disconnection require multiple data transmission, which will also consume flow.

Assuming that there exists one TCP socket connection, after it succeeds to connect, the device sends  heartbeat data of 2 bytes to the server every minute. After receiving data, the server will reply with 2 bytes of heartbeat response data. During this process, the flow consumed per minute is at least 42 (sent by the device) + 40 (reply ack from the server) + 42 (sent by the server) + 40 (reply ack from the device) = 164 bytes. In actual process, one IP packet of 42 bytes can be merged probably by ack responded and data sent by server.  Thus,  at least 42+42+40 = 124 bytes are necessary. This is the simplest algorithm. However, in actual applications, retransmissions and optional bytes in the packet header must also be taken into account, which should result in more flow than calculated here. If you want to calculate accurately, it is recommended to use wireshark to capture and analyze the packets on the server side.

Another common example: when downloading a file through http, why is the flow consumed actually more traffic than the file itself? Similar to the previous example, the size of the file itself is only user data. In addition to user data, there will be flow consumptions in following aspects:

- Connect to the server
- There exists http header in http requests and in stage of sending data by server
- TCP and IP headers in each data package 
- Retransmission
- Disconnect from the server

### **Can the 4G module be used as a server and client at the same time?**
- For the IPv4, it generally cannot be used as a server. What the module can obtain is the LAN IP assigned by the operator; for the private network card, the IP address assigned by the operator can communicate within the private network. Therefore, it can be used as a server.

- For the IPv6 , its global link address is the public IP address since it can be used as a server.

### **One packet of data is less than 50B. However, the traffic consumed in a day is much greater than the actual transmission value**

If the TCP protocol is used, it takes three handshakes and four waves to complete a data interaction. The original data is not much, but since a packet of data determined by the TCP protocol must add header and tail frame verification, etc., however, the packet header and tail frame check shall be added into the package according to TCP. Therefore, the actual traffic consumed will be more than 50B. Some operators demands that each packet of data shall be  1KB at least. If less than 1KB, various checks will be added to reach 1KB.

### **If the KeepAlive is set for a long time, will it be disconnected by the base station?**

Yes, it is generally recommended to set 2 minutes and no more than 4 minutes. The base station will close the connection without data transmission for a long time. 

### **If there are many mqtt devices connected to the server, how to distinguish the devices when sending and receiving data?**

To ensure device uniqueness, you can set IMEI as clientid.