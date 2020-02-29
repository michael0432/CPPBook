# String
###### tags = `C++`


### char[]
在C/C++中，字串就是字元的Array，宣告方式如下:
```cpp=1
char 變數名稱[字串長度] = "123";
char 變數名稱[字串長度] = {'1','2','3'};
```
### string
由於使用char[]的方式處理字串會碰到許多不便，例如不能直接將兩個字串相加，因此，C++有一個string的函式庫用來處理字串。
```cpp=1
#include <string>

string a = "abc";
string b = "cde";
string c = a + b;

for(int i=0 ; i<c.size() ; i++){
    cout << c[i];
}
```

### string常用的method
#### insert
insert用於將字串插入字串中間
```cpp=1
string.insert(位置, 插入字串);


string str = "123";
str.insert(1,"456");
cout << str;
```
#### erase
erase用於清除字串
```cpp=1
string.insert(開始位置, 清除字元數);

string str = "123"
str.erase(1,2);
cout << str;
```
#### length
取得字串長度
```cpp=1
string.length();
```
#### size
取得字串大小
```cpp=1
string.size();
```
#### find
尋找字串
```cpp=1
string.find(目標字串);

string str = "123"
cout << str.find("23");
```
#### empty
判斷是否為空字串，若是會回傳true
```cpp=1
string.empty();

string str = "123"
cout << str.empty();
str = "";
cout << str.empty();
```

### 小練習
* https://www.hackerrank.com/challenges/funny-string/problem
* https://www.hackerrank.com/challenges/designer-pdf-viewer/problem