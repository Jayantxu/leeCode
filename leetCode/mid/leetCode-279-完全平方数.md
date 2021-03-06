*原题链接：👉：[完全平方数](https://leetcode-cn.com/problems/perfect-squares/description/)*

题目描述:

给定正整数 n，找到若干个完全平方数（比如 1, 4, 9, 16, ...）使得它们的和等于 n。你需要让组成和的完全平方数的个数最少。

给你一个整数 n ，返回和为 n 的完全平方数的 最少数量 。

完全平方数 是一个整数，其值等于另一个整数的平方；换句话说，其值等于一个整数自乘的积。例如，1、4、9 和 16 都是完全平方数，而 3 和 11 不是。

示例：

```
输入：n = 12
输出：3 
解释：12 = 4 + 4 + 4
输入：matrix = [[1,4,7,11,15],[2,5,8,12,19],[3,6,9,16,22],[10,13,14,17,24],[18,21,23,26,30]], target = 5
输出：true
```

```
输入：n = 13
输出：2
解释：13 = 4 + 9
```

思路：


> 一开始想得到应该是动态规划的思路，但是不知道怎么转移..

- 题解：

```
Math.min(dp[i], dp[i - j * j] + 1)
```

**code**

```
var numSquares = function(n) {
    let dp = new Array(n + 1).fill(0);
    for(let i = 1; i < n + 1; i++) {
        // dp用于存储每一位数的平方数可能，最坏的情况即都是 1 + 1+ 1+ 1... ，因此数量就是i
        dp[i] = i;
        for(let j = 1; i - j * j >= 0; j++ ) {
            dp[i] = Math.min(dp[i], dp[i - j * j] + 1);
        }
    }
    return dp[n];
};
```