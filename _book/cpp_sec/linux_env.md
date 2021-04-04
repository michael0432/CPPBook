# Linux Environment
###### tags = `APCS`

## Install VirtualBox
VirtualBox是一個執行虛擬機(Virtual Machine)的軟體，在Windows電腦上架設另一個虛擬的環境，用於執行不同的作業系統。這邊使用VirtualBox安裝Linux的環境。

* download page: https://www.virtualbox.org/wiki/Downloads

![](https://i.imgur.com/m7dyEZH.png)


## Install Linux Ubuntu
Linux是一個開源碼(Open source)的作業系統，有很多不同的版本用於不同的情境，其中，Ubuntu其中一個有使用者介面(GUI, Graphic User Interface)的Linux版本。

* download page (18.04, 64bit): https://www.ubuntu-tw.org/modules/tinyd0/

#### 下載完成後打開VirtualBox，選擇Machine->New，輸入使用者名稱跟要安裝的資料夾位置
* Type選擇Linux
* Version選擇Ubuntu(64bit)

![](https://i.imgur.com/PrjogLg.png)

#### 選擇適合大小的Memory (電腦許可的話建議選2G以上)
![](https://i.imgur.com/YQctRk4.png)

#### 選擇Virtual Hard Disk
![](https://i.imgur.com/0HVs2Za.png)

#### 選擇VDI
![](https://i.imgur.com/GiC6eEe.png)

#### 選擇Dynamically Allocate
![](https://i.imgur.com/HukAvUU.png)

#### 選擇初始硬碟大小
![](https://i.imgur.com/jX3Tg3N.png)

#### 完成後，選擇start開啟虛擬機，選擇下載好的Ubuntu Image
![](https://i.imgur.com/AWOMvXP.png)

#### 依照說明安裝Ubuntu，到下面的畫面要選擇清除磁碟並安裝(這裡的清除磁碟指的是虛擬機的磁碟，不會影響到Windows的部分)
![](https://i.imgur.com/6kiDaAQ.png)

#### 安裝完成並重新開機後，選擇Device->Insert Guest Additions CD Image，完成後重開機就可以使用全螢幕
* 右Ctrl+F: 全螢幕
* 右Ctrl+C: 視窗模式

## Install CodeBlock
在terminal中輸入:

    sudo apt install codebloacks
    
## Install C/C++ Compile

在terminal中輸入:

    sudo apt install g++
    
sudo的意思為"以系統管理員身分執行後面這段指令"

## Linux常用指令

    pwd - 顯示當前目錄
    mkdir - 建立資料夾
    ls - 列出當前目錄下的資料夾跟檔案
    cd - 切換目錄
    rm - 刪除檔案
    cp - 複製檔案
    mv - 移動檔案
    cat - 顯示檔案內容
    
