## RAN（Radio Access Network）
終端手機，連接到網路裡面的這個功能，就是 RAN，BaseStation 屬於 RAN。
從 1G 到 5G 都是，手機 -> 接入網 -> 承載網 -> 核心網 -> 承載網 -> 接入網 -> 手機。
一個 BaseStation，通常包括 BBU（Bandwidth Based Unit,主要負責信號調製）、RRU（Remote Radio Unit,主要負責射頻處理）、饋線（連接 RRU 和天線）、天線（主要負責線纜上導行波和空氣中空間波之間的轉換）。

![](https://i.imgur.com/Wqz1WMF.png "基站的組成部分")

### D-RAN（Distributed RAN）
天線 + RRU 使得 RAN 就變成了 D-RAN，就是 Distributed RAN（分布式無線接入網），好處是大大縮短了 RRU 和天線之間饋線的長度，可以**減少信號損耗**，也可以**降低饋線的成本**。
另一方面，可以讓網路規劃更加靈活，畢竟 RRU 加天線比較小，放置空間更靈活了。

但這種部署方式會太耗成本和機房

![](https://i.imgur.com/cB9ivks.png)

### C-RAN（Centralized RAN）
C-RAN，集中化無線接入網。將 RRU 拉遠，把 BBU 全部都集中在中心機房。
此方式極大減少基站機房數量，拉遠之後的 RRU 搭配天線，可以安裝在離用戶更近距離的位置。距離近了，發射功率就低了，低的發射功率意味著用戶終端電池壽命的延長和 RAN 功耗的降低。

那分散的 BBU 變成 BBU 基帶池（放置同一個中心）之後，可以統一**管理**和**調度**，資源調配更加靈活。之後也將 BBU 虛擬化，以前 BBU 是專門的硬體設備，非常昂貴，現在用個 x86 伺服器，裝個虛擬機（VM，Virtual Machines），運行具備 BBU 功能的軟體，然後就能當 BBU 用了。

![](https://i.imgur.com/AvHH9gV.png "NFV")

![](https://i.imgur.com/aj1dDkV.png)

## 5G RAN 變化
5G 網路中，RAN 不再是由 BBU、RRU、天線等組成。而被重構成

- CU（Centralized Unit，集中單元）
  - 原 BBU 的非實時部分將分割出來，重新定義為 CU，負責處理非即時協定和服務。
- DU（Distribute Unit，分布單元）
  - BBU 的剩餘功能重新定義為 DU，負責處理物理層協定和即時服務
- AAU（Active Antenna Unit，有源天線單元）
  - BBU 的部分物理層處理功能與原 RRU 及無源天線合併為 AAU。
  
![](https://i.imgur.com/NTZpgMv.png)

`CU` 和 `DU`，以處理內容的即時性進行區分。

![](https://i.imgur.com/33pgUpL.png)

圖中，EPC（就是 4G 核心網）被分為 New Core（5GC，5G 核心網）和 MEC（行動網路邊緣運算）兩部分。MEC 移動到和 CU 一起，離 BaseStation 更近（下沉）。

![](https://i.imgur.com/Oq7QAjn.png)

要 BBU 功能拆分、核心網部分下沉，根本原因就是為了滿足 5G 不同場景的需要。這就是要應對不同場景的需求，不得不說 5G 的一個技術**網路切片（Network Slicing）**。

## 5G 網路切片
一個物裡的網路，依照需求切分多個邏輯網路。因為需求多變，網路也要多樣化，網路要多樣化就要切片，這樣彼此之間靈活性變大了，因此才出現了 DU、CU 的架構。

![](https://i.imgur.com/LLvIjNC.png "多種部署型態")

## 5G 承載網
5G 要低延遲、大頻寬等，因此有了三大應用場景

![](https://i.imgur.com/pWVMZwE.png)

- eMBB
  - 高數據速率
    - 增強型移動頻寬可實現超高數據速率通信，增強的移動性和更廣泛的網絡覆蓋。5G 網路具有與以太網相似的性能和用戶體驗。
- uRLLC
  - 低延遲，高可靠性
    - 超可靠低延遲通訊對公共安全，電子醫療，醫療自動化，自動駕駛汽車和"觸覺"互聯網至關重要。
- mMTC
  - 物聯網
    - 大規模機器對機器通訊為極大量的設備提供無處不在的連接。這些設備需要高密度的網路連接，能源效率以延長電池壽命並降低每個網路連接的成本。

5G 的接 RAN（就理解為 BaseStation 系統）發生了翻天覆地的變化，進而帶著承載網（BaseStation 和 BaseStation 之間、BaseStation 和核心網之間的連接系統）也發生了巨變



## Ref
[1](http://tech.intchain.io/?thread-489.htm)
