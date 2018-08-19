## What is HTTP ?
Hypertext Transfer Protocol（HTTP）是 World Wide Web 的基礎，用於使用 hypertext links 加載網頁，HTTP 是一種 `application layer` 協議，在網絡設備之間傳輸信息，並在網路協議棧的其它層之上運行。

## What’s in an HTTP request ?
HTTP 請求是 internet 通信平台（如 Web 瀏覽器）詢問加載網站所需資訊的方式。

在 internet 上發出的每個 HTTP 請求都帶有一系列大有不同類型訊息的編碼數據。
典型的 HTTP 請求包含：

1. HTTP version type
2. a URL
3. an HTTP method
4. HTTP request headers
5. Optional HTTP body.

#### What’s an HTTP method?
HTTP 方法（有時稱為 HTTP verb）指示 HTTP 請求希望從查詢的服務器執行的操作。

例如，兩種最常見的 HTTP 方法是 'GET' 和 'POST'。'GET' 請求希望返回訊息（通常以網站的形式）；而 'POST' 請求通常表示客戶端正在向 Web 服務器提交訊息（例如：表單資訊、提交的用戶名和密碼）。

#### What are HTTP request headers ?
HTTP 標頭包含存儲在鍵值對中的文本信息，它們包含在每個 HTTP 請求中以及響應。這些標頭傳達核心訊息，例如：客戶端正在使用哪些瀏覽器使用所請求的數據。
Google Chrome，HTTP 請求標頭示例：
![](https://www.cloudflare.com/img/learning/ddos/glossary/hypertext-transfer-protocol-http/http-request-headers.png)

#### What’s in an HTTP request body ?
request 的主體是包含請求傳輸的 "body" 信息的部分，HTTP 請求的 body 包含提交給 Web 服務器的任何資訊，例如：用戶名和密碼或輸入到表單中的任何其他數據。

## What’s in an HTTP response ?
HTTP response 是 Web 客戶端（通常是瀏覽器）從 Internet 服務器接收的 response，HTTP 請求的響應。 這些 response 根據 HTTP request 中的要求傳達有價值的訊息。

典型的 HTTP response 包含：
1. an HTTP status code
2. HTTP response headers
3. optional HTTP body

#### What’s an HTTP status code?
HTTP status code 是最常用於指示 HTTP request 是否已成功完成的 3位數代碼。

status code 分為以下 5 個：
- 1xx Informational
    - 表示訊息響應
- 2xx Success
    - 以 "2" 開頭的 status code 表示成功。例如：在客戶端請求網頁後，最常見的響應的狀態代碼為 "200 OK"，表示請求已正確完成
- 3xx Redirection
    - 重新定向
- 4xx Client Error
    - 表示存在錯誤，並且不會顯示網頁。表示客戶端錯誤（在URL中輸入拼寫錯誤時遇到"404 NOT FOUND"狀態代碼非常常見）。
- 5xx Server Error
    - 表示存在錯誤，並且不會顯示網頁。服務器端出錯。

> "xx" 指的是 00 ~ 99 之間的不同數字

#### What are HTTP response headers ?
與 HTTP request 非常相似，HTTP response 帶有 headers，用於傳達重要訊息，例如：response "body" 中發送的數據的語言和格式。

HTTP response headers：

![](https://www.cloudflare.com/img/learning/ddos/glossary/hypertext-transfer-protocol-http/http-response-headers.png)

#### What’s in an HTTP response body ?
對 "GET" 請求的成功 HTTP response 通常具有包含所請求信息的主體，在大多數 Web request 中，這是 Web data，Web 瀏覽器將轉換為網頁。

## Can DDoS Attacks Be Launched Over HTTP ?
HTTP 是一種"無狀態"協議，這表示每個命令都獨立於任何其他命令運行。 在原始規範中，HTTP request 每個創建並關閉 TCP 連接，在較新版本的HTTP 協議（HTTP 1.1及更高版本）中，持久連接允許多個 HTTP 請求藉由持久 TCP 連接傳遞，從而改善了資源消耗，在 `DoS` 或 `DDoS` 攻擊的上下文中，大量的 HTTP request 可用於在目標設備上發起攻擊，並被視為應用層攻擊或第 7 層攻擊的一部分。

> 無狀態是指一種把每個請求作為與之前任何請求都無關的獨立的事務的伺服器。HTTP 伺服器就是一個例子，以 URL 形式提交的客戶端請求可能包含cookies 等帶狀態的數據，這些數據完全指定了所需的文檔，而不需要其他之前請求的上下文或內存。
