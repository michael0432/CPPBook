# Priority Queue常用method
###### tags = `Data Structure`

### 宣告
宣告priority queue需要三個變數：queue中的資料型態、用什麼資料結構儲存heap、比較priority的方式。

```cpp=1
#include <queue>
priority_queue<int, vector<int>, greater<int>()> pq;
// int : 資料型態為int
// vector<int> : 用vector存heap，也可以用deque
// greater<int>() : max heap
// less<int>() : min heap
```

### Push
把element放進priority queue中

```cpp=1
pq.push(1);
```

### Push
把element放進priority queue中

```cpp=1
pq.push(1);
```

### Pop
將heap的root刪除

```cpp=1
pq.pop();
```

### Top
回傳heap的root

```cpp=1
cout << pq.top() << endl;
```

### Empty
回傳priority queue是否為空

```cpp=1
if(pq.empty()){

}
```

### Size
回傳priority queue的大小

```cpp=1
cout << pq.size() << endl;
```

### Custom Class Priority Queue
有些時候，priority queue有可能需要存自己定義的class，這時需要自己定義比大小的function。

```cpp=1
class customClass{
    int x;
    int y;
};

class Compare{

};

priority_queue<customClass, vector<customClass>, Compare> pq
class Compare
{
public:
    bool operator() (const customClass& a, const customClass& b)
    {
        return a.x > b.x; // 從小排到大
    }
};
```

### 注意！ C++的priority queue不支援更改權重

