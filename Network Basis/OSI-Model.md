## What is the OSI model ?
 Open Systems Interconnection (OSI) 模型是由國際標準化組織創建的概念模型，其使得各種通信系統能夠使用標準協議進行通信。OSI 為不同的計算機系統提供了能夠相互通訊的標準。

 OSI 模型可被視為計算機網絡的通用語言，它基於通訊系統並分成七個抽象層的概念。

 
 ![](https://www.cloudflare.com/img/learning/ddos/what-is-a-ddos-attack/osi-model-7-layers.svg)
 

 OSI 模型的每一層都處理特定的作業，並與其上下層進行通訊。[ DDoS attacks](https://github.com/CCH0124/NetworkSecurity/DDoS.md) 針對網路連接的特定層，應用層攻擊目標層 7；協議層攻擊目標層 3 和 4。

 ## Why does the OSI model matter ?

 雖然現代互聯網並不嚴格遵循 OSI 模型（它更緊密地遵循更簡單的Internet protocol ），但 OSI 模型對於解決網路問題仍然非常有用。無論是一個無法在 Internet 上使用筆記本電腦的人，還是一個為數千名用戶提供服務的網站，OSI 模型都可以幫助解決問題並隔離問題的根源，因為問題可以縮小到模型的一個特定層，則可以避免許多不必要的工作。

 ## What are the seven layers of the OSI model ?
OSI 模型七個抽象層可以從上到下定義如下：

![](https://www.cloudflare.com/img/learning/ddos/glossary/open-systems-interconnection-model-osi/7-application-layer.svg)

#### 7. The Application Layer

這是唯一直接與用戶數據交互影響的層。Web 瀏覽器和電子郵件客戶端等軟體應用程序依賴於 application layer 來啟動通訊。客戶端軟體應用程序不是應用程序層的一部分；相反，Application Layer 負責軟體依賴的協議和數據操作，以向用戶呈現有意義的數據。Application Layer 協議包括 `HTTP` 和 SMTP（簡單郵件傳輸協議是啟用電子郵件通信的協定之一）。

![](https://www.cloudflare.com/img/learning/ddos/glossary/open-systems-interconnection-model-osi/6-presentation-layer.svg)

#### 6. The Presentation Layer

該層主要負責準備數據，以便 application layer 可以使用它。換句話說，第 6 層使數據可供應用程序使用。presentation layer 負責資料translation、encryption、compression。

兩個通訊設備進行通訊可以使用不同的編碼方法，因此第 6 層負責將輸入數據轉換為接收設備的 application layer 可以理解的語法。

如果設備藉由加密連接進行通訊，則第 6 層負責在發送方端添加加密以及在接收方端解碼加密，以便它可以向 application layer 提供未加密的可讀數據。

Presentation Layer 還負責壓縮從 application layer 接收的數據，然後再將其傳送到第 5 層。這有助於藉由最小化將要傳輸的數據量來提高通訊的速度和效率。

![](https://www.cloudflare.com/img/learning/ddos/glossary/open-systems-interconnection-model-osi/5-session-layer.svg)

#### 5. The Session Layer

這是負責打開和關閉兩個設備之間通訊的層，通訊打開和關閉之間的時間稱為 session，會話層確保會話保持打開足夠長的時間以傳輸所有正在交換的數據，然後立即關閉會話以避免浪費資源。

session layer 還將數據傳輸與檢查點同步。例如，如果正在傳輸 100 megabyte 的文件，則會話層可以每 5 megabyte 設置一個檢查點，如果在傳輸 52 megabyte 後發生斷開連接或崩潰，則可以從最後一個檢查點恢復會話，這意味著只需要傳輸 50 megabyte 的數據，沒有檢查點，整個轉移必須從頭開始。

![](https://www.cloudflare.com/img/learning/ddos/glossary/open-systems-interconnection-model-osi/4-transport-layer.svg)

#### 4. The Transport Layer

第 4 層負責兩個設備之間的 end-to-end 通信，這包括從 session layer 獲取數據並在將其發送到第 3 層之前將其分解為稱為 segments，接收設備上的 Transport Layer 負責將 segments 重組為會 session layer 以使用的數據。

Transport layer 還負責**流量控制**和**錯誤控制**。流量控制確定最佳傳輸速度，以確保具有快速連接的發送器不會使連接速度慢的接收器來不及接收數據而遺失。transport layer 藉由確保接收的數據完成來執行接收端的錯誤控制，如果不是，則請求重傳。

![](https://www.cloudflare.com/img/learning/ddos/glossary/open-systems-interconnection-model-osi/3-network-layer.svg)

#### 3. The Network Layer

Network Layer 負責使兩個不同網路之間的數據傳輸，如果通訊的兩個設備在同一網路上，則不需要 Network Layer， Network Layer 將 Transport Layer 中的 segments 分解為發送方設備上的較小單元（稱為 packet），並在接收設備上重新組裝這些 packet。network Layer 還找到數據到達目的地的最佳物理路徑，這被稱為 routing。

![](https://www.cloudflare.com/img/learning/ddos/glossary/open-systems-interconnection-model-osi/2-data-link-layer.svg)

#### 2. The Data Link Layer

Data Link Layer 非常類似於 network layer，除了 data link layer 有助於同一網路上的兩個設備之間的數據傳輸，data link layer 從 network layer 獲取 packet 並將其分成較小塊 frames。與  network layer 一樣，data link layer 也負責網路內通訊中的流量控制和差錯控制（Transport Layer 僅對網路間通訊進行流量控制和差錯控制）。

![](https://www.cloudflare.com/img/learning/ddos/glossary/open-systems-interconnection-model-osi/1-physical-layer.svg)

#### 1. The Physical Layer

該層包括數據傳輸中涉及的物理設備，例如電纜和交換機。這也是將數據轉換為 bit 的層，該bit 是 1 和 0 的字符串，兩個設備的物理層也必須信號約定達成一致，以便可以將 1s 與兩個設備上的 0 區分開來。

## How data flows through the OSI model

為了使人類可讀訊息藉由網路從一個設備傳輸到另一個設備，數據必須沿著發送設備上的 OSI 模型的七層向下傳播，然後在接收端向上傳輸七層。

例如：itachi 想給 naruto 發一封電子郵件。

1. itachi 在他的筆電上的電子郵件應用程序中撰寫了他的消息，然後點擊 "發送"，他的電子郵件應用程序將他的電子郵件消息傳遞到 application Layer，application Layer 將選擇協議（SMTP）並將數據傳遞到 presentation Layer。

2. 然後， presentation Layer 將壓縮數據，然後它將到達 session layer，這將初始化通信會話

3. 接著，數據將到達發送方的 transport Layer，在那裡它將被 segments，然後這些 segment 將被分解為 network layer 的數據包，這將進一步分解為 data link layer 的 frame

4. 然後，data link layer 將這些 frame 傳送到 physical Layer，physical Layer 將數據轉換為 1 和 0 的 bit，並通過物理介質（如電纜）發送。

5. 一旦 naruto 的計算機通過物理介質（例如 wifi）接收 1 或 0 的 bit 資料，數據將流經他設備上的同一系列層，但順序相反。

6. 首先，physical Layer 將 bit 從 1 和 0 轉換為傳遞到數 data link layer的 frame

7. 然後，data link layer 將 frame 重新組裝成 network layer 的 segment

8. 然後，network layer 將從 transport Layer 的數據包中創建 segment，這將 segment 重新組合成一個數據。

9. 然後，數據將流入接收方的 session layer，該會 session layer 將數據傳遞到 presentation Layer，然後結束通訊會話

10. 然後，presentation Layer 將刪除壓縮並將原始數據傳遞到 application Layer

11. 然後，Application Layer 將人類可讀數據提供給naruto 的電子郵件軟件，這將允許他在筆電屏幕上閱讀 itachi 的電子郵件。