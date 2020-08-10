# Ford-Fulkerson Algorithm 
###### tags = `Algorithm`

## Residual Network(剩餘網路)

Residual Network也是一個directed graph，紀錄graph上的edge還有多少剩餘的容量可以讓flow通過，用residual capacity取代原本圖中的capacity。
若edge(X,Y)有flow f(X,Y)流過:

* c_f(X,Y) = c(X,Y) - f(X,Y)，表示此邊還能容納多少流量。

![](https://i.imgur.com/C6a36uL.png)

除了記錄剩餘容量之外，Residual Networks上會產生一條與flow相反方向的residual capacity。
舉例來說:
* f(A,C) = 6
* c_f(A,C) = c(A,C) - f(A,C) = 2
* 根據Skew symmetry f(C,A) = -f(A,C) = -6
* c_f(C,A) = c(C,A) - f(C,A) = 0 - (-6) = 6

因此，需要在Residual Networks上畫出c_f(C,A) = 6，即紅色的邊，意義上來說表示從C到A還有6單位的容量可以走。

若現在再新增一個flow，路徑為S->C->A->B->T，流量為2。

![](https://i.imgur.com/a3YU0TN.png)

## Augmenting Path
在Residual Networks中，所有能夠從source走到termination的路徑，就是Augmenting Path，每條augmenting path都能使總流量增加。

![](https://i.imgur.com/TfleBSo.png)

以上圖來說，有很多條Augmenting Path:

* S->A->B->T，流量為3單位
* S->C->B->D->T，流量為2單位

![](https://i.imgur.com/mHeSBTQ.png)


## Ford-Fulkerson Algorithm

Ford-Fulkerson Algorithm的核心想法為:

* 在Residual Networks上找出augmenting path
* path上剩餘流量(residual capacity)最低的邊為flow的流量，將此流量加入總流量
* 更新Residual Networks
* 直到找不到augmenting path為止

### Example

* 原圖及初始化的Residual Networks

![](https://i.imgur.com/xLKTMhB.png)

* Step 1: 在Residual Networks上用BFS找到能夠從S走到T的路徑，因為使用BFS找，因此會先找到edge數最少的路徑。
    * 找到S->A->B->T
    * flow為3
    * 更新Residual Networks

![](https://i.imgur.com/O0HKl1z.png)

* Step 2: 在Residual Networks上用BFS找到能夠從S走到T的路徑，因為使用BFS找，因此會先找到edge數最少的路徑。
    * S->C->D->T
    * flow為7
    * 更新Residual Networks

![](https://i.imgur.com/eU4LmQG.png)

* Step 3: 在Residual Networks上用BFS找到能夠從S走到T的路徑，因為使用BFS找，因此會先找到edge數最少的路徑。
    * S->C->B->T
    * flow為2
    * 更新Residual Networks

![](https://i.imgur.com/1VJlGt9.png)


* Step 4: 在Residual Networks上用BFS找到能夠從S走到T的路徑，因為使用BFS找，因此會先找到edge數最少的路徑。
    * S->A->C->B->T
    * flow為4
    * 更新Residual Networks
    
![](https://i.imgur.com/YWeqVpC.png)

* Step 5: 在Residual Networks上用BFS找到能夠從S走到T的路徑，因為使用BFS找，因此會先找到edge數最少的路徑。
    * S->A->C->B->D->T
    * flow為1
    * 更新Residual Networks

![](https://i.imgur.com/7oqwJD0.png)

### 小練習

* https://vjudge.net/problem/UVA-820