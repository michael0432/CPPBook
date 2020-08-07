# Class Polymorphism
###### tags = `OOP`

## 多型(Polymorphism)
Polymorphism為多型，Polymorphism的定義為:
* 相同的訊息可以傳遞給不同的class或method
* 根據不同的class或method，此訊息會引發不同的動作

Polymorphism分為以下幾種:
* Function overloading
* Operator overloading 

## Function overloading
同樣名稱的function在收到不同的input時，會執行不同動作

```cpp=1
void print(string s){
    cout>>"String!">>endl;
}

void print(int i){
    cout>>"Int!">>endl;
}

void print(double d){
    cout>>"Double!">>endl;
}
```

### Function overloading (Constructor)
Class的Constuctor可以視為function的一種，因此也有function overloading的特性

```cpp=1
class myClass{
private:
    int a;
    int b; 
public:
    myClass(){
        a = 0;
        b = 0;
    }
    myClass(int x, int y){
        a = x;
        b = y;
    }
}
```

## Operator overloading
定義運算子不同的意義

```cpp=1
int a = 2;
int b = 3;
int c = a+b;
```

```cpp=1
class myClass{
private:
    int a;
    int b;
}

myClass m1;
myClass m2;
myClass m3 = m1+m2; // ??
```

### 第一種方法: 寫一個function

```cpp=1
class myClass{
private:
    int a;
    int b; 
public:
    myClass(int x, int y){
        a = x;
        b = y;
    }
}

myClass Add(myClass x, myClass y){
    myClass out(x.a+y.a, x.b+y.b);
    return out;
}

myClass m1;
myClass m2;
myClass m3 = Add(m1, m2);
```

### 第二種方法: Operator overloading

```cpp=1
class myClass{
private:
    int a;
    int b;  
public:
    myClass(int x, int y){
        a = x;
        b = y;
    }
    myClass operator+(const myClass& m){
        myClass out(this->a + m.a, this->b + m.b);
        return out;
    }
}

myClass m1;
myClass m2;
myClass m3 = m1+m2;
```

## Override

當class B繼承class A時，若在class B中重新implement跟class A中相同的method，則Class A的method會被override

```cpp=1
class Animal{
public:
    getSound(){
        cout<<"hi!"<<endl;
    }
}

class Frog : Animal{
public:
    getSound(){
        cout<<"gua!"<<endl;
    }
}

int main(){
    Animal a;
    Frog f;
    a.getSound();
    f.getSound();
}
```



