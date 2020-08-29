# Practice - Template & Operator (p3)

將上次練習的Huge Integer改成使用Template及Operator的寫法，這次練習分成兩個部分，第一個部分是將vector內存的資料型態設為template T；第二個部分是將HugeInteger內存的資料型態設為template T。

Source code (p3): https://github.com/michael0432/practiceCplusplus

## Main_1.cpp, HugeInteger_1.h

```cpp=1
template< typename T >
class HugeInteger
{
private:
   vector< T > integer;
   //這裡的T為int, unsigned int...等
}
```

## Main_2.cpp, HugeInteger_1.h

```cpp=1
template< typename T >
class HugeInteger
{
private:
   T integer;
   //這裡的T為vector<int>, vector<unsigned int>...等
}
```

其他的要求與上次的練習完全相同，此次的HugeInteger各個method的程式碼直接寫在.h檔內即可。

## 測試程式是否正確

測試Main_1.cpp, HugeInteger_1.h:

```
g+= -std=c++11 Main_1.cpp -o p3.exe
p3.exe
測資為Test cases.txt
答案會存在Result.txt
```

測試Main_2.cpp, HugeInteger_2.h:

```
g+= -std=c++11 Main_2.cpp -o p3_2.exe
p3_2.exe
測資為Test cases.txt
答案會存在Result.txt
```

這兩個exe會測試Result.txt中的答案是否正確

```
執行
Verification - x64.exe
或
Verification - x86.exe
```

