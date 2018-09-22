# NTP Amplification Attack
利用 NTP protocol 中的漏洞進行的 volumetric DDoS 攻擊，其目標是使伺服器癱瘓，用 UDP 流量。

## What is a NTP amplification attack ?
NTP amplification 攻擊是一種基於映射的 volumetric `distributed denial-of-service`（DDoS）攻擊，其中攻擊者利用網路時間協定（NTP）伺務器功能，以便用一定數量的 `UDP` 流量癱瘓目標網路或伺務器，使正常流量無法訪問目標及其周圍的基礎設施。

## How does a NTP amplification attack work ?
所有 amplification 攻擊都利用了攻擊者與目標 Web 資源之間的 bandwidth 成本差異，當在許多請求中放大成本差異時，由此產生的流量可能會破壞網路基礎設施，透過發送導致大量回應的小查詢，惡意用戶可以從更少的內容獲得更多，當具有在每個機器人這個倍數乘以 `botnet` 進行類似的請求，攻擊者是從檢測既混淆和收穫大大提高了攻擊流量的好處。

`DNS flood` 攻擊與 `DNS amplification` 攻擊不同。DNS amplification 攻擊反映和放大不安全的 DNS 服務器上的流量，以隱藏攻擊的來源並提高其有效性，DNS amplification 攻擊使用 bandwidth 較小的設備向不安全的 DNS 服務器發出大量請求，這些設備對非常大的 DNS 記錄提出了許多小請求，但在發出請求時，攻擊者偽造的返回地址是預期受害者的地址，**放大允許攻擊者僅使用有限的攻擊資源來獲取更大的目標**。

NTP amplification，就像 DNS amplification 一樣，可以被認為是一個惡意的少年打電話給餐館並說：「我將擁有一切，請給我回電話並告訴我我的整個訂單。」當餐廳要求時一個回撥號碼，給出的號碼是目標受害者的電話號碼。然後，目標接收來自餐館的電話，其中包含許多他們未請求的信息。

網路時間協定指在允許 Internet 連接的設備同步其內部時鐘，並在 Internet 體系結構中發揮重要作用。透過利用在某些 NTP 服務器上啟用的 *monlist* 命令，攻擊者能夠將其初始請求流量相乘，從而產生較大的回應。默認情況下，在舊設備上啟用此指令，並使用已對 NTP 服務器發出的請求的最後 600 個來源 IP address 進行回應，來自其 memory 中具有 600 個地址的服務器的 monlist 請求將比初始請求大 206 倍。這意味著擁有 1 GB  Internet 流量的攻擊者可以提供 200 GB 以上的攻擊 => 導致攻擊流量大幅增加。

NTP amplification 攻擊可以分為四個步驟：
1. 攻擊者使用 botnet 將帶有 `spoofed IP` 地址的 UDP packet 發送到啟用了 monlist 指令的 NTP 服務器。每個 packet 上的 spoofed IP 地址指向受害者的真實 IP 地址。
2. 每個 UDP packe 使用其 monlist 指令向 NTP 服務器發出請求，從而產生大量回應。
3. 服務器使用結果數據回應被欺騙的地址
4. 目標的 IP 地址接收回應，周圍的網路基礎設施因流量過大而變得不堪重負，導致 `denial-of-service`。

![](https://www.cloudflare.com/img/learning/ddos/ntp-amplification-ddos-attack/ntp-amplification-attack-ddos-attack-diagram-2.png)cloudflare

由於攻擊流量看起來像來自有效服務器的合法流量，因此很難在不阻止真正NTP 服務器合法活動的情況下減輕此類攻擊流量。由於 UDP packet 不需要handshark，因此 NTP 服務器將向目標服務器發送大量回應，而**不驗證請求是否可信**。這些事實與內置指令相結合，默認情況下發送大量回應，使NTP 服務器成為 DDoS amplification 攻擊的極好反射源。

## How is a NTP amplification attack mitigated ?
對於運營網站或服務的個人或公司，緩解選項是有限的。因為個人的服務器雖然可能是目標，但卻不會感受到  volumetric 攻擊的主要影響，由於產生了大量流量，服務器周圍的基礎設施會產生影響，Internet 服務提供商（ISP）或其他上游基礎架構提供商可能無法處理傳入流量而不會變得不堪重負。因此，ISP 可能將所有流量 `blackhole` 到目標受害者的 IP 地址，保護自己並使目標站點脫機。

#### Disable monlist - reduce the number of NTP servers which support the monlist command.
修補 monlist 漏洞的簡單解決方案是禁用該指令。默認情況下，版本 4.2.7 之前的所有版本的 NTP 軟體都是易受攻擊的，透過將 NTP 服務器升級到 4.2.7 或更高版本，該命令將被禁用，從而修補漏洞，如果無法升級，則按照 US-CERT 說明將允許服務器的管理員進行必要的更改。

#### Source IP verification – stop spoofed packets leaving the network.
由於攻擊者 botnet 發送的 UDP 請求必須具有欺騙受害者 IP 地址的源 IP 地址，因此降低基於 UDP 的 amplification 攻擊有效性的關鍵組合是 Internet 服務提供商（ISP）拒絕任何內部流量欺騙的 IP 地址。如果從網路內部發送一個數據包，其來源地址使其看起來像是在**網路外部發起的**，那麼它可能是一個欺騙性數據包，可以被丟棄。

在 NTP 服務器上禁用 monlist 並在目前允許 IP 欺騙的網路上實現入口過濾的組合是在到達其預期網路之前阻止此類攻擊的有效方法。