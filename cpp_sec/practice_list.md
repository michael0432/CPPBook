# Practice - STL List (p4)

實作一個STL List，List由node所組成，要實作的method有:

* push_front()
* push_back()
* pop_front()
* pop_back()
* operator=()
* clear()
* operator==()

Source code (p4): https://github.com/michael0432/practiceCplusplus

node中包含next, prev, myVal:

```cpp=1
template< typename ValueType >
struct ListNode // list node
{
   ListNode *next;  // successor node, or first element if head
   ListNode *prev;  // predecessor node, or last element if head
   ValueType myVal; // the stored value, unused if head
};
```

next存下一個node的pointer，prev存上一個node的pointer，myVal存值。

List由許多node所組成，其中，當list為空時，裡面仍然會有一個node myHead，myHead中不會存值，且不計入list的大小計算，用途為確保此list為空時也能找到開頭的node做操作。

```cpp=1

template< typename Ty >
class ListVal
{
public:
   ListVal() // initialize data
      : myHead(),
        mySize( 0 )
   {
   }
   node* myHead; // pointer to head node
   size_type mySize; // number of elements
};

template< typename Ty >
class list{ // bidirectional linked list
private:
   ListVal<Ty> myData;
};

```

## Example

### Empty List
List為空時，只有myHead一個node，mySize = 0

![](https://i.imgur.com/J7DnKbi.png)


### Push_back(n1)
將n1放到list的最後方，mySize = 1，此時list的front為n1 (myHead->next)；list的back為n1 (myHead->prev)

![](https://i.imgur.com/2H8Ew9a.png)

### Push_back(n2)
將n2放到list的最後方，mySize = 2，此時list的front為n1 (myHead->next)；list的back為n2 (myHead->prev)

![](https://i.imgur.com/pQfZt71.png)

### Push_front(n3)
將n3放到list的最前方，mySize = 3，此時list的front為n3 (myHead->next)；list的back為n2 (myHead->prev)

![](https://i.imgur.com/zOANeZi.png)

### pop_back()
將list的最後方的node移除，mySize = 2，此時list的front為n3 (myHead->next)；list的back為n1 (myHead->prev)
![](https://i.imgur.com/VSQTj00.png)

## 測試程式是否正確

```
g++ -std=c++11 Main.cpp -o p4.exe
p4.exe

```