# Practice - STL Vector (p1)

實作一個STL Vector，Vector使用三個pointer來儲存vector的內容、大小及容量。

Source code (p1): https://github.com/michael0432/practiceCplusplus

![](https://i.imgur.com/yrM3uxy.png)

![](https://i.imgur.com/XmeWnx1.png)

## Size
Size為Vector目前儲存幾個element，Size一定小於等於Capacity

## Capacity
Capacity為Vector目前容量的大小，Capacity擴大後便不會再變小。

## Implement Method

### Construtor

### Resize
重新設定Vector的Size(非Capacity)

* Capacity擴增的規則:
    * 原vector的size為n，新的size為n'，原capacity為c
        * 若n'<=n，size及capacity都不變
        * 若n'>n且n'<=c，size改成n'，capacity不變
        * 若n'>c
            * 若n'>2*n，新的capacity為n'
            * 若n'<=2*n，新的capacity為2*n

* Example:
    * 原本的vector: size為3、capacity為8，resize(5)
        * resize後: size為5、capacity為8
    * 原本的vector: size為6、capacity為8，resize(10)
        * resize後: size為10、capacity為12
    * 原本的vector: size為6、capacity為8，resize(15)
        * resize後: size為15、capacity為15

### Assign
將vector的值變成跟另一個vector相同

## 測試程式是否正確

```
g+= -std=c++11 Main.cpp VectorImp.cpp  -o p1.exe
p1.exe

```