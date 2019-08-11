# OpenFlow Port 的種類介紹
## OpenFlow Port 
- 用來處理使用 OpenFlow 協定的交換機內部與外面網路的封包
  - 處理 OpenFlow 封包
  - 這些封包會由 Ingress Port 傳入交換機
- 與實體交換機並非直接 1 對 1 對應關係
- 交換機以邏輯性的方式透過 OpenFlow Port 彼此連結
  - 在 OpenFlow 協定中彼此連結著
  
OpenFlow 協定的交換機支援三種不同的 OpenFlow Port
- Physical Port
- Logical Port
- Reserved Port

## Standard Port 

可被定義成 Physical Port、Logical Port 或是稱為 LOCAL Reserved Port 的保留埠，甚至可以是 Ingress Port 或是 Output Port。

- 支援 Group 的概念
- 支援**計數器**的概念

![](https://i.imgur.com/kpE6IZz.png "支援的計數器")

## Physical Port 
這種實體的埠就是對應到硬體上的實體埠。

## Logical Port 
由軟體定義並且管理的埠，這些埠不一定都是由 OpenFlow 協定所建立，也可能是其他方式所產生的（OVS）。

### Logical Port 與 Physical Port 其一不同點

- 與 Logical Port 有關的網路封包會有一個額外的欄位稱為 **Tunnel ID**
- Logical Port 所收到的網路封包都會傳送給 Controller

>Tunnel ID 會被儲存在一個 `OXM_TUNNEL_ID` 欄位中，這個欄位大小為 64 bit。網路封包是由 Physical Port 所收到的，此欄位的數值就是零

## Reserved Port 
在 OpenFlow 協定中，定義了許多種不同的保留埠，可能用來做 
- Flooding（也就是複製並且轉發到所有其他的埠）
- 直接傳送到 Controller
- 套用非 OpenFlow 協定的轉發方式等等

![保留埠的種類與說明](https://i.imgur.com/CqbrqFn.png "保留埠的種類與說明")




