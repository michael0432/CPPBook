# Facade
###### tags = `Software Engineering`

> Interface to a subsystem

![](https://i.imgur.com/JULHyVl.png)


## Example
在作業系統上，當使用者輸入compile的指令後，作業系統需要將程式碼編譯成執行檔，編譯的過程分成幾個部分:
* Pre-process
* Compilation
* Linking

這三個部分都包含了複雜的流程，使用者的程式碼要依序執行這三個流程才能把檔案變成執行檔，設計一個系統讓使用者使用。

### Init Design

![](https://i.imgur.com/QzopbUa.png)

* Problem:
    * 當有多個不同類型的User時，會重複寫一些程式碼
    * 當Compile的過程更複雜時，會有更多的subsystem或是user要自己知道很多流程。

### Refactor
![](https://i.imgur.com/3wTA0EO.png)
