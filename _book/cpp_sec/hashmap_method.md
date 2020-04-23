# HashMap常用Method

###### tags = `Data Structure`

C++中使用unordered_map來表示HashMap

```cpp=1
#include<unordered_map>
unordered_map<string, int> m; // 第一個值為key的資料型態, 第二個值為value的資料型態
```

### Insert
```cpp=1
m["key"] = 1;
```


### Edit
```cpp=1
m["key"] = 1;
```

### Delete
```cpp=1
m.erase("key");
```
### Search
```cpp=1
cout << m["key"];
```

### Empty
```cpp=1
if(m.empty()){

}
```
### 用iterator遍歷
```cpp=1
for(unordered_map<string, int>:: it = m.begin(); it!=m.end(); it++){
    cout << it->first << endl; // key
    cout << it->second << endl; // value
}
```
或是
```cpp=1
for(auto it = m.begin(); it!=m.end(); it++){
    cout << it->first << endl; // key
    cout << it->second << endl; // value
}
```

### 若key為自己定義的class

錯誤的寫法: 沒有hash function
```cpp=1
class CustomClass{
public:
    int a;
    int b;
};

int main(){
    unordered_map<CustomClass, int> m; // compile error
}
```

正確的寫法: 要自己定義hash function
需要兩個元素:
* operator ==
* hash
```cpp=1
struct CustomClass{
public:
    int a;
    int b;
    bool operator==(const CustomClass& c) const
    {
        return a == c.a && b == c.b;
    }
};

class hashFunction{
public:
    size_t operator()(const Person& p) const
    { 
        return p.a + p.b; 
    } 
}

int main(){
    unordered_map<CustomClass, int> m;
}
```