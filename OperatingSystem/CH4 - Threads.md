# Threads（執行緒）

- Threads 是 CPU 使用率的基本單元，由一個 program counter、stack、thread ID 和 registers 組成
- 傳統（heavyweight, 重量級）process 具有單一執行緒控制 - 有一個 program counter，以及可在任何給定時間執行的一系列指令
- 多執行緒應用程序在單個 process 中具有多個執行緒，每個執行緒都有自己的 program counter、stack 和 registers，但共用程式區域、數據和某些 OS 資源（如打開文件）。

![ Single-threaded and multithreaded processes](https://i.imgur.com/k1clVLq.png "Single-threaded and multithreaded processes")

### Motivation（動機）
- 只要 process 有多個任務獨立於其他任務執行，執行緒在現代編程中非常有用
- 當其中一個任務可能阻塞時，並且希望允許其他任務在不阻塞的情況下繼續進行
- process 創建是重量級的，而執行緒創建是輕量級的
- kernel 通常是多執行緒的

![](https://i.imgur.com/rGu4rmQ.png "Multithreaded Server Architecture")
(1)請求
(2)產生新執行緒來服務此請求
(3)繼續監聽額外客戶端請求

##### Example

- 在文字處理器中，後台執行緒可以在前台執行緒處理用戶輸入（擊鍵）時檢查拼寫和語法，而第三個執行緒從硬碟驅動加載圖像，第四個執行緒對正在編輯的文件進行定期自動備份

- Web 服務器，多個執行緒允許同時滿足多個請求，而不必按順序為請求提供服務或為每個傳入請求分離單獨的進程。（後者是在開發執行緒概念之前這樣做的事情。守護進程會在一個端口上監聽，為每個要處理的傳入請求分叉一個子進程，然後再回到偵聽端口。）

### Benefits
多執行緒有四大優點：
1. Responsiveness（應答）
  - 一個執行緒可以提供快速響應，而其他執行緒被阻塞或減慢進行密集計算。
2. Resource sharing（資源分享）
  - 默認情況下，執行緒共享公共代碼、數據和其他資源（共用所屬 process 的記憶體和資源），這允許在單個地址空間中同時執行多個任務
3. Economy（經濟）
  - 建立和管理執行緒（以及它們之間的 context switches）比 process 執行相同的任務要快得多
    - 執行緒共用所屬 process 資源，所以建立和 context switches 比較有優勢
4. Scalability（擴展）
  - 多處理器（multiprocessor）體系結構的利用 - **單執行緒進程只能在一個 CPU 上運行**，無論有多少可用，而**多執行緒應用程序的執行可以在可用處理器之間分配**
    - 當有多個 process 競爭 CPU 時，即當負載平均值高於某個特定閾值時，單執行緒進程仍然可以從多處理器體系結構中受益
    
## Multicore Programming（多核心程式撰寫）
- 在傳統的單核芯片上運行的多執行緒應用程序必須交錯線程。但是，在多核芯片上，執行緒可以分佈在可用內核上，從而實現真正的平行處理，系統可以指定一個單獨的執行緒給每一個核心。

![](https://i.imgur.com/WgwtdPC.png "Concurrent execution on single-core system")
![](https://i.imgur.com/YxEG0lm.png "Parallelism on a multi-core system")

### Programming Challenges
多核心系統的撰寫，五個領域的挑戰
1. Identifying tasks（辨識任務）
  - 檢查應用程序以查找可以分割獨立或並行的任務
2. Balance（平衡）
  - 找到同時運行的任務，提供相同的價值。即不要在瑣碎的任務上浪費一個執行緒
3. Data splitting（資料分割）
  - 預防執行緒相互干擾
4. Data dependency（資料相依）
  - 如果一個任務依賴於另一個任務的結果，則需要同步任務以確保以正確的順序進行訪問
5. Testing and debugging（測試和偵錯）
  - 在並行處理情況下固有的更加困難，因為競爭條件變得更加複雜並且難以識別

### Types of Parallelism（平行的類型）
1. Data parallelism
  - 在多個核（執行緒）之間劃分數據，並在每個數據子集上執行相同的任務
    - 例如，將大圖像分成多個區段並對不同核心上的每個區段執行相同的數字圖像處理
2. Task parallelism
  - 劃分不同核心之間要執行的不同任務並同時執行它們
    - 任務分配
    
>在實踐中，任何程式都不會僅僅由這些中的一個或另一個劃分，而是透過兩種策略混合組合

## Multithreading Models（多執行緒模式）
- 在現今系統中有兩種類型的執行緒需要管理：user thread（使用者執行緒）和 kernel thread（內核執行緒）
- 使用者執行緒的支援在核之上，核心執行緒直接由作業系統支援和管理

### Many-To-One Model
- 此模型，許多使用者執行緒都映射到一個內核執行緒
- 如果進行了 block 系統調用，那麼即使其他用者執行緒能夠繼續，整個進程也會 block
- 單個內核執行緒只能在單個 CPU 上運行，因此多對一模型不允許在多個 CPU 之間拆分單個 process 平行執行

![](https://i.imgur.com/QvUHP2r.png "Many-to-One")

### One-To-One Model
- 此模式，建一個單獨的內核執行緒來處理每個使用者執行緒
- 涉及 blocking 系統調用和跨多個 CPU 分離 process
- 開銷更大，涉及更多開銷和減慢系統速度

![](https://i.imgur.com/6oUXz82.png "One-To-One Model")

### Many-To-Many Model
- 此模式將任意數量的使用者執行緒復用到相同或更少數量的內核執行緒上，結合了一對一和多對一模型的最佳特性
- 使用者對創建的執行緒數量沒有限制
- blocking 內核系統調用不會阻止整個 process
- process 可以分散在多個處理器上
- 可以為各個 process 分配可變數量的內核執行緒，具體取決於存在的 CPU 數量和其他因素

![](https://i.imgur.com/w33O2Vx.png "Many-To-Many Model")

##  Threading Issues（執行緒問題）
### The fork() and exec() System Calls
- 如果一個執行緒呼叫 fork，是整個 process 被複製，還是新 process 是單執行緒的？
  - 取決於系統
  - 如果新 process 立即執行，則無需複制所有其他執行緒，如果沒有，則應複製整個過程
  - 許多版本的 UNIX 為此提供了多個版本的 fork 調用
### Signal Handling（訊號處裡）
- 當多執行緒進程收到訊號時，該訊號應傳遞到哪個執行緒？
  - 將訊號傳遞給訊號作用的執行緒
  - 將訊號傳遞給 process 中的每個執行緒
  - 將訊號傳遞給 process 中的某個執行緒
  - 分配特定執行緒以接收 process 中的所有訊號
- UNIX 允許各個執行緒指示它們接受哪些訊號以及它們忽略哪些訊號。但是，訊號只能傳遞給一個執行緒，這通常是接受該特定訊號的第一個執行緒
- UNIX 提供了兩個獨立的系統調用：kill（pid，signal）和 pthread_kill（tid，signal），分別用於向 process 或特定執行緒傳遞訊號。
- Windows 不支持訊號，但可以使用異步過程調用（APC）模擬它們。APC 被傳遞到特定執行緒，而不是 process

###  Thread Cancellation（執行緒取消）
- Asynchronous Cancellation（異步取消）
  - 立即取消執行緒
- Deferred Cancellation（延遲取消）
  - 設置一個標誌，指示執行緒在方便時應自行取消。然後由取消的執行緒定期檢查此標誌，並在看到標誌設置時很好的退出
  
> 異步取消（共享）資源分配和執行緒間數據傳輸可能存在問題
    
