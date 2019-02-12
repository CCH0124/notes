# Choosing The Best VPN
VPN 服務的保護類型各不相同。雖然沒有針對所有用戶的單一"最佳" VPN，但個人用戶應確定哪種服務最適合他們。
## What’s the best VPN service ?
虛擬專用網絡（VPN）服務的成本和功能範圍各不相同。事實上，每個人都沒有**最佳** VPN 選項。將提供選擇 VPN 服務時應考慮的標準。

![](https://www.cloudflare.com/img/learning/security/threats/meltdown-spectre/speculative-execution.png)

## Beware of articles and blogs that recommend specific VPN services
正在網上搜索資源以幫助他們選擇 VPN 服務的用戶可能會遇到看起來很垃圾的網站，這些網站有十大列表或比較 VPN。其中許多頁面實際上是聯盟營銷人員運行的特定 VPN 的隱蔽廣告。用戶應該警惕任何使用不誠實策略與他們建立業務關係的隱私公司。

## Is a VPN service necessary to remotely access a home network ?
對於只希望能夠從遠端位置訪問其家庭網路的用戶，VPN 服務將無法滿足他們的需求。VPN 服務將用戶連接到由服務運營的遠端網路，而不是家庭網絡，為了能夠訪問家庭網路，用戶可以在家免費運行自己的 VPN。可以在線找到有關從家用計算機或路由器運行 VPN 服務器的說明。
## Choosing a VPN service for changing geolocation
如果用戶只是在尋找能夠讓他們改變地理位置的服務，那麼幾乎任何 VPN 都會這樣做，他們應該尋找價格最低的服務。例如：一個不關心安全性或隱私並且只是想為了觀看 Hulu 或其他流媒體服務而改變其 IP 位置的人將不需要來自更強大的 VPN 的許多安全功能，他們無需支付他們不會使用的額外功能。

![](https://www.cloudflare.com/img/learning/security/vpn/choosing-the-best-vpn/vpn-location.svg)

## Is the protocol a VPN service uses important? Is OpenVPN more secure than PPTP and L2TP/IPsec ?
VPN 服務使用的協定很重要，現代和安全的協定是適當的隱私和安全的要求。**OpenVPN 目前是黃金標準，它是最安全的 VPN 加密協定**。L2TP / IPsec（layer 2 tunneling protocol with IPsec encryption）也被認為是非常安全的，但它比 OpenVPN 慢得多。 PPTP（point-to-point tunneling protocol）是一種過時的協議，其加密不再安全，應避免任何僅提供 PPTP 的服務。

這裡唯一需要注意的是，一些 obile operating systems （移動操作系統）還不支持 OpenVPN。 對於許多移動設備，L2TP/IPsec 仍然是可用的最佳協議。

## Does a VPN need to provide concurrent connections ?
並發連接是另一個值得關注的好功能。大多數用戶都希望 VPN 服務允許多個並發連接，以便他們可以保護家庭中的每個設備。

## Does the size of a VPN’s user base matter ?
具有大量用戶群的 VPN 服務有助於增加該服務上用戶的隱私和匿名性。使用特定 VPN 服務的人越多，挑出一個人的活動就越困難。

![](https://www.cloudflare.com/img/learning/security/vpn/choosing-the-best-vpn/vpn-anonymity.svg)

## Which VPNs are best for bypassing restricted networks ?
受限網路後面的用戶將需要 VPN 服務的特殊功能。像中國的防火牆這樣受限制的網路內置了 VPN 阻止功能。SSH 隧道、TCP 端口 443 和多跳等技術可以幫助用戶克服這些限制。

## What is a VPN killswitch ?
一些 VPN 提供了一個 killswitch，以防止用戶在 VPN 斷開連接中斷時訪問Internet。這樣，用戶不會在沒有 VPN 保護的情況下意外訪問 Internet。這些 killswitch 是一個有用的功能，有幾種方法可以實現它們，一些 killswitch 只能關閉某些程序（如 Web 瀏覽器）的 Internet 流量，而更安全的 killswitch 軟體會關閉設備上的所有 Internet 活動。應該注意的是，沒有 killswitch 是 100% 安全，因為在 killswitch 啟用之前，仍然可以在未受保護的連接上交換某些數據包。

## Why do some VPNs suffer from DNS leaks ?
當用戶在沒有 VPN 的情況下連接到 Internet 時，他們使用其 ISP 的 DNS resolver，該 resolver 可以記錄有關其行為的數據。VPN 可以解決這個問題，但如果 VPN 服務使用其 ISP 的 DNS resolver 甚至公共 DNS resolver，則會造成數據洩漏。一個好的 VPN 服務將運行自己的私有 DNS 服務器，如果 VPN 沒有使用它自己的 DNS，用戶可以詢問是否使用的 DNS，是否針對安全性進行了優化，例如：1.1.1.1 DNS 服務，該服務在 24 小時後清除日誌。

## Does using 2 VPNs simultaneously increase protection ?
傳輸超敏感訊息的一些人使用兩種不同的 VPN，透過在已連接到第一個 VPN 的同時連接到第二個 VPN，它們會添加另一層安全性，他們的數據透過另一個加密隧道內的加密隧道傳輸。鏈接兩個 VPN 服務是一種極端的措施，但對於那些無法承受數據洩漏的人來說可能是一個合理的選擇。

## Are free VPN services too good to be true ?
有些 VPN 服務是免費的，雖然它們不提供與更強大的付費服務相同的功能集，但對於某些用戶來說，它們可能是一個不錯的選擇。免費 VPN 服務通常帶有 bandwidth 限制和使用限制，但只要它們符合良好 VPN 的其他標準，它們對於臨時用戶來說是一個不錯的選擇。

## Conclusion
選擇 VPN 服務時，上述功能都值得考慮。透過確定哪些功能對他們最重要，用戶可以利用本指南幫助他們選擇 VPN 以滿足他們的需求。