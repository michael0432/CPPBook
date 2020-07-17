# Binary Search Tree
###### tags = `Data Structure`

Binary Search Tree用Binary Tree的結構加速搜尋的速度，從root出發，藉由每個node上的資訊判斷要往左子樹搜尋還是右子樹搜尋。

![](https://i.imgur.com/IodBmcf.png)

Binary Search Tree的特性:
* 對於每個子樹而言，root的值>所有左子樹的值，且root的值<所有右子樹的值

要可以維持BST，需要設計下列幾個操作:
* Search
* Insert
* Delete

```cpp=1
class Node{
    Node* left;
    Node* right;
    int value;
};

bool search(Node* root, int target)
bool insert(Node* root, Node* newNode)
```

## Search
從root開始尋找需要的資料，並判斷要往左子樹找還是右子樹找。

```cpp=1
bool search(Node* root, int target){
    if(!root) return false;
    if(root->value == target){
        return true;
    }
    else if(target > root-value){
        return search(root->right, target);
    }
    else{
        return search(root->left, target);
    }
}
```

## Insert
一樣從root開始判斷，判斷要加入的node要被放入左子樹的部分還是右子樹的部分

```cpp=1
bool insert(Node* root, Node* newNode){
    if(newNode->value == root->value){
        return false;
    }
    else{
        if(newNode->value > root->value){
            if(root->right == nullptr){
                root->right = newNode;
                return true;
            }
            else
                return insert(root->right, newNode);
        }
        else{
            if(root->left == nullptr){
                root->left = newNode;
                return true;
            }
            else
                return insert(root->left, newNode);
        }
    }
}
```

## Delete
當要刪除一個node時，會有三種情況:
* 此node沒有child
    * 直接將其parent的此child變成null
* 此node有一個child
    * 將child變成原parent的child
* 此node有兩個child
    * 找右子樹中的最小值來補這個位置，並把右子樹中的最小值刪除

```cpp=1
Node* delete(Node* root, int target){
    if(!root) return false;
    if(root->value == target){
        if(root->left == nullptr && root->right == nullptr){
            root = nullptr;
        }
        else if(root->left == nullptr && root->right != nullptr){
            root = root->right;
        }
        else if(root->left != nullptr && root->right == nullptr){
            root = root->left;
        }
        else{
            Node* n = root->right;
            while(n->left)
                n = n->left;
            root->val = n->val;
            root->right = delete(root->right, n->val);
        }
    }
    else if(target > root->value){
        root->right = delete(root->right, target);
    }
    else{
        root->left = delete(root->left, target);
    }
}
```

時間複雜度?
