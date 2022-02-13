## Transport Layer Protocal

### FSM (Finite-State Machine)
![](https://i.imgur.com/s4VJUX8.jpg)

FSM是一種表示有限個狀態及狀態間關係的數學計算模型，被廣泛應用在軟體系統設計上。一個FSM會用幾個元件組成:
* 狀態 : 用於表示目前此系統處於哪個狀態
* 條件 : 當滿足某些條件時，進行狀態之間的轉移

我們可以用FSM來表示Transport Layer Protocal的行為，舉個簡單的例子:
* Sender端收到Application的請求後，會製作封包並送出封包
* Receiber端收到封包後將封包傳給Application端

![](https://i.imgur.com/guyS3rb.png)

## Stop and Wait Protocal
上述的FSM並沒有處理錯誤控制和流量控制，Stop and Wait Protocal的設計使用了Sliding Window size = 1的設計來處理錯誤控制和流量控制。
Sliding Window size = 1表示:
* Sender一次傳出一個封包，確認收到ack後才會傳出下一個。
* 如果時間內沒收到ack，會重新傳送。
* 因為一次只有一個封包正在傳送，Seq num = 0 or 1即可
* 因為一次只有一個封包正在傳送，Ack num = 0 or 1即可，這邊習慣的用法是Ack會傳送期望下一個收到封包的Seq num。舉例來說，當Receiver收到0，會傳送一個Ack num = 1的ack。

![](https://i.imgur.com/Yl8xLoU.png)
![](https://i.imgur.com/8UTuMeL.png)

## Go-back N protocal
為了提升傳輸的效率，Go-back N protocal把Sliding window size調大，意思是可以有多個封包同時被傳送(Sliding window size = n表示最多有n個封包正在傳送)。
![](https://i.imgur.com/xtp1HDB.png)

* Sender
    * 傳出Seq num = n的封包
    * 傳出Seq num = n+1的封包
    * 若收到Ack num = n+1的封包 => 表示Seq num = n的封包被正確收到
    * 若收到Ack num > n+1的封包 => 表示Seq num = n的封包還沒被正確收到，丟掉此ack並重新傳送Window範圍內所有的封包
    * 若timeout => 全部window內的封包都重傳
![](https://i.imgur.com/MtRIMPA.png)

* Receiver
    * 若收到的封包損毀 => 丟棄封包
    * 若正在等Seq num = n的封包，但收到Seq num > n的封包，則回傳ack num = n+1並丟棄此封包
![](https://i.imgur.com/yyYQ2NF.png)

## UDP(User Datagram Protocal)
UDP是一種不可靠的Transport Layer Protocal，UDP除了提供Process間傳輸資料之外，並沒有提供其他額外的功能。意思是沒有提供錯誤控制、流量控制等等的功能，所以並不能保證資料可以正確無誤地送到接收端。

![](https://i.imgur.com/uah37yu.png)

### UDP的一些性質
* 資料大小(包含header)為65535 Bytes
* 沒有流量控制，所以使用UDP的Application可能需要自製流量控制的功能
* 只有利用checksum進行錯誤控制，不會得知封包掉落
* Check sum會將header及data以2bytes為單位加起來，可以選擇將checksum設為0表示不檢查checksum

### UDP的應用
* 由於UDP沒有保證封包傳送的順序，每個封包可以視為獨立的資料。UDP非常適合進行簡短且資料量小的單次傳輸。
    * Example : DNS(Domain Name System)使用UDP，DNS為client端送出server name(例如www.gooogle.com)；server端需要回應此server的address(例如172.217.163.46)。因為DNS的request跟respond資料量很小且只需傳送一次，因此使用UDP。
    * Example : 通常直播或是即時串流會使用UDP，假設我們正在使用youtube看直播，其中有一個frame的音訊沒有正確傳送
        * 使用TCP : 因為掉封包，重新傳送這個frame，使用者要等到這個frame重新傳送才能繼續觀看，重新重送的過程會沒有畫面
        * 使用UDP : 沒有錯誤控制，直接播放下一個收到的frame。

## TCP(Transmission Control Protocol)
TCP是連結導向的傳輸協定，跟UDP不同，TCP的傳輸過程會先確認接收雙方彼此可以互相連線；傳輸結束後會中斷連線。可以想成有一個虛擬的pipe連結傳輸雙方的Transport Layer。

![](https://i.imgur.com/ElUkEAi.png)

TCP Header分成幾個部分:
* Source Port : 傳輸方的port number
* Destination Port : 接收方的port number
* Seq number : 此Packet的Seq number
* Ack number : 此Packet的Ack number
* Header Length : 用於表示header的大小，大小範圍為20~60個bytes。(20個bytes為必要資訊，圖中的前五列；另外40 bytes為options)
* Reserved bits : 保留空間，沒有使用
* Control bits : 定義六種不同的control bits，用來控制流量、建立連線、終止連線等等
    * URG : 是否使用Urgent pointer
    * ACK : 此封包為ACK用(Ack number為有意義的值)
    * PSH : 要求Push，表示接收方要盡快將資料傳給application
    * RST : Reset，需要重建TCP連線
    * SYN : 連結請求或連結接受請求，用於建立TCP連線
    * FIN : 連結結束
* Window Size : Sliding window的大小
* Check sum : 以16bits為單位計算checksum
* Urget pointer : 指到資料中的某個address，表示此段資料為重要資料

### 建立TCP連線
![](https://i.imgur.com/rJ6r7Cz.jpg)

### 終止TCP連線
![](https://i.imgur.com/luLg80f.png)

### TCP FSM
![](https://i.imgur.com/EsEStId.jpg)
