# 迴圈

###### tags = `C++`

### While
```cpp=1
while(condition){
    // loop
}
```

### For
```cpp=1
for(起始值;　終止條件;　每次迴圈更新的值){
    
}

int i;
for(i = 0; i<n ; i++){
    cout << i << endl;
}

// 也能這樣寫
for(int i=0 ; i<n ; i++){

}

// 可以放很多條件或變數

for(int i=0, int j=0 ; i<n ; i++,j++){

}
 
```

### Break
執行到break時會跳出迴圈
```cpp=1
while(condition){
    if(condition2){
        break; // 跳出while
    }
}

for(int i=0 ; i<n ; i++){
    if(condition){
        break; // 跳出for
    }
}

for(int i=0 ; i<n ; i++){
    for(int j=0 ; j<n ; j++){
        if(condition){
            break; // 跳出第二層for
        }
    }
    // 到這
}
```
### Continue
跳過此次迴圈剩下的指令，執行下一次迴圈
```cpp=1
while(condition){
    if(condition2){
        continue;
    }
}

for(int i=0 ; i<n ; i++){
    if(condition){
        continue;
    }
}

for(int i=0 ; i<n ; i++){
    for(int j=0 ; j<n ; j++){
        if(condition){
            continue;
        }
    }
}
```

### 小練習
https://www.hackerrank.com/challenges/c-tutorial-for-loop/problem