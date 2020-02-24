# Array

陣列是一種存放連續資料的方式，Array中的資料會存放在記憶體中連續的區塊。
![](https://i.imgur.com/ymZag5n.png)

### 1D Array
Array的宣告方式如下:
```cpp=1
資料型態 Array名稱[Array大小];

int arr[5];
```
與Python的list不同，C++的Array要先宣告大小，才能在記憶體中拿到儲存空間。

* 給Array初始值:
```cpp=1
int arr[5] = {1,2,3,6,8};
```
* 取Array的值:
```cpp=1
int a = arr[0];
int b = arr[1];
```
* Input variable: 從stdin input一個大小為n的array
```cpp=1
int main(){
    int n;
    cin << n;
    int array[n];
    for(int i=0 ; i<n ; i++){
        int tmp;
        cin << tmp;
        array[i] = tmp;
    } 
}
```

* 注意:Array之間沒有=這個運算子:
```cpp=1
int arr[5];
int arr2[5];
arr2 = arr // wrong !
```

### 小練習
* https://www.hackerrank.com/challenges/arrays-introduction/problem

### 2D Array
![](https://i.imgur.com/5s2aiaP.png)
2D Array的宣告方式:
```cpp=1
資料型態 Array名稱[列數][行數];

int arr[3][2];
```
* 給Array初始值:
```cpp=1
int arr[3][2] = {{1,2},{3,4},{5,6}};
int arr[3][2] = {0}; // 全部設為0
```
* Input variable: 從stdin input一個大小為n*m的array
```cpp=1
int main(){
    int n,m;
    cin << n << m;
    int array[n][m];
    for(int i=0 ; i<n ; i++){
        for(int j=0 ; j<n ; j++){
            int tmp;
            cin << tmp;
            array[i][j] = tmp;
        }
    } 
}
```