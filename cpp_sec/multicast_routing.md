# Multicast Routing
###### tags = `Network`

除了Unicast routing之外，近代的網路通訊也越來越常使用到multicast routing，multicast routing為一個來源傳遞封包到多個目的地，雖然可以用unicast routing做到一樣的目的，但unitcast routing會有兩個缺點:
* unicast同樣的資料到不同目的地較沒效率
* 假設有大量目的地(例如1000個)，來源端傳出第一個封包到最後一個封包的時間可能會有延遲

![](https://i.imgur.com/G8WphyK.png)

## Routing algorithm
在Unicast routing中，發送封包跟接收封包者各有一個代表自己的IP Address，在multicast routing中，雖然接收者有很多，但為了符合IP的定義，接收者必須共用一個IP，這個IP我們稱為multicast address，用這個address可以表達一個群體，因此，在同一個群體內的機器除了代表自己的unicast IP address之外，還有另一個multicast IP address，因此，n台機器總共會有n+1個address。

Router必須分辨IP Address是Unicast address還是Multicast address，因此，特別保留了224.0.0.0/4(224.0.0.0 ~ 239.255.255.255)作為Multicast address。

如果multicast的目的群體在同一個AS下(例如學校內的線上課程、公司內的直播會議等等)，AS管理者可以利用AS號碼(x,y)選取一段沒被使用的address 239.x.y.0/24作為multicast使用。

如果multicast的目的群體在不同的AS下，只能使用232.0.0.0/8這個範圍的address使用。

### Multicast Forwarding
Multicast與unicast forwarding最不同的是封包會從很多不同條的路徑被傳送出去>
* 對unicast來說，只需要知道封包要傳送到哪邊，不需要知道來源
* 對multicast，需要知道封包的來源，避免傳送重複的封包到同一個目的地。

以下我們假設整個網路上有n個router以及m個群體。
### Source-based Tree
Source-based Tree的方式中，每一個router都會存取m*n個tree，代表當來源為n且目的地為m群體時的routing路徑，並用跟unicast routing類似的方式找出最短路徑。

### Group-shared Tree
Group-shared Tree的方式中，對於每一個群體，找出一個router代表這個群體的root，每個要傳到這個群體的封包都要先傳到這個root，再由root決定如何發送給群體中的其他router。此時，整個multicast routing的傳播過程會分成兩段，第一段為unicast傳送到群體的root router；第二段為從root router傳送到其他成員的multicast routing。

![](https://i.imgur.com/QBaEqmr.png)

## Multicast Routing Protocal