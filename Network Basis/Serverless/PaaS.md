# PaaS
Platform-as-a-Service（PaaS）供應商提供基於雲的平台，用於`構建`和`運行應用程序`。
## What is Platform-as-a-Service (PaaS) ?
在 Platform-as-a-Service （PaaS）模型中，開發人員基本上可以租用構建應用程序所需的一切，**依靠雲提供商來開發工具、基礎架構和作業系統**。這是雲計算的三種服務模型之一。`PaaS` 極大地簡化了 Web 應用程序的開發，從開發人員的角度來看，所有後端管理都是在幕後進行的。儘管 `PaaS` 與 `serverless` 計算有一些相似之處，但它們之間存在許多重要差異。

## What are the three service models of cloud computing ?

![](https://www.cloudflare.com/img/learning/serverless/glossary/platform-as-a-service-paas/saas-paas-iaas-diagram.svg)

雲計算的三種模型是 `PaaS`、`SaaS`（軟體即服務）和 `IaaS`（基礎架構即服務）。`IaaS` 是指雲計算基礎架構-服務器，儲存等由雲供應商管理，而 `SaaS` 是指託管在雲中並由 `SaaS` 供應商維護的完整應用程序。如果 `SaaS` 客戶就像租房子一樣，那麼 `PaaS` 客戶就像租用快速建造房屋所需的所有重型設備和電動工具一樣，如果工具和設備由其所有者不斷維護和修理。

## How does PaaS compare to internally hosted development environments ?
可以透過任何互聯網連接訪問 `PaaS`，從而可以在 Web 瀏覽器中構建整個應用程序。由於開發環境不是本地託管的，因此開發人員可以在世界任何地方使用該應用程序。這使得遍布地理位置的團隊能夠進行協作。這也意味著**開發人員對開發環境的控制較少**，儘管這會帶來更少的開銷。

## What is included in PaaS ?
`PaaS` 供應商提供的主要產品包括：
- Development tools
- Middleware
- Operating systems
- Database management
- Infrastructure

>不同的供應商也可能包括其他服務，但這些是核心 `PaaS` 服務。

##### Development tools
`PaaS` 供應商提供軟體開發所需的各種工具，包括源代碼編輯器，調試器，編譯器和其他必要工具。 這些工具可以作為框架一起提供。 提供的具體工具將取決於供應商，但 `PaaS` 產品應包括開發人員構建其應用程序所需的一切。

##### Middleware
作為服務提供的平台通常包括 Middleware，因此**開發人員不必自己構建它**。Middleware 是位於面向用戶的應用程序和機器操作系統之間的軟體。例如，。Middleware 允許軟體訪問鍵盤和鼠標的輸入。。Middleware 是運行應用程序所必需的，但最終用戶不會與之交互。

##### Operating systems
PaaS 供應商將提供並維護開發人員工作的作業系統並運行應用程序。

##### Databases
PaaS 提供商管理和維護數據庫。他們通常也會為開發人員提供數據庫管理系統。

##### Infrastructure
`PaaS` 是雲計算服務模型中 `IaaS` 的下一層，`IaaS` 中包含的所有內容也包含在 `PaaS` 中。 `PaaS` 提供商可以管理服務器，儲存和物理數據中心，也可以從 `IaaS` 提供商處購買。

![](https://www.cloudflare.com/img/learning/serverless/glossary/platform-as-a-service-paas/saas-paas-iaas-cloud-pyramid.svg)

## Why do developers use PaaS ?
##### Faster time to market
如果開發人員不得不擔心構建配置和配置他們自己的平台和後端基礎架構，那麼 `PaaS` 用於更快地構建應用程序。使用 `PaaS`，他們需要做的就是編寫代碼並測試應用程序，供應商處理剩下的工作。

##### One environment from start to finish
`PaaS` 允許開發人員在同一環境中構建、測試、調試、部署、託管和更新其應用程序。這使開發人員能夠確保 Web 應用程序在發布之前能夠正常運行，並簡化應用程序開發生命週期。

##### Price
在許多情況下，`PaaS` 比利用 `IaaS` 更具成本效益。 由於 `PaaS` 客戶不需要管理和配置虛擬機，因此減少了開銷。此外，一些提供商具有按使用付費的定價結構，其中供應商僅對應用程序使用的計算資源收費，通常為客戶節省資金。但是，每個供應商的定價結構略有不同，一些平台供應商每月收取固定費用。

##### Ease of licensing
`PaaS` 提供商處理操作系統，開發工具以及其平台中包含的所有其他內容的所有許可。

## What are the potential drawbacks of using PaaS ?
##### Vendor lock-in
切換 `PaaS` 提供商可能會變得很困難，因為應用程序是使用供應商的工具構建的，特別是針對他們的平台。每個供應商可能有不同的架構要求。不同的供應商可能不支持用於構建和運行應用程序的相同語言、`libraries`、API、體系結構或作業系統。要切換供應商，開發人員可能需要重建或大量更改其應用程序。

##### Vendor dependency
更改 `PaaS` 供應商所涉及的工作量和資源可能會使公司更依賴於他們當前的供應商。供應商內部流程或基礎架構的微小變化可能會對旨在高效運行舊配置的應用程序的性能產生巨大影響。此外，如果供應商更改其定價模型，則應用程序可能會突然變得更加昂貴。

##### Security and compliance challenges
在 PaaS 架構中，外部供應商將存儲應用程序的大部分或全部數據，並託管其代碼。在某些情況下，供應商實際上可以通過另一個第三方（IaaS 提供商）存儲數據庫。 雖然大多數PaaS 供應商都是具有強大安全性的大型公司，但這使得難以全面評估和測試保護應用程序及其數據的安全措施。此外，對於必須遵守嚴格的數據安全法規的公司，驗證其他外部供應商的合規性將增加進入市場的障礙。
## How is Platform-as-a-Service different from serverless computing ?
`PaaS` 和 `serverless` 計算類似於兩者，開發人員不得不擔心的是編寫和上傳代碼，供應商處理所有後端進程。但是，使用這兩種模型時，縮放會有很大差異。使用無服務器計算或 `FaaS` 構建的應用程序將**自動擴展**，而 `PaaS` 應用程序除非經過編程才能進行擴展。啟動時間也差異很大，無服務器應用程序幾乎可以立即啟動和運行，但 `PaaS` 應用程序更像傳統應用程序，必須在大多數時間或所有時間運行才能立即為用戶提供服務。

另一個區別是無服務器供應商不像 `PaaS` 供應商那樣提供開發工具或框架。最後，定價將兩種模型分開。`PaaS` 計費並不像無服務器計算那樣精確，其中費用被分解為每個函數實例運行的秒數或分數秒。