## Why use Serverless Computing ?
Serverless 計算為 Web 開發人員提供了許多好處，包括可擴展性，更快的上市時間和更低的開銷。但是，在某些情況下，其他問題可能會超過這些好處。

與傳統的基於 cloud 或基於服務器的基礎架構相比，serverless 計算具有許多優勢。對於許多開發人員而言，serverless 架構可以**降低成本**、提供**更高的可擴展性**、更大的靈活性和更快的發佈時間。借助 serverless 架構，**開發人員無需擔心購買，配置和管理後端服務器**。但是，無服務器計算並不是所有 Web 應用程序開發人員的靈丹妙藥。

## How does serverless computing work ?
serverless 計算是供應商在需要時提供後端服務的體系結構。

## What are the advantages of serverless computing ?

##### No server management is necessary
儘管"serverless"計算實際上確實發生在服務器上，但開發人員從不必處理服務器。它們由供應商管理。這可以**減少 DevOps 所需的投資**，從而降低開支，並且還可以讓開發人員免於創建和擴展他們的應用程序，而不受服務器容量的限制。

##### Developers are only charged for the server space they use, reducing cost
與"即用即付"電話計劃一樣，開發人員只需為其使用的內容付費。**代碼僅在 serverless 應用程序需要後端功能時運行，代碼會根據需要自動擴展**，供應是動態、精確和實時的。有些服務是如此精確，以至於它們將收費降低到 100 毫秒的增量；相比之下，在傳統的 "server-full" 架構中，開發人員必須提前預測他們需要多少服務器容量，然後購買該容量，無論他們最終是否使用它。

##### Serverless architectures are inherently scalable
想像一下，如果郵局能夠以某種方式神奇的添加和關閉運輸卡車，隨著郵件數量的增加（例如，在母親節之前）增加其機隊的規模，並且在需要更少交付時減少其機隊。這基本上就是 serverless 應用程序能夠做到的。

使用 serverless 基礎架構構建的應用程序將隨著用戶群增長或使用量的增加而自動擴展。如果一個功能需要在多個實例中運行，那麼供應商的服務器將在需要時啟動、運行和結束它們，通常使用 `containers`。因此，serverless 應用程序將能夠處理異常大量的請求，並且它可以處理來自單個用戶的單個請求。具有固定數量的服務器空間的傳統結構化應用程序可能會被使用量的突然增加所淹沒。

##### Quick deployments and updates are possible
使用 serverless 基礎結構，無需將代碼上傳到服務器或執行任何後端配置以釋放應用程序的工作版本。開發人員可以非常快速地上傳代碼並發布新產品，他們可以一次上傳代碼或一次上傳一個功能，因為應用程序不是單個整體堆棧，而是供應商提供的功能集合。

這樣還可以快速更新、修補、修復或向應用程式添加新功能。沒有必要對整個應用程式進行更改，相反，開發人員可以一次更新應用程式一個功能。

##### Code can run closer to the end user, decreasing latency
由於應用程序不在來源服務器上託管，因此可以從任何位置運行其代碼。因此，根據所使用的供應商，可以在靠近最終用戶的服務器上運行應用程式功能，這減少了**延遲**，因為來自用戶的請求不再需要一直前往原始服務器。

## What are the disadvantages of serverless computing ?

##### Testing and debugging become more challenging
很難復制 serverless 環境，以便了解代碼在部署後將如何實際執行。偵錯更加複雜，因為開發人員無法了解後端進程，並且因為應用程序被分解為單獨的較小功能。

##### Serverless computing introduces new security concerns
當供應商運行整個後端時，可能無法完全審查其安全性，這對處理個人或敏感數據的應用程式尤其有問題。


由於公司沒有分配自己的獨立物理服務器，因此 serverless 提供商通常會在任何給定時間在單個服務器上運行其它客戶的代碼。這個與其他方共享機器的問題被稱為"多租戶" - 想想幾家公司同時試圖在一個辦公室租賃和工作，多租戶可能會影響應用程式性能，如果多租戶服務器配置不當，可能會導致數據洩露。多租戶對沙箱正常運行且具有足夠強大的基礎設施的網路幾乎沒有影響。
##### Serverless architectures are not built for long-running processes
這限制了可以在 serverless 架構中經濟高效運行的應用程式類型。由於無服務器提供商對代碼運行的時間量進行收費，因此與傳統服務器相比，在 serverless 基礎架構中運行具有長時間運行流程的應用程序可能會花費更多。

##### Performance may be affected
因為它不是經常運行，所以 serverless 代碼在使用時可能需要"啟動"，此啟動時間可能會降低性能。但是，如果定期使用一段代碼，serverless 提供程序將保持其準備好被激活 - 對此即用型代碼的請求稱為"warm start"；對一段時間未使用的代碼的請求稱為"cold start"。

##### Vendor lock-in is a risk
允許供應商為應用程序提供所有後端服務不可避免的增加了對該供應商的依賴。如果需要，使用一個供應商設置 serverless 架構可能會使供應商難以切換，特別是因為每個供應商提供的功能和工作流程略有不同。

## Who should use a serverless architecture ?
希望縮短產品上市時間並構建可以快速擴展或更新的輕量級，靈活應用程序的開發人員可以從無服務器計算中受益匪淺。

無服務器架構將降低使用不一致的應用程序的成本，峰值時段與幾乎沒有流量的時間交替，對於此類應用程序，購買服務器或一組始終可用且即使在未使用時始終可用的服務器也可能浪費資源。無服務器設置將在需要時立即響應，並且在休息時不會產生成本。

此外，想要將部分或全部應用程序功能推向最終用戶以減少延遲的開發人員至少需要部分無服務器架構，因為這樣做需要將一些進程移出原始服務器。

## When should developers avoid using a serverless architecture ?
從成本角度和系統架構角度來看，有些情況下使用自我管理或作為服務提供的專用服務器會更有意義。例如，具有相當恆定，可預測的工作負載的大型應用程序可能需要傳統的設置，並且在這種情況下，傳統的設置可能更便宜。

此外，**將遺留應用程序遷移到具有完全不同架構的新基礎架構可能非常困難**。