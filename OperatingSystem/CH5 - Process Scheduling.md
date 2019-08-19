## Basic Concepts
- 幾乎所有程序都有一些交替的 CPU 數字運算週期和等待某種 I/O。（即使從 memory 中獲取簡單的 memory 相對於 CPU 速度也需要很長時間）
- 在運行單個 process 的簡單系統中，等待 I/O 所花費的時間被浪費，並且這些 CPU 週期將永遠丟失
- 調度系統允許一個 process 使用 CPU 而另一個進程正在等待 I/O，從而充分利用原本丟失的 CPU 週期

### CPU-I/O Burst Cycle
- 幾乎所有 process 在一個連續循環中在兩個狀態之間交替，如下面的圖
  - CPU 突發(burst)執行計算
  - I/O 突發(burst)，等待數據傳輸進出系統

![](https://i.imgur.com/sdidxIB.png "Alternating sequence of CPU and I/O bursts.")

- CPU 突發因處理 process 和 program 而異，但廣泛的研究顯示頻率模式類似於下圖

![](https://i.imgur.com/yWZHAFD.png "Histogram of CPU-burst Times")

### CPU Scheduler
