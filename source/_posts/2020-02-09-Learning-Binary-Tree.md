---
title: 二叉树算法从入门到精通
categories:
  - Algorithm
tags:
  - binary tree
  - algorithm
  - leetcode
date: 2020-02-09 23:20:52
---

# 3种遍历顺序

二叉树遍历算法，根据根节点相对于左节点、右节点的不同访问顺序，分为`前序遍历(Pre-Order Traversal)`、`中序遍历(In-Order Traversal)`、`后序遍历(Post-Order Traversal)`。

* 前序遍历：根节点第一个访问：根-左-右；
* 中序遍历：根节点第二个访问：左-根-右；
* 后序遍历：根节点最后访问：左-右-根。

示例，假设有这样一个树：

```
      1
     / \
    /   \
   2     3
  / \   / \
 /   \ /   \
4    5 6    7
```

* 前序遍历：1-2-4-5-3-6-7
* 中序遍历：4-2-5-1-6-3-7
* 后序遍历：4-5-2-6-7-3-1

用递归实现的3种遍历是这样的：

```
/**
 * 这是我们的TreeNode的定义，后面所有例子用的TreeNode都是这样。
 */
public class TreeNode {
    int val;
    TreeNode left;
    TreeNode right;

    TreeNode(int x) {
        val = x;
    }
}

/**
 * 前序遍历
 */
public void treeTraversal(TreeNode root) {
    if (root == null) {
        return;
    }
    if (root.val is target) {
        do something
    }
    treeTraversal(root.left);
    treeTraversal(root.right);
}

/**
 * 中序遍历
 */
public void treeTraversal(TreeNode root) {
    if (root == null) {
        return;
    }
    treeTraversal(root.left);
    if (root.val is target) {
        do something
    }
    treeTraversal(root.right);
}

/**
 * 后序遍历
 */
public void treeTraversal(TreeNode root) {
    if (root == null) {
        return;
    }
    treeTraversal(root.left);
    treeTraversal(root.right);
    if (root.val is target) {
        do something
    }
}
```


# 2种遍历策略

树的遍历分为深度优先遍历和广度优先遍历。

## 深度优先遍历

深度优先(Depth First Search, DFS)的意思就是，先一条道走到黑，走到不能再走了，再回过头了遍历其他。
下面用一个最简单的例子来说明不同的深度优先遍历。

> [LeetCode 104. Maximum Depth of Binary Tree](https://leetcode.com/problems/maximum-depth-of-binary-tree/)

深度优先遍历大致分为递归实现和非递归实现，递归实现又分为分治法实现和遍历法实现。

### 用递归实现

#### 分治法

最常见，最好理解的方法，就是分治法。左右叶子节点返回它们自身的深度，取大的那个深度，+1再返回。

```
public int maxDepth(TreeNode root) {
    if (root == null) {
        return 0;
    }
    int left = maxDepth(root.left);
    int right = maxDepth(root.right);
    return Math.max(left, right) + 1;
}
```

#### 遍历法

遍历法与分治法的本质区别是，分治法有个回归的过程，而遍历法没有。

```
int maxDepth;

public int maxDepth(TreeNode root) {
    maxDepth = 0;
    if (root == null) {
        return maxDepth;
    }
    // 当前的深度为1，开始遍历
    calculateTreeDepth(root, 1);
    // 返回最终结果
    return maxDepth;
}

public void calculateTreeDepth(TreeNode treeNode, int currentDepth) {
    if (treeNode == null) {
        // 如果当前节点是null就什么都不做
        return;
    }
    // 如果当前节点的深度大于已记录的最大深度，更新最大深度
    if (currentDepth > maxDepth) {
        maxDepth = currentDepth;
    }
    // 遍历左右子树，进入左右子树时的深度是当前深度+1，如果子树是null则不会更新maxDepth
    calculateTreeDepth(treeNode.left, currentDepth + 1);
    calculateTreeDepth(treeNode.right, currentDepth + 1);
}
```

### 用非递归实现

非递归（即迭代法）实现需要借助`栈`这个数据结构，实现方法比递归的麻烦一些，但很重要，一定要理解它的实现原理。
还是以上面的计算树的最大深度为例子（其实更好的做法是广度优先遍历，这里只是为了展示非递归的实现）。


```
public int maxDepthDfsNonRecursion(TreeNode root) {
    int maxDepth = 0;
    if (root == null) {
        return maxDepth;
    }

    // Pair: key - TreeNode, value - int, current depth
    Stack<Pair<TreeNode, Integer>> stack = new Stack<>();
    // Push root node to stack
    stack.push(new Pair<>(root, 1));

    while (!stack.isEmpty()) {
        Pair<TreeNode, Integer> pair = stack.pop();
        TreeNode node = pair.getKey();
        int currentDepth = pair.getValue();

        // Leaf, save the max depth
        if (node.left == null && node.right == null && currentDepth > maxDepth) {
            maxDepth = currentDepth;
        }

        if (node.right != null) {
            stack.push(new Pair<>(node.right, currentDepth + 1));
        }
        if (node.left != null) {
            stack.push(new Pair<>(node.left, currentDepth + 1));
        }
    }
    return maxDepth;
}
```

## 广度优先遍历

广度优先(Breadth First Search, BFS)的意思就是，优先遍历当前节点的所有子节点，不管子节点还有没有孙子节点。再一层一层向下遍历。

通常需要借助`队列`来完成遍历，思路是：记录当前层的数量，开始遍历，将叶子节点放入队列，当遍历的数量等于当前层数量时，说明当前层已经遍历完了；检查队列是否为空，不为空说明还有下层，继续重复刚才的操作；知道全部遍历完成。

```
public int maxDepthBfs(TreeNode root) {
    int maxDepth = 0;
    if (root == null) {
        return maxDepth;
    }
    // 建立队列辅助计算，并添加根元素
    Queue<TreeNode> queue = new LinkedList<>();
    queue.offer(root);
    // 队列不空就开始遍历
    while (!queue.isEmpty()) {
        // 记录当前层一共有多少个元素
        int currentLevelSize = queue.size();
        // 记录已经查看了多少个元素
        int visitedSize = 0;
        // 开始遍历当前层
        while (visitedSize < currentLevelSize) {
            TreeNode node = queue.poll();
            if (node != null) {
                if (node.left != null) {
                    // 有左叶子节点，则加入队列
                    queue.offer(node.left);
                }
                if (node.right != null) {
                    // 有右叶子节点，则加入队列
                    queue.offer(node.right);
                }
            }
            // 处理当前node的值，本例中无需处理
            // 记录我们看过一个元素了，当当前层所有元素都看过之后，会退出当前while循环
            visitedSize++;
        }
        // 遍历完一层的时候，最大深度+1，如果下层还有元素，则会继续遍历
        maxDepth++;
    }
    // 所有节点遍历完成，返回结果
    return maxDepth;
}
```
