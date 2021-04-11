# 編譯、輸入、輸出

###### tags = `C++`

## 編譯
C++是一種編譯式的語言，需要使用編譯器(compiler)將原始碼轉換成機器可以讀取的格式。
![](https://i.imgur.com/oFlMhpV.png)

## Hello, World!

Example: 
```cpp=1
#include <iostream>
using namespace std;

int main(){
    cout << "Hello!" << endl;
    return 0;
}
```
```cpp=1
//compile:
g++ -std=c++11 filename.cpp
g++ -g -O2 -std=gnu++11 -static -lm -o
// -g: 只編譯, -O2: 編譯器優化, -static: 禁用動態資料庫(會將所需的資料庫都引入), -lm: 使用數學函式庫 -o: 後方放輸出的檔案名稱(預設為a.out)
//run:
./a.out
```

## 程式架構

```cpp=1
// include函式庫 
#include <iostream>

// 使用namespace
using namespace std;

// 其他function
int xxx(){
    return xxx;
}

// 主程式
int main(){
    cout << "Hello!" << endl;
    return 0;
}
```

## 輸入輸出 (C++)
### 常用function / 指令:

* cin : input
    * 使用>>隔開每個變數
* cout : output
    * 使用<<隔開每個變數
* endl : 換行符號

```cpp
// 輸入輸出的Library
#include <iostream>
using namespace std;

int main(){
    int x;
    string y;
    cin >> x >> y;
    cout << x << y << endl;
}

```

若不使用namespace std

```cpp=1
int x;
string y;
std::cin >> x >> y;
std::cout << x << y << std::endl;
```

## 輸入輸出 (C語言)

C語言在輸入或輸出時，需要指定資料型態:

| 資料型態符號 | 意義 |
| -------- | -------- |
| %d     | 整數(int)     |
| %f     | 浮點數(float)     |
| %lf     | double     |
| %c     | 字元(char)     |
| %s     | 字串(string)     |
| \n     | 換行符號     |

### 常用function / 指令:
* scanf : input
    * scanf(control string, arguments);
    * control string: 包含資料型態符號的字串，用於定義輸入的格式
    * arguments: 要讀入的變數"記憶體位址"，依照順序對應control string中的資料型態符號
* printf : output
    * printf(control string, arguments);
    * control string: 包含資料型態符號的字串，用於定義輸出的格式
    * arguments: 要輸出的變數"變數名稱"
    
```cpp=1
#include <cstdio>

int main(){
    int x;
    float y;
    char c;
    char* s;
    scanf("%d %f %c %s", &x, &y, &c, s); // 讀入輸入，每個值中間隔一個空白
    printf("x: %d\ny: %f\nc: %c\ns: %s\n", x, y, c, s); // 輸出
}
```

#### 從檔案Input / Output到檔案

在執行程式時，可以使用 < 輸入檔案 > 輸出檔案

    ./a.out < input.txt > output.txt



#### EOF
若不確定資料有多少筆，可以使用EOF(end of file)來判斷是否讀完所有的input

```cpp=1
while(scanf("%d",&n) != EOF){ 
	
}
```