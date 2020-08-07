# Class Constructors
###### tags = `OOP`

在C++中，我們使用class來表示一個object，class裡面可以包含變數(variable)跟方法(method)。

```cpp=1
class myClass{
public:
    int num;
    string s;
}

int main(){
    myClass obj;
    obj.num = 3;
    obj.s = "123";
}
```

這種宣告obj的方式會讓程式碼看起來很冗長，因此需要使用Constructor來建立一個object。Constructor是一個名字與class名稱相同，且不需要定義回傳資料型態的method。

```cpp=1
class myClass{
public:
    int num;
    string s;
    myClass(int n, string x){
        num = n;
        s = x;
    }
}

int main(){
    myClass obj(1,"123");
}
```


