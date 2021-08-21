# Signal
###### tags = `Operating System`

除了之前提過的Pipe, Fifo, Socket之外，另一種Process之間傳遞訊息的方法為Signal，Signal是用來傳遞訊號而不是用來傳遞資料的，當對一個Procss傳遞Signal時，Procss對Signal的處理方式有三種:

* Process內部指定特定的function來處理特定的Signal
* 忽略該Signal
* 使用對該Signal預設的處理方式(通常為停止Process)

Linux定義了的Signal:

![](https://i.imgur.com/iaAt5JL.png)

幾個比較常用的Signal:

* SIGINT : 當從鍵盤按ctrl+c，會對此process送出SIGINT，預設的處理方式為強制停止Process
* SIGKILL : Kill process，與SIGINT不同的是，Process內不能忽略、也不能自訂對此Signal的處理方式
* SIGFPE : 發生運算錯誤時會發出此Signal，例如除以0
* SIGTSTP : 暫停Process執行，相當於在鍵盤輸入ctrl+z
* SIGCONT : 繼續執行Process，相當於輸入fg
* SIGCHLD : Child process結束時，Parent會收到這個signal，wait()就是等待這個signal
* SIGUSR1 : User可自訂的Signal，預設為停止Process

可以使用kill -l來查有哪些Signal可以使用

    kill -l 

## Signal Programming

### Signal Handle

在Linux中，使用signal()來定義使用哪個function來處理哪個signal

```cpp=1
#include <signal.h>
void (*signal(int signo, void (*func)(int)))(int);
```

* input : 
    * signo : Signal Number，即為上方表格列的Signal Name，表示要處理的signal
    * func : 處理此signal的function pointer，function的格式是input為1個int，output為void
        * 如果要自定義處理的function，這邊就填自己寫的function
        * 也可以填入Linux定義好的function pointer
            * SIG_IGN : 忽略此Signal
            * SIG_DFL : 使用此Signal的預設處理方式
* return value:
    * 回傳一個function pointer，function的格式是input為1個int，output為void，指向之前指定Singal處理的function

### Example

```cpp=1
# include <signal.h>
# include <stdio.h>
# include <unistd.h>

void singal_handle_func(int signo)
{
    if (signo == SIGINT)
    {
        printf("Receive CTRL + C\n");
        exit(1);
    }
    if (signo == SIGUSR1)
    {
        printf("Receive user defined signal");
    }
}

int main()
{
    if (SIG_ERR == signal(SIGINT, singal_handle_func))
    {
        printf("Set signal SIGINT handle");
    }
    if (SIG_ERR == signal(SIGUSR1, singal_handle_func))
    {
        printf("Set signal SIGUSR1 handle");
    }
    sleep(100);
}
```

### Send Signal

在Linux中，可以使用kill -SIGNO pid來送一個signal給特定process，舉例來說

    kill -SIGINT 1234
    
就相當於對process 1234下ctrl+c。

同樣的，在C/C++，也是使用kill function來傳遞signal。

```cpp=1
#include <sys/types.h>
#include <signal.h>

int kill(pid_t pid, int signo);
```

* input : 
    * pid : 要送signal給哪個Process
    * signo : Signal number
* return value:
    * 成功回傳0
    * 失敗回傳<0

### Example

```cpp=1
# include <signal.h>
# include <stdio.h>
# include <sys/types.h>

int main()
{
    pid_t PID = fork();
    if(PID == -1)
    {
        //error
    }
    else if(PID == 0)
    {
        //child process
        printf("I'm child, child PID = %d", getpid());
        execl("./signal_handle", "./signal_handle");
    }
    else
    {
        // parent process
        printf("I'm parent, parent PID = %d, my child PID = %d", getpid(), PID);
        sleep(1);
        kill(PID, SIGUSR1);
        waitpid(PID, NULL);
    }
}
```

## 小練習

寫兩隻程式碼: signal_sender.cpp及signal_receiver.cpp，其中single_sender為parent process，執行single_sender後fork出4個child process，child process都執行signal_receiver.cpp。

fork完成後，signal_sender每隔0.1秒送出一個SIGUSR1給隨機一個signal_receiver，signal_receiver要計算自己收到幾次signal。

當signal_sender送出20個signal後，對所有signal_receiver送出SIGINT，並wait()等待signal_receiver結束，signal_receiver收到SIGINT印出自己總共收到幾次signal後結束，signal_sender wait到所有signal_receiver後結束。

### Signal sender

* input : no
* output : 每次送出Signal，印出:
    * Send signal to PID XXXX

### Signal receiver

* input : no
* output : 最後結束時印出:
    * PID XXX receive OOO signal
