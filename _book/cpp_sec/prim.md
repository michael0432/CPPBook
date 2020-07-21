# Prim's Algorithm
###### tags = `Algorithm`

Prim's Algorithm一樣使用Greedy的概念找出Minimum Spanning Tree:

* 先選一個點為Tree的root
* 從root出發依序選取cost最小的點加進tree中，為了避免產生cycle，已經被加進tree的點不會再次考慮。

![](https://i.imgur.com/o2M3nyQ.png)

* key[] : 用一維的vector記住目前每個vertex的cost，每次挑選最小的vertex
* visited[] : 記住哪些vertex已經走過了

## Example

![](https://i.imgur.com/h6w4RpE.png)


* Step 1 : 隨便挑一個點當作root，這裡選2作為root
    * key[2]更新為0
    * visted[2] = 1，表示2已經被放進tree中

![](https://i.imgur.com/0br0ay5.png)

* Step 2 : 對2走的到的點做relax
    * key[1] = min(key[1], key[2]+weight(2,1))
    * key[3] = min(key[3], key[2]+weight(2,3))
    * key[6] = min(key[6], key[2]+weight(2,6))

![](https://i.imgur.com/OIu0epf.png)

* Step 3 : 選目前key[]最小的點放進tree
    * visited[3] = 1，表示3已經被放進tree中

![](https://i.imgur.com/6WOAz49.png)

* Step 4 : 對3走的到的點做relax
    * key[4] = min(key[4], key[3]+weight(3,4))
    * key[6] = min(key[6], key[3]+weight(3,6))

![](https://i.imgur.com/qiC016O.png)

* Step 5 : 選目前key[]最小的點放進tree
    * visisted[4] = 1，表示4已經被放進tree中

![](https://i.imgur.com/tPb2azd.png)

...以此類推，最後的結果:

![](https://i.imgur.com/Ric0z6r.png)

### 小練習

* https://vjudge.net/problem/UVA-11228
* https://vjudge.net/problem/UVA-11631