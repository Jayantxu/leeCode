*原题链接：👉：[二叉树的最大深度](https://leetcode-cn.com/problems/maximum-depth-of-binary-tree/description/)*

题目描述:

1. 给定一个二叉树，找出其最大深度。

2、 二叉树的深度为根节点到最远叶子节点的最长路径上的节点数。

说明: 叶子节点是指没有子节点的节点。

示例：
给定二叉树 [3,9,20,null,null,15,7]，

```
    3
   / \
  9  20
    /  \
   15   7
```
返回它的最大深度 3 。

思路：
> 感觉这题和上一题`102`一样呀，一个只是记录返回的数组，一个记录深度，差不多吧。

```
var maxDepth = function(root) {
    let ht = 0
    const dfs = (Root, tmpH) => {
        if(!Root) return ;
        ht = Math.max(ht, tmpH);
        dfs(Root.left, tmpH + 1);
        dfs(Root.right, tmpH + 1);
    }
    dfs(root, 1);
    return ht;
};
```