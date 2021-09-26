# Strategy
###### tags = `Software Engineering`

當有一系列的演算法需要使用的時候，Strategy將這些演算法Abract成一個abstract class或interface，可以藉由替換不同的strategy讓物件有不同的行為。

![](https://i.imgur.com/Cg8sRE3.png)

## Example
有一個網站需要根據商品的價格進行排序:
![](https://i.imgur.com/dv6MqbJ.png)

```cpp=1
class Client
{
public:
    Use()
    {    
        QuickSort qSort;
        new Website(qSort);
        Website.Context();
    }
};

class Website
{
private:
    Sortstrategy sortStrategy;
public:
    Context()
    {
        sortStrategy.sort();
    }
    Website(Sortstrategy s)
    {
        sortStrategy = s;
    }
};
```

## 小練習

在一個遊戲中，有四種交通工具: 飛機、汽車、機車、腳踏車，這四種交通工具都可以移動。
* 飛機可以飛或是在陸上移動；汽車、機車、腳踏車都只能在陸上移動
* 四種交通工具都可以隨時啟動及停止(更改目前是否正在移動的狀態)，且都有低速、中速、高速三種移動速度，可以在run time調整
* 飛機、汽車、機車可以加油，且三種交通工具家不同種油；腳踏車不行
* 四種交通工具都可以在compile time更新顏色(UpdateColor)

設計出一個使用者使用這四種交通工具的軟體架構。

### Init Design

![](https://i.imgur.com/Iue5j7F.png)

有哪些function可能會寫重複的程式碼或是彈性太低?
* Refuel : 三種交通工具都可以加油，只是加不同的油
* changeSpeed : 裡面可能會寫死三種移動速度，不容易調整
    
    ```cpp=1
    changeSpeed(speedMode s)
    {
        if(s == "low")
            ...
        else if(s == "medium")
            ...
        else if(s == "high")
            ...
    }
    ```
    
### Refactor

![](https://i.imgur.com/xfHw6Mf.png)
