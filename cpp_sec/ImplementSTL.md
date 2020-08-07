# Implement STL

## Vector

Implement一個Vector的class，功能包含:
* size(): 回傳vector的大小
* empty(): 回傳vector是否為空
* push_back(): 將一個element放入vector的尾端
* pop(): 將一個element從vector的尾端拿出
* find(): 回傳特定element是否在vector中
* getElement(int i): 回傳特定index的值，若i超過範圍，則回傳-1

vector有兩種，一種為存int，另一種為存char。


```cpp=1
class myVector{
public:
    int size(){
    
    }
    bool empty(){
    
    }
    bool push_back(int a){
    
    }
    bool push_back(char a){
    
    }
    bool pop(){
    
    }
    bool find(int i){
    
    }
    bool find(char i){
    
    }
    int getElement(int i){
        
    }
    char getElement(int i){
    
    }
};
```