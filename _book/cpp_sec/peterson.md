# Peterson Solution
###### tags = `Operating System`

Peterson Solution一個以software為基礎解決Critical Section問題的方法。Peterson solution用於解決兩個Process在critical section跟reminder section交替執行時，避免Race condition的方法。

需要有兩個變數讓兩個Process Pi, Pj共用:
    
    int turn;
    boolean flag[2];
    
* turn: 表示輪到誰進入critical section。turn = i表示Pi進入critical section。
* flag表示一個process是否準備好可以進入critical section
    * flag[i] == true表示Pi準備好進入critical section

Peterson solution:

```cpp=1
// Process i
do
{
    flag[i] = true; // Process i 準備好進入critical section
    turn = j;  // 先把turn讓給j
    while(flag[j] && turn == j); // 如果turn是j且j準備好進入critical section，就要等j做完，等j做完後，會把flag[j] = false
        critical section
    flag[i] = false; // 做完critical section，flag[i] = false
        remainder section
}
while(true);

```

為了證明上面的解法是對的，我們要確認這個解法符合解決Critical section的三個條件:

* Mutual exclustion: 如果Pi正在執行其critical section，其他Process不能執行其critical section
    * 當Pi正在跑critical section的時候，由於turn == i且flag[i] == 0，Pj會被卡在while，不會進入critical section
* Progress: 如果沒有process正在執行其critical section，且有process想要進入critical section，只有不在remainder section的process可以決定誰可以進入critical section，而且必須在時間內做出選擇，避免無止境的互相等待。
    * 如果Pi沒準備好進入critical section，表示flag[i] == false，此時Pj一定可以進入critical section
* Bounded waiting: 當有多個process在等待進入critical section時，不能無限制地等待，要有一個機制確認其他process進入critical section的次數是有限的。
    * 一旦Pi完成其critical section，卡在while的Pj一定可以先進入critical section，因為此時turn = j，Pi會被卡在while

### 如果while(flag[j] && turn == j)少了任一個條件，還會符合這三個條件嗎?