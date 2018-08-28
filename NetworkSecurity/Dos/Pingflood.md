# Ping (ICMP) Flood DDoS Attack
使用 ICMP 請求癱瘓目標的 DDoS 攻擊。

## What is a Ping (ICMP) flood attack ?
ping flood 是一種 `denial-of-service` 攻擊，攻擊者試圖用 ICMP 回應和請求 packet 癱瘓目標設備，導致目標無法被正常流量訪問。當攻擊流量來自多個設備時，攻擊將成為 `DDoS` 或 distributed denial-of-service 攻擊。

## How does a Ping flood attack work ?
用於 Ping Flood 攻擊的 `Internet Control Message Protocol（ICMP）` 是網路設備用於通訊的 Internet 層協定。網路診斷工具 [traceroute](https://www.wikiwand.com/en/Traceroute) 和 [ping](https://www.wikiwand.com/en/Ping_(networking_utility)) 都使用 ICMP 進行操作。通常，ICMP echo-r​​equest 和 echo-r​​eply 消息用於 ping 網路設備，以便診斷設備的健康和連接以及發送器和設備之間的連線。

ICMP 請求需要一些服務器資源來處理每個請求並發送回應封包。該請求還要求傳入消息（echo-r​​equest）和傳出回應（echo-r​​eply）上的 bandwith。Ping Flood 攻擊的目的是癱瘓目標設備，回應**大量請求**和/或使用**假流量**超載網路連接的能力，藉由使用 `botnet` 中的許多設備針對具有 ICMP 請求的相同 internet 屬性或基礎設施組件，攻擊流量顯著增加，可能導致正常網路活動的中斷。從歷史上看，攻擊者經常會偽造一個**假 IP address**為了掩蓋發送的設備，利用現代 `botnet` 攻擊，惡意攻擊者很少看到掩蓋 bot IP 的必要性，而是依靠大量無欺騙機器人網路來使目標的資源飽和。

Ping（ICMP）Flood 的 DDoS 形式可分為兩個重複步驟：
1. 攻擊者使用多個設備向目標服務器發送許多 ICMP 回應請求 packet。
2. 目標服務器將 ICMP 回送 echo-r​​eply packet 作為回應，發送到每個請求設備的 IP address。

![](https://www.cloudflare.com/img/learning/ddos/ping-icmp-flood-ddos-attack/ping-icmp-flood-ddos-attack-diagram.png)

Ping Flood 的破壞性影響與對目標服務器的請求數成正比。與基於反射的 DDoS 攻擊（如： `NTP amplification` 和 `DNS amplification`）不同，Ping Flood 攻擊流量是對稱的，目標設備接收的 bandwith 量只是每個機器人發送的總流量的總和。

## How is a Ping flood attack mitigated ?
透過禁用目標路由器、計算機或其他設備的 ICMP 功能，可以最輕鬆地禁用 ping flood。網路管理員可以訪問設備的管理界面並禁用其使用 ICMP 發送和接收任何請求的能力，從而有效地消除了請求的處理和 Echo Reply。這樣做的結果是所有涉及 ICMP 的網路活動都被禁用，使設備對 ping 請求，traceroute 請求和其他網路活動沒有回應。