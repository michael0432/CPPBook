# Process Communication
###### tags = `Operating System`

作業系統需要提供環境讓process之間互相合作，因此需要提供Process之間互相溝通的方式。

舉例來說，Chrome瀏覽器由多個process所組成，因此當一個分頁crash時，不會導致整個瀏覽器crash。

Process之間互相溝通(Interprocrss communication, IPC)可以分成兩種:
* shared memory: 記憶體的某個區塊被兩個或多個process共同使用，Process藉由讀寫資料到同一塊記憶體來交換資訊。
* message passing: 將訊息傳遞到非兩個process使用的空間，在此空間傳遞訊息

這邊會介紹三種不同的通訊方式: Pipe, Fifo, Socket