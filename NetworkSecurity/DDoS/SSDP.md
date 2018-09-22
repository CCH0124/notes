# SSDP DDoS Attack
利用 Universal Plug and Play 漏洞的 DDoS 攻擊。
## What is a SSDP DDoS Attack ?
Simple Service Discovery Protocol (SSDP) 攻擊是一種基於 reflection 的 distributed denial-of-service (DDoS) 攻擊，它利用 Universal Plug and Play（UPnP）網路協議，向目標受害者發送大量流量，癱瘓目標的所有基礎架構並使其 Web 資源下線。

>這是一個免費工具，用於檢查您的公共 IP 是否有任何暴露的 SSDP 設備：檢查 [SSDP DDoS](https://badupnp.benjojo.co.uk/) 漏洞。

## How does a SSDP Attack work ?
在正常情況下，SSDP protocol 用於允許 UPnP 設備將其存在 broadcast  到網路上的其他設備。
例如
1. 當 UPnP 印表機連接到正常網路時，在接收到 IP 地址之後，印表機能夠透過向稱為 multicast 地址的特殊 IP 地址發送消息來將其服務通告給網路上的計算機。
2. 然後，multicast 地址告訴網路上的所有計算機有關新打印機的訊息，一旦計算機聽到關於印表機的發現消息，它就向印表機發出請求以獲得其服務的完整描述。
3. 然後，印表機直接回應該計算機，並提供其提供的所有內容的完整列表。 SSDP 攻擊透過要求設備回應目標受害者來利用最終的服務請求。

#### Here are the 6 steps of a typical SSDP DDoS attack：
1. 首先，攻擊者進行掃描，尋找可用作 amplification factors 的plug-and-play 設備。
2. 當攻擊者發現聯網設備時，他們會創建所有響應設備的列表。
3. 攻擊者使用目標受害者的 `spoofed IP address` 建立 `UDP` packet。
4. 然後，攻擊者使用 botnet 向每個 plug-and-play 設備發送欺騙性 discovery packet，並透過設置某些 flag（特別是 ssdp：rootdevice 或ssdp：all）請求盡可能多的數據。
5. 因此，每個設備都會向目標受害者發送回覆，其數據量最多可達攻擊者請求的 30 倍左右。
6. 然後，目標從所有設備接收大量流量並變得不堪重負，可能導致對合法流量的 `denial-of-service`。

![](https://www.cloudflare.com/img/learning/ddos/ssdp-ddos-attack/ping-of-death.png)

## How is a SSDP Attack mitigated ?
對於網路管理員來說，關鍵的緩解措施是阻止防火牆端口 1900 上的傳入 UDP 流量。如果流量不足以癱瘓網路基礎設施，則從此端口過濾流量可能會減輕此類攻擊。

[SSDP](https://blog.cloudflare.com/ssdp-100gbps/)

