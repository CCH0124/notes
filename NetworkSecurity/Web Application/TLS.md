# Transport Layer Security (TLS)
TLS 是一種安全協議，可透過 Internet 通訊提供**隱私**和**數據完整性**。TLS 很快成為構建安全 Web 應用程序的標準做法。
## What Is Transport Layer Security (TLS) ?
TLS 的主要用例是保護 Web 應用程序和服務器之間的通訊，例如：加載網站的 Web 瀏覽器。TLS 還可用於保護其他通訊，例如：電子郵件、消息傳遞和 IP 語音（VOIP）。

TLS 由國際標準組織  Internet Engineering Task Force（IETF）提出，該協議的第一版於 1999 年發布。最新版本是 TLS 1.2；於 2008 年發布，版本1.3 目前非常接近發布，並且已經被某些 Web 應用程序使用。

## What’s The Difference Between TLS and SSL ?
TLS 是從之前的 `Secure Socket Layer security`（SSL）安全協議發展而來的，該協議由 Netscape 開發。TLS 版本 1.0 實際上開始作為 SSL 版本 3.1 開發，但協議的名稱在發布之前已更改，以表明它不再與 Netscape 關聯。由於此歷史，術語 TLS 和 SSL 有時可互換使用。

## What’s The Difference Between TLS and HTTPS ?
HTTPS 是在流行的 HTTP 協議之上的 TLS 加密實現，所有網站以及其他一些 Web 服務都使用它。因此，任何使用 HTTPS 的網站都採用 TLS 安全性。

## Why Should You Use TLS ?
TLS 加密可以幫助保護 Web 應用程序免受 `data breaches`（數據洩露）和 `DDoS` 攻擊等攻擊。此外，受 TLS 保護的 HTTPS 正迅速成為網站的標準做法。例如：google 的 Chrome 瀏覽器正在打擊非 HTTPS 網站，日常互聯網用戶開始對那些沒有 HTTPS 掛鎖圖標的網站變得更加謹慎。

## How Does TLS Work ?
Transport layer security 在 OSI 模型的應用層（application layer）上實現，這表示著它可以在 TCP 之類的傳輸層安全協議之上使用。TLS 有三個主要組件：**Encryption**、**Authentication**、**Integrity**。

##### Encryption
隱藏從第三方傳輸的數據。
##### Authentication
確保交換訊息的各方是他們聲稱的人。
##### Integrity
驗證數據未被偽造或篡改。

使用稱為 TLS handshark 的序列啟動 TLS 連接。TLS 握手以類似於 TCP 中使用的 `syn/syn-ack/ack` 交換開始，然後**為每個通訊建立一個密碼套件**。密碼套件是一組算法，用於指定詳細信息，例如：哪個共享加密密鑰將用於該特定會話。由於稱為公鑰加密技術，TLS 能夠在未加密的通道上設置匹配的加密密鑰。handshark 還處理 `authentication`，通常由服務器向客戶端證明其身份，這是使用公鑰完成的。**公鑰是使用單向（one-way）加密的加密密鑰**，這意味著任何人都可以對密鑰進行解密以確保其真實性，但**只有原始發件人才能生成密鑰**。

數據經過加密和驗證後，將使用 `message authentication code`（MAC）進行簽名。然後，**接收方可以驗證 MAC 以確保數據的完整性（integrity）**。這有點像在一瓶阿司匹林上發現的防篡改金屬，消費者知道沒有人篡改他們的藥物，因為他們購買時金屬完好無損。

![](https://www.cloudflare.com/img/learning/cdn/tls-ssl/tls-ssl-handshake.png)

## How Does TLS Affect Web Application Performance ?
**由於設置 TLS 連接所涉及的複雜過程，必須花費一些加載時間和計算能力**。在傳輸任何數據之前，客戶端和服務器必須**來回通訊三次**，這會佔用 Web 應用程序的寶貴加載時間，以及客戶端和服務器的一些內存。

值得慶幸的是，有些技術可以幫助緩解 TLS 握手造成的延遲。一個是 `TLS False Start`，它允許服務器和客戶端在 TLS 握手完成之前開始傳輸數據。另一種加速 TLS 的技術是 `TLS Session Resumption`，它允許先前通訊的客戶端和服務器使用 `abbreviated handshake`。

這些改進有助於使 TLS 成為一種非常快速的協議，不會明顯影響加載時間。至於與 TLS 相關的計算成本，它們在今天的標準中幾乎可以忽略不計。例如，當Google 在 2010 年將整個 Gmail 平台遷移到 HTTPS 時，他們無需啟用任何其他硬體。由於 TLS 加密，其服務器上的額外負載不到 1%。

## Ref
[abbreviated handshake](https://ldapwiki.com/wiki/TLS%20Abbreviated%20Handshake)