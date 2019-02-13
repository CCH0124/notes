# HTTPS
`HTTPS` 是一種在 Web 服務器和 Web 瀏覽器之間發送數據的安全方法。
## What is HTTPS ?
`Hypertext transfer protocol secure（HTTPS）`是 `HTTP` 的安全版本，它是用於在 Web 瀏覽器和網站之間發送數據的主要協定。`HTTPS` 加密，為了提高數據傳輸的安全性。這在傳輸敏感數據時其為重要，例如：登錄銀行帳戶、電子郵件服務或健康保險提供商等。

任何需要登錄憑證的網站都應使用 `HTTPS`。在 Chrome 等現代網路瀏覽器中，不使用 HTTPS 的網站則會被標記。在 URL 欄中查找綠色掛鎖以表示網頁是安全的，Web 瀏覽器應該認真對待 HTTPS，從 2018 年 4 月開始，Google Chrome 的實施會將所有非 HTTPS 網站標記為不安全。

![](https://www.cloudflare.com/img/learning/security/glossary/what-is-https/not-secure.png)
## How does HTTPS work ?
HTTPS 使用安全協定來加密通訊。這兩個協議是 `secure socket layer （SSL）` 和 `transport layer security（TLS）`，後者是前者的延續，雖然現代網路瀏覽器使用`TLS`，但在 internet 上這些名稱通常可以互換使用。在任何一種情況下，HTTPS 都使用加密來保護通訊，使用所謂的**非對稱公鑰基礎結構**。這種類型的安全系統使用兩個不同的密鑰來加密雙方之間的通訊：
1. The private key 
這個密鑰由一個網站的擁有者控制，並且它被保存，正如讀者所推測的那樣，是私有的。此密鑰位於 Web 服務器上，用於解密由公鑰加密的訊息。
2. The public key
每個想要以安全的方式與服務器交互的人都可以使用此密鑰。由公鑰加密的訊息只能透過私鑰解密。

## Why is HTTPS important ? What happens if a website doesn’t have HTTPS ?
`HTTPS` 可以防止網站以任何窺探網路的人輕鬆查看的方式廣播其訊息。當通過常規 HTTP 發送訊息時，訊息將被分解為可以使用開源軟體輕鬆"嗅探"的數據包。這使得透過如公共 Wi-Fi 之類的不安全介質進行通訊非常容易受到攔截。實際上，透過 `HTTP` 發生的所有通訊都以純文本形式發生，這使得任何擁有正確工具且易受中間人攻擊的人都可以完全訪問這些通訊。

使用 HTTPS 讓流量被加密，即使數據包被嗅探或以其他方式被截獲，它們也會被視為無意義的字符。我們來看一個例子：
Before encryption：
```shell
This is a string of text that is completely readable
```
After encryption：
```shell
ITM0IRyiEhVpa6VnKyExMiEgNveroyWBPlgGyfkflYjDaaFf/Kn3bo3OfghBPDWo6AfSHlNtL8N7ITEwIXc1gU5X73xMsJormzzXlwOyrCs+9XCPk63Y+z0=
```

在沒有 HTTPS 的網站中，未經網站擁有者批准，Internet service providers（ISP）或其他中介機構可以將內容注入網頁。這通常採取廣告的形式，其中尋求增加收入的 ISP 將付費廣告注入其客戶的網頁。不出所料，當發生這種情況時，廣告的利潤和這些廣告的質量控制絕不與網站擁有者共享。HTTPS 消除了未加管理的第三方將廣告注入 Web 內容的能力。

## How is HTTPS different from HTTP ?
從技術上講，HTTPS 不是 HTTP 的單獨協定。它只是透過 HTTP 協定使用 `TLS/SSL` 加密。HTTPS 基於證書的傳輸而發生，證明特定提供者是他們所聲稱的人。

當用戶連接到網頁時，網頁將通過其 SSL 證書發送，該證書包含啟動安全會話所需的公鑰。然後，兩台計算機（客戶端和服務器）經歷稱為 `SSL/TLS` 握手的過程，這是用於建立安全連接的一系列來回通訊。

## How does a website start using HTTPS ?
許多網站託管服務提供商和其他服務將提供收費的 HTTPS 證書。這些證書通常會在許多客戶之間共享。可以使用更昂貴的證書，可以單獨註冊到特定的 Web 屬性。