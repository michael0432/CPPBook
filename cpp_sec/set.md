# Set
###### tags = `Data Structure`

Set為集合，即將元素分成一團一團的類別。
舉例來說，我們可以把正整數分成奇數跟偶數兩個集合:

* 奇數:{1,3,5,7.....}
* 偶數:{2,4,6,8.....}

在寫程式或解決日常生活的問題時，我們時常會需要對集合做:
* 合併（兩個集合結合成一個集合）
* 找尋（找某個元素位於哪個集合）
* 交集
* 聯集
* 差集

### Set

#### 用Array儲存set
最直觀的方式是用array儲存一個集合，每一個Array代表一個集合

```cpp=1
class Set{
    vector<int> v(1000);
};
```

缺點：
* 做聯集、交集、差集等運算很慢

#### 用Hash Table儲存set
這時會想到可以使用Hash Table來儲存set，如此一來，在找尋特定元素時的速度就會變快，做聯集、交集、差集的時間也會變快。

```cpp=1
class Set{
    unordered_set<int> s(1000);
};
```

### Disjoint Set
Disjoint Set為互斥集，即是一堆集合們，彼此的element都不會重複，像上面舉的奇數偶數及為互斥集，由於集合們都沒有交集，因此交集、差集等運算都不必要，這種狀況下只需要考慮幾個運算:
* 合併
* 找尋

#### 用Array存disjoint set
假設現在set總共有n個element，便可以使用一個array存所有的element所在的set。

| Key | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 |
| -------- | -------- | -------- | -------- | -------- | -------- | -------- | -------- | -------- | -------- | -------- |
| Value     | 4 | 0 | 1 | 1 | 3 | 2 | 2 | 3 | 4 | 4 |

```cpp=1
vector<int> disjoint_set(10);
```

一開始還沒分團的時候，可以想像每個人是自己一組(總共有10個element，每個set皆為1個element)

| Key | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 |
| -------- | -------- | -------- | -------- | -------- | -------- | -------- | -------- | -------- | -------- | -------- |
| Value     | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 |


```cpp=1
void init(){
    for(int i=0 ; i<10 ; i++){
        disjoint_set[i] = i;
    }
}
```

##### Find

```cpp=1
int find(int x){
    return disjoint_set[x];
}
```

##### Equal

```cpp=1
int equal(int x, int y){
    return disjoint_set[x] == disjoint_set[y];
}
```

##### Union

```cpp=1
void union(int x, int y){
   if(!equal(x,y)){
       for(int i=0 ; i<10 ; i++){
           if(disjoint_set[i] == x){
               disjoint_set[i] = y;
           }
       }
   }
}
```

時間複雜度?

### Disjoint set Forest

上述的方法是用array存每個element分別屬於哪一個set，這裡改成存每個element的老大是誰。

原本的表示法:

| Key | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 |
| -------- | -------- | -------- | -------- | -------- | -------- | -------- | -------- | -------- | -------- | -------- |
| Value     | 0 | 0 | 1 | 1 | 3 | 4 | 1 | 3 | 1 | 1 |

以上述的方式表示:

| Key | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 |
| -------- | -------- | -------- | -------- | -------- | -------- | -------- | -------- | -------- | -------- | -------- |
| Value     | 1 | 1 | 8 | 2 | 4 | 5 | 2 | 4 | 8 | 2 |

![](https://i.imgur.com/PLzwodk.png)

#### Init

在還沒分團的時候，可以想成每個element的老大都是自己

| Key | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 |
| -------- | -------- | -------- | -------- | -------- | -------- | -------- | -------- | -------- | -------- | -------- |
| Value     | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 |

```cpp=1
void init(){
    for(int i=0 ; i<10 ; i++){
        disjoint_set[i] = i;
    }
}
```

#### Find

找到某個element是屬於哪個set(這個set的老大是誰)

```cpp=1
int find(int x){
    while(x != disjoint_set[x]){
        x = disjoint_set[x];
    }
    return x;
}
```

更簡單的寫法:
```cpp=1
int find(int x){
    return x == disjoint_set[x] ? x : find(disjoint_set[x]);
}
```

#### Equal

判斷兩個element是否是同一個set

```cpp=1
int equal(int x, int y){
    return find(x) == find(y);
}
```

#### Union

合併兩個set，並選擇一個element作為老大

![](https://i.imgur.com/ghrRa0x.png)


```cpp=1
void union(int x, int y){
    x = find(x); // x的老大
    y = find(y); // y的老大
    disjoint_set[y] = x;
}
```

### 小練習
* https://leetcode.com/problems/friend-circles/
* https://leetcode.com/problems/number-of-operations-to-make-network-connected/