# Tree

###### tags = `Data Structure`

### Tree
Tree是一種很特別的Graph，定義是：任兩個Vertex間都能相通（到達），並且沒有Cycle（繞圈的路線）。
![](https://i.imgur.com/7QHQ0aJ.png)

* Node(Vertex): 樹的每個點
* Branch(Edge): 分枝
* Root: 根，樹的最初的點，在無向圖中每個node都可以作為root
* Leaf: 沒有繼續分枝的Node(只連著一個edge的node)
* Level(Depth): 樹有幾層
* Parent: 以邊相鄰的任兩點，靠近root者為parent
* Child: 以邊相鄰的任兩點，遠離root者為child
* Ancestor: 一點的parent/parent的parent....皆是該點的ancestor，Root為其他所有點的ancestor
* Descendant: 一點的child/child的child....皆是該點的descendant

### Binary Tree
Binary tree就是分兩岔的樹，每個Node可以有0、1或2個child。有許多演算法都是從Binary Tree衍生而來，例如Binary Search、Merge Sort...
上圖就是一個Binary Tree。
Node of binary tree:
```cpp=1
struct Node{
    Node* parent;
    Node* left;
    Node* right;
    int data;
}
```
### Binary tree traversal
遍歷Binary tree的方式有四種：Preorder、Inorder、Postorder、Level-order，但其實他們只是DFS跟BFS的變形。
* Preorder: 遍歷順序為root、左子樹、右子樹
* Inorder: 遍歷順序為左子樹、root、右子樹
* Postorder: 遍歷順序為左子樹、右子樹、root
* Level-order: BFS
![](https://i.imgur.com/2AAYAE1.png)

### 小練習
* https://leetcode.com/problems/binary-tree-inorder-traversal/
* https://leetcode.com/problems/n-ary-tree-preorder-traversal/

### Reference
* http://www.csie.ntnu.edu.tw/~u91029/Tree.html