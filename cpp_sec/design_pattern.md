# Design Pattern Concepts
###### tags = `Software Engineering`

> Each pattern describes a problem which occurs over and over again
in our environment, and then describes the core of the solution to
that problem, in such a way that you can use this solution a million
times over.

一個design pattern包含:
* Pattern name
* Problem : 這個pattern用在解決哪個類型的問題
* Solution : 描述這個pattern怎麼解決問題
* Consequence : 描述這個解決方法的優缺點，以及可擴展性等等

## Design Principles

在軟體設計上，有一個最重要的原則: **High cohesion, low coupling**，要將相關聯性高的module聚合起來，讓他們成為同一個module下的submodule；但是module與module之間不能太過乎相依賴，避免一個module的改動影響其他的module太多。

為了達到這個目的，我們在設計時要考慮幾點重點:

### Encapsulate what variable
把適合的function/variable放在最適合的class裡

![](https://i.imgur.com/dMSNCI1.png)

### Program to interface
要使用一種class時，使用他的abstract class/interface

![](https://i.imgur.com/yQcOlAR.png)

### Make class easily extend
讓修改class時可以輕鬆加上其他功能；但不會影響舊的功能

![](https://i.imgur.com/L3VNAVF.png)

### Only talk to your friends
減少程式的接口，讓client端進入程式碼的entry point盡量單一

![](https://i.imgur.com/GZc84Gm.png)

### Abstraction, abstraction, abstraction
為了降低各個class之間的關聯性，利用abstraction降低class之間的關聯性

![](https://i.imgur.com/nMcSNMV.png)


## 小練習
畫出Class Diagram以及相對應的code架構，把class的定義放在.h檔，implement放在.cpp檔。

* 設計一個線上購物訂單系統，此系統會將後台的訂單轉換成不同的格式，可以轉換的格式有三種:
    * CSV
    * XML
    * DOC
* 這三種格式的檔案都要包含以下內容:
    * Header
    * 訂單資料
    * Footer

![](https://i.imgur.com/XoLAyqL.png)

```cpp=1
class OrderSystem
{
public:
    transformOrder(Order o, string format)
    {
        if(format == "csv")
        {
            createCSVHeader();
            createCSVData(o);
            createCSVFooter();
        }
        else if(format == "doc")
        {
            createDocHeader();
            createDocData(o);
            createDocFooter();
        }
        else if(format == "xml")
        {
            createXMLHeader();
            createXMLData(o);
            createXMLFooter();
        }
    }
};
```

![](https://i.imgur.com/Nuqqonb.png)



