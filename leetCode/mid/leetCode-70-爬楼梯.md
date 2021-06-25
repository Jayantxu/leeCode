*原题链接：👉：[爬楼梯](https://leetcode-cn.com/problems/climbing-stairs/description/)*

题目描述:

1. 假设你正在爬楼梯。需要 n 阶你才能到达楼顶。
2. 每次你可以爬 1 或 2 个台阶。你有多少种不同的方法可以爬到楼顶呢？
注意：给定 n 是一个正整数。

示例：
```
输入： 2
输出： 2
解释： 有两种方法可以爬到楼顶。
1.  1 阶 + 1 阶
2.  2 阶
```

```
输入： 3
输出： 3
解释： 有三种方法可以爬到楼顶。
1.  1 阶 + 1 阶 + 1 阶
2.  1 阶 + 2 阶
3.  2 阶 + 1 阶
```
	
思路：

> 和那个不同路径似的，第`n`级台阶的到达方式数量，取决与`n-1`以及`n-2`级台阶，即`f(n) = f(n-1) + f(n-2)`。

然后洋洋洒洒的写了这个答案。

```
var climbStairs = function(n) {
    const iter = (level) => {
        if(level === 1) {
            return 1;
        }
        if(level === 2) {
            return 2;
        }
        return iter(level - 1) + iter(level - 2);
    }
    return iter(n);
};
```

结果超时了...😂



看来还是存在着优化的方式的，想了想，无非是每一级台阶均对应着一个数量，这个数量应该缓存下来，如果遇到可以直接取这个值。

```
			   __
			__| 5
		 __| 4
 	  __| 3
   __| 2
__| 1
0
```

什么意思呢？`5`级台阶需要由`4`级`3`级来凑，而其中`4级`有会需要`3`级的数量，可以将需要利用到的数，进行一个缓存。

```
var climbStairs = function(n) {
    const stairMap = new Map();
    const iter = (level) => {
        if(level === 1) {
            return 1;
        }
        if(level === 2) {
            return 2;
        }
        if(stairMap.has(level)) {
            return stairMap.get(level);
        }
        let last1 = iter(level - 1);
        if(!stairMap.has(level - 1)) {
            stairMap.set(level - 1, last1);
        }
        let last2 = iter(level - 2);
        if(!stairMap.has(level - 2)) {
            stairMap.set(level - 2, last2);
        }
        return last1 + last2;
    }
    return iter(n);
};
```

✔get


后来想了想，其实也不止可以从后向前，从前向后也是完全可以的。而且可以用更少的空间。

```
var climbStairs = function(n) {
    let stairArr = [1, 1, 2];
    if(n < 3) {
        return stairArr[n];
        
    }
    for(let i = 3; i <= n; i++) {
        let num = stairArr[i - 2] + stairArr[i - 1];
        stairArr.push(num);
    }
    return stairArr.pop();
};
```