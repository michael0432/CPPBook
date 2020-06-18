# HashMap

###### tags = `Data Structure`

HashMap是儲存Key-Value對應的資料結構，利用Hash function將key對應到value，找到特定資料的時間複雜度為O(1)。

![](https://i.imgur.com/uQqyzME.png)

### 用array實作hash map
我們的目的是在O(1)的時間內對資料進行:
* 新增
* 刪除
* 查詢

最直覺的做法就是用一個很大的Array，Index為key裡面存的值為value
| Key | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 |
| -------- | -------- | -------- | -------- | -------- | -------- | -------- | -------- | -------- | -------- | -------- | -------- | -------- |
| Value     | 4 |  |  |  | 3 |  | 6 |  | 4 |  | 4 | 

這樣做有幾個缺點:
* 浪費記憶體空間
* Key一定要是非負的整數

### Hash Table
Hash Table為了解決上述的問題，只儲存有用到的Key，因此需要使用hash function
![](https://i.imgur.com/RDW0Q7V.png)

#### Hash Function
Hash function的功能為將每個不同的key值對應到不同的value，並且將冗長的key簡化，舉例來說:

    HashFunction("12345") = 1 -> 用hashsum去找對應的value
    HashFunction("sdgasdg") = 2
    HashFunction("agasd") = 3
    HashFunction("akvnkaagafdfgar") = 4

利用hash function就可以比較簡單的將複雜的key快速找到對應的value。但是仍然會發生一個嚴重的問題:
* |U| > m
    * Collision : 兩個不同的key k1, k2, HashFunction(k1) = HashFunction(k2)
    * 解決方法:
        * 用link-list把hash到同一個值的key串起來
        * 用probing method來找空的地方放資料

![](https://i.imgur.com/GsNE9hW.png)


### 小練習
* https://leetcode.com/problems/jewels-and-stones/
* https://leetcode.com/problems/contains-duplicate/
* https://leetcode.com/problems/happy-number/
* https://leetcode.com/problems/two-sum/
* https://leetcode.com/problems/flip-columns-for-maximum-number-of-equal-rows/