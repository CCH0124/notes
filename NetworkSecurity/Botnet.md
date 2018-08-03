# Botnet
## What is Botnet?
Botnet 指的是一群已被 malware 感染並受惡意行為者控制的電腦。術語 Botnet 是機器人和網路這個詞混合而成，每個受感染的設備都被稱為 bot。Bonet 網路可以設計用於完成非法或惡意任務，包括 [spam](https://en.wikipedia.org/wiki/Email_spam)、
stealing data、[ransomware](https://www.avg.com/en/signal/what-is-ransomware)、欺詐性的點擊廣告或  distributed denial-of-service (DDoS) 攻擊。


雖然一些 malware（如 ransomware）會對設備擁有者產生直接影響，但 DDoS botnet malware 可以具有不同級別的可見性；一些 malware 指在完全控制設備，而其它 malware 作為後台 process 默默運行，同時靜靜的等待來自攻擊者或 "bot herde" 的指令。

自我傳播 botnets 透過各種不同的途徑招募額外的 bots，感染途徑包括利用 website vulnerabilities、Trojan horse malware 和 cracking weak authentication 以獲得遠程訪問，一旦獲得訪問權限，所有這些感染方法都會導致在目標設備上安裝 malware，從而允許 botnet 的操作員進行遠程控制。一旦設備被感染，它可能會嘗試透過招募周圍網路中的其它硬體設備來自我傳播 botnet malware。

雖然確定特定 botnets 中殭屍程序的確切數量是不可行的，但對複雜 botnets 中殭屍程序總數的估計範圍從幾千到一百多萬不等。

![](https://www.cloudflare.com/img/learning/ddos/what-is-a-ddos-botnet/ddos-botnet-attack-cropped.png)

## Why are botnets created ?
使用 botnet 的原因包括從激進主義到國家支持的破壞，許多攻擊只是為了獲利而進行的。在線上招聘 botnet 服務相對便宜，特別是與他們可能造成的損害程度有關。創建 botnet 的障礙也足夠低，使其成為某些軟件開發商的利潤豐厚的業務，特別是在監管和執法有限的地理位置，這種組合導致了提供針對僱傭攻擊的在線服務的激增。

## How is a botnet controlled ?

Botnet 的核心特徵是能夠從 bot herder 那裡接收更新的指令，與網路中的每個機器人通訊的能力允許攻擊者交替 attack vectors、更改目標 `IP address`、終止攻擊以及其他自定義操作。botnet 設計各不相同，但控制結構可分為兩大類：
- The client/server botnet model
- The peer-to-peer botnet model
### The client/server botnet model
client/server model 模仿傳統的遠程工作站工作流，其中每台機器連接到中央服務器（或少量集中式服務器）以訪問訊息。在此模型中，每個機器人將連接到 command-and-control center （CnC）資源，如 Web domain 或IRC channel，以便接收指令。藉由使用這些集中式存儲庫為 botnet 提供新命令，攻擊者只需修改每個 botnet 從命令中心消耗的源材料，以便更新受感染機器的指令。控制 botnet 的集中式服務器可以是攻擊者擁有或操作的設備，也可以是受感染的設備。

已經觀察到許多流行的集中式殭屍網絡拓撲，包括：

**Star Network Topology**
![](https://www.cloudflare.com/img/learning/ddos/what-is-a-ddos-botnet/ddos-botnet-star-network-topology.png)

**Multi Server Network Topology**
![](https://www.cloudflare.com/img/learning/ddos/what-is-a-ddos-botnet/ddos-botnet-multi-server-network-topology.png)

**Hierarchical Network Topology**
![](https://www.cloudflare.com/img/learning/ddos/what-is-a-ddos-botnet/ddos-botnet-hierarchical-network-topology.png)

在任何這些 client/server models 中，每個機器人將連接到 CnC，如 Web domain 或 IRC channel，以便接收指令。藉由使用這些集中式存儲庫為 botnet 提供新命令，攻擊者只需修改每個 botnet 從命令中心消耗的源材料，以便更新受感染機器的指令。

從有限數量的集中式源向 botnet 更新指令的簡單性是這些機器的漏洞；為了使用集中式服務器刪除 botnet，只需要中斷服務器。由於此漏洞，botnet malware 的創建者已經發展並轉向新模型，該模型不易受到單個或幾個故障點的干擾。

### The peer-to-peer botnet model
為了規避 client/server model 的漏洞，殭屍網絡最近被設計為使用分散的對等文件的組件，在 botnet 中嵌入控制結構消除了使用集中式服務器的殭屍網絡中存在的單點故障，使緩解工作更加困難。P2P 機器人可以是客戶端和命令中心，與其相鄰節點攜手傳播數據。

點對點殭屍網絡維護一個可信 host 表，用戶可以通過這些 host 提供和接收通信並更新其 malware。藉由限制機器人連接的其他機器的數量，每個機器人僅暴露給相鄰設備，使得更難追蹤並且更難以緩解。缺乏集中式命令服務器使得  peer-to-peer botnet 更容易受到 botnet 創建者以外的其他人的控制，為了防止失去控制，分散的殭屍網絡通常是加密的，因此訪問受到限制。

![](https://www.cloudflare.com/img/learning/ddos/what-is-a-ddos-botnet/ddos-botnet-peer-to-peer.png)

## How do IoT devices become a botnet ?
沒有人透過他們放在後院觀看餵鳥器的 wireless CCTV 攝像機進行網上銀行業務，但這並不意味著該設備無法提供必要的網絡請求。[IoT](https://en.wikipedia.org/wiki/Internet_of_things) 設備的強大功能加上弱配置或配置不當的安全性，為  botnet malware 招募新機器人進入集體創造了機會。物聯網設備的增長導致了 DDoS 攻擊的新局面，因為許多設備配置不當且容易受到攻擊。

如果物聯網設備的漏洞被硬編碼到 [firmware](https://en.wikipedia.org/wiki/Firmware) 中，則更新會更加困難，為降低風險，應更新具有過期 firware 的物聯網設備，因為默認配置通常在初始安裝設備時保持不變。許多折扣硬體製造商沒有受到激勵，使得他們的設備更安全，使 botnet malware 對物聯網設備造成的漏洞仍然是一個未解決的安全風險。

## How is an existing botnet disabled ?

### Disable a botnet’s control centers

一旦可以識別控制中心，就可以更容易的禁用 CnC 設計的 botnet。在故障點切斷領袖可以使整個 botnet offline。因此，系統管理員和執法官員專注於關閉這些 botnet 的控制中心，如果指揮中心在執法能力較差或不願干預的國家開發的業務，這一過程就更加困難，
### Eliminate infection on individual devices

對於個人電腦，重新獲得對電腦的控制權的策略包括運行防病毒軟件，從安全備份重新安裝軟件，或在重新格式化系統後從乾淨的電腦重新啟動。對於物聯網設備，策略可能包括 flashing the firmware，運行恢復出廠設置或以其他方式格式化設備。如果這些選項不可行，則可以從設備製造商或系統管理員處獲得其他策略。

## How can you protect devices from becoming part of a botnet ?

**Create secure passwords**

對於許多易受攻擊的設備，減少 botnet 漏洞的暴露可以像將管理憑據更改為默認用戶名和密碼之外的其他內容一樣簡單。創建安全密碼會使暴力破解變得困難，創建一個非常安全的密碼使得暴力破解幾乎不可能。例如，感染Mirai malware 的設備將掃描尋找回應設備的 IP address，一旦設備回應 ping 請求，機器人將嘗試使用預設的默認憑證列表登錄到找到的設備。如果已更改默認密碼並且已實施安全密碼，則機器人將放棄並繼續前進，尋找更易受攻擊的設備。

**Allow only trusted execution of third-party code**
如果您採用軟體執行的手機模型，則只能運行列入白名單的應用程序，授予更多控制權來殺死被視為惡意軟體的軟體，包括 botnets。僅利用管理程序軟體（即內核）可能導致對設備的利用，這取決於首先擁有安全內核，大多數物聯網設備都沒有，並且更適用於運行第三方軟體的機器。

**Periodic system wipe/restores**
在設定的時間後恢復到已知良好狀態將刪除系統收集的任何垃圾，包括 botnet 軟體，當用作預防措施時，此策略可確保即使是靜默運行的 malware 也會被丟棄。

**Implement good ingress and egress filtering practices**

其他更高級的策略包括網路路由器和防火牆的過濾實踐。

安全網路設計的原則是分層：您對可公開訪問的資源的限制最少，同時不斷增強您認為敏感的內容的安全性。此外，必須仔細檢查跨越這些邊界的任何事物：network traffic、USB drives 等。
質量過濾實踐增加了在進入或離開網路之前捕獲 DDoS malware 及其傳播和通訊方法的可能性。