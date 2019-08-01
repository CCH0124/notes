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
