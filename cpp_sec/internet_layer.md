# Internet Layer Introduction
###### tags = `Network`

![](https://i.imgur.com/KT2KFU5.png)

## Packetizing
Internet Layer負責的工作就像郵局一樣，只會負責封裝上層傳來的資料(信封)，並將其傳到接收者手上再解開封裝，在轉傳的過程中，會經過許多不同層級的Router，經過這些Router時，Router不能解開封裝(郵局、郵差不能拆開信封)，Router只能根據封包的目的地轉傳封包到其他的Router。

## Forwarding
Forwarding定義了封包到達一個Router時，Router需要採取的行動，Router會根據內部的表格，決定要將此封包轉傳給其他一個或多個Router。

![](https://i.imgur.com/zUc6mHZ.png)

## Routing
Routing定義了從來源端到目的地端的最佳路徑，Internet Layer藉由routing algorithm定義了如何選擇最佳的傳遞封包路徑。

## 連線方式
### Connectionless
Connectionless連線方式是比較早期的連線方式，這種連線方式將封包當作獨立的個體，封包之間沒有關聯性，也就是說，Internet Layer只負責將封包送到正確的目的地，但不保證走的路徑完全相同。
![](https://i.imgur.com/cHS9TsL.png)
這種方式會導致接收者收到封包時，不一定會依照封包的正確順序收到，因此，需要在解封包時另外設計邏輯做判斷。

### Connection-oriented
在Connection-oriented連線方式中，相關聯的封包必須用同一個routing路徑傳送，因此，在封包開始傳送之前，需要先建立連線，讓所有的封包都遵循相同的路徑，因此，在Connection-oriented Service中，會有一個stream tag來表示封包是不是屬於同一個資料流。
![](https://i.imgur.com/3VSJYyD.png)

## IPv4
為了辨識每台連結到網路的設備，在Internet Layer定義了IP(Internet Protocal) Address，IPv4則為IP協定的第四版(Internet Protocol version 4)，是目前現行最廣泛使用的IP Address協定。

IPv4由32個bit組成，通常以xxx.xxx.xxx.xxx的格式表示，其中，每個.分開為8個bit。

### 定址
IPv4由兩個部分組成，第一個部分為prefix；第二個部分為suffix，Prefix的用處為定義網路，意思是擁有同一個Prefix的IP為同一個網域下的網路，例如，同一間公司的網路會使用同一個prefix；Suffix的用處為定義終端設備，意思是接在這個網域下的機器。

Prefix的長度不是固定的，若Prefix的長度為n個bit，則suffix的長度為32-n個bit，在過去IPv4使用固定級別的定址，意思是將Prefix的長度定義為幾種:
* A級 : prefix = 8 bit，第一個bit為0，用於大型網路，分配給大型ISP
* B級 : prefix = 16 bit，前兩個bit為10，用於中型網路
* C級 : prefix = 24 bit，前三個bit為110，用於稍小的網路
* D級 : 無prefix，前四個bit為1110，沒有區分Prefix/Suffix
* E級 : 無prefix，前四個bit為1111，保留用

這樣的分級方式有幾個問題，第一個問題就是Address分配的不適當導致浪費許多Address，加速了IPv4 Address耗盡，舉例來說，A級只有128個Prefix，也就是只有128個機構可以使用，且每個機構會被分到2^16個終端設備數量，對於大部分機構來說太多了；C級只有256個設備能夠連結，對大部分使用情況來說太小了。

為了解決這個問題，之後的IPv4採用了新的定址方式:無級別定址(classless addressing)，在這個方式下，可以任意選擇Prefix占用的長度，藉此讓分級變得更彈性。由於Prefix的長度可以改變，因此需要一個表示方式來定義此IP的Prefix長度，定義為在IP後方加上 /len of prefix:
Example : 127.0.0.1/16 => 前16個bit為prefix

![](https://i.imgur.com/Vd0oWB2.png)

在實際應用上，可以使用Mask快速找出一個IP Address的幾個特性:
* xxx.xxx.xxx.xxx/n
* Mask : 左邊n個bit為1；右邊n個bit為0
* 終端機器數量 : NOT(Mask)+1
* 此網路下的第一個address : (此網路下的任意位址) AND (Mask)
* 此網路下的最後一個address : (此網路下的任意位址) OR (NOT(Mask))

分配IP Address的工作由ICANN(Internet Corporation for Assigned Names and Numbers)分配，但不是由ICANN直接分IP給各個使用者，而是將IP分給各大ISP(Internet Service Provider)，由各個ISP申請所需數量的IP Address，但申請的數量一定為2^n(才能使用上述的Prefix作為區分)。ISP也可以使用classless addressing將自己手上的IP分成更小的子網路。

舉例來說:
一個機構拿到128.66.34.0/24這些IP，想將這些IP分成三個區塊，分別有12個、25個、50個Address。
* IP總數為256個
* 每個子區塊大小都要為2^n，因此實際大小為16、32、64
* 因此:
    * 128.66.34.0/26 ~ 128.66.34.63/26分給第三個區塊
    * 128.66.34.64/27 ~ 128.66.34.95/27分給第三個區塊
    * 128.66.34.96/28 ~ 128.66.34.111/28分給第三個區塊

## NAT(Network Address Translation)
從上述IP的介紹可以發現，一個機構(例如公司)拿到從ISP供應商拿到的IP區段是有限的，當機構內部的機器過多時，會發生有機器拿不到IP的問題，此時需要依靠NAT讓多台機器可以共享少量的IP位址。

![](https://i.imgur.com/xBzSrxZ.png)

* 當資料從內部網路往外部網路傳時
    * NAT Router將Source IP(來源IP)轉換為NAT的IP
* 當資料從外部網路往內部網路傳時
    * NAT將Destination IP藉由內部的轉譯表轉成Private IP
    * 為了讓NAT可以轉譯，NAT會在每次有封包出去到外網時，將Private IP及Destination IP的關聯性記起來
