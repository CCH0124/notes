# Memcached DDoS Attack
Memcached 可以加速網站，但是 memcached 服務器也可以被利用來執行 DDoS 攻擊。
## What is an memcached DDoS attack ?
memcached `distributed denial-of-service`（DDoS）攻擊是一種網路攻擊，攻擊者試圖透過 internet 流量使目標受害者overload。攻擊者 `spoofs` 了對易受攻擊的 UDP memcached 服務器的請求，然後服務器會利用 internet 流量癱瘓目標受害者，從而可能癱瘓受害者的資源。當目標的 Internet 基礎結構過載時，無法處理新請求，並且正常流量無法訪問 Internet 資源，從而導致 denial-of-service。

> Memcached 是一個用於加速網站和網路的數據庫緩存系統。

## How does a memcached attack work ?
Memcached 攻擊的操作類似於所有 DDoS amplification 攻擊，例如：NTP amplification 和 DNS amplification。攻擊的工作原理是向易受攻擊的服務器發送欺騙性請求，然後使用比初始請求更多的數據進行回應，從而放大流量。

Memcached amplification 可以被認為是在一個惡意的少年打電話給餐館並說：「我將擁有一切，請給我回電話並告訴我我的整個訂單」的背景下。當餐廳要求回撥號碼時，給出的號碼是目標受害者的電話號碼。然後，目標接收來自餐館的電話，其中包含許多他們未請求的信息。

這種放大攻擊方法是可行的，因為 memcached 服務器可以選擇使用 UDP 協定進行操作。UDP 是一種網路協定，允許在不獲得所謂的 handshark 的情況下發送數據，這是雙方同意通訊的網路過程，使用 UDP 是因為從不詢問目標主機是否願意接收數據，允許在未經他們事先同意的情況下將大量數據發送到目標。

memcached 攻擊分4個步驟發生：
1. 攻擊者在暴露的 memcached 服務器上植入大量有 payload。
2. 攻擊者利用目標受害者的 `IP address` 欺騙 `HTTP GET` 請求。
3. 接收請求的易受攻擊的 memcached 服務器（透過回應嘗試提供幫助）會向目標發送大量回應。
4. 目標服務器或其周圍的基礎結構無法處理從 memcached 服務器發送的大量數據，從而導致合法請求的 overload 和denial-of-service。

![](https://www.cloudflare.com/img/learning/ddos/memcached-ddos-attack/cloudflare-memcached-attack.png)cloudflare 提供

## How big can a memcached amplification attack be ?
這種攻擊的放大係數確實令人咋舌，在實踐中，我們目睹了高達 51,200 倍的放大因子！這意味著對於 15 byte 請求，可以發送 750 kB 回應。這代表了無法承受這一攻擊流量的網路屬性的巨大放大因素和安全風險。擁有如此大的放大係數和易受攻擊的服務器使得 memcached 成為尋求針對各種目標發起 DDoS 的攻擊者的主要用例。

## How can a memcached attack be mitigated ?
1. Disable UDP
    - 對於 memcached 服務器，請確保在不需要時禁用 UDP support。默認情況下，memcached 啟用了 UDP support ，可能會使服務器容易受到攻擊。
2. Firewall memcached servers
    - 透過對來自 Internet 的 memcached 服務器進行防火牆，系統管理員可以在必要時使用 UDP 進行 memcached 而無需暴露。
3. Prevent IP spoofing
    - 只要 IP 地址可以被欺騙，DDoS 攻擊就可以利用此漏洞將流量引導到受害者網路。防止 IP 欺騙是一種更大的解決方案，任何特定的系統管理員都無法實現，並且它要求轉接提供商不允許任何數據包離開其網路，其來源 IP 地址源自網路外部。換句話說，諸如 internet 服務提供商（ISP）之類的公司必須過濾流量，使得不允許離開其網路的 packet 假裝來自其他地方的不同網路。如果所有主要的運輸提供商實施這種類型的過濾，基於欺騙的攻擊將在一夜之間消失。
4. Develop software with reduced UDP responses
    - 消除放大攻擊的另一種方法是去除任何傳入請求的放大因子，如果作為 UDP 請求的結果發送的回應數據小於或等於初始請求，則不再能夠放大。

[memcrashed](https://blog.cloudflare.com/memcrashed-major-amplification-attacks-from-port-11211/)