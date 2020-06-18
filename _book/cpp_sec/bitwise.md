# 位元運算
###### tags = `C++`

### Bitwise operator

位元運算子用於表示bit運算的邏輯關係

* & : and
* | : or
* ^ : xor
* | : not

```cpp=1
#include <iostream>
using namespace std;

int main() { 
    cout << "AND ：" << endl; 
    cout << "0 AND 0\t\t" << (0 & 0) << endl; 
    cout << "0 AND 1\t\t" << (0 & 1) << endl; 
    cout << "1 AND 0\t\t" << (1 & 0) << endl; 
    cout << "1 AND 1\t\t" << (1 & 1) << endl; 

    cout << "OR ：" << endl; 
    cout << "0 OR 0\t\t" << (0 | 0) << endl; 
    cout << "0 OR 1\t\t" << (0 | 1) << endl; 
    cout << "1 OR 0\t\t" << (1 | 0) << endl; 
    cout << "1 OR 1\t\t" << (1 | 1) << endl; 

    cout << "XOR ：" << endl; 
    cout << "0 XOR 0\t\t" << (0 ^ 0) << endl; 
    cout << "0 XOR 1\t\t" << (0 ^ 1) << endl; 
    cout << "1 XOR 0\t\t" << (1 ^ 0) << endl; 
    cout << "1 XOR 1\t\t" << (1 ^ 1) << endl; 

    cout << "NOT ：" << endl; 
    cout << "NOT 0\t\t" << (!0) << endl; 
    cout << "NOT 1\t\t" << (!1) << endl; 

    return 0;
}
```

在C/C++中，可以對不同的資料型態做位元運算：
```cpp=1
int a = 3; // 11
int b = 2; // 10
int c = a & b; // 11 & 10 -> 10
cout << c << endl;
```

### 左移與右移
#### << : 左移，將所有bit往左移
#### >> : 右移，將所有bit往右移

```cpp=1
int n = 1; // 00...0001
n = n << 1; // 00..0010
n = n << 2; // 00..1000
```

### 小練習
* https://leetcode.com/problems/hamming-distance/
* https://leetcode.com/problems/number-complement/
