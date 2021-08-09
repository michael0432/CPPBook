# Mutex
###### tags = `Operating System`

為了避免兩個或多個process同時進入critical section，作業系統使用了Mutex及Semaphere的設計來保護critical section。
在process進入critical section前，必須先取得mutex lock；在結束critical section時將釋放mutex lock。

```cpp=1
do
{
    acquire lock
        critical section
    release lock
        remainder section
}
while(true)

acquire()
{
    while(!available); // wait
    available = false;
}

release()
{
    available = true;
}
```

為了確保沒有race condition，acqurie()跟release()這兩個function必須為atomic operation，意思是這個操作必須不可分割的被執行(中間不能有context switch)，因此，這個部分會使用硬體機制來保證其為atomic。


## Mutex coding in Linux

### Mutex init

Init有兩種方式
* 將mutex變數設為PTHREAD_MUTEX_INITIALIZER，會套用預設的mutex，這個方法不用destroy，作業系統會在process結束時destroy
* 使用pthread_mutex_init，在不用mutex時要自行destroy

```cpp=1
#include <pthread>

pthread_mutex_t mutex = PTHREAD_MUTEX_INITIALIZER;

int pthread_mutex_init(pthread_mutex_t *restrict mutex, const pthread_mutexattr_t *restric attr);
```

* mutex : 放mutex的pointer
* attr : 放mutex的參數，使用NULL即可
* return value : 
    * 成功為0
    * 失敗不為0

### Mutex lock/trylock
* trylock為non-blocking，如果mutex原本就被鎖住了，會回傳error；如果沒被鎖住，回傳0
* lock為blocking，卡在這行等拿到鎖

```cpp=1
#include <pthread>

int pthread_mutex_trylock(pthread_mutex_t* mutex);
int pthread_mutex_lock(pthread_mutex_t *mutex);
```

* return value : 
    * 成功為0
    * 失敗不為0

### Mutex unlock

```cpp=1
#include <pthread>

int pthread_mutex_unlock(pthread_mutex_t *mutex);
```

* return value : 
    * 成功為0
    * 失敗不為0

### Mutex destroy

```cpp=1
#include <pthread>

int pthread_mutex_destroy(pthread_mutex_t *mutex);
```

* return value : 
    * 成功為0
    * 失敗不為0

## 小練習

寫一段程式碼，包含兩種thread: Producer跟Customer，以及一個Producer及Customer共用的Ring buffer。程式碼會包含兩個Producer及三個Customer(兩個thread跑Producer；三個thread跑Customer，共五個thread)，其中，兩個Producer必須使用同一個function，但寫資料的頻率不同；三個Customer必須使用同一個function，但讀資料的頻率不同。

* Producer: 負責將讀取到的data放進Buffer。
    * 每一段時間固定隨機生成一筆資料放進buffer中
    * 資料包含
        * 生成這筆資料的時間(格式隨意，可以Google看C或C++拿linux系統的現在時間怎麼拿)
        * Data(格式為int，隨便寫一個數字即可，可以根據Producer的編號寫不同的值)
* Customer: 負責將data從Ring buffer讀出。
    * 每一段時間固定從Ring buffer中讀取出資料，一次讀出目前buffer中還沒讀過的所有資料
    * 每次讀完資料後，印出讀到的所有資料，以及讀取時的時間
* Buffer: 一塊記憶體空間，暫時存取資料。
    * 固定大小的Ring buffer，如果資料寫到最後一個index，下一筆就從頭開始寫，把原本的資料蓋掉

### Requirement
* 不能出現潛在的Race condition問題 (測試程式時不一定會發生，但是寫程式時要注意!)
* 不能有掉資料的問題，注意Buffer大小跟讀寫資料頻率之間的關係
* Customer讀取到的資料不能重複

### Output

```cpp=1
    // producer 1
    Producer 1 write data: time is : 12345678, data is : 1
    // producer 2
    Producer 2 write data: time is : 12345680, data is : 2
    
    // customer 1
    Customer 1 read data: read time is: 13330000, data time is : 12345678, data is : 1
    Customer 1 read data: read time is: 13330000, data time is : 12345680, data is : 2

```

### Example Code

```cpp=1
#define BufferSize 1000

typedef struct Data{
    time;
    data;
}Data;

Data Buffer[1000];

void ProducerFunction(void* arg)
{
    Input* param = (Input*)argv;
    int id = param->producerid;
    int frequency = param->frequency;
    ...
}

void CustomerFunction(void* arg)
{
    Input* param = (Input*)arg;
    int id = param->customerid;
    int frequency = param->frequency;
    ...
}

int main()
{
    for(int i=0; i<2; i++)
    {
        pthread_create(); // create producer
    }
    for(int i=0; i<3; i++)
    {
        pthread_create(); // create customer
    }
}
```