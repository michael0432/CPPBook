# Procss Lifecycle
###### tags = `Operating System`

## Create Process

在Linux系統中，一個process可以藉由system call建立另一個process。原先的process稱為parent process，新的process稱為child process。這會讓整個作業系統中的process成為一個process tree。

![](https://i.imgur.com/DgXymKa.png)


### Fork

fork為Linux系統中的一個system call，當呼叫了fork()後，會創建一個和當前process一樣的child process，當child process被建立後，會和parent process共用同一份程式碼，一起從fork()處繼續執行。

```cpp=1
#include <unistd.h>

pid_t fork(void);
```

* return value:
    * -1 : 錯誤
    * 0 : 代表為child process
    * >0 : 代表為parent process，回傳值為child process的process id

Example:

```cpp=1
#include <unistd.h>
#include <stdlib.h>

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
    }
    else
    {
        // parent process
        printf("I'm parent, parent PID = %d, my child PID = %d", getpid(), PID);
    }
    printf("Both process exe here!?\n");
}
```

當fork()出child process時，此process為一個獨立的process，擁有自己的PCB。

![](https://i.imgur.com/zpurI1j.png)


### Wait

有時，我們會需要parent process知道child process是否結束，此時可以呼叫wait()，wait()會讓此process block住，等待child process執行完成，如果在呼叫wait()時child process已經結束，則會馬上return。

```cpp=1
#include <sys.types.h>
#include <sys/wait.h>

pid_t wait(int* status);
```

* status: child process結束時的狀態會存在status中，若不在意結束狀態，可以傳NULL
* return value:
    * 成功時回傳child process的process id
    * 失敗時回傳-1
* example:

```cpp=1

int status;
pid_t PID = fork();

if(PID == -1)
{
    //error
}
else if(PID == 0)
{
    //child process
    printf("I'm child, child PID = %d\n", getpid());
    exit(5); // 結束process
}
else
{
    // parent process
    printf("I'm parent, parent PID = %d, my child PID = %d\n", getpid(), PID);
    sleep(1); // 睡1秒
    pid_t child_pid = wait(&status);
    int i = WEXITSTATUS(status); // 取得child process的返回值
    printf("child pid = %d, exit status = %d\n", child_pid, i);
}
```

### WaitPid

一個process可以fork()出多個child process，此時，wait()會等待所有的child process直到其中一個exit()，然而，某些情況下，parent process只想等待其中部分的child process，這時可以使用waitpid()。

```cpp=1
pid_t waitpid(pid_t pid, int* status);
```

* input
    * pid_t pid : 要等待的child process id
        * pid < -1 : wait所有process id == |pid|的child process
        * pid == -1 : wait所有child process，相當於wait()
        * pid == 0 : wait process id == 自己的process
        * pid > 0 : wait所有process id == pid的child process
    * status : child process結束時的狀態會存在status中，若不在意結束狀態，可以傳NULL
* return value:
    * 成功時回傳child process的process id
    * 失敗時回傳-1

### Execl

當fork()出child process後，child process與parent process共用同樣的程式碼，但多數情況下，我們需要讓child process執行不同的程式碼，此時可以使用execl()，使用execl()後，process的所有資訊都會被新的程式碼所代換，跟parent process完全不相同，只有pid會保留。

```cpp=1
#include <unistd.h>

int execl(const char *path, const char *arg...);
```

* input value:
    * path : 要執行的程式路徑(執行檔)
    * arg : 啟動該程式時所帶的參數
        * 第一個變數為程式路徑名稱
        * 第二個變數開始為argv中的參數
* return value:
    * 成功時回傳0
    * 失敗時回傳-1

* example:

```cpp=1

int status;
pid_t PID = fork();

if(PID == -1)
{
    //error
}
else if(PID == 0)
{
    //child process
    printf("I'm child, child PID = %d\n", getpid());
    execl("/bin/ls", "ls", "-al");
}
else
{
    // parent process
    printf("I'm parent, parent PID = %d, my child PID = %d\n", getpid(), PID);
    sleep(1); // 睡1秒
    pid_t child_pid = wait(&status);
    int i = WEXITSTATUS(status); // 取得child process的返回值
    printf("child pid = %d, exit status = %d\n", child_pid, i);
}
```

當執行一個自己寫的C/CPP時，可以利用main function的input拿到程式輸入的參數

```cpp=1
#include <stdio.h>
int main(int argc, char *argv[])
{
    // argc為參數的數量
    // argv為參數內容
    for(int i=0; i<argc; i++)
    {
        printf("i: %d, %s\n", i, argv[i]);
    }
}
```

執行時:

```
./a.out 11 22 33 ts
```

## 小練習

寫一個parent.cpp及child.cpp。

* parent.cpp
    * fork出三個child process，利用exec()讓三個child process都執行child.cpp中的內容，並給三個child process不同的input
    * 等待三個child process完成，並依序印出完成的順序(child process的pid)
* child.cpp
    * 從argv中讀取input，input為一個int N
    * 使用迴圈計算1+....+N的值並印出

### Orphan Process
當parent process比child process還早結束時，會使child process變成孤兒，Linux系統中的init process (pid = 1)會接管此child process，並wait()等待其結束。

解決方式:
* child process執行完畢後會自行結束，init process會收到結束的通知並正常將其資源收回

### Zombie Process
Zombie Process是指完成執行(call exit()或錯誤導致中止)的Process，沒有被parent process wait()所回收，導致其一直存在於系統的process table中，Zombie Process不會佔據系統的記憶體資源，但是會佔據Process id，若zombie process過多會使得Process id超過上限。

解決方式:
* kill掉parent process，使init process接管此zombie process。

* Question: 為什麼需要Zombie process?

### Kill

Linux系統中可以使用kill指令強制結束一個process

```
kill pid
```

pid為想要強迫中止的process id

有時程式當掉時，沒辦法用這個方式停止程式執行，這時會使用-9，意思為強制停止程式執行

```
kill -9 pid
```

不同的參數代表送出不同的signal給該process。
* -2 : 相當於Ctrl+C
* -9 : 強制停止
* -15 : 以正常程序通知程式停止
* -l : 列出所有可用的signal

## 小練習

1. 製造出一個zombie process，使用ps aux指令找到其process id
2. 將其parent kill掉，並檢查zombie process是否還存在

### Vfork

vfork為fork較為節省資源的版本，上面提到，fork後會複製出一份跟parent process一樣的空間，但通常，fork完成後會使用exec馬上執行其他的程式，使得複製空間的操作浪費資源，因此，可以使用vfork，vfork在執行exec前，child process會在parent process的空間上進行操作，parent process會等待child process call exec()或exit()才繼續進行。


## End of Process

![](https://i.imgur.com/6r8Peem.png)

Process結束有幾種方式:
* Call exit()
* return from main()
    * exit(main(argc, argv))
* Call pthread_exit from the last thread
* Terminated by signal
