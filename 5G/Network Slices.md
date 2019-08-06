## 5G 與網路切片
此技術可以讓運營商在一個硬體基礎設施切分出多個虛擬的端到端網路，每個網路切片從設備到接入網到傳輸網再到核心網在邏輯上隔離，適配各種類型服務的不同特徵需求。

對於每一個網路切片，像虛擬服務器、網路頻寬、服務質量等專屬資源都得到充分保證。由於切片之間**相互隔離**，所以一個切片的錯誤或故障不會影響到其它切片的通訊。

## 為何 5G 需要網路切片
在 5G 時代，移動網路需要服務各種類型和需求的設備。大家提的比較多的應用場景包括移動寬帶、大規模物聯網、任務關鍵的物聯網。他們都需要不同類型的網絡，在移動性、計費、安全、策略控制、延時、可靠性等方面有各不相同的要求。

![](https://i.imgur.com/oKDsFLW.png)


可以透過網絡切片技術在一個獨立的物理網路上切分出多個邏輯的網路，這是一個非常節省成本的做法

![](https://i.imgur.com/pnW058m.png)


![](https://i.imgur.com/e9AZPnT.png "NGMN 5G 白皮書")

## 如何實現端到端網絡切片
1. 5G 的無線接入網路和核心網：NFV 化

下圖1，RAN（DU 和 RU）和核心功能由 RAN 廠商提供的專用網路設備構建。為了實現網路切片，網路功能虛擬化（NFV）是一個先決條件。
下圖2，NFV 主要是將網路功能軟體（即分組核心中的 MME，S/P-GW 和 PCRF 以及 RAN 中的 DU）全部部署在商業服務器上的虛擬機，而不是單獨部署在其專用網路設備。這樣，RAN 當作 edge Cloud，而核心功能當作 core colud。位於 edge 和 core 雲中的 VM 之間的連接使用 SDN 進行配置。
下圖3，然後，為每個服務（即電話切片、大規模物聯網切片、任務關鍵的物聯網切片等等）創建切片。

![](https://i.imgur.com/RFZtcBB.png)

下圖顯示了每個服務專用的應用程序如何可以虛擬化並安裝在每個切片中。例如，切片可以配置如下

1. UHD 切片（UHD slice）：在 edge cloud 中虛擬化 DU、5G 核心（UP）和緩存服務器，以及在 core cloud 中虛擬化 5G 核心（CP）和 MVO 服務器
2. 電話切片（phone slice）：在 core cloud 中虛擬化具有全移動功能的 5G 核心（UP 和 CP）和 IMS 服務器
3. 大規模物聯網切片（Massive IoT slice (e.g., sensor network)）：在 core cloud 中虛擬化一個簡單輕便的 5G 內核沒有移動性管理功能
4. 任務關鍵的物聯網切片（Mission-critical IoT slice）：在 edge cloud 中虛擬化 5G 核心（UP）和相關服務器（例如 V2X 服務器），用於最小化傳輸延遲

到目前為止，需要為具有不同要求的服務創建專用切片。並且根據不同服務特性將虛擬網路功能放置在每個切片中的不同位置（即 edge cloud 或 core cloud）。此外，一些網路功能例如策略控制等，在某些切片中可能是必要的，但在其他網路切片中不是必需的。**運營商可以按照他們想要的方式定製網路切片**，而且可能是最具成本效益的方式。

![](https://i.imgur.com/A9Ufzs6.png "How to slice network")

到目前為止，這是 NFV 的工作。那麼，SDN 在網絡切片中扮演什麼角色？

2. Edge 和 Core clouds 之間的網路切片：IP/MPLS-SDN

軟件定義網絡，儘管在首先介紹的時候是一個很簡單的概念，但現在變得越來越複雜。就以 Overlay 形式為例，**SDN 技術能夠在現有的網路基礎設施上提供虛擬機間的網路連接**。

![](https://i.imgur.com/6eR2dXh.png "E2E network slicing")

首先我們看如何保證 edge cloud 與 core cloud 的虛擬機間的網路連接是安全的，虛擬機間的網路需要基於 IP/MPLS-SDN 和 Transport SDN 來實現。本文我們主要討論路由器廠商提供的 IP/MPLS-SDN。 Ericsson 和 Juniper 都提供 IP/MPLS SDN 網路架構產品。操作有些許不同，但基於 SDN 的虛擬機間的連接是極其相似的。
在 core cloud 中是虛擬化服務器。在服務器的管理程序中，運行內置的 vRouter/vSwitch。SDN 控制器提供虛擬化服務器和 DC G/W 路由器（cloud 數據中心中創建 MPLS L3 VPN 的 PE 路由器）間的隧道配置，在 core cloud 中的每個虛擬機（例如 5G IoT 核心）和 DC G/W 路由器間創建 SDN 隧道（即 MPLS GRE 或 VXLAN）。
然後，由 SDN 控制器管理這些隧道和 MPLS L3 VPN（例如 IoT VPN）之間的映射。該過程在 edge cloud 中也是一樣，創建一個物聯網切片從 edge cloud 連接到 IP/MPLS 骨幹，並一直到 core cloud。這個過程可以基於目前為止成熟可用的技術和標準來實現。


3. Edge cloud 與 cell 站點 RU 之間的網路切片

現在剩下的是移動前傳（fronthaul）網絡。如何在 edge cloud 和 5G RU 之間切割這個移動前傳網路？首先，必須首先定義 5G 前傳網路。大家在討論中存在一些選擇（例如通過重新定義 DU 和 RU 的功能來引入新的基於分組的前傳網路），但是還沒有做出標准定義。
下圖是在 ITU IMT 2020 工作組中給出的圖示，並給出了虛擬化前傳網絡的示例。

![](https://i.imgur.com/H0nHVup.png "Example of 5G C-RAN network slicing by ITU [Source: Report on Standards Gap Analysis, ITU, Focus groups on IMT-2020, Oct. 2015]")

5G 時代的網路切片仍在形成，關注和問題仍未得到解決。

## Ref
[netmanias](https://www.netmanias.com/en/?m=view&id=blog&no=8325)
