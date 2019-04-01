# PaaS vs. Serverless
雖然 `PaaS` 和 `serverless` 計算都不涉及開發人員的後端管理，但有兩個因素將這兩個模型分開，包括可擴展性、定價以及在網路邊緣部署的能力。

## How are serverless computing and Platform-as-a-Service (PaaS) different ?
由於 `serverless` 計算和 `Platform-as-a-Service` (PaaS) 後端架構使整個後端對開發人員不可見，因此它們有些相似。但是，兩種架構之間存在一些很大的差異，大多數用例最適合其中一種，但兩者都不兼容。`PaaS` 和 `serverless` 之間的主要差異是可擴展性、定價、啟動時間、工具以及在網路邊緣部署的能力。

![](https://www.cloudflare.com/img/learning/serverless/glossary/serverless-vs-paas/paas-serverless-comparison.svg)

## Which scales better, PaaS or serverless architecture ?
`Serverless` 應用程序可以立即、自動和按需求擴展，**無需開發人員或供應商提供任何額外配置**，它們能夠按定義擴展。相比之下，雖然開發人員可以根據用戶需求對 `PaaS` 託管的應用程序進行編程，但這並不是 `PaaS` 的固有特性，**開發人員必須進行一定量的預測才能正確擴展**。

可以將 `serverless` 計算與來自水龍頭的抽水進行比較，其中水代表計算能力。現代家庭中的水龍頭可以隨時打開，並可以生產盡可能多的水。`PaaS` 更像是使用飲水機和水瓶遞送服務。仍然可以根據需要獲得盡可能多的飲用水，但它並不像打開水龍頭那麼簡單。如果需求增加，消費者必須要求供應商提供更多。在這兩種情況下，其他人正在處理"後端" - 淨化水資源，將其帶到建築物等等 - 但只有自來水可以精確，按需和實時縮放。

`Serverless` 架構能夠通過在請求時啟動應用程序功能的新實例來快速擴展。它還可以通過在不再需要時關閉功能或一旦運行一段時間後關閉功能來快速縮小。實際上，`serverless` 的 Web 應用程序能夠一直縮放到無活動，然後在幾秒或幾毫秒內響應事件再次啟動。基於 `PaaS` 構建的應用程序無法如此快速地擴展或擴展到這種程度。

## How is pricing different between PaaS and serverless ?
為了繼續水的比喻，使用自來水的消費者支付與他們使用的水一樣多的水。同樣，`serverless` 計費非常精確，開發人員只需支付他們使用的費用。一些 `serverless` 供應商僅向開發人員收取其功能運行的精確時間，對於每個功能的每個單獨實例，只需幾分之一秒。其他提供商按請求數收費。

使用飲水機並且運送水壺的消費者也只需支付他們使用的費用，但他們用水壺支付，而不是用液體盎司支付。同樣，一些 `PaaS` 供應商僅**針對應用程序使用的內容向開發人員收費**。但是，計費並不像無服務器那樣精確。其他 `PaaS` 供應商對其服務收取每月固定費用。開發人員通常能夠自定義他們支付的計算能力。然而，這是事先決定的，並且對實時使用的增加和減少沒有響應。

這種差異並不一定意味著 `serverless` 架構總是更實惠。正如水龍頭一直運行很快變得非常昂貴，具有大量恆定使用流量且不會波動很大的 Web 應用程序可能會因使用 `serverless` 計算而變得昂貴。

## What is the difference in launch time between PaaS and serverless applications ?
如上所述，只要事件觸發應用程序功能，`serverless` 應用程序幾乎可以立即激活。`PaaS` 構建的應用程序**可以快速啟動和運行，但它們不像 `serverless` 應用程序那樣輕量級，並且需要更長時間才能啟動和運行**。從用戶的角度來避免延遲，至少 `PaaS` 應用程序的某些功能必須在大多數時間或所有時間運行。

## What tools do PaaS and serverless vendors provide ?
一般來說，`PaaS` 供應商將**為開發人員提供更多工具來`構建`和`管理`他們的應用程序，包括用於`測試`和`調試`的工具**。由於 `serverless` 應用程序**不能在指定的計算機上運行，無論是虛擬的還是其他的，以及 `serverless` 的功能都應該運行相同， `serverless` 供應商可能會提供一些工具，但它們不能提供構建和測試應用程序的完整環境**。

## Can serverless applications be deployed on the network edge ?
`Serverless` 代碼**不能在特定服務器上運行**，並且可以在 Internet 的任何部分上運行，這使得可以非常靠近網絡邊緣的最終用戶部署無服務器應用程序，從而大大減少延遲。

## Can applications built using PaaS be deployed on the network edge ?
從開發人員的角度來看，`PaaS` 中沒有服務器。但是，就託管代碼的位置而言，`PaaS` 仍然與 `serverless` 計算不同。`PaaS` 供應商要麼利用其他供應商的 `IaaS`（Infrastructure-as-a-Service）產品，要麼擁有自己的物理數據中心。結果是，**構建在雲平台上的應用程序可能僅在某些指定的計算機上運行，從而阻止開發人員通過在邊緣運行代碼來優化其應用程序的性能**。