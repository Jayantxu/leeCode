
*原题链接：👉：[搜索旋转排序数组](https://leetcode-cn.com/problems/search-in-rotated-sorted-array/description/)*

题目描述:

1. 整数数组 nums 按升序排列，数组中的值 互不相同 。
2. 在传递给函数之前，nums 在预先未知的某个下标 k（0 <= k < nums.length）上进行了 旋转，使数组变为 [nums[k], nums[k+1], ..., nums[n-1], nums[0], nums[1], ..., nums[k-1]]（下标 从 0 开始 计数）。例如， [0,1,2,4,5,6,7] 在下标 3 处经旋转后可能变为 [4,5,6,7,0,1,2] 。
3. 给你 旋转后 的数组 nums 和一个整数 target ，如果 nums 中存在这个目标值 target ，则返回它的下标，否则返回 -1 。

**进阶**：你可以设计一个时间复杂度为 O(log n) 的解决方案吗？

示例：
```
输入：nums = [4,5,6,7,0,1,2], target = 0
输出：4
```

```
输入：nums = [3,2,1]
输出：[1,2,3]
```

```
输入：nums = [4,5,6,7,0,1,2], target = 3
输出：-1
```

思路：
> 你品一下，我一度感觉这题目怎么这么长，会有很复杂，不存在返回`-1`，品品，这不就是`.indexOf`吗😂

```
var search = function(nums, target) {
    return nums.indexOf(target);
};
```

🤦‍♂️

好吧，`有序`，很容易联想到二分查找，二分查找试试做吧。

```
let numsLen = nums.length - 1;
    if(numsLen === 0) {
        if(nums[0] === target) {
            return 0;
        }
        return -1;
    }
    let L = 0,
        R = numsLen;
    // [4,5,6,7,8,1,2,3]  6
    while(L <= R) {
        let midx = Math.ceil((R + L) / 2);
        if(nums[midx] === target) return midx;
        if(nums[L] >= nums[midx]) { // 非有序的情况，例如 [7,8,1,2,3,4,5,6]; L = 0; R = 7; midx = 4;
            if(nums[midx] <= target && nums[R] >= target) {
                L = midx + 1;
            } else {
                R = midx - 1;
            }
        } else {
            if(nums[L] <= target && nums[midx] >= target) {
                R = midx - 1;
            } else {
                L = midx + 1;
            }
        }
    }
    return -1;
```

⚠其中二分查找其实需要很注意`<=`，`>=`这些边界取值。