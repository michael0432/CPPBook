# Iterator
###### tags = `Software Engineering`
> The iterator pattern is a design pattern in which an iterator is used to traverse a container and access the container's elements.


![](https://i.imgur.com/9EYVPyz.png)


## Example
在一個軟體中，會使用到兩種不同的資料結構，StringList及NodeList。
* StringList: 由多個string組成的list
    * 可以使用length()拿到目前List中有幾個string。
    * 可以使用get(int index)拿到第index個string
* NodeList: 由多個Node組成的list
    * 可以使用size()拿到目前List中有幾個node。
    * 可以使用getNode(int index)拿到第index個Node
設計一個System，可以印出兩種list中的每個element。


### Init Design
![](https://i.imgur.com/e9CHWuD.png)

* 有幾種資料結構就要寫幾種print

### Refactor
![](https://i.imgur.com/cK4Fyt1.png)

```cpp=1
Iterator* createIterator()
{
    return new StringListIterator(this);
}

void printIterator()
{
    while(!iterator.isDone())
    {
        // print current item
        iterator.next();
    }
}
```