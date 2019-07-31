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

![](https://i.imgur.com/1N7ub3p.png)

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
