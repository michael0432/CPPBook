# Git Basic
###### tags = `Git`

Git是一個用於版本控制的系統，可以把檔案更新的紀錄保存下來，並且可以方便地在不同版本的檔案中做切換，把編輯過的檔案回到以前的狀態。

可以簡單的把版本控制分成兩個部分，一個是現在本機端的程式碼(local)；另一個是git server上的程式碼(remote)，每次做完修改後，本機端的程式碼會被改變，可以將本機端的程式碼上傳到server上，server上便會存下這次的改變。

## Download & Install

Github : https://github.com/ 
Download : https://desktop.github.com/
Source Tree (圖像化的git介面) : https://www.sourcetreeapp.com/


安裝完之後會有Git bash，可以用來執行git相關的指令

### New repository

到Github的首頁(https://github.com/)，辦好帳號之後點選new repository
![](https://i.imgur.com/7jGPAsm.png)

![](https://i.imgur.com/avlDho6.png)

* Repository name : Project的名字
* Public / Private : 公開與否，一般的帳號只能選公開

Create後會跑出預設的專案，這時裡面沒有程式碼。
![](https://i.imgur.com/R8pmz3q.png)

這時打開Source tree，點選clone，這邊的clone意思為git clone這個指令:

* 第一個欄位填github連結
* 第二個欄位填要把這個專案連結到本機的哪個資料夾
* 第三個為project名

![](https://i.imgur.com/VoMEGma.png)

clone成功後會看到這個畫面:
![](https://i.imgur.com/b6bPvxz.png)

為了完全理解git的運作，我們後面的操作使用terminal來做操作，會搭配使用source tree來看目前git project的狀態，直接使用source tree也能操作，但因為source tree省略了很多基本步驟，不易學習清楚，所以先使用terminal。

### 上傳程式碼

上傳程式碼分為三個步驟:
* git add : 將新增的檔案加入git版本控制
* git commit : 將這次的修改提交，git commit之後會得到一個版本號，可以使用此版本號回朔及追蹤
* git push : 將local端的commit更新到server上，一定要所有有修改的檔案都git commit之後才可以push

其他常用的指令
* git status : 查看目前git的狀態
* git log : 查看目前的commit紀錄
* git pull : 將git server上更新的紀錄抓回來(當local端版本較server端舊時)

### 小練習

在剛剛創建的git資料夾中，新增一個.txt檔並上傳到github。

### 小練習2

把最近練習的小練習在Virtual Box上上傳到github(所有小練習用一個github project)，並將github連結傳給我。

* Linux安裝git : https://git-scm.com/book/zh-tw/v2/%E9%96%8B%E5%A7%8B-Git-%E5%AE%89%E8%A3%9D%E6%95%99%E5%AD%B8
* Linux git clone ssh設定 : https://blog.jaycetyle.com/2018/02/github-ssh/

#### Requirement

* Commit時養成好習慣，將commit的code做了甚麼事、改了甚麼寫在commit message。


