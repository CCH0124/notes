# Meltdown/Spectre
Meltdown 和 Spectre 是影響大多數 PC 和智慧手機的新處理器漏洞。有可用的補丁，但它們會影響處理器性能。

## What is Meltdown/Spectre ?
Meltdown 和 Spectre 是最近在 Intel、AMD、Apple 和 ARM 處理器芯片中發現的漏洞。這些漏洞是受影響芯片嚴重設計缺陷的結果，而這一問題的發現導致 Windows、Mac 和 Linux 作業系統軟體的強制重新設計，以減輕漏洞並防止攻擊者利用它。

這些漏洞是 Google Project Zero 的研究人員發現的，該團隊致力於在被攻擊者利用之前發現安全漏洞。截至目前，目前還沒有已知的Meltdown 或 Spectre 漏洞。Apple、Intel 和 Microsoft 等主要技術公司的安全團隊以及開源 Linux 開發人員現在都在投入大量資源來確保他們的處理器和操作系統在任何惡意攻擊之前得到保護。
## Who is affected by the Meltdown and Spectre vulnerabilities ?
除了少數例外，擁有 PC 或智能手機的每個人都面臨著風險。根據 Google 的說法，1995 年以後製造的所有帶有 Intel 處理器芯片的設備都會受到影響。 AMD 和 ARM 芯片更難開發，但它們也面臨風險。

## How to protect against the Meltdown/Spectre vulnerability ?
除了更換 PC 的處理器外，關閉漏洞的唯一方法是修補作業系統。蘋果公司在 12 月初悄然向 OSX 推出了一個 Meltdown 補丁，而微軟在1月3日發布了一個 Windows 補丁，而 Linux 開發人員仍然忙於整理補丁。

這些 Meltdown 補丁的一個不幸的副作用是，它們將透過設計降低使用補丁作業系統的計算機的處理速度。這些減速將影響性能約 5-30%，這在很大程度上取決於芯片的類型和正在執行的任務。
## How do the Meltdown and Spectre vulnerabilities actually work ?
Meltdown 和 Spectre 都是在執行稱為 "kernel code" 的特殊低級代碼時創建的漏洞，該代碼在稱為推測執行（speculative execution）的過程中專門運行。

## What is speculative execution ?
使用隱喻可能最容易解釋推測性執行。想像一下，一個徒步旅行者在樹林裡迷路，他們在小徑上遇到一個岔路，創造了兩條大致平行的路徑，一條道路將讓徒步旅行者回家，另一條道路不會。她沒有浪費時間等待另一位徒步旅行者給她方向，而是選擇了她認為最有可能讓她回家的路。在徒步旅行的某個時刻，她遇到一個小道標記，如果那個小道標記通知她她在正確的道路上，那麼她繼續沿著那條小路走回家。如果小道標記告訴她她在錯誤的路徑上，她會迅速回溯並跳到備用小道上，這使她沒有比如果她仍然在小徑的底部希望方向更糟糕。

![](https://www.cloudflare.com/img/learning/security/threats/meltdown-spectre/speculative-execution.png)

許多現代處理器執行稱為推測執行的類似技術，其中 CPU 試圖猜測接下來需要執行哪些代碼，然後在被要求之前執行該代碼。如果不再需要執行的代碼，則還原更改。這是為了節省時間和提高性能。

有關 Meltdown/Spectre 漏洞的報告表明，Intel CPU 可能正在執行代碼的推測性執行，而無需進行重要的安全檢查。可以編寫用於檢查處理器是否已完成通常會被這些安全檢查阻止的指令的軟件。

這種對推測性執行的錯誤處理會產生一個 CPU 漏洞，攻擊者可利用該漏洞訪問內核內存中非常敏感的數據，如：密碼、加密密鑰、個人照片和電子郵件等。

## So what’s a kernel ?
**內核是計算機作業系統核心的程序**。它完全控制作業系統，並管理從啟動到內存處理的所有內容。內核還負責將數據處理指令發送到 CPU（中央處理單元）。大多數 CPU 在 kernel mode 和 user mode 之間不斷地來回切換。

## What’s the difference between kernel mode and user mode ?
在 kernel mode 下，CPU 正在執行對計算機硬體和內存進行無限制訪問的代碼，此模式通常保留用於最低級別和最可信任的操作。CPU 處於 kernel mode 時發生的崩潰可能是災難性的，他們可以崩潰整個作業系統。

在 user mode 下，正在執行的代碼無法訪問硬體或參考內存，而是必須委託給系統 API（系統 API 可以運行 user mode 軟體可以使用適當權限請求的 kernel mode 功能）。user mode 崩潰通常是獨立和可恢復的。大多數代碼在 user mode 下執行。

## Why does the Meltdown patch slow down performance ?
Meltdown 補丁中的修復涉及**內核內存**與**用戶進程**的更為戲劇性的分離。這是透過一種稱為內核頁表隔離（ Kernel Page Table Isolation, KPTI）的方法完成的。KPTI 將 kernel mode 操作移動到與 user mode 操作完全獨立的地址空間。這意味著在 kernel mode 和 user mode 之間切換需要更多的時間。

為了說明這一點，想像一下只賣兩種食品的食品卡車：熱狗和冷檸檬水。食品卡車內的員工可以很容易地到達包含熱狗的蒸籠和包含冷檸檬水的冷卻器，並且商業活動非常活躍。現在想像一下，衛生檢查員會通過，並要求將冷熱食品放在不同的場所。現在員工仍然可以到達熱狗，但必須離開卡車，沿著街道散步去取每個檸檬水。這將導致食品卡車的運行速度變慢，特別是如果人們訂購了大量的檸檬水。這類似於KPTI 如何降低操作系統的性能。

## What’s the difference between Meltdown and Spectre ?
Meltdown 和 Spectre 都是由**處理器處理推測性執行**的方式創建的漏洞，但它們的工作方式和受影響的處理器類型有所不同。

Meltdown 僅影響 Intel 和 Apple 處理器，並可被利用來洩漏由於處理器在推測執行期間執行的代碼而暴露的信息。Meltdown 比 Spectre 更容易被利用，並被安全專家稱為更大的風險。值得慶幸的是，Meltdown 也更容易修補。

Spectre 會影響 Intel、Apple、ARM 和 AMD 處理器，它可以被利用來實際欺騙處理器運行不應該被允許運行的代碼。根據 Google 的安全專家的說法，Spectre 比 Meltdown 更難開發，但它也更難以緩解。

## Ref
[meltdown](https://meltdownattack.com/meltdown.pdf)
[spectreattack](https://spectreattack.com/spectre.pdf)