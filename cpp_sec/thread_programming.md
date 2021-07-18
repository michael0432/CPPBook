# Thread programming

## pthread_create
![](https://i.imgur.com/55S7qTV.png)
![](https://i.imgur.com/zerjEen.png)


```cpp=1
#include <pthread.h>
int pthread_create(pthread_t* threadid,
                          pthread_attr_t* attr,
                          void*(*start_rtn)(void *),
                          void* arg);
```

* threadid: 創建thread成功後，這個pointer指到的位置會存thread id
* attr: Thread的設定，包含優先度、同步設定等等，使用NULL為預設設定
* start_rtn: 給thread使用的function pointer，即指定thread要執行哪個function
* arg: function的input

### Passint arg

pthread_create時，function的arg是用void*當作input，並且在function內部再將input轉回需要的格式。舉例來說:

```cpp=1
// intput一個int
void* funcForThread(void* arg)
{
    int input = *((int*)arg);
}

int main()
{
    int functionInput = 3;
    pthread_create(tid, NULL, funcForThread, (void*) &functionInput);
}

// intput一個string
void* funcForThread(void* arg)
{
    char* input = (char*)arg;
}

int main()
{
    char functionInput[10] = "Hi!";
    pthread_create(tid, NULL, funcForThread, (void*) functionInput);
}

// intput一個vector
void* funcForThread(void* arg)
{
    vector<int>* input_vector = reinterpret_cast<vector<int>*>(arg);
    // or
    vector<int>& input_vector = reinterpret_cast<vector<int>*>(arg);
}

int main()
{
    vector<int> v(3,3);
    pthread_create(tid, NULL, funcForThread, (void*) v);
}
```

## pthread_join
功能跟wait()類似，用於等待thread結束並回收。

```cpp=1
#include <pthread.h>
pthread_join(pthread_t tid, void** retunrvalue);
```

* tid: 要wait的thread id
* return value: 接收exit時returnvalue的值，不在意的話可以填NULL

## pthread_exit
功能跟exit()類似，用於結束一個thread。

```cpp=1
void pthread_exit(void* returnvalue);
```

* returnvalue: 傳給join()的thread 此thread結束的狀態，不需要的話可以填NULL。

### Example

**重要: compile時要下-lpthread才能正常compile !**

    g++ -std=c++11 tmp.cpp -lpthread -o outputname

```cpp=1

#include <stdio.h>
#include <pthread.h>
#include <unistd.h>

typedef struct inputdata
{
    int a;
    int b;
    int res;
}inputdata;

void *func(void *arg)
{
    inputdata* input = (inputdata*)arg;
    input->res = input->a + input->b;
    pthread_exit(NULL);
}


int main()
{
    pthread_t t;
    inputdata input;
    input.a = 3;
    input.b = 4;

    pthread_create(&t, NULL, func, (void*) &input);
    pthread_join(t, NULL);
    printf("%d\n", input.res);
}
```

## pthread_self
拿到自己的thread id

```cpp=1
pthread_t pthread_self();
```

## pthread_equal
比較兩個tid是否相同

```cpp=1
int pthread_equal(pthread_t tid1, pthread_t tid2);
```

* return value
    * 0表示不同
    * 其他值表示相同

## 小練習
寫一隻程式碼，用for loop從1+.....+10000000。並平均將計算量分配給不同的thread
例如，分成10個thread時:
* thread0: 計算1~1000000
* thread1: 計算1000001~2000000

...
以此類推

### input
input可以指定使用幾個thread做計算 (1 ~ 100個thread)

### output
output計算完後的結果

