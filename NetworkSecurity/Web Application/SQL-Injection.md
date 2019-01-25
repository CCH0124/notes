# SQL Injection
使用 SQL injection，攻擊者可以在受害者的 SQL database 上執行未經授權的 database 命令。
## What is SQL injection (SQi) ?
結構化查詢語言（SQL *）注入是一種代碼注入技術，用於修改或檢索 SQL database 中的數據。透過將專門的 SQL 語句插入到輸入字段中，攻擊者能夠執行允許從 database 中檢索數據，破壞敏感數據或其他操縱行為的命令。

通過正確的 SQL 命令執行，**未經授權的用戶可以欺騙更多特權用戶的身份**，使自己或其他 database 管理員，篡改現有數據，修改事務和金額，以及檢索或銷毀所有服務器數據。

在現代計算中，SQL 注入通常通過將惡意 SQL 查詢發送到網站或服務提供的 API 端點而在 Internet 上發生。在最嚴重的形式中，SQL 注入可以允許攻擊者獲得對計算機的 root 訪問權限。

SQL 是一種用於維護大多數 databases 的編程語言。

## How does a SQL injection attack work ?
想像一下，一個名叫鮑勃的男子正在接受審判，並準備出庭。在審訊前填寫文件時，鮑勃將自己的名字寫成"鮑勃可以自由行"。當法官到達他的案子並大聲朗讀"現在叫鮑勃可以自由行"時，法警讓鮑勃離開，因為法官這樣說。

雖然 SQLi 的種類略有不同，但核心漏洞基本相同：應該為特定類型的數據保留的 SQL 查詢字段（例如：數字）會傳遞意外信息。例如：命令，該命令在運行時會超出預期的範圍，從而允許潛在的邪惡行為，通常從輸入到網頁上的表單中的數據填充查詢字段。

看一下普通和惡意 SQL 語句之間的簡單比較：

##### Normal SQL query
在這個普通的 SQL 查詢中，studentId 字符串被傳遞給 SQL 語句。目標是查看與學生輸入匹配的學生的學生列表，一旦找到，該學生的記錄將被退回。簡而言之，該命令說"去找這個用戶並給我他們的數據"。

###### The code might look something like this：
```mysql=
studentId = getRequestString("studentId");
lookupStudent  = "SELECT * FROM students WHERE studentId = " + studentId
```

如果學生在標有"Please enter your student ID number"的網頁表格中輸入學生證號 117

![](https://www.cloudflare.com/img/learning/security/threats/sql-injection-attack/normal-form-field.png)

###### the resulting SQL query will look like：
```mysql=
SELECT * FROM students WHERE studentId = 117;
```
此命令將返回具有 studentId 的特定學生的記錄，這是編寫 API 的開發人員期望發生的事情。

###### SQL Injection query
在此範例中，攻擊者將輸入 SQL 命令或條件邏輯輸入到輸入字段中，然後輸入學生 ID 號碼：
![](https://www.cloudflare.com/img/learning/security/threats/sql-injection-attack/sql-injection-example-form-field.png)

通常，查詢會在數據庫表中搜索匹配的 ID，現在它會查找 ID 或測試以查看 1 是否等於 1。正如您所料，對於列中的每個學生，該語句始終為 true。因此，database 會將學生表中的所有數據返回給進行查詢的攻擊者。

```mysql=
SELECT * FROM students WHERE studentId = 117 OR 1=1;
```

![](https://www.cloudflare.com/img/learning/security/threats/sql-injection-attack/sql-injection-infographic.png)

SQLi 的工作原理是針對易受攻擊的應用程序編程接口或 API。在這種情況下，API 是服務器通過其接收和響應請求的軟體接口。

現實存在常用的工具，允許惡意行為者自動搜索尋找表單的網站，然後嘗試輸入各種 SQL 查詢，這些查詢可能會生成網站軟體開發人員為了利用數據庫而不打算做出的響應。

SQL injections 很容易實現，有趣的是，在適當的開發實踐中也很容易防止。 現實更加模糊，因為緊迫的期限，缺乏經驗的開發人員和遺留代碼通常會導致可變的代碼質量和安全實踐。在可訪問 database 的網站上的任何表單或 API 端點上的單個易受攻擊的字段可能足以暴露漏洞。

## How is a SQL Injection attack prevented ?
有許多方法可以降低因 SQL injections 而導致數據洩露的風險。作為最佳實踐，應該使用幾種策略。

以下探索一些更常見的實現：

##### Use of Prepared Statements (with Parameterized Queries)
這種清理 database 輸入的方法包括強制開發人員首先定義所有 SQL 代碼，然後只將特定參數傳遞給 SQL 查詢，輸入的數據明確地給出了有限的範圍，它無法擴展。這允許 database 區分正在輸入的數據和要運行的代碼，而不管輸入字段中提供的數據類型如何。一些對象關係映射（ORM）庫通常用於此目的，因為某些版本將自動清理數據庫輸入。

##### Escape All User Supplied Input
編寫 SQL 時，特定字符或單詞具有特定含義。例如，'*'字符表示 "any"，單詞"OR" 表示條件。為了規避將這些字符意外或惡意輸入到 database 的 API 請求中的用戶，可以轉義用戶提供的輸入。轉義字符是告訴 database 不要將其解析為命令或條件而是將其視為文字輸入的方式。

##### Use of Stored Procedures 
雖然儲存過程本身不是一個強大的安全策略，但可以幫助限制與 SQL injection 相關的風險。透過適當地限制運行 SQL 查詢的 database 帳戶的權限，即使是易受 SQL 注入攻擊的非健壯的應用程序代碼也將缺少操作不相關的數據庫表所需的權限。儲存過程還可以檢查輸入參數的類型，防止輸入違反字段設計接收類型的數據。在靜態查詢不足的情況下，通常要避免存儲過程。

##### Enforce Least Privilege
作為一般規則，在網站需要使用動態 SQL 的所有情況下，透過將權限限制為執行相關查詢所需的最窄範圍來減少 SQL injection 的風險非常重要。在最明顯的形式中，這意味著管理帳戶在任何情況下都不應該由於未經授權的請求的 API 調用而執行 SQL 命令。**雖然存儲過程最適合用於靜態查詢，但實施最小權限有助於降低動態 SQL 查詢的風險**。

## What is a compound SQL injection attack ?
為了規避安全措施，聰明的攻擊者有時會針對目標網站實施多向量攻擊。雖然可以減輕單一攻擊，但它也可能成為數據庫管理員和訊息安全團隊關注的焦點。`DDoS攻擊`、`DNS 劫持`和其他中斷方法有時被用作實施全面 `SQL 注入`攻擊的分心。因此，全面的威脅緩解策略可提供最廣泛的保護。