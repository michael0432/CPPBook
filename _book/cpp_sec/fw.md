# Floyd-Warshall Algorithm
###### tags = `Algorithm`

Bellman-Ford Algorithm跟Dijkstra's Algorithm都是用於解決Single-source Shortest Path的問題，即出發點固定為一個點，求其到其他所有點的最短距離及最短路徑。
Floyd-Warshall Algorithm用於解決All-pairs Shortest Path的問題，即將每個點都視為起點，尋找所有點到其餘所有點的最短路徑。

## 使用Bellman-Ford及Dijkstra

All-pairs Shortest Path的問題最直觀的解法是對所有的點都做一次Single-source Shortest Path，即可得到結果。

時間複雜度(V為點的個數，E為邊的個數)：

|  | Bellman-Ford | Dijkstra |
| -------- | -------- | -------- |
| Single-source     | O(EV)     | O(E+VlogV)     |
| All-pairs     | O(EV^2)     | O(EV+V^2logV)     |

若考慮最壞的情況，即E=V^2，則時間複雜度為：

|  | Bellman-Ford | Dijkstra |
| -------- | -------- | -------- |
| Single-source     | O(V^3)     | O(V^2+VlogV)     |
| All-pairs     | O(V^4)     | O(V^3+V^2logV)     |

有沒有更好的方法呢？

## Floyd-Warshall Algorithm

* 中繼點(intermediate vertex)
    * 考慮從X->Y的最短路徑，是否因為經過了中繼點Z而減少距離
    * 若原先的最短路徑是Path:X->Y，在引入中繼點Z後，最短路徑就有兩種可能，Path:X->Y或者Path:X->Z->Y

![](https://i.imgur.com/pG4mqnP.png)

以上圖為例，B跟C可以做為A->D的中繼點

* 對每組出發點X及終點Y，都存兩個Set:
    * Set K: 存所有的點
        * 初始狀態為存所有的點
    * Set S: 存目前已經計算過的中繼點
        * 初始狀態為空的
        * 集合S的作用是找到「某個vertex(X)走到某個vertex(Y)」的最短路徑，因此，不同的「起點vertex(X)與終點vertex(Y)」之組合，都有各自的集合S

### Example
以A->D為例
* 左圖
    * Set K = {B,C}
    * Set S = {A,D}
    * Shortest path = 8
* 右圖，加入B為中繼點
    * Set K = {C}
    * Set S = {A,B,D}
    * Shortest path = 5
        
![](https://i.imgur.com/TlwtGgf.png)

* 加入C為中繼點
    * Set K = {}
    * Set S = {A,B,C,D}
    * Shortest path = 1
        
![](https://i.imgur.com/jn71gDa.png)


Floyd-Warshall Algorithm：以較小段的最短路徑(subpath)，連結出最終的最短路徑

* 在引入中繼點vertex(k)之前，已經找到「引入中繼點vertex(k-1)後，從vertex(X)走到vertex(Y)」的最短路徑；
* 在引入中繼點vertex(k-1)之前，已經找到「引入中繼點vertex(k-2)後，從vertex(X)走到vertex(Y)」的最短路徑；
...

* 引入中繼點k時有兩種可能:
    * Vertex(k)在X->Y的最短路徑上
        * 最短路徑變成X->...->k->...Y
    * Vertex(k)不在X->Y的最短路徑上
        * 最短路徑維持X->...->Y
* 可以寫成:
    * ShortestPath(X,Y) = min(ShortestPath(X,k)+ShortestPath(k,Y)), k為所有中繼點(即嘗試所有的點)


### Example

![](https://i.imgur.com/1C1ZNmJ.png)

* 引入A為中繼點
    * 對Distance Matrix上所有的點做引入中繼點A（即對所有可以經過A的路徑做relax）
        * Distance[X][Y] = min(Distance[X][A]+Distance[A][Y], Distance[X][Y])
        * Distnace[C][B] = min(Distance[C][A]+Distance[A][B], Distance[C][B])
        * Distnace[C][B] = 4 + 2 = 6
        
![](https://i.imgur.com/mTY0aMF.png)

* 引入B為中繼點
    * Distance[A][C]=Distance[A][B]+Distance[B][C]=0
    * Distance[A][D]=Distance[A][B]+Distance[B][D]=5
    
![](https://i.imgur.com/pr24vJu.png)

* 引入C為中繼點
* 引入D為中繼點

時間複雜度？

### 小練習
* https://vjudge.net/problem/UVA-821
* https://vjudge.net/problem/UVA-10171
* https://vjudge.net/problem/UVA-10463