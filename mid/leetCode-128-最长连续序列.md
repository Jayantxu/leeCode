*原题链接：👉：[最长连续序列](https://leetcode-cn.com/problems/longest-consecutive-sequence/description/)*

题目描述:

给定一个未排序的整数数组 nums ，找出数字连续的最长序列（不要求序列元素在原数组中连续）的长度。

进阶：你可以设计并实现时间复杂度为 O(n) 的解决方案吗？

示例：

```
输入：nums = [100,4,200,1,3,2]
输出：4
解释：最长数字连续序列是 [1, 2, 3, 4]。它的长度为 4。
```
```
输入：nums = [0,3,7,2,5,8,4,6,0,1]
输出：9
```

思路：

> 一开始想的是用一个对象记录值，开始遍历，遍历到任何一个，都判断一下是否`+1`之后，对象中是否还存在此连续的值，然后统计这么一个`+1`的情况

看了一下题解，思路更清晰直接，学习了。试着写一下。

```
var longestConsecutive = function(nums) {
    if(!nums.length) return 0;
    let sortSetArr = [...new Set(nums)].sort((a, b) => a - b);
    let continueNum = 1;
    let idxSortNum = 1;
    for(let i = 0; i < sortSetArr.length - 1; i++) {
        if(sortSetArr[i] + 1 === sortSetArr[i + 1]) {
            idxSortNum += 1;
            if(i === sortSetArr.length - 2) {
                // 为什么在此计算，因为：[0,3,7,2,5,8,4,6,0,1]没算最后一个8
                continueNum = Math.max(continueNum, idxSortNum);
            }
        } else {
            continueNum = Math.max(continueNum, idxSortNum);
            idxSortNum = 1;
        }
    }
    return continueNum;
};
```

1. 排序、去重
2. 依次遍历，如果下一位等于当前值+1，那么就是有序+1