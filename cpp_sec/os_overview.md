# Operating System Overview
###### tags = `Operating System`

## 什麼是作業系統
作業系統是一個控制電腦操作、配置硬體及軟體資源、協助使用者跟電腦溝通的系統(大型程式)，作業系統需要負責管理記憶體、決定系統資源配置、控制輸入及輸出、管理檔案系統等工作。
![](https://i.imgur.com/5OdFP7c.png)

根據不同的使用方式，作業系統會有不同設計上的考量:
* 個人電腦 - 好操作、常用的功能有方便使用的GUI(Graphical user interface)
* 伺服器 - 公平分配多個使用者的資源
* Embedding System(嵌入式系統) - 不考慮user使用的便利性、盡量輕量化

## Computer System Organization

* I/O的裝置跟CPU同時運作
* 每個裝置都有Local Buffer
* CPU負責把資料在記憶體及各個裝置的Local Buffer中移動
![](https://i.imgur.com/6G0E9oe.png)

* Interupt: 當發生一個事件時，通常由硬體或軟體產生interupt的信號(例如等待I/O、CTRL+C一個process等)，這個信號會傳送到cpu，cpu會存下目前的運算狀態並立即中止運算，改為運算下一個process。

## Storage Structure

CPU只能從記憶體載入程式，任何正在被執行的程式都存在記憶體中，一般電腦中的記憶體被稱為RAM(random-access memory)，以DRAM(dynammic random-access memory)的半導體技術做成。理想上來說，我們希望程式的資料永遠存在memory中，但實際上不太可行，因為:
* Memory通常太小，無法存下大量資料
* 電源被關掉時，Memory中的資料會消失

因此，電腦中的儲存裝置分成下列幾種:

* Cache - 快取記憶體，CPU會優先從Cache找資料(容量最小)
* Memory - 記憶體，CPU可以直接從記憶體中拿資料
* Disk - 硬碟，所有資料存放的位置(容量最大)

![](https://i.imgur.com/laiRtyo.png)


為什麼要設計memory/disk，不直接把資料都放在disk裡就好?

![](https://i.imgur.com/VCwQRpo.png)

## I/O
一般的個人電腦由CPU、Storage及許多I/O裝置所組成，I/O裝置包含螢幕(顯示卡)、鍵盤滑鼠、音響(音效卡)等。每個I/O的裝置都有自己的buffer，為了管理這些I/O裝置傳遞來的訊號及資料，作業系統在每個裝置的controller上都會安裝驅動程式(driver)。在開始一個I/O的動作時，裝置的controller會接受訊號(例如按下鍵盤上的一個按鍵)，並把資料放到local buffer中，當I/O完成後，controller會傳遞一個訊號(interupt)給device driver。

![](https://i.imgur.com/iHHAAXc.png)

## 作業系統的工作

* Process: 每一個正在作業系統上被執行的程式即為一個process，Process需要有資源(CPU, Memory, I/O, Files)才能正常的運作，作業系統需要管理在上面運行的每一個process的狀態，並且分配給每個Process所需的資源。

![](https://i.imgur.com/GSWPhWI.png)

* CPU Scheduling: 一個Process除非CPU執行它的指令，否則不能做任何事情，因此作業系統需要有一套演算法安排各個Process如何分配CPU的資源，避免同一個Process大量占用資源。

![](https://i.imgur.com/4QmIagN.png)

* Memory Management: Process在執行時，它所需的資料必須對應到存該資料的記憶體位址，並在程式結束後釋放記憶體空間，作業系統需要一套演算法管理記憶體:
    * 記錄記憶體的哪些部分正在被使用，及誰在使用
    * 決定哪些資料要放入或移出記憶體
    * 重新配置或回收記憶體空間

* File Management: 電腦中儲存了大量的資料，作業系統需要負責:
    * 建立、刪除檔案
    * 管理檔案的目錄(資料夾)
    * 備份(複製)檔案

## 控制作業系統的方法
一般來說，有兩種控制作業系統的方法:
* 透過command interpreter
    * 在windwos和unix系統中，它們將command interpreter當作一個特殊的程式，command interpreter取得使用者輸入的指令並執行。
    * 舉例來說，在cmd中輸入dir看當前的資料夾有哪些檔案，便是透過command interpreter操作作業系統
* 透過GUI(graphical user interface)
    * 由於輸入命令會讓大部分的使用者難以操作，因此有些作業系統會將常用的操作設計成GUI的形式，利用圖像、影像或表格等畫面，讓使用者可以用鍵盤及滑鼠操作作業系統。

## System Call
作業系統提供System Call讓使用者寫的程式可以使用作業系統的服務，System Call通常用較低階的語言寫成(C/C++/組合語言)。
舉例來說，若一段程式需要將A檔案中的文字複製到B檔案中，就會牽涉到很多的System call
* 開啟A檔案
* 開啟B檔案
* 確認讀取A檔案的權限
* 開啟寫入B檔案的權限
* 讀取A檔案的資料
* 將資料寫入B檔案
* 關閉A檔案
* 關閉B檔案

![](https://i.imgur.com/7rjRU9V.png)


在Linux中，可以藉由man這個指令來取得system call相關的資訊。

```shell=1
man read

#include <unistd.h>
ssize_t read(int fd, void *buf, size_t count);
```

## Kernel

Kernel用來管理軟體發出的I/O請求，並將這些請求轉譯成指令交給驅動程式及硬體做處理。Kernel是作業系統中最重要的部分之一，它管理了軟體(應用程式)對電腦硬體存取的權限，由Kernel決定一個程式可以對哪部分的硬體操作多長的時間。

![](https://i.imgur.com/FcMbVQA.png)


## Reference
* Operating System Concepts, 9th Edition.
* Advanced Programming in the UNIX Environment, third Edition.
* http://linux.vbird.org/