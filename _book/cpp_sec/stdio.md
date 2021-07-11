# Standard I/O (Buffered I/O)
###### tags = `Operating System`

## Unbufferd I/O v.s. Buffered I/O

在Unbuffed I/O中，每次做read/write都要做一次system call。
![](https://i.imgur.com/a1ucVo4.png)

為了減少system call的次數，可以使用Buffered I/O在User space開一塊I/O Buffer，在第一次開啟檔案時，先將整個檔案中的資料read()出來，存到User space2的I/O Buffer，之後每次讀取資料都不需要使用System call。

![](https://i.imgur.com/5pRVlRQ.png)

## fopen

```cpp=1
#include <stdio.h>

FILE* fopen(const char* pathname, const char* mode)
```

* pathname: 檔案路徑
* mode: 用字串表示打開檔案的形式，對應到read()的O_WRONLY, O_RDONLY...
    * "r" : read，檔案必須已經存在
    * "w" : create一個空的檔案
    * "a" : 把資料寫到檔案的尾端，如果檔案不存在，開一個空檔案並寫入
    * "r+" : read/write
    * "w+" : create一個空檔案，可read/write
    * "a+" : 同w+
* return value: 代表此FILE的pointer

![](https://i.imgur.com/5SscPqW.png)

## fdopen

```cpp=1
FILE* fdopen(int fd, const char* type);
```

* fd: File Descripor
* type: 同fopen
* return value: 代表此FILE的pointer

## fileno

```cpp=1
int fileno(FILE* fp);
```

* fp: 代表FILE的pointer
* return value: File Descripor

## fclose

```cpp=1
int fclose(FILE* fp);
```

* fp: 代表FILE的pointer
* return value: 
    * 成功return 0
    * 失敗return -1
* close時會強制flush所有的output data
* close時會強制丟掉所有還沒read的data

## Get

### Char-at-a-Time

```cpp=1
#include <stdio.h>
int getc(FILE *fp);
int fgetc(FILE *fp);
```

* fp: 代表FILE的pointer
* return value: 
    * 成功return 0
    * 失敗return -1


### Line-at-a-Time

```cpp=1
#include <stdio.h>
char* fgets(char* buf, int n, FILE* fp);
```

* buf: 將資料讀取到buf
* n: 讀取的資料大小
* fp: 代表FILE的pointer
* return value
    * 成功return該字串
    * 失敗return NULL

```cpp=1
char* gets(char* buf);
```

* 用途跟fgets相同，只是從stdin讀取

## Put

### Char-at-a-Time

```cpp=1
#include <stdio.h>
int putc(int c, FILE *fp);
int fputc(int c, FILE *fp);
```

* c: char的ascii
* fp: 代表FILE的pointer
* return value: 
    * 成功return 0
    * 失敗return -1


### Line-at-a-Time

```cpp=1
#include <stdio.h>
int fputs(char* buf, FILE* fp);
```

* buf: 要寫入的資料
* n: 寫入的資料大小
* fp: 代表FILE的pointer
* return value
    * 成功return >=0 的值
    * 失敗return -1

```cpp=1
int puts(char* buf);
```

* 用途跟fgets相同，只是寫到stdout

## FFlush
立刻將要輸出的內容立刻從buffer輸出到檔案。

```cpp=1
int fflush(FILE* fp);
```

* fp: 要flush的檔案
* return value: 
    * 成功return >=0 的值
    * 失敗return -1

## Example

```cpp=1
#include <stdio.h>
#include <stdlib.h>

int main()
{
    // read
    FILE * fp;
    char buf[100];
    fp = fopen ("file.txt", "r");
    if(fp != NULL)
    {
        if(fgets(buf, 100, fp) != NULL)
        {
            printf("%s", buf);
        }
    }
    
    fclose(fp);
    
    // write
    fp = open("file.txt", "w+");
    fputs("Hahaha!", fp);
    fputs("Hahahaha!", fp);
    
    fclose(fp);
}
```

## 小練習
寫兩個程式碼input.cpp跟output.cpp。
input.cpp將stdin輸入的字串寫入tmp.txt中，output.cpp將檔案中的字串印出到stdout。