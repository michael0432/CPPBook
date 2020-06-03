# Stack常用method
###### tags = `Data Structure`
stack是一個C++的Container，屬於Standard Library(STL)，需要include。
```cpp=1
#include <stack>

int main(){
    stack<int> st;
}
```
### push(data)
將一筆資料放入stack
```cpp=1
#include <stack>

int main(){
    stack<int> st;
    st.push(3);
}
```
### pop()
將最上面的一筆資料拿出stack
```cpp=1
#include <stack>

int main(){
    stack<int> st;
    st.push(3);
    st.pop();
}
```
### top
取得stack最上面（最後放入的）資料
```cpp=1
#include <stack>

int main(){
    stack<int> st;
    st.push(3);
    cout << st.top();
}
```

### empty()
如果size是0回傳true，其他情況回傳false

```cpp=1
#include <stack>

int main(){
    stack<int> st;
    st.push(3);
    if(!st.empty()){
        cout << st.top();
    }  
}
```

### size()
回傳stack的大小
```cpp=1
#include <stack>

int main(){
    stack<int> st;
    st.push(3);
    cout << st.size() << endl;  
}
```