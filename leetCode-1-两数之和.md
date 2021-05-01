---
title: leetCode.1.两数之和
date: 2021-04-17 09:21:50
tags:
    - 算法
---

*原题链接：👉：[两数之和](https://leetcode-cn.com/problems/two-sum/)*

题目描述：
1. 给定一个整数数组 nums 和一个整数目标值 target，请你在该数组中找出 和为目标值 的那 两个 整数，并返回它们的数组下标。
2. 你可以假设每种输入只会对应一个答案。但是，数组中同一个元素在答案里不能重复出现。
3. 你可以按任意顺序返回答案

示例：
```
输入：nums = [2,7,11,15], target = 9
输出：[0,1]
解释：因为 nums[0] + nums[1] == 9 ，返回 [0, 1] 。
```

```
输入：nums = [3,2,4], target = 6
输出：[1,2]
```
<!--more-->


第一感觉，用映射
> 使用map的特殊结构，我们将遍历数组，每一个值作为map的key，索引作为value，当我们寻找相加等于目标数的时候，即可以得到索引

```
var twoSum = function(nums, target) {
    let idxMap = new Map();
    let startIdx = '', count = 0;
    for(let iTem of nums) {
        let diff = target - iTem;
        if(idxMap.has(diff)) {
            startIdx = idxMap.get(diff);
        } else {
            idxMap.set(iTem, count);
            count++;
        }
        if(startIdx !== '') {
            return [startIdx, count]
        }
    }
};
```

执行结果：`通过`。但是用时和内存有点惨。