## System Model(系統模型)
- 出於死鎖(deadlock)討論的目的，可以將系統模型為有限資源的集合，其可以被劃分為不同的類別，以分配給多個 process，每個進程具有不同的需求
  - 資源類別可能包括 memory、printer、CPU、open files、CD-ROM 等
- 根據定義，類別中的所有資源都是等效的，並且該類別中的任何一個資源都可以同等滿足此類別的請求。如果不是這種情況（即，如果類別中的資源之間存在某些差異），則需要將該類別進一步劃分為單獨的類別
  - 例如，printer 可能需要分成 laser printers 和 color inkjet printers。
  - 某些類別可能只有一個資源
- 在正常操作中，process 必須在使用之前**請求資源**，並在完成後按以下順序釋放它：
  - Request
    - 如果無法立即授予請求，則該 process 必須等待它所需的資源可用。例如，系統調用 `open()`、`malloc()`、`new()` 和 `request()`
  - Use
    - 該過程是使用資源，例如資料印於印表機或從文件讀取
  - Release 
    - 該過程釋放了資源。以便可用於其他 process。例如，`close()`、`free()`、`delete()` 和 `release()`
- 所有內核管理的資源，內核會追蹤哪些資源是空閒的，哪些資源是分配的，它們分配到哪個 process，以及等待此資源可用的 process queue。可以使用互斥鎖或`wait()` 和 `signal()` 調用（即二進製或計數訊號量）來控制應用程序管理的資源。
- 當集合中的每個 process 都在等待當前分配給集合中另一個 process 的資源時，一組 process 就會 deadlock（並且只有在其他等待 process 取得進展時才能釋放）

## Deadlock Characterization(死結的特性)

![](https://i.imgur.com/hulFGUG.jpg)

### Necessary Conditions(必要條件)

- 實現 deadlock 需要四個條件：
  - Mutual Exclusion(互斥)
    - 必須以非共享模式保存至少一個資源;如果任何其他 process 請求此資源，則該 process 必須等待釋放資源
  - Hold and Wait(占用與等待)
    - process 必須同時保存至少一個資源並等待某個其他 process 當前持有的至少一個資源
  - No preemption(不可搶先)
    - 一旦 process 持有資源（即一旦其請求被授予），則該進程自動釋放之前不能從該 process 中取出該資源
  - Circular Wait(循環式等待)
    - 一組 process {P_0, P_1, P_2 , ..., P_n} 必須存在，使得每個 P[i] 都在等待 P[(i + 1)%(N + 1)]。（注意，這個條件意味著保持和等待條件，但如果分別考慮這四個條件，則更容易處理條件。）
    
### Resource-Allocation Graph(資源配置圖)
- deadlock 可用資源配置圖（Resource-Allocation Graphs）的有向圖說明
  - 一組資源類別，{R1, R2, R3,..., Rm}，在圖表上顯示為方形節點。資源節點內的點指示資源的特定實例。（例如，兩個點可能代表兩個雷射印表機。）
  - 一組 process，{ P1, P2, P3,..., Pn}
  - Request Edges
    - 從 Pi 到 Rj 的一組有向弧，表示 process Pi 已請求 Rj，並且當前正在等待該資源變為可用
  - Assignment Edges 
    - 從 Rj 到 Pi 的一組有向弧，表示資源 Rj 已被分配用於 process Pi，並且 Pi 當前正在持有資源 Rj

![](https://i.imgur.com/xavaJnu.png "Example of a Resource Allocation Graph")

- 如果資源分配圖不包含循環，則系統不會有 deadlock(上圖)
- 如果資源分配圖確實包含循環，並且每個資源類別僅包含單個實例，則存在 deadlock
- 如果資源類別包含多個實例，則資源分配圖中存在循環表示存在 deadlock 的可能性，但不保證 deadlock。考慮下圖

![](https://i.imgur.com/I0yYqmx.png "Resource Allocation Graph With A Deadlock")
![](https://i.imgur.com/OAyHhnl.png "Graph With A Cycle But No Deadlock")

## Methods for Handling Deadlocks(處裡死結的方法)
- 一般來說，有三種處理 deadlock 的方法：
1. 防止 deadlock 或避免 - 不要讓系統進入死鎖狀態
2. deadlock 檢測和恢復 - 檢測到 deadlock 時中止 process 或搶占某些資源
3. 忽略問題 - 如果 deadlock 僅在一年左右發生一次，那麼簡單讓它們發生並在必要時重新引導可能比引起與死鎖預防或檢測相關的持續開銷和系統性能損失更好。 這是 Windows 和 UNIX 採用的方法

## Deadlock Prevention(預防死結)
藉由防止以下四個必要條件中的至少一個來防止 deadlock：

### Mutual Exclusion(互斥)
- 至少有一項資源必須是不可用的
- 像是唯讀檔案之類的共享資源不會導致死結
  - 某些資源（如印表機和磁帶驅動器）需要透過單個 process 進行單一訪問
  
### Hold and Wait(占用與等候)
- 必須保證一個 process 再要求一項資源時，不可以占用任何其他的資源
- 為了防止這種情況，必須防止 process 持有一個或多個資源，同時等待一個或多個其他資源。有幾種可能性：
  - 要求所有 process 一次請求所有資源。如果一個 process 在執行先前需要一個資源並且在很久之後不需要其他資源，這可能會浪費系統資源
  - 要求持有資源的 process 必須在請求新資源之前釋放它們，然後在單個新請求中重新獲取已釋放的資源以及新資源。如果 process 已使用資源部分完成操作，然後在釋放後無法重新分配，則可能會出現問題。
  - 如果 process 需要一個或多個普遍資源，則上述任何一種方法都可能導致飢餓(starvation)

### No Preemption(不可搶先)
- 在可能的情況下，搶占 process 資源分配可以防止這種 deadlock 情況
  - 一種方法是，如果在請求新資源時強制 process 等待，則此 process 先前保存的所有其他資源將被隱式釋放（搶先），從而強制此 process 在一個請求中重新獲取舊資源以及新資源
    - 如果持有某些資源的 process 請求另一個無法立即分配給它的資源，則釋放當前持有的所有資源
  - 另一種方法是，當請求資源並且不可用時，系統會查看當前哪些其他 process 具有這些資源並且本身被阻塞等待其他資源。如果找到這樣的 process，那麼它們的一些資源可能被搶先並被添加到 process 正在等待的資源列表中
    - 搶占的資源將添加到進程正在等待的資源列表中
  - 只有當它可以重新獲得舊資源以及它所請求的新資源時，才會重新啟動 process
  - 這些方法中的任何一種都可以適用於狀態易於保存和恢復的資源，例暫存器和記憶體，但通常不適用於其他設備，例如印表機和磁帶驅動器
### Circular Wait(循環式等候)
- 避免循環式等候的一種方法是對所有資源進行編號，並要求 process 僅以嚴格增加（或減少）的順序請求資源
- 為了請求資源 R_j，process 必須首先釋放所有 R_i，使得 i>=j
- 該方法的一個重大挑戰是確定不同資源的相對排序

## Deadlock Avoidance(避免死結)
- 避免 deadlock 的一般思想是防止至少一種上述情況來防止 deadlock 的發生
- 這需要有關每個 process 的更多訊息，並且往往導致設備使用率低
- 在某些演算法中，調度只需要知道 process 可能使用的每個資源的最大數量。 在更複雜的算法中，調度還可以利用可能需要以什麼順序需要的資源的調度
- 當調度發現啟動 process 或授予資源請求可能導致將來出現 deadlock 時，則該 process 才會啟動或未授予請求
- 資源分配狀態由可用資源和已分配資源的數量以及系統中所有 process 的最大要求定義

### Safe State(安全狀態)
- 如果系統可以在不進入 deadlock 狀態的情況下分配所有 process 請求的所有資源（達到其規定的最大值），則狀態是安全的
- 如果存在安全的進程序列{P0，P1，P2，...，Pn}，則狀態是安全的，這樣可以使用當前分配給 Pi 和所有 process 的資源來授予對 Pi 的所有資源請求。Pj，其中j<i。（即，如果 Pi 之前的所有 process 都完成並釋放了他們的資源，那麼 Pi 也可以使用他們釋放的資源完成。）
- 如果不存在安全序列，則系統處於不安全狀態，這可能導致 deadlock。（所有安全狀態都是無 deadlock 的，但並非所有不安全的狀態都會導致 deadlock）

![](https://i.imgur.com/Y6NRpV2.png "Safe, Unsafe, Deadlock State")

- 例如，考慮具有 12 個磁帶驅動器的系統，分配如下。這是一個安全的狀態嗎？什麼是安全順序？

||Maximum Needs|Current Allocation|
|---|---|---|
|P0|10|5|
|P1|4|2|
|P2|9|2|

如果 process P2 請求並再被授予一個磁帶驅動器，上面的表會發生什麼？

- 安全狀態方法的關鍵是當對資源發出請求時，僅當結果分配狀態是安全的時才授予請求

### Resource-Allocation Graph Algorithm(資源配圖演算法)
- 如果資源類別只有其資源的單個實例，則可以透過資源分配圖中的循環檢測 deadlock 狀態
- 透過申請邊(claim edge)的資源分配圖來是別和避免不安全狀態
- 當 process 發出請求時，申請邊 `Pi-> Rj` 將轉換為請求邊。類似的，當資源被釋放時，分配將恢復為申請邊
- 這種方法的工作原理是拒絕在資源分配圖中產生週期的請求，使申請邊生效
- 申請邊 `Pi->Rj` 表示 process `Pj` 可以請求資源 `Rj`(用虛線表示)
- 當 process 請求資源時，申請邊轉換為請求邊
- 將資源分配給 process 時，請求邊轉換為分配邊
- 當進程釋放資源時，分配邊將重新轉換為申請緣
- 必須在系統中事先聲明資源
- 例如，考慮當 process P2 請求資源 R2 時會發生什麼：

![](https://i.imgur.com/5chrqhd.png "Resource-Allocation Graph")

- 生成的資源分配圖將在其中有一個循環，因此無法授予請求

![](https://i.imgur.com/906SdFO.png "Unsafe State In Resource-Allocation Graph")

### Banker's Algorithm(銀行家演算法)
- 對於包含多個實例的資源類別，資源分配圖方法不起作用，必須選擇更複雜（效率更低）的方法
- 銀行家的算法之所以得名，是因為這是一種銀行家可以用來確保當他們藉出資源時仍然可以滿足所有客戶的方法（銀行家不會借一點錢開始建房子，除非他們確信他們以後能夠借出剩下的錢來完成房子。）
- 當 process 啟動時，它必須事先聲明它可能請求的最大資源分配，直到系統上可用數量
- 發出請求時，**調度會確定授予請求是否會使系統處於安全狀態**。如果沒有，該 process 必須其他 process 釋放足夠的資源
- 銀行家的算法依賴於幾個關鍵資料結構:(其中 `n` 是 process 數量，`m` 是資源數量）
  - Available(可用的)
    - 為一長度 `m` 的向量，可表示出每種形式的可用資源量。`Available[j]=k`，表示資源型式 `Rj` 中有 `k` 個實例可用
  - Max(最大值)
    - 為一 `n*m` 的矩陣，定義出各個 process 的最大需求量。`Max[i][j]=k`，表示 process `Pi` 至多可要求 `k` 個實例之資源型式 `Rj`
  - Allocation(占用、分配)
    - 為一 `n*m` 的矩陣，定義現在每一 process 所佔用之各個型式資源的數量。`Allocation[i][j]=k`，process `Pi` 現已佔有資源型式 `Rj` 中之 `k` 個例證
  - Need(需求)
    - 為一 `n*m` 的矩陣，表示每一個 process 剩餘的需求量。若 `Need[i][j]=k`，表示 process `Pi` 還需要再占用資源型式 `Rj` 中之 `k` 個實例。
    - Need[i][j] = Max[i][j] - Allocation[i][j]
##### Safety Algorithm(安全演算法)

![](https://i.imgur.com/roj7N1D.png)

##### Resource-Request Algorithm(The Bankers Algorithm)

- 此演算法確定新請求是否安全，並僅在安全的情況下授予它
- 當發出請求（不超過當前可用資源）時，假裝它已被授予，查看結果狀態是否安全。如果是，請授予請求，如果不是，則拒絕請求，如下所示：
  1. 令 `Request[n][m]` 表示 process 當前請求的每種類型的資源數。如果對任何process `i`，`Request[i]> Need [i]`，則引發錯誤條件。
  2. 如果 `Request[i]>Available` 於任何 process `i`，則該 process 必須等待資源可用。否則，該 process 可以繼續到步驟3。
  3. 檢查是否可以安全地授予請求，假裝它已被授予，然後查看結果狀態是否安全。如果是，則授予請求，如果不是，則該 process 必須等到其請求可以安全授予。授予請求（或假裝用於測試目的）的過程是：
    - Available = Available - Request
    - Allocation = Allocation + Request
    - Need = Need - Request

##### An Illustrative Example(一個例子)
5 processes P0  through P4; 
3 resource types:
A (10 instances),  B (5 instances), and C (7 instances)
Snapshot at time T0:
![](https://i.imgur.com/W2gNaHh.png)

矩陣 *Need* 被定義為 *Max – Allocation*

![](https://i.imgur.com/DdWzy29.png)

現在考慮如果 process P1 請求 1 個 A 實例和 2 個 C 實例會發生什麼。（Request[1] =（1,0,2））

![](https://i.imgur.com/FGc19eV.png)

## Deadlock Detection(死結偵測)
### Single Instance of Each Resource Type
### Several Instances of a Resource Type
### Detection-Algorithm Usage

## Recovery From Deadlock(死結回復)
### Process Termination
### Resource Preemption
1. Selecting a victim (選擇犧牲者)
2. Rollback (回撤)
3. Starvation (飢餓)
