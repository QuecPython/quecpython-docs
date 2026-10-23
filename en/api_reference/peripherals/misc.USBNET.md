# USBNET - USB Network Card

Module feature: USB network card.

> Note: EC600S, EC600N, EC800N, EC200U, EC600U and EC600M series modules support this feature.

## Set Work Type of USB Network Card 

### `USBNET.set_worktype`

```python
USBNET.set_worktype(type)
```

**Parameter:**

- `type` - USBNET working type. Integer type. Type_ECM: ECM mode. Type_RNDIS: RNDIS mode. 

**Return Value:**

`0` - Successful execution

`-1` - Failed execution

> Note: It takes effect after the module is rebooted.

## Get Work Type of USB Network Card 

### `USBNET.get_worktype`

```python
USBNET.get_worktype()
```

**Return Value:**

If successful, it returns current work type of USBNET. If failed, it returns integer type `-1`; `1` indicates ECM mode. `3 ` indicates RNDIS mode.

## Get USENET Current State

### `USBNET.get_status`

```python
USBNET.get_status()
```

**Return Value:**

If successful, it returns the USBNET current state. If failed, it returns the integer  `-1`; `0` indicates no  connection. `1` indicates successful connection.

## Enable USB Network Card

### `USBNET.open`

```python
USBNET.open()
```

**Return Value:**

`0` - Successful execution

`-1` - Failed execution

## Disable USB Network Card

### `USBNET.close`

```
USBNET.close()
```

**Return Value:**

`0` - Successful execution

`-1` - Failed execution

**Example:**

```python
from misc import USBNET
from misc import Power

#work on ECM mode default
USBNET.open()

USBNET.set_worktype(USBNET.Type_RNDIS)

#reset the module
Power.powerRestart()


#After restart
from misc import USBNET

#work on RNDIS mode
USBNET.open()
```

## Get NAT Enabling Status

### `USBNET.getNat`

```python
USBNET.getNat(simid, pid)
```

Gets NAT enabling status of a specified network card.

> Note:Currently only EC200U/EC600U/EC800G series support.
> Note:If NAT mode is enabled for the EC200U/EC600U series, the pid of the enabled route cannot perform IPv6 dial-up.
> Note:If NAT mode is enabled for the EC800G series, both IPv4 and IPv6 Pids of the enabled pid cannot dial up.

**Parameter:**

- `simid` - Integer type. Range: 0 and 1. Currently only `0` is supported. 
- `pid` - Integer type. PDP index. Range: `1-7`.

**Return Value:**

If successful, it returns NAT enable situation. Integer type: 0 and 1. `0`: The NAT mode is not enabled.  `1`: The NAT mode is enabled.

If failed, it returns integer `-1`.

**Example:**

```python
from misc import USBNET
USBNET.getNat(0, 1)
0
```

## NAT Settings

### `USBNET.setNat`

```python
USBNET.setNat(simid, pid, nat)
```

Sets NAT. After the NAT configuration is successful, restart the module and the configuration takes effect. When usbnet.set_worktype () is called, the nat value changes to 1, and NAT mode is enabled for this pid. Therefore, after USBnet is closed, you can call this interface to disable NAT mode and restore the dialing function of this pid.

> Note:Currently only EC200U/EC600U/EC800G series support.
> Note:If NAT mode is enabled for the EC200U/EC600U series, the pid of the enabled route cannot perform IPv6 dial-up.
> Note:If NAT mode is enabled for the EC800G series, both IPv4 and IPv6 Pids of the enabled pid cannot dial up.

**Parameter:**

- `simid` - Integer type. Range: 0 and 1. Currently only `0` is supported. 
- `pid` - Integer type. PDP index. Range: `1-7`.
- `Nat` - Integer type. Range: 0 and 1. `0`: The NAT mode is not enabled; `1`: The NAT mode is enabled.

**Return Value:**

 `0` indicates successful setting. `-1` indicates setting failure.

**Example:**

```python
USBNET.setNat(0, 1, 0)
0
```

## Constants

### `USBNET.Type_ECM`

ECM mode.

### `USBNET.Type_RNDIS`

RNDIS mode.