# Linked List

###### tags = `Data Structure`

### Linked List
Linked List使用Node來記錄、儲存資料，並且利用Node中的Pointer指向下一筆資料，用Null表示終點。
![](https://i.imgur.com/PyBAUPI.png)
![](https://i.imgur.com/46bnbI2.png)

在使用Linked List時，只需要有第一個Node的Address，就可以依序找到後面所有的資料。

```cpp=1
class Node{
public:
    Node *next;
    int data;
}
```

遍歷Linked List:
```cpp=1
void traverse(Node *head){
    while(head != NULL){
        cout << head->data;
        head = head->next;
    }
}
```
### Array and Linked List
|  | Array | Linked List |
| -------- | -------- | -------- |
| Access     | O(1)     | O(n) | 
| Find     | O(logn)     | O(n)  | 
| Insert     |  O(n)    | O(1)   |
| Delete     |  O(n)    | O(1) |


### 小練習
* https://leetcode.com/problems/middle-of-the-linked-list/
* https://leetcode.com/problems/reverse-linked-list/
* https://leetcode.com/problems/merge-two-sorted-lists/

