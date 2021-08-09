# Git Advanced
###### tags = `Git`

## Branch

在開發時，可能會同時有多人在開發同一版本的程式碼，因此，同一個版本可能會切割成不同的開發版本，並且需要分開進行維護。
此時，需要使用到branch(分支)的功能，可以基於其中一個commit，延伸出彼此獨立的branch，並在開發完成後再合併。

![](https://i.imgur.com/EhBpeFf.png)

### Master branch

最初的branch(第一個commit的branch)我們稱為master branch，其他所有使用者自己新增的branch都是從master branch拉出的，通常，為了版本控制穩定，我們不會直接在master branch上進行開發，而是從master branch拉出新的branch做開發，開發到一個段落再merge(合併)回master branch。

### Branch指令

* 新增branch
```
git branch <branchname>
```
* 查看branch
```
git branch
```
* 切換branch
```
git checkout <branchname>
```

## Merge

Merge為將兩個branch合併為一個branch，在操作時，要先checkout到要保留的branch，輸入下面的指令指定要merge的commit id(branch上的某個commit)。

```
git merge <commit id>
```

Merge時可能會發生兩個狀況:
* Merge成功
* Merge conflict : 表示要merge的兩個版本有衝突，意思是這兩個branch改到了相同檔案的同一行，因此git不知道這行要用哪個版本，此時需要使用者自行修正。

### Fix conflict

當conflict發生時，打開該檔案可以看到類似下方的畫面:

![](https://i.imgur.com/Rq3Lt8U.png)

<<<< HEAD的部分是指目前所在的commit (master)
=====下方則為branch上的版本

把這兩行合併成想要的樣子後:
![](https://i.imgur.com/6FFxkYl.png)


把衝突的地方修正成最終版本後，便可以重新merge，重新merge前必須要新增一個commit，表示以此commit的版本最終版本。
```
git add 修完conflict的檔案
git commit -m "fix commit"
```

## Pull

在開發時，可能會有其他協作人push了程式碼到git server端，也就是目前有一個比較新版本的程式碼比自己的local端版本還新:

![](https://i.imgur.com/a47I8jn.png)

此時可以使用git pull把Local端的版本從remote端
```
git pull
```

![](https://i.imgur.com/UxG1t6y.png)

pull之後如果沒有conflict，就會把Remote的commit C, D合併到Local端，並且跟原本Local端的修改合併，變成commit C', D' (跟Merge一樣，不會覆蓋掉自己的修改)。

若是發生conflict，跟上方merge branch一樣，需要修正conflict後重新commit，才能成功pull。

## 小練習

在github上練習用的專案新增一個協作者，讓兩人可以共同修改一個專案

![](https://i.imgur.com/CPbznkE.png)

在第一個commit的地方拉出branch(兩人使用不同branch)，各自對一個檔案做修正，並且先後將自己的branch merge回master，並讓github上的版本保留了兩個人的修正。

* 可能會碰到的問題
    * merge時要注意自己的merge是在local端(自己的電腦上)還是有push到git server(github上)
    * 比較晚merge的人要merge到哪個版本的master branch?
* Tips
    * 如果搞不清楚目前git server上的狀態，可以看source tree上，有origin/開頭的brach為remote branch