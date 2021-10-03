# Factory
###### tags = `Software Engineering`

> Define an interface for creating an object, but let subclasses decide which class to instantiate. Factory Method lets a class defer instantiation to subclasses.

![](https://i.imgur.com/Ehvq00H.png)

![](https://i.imgur.com/vsRs2mv.png)


## Example
有一家文具製造工廠，有一台機器負責製造文具，文具包含鉛筆、原子筆，未來還有可能使用這台機器製造其他不同的筆。

### Init Design
![](https://i.imgur.com/yA0J53l.png)

* In makeProduct

```cpp=1
Product p;
if(productType == "pen")
{
    p = new Pen();
}
else
{
    p = new Pencil();
}
```

* 加入新的Product時會動到Machine這個class
* 當Product數量變多或是init的過程比較複雜時，makeProduct的function會變得很混亂
* 把New class的過程外包!

### Refactor
![](https://i.imgur.com/q5Id61M.png)

## Static Library
當create()中要做的操作不複雜時，可以使用static factory簡化程式碼，並且讓Factory object不用被create出來。

![](https://i.imgur.com/HkpFiZ8.png)

```cpp=1
class ProductFactory
{
public:
    static Product* create(product)
    {
        if(product == pen)
        {
            return new Pen();
        }
    }
};


int main()
{
    ProductFactory.create(pen);
}
```

## 小練習
一間披薩店需要製作各種不同口味的披薩，口味包含: 瑪格莉特披薩、夏威夷披薩、德式香腸披薩。每種披薩的製作方式不同，都包含了bake()、cut()、box()三個製作步驟。除此之外，這間披薩店可以隨時新增、刪除不同口味的披薩。
