## Basic Concepts
- 幾乎所有程序都有一些交替的 CPU 數字運算週期和等待某種 I/O。（即使從 memory 中獲取簡單的 memory 相對於 CPU 速度也需要很長時間）
- 在運行單個 process 的簡單系統中，等待 I/O 所花費的時間被浪費，並且這些 CPU 週期將永遠丟失
- 調度系統允許一個 process 使用 CPU 而另一個進程正在等待 I/O，從而充分利用原本丟失的 CPU 週期

### CPU-I/O Burst Cycle
- 幾乎所有 process 在一個連續循環中在兩個狀態之間交替，如下面的圖
  - CPU 突發(burst)執行計算
  - I/O 突發(burst)，等待數據傳輸進出系統

![](https://i.imgur.com/sdidxIB.png "Alternating sequence of CPU and I/O bursts.")

- CPU 突發因處理 process 和 program 而異，但廣泛的研究顯示頻率模式類似於下圖

![](https://i.imgur.com/yWZHAFD.png "Histogram of CPU-burst Times")

### CPU Scheduler
- 每當 CPU 變為空閒時，CPU 調度程序（也就是短程排班程序）的工作就是從就緒隊列中選擇另一個 process 來運行下一個（從記憶體中準備要執行的數個 process 選擇一個）
- 就緒佇列的儲存結構和用於選擇下一個 process 的演算法不一定是 FIFO 佇列

### Preemptive Scheduling（可搶先排班）
CPU 排班決策發生在以下四種情況：
1. 當一 process 從執行狀態轉變成等待狀態時（I/O 請求、呼叫 wait() 等待 process 結束）
  - 從 running 到 waiting
2. 當一 process 從執行狀態轉變成就緒狀態時（有 interrupt(中斷) 發生時）
  - 從 running 到 ready
3. 當一 process 從等待狀態轉變成就緒狀態時（I/O 結束）
  - 從 waiting 到 ready
4. 當一 process 終止時
  - termnates
  
- 對於條件 1 和 4，沒有選擇 - 必須選擇新的過程
- 對於條件 2 和 3，有一個選擇 - 要麼繼續運行當前 process，要麼選擇另一個 process
- 如果僅在條件 1 和 4 下進行排班，則稱該系統是非搶先（nonpreemptive）的或合作（cooperative）的。
  - 在這些條件下，一旦 process 開始運行，它就會一直運行，直到它自動 blocks 或直到它完成，否則系統被認為是可搶先（preemptive）的
- **當兩個 process 共享數據時，可搶先的排班可能會導致問題，因為在更新共享數據結構的過程中，一個 process 可能會中斷**
- 如果 kernel 在搶占發生時忙於實現系統調用（例如，更新關鍵 kernel 數據結構），則搶占也可能是一個問題。
  - 大多數現代 UNIX 透過使 process 等待直到系統調用完成或被阻塞才允許搶占來處理這個問題，不幸的是這個解決方案對於實時系統來說是有問題的，因為不再能保證實時響應。

###  Dispatcher（分派程式）
- 分派程序是將 CPU 控制權交給短程排班程式選擇的 process 時所採用的模塊。這個功能涉及到
  - switching context（轉換內容）
  - switching to user mode（轉換成使用者模式）
  - jumping to the proper location in the user program to restart that program（跳越到使用者程式的適當位置以便重新開啟程式）
- 分派程式需要盡可能快，因為它在每個上下文切換（context switch）上運行
  - 分派程式消耗的時間稱為分派延遲
  
## Scheduling Criteria（排班原則）
在嘗試為特定情況和環境選擇**最佳**排班演算法時，需要考慮幾個不同的標準，包括：
- CPU utilization（CPU 使用率）
  - 理想情況下，CPU 有著 100％ 使用率，從而浪費 0 個 CPU 週期。在實際系統上，CPU 使用率應在 40％（輕載）到 90％（重載）之間
- Throughput（產量）
  - 每單位時間完成的 process 數量。取決於具體過程，可以從 10/秒到 1/小時
- Turnaround time（回復時間） 
  - 從提交時間到完成，特定流程完成所需的時間
  - 從進入等待主記憶體、在就緒佇列等待，以及 CPU 執行和執行輸入輸出動作等時間的總和
- Waiting time （等候時間）
  - process 在準備好的佇列中花費多少時間等待輪到 CPU 使用
  - 在就緒佇列中等待所花費周期的總合
  - 平均負載（Load average）
    - 準備佇列中等待輪到 CPU 進入的平均 process 數量。由"正常運行時間"和"誰"以 1 分鐘，5 分鐘和 15 分鐘的平均值報告。
- Response time （反應時間）
  - 交互式系統從發出命令到開始反應該命令所花費的時間
  
- 通常，人們希望優化標準的平均值（最大化 CPU 利用率和吞吐量，並最小化所有其他標準值。）但有時候，人們希望做一些不同的事情，例如最小化最大反應時間。
- 有時最希望最小化標準的*差異*而不是實際值。即用戶更容易接受一致的可預測系統而不是一個不一致的系統，即使它有點慢。

## Scheduling Algorithms
### First-Come First-Serve Scheduling, FCFS
- FCFS 只是一個 FIFO 隊列。就像在銀行、郵局或結帳排隊等候的客人一樣
- 然而不幸的是，FCFS 可以**產生一些非常長的平均等待時間**，特別是如果第一個到達那裡的過程需要很長時間。例如，請考慮以下三個過程：

|process|Brust Time|
|---|---|
|P1|24|
|P2|3|
|P3|3|

在下面的第一個甘特圖中，流程 P1 首先到達。三個進程的平均等待時間為（0 + 24 + 27）/ 3 = 17.0 ms。

![](https://i.imgur.com/Jq9CL1W.png)

各 process 等待時間：P1=0; P2=24; P3=27

- 在下面甘特圖中，相同的三個進程的平均等待時間為（0 + 3 + 6）/ 3 = 3.0 ms。 三次的總運行時間是相同的，但在第二種情況下，三次爆發中的兩次完成得更快，而另一次進程僅延遲了一小段時間。

![](https://i.imgur.com/4ryWR7z.png)

各 process 等待時間：P1=6; P2=0; P3=3

- Convoy Effect 漫長過程背後的短暫過程
  - 考慮一個 CPU-bound 和許多 I/O-bound 的 process
  - 當所有其他 process 都在等待一個大 process 離開 CPU 的時候，會產生此現象

### Shortest-Job-First Scheduling, SJF
- 將每一個 process 的下一個 CPU 分割長度和該 process 相結合
  - 當 CPU 有空時，指定給下個 CPU 分割最短的 process
- 基於下一個最短的 CPU 突發而不是整個處理時間來選擇一個 proceess

|process|Brust Time|
|---|---|
|P1|6|
|P2|8|
|P3|7|
|P4|3|

![](https://i.imgur.com/ZDCllOM.png)

平均等待時間為 `(0 + 3 + 9 + 16)/4=7 ms`，較 FCFS 短

- SJF 可以被證明是最快的排班演算法，但是它遇到了一個重要的問題：你怎麼知道下一個 CPU 突發將持續多長時間（如何得知下一個 CPU 要求長度）
- 對於長期排班來說，可以根據用戶在提交作業時為其工作設置的限制來完成，使用 process 時間限制。但如果他們將限制設置得太低，他們就不得不重新提交作業F 
- SJF 不適用 `short-term CPU scheduling`（短程排班） 
- 更實際的方法是基於該過程的最近突發時間的一些歷史測量來**預測**下一突發的長度。一種簡單、快速且相對準確的方法是**指數平均**值，其可以定義如下。

![](https://i.imgur.com/4FxswX2.png)
通常 `α set to ½`

在該方案中，先前的估計包含所有先前時間的歷史，並且 `α` 用作近期數據與過去歷史的相對重要性的加權因子。如果 `α` 為 1.0，則忽略過去的歷史，並且我們假設下一個突發將與最後一個突發的長度相同。如果 `α` 為0.0，則忽略所有測量的突發時間，我們假設一個恆定的突發時間。最常見的 `α` 設置為 0.5，如圖5.3所示：

![](https://i.imgur.com/VbZmpVR.png)

- SJF 可以是**搶先**也可以是**非搶先**。當新 process 到達就緒隊列時發生搶占（新到 process 會搶在目前正在執行 process 之前執行），該就緒隊列的預測突發時間短於突發當前在 CPU 上的進程中剩餘的時間。搶先式 SJF 有時被稱為 `shortest remaining time first scheduling`（剩餘的時間最短的先做）

|Process|Arrival Time| Burst Time|
|---|---|---|
|P1|0|8|
|P2|1|4|
|P3|2|9|
|p4|3|5|

![](https://i.imgur.com/Wde1xiG.png)

`平均到達時間 = [(10-1)+(1-1)+(17-2)+5-3)]/4 = 26/4 = 6.5 msec`

###  Priority Scheduling
- SJF 是一般優先權排班的特例，其中為每個 PROCESS 分配優先權，並且首先調度具有最高優先權的 process 給 CPU
  - SJF 使用下一個預期突發時間的倒數作為其優先權，預期突發越小，優先權越高
- 在實踐中，優先權是使用固定範圍內的整數來實現的，但是沒有就"高"優先權是使用大數還是小數而達成一致的約定。

以下甘特圖基於這些 process 突發時間和優權，並產生 `8.2` 毫秒的平均等待時間：

|Process|Burst Time|Priority|
|---|---|---|
|P1|10|3|
|P2|1|1|
|P3|2|4|
|P4|1|5|
|P5|5|2|

![](https://i.imgur.com/oy2pERW.png "Priority scheduling Gantt Chart")

>書中使用低數字表示高優先級，0 表示最高優先級

- 可以在內部或外部分配優先級。OS 使用像是*平均突發時間*、*CPU 與 I/O 活動的比率*、*系統資源使用*以及*內核*可用的其他因素等標準來分配內部優先級
- 優先權排班可以是搶先的，也可以是非搶先的
- 優先權排班可能會遇到一個主要問題，即 **無限期阻塞(indefinite blocking)** 或 **飢餓(starvation)** ，其中低優先權任務可以永遠等待，因為總有一些其他工作具有更高的優先權
  - 問題飢餓 - 低優先級進程可能永遠不會執行
  - 解決方案 Aging-隨著時間的推移，增加了 process 的優先權

### Round Robin Scheduling 依序循環之排班方式
- 為分時作業系統設計
- 循環排班類似於 FCFS 排班，除了 CPU 突發被分配了稱為*時間量(time quantum)* 的限制
  - 通常為 10-100 毫秒。經過這段時間後，該 procedd 被搶先並添加到就緒隊列(ready queue)的末尾
- 當給 CPU 一個 process 時，為一個時間量設置的值設置一個計時器
  - 如果 process 在時間量的計時器到期之前完成其突發，那麼它就像普通的 FCFS 演算法一樣被換出 CPU
  - 如果計時器先關閉，則該 process 將從 CPU 中換出並移至就緒列隊(ready queue)的後端，這之間會有 `context switch`
- 就緒列隊 (ready queue) 保持為一個循環列隊，因此當所有進程都輪流時，排班程序會將第一個 process 轉為另一個順序，依此類推
- 雖然平均等待時間可能比其他排班演算法長，但 RR 排班可以使所有處理器平均共享 CPU 的效果。在以下示例中，平均等待時間為 `5.66 ms`

|Process|Burst Time|
|---|---|
|P1|24|
|P2|3|
|P3|3|

![](https://i.imgur.com/SUKUILV.png)

- RR 的性能對所選擇的時間量敏感。如果時間量足夠大，則 RR 減少到 FCFS 算法;如果它非常小，則每個 process 獲得處理器時間的 `1/n` 並平均共享 CPU 。但是，真實系統會為每個 `context switch` 調用開銷，並且時間量越小，上下文切換越多。（見下圖）大多數現代系統使用 10 到 100 毫秒之間的時間量，`context switch` 時間大約為 10 微秒，因此相對於時間量，開銷很小。

![](https://i.imgur.com/UPjFsCY.png)

- 周轉時間也隨著時間量時間而變化，以非明顯的方式。例如，考下圖所示的過程：

![](https://i.imgur.com/WbvWiOl.png "Turnaround Time Varies With The Time Quantum")"80% of CPU bursts should be shorter than q"

- 通常，如果大多數 process 在一個時間量內完成下一個 cpu 突發，則周轉時間最小化。 例如，對於每個 10 毫秒突發的三個 process，1 毫秒時間量的平均周轉時間為 29，並且對於 10 毫秒時間量，它減少到 20。但是，如果它太大，那麼 RR 就會退化為 FCFS。 根據經驗，80％ 的 CPU 突發應該小於時間量。

### Multilevel Queue Scheduling(多層佇列排班方式)
- 當可以容易對 process 進行分類時，可以建立多個單獨的列隊，每個列隊實現最適合於該類型的作業的任何排班演算法，和/或具有不同的參數調整
- 還必須在列隊之間進行調度，即調度一個列隊以獲得相對於其他列隊的時間。兩個常見選項是嚴格優先權（較低優先權列隊中的作業不會運行，直到所有較高優先權列隊都為空）和 round-robin（每個列隊依次獲得時間片(time slice 或時間量)，可能具有不同的大小。）
- 請注意，在此算法下，作業無法從列隊切換到列隊 - 一旦為列隊分配了列隊，這就是他們的列隊，直到完成為止

![](https://i.imgur.com/Pz7uj2W.png)

### Multilevel Feedback Queue (多層回饋佇列之排班方式)
- 多層回饋佇列之排班方式類似於上面描述的普通多層佇列排班方式，除了作業可能由於各種原因從一個列隊移動到另一個列隊：
  - 如果作業的特徵在 CPU 密集型和 I/O 密集型之間發生變化，那麼將作業從一個列隊切換到另一個列隊可能是合適的。
  - 老化也可以合併，以便等待很長時間的工作可以暫時升級到更高優先級的列隊。
- 多層回饋佇列之排班是最靈活的，因為它可以針對任何情況進行調整。但由於所有可調參數，它也是最複雜的。定義其中一個系統的一些參數包括：
  - 佇列數量
  - 每個佇列的排班演算法
  - 決定什麼時候把 process 提升到較高優先權佇列的方法
  - 決定降低高優先佇列的 process 到下層佇列時機的方法
  - 當 process 需要服務時，決定該行程進入哪一個佇列的方法
  
![](https://i.imgur.com/gz3E5Y7.png "Multilevel feedback queues")
三個佇列 : 
- Q0 – RR 時間量為 8 毫秒
- Q1 – RR 時間量子為 16 毫秒
- Q2 – FCFS

排班
- 新作業進入佇列 Q0，為 FCFS 提供服務
  - 當它獲得 CPU 時，作業接收 8 毫秒
  - 如果它沒有在 8 毫秒內完成，則作業將移至佇列 Q1
- 在 Q1，作業再次為 FCFS 服務，並再接收 16 毫秒
  - 如果仍未完成，則將其搶先並移至佇列 Q2
 
## Thread Scheduling (執行緒排班)
- process 排班程序僅調度內核線程(kernel threads)
- 使用者線程由線程函式庫映射到內核線程 - OS（特別是排班程序）不知道它們。

### Contention Scope (競爭範圍)
- 競爭範圍是指線程競爭使用 CPU 的範圍
- 在實現多對一和多對多線程的系統上，會發生**爭用範圍(Process Contention Scope) PCS**為競爭發生在屬於同一 process 的線程之間。（這是在單個內核線程上管理/調度多個用戶線程，並由線程庫管理。）
- **stem Contention Scope，SCS** 系統排班程序調度內核線程在一個或多個 CPU 上運行。實現一對一線程（XP，Solaris 9，Linux）的系統僅使用 SCS
- PCS 排班通常以優先級完成，其中程序員可以設置和/或改變由他或她的程序創建的線程的優先級。即使在具有相同優先級的線程之間也不能保證時間切片(time slice,時間量)

## Multiple-Processor Scheduling (多處理器排班方法)

# 尚未補齊
