# What Is UDP ?
UDP 是一種傳輸層通訊協定，是一種非常常見的語音和視頻流量協定。

## What is User Datagram Protocol (UDP/IP) ?
UDP 是一種在 Internet 上使用的通訊協定，特別是用於時間敏感的傳輸，例如：視頻播放或 `DNS` lookups。它藉由不要求 "handshake" 來加速通訊，允許在接收方同意通訊之前傳輸數據，這允許此協定非常快速的運行。

![](https://www.cloudflare.com/img/learning/ddos/glossary/user-datagram-protocol-udp/tcp-vs-udp.svg)

通常用於加載網頁內容的 [TCP]() 連接需要 "handshake"，其中接收方在發送數據之前需要有同意通訊的共識。UDP 將在未經確認的情況下發送數據，即變請求是有欺詐性。

UDP 沒有 TCP 的錯誤檢查和排序功能，最好在不需要錯誤檢查且速度很重要時使用，這種內置的可靠性缺乏是 UDP 有時被稱為 "不可靠數據協定" 的原因。

使用 UDP 的應用程序必須能夠容忍錯誤、丟失和重複，雖然這聽起來不太理想，但有幾種應用，其中更快、更不可靠的協議是最佳選擇。

## What Kind Of Services Rely On UDP ?
UDP 通常用於時間敏感的通訊，偶爾丟棄數據包比等待更好。語音和視頻流量使用此協定發送，因為它們都是時間敏感的，在處理某種程度的丟失上。例如：許多基於 internet 的電話服務使用的 VOIP（IP語音）藉由 UDP 運行，這是因為對於一個非常清晰但嚴重延遲的人而言，他們更喜歡使用電話交談，這也使 UDP 成為線上遊戲的理想協議。同樣，由於 DNS 和 NTP 服務器都需要快速有效，因此它們藉由 UDP 運行，包括 `DNS amplification ` 和 `NTP amplification` 在內 Volumetric `DDoS Attacks` 利用這些服務器的易受攻擊的實例，目的是使用　UDP　流量癱瘓目標。

![](https://www.cloudflare.com/img/learning/ddos/udp-flood-ddos-attack/udp-flood-attack-ddos-attack-diagram.png)