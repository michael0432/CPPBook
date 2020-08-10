# Maximum Flow Problem
###### tags = `Algorithm`

在Maximum Flow的問題中，我們把有向圖看成一個網路架構，每一條邊(每段線路)都有自己的流量上限，並且在圖中有一個起始點(source, 源頭)及一個結束點(termination, 終點)。這種類型的圖稱為Flow Network。

## Flow Network

Flow Network是一個weighted, directed graph，且每個邊的weighted為非負，用來表示邊的流量上限(capacity)。

![](https://i.imgur.com/zYlUX8X.png)


* capacity: Edge(X,Y)的weighted，表示為c(X,Y)
    * 若Edge(X,Y)不存在，則c(X,Y) = 0
* source: flow的源頭，可以產生無限單位數量的flow
* termination: flow的終點，可以接收無限單位數量的flow
* flow: 從Vertex(X)流向Vertex(Y)的flow，表示為f(X,Y)
    * Capacity constraint: 從Vertex(X)流向Vertex(Y)的flow，不能比c(X,Y)大，即c(X,Y) >= f(X,Y)。
    * Skew symmetry: 若f(X,Y) = 5，意思為從Vertex(X)流向Vertex(Y)的flow為5單位，也等價於意思為從Vertex(Y)流向Vertex(X)的flow為-5單位，即f(Y,X) = -5。
    * Flow conservation: 在Graph中除了source跟termination兩點之外，每個Vertex流進的flow要等於流出的flow，也就是flow不會無故的增加或減少。

![](https://i.imgur.com/f1lYFob.png)
