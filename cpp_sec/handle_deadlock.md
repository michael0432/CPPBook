# Handle Deadlock
###### tags = `Operating System`


一般來說，作業系統採用下列三種方法之一來解決Deadlock:
* 使用某個協議防止或避免Deadlock發生
* 允許系統進入Deadlock，偵測到後再想辦法解決
* 忽略Deadlock，假裝沒發生

其實，多數作業系統採用第三種方法，因此，在開發程式時，需要自己考慮到處理deadlock的狀況。

## 預防Deadlock
之前提到，Deadlock的產生有四個必要條件，只要可以使其中之一一定不成立，就可以預防deadlock的產生。

* Mutual exclision : 至少要有一個Resource是不可共用的形式
    * 不可能排除這個條件，因為會造成Race condition
* Hold and wait : 至少一個process必須佔用一個資源，且正在等候其他已被占用的資源
    * 要求每個Process在佔用資源時，一定要一次取得所有所需的資源
    * 要求每個Process在要求資源前，一定要把目前所佔的資源全部釋放
    * 缺點: 會讓資源使用率很低；且可能導致某些Process永遠執行不到
* No preemption : 資源不能被強制搶先使用
    * 要求每個Process在要求的資源無法立即取得時，要把目前所佔的資源全部釋放
    * 缺點: 每當放掉所有資源，Process需要花大量時間重新取得資源
* Circular wait :　必須存在一個互相等待的集合
    * 對所有資源強迫安排一個順序，所有Process要資源時需要照著此順序
    * 缺點: 難以定義作業系統上所有資源的順序，且順序安排不當會使系統效率低

## 避免Deadlock
上述幾個預防Deadlock的方法都有浪費資源的問題，因此要比較有效的避免Deadlock，作業系統需要更多的資訊:

* 系統目前可用的資源數量
* 各Process的資源需求量
* 各Process目前持有的資源量

舉例來說，假設Resource A有12個instance(有12個資源可以同時被使用)，而Process 0, 1, 2分別需要占用部分資源:

|  | 最大需求量 | 目前需求量 |
| -------- | -------- | -------- |
| P0     | 10     | 5     |
| P1     | 4     | 2     |
| P2     | 9     | 2     |

在此時，Safe sequence存在。
* Safe sequence: 當可以找到一個特定順序，讓Process依序拿取資源，拿到最大需求量後，使用完資源並釋放，只要此順序能滿足所有Process的最大需求量，即為Safe sequence。

P1 -> P0 -> P2為一個Safe sequence
* P1使用時，會多拿兩個instance，此時總需求為11個instance，沒有超過總資源12個instance:

|  | 最大需求量 | 目前需求量 |
| -------- | -------- | -------- |
| P0     | 10     | 5     |
| P1     | 4     | 4     |
| P2     | 9     | 2     |

* P1使用完後釋放資源，換P0使用，此時總需求為12個instance，沒有超過總資源12個instance:

|  | 最大需求量 | 目前需求量 |
| -------- | -------- | -------- |
| P0     | 10     | 10     |
| P1     | 0     | 0     |
| P2     | 9     | 2     |

* 最後，P2使用，此時總需求為9個instance，沒有超過總資源12個instance:

|  | 最大需求量 | 目前需求量 |
| -------- | -------- | -------- |
| P0     | 0     | 0     |
| P1     | 0     | 0     |
| P2     | 9     | 9     |

若將一開始P2的需求量改為3，則有可能發生deadlock，因為找不到Safe sequence(找不到一個必定不會發生deadlock的執行順序)，因此，要避免deadlock的方式就是當P2在一開始要求目前需求量時，作業系統要拒絕P2的這個請求。

### Bank Algorithm

Bank Algorithm用於在有多個Resource及instance的系統上避免deadlock，這個演算法使用了以下幾個資料結構，其中m為Resource的數量；n為Process的數量:

    Available : 大小為m的Array，Available[j] = k表示Resource j目前有k個instance可以使用
    Max : 一個n*m的Array，Max[i][j]表示Process i對Resource j的最大需求量
    Allocation : 一個n*m的Array，Allocation[i][j] = k表示目前Process i已佔用k個Resource j
    Need : 一個n*m的Array，Need[i][j] = k表示目前Process i還需要幾個Resource j才能完成其工作
    Finish : 一個大小為n的Bool Array，Work[i] = false表示Process i還沒結束

Bank Algorithm:

```cpp=1
Finish[0:n-1]皆設為False
Work = Available
for (int i = 0; i < n; i++)
{
    if (Finish[i] == False && all Need[i] <= Work)
        All Need[i] < Work => Process i所需的所有資源都能被滿足
        Work = Work + Allocation[i]
        continue
}
if (All Finish[0:n-1] == True) => Safe sequence存在
else => Safe sequence不存在

```

#### Example
假設某系統有5個Process及3個Resource。

* Allocation

| | A | B | C |
| --------| -------- | -------- | -------- |
| P0 | 0     | 1     | 0     |
| P1 | 2     | 0     | 0     |
| P2 | 3     | 0     | 2     |
| P3 | 2     | 1     | 1     |
| P4 | 0     | 0     | 2     |

* Max

| | A | B | C |
| --------| -------- | -------- | -------- |
| P0 | 7     | 5     | 3     |
| P1 | 3     | 2     | 2     |
| P2 | 9     | 0     | 2     |
| P3 | 2     | 2     | 2     |
| P4 | 4     | 3     | 3     |

* Need = Max - Allocation

| | A | B | C |
| --------| -------- | -------- | -------- |
| P0 | 7     | 4     | 3     |
| P1 | 1     | 2     | 2     |
| P2 | 6     | 0     | 0     |
| P3 | 0     | 1     | 1     |
| P4 | 4     | 3     | 1     |

* Available

| A | B | C |
| -------- | -------- | -------- |
| 3 | 3 | 2|

透過Bank Algorithm:

```cpp=1
Work = Allocation = {3,3,2}
for loop
    Finish[1] == False 且 Need[1] = {1,2,2} all < Work = {3,3,2}
        Finish[1] = True
        Work = {3,3,2} + Allocation[1] = {5,3,2}
        
    Finish[3] == False 且 Need[3] = {0,1,1} all < Work = {5,3,2}
        Finish[3] = True
        Work = {5,3,2} + Allocation[3] = {7,4,3}
        
    Finish[4] == False 且 Need[4] = {4,3,1} all < Work = {7,4,3}
        ... 以此類推
```

時間複雜度?


從上述的Bank Algorithm可以發現，不論是使用Bank Algorithm來預防Deadlock(發現找不到safe sequence就不允許特定Process取得特定資源)；或是使用Bank Algorithm來偵測Deadlock(確認是否存在Safe sequence)，對作業系統來說都相當耗費資源，因此，大多數的作業系統才選擇不預防、也不偵測Deadlock。
