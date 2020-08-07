# Heap
###### tags = `Data Structure`

Heap是一個「堆」的資料結構，是一種特別的樹狀結構，在C++之中並不是一個內建的STL。

以下以最基本的Binary heap當做例子，Binary heap有以下的特性：
* 每個node有兩個Children
* 為complete binary tree(每一層從左到右不可以有跳過的Node)
    * 原因：容易找parent-child的關係
    * 假設將這個tree用vector表示，則index為i的node：
        * 其left child必定位在index(2i)；
        * 其right child必定位在index(2i+1)；
        * 其parent必定位在index(⌊i/2⌋)。

![](https://i.imgur.com/NSZWlu9.png)

* Max Heap : 
    * Max Heap的限制為：每個Node都要比自己的child大，上圖即為Max Heap


* Min Heap
     * Min Heap的限制為：每個Node都要比自己的child小。

### Heap的基本操作
以Max heap為例，heap會需要幾個基本的操作：
* Heapify:
    * 目的：確保一個node符合max heap的條件
    * 比較此node與其child，將最小的node變為root

* Build heap:
    * 目的：確保heap為一個max heap(或min heap)
    * 對所有有child的node做heapify

### Heap sort
Heap被發明出來的目的之一是用來作sort使用，假設目前的heap如下圖：

![](https://i.imgur.com/wA3EJy5.png)

Heap Sort的步驟：

    1. 先將最小的node取出
    2. 重建Heap
    3. 重複以上步驟

Example:
    
1. 先將最小的node取出
![](https://i.imgur.com/fHv9OON.png)

2. 重建heap:
    * 將最後一個node放到第一個
    * 做heapify:
        * 比較root與自己的兩個child，找出最小值與root交換
        * 對交換過的subtree做heapify
![](https://i.imgur.com/nmEsFvp.png)
![](https://i.imgur.com/VVbChDh.png)

3. 重複1. 2.兩個步驟直到heap為空

#### 時間複雜度？

* 每次Heapify : O(logn)
* 要做n次heapify : O(n)

時間複雜度為O(nlogn)




