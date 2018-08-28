# Denial-of-Service (DoS)
Denial-of-Service 攻擊是惡意企圖用流量癱瘓 Web 性能以破壞其正常的操作。

## What is a denial-of-service attack ?
Denial-of-service（DoS）攻擊是一種網路攻擊，其中惡意行為者在透過中斷設備的正常運行來使伺服器或其他設備，使得用戶難以使用服務。DoS 攻擊通常藉由壓倒或癱瘓目標伺服器來處理請求，直到無法處理正常流量，從而導致對用戶的 Denial-of-service。DoS 攻擊的特點是使用一台伺服器發動攻擊。

`Distributed denial-of-service`（DDoS）攻擊是一種來自許多分散來源的 DoS 攻擊，例如：`botnet` DDoS 攻擊。

## How does a DoS attack work ?
DoS 攻擊的主要焦點是使目標伺服器的資源過度飽和，從而導致 Denial-of-service 於其它請求。DoS 攻擊的多個攻擊媒介可以按其相似性進行分組。

DoS 攻擊通常分為兩類：
- Buffer overflow attacks
    - memory 緩衝區溢出的攻擊類型可能導致伺服器佔用所有可用的硬碟空間、記憶體或 CPU 時間。這種形式的利用通常會導致行為遲緩，系統崩潰或其他有害的服務器行為，從而導致 Denial-of-service。
- Flood attacks
    - 藉由使用大量數據包使目標服務器飽和，惡意行為者能夠過度飽和服務器資源，從而導致 Denial-of-service。為了使大多數 DoS flood 攻擊成功，惡意行為者必須擁有比目標更多的可用 bandwidth。

## What are some historically significant DoS attacks ?
DoS 攻擊通常利用網路、軟體和硬體設計中存在的安全漏洞。但，這些攻擊變得不那麼普遍，因為 DDoS 攻擊具有更強的破壞性能力，並且在可用工具的基礎上相對容易製造。實際上，大多數 DoS 攻擊也可以轉化為 DDoS 攻擊。
- Smurf attack
    - 一種先前被利用的 DoS 攻擊，其中惡意行為者藉由發送欺騙的 packet 來利用易受攻擊網路的廣播地址，從而導致目標 IP address 癱瘓。
- Ping flood
    - 這種簡單的 Denial-of-service 攻擊基於使用 ICMP（ping）packet 癱瘓目標。此攻擊也可以用作 DDoS 攻擊。
- Ping of Death
    - 經常與 ping flood 攻擊相混淆，Ping of Death 攻擊包括向目標機器發送格式錯誤的 packet，導致系統崩潰等有害行為。

## How can you tell if a computer is experiencing a DoS attack ?
雖然將攻擊與其他網路連線錯誤或大量 bandwith 消耗分開可能很困難，但某些特徵可能表明攻擊正在進行中。

DoS 攻擊的指標包括：
- 非常慢的網路性能
    - 文件或網站的長時間的加載
- 無法加載特定網站
    - 你的網絡媒體資源
- 在同一網絡上的設備之間突然斷開連接

## What is the difference between a DDoS attack and a DOS attack ?
DDoS 和 DoS 之間的區別在於攻擊中使用的連接數。一些 DoS 攻擊，例如：`Slowloris` 之類的**低速和慢速**攻擊，可以在簡單性和最低要求下獲得它們的力量。

![](https://www.cloudflare.com/img/learning/ddos/glossary/dos-attack/dos-vs-ddos-attack.png)

![](https://www.cloudflare.com/img/learning/ddos/glossary/dos-attack/dos-vs-ddos-attack.png)

Dos 使用單一連接，而 DDoS 攻擊利用許多攻擊流量的來源，通常採用 `botnet` 的形式。一般來說，許多攻擊基本相似，可以嘗試使用多個惡意流量來源。