# Pipe
###### tags = `Operating System`

Pipe是一種單向傳輸資料的方式，其中一個process為寫入資料者(Producer)；另一個process為讀出資料者(consumer)，兩個process各用一個fd指向這個pipe。

![](https://i.imgur.com/t17KANh.png)

## Open pipe
由於pipe是藉由fd溝通，因此使用pipe溝通的兩個process，通常互為parent/child process，彼此才能互相知道是透過哪個fd溝通。

### Step 1
Parent process先開好一個pipe，指定兩個fd作為reading端跟writing端
![](https://i.imgur.com/WtkPbqc.png)
![](https://i.imgur.com/WG1MJTs.png)

### Step 2
Parent process fork出child process，此時，child process跟parent process會共用同一樣的fd。
![](https://i.imgur.com/xWOBR92.png)

### Step 3
將不需要的fd關掉。
![](https://i.imgur.com/At83eNg.png)

## Read from pipe / Write to pipe
* 當pipe的write end全部都被close且所有pipe中的資料都被讀取後，會從pipe中讀到EOF，當read end收到EOF後，read end會被close。
* 當pipe的read end全部都被close時，write end會收到一個SIGPIPE的signal。
* Kernel會給pipe一個固定大小的buffer使用。 (PIPE_BUF)
* 再也不會使用到的read end跟write end都要關閉，process才能正常結束。

## dup2
dup2可以"複製"一個fd，使得兩個fd指到同一個文件。
![](https://i.imgur.com/5oUQTW0.png)

```cpp=1
#include <unistd.h>
int dup2(int oldfd, int newfd);
```

* oldfd: 要被複製的fd
* newdf: 新的fd，可以指定為一個合法的數字(0~1023)，如果此fd已被占用，系統會自動close該fd，再做複製的動作。
* return value
    * 成功回傳newfd
    * 失敗回傳-1

## pipe
pipe是linux的system call，用來建立process間的IPC。

```cpp=1
#include <unistd.h>
int pipe(int pipefd[2]);
```
* pipefd[0]為read end
* pipefd[1]為write end
* return value:
    * 建立成功回傳0
    * 建立失敗回傳-1

* example

```cpp=1
#define MAXLINE 100

int main()
{
    int n;
    int fd[2];
    pid_t pid;
    char line[MAXLINE];

    if (pipe(fd) < 0)
    {
        // pipe error
    }
    pid = fork();
    if (pid < 0)
    {
        // fork error
    }
    else if (pid > 0)
    {
        // parent
        close(fd[0]); // close read end
        write(fd[1], "hi\n", 3);
        wait(nullptr);
    }
    else
    {
        // child
        close(fd[1]);
        n = read(fd[0], line, MAXLINE);
        for(int i=0; i<n; i++)
        {
            std::cout << line[i];
        }
        exit(0);
    }
}
```

## 小練習
* 在開始小練習之前，建議先自己寫一段類似上面範例的程式，確認兩個process可以藉由pipe正常溝通。

寫一支程式碼，此程式碼包含一個Sysem及五個Player，共六個process。其中，System為parent process，五個Player為child process。

五個Player會一起參加一個下注的小遊戲，這個遊戲有十輪，每輪每個Player會隨機選擇0~9之間的一個數字下注，並將各自下注的數字傳給System，System會判斷下注最高的Player贏得此輪，並印出此輪的結果，若有多個Player下注最高，則這些Player共同贏得此輪，System要通知所有Player此輪比賽的結果。
在十輪遊戲結束後，System要印出比賽結果並排名，System確認所有Player都結束Process後，印出Gameover並結束Process。

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

* 結束後不可以留下zombie process及orphan process。