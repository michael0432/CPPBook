# Abstract Factory
###### tags = `Software Engineering`
> Provide an interface for creating families of related or dependent objects without specifying their concrete classes.

![](https://i.imgur.com/GnXE5QP.png)


## Example
有一個應用程式，裡面包含了許多可以讓使用者使用的物件: button、scroll bar、window等等，每個application可以有一個或多個物件，此外，這個應用程式支援不同的顯示模式: 一般模式及深色模式，在不同模式下，物件的顯示方式不同，且顯示模式可以在應用程式被開啟後動態的切換。

### Init Design
![](https://i.imgur.com/qxzLWvm.png)


### Refactor
![](https://i.imgur.com/aFD4ktH.png)

![](https://i.imgur.com/pMuKaIU.png)

## 小練習
一間披薩店需要製作各種不同口味的披薩，口味包含: 瑪格莉特披薩、夏威夷披薩、德式香腸披薩。每種口味的披薩分成兩種風格: Italian Style跟Taiwan Sytle，兩種風格的Pizza使用了不同的醬汁以及起司。