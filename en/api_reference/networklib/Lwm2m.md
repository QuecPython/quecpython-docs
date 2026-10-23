# LWM2M - Lightweight M2M (Machine to Machine)
## Create Object
The lightweight IoT protocol provides functionality for managing devices on a cloud platform, such as checking device online status, monitoring current signal conditions, restarting devices, and more.

>The following models support this module function. For customized functions,please refer to the actual features:
>
>EC200UEU_AB_SANX/BG95M8_SANX/EC200AAU_HA_SANX/EG912NEN_AA.

### `lwm2m`

```python
lw=lwm2m()
```

Create LwM2M object.

**Parameter Description:**

No parameters.

**Return Value Description:**

Returns the lwm2m object.

## Initialize Object

### `lwm2m.init`

```python
lwm2m.init()
```

Initialize the object.

**Parameter Description:**

No parameters.

**Return Value Description:**

Returns an integer value of 0 for success, and an integer value of -1 for failure.

## Configure Object

### `lwm2m.config`

```python
lwm2m.config(config_type,config_list)
```

Configure functional module parameters.

**Parameter Description:**

* `config_type` -  Configuration type, an enumerated value under the lwm2m object, see below.

| Configuration Type | Corresponding config_list Parameter                                 | Remarks                                |
| ------------------ | ------------------------------------------------------------------- | -------------------------------------- |
| lwm2m.Epnamemode   | [mode]                                                              |                                        |
| lwm2m.Epname       | [epname]                                                            |                                        |
| lwm2m.Reset        | []                                                                  | Empty list                             |
| lwm2m.Security     | When security_mode = 0: [serverID,SSID,server_addr,bootstrap,security_mode,psk_id,psk_key] <br/> When security_mode = 2: [serverID,SSID,server_addr,bootstrap,security_mode,ca_cert,client_cert,client_key] <br/> When security_mode = 3: [serverID,SSID,server_addr,bootstrap,security_mode] | Different security modes have different config_list elements |
| lwm2m.Server       | [serverID,life_time,pmin,pmax,disable_timeout,storing,binding_mode] |                                        |
| lwm2m.Urc          | [URC_onoff]                                                         | Must be enabled to capture URC         |
| lwm2m.Fota         | [download,update]                                                   | To capture URC, configure both as manual (not automatic) |

* `config_list` -  List of configuration parameters, list data type, list element values are explained in below.

| Field Name      | Data Type | Field Description                                                                                                                                                                                                                 |
| --------------- | --------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| serverID        | Integer   | 0 - Bootstrap server <br/>1 - DM server <br/>                                                                                                                                                                                      |
| SSID            | Integer   | 100 - Bootstrap server <br/>1000 - Diagnostics server <br/>                                                                                                                                                                        |
| server_addr     | String    | Server address in the format "address:port", maximum length 256.                                                                                                                                                                  |
| bootstrap       | Integer   | 0 - Do not use bootstrap (only when serverID is not 0)<br/>1 - Use bootstrap (only when serverID is 0)                                                                                                                             |
| security_mode   | Integer   | Encryption method: <br/>0 - Pre-shared key mode <br/>2 - Certificate mode <br/>3 - No security mode                                                                                                                                |
| psk_id          | String    | Used when security_mode is 0                                                                                                                                                                                                       |
| psk_key         | String    | Used when security_mode is 0                                                                                                                                                                                                       |
| ca_cert         | String    | Used when security_mode is 2, CA certificate (for verifying the server certificate chain)                                                                                                                                          |
| client_cert     | String    | Used when security_mode is 2, client certificate                                                                                                                                                                                   |
| client_key      | String    | Used when security_mode is 2, client private key                                                                                                                                                                                   |
| life_time       | Integer   | Lifetime, range 1-86400, default is 86400, in seconds.                                                                                                                                                                             |
| pmin            | Integer   | Default pmin period, range 1-86400, default is 86400, in seconds.                                                                                                                                                                 |
| pmax            | Integer   | Default pmax period, range 1-86400, default is 86400, in seconds.                                                                                                                                                                 |
| disable_timeout | Integer   | Time interval before the next connection after disconnection from lwm2m server. Range 1-86400, default is 86400, in seconds.<br/>In actual testing, the connection can be started immediately after disconnection; this parameter is sent to the server but is not enforced by it. |
| storing         | Integer   | Whether to save server information: <br/>0 - Do not save <br/>1 - Save                                                                                                                                                            |
| binding_mode    | String    | Connection method to the server, currently only supporting UDP-based connection <br/>"U" - UDP mode <br/>"UQ" - UDP with queue mode <br/>"S" - SMS mode <br/>"SQ" - SMS with queue mode <br/>"US" - UDP and SMS mode <br/>"UQS" - UDP and SMS with queue mode |
| mode            | Integer   | Terminal device name format: <br/>1 - Use the custom name configured via lwm2m.Epname <br/>3 - Use psk_id as the Endpoint Name (usually used with PSK mode)                                                                         |
| epname          | String    | Endpoint Name of the terminal device                                                                                                                                                                                               |
| download        | Integer   | Download mode: <br/>0 - Manual download <br/>1 - Automatic download                                                                                                                                                                |
| update          | Integer   | Update mode: <br/>0 - Manual update <br/>1 - Automatic update                                                                                                                                                                      |
| URC_onoff       | Integer   | Enable/Disable URC reporting for lwm2m: <br/>0 - Disable URC reporting <br/>1 - Enable URC reporting                                                                                                                               |

**Return Value Description:**

Returns an integer value of 0 for success, an integer value of -1 for failure, and an integer value of -2 for unsupported operations.

## Register

### `lwm2m.register`

```python
lwm2m.register()
```

Send a login request to the LwM2M server. The result of the login, whether successful or failed, will be sent through a callback function with the login success message.

**Parameter Description:**

No parameters.

**Return Value Description:**

Returns 0 for success, -1 for failure. The registration result requires calling the query interface to check.

## Unregister

### `lwm2m.unregister`

```python
lwm2m.unregister()
```

Send a logout request to the LwM2M server. The result, whether successful or failed, will be notified through a callback function.

**Parameter Description:**

No parameters.

**Return Value Description:**

Returns 0 for success, -1 for failure. The unregister result requires calling the query interface to check.

## Query Registration Status

### `lwm2m.stat`

```python
lwm2m.stat()
```

Query the current login status.

**Parameter Description:**

No parameters.

**Return Value Description:**

Returns an integer value of 0 for 'Not Registered' status, 1 for 'Registering' status, 2 for 'Registered' status, 3 for 'Deregistering' status, and 4 for 'Login Failed' status.

## Send Heartbeat

### `lwm2m.update`

```python
lwm2m.update(ssid)
```

Send a heartbeat packet to the LwM2M server. There is an automatic heartbeat process internally, and it is not recommended to call this interface manually.

**Parameter Description:**

* `ssid` -  Configured ssid or the ssid issued during BS login in the bootstrap stage, integer value.

**Return Value Description:**

Returns 0 for success, -1 for failure. The result of the update is notified through the callback function.

## Register Callback Function

### `lwm2m.register_call`

```python
lwm2m.register_call(usrfun)
```

Register a callback function. This callback function will notify the upper-layer application of the login result and messages sent by the server.

**Parameter Description:**

* `usrfun` -  The callback function to be set, which is called when the server responds.

**Callback Function Parameters**

* `list` -  The callback function notification data, list type, described in detail below:

| Element | Data Type | Description                                                                                             |
| ------- | --------- | ------------------------------------------------------------------------------------------------------- |
| list[0] | Integer   | Event type (can be ignored)                                                                             |
| list[1] | Integer   | Event code (can be ignored)                                                                             |
| list[2] | String    | Captured URC string data (specific event description can be found in [Event-Description](#Event-Description)), used to determine the connection event of Lwm2m. |

**Return Value Description:**

Returns 0 for success, -1 for failure. To remove an already registered callback function, pass in None.


## Event-Description

The event types may vary slightly across platforms, for example, some platforms do not have APN, which does not affect functionality or usage.

| Event Type                                                     | Description                                                                                                     | Remarks                                                                                                                                                    |
| -------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------- |
| +QLWURC: "pdp active",result,APN                               | Result of PDP activation. Before sending the registration request to LwM2M, the PDP server should be activated. | result: string type<br/>"successfully" <br/>"failed"<br />APN: string type<br />APN value                                                                  |
| +QLWURC: "initial",result,SSID                                 | Initialization result when the client connects to the lwm2m server.                                             | result: string type<br/>"successfully" <br/>"failed"<br />SSID: integer type<br />0 - All servers <br />Other values - Specific servers                    |
| +QLWURC: "dtls",result,SSID                                    | DTLS handshake result when using encryption.                                                                    | Same as above                                                                                                                                              |
| +QLWURC: "bootstraping"                                        | Reports this URC when the bootstrap process is active.                                                          |                                                                                                                                                            |
| +QLWURC: "bootstrap",result,SSID                               | Result of the Bootstrap process.                                                                                | Same as above                                                                                                                                              |
| +QLWURC: "registering"                                         | Reports this URC when the client is registering with the lwm2m server.                                          | Same as above                                                                                                                                              |
| +QLWURC: "ready",result,SSID                                   | Result of registration request sent to the lwm2m server.                                                        | Same as above                                                                                                                                              |
| +QLWURC: "update",result,SSID                                  | Result of update request sent to the lwm2m server.                                                              | Same as above                                                                                                                                              |
| +QLWURC: "deregister",SSID,code<br/>+QLWURC: "deregister",code | Result of the deregistration request sent to the lwm2m server.                                                  | SSID: integer type<br />0 - All servers <br />Other values - Specific servers<br />Code: integer<br />0 - Success <br />1 - Failure<br />3 - Unknown Error |
| +QLWURC: "fota/pkgurl",url                                     | The URL address sent by the lwm2m server.                                                                                    |                                                                                                                                                            |
| +QLWURC: "apn_changed",APN                                     | The APN sent by the lwm2m server.                                                                                      |                                                                                                                                                            |
| +QLWURC: "user_name_changed",user                              | The user name sent by the lwm2m server.                                                                                   |                                                                                                                                                            |
| +QLWURC: "secret_changed",password                             | The password sent by the lwm2m server.                                                                                |                                                                                                                                                            |
| +QLWURC: "authentication_type_changed",<auth>                  | The authentication type sent by the lwm2m server.                                                                        |                                                                                                                                                            |
| +QLWURC: "lifetime_changed",lifetime                           | The lifetime (heartbeat period) sent by the lwm2m server.                                                                                  |                                                                                                                                                            |
| +QLWURC: "current_time_changed",data_time                      | The timestamp sent by the lwm2m server.                                                                   |                                                                                                                                                            |

---

## Get example
*[Click to view example code.](https://github.com/QuecPython/examples/tree/main/network-comm/net-protocols/lwm2m/example_lwm2m.py)*
