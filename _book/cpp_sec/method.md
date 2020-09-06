# Class Methods
###### tags = `OOP`

方法(method)是class中的function，有兩種方式可以定義method:
* Inside class
* Outside class

## Inside

```cpp=1
class myClass{
public:
    void myMethod(){
        cout << "hi!!" << endl;
    }
}

int main(){
    Myclass obj;
    obj.myMethod();
}
```

## Outside

```cpp=1
class myClass{
public:
    void myMethod();
}

void myClass::myMethod(){
    cout << "hi!!" << endl;
}

int main(){
    Myclass obj;
    obj.myMethod();
}
```

通常，為了讓程式簡潔，我們會將class的定義寫在header file裡(.h檔)，將method的實作寫在.cpp檔裡。

myClass.h
```cpp=1
class myClass{
public:
    void myMethod();
}
```

myClass.cpp
```cpp=1
void myClass::myMethod(){
    cout << "hi!!" << endl;
}
```

Main.cpp
```cpp=1
int main(){
    Myclass obj;
    obj.myMethod();
}
```