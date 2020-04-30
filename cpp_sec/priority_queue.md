# Priority Queue
###### tags = `Data Structure`

Priority Queue是一種「先取出最重要資料」的queue。
* Priority : 資料的優先度（重要度）

Priority queue分為兩種：Max priority queue / Min priority queue，分別為優先取出權重最重的資料即取出權重最輕的資料。

以Max priority queue為例．需要三個最基本的操作：
* Delete : 取得最重要的資料並將其從queue中刪除
* Insert : 將一筆資料放進queue中
* Update Priority : 更新資料的優先程度

![](https://i.imgur.com/NSZWlu9.png)

### 用Max heap實現priority queue
我們會發現，上述需要的三個基本的操作都可以對應到max heap的操作：
* Delete : 將heap的root刪除，並將heap更新（與heap sort的方法相同）
* Insert : 將一筆資料放進heap的最尾端（complete binary tree的最尾端），並對其所有的祖先做heapify
* Update Priority : 
    * 將一個node的優先程度提高：更新該node的優先度並對該node所有的祖先做heapify
    * 將一個node的優先程度降低：更新該node的優先度並對該node的所有子孫做heapify

### 小練習
* https://leetcode.com/problems/last-stone-weight/
* https://leetcode.com/problems/sort-characters-by-frequency/