# VPN
虛擬專用網絡（VPN）允許用戶遠端訪問專用網路，以實現隱私和安全。
## What is a VPN ?
虛擬專用網絡（VPN）是一種 Internet 安全服務，允許用戶訪問 Internet，就好像它們連接到專用網絡一樣。其中加密了 Internet 通訊並提供了很強的匿名性。使用 VPN 的一些最常見的原因是防止窺探公共 WiFi，繞過 Internet 審查，或連接到企業的內部網路以進行遠端工作。

![](https://www.cloudflare.com/img/learning/security/vpn/what-is-a-vpn/what-is-a-vpn.svg)

## How does a VPN work ?
通常，大多數 Internet 流量都是**未加密**的並且**非常公開**。當用戶創建Internet 連接時，例如：訪問瀏覽器中的網站，用戶的設備將連接到他們的Internet Service Provider（ISP），然後 ISP 將連接到 Internet 以找到要與之通訊的相應 Web 服務器，獲取請求的網站。

在網站請求的每個步驟中都會公開有關用戶的訊息。由於用戶的 IP 地址在整個過程中都會暴露出來，因此 ISP 和任何其他`中間人`都可以記錄用戶的瀏覽習慣。另外，用戶設備和 Web 服務器之間**流動的數據是未加密**的，這為惡意攻擊者創造了窺探數據或對用戶進行攻擊的機會，例如：`man-in-the-middle attack`（中間人攻擊）。

相反，使用 VPN 服務連接到 Internet 的用戶具有更高級別的**安全性**和**隱私性**。 VPN 連接涉及以下 4 個步驟：
1. VPN 客戶端，使用加密連接連接到 ISP。
2. ISP 將 VPN 客戶端連接到 VPN 服務器，維護加密連接。
3. VPN 服務器解密來自用戶設備的數據，然後連接到 Internet 以透過未加密的通訊訪問 Web 服務器。
4. VPN 服務器與客戶端建立加密連線，稱為**VPN tunnel**。

VPN 客戶端和 VPN 服務器之間的 VPN tunnel 透過 ISP，但由於所有數據都已加密，因此 ISP 無法查看用戶的活動。VPN 服務器與 Internet 的通訊是未加密的，但 Web 服務器只會記錄 VPN 服務器的 IP 地址，這不會提供有關用戶的訊息。

>VPN 客戶端是安裝在用戶設備上的 VPN 軟體

## Is a VPN only for people with something to hide ?
與其他 Internet privacy services 一樣，VPN 有時被歸類為非法或顛覆活動的工具。事實是，使用 VPN 有許多有效和合理的理由。
以下是一些最常見的：
##### Protection over public WiFi
在沒有 VPN 的情況下使用公共 WiFi 網路的用戶正在冒險。他們的 Internet 流量未加密，同一網絡上的其他用戶可以使用易於訪問的工具監控他們的活動，這是攻擊者竊取登錄憑據和其他敏感信息的常用方法，如果用戶透過 VPN 連接，則窺探攻擊者將只能看到加密數據，這不會洩露任何敏感信息。
##### Remote work 
許多企業允許其員工使用 VPN 遠端工作。這可以允許遠端的員工訪問公司的內部網路，並提供加密以保護企業免受攻擊者或間諜活動的侵害。
##### Freedom from censorship in oppressive states
在世界的某些地方，禁止表達甚至閱讀批評政府的觀點。這些國家中的許多城市還向其公民提供阻止大量域名的受抑制的 Internet 版本。在這些國家訪問 Internet 的人可以使用 VPN 訪問其國家阻止的內容，也可以在線自由發言，因為 VPN 加密可以保護他們的活動免受國家監控。
##### Location anonymity 
某些 Web 服務將根據用戶的位置限製或過濾內容。VPN 可用於匿名用戶的位置並繞過這些限制。
##### The right to online privacy
眾所皆知，ISP 出售其使用的私人數據。同樣，一些網站會出售有關其訪客的訊息。VPN 服務提供的隱私使消費者能夠選擇不收集他們的數據。

## What are the downsides of a VPN ?
VPN 服務不保證提高安全級別，如果用戶信任 VPN 提供商，他們只能感到安全。一個不誠實的 VPN 提供商可以出售他們的用戶訊息或讓他們受到攻擊。值得注意的是，大多數 VPN 服務每月都會產生經常性費用。某些 VPN 用戶也可能遇到性能問題。

## How does a VPN affect performance ?
有些用戶會遇到來自 VPN 的性能下降，這在很大程度上取決於他們使用的 VPN 服務。並非所有 VPN 都是相同的，如果 VPN 服務沒有服務器空間來處理用戶創建的負載，那麼這些用戶的 Internet 連接速度將會減慢。此外，如果 VPN 與用戶和他們嘗試訪問的 Web 服務器距離很遠，則所產生的旅行時間（路由）可能會產生延遲。例如：如果舊金山的用戶正在訪問其服務器也在舊金山的網站，但該用戶的 VPN 服務位於東京，則用戶的請求必須在世界各地旅行並返回服務器之前返回距離酒店僅幾英里，這有時被稱為 **trombone effect**。

![](https://www.cloudflare.com/img/learning/cdn/glossary/ixp/trombone-effect.png)