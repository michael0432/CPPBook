# Practice - STL HugeInteger

利用上次練習的Vector實作HugeInteger的+,-,*,/,%運算
Prob : https://github.com/michael0432/leetcode/tree/master/practice/prob/p2
Ans : https://github.com/michael0432/leetcode/tree/master/practice/ans/p2

![](https://i.imgur.com/WhJhVHm.png)
![](https://i.imgur.com/DSA5RwQ.png)

HugeInteger::convert

![](https://i.imgur.com/Ww4yYlc.png)


## Implement Method

### less
比較此HugeInteger是否小於input HugeInteger

### equal
比較此HugeInteger是否等於input HugeInteger

### add
將兩個HugeInteger加起來(已完成)

### substract
將兩個HugeInteger相減

### multiply
將兩個HugeInteger相乘

### divide
將兩個HugeInteger相除

### mod
將此HugeInteger % input HugeInteger

## 測試程式是否正確

### 自己測試答案

```
g+= -std=c++11 Main.cpp HugeIntegerImpl.cpp VectorImpl.cpp -o p2.exe
p2.exe
測資為Test cases.txt
答案會存在Result.txt
```

### 自動對答案

```
直接執行
Verification - x64.exe
或
Verification - x86.exe
```