# File System
###### tags = `Operating System`

File system提供作業系統以及使用者儲存和存取資料的功能，檔案系統由兩個不同的部分組成: File(檔案)和Directory(資料夾)。

## File
在作業系統上，所有的資料必須放在檔案中，才能儲存在儲存裝置中(硬碟等等)，一個檔案通常會有幾個特性:
* File name : 檔案名稱，人看的懂的名字
* ID : 檔案識別符號，通常是一個獨一無二的數字，讓作業系統識別檔案
* Type : 對於支援不同檔案型態的作業系統來說，需要Type來分辨此檔案型態為何，不同的作業系統支援的檔案型態可能不同，例如，Windows系統支援exe檔；但是Linux不支援。
* Location : 一個指向該檔案儲存位置的指標
* Size : 檔案目前的容量大小
* Protection : 控制哪個使用者能讀、寫、執行此檔案
* Time : 包含上次修改、上次使用的時間等等資訊 

![](https://i.imgur.com/GnmVN0u.png)

## Directory

作業系統上，通常會需要儲存大量的檔案，因此，需要資料夾的概念將檔案分門別類的整理，讓使用者可以快速找到需要的檔案，現今的作業系統通常使用樹狀目錄來儲存資料夾。

![](https://i.imgur.com/WcmoOrk.png)

從使用者的觀點來看，可以使用檔案路徑來找到需要的檔案，檔案路徑分為兩種: 絕對路徑(Absolute path name)；相對路徑(relative path name)。
* 絕對路徑: 由根部開始循著一條路找到檔案 
    * 在linux中，絕對路徑的表示法為/開頭，舉例來說: /bin/stat/
* 相對路徑: 由目前現用的位置去的路徑
    * 在linux中，相對路徑的表示法為./或沒有開頭，舉例來說: mydir/a.txt或./mydir/a.txt

在樹狀目錄中，整個檔案系統的結構是一個Tree，意思是，沒有任何一個檔案或資料夾有兩個parent，這項定義限制了檔案或資料夾的共用。

### Acyclic Graph

一個Acyclic graph允許相同的資料夾或是檔案可以存在不同的資料夾中，在被許多人共用的Server上，可以使用Acyclic Graph作為檔案系統。

![](https://i.imgur.com/XVGUnE2.png)

用Acyclic graph作為檔案系統的結構讓整個檔案系統更加彈性，但是要解決的問題也變多了，這時，一個檔案可以有多個絕對路徑，因此，不同的檔案名稱(路徑名稱)可能指向同一個檔案，且在刪除檔案時也需要檢查這個檔案是否有多個指標使用，並找出這些指標的主人，這可能會花費相當大的系統效能。

## File System

一個File system的架構如下圖:

![](https://i.imgur.com/lO9Oz5V.png)

* Basic file system : 負責發出命令給Driver，例如，讀XX硬碟的OO sector。
* File organization module : 知道每個檔案對應的邏輯區段(作業系統上的檔案路徑)以及實體區段(在硬碟上的位置)
* Logical file system : 包含檔案的metadata，但不包含真正的資料，通常被稱為FCB或inode。

整個讀取檔案的流程為:
* Application : Process發出讀取檔案的請求
* Logical file system : 確認權限，根據檔案路徑找到對應的FCB或inode
* File organization module : 將檔案路徑對應到硬碟實體的區塊
* Basic file system : 對硬碟的Driver下I/O指令
* I/O Control : 實際的I/O控制
* Device : 從儲存裝置中讀出資料

### Mount and Partition

一個磁碟可以根據作業系統的設定有不同的規劃，一個磁碟可以被分割成多個Partition，各個Partition可以有不同的功能以及不同的格式，舉例來說，root partition包含作業系統的kernel以及其他系統相關的檔案，在電腦被啟動時就會被mounted(掛載)，其他的partition可以在稍後被手動或自動mounted。

一個檔案系統需要先Mount才可以在作業系統上使用，在Linux上，一個硬碟上也可以有許多個File System: 

![](https://i.imgur.com/UBfehvY.png)

每種不同的file system有不同的使用情境，舉例來說，tmpfs : 當記憶體產生暫時的檔案時，會先放在tmpfs下，系統重製時，整個tmpfs都會被清除。

![](https://i.imgur.com/eiMeOJ3.png)
