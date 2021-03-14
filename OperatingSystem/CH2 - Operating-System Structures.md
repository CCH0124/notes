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

## System Programs
- `system program` 又稱 `system utility`，它們提供一些程式開發與執行的便利環境，有些則是使用者和系統呼叫之間的介面
- `System programs` 可分為這幾類
  - File management
    - 用於創建、刪除、複製、重命名、列印，列出和一般操作檔案和目錄的程序
  - Status information
    - 用於檢查日期、時間、用戶數、運行進程、數據記錄等的實用程序
    - 系統註冊表用於儲存和調用特定應用程序的配置訊息
  - File modification
    - 文字編輯器和其他可以更改文件內容的工具
  - Programming-language support
    - Compilers、linkers、debuggers、profilers、assemblers、library archive management，通用語言的解釋器以及 make 的支持
  - Program loading and execution
    - loaders、 dynamic loaders、 overlay loaders 等，以及交互式調試器
  - Communications
    - 用於在進程和用戶之間提供連接的程序，包括郵件、Web瀏覽器、遠端登錄、文件傳輸和遠程命令執行。
  - Background services
    - System daemons 通常在系統引導時啟動，並在系統運行時運行，處理必要的服務。示例包括 network daemons，列印服務器，process schedulers 和系統錯誤監視服務
    
## System Boot

- 在系統上初始化電源時，執行從固定的內存位置開始。
  - Firmware ROM 用於保存初始 boot code。
- 必須使 OS 可用於硬體，以便硬體可以啟動它。
  - 一小段代碼 - 存儲在ROM 或 EEPROM 中的 bootstrap loader 定位內核，將其加載到內存中並啟動它
  - 有時兩步過程，其中固定位置的 boot block 由 ROM 代碼加載，從 disk 加載 bootstrap loader。
- 通用 bootstrap loader GRUB 允許從多個 disk、版本、內核選項中選擇內核。
- 然後運行 Kernel loads 和系統。

[補充](https://www.cs.uic.edu/~jbell/CourseNotes/OperatingSystems/2_Structures.html)

- 當系統上電時，會產生一個中斷，將一個儲存器地址加載到 `program counter` 中，系統開始執行在該地址找到的指令。該地址指向位於主板上 ROM 芯片（或EPROM 芯片）中的 `bootstrap` 程序。
- ROM 引導程序首先運行硬體檢查，確定存在的物理資源以及執行適用的所有 HW 的開機自檢（POST）。某些設備（例如 controller cards）可能具有自己的板載診斷，這些診斷由 ROM 引導程序調用。
- 用戶通常可以選擇在 POST 過程中按下特殊鍵，如果按下，將啟動 `ROM BIOS` 配置實用程序。此實用程序允許用戶指定和配置某些硬體參數，以查找操作系統的位置以及是否使用密碼限制對該實用程序的訪問。
  - 某些硬體還可以提供對其他配置設置程序的訪問，例如 `RAID` disk 控制器或某些特殊圖形或網卡。
- 假設尚未調用該實用程序，則引導程序查找包含 OS 的 non-volatile storage 設備。根據配置，它可能按照 HW 配置實用程序指定的順序查找 floppy drive、CD ROM 驅動器或 primary hard drives 驅動器或 secondary hard drives 驅動器。
- 假設它進入硬碟驅動，它將找到硬碟驅動上的第一個扇區並加載 fdisk 表，其中包含有關如何將物理硬碟驅動劃分為邏輯分區的訊息，每個分區的開始和結束位置，以及 partition 是用於啟動系統的"active"分區。
- 在第一個硬碟區塊的未被 fdisk 表佔用的部分中也存在非常少量的系統代碼。此 `bootstrap code` 是未構建到硬體中的第一步，即第一部分可能以任何方式特定於OS。通常，此代碼只知道訪問硬碟驅動，以及加載和執行（略微）更大的導程序。
- 對於單啟動系統，從硬碟加載的 boot program 將繼續在硬碟驅動上定位內核，將內核加載到內存中，然後將控制轉移到內核。可能有一些機會指定要在此階段加載的特定內核，如果剛剛生成並且不起作用的新內核，或者系統具有針對不同目的的具有不同配置的多個內核，則這可能是有用的。（某些系統可能會自動啟動不同的配置，具體取決於之前步驟中找到的硬體。）
- 對於雙啟動或多啟動系統，啟動程序將為用戶提供指定要加載的特定 OS 的機會，如果用戶未在給定時間範圍內選擇特定 OS，則使用默認選項。 然後，`boot program` 找到所選單引導 OS 的引導加載程序，並按照上一個項目符號點中的描述運行該程序。
- 內核運行後，它可以讓用戶有機會進入單用戶模式，也稱為維護模式。 如果有任何系統服務，此模式啟動的次數非常少，並且除控制台上的主登錄之外不啟用任何其他登錄。 該模式主要用於系統維護和診斷。
- 當系統進入完全多用戶多任務模式時，它會檢查配置文件以確定要啟動哪些系統服務，並依次啟動每個服務。 然後，它會在已配置為啟用用戶登錄的每個登錄設備上生成登錄程序（gettys）。
  - （getty程序初始化終端 I/O，發出登錄提示，接受登錄名和密碼，並對用戶進行身份驗證。如果用戶的密碼經過身份驗證，則 getty 會在系統文件中查找以確定為用戶分配的 shell， 然後"execs"（成為）用戶的 shell。shell 程序將查看系統和用戶配置文件以初始化自己，然後發出用戶命令的提示。每當 shell 死亡時，通過註銷或其他方式，然後系統 將為該終端設備發布新的 getty。） 

## OS Architecture
##### Simple 
- 簡單、快速，但安全性不高

![](https://i.imgur.com/FjjXEGt.png)

##### Layer   

![](https://i.imgur.com/sGscSHR.png)

- 容易除錯，原因是每一層只能存取內層的東西

##### Microkernel 

![](https://i.imgur.com/uP27C3a.png)

- 安全性提高，Kernel 變小很多
- 溝通透過 Message pass 效能不是很好

##### Modular

![](https://i.imgur.com/d4a8QDh.png)

- Uses object-oriented approach
- Each core component is separate
- Each talks to the others over known interfaces
- Each is loadable as needed within the kernel

##### Virtual Machine
- 在作業統上的 Kernel 模擬一個 Kernel 使得每台虛擬機相互隔離
- 但實際上虛擬機的 Kernel 是運行在 User Space 上因此，做呼叫時會透過 interrupter 到實體機的 Kernel 進行動作，這也導致虛擬機效能不好的原因
- 現今的 CPU 或一些硬體可以資源虛擬化，這也讓虛擬機的 Kernel 可被識別，這提升了效能
