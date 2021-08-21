# Memory
###### tags = `Operating System`

作業系統除了負責管理Process及CPU運算的資源外，另一項重要的工作室管理記憶體。由於CPU執行運算時，所需的資料都必須存在記憶體裡，所以作業系統需要將所需的資料放進記憶體裡，並且可以盡可能快的在記憶體裡找到所需的資料。

## Memory Hardware

在對記憶體做讀取時，硬體使用記憶體位址(Memory Address)作為參數，我們在軟體層面上看到的是被抽象化過後的虛擬位址(Virtual Address)；實際上硬體使用的記憶體位址為Physical Address。首先，我們先考慮Physical Address的設計。

當一個Process在作業系統上執行時，會占用一個記憶體空間，且每個Process彼此占用的記憶體空間是不重疊的，因此，作業系統需要知道每個Process佔用的記憶體空間範圍，最基本的設計為使用base register及limit register兩個暫存器來存取Process使用的範圍。

![](https://i.imgur.com/rFWip2z.png)

當Process在CPU中做運算時，作業系統會根據Process要access的physical address做下面的判斷:

![](https://i.imgur.com/IwnqHEe.png)

## Memory Binding

在程式被執行前，需要經過一系列的步驟，讓程式中的指令以及變數被對應到記憶體上的某個address(決定base位址)，這個步驟稱為Binding，binding可以在三個階段執行: compile time、load time、execution time。

* Compile time : 由compiler決定此程式在記憶體中的位置(找到一個可用的base)。
* Load time : 程式不一定要在固定的記憶體位置開始執行，所以等到load module的時候才執行，此時，只要include的library都要決定記憶體位址。
* Execution time : 由作業系統動態決定此程式的記憶體位址，等到程式開始執行才會決定位址，多數的作業系統使用此方法。

![](https://i.imgur.com/h6y3nsq.png)

## Logical Address / Virtual Address

當作業系統在Execution time決定Process的記憶體位址時，作業系統會將Physical Address抽象化為Virtual Address，並使用MMU(Memory-management unit)這個硬體裝置來做Virtual Address跟Physical Address的轉換。

![](https://i.imgur.com/g26whZA.png)

所有User執行的程式都沒有辦法看到Physical Address，所有的操作都是對Virtual address執行，例如，C/C++中將變數的記憶體位址印出來，印出的是Virtual address，這個設計的目的是為了保留作業系統針對Memory做進一步的管理。



