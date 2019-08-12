# Threads（執行緒）

- Threads 是 CPU 使用率的基本單元，由一個 program counter、stack、thread ID 和 registers 組成
- 傳統（heavyweight, 重量級）process 具有單一執行緒控制 - 有一個 program counter，以及可在任何給定時間執行的一系列指令
- 多執行緒應用程序在單個 process 中具有多個執行緒，每個執行緒都有自己的 program counter、stack 和 registers，但共用程式區域、數據和某些 OS 資源（如打開文件）。

![ Single-threaded and multithreaded processes](https://i.imgur.com/k1clVLq.png "Single-threaded and multithreaded processes")

### Motivation
