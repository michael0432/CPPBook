# Shortest Path
###### tags = `Data Structure`

### Shortest Path(最短路徑問題)

以最複雜的weighted directed graph為例，weight表示path間的cost，edge有方向性。
Shortest Path的問題為從出發的vertex(source)到終點(destination)所經過最少cost的路徑，著重討論Single-Source Shortest Path問題，即從單一的出發點，抵達Graph中其餘所有vertex之最短路徑。

![](https://i.imgur.com/4i8Roal.png)

* 從v(0)到v(2)，有很多種走法：
    * 試著走最少邊的路徑: 0->1->2，cost為11
    * 但是cost最少的路徑為: 0->1->4->3->2，cost為6
* weight可以有負值！

在解最短路徑問題時，會用到兩個資料記錄：
* distance[]：紀錄從起點走到該點，目前已知的最短距離。
* predecessor[]：紀錄最短距離是由哪個點走到該點的（紀錄上一個點），即可往回找路徑。
![](https://i.imgur.com/U750kD2.png)


**為什麼最短路徑一定不包含cycle?**

### Relaxation
![](https://i.imgur.com/kQLxp9Y.png)
考慮上圖的情況，S為起點，我們想知道S走到X及Y的最短路徑。
* 走到X
    * S->X
* 走到Y
    * S->X->Y
    * S->Y
    * 挑weight和小的

有可能走到Y的點有兩個：S和X
* 從S走來：
    * distance[S] + w(S,Y)
* 從X走來：
    * distance[X] + w(X,Y)

因此，我們可以寫出下列的程式：
```cpp=1
void relax(source_node){
    if(distance[source_node] + w(source_node, Y) < distance[Y]){
        distance[Y] = distance[source_node] + w(source_node, Y);
        // and update predecessor
    }
}

int main(){
    // update y
    relax(S);
    relax(X);
}
```
**以上「比較distance後更新distance(predecessor)的步驟」為Relaxation。**
   
### Relaxation的性質

Graph中的其中3個vertex S,X,Y，其中S為起點，F(S,Y)為S到Y的最短距離。
#### Triangle inequality
若edge(X,Y)存在：

    F(S,Y) <= F(S,X) + W(X,Y)
    
#### Convergence property
* 若edge(X,Y)存在且S到Y的最短距離路線包含edge(X,Y)且relax(X,Y)之前，S到X的最短距離已知。
    * 則relax(X,Y)之後，distance(Y) = F(S,Y)，且distance(Y)不再被更新
    
#### Path-relaxation property
考慮一條從vertex 0到vertex k的路徑P: v0->v1->...vk。
對edge relax()的順序中，如果出現edge(v0,v1), edge(v1,v2) ... edge(vk-1,vk)的順序，distance[k]必定為F(S,K) (最短路徑)。

舉例來說：
* 假設v0到v3的最短路徑為Path : 0-1-2-3
    * 根據Convergence property
        * 若relax(v1,v2)之前，已經relax(v0,v1)，則0-1-2一定是最短路徑
    * 也就是說，只要relax的過程中，曾經出現「relax(v0,v1), relax(v1,v2), relax(v2,v3)」的順序，不論中間是否有其他relax，都不會影響最後的結果。

**小結：所以要找出最短路徑，最直觀的方式就是把所有edge的排列順序都試過一次，就能找到最短路徑。**

### Reference
* http://alrightchiu.github.io/SecondRound/shortest-pathintrojian-jie.html