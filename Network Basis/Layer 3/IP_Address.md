# What Is My IP Address ?
IP address 是用於確定誰是 Internet 用戶的唯一標識符（想像是地址）。 IP地址的格式可以不同，具體取決於它們是使用IPv4 還是 IPv6 協定。

## So what is an IP address and why does it matter ?
"IP" 代表 Internet Protocol，它是一組規則，使設備可以透過 Internet 進行通訊，每天有數十億人訪問 Internet，需要使用唯一的標識符來追蹤誰在做什麼，Internet Protocol 透過為訪問 Internet 的每個設備分配的 IP 來解決此問題。

![](https://www.cloudflare.com/img/learning/ddos/glossary/domain-name-system-dns/ddos-dns-request.png)

計算機的 IP address 就像房子的地址。如果有人打電話給比薩店訂購比薩，訂購者需要提供地址。如果沒有這個地址，披薩派送人員將不知道將披薩送到哪個房子！

例如，當用戶在網路瀏覽器中輸入域名（google.com）時，會向 Google 的網路服務器發出請求內容的請求（Google 主頁）。Google 收到請求後，需要知道將網站內容送到何處，因此，請求將包含訪問者的 IP address，使用提供的 IP address，Google 可以將響應發送回用戶的設備，然後該設備將在用戶的 Web 瀏覽器中顯示該內容。

編排所有這些的系統稱為 DNS。它的工作方式類似於 IP address 的電話簿，因此用戶可以使用人性化的 domain names 訪問 Web 服務。當用戶在其瀏覽器窗口中鍵入 "facebook.com" 等 domain names 時，這將開始 DNS query，最終導致 DNS server 將 domain names 轉換為 IP address。

IP address 是什麼樣的？這些地址具有不同的格式，具體取決於它們是 IPv4 還是 IPv6。

## What’s the difference between IPv4 and IPv6 ?
IPv4 和 IPv6 是 Internet Protocol 的不同版本。IPv4 於 1983 年實施，至今仍在使用。 IPv4 address 的格式是由點分隔的四組數字，例如：'8.8.8.8'。這是一種 32-bit 格式，這表示它允許 2<sup>32</sup> 或大約 43 億個唯一的 IP address，而這對於現在在 Internet 上的設備數量來說是不夠的。需要更多 IP address 才能實現 IPv6*。 IPv6 address 使用更複雜的格式，該格式利用由單冒號或雙冒號分隔的數字和字母組，例如："2607:f860:4005:804::200e"。這種 128-bit 可以支持 2<sup>128</sup> 個唯一 IP address。

IPv6 為 IPv4 提供了一些其它更新，包括安全性和隱私改進。儘管存在差異，但 IPv4 和 IPv6 在網路上同時使用了大約十年。這兩個版本可以並行運行，但必須採取特殊措施來促進 IPv4 和 IPv6 設備之間的通訊，必須做出這種妥協，因為很多網路仍在 IPv4 address 上運行。

## What’s the difference between static IPs and dynamic IPs ?
IPv4 address 的有限供應導致動態分配 IP address 的引入，這仍然是一種非常常見的做法。連接到 Internet 的大多數設備都會分配臨時 IP address。例如，當家庭用戶通過其筆記本電腦連接到 Internet 時，該用戶的 ISP 會從共享 IP address pool 中為其分配臨時 IP address。這稱為動態 IP address。對於 ISP 而言，這比為每個用戶分配永久或靜態 IP address 更具成本效益。