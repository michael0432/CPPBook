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