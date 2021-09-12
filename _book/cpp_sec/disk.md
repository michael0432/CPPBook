# Disk
###### tags = `Operating System`

### HDD (Hard Disk Drive)

Disk (硬碟)提供大量的儲存空間，由cylinder(磁柱)、track(磁軌)、sector(磁區)組成，在使用時，磁碟機的馬達會高速旋轉，以RPM(Rotation per minuites)當單位。

![](https://i.imgur.com/hscIB0n.png)

硬碟傳遞的速率可以分為兩個部分:
* Transfer rate: 資料在硬碟及電腦間傳送的速率
* Positioning time: 移動arm到指定sector的時間

### SSD (Solid-state Disk)

SSD是一種非揮發性記憶體(Non-Volatile Memory)，當斷電時，儲存的資料不會消失，SSD跟HDD的構造完全不同，沒有arm、track、sector等設計，而是比較像是一個儲存空間較大的記憶體，由於不需要移動磁臂尋找特定的sector，因此有存取資料較快的優點。

## 磁碟排班

在使用傳統HDD的系統中，作業系統的責任之一就是有效的使用硬碟，也就是說，要盡量讓磁碟在單位時間內盡可能傳更多的資料，由於磁臂的移動距離越長、意味著Positioning time越大，就會花越多的時間在移動磁臂而不是傳輸資料，因此，下面會介紹一些演算法，來處理磁碟排班的問題。

### FCFS
FCFS(First-come, first served)是最簡單的排班原則，他是最公平的演算法，但不是讓磁碟效能最大化利用的演算法。
舉例來說，當以下的sector依序有I/O需求，且一開始的位置在sector 53:

    98, 183, 37, 122, 14, 124, 65, 67

如果採用FCFS演算法，要依序移動: 53->98, 98->183...，共需移動640單位距離

![](https://i.imgur.com/UOLQc6k.png)

### SSTF
SSTF(Shortest-seek-time-first)，先找離目前位置最近的sector做I/O，再往較遠的移動，SSTF相較於FCFS來說是較好的演算法，但並不是最佳化的演算法，而且可能會有Starvation的問題。

![](https://i.imgur.com/GXKRITB.png)

### Scan
Scan演算法讓arm從磁碟的一端開始向另一端移動，然後依序對到達的sector做I/O，直到到達磁碟的另一端為止。以上述例子來說，會依序做I/O:

    14, 37, 65, 67, 98, 122, 124, 183

當各個有I/O需求的sector接近平均分佈時，當arm移動到其中一端並往回移動時，由於剛處理過靠近這端的I/O需求，因此這端的I/O請求應該會相對較少。

![](https://i.imgur.com/VwAupvv.png)


### C-Scan
C-Scan(Circular scan)可以解決Scan演算法的問題，C-Scan將磁臂自硬碟的一端移動到另一端，但當它到另一端後，立即返回到起始點。

![](https://i.imgur.com/T7LEB47.png)

### Look & CLook

在Scan/C-Scan演算法中，我們將磁臂移動到磁碟的最前端跟最後端，實際上，沒有任何一個演算法採用這個方式製作，大部分的演算法只將磁臂移動到最後一個sector那麼遠，就反轉方向。

![](https://i.imgur.com/bEJJAUz.png)


## RAID

RAID(Redundant array of inexpensive disk)，因為硬碟不斷地變小、變便宜，所以把多個硬碟放進一台電腦系統中就變得可行，RAID可以由HDD或SSD組成，主要的功能有兩個: 防止單一硬碟毀損導致資料遺失；平行化處理多個硬碟的I/O。

### Redundancy

若一台電腦中只有一個磁碟機，當此磁碟機損壞時，儲存的資料都會不見，然而，若是電腦中的一個(邏輯)磁碟機是一個RAID，內部由兩個磁碟機所構成，資料遺失的機會其實更高(兩個磁碟機任一個毀損就會遺失資料)。為了解決這個問題，最簡單(但浪費空間)的做法是mirroring，在每次寫入資料時，同時將資料寫進兩個磁碟機中，當任何一個磁碟機壞掉時，可以將資料從另一個磁碟機中讀出。使用了mirroring後，要兩台磁碟機同時壞掉才會遺失資料。

### Parallel

當我們對RAID進行讀取資料時，由於兩台(多台)實體磁碟機各自的讀取速度都跟一台磁碟機時一樣，但可以同時對多台磁碟機做讀取，因此，RAID總合起來的讀取速度會比對單一磁碟機快。

然而，若要完全的mirror，在獲得n倍讀取速度的同時，意味著儲存的空間會變成1/n，因此，我們再來會介紹利用不同方法、不同考量下設計的RAID架構，在兩者之間取得平衡。

![](https://i.imgur.com/Jf5mo5B.png)

Reference : https://zh.wikipedia.org/wiki/RAID

<!-- #### RAID0

RAID0，沒有任何Mirror的RAID架構，所有的資料都沒有重複。

#### RAID1

RAID1，所有的磁碟都有Mirror。

#### RAID2

RAID2利用了ECC(Error correcting code)的技術，ECC的基本概念時: 對於每一個Bytes，都有一個bit負責儲存此Bytes 1的數量是奇數還是偶數，藉由這個bit，可以用於判斷Bytes是否損毀。
在RAID2中，使用Hamming Code將將資料進行編碼後分割為獨立的bit

![](https://i.imgur.com/LyZbS4w.png)

Hamming code : https://zh.wikipedia.org/wiki/%E6%B1%89%E6%98%8E%E7%A0%81

#### RAID3



#### RAID4
#### RAID5
#### RAID6 -->
