*原题链接：👉：[多数元素](https://leetcode-cn.com/problems/majority-element/description/)*

题目描述:

1. 给定一个大小为 n 的数组，找到其中的多数元素。多数元素是指在数组中出现次数 大于 ⌊ n/2 ⌋ 的元素。

2. 你可以假设数组是非空的，并且给定的数组总是存在多数元素。

示例：
```
输入：[3,2,3]
输出：3
```

```
输入：[2,2,1,1,1,2,2]
输出：2
```

尝试设计时间复杂度为 O(n)、空间复杂度为 O(1) 的算法解决此问题。

思路：
> 多数，`> n/2`，那么这些数组中的这个多数元素应该占到一半以上，排序之后在取中间那个就是该多数元素。

```
var majorityElement = function(nums) {
    nums.sort();
    return nums[Math.floor(nums.length / 2)];
};
```

但是`sort`使用了快排，时间复杂度不满足。

- 题解：

投票算法。地铁，老人，手机。😑


[知乎-如何理解摩尔投票算法？](https://www.zhihu.com/question/49973163)

```
var majorityElement = function(nums) {
    let count = 0,
        preVal = 0;
    for(let iTem of nums) {
        if(count === 0) {
            preVal = iTem;
        }
        if(iTem === preVal) {
            count += 1;
        } else {
            count -= 1;
        }
    }
    return preVal;
};
```