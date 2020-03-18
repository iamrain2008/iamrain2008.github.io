---
title: 特殊的二叉树
date: 2020-03-09 00:26:27
categories:
  - Algorithm
tags:
  - binary tree
  - n-ary tree
  - binary search tree
  - algorithm
  - leetcode
---

# 二叉搜索树

## 特性

二叉搜索树是一种特殊的二叉树，它具有这样的性质：每个节点中的值必须大于（或等于）其左侧子树中的任何值，但小于（或等于）其右侧子树中的任何值。等于这个条件视情况而定，通常根据题目的要求来。

由于这样的特性，我们采用中序遍历的时候，会得到一个有序的排列，所以很多二叉搜索树的问题会用到中序遍历。

例子：

* [LeetCode 98. Validate Binary Search Tree](https://leetcode.com/problems/validate-binary-search-tree/) 

题目要求验证一个树是二叉搜索树。如果用普通的递归思想，验证左树是不是 && 验证右树是不是，会掉入题目的陷阱，即可能出现左树是二叉搜索树，右树也是，但左树有比当前节点更大的数值，或者右树有比节点更小的数字。这时候你就要额外的去记录左侧最大值和右侧最小值，它们必须小于和大于当前节点的值。

这时候我们换个思路，我们用中序遍历把值都放进一个数组，这个数组是有序的，则说明这是一个合格的二叉搜索树。当然也可以优化一下，每次遍历的节点都必然比上一个遍历的节点大。

```
class Solution {
    // 记录上一个访问过的节点的值
    private Integer previousNodeVal = null;
    
    public boolean isValidBST(TreeNode node) {
        if (node == null) {
            return true;
        }
        // 左 -> 中 -> 右，中序遍历
        boolean checkLeft = isValidBST(node.left);
        boolean checkSelf;
        if (previousNodeVal == null) {
            checkSelf = true;
        } else {
            // 当前节点大于上一个节点，说明合法
            checkSelf = node.val > previousNodeVal;
        }
        // 将每次访问过的节点记录下来
        previousNodeVal = node.val;
        
        boolean checkRight = isValidBST(node.right);

        return checkLeft && checkSelf && checkRight;
    }
    
}
```

## 查找

这个没什么好说的，递归或者非递归的写法都行，思路就是根据目标相对于当前节点的大小来判断去左边还是右边找。

* [LeetCode 98. Validate Binary Search Tree](https://leetcode.com/problems/validate-binary-search-tree/) 

## 插入

插入操作通常会有多个符合条件的结果，我通常用比较容易记忆的一种方案，就是根据需要插入的值不停的跟节点比较大小，一直找到某个节点的left或right为空的情况，然后创建一个节点放到对应的位置。

* [LeetCode 701. Insert into a Binary Search Tree](https://leetcode.com/problems/insert-into-a-binary-search-tree/) 

## 删除

二搜索树的删除稍微复杂一些，因为删除节点后，需要维持二叉树的性质。删除节点分为几种情况，假设有这样一个二叉搜索树：

```
     4
   /   \
  2     5
 / \     \
1   3     6
```

### 待删除节点有0个子树

例如我要删除节点6，直接删除即可，变成这样：

```
     4
   /   \
  2     5
 / \     
1   3     
```

### 待删除节点有1个子树

例如我要删除节点5，将5的子树6放到5原本的位置，删除5即可，变成这样：

```
     4
   /   \
  2     6
 / \      
1   3      
```

### 待删除节点有2个子树

还是这棵树，例如我要删除节点4，这怎么办呢，常用的删除方法又有2种：

```
     4
   /   \
  2     5
 / \     \
1   3     6
```

1. **方法1**：合并删除。找到待删除节点的左边最大值节点，这里是3，然后将待删除节点的右子树合并到3下面，变成：
    ```
      2 
     / \
    1   3
         \
          5
           \
            6 
    ```
2. **方法2**：复制删除。找到待删除节点左子树中的最大值，这里是3，复制到当前节点，这里是把4变成3，再删除被复制的节点，变成：
    ```
         3
       /   \
      2     5
     /       \
    1         6
    ```

可以看出来，方法1可能会导致树的平衡遭到破坏，这会降低二叉搜索树的效率。方法2会尽可能的维持原本的数高度，所以更推荐使用方法2删除节点。

* [LeetCode 450. Delete Node in a BST](https://leetcode.com/problems/delete-node-in-a-bst/)
    > 思路虽然容易理解，但代码写起来还是有点麻烦的，建议上手练习一下。

# 红黑树

假设给一个有序数组建立二叉搜索树，比如 [1,2,3,4,5]，会变成这样：
```
1
 \
  2
   \
    3
     \
      4
       \
        5
```
这样的话二叉搜索树就变得不平衡，效率低下，所以大佬们想了很多方法来构建一个平衡的二叉搜索树，有：

* [红黑树(Red–black tree)](https://zh.wikipedia.org/wiki/%E7%BA%A2%E9%BB%91%E6%A0%91)
* [AVL树(AVL tree)](https://zh.wikipedia.org/wiki/AVL%E6%A0%91)
* [数堆(Treap)](https://zh.wikipedia.org/wiki/%E6%A0%91%E5%A0%86)
* [伸展树(Splay tree)](https://zh.wikipedia.org/wiki/%E4%BC%B8%E5%B1%95%E6%A0%91)
* [加权平衡树（Weight balanced tree）](https://zh.wikipedia.org/wiki/%E5%8A%A0%E6%9D%83%E5%B9%B3%E8%A1%A1%E6%A0%91)
* ……

这些离我们都比较遥远，很少会用到，只要有了解就可以了。

里面稍微常见一点的是红黑树，红黑树具备这样的特征：

* 节点是红色或黑色。
* 根是黑色。
* 所有叶子都是黑色（叶子是NIL节点）。
* 每个红色节点必须有两个黑色的子节点。（从每个叶子到根的所有路径上不能有两个连续的红色节点。）
* 从任一节点到其每个叶子的所有简单路径都包含相同数目的黑色节点。

理解起来有点复杂，大家可以去google一下，有很多优秀的文章讲解。

例如 
* [红黑树 - 维基百科](https://zh.wikipedia.org/wiki/%E7%BA%A2%E9%BB%91%E6%A0%91)
* [30张图带你彻底理解红黑树](https://www.jianshu.com/p/e136ec79235c)
