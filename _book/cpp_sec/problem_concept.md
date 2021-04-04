# Problem - 觀念題
###### tags = `APCS`

## 資料型態/輸出輸入

### Q1: A) int B) char C) float D) double 請將這四種變數型態，依在記憶體中所佔的位元數大小，由大至小排列:

a) A>C>D>B
b) D>B>C>=A
c) D>C>>=A>B
d) B>D>C>=A


### Q2: 請問下列程式碼中有效的敘述有幾行?
```cpp=1
// hello world!
int i;
int f;
/* float g; */
```

a) 1行
b) 2行
c) 3行
d) 4行

    
### Q3: 請問下列程式碼何處有誤?

```cpp=1
cout<<"請輸入生日: "ex: 100/01/01":";
```

### Q4: 請將45以二進位及16進位表示

### Q5: 請問下列程式碼何者有誤?

```cpp=1
#include <iostream>
using namespace std;
int main(){
    cout << "Hello! World!"
    return 0;
}
```

### Q6: 請問下列程式碼會輸出甚麼?
```cpp=1
int i = 100, j = 3;
float Result;
Result = i/j;
printf("%f", Result);
```
a) 33
b) 33.000000
c) 33.333333
d) 33.0000000000 

## 流程控制

### Q1: 變數a原先的值為0，請從下列敘述中，選出變數a的值結果不為1的敘述:

a) a++;
b) ++a:
c) a = a >= 1 ? 0: 1;
d) a = sizeof(double)/4;


### Q2: 下方敘述中的變數都為int型態，請問運算結果? Ans值為多少?

    a = 1;
    b = 2;
    c = 3;
    Ans = a/b + c/b - (!(a&&b) ? 0 : 1) + (c+c+a)%b;
    
a) 1
b) 2
c) 3
d) 4

### Q3: 試比較底下兩段迴圈程式碼的執行結果:
a)
```cpp=1
for (int i = 0; i < 10; i++)
{
    cout << 1;
    if(i == 5)
        break;
}
```
b)
```cpp=1
for (int i = 0; i < 10; i++)
{
    cout << i;
    if(i == 5)
        continue;
}
```

 ### Q4: 試問下列程式碼中，最後k值會為多少? 試說明之。
 ```cpp=1
 int k = 10;
 while (k <= 25)
 {
     k++;
 }
 cout << k;
 ```
 
 ### Q5: 下面的程式無法正確判斷考生是否合格，請問它出了什麼問題?
  ```cpp=1
  #include <iostream>
  int main(){
      int int_math, int_physical;
      cout << "請輸入數學與物理分數" << endl;    
      cin >> int_math >> int_physical;
      if(int_math >= 60 & int_physical >= 60)
          cout << "該名考生合格!";
      else
          cout << "該名考生不合格!";
      return 0;
  }
 ```
 
 
 ## 迴圈、陣列
 
 ### Q1: 下列程式碼何處有誤
 ```cpp=1
 int a[2,3] = { {1,2,3}, {4,5,6} };
 int i, j;
 for(i = 0; i < 2; i++)
     for(j = 0; j < 3; j++)
         cout << a[i,j];
```

### Q2: 請指出下列程式碼何處有誤，並改正使其能編譯成功
```cpp=1
#include <iostream>
int main(void)
{
    int i;
    
    char str[30] = "this is my first program.";
    char str1[20] = "my company is ZCT.";
    cout << "原始字串 str = " << str << endl;
    cout <<"字串 str1 = " << str1;
    str1 = str;
    cout << "複製字串 str1 = " << str1;
    return 0;
}
```

### Q3: 下列程式碼輸出結果為何?
``` cpp=1
int n1[5];
int i;
for(i = 0; i < 5; i++)
    n1[i] = i+6;
cout << n1 [3];
```

### Q4: 下列程式碼何處有誤?
``` cpp=1
#include <iostream>
using namespace std;
int main()
{
    int arr[5] = {1,2,3,4,5};
    int i;
    for(i = 1; i <= 5; i++)
        cout << "a["<<i<<"] = " << arr [i] << endl;
    return 0;
}
```