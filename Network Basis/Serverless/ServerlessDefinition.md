# Serverless Definition
`serverless` 計算是一種在使用的基礎上提供後端服務的方法。仍然使用服務器，但是從 `serverless` 供應商獲得後端服務的公司根據使用情況收費，而不是根據固定數量的 bandwidth 或服務器數量收費。

## What is serverless computing ?
`serverless` 計算是一種在使用的基礎上提供後端服務的方法。**serverless 提供程序允許用戶編寫和部署代碼，而無需擔心底層基礎架構**，從 `serverless` 供應商獲得後端服務的公司根據其計算收費，並且不必為固定數量的 bandwidth 或服務器數量預留和支付，因為**該服務是自動擴展**的。

>雖然稱為 serverless ，但仍然使用物理服務器，但開發人員不需要了解它們。

在網路發展初期，任何想要構建 Web 應用程序的人都必須擁有運行服務器所需的物理硬體，這是一項繁瑣且昂貴的任務。

然後是 `cloud`，可以遠端租用固定數量的服務器或服務器空間。租用這些固定服務器空間的開發商和公司通常會過度(經由提供商)購買，以確保流量或活動的高峰不會超過其一個月限制並破壞其應用程序。這意味著付出的大部分服務器空間通常都會浪費掉，雲供應商已經引入了自動擴展模型來解決這個問題，但即使使用自動擴展，不必要的活動峰值（例如：DDoS 攻擊）也可能會非常昂貴。

![](https://www.cloudflare.com/img/learning/serverless/what-is-serverless/benefits-of-serverless.svg)

`serverless` 計算允許開發人員以靈活的"即用即付"方式購買後端服務，這意味著開發人員只需為他們使用的服務付費。這就像從具有每月固定限制的手機數據計劃切換到僅對實際使用的每個數據流量收費的計劃。

"serverless"一詞有些誤導，因為仍有服務器提供這些後端服務，但所有服務器空間和基礎架構問題都由供應商處理。serverless 意味著開發人員可以完成工作而無需擔心服務器。

## What are backend services ? What’s the difference between frontend and backend ?

應用程序開發通常分為兩個領域：frontend 和 backend。
##### frontend
frontend 是用戶查看和交互的應用程序的一部分，例如可視佈局。 
##### backend
backend 是用戶看不到的部分。這包括應用程式文件所在的服務器以及持久保存用戶數據和業務邏輯的數據庫。

![](https://www.cloudflare.com/img/learning/serverless/what-is-serverless/frontend-vs-backend.svg)

## Example
讓我們想像一個銷售音樂會門票的網站。
1. 當用戶在瀏覽器窗口中鍵入**請求**時，瀏覽器向**後端服務器發送請求**，後端服務器回應網站數據。
2. 然後，用戶將看到網站的前端，其中包括用戶填寫的文字、圖像和表單字段。
3. 然後，用戶可以與前端上的一個表單字段交互以搜索他們喜歡的音樂表演。
4. 當用戶點擊"提交"時，這將**觸發對後端的另一個請求**。
5. 後端代碼檢查其數據庫以查看是否存在具有此名稱的執行者，如果存在，則何時將播放下一個，以及可用的票數。
6. 然後，後端將該數據傳遞回前端，並且前端將以對用戶有意義的方式顯示結果。
7. 類似的，當用戶創建帳號並輸入財務訊息以購買門票時，將發生前端和後端之間的另一次來回通訊。

## What kind of backend services can serverless computing provide ?

大多數 `serverless` 提供商為其客戶提供數據庫和儲存服務，而且許多提供商還擁有`Function-as-a-Service （FaaS）` 平台，如：Cloudflare Workers，這些平台可以在邊緣上執行代碼片段而不存儲任何數據。

## What are the advantages of serverless computing ?
##### Lower costs 
`Serverless` 計算通常非常具有成本效益，因為後端服務（服務器分配）的傳統雲提供商通常會導致用戶為未使用的空間或空閒 CPU 時間付費。
##### Simplified scalability
使用 `serverless` 架構的開發人員**不必擔心擴展其代碼的策略**。`Serverless` 供應商按需求處理所有擴展。
##### Simplified backend code
使用 `FaaS`，開發人員可以創建獨立執行單一目的的簡單函數。例如：進行 `API` 調用。
##### Quicker turnaround
`Serverless` 架構可以顯著縮短產品上市時間。開發人員可以逐個添加和修改代碼，而不需要復雜的部署過程來推出錯誤修復和新功能。
