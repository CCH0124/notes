# UDP Flood Attack
UDP flood 可能會癱瘓 server 和保護它的 firewall。
## What is a UDP flood attack ?
UDP flood 主要透過利用 server 回應發送到其中一個端口的 UDP packet 時所採取的步驟來工作。
在正常情況下，當 server 在特定端口收到 UDP packet 時，它會透過兩個步驟作為回應：
1. Server 首先檢查是否有正在運行的程序正在偵聽指定端口的請求。
2. 如果沒有程序在該端口接收 packet，則 server 使用 `ICMP`（ping）packet 進行回應，以通知發送方目標無法訪問。

可以在酒店接待員路由呼叫的上下文中考慮 UDP flood。首先，接待員接到電話呼叫，呼叫者要求連接到特定房間。接待員然後需要查看所有房間的列表，以確保客人可以在房間裡，並願意接聽電話。一旦接待員意識到客人沒有接聽任何電話，他們必須重新接聽電話並告訴來電者客人不接聽電話。如果突然所有電話線同時點亮同樣的請求，那麼它們很快就會變得不堪重負。

![](https://www.cloudflare.com/img/learning/ddos/udp-flood-ddos-attack/amplification-ddos-attack-metaphor.png)

當 server 接收到每個新的 UDP packet 時，它會透過步驟來處理請求，利用該過程中的 server 資源，發送 UDP packet 時，每個 packet 將包含來源設備的 IP address，在這種類型的 DDoS 攻擊中，攻擊者通常不會使用自己的真實 IP address，而是會欺騙 UDP packet 的來源 IP address，阻止攻擊者的真實位置被暴露，並可能使目標的 server 回應 packet 飽和。

由於目標 server 利用資源來檢查然後回應每個接收的 UDP packet，當收到大量 UDP packet 時，目標的資源可能會迅速耗盡，從而導致對正常流量的 denial-of-service。

![](https://www.cloudflare.com/img/learning/ddos/udp-flood-ddos-attack/udp-flood-attack-ddos-attack-diagram.png)

## How is a UDP flood attack mitigated ?
大多數作業系統限制 ICMP packet 的回應速率，部分是為了破壞需要 ICMP 回應的 DDoS 攻擊。這種類型的緩解的一個缺點是在攻擊期間也可以在該過程中過濾合法分組。如果 UDP flood 的量足夠大使目標 server 的防火牆的狀態表飽和，那麼在 server 級別發生的任何緩解都將是不充分的，因為瓶頸將在目標設備的上游發生。