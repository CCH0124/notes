# Cross-Site Scripting
Cross-sites scripting（跨站點腳本）攻擊會欺騙 Web 瀏覽器運行惡意代碼。

## What is cross-site scripting ?
跨站點腳本（XSS）是一種攻擊，攻擊者將代碼附加到合法網站上（可以透過多種方式插入惡意代碼），該網站將在受害者加載網站時執行。最常見的是，它可以添加到 URL 的末尾，也可以直接發佈到顯示用戶生成內容的頁面上。**在更多技術術語中，跨站點腳本是客戶端代碼注入攻擊**。

![](https://www.cloudflare.com/img/learning/security/threats/cross-site-scripting/xss-attack.png)

## What is client-side code ?
客戶端代碼是在用戶計算機上運行的 JavaScript 代碼。就網站而言，客戶端代碼通常是在瀏覽器加載網頁後由 Web 瀏覽器執行的代碼，這與服務器端代碼形成對比，服務器端代碼在主機的 Web 服務器上執行。客戶端代碼對於交互式網頁非常有用，交互式內容運行得更快，更可靠，因為每次進行交互時，用戶的計算機都不必與 Web 服務器通訊。基於瀏覽器的遊戲是客戶端代碼的一個流行平台，因為無論連接問題如何，客戶端代碼都可以確保遊戲順利運行。

運行客戶端的代碼在現代 Web 開發中非常流行，並且在大多數現代網站上使用。由於跨站點代碼是現代 Web 的主要內容，跨站點腳本已成為最常報告的網絡安全漏洞之一，並且跨站點腳本攻擊已經觸及 YouTube、Facebook 和 Twitter 等主要站點。

## What is an example of cross-site scripting ?
跨站點腳本攻擊的一個有用示例通常出現在具有**未經驗證**的評論論壇的網站上。在這種情況下，攻擊者將發布包含在 `<script> </script>` 標籤中的可執行代碼的註釋。這些標籤告訴 Web 瀏覽器將標籤之間的所有內容解釋為JavaScript 代碼。一旦該評論出現在頁面上，當任何其他用戶加載該網站時，腳本標籤之間的惡意代碼將由其 Web 瀏覽器執行，並且它們將成為攻擊的受害者。

## How can an attacker use cross-site scripting to cause harm ?
JavaScript 跨站點腳本攻擊很受歡迎，因為 JavaScript 可以訪問一些可用於身份盜用和其他惡意目的的敏感數據。例如，JavaScript 可以訪問 cookie *，攻擊者可以使用 XSS 攻擊來竊取用戶的 cookie 並在線上模擬它們。JavaScript 還可以創建 HTTP 請求，這些請求可用於將數據（例如：被盜的cookie）發送回攻擊者。此外，客戶端 JavaScript 還可以幫助攻擊者訪問包含地理位置坐標，網路攝像頭數據和其他敏感信息的 API。

典型的跨站點腳本攻擊流程如下：
1. 受害者加載網頁，惡意代碼複製用戶的 cookie
2. 然後，代碼向攻擊者的網路服務器發送 HTTP 請求，其中包含請求 body 中的被盜 cookie。
3. 攻擊者可以使用這些 cookie 來冒充該網站上的用戶，以進行社交工程攻擊，甚至可以訪問銀行帳號或其他敏感數據。

Cookie是保存在用戶計算機上的臨時登錄憑據。例如，當用戶登錄 Facebook 這樣的網站時，該網站會為他們提供一個 cookie，這樣如果他們關閉瀏覽器窗口並在當天晚些時候返回 Facebook，他們會自動通過 cookie 進行身份驗證，無需再次登錄。

## What are the different types of cross-site scripting ?
兩種最常見的跨站點腳本攻擊類型反映了跨站點腳本和持久的跨站點腳本。

##### Reflected cross-site scripting
這是最常見的跨站點腳本攻擊。透過反射攻擊，惡意代碼被添加到網站的 URL 的末尾，通常這是一個合法、值得信賴的網站，當受害者在其 Web 瀏覽器中加載此鏈接時，瀏覽器將執行注入 URL 的代碼。攻擊者通常使用某種形式的 `social engineering` 來欺騙受害者點擊鏈接。

例如，用戶可能會收到聲稱來自其銀行的看似合法的電子郵件。該電子郵件將要求他們在銀行網站上採取一些行動，並提供鏈接。該鏈接可能最終看起來像這樣：

`http://legitamite-bank.com/index.php?user=&script>here is some bad code!</script>`

儘管 url 的第一部分看起來很安全並且包含受信任網站的域，但注入到 url 末尾的代碼可能是惡意的。

##### Persistent cross-site scripting
這種情況發生在允許用戶發布其他用戶將看到的內容的網站上，例如：評論論壇或社交媒體網站。如果站點未正確驗證用戶生成內容的輸入，則攻擊者可以插入其他用戶的瀏覽器在頁面加載時將執行的代碼。例如，攻擊者可能會去一個在線約會網站，可能會在他們的個人資料中添加這樣的內容：
`"Hi! My name is Dave, I enjoy long walks on the beach and <script>malicious code here</script>"`

任何試圖訪問 Dave 配置文件的用戶都將成為 Dave 持久的跨站點腳本攻擊的受害者。

## How to prevent cross-site scripting
沒有單一的策略來減輕 cross-site scripting，不同類型的 Web 應用程序需要不同級別的保護。可以採取一些保護措施，下面概述幾個。

- If possible, avoiding HTML in inputs 
    - 避免持久性 cross-site scripting 攻擊的一種非常有效的方法是阻止用戶將 HTML 發佈到表單輸入中。還有其他選項可以讓用戶在不使用 HTML 的情況下創建豐富的內容，例如 markdown 和 WYSIWYG 編輯器。
- Validating inputs 
    - 驗證意味著實施規則，防止用戶將數據發佈到不符合特定條件的表單中。例如，要求用戶的"姓氏"的輸入應具有僅允許用戶提交由字母數字字符組成的數據的驗證規則。驗證規則也可以設置為拒絕跨站點腳本中常用的任何標籤或字符，例如 `<script>` 標籤。
- Sanitizing data 
    - 清理數據類似於驗證，但它發生在數據已經發佈到 Web 服務器之後，但仍然在顯示給另一個用戶之前。有幾種在線工具可以清理 HTML 並過濾掉任何惡意代碼注入。
- Taking cookie security measures
    - Web 應用程序還可以為其 cookie 處理設置特殊規則，以透過跨站點腳本攻擊來緩解 cookie 竊取。Cookie 可以綁定到特定的 IP 地址，以便跨站點腳本攻擊者無法訪問它們。此外，可以創建規則來阻止 JavaScript 完全訪問 cookie。
- Setting WAF rules
    - 還可以將 WAF 配置為強制執行規則，以防止 reflected cross-site scripting。
    - 這些 WAF 規則採用的策略將阻止對服務器的奇怪請求，包括 cross-site scripting 攻擊。