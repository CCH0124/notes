# Primary Vs Secondary DNS
`Primary DNS` 服務器承載控制 `zone` 檔案，而 `Secondary DNS` 服務器用於 reliability（可靠性）和 redundancy（冗餘）。
## What are primary and secondary DNS servers ?
設置 DNS 服務器時，服務器管理員可以**選擇是將 DNS 服務器指定為 Primary 服務器還是 Secondary 服務器（也稱為 slave 服務器）**。在某些情況下，服務器可以是一個 zone 的 Primary 服務器，也可以是另一個 zone 的 Secondary 服務器。

Primary 服務器託管控制 zone 檔案，該**檔案包含域的所有權威信息**（這表示它是重要訊息的可信來源，例如：域的 IP 地址）。這包括重要訊息，例如域的 IP 地址以及負責該域管理的人員，Primary 服務器直接從本地文件獲取此信息。**只能在 Primary 服務器上更改區域的 DNS 記錄，然後主服務器才能更新 Secondary 服務器**。

**Secondary 服務器包含 zone 檔案的只讀副本，它們透過稱為區域傳輸的通訊從主服務器獲取其信息**。**每個區域只能有一個 Primary DNS 服務器，但它可以有任意數量的 Secondary DNS 服務器**。無法在 Secondary 服務器上更改區域的 DNS 記錄，但在某些情況下，Secondary 服務器可以將更改請求傳遞到 Primary 服務器。

## Why have a secondary DNS server ?
Primary DNS 服務器包含所有相關資源記錄，並且可以處理域的 DNS 查詢，但標準（並且許多註冊商需要）至少具有一個 Secondary DNS 服務器，這些 Secondary 服務器的好處是它們在 Primary DNS 服務器關閉時提供冗餘，**並且它們還有助於將請求的負載分配到域**，以便 Primary 服務器不會過載，這可能導致 `denial-of-service`。他們可以使用 `round-robin DNS` 來實現這一點，**`round-robin DNS` 是一種負載平衡技術**，只在為群集中的每個服務器發送大致相等的流量。