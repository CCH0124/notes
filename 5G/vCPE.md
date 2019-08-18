vCPE 是一種透過軟體而非硬體向分支機構或邊緣網路提供路由、安全、SD-WAN 等虛擬託管服務的方法。透過 vCPE，現在可以透過基於軟體的虛擬功能實現所有基於硬體的操作。

## What Is vCPE ?
**虛擬客戶端設備（vCPE）也稱為 `cloud-CPE`，使用基於軟體的功能代替在硬體上執行的操作**。

CPE 是在客戶場所運行並執行特定服務或任務的任何類型的設備，可以是 `firewall`、`border gateway`、`route`、`NAT`、`VPN` 等。
現在，vCPE 透過分離傳統硬體的功能並讓遠程服務提供商或管理平台託管所有基於軟體的功能來改變這種方法。vCPE 運行在商用硬體和虛擬網路功能（VNF）上，而不是執行特定網路功能的專有 `ASIC`。

![](https://i.imgur.com/wz2VUll.png)

## Traditional vs Virtualized CPEs
傳統上，為了提供 VPN 或 WAN 連接等服務，服務提供商需要在客戶所在地方安裝一個專門為客戶而設計硬體。

服務提供商的技術人員必須認真訪問客戶需求以配置 CPE，並且每次客戶需要新服務時，需要支付非常昂貴的費用。最重要的是，傳統的 CPE 過去常常在客戶的機架空間佔用大量空間，消耗大量能源，並且需要而外專家的維護。

下面的表顯示了 CPE 和 vCPE 之間的主要區別。
||功能|平台|硬體類型|服務管理|
|---|---|---|---|---|
|CPE|基於硬體|實體網路|專有 ASIC|分散式|
|vCPE|基於軟體|虛擬網路|通用硬體|集中式|

如今，託管服務提供商（Managed Service Providers,MSP）正在尋找獲得競爭優勢的方法。借助 vCPE，他們可以提供除了 Internet 連接之外的服務，如託管 IP-VPN、SD-WAN（軟體定義 WAN）、SIP（會話啟動協議）等等。

由於功能在提供商的遠程數據中心，並且軟體在現場硬體上運行，因此客戶只需部署具有足夠資源和功能的通用框架。

## Advantages of using a vCPE
很明顯，從軟體中分離底層硬體會帶來無數好處。但是使用 vCPE 的那些優勢到底是什麼？
1. Easier and Faster Deployment（輕鬆快速部署）
	- 服務提供商可以更輕鬆的推出新服務，並明顯的縮短服務時間（TTS）
	- 還可以從 NOC 遠程部署、升級和刪除任何新的演算法和協定，無需親自訪問客戶的場所
2. Scalability and Elasticity（可擴展和伸縮）
	- 服務提供商可以利用共享的計算能力、儲存和內存
	- 如果虛擬路由器未充分利用或過度使用，則可以與其他附近客戶平均分配資源以匹配需求，這種方法為更好的可擴展性提供了空間，同時也降低了成本
3. Reduce CPE’s CapEx and OpEx（降低 CPE 的資本支出和營運支出）
	- 由於 vCPE 並非在內部部署，因此有助於降低服務提供商的資本和運營費用
	- 無需專用硬體、交付、安裝和專業技術人員
4. Centralized User Interface（集中使用者介面）
	- vCPE 由虛擬化管理，因此無需登錄許多設備，所有管理和監控都可以通過一致且統一的界面完成
5. Sophisticated and flexible services（精緻和靈活的服務）
	- 許多新的 VNF 提供商開始創新傳統網路演算法
	- vCPE 是創造新演算法和服務的絕佳機會，這在傳統的基於硬體的 CPE 中是不可能的
	
## How Does a vCPE Work
VNF 是 vCPE 的基礎。所有傳統的網路服務（routing、switching、session management、intrusion 和 Malware detection）都開始轉換為軟體功能（VNF）。

這些網路功能可以透過 MSP 的網路進行管理，並直接推送到託管 vCPE 的客戶標準化 x86 硬體中。網路接口設備（Network Interface Devices, NID）是由 MSP 的 NFV 提供支援的 vCPE 模塊。NID 模組是客戶端和 MSP 網路之間的接口。

![](https://i.imgur.com/ROnVTJO.png)

雖然許多 VNF 仍然是傳統設備的 VM（虛擬機）版本，但是許多流行的網路供應商，如愛立信、華為、阿爾卡特和諾基亞，都開始開發**基於雲的 VNF 解決方案**，這些解決方案已經為 vCPE 做好了準備。

如果 MSP 想要推送這個 vCPE 軟體，他們可以使用容器或微服務架構來實現。路由和交換器等傳統服務通常包含在 vCPE 的軟體中，但 MSP 可以透過提供 VNF 供應商提供的模組並透過 VNF 雲端平台進行管理來擴展其功能。

## When to use vCPE? A Use Case
雖然 MPLS 等 WAN 技術多年來一直可靠，但企業開始尋求更便宜、更靈活的解決方案。企業應用程序和用戶的數據消耗似乎只會增加，而 MPLS 也有限制。

**MPLS 的可擴展性具有很高的代價，其安裝和維護成本很高，而且最重要的是，它對頻寬有限制**。

**SD-WAN 是 vCPE 的最佳應用之一**

SD-WAN（Software Defined-WAN）通過將底層網路硬體與其控制平面分離來改進 WAN 技術的操作。與 vCPE 一樣，SD-WAN 的想法是虛擬化。它用虛擬化設備取代了傳統的 WAN 分支路由器。

![](https://i.imgur.com/YwaGOFR.png)

>根據市場研究公司 Gartner 在 2018 年的一份報告中預測，"到 2023 年，超過 90% 的廣域網邊緣基礎設施更新計劃將基於虛擬客戶端設備（vCPE）平台或 SD-WAN 軟體/設備。"

## Network Appliances for vCPE
雖然虛擬化環境中最重要的元素是"智能"，但它仍然需要一個強大的硬體來在客戶端啟用和運行軟體。

選擇 vCPE 平台時有哪些注意事項？
- 與主要 VNF 和 SD-WAN 提供商進行先前驗證
- 兼容開放式架構的支持
- 機架式和台式機設備
- 端口可用性。LAN 端口，如 RJ45，光纖（SFP）和無線連接（WiFi、藍牙、LTE）
- 強大的內存、處理器和儲存

## What's Next
基於軟體的系統在虛擬化方面的爆炸式增長正在逐漸走向大規模的技術發展，特別是在採用通用邊緣計算設備方面。今天，MSP 不是讓硬體確定每個設備的功能範圍，而是透過 NFV 平台將必要的軟體推送到通用設備並調用它，即 vCPE。

相信 vCPE 是 NFV（網路功能虛擬化）的未來，因為它可以改善和加速服務交付。

vCPE 是 NFV 的最佳用例，SD-WAN 是 vCPE 的最佳用例。因此，我們可能會開始看到為 CPE 構建更多通用硬體，運行 VNF，以及將 SD-WAN 等網路功能推向分支機構的 MSP。

## Ref
[原文](https://www.lanner-america.com/blog/vcpe-virtualizing-cpe/)
[參考](https://www.sdnlab.com/23409.html)
