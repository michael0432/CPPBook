# Dijkstra's Algorithm
###### tags = `Data Structure`

### Dijkstra's Algorithm
* 條件：用於所有weight>=0
* 時間複雜度：取決於Priorty queue的複雜度

演算法：
根據Bellmon-Ford及Path-relaxation property，我們可以藉由對所有的edge relax |V|-1次找到最短路徑。當所有的weight都>=0的時候，每多走一個edge，累積的weight一定越大。
舉例來說path 0->1->2 跟 path 0->1，前者的距離一定比較遠，因此，若每次都挑distance最小的點先relax，就可以找到最短路徑。（因為此點的distance一定是最短距離，不可能更短了）

* 將所有點放進Prioty queue，並預設好distance及predecessor，distance越小，prioty越大
* 重複以下步驟：
    * relax所有pq.front()為出發點的邊
    * pq.pop()
    * 更新prioty
* 直到Priority queue為空的

### Dijkstra's Algorithm Example

#### Graph
![](https://i.imgur.com/QM5MkYS.png)

#### 第一次取relax
* 取出pq.front() : v0
* 對v0出發的邊relax()
* pq.pop()
* 更新priorty
![](https://i.imgur.com/Z4AFTne.png)

#### 第二次取relax
* 取出pq.front() : v5
* 對v5出發的邊relax()
* pq.pop()
* 更新priorty

![](https://i.imgur.com/Qg4OO8E.png)

... 以此類推

#### 第三次取relax
![](https://i.imgur.com/W3zLVBm.png)

#### 第四次取relax
![](https://i.imgur.com/OOlCqfJ.png)

#### 第五次取relax
![](https://i.imgur.com/hD9YaTa.png)

#### 第六次取relax
![](https://i.imgur.com/H0zAJRH.png)


### 小練習
* https://vjudge.net/problem/UVA-929
* https://vjudge.net/problem/UVA-1112
* https://leetcode.com/problems/network-delay-time/

### Reference
* http://alrightchiu.github.io/SecondRound/single-source-shortest-pathdijkstras-algorithm.html


