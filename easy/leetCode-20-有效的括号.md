
*原题链接：👉：[有效的括号](https://leetcode-cn.com/problems/valid-parentheses/)*

题目描述:

1. 给定一个只包括 '('，')'，'{'，'}'，'['，']' 的字符串 s ，判断字符串是否有效。

2. 有效字符串需满足：
    - 左括号必须用相同类型的右括号闭合。
    - 左括号必须以正确的顺序闭合。


示例：
```
输入：s = "()"
输出：true
```

```
输入：s = "()[]{}"
输出：true
```

思路：
> 入栈出栈，这个之前还遇到过`*`号匹配括号的，难度++版本。
> 思路就是，遇到`{`、`(`、`[`则入栈，遇到`)`、`]`、`}`则出栈，最终判断栈是否为空，当然用数组模拟栈即可。❗并且需要注意，可以提前`return false`，什么意思呢？在入栈的时候，如果判断了是不匹配的组合，肯定是不闭合的，例如`{`，然后再入栈`]`，组成了`{]`，那么肯定是不闭合。

```
var isValid = function(s) {
    let strArr = s.split('');
    let resArr = [];
    let mapRelate = {
        '}': '{',
        ')': '(',
        ']': '['
    }
    for(let item of strArr) {
        switch(item) {
            case '{':
            case '(':
            case '[':
                resArr.push(item);
                break;
            case '}':
            case ')':
            case ']':
                let resLen = resArr.length - 1;
                let endStr = resArr[resLen];
                if(mapRelate[item] !== endStr) {
                    return false;
                } else {
                    resArr.pop();
                }
                break;
        }
    }
    if(resArr.length === 0) return true;
    return false;
};
```