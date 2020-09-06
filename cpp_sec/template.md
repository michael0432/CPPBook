# Template
###### tags = `OOP`

template是一種模板，可以用來把資料型態參數化，透過編譯器根據樣板內容產生實際的程式碼。template分成兩種:
* Function Template
* Class Template

## Function Template

Template的用法是用<>括起參數

```cpp=1
template <型態 樣板名稱>
```

舉例來說，如果要寫兩個變數相加的function，當input跟output的資料型態不同時，就要重複寫很多幾乎一樣的function:

```cpp=1

int add(int a, int b){
    return a+b;
}
float add(float a, float b){
    return a+b;
}
```

如果使用template，可以簡化這段程式碼:

```cpp=1
template <class T>
T add(T a, T  b){
    return a+b;
};
```

編譯器會在執行到這個function時，建立一個此模板的實例(instance)

```cpp=1
template <class T>
T add(T a, T  b){
    return a+b;
};

int main(){
    int a = 2, b = 3;
    int c = add(a,b);
    // 編譯器新增:
    //int add(int a, int b){
        //return a+b;
    //}
}

```

如果有某個版本，不想要透過編譯器建立:

```cpp=1
template <class T>
T add(T a, T  b){
    return a+b;
};
template <>
string add(string a, string b){
    return a.size() + b.size();
};
```

若input的格式不同，可以更改template宣告時的input:
```cpp=1
template <class T1, class T2>
T2 add(T1 a, T2 b){
    return a+b;
};

int main(){
    int a = 2;
    double b = 2.3;
    double c = a + b;
}
```

```cpp=1
template <class T, int i>
T2 add(T1 a, T2 b){
    return a+b;
};
```

## Class Template

class template的用法與function template大致相同，一樣透過編譯器根據樣板內容產生實際的程式碼，不同的是，編譯器會根據樣板建立一個此class的實例(instance)。

舉例來說:

```cpp=1
template <class T>
class myPair{
private:
    T v1;
    T v2;
public:
    myPair(T first, T second){
        v1 = first;
        v2 = second;
    }
};

int main(){
    myPair<int> pair(13,27);
    myPair<string> pair2("123","456");
}

```

跟function template相同，若有某個版本不想透過編譯器直接建立:

```cpp=1
template <class T>
class myPair{
private:
    T v1;
    T v2;
public:
    myPair(T first, T second){
        v1 = first;
        v2 = second;
    }
    T getv1(){
        return v1;
    }
};

template <>
class myPair<string>{
private:
    string v1;
    string v2;
public:
    myPair(string first, string second){
        v1 = second;
        v2 = first;
    }
    string getv1(){
        return v1;
    }
};

int main(){
    myPair<int> pair(13,27);
    myPair<string> pair2("123","456");
    cout << pair.getv1() << pair2.getv1() << endl;
}

```

此外，template的資料型態不一定全部都要是自訂的template，也可以使用固定的型態:

```cpp=1

template <class T, int N>
class mysequence {
private:
    T memblock [N];
public:
    void setmember (int x, T value){
        memblock[x] = value;
    }
    T getmember (int x){
        return memblock[x];
    }
};

int main(){
    mysequence<int, 5> myints;
    mysequence <double,5> mydoubles;
    myints.setmember (0,100);
    mydoubles.setmember (3,3.1416);
    cout << myints.getmember(0) << endl;
    cout << mydoubles.getmember(3) << endl;
}

```