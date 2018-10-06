# DNS
DNS 允許用戶使用 domain 而不是 IP address 連接到網站。

## What is DNS ?
Domain Name System（DNS）是 Internet 的電話簿。人類透過 domain 線上訪問訊息，如 `edu.tw` 或 `espn.com`。Web 瀏覽器透過 Internet protocol（IP）地址進行交互。DNS 將 domain 轉換為 IP address，以便瀏覽器可以加載 Internet 資源。

連接到 Internet 的每個設備都有一個唯一的 IP address，其它計算機可以使用該 IP address 來查找設備。DNS server 無需人類記憶 IP address，如192.168.1.1 或更複雜的 IPv6 address，如 2400：cb00：2048：1 :: c629：d7a2。

![](https://www.cloudflare.com/img/learning/dns/what-is-dns/theinternet-dns.svg)

## How does DNS work ?
DNS 解析過程涉及將主機名，例如：www.example.com 轉換為計算機友好的 IP address，例如：192.168.1.1。Internet 上的每個設備都會獲得一個 IP address，該地址是查找相應 Internet 設備所必需的，就像使用街道地址查找特定的家庭一樣。當用戶想要加載網頁時，必須在用戶輸入其 Web 瀏覽器（example.com）和找到example.com 網頁所需的機器友好地址之間進行翻譯。

為了理解 DNS resolution 背後的過程，了解 DNS query 必須在其間傳遞的不同硬件組件非常重要。對於 Web 瀏覽器，DNS query 發生除了初始請求之外，不需要來自用戶計算機的交互。

## There are 4 DNS servers involved in loading a webpage
- DNS recursor
  - 可以將 recursor 視為圖書管理員，要求他在圖書館的某處找到某本書。DNS recursor 是一個 server，只在透過 Web 瀏覽器等應用程序從客戶端計算機接收查詢。通常，recursor 負責發出其它請求以滿足客戶端的 DNS query。
- Root nameserver
  - `root server` 是將人類可讀主機名轉換（解析）為 IP address 的第一步。它可以被認為是圖書館中指向不同書籍的書籍索引，通常它可以作為對其它更具體位置的參考。
- TLD nameserver 
  - Top level domain server（TLD）可以被視為圖書館中特定的書架。此 nameserver 是搜索特定 IP address 的下一步，它託管主機名的最後一部分（在example.com 中，TLD server 是 "com"）。
- Authoritative nameserver
  - 這個最終的 nameserver 可以被認為是書架上的字典，其中可以將特定名稱翻譯成其定義。authoritative nameserver 是 nameserver query 中的最後一站。如果 authoritative nameserver 可以訪問所請求的記錄，它將把請求的主機名的 IP address 返回給發出初始請求的 DNS Recursor（圖書管理員）。

## What's the difference between an authoritative DNS server and a recursive DNS resolver ?
這兩個概念都指的是與 DNS 基礎結構不可分割的 server（groups of servers），但每個 server 執行不同的角色並且位於 DNS query 管道內的不同位置。考慮差異的一種方法是 recursive resolver 位於 DNS query 的開頭，而 authoritative  nameserver 位於最後。

#### Recursive DNS resolver
Recursive resolver 是響應來自客戶端的遞歸請求並花時間追蹤 `DNS record` 的計算機。它透過發出一系列請求直到它到達請求記錄的 authoritative DNS nameserve（或者如果沒有找到記錄則超時或返回錯誤）來完成此操作。幸運的是，recursive DNS resolvers 並不總是需要發出多個請求才能追蹤響應客戶端所需的記錄，緩存是一種數據持久性過程，它透過在 DNS 查找中提前提供請求的資源記錄來幫助縮短必要的請求。

![](https://www.cloudflare.com/img/learning/dns/what-is-dns/dns-record-request-sequence-1.png)

#### Authoritative DNS server
簡而言之，authoritative DNS server 是實際持有並負責 DNS resource records 的 server。這是 DNS lookup 鏈底部的 server，它將使用查詢的 resource records 進行響應，最終允許 Web 瀏覽器發出請求以訪問訪問網站或其他 Web 資源所需的 IP address。authoritative nameserver 可以滿足來自其自身數據的查詢，而無需查詢其它來源，因為它是某些 DNS records 的最終真實來源。
![](https://www.cloudflare.com/img/learning/dns/what-is-dns/dns-record-request-sequence-2.png)

值得一提的是，在查詢針對 subdomain （如 foo.example.com 或 blog.cloudflare.com）的情況下，將在 authoritative nameserver 之後添加一個額外的 nameserver，該 nameserver 負責存儲 subdomain 的 `CNAME` 記錄。

![](https://www.cloudflare.com/img/learning/dns/what-is-dns/dns-record-request-sequence-3.png)

## What are the steps in a DNS lookup ?
在大多數情況下，DNS 涉及將 domain name 轉換為適當的 IP address。要了解此過程的工作原理，有助於在從 Web 瀏覽器到 DNS 查找過程再次返回時遵循 DNS 查找的路徑。

>DNS 查找訊息通常會在查詢計算機內部**緩存**或在 DNS 基礎結構中**遠程緩存**。DNS 查找通常有8個步驟。緩存 DNS 訊息時，將從 DNS 查找過程中跳過步驟，這樣可以更快地完成。

示例沒有緩存任何內容時的所有 8 個步驟：
1. 用戶在 Web 瀏覽器中鍵入 "example.com"，查詢將進入 Internet 並由 DNS recursive resolver 程序接收。
2. resolver 然後查詢 DNS root nameserver（.）。
3. root server 使用 Top Level Domain （TLD）DNS server（例如 .com 或 .net）的 address 響應 resolver，該 server 存儲其 domain 的信息。
4. resolver 然後向 .com TLD 提出請求。
5. TLD server 使用 domain’s nameserver example.com 的 IP address 進行回應。
6. recursive resolver 向 domain’s nameserver 發送查詢。
7. example.com 的 IP address 將從 nameserver 返回到 resolver 程序。
8. DNS resolver 使用最初請求的 domain 的 IP address 回應 Web 瀏覽器。

一旦 DNS 查找的 8 個步驟返回了 example.com 的 IP address，瀏覽器就能夠發出對網頁的請求：

9. 瀏覽器向 IP address 發出 HTTP request 。
10. 該 IP 的 server 返回要在瀏覽器中呈現的網頁（步驟10）。

![](https://www.cloudflare.com/img/learning/dns/what-is-dns/dns-lookup-diagram.png)

## What is a DNS resolver ?
DNS resolver 是 DNS 查找中的第一站，它負責處理發出初始請求的客戶端。resolver 啟動查詢序列，最終讓 URL 被轉換為必要的 IP address。

>典型的未緩存 DNS 查找將涉及 recursive 和 iterative 查詢。

區分 recursive DNS query 和 recursive DNS resolver 很重要。
- recursive DNS query 
  - 是指對需要解析查詢的 DNS resolver 發出的請求。
- recursive DNS resolver
  - 接受 recursive query 並藉由發出必要請求來處理響應的計算機。

![](https://www.cloudflare.com/img/learning/dns/what-is-dns/dns-recursive-query.png)

## What are the types of DNS Queries ?
在典型的 DNS 查找中，會發生三種類型的查詢。透過使用這些查詢的組合，用於 DNS 解析的優化過程可以使行進距離的減少。在理想情況下，緩存記錄數據將可用，允許 DNS name server 返回 non-recursive query。

3 types of DNS queries：
1. Recursive query
在 recursive query 中，DNS 客戶端要求 DNS server（通常是 DNS recursive resolver）將使用請求的資源記錄回應客戶端，或者如果 resolver 無法找到記錄，則會響應錯誤消息。
![](https://i.imgur.com/TntvAUL.png)
2. Iterative query
在這種情況下，DNS 客戶端將允許 DNS server 返回它可以的最佳答案。如果 query 的 DNS server 與 query 名稱不匹配，則它將返回對 domain namespace 的較低級別具有權威性的 DNS server 的引用。然後，DNS 客戶端將對引用地址進行查詢。此過程將繼續使用查詢鏈中的其他 DNS server，直到發生錯誤或超時。
![](https://i.imgur.com/7LciyMW.png)
3. Non-recursive query
通常，當 DNS resolver 客戶端向 DNS server 查詢它有權訪問的記錄時，會發生這種情況，因為它對記錄具有權威性，或者記錄存在於其緩存中。通常，DNS server 將緩存 DNS records 以防止額外的 bandwidth 消耗和上游 server 的負載。

## What is DNS caching ? Where does DNS caching occur ?
緩存的目的是將數據臨時存儲在一個位置，從而提高數據請求的性能和可靠性。DNS 緩存涉及將數據存儲在更靠近請求客戶端的位置，以便可以更早地解析 DNS 查詢，並且可以避免在 DNS 查找鏈中進一步查詢，從而改善加載時間並減少 bandwidth、CPU 消耗。DNS 數據可以緩存在各種位置，每個位置將存儲 DNS 記錄一段時間，該時間由 `time-to-live`（TTL）決定。

#### Browser DNS caching
默認情況下，現代 Web 瀏覽器設計為在一段時間內緩存 DNS records。這裡的目的很明顯 DNS 緩存越接近 Web 瀏覽器，為了檢查緩存並對 IP address 發出正確的請求，必須採取的處理步驟越少。當請求 DNS records 時，瀏覽器緩存是為請求的記錄檢查的第一個位置。

>在 chrome 中，您可以轉到 chrome://net-internals/#dns 查看 DNS 緩存的狀態。

#### Operating system (OS) level DNS caching
Operating system DNS resolver 是 DNS 查詢離開計算機之前的第二個也是最後一個本地停止。設計用於處理此查詢的 operating system 內部的 process 通常稱為 "stub resolver" 或 DNS 客戶端。當 stub resolver 從應用程序獲取請求時，它首先檢查自己的緩存以查看它是否具有該記錄。如果沒有，則它將本地網路外部的 DNS 查詢（帶有 recursive flag set）發送到 Internet service provider（ISP）內的 DNS recursive resolver 。

#### Recursive resolver DNS caching
當 ISP 內部的 recursive resolver 收到 DNS 查詢時，如同之前的所有步驟一樣，它還將檢查所請求的主機到 IP address 轉換是否已儲存在其本地 persistence layer 內。

Recursive resolver 還具有其他功能，具體取決於它在緩存中的記錄類型：

1. DNS 緩存涉及將數據存儲在更靠近請求客戶端的位置，以便如果解析器沒有 A records，但確實擁有 authoritative nameservers 的 NS records，它將直接查詢這些 nameserver，繞過 DNS 查詢中的幾個步驟。 此快捷方式可防止從 root 和 .com nameserver（在我們的 example.com 搜索中）進行查找，並有助於更快的解析 DNS 查詢。可以提前解析 DNS 查詢，並且可以在 DNS 查找鏈中進一步查詢其他查詢。從而改善加載時間並減少 bandwidth/CPU 消耗。

2. 如果 resolver 沒有 NS records，它將向 TLD server 發送查詢（在我們的例子中為 .com），跳過 root server。
3. 萬一 resolver 沒有指向 TLD server 的記錄，它將查詢 root server。此事件通常在清除 DNS 緩存後發生。