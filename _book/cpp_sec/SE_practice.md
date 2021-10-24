# Practice!
###### tags = `Software Engineering`

## 題目

設計一個電影院的現場售票系統，這個系統包含:
* 售票員(可能有一個或多個售票員)
    * 賣票、收錢、選擇座位等等
* 顧客
    * 買票、付錢、選擇要看的電影等等
* 影廳(可能有一個或多個影廳)
    * 包含多個座位，假設座位表為一個2D的Array
    * 每個影廳大小可能不同、播放的電影可能不同
* 播放時刻表
    * 包含目前上映的電影、播放時刻、播放的影廳等等資訊
* 電影
    * 包含電影的資訊，例如片長、是否上映、電影簡介等等
* 販賣部
    * 顧客可以跟販買部購買各種食物、飲料等等，品項可以動態的增減
    * 預設賣可樂、紅茶、爆米花、吉拿棒

售票系統除了買賣票之外，如果有新上映的電影或要下映的電影，也要透過此系統更新(顧客不能買到沒有上映的電影票)。

## Input/Output
* 新增電影
    * Input
        * New_Movie {movie_name} {movie_time} {movie_description}
        * movie_time為電影長度，單位為分鐘
    * Output
        * 成功: New_Movie {movie_name} success.
        * 失敗: New_Movie {movie_name} fail.
* 上映電影
    * Input
        * Add_Movie {movie_name}
        * 選擇一個可以的影廳/時段上映，並加入播放時刻表
    * Output
        * 成功: Add_Movie {movie_name} at {cinema_id} success, time is {time}.
        * 失敗: Add_Movie {movie_name} fail.
* 下架電影
    * Input
        * Delete_Movie {movie_name}
    * Output
        * 成功: Delete_Movie {movie_name} success.
        * 失敗: Delete_Movie {movie_name} fail.
* 新增顧客
    * Input
        * New_Client {client_id}
    * Output
        * 成功: New_Client {client_id} success.
        * 失敗: New_Client {client_id} fail.
* 新增售票員
    * Input
        * New_Clerk {clerk_id}
    * Output
        * 成功: New_Clerk {clerk_id} success.
        * 失敗: New_Clerk {clerk_id} fail.
* 新增影廳
    * Input
        * New_Cinema {cinema_id} {row_num} {col_num}
    * Output
        * 成功: New_Cinema {cinema_id} success.
        * 失敗: New_Cinema {cinema_id} fail.
* 查看電影時間
    * Input
        * Search {movie_name}
    * Output
        * 成功，每個還有空位的影廳／時段都印出一行:
            * Movie {movie_name} {start_time} at {cinema_id}
        * 失敗:
            * Search {movie_name} fail.
* 購買電影票
    * Input
        * Buyticket {client_id} {clerk_id} {movie_name} {cinema_id} {start_time}
    * Output
        * 成功: {client_id} buy {movie_name} success, seat is {row}, {col}.
        * 失敗: {client_id} buy {movie_name} fail.
* 購買食物
    * Input
        * BuyFood {client_id} {clerk_id} {food_name}
        * Default food_name: Coke, Black Tea, Popcorn、Churros
    * Output
        * 成功: {client_id} buy {food_name} success.
        * 失敗: {client_id} buy {food_name} fail.

### Sample Input

```cpp=1
New_Movie {Harry Potter} {110} {A cool movie.}
New_Movie {Lord of The Ring} {130} {A cool movie too.}
New_Movie {Lord of The Ring} {110} {A cool movie too.}
Add_Movie {Harry Potter}
New_Cinema {1} {2} {2}
New_Cinema {2} {4} {4}
New_Cinema {2} {5} {5}
Add_Movie {Harry Potter}
Add_Movie {Harry Potter}
Add_Movie {Lord of The Ring}
Delete_Movie {Lord of The Ring}
Delete_Movie {Lord of The Ring}
New_Client {1}
New_Client {2}
New_Client {1}
New_Clerk {1}
Search {Harry Potter}
Search {Lord of The Ring}
Buyticket {1} {1} {Harry Potter} {1} {00:00}
Buyticket {1} {2} {Harry Potter} {1} {00:00}
Buyticket {3} {1} {Harry Potter} {1} {00:00}
Buyticket {2} {1} {Lord of The Ring} {1} {00:00}
Buyticket {2} {1} {Harry Potter} {1} {00:00}
Buyticket {2} {1} {Harry Potter} {1} {00:00}
Buyticket {2} {1} {Harry Potter} {1} {00:00}
Buyticket {1} {1} {Harry Potter} {1} {00:00}
Search {Harry Potter}
BuyFood {1} {1} {Black Tea}
BuyFood {2} {1} {Coke}
BuyFood {2} {1} {Banana}
```

### Sample Output

```cpp=1
New_Movie {Harry Potter} success.
New_Movie {Lord of The Ring} success.
New_Movie {Lord of The Ring} fail.
Add_Movie {Harry Potter} fail.
New_Cinema {1} success.
New_Cinema {2} success.
New_Cinema {2} fail.
Add_Movie {Harry Potter} at {1} success, time is {00:00}.
Add_Movie {Harry Potter} at {1} success, time is {02:00}.
Add_Movie {Lord of The Ring} at {2} success, time is {00:00}.
Delete_Movie {Lord of The Ring} success.
Delete_Movie {Lord of The Ring} fail.
New_Client {1} success.
New_Client {2} success.
New_Client {1} fail.
New_Clerk {1} success.
Movie {Harry Potter} {00:00} at {1}
Movie {Harry Potter} {02:00} at {1}
Search {Lord of The Ring} fail.
{1} buy {Harry Potter} success, seat is {1}, {1}.
{1} buy {Harry Potter} fail.
{3} buy {Harry Potter} fail.
{1} buy {Lord of The Ring} fail.
{2} buy {Harry Potter} success, seat is {1}, {2}.
{2} buy {Harry Potter} success, seat is {2}, {1}.
{2} buy {Harry Potter} success, seat is {2}, {2}.
{1} buy {Harry Potter} fail.
Movie {Harry Potter} {02:00} at {1}
{client_id} buy {Black Tea} success.
{2} buy {Coke} success.
{2} buy {Banana} fail.
```