
*原题链接：👉：[整数转罗马数字](https://leetcode-cn.com/problems/integer-to-roman/)*

题目描述:
1. 罗马数字包含以下七种字符： I， V， X， L，C，D 和 M。
```
字符          数值
I             1
V             5
X             10
L             50
C             100
D             500
M             1000
```

例如， 罗马数字 2 写做 II ，即为两个并列的 1。12 写做 XII ，即为 X + II 。 27 写做  XXVII, 即为 XX + V + II 。

通常情况下，罗马数字中小的数字在大的数字的右边。但也存在特例，例如 4 不写做 IIII，而是 IV。数字 1 在数字 5 的左边，所表示的数等于大数 5 减小数 1 得到的数值 4 。同样地，数字 9 表示为 IX。这个特殊的规则只适用于以下六种情况：
- I 可以放在 V (5) 和 X (10) 的左边，来表示 4 和 9。
- X 可以放在 L (50) 和 C (100) 的左边，来表示 40 和 90。 
- C 可以放在 D (500) 和 M (1000) 的左边，来表示 400 和 900。

示例：
```
输入: 3
输出: "III"
```

```
输入: 4
输出: "IV"
```
思路：

> 依稀记得，很久以前做过是用了啥巧妙的方法还是数据结构来着...
> 最大是不超过3999，再这个区间内，把所有的可能整数都列出来呗。



```
var intToRoman = function(num) {
                //   0   1    2      3      4    5    6      7      8      9
    const roman1 = [
                        ['', 'I', 'II', 'III', 'IV', 'V', 'VI', 'VII', 'VIII', 'IX'],
                        ['', 'X', 'XX', 'XXX', 'XL', 'L', 'LX', 'LXX', 'LXXX', 'XC'],
                        ["", "C", "CC", "CCC", "CD", "D", "DC", "DCC", "DCCC", "CM"],
                        ["", "M", "MM", "MMM"]
                    ];
    let numStr = num.toString();
    let iterLen = 0
    let changeArr = [];
    for(let i = numStr.length - 1; i >= 0; i--) {
        let idx = numStr.charAt(i);
        changeArr.unshift(roman1[iterLen][idx]);
        iterLen += 1;   
    }
    return changeArr.join('');
};

```


