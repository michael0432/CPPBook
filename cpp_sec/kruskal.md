# Kruskal's Algorithm
###### tags = `Algorithm`

Kruskal Algorithm利用Greedy的概念找出Minimum Spanning Tree:
* 從weight最小的邊開始選
* 如果選了Edge e後，會產生cycle，則跳過此邊不選
* 由小到大考慮完所有的邊後結束演算法

![](https://i.imgur.com/K2iWeGO.png)

要實現Kruskal Algorithm，最主要的問題是如何判斷選了某條邊後會形成Cycle?
最簡單的方式是將目前看過的點依照哪些點互相相連存下來，相連的點放在同一個set中，若目前判斷的邊Edge e的兩個端點v1及v2為同一個set，則加入Edge e後會形成cycle。

### Example

由於每個vertex只會存在一個set中，因此可以使用disjoint set資料結構。

* Step 1 : 初始狀態，沒有任何一點被選

![](https://i.imgur.com/9EGufDN.png)


| Vertex | 0 | 1 | 2 | 3 | 4 | 5 | 6 |
| -------- | -------- | -------- | -------- | -------- | -------- | -------- | -------- |
| Set | 0 | 1 | 2 | 3 | 4 | 5 | 6 |


* Step 2 : 先選weight最小的邊，並將兩端點放到同一個set

![](https://i.imgur.com/XNkFa9b.png)

| Vertex | 0 | 1 | 2 | 3 | 4 | 5 | 6 |
| -------- | -------- | -------- | -------- | -------- | -------- | -------- | -------- |
| Set | 0 | 1 | 2 | 3 | 1 | 5 | 6 |

* Step 3 : 選weight第二小的邊，並將兩端點放到同一個set

![](https://i.imgur.com/aP6KFM9.png)

| Vertex | 0 | 1 | 2 | 3 | 4 | 5 | 6 |
| -------- | -------- | -------- | -------- | -------- | -------- | -------- | -------- |
| Set | 0 | 1 | 2 | 3 | 1 | 5 | 1 |

* Step 4 : 選weight第三小的邊，並將兩端點放到同一個set

![](https://i.imgur.com/vxV5Ya9.png)

| Vertex | 0 | 1 | 2 | 3 | 4 | 5 | 6 |
| -------- | -------- | -------- | -------- | -------- | -------- | -------- | -------- |
| Set | 0 | 1 | 2 | 3 | 1 | 0 | 1 |

* Step 4 : 選weight第四小的邊，並將兩端點放到同一個set，發現兩端點(1,6)原本就是同一個set，因此會形成cycle，此邊不能加入

![](https://i.imgur.com/SBwVYa7.png)


| Vertex | 0 | 1 | 2 | 3 | 4 | 5 | 6 |
| -------- | -------- | -------- | -------- | -------- | -------- | -------- | -------- |
| Set | 0 | 1 | 2 | 3 | 1 | 0 | 1 |

...以此類推能得到最後的結果

![](https://i.imgur.com/HHKJ4Lm.png)

| Vertex | 0 | 1 | 2 | 3 | 4 | 5 | 6 |
| -------- | -------- | -------- | -------- | -------- | -------- | -------- | -------- |
| Set | 0 | 0 | 0 | 0 | 0 | 0 | 0 |

### 小練習

* https://vjudge.net/problem/UVA-11228
* https://vjudge.net/problem/UVA-11631
