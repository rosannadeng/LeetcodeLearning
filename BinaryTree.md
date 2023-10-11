二叉树
===
* [一. 树的遍历](#遍历)

* [二. 树的递归](#递归解法)

* [三. 解题思路](#二叉树的题目通用思考过程)

* [四. 前中后序详解](#前中后后序位置的特殊之处)

* [五. 二叉树问题拓展](#二叉树问题的扩展：)

### 遍历
前中后序是遍历二叉树过程中处理每一个节点的三个特殊时间点：

- 前序位置的代码在刚刚进入一个二叉树节点的时候执行；

- 后序位置的代码在将要离开一个二叉树节点的时候执行；

- 中序位置的代码在一个二叉树节点左子树都遍历完，即将开始遍历右子树的时候执行。

```python
def traverse(root):
    if root is None: return
    #前序位置
    traverse(root.left)
    #中序位置
    traverse(root.right)
    #后序位置
```

*为什么多叉树没有中序位置?*

因为二叉树的每个节点只会进行唯一一次左子树切换右子树，而多叉树节点可能有很多子节点，会多次切换子树去遍历，所以多叉树节点没有「唯一」的中序遍历位置
### 递归解法
- 遍历 -> 回溯
- 分而治之 -> 动态规划

- 应用：
[104. 二叉树最大深度](https://leetcode.cn/problems/maximum-depth-of-binary-tree/) 
    - 遍历
    ```python
    class Solution:
        res = 0
        depth = 0
        def maxdepth(root):
            traverse(root)
            return res
        def traverse(root):
            if not root: return
            global res,depth
            depth+=1
            if not root.left and not root.right:
                res = max(depth,res)
            traverse(root.left)
            traverse(root.right)
            depth-=1
    ```
    - 分而治之 -> 通过子树深度推导
    ```python
    def maxDepth(root):
        if not root: return 0

        leftm = maxDepth(root.left)
        rightm = maxDepth(root.right)
        # 整棵树的最大深度等于左右子树的最大深度取最大值，
        # 然后再加上根节点自己
        res = max(leftm,rightm)

        return res
    
    ```
### 二叉树的题目通用思考过程

1、是否可以通过遍历一遍二叉树得到答案？-> traverse 函数配合外部变量来实现。

2、是否可以定义一个递归函数，通过子问题（子树）的答案推导出原问题的答案？-> 写出这个递归函数的定义，并充分利用这个函数的返回值。

3、明白二叉树的每一个节点需要做什么，需要在什么时候（前中后序）做。

### 前中后后序位置的特殊之处

- 中序位置主要用在 BST 场景中，你完全可以把 BST 的中序遍历认为是遍历有序数组。

- 前序位置本身其实没有什么特别的性质，代码执行是自顶向下的，而后序位置的代码执行是自底向上的：
    #### 意味着前序位置的代码只能从函数参数中获取父节点传递来的数据，而后序位置的代码不仅可以获取参数数据，还可以获取到子树通过函数返回值传递回来的数据 ####

- 应用：
    - 根节点看作第1层，打印出每一个节点所在的层数 -> 需要在进入节点时进行迭代 -> 前序位置print 
    - 打印左右子树节点数 -> 需要子树的结果 -> 后序位置print 

### 二叉树问题的扩展：

- 动态规划算法属于分解问题的思路，它的关注点在整棵「子树」。
- 回溯算法属于遍历的思路，它的关注点在节点间的「树枝」。
- DFS 算法属于遍历的思路，它的关注点在单个「节点」。