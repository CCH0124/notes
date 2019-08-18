支援 OpenFlow 協定的交換機可以分為兩種類型：
- 支援 OpenFlow 協定的處理方式
  - 只能處理 OpenFlow 協定的動作
- 同時支援 OpenFlow 協定以及非 OpenFlow 協定的混合式（Hybrid）交換機
  - 可以處理 OpenFlow 協定的動作以及一般乙太網路（Ethernet）的處理動作，例如傳統式 Level 2 Ethernet Switching、VLAN 處理以及 Level 3 Routing、ACL 與 QoS 等等

混合式的網路交換機來說，這種交換機必須具備一種分辨機制（Clarification Mechanism）可以將網路封包導向 OpenFlow 的 Pipeline 處理過程或是一般網路（非OpenFlow）的 Pipeline 處理過程。

舉例來說，這種分辨機制可能是根據 VLAN 的 Tag 來辨識，或是根據所進來的埠（Input Port）來判斷是否應該要丟往 OpenFlow 協定的 Pipeline。當然，也可能是直接把所有的封包都導向 OpenFlow 協定的 Pipeline。

混合式網路交換機也可以透過保留埠（Reserved Port）把網路封包從 OpenFlow 協定的 Pipeline 轉向一般的 Pipeline，只要標示成 `NORMAL` 或 `FLOOD` 即可。

## Pipeline 處理流程說明
OpenFlow Pipeline 的目的在於定義網路封包如何與這些 Flow Table 做互動。

Flow Table 有從零開始的序號，每個 Flow Table 都有標號碼，而每個網路封包的 Pipeline 處理都會從 Flow Table 0 開始做處理，而整個 Pipeline 處理流程大略如下圖所示。

![](https://i.imgur.com/RpCZNbu.png "Pipeline處理流程")

### Goto 指令
找到符合規則的 Flow Entry，其包含的指令可能會是 Goto 這樣的動作指令。直接跳往指定的 Flow Table 編號，但只能往後跳，不能往前跳。


以例子來做說明，如果包含 Goto 這個 Flow Entry 是位於 Flow Table 0，然後 Goto 指令希望跳到 Flow Table 4，則代表接下來會直接跳過編號為 1、2 以及 3 這些 Flow Table，直接跳往 Flow Table 4 繼續尋找符合規則的 Flow Entry。

最後一個 Flow Table 的 Flow Entry 是不能包含 Goto 指令的，因為後面已經沒有多餘的 Flow Table 可以繼續跳了。如果 Flow Entry 中的指令沒有 Goto，大部分的情況就是會包含需要直接的動作，大多都是進行轉發（Forwarding），而此時也代表這個封包的 Pipeline 過程到此為止。

### Table Miss 情況
在某個 Flow Table 尋找符合規則的 Flow Entry 時，都找不到相對應的 Flow Entry，這種情況就稱為 Table Miss。

發生 Table Miss 時，會執行原本設定好的 Table Miss 指定動作，如丟棄、到另一個指定的 Flow Table 甚至送往 controller，這個設定是每個 Flow Table 分開的。

## 了解 Flow Entry 內容

一個 Flow Table 會包含多個 Flow Entry，而一個 Flow Entry 事實上包含以下這些資料：
1. 比對欄位：這會包含 Ingress Port、網路封包的 Header，以及可能從上一個 Flow Table 傳過來的 Metadata，最後這個部分可有可無
2. 優先序：Flow Entry 的優先序
3. 計數器：一旦有相符合的網路封包，這個計數器就會被更新
4. 執行動作：也就是針對符合比對的網路封包的動作
5. 逾時時間：這是代表這筆 Flow Entry 存在的最大逾時時間
6. Cookie：Controller 所使用的資料

重要的是**比對欄位**和**優先序**。優先序數值從 0 開始，**數字越小代表優先序越高**，所以如果設定某個 Flow Entry 的優先序為零，而且比對欄位是什麼都通過的話，等於就是把這筆 Flow Entry 設定為 Table-miss Flow Entry。
