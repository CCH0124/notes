# SYN Flood Attack
SYN Flood 利用 TCP/IP handshake 中的漏洞試圖破壞 Web 服務。

## What is a SYN flood attack ?
SYN flood（half-open attack）是一種 denial-of-service (DDoS) 攻擊，只在透過消耗所有可用的伺務器資源使伺務器對合法流量不可用。透過重複發送初始連接請求（SYN）數據包，攻擊者可以淹沒目標服務器計算機上的所有可用端口，從而導致目標設備緩慢回應合法流量或根本不回應。

## How does a SYN flood attack work ?
SYN flood 攻擊透過利用 TCP 連線的 handshake 過程來工作。正常情況下，TCP 連線展示三個不同的過程以建立連接。
1. client 向 server 發送 SYN packet 以啟動連接。
2. server 使用 SYN/ACK packet 回應該初始數據包，以便確認通訊。
3. client 返回 ACK packet 以確認從 server 接收到 packet。完成此 packet 發送和接收序列後，TCP 連線將打開並能夠發送和接收數據。

![](https://www.cloudflare.com/img/learning/ddos/syn-flood-ddos-attack/syn-flood-attack-ddos-attack-diagram-1.png)

為了建立 `denial-of-service`，攻擊者利用這樣的事實：在收到初始 SYN packet 之後，server 將使用一個或多個 SYN/ACK packet 進行回應，並等待 handshake 中的最後一步。

以下是它的工作原理：
1. 攻擊者向目標 server 發送大量 SYN packet，通常使用 `spoofed`  IP 地址。
2. server 回應每個連接請求，並使開放端口準備好接收回應。
3. server 等待從未到達的最終 ACK packet 時，攻擊者繼續發送更多的SYN packet，每個新 SYN packet 的到達使服務器暫時維持一個新的開放端口連接一段時間，並且一旦利用了所有可用端口，服務器就無法正常運行。

![](https://www.cloudflare.com/img/learning/ddos/syn-flood-ddos-attack/syn-flood-attack-ddos-attack-diagram-2.png)

在網路中，當 server 打開連接但連接另一端的機器不打開時，連接被認為是半開放的。在這種類型的 DDoS 攻擊中，目標 server 不斷離開開放連線並等待每個連線超時，然後端口再次可用。結果是這種類型的攻擊可以被認為是 "**half-open attack**"。

SYN flood 可以以三種不同的方式發生：
1. Direct attack：
SYN flood 所在的 `IP address` 不被欺騙被稱為直接攻擊。在此攻擊中，攻擊者根本不會屏蔽其 IP address。由於攻擊者使用具有真實 IP address 的單一來源設備來建立攻擊，因此攻擊者極易受到發現和緩解。為了在目標機器上建立半開狀態，黑客阻止他們的機器回應 server 的 SYN-ACK packet。這通常透過防火牆規則來實現，該規則可以阻止除 SYN packet 之外的傳出 packet，或者在傳輸到的任何 SYN-ACK packet 到達惡意用戶計算機之前將其過濾掉。實際上，這種方法很少使用（如果有的話），因為緩解是相當簡單的，只是阻止每個惡意系統的 IP address。如果攻擊者使用 `botnet` 例如：Mirai botnet，他們不會關心屏蔽受感染設備的 IP。
2. Spoofed Attack：
惡意用戶還可以欺騙他們發送的每個 SYN packet 上的 IP address，以便抑制緩解工作並使其身份更難以發現。雖然數據包可能是欺騙性的，但這些數據包可能會被追溯到其源頭。這種偵探工作很難做到，但這並非不可能，特別是如果互聯網服務提供商（ISP）願意提供幫助的話。
3. Distributed attack (DDoS)：
如果使用 botnet 建立攻擊，則將攻擊追溯到其源的可能性很低。對於增加的混淆級別，攻擊者可能使每個分佈式設備也欺騙其發送數據包的 IP address。如果攻擊者使用像是 Mirai botnet 之類的殭屍網絡，他們通常不會關心屏蔽受感染設備的 IP。

透過使用 SYN flood 攻擊，不良行為者可以嘗試在目標設備或服務中創建 `denial-of-service`，其流量遠低於其他 DDoS 攻擊，只在使目標周圍的網路基礎設施飽和的 volumetric 攻擊不僅僅是針對目標操作系統中可用 backlog 的 SYN 攻擊。如果攻擊者能夠確定 backlog 的大小以及每個連線在超時之前保持打開的時間長度，攻擊者可以定位禁用系統所需的確切參數，從而將總流量減少到建立所需的最小量拒絕服務。

>體積型的攻擊（Volumetric Attacks）：對於大量佔用目標物或服務端網路頻寬的 DDoS 攻擊，他們稱為體積型的攻擊。

## How is a SYN flood attack mitigated ?
SYN flood 漏洞已為人所知很長時間，並且已經使用了許多緩解途徑。
- Increasing Backlog queue
目標設備上的每個作業系統都有一定數量的 half-open 連接。對大量 SYN packet 的一種回應是增加作業系統允許的最大可能的 half-open 連接數。為了成功增加最大 Backlog，系統必須保留額外的內存資源來處理所有新請求。如果系統沒有足夠的內存來處理增加的 Backlog queue 大小，系統性能將受到負面影響，但這仍然可能比拒絕服務更好。
- Recycling the Oldest Half-Open TCP connection
另一個緩解策略涉及在填充 Backlog 後覆蓋最舊的 half-open 連接。這種策略要求合法連接可以在比 Backlog 可以填充惡意 SYN packet 的時間更短的時間內完全建立。當攻擊量增加時，或者如果 Backlog 大小太小而不實用，則此特定防禦失敗。
- SYN cookies
此策略涉及 server 建立 cookie。為了避免在填充 Backlog 時丟失連線的風險，server 使用 SYN-ACK packet 回應每個連線請求，但隨後從 Backlog 中刪除 SYN 請求，從內存中刪除請求並使端口保持打開狀態準備建立新的連接。如果連接是合法請求，並且最終 ACK packet 從客戶端機器發送回服務器，則服務器將重建（有一些限制）SYN Backlog queue 條目。雖然這種緩解措施確實會丟失有關 TCP 連線的一些信息，但它最好是允許合法用戶因攻擊而發生 denial-of-service。

## Ref
[SYN](https://www.itread01.com/articles/1476528936.html)
