# Web Application Firewall (WAF)
WAF 在 Web 應用程序和 Internet 之間創建隔閡，這個盾牌可以幫助減輕許多常見的攻擊。

## What is a Web Application Firewall (WAF) ?
WAF 或 Web 應用程序防火牆藉由過濾和監視 Web 應用程序與 Internet 之間的 HTTP 流量來幫助保護 Web應用程序。它通常可以保護 Web 應用程序免受諸如 cross-site forgery、cross-site-scripting (XSS)，file inclusion 和 SQL injection。WAF 是協定第 7 層防禦（在`OSI模型`中），並非只在防禦所有類型的攻擊，這種攻擊緩解方法通常是一套工具的一部分，這些工具共同創建了針對一系列攻擊媒介的整體防禦。

藉由在 Web 應用程序前部署 WAF，在 Web 應用程序和 Internet 之間放置一個隔閡。雖然代理服務器通過使用中介的方式來保護客戶端電腦的身份，但 WAF 是一種反向代理，藉由讓客戶端在​​到達服務器之前通過 WAF 來保護服務器免受暴露。

WAF 藉由一組通常稱為「策略」的規則來運行。這些策略只在藉由過濾惡意流量來防範應用程序中的漏洞。WAF 的價值部分來自於實施政策修改的速度和難易程度，可以更快的回應不同的攻擊媒介，在 `DDoS` 攻擊期間，可以藉由修改 WAF 策略快速實現速率限制。

![](https://www.cloudflare.com/img/learning/ddos/glossary/waf/waf.png)

## What is the difference between blacklist and whitelist WAFs ?

基於黑名單（negative security model）運行的 WAF 可防止已知攻擊。想想一個黑名單 WAF 作為俱樂部保鏢被指示拒絕接納不符合著裝要求的客人。相反，基於白名單（positive security model）的 WAF 僅允許預先批准的流量。這就像專屬派對的保鏢一樣，他或她只承認名單上的人。黑名單和白名單都有其優點和缺點，這就是許多 WAF 提供混合安全模型的原因，它實現了兩者。

## What are network-based, host-based, and cloud-based WAFs ?

WAF 可以透過三種不同的方式實現，每種方式都有自己的優點和缺點：
- network-based WAF 通常是基於硬件的。由於它們是本地安裝的，因此可以最大限度的減少延遲，但基於網絡的 WAF 是最昂貴的選擇，還需要物理設備的存儲和維護。
- host-based  WAF 可以完全集成到應用程序的軟體中。該解決方案比基於網路的 WAF 便宜，並提供更多可定制性。基於主機的 WAF 的缺點是本地服務器資源的消耗，實現複雜性和維護成本，這些組件通常需要工做時間，並且可能成本高。
- cloud-based WAF 提供了一個非常容易實現的經濟實惠的選項，它們通常提供交鑰匙安裝，就像更改 DNS 以重定向流量一樣簡單。基於雲的 WAF 也具有最小的前期成本，因為用戶每月或每年支付安全即服務費用。基於雲的 WAF 還可以提供持續更新的解決方案，以防止最新的威脅，而無需在用戶端進行任何額外的工作或成本。基於雲的 WAF 的缺點是用戶將責任移交給第三方，因此 WAF 的某些功能可能是他們的黑盒子。
