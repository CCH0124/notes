# 以指令列進行封包分析
## 捕捉及儲存封包
#### 列出網路介面卡

```shell=
PS D:\Wireshark> .\tshark -D
1. \Device\NPF_{3C77DEEF-ABE3-4DCD-B9EA-899F37CEE032} (VMware Network Adapter VMnet8)
2. \Device\NPF_{CE44EA7A-96E9-4CFD-980C-DC497A50DF2A} (VMware Network Adapter VMnet1)
3. \Device\NPF_{F5673E19-7AAF-4AF4-A63D-C0B9EA556CC3} (Wi-Fi)
4. \\.\USBPcap1 (USBPcap1)
5. ciscodump (Cisco remote capture)
6. randpkt (Random packet generator)
7. sshdump (SSH remote capture)
8. udpdump (UDP Listener remote capture)
```
#### 指定介面卡捕捉封包 -i
```shell=
PS D:\Wireshark> .\tshark -i 3
Capturing on 'Wi-Fi'
...
```
#### 儲存至檔案 -w
```shell=
PS D:\Wireshark> .\tshark -i 3 -w wifi_pack.pcap
Capturing on 'Wi-Fi'
10
```
捕捉動作直到 `CTRL+C` 為止，儲存檔案若未指定位置，預設則和 wireshark 執行檔同一個目錄，

#### 讀取檔案 -r
```shell=
PS D:\Wireshark> .\tshark -i 3 -r wifi_pack.pcap
```
#### 限制顯示或抓取封包的數目 -c
在於讀取千筆資訊時，會導致前半部封包在螢幕一閃而過。
```shell=
PS D:\Wireshark> .\tshark -i 3 -r wifi_pack.pcap -c 5 # 顯示前 5 筆
```
控制在抓取封包時的數目
```shell=
PS D:\Wireshark> .\tshark -i 3 -w wifi_pack.pcap -c 8
```

## 操作輸出
指令方式抓取封包，通常只給基本資訊，剩下需透過指令完成。
Tshark 會解析到封包第七層協定。因此提供資訊較 tcpdump 多。
#### 取出更詳細的彙整 -V
```shell=
PS D:\Wireshark> .\tshark -r wifi_pack.pcap -c1 -V
Frame 1: 2339 bytes on wire (18712 bits), 2339 bytes captured (18712 bits) on interface 0
...
```

#### 用 16 進位或 ASCII 查看 -x 
```shell=
PS D:\Wireshark> .\tshark -xr wifi_pack.pcap -c1
0000  0a c5 e1 06 8d 8e 94 b8 6d fa ca 8a 08 00 45 00   ........m.....E.
0010  09 15 50 92 40 00 80 06 00 00 c0 a8 2b f9 ac d9   ..P.@.......+...
0020  18 0a 2e 6d 01 bb 34 60 c1 c1 93 e2 6b 8d 50 18   ...m..4`....k.P.
0030  00 fd b1 8b 00 00 17 03 03 08 e8 97 72 c4 91 4e   ............r..N
0040  c6 52 86 15 b6 ca e8 c5 02 a1 b5 fd 80 3b 73 e6   .R...........;s.
0050  7d 85 5e 96 ac 96 69 26 e2 00 9e b3 8a d5 ed 1b   }.^...i&........
0060  01 2c 08 ee c3 6f a8 fe 52 98 84 7f 03 1c 69 ff   .,...o..R.....i.
0070  08 82 8b d0 14 80 5c 44 6c 03 7b 5a 91 04 23 88   ......\Dl.{Z..#.
...
```

## 名稱解釋 -N
把 address 和 port 轉換容易難懂得名稱。

以下是搭配引數 -N 參數值
|參數|說明|
|---|---|
|m|解釋 MAC 位址|
|n|解釋網路層位址|
|t|解釋傳輸層 port|
|N|使用外部解釋|
|C|同時進行 DNS 解析|

```shell=
PS D:\Wireshark> .\tshark -ni 3 # -n 關閉名稱解釋
Capturing on 'Wi-Fi'
    1   0.000000 192.168.43.249 → 52.192.39.44 TLSv1.2 92 Application Data
    2   0.256130 52.192.39.44 → 192.168.43.249 TLSv1.2 86 Application Data
    3   0.295900 192.168.43.249 → 52.192.39.44 TCP 54 12293 → 443 [ACK] Seq=39 Ack=33 Win=255 Len=
0
```
```shell=
PS D:\Wireshark> .\tshark -i 3 -Ntm # 保留傳輸層和 MAC 位址解釋
```

## 套用篩選器
TShark 和 tcpdump 都支援 BPF（Berkeley Packet Filter）

#### 捕捉篩選器  -f
捕捉 tcp port 443 並儲存
```shell=
PS D:\Wireshark> .\tshark -i 3 -Nnt -w packets.pcap -f "tcp port 443"
```
#### 顯示篩選器 -Y
讀取捕捉封包檔案並搜尋目的端 port 為 443 的封包
```shell=
PS D:\Wireshark> .\tshark -r packets.pcap -Y "tcp.dstport == 443"  -c 2 -V
Frame 1: 55 bytes on wire (440 bits), 55 bytes captured (440 bits) on interface 0
    Interface id: 0 (\Device\NPF_{F5673E19-7AAF-4AF4-A63D-C0B9EA556CC3})
        Interface name: \Device\NPF_{F5673E19-7AAF-4AF4-A63D-C0B9EA556CC3}
    Encapsulation type: Ethernet (1)
    Arrival Time: Nov 27, 2018 21:45:42.887315000 台北標準時間
    [Time shift for this packet: 0.000000000 seconds]
    Epoch Time: 1543326342.887315000 seconds
    [Time delta from previous captured frame: 0.000000000 seconds]
    [Time delta from previous displayed frame: 0.000000000 seconds]
    [Time since reference or first frame: 0.000000000 seconds]
    Frame Number: 1
    Frame Length: 55 bytes (440 bits)
    Capture Length: 55 bytes (440 bits)
...
```

#### 篩選結果轉存另一個檔案
```shell=
PS D:\Wireshark> .\tshark -ni 3 -r packets.pcap -Y "tcp.dstport == 443" -w dstport443.pcap
PS D:\Wireshark> .\tshark -r .\dstport443.pcap -c 2
    1   0.000000 192.168.43.249 → 74.125.23.95 TCP 55 [TCP segment of a reassembled PDU]
    2   0.004881 192.168.43.249 → 52.192.39.44 TLSv1.2 92 Application Data
```
>BPF 篩選器並不支援註解

## TShark 的時間顯示格式 -t 

|參數|時間戳記說明|範例|
|---|---|---|
|a|捕捉到封包時絕對時間(時區)|18:28:11.002672|
|ad|捕捉到封包時絕對時間與日期(時區)|2018-12-25 18:28:11.002672|
|d|從捕捉前一個封包之後經過的時間(時差)|0.000250|
|dd|從捕捉前一個封包之後經過的時間|0.000250|
|e|從歷史的一刻(UTC 1970 年 1 月 1 日)至今|1444420078.004669|
|r|從第一個封包到目前這個封包所經過的時間|0.000140|
|u|捕捉到封包時的絕對時間(UTC)|19:47:13.004669|
|ud|捕捉到封包時絕對時間與日期(UTC)|2018-12-25 18:28:11.002672|

```shell=
PS D:\Wireshark> .\tshark -r .\dstport443.pcap -c 3 -t a
    1 21:45:42.887315 192.168.43.249 → 74.125.23.95 TCP 55 [TCP segment of a reassembled PDU]
    2 21:45:42.892196 192.168.43.249 → 52.192.39.44 TLSv1.2 92 Application Data
    3 21:45:43.000610 192.168.43.249 → 52.192.39.44 TCP 54 12293 → 443 [ACK] Seq=39 Ack=33 Win=257
 Len=0
PS D:\Wireshark> .\tshark -r .\dstport443.pcap -c 3 -t ad
    1 2018-11-27 21:45:42.887315 192.168.43.249 → 74.125.23.95 TCP 55 [TCP segment of a reassembled
 PDU]
    2 2018-11-27 21:45:42.892196 192.168.43.249 → 52.192.39.44 TLSv1.2 92 Application Data
    3 2018-11-27 21:45:43.000610 192.168.43.249 → 52.192.39.44 TCP 54 12293 → 443 [ACK] Seq=39 Ack
=33 Win=257 Len=0
PS D:\Wireshark> .\tshark -r .\dstport443.pcap -c 3 -t d
    1   0.000000 192.168.43.249 → 74.125.23.95 TCP 55 [TCP segment of a reassembled PDU]
    2   0.004881 192.168.43.249 → 52.192.39.44 TLSv1.2 92 Application Data
    3   0.108414 192.168.43.249 → 52.192.39.44 TCP 54 12293 → 443 [ACK] Seq=39 Ack=33 Win=257 Len=
0
PS D:\Wireshark> .\tshark -r .\dstport443.pcap -c 3 -t dd
    1   0.000000 192.168.43.249 → 74.125.23.95 TCP 55 [TCP segment of a reassembled PDU]
    2   0.004881 192.168.43.249 → 52.192.39.44 TLSv1.2 92 Application Data
    3   0.108414 192.168.43.249 → 52.192.39.44 TCP 54 12293 → 443 [ACK] Seq=39 Ack=33 Win=257 Len=
0
PS D:\Wireshark> .\tshark -r .\dstport443.pcap -c 3 -t e
    1 1543326342.887315 192.168.43.249 → 74.125.23.95 TCP 55 [TCP segment of a reassembled PDU]
    2 1543326342.892196 192.168.43.249 → 52.192.39.44 TLSv1.2 92 Application Data
    3 1543326343.000610 192.168.43.249 → 52.192.39.44 TCP 54 12293 → 443 [ACK] Seq=39 Ack=33 Win=2
57 Len=0
PS D:\Wireshark> .\tshark -r .\dstport443.pcap -c 3 -t r
    1   0.000000 192.168.43.249 → 74.125.23.95 TCP 55 [TCP segment of a reassembled PDU]
    2   0.004881 192.168.43.249 → 52.192.39.44 TLSv1.2 92 Application Data
    3   0.113295 192.168.43.249 → 52.192.39.44 TCP 54 12293 → 443 [ACK] Seq=39 Ack=33 Win=257 Len=
0
PS D:\Wireshark> .\tshark -r .\dstport443.pcap -c 3 -t u
    1 13:45:42.887315 192.168.43.249 → 74.125.23.95 TCP 55 [TCP segment of a reassembled PDU]
    2 13:45:42.892196 192.168.43.249 → 52.192.39.44 TLSv1.2 92 Application Data
    3 13:45:43.000610 192.168.43.249 → 52.192.39.44 TCP 54 12293 → 443 [ACK] Seq=39 Ack=33 Win=257
 Len=0
PS D:\Wireshark> .\tshark -r .\dstport443.pcap -c 3 -t ud
    1 2018-11-27 13:45:42.887315 192.168.43.249 → 74.125.23.95 TCP 55 [TCP segment of a reassembled
 PDU]
    2 2018-11-27 13:45:42.892196 192.168.43.249 → 52.192.39.44 TLSv1.2 92 Application Data
    3 2018-11-27 13:45:43.000610 192.168.43.249 → 52.192.39.44 TCP 54 12293 → 443 [ACK] Seq=39 Ack
=33 Win=257 Len=0
```

> tcpdump 無此功能

## TShark 統計歸納
此功能也是跟 tcpdump 不一樣，可以從捕捉結果產出特定項目統計數據的能力。

### 統計數據 -z 
```shell=
PS D:\Wireshark> .\tshark -z help # 提供多個方法
tshark: The available statistics for the "-z" option are:
     afp,srt
     ancp,tree
     ansi_a,bsmap
     ansi_a,dtap
     ansi_map
     bacapp_instanceid,tree
     ...
```
### 查看 HTTP 請求及回覆
```shell=
S D:\Wireshark> .\tshark -r .\packets.pcap -z http,tree
```