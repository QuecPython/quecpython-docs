# USSD - Unstructured Supplementary Service Data

USSD (Unstructured Supplementary Service Data) is a new type of real-time online interactive session data service based on the global mobile communication system GSM (Global System for Mobile Communications) network. It is based on the SIM card and uses the GSM network's signaling channel to transmit data. It is a new service launched on the technical basis of the GSM short message system, and its ability to explore businesses is far stronger than the SMS system.

USSD uses character-based codes, supporting up to 182 characters. Users can directly interact through their phones by making selections from various menus. Unlike SMS messages, a real-time connection is created during a USSD session. This means that as long as the communication line remains smooth, USSD can enable two-way information communication. Therefore, queries and answers are almost instantaneous.

USSD maintains a call connection during the session, providing a transparent channel without storage and forwarding. SMS, on the other hand, has no session channel at the physical bearer layer. It is a store and forward system that requires multiple session processes for a user to complete a query. Since USSD is similar to GPRS and maintains a session process during interaction without needing to re-establish the channel for each data transmission, the USSD system responds instantly to user call requests, greatly speeding up response times. The main response delay has shifted to the application server side, with response times much faster than short messages.



> USSD support requires 3 conditions:
> 1. The module must support USSD functions; 
> 2. The SIM card must support it;
> 3. The network must support it and have a USSD server;
>
> Currently, operators in China do not support USSD functions. Please consult local operators in other areas.
>
> Currently, only the EC200UEU_AA and EG915ULA_AB_VOLTE modules support USSD functions.



**Example**

```python
>>> import ussd  

>>> def ussdCallback(args):
...   print(args)
...
>>> ussd.setCallback(ussdCallback)   # Register callback function  

>>> ussd.send("*101#")    # Send USSD command *101# to network
0
>>> 
(4, '', 0, 4)  # Callback receives network reply content  

>>> ussd.getSessionState()   #Query if in continuous session state
False
```

## Callback Registration Functionality  

### `ussd.setCallback`

```  
ussd.setCallback(fun)
```

Registers a callback function. Receives USSD responses to network-initiated or network-originating operations in the callback function. The callback will be triggered whenever a USSD message is received from the network side.  

**Parameter**  

* `fun` - Callback function name. Callback function format and callback function parameter description are as follows:  

```python
def ussdCallback(args):
	pass
```

| Parameter | Type    | Description                                                  |
| --------- | ------- | ------------------------------------------------------------ |
| `args[0]` | Integer | Network response status.<br>`0` Response succeeded, returns network reply string content, see `args[1]`;<br>`1` Response succeeded, end session;<br>`2`,`3` Response succeeded, no reply content;<br>`4` Local error/network rejection/network error return. |
| `args[1]` | String  | Received network-side response USSD message content. Only a normal string when `args[0]` is 0, otherwise empty string; |
| `args[2]` | Integer | Network-issued USSD DCS. DCS: Cell broadcast data coding scheme. |
| `args[4]` | Integer | Exception status error code.                                 |

**Example**  

```python  
import ussd

def ussdCallback(args):
    resp_type = args[0]  
    ussd_str = args[1]
    dcs = args[2]
    err_code = args[3]
    if resp_type == 0:
        print('### USSD Network successful return:',ussd_str) 
    elif resp_type == 1:
        print('### USSD end of session.'
    elif resp_type in [2,3]: 
        print('### USSD Network successful return None')
    else:
        print('### USSD Network return error,err_code:',err_code)
ussd.setCallback(ussdCallback)
```



## Sending USSD Commands  

### `ussd.send`  

```  
ussd.send(reqstr) 
```

This method is used to send USSD commands to the network with the default DCS equal to 15. The network reply result is displayed directly in the callback function.  

**Parameter**  

* `reqstr`- USSD sent to the network, string type, cannot be empty, range of 1~159 bytes.  

**Return Value**

Returns an integer value, `0` indicates send success, `-1` indicates send failure.  

**Example**

```python  
>>> ussd
>>> ussd.send("*101#")    # Send USSD command to network.  
0
```



## Querying Session Status  

### `ussd.getSessionState`  

```
ussd.getSessionState()  
```

This method is used to query the USSD session status. When in continuous session state, further user operations are needed.

**Return Value**  

  Returns Boolean type, `False` non-continuous session state, no further user action needed (network-initiated USSD notification, or no further information needed after device-initiated operation);  

`True` continuous session state, further user action needed (network-initiated USSD request or further information needed after device-initiated operation).  



> When `ussd.getSessionState()` returns `True`, the network side will keep the interactive session channel with the user open, requiring further user action. There are two ways to take action:  
>
> 1. Continue to use `ussd.send` to send USSD commands to interact with the network side;  
> 2. Use `ussd.stopSession` to end the continuous session state;  

**Example**  

```python 
>>> import ussd
>>> ussd.getSessionState()
False
```



## Ending Continuous Session State  

### ``ussd.stopSession``  

```  
ussd.stopSession()
```

This method is used to end the continuous session state and releases the USSD session channel preserved by the network side. This will trigger the USSD callback function to end the session with `args[0]` equal to 1.  

**Return Value**

  Returns an integer value, `0` indicates successfully ending the session, `-1` indicates failure or no need to end session state.  

```python
>>> import ussd  
>>> ussd.getSessionState()  
True
>>> ussd.stopSession()
0
```



## DCS Configuration and Query  

### `ussd.getDcs`  

```  
ussd.getDcs()
```

This method is used to query the data coding scheme for sending USSD commands.  

**Return Value**  

  Returns the data coding scheme DCS for sending USSD commands, integer, range 1~17, 68, 72, 240.  



>DCS: 3GPP TS 23.038 Cell Broadcast Data Coding Scheme. Default: 15, generally does not need to be set.  



**Example**  

```python
>>> import ussd
>>> ussd.getDcs() 
15
```



### `ussd.setDcs`  

```  
ussd.getDcs(Dcs)  
```

This method is used to set the DCS for sending USSD commands.  

**Parameter**  

* `Dcs`- Cell broadcast data coding scheme, integer, range `0~17, 68, 72, 240`; 0 indicates restoring default value.   

**Return Value**  

  Returns an integer value, `0` indicates success, `-1` indicates failure.  

**Example**  

```python  
>>> import ussd
>>> ussd.getDcs()
15
>>> import ussd
>>> ussd.setDcs(1) 
0
>>> ussd.getDcs()
1
```