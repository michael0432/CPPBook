# Stack
###### tags = `Data Structure`

Stack是一種Last-in-First-Out的資料結構，最晚進入stack的資料會最先被拿出來。
![](https://i.imgur.com/Xg3IkGP.png)

Stack最主要的功能是「記得先前的資訊」，通常用來「回復到先前的狀態」(Back-tracking)

Stack有幾個基本的method：
* push()
* pop()
* isempty()
* top()
* size()

### 用Array實作Stack


使用array來實作stack時：
* top()
    * 紀錄目前stack的最後一筆資料在array的哪個位置
    * return array[top]
* push()
    * 將一筆資料放進array[top]
    * 若array大小不夠，重新建立一個大小更大的array
* pop()
    * top--
* isempty()
    * 看top是否為0
* size()
    * return top

### 用Linked-List實作Stack

用Linked-List來實作stack時：
* top()
    * Linklist的first（第一個node）
* push()
    * 將新的node指向原本的第一個node，並將第一個node改為此node
* pop()
    * 將first->next改為第一個node，移除原本的first
* isempty()
    * 確認first是否為null
* size()
    * 整個linked-list的size

### 小練習
* https://www.hackerrank.com/challenges/maximum-element/problem
* https://leetcode.com/problems/valid-parentheses/
* https://leetcode.com/problems/backspace-string-compare/
* https://leetcode.com/problems/reverse-substrings-between-each-pair-of-parentheses/
