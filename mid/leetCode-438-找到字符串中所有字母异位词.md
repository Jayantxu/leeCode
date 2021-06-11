*原题链接：👉：[找到字符串中所有字母异位词](https://leetcode-cn.com/problems/find-all-anagrams-in-a-string/)*

题目描述:

给定一个字符串 s 和一个非空字符串 p，找到 s 中所有是 p 的字母异位词的子串，返回这些子串的起始索引。
字符串只包含小写英文字母，并且字符串 s 和 p 的长度都不超过 20100。
说明：
1. 字母异位词指字母相同，但排列不同的字符串。
2. 不考虑答案输出的顺序。

示例 1：
```
输入:
s: "cbaebabacd" p: "abc"

输出:
[0, 6]

解释:
起始索引等于 0 的子串是 "cba", 它是 "abc" 的字母异位词。
起始索引等于 6 的子串是 "bac", 它是 "abc" 的字母异位词。

```

思路：

> 滑动窗口

```
var findAnagrams = function(s, p) {
    let resArr = [];
    let pMap = new Map(),
        pObj = {};
    // 使用一个Map记录需要p字符串的字符种类及要求数量，这个是不会变的
    // 使用一个Obj用于实时记录窗口内的字符种类及数量，这个种类也必须只能是p串上有的字符
    for(let i of p) {
        if(!pMap.has(i)) {
            pMap.set(i, 0);
            pObj[i] = 0;
        }
        pMap.set(i, pMap.get(i) + 1);
    }
    // 使用两指针用于表示维护的窗口
    let slow = 0,
        fast = 0,
        isValidCharType = 0; // 记录窗口内字符数是否是满足条件的
    while(fast < s.length) {
        let nowChar = s.charAt(fast);
        fast ++;
        // 如果当前fast的字符在是pMap内
        if(pMap.get(nowChar)) {
            // pObj统计的该字符数量要增加
            pObj[nowChar]++;
            // 当数量满足统计数量时，有效字符类型要增加
            if(pMap.get(nowChar) === pObj[nowChar]) {
                isValidCharType++;
            }
        }
        // 当fast - slow 构成的窗口大小，大于等于p字符的长度之后
        while(fast - slow >= p.length) {
            // 如果当前窗口内的字符数量等于需要统计的字符数量
            // 则记录一下slow的指向
            if(isValidCharType === pMap.size) {
                resArr.push(slow);
            }
            // slow开始左移
            let slowChar = s.charAt(slow);
            slow++;
            if(pMap.get(slowChar)) {
                // 如果本身数量正好是等于的，左移之后元素必定就不符合了，因此有效性-1
                if(pObj[slowChar] === pMap.get(slowChar)) {
                    isValidCharType--;
                }
                // 左移之后元素减一
                pObj[slowChar]--;
            }
        }
    }
    return resArr;
};
```