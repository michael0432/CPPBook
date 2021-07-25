# Synchronization
###### tags = `Operating System`

藉由CPU排班，Process及thread可以(乍看之下)平行化地執行，由於Context switch，Process(Thread)有可能在任何的時間點被中斷，把CPU資源讓出給其他的Process(Thread)執行。

考慮一個Producer-Consumer的範例，這是一個軟體工程設計上很常見的範例，Producer的角色為資料的提供者，通常會將資料寫進一塊Consumer可以讀取的Buffer中(或檔案中)；Consumer的角色為資料的使用者，會將資料從一塊Buffer或檔案中讀取出來使用。

範例:
buffer為兩者共有的Buffer，Producer使用in當作index計算目前buffer寫到哪個位置；Consumer使用out當作index計算目前讀到哪個位置，兩者使用counter來計算buffer中目前有幾筆data還未讀取。
```cpp=1
// Producer
while(true)
{
    while(counter == BUFFER_SIZE); // do nothing
    buffer[in] = next_produced; // next_produced: 要寫入的資料
    in = (in + 1) % BUFFER_SIZE;
    counter++;
}

// Consumer
while(true)
{
    while(counter == 0); // do nothing
    next_consumed = buffer[out]; // next_consumed: 讀到的資料
    out = (out + 1) % BUFFER_SIZE;
    counter--;
}
```

* 上面的範例會有甚麼問題嗎?

## Race Condition

當兩個process或thread同時存取和處理資料時，有可能發生執行的結果取決於存取時的特殊順序，使得結果不如兩個process或thread的預期，這種情況稱為Race Condition。為了避免這個現象，我們必須對可能發生Race condition的資料做出保護，保證同時只有一個process或thread進行修改，這個需要做保護的程式範圍稱為Critical Section。

以上述的例子來說，在下列這個情況時會發生Race Condition:
* 假設counter目前是5
* 假設Producer及Customer同時各自執行自己的程式碼
    * 且同時對counter做++或--

```cpp=1
// register為存counter值的CPU暫存器，CPU做完運算時會短暫的將資料存在register中，不會馬上更新到memory

// Producer做counter++時，register的變化:
register1 = counter
register1 = register1 + 1
counter = register1

// Customer做counter--時，register的變化:
register2 = counter
register2 = register2 - 1
counter = register2
```

上述程式碼可以發現，當執行順序為下時，會發生錯誤:

    Producer執行regiset1 = counter                // register1 = 5
    Producer執行register1 = register1 + 1         // register1 = 6
    Customer執行register2 = counter               // register2 = 5
    Customer執行register2 = register2 - 1         // register2 = 4
    Producer執行counter = register1               // counter = 6
    Customer執行counter = register2               // counter = 4
    
從結果發現，兩者最後拿到的counter值分別為6與4，不符合預期，正確的counter值應該為5。
上述的狀況在多核心系統及Multithread的情況下特別容易發生，因此，我們需要設計機制來避免Race condition。

## Critical Section
Critical section為process或thread中，可能會發生race condition的程式區間，例如這段程式碼會對與其他process共用的參數或資料做更新等等。當一個process或thread在執行其Critical section時，要避免其他process或thread執行它們的critical section。

所以我們可以將一個process或thread分成以下的部分:

```cpp=1
while()
{
    entry section // 要求進入critical secition的程式碼
    critical section
    exit section // 宣告離開critical section的程式碼
    remainder section // 不須保護的區間
}
```

假設一個系統含有n個process {P0...Pn-1}，Critcal section的設計要滿足下列三點:
* Mutual exclustion: 如果Pi正在執行其critical section，其他Process不能執行其critical section
* Progress: 如果沒有process正在執行其critical section，且有process想要進入critical section，只有不在remainder section的process可以決定誰可以進入critical section，而且必須在時間內做出選擇，避免無止境的互相等待。
* Bounded waiting: 當有多個process在等待進入critical section時，不能無限制地等待，要有一個機制確認其他process進入critical section的次數是有限的。

