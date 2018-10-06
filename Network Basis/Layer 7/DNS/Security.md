# DNS Security
DNS 的設計並未考慮安全性，並且為了利用 DNS 系統中的漏洞而創建了許多類型的攻擊。
## Why is DNS security important ?
幾乎所有 Web 流量都需要標準 DNS queries，這為 DNS 攻擊創造了機會，例如：DNS hijacking 和 man-in-the-middle attacks。這些攻擊可以將網站的入站流量重定向到網站的假鏈結，收集敏感的用戶訊息並使企業面臨重大責任。防止 DNS 威脅的最著名方法之一是採用 DNSSEC 協議。

## What Is DNSSEC ?
與許多 internet protocols 一樣，DNS 系統的設計並未考慮安全性，並且包含一些設計限制。這些限制與技術進步相結合，使攻擊者可以輕鬆地劫持 DNS lookup 以達到惡意目的，例如：將用戶發送到可以分發 malware 或收集個人信息的欺詐性網站。DNS Security Extensions（DNSSEC）是為緩解此問題而創建的安全協議，DNSSEC 透過 digital signing 數據來防止攻擊，以幫助確保其有效性，為了確保安全查找，signing 必須在 DNS lookup 過程的每個級別進行。

此 signing 過程類似於用筆簽署法律文件的人，該人使用其他人無法創建的獨特簽名進行簽名，並且法院專家可以查看該簽名並驗證該文檔是否由該人簽名。**這些 digital signing 可確保數據未被篡改**。

DNSSEC 在所有 DNS 層實施分層 digital signing 策略。例如：在 "google.com" 查找的情況下，`root DNS server` 將為 `.COM nameserver` 的密鑰簽名，然後 .COM nameserver 將為 google.com 的 `authoritative nameserver` 簽署密鑰。

雖然始終首選改進的安全性，但 DNSSEC 指在向後兼容，以確保傳統的 DNS lookup 仍能正確解析，儘管沒有增加的安全性。DNSSEC 指在與其他安全措施（如`SSL / TLS`）一起使用，作為整體 Internet 安全策略的一部分。

DNSSEC 創建一個父子信任列，一直到達 root zone。這種信任鏈不能在任何 DNS 層受到損害，否則該請求將對 man-in-the-middle attack。

要關閉信任鏈，root zone 本身需要進行驗證（證明沒有篡改或欺詐），這實際上是使用**人工干預完成**的，有趣的是，在所謂的 Root Zone Signing Ceremony 中，來自世界各地的選定人員會以公開和審計的方式簽署 root DNSKEY RRset。

[dnssec](https://blog.cloudflare.com/dnssec-an-introduction/)

## What are some common attacks involving DNS ?
DNSSEC 是一種功能強大的安全協議，但遺憾的是它目前尚未普遍採用。除了 DNS 是大多數 internet requests 不可或缺的一部分之外，這種缺乏採用以及其他潛在漏洞使得 DNS 成為惡意攻擊的主要目標。攻擊者已經找到了許多定位和利用 DNS server 的方法，這裡有一些最常見的：

- DNS spoofing/cache poisoning
  - 這是一種攻擊，其中偽造的 DNS 數據被引入 DNS resolver’s cache，導致 resolver 返回 domain 的錯誤 IP address。流量可以轉移到 malicious machine 或攻擊者所需的任何其它地方，而不是去正確的網站，通常，這將是用於惡意目的的原始站點的假冒點，例如：分發 malware 或收集登錄信息。
- DNS tunnelling
  - 此攻擊使用其他協議來隧道傳輸DNS查詢和響應。攻擊者可以使用 `SSH`、`TCP` 或 `HTTP` 將 malware 或被盜訊息傳遞到 DNS queries 中，而大多數防火牆都無法檢測到這些信息。
- DNS hijacking
  - 在 DNS hijacking 中，攻擊者將查詢重定向到不同的 domain name server，這可以透過 malware 或未經授權的 DNS server 修改來完成。雖然結果類似於 DNS spoofing，但這是一種根本不同的攻擊，因為它針對的是 nameserver 上網站的 DNS record，而不是 resolver’s cache。
  
![](https://www.cloudflare.com/img/learning/dns/dns-security/dns-hijacking.png)

- NXDOMAIN attack
  - 這是一種 DNS flood attack，攻擊者透過請求癱瘓 DNS server，詢問不存在的 records，試圖導致合法流量的 denial-of-service，這可以使用複雜的攻擊工具來完成，這些工具可以為每個請求自動生成唯一的 subdomains，NXDOMAIN 攻擊還可以針對 recursive resolver，目標是使用垃圾請求填充 resolver’s cache。
- Phantom domain attack
  - phantom domain attack 與 DNS resolver 上的 NXDOMAIN 攻擊具有類似的結果。攻擊者設置了一堆 "phantom" domain servers，這些 server 要嘛非常緩慢地響應請求；要嘛根本不響應。然後，resolver 會向這些 domains 發出大量請求，resolver 會等待響應，導致性能下降和denial-of-service。
- Random subdomain attack
  - 在這種情況下，攻擊者會為一個合法站點的幾個隨機不存在的 subdomains 發送 DNS queries。目標是為 domain 的 authoritative nameserver 創建 denial-of-service，從而無法從 nameserver 中查找網站，服務於攻擊者的 ISP 也可能受到影響，因為他們的 recursive resolver's 的 cache 將加載不良請求。
- Domain lock-up attack
  - 壞演員透過設置特殊 domains 和 resolvers 來與其他合法 resolvers 建立 TCP 連線來協調這種形式的攻擊。當目標 resolvers 發送請求時，這些 domains 會發回慢速的隨機封包流，從而佔用 resolvers 的資源。
- Botnet-based CPE attack
  - 這些攻擊是使用 CPE 設備進行的（客戶端設備，這是由服務提供商提供給客戶使用的硬體，如 modems、routers、cable boxes 等）。攻擊者危及CPE，設備成為 `botnet`，用於對一個站點或 domain 執行隨機 subdomain  攻擊。

## What’s the best way to protect against DNS-based attacks ?
除了 DNSSEC 之外，DNS zone 的運營商還可以採取進一步措施來保護其服務器，謹慎配置基礎架構是克服 `DDoS` 攻擊的一種簡單策略。簡而言之，如果nameserver 可以處理比您預期的多倍的流量，則 volume-based 的攻擊更難以癱瘓 server。

`Anycast routing` 是另一個可以破壞 DDoS 攻擊的便利工具，Anycast 允許多個 server 共享一個 IP address，因此即使一個 DNS server 關閉，仍然會有其他 server 啟動和服務。保護 DNS server 的另一種流行策略是**DNS 防火牆**。

## What is a DNS firewall ?
DNS 防火牆是一種可以為 DNS server 提供大量安全性和性能服務的工具。DNS 防火牆位於用戶的  recursive resolver 和他們嘗試訪問的網站或服務的 authoritative nameserver 之間，防火牆可以提供速率限制服務來關閉試圖癱瘓 server 的攻擊者，如果 server 確實因攻擊或任何其他原因而遇到停機，則 DNS 防火牆可以透過從 cache 提供 DNS 響應來保持運營商的站點或服務。除了其安全功能外，DNS 防火牆還可以提供性能解決方案，例如：更快的 DNS lookups 和 DNS 運營商的 bandwidth 成本降低。

## DNS as a security tool
DNS resolvers 還可以配置為其最終用戶（瀏覽 Internet 的人）提供安全解決方案。一些 DNS resolvers 提供的功能包括內容過濾，可以阻止已知分發 malware 和 spam 的站點；以及阻止與已知 botnet 通信的 botnet 保護。許多這些安全的 DNS resolvers 都可以免費使用，用戶可以透過更改本地 router 中的單個設置來切換到這些 recursive DNS services 之一。