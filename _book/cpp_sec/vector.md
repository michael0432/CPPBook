# Vector

###### tags = `Data Structure`

### Vector
Vector是進階版的Array，是C++C++ 標準程式庫中的一個 class，可以視為會自動擴增大小的Array。當使用Array時，需要先宣告Array的大小，當資料量超過Array的大小時，需要先重新宣告一個更大的Array，再將資料從舊的Array複製到新的Array，Vector已經內建了這個操作。

宣告方式：
```cpp=1
#include <vector> // inlcude library
using namespace std;

vector<資料型態> 名稱;
vector<資料型態> 名稱{1,2,3};
vector<資料型態> 名稱(初始值,大小);
```


```cpp=1
#include <vector>
using namespace std;

int main(){
    vector<int> v1;
    vector<int> v2{1,2,3};
    vector<int> v3(0,3);
}
```


### Vector and Array
|  | Array | Vector |
| -------- | -------- | -------- |
| Access     | O(1)     | O(1) | 
| Find     | O(logn)     | O(logn)  | 
| Insert     |  O(n)    | O(n)   |
| Delete     |  O(n)    | O(n) |

### 小練習
* https://www.hackerrank.com/challenges/vector-sort/problem
* https://leetcode.com/problems/squares-of-a-sorted-array/
* https://leetcode.com/problems/move-zeroes/
* https://leetcode.com/problems/flipping-an-image/
* https://leetcode.com/problems/toeplitz-matrix/