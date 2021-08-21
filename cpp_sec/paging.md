# Paging
###### tags = `Operating System`

## Swap

![](https://i.imgur.com/vbgg4VW.png)

當Process正在被CPU運算時，所需的資料一定存在記憶體中，但是，記憶體的空間是有限的，當context switch時，可能會沒有足夠的空間將下一個process所需的資料放進記憶體中，此時，作業系統或進行Swap - 將暫時不用的資料放到secondary memory(通常是disk)；將需要的資料放進memory。

## 記憶體配置

前面提到，作業系統需要在程式開始執行時，分配給該程式需要的記憶體，因此，作業系統需要有一個方式控管記憶體，知道哪段記憶體空間可用、如何分配等等。記憶體配置需要考慮當有新Process要使用記憶體時要如何分配，以及有Process要釋放記憶體時要如何處理。

新Process要使用記憶體時，最簡單的分配方法有三種:
* First-fit : 從記憶體的最前方開始搜尋，找到第一個足夠大的空間，使用這個空間分配給Process
* Best-fit : 在所有容量夠大的區間中，將最小足夠大的空間分配給Process
* Wortst-fit : 將最大的空間分配給Process

無論使用哪種分配方法，都會碰到External Fragmentation的問題:

### External Fragmentation

當記憶體被不斷分配及釋放後，記憶體的配置可能會變得很零碎，導致剩餘的記憶體空間總量看似足夠，但是找不到一個夠大的連續記憶體空間分配給Process做使用。

![](https://i.imgur.com/S9BD8po.png)

要解決external fragmenation的問題，最常用的方式是compaction(聚集)，目的在於將記憶體內零散的可用空間集合起來，使其變成一個完整的大區間。然而，這個解決方法的缺點是它並不能隨時可以進行，因為隨意移動正在執行的Process的Physical Memory會造成問題。

因此，現代的作業系統使用Paging來解決這個問題。

## Paging

Paging將Physical Memory打散為固定大小一個個區段，稱為Frame；將Virtual Memory也打散為相同大小的一個個區段，稱為Page。一個Process會占用一些page，這些page會在Process要被執行時載入任何的Frame裡，因為Page跟Frame的大小相同，所以只要找到空的Frame用即可。

作業系統利用Page table做virtual address及physical address的對應，page table上的每個page為Frame的位址。

Paging將Virtual address分為兩個部分: page number以及page offset來找address對應到的page。
* Page number : 指向Page table上的某一個Page。
* Page offset : 指向Page上某個記憶體位址，表示從page的最前端位址往後數offset個btyes。

![](https://i.imgur.com/ZQ3VMSZ.png)

找到對應的page後，裡面存的資料是Frame的位址，將Frame的位址+page offset便能在Physical Memory上找到對應的Physical address。

![](https://i.imgur.com/c1thfrZ.png)

有些作業系統會在Page table上的每個欄位多加一個valid bit，用於表示此欄位是否可以存取(是否真的有指到存在資料的frame)，避免在操作page table的過程中指到空的記憶體位址。

![](https://i.imgur.com/pagNyHo.png)


### TLB

上述的Paging方式看起來可以有效解決TLB的問題，但是，我們花了額外的資料結構 - Page table來存取page-frame mapping，而這整個page table也是存在記憶體裡面，因此，每次要做paging時，要先找到page table的記憶體位址；再利用page table找到pshysical address，這表示，原本只要做一次記憶體存取，變成要做兩次記憶體存取。

因此，我們需要用一些硬體的方法來減少查詢page table的次數跟頻率，TLB(Translation look-aside buffer)是一種cache，是一個空間遠小於記憶體的硬體，裡面的構造是一個類似hashmap的key-value架構，key為page number；value為frame number，可以在非常快的時間根據key對應到value。

![](https://i.imgur.com/6Zq9OPw.png)

每次要做virtual address跟physical address的對應時，會先到TLB中尋找有沒有已經被存下來的page number，如果有，就直接使用存在TLB中的frame number；若找不到，再到page table中依照原本的流程找frame number，由於TLB可以存的page number很少，所以要盡量讓最常使用到的page存在TLB中。

我們可以使用TLB hit rate來評估TLB的效益，假設每次記憶體存取要花100ns，從TLB中找到frame number要20ns:

* 不使用TLB時，每次paging的時間:
    * 100ns * 2 = 200ns
* 當TLB hit rate為80%時:
    * (20ns+100ns) * 0.8 + 200ns * 0.2 = 136ns
* 當TLB hit rate為99%時:
    * (20ns+100ns) * 0.99 + 200ns * 0.01 = 120.8ns


### Paging Structure

由於現代的電腦都有非常大的Virtual memory，因此，Page table會變得非常的大，舉例來說，在一個32位元的系統上(每個記憶體位址用32個bit表示)，總共有2^32個Address，若每個page的大小是4KB(2^12)，則page table上會有(2^32/2^10)個page，每個page需要用4個bytes來存，也就是說，光是page table就佔了4 * (2^32/2^10) bytes(大約4MB)。
(如果是64位元的系統，就佔了7000000GB!)

因此，最直觀的解決方式是把page table分層，原本的virtual address為page number + offset，改成page number 1 + page number 2 + offset。

* page number 1 : 第一層page table的page number
* page number 2 : 第二層page table的page number
* offset : 對照到frame number後用的offset

![](https://i.imgur.com/LyO3wdO.png)

這樣的方法可以有效減少Page table的大小，但對於64位元的系統來說，page table還是太大了，其實，在x86-64的系統上(intel的64位元系統)，實際上可用的記憶體位址也不到2^64，目前最多使用48位元的記憶體，並提供了四層的paging table。

![](https://i.imgur.com/Mwv6SXq.png)



