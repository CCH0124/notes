# Reverse DNS
反向 DNS lookup 採用 IP 地址並返回與該 IP 關聯的域名。傳統的 DNS 查找恰恰相反。

## What is reverse DNS ?
反向 DNS lookup 是與給定 IP 地址關聯的域名的 DNS 查詢。這實現了與更常用的 forward DNS lookup 相反的方式，其中查詢 DNS 系統以返回 IP 地址。

![](https://www.cloudflare.com/img/learning/dns/glossary/reverse-dns/reverse-dns.png)

Internet Engineering Task Force（IETF）提出的標準建議每個域都應該能夠進行反向 DNS lookup，但由於反向查找對於 Internet 的正常功能並不重要，因此它們並不是一項硬性要求。因此，反向 DNS 查找並非普遍採用。

## What are reverse DNS lookups used for ?
**電子郵件服務器經常使用反向查找**。許多電子郵件服務器將拒絕來自任何不支持反向查找的服務器的消息。這是因為垃圾郵件發送者通常使用無效的 IP，因此這些電子郵件服務器會在將郵件發送到網絡之前檢查並查看郵件是否來自有效的服務器。

記錄軟體採用反向查找也很常見，以便為用戶提供日誌數據中的人類可讀域，而不是一堆數字 IP 地址。
## How does reverse DNS work ?
反向 DNS lookup 查詢 DNS 服務器以獲取 `PTR`（指針）記錄，**如果服務器沒有 `PTR` 記錄，則無法解析反向查找**。PTR 記錄存儲的 IP 地址與其 segments 相反，並且它們附加 `'.in-addr.arpa'`。例如，如果域的 IP 地址為 1.2.3.4，則 PTR 記錄將該信息存儲為 4.3.2.1.in-addr.arpa（segments 相反）。