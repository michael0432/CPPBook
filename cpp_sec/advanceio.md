# Advance I/O
###### tags = `Operating System`

## I/O Multiplexing
有時，我們的應用需要同時聽/寫多個不同的fd，例如，當我們使用socket時，可能會需要同時listen多個client。
這種情況我們稱為I/O Multiplexing，我們可以使用select這個system call來解決需要同時監控多個fd的問題。

![](https://i.imgur.com/lkxFZJh.png)


### Select

![](https://i.imgur.com/hmF1R92.png)

Select會監控放進各fd set，若在set中任一個fd有變化(有新的資料可讀寫)，則會將fd set清空，只留下有變化的fd。

```cpp=1
#include <sys/select.h>

int select(int nfds,fd_set* readfds,fd_set* writefds,fd_set* exceptfds,struct timeval* timeout);


struct timeval {
	long	tv_sec;		/* seconds */
	long	tv_usec;		/* microseconds */
};   
```

* nfds: listen的file descriptor範圍，listen的範圍為0~nfds-1
* readfds: 放read file descriptor的集合，判斷是否可以從這些檔案中讀取資料了，如果這個集合中有一個檔案可讀，select就會返回一個大於0的值，表示有檔案可讀。
* writefds: 放write file descriptor的集合，是否可以向這些檔案中寫入資料了，如果這個集合中有一個檔案可寫，select就會返回一個大於0的值，表示有檔案可寫。
* exceptfds: 用於監視檔案是否有錯誤
* timeout: select是否超時，若超過特定時間都沒有ready的fd，則回傳0表示timeout，若input時將timeout設為NULL，表示將select設為blocking。
* return value:
    * 回傳值>0 : 回傳目前ready的fd數量
    * 回傳值==0 : timeout
    * 回傳值<0 : error


### fd_set

* void FD_ZERO(fd_set* fs); 清空集合
* void FD_SET(int fd, fd_set* fs); 將一個給定的檔案描述符加入集合之中
* void FD_CLR(int fd, fd_set* fs); 將一個給定的檔案描述符從集合中刪除
* int FD_ISSET(int fd, fd_set* fs); 確認fd是否在此fd set中
    * return value: 如果fd在此set中，回傳>0，不在則回傳0

* example

```cpp=1
struct fdset readset;
FD_ZERO(&readset);

FD_SET(0, &readset);
```

## Example

```cpp=1
int main()
{
    struct fd_set readset, workset;
    struct timeval timeout;
    FD_ZERO(&readset);
    FD_SET(0, &readset);
    timeout.tv_sec = 3;
    timeout.tv_usec = 0;
    
    while(1)
    {
        memcpy(&readset, &workset, sizeof(readset)); // workset = readset
        select(1, &workset, NULL, NULL, &timeout);
        if(FD_ISSET(0, &workset))
        {
            // 0這個fd還在set中，表示此fd有新的資料可讀
            // read
        }
    }
}
```

## 小練習
寫兩個程式: shop.cpp 及 customer.cpp。

### Shop
Shop提供三種商品: Apple, Banana, Orange，分別各有5個，shop利用select監聽三個fifo，分別對應到三個customer，確認是否有customer要購買商品，每當有customer購買商品，Shop會確認此商品是否還有庫存，若還有庫存，則回傳購買成功，並將庫存-1；若沒有庫存，則回傳購買失敗。

當所有庫存賣完後，印出賣出的順序及賣給哪個Customer。

#### Output

    Sell Apple to customer 1
    Sell Oragne to customer 0
    Sell Oragne to customer 1
    ...

### Customer
Customer會透過fifo跟shop購買商品，每次購買商品只能隨機選擇一樣商品並購買一個，每個customer再購買商品完成後會隨機sleep 0.1到1秒。

當每次收到Shop的回應，Customer印出是否購買成功及購買的商品為何。

#### Output

    Customer 0 buys 1 orange
    Customer 0 buys 0 orange
    ...

## Record/Byte-range Locking