# DNS
域名服務將域名轉換為計算機可讀的 IP Address

## What is DNS ?
DNS 通常被稱為 internet 的電話簿，當用戶在其瀏覽器中鍵入 URI 時，DNS 將該用戶與他們正在尋找的網站連接起來。DNS 代表域名系統，DNS 維護 internet 上每個網站的目錄。

計算機只能使用它的 IP address 找到一個網站，這是一個長的、間斷的數字字符串，例如較舊的 IPv4 格式的 192.168.1.1 或新的 IPv6 中的 2400：cb00：2048：1 :: c629：d7a2，這些地址對於人類來說很難記住，而且最重要的是，某些網站的 IP address 是動態的，並且可能會定期更改。DNS 使人們更容易訪問存取網站，方法是讓他們使用人性化的網址，也稱為 URL。

例如， kuas 的當前 IPv4 IP address 為 140.127.113.35 。用戶可以在瀏覽器中輸入 " www.kuas.edu.tw"，而不是記住該 IP address。當發生這種情況時，瀏覽器向 DNS 發出請求，並且 DNS 返迴回應，告知瀏覽器該網站的 IP address，然後瀏覽器向該 IP address 發送請求，該請求回應網站的數據。

## How does a DNS request work ?
![](https://www.cloudflare.com/img/learning/ddos/glossary/domain-name-system-dns/ddos-dns-request.png)

DNS 服務器設置在分散式層次結構中，這有著數據分散在多台伺服器上。當客戶端發出 DNS 請求時，請求由遞歸解析器處理，該解析器是一個 DNS 服務器，它啟動與其它 DNS 服務器的一系列通訊，直到找到所請求的 IP address，並將其返回給客戶端。遞歸解析器還可以暫存 DNS 記錄，使經常訪問的記錄更容易獲得。

## DNS and DDoS Attacks ?
有兩種流行的 DDoS 攻擊利用 DNS 服務器
- DNS amplification 攻擊
    - DNS amplification 攻擊是基於反射的 DDoS 攻擊，攻擊者向開放的DNS 服務器發送欺騙性查找請求，然後服務器將響應發送回目標受害者，攻擊被放大，因為攻擊者發送的請求數據小於受害者收到的回應數據。
- DNS flood 攻擊
    - 在 DNS flood 攻擊中，攻擊者試圖癱瘓特定區域的 DNS 服務器，企圖破壞到該區域的合法流量。這種類型的攻擊通常是透過使用 `botnet` 用查找請求來癱瘓 DNS 解析器來完成的。

## What else does the Domain Name System do ?

DNS 還定義了 DNS protocol，DNS protocol 是 DNS 中使用的通訊交換和數據結構的詳細規範。這屬於 Internet protocol 套件（TCP / IP）。此外，DNS 還維護著名的 `Blackhole ` IP address 列表，該列表已知用於發送垃圾郵件。可以根據此列表配置郵件服務器，以標記或拒絕懷疑是垃圾郵件的郵件。