# DNS Root Server
DNS root server 是 DNS lookup 中的第一站。
## What is a DNS root server ?
Domain Name System (DNS) 的管理使用不同的管理區域或 "zones" 構建在層次結構中，root zone 位於該層次結構的最頂層。
Root servers 是在 root zone 中運行的 DNS nameservers。這些服務器可以直接回答對 root zone 中存儲或緩存的記錄的查詢，並且還可以將其它請求引用到相應的 Top Level Domain (TLD) server。TLD server 是 DNS server 群組，位於 DNS 層次結構中 root server 的一步，它們是解析 DNS 查詢的組成部分。

![](https://www.cloudflare.com/img/learning/dns/glossary/dns-root-server/dns-root-server.png)

在未緩存的 DNS query 期間，只要用戶在其瀏覽器中輸入 Web address，此操作就會觸發 DNS lookup，並且所有 DNS lookup 都從 root zone 開始。一旦查找到達 root zone，查找將沿著 DNS 系統的層次結構向下移動，首先命中TLDs server，然後命中特定 domains（可能是 subdomains）的 server，直到它最終命中正確 domain 的 authoritative nameserver，其中包含所尋求網站的數字 IP address。然後將該 IP address 返回給客戶端。

Root servers 是 Internet 基礎設施的重要組成部分。沒有它們，Web 瀏覽器和許多其他 Internet 工具將無法運行。有 13 個不同的 IP address 為 DNS root zone 提供服務，全球有數百個冗餘 root servers 來處理對 root zone 的請求。

## Why are there only 13 DNS root server addresses ?
一個常見的誤解是世界上只有 13 個 root servers。實際上還有更多，但仍然只有 13 個 IP address 用於查詢不同的 root servers 網路，DNS 原始體系結構的限制要求 root zone 中最多有 13 個服務器地址。在 Internet 發展初期，13 個 IP address 中只有一個服務器，其中大多數位於美國。今天，13個 IP address 中的每一個都有幾個服務器，它們使用 Anycast 路由來根據負載和接近度分配請求。現在，有超過 600 種不同的 DNS root servers 分佈在地球上每個人口稠密的大陸上。

## Who Has Authority Over DNS Root Servers ?
Root zone 的最終權限屬於 National Telecommunications and Information Administration （NTIA），後者是 US Department of Commerce 的一部分。NTIA 將 root zone 的管理委託給 Internet Corporation for Assigned Names and Numbers（ICANN）。

ICANN 為 root zone 中的 13 個 IP address 之一運行服務器，並將其他 12 個 IP address 的操作委託給各種組織，包括 NASA、馬里蘭大學和Verisign，後者是唯一一個運營兩個 root IP address 的組織。

## How do resolvers find DNS root servers ?
由於 DNS root zone 位於 DNS 層次結構的頂部，因此無法在 DNS lookup 中將 recursive resolvers 定向到它們。因此，每個 DNS resolver 都有一個內置於其軟體中的 13 個 IP root server 地址的列表。無論何時啟動 DNS lookup，recursor 的第一次通訊都是使用這 13 個 IP address 之一。

## What happens if a DNS root server becomes unavailable ?
由於使用 Anycast 路由和 heavy redundancy，root servers 非常可靠。但在極少數情況下，root server 必須更新其 IP address，在這種情況下，recursive resolvers 可以繼續使用  root zone 中的其它 12 個 IP address 來執行 DNS lookup，直到使用所有 13 個服務器的正確地址更新其軟件。由於所有 root servers 都能夠將 DNS 請求轉發到 TLD server，因此當一個 root server 關閉時，Internet 的正常操作不會中斷。