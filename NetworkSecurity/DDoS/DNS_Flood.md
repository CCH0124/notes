# DNS Flood DDoS Attack
只在癱瘓目標 DNS 伺服器的攻擊。

## What is a DNS Flood ?
Domain Name System（`DNS`）伺服器是互聯網的"電話簿"，它們是 Internet 設備能夠透過其查找特定 Web 伺服器以訪問 Internet 內容的路徑。DNS flood 是一種 `distributed denial-of-service attack （DDoS）` ，攻擊者癱瘓特定 domain 的 DNS 伺服器以試圖破壞該 domain 的 DNS 解析。如果用戶無法找到電話簿，則無法查找該地址以撥打特定資源。透過破壞 DNS 解析，DNS flood 攻擊將危及網站 API 或 Web 應用程式回應合法流量的能力。DNS flood 攻擊很難與正常的繁忙流量區分開來，因為大量流量通常來自多個獨特位置，查詢域上的真實記錄，模仿合法流量。

## How does a DNS flood attack work ?
Domain Name System 的功能是在易於記憶的名稱（example.com）和難以記住的網站伺服器地址（192.168.0.1）之間進行轉換，因此成功攻擊 DNS 基礎設施使得大多數人無法使用互聯網。DNS flood 攻擊是一種相對較新的基於 DNS 的攻擊，隨著高 bandwith 物聯網（IoT）`botnet`（Mirai）的興起而激增。DNS flood 攻擊使用IP 攝像機、DVR 盒和其他物聯網設備的高 bandwith 連接直接淹沒主要提供商的 DNS 伺服器。來自 IoT 設備的請求量超過了 DNS 提供商的服務，並阻止合法用戶訪問提供商的 DNS 伺服器。

![DNS](https://www.cloudflare.com/img/learning/ddos/dns-flood-ddos-attack/dns-flood-ddos-attack-diagram.png)

DNS flood 攻擊與 `DNS amplification 攻擊`不同。與 DNS flood 不同，DNS amplification 攻擊反映和放大不安全的 DNS 伺服器上的流量，以隱藏攻擊的起源並提高其有效性。DNS amplification 攻擊使用 bandwith 較小的設備向不安全的 DNS 伺服器發出大量請求。這些設備對非常大的 DNS 記錄提出了許多小請求，但在發出請求時，攻擊者偽造的返回地址是預期受害者的地址。放大允許攻擊者僅使用有限的攻擊資源來獲取更大的目標。

## How can a DNS Flood attack be mitigated ?
DNS flood 代表了傳統的基於放大的攻擊方法的變化。借助易於訪問的高 bandwith 殭屍網絡，攻擊者現在可以瞄準大型組織，在可以更新或替換受損的 IoT 設備之前，抵禦這些類型攻擊的唯一方法是使用非常大且高度分佈的DNS 系統，該系統可以實時監控，吸收和阻止攻擊流量。