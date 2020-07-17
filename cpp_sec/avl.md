# AVL Tree
###### tags = `Data Structure`

AVL Tree是一種維持BST平衡的BST，藉此可以維持運算的時間複雜度穩定。
平衡的BST的定義為:
* 左子樹及右子樹的高度最多相差1
* Balance Factor(N): 節點N的左子樹高度-右子樹高度
    * BF(N)<-1: 右子樹較高，需要將右子樹的點往左移
    * BF(N)>=-1 and BF(N)<=1: 平衡的樹
    * BF(N)>1: 左子樹較高，需要將左子樹的點往右移
    
![](https://i.imgur.com/ArPqtvG.png)

## Rebalance

遇到不平衡的情況時，需要調整樹的節點

### Left Rotation

讓樹往左邊旋轉達到平衡
下圖對A做left rotation時:
* 把B的left child設成A right child
* 把A變成B的left child

![](https://i.imgur.com/g9h17uG.png)
![](https://i.imgur.com/r9s3T30.png)



### Right Rotation

讓樹往左邊旋轉達到平衡
下圖對A做left rotation時:
* 把B的right child設成A left child
* 把A變成B的right child

![](https://i.imgur.com/NqR5aPZ.png)
![](https://i.imgur.com/8rgw11l.png)

* 當BF>1，做right rotation
* 當BF<-1，做left rotation

### Example

1. LL - 對A做Right rotation
![](https://i.imgur.com/2xCzBx8.png)

2. LR - 對B做完left rotation後，會變成LL
![](https://i.imgur.com/1xDpOwo.png)

3. RR - 對A做Left rotation
![](https://i.imgur.com/iiYSu4d.png)

5. RL - 對B做完right rotation後，會變成RR
![](https://i.imgur.com/z08AJg1.png)
