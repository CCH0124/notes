#　BGP
Border Gateway Protocol（BGP）是 Internet 的 routing protocol。與郵局處理郵件非常相似，BGP 選擇最有效的路由來提供 Internet 流量。

## What is BGP ?
Border Gateway Protocol（BGP）是 Internet 的郵政服務。當有人將信件放入郵箱時，郵政服務處理該郵件並選擇快速有效的路徑將該信件發送給其收件人。同樣，當有人通過 Internet 提交數據時，BGP 負責查看數據可以傳輸的所有可用路徑並選擇最佳路由，這通常在自治系統之間跳轉。

BGP 是使 Internet 工作的協定。它透過在 Internet 上啟用數據路由來實現此目的。當新加坡的用戶在阿根廷加載具有`來源服務器`的網站時，BGP 是使該通訊快速有效地發生的協議。

## What is an autonomous system ?
Internet 是一個網路中的網路，它被分解成數十萬個稱為 autonomous systems（AS）的小型網絡。這些網路中的每一個本質上都是由單個組織運行的大型路由器池。

![](https://www.cloudflare.com/img/learning/security/glossary/what-is-bgp/network-of-networks.svg)

如果我們繼續將 BGP 視為 Internet 的郵政服務，AS 就像個人郵局分支機構一樣。一個城鎮可能有數百個郵箱，但這些郵箱中的郵件必須通過本地郵政分支才能被路由到另一個目的地。AS 中的內部路由器就像郵箱一樣，它們將出站傳輸轉發給 AS，AS 然後使用 BGP 路由將這些傳輸傳輸到目的地。

![](https://www.cloudflare.com/img/learning/security/glossary/what-is-bgp/bgp-simplified.svg)

上圖說明了 BGP 的簡化版本。在這個版本中，Internet 上只有 6 個自治系統。如果 AS1 需要將數據包路由到 AS3，它有兩個不同的選項：

Hopping to AS2 and then to AS3:
AS2 → AS3

Or hopping to AS6, then to AS5, AS4, and finally to AS3:
AS6 → AS5 → AS4 → AS3

在這個簡化的模型中，決定似乎很簡單。AS2 路由比 AS6 路由需要更少的跳數（路徑），因此它是最快，最有效的路由。現在假設有數十萬個 AS，並且跳數只是複雜路由選擇算法的一部分。這就是 Internet 上 BGP 路由的現實。

Internet 的結構不斷變化，新的系統突然出現，現有的系統變得不可用。因此，每個 AS 必須及時更新有關新路線和過時路線的訊息，這是透過對等會話來完成的，其中每個 AS 透過 `TCP/IP` 連接連接到相鄰的 AS，以便共享路由訊息。使用此訊息，每個 AS 都可以正確路由來自內部的出站數據傳輸。

這是我們類比的一部分分崩離析：與郵局分支不同，**自治系統並非都屬於同一個組織**。因此，他們沒有理由相互友好，往往是商業競爭對手！因此，BGP 路由有時會考慮業務因素。自治系統通常相互收費以在其網絡上傳輸流量，並且可以將訪問的價格考慮在最終選擇哪條路由中。

## Who operates BGP autonomous systems ?
自治系統通常屬於 ISP 或其他大型高科技組織，如：科技公司、大學、政府機構和科研機構。希望交換路由訊息的每個自治系統必須具有註冊的自治系統號（ASN）。互聯網號碼分配機構（IANA）將 ASN 分配給地區互聯網註冊機構（RIR），然後將其分配給 ISP 和網路。ASN 是 1 到 65534 之間的 16 位數字和 131072 到 4294967294 之間的 32 位數字。截至2018年，全球大約有 64,000 個 ASN 在使用中。這些 ASN 僅對外部 BGP 是必需的。

## What’s the difference between external BGP and internal BGP ?
交換路由，並使用 `external BGP` 或 `eBGP` 透過 Internet 傳輸流量。自治系統還可以使用內部版本的 BGP 來路由其內部網路，這稱為 `internal BGP`，簡稱 `iBGP`。應該注意，使用 `internal BGP` 不是使用 `external BGP` 的要求。自治系統可以從許多內部協定中進行選擇，以連接其內部網路上的路由器。

`External BGP` 就像國際航運，在國際上發送郵件時需要遵循某些標準和準則。一旦該郵件到達其目的地國家，它必須通過目的地國家的本地郵件服務才能到達其最終目的地。每個國家/地區都有自己的內部郵件服務，不一定遵循與其他國家/地區相同的指導原則。類似地，每個自治系統可以具有其自己的內部路由協議，用於在其自己的網絡內路由數據。

## How BGP can break the Internet
2004 年，一家名為 TTNet 的土耳其互聯網服務提供商（ISP）意外向其鄰居發布了錯誤的 BGP 路由。這些路線聲稱 TTNet 本身是互聯網上所有流量的最佳目的地。隨著這些路線越來越多地擴展到更自治的系統，發生了大規模的中斷，造成了為期一天的危機，世界上許多人無法訪問部分或全部互聯網。

同樣，2008 年，巴基斯坦 ISP 嘗試使用 BGP 路由阻止巴基斯坦用戶訪問 YouTube。然後 ISP 意外地將這些路由與其相鄰的 AS 進行通告，並且該路由快速傳播到 internet 的 BGP 網路。這條路線將嘗試訪問 YouTube 的用戶發送到了死胡同，導致 YouTube 幾個小時無法訪問。

這些是稱為 `BGP hijacking` 的實踐示例，並不總是偶然的。在 2018 年 4 月，攻擊者故意創建錯誤的 BGP 路由來重定向用於亞馬遜 DNS 服務的流量。通過將此流量重定向到自己，攻擊者能夠竊取價值超過 10 萬美元的加密貨幣。

這樣的事件可能發生，因為 BGP 的路由共享功能依賴於信任，而自治系統隱含地信任與它們共享的路由。儘管有許多雄心勃勃的提案旨在使 BGP 更加安全，但這些提案很難實現，因為它們需要每個自治系統同時更新其行為。由於這需要數十萬個組織的協調，並可能導致整個互聯網的臨時刪除，因此這些主要提案似乎不太可能很快實施。