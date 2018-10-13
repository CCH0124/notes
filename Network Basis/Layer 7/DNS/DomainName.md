# What is a Domain Name ?
Domain name 是用於訪問網站的唯一並易於記憶的地址，例如 "google.com" 和 "facebook.com"。借助 DNS 系統，用戶可以使用域名連接到網站。

## What is a domain name ?
Domain name 是一個字串，映射到數字 IP address，用於從客戶端軟體訪問網站，在簡單的英語中，domain name 是用戶在瀏覽器窗口中鍵入以到達特定網站的字串。
網站的實際地址是一個複雜的數字 IP address（例如 103.21.244.0），但由於 DNS 用戶可以輸入人性化的 domain name 並被路由到他們正在尋找的網站，此過程稱為 `DNS lookup`。

## Who manages domain names ?
Domain name 全部由域名註冊管理機構管理，域名註冊機構將域名保留委託給註冊商，任何想要創建網站的人都可以向註冊商註冊域名。

## What’s the difference between a domain name and a URL ?
Uniform resource locato（URL）有時稱為 Web address，包含站點的  domain name 以及其它訊息，包括傳輸協定和路徑。例如，在 URL "http://www.kuas.edu.tw/files/11-1000-64.php" 中，"www.kuas.edu.tw" 是域名，而 "http" 是協定，"/files/11-1000-64.php" 是指向特定頁面的路徑網站。

## Anatomy of a domain name
域名通常分為兩個或三個部分，每個部分用點（.）分隔。從右向左閱讀時，域名中的標識符從最常見到最具體。域名中最後一個點右側的部分是 top-level domain（TLD）。其中包括 "通用" 頂級域名（如".com"、".net" 和 ".org"）以及國家/地區特定頂級域名（如 ".uk"、".tw" 和 ".jp"）。

TLD 左側是 second-level domain（2LD），如果 2LD 左側有任何內容，則稱為 third-level domain（3LD）。我們來看幾個例子：
對於 Google 的美國域名 "google.com"：
- '.com' is the TLD (most general)
- 'google' is the 2LD (most specific)

但對於 Google UK 的域名，"google.co.uk"：
- '.com' is the TLD (most general)
- '.co'* is the 2LD
- 'google' is the 3LD (most specific)
>* 在這種情況下，2LD 表示註冊域名的組織類型（英國的 .co 是公司註冊的站點）

## How to keep a domain name secure
一旦域名註冊到註冊商，該註冊商就會在域名即將到期時通知註冊人，並讓他們有機會續訂，確保他們不會丟失域名。在某些情況下，註冊商會透過在他們過期的第二個域名中購買這些域名然後以高昂的價格將其出售給原始註冊人來捕獲用戶的過期域名。選擇誠實可靠的註冊商來避免這些掠奪性做法非常重要。