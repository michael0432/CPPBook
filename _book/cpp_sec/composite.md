# Composite
###### tags = `Software Engineering`

> The composite pattern describes a group of objects that are treated the same way as a single instance of the same type of object. The intent of a composite is to "compose" objects into tree structures to represent part-whole hierarchies. Implementing the composite pattern lets clients treat individual objects and compositions uniformly.

![](https://i.imgur.com/qEKwxtq.png)


## Example
在PPT中，使用者可以在頁面上畫方塊、直線以及插入文字，除此之外，也可以將這些物件組合成群組，且一個群組物件中，可以包含其他群組物件。

### Init Design
![](https://i.imgur.com/kEoqdS8.png)

* Problem:
    * 如果新增了新的物件，會需要更改group的程式碼。


### Refactor
![](https://i.imgur.com/2eOXQuE.png)

