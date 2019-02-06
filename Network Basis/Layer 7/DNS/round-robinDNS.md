# Round-Robin DNS
Round-Robin DNS 是一種負載平衡技術，涉及為單個域名使用多個不同的 IP 地址。

## What is round-robin DNS ?
`Round-Robin DNS` 是一種負載平衡技術，**其中平衡由一種稱為 `authoritative nameserver` 的 DNS 服務器完成**，而不是使用專用的負載平衡硬體。當網站或服務的內容託管在多個冗餘 Web 服務器上時，可以使用 `Round-Robin DNS`，當查詢 DNS `authoritative nameserver` 的 IP 地址時，服務器每次都會發出不同的地址，在輪換時進行操作。當冗餘 Web 服務器在地理上分離時，這尤其有用，這使得傳統的負載平衡變得困難。 Round-robin 以其易於實現而聞名，但它也有很大的缺點。

啟用了 Round-robin 的 DNS 服務器將具有多個不同的 A 記錄，每個記錄具有相同的域名但具有不同的 IP 地址。每次查詢 DNS 服務器時，它都會將最近響應的 IP 地址發送到隊列的後面，並在循環中運行。`Round-robin DNS` 服務器中的 IP 地址就像擊球陣容中的棒球運動員：每一個都轉彎，然後移動到線路的後面。

## What are the drawbacks of Round-Robin DNS ?
由於 DNS caching 和客戶端緩存，Round-robin 並不總是提供均勻分佈的負載平衡。如果用戶對特定網站的特定高流量 `recursive resolver` 進行 DNS 查詢，則該 resolver 將緩存網站的 IP，從而可能向該 IP 發送大量流量。

另一個缺點是 Round-robin 不能依賴於現場可靠性，如果其中一個服務器出現故障，DNS 服務器仍會將該服務器的 IP 保留在循環輪換中。因此，如果有 6 台服務器，其中一台脫機，則六分之一的用戶將被拒絕服務。此外，Round-robin DNS 不考慮服務器負載、事務時間、地理距離以及可以配置傳統負載平衡的其他因素。

一些高級 round-robin 服務有一些方法可以克服一些缺點，例如：能夠檢測到無響應的服務器並使它們脫離循環輪換，但是無法解決緩存問題。