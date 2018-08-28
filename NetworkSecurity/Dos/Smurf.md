# Smurf Attack
## What is a Smurf attack ?
Smurf 攻擊是一種 `distributed denial-of-service`（DDoS）攻擊，攻擊者試圖藉由 `Internet Control Message Protocol`（ICMP）packet 來癱瘓目標服務器。藉由向一個或多個計算機網路發出具有目標設備的 spoofed IP 地址請求計算機網路然後回應目標服務器，放大初始攻擊流量並可能使目標被癱瘓，使得無法訪問。此攻擊向量通常被視為已解決的漏洞，不再流行。

## How does a Smurf attack work ?
雖然 ICMP 數據包可用於 DDoS 攻擊，但通常它們在網路管理中提供有價值的功能。ping 應用程序利用 ICMP 數據包，網絡管理員使用它來測試網絡硬體設備，如：計算機，列表機或路由器。ping 通常用於查看設備是否正常運行，以及跟蹤訊息從來源設備到目標並返來源的往返時間。由於 ICMP protocol 不包括 **handshark**，因此接收請求的硬體設備無法驗證請求是否合法。

這種類型的 DDoS 攻擊可以被認為是一個調用辦公室經理並偽裝成公司首席 CEO 的惡作劇者。這個惡作劇者要求經理告訴每個員工用他的私人號碼打電話給行政人員，並告訴他們他們如何做的最新情況。惡作劇者給出目標受害者的回叫號碼，然後接收與辦公室裡的人一樣多的不必要的電話。

Smurf 攻擊的工作原理：
1. Smurf mawlare 構建一個欺騙性數據包，其來源地址設置為目標受害者的真實 IP 地址。
2. 將 packet 發送到路由器或防火牆的 IP broadcast 地址，路由器或防火牆又廣播網路內的每個主機設備地址發送請求，從而增加網路上連網設備數量的請求數量。
3. 網路內的每個設備接收來自發送 broadcast 的設備請求，然後用 ICMP Echo Reply packet 回應目標的欺騙地址。
4. 目標受害者收到大量 ICMP Echo Reply packet，可能會變得不堪重負，導致合法流量的 denial-of-service。

## How can a Smurf attack be mitigated ?
該漏洞在很大程度上被認為是已經解決的。在有限數量的遺留系統上，仍可能需要應用緩解技術。一個簡單的解決方案是在每個網路路由器和防火牆上禁用 IP broadcast 地址。較舊的路由器可能默認啟用廣播，而較新的路由器可能已禁用廣播。