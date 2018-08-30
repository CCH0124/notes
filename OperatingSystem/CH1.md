## OS 示意圖
![](https://2.bp.blogspot.com/-gzomOqKpa74/VLPwd72Q8KI/AAAAAAAAk-E/mnLR8NAJfLY/s640/%E8%9E%A2%E5%B9%95%E5%BF%AB%E7%85%A7%2B2014-12-12%2B%E4%B8%8B%E5%8D%882.48.33-17.png)

一部電腦大致上可分為四個單元
- hardware（Bare Machine）
    - provides basic computing resources CPU、memory、I/O devices
- operating system
    - Controls and coordinates use of hardware among various applications and users
- application program
    - define the ways in which the system resources are used to solve the computing problems of the users
    - Word processors、compilers、web browsers、database systems、video games
- user
    - People、machines、other computers

## Bare Machine & Extended Machine
- Bare Machine(裸機)
    - 只有 Hardware Components 所組成(eg. CPU, memory, I/O device)，沒有任何輔助 user 的 system program(系統程式)存在
- Extended Machine
    - 在 Bare Machine 上加上輔助 user 的System Programs(eg. OS, Compiler, DBMS...)

## 使用者觀點看 OS
- easy of use
    - OS 對內對外管理了資源
-  Don’t care about resource utilization
### 範例
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
## 系統觀點

- 作業系統是電腦上的 resource allocator，負責管理並有效且公平地分配資源（cpu時間、記憶體空間、檔案儲存空間、 I/O裝置）
- OS is a resource allocator
    - 管理所有資源決定衝突的請求之間有效和公平的資源使用
- OS is a control program
    - 控製程序的執行以防止錯誤和不當使用電腦

>"The one program running at all times on the computer" is the kernel.

## Computer-System Organization

1. 靴帶式程式（bootstrap program）存於唯讀記憶體（ROM）或可消除式唯讀記憶體（EEPROM）[通常稱為**韌體** firmware]
2. 靴帶式程式找作業系統的核心，將作業系統核心載入**記憶體**
3. 執行 OS

![](https://i.imgur.com/EKy5uKh.png)

匯流排分為三種
- control
- Address
- data

上圖由 databus 傳輸，由 OS 管理那些資源。

## 發生事件

1. 由硬體或軟體產生（interrupt）
- 硬體
    - 硬體在任何時間由系統匯流排，給 CPU 一個訊號觸發中斷
- 軟體
    - 藉由 system call，觸發中斷
2. CPU 被中斷時，立即停止正在運行的工作
3. 並把被中斷程式的位址傳到 interrupt table，轉換執行到一個固定的位址
4. 動作執行完後，CPU 變回到中斷前的運算

>紀錄 interrupt 在哪個程式執行到哪個記憶體片段
contains the memory address(es) of all the service routines (ISR)
採用 Stack 方法將中斷時每一步驟之位址依序紀錄於特定堆疊中，等待中斷事件執行完畢後，在由堆疊中取得各位址，反序找到中斷位址，繼續執行原來之工作

![](https://i.imgur.com/Z4Csqa1.png)
## Storage-Device Hierarchy
![](https://i.imgur.com/IMKWzvs.png)
## How a Modern Computer Works
- Direct Memory Access （DMA）
    - 用於能夠以接近 memory 速度傳輸訊息的高速 I/O 設備
    - 設備控制器將數據塊從 buffer storage 直接傳輸到 main memory，無需 CPU 干預。
    - 每個 block 只產生一個中斷，而不是每個 byte 一個中斷
- CPU 
    - concurren
    - parallel

![](https://i.imgur.com/mPmA7I7.png)

## Symmetric Multiprocessing Architecture

![](https://i.imgur.com/UDo5JpQ.png)

此圖可看出共用 clock 和匯流排。
- Multiprocessor 多顆處理器系統
    - 一部機器中，同時具有多顆 CPUs（或 processors）
        - 這些 processors 共享 memory、I/O Device、Bus
        - 受同一個clock控制
        - 一般也受同一個OS所管控
        - processors 之間的溝通，是採 share memory 方式
    - 支援parallel computing(平行計算)
    - 可再分成兩種型態
        - Symmetric Multiprocessors(SMP)
            - 每個processor的能力皆相同，即可負責的功能完全一樣。萬一某個processor壞了，其上工作可由其他processor接手，系統不會整個crash，只是整體效能下降而已
            - Reliability（可靠性）大幅提升
            - 強調 load balancing（每個CPU的工作負擔相同）
        - Asymmetric Multiprocessors(ASMP)
            - 不同（群）顆的 processor，其負擔的功能不盡相同，此架構下，通常含有一個 Master CPU，負責工作的分派與協調，其他 CPUs 稱為 Slave CPU（稱為 Master-Slave Architecture）效能通常較 SMP 高，但可靠度較差
    - Multiprocessors優點
        - 產能提升
        - 經濟效益（成本低）
        - 可靠性提升
            - Graceful degradation、Fault-Tolerant（Fault-Soft Syetem）無視硬體失誤而持續作業系統的能力，稱適度的降級
## Clustered Systems
- Usually sharing storage via a **storage-area network** (SAN)
- Provides a **high-availability** service which survives failures
- 集合許多 CPU 以完成計算工作，不同於 parallel system，它們是由兩個或更多個別系統集結組成，彼此經由 LAN 緊密連結而成，且共享相同的stroage device 目的在於提供 High availability
- 分為兩種 modes
    - Asymmetric clustering
        - 有一台機器處於 Hot-standby，用於監督其他正在執行應用程式的機器，若某台機器壞了，則此監督者可以取得壞掉機器的儲存體所有權，並重新執行其正執行的應用程式
    - Symmetric clustering
        - 兩台或多台機器在執行應用程式，且彼此互相監督，此 mode 較有效率（它可以使用所有可以使用之HW）

![](https://i.imgur.com/1ZP4QYo.png)

## Operating System Structure
### Multiprogramming System
- 單個用戶無法始終保持 CPU 和 I/O 設備忙碌
- Multiprogramming 設置組織作業（code 和 data），因此 CPU 始終有一個要執行的作業
- 經由 job scheduling 選擇並運行一個作業
- 允許系統（or memory）內存在多個 process （處理程序）同時執行，透過 CPU scheduling 技術，當某個 process 取得 CPU 執行時，若因為某些事情發生(eg. wait for I/O complete, resource not available, etc.)而無法往下執行時，則 OS 可將 CPU 切換給其他 process 使用，如此一來，CPU 在各個 processes 切換，則 CPU 總是 Busy
    - 可以避免 CPU idle，提高 CPU utilization
- Multiprogramming Degree
    - 系統內存在執行的 process 數目，一般而言 Multiprogramming Degree 越高，則 CPU utilization 越高
- 多個 process 同時執行，mode 有兩種
    - Concurrent(並行)
    - Parallel(平行)
### Time Sharing System
- 是 Multiprogramming 的一種，在 CPU 排班法則方面，其使用 RR(Round-Robin)法則
    - 即 OS 規定一個 CPU time quantum，若 process 在取得 CPU 後，未能於 quantum 內完成工作，則必須被迫放棄 CPU，等待下一次輪迴。
- 對每個 user 皆是公平的
- 適用在 user interactive 且 response time(反應時間)要求較短的系統環境
- 透過 resource sharing 技術（eg. CPU scheduling、memory sharing、spooling 達到 I/O Device 共享）使得每個 user 皆認為有專屬的系統存在。

####  creating interactive computing
Response time should be < 1 second

Each user has at least one program executing in
memory ⇨ **process**

If several jobs ready to run at the same time ⇨
**CPU scheduling**

If processes don’t fit in memory, **swapping**
moves them in and out to run

**Virtual memory** allows execution of processes
not completely in memory

## Operating-System Operations
