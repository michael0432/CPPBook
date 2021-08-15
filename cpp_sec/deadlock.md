# Deadlock
###### tags = `Operating System`

在Multi-thread或Multi-process的作業系統上，許多Process或Thread要競爭一些有限的資源，但是為了避免Race condition，在一個Process/Thread要使用一個資源時，此資源不一定是可用的，可能需要等待其他Process/Thread釋出此資源的使用權。

> 當兩輛火車同時到達一個交叉路口時，雙方都必須完全停止，等待一輛駛去後，另一輛再通過。

在作業系統上，也有可能發生Process/Thread之間互相等待資源，最後導致沒有任何一個Process/Thread可以取得資源的情形，這種情況我們稱為Deadlock。

舉例來說，假設作業系統上有兩個資源Resource A, Resource B，以及兩個Process X, Process Y，X及Y都同時需要兩個資源才能做運算，此時，Process X先lock A，再lock B；Process Y先lock B，再lock A:

```cpp=1
// X.cpp
mutexA->lock();
mutexB->lock();
A + B
mutexA->unlock();
mutexB->unlock();

// Y.cpp
mutexB->lock();
mutexA->lock();
A + B
mutexB->unlock();
mutexA->unlock();

```

當X拿到Resource A時，若發生context switch；Y拿到Resource B，此時就會發生deadlock，因為X在等A；Y在等B，但是兩者都永遠等不到資源；也不會放掉手上的資源。

## Deadlock發生的條件
當下列四種情況同時成立時，deadlock會成立

### Mutual exclision
至少要有一個Resource是不可共用的形式，一次只有一個process/thread可以使用

### Hold and wait
至少一個process必須佔用一個資源，且正在等候其他已被占用的資源

### No preemption
資源不能被強制搶先使用，一個資源必須等待使用它的人使用完才被釋放

### Circular wait
必須存在一個互相等待的集合{P0, P1... ,Pn}，其中P0等待的資源被P1占用，P1的資源被P2占用....Pn-1的資源被Pn占用。

## Resource Allocation Graph

![](https://i.imgur.com/4yc9wCi.png)

* P : Process
* R : Resource
* R->P : P正占用R的資源
* P->R : P正在等待R的資源

如果Resource Allocation Graph存在Cycle，表示**可能**存在Deadlock，但不表示一定存在Deadlock。以上圖而言，R1->P1->R2->P2->R1，是一個Cycle，但是並不是deadlock，因為當P3釋放R2之後，P1可以獲得R2，此時P1滿足所有需求，執行完後會釋放R1,R2，此時P2即可執行。

![](https://i.imgur.com/dZbo7ui.png)




