# DFS
###### tags = `Data Structure`

給一張Graph，要怎麼遍歷這張圖？（讀到圖中的每個Node）

![](https://i.imgur.com/OqbqQjQ.png)

* DFS(Depth-first Search)
    * 從其中一個vertex出發
    * 找一條路一直走走到沒路走為止(先遇到的vertex先搜尋)
    * 再回到有路走的node繼續走
* Tree的Inorder, Preorder, Postorder都是DFS
* 以上圖來說，有很多種dfs的走法
    * Example: 1->2->5->9->10->6->3->4->7->11->8->12
* 以DFS遍歷通常都是用遞迴

### Traverse Tree
* Preorder
* Inorder
* Postorder

### Traverse Graph
用DFS遍歷Graph的時候一樣會碰到怎麼處理Cycle的問題，我們一樣使用白灰黑三種狀態來表示Node的狀態。
* 白：還未遍歷到此Node
* 灰：遍歷到此Node，但他的child還沒遍歷完
* 黑：已經遍歷完此Node

當作dfs時，若是走到該Node為灰色或黑色，則有Cycle

```cpp=1
struct Node{
    int data;
    int status; //白:0, 灰:1, 黑:2
    vector<*Node> neigh; 
}; 

void dfs(Node* first){
    first->status = 1;
    first->data; // use data
    for(int i=0 ; i<first->neigh.size() ; i++){
        if(first->neigh[i]->status == 0) 
            dfs(first->neigh[i]);
        else
            // cycle
    }
    return;
}
```

### 小練習
* https://leetcode.com/problems/maximum-depth-of-n-ary-tree/
* https://leetcode.com/problems/same-tree/
* https://leetcode.com/problems/path-sum/
* https://leetcode.com/problems/flood-fill/
* https://leetcode.com/problems/number-of-closed-islands/

