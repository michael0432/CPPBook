# BFS
###### tags = `Data Structure`


給一張Graph，要怎麼遍歷這張圖？（讀到圖中的每個Node）
* BFS(Breadth-fisrt Search)
    * 從圖的一個Vertex出發
    * 先走離該Vertex相鄰的Vertex(距離為1)
    * 再走到距離為2的Vertex
    * 直到走到最遠的點（走完所有點）

![](https://i.imgur.com/OqbqQjQ.png)

* DFS(Depth-first Search)




想要依照BFS的順序走完所有點，可以想成找出尚未遍歷的點當做起點：
1. 把起點放入queue
2. 拿出queue的front
3. 找出與queue.front()相鄰的點放入queue
4. queue.pop()
5. 重複2~4直到queue為空

![](https://i.imgur.com/kFEzg9G.png)
### Traverse Tree
```cpp=1
struct Node{
    int data;
    vector<*Node> neigh; 
}; 

void bfs(Node* first){
    queue<Node*> q;
    q.push(first);
    while(!q.empty()){
        // do what u want
        q.front()->val;
        //
        for(int i=0 ; i<q.front()->neigh.size() ; i++){
            q.push(q.front()->neigh[i]);
        }
        q.pop();
    }
}

```

### Traverse Graph
上面的例子只能遍歷整個Tree，但若想遍歷Graph，會碰到一問題：會遍歷到重複的點導致遍歷的順序混亂，因此我們需要在Node中加入一個變數儲存Node的狀態，在上圖中用白、灰、黑表示。
* 白：還未遍歷到此Node（未放進Queue）
* 灰：遍歷到此Node，但尚未離開Queue
* 黑：已經遍歷完此Node（已pop掉）

```cpp=1
struct Node{
    int data;
    int status; //白:0, 灰:1, 黑:2
    vector<*Node> neigh; 
}; 

void bfs(Node* first){
    queue<Node*> q;
    q.push(first);
    while(!q.empty()){
        q.front()->status = 1;
        // do what u want
        q.front()->val;
        //
        int size = q.size();
        for(int d=0 ; d<size ; d++){
            // 一層
            for(int i=0 ; i<q.front()->neigh.size() ; i++){
                // 一個鄰居，白色才放進queue
                if(q.front()->neigh[i]->status == 0){
                    q.push(q.front()->neigh[i]);
                }
            }
            q.front()->status = 2;
            q.pop();
        }
    }
}
```

### 小練習
* https://leetcode.com/problems/binary-tree-right-side-view/
* https://leetcode.com/problems/maximum-level-sum-of-a-binary-tree/
* https://leetcode.com/problems/number-of-closed-islands/
* https://leetcode.com/problems/cousins-in-binary-tree/
* https://leetcode.com/problems/binary-tree-level-order-traversal/
* https://www.hackerrank.com/challenges/bfsshortreach/problem
