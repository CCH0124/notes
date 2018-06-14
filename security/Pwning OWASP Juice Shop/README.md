### Running OWASP Juice Shop
- Docker image
  1. 在你電腦上安裝 Docker
  2. 在 command line 執行 docker pull bkimminich/juice-shop 下載 latest image.
  3. Docker 跑 run -d -p 3000:3000 bkimminich/juice-shop.
  4. Browse to http://localhost:3000.
### Challenge hunting
- Finding the Score Board

  **Challenge** | **Difficulty** |
  --- | --- |
  Find the carefully hidden 'Score Board' page. | :star: |
  
  - Security by obscurity
  > 我把錢鎖在一個安全性未知的保險箱裡，但這個保險箱藏在隱匿的地方或者你家的前門被灌木和樹木覆蓋著。
  > 這就是 security by obscurity 的安全基礎。
  
  **Find the carefully hidden 'Score Board' page**
  
  ![image](https://github.com/CCH0124/OWASP-Practice/blob/master/Pwning%20OWASP%20Juice%20Shop/image/1.png "Image")
  
  - Ref
    - [security by obscurity](https://www.wikiwand.com/en/Security_through_obscurity "security by obscurity")
    
    - [Security Through Obscurity](https://www.lifewire.com/security-through-obscurity-2486707 "security by obscurity")
    
    - [security through obscurity](http://users.softlab.ntua.gr/~taver/security/secur3.html "security by obscurity")
    
### Injection

  **Challenge** |	**Difficulty** |
  --- | --- |
  Order the Christmas special offer of 2014. | :star::star: |
  Log in with the administrator's user account. | :star::star: |
  Log in with Bender's user account. | :star::star::star: |
  Log in with Jim's user account. | :star::star::star: |
  Let the server sleep for some time. (It has done more than enough hard work for you) | :star::star::star::star: |
  Update multiple product reviews at the same time. | :star::star::star::star: |
  Retrieve a list of all user credentials via SQL Injection | :star::star::star::star: |
  
  **Order the Christmas special offer of 2014**
  
  - SQL Injection 
  > 隱碼攻擊，攻擊者可以欺騙 Web 應用程式，將惡意查詢轉發給資料庫，當應用程式設計不夠良好，未對
  > 取自查詢欄位的資料進行檢查，則當伺服器執行 SQL 指令時便會造成資料庫的內容遭到破壞或竊取。
  
  - 如何證明其存在
  > 破壞底層 SQL query 的 payload （例如 ' 或 ';）
  
  **Log in with the administrator's user account**
  
  ![Injectoin](https://github.com/CCH0124/OWASP-Practice/blob/master/Pwning%20OWASP%20Juice%20Shop/image/2.png "Injectoin")
  
  1. 輸入 Email ：
  
  ```mysql
  ' or 1=1 --
  ```
  2. 輸入 Password：任意  
  3. 即可以最高權限登入
  
  **Order the Christmas special offer of 2014**
  
  ![Injection](https://github.com/CCH0124/OWASP-Practice/blob/master/Pwning%20OWASP%20Juice%20Shop/image/3-1.png "Injection")
  1. 注入 
  
  ```mysql 
  ' or 1=1 /* 
  ```
  
  2. 發現開發人員 consle 出現 500 Error
  ![Injection](https://github.com/CCH0124/OWASP-Practice/blob/master/Pwning%20OWASP%20Juice%20Shop/image/4.png "Injection")
  
  其中 
  
  ```mysql
  SELECT * FROM Products WHERE ((name LIKE '%' or 1=1 /*%' OR description LIKE '%' or 1=1 /*%') AND deletedAt IS NULL) ORDER BY name
  deletedAt IS NULL 
  ```
  應該是 Database 必須執行某種 soft delete
  在 search 中下這一段 ***Christmas%25'))*** 會變成
  
  ```mysql
  SELECT * FROM Products WHERE ((name LIKE '%Christmas%'))%' OR description LIKE '%Christmas%'))%') AND deletedAt IS NULL) ORDER BY name
  ```
  後頭再加上 `/*` 將後半部語法註解，接著就可以結帳去...
  
  ![Injection](https://github.com/CCH0124/OWASP-Practice/blob/master/Pwning%20OWASP%20Juice%20Shop/image/4-1.png "Injection")
  ![Injection](https://github.com/CCH0124/OWASP-Practice/blob/master/Pwning%20OWASP%20Juice%20Shop/image/4-2.png "Injection")

  **Log in with Bender's user account**
  Login 的 Email 輸入 
  ```mysql
  ' or 1=1 AND email LIKE '%Bender%' /*
  ```
  **Log in with Jim's user account**
  Login 的 Email 輸入 
  ```mysql
  ' or 1=1 AND email LIKE '%Jim%' /*
  ```
  **Let the server sleep for some time**
  
  **Update multiple product reviews at the same time**
  
  **Retrieve a list of all user credentials via SQL Injection**
  
  **為完待續~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~**
 
  **REF**
  - [Injection Flaws](https://www.owasp.org/index.php/Injection_Flaws)
  - [（wiki）SQL Injection](https://www.wikiwand.com/en/SQL_injection)
  - [（IT）SQL Injection](https://ithelp.ithome.com.tw/articles/10195076)
  - [Testing for NoSQL injection](https://www.owasp.org/index.php/Testing_for_NoSQL_injection)
  
### Broken Authentication

  **Challenge** | **Difficulty** |
  --- | --- |
  Log in with the administrator's user credentials without previously changing them or applying SQL Injection. | :star::star: |
  Reset Jim's password via the Forgot Password mechanism with the truthful answer to his security question. | :star::star::star: |
  Change Bender's password into slurmCl4ssic without using SQL Injection. | :star::star::star::star: |
  Log in with Bjoern's user account without previously changing his password, applying SQL Injection, or hacking his Google account. |  :star::star::star::star: |
  Reset Bender's password via the Forgot Password mechanism with the truthful answer to his security question. |  :star::star::star::star: |
  Exploit OAuth 2.0 to log in with the Chief Information Security Officer's user account. | :star::star::star::star::star: |
  Reset Bjoern's password via the Forgot Password mechanism with the truthful answer to his security question. |  :star::star::star::star::star: |
  
  **Log in with the administrator's user credentials without previously changing them or applying SQL Injection**
  1. 用 `Log in with the administrator's user account` 方式登入，接著 F12 選擇 Network 面板，畫面重整（下圖）。
  
  ![Authentication](https://github.com/CCH0124/OWASP-Practice/blob/master/Pwning%20OWASP%20Juice%20Shop/image/5.png "Authentication")

  到此 [jwt](https://jwt.io/) 將 token 貼上去會自動解析轉 json 格式。
  
 > JWT的由下面三個部分組成，每一個部分用點號 . 分開。 
 > 1. 標頭 Header   
 > 2. 酬載 Payload  
 > 3. 簽名 Signature  

  ![json](https://github.com/CCH0124/OWASP-Practice/blob/master/Pwning%20OWASP%20Juice%20Shop/image/6.png "json")
  
  2. 將 password 解密，最後可得知
    - Email： `admin@juice-sh.op`
    - Password：`admin123`
    
  **Reset Jim's password via the Forgot Password mechanism with the truthful answer to his security question.**
  
> 許多網站註冊使用密碼取回/重置和登錄驗證的安全問題。有些用戶在打電話時也會提出相同的安全問題。
> 安全問題是驗證用戶並阻止未經授權的訪問的一種方法。但是安全問題存在問題。網站可能使用安全性差的
> 問題，可能會產生負面結果。用戶無法準確記住答案或答案改變，這個問題對用戶不起作用，問題不安全，
> 可能被他人發現或猜測。我們使用很好的問題是至關重要的。良好的安全問題符合五個標準。

>>一個好的安全問題的答案是：

>>安全：不能被猜測或研究

>>穩定：不隨時間變化

>>令人難忘：可以記住

>>簡單：精確、簡單、一致

>>很多：有很多可能的答案

> 很難找到滿足所有五個標準的問題，這意味著一些問題很好，一些公平，而且大多數都很差。實際上，如果
> 有任何良好的安全問題，很少。人們在社交媒體，博客和網站上分享如此多的個人訊息，很難找到符合上述
> 標準的問題。另外，很多問題不適用於某些人；例如：你最大的孩子的暱稱是什麼 - 但你沒有孩子。
  
  1. 由 Log in with Jim's user account 得知，Jim email 為 jim@juice-sh.op
  2. James 並閱讀描述部分
  3. 發現，Jim 有一個叫  George Samuel Kirk 的兄弟
  4. Your eldest siblings middle name ? 就是 samuel
  5. 最後 Change 即可

  ![Authentication](https://github.com/CCH0124/OWASP-Practice/blob/master/Pwning%20OWASP%20Juice%20Shop/image/7.png "Authentication")
  
  **Change Bender's password into slurmCl4ssic without using SQL Injection**
  
  **Log in with Bjoern's user account without previously changing his password, applying SQL Injection, or hacking his Google account**
  
  **Reset Bender's password via the Forgot Password mechanism with the truthful answer to his security question**
  
  **Exploit OAuth 2.0 to log in with the Chief Information Security Officer's user account**
  
  **Reset Bjoern's password via the Forgot Password mechanism with the truthful answer to his security question**
  
  **為完待續~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~**
  
  ### Forgotten content
  
  **Challenge** | **Difficulty** |
  --- | --- |
Let us redirect you to a donation site that went out of business. | :star: |
Use a deprecated B2B interface that was not properly shut down. | :star::star: |
Travel back in time to the golden era of web design. | :star::star::star::star: |
Retrieve the language file that never made it into production. | :star::star::star::star::star: |
Deprive the shop of earnings by downloading the blueprint for one of its products. | :star::star::star::star::star: |

  **Let us redirect you to a donation site that went out of business**
  
  ![Forgotten content](https://github.com/CCH0124/OWASP-Practice/blob/master/Pwning%20OWASP%20Juice%20Shop/image/Let%20us%20redirect%20you%20to%20a%20donation%20site%20that%20went%20out%20of%20business..png "Forgotten content")
  
1. 發現有一個註解代碼，如下
    
  <pre>
    <a href="/redirect?to=https://gratipay.com/juice-shop\" target="_blank" class="btn btn-danger">
                        <i class="fab fa-gratipay fa-lg"></i> Gratipay
                    </a>
  </pre>
    
2. 接著將網址如下導向
    
    `ip:port/redirect?to=https://gratipay.com/juice-shop`
    
3. 導向至一個 404 的頁面
    
    `https://gratipay.com/juice-shop`
    
 **Use a deprecated B2B interface that was not properly shut down**
 
 1. 我已 `admin` 登入
 2. 選 `Complain?`
 3. 點選開發人員查看程式碼要點如下
    <pre> 
      ngf-pattern="'.pdf,.xml'" # 只允許 pdf xml 檔
      ngf-accept="'.pdf'" # 接受 pdf
      ngf-max-size="100KB" # 大小 100 KB
    </pre>
 4. 點擊`選擇檔案`。它默認只過濾 PDF
 5. 隨便上傳一個小於 100 KB 的 xml 即可
 6. 在 console 會發現 `410 (Gone)` HTTP 錯誤
 
 ![B2B](https://github.com/CCH0124/OWASP-Practice/blob/master/Pwning%20OWASP%20Juice%20Shop/image/Use%20a%20deprecated%20B2B%20interface%20that%20was%20not%20properly%20shut%20down.png "B2B")
 
 **Travel back in time to the golden era of web design**
 
 **Retrieve the language file that never made it into production**
 
 **Deprive the shop of earnings by downloading the blueprint for one of its products**
 
 ### Sensitive Data Exposure
 
 Challenge | Difficulty |
 --- | --- |
Access a confidential document. | :star: |
Log in with MC SafeSearch's original user credentials without applying SQL Injection or any other bypass. | :star::star: |
Inform the shop about an algorithm or library it should definitely not use the way it does. | :star::star: |
Forge a coupon code that gives you a discount of at least 80%. | :star::star::star::star::star::star: |
Solve challenge #99. Unfortunately, this challenge does not exist. | :star::star::star::star::star::star: |
Unlock Premium Challenge to access exclusive content. | :star::star::star::star::star::star: |

**Access a confidential document**

1. 點選 `About Us` 選單
2. 有個 `Check out our boring terms of use if you are interested in such lame stuff.` 的超鏈結
3. 右鍵 `複製鏈接網址` 
4. 網址 `http://IP:port/ftp/legal.md?md_debug=true`
5. 將它 `http://IP:port/ftp` 發現 FTP 安全設置沒設定，出現多個資料。如下圖
![Access a confidential document](https://github.com/CCH0124/OWASP-Practice/blob/master/Pwning%20OWASP%20Juice%20Shop/image/Access%20a%20confidential%20document.png "Access a confidential document")

**Log in with MC SafeSearch's original user credentials**

1. 聽這首 [Protect Ya' Passwordz](https://www.youtube.com/watch?v=v59CX2DiX0Y)，並了解歌詞
2. 有一段 `my dog mr.noodles`
3. 把 o 換成 0
4. login 測試
  - Email `mc.safesearch@juice-sh.op`
  - Password `Mr. N00dles`

**Inform the shop about an algorithm or library it should definitely not use the way it does**

**Forge a coupon code that gives you a discount of at least 80%**

**Solve challenge #99. Unfortunately, this challenge does not exist**

**Unlock Premium Challenge to access exclusive content**

