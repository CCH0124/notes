# BGP Hijacking
BGP Hijacking 是 internet 流量的惡意重新路由，它利用了 internet 路由協定 BGP 的信任特性。

## What Is BGP Hijacking ?
BGP Hijacking 是攻擊者惡意重新路由 Internet 流量的時候。攻擊者透過錯誤的宣布他們實際上不擁有控製或路由到的 IP 地址組（稱為 IP prefixes）的所有權來實現此目的。如果有人要改變一段高速公路上的所有標誌並將汽車交通重新路由到不正確的出口，那麼 BGP hijack 很像。

![](https://www.cloudflare.com/img/learning/security/glossary/bgp-hijacking/bgp-hijacking-reroute.png)

因為 BGP 是建立在互聯網路，正在告訴他們擁有哪些 IP 地址的假設的基礎上的，所以 BGP hijack 幾乎是不可能停止的 - 想像一下，如果沒有人在看高速公路標誌，並且唯一的方法就是告訴他們是否是惡意的改變的是觀察到許多汽車最終落入錯誤的社區。但是，為了發生劫持，攻擊者需要控製或破壞在一個自治系統（AS）和另一個自治系統之間橋接的啟用 BGP 的路由器，因此不只是任何人都可以執行 BGP hijack。

## What is BGP ?
BGP 代表 Border Gateway Protocol，它是 Internet 的路由協定。換句話說，它提供了方向，以便流量盡可能高效地從一個 IP 地址傳輸到另一個 IP 地址。IP 地址是給定網站的實際 Web 地址。當用戶鍵入網站名稱並且瀏覽器找到並加載它時，請求和響應在用戶的 IP 地址和網站的 IP 地址之間來回傳遞。DNS（域名系統）服務器提供 IP 地址，但 BGP 提供了最有效的方式來訪問該 IP 地址。粗略地說，如果 DNS 是互聯網的地址簿，那麼 BGP 就是互聯網的路線圖。

每個 BGP 路由器存儲一個路由表，其中包含自治系統之間的最佳路由。由於每個 AS（通常是 Internet 服務提供商（ISP））廣播他們擁有的新 IP prefixes，因此幾乎不斷更新這些內容。BGP 總是傾向於從 AS 到 AS 的最短和最直接的路徑，以便通過網路中盡可能少的跳躍來到達 IP 地址。

##### 自治系統（AS）的定義
自治系統是由單個組織管理的大型網路或網路組。AS 可能有許多子網，但都共享相同的路由策略。通常，AS 是 ISP 或具有自己的網路的非常大的組織，以及從該網路到 ISP 的多個上游連接（這稱為"multihomed network"）。每個 AS 都分配有自己的自治系統編號（ASN），以便輕鬆識別它們。

## Why is BGP important ?
BGP 使得互聯網的大規模增長成為可能。互聯網由多個互連的大型網路組成。由於它是分散的，因此沒有管理機構或交通警察為數據包傳送到其預期的 IP 地址目的地舖設最佳路由。BGP 履行這一職責。如果不是 BGP，由於路由效率低，網路流量可能需要花費大量時間才能到達目的地，或者根本不會到達預定目的地。

## How can BGP be hijacked ?
當 AS 宣布它實際上不控制的 IP 前綴的路由時，該公告（如果未被過濾）可以傳播並被添加到 internet 網上的 BGP 路由器中的路由表。從那時起，直到有人注意到並糾正路由，這些 IP 的流量將被路由到該 AS。如果沒有當地政府來核實和執行財產契約，那就像要求領土一樣。

BGP 始終支持所需 IP 地址的最短，最具體的路徑。為了使BGP劫持成功，路由通知必須：

1. 透過宣布比其他 AS 先前宣布的更小範圍的 IP 地址來提供更具體的路由。

2. 為某些 IP 地址區塊提供更短的路由。此外，不只是任何人都可以宣佈到更大的互聯網的 BGP 路由。為了發生 BGP hijack，必須由 AS 的運營商或已經破壞 AS 的威脅行為者發布通知（第二種情況更為罕見）。

![](https://www.cloudflare.com/img/learning/security/glossary/bgp-hijacking/bgp-hijacking-technical-flow.png)

似乎令人驚訝的是，大型網路或網路組的運營商（其中許多是 ISP）會肆無忌憚地進行此類惡意活動。但考慮到現在全球有超過 80,000 個自治系統，有些人不值得信任也就不足為奇了。此外，BGP hijack 並不總是顯而易見或易於檢測。不良行為者可能會偽裝他們在其他 AS 之後的活動，或者可能會宣布未使用的 IP 前綴塊，這些前綴不太可能被注意到以便保持在雷達之下。

## What happens when BGP is hijacked ?
由於 BGP hijack，Internet 流量可能出錯，被監控或攔截，被 `black holed`，或者作為 `man-in-the-middle` 攻擊的一部分被導向虛假網站。此外，垃圾郵件發送者可以使用 BGP hijack 或實施 BGP hijack 的 AS 網路，以欺騙合法 IP 以進行垃圾郵件。從用戶的角度來看，頁面加載時間將會增加，因為請求和響應將不會遵循最有效的網路路由，甚至可能不必要地遍歷全世界。

在最佳情況下，流量只會佔用不必要的長路徑，從而增加延遲。在最糟糕的情況下，攻擊者可能正在進行中間人攻擊，或者將用戶重定向到虛假網站以竊取憑據。

## BGP hijacking in the real world
有許多關於故意 BGP hijack 的真實例子。例如，在2018年4月，俄羅斯提供商宣布了一些實際屬於 Route53 Amazon DNS 服務器的 IP 前綴（IP 地址組）。簡而言之，最終結果是嘗試登錄加密貨幣站點的用戶被重定向到由黑客控制的虛假版本的網站。因此，黑客能夠竊取大約 152,000 美元的加密貨幣。（為了更具體：透過BGP劫持，黑客劫持了亞馬遜DNS 查詢，以便 myetherwallet.com 的 DNS 查詢進入他們控制的服務器，返回錯誤的 IP 地址，並將 HTTP 請求定向到虛假網站。閱讀更多內容帖子：' [BGP洩密和加密貨幣](https://blog.cloudflare.com/bgp-leaks-and-crypto-currencies/)'。）
 
無意中發生 BGP hijack 事件也很普遍，它們可能對整個全球互聯網產生負面影響。2008年，巴基斯坦政府所有的巴基斯坦電信試圖通過更新其網站的BGP路線來審查巴基斯坦境內的 Youtube。看似意外，新航線被宣布給巴基斯坦電信的上游供應商，並從那裡播出到整個互聯網。突然之間，Youtube 的所有網絡請求都被導向了巴基斯坦電信，導致幾乎整個互聯網網站長達數小時的中斷，並使 ISP 無法接受。

## How can users and networks defend themselves from BGP hijacking ?
除了持續監控互聯網流量如何路由之外，用戶和網路可以做很少的事情來防止 BGP hijack。

##### IP prefix filtering
大多數網路應該只在必要時接受 IP 前綴聲明，並且只應將其 IP 前綴聲明到某些網絡，而不是整個 Internet。這樣做有助於防止意外的路由劫持，並可能使 AS 不接受偽造的 IP 前綴聲明。但是，在實踐中，這很難實施。

##### BGP hijacking detection
增加的延遲，降低的網路性能和錯誤的 Internet 流量都是 BGP hijack 的可能跡象。許多大型網絡將監控 BGP 更新，以確保其客戶不會遇到延遲問題，並且一些安全研究人員實際上會監控互聯網流量並發布他們的發現。

##### Making BGP more secure
BGP 是指在使 Internet 工作，它肯定會這樣做。但 BGP 的設計並未考慮安全性。整個互聯網（如 BGPsec）的安全路由解決方案正在開發中，但尚未採用它們。目前，BGP 具有內在的脆弱性，並將繼續存在。