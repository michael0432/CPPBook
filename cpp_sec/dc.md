# Divide and Conquer
###### tags = `Algorithm`

* Divide : 把大問題切成小問題
* Conquer : 把小問題的解合併成大問題的解

其實就是dfs的概念


### Binary Search

* https://leetcode.com/problems/binary-search/

* Divide : 把Array切成兩段，決定要往左找還是往右找
* Conquer : 回傳左邊或右邊的解
* 時間複雜度: ?

![](https://i.imgur.com/W8tlCUz.png)


### Merge Sort

* Divide : 把Array切成兩段，左右各自排序
* Conquer : 將左右兩個Array合併
* 時間複雜度: ?

![](https://i.imgur.com/7bZs2tn.png)

### Quick Sort

* Divide : 在數列中任意選一個數，然後調整數列，讓該數字左邊的數都比他小，右邊的數都比他大，並對左右數列做quick sort
* Conquer : X
* 時間複雜度: ?

![](https://i.imgur.com/Ay9aoQc.png)


### C++ Sort

```cpp=1
#include<algorithm>

vector<int> v;
sort(v.begin(), v.end(), greater<int>);
```

```cpp=1
class CustomClass{
public:
    int x;
    int y;
};

vector<CustomClass> v;
sort(v.begin(), v.end(), [](const CustomClass& a, const CustomClass& b){
    return a.x < b.x;
});

```

### 小練習

* https://leetcode.com/problems/first-bad-version/
* https://leetcode.com/problems/search-insert-position/
* https://leetcode.com/problems/search-a-2d-matrix-ii/


