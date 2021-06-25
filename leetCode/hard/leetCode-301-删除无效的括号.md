*原题链接：👉：[删除无效的括号](https://leetcode-cn.com/problems/remove-invalid-parentheses/description/)*

题目描述:

1. 给你一个由若干括号和字母组成的字符串 s ，删除最小数量的无效括号，使得输入的字符串有效。

2. 返回所有可能的结果。答案可以按 任意顺序 返回。


示例：

```
输入：s = "()())()"
输出：["(())()","()()()"]
```

```
输入：s = "(a)())()"
输出：["(a())()","(a)()()"]
```

```
输入：s = ")("
输出：[""]
```

思路：

- 题解

> 深度遍历
> 具体的情况步骤是这样的：

1. 遍历鉴别出需要删除的括号类型及数量，左括号几个呢？右括号几个呢，用`lRemove`，`rRemove`表示。
2. 深度遍历的时候根据不同的可能依次前进：
  - 如果遍历到`(`左括号，需要删除的`lRemove`仍有剩余，那么可以此下标的`(`可以当作无效括号删除处理。
    ```
    if(idxS === '(' && lRemove > 0) {
        dfsValid(idx + 1, hasL, hasR, lRemove - 1, rRemove, nowStr);
    }
    ```
  - 右括号同样
    ```
    if(idxS === ')' && rRemove > 0) {
        dfsValid(idx + 1, hasL, hasR, lRemove, rRemove - 1, nowStr);
    }
    ```
  - 如果是非括号，那么直接加入即可
    ```
    if(idxS !== ')' && idxS !== '(') {
        // 非左右括号，是其他字符的情况
        dfsValid(idx + 1, hasL, hasR, lRemove, rRemove, nowStr + idxS);
    } 
    ```
  - 如果是左括号`(`并且已经没有可移除的`lRemove`了，那么还是加入
    ```
    if (idxS === '(') {
        dfsValid(idx + 1, hasL + 1, hasR, lRemove, rRemove, nowStr + idxS);
    }
    ```
  - 如果是右括号，那么要判断当前可以容纳的`hasL`数量是否大于`hasR`，否则会出现`()())()`这种情况的时候，`id = 4`的时候，会被加入，而最后一个`)`无效，无法有效组合造成`()())(`的情况，如果以后想不起，debugger一遍👨‍🦳
    ```
    if (idxS === ')' && hasL > hasR) {
        dfsValid(idx + 1, hasL, hasR + 1, lRemove, rRemove,  nowStr + idxS);
    }
    ```
  - 最后，使用的是`Set`存储，因为需要去重。

  
**code**


```
var removeInvalidParentheses = function(s) {
    let len = s.length;
    let validSet = new Set();

    // 当前下标，已经遍历的左括号，已经遍历的右括号，需要删的左括号数， 需要删的右括号树，字符结果
    const dfsValid = (idx, hasL, hasR, lRemove, rRemove, nowStr) => {
        // 遍历到最后
        if(idx === len) {
            if(lRemove === 0 && rRemove === 0) {
                validSet.add(nowStr);
            }
            return ;
        }
        let idxS = s.charAt(idx);
        // 如果当前遍历到左括号，可删除左括号仍有余, 选择不遍历，跳过
        if(idxS === '(' && lRemove > 0) {
            dfsValid(idx + 1, hasL, hasR, lRemove - 1, rRemove, nowStr);
        }
        // 如果当前遍历到右括号，可删除右括号仍有余, 选择不遍历，跳过
        if(idxS === ')' && rRemove > 0) {
            dfsValid(idx + 1, hasL, hasR, lRemove, rRemove - 1, nowStr);
        }

        if(idxS !== ')' && idxS !== '(') {
            // 非左右括号，是其他字符的情况
            dfsValid(idx + 1, hasL, hasR, lRemove, rRemove, nowStr + idxS);
        } 
        if (idxS === '(') {
            dfsValid(idx + 1, hasL + 1, hasR, lRemove, rRemove, nowStr + idxS);
        }
        //  hasL > hasR, 这一判断非常重要，只有判断了(( 数量足够，允许再加 )的情况下，再可以
        // 不然会 ()())() 会出现 '()())(' 的 组合!!
        // if (idxS === ')') {
        if (idxS === ')' && hasL > hasR) {
            dfsValid(idx + 1, hasL, hasR + 1, lRemove, rRemove,  nowStr + idxS);
        }
        nowStr = nowStr.substr(0, nowStr.length - 1);
    }

    let l = 0,
        r = 0;
    // 遍历，用于求出多余的括号，左或者右
    for(let i = 0; i < len; i++) {
        // 如果是左括号，加起来
        if(s.charAt(i) === '(') {
            l += 1;
        } else if (s.charAt(i) === ')') {
            // 如果是右括号，并且左括号已经没了，则加1，因为是多余的
            if(l === 0) {
                r += 1;
            } else if (l > 0) {
                // 如果还有左括号，证明还可以抵消匹配掉
                l -= 1;
            }
        }
    }
    // 开始深度遍历
    dfsValid(0, 0, 0, l, r, '');
    return [...validSet];
};
```