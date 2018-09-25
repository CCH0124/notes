# Application Layer DDoS attacks
應用層 DDoS 攻擊以 Internet 的應用層為目標，以破壞網站或服務的正常流量。

## What is an Application Layer DDoS attack ?
Application layer 攻擊或第 7 層（L7）DDoS 攻擊是指一種惡意行為，只在針對 `OSI model `中的"頂層"，其中發生 `HTTP` GET 和 HTTP POST 等常見 Internet 請求與 network layer 攻擊（`DNS Amplification`）相比，這些第 7 層攻擊由於除了**網路資源之外還消耗了服務器資源**，因此特別有效。

## How do application layer attacks work ?
大多數 DDoS 攻擊的潛在有效性來自於發起攻擊所需的資源量與吸收或減緩攻擊所需的資源量之間的差異。雖然 L7 攻擊仍然存在這種情況，但影響目標服務器和網路的效率需要較少的總 bandwidth 才能達到相同的破壞性效果，application layer 攻擊會在總 bandwidth 減少的情況下造成更多損害。

為了探究為什麼會出現這種情況，讓我們來看看發出請求的客戶端和回應請求的 server 之間相對資源消耗的差異。當用戶發送登錄到 Gmail 帳戶等線上帳戶的請求時，用戶電腦必須使用的數據量和資源量是最小的，與檢查登錄憑據、加載相關用戶的過程中消耗的資源量不成比例來自數據庫的數據，然後發回包含所請求網頁的回應。

即使沒有登錄，很多時候從客戶端接收請求的 server 必須進行數據庫查詢或其他 API 調用才能生成網頁。如果由於許多設備針對單個網路資產（`botnet` 攻擊期間）而導致此差異被放大，則效果可能會使目標 server崩潰，從而導致拒絕為合法流量提供服務。在許多情況下，僅使用 L7 攻擊定位 API 就足以使服務 offline。

#### Why is it difficult to stop application layer DDoS attacks ?

區分攻擊流量和正常流量是困難的，特別是在應用層攻擊的情況下，例如 botnet 對受害者的 server 執行 HTTP Flood 攻擊。因為 botnet 中的每個 botnet 都會產生看似合法的網路請求，所以流量不會被欺騙，並且可能在原點顯得"正常"。

應用層攻擊需要一種自適應策略，包括根據特定規則集限制流量的能力，這些規則可能會定期波動。像是正確配置的 `WAF` 之類的工具可以減少傳遞到來源 server 的假流量，從而大大減少 DDoS 嘗試的影響。

對於像是 `SYN floods` 或 `NTP amplification` 的 reflection 攻擊之類的其它攻擊，如果網路本身具有接收它們的 bandwidth，則可以使用策略有效的丟棄流量。遺憾的是，大多數網路無法接收 300Gbps 的放大攻擊，甚至更少的網路也可以正確路由和服務 L7 攻擊可以生成的應用層請求量。

## What tactics help mitigate application layer attacks ?
一種方法是對發出網路請求的設備實施質詢，以便測試它是否是機器人。這是透過測試完成的，就像線上創建帳戶時常見的 CAPTCHA 測試一樣。透過提出像是 JavaScript 計算挑戰之類的要求，可以減輕許多攻擊。

阻止 HTTP flood 的其他途徑包括使用 Web 應用程式防火牆，透過 IP 信任數據庫管理和過濾流量，以及透過工程師的即時網路分析。