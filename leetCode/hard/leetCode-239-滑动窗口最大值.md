*原题链接：👉：[滑动窗口最大值](https://leetcode-cn.com/problems/sliding-window-maximum/description/)*

题目描述:

1. 给你一个整数数组 nums，有一个大小为 k 的滑动窗口从数组的最左侧移动到数组的最右侧。你只可以看到在滑动窗口内的 k 个数字。滑动窗口每次只向右移动一位。

2. 返回滑动窗口中的最大值。


示例：
```
输入：nums = [1,3,-1,-3,5,3,6,7], k = 3
输出：[3,3,5,5,6,7]
解释：
滑动窗口的位置                最大值
---------------               -----
[1  3  -1] -3  5  3  6  7       3
 1 [3  -1  -3] 5  3  6  7       3
 1  3 [-1  -3  5] 3  6  7       5
 1  3  -1 [-3  5  3] 6  7       5
 1  3  -1  -3 [5  3  6] 7       6
 1  3  -1  -3  5 [3  6  7]      7
```

```
输入：nums = [1], k = 1
输出：[1]
```

思路：
> 查看了题解思路写下的答案

我们维护一个单调递减的栈，当窗口长度符合要求开始，就开始可以出栈了，什么意思呢？

以示例`nums = [1,3,-1,-3,5,3,6,7], k = 3`为例子。

他的过程是这样的：

- 一
```

nums：
idx:   0, 1,  2,  3, 4, 5, 6, 7
nums: [1, 3, -1, -3, 5, 3, 6, 7]
       L
       R
kValArr: [0]

kValArr[0] = 0 = L 不 shift

R + 1 = 1 !> k(3)
```

- 二

```

nums：
idx:   0, 1,  2,  3, 4, 5, 6, 7
nums: [1, 3, -1, -3, 5, 3, 6, 7]
       L
          R

nums[R] > nums[0], 0出栈， R的索引入栈
kValArr: [1]

kValArr[0] = 0 = L 不 shift

R + 1 = 2 !> k(3)
```

- 三

```

nums：
idx:   0, 1,  2,  3, 4, 5, 6, 7
nums: [1, 3, -1, -3, 5, 3, 6, 7]
       L
              R
nums[R] < nums[1]，R入栈
kValArr: [1, 2]

kValArr[0] = 0 = L 不 shift

R + 1 = 3 = k(3)

kValArr[0] = 1 => nums[1] = 3
resArr = [3]
L++

```

- 四


```

nums：
idx:   0, 1,  2,  3, 4, 5, 6, 7
nums: [1, 3, -1, -3, 5, 3, 6, 7]
          L
                  R
nums[R] < nums[2]，R入栈
kValArr: [1, 2, 3]

kValArr[0] = 1 = L 不 shift

R + 1 = 3 = k(3)

kValArr[0] = 1 => nums[1] = 3
resArr = [3, 3]
L++

```

- 五
```

nums：
idx:   0, 1,  2,  3, 4, 5, 6, 7
nums: [1, 3, -1, -3, 5, 3, 6, 7]
              L
                     R
nums[R] > nums[3]，接连出栈，R入栈
kValArr: [4]

kValArr[0] = 4 > L 不 shift

R + 1 = 3 = k(3)

kValArr[0] = 4 => nums[4] = 5
resArr = [3, 3, 5]
L++

```

...依次类推，debugger一下更清晰


**code**

```
var maxSlidingWindow = function(nums, k) {
    let resArr = [];
    let L = 0,
        R = 0;
    let kValArr = [];
    // R开始遍历，R到右边界的时候就该停了
    for(; R < nums.length; R++) {
        
        // 形成一个单调递减栈
        while(kValArr.length && nums[kValArr[kValArr.length - 1]] <= nums[R]) {
            kValArr.pop();
        }
        // 保存索引下标
        kValArr.push(R);

        // 随着L的增加，如果队头的元素已经不在窗口里，那么它就不应该进入下一轮
        while(kValArr[0] < L) {
            kValArr.shift();
        }
        // 当R + 1 大于等于k 时，开始可以计算最大值了，按示例来说，k是三，索引，R+1是索引，大于等于三的时候就行程窗口了
        if(R + 1 >= k) {
            resArr.push(nums[kValArr[0]]);
            L++;
        }
    }
    return resArr;
};
```
