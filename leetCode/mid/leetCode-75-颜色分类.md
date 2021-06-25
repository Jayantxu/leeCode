*原题链接：👉：[颜色分类](https://leetcode-cn.com/problems/sort-colors/description/)*

题目描述:

1. 给定一个包含红色、白色和蓝色，一共 n 个元素的数组，原地对它们进行排序，使得相同颜色的元素相邻，并按照红色、白色、蓝色顺序排列。
2. 此题中，我们使用整数 0、 1 和 2 分别表示红色、白色和蓝色

示例：
```
输入：nums = [2,0,2,1,1,0]
输出：[0,0,1,1,2,2]
```

```
输入：nums = [2,0,1]
输出：[0,1,2]
```
	
思路：

> 遍历替换即可。

```
var sortColors = function(nums) {
    let nLen = nums.length;
    let flag = 0;
    while(nums[flag] === 0) {
        flag += 1;
    }
    let iStart = flag;
    for(let i = iStart; i < nLen; i++) {
        if(nums[i] === 0) {
            [nums[i], nums[flag]] = [nums[flag], nums[i]];
            flag += 1;
        }
        while(nums[flag] === 0) {
            flag += 1;
        }
    }
    iStart = flag;
    flag = nLen - 1;
    while(nums[flag] === 2) {
        flag -= 1;
    }
    for(let i = iStart; i < flag; i++) {
        if(nums[i] === 2) {
            [nums[i], nums[flag]] = [nums[flag], nums[i]];
            flag -= 1;
        }
        while(nums[flag] === 2) {
            flag -= 1;
        }
    }
    return nums;
};
```


还是还遇到难度加`plus`版，需要在一次遍历内，原地返回 完成。

想必还是双指针...

如果要双指针，那么逻辑应该是怎么样的呢？在草纸上画了一下。

```
一：

 i
 p0                p1
[1, 1, 0, 2, 0, 1, 2];


二：p0，p1先前进到非0， 非2上。

p0              p1
[1, 1, 0, 2, 0, 1, 2]

三： i开始扫描

p0     i        p1
[1, 1, 0, 2, 0, 1, 2]

交换i与p0

p0     i        p1
[0, 1, 1, 2, 0, 1, 2]


继续前进

    p0    i     p1
[0, 1, 1, 2, 0, 1, 2]

交换p1与i

    p0    i     p1
[0, 1, 1, 1, 0, 2, 2]


继续前进
			 p1
	p0	     i  
[0, 1, 1, 1, 0, 2, 2]

交换i与p0

[0, 0, 1, 1, 1, 2, 2]

```

其实关键点在于每一次`p0`、`p1`都要前进到非0，非2上。


```
var sortColors = function(nums) {
    let nLen = nums.length;
    let p0 = 0,
        p1 = nLen - 1;
    while(nums[p0] === 0) {
        p0 += 1;
    }
    while(nums[p1] === 2) {
        p1 -= 1;
    }
    let start = p0;
    // 注意点：i与p1的关系
    for(let i = start; i <= p1; i++) {
        while(i < p1 && nums[i] === 2) {
            [nums[i], nums[p1]] = [nums[p1], nums[i]];
            p1 -= 1;
        }
        if(nums[i] === 0) {
            [nums[i], nums[p0]] = [nums[p0], nums[i]];
            p0 += 1;
        }
    }
    return nums;
};
```


✔