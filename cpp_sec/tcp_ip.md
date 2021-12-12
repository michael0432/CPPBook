# TCP IP & OSI 7 Layer
###### tags = `Network`

在網路通訊的協定(Protocal)中，將網路模型分成多個層次，每個層次各司其職。
在OSI模型中，將網路架構分成七層:
* Application
* Presentation
* Session
* Transport
* Internet
* Data-link
* Physical

在TCP/IP協定中，將網路架構分為四層:
* Application
* Transport
* Internet
* Network Access

![](https://i.imgur.com/dx50HVV.png)

不論是在OSI還是TCP/IP架構上，每層都有對應的協定(Protocal)

## Why protocal?

試想一個情境，在一個系統中，若兩個Process間要互相溝通:
![](https://i.imgur.com/n3Assmk.png)
在這樣的情境下，雙方需要定義他們傳輸資料的格式與順序，才能讓Sender跟Receiver正確收到資訊並解讀資訊。

如果這兩個Process現在被放在不同系統上，需要跨系統的交流，並且在傳輸資訊時，需要經過加解密，並且透過一個中介系統協助他們轉傳資料。
![](https://i.imgur.com/BI7mtNq.png)

我們可以把加解密跟傳輸相關的設計都寫在Sender跟Receiver內，但是，當這個架構越來越複雜時(例如加解密的演算法要變複雜)，會讓所有的程式碼都寫在Sender跟Receiver裡難以維護，除此之外，當有更多的Sender跟Receiver需要傳遞資料時，就必須重新實作一次整個傳輸的流程，因此，我們將傳輸過程中所需的流程模組化，被模組化的區塊變成一個黑盒子，每層模組之間只需要知道如何跟其他模組溝通，而不需要知道其細節。

在分層協定(Layer Protocal)的概念中，每層都會需要能夠執行兩個相對的動作(例如加密跟解密)且對應分層的兩端必須相同，這樣才能進行雙向的溝通。

## TCP/IP
### Application
Application Layer為兩個應用之間互相的溝通，通常是在兩個Process之間的溝通，不同類型的Process會透過不同的協定進行溝通，例如HTTP協定為存取網頁的協定；FTP協定為傳輸檔案的協定，這些Application層的協定會定義Process間要用哪種格式來解讀傳輸的資料。

### Transport
Transport層負責將Application層拿到的訊息，封裝成一個一個的封包(Packet)並送出去，接收端接收到封包後，再根據協定將封包合併成Application層看得懂的格式，TCP協定是主要的Transport層協定之一。

### Internet
Internet層的通訊為主機對主機的連線，這裡的主機到主機不只代表傳輸者與接收者所在的兩個裝置上，而是包含了所有傳輸上會經過的Router、Hub等等，Internet層的主要協定為IP，它定義了Internet層的封包格式，藉由此格式決定Packet應該如何在Router之間流通。

### Network Access
Network Access層用bit為單位傳輸資料，有許多協定能將位元轉換成訊號，除此之外Network Access層不一定為有線的連結，連結方式大致上可以分為四種:有線LAN、無線LAN、有線WAN、無線WAN，可以根據不同的連結方式定義不同的協定(例如WIFI就有很多不同代的協定)。

![](https://i.imgur.com/KT2KFU5.png)

## OSI (Open System Interconnection Model)
OSI比TCP/IP協定晚提出，但結構相較於TCP/IP協定完整，在OSI協定中，將網路架構分為七層，但由於當時網路架構已經根據TCP/IP協定架設及運行，若改為使用OSI協定，耗費的成本過高，因此，OSI模型目前是一種理論上的架構，可以用OSI協定來學習網路架構的概念，但實際上目前網路世界的運作還是根據TCP/IP協定。