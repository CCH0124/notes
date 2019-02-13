# SSL
SSL 是一種安全協定，可為 Internet 通訊提供隱私、身份驗證和完整性。SSL 最終演變為 `Transport Layer Security（TLS）`
## What is SSL ?
`SSL` 或 `Secure Sockets Layer` 是一種基於加密的 Internet 安全協定。它最初是由 Netscape 於 1995 年開發的，目的是確保 Internet 通訊中的隱私、身份驗證和數據完整性。SSL 是當今使用的現代 TLS 加密的前身。

![](https://www.cloudflare.com/img/learning/security/glossary/what-is-ssl/http-vs-https.svg)

## How does SSL/TLS work ?
- 為了提供高度的隱私，SSL 加密透過 Web 傳輸的數據。這表示著任何試圖攔截這些數據的人都只能看到幾乎無法解密的混亂字符組合。
- SSL 在兩個通訊設備之間啟動稱為 `handshake` 的身份驗證過程，以確保兩個設備確實是他們聲稱的身份。
- SSL 還對數據進行 `digitally signs `，以提供數據完整性，驗證數據在到達預期收件人之前未被篡改。

## Are SSL and TLS the same thing ?
SSL 是另一種稱為 TLS（傳輸層安全性）協定的直接前身。1999 年，Internet Engineering Task Force（IETF）提出了對 SSL 的更新。由於此更新由 IETF 開發，並且不再涉及 Netscape，因此名稱已更改為 TLS。最終版本的 SSL（3.0）與第一版 TLS 之間的差異並不大，名稱更改用於表示所有權的變更。

由於它們密切相關，因此這兩個術語經常互換使用並混淆。有些人仍然使用 SSL 來引用 TLS，其他人使用術語 "SSL/TLS encryption"，因為 SSL 仍然有如此多的名稱識別。

## Is SSL still up to date ?
自 1996 年 SSL 3.0 以來，SSL 尚未更新，現在認為已棄用。SSL 協議中存在多個已知漏洞，安全專家建議停止使用它。實際上，大多數現代 Web 瀏覽器根本不再支持 SSL。

TLS 是最新的加密協定，仍然在線實施，即使很多人仍將其稱為"SSL encryption"。對於購買安全解決方案的消費者而言，這可能會引起混淆。事實是，現在任何提供 "SSL" 的供應商幾乎都可以提供 TLS 保護，這已經成為近 20 年的行業標準。但由於許多人仍在尋找"SSL protection"，因此該術語仍然在許多產品頁面上佔據顯著位置。