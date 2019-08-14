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
