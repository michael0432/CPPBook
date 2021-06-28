# Socket
###### tags = `Operating System`


## Network Protocols
![](https://i.imgur.com/rkEdx4n.png)


Socket是一個網路通訊上的End point，只要知道IP Address跟port，就可以找到對應的socket透過網路進行I/O，在linux系統上，socket也可以被視為一個類似檔案的存在，一個socket會對應到一個file descriptor。

Socket由IP Address及port組成，通常表示成:
* 123.456.789.012:3456
* :前為IP Address，:後為port

![](https://i.imgur.com/cwBJSRN.png)


### IP Address
IP Address用於辨認機器，分為IPv4及IPv6。

### Port
Port在同一台機器上不會重複，為一個16bit int，用於接收/傳遞不同地方來的封包，通常一個process會負責處理一個port或多個port的資料。

## TCP & UDP
TCP跟UDP是在Transport Layer中最常見的兩種傳輸協定，這兩種傳輸協定定義了在系統之間傳輸資料的形式，包含如何建立連線、如何傳遞封包、封包的格式、錯誤處理等等。

### TCP (Transmission Control Protocol)

TCP是一種確保封包可以連續性傳遞的協定，TCP會將資料分成較小的封包，並根據其順序依序編號，確保封包排列的順序。為了確保資料正確無誤的傳輸，Client與Server端會使用handshake的機制讓雙方確認有收到正確的資料。
在每個封包傳遞時，步驟如下:

* Client傳送封包給Server
* Server收到封包後，回傳ACK
* Client收到ACK，表示Server有收到封包
* Client傳送ACK給Server，表示有收到Server的ACK，此時Client可以確定此次傳輸無誤
* Server收到ACK，此時Server可以確認此次傳輸無誤

若上述傳遞過程有誤，發現錯誤的一方會通知另一方，並重新傳輸資料，因此TCP是一種可以確保資料正確傳輸的傳輸協定。

![](https://i.imgur.com/ihK3pfb.png)

### UDP (User Datagram Protocol)

相較於TCP，UDP是一種較不可靠(較不確認資料正確傳輸)的傳輸協定，由於UDP協定本身不需要確保資料的完整性及正確性，因此傳輸負擔較輕(花較少資源傳遞協定相關的資料)。
UDP不會使用確認的機制確保資料正確、照順序傳輸，接收資料時也不一定要照順序接收資料，當資料有遺漏時，不會重新傳遞資料。

![](https://i.imgur.com/5AjDlyF.png)



### TCP v.s. UDP

![](https://i.imgur.com/gIidEe2.png)


### TCP programming in Linux

![](https://i.imgur.com/sanmfyU.png)

### UDP programming in Linux

![](https://i.imgur.com/95ysD5X.png)

## Socket 用法

### Socket 

建立一個Socket

```cpp=1
#include <sys/socket.h>
int socket(int domain, int type, int protocol);
```

* domain: 定義socket溝通的protocal
    * AF_INET : IPv4
    * AF_INET6 : IPv6
    * AF_UNIX  : 本機Process間的溝通
* type: 選擇此socket使用TCP還是UDP傳輸
    * SOCK_STREAM: TCP，資料會序列化的傳送
    * SOCK_DGRAM: UDP，資料為一個一個datagram傳送
* protocol: 通常設定為0，kernel會根據type選擇適合的protocal
* return value:
    * 成功時回傳fd
    * 失敗時為傳-1

### Bind

Bind將socket fd跟socket address組合起來，socket address會包含socket所需的所有資訊。

```cpp=1
#include <sys/socket.h>
#include <metinet/in.h>

int bind(int socketfd, struct sockaddr *address, socklen_t address_len);

struct sockaddr 
{
    unsigned short sa_family; // domain, eg. AF_INET
    char sa_data[14];         // 14 bytes address
}

struct sockaddr_in 
{
    short sin_family; // domain
    // 下面三個變數會組合成 sockaddr.sa_data
    unsigned_short sin_port; // htons, 2 bytes
    struct in_addr sin_addr; // 用於填寫IP Address, 4 bytes
    char sin_zero[8];        // 不會用到
}

```

* socketfd : socket fd
* address : 填寫IP Address、domain
    * 由於sa_data很難填寫，填寫時通常使用sockaddr_in這個結構體填寫後，再轉型成sockaddr
* address_len : address這個結構體的大小，填sizeof(address)即可
* return value:
    * 成功回傳0
    * 失敗回傳-1

### Listen

Server使用listen來確認是否有新的client，會反覆去查看是否有新的client連線。

```cpp=1
#include <sys/socket.h>
int listen(int socketfd, int backlog);
```

* socketfd : socket fd
* backlog : 指定可以有幾個client連到此socket，若超過一個client連入，會依序排隊
* return value:
    * 成功回傳0
    * 失敗回傳-1

### Accept

當有Client連線後，使用accept接受此連線，若成功接收連線，會為此連線新增一個新的socket。

```cpp=1
#include <sys/socket.h>
int accept(int socketfd, struct sockaddr *address, socklen_t address_len);
```

* socketfd : socket fd
* address : 放進accpet前為空，用於儲存Client端的address資訊(port及IP)
* address這個結構體的大小，填sizeof(address)即可
* return value:
    * 成功回傳接受連線的Socket fd
    * 失敗回傳-1

### Connect

Client連向Server時，需要先知道Server的IP跟port，並使用Connect請求連線。

```cpp=1
#include <sys/socket.h>
int connect(int socketfd, struct sockaddr *address, socklen_t address_len);
```

* socketfd : socket fd
* address : server的IP及Port等資訊
* address_len : address這個結構體的大小，填sizeof(address)即可

### Recv

Server接收資料時，就是從socket fd讀取資料，用最基本的read就可以讀資料了，這邊介紹另一種讀取方式recv，recv只有在fd為socket時才能使用。

```cpp=1
#include <sys/socket.h>
ssize_t recv(int socketfd, void* buf, size_t len, int flags);
```

* socketfd : socket fd
* buf : 一塊buffer，用於讀取socket中拿到的資料
* len : buffer的大小
* flags : 通常設定為0，用來做其他讀取相關的設定，例如設定Nonblock等等。
* return value:
    * 回傳收到了幾個bytes的資料
    * 如果對方結束連線，會收到0
    * 失敗回傳-1

### Send

Send將資料寫到特定的socket fd，跟write的用法類似，但一樣只能使用在fd為socket的情況下。

```cpp=1
#include <sys/socket.h>
ssize_t send(int socketfd, const void* buf, size_t len, int flags);
```

* socketfd : socket fd
* buf : 一塊buffer，放要寫入的資料
* len : buf的大小
* flags : 通常設定為0，用來做其他寫入相關的設定，例如設定Nonblock等等。
* return value:
    * 回傳送出了幾個bytes的資料
    * 失敗回傳-1

## Example

### Server

```cpp=1
// server.cpp
# include <stdio.h>
# include <stdlib.h>
# include <unistd.h>
# include <sys/types.h>
# include <sys/socket.h>
# include <arpa/inet.h>
# include <netinet/in.h>

int main()
{
    int sockfd = socket(AF_INET, SOCK_STREAM, 0);
    if(sockfd < 0) printf("Socket Create Error\n");

    struct sockaddr_in info;
    struct sockaddr_in client_info;
    socklen_t client_addr_len;
    info.sin_family = AF_INET;
    info.sin_addr.s_addr = INADDR_ANY; // Local IP Address
    info.sin_port = htons(8080); // Port 8080

    int bind_result = bind(sockfd, (struct sockaddr*)&info, sizeof(info));
    if(bind_result < 0) printf("Bind Error\n");

    listen(sockfd, 1); // block until listen a client

    int client_fd;
    while(1)
    {
        client_fd = accept(sockfd, (struct sockaddr*)&client_info, &client_addr_len);
        printf("Accept client, client port: %d\n", client_info.sin_port);
    }
    return 0;
}

```

### Client

```cpp=1
//client.cpp

# include <stdio.h>
# include <stdlib.h>
# include <unistd.h>
# include <sys/types.h>
# include <sys/socket.h>
# include <arpa/inet.h>
# include <netinet/in.h>

int main()
{
    int sockfd = socket(AF_INET, SOCK_STREAM, 0);
    if(sockfd < 0) printf("Socket Create Error\n");

    struct sockaddr_in info;
    info.sin_family = AF_INET;
    info.sin_addr.s_addr = inet_addr("127.0.0.1"); // Local IP Address
    info.sin_port = htons(8080); // Port 8080

    int connect_result = connect(sockfd, (struct sockaddr*)&info, sizeof(info));
    if(connect_result) printf("Connect Error\n");

}

```

## 小練習
寫兩個程式碼: server.cpp, client.cpp，讓兩者可以連線並傳輸資料。

* Server先接收到連線後，收到client傳來的資料，在server印出收到的資料
* Server回傳資料給client，在client印出收到的資料
* 資料格式不限制 
