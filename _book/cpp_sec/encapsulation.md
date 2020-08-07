# Class Encapsulation
###### tags = `OOP`

## 封裝(Encapsilation)

將變數(variable)及方法(method)包裝在物件中，隱藏具體執行的步驟，限制只有特定的物件或method可以存取或使用這些變數或方法。

```cpp=1
class Pocket(){
public:
    int money;
};

int main(){
    Pocket p;
    p += 1; // add money
}
```

```cpp=1
class Pocket(){
private:
    int money;
public:
    int getMoney(){
        return this->money;
    }
    void addMoney(){
       money += 1; 
    }
};

int main(){
    Pocket p;
    p.addMoney();
}
```

C++利用public, protected, private來定義class中哪些變數或方法能不能被外部使用。
* public : 可以被外部使用
* protected : 只能在此class中及繼承此class的class中使用
* private : 只能在此class中使用

