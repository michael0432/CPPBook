# Observer
###### tags = `Software Engineering`

Observer Pattern用於定義物件之間一對多的依賴關係，當一個物件的狀態發生改變時，所有依賴於他的物件都要自動更新。

![](https://i.imgur.com/Giqv4uD.png)

![](https://i.imgur.com/J7K2Opo.png)

* Subject : 被觀察的人，提供增減Observer的操作，且讓任意數量的Observer可以觀察此Subject
* Observer : 制定更新操作的介面，當Subject狀態改變時通知Observer
* ConcreteSubject : 具體用來通知ConcreteObserver的角色
* ConcreteObserver : 具體需要更新的介面

## Example

在Excel中，我們可以根據同一份資料，產生不同的圖表，例如：長條圖、圓餅圖、折線圖...
當資料或任何一個圖表被使用者改變時，所有根據這份資料產生出的圖表，都必須自動更新。

### Init Design
![](https://i.imgur.com/QVYArHs.png)

* 每當新增一種不同的表格，data.change()裡就要新增程式碼

![](https://i.imgur.com/po3GDzu.png)

## 小練習
一個氣象觀測系統包含以下的功能:
* 在各地有許多氣象觀測站，其中，每個觀測站都會有觀測到的WeatherData
* WeatherData包含:
    * 溫度
    * 濕度
    * 大氣壓力
* 氣象觀測系統提供了兩種顯示天氣資料的方式，且觀測人員可以自由的新增其他顯示資料的方式:
    * 表格
    * 長條圖