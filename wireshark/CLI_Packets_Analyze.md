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