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
## System Calls

![Example of how system calls are used.](https://i.imgur.com/m0JTRVs.png)

- `system call` 為用戶或應用程序提供一個由 OS 服務的介面

![](https://i.imgur.com/CDrHFlY.png)
- 使用 `API` 而不是直接系統調用，可以在不同系統之間提供更大的程序可移植性
  - `API` 透過系統調用接口進行適當的系統調用，使用表查找來訪問特定的編號系統調用
- 將參數傳遞給 OS 的三種常規方法
  - 參數傳遞於 `registers` 中
  - 用 `block`、`table` 儲存的方式存在 memory 中
    - `block` 的位址是置於 `registers` 的參數傳遞
    - 程式將參數放置或推入 stack，並由操作系統彈出 stack
      - 不限制傳遞的參數的數量或長度

![](https://i.imgur.com/PWR3tqS.png)

## Types of System Calls
- Process control
  - end, abort（正常結束、中止執行）
  - load, execute（載入、執行）
  - create process, terminate process（建立行程、終止行程）
  - get process attributes, set process attributes（獲取行程屬性、設定行程屬性）
  - wait for time（等待時間）
  - wait event, signal event（等待事件、訊號事件）
  - allocate and free memory（配置及釋放記憶體空間）
  - Dump memory if error
  - Debugger for determining bugs, single step execution
  - Locks for managing access to shared data between processes
- File management
  - create file, delete file（建立檔案、刪除檔案）
  - open, close file（開啟、關閉）
  - read, write, reposition（讀、寫、重定位置）
  - get and set file attributes（獲取、設定檔案屬性）
- Device management
  - request device, release device（要求、釋放裝置）
  - read, write, reposition（讀入、寫出、重定位置）
  - get device attributes, set device attributes（獲取裝置屬性、設定裝置屬性）
  - logically attach or detach devices（邏輯上的加入或移除裝置）
- Information maintenance
  - get time or date, set time or date（獲取時間或日期、設定時間或日期）
  - get system data, set system data（取得系統資料、設定系統資料）
  - get and set process, file, or device attributes（設定行程、檔案或裝置屬性）
- Communications
  - create, delete communication connection（建立、刪除通訊連接）
  - send, receive messages if message passing model to host name or process name（傳輸、接收訊息）
  - From client to server
  - Shared-memory model create and gain access to memory regions
  - transfer status information（傳輸狀況訊息）
  - attach and detach remote devices（連接或移除遠端裝置）
- Protection
  - Control access to resources
  - Get and set permissions
  - Allow and deny user access
  
![Standard C Library Example](https://i.imgur.com/M5d30cW.png)

