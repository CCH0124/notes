## VNF
簡單來說，VNF 代表 virtualized network function（虛擬化網路功能），通常指網路設備的軟體形式，如路由器、防火牆、負載均衡器等。
VNF 主要部署為 Linux KVM 或 VMware vSphere 虛擬機管理程序上的虛擬機（VM）商用現貨硬件（COTS）。與 VNF 相反，物理網絡功能（physical network function, PNF）是指專有硬體上的傳統網路設備。雲本地網絡功能（CNF）是指容器化的VNF，可以是微服務之間的容器組網和服務網。
## NFV
另一方面，NFV 代表  network function virtualization （網路功能虛擬化）。
它指的是在 COTS 硬體上在虛擬化基礎架構上編排和自動化 VNF 軟體設備，然後通過端到端生命週期管理 VNF 設備的操作框架。
NFV 依賴於軟件定義網絡（SDN）原則，將網路操作分為用戶平面、控制平面和管理與編配（MANO）平面。
