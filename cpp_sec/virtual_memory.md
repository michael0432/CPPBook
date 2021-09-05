# Virtual Memory
###### tags = `Operating System`

之前介紹記憶體的設計有一個重點: Process在執行前，所需的資料一定要在memory裡。Virtual memory(虛擬記憶體)是一種允許整個Process的資料不用都在Memory裡，但仍然可以執行的技術，這個方法的優點就是可以執行比記憶體空間還大的Process，而且將Virtual Memory的大小及Physical Memory的大小分開，兩者不用一樣大，有利於程式設計時的彈性。

在介紹Virtual memory是怎麼運作前，先考慮看看為什麼將Process所有所需的資料放入記憶體是不必要的:
* 不是Process中所有的程式碼都會經常被執行，例如檢查錯誤的程式碼可能很少被執行
* 使用者常常宣告比實際使用還大的記憶體空間，例如先宣告一個很大的array，但並不真的會使用到那麼大的空間
* 某些條件判斷幾乎不會進入

因此，這些不常使用到的部分如果可以不被放進實體記憶體中，等到有必要的時候再swap進記憶體，對於系統來說可以減少一些負擔，也能更有效的利用記憶體。

## Demand Paging

在Process開始執行時，我們不直接將所有資料放進記憶體中，而是先載入一開始所需要的page，這個策略為demand paging，也就是page只有在真的需要時才被swap in記憶體中，執行這個操作判斷的程式稱為Pager。

如果Process不對沒有被Pager選到放進記憶體中的page做操作，那Process可以正常的執行，但是，若Process嘗試使用不在記憶體中的page，則會發生Page-fault，為了避免Page-fault，需要在page table上存下哪些page是真的可以使用的資訊。

![](https://i.imgur.com/6GC6Fpy.png)

因此，Process存取資料時的流程如下:

1. 檢查Page table，確認page是否valid
2. 若Page是invalid，則發出interruput signal，通知作業系統此Process需要做I/O後才能繼續運作
3. 從Disk拿取需要的資料
4. 在Memory找到一個空白的Frame，將資料放入此frame
5. 更新page table，因為此page已經可以對應到實體記憶體中的frame了
6. 重新執行原本的指令

![](https://i.imgur.com/XScA866.png)

## Effectiveness of Demand Paging

Page Fault的機率會大大影響系統的效能，因為每次page fault時，作業系統都要做下列這些事:

* 發出interrupt，並確認這個interrupt是因為page fault導致
* 保存目前Process的狀態及Context switch
* 找到該page存在disk上的何處並且做I/O
* 再次interrupt，由disk發出做好I/O的訊號
* 更新memory及page table

這些工作總計約為8ms。

假設page fault發生的機率為p，當沒有發生page fault時，存取記憶體的時間為200ns，有效存取時間(effective access time)為:

    effective access time = (1-p) * 200ns + p * 8000000ns = 200 + 7999800p
    
可以發現page fault發生的機率對於effective access time的影響非常大，因此，我們再來會討論如何有效替換不需要的page，降低page fault發生的機率。