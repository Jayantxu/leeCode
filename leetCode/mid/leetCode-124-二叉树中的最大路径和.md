*原题链接：👉：[二叉树中的最大路径和](https://leetcode-cn.com/problems/binary-tree-maximum-path-sum/description/)*

题目描述:

路径 被定义为一条从树中任意节点出发，沿父节点-子节点连接，达到任意节点的序列。同一个节点在一条路径序列中 至多出现一次 。该路径 至少包含一个 节点，且不一定经过根节点。

路径和 是路径中各节点值的总和。

给你一个二叉树的根节点 root ，返回其 最大路径和 。

注意:
你可以假设树中没有重复的元素。

示例：

![二叉树中的最大路径和](https://assets.leetcode.com/uploads/2020/10/13/exx1.jpg)
```
输入：root = [1,2,3]
输出：6
解释：最优路径是 2 -> 1 -> 3 ，路径和为 2 + 1 + 3 = 6
```

![二叉树中的最大路径和2](https://assets.leetcode.com/uploads/2020/10/13/exx2.jpg)

```
输入：root = [-10,9,20,null,null,15,7]
输出：42
解释：最优路径是 15 -> 20 -> 7 ，路径和为 15 + 20 + 7 = 42
```
思路：

- 题解，递归，每一个小树的和可以算为左子树的路径和 + 右子树的路径和，因此递归算起来，再求记录最大值即可。

```
var maxPathSum = function(root) {
    let recordMaxVal = -Infinity;
    const treeDfs = (ROOT) => {
        if(!ROOT) return 0;
        let leftMaxVal = treeDfs(ROOT.left);
        let rightMaxVal = treeDfs(ROOT.right);
        let nowRootVal = ROOT.val + leftMaxVal + rightMaxVal;
        // ①
        recordMaxVal = Math.max(recordMaxVal, nowRootVal);
        // ②
        const recordRootOutMax = ROOT.val + Math.max(leftMaxVal, rightMaxVal);
        // ③
        return recordRootOutMax < 0 ? 0 : recordRootOutMax;
    }
    treeDfs(root);
    return recordMaxVal;
};
```

题上有两个注释，
①：因为当前计算的这个节点可能是最大路径上的根节点，因此它可以作为最大的值被用来计算。

②：如果当前这个`ROOT`节点不是最大路径的根节点，那么左右两个节点也只能是路径的一个分支，因此暴露出去的节点值，只能从中二选一，因此`Math.max(leftMaxVal, rightMaxVal)`。

③：如果当前节点暴露的值`小于0`的话，那么外卖的值加它的话，计算出来会更小，因此直接计算为0。