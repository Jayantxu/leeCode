---
title: 最长回文子串
date: 2021-04-12
tags:
    - 算法
---


*原题链接：👉：[最长回文子串](https://leetcode-cn.com/problems/longest-palindromic-substring/)*

题目描述：给你一个字符串`s`，找到`s`的最长回文子串。

示例：
```
输入：s = "babad"
输出："bab"
解释："aba" 同样是符合题意的答案。
```

本篇提供了三种解法：

1. 暴力解法
2. 中心扩展
3. 动态规划

<!--more-->

#### 暴力解法

首先，题目是可以尝试使用暴力解的。
> 思路：暴力解的思路是容易想到的，我们采用遍历不断取出字符串`s`的子串，对取出的子串进行回文判断，怎么判断呢？我们可以讲取出的子串对半切，第一个元素与最后一个对比，第二个与倒数第二个对比...

1. 循环取出子串

```
const longestPalindrome = (s) => {
    let max = 0; // 记录最长的回文子串长度
    let maxStr = ''; // 记录最长的回文子串
    let sLen = s.length; // 字符串的长度
    for(let i = 0; i < sLen; i++ ) {
        for(let j = i+1; j <= sLen; j++) {
            let sonStr = s.slice(i, j);
            if(sonStr.length > max && isPalindrome(sonStr)) { 
                /**
                * isPalindrome方法用于判断是否回文子串，并且长度大于max的情况下执行
                * 有必要先对比长度，如果子串长度都不如最长回文，则压根没必要进入回文的判断方法🌟
                */
                 max = sonStr.length;
                 maxStr = sonStr;
            }
        }
    }
    return maxStr;
}

```

2. 判断是否回文

```
let isPalindrome = (sonStr) => {
    let enmuLen = sonStr.length; // 对半切 
    for(let i = 0; i < enmuLen / 2; i++) {
        if(sonStr.charAt(i) !== sonStr.charAt(enmuLen-1-i)) {
            return false;
        }
    }
    return true;
}
```

#### 中心扩展法

> 思路：我们只要遍历，每次取出一个中心，从中心起始前后寻找，对比字符情况。

1. 中心取值

```
let longestPalindrome = (str) => {
    if(str === '') {
        return '';
    }
    if(str.length === 2 && str.charAt(0) === str.charAt(1)) {
        return str;
    }
    let start = 0, end = 0; // 记录最长回文的开头位置，结束位置
    for(let i = 0; i < str.length; i++ ) {
        let len = centerExpand(str, i, i);
        if(len > end - start) {
            start = i - (len - 1) / 2; 
            end = i + len / 2; 
        }
    }
    return str.slice(start, end + 1);
}

```

2. 中心扩展法对比

```
let centerExpand = (str, left, right) => {
    let L = left, R = right;
    while( L >= 0 && R < str.length && str.charAt(L) === str.charAt(R) ) {
        L--;
        R++;
    }
    return R - L - 1;
}
```


#### 动态规划

1. 定义状态
    - dp[i,j]：字符串s从索引i到j的子串是否是回文串
        - true： s[i,j] 是回文串
        - false：s[i,j] 不是回文串
2. 转移方程
    - dp[i][j] = dp[i+1][j-1] && s[i] === s[j]
        - s[i] === s[j]：表明当前中心可以继续扩张
        - d[i+1][j-1]： true
            - 说明子串`s[i+1][j-1]`也是回文串
3. 说明：i从最大值开始遍历，j从最小值开始遍历。
4. 总结：
    - dp[i][j] = s[i] == s[j] && ( dp[i+1][j-1] || j - i < 2)

