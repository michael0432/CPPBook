# Network Introduction
###### tags = `Network`

網路是一套將End system互相連結的系統(服務)，這些End system透過一些標準的網路協定進行資訊的串流。

## 連結的型式
網路由兩個或多個設備所構成，我們可以把網路想成把設備互相串連起來的線路，為了讓設備之間可以互相連結，設備間一定要有某種方式的連結，讓其可以接到另一個設備。

### Point to point
Point to point是最簡單可以將兩個設備連結的方式，只要兩個設備設定好同樣的傳輸協定(protocal)，就可以傳輸資料，例如使用USB將兩個設備連結，即是一種point to point的連結。

### Multi-point
Multi-point connection為多個設備互相連結的方式，將兩點之間用連線連起來，最終，這些設備間的連線就會變成一個Graph，我們稱為Topology。
![](https://i.imgur.com/0B6Y2Tw.png)

Topology可以是一個full-connected的graph，也可以不是。當Topology邊的數量足夠多時，就可以讓整個網路的架構比較穩定，不會因為其中任何一台設備損毀或故障，導致所有裝置間的資料傳遞出現問題。

除了將設備作為Node的Topology之外，另一種常使用的連結形式為Bus topology。Bus topology的概念為建立一條主幹，將需要串連的設備都連起來，並且透過訊號跟Bus上不同的裝置溝通，例如I2C就是Bus topology的例子。

![](https://i.imgur.com/mdG9whZ.png)

Bus topology的優點是架構簡單，因此常用來做不複雜的資料傳輸，但是，只要Bus上有任何地方損壞，就會導致所有的設備都無法溝通。

在網路的架構中，我們會根據不同的使用情況選擇不同的連結方式。

## LAN/WAN
### LAN(Local Area Network)
LAN通常為一個私有的內網，在同一個LAN中的設備最終會連到統一對外的Router，在過去，同一個LAN上的機器都會連結在同一個網路線或纜線上，所以每台設備都會收到其他同個LAN上設備傳出來的封包(Bus topology)，每台設備再根據自身的需求決定是否忽略收到的封包。之後，Router的出現讓封包可以帶上IP及Port等資訊，用於辨識封包的目的地。

![](https://i.imgur.com/oyfTXpt.png)

### WAN(Wide Area Network)
WAN跟LAN都是將具有通訊能力的設備相連，不同的是，WAN連接的是Switch/Router/Hub等設備，將不同地區的LAN相連，通常會跨越很大的物理範圍。在現今的網路架構中，WAN的架構通常為一個很大的Topology(Graph)。

![](https://i.imgur.com/2xvNbxv.png)
