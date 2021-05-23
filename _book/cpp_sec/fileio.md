# File I/O (Unbuffered I/O)
###### tags = `Operating System`

File I/O透過System Call直接跟kernel溝通，Linux透過File Descriptor來識別不同的檔案，並且直接存取檔案以及對檔案的內容操作，每一次的操作都包含了至少一個System Call。

![](https://i.imgur.com/LVs7Auc.png)


常用的操作分為Create(新增檔案)、Open(開啟檔案)、Read(讀取檔案)、Write(寫入檔案)。

File I/O又被稱為Unbuffered I/O，此處的Unbuffered不代表整個操作的過程不會需要buffer(緩衝)，而是buffer是存在kernel space，而不是存在user space，也就是使用者不會需要自己控管buffer。

![](https://i.imgur.com/1yMvaSd.png)

![](https://i.imgur.com/4jyrnAg.png)


## File Descriptors

File Descriptor是一個>=0的整數，當某隻程式Open或Create一個檔案時，kernel會回傳一個File Descriptor，當程式需要對此檔案操作時，便利用這個File Descriptor進行操作。

對於每個process:
![](https://i.imgur.com/BOPDIAd.png)


在unistd.h中定義:
* fd 0 - 對應到standard input
    * 預設為從terminal直接輸入
* fd 1 - 對應到standard output
    * 預設為輸出到terminal
* fd 2 - 對應到standard error
    * 預設為輸出到terminal

## Open

從這邊開始，所有的程式碼都用C語言當例子，也可以使用.cpp來寫

```c=1
#include <fcntl.h> // include open() function
#include <sys/types> // include mode_t class
#include <sys/stat.h> // flag of open function

int open(const char*pathname, int oflag, …/* mode_t mode */);
```

* pathname
    * 可以使用絕對路徑或是相對路徑
        * 絕對路徑: 完整的路徑 example :　/usr/bin/
        * 相對路徑: 相對目前所在位置的路徑 : ./bin/
* oflag
    * 利用Flag來控制Open檔案的參數設定，用 | 相連每個參數
    * 常用參數
        * O_RDONLY : read only
        * O_WRONLY : wrie only
        * O_RDWR : read/write
        * O_EXEC : execute only
        * O_CREAT : create the file if doesn't exist
        * O_APPEND : append to end of file on each write
    * Example
        * int fd = open(pathname, O_RDONLY | O_CREAT);
* mode
    * 第三個參數mode可以選擇不加，用於指定該檔案的權限，若有用O_CREATE則必須要加

* return value
    * 開啟成功時回傳fd
    * 開啟失敗時回傳-1

### 檔案權限

使用ls -al可以看到檔案的權限，在Linux系統中，檔案的權限用十個字元表示。
* 第一個字元表示檔案形式，d為資料夾；-為檔案
* 接下來的為三個字元一組，分為三組，皆由rwx所組成，第一組為檔案擁有者具有的權限；第二組為加入此群組帳號的權限；第三組為非檔案擁有者且沒加入此群組的帳號的權限。
    * r : 可讀
    * w : 可寫
    * x : 可執行
    * - : 沒有權限

舉例來說:

    -rwxrwx---
    
* 1 : -表示此為檔案
* 234 : 擁有此檔案者有讀寫及執行此檔案的權限
* 567 : 同群組的使用者有讀寫及執行此檔案的權限
* 8910 : 其他使用者沒有任何權限

#### 更改權限

檔案的權限為三個三個一組，在linux中使用一個數字來表達一組權限

* r = 4
* w = 2
* x = 1

因此，一組rwx的權限可以表示為4+2+1 = 7

可以使用chmod (change mode來改變一個檔案或資料夾的權限)

    chmod mode filename
    chmod 740 text.txt
    
以上述例子來說，chmod 740 text.txt會把此檔案的權限改為-rwxr-----。

## Create

```c=1
#include <fcntl.h>
#include <sys/types>
#include <sys/stat.h>

int creat(const char*pathname, mode_t mode);
```

creat等同於
    
    open(pathname, O_WRONLY | O_CREAT | O_TRUNC, mode);

* return value
    * 建立成功時回傳fd
    * 建立失敗時回傳-1

## Close

```c=1
#include <unistd.h>

int close(int fd);
```

所有打開的檔案都會在process結束時自動被關閉

### 小練習

下列程式碼的output為何?

``` c=1
int main()
{
    int fd1, fd2;
    fd1 = open("foo.txt", O_RDONLY, 0);
    close(fd1);
    fd2 = open("foo.txt", O_RDONLY, 0);
    printf("fd2 = %d\n", fd2);
}
```

### 當兩個process開啟同個檔案時

```c=1
// Process A:
    open("foo.txt", O_RDONLY, 0);
// Process B:
    open("foo.txt", O_RDONLY, 0);
```
![](https://i.imgur.com/MwD5E2c.png)

### 當同個process開啟一個同樣的檔案兩次時

```c=1
// Process A:
    open("foo.txt", O_RDONLY, 0);
    open("foo.txt", O_WRONLY, 0);
```
![](https://i.imgur.com/3D0vS2q.png)

## lseek

每一個被打開的文件都有一個讀寫位置，此讀寫位置稱為offset，指的是從文件的開頭開始數第幾個bytes的位置，前面提到的O_APPEND便是把offset移動到檔案的最尾端。當使用read或是write時，讀寫的位置會隨著讀/寫的操作增加。

lseek用來控制檔案的offset

```c=1
off_t  lseek(int fd, off_t offset, int whence);
```

* off_t為一個int或是long long int，offset值在SEEK_CUR及SEEK_END時可以為負
* whence
    * SEEK_SET: offset為新的讀寫位置
    * SEEK_CUR: 從目前的讀寫位置往後移動offset個位移量
    * SEEK_END: 將讀寫位置移動到最尾後，再移動offset個位移量
* example:
    * lseek(3, 0, SEEK_SET);
    * lseek(3, 0, SEEK_END);
    * lseek(3, 0, SEEK_CUR);
* return value:
    * 成功時回傳當前的讀寫位置
    * 失敗時回傳-1

![](https://i.imgur.com/UNdsjTy.png)


## Read

```c=1
#include<unistd.h>
ssize_t read(int fd, void* buf, size_t nbytes);
```

* fd: 檔案的file descriptor
* buf: 讀出資料的緩衝區，即為存取資料的記憶體位址
* bnytes: 請求讀取的位元數，讀取完後offset往後移
* return value: 
    * 成功: 回傳讀出的位元數
    * 失敗: 回傳-1
    
example:

```c=1
char* c = new char(10);

int re = read(0, c, 10*sizeof(char)); // fd為0，從stdin讀取
printf("%s", c);
```

## Write

```c=1
#include<unistd.h>
ssize_t write(int fd, const void* buf, size_t nbytes);
```

* fd: 檔案的file descriptor
* buf: 從buf寫入資料到檔案
* nbytes: 寫入多少位元數的資料，寫完後offset往後移
* return value:
    * 成功: 回傳寫入的位元數
    * 失敗: 回傳-1

example:

```c=1
	char* c = "123";
    int re = write(1, c, 3*sizeof(char)); // fd為1，從stdout讀取
```

## 小練習

寫兩份程式碼provider.cpp跟customer.cpp

provider.cpp有兩個功能，writeNum()跟readAns():
* writeNum()，將兩個數字寫入一個tmp.txt中，以空格隔開，num1及num2為int
    * Input: writeNum num1 num2
* readAns()，從tmp.txt中讀出b.cpp寫入的答案，並印出
    * Input: readAns

customer.cpp有一個功能，calAns():
* 從tmp.txt中讀到兩個數字
* 將兩數字相加，將相加後的結果寫到tmp.txt中，並以空格隔開
* Input: calAns

執行的順序為:
* provider.cpp的writeNum()
* customer.cpp的calAns()
* provider.cpp的readAns()

### Test Input

```c=1
// provider
writeNum 10 15

// customer
calAns

// provider
readAns
```

在writeNum執行後，tmp.txt被create，且檔案內容為10 15
calAns執行後，tmp.txt中的值為10 15 25
readAns會印出25

### Output
25