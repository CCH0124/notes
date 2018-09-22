# HTTP Flood Attack
HTTP flood 攻擊是一種 volumetric distributed denial-of-service（DDoS）攻擊，只在透過 HTTP 請求癱瘓目標服務器。
## What is an HTTP flood DDoS attack ?
HTTP flood 攻擊是一種 `volumetric distributed denial-of-service（DDoS）`攻擊，只在透過 `HTTP request` 癱瘓目標服務器。一旦目標已滿足請求並且無法回應正常流量，則對於來自實際用戶的其他請求將發生 `denial-of-service`。

![](https://www.cloudflare.com/img/learning/ddos/http-flood-ddos-attack/http-flood-attack.png)

## How does an HTTP flood attack work ?
HTTP flood 攻擊是一種第 7 層 DDoS 攻擊。第 7 層是 `OSI model`的應用層，並且指的是諸如 HTTP 之類的 Internet 協定。HTTP 是基於瀏覽器的Internet 請求的基礎，通常用於加載網頁或透過 Internet 發送表單內容。減輕應用層攻擊特別複雜，因為惡意流量很難與正常流量區分開來。

為了實現最高效率，惡意行為者通常會使用或創建 `botnets`，以最大限度的發揮其攻擊的影響。透過利用許多受 malware 感染的設備，攻擊者可以透過發起更大量的攻擊流量來利用他們的力量。

#### There are two varieties of HTTP flood attacks：
1. HTTP GET attack
在這種形式的攻擊中，協調多台計算機或其他設備以從目標服務器發送對圖像，文件或某些其他資產的多個請求。當目標被傳入的 requests 和 responses 癱瘓時，來自合法流量源的其他請求將發生 denial-of-service。
2. HTTP POST attack
通常在網站上提交表單時，服務器必須處理傳入的請求並將數據推送到持久層，通常是 DB。與發送 POST 請求所需的處理能力和 bandwith 量相比，處理表單數據和運行必要的 DB 命令的過程相對密集。此攻擊利用相對資源消耗的差異，透過將許多發布請求直接發送到目標服務器，直到其流量飽和並發生denial-of-service。

## How can an HTTP flood be mitigated ?
如前所述，減輕第 7 層攻擊是複雜的，通常是多方面的。一種方法是對請求機器實施挑戰，以便測試它是否是 bot，就像線上創建帳戶時常見的驗證碼測試一樣。通過提出諸如 JavaScript 計算挑戰之類的要求，可以減輕許多攻擊。

阻止 HTTP flood 的其他途徑包括使用 Web 應用程序防火牆（WAF），管理IP 信譽數據庫以跟蹤和有選擇地阻止惡意流量，以及工程師的即時分析。