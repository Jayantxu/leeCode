*原题链接：👉：[最短无序连续子数组](https://leetcode-cn.com/problems/shortest-unsorted-continuous-subarray/description/)*

题目描述:

1. 给你一个整数数组 nums ，你需要找出一个 连续子数组 ，如果对这个子数组进行升序排序，那么整个数组都会变为升序排序。

请你找出符合题意的 最短 子数组，并输出它的长度。

示例 1：

```
输入：nums = [2,6,4,8,10,9,15]
输出：5
解释：你只需要对 [6, 4, 8, 10, 9] 进行升序排序，那么整个表都会变为升序排序。
```

```
输入：nums = [1,2,3,4]
输出：0
```

思路：

> 找出升序的片段，那么意味着除了这个片段，其他都已经是升序的，那么就是找出这个无序片段的开头及结尾的索引位置，因为是返回长度。

> 那么复制一个数组，排序，然后遍历，找不同即可。

```
var findUnsortedSubarray = function(nums) {
    let i = '',
        j = '';
    // 排序，然后遍历两个数组，找不出一样的下标位置。
    let newArr = nums.slice().sort((a, b) => a - b);
    for(let idx = 0; idx < nums.length; idx++) {
        if(nums[idx] !== newArr[idx] && i === '') {
            i = idx;
        }
        if(nums[idx] !== newArr[idx]) {
            j = Math.max(idx, j);
        }
    }
    if(i === '') {
        return 0;
    }
    return j - i + 1;
};
```