# Decorator
###### tags = `Software Engineering`

> Attach additional responsibilities to an object dynamically. Decorators provide a flexible alternative to subclassing for extending functionality.

![](https://i.imgur.com/3m8p7lC.png)
![](https://i.imgur.com/n2k7sMZ.png)



## Example
在一家飲料店中，有賣各種不同的飲料，例如紅茶、綠茶、牛奶，除了這些飲料之外，他們也可以彼此混搭，例如紅茶加牛奶可以變成奶茶，除此之外，每種飲料客人可以自由選擇要不要加珍珠或是果凍，設計一個飲料店的架構，客人可以點菜單上的飲料，並且店家可以新增新的飲料到菜單上。

![](https://i.imgur.com/wgcyuwk.png)

* Problem
    * 如果新增很多飲料，每個飲料都要繼承Beverage
    * 如果有混搭的飲料(例如奶茶 = 牛奶+紅茶)，因為奶茶是一個獨立的class，可能會多寫很多重複的code，當設計出更多混達的飲料時，程式碼會很難維護。

Decorator Pattern使用對同一個物件"附加"其他特性的概念來解決這個問題:

![](https://i.imgur.com/GtdsHpH.png)

當要製作出一杯珍珠奶茶時，流程會變成:
* 拿到杯子
* 加上紅茶
* 加上牛奶
* 加上珍珠


```cpp=1
Beverage* b = new Bottle();
b = new Tea(b); // beverage = b
b = new Milk(b);
b = new Bubble(b);
b->make(); // beverage->make
```

* Bubble中，存的b為Milk；Milk中存的b為Tea；Tea中存的b為Bottle
* b->make
    * Bubble->make
    * Milk->make
    * Tea->make
    * Bottle->make

## 小練習

寫出一個蛋糕工廠的流程設計，在製作一個蛋糕的時候，除了蛋糕體本身之外，可以加入不同的配料，配料包含奶油、草莓、水蜜桃，除此之外，蛋糕工廠也能隨時新增不同的配料，製作出新口味的蛋糕。






