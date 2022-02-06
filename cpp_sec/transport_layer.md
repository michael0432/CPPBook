# Transport Layer Introduction
###### tags = `Network`

透過Internet Layer後傳輸封包後，Receiver會接受到從Sender傳來的封包。這時需要使用Transport Layer的協定來解析這個封包。對於Sender和Receiver來說，Transport Layer就像是一個假想的直接連線，可以透過Transport Layer的協定建立一個連線並傳送或收取訊息。

![](https://i.imgur.com/KT2KFU5.png)

Internet Layer透過IP Address定義了主機之間如何通訊。Transport Layer則是定義了Process之間的通訊(Process屬於application layer)，當透過Internet Layer將封包送到正確的機器上後，要透過Transport Layer將封包傳送給正確的Process。

## Port
為了可以做到Process之間的通訊，需要有一個分辨來源及接受Process資訊的值，這個值稱為Port。Port number介於0到65535之間(16bit)，其中:
* 0~1023 : 預設已被占用的port，不同的port有不同的用途
    * https://en.wikipedia.org/wiki/List_of_TCP_and_UDP_port_numbers
* 1024~49151 : 可以自由使用，也可以在系統上註冊為特定用途
* 19152~65535 : 自由使用

![](https://i.imgur.com/QANtzyu.png)

## Packeting
在發送端Transport Layer收到從Application Layer傳來的資料後，會將其包裝成Packet（封包），意思是將資料加上header。
在接收端Transport Layer收到封包後，會將header移除後把資料傳給對應的Process(Application Layer)。

![](https://i.imgur.com/ElUkEAi.png)

## 流量控制
Transport Layer還需要處理流量控制的問題。當發送端不斷傳送封包到接收端；而接收端來不及處理資料時，就有可能導致資料的丟失。反之，當發送端為了避免資料丟失降低傳送速度，就有可能導致系統效率降低。因此Transport Layer需要一個機制解決這個問題。
這個問題牽扯到四個角色：
* 發送端Process(Application) : 一直傳出資料
* 發送端Transport Layer : 接收發送端Process傳來的資料，並傳出封包到接收端Process
* 接收端Transport Layer : 接收封包，解開封包後傳出資料給接收端Process
* 接收端Process(Application) : 一直接收資料

### Buffer
為了解決流量控制的問題，在發送端Transport Layer及接收端Transport Layer都有一個buffer。
* 當發送端Transport Layer buffer滿時，會通知發送端Process暫停傳輸
* 當接收端Transport Layer buffer滿時，會通知發送端Transport Layer暫停傳輸

## 錯誤控制
在網路架構中，Internet Layer以下的協定並不保證是可靠的（有可能掉資料），因此若Application需要可靠的傳輸，需要透過Transport Layer進行錯誤控制。錯誤控制包含:
* 若封包資料毀損，需丟棄
* 若遺漏封包，需重新傳送
* 若有重複的封包，需丟棄
* 若封包順序錯誤，需確認是否有遺漏封包，並重新排列
為了錯誤控制，Transport Layer加上的header會需要以下的資訊

### Sequence Number
Sequence Number是封包的序號，傳輸端每次傳出封包時會依序給予封包Sequence Number，讓接收端可以知道封包的順序，並確認是否有遺漏。Sequence Number的範圍為2^m(在TCP中m=32)。

### Ack
在接收端收到封包後，會回傳一個確認的訊號Ack給傳輸端，表示確認這個封包有正確的抵達。
在接收端會有一個計時器，若時間內沒有收到Ack，會重新傳一次封包。
若Ack太晚抵達，接收端可能會收到兩次重複的封包，這時會使用Seq number判斷封包重複並丟棄。

### Checksum
Checksum是一種確認資料是否有正確傳遞的機制，有許多現行的Checksum演算法: md5、SHA1等等。我們舉一個最簡單的例子：
* 傳輸端再傳出封包前，先把傳輸的資料內每個一個bytes加總後mod 2^32，將結果存成一個32bit的資料，並加進封包的header。
* 接收端收到封包後，可以把收到的資料內個一個bytes加總後mod 2^32，確認值是否跟header存的32bits相同。

若checksum通過，可以某種程度的確認傳遞的資料是沒有毀損的。

## Sliding Window
上面討論完流量控制以及錯誤控制，我們可以利用Sliding Window把兩個需求結合起來。
![](https://i.imgur.com/xtp1HDB.png)

Sliding window由兩個index組成，兩個index間的封包表示尚未收到ack的封包。
當收到ack時:
* 若ack == 小的index，小的index++
* 若ack > 小的index，表示有可能遺漏封包，重新傳送並暫時不處理這個ack
當傳出新封包時:
* 大的index++

可以透過sliding windows大小的上限值(大的index-小的index)來控制流量。