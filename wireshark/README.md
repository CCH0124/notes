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

## 設定捕捉選項

Capture -> Options

![](https://i.imgur.com/PLfQfNI.png)

有三個主頁面：
- input
- output
- options

### input
![](https://i.imgur.com/PLfQfNI.png)

顯示可用來捕捉封包的介面，和該介面的基本資訊，如：OS 的介面名稱、該介面吞吐量折線圖、混雜模式、緩衝區大小等等。

### output
![](https://i.imgur.com/rqFxXWR.png)

指定捕捉到封包的自動儲存檔案方式，非先捕捉再個別存檔。
當捕捉大量的流量或是進行長時間持續的捕捉，採用一組檔案集合來儲存的方式會特別方便。若要啟動此功能需勾選 **Create a new file automatically after ...**，可以選擇每捕捉 1 MB 或每捕捉 1 分鐘的流量，建立檔案儲存。如果加上 **ring buffer** 選項，可控制輸出檔案的數量，當超過保留預設數量時，會從頭開始複寫檔案。

>檔案集合（file set），一群按照特定情況分組的檔案。

### options
這裡也是捕捉封包的選項，例如顯示方式、Name Resolution、自動結束捕捉等等。

![](https://i.imgur.com/c6ejTHQ.png)

#### Display Options
捕捉封包後的呈現方式。

- Update list of packet in real-time
  - 即時更新封包清單顯示
- Automatically scroll during live capture
  - 進行捕捉時自動自動滾動畫面
- Show extra capture information dialog
  - 顯示其它捕捉資訊對話框

>Update list of packet in real-time 和 Automatically scroll during live capture 對主機處理器造成負擔，非必要時應適當關閉。

#### Name Resolution
主要用來開啟 MAC、Network、Transport 等名稱解釋，讓捕捉的封包叫好解釋。

- Stop capture automatically after ...
  - 在...之後自動停止捕捉

## 使用篩選器
用於界定要分析的封包，簡而言之就是一個**表示式（expression）**

### 捕捉用篩選器
![](https://i.imgur.com/oz3Vw2p.png)

### 顯示用篩選器
![](https://i.imgur.com/mRe5R46.png)

### 顯示篩選器表示式對花框
點選 **Expression**

![](https://i.imgur.com/Tmj9a6G.png)

其中格式應該都是以 *protocol.feature.subfeature*

### 儲存篩選器
輸入且常用的捕捉篩選器，不用一直輸入，可以利用 wireshark 幫你儲存。
1. 點選 **Capture -> Capture Filters**，開啟捕捉篩選器視窗
2. 點選左下 **+** 選項，用以新增篩選器
3. 分別在 Name 欄位輸入篩選器名稱和在 Filter 欄位輸入表達式
4. 點選 **OK**，將它儲存

![](https://i.imgur.com/aOl87JN.png)

## 端點與會話
端點指的是網路上收發資料的個裝置；會話指的是兩個端點之間的通訊。在會話要如何以位址識別端點？

![](https://i.imgur.com/ukqMh2u.png)

圖中會話 A 有兩個端點在資料鏈接層通訊（MAC）。
圖中會話 B 有兩個端點在網路層通訊（IP）。

### 檢視端點統計
在分析流量時，會發現可以將問題精準的歸咎於網路中的某個端點。
點選 **Statistic -> Endpoint**，該視窗顯示多欄位的統計資料，如位址、封包數量、收發位元組數量等等。

![](https://i.imgur.com/Er0wTHC.png)


>想顯示其它的協定欄位，點擊 **Endpoint Types**，勾選想加入顯示的協定即可。
>在一個龐大的捕捉結果，可以在篩選器做篩選，在開啟 Endpoint 並勾選 **Limit to display filter**，會統計篩選過後的端點資訊。

### 檢視網路會話
點選 **Statistic -> Conversations**，該視窗顯示捕捉的每一組會話。這邊 Conversations 視窗在每一行顯示兩個位置，表示兩者間的會話。

![](https://i.imgur.com/iXfQcCO.png)

欄位 *Address A* 表示會話發起點，欄位 *Address B* 表示會話目的地。
透過檢視看到發送的封包流量，再透過篩選器去過濾。

>想顯示其它的協定欄位，點擊 **Conversations Types**，勾選想加入顯示的協定即可。

### 協定的階層式統計

對一個陌生的捕捉流量結果，有時必須借助流量中的協定分布狀況來判斷。透過 wireshark 的協定的階層式統計可以發掘 TCP、IP、DHCP 和其它協定的流量分別占用了多少。

點選 **Statistics -> Protocol Hierarchy** 如下圖

![](https://i.imgur.com/X5lHj0s.png)

其中 98.7% 都是 IPv4，TCP 96.7% 等等。或者可能 ARP 協定正常只有 1%，結果突然暴增 5%，代表一定有問題。

## 名稱解釋

端點間之所以可以交換資料，都是依靠數字和文字的系統，像是 MAC、IPv4、IPv6 等等。於是 wireshark 有個像 DNS 作用的功能，**名稱解釋（Name Resolution）**，可把較難記憶的 address 轉換方便記憶的 address。如 216.58.217.238 較難以計憶於是轉換成 google.com 使得較方便記憶。

### 啟用名稱解釋
點選 **Edit -> Preferences ->Name Resolution**
![](https://i.imgur.com/c7csNV2.png)

#### Resolve MAC address

其中 **Resolve MAC address** 試圖藉由 ARP 協定轉換成類似第三層的位址。如果都轉不成功，wireshark 會把前 3 個位元組轉換成 IEEE 指定的製造商名稱，如 Vmware_f3:ca:52 (00:50:56:f3:ca:52)

#### Resolve transport names
這個設定會嘗試把通訊 port 轉譯成熟知名稱，如 80 port 轉 http。在碰到不熟悉的或不常見的 port 可以使用此功能。

#### Resolve network addresses

此設定會把 192.168.1.100 第三層的位址轉換易懂的 DNS 名稱。透果此選項可以讓管理者較好辨識。

#### Use captured DNS packet data for address resolution

將捕捉到的 DNS 封包裡的資料做解析，藉以將 IP address 轉換 DNS 名稱。

#### Use an external network name resolver
讓 wireshark 直接詢問分析主機所仰賴的 DNS server，藉以將 IP address 轉換為 DNS 名稱。如果分析的捕捉結果裡缺乏 DNS 相關封包可參照，這種轉譯方式會比較好用。

#### Maximum concurrent requests
限制可以同時進行的 DNS 查詢動作數量。如果捕捉檔案結果在轉譯名稱時會產生大量 DNS 查詢請求，可能因此會占用太多網路或 DNS server 的可用頻寬，可以參考此選項。

#### Only use the profile hosts file
將 DNS 解釋範圍限制在 wireshark 的設定檔所知範圍內。

> Preferences 會將設定檔部分會儲存起來。暫時性設定可透過 **View -> Name Resolution**

### 名稱解釋的缺點
- 無 DNS server 協助將 IP address 轉換名稱，則轉譯就會失敗。
- 如果在不同網路上開起捕捉檔案，則系統可能無法取用原使網路的 DNS server，導致無法正確解釋捕捉檔的相關名稱。
- Use an external network name resolver 盡量關閉，避免讓駭客得知行蹤。

## 解析協定內容
wireshark 提供了一個 framework，讓你可以自行建立**封包解析器（protocol dissectors）**。wireshark 靠解析器來辨識協定並解釋（decode）成各個區塊。

### 更改解析器
假設把 FTP 預設 port 設 443 wireshark 極可能將此 port 誤認 SSL。要修正此問題須使用*強制解碼（forced decode）*以便正確分析。
1. 選一個誤判的 SSL 封包，點選右鍵選 **Decode As**
2. 在 Field 欄位改選 TCP port，value 輸入 443，current 欄位選 FTP 來解析所有 TCP 443 port 流量
![](https://i.imgur.com/BK7Y4op.png)
3. 設定完成後，會有不一樣的分析結果

## 追蹤串流
此功能從多個封包裡，把分散的資料在重整一致、容易判讀的格式，稱之**封包文本（packet transcript）**，這樣不必在用戶端與伺服器之間往來的封包片段中檢閱。

- TCP stream
  - 從使用 TCP 的協定中把資料組合起來，如 FTP
- UDP stream
  - 從使用 UDP 的協定中把資料組合起來，如 DNS
- SSL stream
  - 從已加密的協定中把資料組合起來，如 HTTPS。
  - 必須要有密鑰才能將流量解密
- HTTP stream
  - 從 HTTP 協定中把資料解壓縮及組合起來。
  - 利用 TCP stream 追蹤 HTTP 資料，無法把 HTTP 完全解碼時，需要此模式 
### 追蹤 SSL 串流
SSL 串流內容都是已經加密，必須要拿到伺服器端當初替流量加密的**私鑰（private key）**。取得此私鑰要看伺服器所採用的加密技術而定。

wireshark 運用：
1. 點選 **Edit -> preferences**，進入 wireshark 的偏好設定
2. 展開 **protocol**，選 **SSL** 協定。
![](https://i.imgur.com/7QFJhIz.png)
3. 點選 **RSA key list** 的 **Edit** 並按 **+**
4. 填入負責加密的伺服器 IP address、Port、protocol、密鑰檔案位置，若必要還需填上密鑰檔案的使用密碼
5. 完成後，即可捕捉用戶端與伺服端的加密流量並利用 SSL 串流查看解密。

## 封包長度
在正常的情況下，一個乙太網路訊框最大長度是 `1518` 個位元組。
如果從封包拆掉乙太網路、IP 和 TCP 等表頭，約還剩下 1460 個位元組傳遞第七層的協定表頭或資料。
使用 wireshark 的 **packet Length** 對話框檢視封包長度分佈。點選 Statistics -> packet Length

![](https://i.imgur.com/JCUnW0y.png)

落在 1280 ~ 2559 位元組之間的大型封包通常表示有資料正在傳輸，40 ~ 79 不攜帶任何資料可能代表者協定的`控制序列`。
乙太網路表頭長度是 14 位元組（外加 4 個位元組的 CRC）、IP 表頭至少 20 個位元組，而不帶資料或其他選項的 TCP 表頭則是 20 個位元組。一個表準的 TCP 控制封包（SYN、ACK、RST、FIN），長度約 54 個位元組左右（對應前面 40 ~ 79）。

檢視封包長度約略遍覽大量捕捉結果檔案的最佳手法。*但這並非鐵則*

## 圖表
歸納資料最好的方法。
### 檢視 IO 圖表    
- 網路資料吞吐量
    - 從中觀察個別協定的效能和慈遲滯現象
    - 比較同時進行的資料串流

選擇 Statistics -> IO Graph

![](https://i.imgur.com/ALvr0Vu.png)

其中可在下方視窗中設定篩選器，並指定篩選後呈現圖表樣式。

### 往返時間圖表
*round-trip time（RTT）* 是指收到一個封包抵達後確認回覆所需要的時間。簡單來說就是來源封包抵達目的地所需的時間，再加上確認封包已抵達的回覆送到來源這邊的全部時間。
- 通訊途中的緩慢
- 瓶頸之處
- 想確認是否有任何延遲現象

點選 Statistics -> TCP Stream Graphs -> Round Trip Time Graph
![](https://i.imgur.com/iXkp8w2.png)

上途中的每一個點都代表著某個封包的 RTT

### 資料流動圖表
對於檢視資料隨時間而流動的過程，圖中裡資訊可以更清楚看出裝置之間如何通訊。

點選 Statistics -> Flow Graph

![](https://i.imgur.com/0Le1UrU.png)

每一條垂直線都代表著某一台主機。
Flow Graph 是用來檢視兩個裝置之間來回通訊絕佳方式，也可以提高查看主機之間的關聯性。

## 專家資訊
每個協定解析器都有定義**專家資訊（expert info）**，會在協定封包出現特定狀態時發出警示。
狀態分為以下：
- Chat
    - 關於通訊協定基本資訊
- Note
    - 不尋常的封包，但可能還是正常通訊的一部分
- Warning
    - 不尋常的封包，很可能也不是正常通訊的一部分
- Error
    - 封包中或其解析器中有錯誤

點選 Analyze -> Expert Information

![](https://i.imgur.com/uvFJI9U.png)

##### Chat 訊息
- Window Update（接收窗口異動）
    - 由接收端發出，通知發送端 TCP receive window 已變更了。
##### Note 訊息
- TCP Retransimission（TCP 重傳）
    - 這是封包遺失的後果。如果收到重複的 ACK 或是封包計時器已逾時，就會出現這種情況。
- Duplicate ACK（重複的 ACK）
    - 當主機未依他期待得順序接收時，他就會用前一個接收的封包產生一個重複的 ACK。
- Zero window Probe（偵測接收窗口為零）
    - 一旦發出 `Zero window` 封包後，會監控 TCP recevie window 的狀態。
- Keep Alive ACK（維持連線的 ACK）
    - 為回應 `Keep Alive` 封包而發出。
- Zero window Probe ACK（偵測接收窗口為零的 ACK）
    - 為回應 `Zero window Probe` 封包而發出。
- Window Is Full（接收窗口已滿）
    - 通知發送端主機案接收端的 TCP 接收窗口已滿。
##### Warning 訊息
- Previous Segment Lost（前一區段遺失）
    - 代表封包已遺失。當某資料串流裡的預期序號被略過時，這個訊息就會發生。
- ACKed Lost Packet（以確認過有封包遺失）
    - 代表已經看到 ACK 封包，但卻沒看到 ACK 封包所確認的封包本身。
- Keep Alive（維持連線）
    - 如果看到連線的 `keep alive` 封包時就會觸發。
- Zero Window（接收窗口為零）
    - 一旦 TCP 接收窗口大小降為零且接收窗口為零的通告也發出時，就會藉此要求發送方不要再發送資料。
- Out-of-Order（順序已亂）
    - 利用序號來偵測封包不依順序接收。
- Fast Retransmission（快速重傳）
    - 在收到重複 ACK 的 20 毫秒內重送。
##### Error 訊息
- No Error Messages（沒有錯誤訊息）

