# C/C++ Compile and Link
###### tags = `C++`

之前，我們編譯把C/C++的code編譯成可執行檔時，都是使用下面這個指令:
    
    g++ -std=c++11 xxx.cpp -o xxx.exe

但其實，這行指令包含了四個流程:
* Preprocessing
* Compilation
* Assembly
* Linking

其中，前三個步驟常被統稱為compile，會將.cpp/.h檔編譯成.o檔(object file)；第四個步驟為Link，會將編出來的.o檔組合在一起，變成一個完整的執行檔，這樣的設計是為了讓一個大型專案的obj之間的關聯性降低，可以分開編譯加快編譯速度。
上述編譯的指令也可以拆開成兩個部分:

    g++ -std=c++11 -c xxx.cpp
    g++ -o xxx.exe xxx.o yyy.o
    
第一個指令會將c/cpp/h檔編譯成.o檔，第二個指令會將多個.o檔組合成一個執行檔。

![](https://i.imgur.com/hZ4Ymj9.png)

各個步驟詳細的流程可以參考這篇 : https://medium.com/@alastor0325/https-medium-com-alastor0325-compilation-to-linking-c07121e2803