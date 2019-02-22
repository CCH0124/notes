# Malicious Payload
惡意有效載荷（Malicious payloads）是造成傷害的網路攻擊的一部分。惡意有效負載可以在觸發之前在計算機或網路上休眠幾秒甚至幾個月。

## What is a malicious payload ?
在網路攻擊的背景下，有效載荷是攻擊的組成部分，對受害者造成傷害。就像希臘士兵藏在特洛伊木馬的故事中的木馬里面一樣，惡意的有效載荷可以無害地停留一段時間直到被觸發。

病毒、wurms 和惡意軟體等攻擊媒介都可以包含一個或多個惡意有效負載。惡意負載也可以在電子郵件附件中找到，實際上賽門鐵克報告說，現有的每359封電子郵件中就有一封包含惡意負載，這個比例呈上升趨勢。

## How do malicious payloads harm their victims ?
惡意負載導致損壞的一些典型示例：
- Data theft
    - 特別常見的是透過各種形式的 `data breaches` 竊取敏感訊息，例如：登錄憑證或財務信息。
- Activity monitoring
    - 執行的惡意有效載荷可以用於監視計算機上的用戶活動，這可以用於spying、blackmail 或聚合可以出售給廣告商的消費者行為的目的。
- Displaying advertisements
    - 某些惡意有效負載可以向受害者顯示持久的，不需要的廣告，例如彈出窗口和彈出式廣告。
- Deleting or modifying files
    - 這是惡意有效載荷產生的最嚴重後果之一。可以刪除或修改文件以影響計算機的行為，甚至禁用操作系統和/或啟動過程。例如：一些惡意有效載荷被設計為"brick"智能手機，這意味著它們不能再以任何方式打開或使用。
- Downloading new files
    - 一些惡意有效載荷來自非常輕量級的文件，易於分發，但一旦執行，它們將觸發下載更大的惡意軟件。
- Running background processes
    - 還可以觸發惡意有效負載以在後台安靜地運行進程，例如：加密貨幣挖掘或數據存儲。
## How are malicious payloads executed ?
攻擊者必須首先找到一種方法將惡意負載傳送到受害者的計算機上。`Social engineering attacks` 和 `DNS hijacking` 是有效載荷傳遞技術的兩個常見示例。

一旦有效載荷到位，它通常會處於休眠狀態直到被執行。攻擊者可以從許多不同的方式中選擇執行惡意負載。執行惡意負載的一些常用方法：
- Opening an executable file
    - 例如：受害者下載他們認為是盜版軟件的電子郵件附件，並雙擊執行有效負載的安裝文件。
- Setting off a specific set of behavioral conditions
    - 這被稱為邏輯炸彈。例如：一個不道德的員工可能會將邏輯炸彈集成到他公司的網路中，該網路會不斷檢查該員工是否仍在工資單上。當他不再在工資單上時，邏輯炸彈將滿足其條件並且將執行惡意有效載荷。
- Opening certain non-executable files
    - 甚至一些非可執行文件也可能包含惡意負載。例如，存在惡意有效負載隱藏在.PNG圖像文件中的攻擊。當受害者打開這些圖像文件時，將執行有效負載。
## How To Stop Malicious Payloads
由於有許多不同的方法來分發和執行惡意有效載荷，因此沒有簡單的靈丹妙藥來緩解它們。除了警惕 `phishing scams` 詐騙和其他 `social engineering` 攻擊外，每當下載文件或從 Internet 接收任何類型的數據時，都應採取安全措施。一個好的經驗法則是始終對下載的文件運行病毒掃描，即使它們看起來來自可信來源。