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
