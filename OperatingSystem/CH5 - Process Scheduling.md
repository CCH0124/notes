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


