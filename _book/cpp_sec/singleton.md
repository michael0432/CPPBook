# Singleton
###### tags = `Software Engineering`
> the sole instance of a class

有些物件只需要一個instance，可能是因為整個系統中只允許一個instance存在，或是某段程式碼只能初始化一次等等。

## Example
有一個 Chocolate Boiler 裝置，可以煮沸巧克力，Boiler只有一個且每次只能放入一單位的巧克力，每單位的巧克力不能被重複煮沸(只能煮沸一次)，當巧克力煮沸完後，需要將巧克力脫水並清空Boiler。

### Init Design
![](https://i.imgur.com/Wpg19SY.png)

* Mutli-thread
    * 當多個thread同時進入getInstance()時，可能會同判斷instance == NULL，造成new出兩個Boiler

#### Eager Singleton
利用static的性質，在進入main function以前就初始化好Singleton的class，由於static member variable被視為全域變數，因此將getInstance設為static可以解決race condition的問題。

```cpp=1
class Boiler
{
public:
    static Boiler& getInstance()
    {
        return instance;
    }
private:
    Boiler()
    {
    
    }
    static Boiler instance;
};
```

* 優點: 不會有thread-safe的問題
* 缺點: 如果初始化此物件的時間很長，會讓程式碼比較慢進到main

#### Lazy Singleton
Lazy singleton在使用到此物件時才初始化，利用mutex讓getInstance為thread-safe。

```cpp=1
class Boiler
{
public:
    static Boiler& getInstance()
    {
        std::unique_lock<std::mutex> lock(m_mutex);
        if (instance == NULL)
        {
            instance = new Boiler();
        }
        return *instance;
    }
private:
    Boiler()
    {
    
    }
    static Boiler* instance;
    std::mutex m_mutex
};
```

#### Meyers Singleton
在C++11之後的版本，local static object新增了一個特性:
> If control enters the declaration concurrently while the variable is being initialized, the concurrent execution shall wait for completion of the initialization.

```cpp=1
class Boiler
{
public:
    static Boiler& getInstance()
    {
        static Boiler instance;
        return instance;
    }
private:
    Boiler()
    {
    
    }
};
```

