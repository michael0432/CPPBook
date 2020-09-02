# Practice - Bank System
## !!!!建議兩個人一起寫

設計一個銀行系統，系統的功能包含:
* login: 根據使用者ID及密碼，確認使用者是否能登入，系統中不能直接存使用者的密碼
* create: 建立一個帳戶，若輸入的帳號存在，系統需建議幾個相近的帳戶
* delete: 刪除一個帳戶
* deposit: 將錢存入一個帳戶
* withdraw: 將錢從一個帳戶中提領
* transfer: 將錢從一個帳戶轉到另一個帳戶
* merge: 將兩個帳戶合併成一個帳戶
* search: 尋找所有transfer的紀錄
* find: 根據搜尋條件，找出所有符合搜尋條件的帳戶ID

## Spec

* Data Type
    * ID, Password: 只包含數字及英文字母大小寫
    * moneny: integer範圍的正整數(0~2^31-1)

* Password Encoding
    * md5: https://en.wikipedia.org/wiki/Password
    * 使用md5加密密碼，可以使用C++的套件 (http://www.zedwood.com/article/cpp-md5-function)

* 帳號存在時，推薦其他帳號的方式
    * 系統要推薦十個沒人使用的帳號
    * 根據使用者輸入的帳號，算出十個分數最低且沒人使用的帳號
    * 分數的計算方式，u、v為兩個帳號:
        * $\Delta{L} = |u.size() - v.size()|$，兩個字串的長度差距
        * $L = min(u.size(), v.size())$，兩個字串中比較短的長度
        * $score(u,v) = \Sigma_{i=1}^{\Delta{L}} i + \Sigma_{i=0}^{L-1} (u[i]!=v[i]?(L-i):0)$
        * $\Sigma_{i=1}^{\Delta{L}} i$ : 這段的意思為累加長度差距(例如$\Delta{L}$=2時，這段累加的結果為3)
        * $\Sigma_{i=0}^{L-1} (u[i]!=v[i]?(L-i):0)$ : 這段的意思為，從頭到尾比字串，若字元不同，分數加L-i，即越前面的字元不同，分數越高
        * Example:
            * score("abc", "abcd") = 1 + 0 = 1 
            * score("abce", "abcd") = 0 + 1 = 1
            * score("abed", "abcd") = 0 + 2 = 2
            * score("abde", "abcd") = 0 + (2 + 1) = 3
            * score("abcefg", "abcd") = (1 + 2) + 1 = 4
    * 輸出十個帳號時，分數從低排到高，分數一樣時，直接根據C++內的字串比大小排序，從小排到大
    
* 各指令的細節
    * 若Output符合列出的好幾個狀況，輸出優先順序為由上到下，只會輸出優先順序最高的output
    *
    * 以下指令不需要登入
        * login
            * Input: login [ID] [password]
            * Output:
                * ID [ID] not found 
                * wrong password
                * success
            * 
        * create
            * Input: create [ID] [password]
            * Output:
                * ID [ID] exists, [best 10 unused IDs (separated by ",")]
                * success
        * delete
            * Input: delete [ID] [password]
            * Output:
                * ID [ID] not found
                * wrong password
                * success
            * [ID]不會等於目前登入的ID
        * merge
            * Input: merge [ID1] [password1] [ID2] [password2]
            * Output:
                * ID [ID1] not found
                * ID [ID2] not found
                * wrong password1
                * wrong password2
                * success, [ID1] has [X] dollars
            * [ID1]不會等於[ID2]
            * 若[ID1]跟[ID2]有交易紀錄(這筆交易紀錄在兩個帳戶的紀錄時間會同時)，合併後[ID1]的交易紀錄在前，另一筆在後
        * find
            * [wildcard ID]是一個包含*跟?的字串，*表示0到n個任意的char，?表示剛好1個任意的char，*不會連續出現(**)，?跟*也不會連續出現(*?或?*)
            * Input: find [wildcard ID]
            * Output
                * [all satisfying IDs (separated by ",") in ascending dictionary order]
            * Example
                * a*b?:  ab0,a0b1,a00b2,abbbbb ...
                * a??b*: abbb,a01bbbb,a01bacb ...
                * a*b*: ab,abb,abbbb,acccbccab ...
                * *a?b*: aab,cccasbbb,twaiblwe ...
            
            
    * 以下指令需要登入
        * deposit
            * Input: deposit [num]
            * Output:
                * success, [X] dollars in current account
        * withdraw
            * Input: withdraw [num]
            * Output
                * fail, [X] dollars only in current account //錢不夠
                * success, [X] dollars left in current account
        * transfer
            * Input: transfer [ID] [num]
            * Output
                * ID [ID] not found
                * fail, [X] dollars only in current account //錢不夠
                * success, [X] dollars left in current account
        * search
            * Input: search [ID]
            * Output
                * no record //包含[ID]不存在
                * [transfer history from/to ID, line by line]
                    * 格式: From/To [ID] [dollars]
                    * Example
                        * From Frank 10000
                        * To Franl 200
        

## Sample Input / Output

### Sample Input

    create g1 123
    create ab0 456
    create aabb 789
    create g1 1234
    login g1 123
    deposit 5000
    transfer aabb 2499
    search aabb
    login aabb 789
    search g1
    
### Sample Output

    success
    success
    success
    ID g1 exists, g,g0,g10,g11,g12,g13,g14,g15,g16,g17
    success
    success, 5000 dollars in current account
    success, 2501 dollars left in current account
    To aabb 2499
    success
    From g1 2499
    
## 製作Test Case

製作一個100筆的測試資料，並測試自己寫的程式是否正確

## 比較兩個檔案是否相同

在cmd中:

    dc file1.txt file2.txt

