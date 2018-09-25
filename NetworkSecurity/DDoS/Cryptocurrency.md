# Cryptocurrency DDoS Attacks
硬幣交換的目標是 DDoS 攻擊，試圖破壞服務。

## What is a cryptocurrency ?
cryptocurrency 是一種數字貨幣形式，它有兩個與傳統貨幣區別開來的關鍵差異。
- cryptocurrency 不受任何政府的約束或支持，使其成為真正的國際貨幣形式
- cryptocurrency  總是分散的

## What is the difference between centralized and decentralized currency ?
傳統的資金是集中的，如果擁有美元的人將錢存入實體銀行，該銀行將負責跟蹤該貨幣。這筆錢的擁有者將不得不回到那家銀行取錢。同樣，如果該人想將他們的美元存入網上銀行，該線上銀行將記錄該中央服務器中該人的賬戶中的金額，該銀行的中央服務器是該人帳戶的權威訊息來源。在這兩種情況下，貨幣都是集中的，這意味著它可以在一個地方追蹤。

使用 cryptocurrency，每個投資者擁有的貨幣數量訊息存儲在許多（通常是數千個）不同的計算機上，稱為節點。這些節點上的訊息可公開訪問，並且沒有單一的中央機構負責追蹤這些帳戶，這意味著無論何時發生事務，都必須同時更新每個節點，以確保所有節點中的 ledgers 保持同步。

所有這些節點保持同步是非常重要的，因為否則在所有分類賬有機會更新之前，有人花兩倍錢的可能性會使貨幣無效這被稱為**雙重支出**。 cryptocurrency 的雙重支出被叫 blockchain 的過程阻止了。

## What is blockchain ?
Blockchain 是使 cryptocurrency 成為可能的技術突破。區塊鏈的完整技術解釋超出了本文的範圍，但重要的是區塊鏈需要挖掘。

## What is cryptocurrency mining ?
Cryptocurrency 的挖掘是計算機花費時間解決高度複雜的數學問題的過程，一旦該問題得到解決，一組 cryptocurrency 交易就會被添加到類似的交易集合的隊列中，這些交易將被廣播到所有節點，更新他們的 ledgers。為了鼓勵採礦，解決這些複雜數學問題的計算機會因為他們正在挖掘的貨幣的新鑄幣而獲得獎勵。這就是新加密貨幣流通的方式。

## Why are cryptocurrencies being DDoSed ?
與大多數高能見度業務一樣，硬幣交換已成為 DDoS 攻擊的目標。隨著興趣的激增以及由此產生的加密貨幣流量的增加，為不良行為者打開了大門，企圖破壞加密貨幣資源，拒絕加密客戶用戶訪問。

一個 `distributed denial-of-servic`（DDoS）攻擊是中斷的，在現代 Internet 的主要方法之一。透過假流量超載目標，壞人可以使網站或服務不可用。加密貨幣的普及和重要性使它們成為攻擊的主要目標。

這些攻擊擊中了我們網路上的許多硬幣交換，以便衡量任何可識別的感興趣模式。最顯著的 DDoS 流​​量來自 `SSDP amplification`、`NTP amplification`和應用層攻擊。

一個流行的硬幣兌換服務已被標記為大約一年的 76 個應用程序層 DDoS 攻擊，但值得注意的是，令人難以置信的流量激增可能會產生誤報，正常流量可能會出現一些攻擊跡象。無論如何，比特幣交易已經成為 DDoS 的主要目標。

這是一張圖表，顯示截至2017年12月中旬針對流行的加密貨幣網站屬性的潛在應用層攻擊數量。

![](https://www.cloudflare.com/img/learning/ddos/cryptocurrency-ddos-attacks/cryptocurrency-ddos-graph.png)

特別令人感興趣的是11月11日左右的大規模襲擊事件。在此期間，許多 blockchain 貨幣提供商網站似乎已被定位。

即使在最好的情況下，許多與比特幣和其他加密貨幣相關的網站和應用程序也沒有足夠的資源來應對大量的流量激增。這種增加可能發生在 DDoS 攻擊期間或高水平的正常活動期間，導致臨時中斷和 denial-of-servic。託管 CDN 上的內容對於保持站點在線是必不可少的，儘管它也可能需要一個適當  `load balanced` 的服務器網路，以便能夠處理浪湧可以產生的數據庫請求的數量。