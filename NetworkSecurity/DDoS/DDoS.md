# DDoS
## DoS attack(Denial of Service attack)
- 耗盡提供服務伺服器的
    - 資源
    - 頻寬
- 藉由各種的攻擊手法,使資訊服務無法正常運作
## DDoS attack(distributed denial-of-service attack)
- 針對
    - 網路頻寬
    - 系統資源
- 由多重的來源進行攻擊,使資訊服務無法正常運作

## what is a ddos attack
分佈式拒絕服務（DDoS）攻擊是惡意破壞目標服務器，服務或網絡的正常流量的惡意嘗試，透過大量的 Internet 流量壓倒目標或其周圍基礎架構。DDoS 攻擊透過利用多個受損電腦系統作為攻擊流量來實現有效性。被利用的機器可以包括計算機和其他網絡資源，例如物聯網設備。從高層次來看，DDoS 攻擊就像高速公路堵塞，阻止正常流量到達預期目的地。

## How does a DDoS attack work?
DDoS 攻擊要求攻擊者取得對線上主機網路的控制權以執行攻擊。電腦主機和其他機器（例如物聯網設備）感染了`malware`，將每個機器人變成`bot`(zombie)。然後，攻擊者可以遠程控制一群殭屍程序，這就是所謂的` botnet`。
botnet 建立之後，攻擊者就可以藉由遠端控制的方式向每個機器發送更新的指令，從而掌控機器。當受害者的 `IP address` 是 botnet 的目標時，每個 bot 程序都會藉由向目標發送請求進行響應，可能導致目標 server 或 network to overflow，從而導致正常流量 ` denial-of-service`。由於每個 bot 都是合法的網路設備，因此將攻擊流量與正常流量分開可能很困難。

## What are common types of DDoS attacks?
不同的 DDoS 攻擊向量針對網絡連接的不同組件。為了了解不同的DDoS攻擊如何工作，有必要了解如何建立網絡連接。Internet 上的網路連接由許多不同的組件或"layer"組成。像從頭開始建造房屋一樣，模型中的每一步都有不同的目的。下面顯示的 `OSI model` 是用於描述 7 個不同層中的網路連接的概念框架。

![OSI model](https://www.cloudflare.com/img/learning/ddos/what-is-a-ddos-attack/osi-model-7-layers.svg)

雖然幾乎所有的 DDoS 攻擊都涉及壓倒目標設備或帶有流量的網絡，但攻擊可分為三類。攻擊者可能會使用一個或多個不同的攻擊向量，或者可能基於目標所採取的對策措施來循環攻擊向量。

## Application Layer Attacks

#### The Goal of the Attack:
有時被稱為第7層 DDoS 攻擊（參照OSI模型的第7層），這些攻擊的目標是**耗盡目標資源**。攻擊的目標是在服務器上產生網頁並 response ，`HTTP` requests 而傳遞的應用層。單個 HTTP requests 在客戶端執行起來很簡易，並且目標服務器 response 可能很麻煩，因為服務器通常必須加載多個文件並運行 Database 查詢才能創建網頁。`第7層`攻擊難以防禦，因為流量很難被標記為惡意攻擊。

#### Application Layer Attack Example:
![](https://i.imgur.com/YQmo5yY.png)
來源 cloudflare

#### HTTP Flood
此攻擊類似於同時在多個不同主機上反覆按 Web 瀏覽器中的刷新 - 大量 HTTP 請求氾濫服務器，導致 denial-of-service。
這種類型的攻擊範圍從簡單到復雜。更簡單的實現可以訪問具有相同範圍的攻擊 IP addreaa，引用者和用戶代理的一個URL；複雜版本可能使用大量攻擊性 IP address，並使用隨機引用和用戶代理來定位隨機 URL。
[HTTP flood](https://www.wikiwand.com/en/HTTP_Flood)

## Protocol Attacks
#### The Goal of the Attack:
Protocol Attacks（也稱為狀態耗盡攻擊）藉由消耗 Web application 服務器或 firewalls 和 load balancers 等中間資源的所有可用狀態表容量來導致服務中斷。

#### Protocol Attack Example:
![](https://i.imgur.com/mU7qEa2.png)
來源 cloudflare
#### SYN Flood
`SYN Flood` 類似於接收來自商店前面的請求的供應室中的工作人員，工作人員收到請求，去取得包裹，並在將包裹拿出前等待確認。然後，工作人員在沒有確認的情況下又獲得更多的包裹請求，直到他們無法攜帶更多的包裹，變得不堪重負，並且請求開始無人接收。

此攻擊藉由向目標發送具有`spoofed`來源 IP address 的大量 TCP "Initial Connection Request" SYN 數據包，來利用 `TCP handshake`。目標機器response 每個連接請求，然後等待 handshake 中的最後一步，但這一步從未發生過，耗盡了 process 中的目標資源。

## Volumetric Attacks
#### The Goal of the Attack:
此類攻擊試圖通過消耗目標與較大 Internet 之間的所有可用頻寬來造成擁塞。藉由使用放大形式或其他造成大量流量的方式（來自 botnet 的請求）將大量數據發送到目標。
#### Amplification Example:
![](https://i.imgur.com/SySGDdx.png)
來源 cloudflare
#### DNS Amplification
`DNS Amplification` 就像有人打電話到餐館並說"I’ll have one of everything, please call me back and tell me my whole order"，其中他們給出的回撥電話號碼是目標號碼。只需很少的功夫，就會產生很長的響應。
藉由向具有 spoofed IP address （目標的真實IP address）的開放 DNS 服務器發出 request，目標 IP address 從服務器接收 response。攻擊者構造請求，以便 DNS 服務器以大量數據回應目標。結果，目標接收到攻擊者初始查詢的放大。

## What is the process for mitigating a DDoS attack?
減輕 DDoS 攻擊的關鍵問題是區分攻擊和正常流量。例如，如果產品發布的公司網站被正常的客戶瀏覽網頁流量所淹沒，那麼切斷所有流量是錯誤的。如果該公司突然出現來自已知不良行為者的流量爆增，則可能需要努力減輕攻擊。困難在於它將真實客戶和攻擊流量區分開來。

在現代互聯網中，DDoS 流量有多種形式，設計的流量可以從 un-spoofed 的單一來源攻擊到復雜的自適應 multi-vector 攻擊，multi-vector DDoS 攻擊使用多個攻擊路徑以不同方式淹沒目標，可能會分散任何一條軌跡上的緩解措施，同時針對 protocol stack 的多個層的攻擊，例如與 HTTP flood (targeting layer 7) 耦合的 DNS amplification (targeting layers 3/4) 是 multi-vector DDoS 的示例。

減輕 multi-vector DDoS 攻擊需要各種策略以對抗不同的軌跡。一般來說，攻擊越複雜，流量越難以與正常流量分離 - 攻擊者的目標是盡可能的混合，使緩解盡可能無法較有效率。牽涉不加選擇的丟棄或限制流量的緩解嘗試可能會誤判適當正常流量，並且攻擊也可以修改和適應以規避對策。為了克服複雜的破壞嘗試，分層解決方案將帶來最大的好處。

#### Black Hole Routing
實際上所有網路管理員都可以使用的一種解決方案是創建`blackhole route`並將流量匯集到該路由中，在最簡單的形式中，當在沒有特定限制標準的情況下實施blackhole route 過濾時，合法和惡意網路流量都被路由到空路由或黑洞並從網路中丟棄。如果 Internet 相關的服務遇到 DDoS 攻擊，則該服務的Internet 服務提供商（ISP）可能會將所有站點的流量發送到 blackhole 作為防禦。
#### Rate Limiting
限制服務器在特定時間窗口內接受的請求數量也是減輕拒絕服務攻擊的一種方法，雖然速率限制有助於減緩 Web Scraper 竊取內容和減少爆力登錄嘗試，但僅憑它可能不足以有效的處理複雜的 DDoS 攻擊。然而，速率限制是有效 DDoS 緩解策略中的有用組件。

#### Web Application Firewall
`Web Application Firewall`（WAF）是一種可以幫助減輕第 7 層 DDoS 攻擊的工具。透過在 Internet 和來源服務器之間放置 WAF，WAF 可以充當 reverse proxy，保護目標服務器免受某些類型的惡意流量的影響，透過基於用於識別 DDoS 工具的一系列規則過濾請求，可以阻止第 7 層攻擊，有效 WAF 的一個關鍵值是能夠快速實施自定義規則以回應攻擊。

#### Anycast Network Diffusion
此緩解方法使用 Anycast Network 將攻擊流量分散到分佈式服務器網路中，直至網路吸收流量，就像將湍急的河流引入不同的較小矩到一樣，這種方法可以將分佈式攻擊流量的影響擴展到可管理的程度，從而擴散任何破壞性功能。

`Anycast Network` 緩解 DDoS 攻擊的可靠性取決於攻擊的大小以及網絡的規模和效率。

