# DNS Zone
DNS Zone 將 DNS 分解為明確的層次結構。

## What is a DNS zone ?
DNS 被分解為許多不同的 zones。這些 zones 區分 DNS namespace 中的明確管理區域，DNS zone 是由特定組織或管理員管理的 DNS namespace 的一部分。DNS zone 是一個管理空間，允許對 DNS 組件進行更細粒度的控制，例如：authoritative nameservers。domain name space 是分層樹，DNS root domain 位於頂部。DNS zone 從樹中的 domain 開始，也可以向下擴展到子域，以便一個實體可以管理多個 subdomains。

常見的錯誤是將 DNS zone 與 domain name 或單個 DNS server 相關聯。實際上，DNS zone 可以包含多個 subdomains，並且同一 server 上可以存在多個 zones，DNS zones 不一定在物理上彼此分離，zones 嚴格用於委派控制。

例如，想像一下 cloudflare.com domain 及其三個 subdomains 的假設區域：support.cloudflare.com、community.cloudflare.com 和 blog.cloudflare.com。假設 blog 是一個健壯、獨立的站點，需要單獨管理，但 support 和 community 頁面與 cloudflare.com 關係更密切，可以在與 primary domain 相同的 zone 中進行管理，在這種情況下，cloudflare.com 以及 support 和 community 站點都將位於一個 zone 中，而 blog.cloudflare.com 將存在於其自己的 zone 中。

![](https://www.cloudflare.com/img/learning/dns/glossary/dns-zone/dns-zone.png)

Zone 的所有訊息都存儲在所謂的 DNS zone 文件中，這是了解 DNS zone 如何運行的關鍵。

## What is a DNS zone file ?
Zone 文件是存儲在 DNS server 中的純文字文件，其中包含 zone 的實際表示形式，並包含 zone 中每個 domain 的所有記錄。Zone 文件必須 Start of Authority（SOA）record 開頭，該 record 包含重要訊息，包括 zone  管理員的聯繫訊息。

## What is a Reverse Lookup Zone ?
Reverse lookup zone 包含從 IP address 到主機的映射（大多數 DNS zone 的相反功能）。這些 zone 用於故障排除，spam 過濾和 bot 檢測。