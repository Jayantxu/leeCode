
*原题链接：👉：[罗马数字转整数](https://leetcode-cn.com/problems/roman-to-integer/)*

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
输入: "III"
输出: 3
```

```
输入: "IV"
输出: 4
```
思路：
- *看过题解*
> 从左向右遍历，匹配字符，特殊的情况题目已经说明了，`IV`、`IX`、`XL`、`XC`、`CD`、`CM`，如果符合两个字符，则优先匹配两个字符？ +两个字符对应的值，否则 +一个字符对应的值，所有就还需要一个对象存这些字符。

```
var romanToInt = function(s) {
    let strMap = new Map();
    strMap.set('I', 1);
    strMap.set('V', 5);
    strMap.set('X', 10);
    strMap.set('L', 50);
    strMap.set('C', 100);
    strMap.set('D', 500);
    strMap.set('M', 1000);
    strMap.set('IV', 4);
    strMap.set('IX', 9);
    strMap.set('XL', 40);
    strMap.set('XC', 90);
    strMap.set('CD', 400);
    strMap.set('CM', 900);

    let strLen = s.length;
    let num = 0;
    for(let i = strLen - 1; i >= 0; ) {
        if( strMap.has(s.slice(i - 1, i+ 1)) ) {
            num += strMap.get(s.slice(i - 1, i + 1));
            i -= 2;
        } else {
            num += strMap.get(s.slice(i, i + 1));
            i -= 1;
        }
    }
    return num;
};
```


