# OpenFlow 包含元件

![](https://i.imgur.com/mAnCzBi.png)

交換機會與上方的 Controller 溝通，這個 Controller 再透過 OpenFlow 協定與 OpenFlow Channel 溝通。

## Flow Table
- Flow Table 中包含多筆 Flow Entry 資料
  - 經由 OpenFlow 協定，Controller 可以新增、修改以及刪除這些 Flow Entry 的內容

當封包抵達網路交換機，會根據 Flow Table 的內容去比對符合的條件，Flow Table 中的每筆資料都有著優先順序，Flow Table 間的表格也是如此。一旦找到相符的 Flow Entry，就會去執行 Flow Entry 中所設定好的動作指令，否則會根據預先設定好的預設值執行動作指令，這預先設定好的動作可能是尋找下一個 Flow Table 的內容、或是直接丟棄、或者轉給 OpenFlow Channel 來處理。

## 動作指令
動作指令內容可以是一般的動作（Action）或是所謂的 Pipeline Processing。當封包找到符合的 Flow Entry，就會執行相對應的動作指令。


一般的動作指令可能是針對封包
- 轉發
- 修改內容
- 透過 Pipeline Processing，讓下一個 Flow Table 處裡
- 給 Group 處理

一個 Group 可包含多個複雜的處理過程，這些處理過程可能是 Flooding、Multi-path Forwarding，或是 Fast Reroute，或者 Link Aggregation 等等。Group 甚至可以將多個不同的 Flow Entry 透過同樣的處理過程來對網路封包做相同的處理動作。


## Group Table
- Group Table 包含許多 Group Entry
  - 每一個 Group Entry 包含多個 Action Bucket
    - 這些 Action Bucket 會組成一個 List
    - 這些 Action Bucket 會根據各個 Group 種類而不同

一旦網路封包送到指定的 Group，一個或多個相對應的 Action Bucket 中的動作就會被執行。

## 埠的處理
Flow Entry 的動作中可能會指定要轉發到指定的 Port。可能丟置 controller，或不是使用 Open Flow 處裡
Port 可能是
- 虛擬
- 實體
- 保留

## SDN 與 OpenFlow 協定
- SDN 將路由器或交換機的控制方式與路由器或交換機分離，透過撰寫的軟體來控制路由器或交換機
  - 藉此規劃網路、控制網路流量，而且整個過程不需要更改硬體的規劃。
- OpenFlow 協定用於控制路由器或交換機的轉發平面，來達到以軟體來定義網路的目的
