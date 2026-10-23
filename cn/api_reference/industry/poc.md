# poc - 对讲服务

该库对接的是伯纳德对讲平台，并且只能使用支持 PoC 功能的固件。

支持 PoC 功能的模组型号如下：

| 系列   | 型号                                                         |
| :----- | :----------------------------------------------------------- |
| EC600M | EC600MCN_LA、EC600MCN_LE、EC600MCN_LF、EC600MEU_LA、EC600MLA_LA |
| EC800M | EC800MCN_LA、EC800MCN_LE                                     |
| EC600U | EC600UEU_AB                                                  |
| EC800G | EC800GCN_LD                                                  |

> 💡 **Tips**
> - 非商业测试，请联系移远销售人员（ li.bao@quectel.com ），申请伯纳德**芯平台**测试账号。
> - 商业应用请走商务流程。

访问 [github 仓库](https://github.com/QuecPython/solution-POC/tree/EC600MCNLE-Demo)获取[示例代码](https://github.com/QuecPython/solution-POC/tree/EC600MCNLE-Demo/code)、[使用说明](https://github.com/QuecPython/solution-POC/blob/EC600MCNLE-Demo/README.zh.md)和[适用的固件](https://github.com/QuecPython/solution-POC/releases/)。


## 导入 `poc` 模块

**代码示例：**

```python
import poc
```

## 初始化

### `poc.init(init_cb)`

该方法用于初始化 poc。

**参数描述：**

- `init_cb` - 初始化回调函数，fun类型。用于通知用户 PoC 是否初始化成功。
  函数原型：`init_cb(msg)`。
  - `msg` 表示初始化成功与否的结果。
    - `1` - 初始化成功；
    - `0` - 初始化失败。

**代码示例：**

```python
def init_cb(msg):
    if msg:
        print("poc init successfully.")
    else:
        print("poc init failed.")

poc.init(init_cb)
```

### `poc.get_init_status()`

该方法用于获取初始化状态。

**返回值描述：**

- `1` - 已初始化；
- `0` - 未初始化。

## 参数设置

### `poc.set_solution(solution)`

该方法用于设置方案名。

**参数描述：**

- `solution` - 方案名，str 类型。
  方案名用来记录本次方案的具体名称，用户可自行定义。

**返回值描述：**

- `0` - 成功；
- 其它 - 失败。

### `poc.set_solution_version(version)`

该方法用于设置方案版本号。

**参数描述：**

- `version` - 方案版本号，str 类型。
  方案版本号用来记录本次方案的版本号，用户可自行定义。

**返回值描述：**

- `0`- 成功；
- 其它 - 失败。

### `poc.set_productInfo(productInfo)`

该方法用于设置产品信息。

**参数描述：**

- `productInfo` - 产品信息，str 类型。
  产品信息用来记录对讲产品的具体信息，用户可自行定义。

**返回值描述：**

- `0`- 成功；
- 其它 - 失败。

### `poc.set_manufacturer(manufacturer)`

该方法用于设置制造商信息。

**参数描述：**

- `manufacturer` - 制造商信息，str 类型。
  制造商信息用来记录对讲方案的制造商信息，用户可自行定义。

**返回值描述：**

- `0` - 成功；
- 其它 - 失败。

### `poc.set_notify_mode(value)`

该方法用于设置通知模式。

**参数描述：**

- `value` - 设置的通知模式，int 类型。
  - `0` - VOICE；
  - `1` - TTS。

**返回值描述：**

- `0` - 成功；
- `-1` - 失败。

### `poc.set_ptt_status(status)`

该方法用于设置 PTT 状态。

**参数描述：**

- `status` - 设置 ppt 的状态值，int 类型。

**返回值描述：**

- `0` - 成功；
- `-1` - 失败。

### `poc.speak_mode(mode)`

该方法用于设置/查询语音处理模式。

#### 设置模式

**参数描述：**

- `mode` - 需要设置的语音处理模式，int 类型。
  - `0` - 根据用户处理；
  - `1` - 根据回调处理。

**返回值描述：**

- `0` - 成功；
- `-1` - 失败。

#### 查询模式

**参数描述：**

无参数。

**返回值描述：**

当前设置的语音处理模式。
- `0` - 根据用户处理；
- `1` - 根据回调处理。

### `poc.log(value)`

该方法用于打开/关闭 log。

**参数描述：**

- `value` - 设置 log 的模式，int 类型。
  - `0` - 关闭 log；
  - `1` - 打开 log。

**返回值描述：**

无。

### `poc.change_logininfo(account, pwd)`

该方法用于更新登录信息。

**参数描述：**

- `account` - 用户登录账户，str 类型；
- `pwd` - 用户登录密码，str 类型。

**返回值描述：**

- `0` - 成功；
- `-1` - 失败。

## 参数查询

### `poc.member_getbyid(uid)`

该方法用于查询用户信息。

**参数描述：**

- `uid` - 被查询用户的 id，int 类型。
  若为`0`，则查询当前用户信息。

**返回值描述：**

列表：`[uid, user_name, online_sta, index]`
- `uid` - 被查询用户的 id，int 类型；
- `user_name` - 被查询用户的名称，str 类型；
- `online_sta` - 被查询用户的在线状态， int 类型；
  - `1` - 在线；
  - `2` - 离线；
  - `3` - 在线在组。
- `index` - 被查询用户的索引号，int 类型。

### `poc.get_loginstate()`

该方法用于查询当前用户在线状态。

**返回值描述：**

- `1` - 在线；
- `2` - 离线；
- `3` - 在线在组。

### `poc.get_logininfo()`

该方法用于获取登录信息。

**返回值描述：**

元组：`(account, pwd)`
- `account` - 用户登录账户，str 类型；
- `pwd` - 用户登录密码，str 类型。

### `poc.get_version()`

该方法用于获取对讲平台的版本信息。

**返回值描述：**

获取到的版本信息，str 类型。

### `poc.ptt_cfg()`

该方法用于获取用户用来呼叫的 PTT 按键信息。

**返回值描述：**

列表：`[key_type, gpio_num, level]`
- `key_type` - PTT 按键对应的按键类型，int 类型；
  - `1` - 普通 GPIO 按键；
  - `2` - 键盘按键。
- `gpio_num` - PTT 按键对应的 GPIO 引脚号，int 类型；
- `level` - PTT 按键对应引脚号的电平状态，int 类型。
  - `0` - 低电平；
  - `1` - 高电平。

### `poc.current_zone_time(zone)`

该方法用于获取某个时区的当前时间。

**参数描述：**

- `zone` - 需要获取时间的时区，int 类型。

**返回值描述：**

元组：`(year, month, day, hour, minute, second, millisecond, zone)`
- `year` - 年；
- `month` - 月；
- `day` - 日；
- `hour` - 时；
- `minute` - 分；
- `second` - 秒；
- `millisecond` - 毫秒；
- `zone` - 查询时间的时区。

### `poc.send_ping()`

该方法用于发送 ping 请求。

**返回值描述：**

- `0` - 成功；
- 其他 - 失败。

## 登录对讲平台

### `poc.login(login_cb)`

该方法用于登录对讲平台。

**参数描述：**

- `login_cb` - 登录状态回调函数，fun 类型。用于通知用户对讲平台是否登录成功。
  函数原型：`login_cb(msg)`。
  - `msg` 表示初始化成功与否的结果。
    - `1` - 已登录；
    - `0` - 未登录。

**返回值描述：**

- `0` - 成功；
- 小于`0` - 失败。

## 退出对讲平台

### `poc.logout()`

该方法用于退出对讲平台。

**返回值描述：**

- `0` - 成功；
- 其它 - 失败。

## 呼叫

### `poc.speak(ctrl)`

该方法用于呼叫组内用户。

**参数描述：**

- `ctrl` - 控制组内呼叫行为。
  - `1` - 呼叫；
  - `0` - 退出呼叫。

**返回值描述：**

- `0` - 成功；
- 其它 - 失败。

### `poc.calluser(uid, cb)`

该方法用于呼叫指定用户。

**参数描述：**

- `uid` - 对方用户 id，int 类型。
- `cb` - 单呼回调，fun 类型。
  函数原型：`cb(msg)`。
  - `msg` 表示呼叫指定用户是否成功。
    - `1` - 呼叫成功；
    - `0` - 呼叫失败。

**返回值描述：**

- `0` - 成功；
- 其它 - 失败。

## 群组管理

### `poc.jiongroup(gid)`

该方法用于设备进入群组。

**参数描述：**

- `gid` - 群组 id，int 类型。

**返回值描述：**

- `0` - 成功；
- 其它 - 失败。

### `poc.leavegroup()`

该方法用于设备离开当前群组。

**返回值描述：**

- `0` - 成功；
- 其它 - 失败。

### `poc.get_groupcount()`

该方法用于获取群组数。

**返回值描述：**

群组个数，int 类型。

### `poc.get_grouplist(index_begin, count)`

该方法用于查询群组信息。

**参数描述：**

- `index_begin` - 查询的索引起始值，int 类型；
- `count` - 计划查询的个数，int 类型。

**返回值描述：**

- `-1` - 失败；
- 元组 - 成功，定义为：`(gid, group_name, group_type, index)`
  - `gid` - 被查询的群组 id，int 类型；
  - `group_name` - 被查询的群组名，str 类型；
  - `group_type` - 被查询的群组类型，str 类型；
  - `index` - 被查询的群组索引号，int 类型。

### `poc.group_getbyindex(index)`

该方法用于按索引查询群组信息。

**参数描述：**

- `index` - 被查询群组的索引号，int 类型。

**返回值描述：**

列表：`[gid, group_name, group_type, index]`
- `gid` - 被查询的群组 id，int 类型；
- `group_name` - 被查询的群组名，str 类型；
- `group_type` - 被查询的群组类型，str 类型；
- `index` - 被查询的群组索引号，int 类型。

### `poc.group_getbyid(gid)`

该方法用于按组 id 查询群组信息。

**参数描述：**

- `gid` - 群组 id，int 类型。
  若为 `0`，则查询当前群组信息。

**返回值描述：**

列表：`[gid, group_name, group_type, index]`
- `gid` - 被查询的群组 id，int 类型；
- `group_name` - 被查询的群组名，str 类型；
- `group_type` - 被查询的群组类型，str 类型；
- `index` - 被查询的群组索引号，int 类型。

### `poc.get_membercount(gid)`

该方法用于查询群组内成员个数。

**参数描述：**

- `gid` - 群组 id，int 类型。

**返回值描述：**

成员个数，int 类型。

### `poc.get_memberlist(gid, index_begin, count)`

该方法用于获取群组内成员列表。

**参数描述：**

- `gid` - 群组 id，int 类型；
- `index_begin` - 查询的索引起始值，int 类型；
- `count` - 计划查询的个数，int 类型。

**返回值描述：**

- `-1`- 失败；
- 元组 - 成功，定义为：`(uid, user_name, online_sta, priority, index)`
  - `uid` - 组内用户的 id，int 类型；
  - `user_name` - 组内用户的名称，str 类型；
  - `online_sta` - 组内用户的在线状态， int 类型。
    - `1` - 在线;
    - `2` - 离线;
    - `3` - 在线在组。
  - `priority` - 组内用户呼叫的优先级，int 类型。
  - `index` - 用户索引号，int 类型。

## 音频播放管理

### `poc.get_audiostate()`

该方法用于获取当前音频状态。

**返回值描述：**

- `0` - 空闲；
- `1` - 开始通话；
- `2` - 通话中；
- `3` - 停止通话；
- `4` - 开始接听；
- `5` - 接听中；
- `6` - 停止接听；
- `7` - 开始播放tts；
- `8` - 停止播放tts；
- `9` - 开始播放提示音；
- `10` - 停止播放提示音；
- `11` - 开始录音；
- `12` - 结束录音。

### `poc.set_tts_enable(enable)`

该方法用于使能 tts。

**参数描述：**

- `enable` - 是否使能 tts，int 类型。
  - `1`：使能；
  - `0`：不使能。

**返回值描述：**

- `0` - 成功；
- `-1` - 失败。

### `poc.play_tts(tts_str, interrupt)`

该方法用于播放 tts。

**参数描述：**

- `tts_str` - tts 播放的内容，str 类型。
- `interrupt` - 是否可以被打断，int 类型。
  - `1`：表示中断当前 tts 播放；
  - `0`：表示不中断当前 tts 播放。

**返回值描述：**

- `0` - 成功；
- 其它 - 失败。

### `poc.set_vol(type, vol)`

该方法用于设置音量。

**参数描述：**

- `type` - 设置音量的类型，int 类型。
  - 0 - VOICE；
  - 1 - TTS；
  - 2-  TONE。
- `vol` - 音量大小，int 类型。

**返回值描述：**

- `0` - 成功；
- `-1` - 失败。

### `poc.get_vol(type)`

该方法用于获取音量。

**参数描述：**

- `type` - 获取音量的类型，int 类型。
  - 0 - VOICE；
  - 1 - TTS；
  - 2-  TONE。

**返回值描述：**

音量的大小，int 类型。

## 回调注册

### `poc.register_join_group_cb(cb)`

该方法用于注册入组回调。

**参数描述：**

- `cb` - 进组回调，触发源包括主动进组和被动进组，fun 类型。用于通知用户设备是否成功进入群组。
  函数原型：`cb(msg)`。
  - `msg`为一个列表：`[group_name, gid]`。
    - `group_name` - 当前群组的名字；
    - `gid` - 当前群组的 id。

### `poc.register_audio_cb(poc_audio_cb)`

该方法用于注册音频回调。

**参数描述：**

- `poc_audio_cb` - 音频回调，fun 类型。用于通知用户当前的音频信息。
  函数原型：`poc_audio_cb(msg)`。
  - `msg`为一个列表：`[state, uid, name, flag]`。
    - `state` - 当前语音状态：收听/讲话/TTS/TONE；
    - `uid` - 当前操作用户 id，讲话/TTS/TONE时为0；
    - `name` - 当前操作用户名字，讲话/TTS/TONE时为NULL；
    - `flag` - 如果 state 为 BND_LISTEN_START，flag 为 `1` 表示本机可以打断对方讲话，flag 为 `0` 表示本机不能打断对方讲话。

### `poc.register_listupdate_cb(poc_listupdate_cb)`

该方法注册数据更新回调。

**参数描述：**

- `poc_listupdate_cb` - 数据变化的回调函数，fun 类型。用于通知用户当前数据变化的信息。
  函数原型：`poc_listupdate_cb(msg)`。
  - `msg`表示列表变化的对象。
    - `1` - 群组列表变化；
    - `2` - 成员列表变化。

### `poc.register_upgrade_cb(cb)`

该方法用于注册是否升级的回调。

**参数描述：**

- `cb` - 升级回调函数，fun 类型。用于通知用户是否需要升级。
  函数原型：`cb(msg)`。
  - `msg`表示是否需要升级。
    - `1` - 需要升级；
    - `0` - 不需要升级。

### `poc.register_cell_location_change_cb(cb)`

该方法用于注册位置信息变化回调。

**参数描述：**

- `cb` - 位置信息变化回调，fun 类型。用于通知用户位置信息发生变化。
  函数原型：`cb(msg)`。
  - `msg`为一个列表：`[location_switch, report_rate]`。
    - `location_switch` - 位置开关的状态，int 类型；
    - `report_rate` - 报告频率，int 类型。

### `poc.register_error_cb(cb)`

该方法用于注册异常错误信息的回调。

**参数描述：**

- `cb` - 异常错误信息回调，fun 类型。用于通知用户发生异常错误。
  函数原型：`cb(info)`。
  - `info`为异常错误信息。

### `poc.register_recv_report_sos_ack_cb(cb)`

该方法用于注册 sos 确认回调。

**参数描述：**

- `cb` - sos 确认回调，fun 类型。用于通知用户 sos 请求的确认结果。
  函数原型：`cb(result)`。
  - `result`为确认结果。
    - `0` - 成功；
    - 其它 - 失败。

### `poc.register_recv_report_sos_ack_timeout_cb(cb)`

该方法用于注册 sos 超时回调。

**参数描述：**

- `cb` - sos 超时回调，fun 类型。用于通知用户 sos 请求回复超时。
  函数原型：`cb(msg)`。
  - `msg`为超时信息。

### `poc.register_recv_sos_alarm_cb(cb)`

该方法用于注册 sos 报警回调。

**参数描述：**

- `cb` - sos 报警回调，fun 类型。用于通知用户某一用户正在发送 sos 请求。
  函数原型：`cb(msg)`。
  - `msg`为一个列表：`[uid, uuid, lon, lat, gid]`。
    - `uid` - 用户 id，即请求者 id，int 类型；
    - `uuid` - sos 唯一标识，int 类型；
    - `lon` - 经度，double 类型；
    - `lat` - 维度，double 类型；
    - `gid` - 群组 id，即请求者当前在哪个群组，int 类型。

### `poc.register_recv_sos_info_cb(cb)`

该方法用于注册 sos 请求信息回调。

**参数描述：**

- `cb` - sos 请求信息回调，fun 类型。用于通知用户发送的 sos 请求信息。
  函数原型：`cb(msg)`。
  - `msg`为一个列表：`[name, timestamp, lon, lat]`。
    - `name` - 请求者名称，str 类型；
    - `timestamp` - 请求时间，时间戳形式，str 类型；
    - `lon` - 经度，double 类型；
    - `lat` - 维度，double 类型。

### `poc.register_request_invite_ack_cb(cb)`

该方法用于注册邀请请求的回调。

**参数描述：**

- `cb` - 邀请请求回调，fun 类型。用于通知用户邀请请求的结果。
  函数原型：`cb(msg)`。
  - `msg`为一个列表：`[invite_id, result, invites, invites_count, reason]`。
    - `invite_id` - 邀请 id，用于唯一标识一个邀请请求，int类型；
    - `result` - 邀请结果，int 类型；
      - `0` - 正常拒绝；
      - `1` - 接受邀请；
      - `2` - 免打扰。
    - `invites` - 一个包含多个邀请 id 的数组；
    - `invites_count` - invites 数组中的元素个数，int 类型；
    - `reason` - 对方拒绝原因，int 类型。
      - `2` - 对方不支持；
      - `1` - 对方正在讲话，或者对方不允许被呼叫等错误。

### `poc.register_sos_status_cb(cb)`

该方法用于注册 sos 状态变化回调。

**参数描述：**

- `cb` - sos 状态变化回调，fun 类型。用于通知用户 sos 的状态。
  函数原型：`cb(state)`。
  - `state`为 sos 的状态。
    - `0` - 成功；
    - `1` - 错误；
    - `2` - 超时；
    - `3` - 没有启用 sos 请求。

### `poc.register_recv_broadcast_info_cb(cb)`

该方法用于注册广播播报信息回调。

**参数描述：**

- `cb` - 广播信息回调，fun 类型。用于播报 TTS 信息。
  函数原型：`cb(msg)`。
  - `msg`为一个列表：`[flag, tts]`。
    - `flag` - 控制广播播报，int类型；
      - `1` - 开始播放广播；
      - `0` - 停止播放广播。
    - `tts` - 播报的 tts 信息，str类型。

### `poc.register_recv_enclosure_alarm_cb(cb)`

该方法用于注册接收警报回调。

**参数描述：**

- `cb` - 接收警报回调，fun 类型。用于通知用户警报信息。
  函数原型：`cb(msg)`。
  - `msg`为一个列表：`[alarm, tips]`。
    - `alarm` - 警报类型，int 类型；
      - `1` - 越界；
      - `0` - 消除。
    - `tips` - 警报提示信息，str 类型。

## sos 功能 & 位置信息

### `poc.sos_start(lon, lat)`

该方法用于启动 sos 功能，适用于国内伯纳德平台。

**参数描述：**

- `lon` - 经度，double 类型；
- `lat` - 纬度，double 类型。

**返回值描述：**

- `0` - 成功；
- `-1` - 失败。

### `poc.report_sos(lon, lat, time, alt, speed, dirang)`

该方法用于发送 sos，适用于海外伯纳德平台和国内、外管理平台。

**参数描述：**

- `lon` - 经度，double 类型；
- `lat` - 纬度，double 类型;
- `time` - 时间，str 类型；
- `alt` - 海拔，double 类型；
- `speed` - 速度，double 类型；
- `dirang`- 方向，double 类型。

**返回值描述：**

- `0` - 成功；
- `-1` - 内部错误；
- `-2` - 发送 sos 频繁。

### `poc.query_sos_map(w, h, lon, lat)`

该方法用于查询 sos 地图。

**参数描述：**

- `w` - 宽度，int 类型；
- `h` - 高度，int 类型；
- `lon` - 经度，double 类型；
- `lat` - 纬度，double 类型。

**返回值描述：**

- `0` - 成功；
- `-1` - 失败。

### `poc.send_gpsinfo(lon, lat, time, alt, speed, dirang)`

该方法用于发送定位信息。

**参数描述：**

- `lon` - 经度，double 类型；
- `lat` - 纬度，double 类型;
- `time` - 定位时间，包括（年、月、日、时、分、秒、毫秒），str 类型；
- `alt` - 海拔，double 类型；
- `speed` - 速度，double 类型；
- `dirang`- 方向，double 类型。

**返回值描述：**

- `0` - 成功；
- `-1` - 失败。

### `poc.request_lbs_info(w, h, lon, lat)`

该方法用于请求位置信息。

**参数描述：**

- `w` - 宽度，int 类型；
- `h` - 高度，int 类型；
- `lon` - 经度，double 类型；
- `lat` - 纬度，double 类型。

**返回值描述：**

- `0` - 成功；
- `-1` - 失败。