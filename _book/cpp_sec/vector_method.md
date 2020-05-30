# Vector常用method
###### tags = `Data Structure`

### operator[]
跟array一樣，Vector可以用[]來拿對應index的值
```cpp=1
#include<vector> 

int main(){
    vector<int> v = {1,2,3};
    cout << v[0] << v[1] << v[2] << endl;   
}
```
### size()
取得vector的大小。
* 時間複雜度：O(1)

```cpp=1
#include<vector> 

int main(){
    vector<int> v = {1,2,3};
    cout << v.size() << endl;   
}
```
### iterator
iterator是一種比較聰明的pointer，可以指到一個資料結構中的任何位置，然後對該位置的資料進行操作。
要印出array的值時：

```cpp=1
// 用index
int arr[] = {1, 2, 3, 4, 5};
int size = 5;
for(int i=0 ; i<size ; i++){
    cout << arr[i] << endl;
}

// 用pointer
int *begin = arr + 0;
int *end = arr + size;
int *ptr;
for(ptr=begin ; ptr!=end ; ptr++){
    cout << *ptr << endl;   // 1, 2, 3, 4, 5
}
```
同理，要印出整個Vector除了使用index外，也可以使用pointer(iterator):

```cpp=1
// 用index
vector<int> vec = {1, 2, 3, 4, 5};
for(int i=0 ; i<vec.size() ; i++){
    cout << vec[i] << endl;
}

// 用iterator
vector<int>::iterator begin = vec.begin(); // begin指的是index 0的位置
vector<int>::iterator end = vec.end(); // end指的是v.size()-1後面那格的位置

vector<int>::iterator it;
for(it=begin ; it!=end ; it++){
    cout << *it << endl;
}

// 用auto可以不要寫那麼複雜，auto表示自動抓 = 後面值的資料型態
for(auto it=vec.begin() ; it!=vec.end() ; it++){
    cout << *it << endl;
}
```

### push_back(new data)
將一個element放到vector的最後方。
* 時間複雜度：大部分是O(1)，少數情況是O(n)

```cpp=1
#include<vector> 

int main(){
    vector<int> v = {1,2,3};
    v.push_back(4);
    cout << v[0] << v[1] << v[2] << v[3] << endl;   
}
```

### pop_back()
將最後一個element移除。
* 時間複雜度：O(1)

```cpp=1
#include<vector> 

int main(){
    vector<int> v = {1,2,3};
    v.pop_back();
    cout << v.size() << endl;   
}
```
### insert(插入位置的iterator, data)
將值插入vector的指定位置
* 時間複雜度：O(n)

```cpp=1
#include<vector> 

int main(){
    vector<int> v = {1,2,3};
    v.insert(vec.begin()+2, 0);
    for(int i=0 ; i<v.size() ; i++){
        cout << v[i] << endl;   
    }
}
```

### erase(開始iterator, 結束iterator)
將值插入vector的指定位置
* 時間複雜度：O(n)

```cpp=1
#include<vector> 

int main(){
    vector<int> v = {1,2,3,4,5};
    v.erase(vec.begin()); // 移除第0個
    for(int i=0 ; i<v.size() ; i++){
        cout << v[i] << endl;   
    }
    v.erase(vec.begin(),vec.begin()+2); // 移除前兩個
    for(int i=0 ; i<v.size() ; i++){
        cout << v[i] << endl;   
    }
}
```
### clear()
將vector清空

```cpp=1
#include<vector> 

int main(){
    vector<int> v = {1,2,3,4,5};
    v.clear()
}
```

### sort(開始iterator, 結束iterator)
將vector的某個段落排序，需要include algorithm
* 時間複雜度: O(nlogn)

```cpp=1
#include<vector>
#include<algorithm>

int main(){
    vector<int> v = {1,3,2,5,4};
    sort(v.begin(), v.end());
    for(int i=0 ; i<v.size() ; i++){
        cout << v[i] << endl;   
    }
}
```