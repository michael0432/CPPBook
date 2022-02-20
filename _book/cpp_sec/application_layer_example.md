# Application Layer Example

## HTTP
HTTP(Hyper Text Transfer Protocal)是一種用於網頁瀏覽的協定，其設計的目的是提供發送和接收HTML頁面。
HTTP為Client-Server架構，並且假設Transport Layer以下的傳輸為可靠的傳輸，因此通常搭配TCP使用。
http Sserver通常使用port 80/443 (http/https)，Client端向Server端的port 80/443發送一個http request，http server會在port 80/443 listen requeset，一旦收到request並處理完後，會向client端回傳一個狀態。
![](https://i.imgur.com/tc0aGHG.png)

Http request中定義了幾種形式，用於對Server執行不同的操作，Server端會根據收到的request類別做不同的操作。

* Get : 讀取資料
* Post : 提交資料
* Put : 更新資料
* Delete : 刪除資料

## FTP
FTP(File Transfer Protocal)是一種用於傳輸檔案的協定，其設計的目的是用於在不同的裝置上透過網路傳遞檔案。
FTP為Client-Server架構，因為傳輸檔案不允許任何檔案毀損及錯誤，所以FTP通常搭配TCP使用。
FTP會使用到port 20及port 21；port 20用來傳輸資料，port 21用來下指令給server。
![](https://i.imgur.com/EZwUu3Q.png)

1. Client下指令到port 21，確認連線並指定傳輸內容
2. Server ACK
3. Server開始傳輸資料到Client端
4. Client ACK

## DHCP
DHCP(Dynamic Host Configuration Protocol)用於內網分配IP使用，DHCP Server會分給內網裝置Private IP以避免Public IP不足的問題。
DHCP為Client-Server架構，其中Client端為需要Private IP的機器；Server端為DHCP Server(通常為內網對外的Router)。由於Request Private IP只會在機器連上網路時做一次，因此使用UDP。

![](https://i.imgur.com/MhBYqTW.png)

## DNS
DNS(Domain Name System)用於將Host name mapping到IP Address。根據TCP/IP的概念，當使用者想要跟一台Server連線，需要知道其IP Address及Port number。根據不同的連線需求，我們可以知道要用哪個port number；但我們沒辦法記下所有的IP Address，通常使用者選擇輸入網址(Host name)而不是IP Address。

DNS為Client-Server架構，使用者透過跟DNS服務溝通取得IP Address，由於跟DNS溝通的次數通常為一次，因此DNS通常使用UDP連線。

![](https://i.imgur.com/RyKOImv.png)

## SMTP
SMTP(Simple Mail Transfer Protocol)用於傳送和接收電子郵件。SMTP有四個常用的port:25、465、587。
* Port 25: 主要用於Server-to-server的郵件傳送
* Port 465: 最初作為SMTPS使用的port，加密版的smtp
* Port 587: 目前最常使用的smtps port

SMTP為Client-Server架構，使用者透過跟Mail server連線以傳送、接收電子郵件。

![](https://i.imgur.com/JH71Mdp.png)

* Note : 目前SMTP幾乎只被用於傳出電子郵件；接收電子郵件會使用IMAP或POP協定。
    * POP : 一旦郵件下載到本機端就會在Mail server上被刪除
    * IMAP : 不下載電子郵件而是直接從Mail server上讀取郵件
    
## SSH
SSH(Secure Shell)用於進入Server，通常我們連入Sever會使用:
    
    ssh username@servername
    ssh username@serverip

SSH為Client-Server架構，在本機端會有一個ssh client process；在server端會有一個ssh server process。ssh server process會listen port 22的TCP連線，用來確認是否有使用者連入。

ssh連線的雙方會有彼此的公鑰(public key)；並各自擁有自己的私鑰(private key)。

當傳送方送出資料時:
* 用自己的private key簽名
* 用接收方的public key加密資料

當接收方收到資料時:
* 用接收方的private key解密
* 用傳送方的public key確認簽名正確

![](https://i.imgur.com/ERKpgpl.png)
