# 1. 事件结构文档 v0.0.1

## 1.1. 本次修改目标

- `尽量不删除原有字段`
- `尽量向后兼容`
- `尽量在事件上增加客户需要某些字段`
- `删除无用字段`
- `建议后端在发送事件给前端时，先考虑一下该字段前端是否需要，然后再添加字段。如果是后端的一些记录字段，建议在发送给前端之前可以删除`

## 1.2. 事件层次组成

每个字段建议都有以下三个层次的字段组成。

- `根字段` 每个事件都应该带有的字段
- `类型字段` 同一类型的事件都带有的字段，例如座席状态事件类型，呼叫事件类型
- `独享字段` 该字段属于某一个事件独有的字段

## 1.3. 标识说明
- `ing` 代表正在商讨
  - `发送给前端之前删除无用字段`
  - `挂断事件的建议，参考挂断事件`
- `ed` 代表商讨完毕，可以用来实施

## 1.4. 删除无用字段

`建议后端在发送事件给前端时，能够删除事件上的调试信息字段`

wellClient在内部收到事件后，会马上删除三个字段，因为这三个字段都是后端的一些记录，对前端没什么用。当然最好的解决方式是后端不要把这些字段发送给前端。

这三个字段包括：
- `params`
- `_type`
- `topics`

```
try {
  eventInfo = JSON.parse(event.body)
  delete eventInfo.params
  delete eventInfo._type
  delete eventInfo.topics
} catch (e) {
  console.log(e)
  return
}
```

你可以从挂断事件的原始数据中看到，无用的`params`，`_type`，`topics`等字段`几乎占到整个数据的一半。`
而且这三个字段在每个事件中都有，这就会导致我们`几乎一半的带宽都在发送无用数据`。

```
// 原始事件
{
  "eventName": "connectionCleared",
  "eventSrc": "8004@welljoint.cc",
  "eventTime": "2018.02.28 15:18:08",
  "eventType": "csta",
  "serial": 182663,
  "params": {
    "agent": "5002@welljoint.cc",
    "log": "false"
  },
  "_type": "component.cti.event.ConnectionClearedEvent",
  "topics": [
    "connectionCleared",
    "csta",
    "device:8004@welljoint.cc",
    "extension",
    "crossRefId:10275",
    "CtiWorker_ctiworker-1455077780-29n01",
    "agent:5002@welljoint.cc"
  ],
  "namespace": "welljoint.cc",
  "srcDeviceId": "8004@welljoint.cc",
  "callId": "dd09b01c-5ebe-4205-98c5-0537f12d2df0",
  "deviceId": "515021962027@welljoint.cc",
  "localState": "Idle",
  "connectionId": "8004@welljoint.cc|dd09b01c-5ebe-4205-98c5-0537f12d2df0",
  "cause": "normalClearing",
  "releasingDevice": "515021962027@welljoint.cc"
}
```


# 2. 事件名及其数据结构
## 2.1. 根字段 ing

字段名 | 含义 | 备注
--- | --- | ---
eventName | 事件名 | 
eventTime | 事件时间戳 | 格式：`YYYY.MM.dd HH:MM:SS`
eventType | 事件类型 | agent代表座席状态事件, csta代表呼叫事件
serial | 事件序列号 | 一个整数，一般会递增，可以用来判断有没有事件重复，或者顺序错误

## 2.2. 座席状态事件


### 2.2.1. 座席状态事件通用字段
### 2.2.2. agentLoggedOn 登录
**当前字段**
```
// 已经删除了params，_type，topics三个字段

{
  "eventName": "agentLoggedOn",
  "eventSrc": "52592@cmb.cc",
  "eventTime": "2018.02.27 09:21:29",
  "eventType": "agent",
  "serial": 1931776,
  "namespace": "cmb.cc",
  "srcDeviceId": "811151@cmb.cc",
  "deviceId": "811151@cmb.cc",
  "agentId": "52592@cmb.cc",
  "agentMode": "NotReady",
  "devices": {
    "Voice": "811151@cmb.cc"
  }
}
```

### 2.2.3. agentLoggedOff 登出

**当前字段**
```
// 已经删除了params，_type，topics三个字段
{
  "eventName": "agentLoggedOff",
  "eventSrc": "51448@cmb.cc",
  "eventTime": "2018.02.27 09:21:36",
  "eventType": "agent",
  "serial": 1932017,
  "namespace": "cmb.cc",
  "srcDeviceId": "812002@cmb.cc",
  "deviceId": "812002@cmb.cc",
  "agentId": "51448@cmb.cc",
  "agentMode": "Logout",
  "devices": {
    "Voice": "812002@cmb.cc"
  }
}
```

### 2.2.4. agentReady 就绪

**当前字段**
```
// 已经删除了params，_type，topics三个字段

{
  "eventName": "agentReady",
  "eventSrc": "52009@cmb.cc",
  "eventTime": "2018.02.27 09:21:29",
  "eventType": "agent",
  "serial": 1931747,
  "namespace": "cmb.cc",
  "srcDeviceId": "802040@cmb.cc",
  "deviceId": "802040@cmb.cc",
  "agentId": "52009@cmb.cc",
  "agentMode": "Ready",
  "devices": {
    "Voice": "802040@cmb.cc"
  }
}

```

### 2.2.5. agentWorkingAfterCall 话后处理

**当前字段**
```
// 已经删除了params，_type，topics三个字段

{
  "eventName": "agentWorkingAfterCall",
  "eventSrc": "51792@cmb.cc",
  "eventTime": "2018.02.27 09:21:28",
  "eventType": "agent",
  "serial": 1931732,
  "namespace": "cmb.cc",
  "srcDeviceId": "810030@cmb.cc",
  "deviceId": "810030@cmb.cc",
  "agentId": "51792@cmb.cc",
  "agentMode": "WorkNotReady",
  "devices": {
    "Voice": "810030@cmb.cc"
  }
}

```

### 2.2.6. agentAllocated 预占

**当前字段**
```
// 已经删除了params，_type，topics三个字段
{
  "eventName": "agentAllocated",
  "eventSrc": "53799@cmb.cc",
  "eventTime": "2018.02.27 09:21:28",
  "eventType": "agent",
  "serial": 1931739,
  "namespace": "cmb.cc",
  "srcDeviceId": "811088@cmb.cc",
  "deviceId": "811088@cmb.cc",
  "agentId": "53799@cmb.cc",
  "agentMode": "Allocated",
  "devices": {
    "Voice": "811088@cmb.cc"
  },
  "seqId": "eb910d9aebb448a7ac8ddd2a4c702f54"
}
```

### 2.2.7. agentNotReady 未就绪

**当前字段**
```
// 已经删除了params，_type，topics三个字段

{
  "eventName": "agentNotReady",
  "eventSrc": "51519@cmb.cc",
  "eventTime": "2018.02.27 09:21:29",
  "eventType": "agent",
  "serial": 1931748,
  "namespace": "cmb.cc",
  "srcDeviceId": "812120@cmb.cc",
  "deviceId": "812120@cmb.cc",
  "agentId": "51519@cmb.cc",
  "agentMode": "NotReady",
  "devices": {
    "Voice": "812120@cmb.cc"
  }
}
```


## 2.3. 呼叫事件
### 2.3.1. 呼叫事件通用字段

字段名 | 含义 | 备注
--- | --- | ---
callId | 呼叫标识 | 36位长度字符串

### 2.3.2. serviceInitiated 摘机

**当前字段**
```
// 已经删除了params，_type，topics三个字段
{
  "eventName": "serviceInitiated",
  "eventSrc": "808122@cmb.cc",
  "eventTime": "2018.02.27 09:21:28",
  "eventType": "csta",
  "serial": 2791006,
  "namespace": "cmb.cc",
  "srcDeviceId": "808122@cmb.cc",
  "callId": "170b832d-cd0d-44d8-94f1-327e40f7baab",
  "deviceId": "808122@cmb.cc",
  "localState": "Initiate",
  "connectionId": "808122@cmb.cc|170b832d-cd0d-44d8-94f1-327e40f7baab",
  "cause": "normal",
  "initiatedDevice": "808122@cmb.cc"
}
```

### 2.3.3. originated 呼出

**当前字段**
```
// 已经删除了params，_type，topics三个字段
{
  "eventName": "originated",
  "eventSrc": "808122@cmb.cc",
  "eventTime": "2018.02.27 09:21:28",
  "eventType": "csta",
  "serial": 2791009,
  "namespace": "cmb.cc",
  "srcDeviceId": "808122@cmb.cc",
  "callId": "170b832d-cd0d-44d8-94f1-327e40f7baab",
  "deviceId": "808122@cmb.cc",
  "localState": "Initiate",
  "connectionId": "808122@cmb.cc|170b832d-cd0d-44d8-94f1-327e40f7baab",
  "cause": "newCall",
  "callingDevice": "808122@cmb.cc",
  "calledDevice": "9018690484803@cmb.cc"
}
```

### 2.3.4. delivered 振铃

**当前字段**
```
// 已经删除了params，_type，topics三个字段

{
  "eventName": "delivered",
  "eventSrc": "811159@cmb.cc",
  "eventTime": "2018.02.27 09:21:29",
  "eventType": "csta",
  "serial": 2791025,
  "namespace": "cmb.cc",
  "srcDeviceId": "811159@cmb.cc",
  "callId": "25da573c-5ddf-4a28-95b6-430976ea3456",
  "deviceId": "811159@cmb.cc",
  "localState": "Alerting",
  "connectionId": "811159@cmb.cc|25da573c-5ddf-4a28-95b6-430976ea3456",
  "cause": "PREOCCUPIED",
  "alertingDevice": "811159@cmb.cc",
  "callingDevice": "01592648****@cmb.cc",
  "calledDevice": "811159@cmb.cc",
  "userData": {
    "data": {
      "id": "49108734",
      "selectAgent": "53728@cmb.cc",
      "originalANI": "01592648****@cmb.cc",
      "originalDNIS": "811159@cmb.cc",
      "originalCallId": "25da573c-5ddf-4a28-95b6-430976ea3456",
      "prdType": "E",
      "callType": "PREOCCUPIED"
    }
  },
  "split": "7021@cmb.cc"
}
```

### 2.3.5. established 接通

**当前字段**
```
// 已经删除了params，_type，topics三个字段

{
  "eventName": "established",
  "eventSrc": "807157@cmb.cc",
  "eventTime": "2018.02.27 09:21:29",
  "eventType": "csta",
  "serial": 2791031,
  "namespace": "cmb.cc",
  "srcDeviceId": "807157@cmb.cc",
  "callId": "a7b57e12-90bc-4c2f-9cd8-2be202b36382",
  "deviceId": "807157@cmb.cc",
  "localState": "Connect",
  "connectionId": "807157@cmb.cc|a7b57e12-90bc-4c2f-9cd8-2be202b36382",
  "cause": "newCall",
  "answeringDevice": "807157@cmb.cc",
  "callingDevice": "01368248****@cmb.cc",
  "calledDevice": "807157@cmb.cc",
  "split": "7017@cmb.cc",
  "sipCallId": "60b16b4d-95ff-1236-8b95-525433b6115b",
  "sipFromTag": "aB2my4604mZSD",
  "sipToTag": "609731328"
}
```

### 2.3.6. connectionCleared 挂断 ing

**建议增加字段**

字段名 | 字段类型 | 描述
---|---|---
isEstablished | Boolean | 挂断前，该呼叫有没有接通
firstReleaseDevice | String | 是哪个设备先挂断的电话，建议不要修改releaseDevice了
deliveredTimestamp | Int | 振铃时间戳
establishedTimestamp | Int | 接通时间戳
deliverLength | Int | 振铃时长
talkLength | Int | 通话时长，如果没有通话，则该值为0
isInnerCall | Boolean | 是内部分机之间的呼叫，还是内外线之间的呼叫
endReason | String | 挂断原因，客户先挂，座席先挂，还是未接听之类的

> 当前模型的不合理之处: 当前挂断事件在手工外呼时，挂断事件座席端会收到两个；在预占式外呼时，挂断事件只有一个。`我认为在两方通话时，应该统一成只发送一个挂断事件给客户端`

```
// 手工外呼时，座席收到两个挂断事件例子
"02-27 09:14:06.965 52017@cmb.cc 802106@cmb.cc  {"eventName":"connectionCleared","eventSrc":"802106@cmb.cc","eventTime":"2018.02.27 09:15:57","eventType":"csta","serial":2764343,"namespace":"cmb.cc","srcDeviceId":"802106@cmb.cc","callId":"c52617fb-8409-4d21-83e3-244d6af94ba0","deviceId":"802106@cmb.cc","localState":"Idle","connectionId":"802106@cmb.cc|c52617fb-8409-4d21-83e3-244d6af94ba0","cause":"normalClearing","releasingDevice":"802106@cmb.cc"}"
"02-27 09:14:06.970 52017@cmb.cc 802106@cmb.cc  {"eventName":"connectionCleared","eventSrc":"802106@cmb.cc","eventTime":"2018.02.27 09:15:57","eventType":"csta","serial":2764346,"namespace":"cmb.cc","srcDeviceId":"802106@cmb.cc","callId":"c52617fb-8409-4d21-83e3-244d6af94ba0","deviceId":"918691750603@cmb.cc","localState":"Idle","connectionId":"802106@cmb.cc|c52617fb-8409-4d21-83e3-244d6af94ba0","cause":"normalClearing","releasingDevice":"918691750603@cmb.cc"}"
```

**当前字段**
```
// 已经删除了params，_type，topics三个字段
{
  "eventName": "connectionCleared",
  "eventSrc": "812137@cmb.cc",
  "eventTime": "2018.02.27 09:21:28",
  "eventType": "csta",
  "serial": 2791010,
  "namespace": "cmb.cc",
  "srcDeviceId": "812137@cmb.cc",
  "callId": "82d3ca4c-bbbb-4dad-b577-d5b0b5962b3e",
  "deviceId": "9015254107977@cmb.cc",
  "localState": "Idle",
  "connectionId": "812137@cmb.cc|82d3ca4c-bbbb-4dad-b577-d5b0b5962b3e",
  "cause": "busy",
  "releasingDevice": "9015254107977@cmb.cc"
}
```

### 2.3.7. transferred 转移

**当前字段**
```
// 已经删除了params，_type，topics三个字段
{
  "eventName": "transferred",
  "eventSrc": "8002@zhen04.cc",
  "eventTime": "2017.03.18 14:39:31",
  "eventType": "csta",
  "serial": 121645,
  "namespace": "zhen04.cc",
  "srcDeviceId": "8002@zhen04.cc",
  "callId": "10549a5f-41c8-4309-a1ad-faa61c8f3777",
  "deviceId": "8002@zhen04.cc",
  "localState": "Queued",
  "originCallInfo": {

  },
  "connectionId": "8002@zhen04.cc|10549a5f-41c8-4309-a1ad-faa61c8f3777",
  "primaryOldCall": "0",
  "secondaryOldCall": "10549a5f-41c8-4309-a1ad-faa61c8f3777",
  "transferringDevice": "8002@zhen04.cc",
  "transferredToDevice": "8003@zhen04.cc",
  "newCall": "10549a5f-41c8-4309-a1ad-faa61c8f3777"
}
```

### 2.3.8. conferenced 会议

**当前字段**
```
// 已经删除了params，_type，topics三个字段
{
  "eventName": "conferenced",
  "eventSrc": "8001@zhen04.cc",
  "eventTime": "2017.03.18 14:15:37",
  "eventType": "csta",
  "serial": 121110,
  "namespace": "zhen04.cc",
  "srcDeviceId": "8001@zhen04.cc",
  "callId": "b81b0af2-e40a-4e0e-a8ce-47be9474f245",
  "deviceId": "8003@zhen04.cc",
  "localState": "Connect",
  "connectionId": "8003@zhen04.cc|b81b0af2-e40a-4e0e-a8ce-47be9474f245",
  "primaryOldCall": "0",
  "secondaryOldCall": "b81b0af2-e40a-4e0e-a8ce-47be9474f245",
  "conferencingDevice": "8001@zhen04.cc",
  "addedParty": "8003@zhen04.cc",
  "newCall": "b81b0af2-e40a-4e0e-a8ce-47be9474f245"
}
```

### 2.3.9. retrieved 取回

**当前字段**
```
// 已经删除了params，_type，topics三个字段
{
  "eventName": "retrieved",
  "eventSrc": "8008@jasmine.cc",
  "eventTime": "2018.02.28 15:52:59",
  "eventType": "csta",
  "serial": 46,
  "namespace": "jasmine.cc",
  "srcDeviceId": "8008@jasmine.cc",
  "callId": "1dc24f9d-f740-4d3a-8fe7-bd108dc8c62a",
  "deviceId": "8008@jasmine.cc",
  "localState": "Connect",
  "connectionId": "8008@jasmine.cc|1dc24f9d-f740-4d3a-8fe7-bd108dc8c62a",
  "cause": "NORMAL",
  "retrievingDevice": "8008@jasmine.cc"
}
```

### 2.3.10. held 保持

**当前字段**
```
// 已经删除了params，_type，topics三个字段
{
  "eventName": "held",
  "eventSrc": "8008@jasmine.cc",
  "eventTime": "2018.02.28 15:52:45",
  "eventType": "csta",
  "serial": 45,
  "namespace": "jasmine.cc",
  "srcDeviceId": "8008@jasmine.cc",
  "callId": "1dc24f9d-f740-4d3a-8fe7-bd108dc8c62a",
  "deviceId": "8008@jasmine.cc",
  "localState": "Hold",
  "connectionId": "8008@jasmine.cc|1dc24f9d-f740-4d3a-8fe7-bd108dc8c62a",
  "cause": "NORMAL",
  "holdingDevice": "8008@jasmine.cc"
}
```

# 3. 统一字段说明表

字段名 | 含义 | 备注
--- | --- | ----
eventName | 事件名称 | 
eventSrc | 事件源 | 
eventTime | 事件时间 |
eventType | 事件类型 | ["csta", "agent"]
serial | 事件序号 | 
namespace | 命名空间 ,
srcDeviceId | 订阅事件的设备 | 
callId | 呼叫ID | 
deviceId | 发生变化的设备 | 
localState | 事件发生后设备的状态 | ['Connect', 'Initiate', 'Alerting', 'Hold', 'None', 'Queued', 'Fail', 'Idle'],
agentStatus | 座席状态 | ['NotReady', 'WorkNotReady', 'Idle', 'OnCallIn', 'OnCallOut', 'Logout', 'Ringing', 'OffHook', 'CallInternal', 'Dailing', 'Ringback', 'Conference', 'OnHold', 'Other'],
connectionId | 连接 ID | 
primaryOldCall | 会议前的被保持的呼叫 | 
secondaryOldCall | 会议前活动的呼叫 | 
conferencingDevice | 发起会议的设备 | 
addedParty | 加入会议的设备 | 
newCall | 会议后新的呼叫ID |
userData | 随路数据 | 
alertingDevice | 振铃设备Id | 
callingDevice | 主叫设备 |
calledDevice | 被叫设备 | 
