# Graph

###### tags = `Data Structure`

在Computer Science中，Graph指的是紀錄關聯性的資料結構，Graph由以下兩個元素組成：
* Vertex(Node) 點
* Edge 邊
![](https://i.imgur.com/mgZ9lpO.png)

Vertex間可以以很多條邊相連，也可以將自己與自己相連。
Graph最常見的儲存方式有兩種：Edge List及Adjacency Matrix

### Derected Graph 
邊擁有方向性的Graph。
![](https://i.imgur.com/rzzLtuN.png)


### Weighting
邊有權重的Graph
![](https://i.imgur.com/jRRPFbF.png)

### Edge List
Edge List就是將每個Edge是將哪兩個點相連記錄下來，以下圖為例。
* 好處：節省空間
* 壞處：不易計算

|  Edge | vertex1 | vertex2 |
| -------- | -------- | -------- |
|  0 | A | C |
|  1 | C | D |
|  2 | D | B |
|  3 | B | E |
|  4 | E | E |
|  5 | E | F |
|  6 | B | F |
|  7 | C | F |

![](https://i.imgur.com/mgZ9lpO.png)

```cpp=1
struct Edge{
    char vertex1;
    char vertex2;
}
Edge edge[8];
```
### Adjacency Matrix
把一張圖上的Vertex編號後建立一個2D的Matrix，紀錄連接的資訊。
* 優點：查找方便、計算簡單
* 缺點：用比較多空間、無法表示同兩個Vertex間有多個邊

將上圖用Adjacency Matrix表示：

|   | A | B | C | D | E | F |
| -------- | -------- | -------- | -------- | -------- | -------- | -------- |
| A | X | X | O | X | X | X |
| B | X | X | O | X | X | X |
| C | O | X | X | O | X | O |
| D | X | O | O | X | X | X |
| E | X | O | X | X | O | O |
| F | X | O | O | X | O | X |
