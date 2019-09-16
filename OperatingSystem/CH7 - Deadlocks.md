## System Model(系統模型)
- 出於死鎖(deadlock)討論的目的，可以將系統模型為有限資源的集合，其可以被劃分為不同的類別，以分配給多個 process，每個進程具有不同的需求
  - 資源類別可能包括 memory、printer、CPU、open files、CD-ROM 等
- 根據定義，類別中的所有資源都是等效的，並且該類別中的任何一個資源都可以同等滿足此類別的請求。如果不是這種情況（即，如果類別中的資源之間存在某些差異），則需要將該類別進一步劃分為單獨的類別
  - 例如，printer 可能需要分成 laser printers 和 color inkjet printers。
  - 某些類別可能只有一個資源
- 在正常操作中，process 必須在使用之前**請求資源**，並在完成後按以下順序釋放它：
  - Request
    - 如果無法立即授予請求，則該 process 必須等待它所需的資源可用。例如，系統調用 `open()`、`malloc()`、`new()` 和 `request()`
  - Use
    - 該過程是使用資源，例如資料印於印表機或從文件讀取
  - Release 
    - 該過程釋放了資源。以便可用於其他 process。例如，`close()`、`free()`、`delete()` 和 `release()`
- 所有內核管理的資源，內核會追蹤哪些資源是空閒的，哪些資源是分配的，它們分配到哪個 process，以及等待此資源可用的 process queue。可以使用互斥鎖或`wait()` 和 `signal()` 調用（即二進製或計數訊號量）來控制應用程序管理的資源。
- 當集合中的每個 process 都在等待當前分配給集合中另一個 process 的資源時，一組 process 就會 deadlock（並且只有在其他等待 process 取得進展時才能釋放）

## Deadlock Characterization(死結的特性)

![](https://i.imgur.com/hulFGUG.jpg)

### Necessary Conditions(必要條件)

- 實現 deadlock 需要四個條件：
  - Mutual Exclusion(互斥)
    - 必須以非共享模式保存至少一個資源;如果任何其他 process 請求此資源，則該 process 必須等待釋放資源
  - Hold and Wait(占用與等待)
    - process 必須同時保存至少一個資源並等待某個其他 process 當前持有的至少一個資源
  - No preemption(不可搶先)
    - 一旦 process 持有資源（即一旦其請求被授予），則該進程自動釋放之前不能從該 process 中取出該資源
  - Circular Wait(循環式等待)
    - 一組 process {P_0, P_1, P_2 , ..., P_n} 必須存在，使得每個 P[i] 都在等待 P[(i + 1)%(N + 1)]。（注意，這個條件意味著保持和等待條件，但如果分別考慮這四個條件，則更容易處理條件。）
    
### Resource-Allocation Graph(資源配置圖)
- deadlock 可用資源配置圖（Resource-Allocation Graphs）的有向圖說明
  - 一組資源類別，{R1, R2, R3,..., Rm}，在圖表上顯示為方形節點。資源節點內的點指示資源的特定實例。（例如，兩個點可能代表兩個雷射印表機。）
  - 一組 process，{ P1, P2, P3,..., Pn}
  - Request Edges
    - 從 Pi 到 Rj 的一組有向弧，表示 process Pi 已請求 Rj，並且當前正在等待該資源變為可用
  - Assignment Edges 
    - 從 Rj 到 Pi 的一組有向弧，表示資源 Rj 已被分配用於 process Pi，並且 Pi 當前正在持有資源 Rj

![](https://i.imgur.com/xavaJnu.png "Example of a Resource Allocation Graph")

- 如果資源分配圖不包含循環，則系統不會有 deadlock(上圖)
- 如果資源分配圖確實包含循環，並且每個資源類別僅包含單個實例，則存在 deadlock
- 如果資源類別包含多個實例，則資源分配圖中存在循環表示存在 deadlock 的可能性，但不保證 deadlock。考慮下圖

![](https://i.imgur.com/I0yYqmx.png "Resource Allocation Graph With A Deadlock")
![](https://i.imgur.com/OAyHhnl.png "Graph With A Cycle But No Deadlock")

## Methods for Handling Deadlocks(處裡死結的方法)
- 一般來說，有三種處理 deadlock 的方法：
1. 防止 deadlock 或避免 - 不要讓系統進入死鎖狀態
2. deadlock 檢測和恢復 - 檢測到 deadlock 時中止 process 或搶占某些資源
3. 忽略問題 - 如果 deadlock 僅在一年左右發生一次，那麼簡單讓它們發生並在必要時重新引導可能比引起與死鎖預防或檢測相關的持續開銷和系統性能損失更好。 這是 Windows 和 UNIX 採用的方法

## 7.4 Deadlock Prevention(預防死結)
藉由防止以下四個必要條件中的至少一個來防止 deadlock：

### 7.4.1 Mutual Exclusion(互斥)
- 至少有一項資源必須是不可用的
- 像是唯讀檔案之類的共享資源不會導致死結
  - 某些資源（如印表機和磁帶驅動器）需要透過單個 process 進行單一訪問
  
### 7.4.2 Hold and Wait(占用與等候)
- 必須保證一個 process 再要求一項資源時，不可以占用任何其他的資源
- 為了防止這種情況，必須防止 process 持有一個或多個資源，同時等待一個或多個其他資源。有幾種可能性：
  - 要求所有 process 一次請求所有資源。如果一個 process 在執行先前需要一個資源並且在很久之後不需要其他資源，這可能會浪費系統資源
  - 要求持有資源的 process 必須在請求新資源之前釋放它們，然後在單個新請求中重新獲取已釋放的資源以及新資源。如果 process 已使用資源部分完成操作，然後在釋放後無法重新分配，則可能會出現問題。
  - 如果 process 需要一個或多個普遍資源，則上述任何一種方法都可能導致飢餓(starvation)

### 7.4.3 No Preemption(不可搶先)
