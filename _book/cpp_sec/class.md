# Struct / Class

###### tags = `C++`

### Struct
有些資料會有相關性，相關聯的資料組織在一起，對於資料本身的可用性或者是程式碼的可讀性，Struct 是C語言中的結構定義，可以將多個資料型態組合在一起變成一個獨立的資料型態：
```cpp=1
struct Account{
    string id;
    string name;
};

int main(){
    struct Account acc;
    acc.id = "123";
    acc.name = "456";
}
```

### Class
在C++中，我們使用Class來定義一個物件。並將所有物件的成員分成public及private兩類。
```cpp=1
class Account{
public:
    string id;
    string name;
};
int main(){
    Account acc;
    acc.id = "123";
    acc.name = "456";
}
```
我們可以在class中寫屬於此class的function:
```cpp=1
#include <iostream>

using namespace std;

class Account{
public:
    string id;
    string name;
    
    string getId(){
        return id;
        //return this->id;
    }
};
int main(){
    Account acc;
    acc.id = "123";
    acc.name = "456";
    cout << acc.getId() << endl;
}
```
因為在寫大型的程式時，不會將Class跟main寫在同一個cpp檔案裡，而是將class寫在.h檔中，因此，通常會把class的funtion宣告寫好，但在class外部implement。
```cpp=1
#include <iostream>

using namespace std;

class Account{
public:
    string id;
    string name;
    string getId();
};
string Account:: getId(){
    return id;
    //return this->id;
}

int main(){
    Account acc;
    acc.id = "123";
    acc.name = "456";
    cout << acc.getId() << endl;
}
```
### .h檔
.h檔被稱為是標頭檔(header)，代表的是程式與程式間的介面(interface)，在使用很多函式庫的時候，我們只在乎怎麼使用，不需要知道實作的細節，因此只需要看標頭檔就可以了。
以上面Account的例子來說，使用者使用的時候只要知道Account有getId這個funtion，並不在意裡面怎麼實作，因此：
```cpp=1
//account.h : interface
class Account{
public:
    string id;
    string name;
    string getId();
};
```

```cpp=1
//main.cpp
#include <account.h>

string Account:: getId(){
    return id;
    //return this->id;
}

int main(){
    Account acc;
    acc.id = "123";
    acc.name = "456";
    cout << acc.getId() << endl;
}
```
#### Constructor
在宣告一個Class時，我們可能會需要給其中一部分的成員初始值，但是一個一個給值會讓程式看起來很冗長，而且沒辦法直接給private的變數初始值，因此我們需要Constructor（建構子）。
Constructor是一個跟Class名稱相同的function：
```cpp=1
class Account{
public:
    string id;
    string name;
    Account(string input_id, string input_name, string input_password){
        this->id = input_id;
        this->name = input_name;
        this->password = input_password;
    };
private:
    string password;
};

int main(){
    Account acc("123","456");
}
```
也可以把Constructor移出來：
```cpp=1
class Account{
public:
    string id;
    string name;
    Account(string input_id, string input_name, string input_password);
private:
    string password;
};

Account::Account(string input_id, string input_name, string input_password){
    this->id = input_id;
    this->name = input_name;
    this->password = input_password;
}

int main(){
    Account acc("123","456","789");
}
```
### Constructor Delegation
在C++中，一個Class可以有很多個不同型態的Constructor，讓使用者可以根據不同情況將Class初始化：
```cpp=1
class Account{
public:
    string id;
    string name;
    Account(string input_id, string input_name, string input_password){
        this->id = input_id;
        this->name = input_name;
        this->password = input_password;
    };
    Account(){
        this->id = "default id";
        this->name = "default name";
        this->password = "default password";
    }
private:
    string password;
};

int main(){
    Account acc("123","456");
    Account acc2();
}
```
### 小練習
* https://www.hackerrank.com/challenges/c-tutorial-struct/problem
* https://www.hackerrank.com/challenges/c-tutorial-class/problem
* https://www.hackerrank.com/challenges/classes-objects/problem




