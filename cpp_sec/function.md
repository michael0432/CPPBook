# Function

###### tags = `C++`

### 定義Funtion

```cpp=1
回傳的資料型態 funtion名稱(資料型態1 變數名稱1,資料型態2 變數名稱2){
    return 回傳值
}

// Example
int function1(int a, string b){
    return b.size();
}
```

### Local Variable
在function中，input的變數為local variable，即在該funtion中才能使用該變數。
```cpp=1
void function1(int n, int m){
    // 這裡的 n,m 與 main中的n,m沒有關聯性（存在memory不同的地方）
    n++; // n += 1
    m++; // m += 1
    return;
}

int main(){
    int n = 1;
    int m = 2;
    function1(n, m);
    cout << n << m << endl;
}
```

### Global Variable
Global Variable為整個程式中共用的變數，需要寫在所有的funtion外。
```cpp=1
int n = 1; // global variable

void function1(int m){
    // 這裡的 n,m 與 main中的n,m沒有關聯性（存在memory不同的地方）
    n++; // n += 1
    m++; // m += 1
    return;
}

int main(){
    int m = 2; // local variable
    function1(m);
    cout << n << m << endl;
}
```
如果這樣做呢？
```cpp=1
int n = 1; // global variable

void function1(int n,int m){
    // 這裡的 n,m 與 main中的n,m沒有關聯性（存在memory不同的地方）
    n++; // n += 1
    m++; // m += 1
    return;
}

int main(){
    int n = 3;
    int m = 2; // local variable
    function1(n,m);
    cout << n << m << endl;
}
```

### 小練習
* https://www.hackerrank.com/challenges/c-tutorial-functions/problem
