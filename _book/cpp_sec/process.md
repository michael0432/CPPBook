# Process
###### tags = `Operating System`

## Process
Process為一個執行中的程式，是作業系統基本的工作單元，CPU會根據排程演算法選擇要執行哪一個Process，CPU的一個core，只能執行一個process。Process要完成工作需要許多種類的資源:CPU計算、記憶體儲存所需的資料、I/O裝置、檔案等等。

在Linux系統中，可以使用top及ps來顯示目前各Process的狀態。

    top
    ps
    ps aux

### Process State
當Process執行時，它會改變其狀態，可以分為下列幾種狀態:
* new : Process正在被產生
* running : Process正在執行
* waiting : 正在等待某事件發生(例如I/O)
* ready : 正在等待CPU資源
* terminated : 完成執行，正在被結束

![](https://i.imgur.com/ShhrsGS.png)


### Process Control Block
每一個Process在作業系統中，都有對用的Process Control Block(PCB)，用於紀錄Process的資訊，資訊包含:

* Process state: New, Ready, Waiting, Running, Terminated
* Process number: Process id, 用來識別Process，每個Process id對應到一個PCB
* Program counter: 紀錄CPU下一行要執行的指令位置
* Register: 存process相關的其他資訊(例如priority)
* Memory Limits: Memory管理相關
* List of open files: File Descriptor

![](https://i.imgur.com/IApBnFW.png)

In register:

![](https://i.imgur.com/Pn2SlqM.png)


### Special Process
在Linux系統中，有一些特別的Process id是系統所使用的。
* pid = 0 : Swapper(Schedulre)，負責管理CPU排程的Process
* pid = 1 : Init，所有的process透過這個process被建立及回收


### Context Switch

當CPU收到interrupt信號時，會先將目前在做的process狀態存在該process的PCB內，並且根據下一個process的PCB資訊，回復下一個process到上次執行完的狀態。這整個過程被稱為Context Switch。

![](https://i.imgur.com/t29Dhon.png)
