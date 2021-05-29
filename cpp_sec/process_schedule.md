# Procss Schedule
###### tags = `Operating System`

作業系統中，通常會有以下三種queue存放process。
* Job queue : 存放所有的process
* Ready queue : 存放狀態為ready且已經被放進memory中等待執行的process
* Device queue : 存放正在等待I/O的process

這些queue一般都是用linked list儲存，每個PCB中會包含指向下一個PCB的Pointer。



## Process scheduler

一個process在生命週期中，會在各個不同的queue中移動，作業系統為了排班的目的，需要有一套機制以某些次序從這些queue中選取process，這套機制稱為scheduler。
Scheduler通常被分為Long-term scheduler及Short-term scheduler，兩者的區分為它們的執行次數。

### Short-term scheduler
Short term scheduler也被稱為CPU scheduler，功能為從ready queue中選出一個process，並將CPU的運算資源分配給它。Short term scheduler需要在盡量短的時間內決定下一個process，避免浪費CPU的資源，且會頻繁的被執行(至少每100毫秒會執行一次)。

### Long-term scheduler
Long term Scheduler也被稱為Job Scheduler，從Job queue中找出適合的process，將其需要的資訊放入memory中，並將此process放入ready queue。Long term scheduler只有在有process新增或刪除時才會執行，因此相較於Short-term scheduler有相當長的時間可以決定適合的process。

一般來說，多數的Process都可以被分成IO-Bound或是CPU-Bound。
* I/O Bound : 做I/O的時間較多
* CPU Bound : 做運算的時間較多

Long-term scheduler會傾向讓Ready queue中的I/O Bound Process及CPU Bound Process比例合適。如果幾乎所有process都是I/O Bound，會導致Ready queue中的process太少；如果幾乎所有process都是CPU Bound，會導致Device queue中的process太少。這兩種狀況會導致系統的效能沒有最大化。


