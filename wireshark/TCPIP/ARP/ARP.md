## What is ARP
ARP（Address Resolution Prtocol，位址解析通訊協定）根據 IP Address 取得實體位址的 TCP/IP 協定。在 OSI 裡 IP Address 屬於第三層，MAC Address 屬於第二層，層跟層之間不能相互直接通訊。再透過乙太網路發送 IP 資料包時，會先封裝第三層（32 位元 IP Address）和第二層（48 位元 MAC Address）的表頭。發送資料包時只知道目的地 IP 位址，卻不知道其 MAC，且層與層之間不能跨越，所以才需要一個 ARP 的協定完這件事。 
ARP 的功能就是將已知的 IP Address，解析成 MAC Address 進行主機跟主機之間的溝通。
## ARP operation

![topology](https://github.com/CCH0124/Wireshark-Operating/blob/master/Image/arp/ARP-topo.png "topology")

當 R1 想找 PC1，但 R1 什麼都不知道，只認識自己。R1 發送 broadcast 發出一個 ARP Request，ARP Request 包含了 PC1 的 IP Address 等，到了 Switch1 進行 flooding 使每個目的端 Host 都接收（因為是broadcast）。接著，目的端都接收，會進行 ARP reply 的動作，只有符合 R1 要找的 IP 才會進行回覆以 unicast方式，其餘的 Host 會把此 frame Drop 掉，之後 R1 會把找到的 Host MAC 給記錄到 ARP 表格。ARP 表格過一段時間會自動清除，清除後它又不認的對方是誰了，就會再進行一次 ARP Request。

>可以想像 R1 是授課老師，PC 則是學生。開學，學生又是新生，授課老師不認識大家，第一天上課老師想找小明（當 PC1），但老師只知道自己，於是授課老師用廣播的方式讓全班人都聽到他要找小明，接著小明聽到了授課老師的請求，於是他回覆了教授。接著授課老師，把小明記在腦中，接著過了一星期授課老師又忘記小明在哪了，於是授課老師重複了之前的步驟。

## Example

利用上面拓樸，設定好 R1 IP Address 和 PC1、2、3 IP Address 和 gateway。

1.

只認得自己

```bash
R1#show arp
Protocol Address Age (min) Hardware Addr Type Interface
Internet 192.168.10.1 -   ca01.07ae.0000 ARPA FastEthernet0/0
```
2.

使用 Wireshark 監聽 R1 f0/0 介面

3.

R1 ping PC1

```bash
R1#ping 192.168.10.10
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 192.168.10.10, timeout is 2 seconds:
.!!!!
Success rate is 80 percent (4/5), round-trip min/avg/max = 4/7/12 ms
```
4.

查看 Wireshark 介面，其中第 9、10 就是 ARP 封包
!["wireshark"](https://github.com/CCH0124/Wireshark-Operating/blob/master/Image/arp/wireshark_ARP.png "wireshark")

5.

查看第 9 個 Frame，ARP request
!["Frame9"](https://github.com/CCH0124/Wireshark-Operating/blob/master/Image/arp/wireshark_ARP2.png "Frame9")

```bash
Ethernet
Destination：ff:ff:ff:ff:ff:ff (broadcast) for ARP request
Source：ARP requester
Type：
ARP request/reply: 0x0806
RARP request/reply: 0x8035
IP datagram: 0x0800
Address Resolution Protocol （request）
Hardware type: 1 for ethernet
Protocol type: 0x0800 for IP (0000.1000.0000.0000)
同樣的 Ethernet 標頭攜帶 IP datagram!
Hardware len = length in bytes of hardware addresses (6 bytes for ethernet)
Protocol len = length in bytes of logical addresses (4 bytes for IP)
ARP operation（Opcode）:
1=request
2=reply
RARP
3=request
4=reply
```
查看第 10 個 Frame，ARP reply
!["Frame 10"](https://github.com/CCH0124/Wireshark-Operating/blob/master/Image/arp/wireshark_ARP3.png "Frame 10")

6.
R1 查看 ART Table，發現 R1 已經學習到 PC1
```bash
R1#show arp
Protocol Address Age (min) Hardware Addr Type Interface
Internet 192.168.10.1 - ca01.07ae.0000 ARPA FastEthernet0/0
Internet 192.168.10.10 0 0050.7966.6800 ARPA FastEthernet0/0
```
7.
檢查是否與 PC1 MAC 相同
```bash
VPCS> show ip/all
NAME   IP/MASK         GATEWAY            MAC           DNS
VPCS1 192.168.10.10/24 192.168.10.1 00:50:79:66:68:0
```


