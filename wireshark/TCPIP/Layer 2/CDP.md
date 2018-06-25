# CDP
在規劃一個網路架構時，如果之後不小心失去怎個網路架構的資訊，肯定會造成網管上的混亂，如果該環境設備都是 Cisco 設備，則可透過 CDP 協定探索收集到全部 Cisco 網路設備的軟硬體資訊，立即重建網路拓樸資訊。

## 利用 CDP 協定交換資訊
Cisco 網路設備會定期與周遭的 Cisco 網路設備進行資訊交換的動作，要達到這樣定期的資料交換動作，就必須透過CDP協定才行。CDP（Cisco Discovery Protocol）是 Cisco 的專利，運行於 OSI 七層協定之中第二層 `Data Link Layer` 的協定。可以透過它來讓設備相互收集設備的資訊，如：ISO 版本、IP 位址等。CDP 對於 Cisco 設備的管理人員是一大福音。

### CDP 協定所提供的資訊 

- 設備ID 
    - 設備的名稱
- 第三層的IP位址清單
    - `來源端`和`目的端`埠名稱，例如：ethernet0
- 埠的ID 
- 所支援的功能列表 
- 硬體平台
    - 硬體型號

### 關閉或開啟 CDP 協定的指令 
以下範例拓樸圖
![](https://i.imgur.com/m9FvIID.png)

#### 開啟或關閉 CDP

```bash
R1(config)#no cdp run
R1(config)#cdp run
```
#### 關閉或重啟某介面的 CDP 

```bash
R1(config-if)#no cdp enable
R1(config-if)#cdp enable
```

### CDP 協定的設定方式

#### 顯示CDP協定資訊的指令

Cisco 路由器上的 CDP 協定可以開啟或關閉，預設為開啟。

```bash
R1#show cdp 
Global CDP information:
    Sending CDP packets every 60 seconds
    Sending a holdtime value of 180 seconds
    Sending CDPv2 advertisements is enabled
```
#### CDP 指令
環境是以 Packet tracer

```bash
R1#show cdp ?
  entry      Information for specific neighbor entry
  interface  CDP interface status and configuration
  neighbors  CDP neighbor entries
  traffic    CDP statistics

```

#### CDP neighbor entries
```bash
R1#show cdp neighbors
Capability Codes: R - Router, T - Trans Bridge, B - Source Route Bridge
                  S - Switch, H - Host, I - IGMP, r - Repeater

Device ID        Local Intrfce     Holdtme    Capability  Platform  Port ID
R2               Ser 3/0            166           R       7206VXR   Ser 3/0
ESW2             Fas 0/0            166          S I      3745      Fas 0/0

```

- Device ID 
    - 鄰近設備的 ID
- Local Intrfce 
    - 本地端的介面 
- Holdtme
    - 設備持有目前資訊長達多久的時間
    - 單位是秒
- Capability
    - 鄰近設備的種類代碼 
- Platform
    - 鄰近設備的硬體平台 
- Port ID
    - 鄰近設備的遠端埠ID

#### 鄰近設備的種類代碼

**設備代碼** | **意義** |
--- | --- |
R | Router
T | Trans Bridge(Ethernet 架構)
B | Source Route Bridge(Token Ring 架構)
S | Switch
H | Host
I | IGMP
r | Repeater
P | Phone

#### 取得更詳細的資訊

```bash
R1#show cdp neighbors detail
-------------------------
Device ID: R2
Entry address(es):
  IP address: 192.168.1.254
Platform: Cisco 7206VXR,  Capabilities: Router
Interface: Serial3/0,  Port ID (outgoing port): Serial3/0
Holdtime : 126 sec

Version :
Cisco IOS Software, 7200 Software (C7200-ADVENTERPRISEK9-M), Version 12.4(24)T5, RELEASE SOFTWARE (fc3)
Technical Support: http://www.cisco.com/techsupport
Copyright (c) 1986-2011 by Cisco Systems, Inc.
Compiled Fri 04-Mar-11 06:49 by prod_rel_team

advertisement version: 2

-------------------------
Device ID: ESW2
Entry address(es):
  IP address: 192.168.100.10
Platform: Cisco 3745,  Capabilities: Switch IGMP
Interface: FastEthernet0/0,  Port ID (outgoing port): FastEthernet0/0
Holdtime : 126 sec

Version :
Cisco IOS Software, 3700 Software (C3745-ADVIPSERVICESK9-M), Version 12.4(25d), RELEASE SOFTWARE (fc1)
Technical Support: http://www.cisco.com/techsupport
Copyright (c) 1986-2010 by Cisco Systems, Inc.
Compiled Wed 18-Aug-10 08:18 by prod_rel_team

advertisement version: 2
VTP Management Domain: ''
Duplex: half


```

#### 特定鄰近設備的相關資訊

**參數**

```bash
Router#show cdp entry ?
  *     all CDP neighbor entries
  WORD  Name of CDP neighbor entry
```
**指定某台設備**
```bash
R1#show cdp entry R2
-------------------------
Device ID: R2
Entry address(es):
  IP address: 192.168.1.254
Platform: Cisco 7206VXR,  Capabilities: Router
Interface: Serial3/0,  Port ID (outgoing port): Serial3/0
Holdtime : 138 sec

Version :
Cisco IOS Software, 7200 Software (C7200-ADVENTERPRISEK9-M), Version 12.4(24)T5, RELEASE SOFTWARE (fc3)
Technical Support: http://www.cisco.com/techsupport
Copyright (c) 1986-2011 by Cisco Systems, Inc.
Compiled Fri 04-Mar-11 06:49 by prod_rel_team

advertisement version: 2

```
**顯示的資訊**

- 鄰近設備的Device ID
    - R2
- 網路第三層協定的資訊
    - 192.168.1.254
- 鄰近設備的硬體平台
    - Cisco 7206VXR
- 鄰近設備的種類
    - Router
- 本地端介面種類和outgoing遠端的埠ID 
    - Serial3/0, Serial3/0
- Hold-time值，單位是秒 
    - 138
- Cisco IOS軟體種類和版本資訊
    - Cisco IOS Software, 7200 Software (C7200-ADVENTERPRISEK9-M), Version 12.4(24)T5, RELEASE SOFTWARE (fc3)...

>鄰近設備的種類可以顯示出這設備是 Route、Switch 或是其它種類的設備。

#### 顯示目前傳送CDP的介面狀況

**指定 Serial 介面**

```bash
R1#show cdp interface s3/0
Serial3/0 is up, line protocol is up
  Encapsulation HDLC
  Sending CDP packets every 60 seconds
  Holdtime is 180 seconds

```

**可得到的資訊**

- 介面是否運作中
    - FastEthernet0/0 is up
- 每隔多久送出CDP封包（預設是每隔60秒）
    - Sending CDP packets every 60 seconds
- Hold-time
    - 180 seconds(預設)
- 封裝種類
    - HDLC

#### 顯示與介面的流量相關的資訊

```bash
R1#show cdp traffic
CDP counters :
        Total packets output: 36, Input: 35
        Hdr syntax: 0, Chksum error: 0, Encaps failed: 0
        No memory: 0, Invalid packet: 0, Fragmented: 0
        CDP version 1 advertisements output: 0, Input: 0
        CDP version 2 advertisements output: 36, Input: 35

```

**顯示資訊**
- 資料錯誤
- Checksum錯誤
- 封裝失敗
- 記憶體用盡（Out of memory）
- 無效的封包
- 已分割的封包
- 已經送出和已經接收到的CDPv1封包
- 已經送出和已經接收到的CDPv2封包 

#### 清除 CDP traffic 相關資訊

用來把 CDP 的 traffic counters 歸零

```bash
R1#clear cdp counters
R1#show cdp traffic # 驗證
CDP counters :
        Total packets output: 1, Input: 0
        Hdr syntax: 0, Chksum error: 0, Encaps failed: 0
        No memory: 0, Invalid packet: 0, Fragmented: 0
        CDP version 1 advertisements output: 0, Input: 0
        CDP version 2 advertisements output: 1, Input: 0

```

#### 清除 CDP neighbors 相關資訊

將 show cdp neighbors 資訊清除

```bash
R1#clear cdp table
R1#show cdp neighbors
Capability Codes: R - Router, T - Trans Bridge, B - Source Route Bridge
                  S - Switch, H - Host, I - IGMP, r - Repeater

Device ID        Local Intrfce     Holdtme    Capability  Platform  Port ID

```
### 設定 CDP Timer 和 hold-time 值

> CDP 資訊的交換頻率預設為 60 秒交換一次

**改變 CDP Timer**
將交換 CDP 的頻率改為每隔 65 秒交換
```bash
R1(config)#cdp timer ?
  <5-254>  Rate at which CDP packets are sent (in  sec)

R1(config)#cdp timer 65
R1(config)#do show cdp
Global CDP information:
        Sending CDP packets every 65 seconds
        Sending a holdtime value of 180 seconds
        Sending CDPv2 advertisements is  enabled
```
變為預設 60 秒
```bash
R1(config)#no cdp timer
R1(config)#do show cdp
Global CDP information:
        Sending CDP packets every 60 seconds
        Sending a holdtime value of 180 seconds
        Sending CDPv2 advertisements is  enabled
```

>通常要更改 CDP 的發送頻率都是因為頻寬的考量，因此可以根據所使用的環境來設定適合的發送頻率。

**改變 hold-time**

改為保存 200 秒 CDP 設備資訊
```bash
R1(config)#cdp holdtime 200
R1(config)#do sh cdp
Global CDP information:
        Sending CDP packets every 60 seconds
        Sending a holdtime value of 200 seconds
        Sending CDPv2 advertisements is  enabled

```

還原預設
```bash
R1(config)#no cdp holdtime 200
R1(config)#do sh cdp
Global CDP information:
        Sending CDP packets every 60 seconds
        Sending a holdtime value of 180 seconds
        Sending CDPv2 advertisements is  enabled

```
>hold-time 表示 Cisco 設備要在本地端保留其他設備的 CDP 資訊長達多久的時間，預設值為 180 秒。

## VLAN 與 CDP
>通常預設的VLAN都是VLAN 1，因此，CDP 和 VTP 協定的封包通常都會送到VLAN 1之中。
