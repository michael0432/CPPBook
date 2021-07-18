# Thread

###### tags = `Operating System`

Thread(執行緒)是作業系統可以運算排程的最小單位，一個Process可以擁有多個Thread，每個Thread可以執行不同的任務，在User space的觀點下，這些thread是類似平行化的運算(實際上並不是，多個thread會根據CPU的排程在不同時間內拿到CPU的運算資源)。

![](https://i.imgur.com/8peU4uM.png)

在一個process中的各個thread可以共享process的部份資源，例如開啟的檔案、全域變數等等。但同時，各個thread也擁有各自的register及stack，可以使用不被其他thread干擾的區域變數。

## Multi-Thread V.S. Multi-Process

* Multi-process:
    * Process間透過pipe/fifo/socket溝通、可以直接共享的資源不多
    * 不同的Process的使用者可能不同
    * 新增一個process(fork())消耗的資源跟花費的時間較大
    * Context switch花的時間較長
* Multi-thread:
    * Thread間可以透過process的內部資源溝通
    * 同一個user使用
    * 新增一個thread花費的資源較小，且在thread之間切換context switch的時間較短

## User level thread / Kernel level thread

* User level thread : 在User space使用的thread，在使用者自己寫的process中使用的thread為user level thread
* Kernel level thread : 在Kernel space使用的thread，由作業系統管理，CPU排班時實際上參考的資源。

User level thread跟Kernel level thread彼此之間有對應的關係，根據作業系統的設計有所不同。

![](https://i.imgur.com/ChTXZz8.png)

* 多對一 : 多個User level thread直接對應到一個Kernel level thread
    * 優點: 有效率，不需要搜尋可用的Kernel level thread
    * 缺點: 其中一個user level thread call blocking call時，會卡住其他的thread
* 一對一 : 一個User level thread直接對應到一個Kernel level thread
    * 優點: user level thread之間不會互相blocking
    * 缺點: 需要大量的Kernel level thread
* 多對多 : 多個User level thread直接對應到多個Kernel level thread
    * 優點: 使用彈性，使用者程式設計時不需要考慮thread對應的問題
    * 缺點: 花較多的效能做thread mapping









