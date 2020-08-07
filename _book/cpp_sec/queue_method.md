# Queue 常用method

###### tags = `Data Structure`

### empty()
確認queue是否為空，回傳true/false
```cpp=1
queue<int> q;
if(q.empty()){

}
```
### push(放入的資料)
將資料放入queue的尾端
```cpp=1
queue<int> q;
q.push(1);
```
### pop()
將資料從queue的front移除
```cpp=1
queue<int> q;
q.push(1);
q.push(2);
q.pop();
```
### size()
回傳queue的大小
```cpp=1
queue<int> q;
q.push(1);
cout << q.size() << endl;
```
### front
回傳queue的front的資料
```cpp=1
queue<int> q;
q.push(1);
q.push(2);
cout << q.front() << endl;
```
### back
回傳queue的rear的資料
```cpp=1
queue<int> q;
q.push(1);
q.push(2);
cout << q.front() << endl;
```
