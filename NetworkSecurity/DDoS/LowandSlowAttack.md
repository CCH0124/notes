# Low and Slow Attack
Low And Slow 攻擊是一種DDoS攻擊，旨在使用極慢的 HTTP 或 TCP 流量來阻止 Web 服務。

## What is a low and slow attack ?
Low and slow 攻擊是一種 `DoS` 或 `DDoS` 攻擊，它依賴於可以針對應用程式或服務器資源的非常慢的流量的小流，與更傳統的暴力攻擊不同，low and slow 攻擊需要非常少的 bandwidth 並且難以緩解，因為它們產生的流量很難與正常流量區分開來。因為它們不需要大量資源來啟動，所以可以使用一台計算機成功啟動 low and slow 攻擊，用於發動 low and slow 攻擊的兩種最流行的工具叫做 `Slowloris` 和 `R.U.D.Y`。

## How does a low and slow attack work ?
Low and slow 攻擊針對  thread-based 的 Web 服務器，目的是使用慢速請求捆綁每個線程，從而阻止真正的用戶訪問該服務，這是透過非常緩慢的傳輸數據來實現的，但速度足夠快以防止服務器超時。想想一條 4 車道的橋樑，每條車道都有一個收費站，司機拉到收費站，交出一張鈔票或一把硬幣，然後開過橋，打開車道到下一個司機。現在想像一下，四個司機同時出現並佔據每條開放式車道，同時他們各自慢慢的將錢交給收費站操作員，一次一枚硬幣，將所有可用車道堵塞數小時並防止其他司機通過。這種令人難以置信的令人沮喪的情況與 low and slow 攻擊的工作方式非常相似。

攻擊者可以使用 `HTTP` headers，HTTP 發布請求或 `TCP` 流量來執行 low and slow 攻擊。

以下是 3 個常見的攻擊示例：

- Slowloris 工具連接到 server，然後慢慢發送部分 HTTP 標頭。這會導致服務器保持連接打開，以便它可以接收其餘的標頭，從而佔用 thread。
- RUDY（RU-DEAD-YET ?）的工具生成 HTTP post 請求以填寫表單字段。它告訴 server 預期有多少數據，然後以非常緩慢的速度發送數據。服務器保持連線打開，因為它預計會有更多數據。
- 另一種類型的 low and slow 攻擊是 Sockstress 攻擊，它利用 TCP/IP 3-way handshake 中的漏洞，創建無限連接。

## How to stop a low and slow attack ?
用於阻止傳統 DDoS 攻擊的速率檢測技術不會發現 low and slow 攻擊。緩解 low and slow 攻擊的一種方法是升級 server 可用性，server 可以同時維護的連接越多，攻擊阻塞服務器的難度就越大。此方法的問題在於攻擊者可以嘗試擴展攻擊以滿足 server 的可用性。另一種解決方案是基於反向代理的保護，它可以在它們到達您的源服務器之前緩解 low and slow 攻擊。