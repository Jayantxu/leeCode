*原题链接：👉：[移动零](https://leetcode-cn.com/problems/move-zeroes/description/)*

题目描述:

给定一个数组 nums，编写一个函数将所有 0 移动到数组的末尾，同时保持非零元素的相对顺序。


示例：
```
输入: [0,1,0,3,12]
输出: [1,3,12,0,0]
```

思路：

> 有点类似冒泡的意思...，但是冒的是`0`，让`0`和非`0`的交换就好了。

```
var moveZeroes = function(nums) {
    let i = 0,
        j = 0;
    while(i < nums.length) {
        // 超过长度就不行了，注意
        while(nums[i] !== 0 && i < nums.length) {
            i++;
        }
        j = i;
        // 超过长度就不行了， 注意
        while(nums[j] === 0 && j < nums.length) {
            j += 1;
        }
        if(j >= nums.length) {
            break;
        }
        [nums[i], nums[j]] = [nums[j], nums[i]];
        i++;
        j++;
    }
    return nums;
};
```