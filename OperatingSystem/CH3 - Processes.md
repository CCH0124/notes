# Processes
## Process Concept
- process 是執行中的程序的實例
- process 是儲存在硬碟上的 **passive** 實體（可執行文件），process 是 `active` 的
  - 當可執行檔案加載到 memory 中時程序成為 process
### The Process
process memory 分為四個部分：

![](https://i.imgur.com/oy9BbJl.png)

- `text` 包括編譯的 program code，在程序啟動時從 non-volatile 存儲器讀入。當前 activity 包括 `program counter`，處理器暫存器
- `data` 部分儲存全局和靜態變數，在執行 main 之前分配和初始化
- `heap` 用於動態 memory 分配，並通過調用 new、delete、malloc、free 等進行管理
- `stack` 用於 `local variables`。 當宣告它們時（在函數入口或其他地方，取決於語言），stack 上的空間被保留用於 `local variables`，並且當變數超出範圍時，空間被釋放
  - 請注意，stack 也用於函數返回值，stack 管理的確切機制可能是特定於語言的
  - containing temporary data, Function parameters, return addresses, local variables
  
> stack 和 heap 從 process 的可用空間的兩端開始並朝向彼此增長。如果它們無配置空間，則會發生 stack 溢出錯誤，或者由於 memory 不足而對 `new` 或 `malloc` 的調用將失敗
  
- 當 process 從 memory 中換出並稍後還原時，還必須儲存和還原其他訊息。其中的關鍵是 `program counter` 和所有程序暫存器的值。
  
### Process State
進程可能處於 5 種狀態之一 

![process state](https://i.imgur.com/1N7ub3p.png "process state")

- New
  - 該 process 正在產生中
- Ready 
  - 該 process 具有運行所需的所有可用資源，但 CPU 當前沒有處理此進程的指令
- Running 
  - CPU 正在處理此過程的命令
- Waiting 
  - 此 process 目前無法運行，因為它正在等待某些資源可用或某些事件發生。例如，process 可能正在等待鍵盤輸入、硬碟訪問請求、process 消息、計時器關閉或子進程完成。
- Terminated 
  - 這 process 已經完成
  
### Process Control Block
每一個 process 在 OS 之中都對應著一個**行程控制表（process control block, PCB）**。

![PCB](https://i.imgur.com/ckITcAM.png "PCB")

- Process State
  - 可以是 `new`、`ready`、`running`、`waiting` 或 `halted` 等，在 process state 中有提到過
- Process ID
  - 父 process ID
- program counter
  - 表示該 process 下一個要執行的指令位址
- CPU registers and Program Counter
  - 在將 process 交換進出 CPU 時，需要保存和恢復這些內容
  - 下圖
![Diagram showing CPU switch from process to process](https://i.imgur.com/51Ulf7G.png "Diagram showing CPU switch from process to process")
- CPU-Scheduling information
  - 包括 `priority`、`queue` 等方式
- Memory-Management information
  - `page tables` 或 `segment tables`
  - `base register` 或 `limit register`
- Accounting information
  - 用戶和內核 CPU 消耗的時間、帳號、限制或 process id 等
- I/O Status information
  - 分配裝置，包刮開啟檔案的串列等

**PCB 只是用來儲存各個 process 的相關資訊而已**。

## Process Scheduling
Process Scheduling 主要是要讓 CPU 保持忙碌，快速切換 CPU 已進行時間的共享。

### Scheduling Queues
- 所有 process 都儲存至 `job queue`
- 位於主記憶體中且就緒等待的 process 是保存在一個叫 `ready queue` 串列中
- 等待設備變為可用或傳遞數據的 process 被放置在 `device queue` 中。每個設備通常都有一個單獨的 `device queue`
![The ready queue and various I/O device queues](https://i.imgur.com/TMEibhZ.png "The ready queue and various I/O device queues")

### Schedulers
- long-term scheduler 
  - 典型的批處理系統或負載很重的系統。 它不經常運行（例如當一個 process 結束選擇另一個 process 從硬碟加載到其位置時），並且可以花時間來實現智能和高級調度算法。
  - 選擇應將哪些 process 帶入就緒隊列，從 process pools 中選擇 process，並將它們載入至記憶體
  - 控制著 degree of multiprogramming（記憶體中 process 數量）
- short-term scheduler
  - 非常頻繁地運行，大約 100 毫秒，並且必須非常快速地將一個 process 交換出 CPU 並交換另一個 process
  - 選擇下一個應該執行的 process 並分配 CPU，從 memory 中選出一個已經準備就緒的 process
- medium-term scheduler
  - 此調度程序將一個或多個 process 從 ready queue 系統中交換幾秒鐘，以便允許更小的更快作業快速完成並清除系統 
  - 從 memory 中刪除 process，儲存在硬碟上，從硬碟恢復以繼續執行：**swapping**
- 高效的調度系統將選擇 CPU-bound process 和 I/O-bound process 的良好 process 組合
  - CPU-bound process
    - 花費更多時間進行計算，幾乎沒有很長的 CPU 爆發
  - I/O-bound process
    - 花費更多時間進行 I/O 而非計算，許多短 CPU 突發
 
 ![Queueing-diagram representation of process scheduling](https://i.imgur.com/Re8RSAY.png "Queueing-diagram representation of process scheduling")
 
 ![Addition of a medium-term scheduling to the queueing diagram](https://i.imgur.com/AjSr5ZY.png "Addition of a medium-term scheduling to the queueing diagram")

### Context Switch
- 每當中斷來時，CPU 必須對當前正在運行的 process 執行狀態保存，然後切換到內核模式以處理中斷，然後對中斷的 process 進行狀態恢復
- 類似的，當一個 process 的時間配量已到期並且要從 `ready queue` 加載新 process 時，會發生上 **Context Switch**
  - 這將由定時器中斷發起，然後中斷將導致當前 process 的狀態被保存並且新 process 的狀態將被恢復。
- 保存和恢復狀態涉及保存和恢復所有暫存器和 program counter，以及上述 `PCB`。
- **Context Switch** 切換非常頻繁發生，並且執行切換的開銷只是丟失了 CPU 時間，因此 **Context Switch**（狀態保存和恢復）需要盡可能快。
  - 某些硬體具有加速此操作的特殊規定，例如一次性保存或恢復所有暫存器的單機指令
## Operations on Processes
### Process Creation
- Process 可以透過適當的系統調用創建其他 process，例如 `fork` 或 `spawn`。進行創建的過程稱為另一個過程的父進程（process），稱為其子進程（process）
- 每個 process 都有一個整數標識符，稱為進程標識符或 `PID`
  - 為每個 process 儲存父 PID（PPID）
- 典型 UNIX 系統上，process 調度程序被稱為 **sched**，並且給定 **PID 0**。 它在系統啟動時做的第一件事就是啟動 `init`，它給出了 process **PID 1**。Init 後啟動所有系統守護進程和用戶登錄，並成為所有其他 process 的最終父進程。 下圖顯示了 Linux 系統的典型流程樹

![A Tree of Processes in Linux](https://i.imgur.com/bif4V3H.png "A Tree of Processes in Linux")

- Resource sharing options
  - 子進程可能會與其父進程接收一些共享資源。子進程可以或可以不限於最初分配給父進程的資源的子集，從而防止失控的子進程消耗所有特定係統資源。
    - Parent and children share all resources
    - Children share subset of parent’s resources
    - Parent and child share no resources
- 創建子進程後，父進程有兩個選項
  - 在繼續之前等待子進程終止。對於特定子級或任何子級，父級進行 `wait（）`系統調用，這會導致父進程阻塞，直到 `wait（）` 返回。 UNIX shell 通常會在發出新提示之前等待其子進程完成。
    - Parent and children execute concurrently（父進程議續執行而子進程也同時執行）
  - 與子進程同時運行，繼續處理而無需等待。 這是 UNIX shell 將 process 作為後台任務運行時的操作。父進程也可能運行一段時間，然後等待子進程，這可能發生在某種並行處理操作中。（例如，父母可以在不等待任何一個孩子的情況下 `fork` 一些孩子，然後做一些自己的工作，然後等待孩子。）
    - Parent waits until children terminate（父進程等著他的所有子進程終止後才繼續執行）
- 子進程相對於父進程的地址空間的兩種可能性
  - 子進程可以是父進程的精確副本，在內存中共享相同的 `program` 和 `data segments`。每個都有自己的 `PCB`，包括 `program counter`、暫存器和 `PID`。 這是 UNIX 中 fork 系統調用的行為
    - Child duplicate of parent（子進程是父進程的複製品）
  - 子進程可能有一個新程序加載到其地址空間，包含所有 `program` 和 `data segments`。這是 Windows 中的 `spawn` 系統調用的行為。UNIX 系統使用 `exec` 系統調用作為第二步實現此目的。
    - Child has a program loaded into it（子進程有一個程式載入其中）

![Process Creation](https://i.imgur.com/4AC4uEl.png "Process Creation")

## Process Termination
- Process 可以透過 `exit()` 系統調用來請求它們自己要終止，通常返回一個 `int`。如果它正在執行 `wait()`，則將此 `int` 傳遞給父進程，並且在成功完成時通常為`0`，並且在出現問題時通常為非零代碼。
- 系統也可以出於各種原因終止 process：
  - 系統無法提供必要的系統資源
  - 響應 KILL 命令或其他未處理的 process 中斷
  - 如果不再需要分配給他們的任務，父進程可以 kill 他們的子進程
  - 如果父進程退出，系統可能會或可能不會允許子進程在沒有父進程的情況下繼續。（在 UNIX 系統上，孤立進程通常由 `init` 繼承，然後繼續執行它們，UNIX `nohup` 命令允許子進程在父進程退出後繼續執行。）
- **當進程終止時，其所有系統資源都被釋放**，打開檔案被刷新和關閉等。如果父進程正在等待子進程終止，則進程終止狀態和執行時間將返回到父進程，如果進程變為孤立，則最終返回到 init。（正在嘗試終止但不能因為父進程不等待它們的進程被稱為殭屍。這些進程最終被 init 作為孤兒繼承並被殺死。）
  - 如果沒有父進程等待，那麼終止進程就是一個 `zombie`
  - 如果父進程終止，則進程是 `orphans` 的
>現代 UNIX shell 不會產生與舊系統一樣多的孤兒和殭屍。

## Interprocess Communication
- Independent Processes
  - 在系統上同時運行的**獨立進程**是那些既不會影響其他 process 也不會受其他進程影響的 process
- Cooperating Processes 
  - 合作行程是指可能影響或受其他 process 影響的 process。允許合作進程的原因有幾個：
    - Information Sharing
      - 可能有幾個 process 需要訪問同一檔案。（例如 pipelines）
    - Computation speedup
      - 如果問題可以分解為要同時解決的子任務（特別是涉及多個處理器時），通常可以更快的解決問題的解決方案
    - Modularity
      - 最有效的架構可能是將系統分解為協同模組。（例如，具有客戶端 - 服務器體系結構的數據庫。）
    - Convenience 
      - 即使是單個用戶也可能是多任務的，例如編輯、編譯、列印以及在不同的窗口中運行相同的代碼
- `Cooperating processes` 需要某種類型的行程間通信，這通常是兩種類型之一：`shared memory` 或 `message passing`。下圖說明了兩個系統之間的區別

![Communications models](https://i.imgur.com/2XtM18S.png "Communications models: (a) Message passing. (b) Shared memory.")
  
- Shared Memory
  - 一旦設置就會更快，因為不需要系統調用，並且以正常的內存速度進行訪問。但是，**設置起來更複雜**，並且在多台計算機上不能正常工作。當必須在同一台計算機上快速共享大量訊息時，`Shared Memory` 通常是首選
- Message Passing
  - `Message Passing` 需要對每個訊息傳輸**進行系統調用**，因此速度較慢，但設置起來比較簡單，並且可以在多台計算機上正常工作。當數據傳輸的數量和/或頻率很小時，或者涉及多台計算機時，通常選 `Message Passing`。

### Shared-Memory Systems 共享記憶體
- 通常，**要在共享記憶體系統中共享的記憶體最初在特定 process 的地址空間內**，該 process 需要進行系統調用以使該記憶體對一個或多個其他 process 公開可用
- 其他希望使用共享記憶體的 process 必須進行自己的系統調用，以將共享記憶體區域連接到其地址空間
- 通常，首先必須在 `cooperating processes` 之間來回傳遞一些消訊息，以便建立和共享記憶體訪問

### Message-Passing Systems 訊息傳遞
- 消息傳遞系統必須至少支持系統調用"send message"和"receive message"
- 在發送消息之前，必須在 `cooperating processes` 之間建立通訊連線
- 在消息傳遞系統中有三個要解決的關鍵問題
  - Direct or indirect communication（直接聯繫）
  - Synchronous or asynchronous communication（同步或非同步）
  - Automatic or explicit buffering（自動或明確緩衝）

##### Naming
- 透過直接聯繫，發送方必須知道向發送消息的接收方的名稱
  - 每個發送者 - 接收者對之間存在一對一的連接
  - 對於對稱通訊，接收方還必須知道從其接收消息的發送方的特定名稱
    - 對於非對稱通信，這不是必需的
- 間接通訊使用共享郵箱或端口（port）
  - 多個 process 可以共享相同的郵箱

##### Synchronization
- 無論是發送或接收消息（或者都不是或兩者）的可以是 `blocking`  或 `non-blocking`，也稱為 `synchronous` 和 `asynchronous`

##### Buffering
- 消息透過隊列（queue）傳遞，隊列可能具有三種容量配置之一
  - Zero capacity（零容量）
    - 消息無法儲存在隊列中，因此發送方必須 `blocking`，直到接收方接受消息。
  - Bounded capacity（有限的容量）
    - 隊列中存在一定的預設有限容量。如果隊列已滿，發送發必須 `block`，直到隊列中的空間可用，否則可能是 `block` 或 `non-block`
  - Unbounded capacity （無限制的容量）
    - 隊列具有理論上的無限容量，因此發送者永遠不會被強制 `block`
