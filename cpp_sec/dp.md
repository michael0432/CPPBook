# Dynamic Programming
###### tags = `Algorithm`

### 基本概念
Dynamic Programming(動態規劃)是一種利用小問題解大問題的演算法，藉由把小問題的答案記起來，避免計算重複的解，用空間換取時間。

### 費式數列

* https://leetcode.com/problems/fibonacci-number/

#### Recursive(Top Down)

* F(n) = F(n-1) + F(n-2)
    
```cpp=1
int fib(int n){
    if(n == 0) return 0;
    if(n == 1) return 1;
    return fib(n-1) + fib(n-2);
}
```

時間複雜度?

![](https://i.imgur.com/Nrqe5M0.png)

* 重複算了很多東西! 如果記下來呢?

```cpp=1
int fib(vector<int>& mem, int n){
    if(n == 0) return 0;
    if(n == 1) return 1;
    if(mem[n] != -1){
        return mem[n];
    }
    else{
        mem[n] = fib(n-1) + fib(n-2);
        return mem[n];
    }
}

int main(){
    int n;
    cin << n;
    vector<int> mem(n+1, -1);
    return fib(mem, n);
}
```

時間複雜度?


#### Bottom Up

從上面的方法發現，避免重複計算最簡單的方式就是從f(0)開始計算到f(n)，並把過程中的結果都記起來。

```cpp=1
int fib(int n){
    if(n == 0) return 0;
    vector<int> mem(n,-1);
    mem[0] = 0;
    mem[1] = 1;
    for(int i=2; i<=n ; i++){
        mem[i] = mem[i-1] + mem[i-2];
    }
    return mem[n];
}
時間複雜度?

```

#### 思路

Dynammic Programming的題目通常比較難想到解法，可以嘗試用以下的思路

1. 找出recursive function
2. 確認edge case
3. 確認要用top down還是bottom up

### Climb Stairs

* https://leetcode.com/problems/climbing-stairs/

1. 找出recursive function
2. 確認edge case
3. 確認要用top down還是bottom up

時間複雜度?

### Unique Path

* https://leetcode.com/problems/unique-paths/

1. 找出recursive function
2. 確認edge case
3. 確認要用top down還是bottom up

時間複雜度?

### LCS

* https://leetcode.com/problems/longest-common-subsequence/

1. 找出recursive function
    * 把text1跟text2拆成兩個部分: text1 = sub1 + e1, text2 = sub2 + e2，e1跟e2為最後一個字元
    * LCS(text1, text2)表示text1, text2的LCS
    * 考慮幾個case:
        * LCS包含e1，不包含e2 : LCS(text1, text2) = LCS(text1, sub2)
        * LCS包含e2，不包含e1 : LCS(text1, text2) = LCS(sub1, text2)
        * LCS包含e1, e2 : LCS(text1, text2) = LCS(sub1, sub2) + e1
        * LCS不包含e1, e2 : LCS(text1, text2) = LCS(sub1, sub2)
    * if e1 != e2
        * LCS(text1, text2) = max(LCS(text1, sub2), LCS(sub1, text2))
    * else
        * LCS(sub1, sub2) + e1
        
3. 確認edge case
    * LCS("", "") = ""
    
4. 確認要用top down還是bottom up
    * 用一個mem[text1.size()][text2.size()]存

時間複雜度?

### LIS

* https://leetcode.com/problems/longest-increasing-subsequence/

#### DP

1. 找出recursive function
2. 確認edge case
3. 確認要用top down還是bottom up

時間複雜度?

#### Greedy

1. 找出每個數字「最好情況」可以被擺在哪

### Maximum Subarray

* https://leetcode.com/problems/maximum-subarray/

#### DP
1. 找出recursive function
2. 確認edge case
3. 確認要用top down還是bottom up

### 0-1 背包問題

每種物品只能放進背包一個或是不放進背包，在背包承重量有限的情況下，如何放進總價值最高的物品？

| 物品 | 重量 |  價值 |
| -------- | -------- |-------- |
|   0   | 1 |  1  | 
|   1   | 2 |  4  | 
|   2   | 3 |  6  | 
|   3   | 5 |  3  | 
|   4   | 7 |  10  | 

* 直覺：Greedy
    * 可行嗎
* DP
    * 找出recursive function
        * f(n, w) = max(f(n-1, w), f(n1, w-weight[n]) + value[n]);
    * 確認edge case
    * 確認要用top down還是bottom up


* https://leetcode.com/problems/coin-change-2/description/


### 小練習
* https://leetcode.com/problems/house-robber/
* https://leetcode.com/problems/matrix-block-sum/
* https://leetcode.com/problems/stone-game/
* https://www.hackerrank.com/challenges/construct-the-array/problem





