# Unicast Routing
###### tags = `Network`
Unicast routing指的是資料只將資料送往一個目的地(一對一傳播)，Routing主要會討論兩個主題:
* Routing algorithm: 如何決定Packet透過哪些Router、以哪條路徑進行傳輸
* Unicast Routing Protocal: 定義unicast routing在internet layer中的幾個協定

## Routing algorithm
在routing的過程中，由於封包依賴網路中的無數個Router進行forwarding，因此，routing algorithm有兩個主要的目的:
* 讓每個router都有一個forwarding table，知道收到packet後要forwarding給哪個router
* 盡量讓packet最快送達到目的

一個由router組成的網路架構可以被看做一個weighted graph，每個node為router；每個edge上的cost為router到router之間所要花費的成本或代價(這邊的cost可以想成花費的時間、或是花費的系統效能)，當兩個router之間不相連，其cost為無限大。

![](https://i.imgur.com/UqpKskn.png)

### Least Cost Tree (Minimun Spanning Tree)
由於每個router無法決定其他router會怎麼傳送packet，因此，最好的做法是在每個router中存一個table，這個table表示此router到其他所有router的最短路徑，以及要轉傳給哪個router才能走到這個最短路徑。
以Router B為例:

| Destination Router | Output Router | Distance |
| -------- | -------- | -------- |
| A     | A     | 2     |
| B     | B     | 0     |
| C     | C     | 5     |
| D     | A     | 5     |
| E     | E     | 4     |
| F     | E     | 6     |
| G     | E     | 7     |

也就是說，Router algorithm的最終目的其實是幫讓每一個router，以自己為出發點，建一個Least Cost Tree (Minimum Spanning Tree)，這個Tree表示到每個Node的最短距離(Cost最小的距離)。
以Router B為例:
![](https://i.imgur.com/LjrQ7G6.png)

我們可以透過之前介紹過的Bellman-Ford algorithm或Dijkstra algorithm來找到每個Router的Least Cost Tree並建立出forwarding table。

## Unicast Routing Protocal
在現今的網路架構中，整個網路是由非常多的router所構成，如果單單使用一種協定決定routing，會有兩個問題:
* 擴充性問題: 每個router的forwarding table過大，導致搜尋耗時且難以更新
* 管理問題: 每一個router可能隸屬於不同的ISP，ISP擁有對系統的控制權

![](https://i.imgur.com/dZHpFgo.png)

現今的網路架構由大至小由三個階層組成: Backbone、Provider Network、Customer Network，每個ISP管理不同階層或同個階層的不同系統服務，這些階層式的每個服務被稱為Autonomous System(AS)。在Unicast Routing Protocal中，有三個主要的協定: RIP、OSPF及BGP，其中，RIP及OSPF用於AS內部，BGP用於AS之間。

* Stub AS : Stub AS只有一條線連到其他的AS，Stub AS只會是資料的來源或是終點，不能協助轉傳其他資料，當Customer Network只使用一個Provider Network時，其為Stub AS。
* Multihomed AS : 有兩條以上的資料連線到其他AS，但不允許協助轉傳資料，當Customer Network只使用兩個以上的Provider Network時，其為Multihomed AS。
* Transient AS : 連結兩個以上的AS，負責轉傳資料。

### RIP(Routing Information Protocal)
RIP協定利用hop來定義cost，意思是，如果從router到目的地需要一次forwarding，cost為1；需要兩次forwarding，cost為2，因此，可以想成是上述的weighted graph中，只要是彼此相連的router，連結兩個router的邊cost都為1。
![](https://i.imgur.com/FwrOQLq.png)

在RIP協定中，Routing table由三個欄位組成: Destination Network(目的地)、Next Router、Cost，並藉由之前提過的最短路徑演算法來更新cost，要注意的是，當更新forwarding table時，router會傳送整個forwarding table給鄰近的router，因此，RIP協定通常用在比較小型的AS內部，且會定義一個cost的上限值，例如cost超過15視為無限。
![](https://i.imgur.com/ETgQJyb.png)
![](https://i.imgur.com/M999j8j.png)
![](https://i.imgur.com/aW6Dc5p.png)

RIP format:
![](https://i.imgur.com/86iClHw.png)

### OSPF(Open Shortest Path First)
RIP是為了小型的AS所設計；而OSPF則是為了中型的AS所設計，OSPF與RIP使用相同的Routing algorithm，唯一不同的是，其cost由多個變數所組成，可能包含傳輸量、傳輸來回時間等等來計算，不同的AS可以設計不同的計算策略。但只有cost計算的改變並不能解決router過多時更新forwarding table太慢等問題，因此OSPF定義了階層式的概念。
![](https://i.imgur.com/hxEoKBN.png)

如此一來，每一個Router的forwarding table只需要知道及更新自己區域內相關的router以及area border router，可以大幅降低更新forwarding table所需要花的時間。

### Format
![](https://i.imgur.com/2mQbj1n.png)
* Type:
    * Hello : 讓Router跟相鄰的Router建立關係
    * Database Description : 回應Hello訊息，讓新加入的Router取得transfer table
    * Link-state Request : 向相鄰的router要求某些cost或連結狀態的資訊
    * Link-state Update : 回應Link-state Request
    * Link-state ack : 確認每一個訊息都有收到回應

### BGP(Border Gateway Protocal)