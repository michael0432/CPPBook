# Page Replacement
###### tags = `Operating System`

Page replacement基本的概念為: 如果目前沒有空白的page可用，就去找一個目前沒有在使用的frame，並且把它空出來，空出來的frame就可以用來存page fault的那一個page。

![](https://i.imgur.com/TBExccE.png)

為了降低替換page時的系統負擔，可以藉由modify bit(也被稱為dirty bit)來存取每個page(frame)在被從硬碟讀入記憶體後，是否有被修改過，若沒有被修改過，則不需要做存回硬碟的動作，藉此減少I/O的時間。

藉由Page replacement，可以做到Demand paging的需求:不直接將所有資料放進記憶體中，且可以執行使用Memory比實體Memory大的Process。

作業系統會藉由Page replacement algorithm來決定要替換掉哪個page，一般來說，我們希望演算法可以讓Page fault發生的機率越低越好，為了評估一個演算法Page fault的次數，我們會隨機生成Reference string，用於表示隨機使用記憶體的某個位址，舉例來說，我們可以假設某個process依序會用到下列的address:

0100, 0432, 0101, 0612, 0102, 0103, 0104, 0101, 0611, 0102, 0103, 0104, 0101, 0610, 0102, 0103, 0104, 0101, 0609, 0102, 0105

我們假設每個page為100個bytes，意思是，這個process會依序用到下列的page:
1, 4, 1, 6, 1, 6, 1, 6, 1, 6, 1

為了要確認這個Referenc string會造成page fault的次數，我們也需要知道有多少page可以使用，要注意的是，當第一次使用到某個page時，一定會發生page fault。

| Page Number | 1 | 2 | 3 | 
| -------- | -------- | -------- | -------- |
| Page Fault     | ?     | ?     |3     |

後面我們討論的各個演算法，都使用下列這個reference string:

    7, 0, 1, 2, 0, 3, 0, 4, 2, 3, 0, 3, 2, 1, 2, 0, 1, 7, 0, 1

## FIFO

FIFO(First in first out)是最簡單的page replacement演算法，當我們要替換一個page時，就去替換最老的一頁，可以用一個quque存取目前有在使用的page，每次要載入新的page時，就把目前quque中最前面的page釋放。以上述的reference string為例，當page table的大小為3:

* 前三個page (7,0,1) 會發生page fault
* 第四個page 2會替換掉7，變成(0,1,2)
* 第六個page 3會替換掉0，變成(1,2,3)
* 以此類推，這個reference string會發生15次page fault

![](https://i.imgur.com/tqJSbyr.png)

FIFO page replacement有一個問題，由於替換的是最老的page，但我們無法確認此page是否正在被使用，因此，當這個page一被釋放，馬上就會再度發生page fault，讓此page需要再替換回來，雖然不會造成程式運行錯誤，但是會讓系統的效能降低(多做了無謂的page replacement)。

## Optimal

Optimal page replacement algorithm(OPT)，這個演算法的規則是: 把未來最長時間內不會被用到的那一個page替換掉，這個演算法可以保證得到最低的page fault機率，但是我們無法預先知道作業系統運行時，未來reference string的狀況。

## LRU

從上面的兩個演算法可以發現，FIFO參考的是某頁被載入記憶體的時間；OPT參考的是某頁被使用的時間，對於減少page fault的目的來說，參考某頁被使用的時間比較有機會做到降低page fault的機率。

Least recently used algorithm(LRU)，當要替換一個page時，選擇近期最少被使用的page做替換，LRU將OPT往未來看的特性改成往過去看，當作業系統運行沒有發生太大的改變，LRU可以做到接近OPT的效果。LRU最大的問題是: 如何定義及存取每個page上次被使用的時間，讓LRU可以把最久沒有被使用的page替換掉:

* 使用counter: 在最簡單的情況下，我們讓page table中每一個page都有一個暫存器，每當此page被使用時，就把暫存器上的時間改成被使用時的系統時間。
* 使用stack (or double-linked list) : 使用一個stack來保存每一個page，當page被使用時，就把此page放到stack的最上面，在stack頂端的page就是最新被使用到的page；在stack底部就是最久沒有被使用到的page。由於page需要從stack的中間被移出來，可以使用double-linked list代替stack。

![](https://i.imgur.com/x1hzXZQ.png)

無論以上述哪個方式來執行LRU，都需要硬體上的幫助，因為每次使用page都會需要更改page上的內容或是stack的內容，如果完全使用軟體的方式運算，每次運算時都要做context switch，才能讓軟體來更改這個資料結構，這樣也會大幅降低系統的效率，因此，上述的兩種方法在現代的電腦中，都是藉由硬體來達到。

### Reference Bit

並不是所有的硬體都能夠支援上述的LRU演算法，因此，我們可以藉由一些簡單的方法來做到接近LRU演算法的功能。

Reference bit的設計可以解決這個問題，在每個page上，都留了一個bit作為reference bit，這個bit在page被載入記憶體時預設為0，並在有被使用到後改設為1，藉由reference bit，在做page replacement時，作業系統雖然沒辦法知道page被使用的順序，但是可以知道哪些page被使用過；哪些page沒有被使用過，因此，可以選擇替換沒有被使用過的page。

使用reference bit作為LRU的替代會有一個很顯著的缺點: 有一定的可能性砍掉常用的page，這邊提出兩個可能的解決方案:

#### 更多的reference bit

將reference bit由一個bit改成8個bit，並且每經過一段時間，作業系統就統一把所有page的reference bit往左推一位，舉例來說: 
* 當一個page剛被載入記憶體時: 00000000
* 當此page被使用: 00000001
* 過了一段時間後，作業系統將reference bit左推一位: 00000010
* 當此page被使用: 00000011

這個8bit的暫存器就可以保存過去八段時間內，此page被使用的紀錄，藉由這個方式，就可以找到相對較少被使用的page。

#### Second Chance Algorithm

Second Chance Algorithm是一種藉由reference bit改良過後的FIFO演算法，在FIFO演算法中，會強制將最老的page移除，但Second Chance Algorithm會給這個page第二次機會，當要被移除的page reference bit為1時(表示在被放進記憶體的這段時間內，有被使用過)，就將reference bit設為0，並且將其放回queue的最尾端；當要被移除的page reference bit為0時，就將其移除，這個演算法改善了FIFO演算法的缺點，若一個page經常被使用，他的reference bit通常會為1，因此不容易將其移除。此外Second Chance Algorithm也可以用更多的reference bit強化移除時的判斷。

## 小練習

https://leetcode.com/problems/lru-cache/
