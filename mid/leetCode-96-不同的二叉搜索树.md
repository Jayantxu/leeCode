*原题链接：👉：[不同的二叉搜索树](https://leetcode-cn.com/problems/unique-binary-search-trees/description/)*

题目描述:

1. 给你一个整数 n ，求恰由 n 个节点组成且节点值从 1 到 n 互不相同的 二叉搜索树 有多少种？返回满足题意的二叉搜索树的种数。

![不同的二叉搜索树](https://assets.leetcode.com/uploads/2021/01/18/uniquebstn3.jpg)

示例：
```
输入：n = 3
输出：5
```

思路：
> 先做了`98`题，再回头看这题，二叉搜索树是一个递增的状态，而且题目给了，`n`表示从`0~n`，的这么一个序列状态，因此我们逐步遍历，，加入遍历到`i`，那么`0~i-1`这个区间的自己去组成左子树，`i+1, n`这个区间的就是右子树，然后开始再构建。

> 动态规划。

- 题解：

[题解思路](https://leetcode-cn.com/problems/unique-binary-search-trees/solution/er-cha-sou-suo-shu-fu-xi-li-zi-jie-shi-si-lu-by-xi/)


