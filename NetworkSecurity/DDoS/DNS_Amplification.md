# DNS Amplification Attack
DNS amplification 是一種 DDoS 攻擊，它利用 DNS resolvers 來癱瘓受害者的流量。

## What is a DNS amplification attack ?
這 DDoS 攻擊是一種基於反射的 volumetric  istributed denial-of-service （DDoS）攻擊，其中攻擊者利用開放的功能DNS resolvers 以癱瘓目標伺服器或讓網路流量增加，呈現伺服器和它周圍的基礎設施無法進入。

## How does a DNS amplification attack work ?
所有放大攻擊都利用了攻擊者和目標 Web 資源之間的 bandwidth 消耗差異。當在許多請求中放大成本差異時，由此產生的流量可能會破壞網路基礎設施。透過發送導致大量回應的小查詢，惡意用戶可以從更少的內容獲得更多。具有在每個機器人這個倍數乘以 botnet 進行類似的請求，攻擊者是從檢測既混淆和收穫大大提高了攻擊流量的好處。

DNS 放大攻擊中的一個機器人可以被認為是一個惡意的少年打電話給餐館並說：「我將擁有一切，請給我回電話並告訴我我的整個訂單。」當餐廳要求時一個回撥號碼，給出的號碼是目標受害者的電話號碼。然後，目標接收來自餐館的電話，其中包含許多他們未請求的信息。

由於每個機器人都要求使用 `spoofed IP address` 打開 DNS resolvers，該 IP 地址已更改為目標受害者的真實來源 IP 地址，然後目標會從 DNS resolvers 接收回應。為了創建大量流量，攻擊者以盡可能從 DNS resolvers 生成回應的方式構造請求。結果，目標接收到攻擊者初始流量的放大，並且他們的網路被虛假流量阻塞，導致 `denial-of-service`。

![](https://www.cloudflare.com/img/learning/ddos/dns-amplification-ddos-attack/dns-amplification-attack-1.png)

 DNS amplification 可分為四個步驟：
 1. 攻擊者使用受損端點將帶有欺騙性 IP 地址的 UDP 數據包發送到 DNS recursor。數據包上的欺騙地址指向受害者的真實IP地址。
 2. 每個 UDP 數據包都向 DNS resolvers 發出請求，通常會傳遞諸如 "ANY" 之類的參數，以便接收可能的最大響應。
 3. 在收到請求後，嘗試透過回應提供幫助的 DNS resolvers 會向欺騙的 IP 地址發送大量回應。
 4. 目標的 IP 地址接收回應，周圍的網路基礎設施因流量負擔過多而變得不堪重負，導致 denial-of-service。

 雖然一些請求不足以取消網路基礎設施，但當此序列在多個請求和 DNS resolvers 之間成倍增加時，目標接收的數據放大可能很大。

## How is a DNS amplification attack mitigated ?
對於運營網站或服務的個人或公司，緩解選項是有限的。因為個人的服務器雖然可能是目標，但卻不會感受到  volumetric 攻擊的主要影響，由於產生了大量流量，服務器周圍的基礎設施會產生影響，Internet 服務提供商（ISP）或其他上游基礎架構提供商可能無法處理傳入流量而不會變得不堪重負。因此，ISP 可能將所有流量 `blackhole` 到目標受害者的 IP 地址，保護自己並使目標站點脫機。

#### Reduce the total number of open DNS resolvers
DNS amplification 攻擊的一個重要組成部分是訪問開放式 DNS resolvers。透過將配置不當的 DNS resolvers 暴露給 Internet，攻擊者需要做的就是利用 DNS resolvers 來發現它。理想情況下，DNS resolvers 應僅向源自受信任區域的設備提供其服務。在基於 reflection  的攻擊的情況下，開放的 DNS resolvers 將回應來自 Internet 上任何地方的查詢，從而允許利用漏洞，限制 DNS resolvers 以使其僅回應來自可信來源的查詢使得伺服器成為任何類型的放大攻擊的不良工具。

#### Source IP verification – stop spoofed packets leaving network
由於攻擊者 botnet 發送的 UDP 請求必須具有欺騙受害者 IP 地址的源 IP 地址，因此降低基於 UDP 的放大攻擊有效性的關鍵組件是 Internet 服務提供商（ISP）拒絕任何內部流量欺騙的 IP 地址。如果從網路內部發送一個 packet，其源地址使其看起來像是在網路外部發起的，那麼它可能是一個欺騙性數據包，可以被丟棄。

[reflections](https://blog.cloudflare.com/reflections-on-reflections/)