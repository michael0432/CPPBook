# Problem - 觀念題2
###### tags = `APCS`

## 遞迴

### Q1: 請問下列程式碼中會輸出幾個*?

```cpp=1
int func(int n)
{
    if(n > 0)
    {
        printf("*\n");
    }
    else
    {
        return 1;
    }
    return func(n-1);
}

int main()
{
    func(5);
}
```

### Q2: 請問下列程式碼中會輸出幾個*?

```cpp=1
int func(int n, int k)
{
    if(n > 0 && k < 5)
    {
        printf("*\n");
    }
    else
    {
        return 1;
    }
    return func(n-1, k+1);
}

int main()
{
    func(4, 3);
}
```

### Q3: 請問下列程式碼中會輸出幾個*?

```cpp=1
int func(int n, int k)
{
    if(n > 0 && k < 5)
    {
        printf("*\n");
    }
    else
    {
        return 1;
    }
    return func(n-1, k) + func(n, k+1);
}

int main()
{
    func(2, 3);
}
```

### Q4: 下列程式的輸出為何

```cpp=1
void func(int n)
{
    printf("%d\n", n);
    if(n <= 2) 
        return;
    if(n % 2 == 1)
        return func(n + 1);
    else
        return func(n / 2);
}

int main()
{
    func(13);
}
```

### Q5: 下列程式的輸出為何

```cpp=1
int func(int n)
{
    if(n <= 0) return 0;
    if(n % 2 == 0)
        return 1;
    else
        return func(n-1) + func(n-2);
}

int main()
{
    printf("%d\n", func(5));
}
```

## 變數

### Q6: 下列程式的輸出為何

```cpp=1
int a = 2;
int b = 3;

void func()
{
    b = 4;
}

int main()
{
    printf("%d\n", a + b);
    func();
    printf("%d\n", a + b);
}
```

### Q7: 下列程式的輸出為何

```cpp=1
int a = 20, b = 30;

int func(int x)
{
    int a = 10;
    return x + a;
}

int main()
{
    b = func(b);
    printf("%d\n", func(b));
}
```

## ANS
### Q1: 5
### Q2: 2
### Q3: 5
### Q4: 13 14 7 8 4 2
### Q5: 2
### Q6: 5 6
### Q7: 50

