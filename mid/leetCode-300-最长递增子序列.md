*原题链接：👉：[最长递增子序列](https://leetcode-cn.com/problems/longest-increasing-subsequence/description/)*

题目描述:

给你一个整数数组 nums ，找到其中最长严格递增子序列的长度。

子序列是由数组派生而来的序列，删除（或不删除）数组中的元素而不改变其余元素的顺序。例如，[3,6,2,7] 是数组 [0,3,1,6,2,2,7] 的子序列。

思路：

> 一开始这个，我还想着构建什么树，什么堆的形式去解答，看来是我想太多了；动态规划，两次遍历，转移方程是`Math.max(dp[i], dp[j] + 1)`；

第二次遍历，计算索引`j`的位置处的值与`i`的值关系...

```
var lengthOfLIS = function(nums) {
    if(!nums.length) return 0;
    
    let dp = new Array(nums.length);
    
    dp[0] = 1;
    let recordMax = 1;
    for(let i = 1; i < nums.length; i++) {
        dp[i] = 1;
        for(let j = 0; j < i; j++) {
            if(nums[i] > nums[j]) {
                dp[i] = Math.max(dp[i], dp[j] + 1);
            }
        }
        recordMax = Math.max(recordMax, dp[i]);
    }
    return recordMax;
};
```