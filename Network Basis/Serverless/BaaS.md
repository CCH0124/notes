# BaaS
Backend-as-a-Service (BaaS) 允許開發人員專注於其應用程序的前端，並利用後端服務而無需構建或維護它們。`BaaS` 和 `serverless` 計算有一些相似之處，許多提供商都提供這兩者，但這兩個模型有幾個不同之處。
## What is BaaS ?
後端即服務（BaaS）是一種雲服務模型，其中開發人員將 Web 或  `mobile application` 的所有幕後方面外包，以便他們只需編寫和維護前端。`BaaS` 供應商為在服務器上發生的活動提供預先編寫的軟體，例如：用戶身份驗證、數據庫管理、遠程更新和推送通知（用於移動應用程序），以及雲儲存和託管。

![](https://www.cloudflare.com/img/learning/serverless/glossary/backend-as-a-service-baas/what-is-backend-as-a-service.svg)

考慮在不使用 `BaaS` 提供商指導電影的情況下開發應用程序。除了實際拍攝和指導將出現在電影中的場景之外，電影導演還負責監督或管理攝影人員、燈光、佈景、衣櫥、演員陣容和製作時間表。現在想像一下，如果有一項服務能夠處理所有幕後活動，那麼導演所要做的就是直接拍攝現場。這就是 `BaaS` 的想法：供應商負責"燈光"和"相機"（或服務器端*功能），以便導演（開發人員）可以專注於'行動' - 結束了什麼用戶看到和體驗。

`BaaS` 使開發人員能夠專注於編寫前端應用程序代碼。透過 `API`（由程序提出另一個程序請求的方式）和由 `BaaS` 供應商提供的SDK（用於構建軟體的工具包），他們能夠集成所需的所有後端功能，而無需構建後端他們自己。他們也不必管理服務器，虛擬機或容器來保持應用程序的運行。因此，他們可以更快地構建和啟動移動應用程序和 Web 應用程序（包括單頁應用程序）。

>服務器端是指託管在服務器上或在服務器上發生的所有內容，而不是在 Internet 客戶端 - 服務器模型中的客戶端上。

## What is Mobile-Backend-as-a-Service (MBaaS)?
Mobile-Backend-as-a-Service (MBaaS) 是專門用於構建移動應用程序的 `BaaS`。雖然一些消息來源認為 `BaaS` 和 `MBaaS` 基本上是可互換的術語，但 `BaaS` 服務不一定必須用於構建 `mobile applications`。

## What is included in BaaS ?
`BaaS` 提供商提供許多服務器端功能。例如：
- Database management
- Cloud storage (for user-generated content)
- User authentication
- Push notifications
- Remote updating
- Hosting
- Other platform- or vendor-specific functionalities; for instance, Firebase offers Google search indexing

`BaaS` 和 `MBaaS` 提供商包括 Google Firebase 和 Microsoft Azure。

## What are the differences between BaaS and serverless computing ?
`BaaS` 和 `serverless` 計算之間存在一些重疊，因為開發人員只需編寫應用程序代碼而不考慮後端。此外，許多 `BaaS` 提供商還提供 `serverless` 計算服務。但是，使用 `BaaS` 構建的應用程序與真正的無服務器架構之間存在顯著的操作差異。

##### How the application is constructed
無服務器應用程序的後端被分解為多個功能，每個功能都響應事件並僅執行一個操作（FaaS）。同時，`BaaS` 服務器端功能是在提供商想要的情況下構建的，並且**開發人員不必關心編寫除應用程序前端之外的任何內容**。
##### When code runs
`Serverless` 架構是事件驅動的，這意味著它們是**為響應事件而運行的**。每個函數僅在某個事件觸發時運行，否則不運行。使用 `BaaS` 構建的應用程序通常**不是事件驅動**的，這意味著它們**需要更多的服務器資源**。
##### Where code runs
`Serverless` 功能可以在任何機器上的任何位置運行，只要它們仍然與應用程序的其餘部分進行通訊，這樣就可以透過在網路邊緣運行代碼將邊緣計算結合到應用程序的體系結構中。`BaaS` 不一定要設置為隨時隨地運行代碼（儘管可能是，取決於提供商）。

##### How the application scales
可擴展性是將 `serverless` 架構與其他架構分離的最大區別之一。在 `serverless` 計算中，應用程序會隨著使用量的增加**自動擴展**。雲供應商的基礎架構根據需要啟動每個功能的短暫實例。除非 `BaaS` 提供商還提供 `serverless` 計算，並且開發人員將其構建到他們的應用程序中，否則 `BaaS` 應用程序不會以這種方式進行擴展。

## What is the difference between BaaS and Platform-as-a-Service (PaaS) ?
PaaS 通過雲提供了一個平台，供開發人員構建他們的應用程序。與 `serverless` 計算和 `BaaS` 一樣，Platform-as-a-Service（PaaS）消除了開發人員構建和管理應用程序後端的需要。但是，**`PaaS` 不包括預構建的服務器端應用程序邏輯**，例如推送通知和用戶身份驗證。`PaaS` 為開發人員提供了更大的靈活性，而 `BaaS` 提供了更多功能。