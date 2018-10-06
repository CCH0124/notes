# DNS Server Types
有四種不同類型的 DNS server 必須協調工作才能提供單個網頁。

## What are the different types of DNS server ?
所有 DNS server 都屬於以下四類之一：Recursive resolvers、root nameservers、TLD nameservers、authoritative nameservers。在典型的 DNS 查找中（當播放中沒有緩存時），這四個 DNS server 協同工作完成後，將指定 domain 的 IP address 傳遞給客戶端的任務（客戶端通常是 stub resolver - 簡單 resolver 內置於 operating system 中）。

## What is a DNS recursive resolver ?
![](https://www.cloudflare.com/img/learning/dns/dns-server-types/recursive-resolver.png)

Recursive resolver（也稱為 DNS recursor）是 DNS 查詢中的第一站，recursive resolver 充當客戶端和 DNS nameserver 之間的中間人。從Web 客戶端收到 DNS 查詢後，recursive resolver 將使用緩存數據進行回應，或者向 root nameserver 發送請求，然後向 TLD nameserver 發送另一個請求，然後向 authoritative nameserver 發送最後一個請求，在從包含所請求的 IP address 的 authoritative nameserver 接收到回應之後，recursive resolver 然後向客戶端發送回應。

在此過程中，recursive resolver 將緩存從 authoritative name servers 接收的信息。當客戶端請求另一個客戶端最近請求的 domain name 的 IP address 時，resolver 可以繞過與 nameservers 通訊的過程，並且只是從其緩存向客戶端傳遞所請求的記錄。

大多數 internet 用戶使用其 ISP 提供的 recursive resolver。

## What is a DNS root nameserver ?
![](https://www.cloudflare.com/img/learning/dns/dns-server-types/root-nameserver.png)

每個 recursive resolver 都知道 13 個 DNS root nameserver，它們是recursive resolver 尋求 DNS 記錄的第一站。root server 接受包含domain name 的 recursive resolver’s  查詢，並且 root nameserver 透過將 recursive resolver 指向 TLD nameserver 基於該 domain 的擴展名（.com、.net、.org等）進行回應。root nameserver Internet Corporation for Assigned Names and Numbers （ICANN）的非營利組織監管。

>雖然有 13 個 root nameserver，但這並不意味著 root nameserver 系統中只有 13 台計算機。有 13 種類型的 root nameserver，但世界各地都有多個副本，它們使用 Anycast routing 來提供快速回應。如果您添加了 root nameserver 的所有實例，則您將擁有 632 個不同的 server（截至2016年10月）。

## What is a TLD nameserver ?
![](https://www.cloudflare.com/img/learning/dns/dns-server-types/tld-nameserver.png)

TLD nameserver 維護共享公共域擴展名的所有 domain names 的訊息，例如 .com、.net 或網址中最後一個點之後的任何域名。例如：.com TLD nameserver 包含以 ".com" 結尾的每個網站的訊息。如果用戶正在搜索google.com，則在收到來自 root nameserver 的回應後，recursive resolver 會向 .com TLD nameserver 發送查詢，該 server 將透過指向該域的 authoritative nameserver（請參閱下文）進行回應。

TLD nameservers 的管理由 Internet Assigned Numbers Authority （IANA）處理，IANA 是 ICANN 的一個分支機構。 IANA 將 TLD server 分為兩大類：
- Generic top-level domains：
  - 這些 domain names 不是特定國家/地區，一些最著名的通用頂級域名包括 .com、.org、.net、.edu 和 .gov。
- Country code top-level domains：
  - 這些包括特定於國家或州的任何域，包括 .uk、.us、.ru 和 .jp。

基礎設施 domains 實際上有第三類，但幾乎從未使用過。此類別是為 .arpa domain 創建的，該 domain 是用於創建現代 DNS 的過渡域，今天它的意義主要是歷史性的。

## What is an authoritative nameserver ?
![](https://www.cloudflare.com/img/learning/dns/dns-server-types/authoritative-nameserver.png)

當 recursive resolver 收到來自 TLD nameserver 的回應時，該回應會將 resolver 指向 authoritative nameserver。authoritative nameserver 通常是 resolver’s 在 IP address 旅程中的最後一步。authoritative nameserver 包含特定於其服務的 domain name 的訊息（例如 google.com），它可以提供  recursive resolver，其中包含 DNS A record 中找到的該 server 的 IP address，或者 domain names 是否具有 CNAME record（別名）它將為 recursive resolver 提供別名域，此時 recursive resolver 必須執行全新的 DNS 查找以從 authoritative nameserver（通常是包含 IP address 的 A record）獲取記錄。