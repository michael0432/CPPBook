# 邏輯判斷

###### tags = `C++`

C++中常用的邏輯判斷主要有三種：if/else, ?, switch

### IF/ELSE

```cpp=1
if(condition){
    ...
}
else if(condition){
    ...
}
else if(condition){
    ...
}
else{
    ...
}
```

### ?
?用來替代簡單的if/else判斷，讓程式碼看起來比較簡潔。
```cpp=1
if(condition1){
    cout << 1 << endl;
}
else{
    cout << 2 << endl;
}

(condition1) ? cout << 1 << endl : cout << 2 << endl;

```

### Switch
Switch與if/else if/else的差別在於switch根據條件的結果執行不同區段的程式。

```cpp=1
switch(condition){
    case 結果1:
        ...
        break;
    case 結果2:
        ...
        break;
    case 結果3:
        ...
        break;
    default: // 類似else的功能，不可省略
        ...
}
```
Example:
```cpp=1
char c;
switch(c){
    case 'a':
        ...
        cout << "is a" << endl;
        break;
    case 'b':
        cout << "is b" << endl;
        break;
    case 'c':
        cout << "is c" << endl;
        break;
    default: // 類似else的功能，不可省略
        cout << "not abc" << endl;
}

```

### 小練習
* https://www.hackerrank.com/challenges/drawing-book/problem
* https://www.hackerrank.com/challenges/cats-and-a-mouse/problem
* https://www.hackerrank.com/challenges/day-of-the-programmer/problem