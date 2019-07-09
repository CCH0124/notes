## What Operating Systems Do

![](https://i.imgur.com/VEpL5tv.png)

一部電腦大致上可分為四個單元：
- hardware（Bare Machine）
    - provides basic computing resources CPU、memory、I/O devices
- operating system
    - Controls and coordinates use of hardware among various applications and users
- application program
    - define the ways in which the system resources are used to solve the computing problems of the users
    - Word processors、compilers、web browsers、database systems、video games
- user
    - People、machines、other computers
 
Computer = hardware + operating system + application program + user

##### Bare Machine & Extended Machine
- Bare Machine(裸機)
    - 只有 Hardware Components 所組成(eg. CPU, memory, I/O device)，沒有任何輔助 user 的 system program(系統程式)存在
- Extended Machine
    - 在 Bare Machine 上加上輔助 user 的System Programs(eg. OS, Compiler, DBMS...)
### For Users

- easy of use
    - OS 對內對外管理了資源
-  Don’t care about resource utilization

##### Example
1. 大型電腦或迷你電腦作業系統
   - 藉由終端機連接到大型電腦或迷你電腦來共享資源及交換訊息
   - 發揮系統資源最大使用率（cpu 時間、記憶體、I/O）
   - 使用者程式不會相互影響
2. workstation 或 server 作業系統
   - 使用者有特定的資源
   - 分享資源共享
       - 網路
       - 伺服器（檔案、運算）
3. 手持式系統作業系統
   – 使用方便
   
##### OS

- 作業系統是電腦上的 resource allocator，負責管理並有效且公平地分配資源（cpu 時間、記憶體空間、檔案儲存空間、 I/O 裝置）
- OS is a resource allocator
    - 管理所有資源決定衝突的請求之間有效和公平的資源使用
- OS is a control program
    - 控製程序的執行以防止錯誤和不當使用電腦

>"The one program running at all times on the computer" is the kernel.

## Computer-System Organization

1. 靴帶式程式（bootstrap program）存於唯讀記憶體（ROM）或可消除式唯讀記憶體（EEPROM）[通常稱為**韌體** firmware]
2. 靴帶式程式找作業系統的核心，將作業系統核心載入**記憶體**
3. 執行 OS

![A modern computer system](https://i.imgur.com/EKy5uKh.png)

匯流排分為三種
- control
- Address
- data

上圖由 databus 傳輸，由 OS 管理那些資源。


![Interrupt Timeline](https://i.imgur.com/Vzd8WOZ.png)

1. 由硬體或軟體產生（interrupt）
- 硬體
  - 硬體在任何時間由系統匯流排，給 CPU 一個訊號觸發中斷
- 軟體
  - 藉由 system call，觸發中斷
2. CPU 被中斷時，立即停止正在運行的工作
3. 並把被中斷程式的位址傳到 interrupt table，轉換執行到一個固定的位址
4. 動作執行完後，CPU 變回到中斷前的運算
5. 紀錄 interrupt 在哪個程式執行到哪個記憶體片段 contains the memory address(es) of all the service routines (ISR) 採用 Stack 方法將中斷時每一步驟之位址依序紀錄於特定堆疊中，等待中斷事件執行完畢後，在由堆疊中取得各位址，反序找到中斷位址，繼續執行原來之工作

### Storage-Device Hierarchy

![](https://i.imgur.com/JDH0CFE.png)

- Main memory ( RAM )
  - Programs must be loaded into RAM to run
- Other electronic ( volatile ) memory is faster, smaller, and more expensive per bit
  - Registers
  - CPU Cache
- Non-volatile memory ( "permanent" storage ) is slower, larger, and less expensive per bit
  - Magnetic disks
  - Optical disks
  - Magnetic Tapes
  
## Computer-System Architecture
###  I/O Structure
典型操作涉及 `I/O` 請求，`direct memory access`（DMA）和 `interrupt` 處理。
![](https://i.imgur.com/PAKnoJr.png)

- Direct Memory Access （DMA）
  - 用於能夠以接近 memory 速度傳輸訊息的高速 I/O 設備
  - 設備控制器將數據塊從 buffer storage 直接傳輸到 main memory，無需 CPU 干預。
  - 每個 block 只產生一個中斷，而不是每個 byte 一個中斷
- CPU
  - concurren
  - parallel

### Symmetric Multiprocessing Architecture

![Symmetric multiprocessing architecture](https://i.imgur.com/Fa7f9VS.png)

- Single-Processor Systems
  - 一個主要 CPU 管理計算機並運行用戶應用程序
  - 其他特定處理器（Disk controllers，GPU 等）不運行用戶應用程序。
- Multiprocessor Systems
  - 一部機器中，同時具有多顆 CPUs（或 processors）
    - 這些 processors 共享 memory、I/O Device、Bus
    - 受同一個 clock 控制
    - 一般也受同一個 OS 所管控
    - processors 之間的溝通，是採 share memory 方式
  - 支援 parallel computing (平行計算)
  - Increased throughput
    - 執行速度更快，但不是100％線性加速。
  - Economy of scale
    - 外圍設備（Disk、memory）處理器之間共享。
  - Increased reliability
    - Graceful degradation、Fault-Tolerant（Fault-Soft Syetem）無視硬體失誤而持續作業系統的能力，稱適度的降級

![ A dual-core design with two cores placed on the same chip](https://i.imgur.com/Xdtstj5.png)

兩個核心放置在同一芯片上

### Clustered Systems

![](https://i.imgur.com/wvX3I4O.png)

- Usually sharing storage via a storage-area network (SAN)
- Provides a high-availability service which survives failures
- 集合許多 CPU 以完成計算工作，不同於 parallel system，它們是由兩個或更多個別系統集結組成，彼此經由 LAN 緊密連結而成，且共享相同的stroage device 目的在於提供 High availability
- 分為兩種 modes
    - Asymmetric clustering
        - 有一台機器處於 Hot-standby，用於監督其他正在執行應用程式的機器，若某台機器壞了，則此監督者可以取得壞掉機器的儲存體所有權，並重新執行其正執行的應用程式
- Symmetric clustering
    - 兩台或多台機器在執行應用程式，且彼此互相監督，此 mode 較有效率（它可以使用所有可以使用之 HW）

## Operating-System Structure

### A time-sharing ( multi-user multi-tasking )
- Memory management
- Process management
- Job scheduling
- Resource allocation strategies
- Swap space / virtual memory in physical memory
- Interrupt handling
- File system management
- Protection and security
- Inter-process communications

- 是 Multiprogramming 的一種，在 CPU 排班法則方面，其使用 RR(Round-Robin)法則
- 即 OS 規定一個 CPU time quantum，若 process 在取得 CPU 後，未能於 quantum 內完成工作，則必須被迫放棄 CPU，等待下一次輪迴。
對每個 user 皆是公平的
- 適用在 user interactive 且 response time(反應時間)要求較短的系統環境
- 透過 resource sharing 技術（eg. CPU scheduling、memory sharing、spooling 達到 I/O Device 共享）使得每個 user 皆認為有專屬的系統存在。

![Memory layout for a multiprogramming system](https://i.imgur.com/FT8lY2M.png)


##### creating interactive computing
Response time should be < 1 second

Each user has at least one program executing in memory ⇨ **process**

If several jobs ready to run at the same time ⇨ **CPU scheduling**

If processes don’t fit in memory, **swapping** moves them in and out to run

**Virtual memory** allows execution of processes not completely in memory

>Multiprogramming system，其中它是以 Concurrent 模式去執行

## Operating-System Operations

![](https://i.imgur.com/1PX2gf4.png)

- **Interrupt** driven by hardware
- A **trap** or an **exception**
    - Software error or reques
        - Division by zero, request for operating system service
### Dual-Mode and Multimode Operation

- Dual-mode operation 操作允許操作系統保護自身和其他系統組件
    - User mode
        - 在此 mode 之下，不能執行特權指令，否則會產生致命錯誤中斷(trap)，OS 會強迫 process 中止
        - CPU 所提供的執行模式之一，只能存取有限的硬體資源
            - 普通運算所需的暫存器
            - 部分記憶體內容
    - kernel mode
        - 是 OS 的 system processes 在執行(OS 的 system process 執行的狀態，在此 mode 下，OS 掌控系統的控制權)(eg. ISR、systemcall，對應的 service routine)
        - 有權執行特權指令(priveleged instruction)
        - 可以對硬體做任何的變更
            - 控制暫存器（control registers）
            - 所有記憶體
    - Mode bit provided by hardware
        - 0：kernel mode
        - 1：user mode
        - 提供區分系統何時運行 user 代碼或 kernel 代碼的能力
        - 一些指令被指定為 **privileged**，只能在 kernel mode 下執行
        - 系統調用更改模式到內核，從調用返回將其重置為用戶

>Dual mode 目的，對 Hardware 重要的 resources 實施 protection，把可能引起危害的一些機器指令，設為 priveleged instruction，如此可防止 user program 直接使用這些指令，避免 user program 執行這些指令對系統或其它 user 造成危害

### Timer
- 在 `kernel` 開始執行用戶代碼之前，設置定時器以生成中斷
- 定時器 `interrupt` 處理恢復控制回內核
- 這確保沒有用戶 `process` 可以接管系統
- 定時器控制是一種 `privileged` 指令（需要 `kernel mode`）。

## Process Management
- A process is a program in execution.
    - 系統內的一個單元
    - Program is a **passive entity**
    - process is an **active entity**
- process 需要資源，結束任務後必須釋放資源
- Single-threaded process 有一個 **program counter**，用於指定要執行的下一條指令的位置



## Memory Management
## Storage Management
## Protection and Security
## Kernel Data Structures
## Computing Environments
## Open-Source Operating Systems
## Ref
[](https://www.cs.uic.edu/~jbell/CourseNotes/OperatingSystems/?fbclid=IwAR1wr5Q62scV03fYFSAkax4Z5A_9V0LlqvO6IjJ7cOplwfyiVTOXOugTrD4)
