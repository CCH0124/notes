# What are IP & TCP?
Internet Protocol （IP）是 internet 的 Address 系統，其核心功能是將訊息 packets 從來源設備傳送到目標設備，IP 是建立網路連接的主要方式，它建立了 internet 的基礎。IP 不處理 packet 排序或錯誤檢查，這種功能需要其他協議通常為 TCP。

TCP / IP 關係類似於藉由郵件向某人發送拼圖上寫的訊息，訊息被寫下來，拼圖被分成幾部分(物品)。然後每件作品都可以通過不同的郵政路線，其中一些會比其他郵件路線更長，當拼圖塊穿過它們的不同路徑後到達時，這些拼圖可能會出現故障，Internet Protocol 確保這些部分到達目的地址。TCP protocol 可以被認為是另一方面的拼圖彙編程序，它以正確的順序將各個部分組合在一起，要求重新發送丟失的部分，並讓發送者知道已收到拼圖，在發送最後一個拼圖之後，TCP 會在發送第一個拼圖之前保持與發件人的連接。

IP 是一種無連接協議，這表示每個數據單元都是單獨尋址的，並從來源設備路由到目標設備，目標不會將收到的訊息確認發送回來源。這就是傳輸控制協議（TCP）等協議的用武之地，TCP 與 IP 結合使用，以保持發送方和目標之間的連接，並確保數據包順序。

例如，藉由 TCP 發送電子郵件時，將建立連接並進行 3-way handshake。 

1. 來源向目標 sercer 發送 SYN "initial request" packet 以便開始對話
2. 目標 server 發送 SYN-ACK packet 以同意該過程
3. 來源向目標發送 ACK packet 以確認該過程，之後可以發送消息內容

在將每個 packet 發送到 internet 之前，電子郵件訊息最終被分解為 packet，其中它在到達目標設備之前遍歷一系列 gateways，其中該 packet 被 TCP 重新組裝成電子郵件的原始內容。

![Three - way handshake (TCP)](https://www.cloudflare.com/img/learning/cdn/tls-ssl/tcp-handshake-diagram.png)

如今 internet 上使用的 IP 的主要版本是 Internet protocol 版本 4（IPv4），由於 IPv4 中可能地址總數的大小限制，開發了一種較新的協議，較新的協議稱為 IPv6，它可以提供更多地址，並且正在逐步採用。