# 資料型態&宣告

###### tags = `C++`

## 資料型態
C++常用/最基本的資料型態如下:
* 1bytes = 8 bits

| 資料型態 | 資料長度(bytes) | 最小值 | 最大值 |
| -------- | -------- | -------- | -------- |
| int     | 4     | -2147783648 (-2^31)    | 2147783647(2^31-1)  | 
| unsigned int     | 4     | 0    | 4294967295(2^32-1)  | 
| float     | 4     |  精準至小數點後第七位   |   |
| double     | 8     |  精準至小數點後第15位   |   |
| char     | 1     |  -128   | 127  |


```cpp=1
sizeof(資料型態或變數名稱) // 該資料型態或變數的資料長度
```
![](https://i.imgur.com/c0CEV55.png)

## 宣告變數
C++宣告變數的格式為：
    
    資料型態 變數名稱
Example :  
```cpp=1
int a = 3;
float b = 4.7;
char c = '1';
```

## 小練習
### 小練習1
#### stdin
    Myage 10
#### stdout
    My age is 10
### 小練習2
https://www.hackerrank.com/challenges/cpp-input-and-output/problem


## 資料型態轉換

當要對不同的資料型態進行運算時，建議先轉換成相同的資料型態，轉換型態的方式為：

    (轉換目標型態)value
    
Example:
```cpp=1
int a, b;
float c = ((float)a + (float)b) / 2; // right!
float c = (float)(a+b) / 2; // right!
```
### 小練習3
加法，數字跟符號間會有" "隔開，將運算的答案無條件捨去後輸出。
#### stdin
1.7 + 5.5
#### stdout
7

### ASCII
ASCII是一個電腦編碼系統，用於顯示英文、常用符號、數字。
![](https://i.imgur.com/qOtwuL2.png)

在C++中，資料型態char儲存-128~127的值，並將此值對照到ASCII表得到要顯示的字元，因此，若是要將char轉變成int:
```cpp=1
char c = '3';
int i = (int)c - '0';
```
* Reference :https://zh.wikipedia.org/wiki/ASCII

### 小練習
* https://www.hackerrank.com/challenges/a-very-big-sum/problem
