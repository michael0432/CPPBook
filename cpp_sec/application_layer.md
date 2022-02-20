# Application Layer Introduction
Application Layer提供應用程式(Process間)交流的Protocal，用來設定兩個或多個應用程式之間的通訊。常見的架構有兩種：Server-Client架構以及P2P架構。

## Server-Client架構
Server-Client架構是現今最常見的架構，主動提出Request的一方稱為Client端（提出連線、資料傳送的一方）；被動回復Response的一方稱為Server端。
![](https://i.imgur.com/QUJeqrr.png)

通常Server端會在機器上根據不同的功能使用不同的port，讓Client端可以根據其需求及想走的協定發Requeset到對應的port。

### Scalability
Server-Client架構的第一個棘手的問題是可擴充性問題，當Client端的數目逐漸增加，Server的負擔會加重，且每台Server單位時間可以處理的Request數量是有限的。現在的Server靠著分散式系統解決Scalability的問題。

### Reliability
第二個問題是可靠度的問題，可靠度指的是系統的穩定度及資料是否會遺失等等。由當Server錯誤或中斷時，會導致整個Client-Server架構停擺，現在的Server一樣靠著分散式系統解決Reliability的問題。

## P2P架構
P2P(Peer-to-peer)是一種點對點的架構，各個連線裝置之間並沒有主從關係，每個裝置可以主動地發送資料、也可以主動地接收資料。
![](https://i.imgur.com/gWytlzz.png)

由於每台裝置都同時扮演Client及Server的角色，就不容易碰到Scalability跟Reliability的問題，因為Client的增加同時也意味著Server的增加；一台機器的錯誤也不會導致整個P2P網路的錯誤。

然而，由於P2P沒有一個中心化的管理，因此會有很多資訊安全相關的問題需要處理。此外，由於資料可能會重複存在不同台設備中、也有可能只存在少量的設備中，使得資料被重複儲存浪費空間及資料能否永久保存都充滿不確定性。

