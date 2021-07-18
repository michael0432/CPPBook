# Function Pointer

function pointer為存function起始memory address的變數，可以用來將function作為input傳進另一個function中使用。

function pointer可以表示為:

```cpp=1
// 一個function
int Add(int a, int b)
{
    return a+b;
}

// function pointer，宣告一個output為int，input為兩個int的function pointer
int (*functionAddPtr)(int a, int b); 
functionAddPtr = &Add; // 指到Add這個function
```

## exanple

``` cpp=1
int Add(int a, int b)
{
    return a+b;
}

int Mul(int a, int b)
{
    return a*b;
}

int Cal(int a, int b, int (*fp)(int, int))
{
    // input為function pointer
    // 第一個int為此function pointer指到的function回傳值為int
    // (int, int)指此function pointer指到的function輸入值為兩個int
    return fp(a,b);
}

int main()
{
    int a = 3, b = 5;
    printf("%d", Cal(a, b, Add());
    printf("%d", Cal(a, b, Mul());
}
```

## Typedef

typedef用於定義變數型態的名稱，可以用於簡化名稱冗長的變數型態:

```cpp=1
typedef int myint;
typedef char mychar;

int main()
{
    myint i = 3; // int i = 3
    mychar c = 'a' // char c = 'a'
}
```

typedef通常用於定義struct或是function pointer這種定義上名稱較難使用的變數

### typedef struct

```cpp=1
// 不使用 typedef
struct myStruct
{
    int a;
    float b;
}

int main()
{
    struct myStruct a;
}

// 使用 typedef
typedef struct myStruct
{
    int a;
    float b;
}myStruct;

//其實意思相等於

struct myStruct
{
    int a;
    float b;
}
typedef (struct myStruct) mySturct

int main()
{
    myStruct a;
}
```

## typedef function pointer

使用typedef定義function pointer:

```cpp=1
typedef int(*functionAddPtr)(int, int);

int main()
{
    functionAddPtr addFunction = &Add; // int(*functionAddPtr)(int, int) = & Add
    int result = (*AddFunction)(1234,5678);
}
```
