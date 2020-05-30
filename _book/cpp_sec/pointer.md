# Pointer and Reference

###### tags = `C++`

### Pointer
變數提供一個記憶體儲存空間，其中，一個變數裡面存的資訊有：
* 資料型態
* 記憶體位址
* 變數的值

如果想知道變數的位址為何，可以使用&來取得：

```cpp=1
#include <iostream> 
using namespace std; 

int main() { 
    int n = 10; 

    cout << "n 的值：" << n << endl << "n的位址：" << &n << endl; 

    return 0; 
}
```

在C++中，可以使用Pointer存取記憶體位址：
```cpp=1
資料型態* 指標名稱;
int n;
int* a = &n;
```
```cpp=1
#include <iostream> 
using namespace std; 

int main() { 
    int n = 10; 
    int *p = &n;
    cout << "n的位址：" << &n << endl << "p存的值：" << p << endl; 
    cout << "p位址處的物件值：" << *p << endl;

    return 0; 
}
```

若改變*p的值，即是改變n的值，因為是更改同一個記憶體區塊儲存的值。
```cpp=1
#include <iostream> 
using namespace std; 

int main() { 
    int n = 10; 
    int *p = &n ; 

    cout << "n = " << n << endl
         << "*p = " << *p << endl; 

    *p = 20; 

    cout << "n = " << n << endl
         << "*p = " << *p << endl;

    return 0; 
}
```


# 小練習
* https://www.hackerrank.com/challenges/c-tutorial-pointer/problem

### Pointer and Array
Array將資料存在一段連續的記憶體位置，而使用Pointer可以存取記憶體位址，因此，可以將Pointer指向Array開始的位址，之後就可以一一遍歷。
```cpp=1
#include <iostream> 
using namespace std; 

int main() { 
    int size = 2;
    int arr[] = {1,2};
    int *ptr;
    ptr = arr; //指向array

    for(int i=0 ; i<size ; i++){
        cout << "位址: arr[" << i << "] :" << &arr[i] << ", " << ptr+i<< endl;
    }
}
```
也就是說，我們可以將一個Array用Pointer來表示：
```cpp=1
#include <iostream> 
using namespace std; 

int main() { 
    int size = 2;
    int arr[] = {1,2};

    for(int i=0 ; i<size ; i++){
        cout << "位址: arr[" << i << "] :" << &arr[i] << ", " << arr+i << endl;
    }
    for(int i=0 ; i<size ; i++){
        cout << "值: arr[" << i << "] :" << arr[i] << ", " << *(arr+i) << endl;
    }
}
```

### Pointer ++, --
對pointer進行 ++或-- 的運算時，會將pointer移到前一格，因此可以用pointer的方式遍歷array
```cpp=1
#include <iostream> 
using namespace std; 

int main() { 
    int size = 2;
    int arr[] = {1,2};
    int *begin = arr + 0;
    int *end = arr + size;
    int *ptr;
    for(ptr=begin ; ptr!=end ; ptr++){
        cout << *ptr << endl;
    }
}
```

### Pointer and 2D Array
同樣的，2D的Array也可以用pointer來表示：

| Array |  用Pointer表示 | 用Pointer取值 |
| -------- |  -------- | -------- |
| a[0][0]     | **a     | * (*(a+0)+0) |
| a[0][1]     | *(*a+1)     | * ( *(a+0)+1) |

### Reference
Reference是物件的別名，也就是一個變數的替代名稱。
```cpp=1
int n = 10;
int *p = &n; //Pointer，儲存n的位址
int &r = n; // Reference，n的別名
int &r; // wrong
```

Reference的變數與原本的變數都對應到同一個記憶體空間，因此對任何一項改值，兩者的值皆會更改。
Example:
```cpp=1
#include <iostream>
using namespace std;

int main() {
    int n = 10;
    int &r = n;

    cout << "n：" << n << endl
         << "r：" << r << endl;

    r = 20;

    cout << "n：" << n << endl
         << "r：" << r << endl;

    return 0;
}
```








