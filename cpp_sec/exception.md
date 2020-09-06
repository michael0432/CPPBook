# Exception Handling
###### tags = `OOP`

當程式發生runtime error時，若沒有做Exception Handling(Error Handling)，程式會直接結束執行，然而，我們可以藉由Exception Handling來讓設定當程式碰到特定錯誤時，該做甚麼處理。

在C++中，利用try, catch, throw 來做Exception Handling:
* throw: 當問題發生時，丟出一個exception
* catch: 收到exception，並決定如何處理exception
* try: try定義哪個段落的程式碼需要做Exception Handling

catch, try基本的格式長這樣:

```cpp=1
try{
    // protected code
}
catch(ExceptionName e1){
    // catch block
}
catch(ExceptionName e2){
    // catch block
}
...
```

throw基本格式:

```cpp=1
int divide(int a, int b){
    if(b == 0)
        string errmsg = "divide zero";
        throw errmsg;
    return a/b;
}
```

Example:

```cpp=1
#include <iostream>
#include <exception>
#include <string>

using namespace std;


int divide(int a, int b){
    if(b == 0){
        string errmsg = "divide zero";
        throw errmsg;
    }
    return a/b;
}

int main(){
    try{
        cout << divide(6,2) << endl;
        cout << divide(6,0) << endl;
        cout << divide(6,3) << endl;
    }
    
    catch(const string e){
        cerr<<e<<endl;
    }
    cout << "keep doing something" << endl;
    return 0;
}
```

## C++ Standard Exception

C++提供了很多定義好的Standard Exception，可以利用Standard Exception來丟出自己定義的error

![](https://i.imgur.com/WpluogE.png)

```cpp=1
#include <iostream>
#include <exception>
#include <string>
#include <vector>
using namespace std;

int main(){
    vector<int> myVec(1);
    try{
        myVec.at(20);
    }
    catch(exception& e){
        cerr << "Standard exception: " << e.what() << endl;
    }
    cout << "keep doing something" << endl;
}
```

## 自定義exceptions

可以藉由繼承exception這個class自己定義exception

```cpp=1
#include <iostream>
#include <exception>
using namespace std;

struct MyException : public exception {
   const char* what () const throw () {
      return "C++ Exception";
   }
};
 
int main() {
   try {
      throw MyException();
   } catch(MyException& e) {
      cout << "MyException caught" << std::endl;
      cout << e.what() << endl;
   } catch(exception& e) {
      //Other errors
   }
}
```


