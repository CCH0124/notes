# Man-In-The-Middle Attack
## What is a man-in-the-middle attack ?
在中間人攻擊（man-in-the-middle）中，攻擊者將自己置於兩個設備（通常是 Web 瀏覽器和 Web 服務器）之間，並攔截或修改兩者之間的通信。接著，攻擊者可以收集訊息並冒充兩個代理中的任何一個。除了網站，這些攻擊還可以針對電子郵件通訊、`DNS lookup` 和公共 WiFi 網路。中間人攻擊的典型目標包括 SaaS 業務，電子商務業務和財務應用用戶。

![](https://www.cloudflare.com/img/learning/security/threats/man-in-the-middle-attack/man-in-the-middle-attack.svg)

你可以想到一個中間人的攻擊者就像一個流氓郵政工作者，他坐在郵局裡攔截兩個人之間寫的信件。這個郵政工作人員可以閱讀私信，甚至可以在將這些信件的內容傳遞給目標收件人之前對其進行編輯。

在一個更現代的例子中，中間人攻擊者可以坐在用戶和他們想要訪問的網站之間，並收集他們的用戶名和密碼。這可以透過定位用戶和網站之間的 `HTTP` 連接來完成，劫持此連接可讓攻擊者充當代理，收集和修改用戶與站點之間發送的訊息。或者，攻擊者可以竊取用戶的 cookie（由網站創建並存儲在用戶計算機上的小數據，用於識別和其他目的）。這些被盜的 cookie 可用於劫持用戶的 session，讓攻擊者在網站上冒充該用戶。

中間人攻擊也可以針對 `DNS server`。DNS 查找過程允許 Web 瀏覽器透過將域名轉換為 IP 地址來查找網站。在 `DNS spoofing` 和 `DNS hijacking` 等DNS 中間人攻擊中，攻擊者可能會破壞 DNS 查找過程並將用戶發送到錯誤的站點，**通常是分發 malware 和/或收集敏感信息的站點**。

![](https://www.cloudflare.com/img/learning/security/threats/man-in-the-middle-attack/dns-hijacking.png)

## What is email hijacking ?
另一種常見的中間人攻擊是電子郵件劫持，攻擊者通過將自己置於電子郵件服務器和網路之間來侵入電子郵件服務器。一旦服務器遭到入侵，攻擊者就可以出於各種目的監控電子郵件通訊。一個這樣的騙局涉及等待一個人需要將錢轉移到另一個人（例如，支付業務的客戶）的情況。然後，攻擊者可以使用欺騙性電子郵件地址請求將資金轉移到攻擊者的帳戶。該電子郵件對於收件人來說似乎是合法且無害的（"抱歉我的上一封電子郵件中有一個錯字！我的帳號實際上是：XXX-XXXX"）使這次攻擊非常有效並且在財務上具有破壞性。2015 年，比利時的一個網絡犯罪集團利用電子郵件劫持從歐洲各公司竊取600多萬歐元。

## Why is it risky to use public WiFi networks ?
中間人攻擊經常通過 WiFi 網路進行。攻擊者可以創建惡意的 WiFi 網路，這些網路似乎無害或者是合法 WiFi 網路的副本。一個用戶連接到受損的 WiFi 網路，攻擊者可以監視該用戶的在線活動。複雜的攻擊者甚至可能將用戶的瀏覽器重定向到合法網站的偽造副本。

## What are ways to protect against a man-in-the-middle attack ?
由於有多種方法可以實現中間人攻擊，因此沒有針對這些攻擊的一體化解決方案。防止針對 HTTP 流量的中間人攻擊的最基本方法之一是採用 SSL/TLS，在用戶和Web 服務之間創建安全連接。不幸的是，這不是一個萬無一失的解決方案，因為有一些更複雜的中間人攻擊可以解決 SSL/TLS 保護問題。為了進一步防止這些類型的攻擊，一些 Web 服務實現了 HTTP Strict Transport Security（HSTS），它強制與任何瀏覽器或應用程序建立安全的 SSL/TLS 連接，阻止任何不安全的HTTP 連接並防止 cookie 被盜。

Authentication certificates（身份驗證證書）還可用於防止中間人攻擊。組織可以在其所有設備上實施基於證書的身份驗證，以便只有具有正確配置的證書的用戶才能訪問其系統。

為防止電子郵件劫持，可以使用安全/多用途 Internet Mail Extensions（S/MIME）。該協議對電子郵件進行**加密**，並允許用戶使用唯一的 Digital Certificate（數字證書）對電子郵件進行數字簽名，讓接收者知道該郵件是合法的。

個人用戶還可以通透過避免在任何公共 WiFi 網絡上提交任何敏感信息來保護自己免受中間人攻擊，除非他們受到安全 Virtual Private Network （VPN）的保護。