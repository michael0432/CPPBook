# FIFO
###### tags = `Operating System`

Pipe有幾個不方便使用的缺點:
* 只能提供單向傳輸
* 只能用在child-parent process之間
* 沒有名字

FIFO(named pipe)可以解決這個問題，FIFO提供了一個路徑名字，像是一個檔案一樣，讓其他的process也可以找到這個FIFO。FIFO遵循First-in-first-out的原則，先進到FIFO的資料一定會先被讀出，一旦資料被讀取，就會從FIFO中被移除，所以FIFO不支援lseek。

## Create

FIFO可以透過terminal輸入指令建立:
    
    mkfifo filename
    
也可以在程式執行時建立:

```cpp=1
#include <sys/types.h>
#include <sys/stat.h>

int mkfifo(const char *pathname, mode_t mode);  
```

* pathname: 此FIFO的檔案路徑及名稱
* mode: 與open()中的mode相同，也就是檔案的權限
* return value:
    * 成功return 0
    * 失敗return -1
* example

```cpp=1
// read.cpp
int main()
{
    if(mkfifo("fifo1", 0777) < 0)
    {
        // error
    }
    else
    {
        int fd;
        fd = open("fifo1", O_RDONLY);
        if(fd < 0)
        {
            // error
        }
        else
        {
            int l;
            char buf[1024];
            while(l = read(fd, buf, 1024) > 0)
            {
                // 如果fifo裡有資料就讀出來
                printf("%s", buf);
            }
            close(fd);
            return 0;
        }
    }
}
```

```cpp=1
// write.cpp
int main()
{
    int fd = open("fifo1", O_WRONLY);
    if(fd < 0)
    {
        // error
    }
    else
    {
        for(int i=0; i<10; i++)
        {
            char buf[1024];
            int n = sprintf(buf, "Write: %d\n", i);
            if(write(fd, buf, n) < 0)
            {
                // error
                exit(0);
            }
            sleep(1);
        }
    }
}   
```

## BLOCK/NONBLOCK

讀取的方式可以分為block及non-block兩種:
* Block (預設): 
    * open為O_RDONLY時，會卡在open直到某個process使用write相關的模式開啟fifo
    * open為O_WRONLY時，會卡在open直到某個process使用read相關的模式開啟fifo
    * open為O_RDWR時，會卡在call read function時，直到read的到資料才會繼續
    * call write 時，如果buffer滿了，會卡在write直到buffer足夠
* Non-block (藉由O_NONBLOCK設定為non-block模式)
    * open為O_RDONLY時，如果沒有process使用write相關模式開啟fifo，open會回傳成功
    * open為O_WRONLY時，如果沒有process使用read相關的模式開啟fifo，open回傳-1(失敗)
    * open為O_RDWR時，即使讀不到資料也不會卡在read
    * call write 時，如果buffer滿了，write會回傳-1(失敗)

就軟體設計層面而言，不論使用block或non-block作為通訊方式，都應該要確認write進fifo的資料可以盡快被read讀出。

## 小練習
參考pipe的小練習，寫一個類似的下注小遊戲，不同的是，改成使用5個FIFO來實作，且Sysem及五個Player這六個process沒有parent-child的關係。

System會先執行並開啟五個FIFO，等待五個FIFO的input，五個player要根據自己是幾號player，輸入自己每輪的下注。

### Input
無input

### Ouput

#### System
System在每輪結束時要印出如下例子的此輪結果

    Round1 :
    Player0 : 3
    Player1 : 7
    Player2 : 4
    Player3 : 4
    Player4 : 5
    Player1 wins round 1.
    
在十輪比賽結束後，System要印出比賽結果，依照排名順序印出Player編號及贏的次數，若排名相同先印出編號小的Player，並印出Gameover。

    Result:
    Player3, win 3 times.
    Player0, win 2 times.
    Player1, win 2 times.
    Player2, win 2 times.
    Player4, win 1 times.
    Gameover

#### Player
各個Player在每輪結束時印出目前自己贏過幾次。

    Player0 wins 0 times
    
    Player1 wins 1 times
    
    Player2 wins 0 times

    Player3 wins 0 times
    
    Player4 wins 0 times

### Other requirement

* 對每個player只能使用一個FIFO，也就是此FIFO需要雙向的傳遞資料(因為Player需要在每輪結束收到自己是否獲勝)
* 根據上一點，也就是說player只能一次寫進一輪的下注，不能一次寫進十輪