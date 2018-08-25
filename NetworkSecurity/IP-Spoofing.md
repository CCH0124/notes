# IP spoofing 
具有偽造來源地址的欺騙 IP packet 通常用於攻擊，目的是避免檢測。

## What is IP spoofing ?
IP spoofing 是創建具有修改過的來源位址的 Internet Protocol（IP）packet，以便隱藏發送者的身份，冒充另一個電腦系統，或兩者。這是一種不良參與者經常用來對目標設備或周圍基礎設施調用 `DDoS` 攻擊的技術。

發送和接收 IP packet 是 network computer 和其他設備通訊的主要方式，並且構成現代 internet 的基礎，所有 IP packets 都包含一個標頭，該標頭位於數據包主體之前，並包含重要的 routing information​​、source address。在普通 packets 中，來源 IP Address 是 packet 發送方的地址，如果 packet 已被欺騙，則來源地址將被偽造。

![](https://www.cloudflare.com/img/learning/ddos/glossary/ip-spoofing/ip-spoofing.png)

IP spoofing 類似於攻擊者將包裹發送給列出錯誤返回地址的人。如果收到包裹的人想要阻止發件人發送包裹，那麼阻止虛假地址中的所有包裹將無濟於事，因為返回地址很容易改變。相反的，如果接收者想要回應返回地址，則他們回應的包裹將去除真實發送者之外的某個地方。欺騙數據包地址的能力是許多 `DDoS` 攻擊利用的核心漏洞。

DDoS 攻擊通常會利用欺騙，其目標是在屏蔽惡意來源的身份的同時用流量壓倒目標，從而防止緩解工作，如果來源 IP Address 被偽造並且連續隨機化，則阻止惡意請求變得困難，IP 欺騙還使執法部門和網路安全團隊難以追蹤攻擊的實施者。

欺騙也用於偽裝成另一個設備，以便將回應發送到該目標設備。`NTP Amplification` 和 `DNS Amplification` 等 vulnerability attacks 利用此漏洞。修改來源 IP 的能力是 `TCP / IP` 設計所固有的，使其成為持續的安全問題。

與 DDoS 攻擊相關，還可以進行欺騙，目的是偽裝成另一個設備，以避開身份驗證並獲取或 "hijack" 用戶的 session。

## How to protect against IP spoofing (packet filtering)
雖然無法防止 IP spoofing，但可以採取措施阻止欺騙性 packets 滲透網路。針對欺騙的一種非常常見的防禦是入口過濾，在 BCP38（a Best Common Practice document）中概述。入口過濾是一種通常在網路邊緣設備上實現的 packets 過濾形式，它檢查傳入的 IP packets 並查看其源頭，如果這些 packets 上的源標頭與其來源不匹配，或者看起來很可疑，則會拒絕這些 packets，一些網路還實現出口過濾，其查看退出網路的 IP packets，確保這些數據包具有合法的源標頭，以防止網路中的某人使用 IP spoofing 發起出站惡意攻擊。