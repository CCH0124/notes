# What Is ICMP ?
Internet Control Message Protocol 用來診斷 internet 上網路問題

## What is the Internet Control Message Protocol (ICMP) ?
Internet Control Message Protocol 是網絡設備用於診斷網路通訊問題的網路層協議。ICMP 主要用於確定數據是否及時到達預定目的地。通常，ICMP 協議用於網路設備，例如：routers。

## What Is ICMP Used For ?
ICMP 的主要目的是用於錯誤報告。當兩個設備透過 Internet 連線時，如果任何數據未到達其預期目的地，則 ICMP 會生成錯誤以與發送設備共享。

ICMP 協定的第二個用途是執行網絡診斷。常用的終端應用程序 traceroute 和 ping 都使用 ICMP 進行操作，traceroute 應用程序用於顯示兩個 Internet 設備之間的路由路徑，路由路徑是請求在到達目的地之前必須透過的已連接路由器的實際物理路徑。一個路由器和另一個路由器之間的旅程稱為 "hop"，且 traceroute 還報告沿途每個躍點所需的時間，這對於確定網絡延遲的來源非常有用。

ping 應用程序是 traceroute 的簡化版本。ping 將測試兩個設備之間的連線速度，並準確報告數據包到達目的地所需的時間並返回發送方的設備，儘管 ping 不提供有關 routing 或 hop 的數據，但它仍然是衡量兩個設備之間延遲的非常有用的指標。ICMP echo-r​​equest 和 echo-r​​eply 訊息通常用於執行 ping，可怕的是，網路攻擊可以利用此過程，建立中斷手段，如：`ICMP Flood` 攻擊和 `Ping of Death` 攻擊。

ICMP Flood Attack：
![](https://www.cloudflare.com/img/learning/ddos/ping-icmp-flood-ddos-attack/ping-icmp-flood-ddos-attack-diagram.png)