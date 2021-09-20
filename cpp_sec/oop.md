# Object Orientation Concepts
###### tags = `Software Engineering`

## Modeling-First, Coding Later
在解決一個軟體工程問題的時候，第一步不是開始寫程式，而是先做好"Modeling"
* 用實際上運作的架構去想，怎麼劃分各個區塊的功能
* 定義清楚每個區塊要做什麼是、彼此之間怎麼互動合作

通常，我們會盡量用現實中的"Object"功能，去對應到軟體架構中的"Object"，舉例來說，如果要設計一個電影院的售票系統，我們會先想想，在現實中，一個電影院售票的過程有哪些東西參與:
* 售票員
* 顧客
* 影廳
* 要播放的電影
* 座位
* 電影票
* 錢

當我們把這些現實中的"Object"，對應到軟體架構中的"Object"，就可以用物件導向的方式做Modeling:
* 售票員
    * 收錢
    * 把電影票給顧客
    * 選擇影廳
    * 確認是否有座位
* 顧客
    * 給錢
    * 拿到電影票
    * 選擇要看的電影
    * 選擇座位
* 影廳
    * 包含座位以及要撥放的電影
* 要播放的電影
    * 包含電影資訊
* 座位
* 電影票
* 錢

列出各個Object後，我們就能設計出對應的member function以及member variable:
* 售票員
    * function
        * sellTicket()
        * selectTheater()
        * checkSeat()
    * variable
        * array of 影廳
        * array of 電影
        * Money
* 顧客
    * function
        * buyTicket()
        * selectMovie()
        * selectSeat()
    * variable
        * Money
        * array of 電影票
* 影廳
    * variable
        * array of 空座位
        * array of 已訂座位
        * 播放的電影
* 座位
    * variable
        * 座位編號
* 電影票
    * variable
        * 電影資訊
* 錢
    * variable
        * 金額

如果先透過上述Modeling完後，再開始寫程式，可以讓架構更加明確、程式碼更好維護及更好擴充，也讓Debug比較簡單。

## Feature of Object Orientation

### Class Diagram
在設計時，我們會利用Class Diagram來表達一個class裡面有什麼function以及variable，以及畫出class之間的關聯性。

![](https://i.imgur.com/GDHdyde.png)

### Identity
每個Object的instance都是獨一無二的，即使他們的所有member variable值都一樣，也要被當成兩個獨立的instance。

```cpp=1
Class Bicycle
{
private:
    double wheelSize;
public:
    void move();
};
```

### Class
Class定義object的行為，包含有哪些method及變數可以使用，在Class Diagram中，Class的表示會分成三個部分: Class的名字、Class有哪些member variable(attribute)、Class有哪些method(Operation)。
* Member variable (Attribute)
    * {+ - #} 變數名稱 : 變數型態 = 預設值
    * +表示 public、-表示private、#表示protected
* Static member variable
    * 用底線表示static的變數
    * static member variable表示這個變數為整個class共用，而不是隸屬於各個instance，所有instance的static member variable都對應到同一個值(同一個記憶體位址)
* Method(Operation)
    * {+ - #} methoid名稱(input type): output type
    * +表示 public、-表示private、#表示protected
* Static method
    * 用底線表示static的method
    * Static method表示這個class的所有instance都共用這個function，且當此class沒有任何instance時也可以使用這個function

![](https://i.imgur.com/GyzMjMS.png)


```cpp=1
class Bicycle
{
private:
    double wheelSize;
    static int color = 0;
public:
    bool move(int dir);
};
```

### Abstract class / Interface
Abstract class跟Interface在C++中的用法相同，但在某些程式語言中，用法不完全相同(例如Java)，以下都用C++的Abstract class說明。
Abstract class為一種特殊的class，在Abstract class中，可以定義member variable及member function，但是，它不能擁有instance，只能用來被繼承，其目的是定義某種類型的class，讓多個class繼承並自行實作各function。

在C++中，只要class有一個method為pure virtual function，則此class為abstract class。

```cpp=1
class A
{
public:
    int a;
    virtual int function() = 0; // pure virtual fucntion
}

int main()
{
    A a1; // error, A是abstract class所以不能有instance
}
```

繼承abstract class的class，一定要實作所有的pure virtual function

```cpp=1
class A
{
public:
    int a;
    virtual int function() = 0;
}

class B: public A
{
public:
    int function()
    {
        ...
    }
    int b;
}
```

### Inheritance
Subclass(child class)會繼承parent class所有的variable跟method

![](https://i.imgur.com/L7AcbC4.png)

### Polymorphism
同樣名稱的method可以有不同的input/output以及不同的行為

```cpp=1
class TimeOfDay
{
public:
    void setTime(string s);
    void setTime(int h, int m, int s);
}
```

Virtual function在繼承後，可以被不同的subclass重新implement(override)，一般的function則不行。

### Encapsulation
在物件導向程式設計中，會將所有Object相關的性質"封裝"到此Object中，並且將Object內部的實作隱藏起來，避免外部Object直接影響到Object內部實作的細節，在C++中，使用private、protected、public來區分。

```cpp=1
class Account
{
private:
    int money;
public:
    bool withdrawal(int amount);
    int getCurMoney();
}
```

### Relationship
Class之間根據其關係的緊密度可以由小到大分成下列幾種:
#### Dependency
Use，表示Class A會使用Class B
![](https://i.imgur.com/P6Y0Txx.png)

```cpp=1
class User
{
public:
    int use(ClassA a)
    {
        a.method();
    }
};
```

#### Association
Has-a，比Dependency的關聯性略強，表示Class A會將使用並將Class B的instance存起來(但不負責new Class B)
![](https://i.imgur.com/h9KiIZi.png)

```cpp=1
class Writer
{
private:
    vector<Book> books;
public:
    write(Book b)
    {
        books.push_back(b);
    }
};
```

#### Aggregation
Owns-a，比Association的關聯性略強，表示Class A會將使用Class B，且Class B可以屬於Class A的一部分(由Class A new)
![](https://i.imgur.com/3aIO9Ie.png)

```cpp=1
class Car
{
private:
    Engine e;
    vector<Wheel> wheels;
public:
    Car()
    {
        e = new Engine();
        for(int i=0; i<4; i++)
        {
            Wheel w = new Wheel();
            wheels.push_back(w);
        }
    }
};
```
#### Composition
Is-part-of，比Aggregation的關聯性更強，表示Class B只能屬於Class A，且會跟著class A一起生成/死亡。
![](https://i.imgur.com/KUmdDfc.png)

```cpp=1
class Human
{
private:
    Head h;
    Hand left;
    Hand right
public:
    Human()
    {
        h = new Head();
        left = new Hand();
        right = new Hand();
    }
    ~Human()
    {
        delete h;
        delete left;
        delete right;
    }
};
```

#### Generaliztion
Is-a，Class A跟B之間有繼承關係

![](https://i.imgur.com/CWlyOzU.png)

## 小練習1
畫出Class Diagram以及相對應的code架構，把class的定義放在.h檔，implement放在.cpp檔。

* 一個醫院有很多的醫生可以預約看診
* 一個病人只能預約一個醫生看診
* 一個醫生可以被多個病人預約
* 當醫生看診完一個病人，醫院要留下看診紀錄

## 小練習2
畫出Class Diagram以及相對應的code架構，把class的定義放在.h檔，implement放在.cpp檔。

* 設計一個線上購物訂單系統，此系統會將後台的訂單轉換成不同的格式，可以轉換的格式有三種:
    * CSV
    * XML
    * DOC
* 這三種格式的檔案都要包含以下內容:
    * Header
    * 訂單資料
    * Footer

## 小練習3
畫出Class Diagram以及相對應的code架構，把class的定義放在.h檔，implement放在.cpp檔。

* 設計一個整理各部門銷售紀錄的系統，這個系統要把多個部門的銷售資料整理好
* 使用者可以選擇看特定部門的資料
* 選擇完部門後，有兩種不同格式的資料可以選擇
    * 月報
    * 年報
* 當選擇其他部門後，顯示的資料要刷新成新選擇部門的資料

