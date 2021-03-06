
*原题链接：👉：[下一个排列](https://leetcode-cn.com/problems/next-permutation/description/)*

题目描述:

1. 实现获取 下一个排列 的函数，算法需要将给定数字序列重新排列成字典序中下一个更大的排列。
2. 如果不存在下一个更大的排列，则将数字重新排列成最小的排列（即升序排列）。
3. 必须 原地 修改，只允许使用额外常数空间。

示例：
```
输入：nums = [1,2,3]
输出：[1,3,2]
```

```
输入：nums = [3,2,1]
输出：[1,2,3]
```

```
输入：nums = [1,1,5]
输出：[1,5,1]
```

```
输入：nums = [1]
输出：[1]
```

思路：
> 原地排序，不能使用额外的更多空间..
> 再草稿纸上画了一下，似乎找到了什么规律，我们一起看一下：
```
[1, 0, 2] => [1, 2, 0]; // 替换了*2*和*0*
[8, 9] => [9, 8]; // 替换了*8*和*9*
[8] => [8]; // 不动
[1, 2] => [2, 1] // 替换了*1*和*2*
[3, 9, 0] => [9, 3, 0] // 替换了*9*和*3*

🤦‍♂️我以为自己悟到了规律，从后向前找，交换两个靠近的升序元素就好了，然后我发现了

[1, 7, 2] => [7, 1, 2]; // 替换了*1*和*7*

这不对呀，不是应该是[2, 1, 7]更符合吗..
然后就陷入了沉思...

```

- 题解

*还是大佬优秀啊😭*

> 首先从后向前查找第一个顺序对`(i, i+1)`，满足`a[i] < a[i+1]`，这样*较小数*，即`a[i]`，此时`[i+1, n)`必然是下降序列；
> 如果找到了顺序对，那么区间`[i+1, n)`中从后向前查找第一个元素`j`满足`a[i] < a[j]`，这样*较大数*即为a[j]；
> 交换`a[i]`与`a[j]`，此时区间`[i+1, n)`比如降序，我们使用双指针反转区间`[i+1, n)`为升序。

具体例子：

[4, 5, 2, 6, 3, 1]

- 找到符合条件的一对*较小数*与*较大数*组合，2与3，交换两者，排列变为`[4, 5, 3, 6, 2, 1]`，此时重新排列*较小数*右边的序列，序列变为`[4, 5, 3, 1, 2, 6]`

```
var nextPermutation = function(nums) {
    let idx = nums.length - 2 ;
    while(idx >= 0 && nums[idx] >= nums[idx + 1]) {
        idx--;
    }
    if(idx >= 0) {
        let jdx = nums.length - 1;
        while(jdx >= 0 && nums[idx] >= nums[jdx]) {
            jdx--;
        }
        [nums[idx], nums[jdx]] = [nums[jdx], nums[idx]];
    }
    idx = idx + 1;
    let jdx = nums.length - 1;
    while(idx < jdx) {
        [nums[idx], nums[jdx]] = [nums[jdx], nums[idx]];
        idx++;
        jdx--;
    }
};
```