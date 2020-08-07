# Queue
###### tags = `Data Structure`

Queue是一種First-In-First-Out的資料結構，就像是日常生活中的排隊，排在前面的人可以先買到東西然後離開。

![](https://i.imgur.com/C0dvAbq.png)

* Enqueue : 將資料從後方(back)放進queue中
* Dequeue : 將資料從前方(front)取出

因此Queue有幾個基本的method：
* Push : 將資料從後方(back)放進queue中
* Pop : 將資料從前方(front)取出
* getFront : 回傳front的資料
* getBack : 回傳back的資料
* isempty : 檢查queue是否為空
* getSize : 回傳queue的大小


### 用linked list實作Queue

用linked list實作queue很簡單，只需要記住幾個資訊：開頭的pointer、結尾的pointer、size
![](https://i.imgur.com/6BeOeza.png)


* Push : 將新的資料放到list的尾巴，並更新結尾的pointer位址，更新size
* Pop : 將開頭的pointer移除，並更新開頭的pointer位址，更新size
* getFront : 回傳開頭的pointer存的值
* getBack : 回傳結尾的pointer存的值
* isempty : 看開頭的pointer是否為null
* getSize : 回傳size

### 用Array實作Queue 

用Array實作queue有兩種方式:
* Sequential queue
* Circular queue

#### Sequential queue
Sequential queue是比較浪費記憶體空間的實作方式

![](https://i.imgur.com/AQc2sLG.png)


* 以front記住開頭的index
* 以rear記住尾端的index
* Push : 將新的資料放進array後方
* Pop : 將開頭的資料移除
* empty : front == rear
* getSize : rear - front

#### Circular queue
Circular queue用環狀的概念讓資料可以繞回Array的前端
![](https://i.imgur.com/vzNyXYa.png)

* 以front記住開頭的index
* 以rear記住尾端的index
* Push : 將新的資料放進array後方，若array被放滿且前方還有空間，則依序放入
* Pop : 將front的資料移除
* empty : front == rear
* getSize : 
    * if front > rear : total_size - (front - rear)
    * else : rear - front

### 小練習
* https://leetcode.com/problems/lemonade-change/



