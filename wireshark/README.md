# Wireshark-Operating
操作 wireshark 文件

## Configuration profile
使用者可以定義出好幾組彼此不同的偏好設定，並個別儲存

組態設定檔包含以下資訊：
- 偏好設定
- 捕捉用的篩選器
- 顯示用的篩選器
- 分色規則
- 關閉不用的協定
- 強制解碼
- 最近的設定值，視窗大小、欄位寬度等等
- 與協定有關的表單，SNMP 使用者和自訂的 HTTP 表頭

## Setting Packe List


## Capture Option

**Use multiple file**
- Next file every
  - 每個檔案捕捉大小為 2 MB（kilobyte、gigabyte）
  - 每 1 minute（second、hour、day） 產生一個檔案
- Ring buffer with
  - 捕捉完第 2 個檔案時，將刪除第 1 個捕捉的檔案。並建立第 3 個檔案，繼續執行
  
## Capture skills

**MAC/IP**
- host {IP Address}
  - 捕捉到達/來自 {IP Address} Host 資料
- not host {IP Address}
  - 不捕捉來自/到達 {IP Address} 的資料
- src host {IP Address}
  - 捕捉來源來自 {IP Address} Host 上資料
- dst host {IP Address}
  - 捕捉目的來自 {IP Address} Host 上資料
- host {Domain}
  - 捕捉解析 {Domain} 的 IP 位址上的資料
  
**IP Address**

- net {IP Address/netmask}
  - 捕捉到達/來自 {IP Address/netmask} 網路中任何資料
- ip6 net {IP Address}
  - 捕捉到達/來自 {IP Address} 網路中任何資料
- not dst net {IP Address/netmask}
  - 捕捉目的為 {IP Address/netmask} 之外的資料
- dst net {IP Address/netmask}
  - 捕捉目的來自 {IP Address/netmask} Host 上資料
- src net {IP Address/netmask}
  - 捕捉來源來自 {IP Address/netmask} Host 上資料
  
**Broadcast**
- ip broadcast
  - 捕捉 255.255.255.255 的資料
- ip multicast
  - 捕捉透過 239.255.255.255 ~ 224.0.0.0 的資料
- dst host ff02::1
  - 捕捉 Host 到目的 ff02::1 廣播位址的資料
- dst host ff02::2
  - 捕捉路由到目的 ff02::2 廣播地址的資料
  
**MAC**
- ether host {Host MAC Address}
  - 捕捉到達來自此 Host MAC Address 的資料
- ether src {Host MAC Address}
  - 捕捉來源為 Host MAC Address 的資料
- ether dst {Host MAC Address}
  - 捕捉目的地為 Host MAC Address 的資料
- not ether host {Host MAC Address}
  - 捕捉除了此 Host MAC Address 之外的流量
  
**Port**
- port 53
  - 捕捉來自/到達 Port 53 的 UDP/TCP 資料
- not port 53
  - 捕捉除了來自/到達 Port 53 的 UDP/TCP 以外的資料
- udp port 67
  - 捕捉來自/到達 Port 67 的 UDP 資料
- tcp port  21
  - 捕捉來自/到達 Port 21 的 TCP 資料
- portrange 1-80
  - 捕捉來自/到達 Port 1-80 的 UDP/TCP 資料
- tcp porttange 1-80
  - 捕捉來自/到達 Port 1-80 的 TCP 資料

**IP address range**
- ip.addr > 10.3.0.1 && ip.addr < 10.3.0.5
  - 顯示到達 / 來自 IP 位址 10.3.0.2 ~ 10.3.0.4 的位址資料

**Subnet IP**
- ip.addr == 192.168.25.0/24
  - 捕捉所有這範圍的資料
  
## 處裡捕捉的檔案
### 合併捕捉的檔案
### 尋找封包
`ctrl + f` 開啟尋找封包的工具列。
![search packet](https://github.com/CCH0124/Wireshark-Operating/blob/master/Image/arp/Search%20Packet.png)

三種尋找封包：
- Display filter 輸入表示式，如 not ip or arp
- Hex Value 依指定的 16 進位數值搜尋，如 ff:ff:ff:ff
- String 依照指定的字串來搜尋，如 domain
> 如果要找出下一個符合條件的封包，使用 ctrl+N；往回找前一個 ctrl+B

### 標記封包
可能想把你特別注意的封包標記起來。

>`ctrl+M`，在按一次則取消。檢視下一個被標記的封包 `shift+ctrl+N`，檢視上一個則是 `shift+ctrl+B`
