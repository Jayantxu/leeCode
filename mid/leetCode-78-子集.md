*原题链接：👉：[子集](https://leetcode-cn.com/problems/subsets/description/)*

题目描述:

1. 给你一个整数数组 nums ，数组中的元素 互不相同 。返回该数组所有可能的子集（幂集）。
2.解集 不能 包含重复的子集。你可以按 任意顺序 返回解集。

示例：
```
输入：nums = [1,2,3]
输出：[[],[1],[2],[1,2],[3],[1,3],[2,3],[1,2,3]]
```
```
输入：nums = [0]
输出：[[],[0]]
```

思路：
> 一个全排列问题，之前的题目有遇到过类似的。回溯

```
var subsets = function(nums) {
    let nLen = nums.length;
    let resArr = [];
    const numfs = (idx, arr) => {
        resArr.push(arr.slice());
        for(let i = idx; i < nLen; i++) {
            arr.push(nums[i]);
            numfs(i + 1, arr);
            arr.pop();
        }
    }
    numfs(0, []);
    return resArr;
};
```