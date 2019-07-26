# Operating-System Structures
## Operating-System Services

![A view of operating system services](https://i.imgur.com/5iPQPu4.png)
- User Interfaces
  - 幾乎所有 OS 都具有用戶界面（UI）、命令列（CLI）、圖形化界面（GUI）
  - 用戶可以向系統發出命令的方法
- Program Execution 
  - OS 必須將程式載入至 RAM 並且執行，並正常或異常終止程序
- I/O Operations
  - 運行程式可能需要 I/O
  - OS 負責與 I/O 設備之間的數據傳輸，包括鍵盤、終端、印表機和儲存設備
- File-System Manipulation
  - 程式需要讀寫檔案和目錄，也需要用名稱建立或刪除甚至搜尋等
  - OS 負責維護目錄和子目錄結構，將文件名映射到特定的數據儲存區塊，以及提供導航和利用文件系統的工具
- Communications
  - process 之間可能會交換訊息，在相同電腦上或由網路串接的不同電腦上
  - 藉由 shared memory 或 message passing 技術完成通訊
    - shared memory
      - 兩個或多個 process 讀寫共用區域的記憶體
    - message passing
      - 經由預設格式 packet 由 OS 在 process 間移動
- Error detection 
  - OS 必須不斷檢測和修正錯誤
    - CPU、memory、I/O、RAID 上
- Resource allocation 
  - 多個使用者有多個 task 要執行，CPU 週期、main memory、storage space 等都要由 OS 去分配給這些使用者
- Accounting 
  - 追蹤系統活動和資源使用情況，用於計費目的或用於優化未來性能的統計記錄保存
- Protection and security
  - protection
    - 涉及確保控制對系統資源的所有訪問
  - security
    - 來自外部人員的系統安全性需要用戶身份驗證，擴展到防止外部 I/O 設備進行無效訪問嘗試
