*原题链接：👉：[只出现一次的数字](https://leetcode-cn.com/problems/single-number/description/)*

题目描述:

1. 给定一个非空整数数组，除了某个元素只出现一次以外，其余每个元素均出现两次。找出那个只出现了一次的元素。

说明：
你的算法应该具有线性时间复杂度。 你可以不使用额外空间来实现吗？

示例：

```
输入: [2,2,1]
输出: 1
```
```
输入: [4,1,2,1,2]
输出: 4
```

思路：
> 我们先做出这个

```
var singleNumber = function(nums) {
    let numMap = new Map();
    numMap.set(1, []);
    numMap.set(2, []);
    for(let i = 0; i < nums.length; i++) {
        let nowVal = nums[i];
        let firstIdx = numMap.get(1).indexOf(nowVal);
        if(firstIdx !== -1) {
            numMap.get(1).splice(firstIdx, 1);
            numMap.get(2).push(nowVal);
        } else {
            numMap.get(1).push(nowVal);
        }
    }
    return numMap.get(1)[0];
};
```

😢但是这个用了额外的空间，不用额外的空间咋记录

- 题解

我看到两个常量空间的答案：
1. 排序后验证
2. 异或

没想到呀，这思路，这角度🥇

1. 排序
排序的很容易理解。

```
var singleNumber = function(nums) {
    nums.sort((a, b) => a - b);
    for(let i = 0; i < nums.length; i++) {
        if(nums[i] !== nums[i + 1]) {
            return nums[i];
        } else {
            if(i === nums.length - 1) {
                return nums[i];
            }
            i += 1;
        }
    }
};
```


2. 异或

异或运算有三个性质：
- 任何数和`0`做异或运算，结果仍然是原来的数，即`a ⊕ 0 = a`；
- 任何数和自身做异或运算，结果是`0`，即`a ⊕ a = 0`；
- 异或运算满足交换律和结合律，即`a ⊕ b ⊕ a = b ⊕ a ⊕ a = b ⊕ ( a ⊕ a ) = b ⊕ 0 = b`；

所以上面的数组元素全部进行异或运算，即：
```
(a ⊕ a) ⊕ ( b ⊕ b) ⊕ (c ⊕ c) ⊕ ... ⊕ z
```
简化之后：

```
0 ⊕ 0 ⊕ 0 ⊕ 0 ⊕ N = N;
```

于是就得出这个唯一出现的这个数。

```
var singleNumber = function(nums) {
    let resNum = 0;
    for(let iTem of nums) {
        resNum ^= iTem;
    }
    return resNum;
};
```

异或.想不到呀。💥💥💥