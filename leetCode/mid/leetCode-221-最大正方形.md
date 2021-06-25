*原题链接：👉：[最大正方形](https://leetcode-cn.com/problems/maximal-square/description/)*

题目描述:

1. 在一个由 '0' 和 '1' 组成的二维矩阵内，找到只包含 '1' 的最大正方形，并返回其面积。

示例：


![示例1](https://assets.leetcode.com/uploads/2020/11/26/max1grid.jpg)
```
输入：matrix = [["1","0","1","0","0"],["1","0","1","1","1"],["1","1","1","1","1"],["1","0","0","1","0"]]
输出：4
```

![示例2](https://assets.leetcode.com/uploads/2020/11/26/max2grid.jpg)
```
输入：matrix = [["0","1"],["1","0"]]
输出：1
```

思路：

- 题解

动态规划，最大正方形面积就意味着会有最大的边长，因此我们计算出`1`可以组成的最大边长即可。

```
[
    [1,  1,  0]
    [0,  1,  1],
    [1,  1,  1]
]
```

我们使用`dp[i][j]`表示以`i,j`为右下角的正方形，计算以此为右下角的正方形的最大边长。那么它的最大边长即可以转移为其他三个组成的坐标中最小的边长组成。

```
dp[i][j] = Math.min( dp[i][j-1], dp[i-1][j-1], dp[i-1][j] ) + 1
```

以上面的的示例二维数组来说，即计算后:

```
[
    [1,  1,  0]
    [0,  1,  1],
    [1,  1,  2]
]
```

**code：**

```
var maximalSquare = function(matrix) {
    // 做大的正方形边数
    let maxArea = 0;
    // 如果有以下不符合的情况
    if(!matrix || !matrix.length || !matrix[0].length) return maxArea;
    // 记录dp
    const dp = new Array(matrix.length).fill(0).map(item => new Array(matrix[0].length).fill(0));
    for(let i = 0; i < matrix.length; i++) {
        for(let j = 0; j < matrix[0].length; j++) {
            // '1'的情况下，才能开始作为记录
            if(matrix[i][j] === '1') {
                // 如果是起始位置，那么肯定为1的
                if(i === 0 || j === 0) {
                    dp[i][j] = 1;
                } else {
                    // 转移方程，以自己为右下角的情况下，统计这么一个正方形最大边
                    dp[i][j] = Math.min(dp[i - 1][j], dp[i][j - 1], dp[i - 1][j - 1]) + 1;
                }
                maxArea = Math.max(maxArea, dp[i][j]);
            }
        }
    }
    // 面积 = 边长 * 边长
    maxArea = maxArea * maxArea;
    return maxArea;
};
```