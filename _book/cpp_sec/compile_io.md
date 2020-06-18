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
compile:
g++ -std=c++11 filename.cpp
run:
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

## 輸入輸出
常用function / 指令:
* cin : input
    * 使用>>隔開每個變數
* cout : output
    * 使用<<隔開每個變數
* endl : 換行符號
```cpp
// 輸入輸出的Library
#include <iostream>
using namespace std;

int x;
string y;
cin >> x >> y;
cout << x << y << endl;
```
