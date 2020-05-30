# Bellman-Ford Algorithm
###### tags = `Data Structure`

### Bellman-Ford Algorithm

* 只要Graph中沒有negative cycle就可以用！
* 時間複雜度 : O(VE)

根據Path-Relaxation Property，Bellman-Ford Algorithm：

* 執行|V| - 1次迴圈
* 每次迴圈對所有edge進行一次relax
* 經過|V| - 1次後，必定有「按照最短路徑上之edge順序的Relax()順序」（因為從起點走到任一vertex之最短路徑，最多只會有|V|−1條edge）

### Bellman-Ford Algorithm Example

#### 第一次迴圈

* relax(0,1)
![](https://i.imgur.com/2iSfVEN.png)

* relax(1,2), relax(1,4)
![](https://i.imgur.com/bbZlIgO.png)

* relax(2,4), relax(2,5)
![](https://i.imgur.com/ctC0V3d.png)

* relax(3,2)
![](https://i.imgur.com/z4N6tdH.png)

* relax(4,3), relax(4,5)
![](https://i.imgur.com/ZZDqETo.png)

#### 第二次迴圈以此類推....直到做到第|V|−1次迴圈

![](https://i.imgur.com/6i9Ko52.png)

#### 小練習
* https://leetcode.com/problems/cheapest-flights-within-k-stops/
* https://leetcode.com/problems/network-delay-time/