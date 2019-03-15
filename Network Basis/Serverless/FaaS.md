# Function as a Service (FaaS)
FaaS 是一種無服務器後端服務，允許開發人員動態編寫模塊化代碼片段，以響應某些事件而執行。
## What is function as a service ?
Function as a service (FaaS) 在邊緣上執行模組化代碼片段的無服務器方式。FaaS 允許開發人員動態編寫和更新一段代碼，然後可以響應事件執行，例如：用戶點擊 Web 應用程序中的元素。這使得擴展代碼變得容易，並且是實現微服務（microservices）的經濟高效的方式。

## What are microservices ?
如果一個 Web 應用程序是一個視覺藝術作品，使用微服務架構就像用馬賽克瓷磚集合製作藝術品。藝術家可以一次輕鬆添加，替換和修復一個圖塊。整體式架構就像在一塊畫布上繪製整個作品一樣。

![](https://www.cloudflare.com/img/learning/serverless/glossary/function-as-a-service-faas/monolithic-application-microservice-faas.svg)

**這種從一組模塊化組件構建應用程序的方法稱為微服務架構**。將應用程序劃分為微服務對開發人員很有吸引力，因為這意味著他們可以創建和修改可以輕鬆實現到其代碼庫中的小塊代碼。這與單片架構形成對比，在整體架構中，所有代碼都交織在一個大型系統中。對於大型單片系統，即使對應用程序進行微小更改也需要大量的部署過程，**FaaS 消除了這種部署的複雜性**。

使用像 FaaS 這樣的無服務器代碼，Web 開發人員可以專注於編寫應用程序代碼，而 `serverless` 提供商負責服務器分配和後端服務。

## What are the advantages of using FaaS ?
##### Improved developer velocity
使用 FaaS，開發人員可以花更多時間編寫應用程序邏輯，**而不必擔心服務器和部署**。這通常意味著更快的開發周轉時間。
##### Built-in scalability
由於 FaaS 代碼具有固有的可擴展性，因此開發人員不必擔心為高流量或大量使用創建突發事件。`serverless` 提供程序將處理所有擴展問題。
##### Cost efficiency
與傳統雲提供商不同，serverless  FaaS 提供商不會向客戶收取空閒計算時間。因此，客戶只需支付他們使用的計算時間，並且不需要浪費資金過度配置雲資源。

## What are the drawbacks of FaaS ?
##### Less system control
讓第三方管理部分基礎架構使得很難理解整個系統並增加調試挑戰。
##### More complexity required for testing
將 FaaS 代碼合併到本地測試環境中非常困難，使得對應用程序的全面測試成為更加密集的任務。

## How to get started with FaaS
開發人員必須與 `serverless` 提供商建立關係，以便為 Web 應用程序啟用 FaaS 功能。由於 FaaS 集成意味著一些應用程序代碼將從邊緣提供，因此**邊緣服務器的可用性和地理分佈是一個重要的考慮因素**。在意大利訪問依賴於巴西超載數據中心提供的 `FaaS` 邊緣代碼的網站的用戶將遇到導致高跳出率的延遲。