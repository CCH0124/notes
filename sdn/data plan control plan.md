# 轉發平面與控制平面

###### tags: `SDN` `network`

## 何謂轉發平面
轉發平面（Forwarding Plane）就是在路由器中架構中處裡送進來的網路封包，並藉由查找路由表來決定接下來要把網路封包轉發到哪一個網路介面。

Forwarding Plane 有時候也被稱為 Data Plane 或是 User Plane。而這個轉發平面只會處理 Inbound 介面進來的網路封包。

## 轉發平面所使用的資料來源
- 路由表（Routing Table）中的資訊來決定如何轉發網路封包
- 路由表有時候也稱為RIB，也就是 Routing Information Base
- 路由表是由 Routing Control Plane 來產生
- 路由表有不同的產生方式
    - 靜態路由
    - 動態路由
### 靜態路由
人工輸入，速度快，靈活性不高只要拓樸變更，管理員就要維護。

通常會使用在 stub Network 之間連線或使用在指定預設閘道上，讓不知道要送往哪裡的網路封包送到預設閘道。

![](https://i.imgur.com/PnHUAuw.png)

設定方式

```shell=
routeA(config)# ip route 172.16.1.0 255.255.255.0 172.16.2.1 permanent # permanent 介面關掉，讓此設定繼續存在
```

### 動態路由
一切的工作都交給路由器設備之間去協調，互相交換並學習這些資料，管理人員只要做 Routing Protocol 的設定即可，但是這種方式比較耗費系統資源，速度也稍微慢一些。

對於內部路由協定而言，演算法大致分為以下三種：

- Distance Vector
    - 設備數目（Hops）來決定路徑
- Link State
    - 最短路徑演算法（Shortest Path First）
- Balanced Hybrid
    - 綜合上述兩點

### 預設路由
所謂的預設路由就是當不知道要將這個封包送往哪裡的時候，就會採用這個預設路由所指定的路徑。

![](https://i.imgur.com/ftrFrhB.png)

```shell=
routeB(config)# ip route 0.0.0.0 0.0.0.0 10.1.1.2
routeB(config)# ip classless # 封包的目的端網路位址即使沒有存在於路由器B的路由表內，路由器B設備也會把這個封包轉送給預設路由所指到的路由器設備，而不會直接丟棄。
```

### 轉發平面的效能考量
以下這些都是路由器在轉發網路封包時，在效能上扮演著重要的角色：

1. 拆解（Extract）和解碼（Decode）網路封包取得Header中所需的資料。
2. 分析（Analyze）網路封包的資料。

在查找的速度可能會使用 Hash table、Binary tree、Radix Tree 等

## 什麼是控制平面
控制平面（Control Plane）最主要的目的在**建立路由表**，進而刻畫出整個網路拓撲，決定當網路封包進來的時候，應該要如何做，這包含決定網路封包應該要由哪一個介面轉發出去，或者是否應該要丟棄，所以控制平面建立這樣的資料後，再由轉發平面去執行。

路由協定　RIP、IGRP、EIGRP　等等，都是用很多不同複雜的演算法來建立路由表，這些動作就都是控制平面所負責的。
