# Domain Name Registrar
域名註冊商向公眾提供域名註冊。一個常見的誤解是註冊商出售域名，這些域名實際上歸註冊管理機構所有，只能由用戶租用。
## What is a Domain Name Registrar ?
域名註冊商是處理域名保留以及這些域名的 IP 地址分配的企業。域名是用於訪問網站的字母數字別名，例如：Google 的域名是"google.com"，其 IP 地址為 192.168.1.1。域名可以更輕鬆地訪問網站，而無需記憶和輸入數字 IP 地址。

![](https://www.cloudflare.com/img/learning/dns/what-is-1.1.1.1/dns-lookup.png)

>應該注意的是，註冊商實際上並不`管理`和`維護`域名，該部分由域名註冊表完成。

## What’s the difference between a registrar and a registry ?
註冊管理機構是管理  top-level domains （TLD）的組織，例如：以 `.com` 和`.net` 結尾的域。這些註冊管理機構由 Internet Assigned Numbers Authority（互聯網號碼分配機構，IANA）管理，IANA 是  Internet Corporation for Assigned Names and Numbers（互聯網名稱與數字地址分配機構，ICANN）的一個部門。

註冊管理機構將域名註冊的商業銷售委託給註冊商。例如，VeriSign 是 `.com` 域名的註冊機構，當註冊商向用戶出售 `.com` 域名註冊時，註冊商必須通知 VeriSign 才能正確保留域名，註冊商還必須向 VeriSign 支付費用，該費用計入註冊商向最終用戶收取的費用。

![](https://www.cloudflare.com/img/learning/dns/glossary/what-is-a-domain-name-registrar/registrar-flow.png)

這很像汽車經銷商。有興趣購買汽車的顧客走進展廳，並由知識淵博的銷售助理展示可用的汽車。如果客戶選擇購買汽車（對於這個例子，沒有一個已經有貨），經銷商必須從製造商那裡訂購汽車。客戶最終拿起汽車並從經銷商處接收客戶服務。

註冊商就像域名的經銷商，註冊表就像製造商。註冊商為交易提供便利並提供支持服務，而註冊管理機構負責生產和交付貨物。應該指出的是，註冊域名和購買汽車之間的關鍵區別在於，汽車可以由消費者擁有，而域名則不能。

## What does it mean to "own" a domain name ?
雖然人們經常會談到購買和擁有域名，但事實是註冊管理機構擁有所有域名，註冊商只是讓客戶有機會在有限的時間內保留這些域名。

域名的最長預訂期限為十年。用戶可以持有超過十年的域名，因為註冊商讓他們無限期的續訂預訂，但用戶從未真正擁有域名。他們只是租賃他們。

## Are registrars the only ones who can sell domain name registrations ?
除註冊商外，還有出售域名註冊的經銷商。這些經銷商代表註冊商出售域名，以換取查詢者的費用，雖然這些經銷商是合法的，但它們通常屬於副業，而且他們可能缺乏專門的客戶支援。

經銷商的網站很少明確聲明他們是經銷商，將它們與註冊商區分開來可能很棘手。幸運的是，有一種簡單的方法可以了解公司是否是合法的註冊商：ICANN在其網站上公佈了每個經過認證和活躍的域名註冊商的列表。

## How do domain name registrars protect user privacy ?
保留頂級域名（TLD）的每個人都必須填寫該域名的 `WHOIS` 信息。這是有關註冊域名（註冊人）的人員的訊息，包括他們的姓名、電子郵件地址、實際地址和電話號碼。許多註冊商提供私人註冊選項，在此安排中，註冊商的訊息在該域的 WHOIS 列表中提供，註冊商充當註冊人的代理，此私人註冊僅與註冊商一樣安全，因為實際註冊人的信息保存在註冊商的資料庫中。