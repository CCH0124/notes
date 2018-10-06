# DNS Records
DNS Records 是存在於 DNS server 上的指令集。這些指令對 DNS lookup 的成功至關重要。

## What is a DNS record ?
DNS records (aka zone files) 是存在於 authoritative `DNS servers` 中的指令，它提供有關 domain 的信息，包括與該域關聯的 `IP address` 以及如何處理該域的請求。這些記錄由一系列用所謂的 DNS 語法編寫的文本文件組成，DNS 語法只是一串字符，用作告訴 DNS server 做什麼的命令。所有 DNS records 也都有一個 "TTL"，代表生存時間，並指示 DNS server 刷新該記錄的頻率。

您可以將一組 DNS records 視為 Yelp 上的商家信息，該列表將為您提供一系列有關商家的有用信息，例如：他們的位置、營業時間、提供的服務等。所有 domain 都必須至少具有一些必要的 DNS records，以便用戶能夠使用域名訪問其網站，並且有幾個可選記錄可用於其他目的。

## What are the most common types of DNS record ?
#### A record 
DNS A 記錄指向給定 domain 的 IP address。

'A' 代表 'address'，這是最基本的 DNS record 類型，它表示給定 domain 的 IP address。例如：如果您提取 google.com 的 DNS 記錄，則 "A" 記錄當前返回的 IP address 為 172.217.5.78。'A' 記錄只保存 Ipv4 address，如果該站點有 Ipv6 address，它將改為使用 'AAAA' 記錄。

Example of an A record：
|example.com|record type|value|TTL|
|--|--|--|--|
|@|A|12.34.56.78|14400

這裡的 '@' 表示這是 root domain 的記錄，'14400' 值是 TTL（生存時間），以秒為單位列出。 A 記錄的默認 TTL 為 14400 秒。這意味著如果 A記錄更新，則需要 240 分鐘（14400秒）才能生效。

絕大多數網站只有一個 A 記錄，但可能有幾個。一些較高知名度的網站將具有幾個不同的 A 記錄，作為稱為循環負載平衡的技術的一部分，該技術可以將請求流量分配到多個 IP address 之一，每個 IP address 託管相同的內容。

#### CNAME record 
將一個 domain 或 subdomain 轉發到另一個 domain，不提供 IP address。

DNS CNAME record 用作共享單個 IP address 的 domain 的別名。

當 domain 或 subdomain 是另一個 domain 的別名時，"canonical name" 記錄用於代替 A record。想像一下尋寶遊戲，每條線索指向另一條線索，最後的線索指向寶藏。具有 CNAME record 的 domain 就像一條線索，它可以指向另一條線索（具有 CNAME record 的另一個 domain）或寶藏（具有 A record 的 domain）。例如，假設 www.example.com 的 CNAME record 值為 "example.com"（沒有 "www"），這說明了當 DNS server 訪問 www.example.com 的 DNS record 時，它實際上會觸發另一個 DNS 查找到 example.com，返回 example.com 的 IP address，在這種情況下，我們會說 example.com 是 blog.example.com 的 canonical name（或真實名稱）。**所有 CNAME record 必須指向 domain，而不是指向 IP address**。

通常，當站點具有 subdomain（例如 blog.example.com 或 shop.example.com）時，這些 subdomain 將具有指向 root domain（example.com）的 CNAME record。這樣，如果主機的 IP 發生更改，則只需更新 root domain 的 DNS A record，並且所有 CNAME record 將跟隨對 root 進行的任何更改。

一個經常出現的誤解是 CNAME record 必須始終 resolve 到與其指向的 domain 相同的網站，但事實並非如此，CNAME record 記錄僅將客戶端指向與 root domain 相同的 IP address，一旦客戶端訪問該 IP address，Web server 仍將相對應地處理該 URL。例如，blog.example.com 可能有一個指向 example.com 的 CNAME，將客戶端指向 example.com 的 IP address，但是當客戶端實際連接到該 IP address 時，Web server 將查看URL，查看它是 blog.example.com，並提供 blog 頁面而不是主頁。

Example of a CNAME record：
|blog.example.com|record type|value|TTL|
|--|--|--|--|
|@|CNAME|is an alias of example.com|32600

在此示例中，您可以看到 blog.example.com 指向 example.com，並假設它基於我們的 A record，我們知道它最終將解析為 IP address 12.34.56.78。

#### MX record
將郵件定向到 mail server。
MX record 將電子郵件定向到 mail exchange server。

這是 "mail exchange" 記錄，它將電子郵件定向到 mail server。MX record 指示應該如何根據 Simple Mail Transfer Protocol（SMTP，所有電子郵件的標準協議）路由電子郵件。與CNAME記錄一樣，MX記錄必須始終指向另一個域。**與 CNAME record 一樣，MX record 必須始終指向另一個 domain**。

Example of an MX record：
|example.com|record type|value|TTL|
|--|--|--|--|
|@|MX|10 mailhost.example.com|45000
|@|MX|20 mailhost2.example.com|45000

這些 MX record 的條目中的 domain 之前的數字表示首選項，服務器將始終首先嘗試 mailhost，因為 10 低於 20，在消息發送失敗的結果中，服務器將默認為 mailhost2。

#### TXT record
允許管理員在記錄中存儲文本 notes。
TXT record 允許域管理員在 DNS server 上留下 notes。

"TXT" record 讓 domain 管理員在 DNS 記錄中輸入文本，因為它最初是用作人類可讀筆記的地方。但是現在也可以將一些機器可讀代碼放入 TXT record 中。一個 domain 可以有許多 TXT record，它們通常用於發件人 Sender Policy Framework（SPF）代碼，這些代碼可幫助電子郵件服務器確定郵件是否來自受信任的來源，以及 domain 的所有權驗證。例如，某些網站管理員工具會要求您向 domain 中添加 TXT record，以證明您是該 domain 的真正所有者。

Example of a TXT record：
|example.com|record type|value|TTL|
|--|--|--|--|
|@|TXT|This is an awesome domain! Definitely not spammy.|32600|

#### NS record
存儲 DNS 輸入的 name server。
NS record 表明哪個 DNS server 具有權威性。

NS 代表 "name server"，此記錄表示哪個 DNS server 對該 domain 具有權威性（哪個 server 包含實際的 DNS record）。domain 通常具有多個 NS record，這些 record 可以指示該 domain 的主要和備用 name servers 。

Example of an NS record：
|example.com|record type|value|TTL|
|--|--|--|--|
|@|NS|ns1.exampleserver.com|21600|

#### SOA record 
存儲有關 domain 的管理員訊息。
SOA record 包含有關 domain 的重要訊息以及由誰負責的 domain。

start of authority 記錄可以存儲有關 domain 的重要訊息，例如：管理員的電子郵件地址、domain 上次更新的時間以及服務器在刷新之間應等待的時間。

Example of an SOA record：
|example.com|record type|value|TTL|
|--|--|--|--|
|@|SOA|admin.example.com|11200|

此處的值表示電子郵件地址，由於缺少 "@" 符號，因此可能會引起混淆，但在 SOA record 中，admin.example.com 相當於 admin@example.com。

#### SRV record
指定特定服務的端口。
SRV record 用於 VOIP 等特殊服務。

Service record 指定特定服務的主機和端口，例如：IP 語音（VOIP），即時消息等。

Example of an SRV record：
|example.com|record type|value|TTL|
|--|--|--|--|
|@_sip._tcp.example.com.|SRV|8080 exampledomain.com|41200|

在上面的示例中，'_ sip' 表示服務類型、'_ tcp' 表示 protocol、'8080' 表示 port 、'example.com' 是 host。

#### PTR record
在反向查找中提供 domain name。
PTR record 用於反向 DNS 查找。

"pointer" record 正好與 "A" record 相反。PTR address 將為您提供與給定 IP address 關聯的 domain。 PTR record 用於反向查找 zones 以進行反向 DNS 搜索。

Example of an PTR record：
|example.com|record type|value|TTL|
|--|--|--|--|
|@|PTR|example.com|71200|

## What are some of the less commonly used DNS records ?
- AFSDB record
  - 此 record 用於，由 Carnegie Melon 開發的 Andrew File System（AFS）的客戶端。AFSDB record 用於查找其他 AFS 單元。
- APL record
  - "address prefix list" 是指定 address 範圍列表的實驗記錄。
- CAA record
  - 這是 "certification authority authorization" record，它允許 domain 所有者聲明哪個證書頒發機構可以為該 domain 頒發證書。
- DNSKEY record
  - "DNS Key Record" 包含用於驗證 `domain name System Security Extension`（DNSSEC）簽名的公鑰。
- CDNSKEY record
  - 這是 DNSKEY record 的子副本，指在轉移到父級。
- CERT record
  - certificate record 存儲公鑰證書。
- DCHID record
  - DHCP Identifier 存儲 Dynamic Host Configuration Protocol （DHCP）的信息，DHCP 是 IP 網路上使用的標準化網路協議。
- DNAME record
  - "delegation name" record 創建 domain  別名，就像 CNAME 一樣，但此別名也會**重定向**所有 subdomains。例如：如果 'example.com' 的所有者購買了域名 'website.net' 並且給了它一個指向 'example.com' 的 DNAME 記錄，那麼該指針也將擴展到 'blog.website.net' 和任何其他 subdomains。
- HIP record
  - 此 record 使用 "Host identity protocol"，一種分隔 IP address 角色的方法，此記錄最常用於 mobile computing。
- IPSECKEY record
  - "IPSEC key" record 與 Internet Protocol Security（IPSEC）一起使用，這是一種 end-to-end 安全協議框架，是  Internet Protocol Suite（TCP / IP）的一部分。
- LOC record
  - "location" record 包含經度和緯度坐標形式的域的地理信息。
- NAPTR record
  - "name authority pointer" record 可以與 `SRV record` 組合，以**基於正則表達式動態創建指向的 URI**。
- NSEC record
  - "next secure record" 是 DNSSEC 的一部分，它用於證明所請求的 DNS 資源記錄不存在。
- RRSIG record
  - "resource record signature" 是用於存儲用於根據 DNSSEC 認證記錄的 digital signatures 的記錄。
- RP record
  - 這是 "responsible person" 記錄，它存儲負責該 domain 的人員的電子郵件地址。
- SSHFP record
  - 此記錄存儲 "SSH public key fingerprints"。SSH 代表 Secure Shell，它是一種加密網路協議，用於透過不安全的網路進行安全通訊。