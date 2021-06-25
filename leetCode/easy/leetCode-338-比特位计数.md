*原题链接：👉：[比特位计数](https://leetcode-cn.com/problems/counting-bits/description/)*

题目描述:

给定一个非负整数 num。对于 0 ≤ i ≤ num 范围中的每个数字 i ，计算其二进制数中的 1 的数目并将它们作为数组返回。


示例：
```
输入: 2
输出: [0,1,1]
```

```
输入: 5
输出: [0,1,1,2,1,2]
```

思路：


- 题解

知识点：

```
n &= (n-1)
```
进行运算，可以将`n`的最后一个1变为0。变为0之前，操作次数就是`1`的数量。

```
var countBits = function(n) {
    // n &= (n-1)
    let bitArr = [];

    const countBit = (i) => {
        let num = 0;
        while(i > 0) {
            num += 1;
            i &= (i-1);
        }
        return num;
    }
    
    for(let i = 0; i <= n; i++) {
        bitArr[i] = countBit(i);
    }
    return bitArr;

};
```