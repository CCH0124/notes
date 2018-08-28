# Ping of Death Attack
一種 DoS 攻擊，只透過發送異常大的 packet 來破壞 Web 伺服器。

## What is a Ping of Death attack ?
Ping of Death 攻擊是一種 `denial-of-service`（DoS）攻擊，其中攻擊者透過發送大於最大允許大小的 packet 來破壞目標伺服器，從而導致目標伺服器當機或崩潰。最初的 Ping of Death 攻擊在現在不太常見。一種稱為 `ICMP flood` 攻擊的相關攻擊更為普遍。

## How does a Ping of Death work ?
`Internet Control Message Protocol` (ICMP) echo-reply 或 "ping"，是用於測試網路連接的網路工具，它很像聲納，一個 "pulse" 被送出和 "echo" 從 pulse 告訴工作人員有關環境的訊息。如果連接正常，來源計算機將收到目標計算機的回覆。

雖然一些 ping packet 非常小，但 IP4 ping packet 比較大一點，並且可以與最大允許 packet 大小 65,535 bytes 一樣大。一些 `TCP/IP` 系統從未被設計為處理大於最大值的 packet，使得它們容易受到超過該大小的 packet 攻擊。

![](https://www.cloudflare.com/img/learning/ddos/ping-of-death-ddos-attack/attack-mitigation.png)

當惡意的 packet 從攻擊者傳輸到目標時，packet 會被分段為 segments，每個 segments 都低於最大大小限制。當目標伺服器嘗試將這些部分重新組合在一起時，總數超過了大小限制並且可能發生 buffer overflow，從而導致目標伺服器當機，崩潰或重新啟動。

雖然 ICMP echo 可用於此攻擊，但任何發送 IP packet 的內容都可用於此攻擊。這包括 `TCP`、`UDP` 和 IPX 傳輸。

## How is a Ping of Death DDoS attack mitigated ?
停止攻擊的一種解決方案是向重組過程添加檢查動作，以確保在 packet 重組後不會超過最大 packet 大小約束。另一種解決方案是建立一個具有足夠空間的 memory 緩衝區來處理超出最大值的 packet。

原來的 Ping of Death 攻擊大部分都是恐龍襲擊的方式，1998 年以後創建的設備通常受到這種攻擊的保護，一些傳統設備可能仍然易受攻擊。最近發現了針對Microsoft Windows 的 IPv6 packet 的新 Ping of Death 攻擊，並且在 2013 年中期進行了修補。