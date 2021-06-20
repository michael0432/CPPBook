# Process Schedule Algorithm

###### tags = `Operating System`

選取CPU要執行的process是由short term scheduler來執行，其中，發生以下四種情況其中之一時，short term scheduler要執行schedule的演算法:
* 當一個process從Runnig變成Waiting狀態時 (I/O或是wait())
    * 選擇一個新的Process執行
* 當一個process從Runnig變成Ready狀態時 (Interrupt發生時)
* 當一個process從Waiting變成Ready狀態時 (I/O或是wait()結束)
* 當process結束時
    * 選擇一個新的Process執行

當重新排班只在1, 4兩種狀況執行時，稱為不可搶先(non-preemtive)的排程；若四種狀況都執行，則為可搶先的排程(preemtive)。
當排班方式為preemtive時，可能會發生race condition。

## Schedule原則

不同的CPU Schedule方式有不同的特性，通常在評估CPU Schedule的方式時，會參考以下幾個標準:
* CPU使用率: 使CPU盡可能忙碌
* 產量(Throughtput): 單位時間內能完成的工作量(每單位時間能完成的process數量)
* 回復時間(turnaround time): 對特定Process而言，完成的總時間，包含在Ready Queue中等待的時間及在CPU執行的時間。
* 等候時間(waiting time): 對特定Process而言，在Ready Queue中等待的時間。

在後面介紹的演算法評估上，我們暫時將考量簡化到只考慮每個Process需要的CPU Time，來考量整體排班演算法的效率

## FIFO

FIFO(First-in-first-out)，CPU優先運算Ready Queue中的第一個Process，直到此Process結束再執行下一個Process。

考慮以下的Process:

| Process | CPU Time | 
| -------- | -------- | 
| P1     | 24     |
| P2     | 3     |
| P3     | 3     |

此時，CPU的使用情況為:

| 0~24 | 24~27 | 27~30 |
| -------- | -------- | -------- |
| P1     | P2    | P3     |

平均等待時間為 (0+24+27)/3 = 17:
| Process | CPU Time | Wait Time 
| -------- | -------- | -------- |
| P1     | 24     | 0 |
| P2     | 3     | 24 |
| P3     | 3     | 27 |

但是，若Process到達Ready Queue的順序變成P2, P3, P1，新的CPU使用狀況變成:

| 0~3 | 3~6 | 6~30 |
| -------- | -------- | -------- |
| P2     | P3    | P1     |

平均等待時間變成 (0+3+6)/3 = 3:
| Process | CPU Time | Wait Time 
| -------- | -------- | -------- |
| P1     | 24     | 6 |
| P2     | 3     | 0 |
| P3     | 3     | 3 |

由上述例子可以知道，FIFO演篹法不一定是最差的演算法，但平均等待時間可能會因為Ready Queue中Process的排列順序有很大的差異，是一個不太穩定的演算法，當有一個執行時間過長的Process存在，便容易導致後方所有的Process都因其很等待時間拉長。

## SJF
Shortest-Job-First(SJF)，優先執行CPU Time最短的Process。

考慮以下的Process:

| Process | CPU Time | 
| -------- | -------- | 
| P1     | 6     |
| P2     | 8     |
| P3     | 7     |
| P4     | 3     |

此時，CPU的使用情況為:

| 0~3 | 3~9 | 9~16 | 16~24 |
| -------- | -------- | -------- | -------- |
| P4     | P1    | P3     | P2|

平均等待時間: (0+3+9+16)/4 = 7
| Process | CPU Time | Wait Time |
| -------- | -------- | -------- | 
| P1     | 6     | 3 |
| P2     | 8     | 16 |
| P3     | 7     | 9 |
| P4     | 3     | 0 |

根據Greedy演算法，可以得知SJF是最理想的演算法，可以得到一組平均等待時間最短的排序法，然而，SJF演算法困難的部分在於如何得知每一個Process所需要的CPU時間。

若將SJF演算法改為preemtive，可以將判斷的條件改成剩餘時間最短的Process最先做。

| Process | Arrive Time |CPU Time | 
| -------- | -------- | -------- |
| P1     | 0 |8     |
| P2     | 1 |4     |
| P3     | 2 |9     |
| P4     | 3 |5     |

此時，CPU的使用情況為:

| 0~1 | 1~5 | 5~10 | 10~17 | 17~26 |
| -------- | -------- | -------- | -------- | -------- |
| P1     | P2    | P4     | P1| P3 |

由於P1從時間0開始，此時只有一個Process，因此可以先佔用1單位時間，當P2在時間1到達時，P2剩餘時間為4，P1剩餘時間為7，因此先讓P2執行，後方以此類推。

此例的preemtive SJF，平均等待時間為6.5；相較於non-preemtive SJF的7.75稍快一些。

## Priority Schedule

在多數的作業系統中，不同的Process Priority可能不同，因此在Schedule時也需要考慮不同Process的Priority，每個作業系統表示Priority的值不同，有可能是值越大越優先，也有可能相反，這裡用數值小代表越優先。

最基本的Priority Schedule遵循兩個原則:
* Priority越高的Process優先執行
* Priority相同的Process，採用FIFO

Priority Schedule可以是preemtive也可以是nonpreemtive，preemtive時，若新進的Process優先度比目前在執行的Process高，則搶先執行。

考慮以下的Process:

| Process | CPU Time | Priority | 
| -------- | -------- | -------- |
| P1     | 10     | 3 |
| P2     | 1     | 1 |
| P3     | 2     | 4 |
| P4     | 1     | 5 |
| P5     | 5     | 2 |

此時，CPU的使用情況為，平均等待時間為8.2:

| 0~1 | 1~6 | 6~16 | 16~18 | 18~19 |
| -------- | -------- | -------- | -------- | -------- |
| P2     | P5    | P1     | P3| P4 |

Priority Schedule最大的問題是starvation，一些優先權低的Process可能會一直無期限的等待CPU，在系統較繁忙的情況下，一連串高優先權的Process會讓優先權低的Process始終得不到CPU。此問題的解決方式是aging，逐漸提高停留在系統之中長時間等待Process的優先權。

## RR

Round-robin(RR)演算法是為了Time-sharing的作業系統設計的，Time-sharing的作業系統使每個Process在執行一段時間後強制釋放CPU資源給其他的Process，這個一段時間稱為time slice。

考慮以下的Process:

| Process | CPU Time | 
| -------- | -------- | 
| P1     | 24     |
| P2     | 3     |
| P3     | 3     |

有兩種情況會發生:
* 某process剩餘的CPU Time > time slice，此時先執行time slice後，作業系統會發出interupt，使該process回到Ready Queue中等待。
* 某process剩餘的CPU Time <= time slice，此時process會自然結束。

當time slice為4時:

| 0~4 | 4~7 | 7~10 | 10~14 | 14~18 | 18~22 | 22~26 | 26~30 |
| -------- | -------- | -------- | -------- | -------- |-------- |-------- |-------- |
| P1     | P2    | P3     | P1| P1 | P1 |P1 |P1 |

平均等待時間為: (6+4+7)/3 = 5.66

RR演算法的效能決定於time slice的長短，如果time slice太大，結果就跟FIFO一樣，如果time slice太短，大部分的時間都花在context switch。

## MultiLevel

由於不同種Schedule Algorithm有不同的優缺點，可以根據不同Process性質將其分類，將比較高priority的process跟較低priority的process放在不同的quque中，以不同的方式schedule。

對於priority不同的queue有兩種常見的設計方式:
* Preemtive，高優先度的process永遠可以先執行
* Time sharing，安排較高比例的CPU時間給高優先度的Process(例如System process 80%、其他process 20%)

![](https://i.imgur.com/eTob5PB.png)
