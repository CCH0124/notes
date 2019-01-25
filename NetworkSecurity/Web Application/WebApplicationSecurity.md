## What is Web Application Security ?
- Web 應用程序安全性是任何基於 Web 的業務的核心組件。
- Internet 的全球性使網路資產暴露於來自不同地點以及各種規模和復雜程度的攻擊。
- Web 應用程序安全性專門處理網站環境，Web 應用程序和 Web 服務（如 API）的安全性。

## What are common web app security vulnerabilities ?

針對 Web 應用程序的攻擊範圍從目標資料庫操作到大規模網路中斷。
以下探討一些常見的攻擊方法或常用的"cevtors"。

- Cross site scripting (XSS) 
    - XSS 是一個漏洞，允許攻擊者將用戶端 script 注入網頁，以便直接訪問重要信息，冒充用戶或誘騙用戶洩露重要信息。
- SQL injection (SQi)
    - SQi是一種攻擊者利用資料庫執行**搜索查詢**的方式利用漏洞的方法。
    - 攻擊者使用 SQi 訪問未經授權的信息，修改或創建新的用戶權限，或以其他方式操縱或破壞敏感數據。
- Denial-of-service (DoS) and distributed denial-of-service (DDoS) attacks 
    - 通過各種 "vectors"，攻擊者可以使用不同類型的攻擊流量使目標服務器或其周圍的基礎架構 overload 。
    - 當服務器不再能夠有效地處理傳入請求時，它開始表現緩慢並最終拒絕來自合法用戶的傳入請求的服務。
- Memory corruption
    - 當 memory 中的某個位置被無意修改時，會發生 memory 損壞，從而導致軟體中出現意外行為。
    - 糟糕的人將試圖透過諸如 **code injections** 或 **buffer overflow** 攻擊等漏洞利用，來嗅出並利用 memory 損壞。
- Buffer overflow 
    - 當軟體將數據寫入稱為 buffer 的 memory 中的已定義空間時，Buffer overflow 是一種異常現象。
    - 溢出 buffer 的記憶體位址大小會導致相鄰的存儲器位置被數據覆蓋。
    - 可以利用此行為將惡意代碼注入內存，從而可能在目標計算機中創建漏洞。
- Cross-site request forgery (CSRF)
    - 跨站點請求偽造涉及欺騙受害者提出利用其**身份驗證**或**授權的請求**。
    - 透過利用用戶的帳戶權限，攻擊者能夠發送偽裝成用戶的請求，一旦用戶的帳戶遭到入侵，攻擊者就可以洩露，破壞或修改重要信息。管理員或高級管理人員等高權限帳戶通常都是針對性的。
- Data breach
    - 與特定攻擊媒介不同，數據洩露是指敏感或機密信息發布的一般術語，可以透過**惡意行為**或**錯誤**發生。
    - 被視為數據洩​​露的範圍相當廣泛，可能包含一些高價值的記錄，一直到數百萬個公開的用戶帳戶。

## What are the best practices to mitigate vulnerabilities ?
保護 Web 應用程序免受攻擊的重要步驟包括使用**最新加密**，需要**正確的身份驗證**，**不斷修補已發現的漏洞以及良好的 software development hygiene**。

現實情況是，即使在相當強大的安全環境中，聰明的攻擊者也許能夠發現漏洞，並建議採用整體安全策略。

通過防禦 DDoS、Application 層和 DNS 攻擊，可以提高 Web 應用程序的安全性：

##### WAF - Protected against Application Layer attacks
Web 應用程序防火牆或 WAF 有助於保護 Web 應用程序免受惡意 HTTP 流量的侵害。透過在目標服務器和攻擊者之間放置過濾屏障，WAF 能夠防止 CSRF、XSS 和SQL injection 等攻擊。

![](https://www.cloudflare.com/img/learning/ddos/glossary/waf/waf.png)

##### DDoS mitigation
破壞 Web 應用程序的常用方法是使用 distributed denial-of-service 或DDoS 攻擊。

![](https://www.cloudflare.com/img/learning/ddos/what-is-a-ddos-attack/ntp-amplification-botnet-ddos-attack.png)

##### DNS Security - DNSSEC protection
Domain name system 或 DNS 是 Internet 的電話簿，表示像是 Web 瀏覽器之類的 Internet 工具查找正確服務器的方式。
壞人將試圖透過 DNS cache poisoning、man-in-the-middle attacks 和其他干擾 DNS 查詢生命週期的方法來劫持此 DNS 請求流程。
如果 DNS 是 Internet 的電話簿，則 DNSSEC 是不可信的呼叫者 ID。