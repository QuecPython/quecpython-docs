# Introduction to NICs Supported by QuecPython

This section provides a brief introduction to the NICs supported by the QuecPython solution, including cellular NIC, Wi-Fi NIC, Ethernet NIC, and USB network adapter.

## Introduction to Supported NICs

### Cellular NIC

Most of the modules supported by QuecPython are cellular communication modules, which gives QuecPython a natural advantage in supporting cellular NICs. It boasts comprehensive functional interfaces and robust underlying technical support.

### Wi-Fi NIC
QuecPython provides Wi-Fi device management functions, enabling control of Wi-Fi devices through hardware interfaces by attaching external Wi-Fi chips or through porting QuecPython onto Wi-Fi chips. 
For external Wi-Fi NICs, which are intended for module chips that do not have built-in Wi-Fi NIC, QuecPython supports connecting external Wi-Fi NICs through serial ports or SDIO hardware interfaces, such as ESP8266/FC41D/ASR5803. 
For built-in Wi-Fi NICs, QuecPython can be directly ported to the Wi-Fi chip, such as FCM360W/FC41D.

### Ethernet NIC
QuecPython provides Ethernet NIC/PHY chip management functions. QuecPython has built-in lwIP protocol stack and EMAC layer, which enables it to drive Ethernet/PHY chips. It now supports various SPI/RMII interface Ethernet chips.

### USB Network Adapter
QuecPython provides USB network adapter management functions. QuecPython integrates RNDIS/ECM/MBIM protocols on the USB interface, which allows it to be recognized as a NIC device in different systems (such as Windows/Linux/Android/IOS) and directly establish network connections.

## List of Supported NICs

<table>
    <tr>
        <th colspan = '2'>NIC Type</th>
        <th colspan = '1'>Supported Module</th>
    </tr>
    <tr>
        <td  colspan = '2'>Cellular NIC</td>
        <td>All except Wi-Fi chip modules</td>
    </tr>
    <tr>
        <td rowspan="4" >Wi-Fi NIC</td>
        <td>ESP8266</td>
        <td>EC600N series, EC600M series</td>
    </tr>
        <tr>
        <td>FC41D</td>
        <td>EC600N series, EC600M series</td>
    </tr>
    <tr>
        <td>ASR5803</td>
        <td>EC200A series</td>
    </tr>
    <tr>
        <td>Built-in WLAN</td>
        <td>FCM360W and FC41D</td>
    </tr>
    <tr>
        <td rowspan="4" >Ethernet NIC</td>
        <td>W5500</td>
        <td>EC600N series and EC600M/EC800M series</td>
    </tr>
    <tr>
        <td>DM9051</td>
        <td>EC600N series</td>
    </tr>
    <tr>
        <td>CH395</td>
        <td>EC600U series</td>
    </tr>
        <td>YT8512/SZ18201/JL1101</td>
        <td>EC200A series</td>
    </tr>
    <tr>
        <td rowspan="3">USB Network Adapter</td>
        <td>RNDIS</td>
        <td>All except Qualcomm chip modules and Wi-Fi chip modules</td>
    </tr>
    <tr>
        <td>ECM</td>
        <td>All except Qualcomm chip modules and Wi-Fi chip modules</td>
    </tr>
        <tr>
        <td>MBIM</td>
        <td>Only supported on Qualcomm chip modules</td>
    </tr>



</table>