# Builder
###### tags = `Software Engineering`

> Builder is a creational design pattern that lets you construct complex objects step by step. The pattern allows you to produce different types and representations of an object using the same construction code.

![](https://i.imgur.com/EoHYTu7.png)


## Example
建商想要建造數間房子，每間房子有不同的特性，例如: 有無游泳池、有無花園、有不同數量的房間、有無車庫、有無健身房等等。

### Init Design
![](https://i.imgur.com/WhX3Zir.png)

In House:
```cpp=1
class House
{
private:
    bool hasGarden;
    bool hasPool;
    bool hasGym;
    bool hasGarage;
    int roomNum;
public:
    House(bool garden, bool pool, bool gym, bool garage, int room)
    {
        hasGarden = garden;
        hasPool = pool;
        hasGym = gym;
        hasGarage = garage;
        roomNum = room;
    }
};
```
當要設置的變數很多時，會造成Constructor的程式碼以及new的程式碼很複雜。

### Refactor
![](https://i.imgur.com/RCBC07j.png)


```cpp=1
// Builder.build()

HouseBuilder hb = new houseType1Builder();
hb.setHasGarden(true);
hb.setRoomNum(2);
House h = hb.getHouse();

```

## 小練習
設計一個旅行社協助規劃行程的系統，行程可以設定各個不同的特性，例如: 幾天幾夜、目的地、交通方式等等。