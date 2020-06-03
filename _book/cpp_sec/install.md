# 安裝環境

###### tags = `C++`

## Windows

* 安裝MinGW
    * https://sourceforge.net/projects/mingw-w64/files/
    * ![](https://i.imgur.com/QcZTGWT.png)

* 安裝LLVM
    * https://releases.llvm.org/download.html
    * ![](https://i.imgur.com/3jFM9N8.png)
    * ![](https://i.imgur.com/ZcQkafj.png)

* Merge
    * 分別打開 MinGW-w64 以及 LLVM 的安裝位置，依照剛剛的安裝狀態，分別會位於 “C:\mingw-w64\x86_64-8.1.0-posix-seh-rt_v6-rev0\mingw64” 以及 “C:\LLVM”。接著將 mingw64 資料夾的檔案全部複製至 LLVM 資料夾中。

* VSCODE
    * ![](https://i.imgur.com/TXCdctW.png)
* 新增一個放cpp檔案的資料夾
    * 新增.vscode資料夾
    * https://drive.google.com/open?id=1JweMW0Pme4U_1FaiWpeZEKlsgEOnou4S

* 測試

```cpp=1
g++ -std=c++11 filename.cpp
```
