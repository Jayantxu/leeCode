*原题链接：👉：[除法求值](https://leetcode-cn.com/problems/evaluate-division/description/)*

题目描述:

1. '[["a","c"],["b","a"],["a","e"],["a","a"],["x","x"]]'
给你一个变量对数组 equations 和一个实数值数组 values 作为已知条件，其中 equations[i] = [Ai, Bi] 和 values[i] 共同表示等式 Ai / Bi = values[i] 。每个 Ai 或 Bi 是一个表示单个变量的字符串。

2. 另有一些以数组 queries 表示的问题，其中 queries[j] = [Cj, Dj] 表示第 j 个问题，请你根据已知条件找出 Cj / Dj = ? 的结果作为答案。

3. 返回 所有问题的答案 。如果存在某个无法确定的答案，则用 -1.0 替代这个答案。如果问题中出现了给定的已知条件中没有出现的字符串，也需要用 -1.0 替代这个答案。

4. 注意：输入总是有效的。你可以假设除法运算中不会出现除数为 0 的情况，且不存在任何矛盾的结果。

示例 1：
```
输入：equations = [["a","b"],["b","c"]], values = [2.0,3.0], queries = [["a","c"],["b","a"],["a","e"],["a","a"],["x","x"]]
输出：[6.00000,0.50000,-1.00000,1.00000,-1.00000]
解释：
条件：a / b = 2.0, b / c = 3.0
问题：a / c = ?, b / a = ?, a / e = ?, a / a = ?, x / x = ?
结果：[6.0, 0.5, -1.0, 1.0, -1.0 ]
```

示例 2：
```
输入：equations = [["a","b"],["b","c"],["bc","cd"]], values = [1.5,2.5,5.0], queries = [["a","c"],["c","b"],["bc","cd"],["cd","bc"]]
输出：[3.75000,0.40000,5.00000,0.20000]
```

示例 3：
```
输入：equations = [["a","b"]], values = [0.5], queries = [["a","b"],["b","a"],["a","c"],["x","y"]]
输出：[0.50000,2.00000,-1.00000,-1.00000]
```

思路：

> 草稿画了画，知道应该构建互相之间的关系，但是一下子不知道用什么形式存储和寻找...

- 题解

> 使用`map`构建图，每一个字符作为`key`，`value`存储有关系的节点及路径权值组成的数组，例如：

```
1: [ [a, b], [b, c] ]
// 转换后
{
    a: [
        [b, 2]
    ],
    b: [
        [a, 0.5],
        [c, 2]
    ],
    c: [
        [b, 0.5]
    ]
}
```

这样的形式，然后在依据此进行深度遍历再查找，同时为了避免陷入循环，例如：
```
求 a / c ？

a -> b -> c

```
找到a， 再找 b， b的数组中有a，因此不能取a，所以应该有一个标识防止。因此建立了一个`visitMap`存储`key`用于判断。



**code**

```
var calcEquation = function(equations, values, queries) {
    let eMap = new Map(), // 保存值之间的映射关系与路径值
        resArr = [], // 保存结果数组
        visitMap = new Map(); // 再向下撰取的过程中记录是否访问过，避免陷入循环
    // 深度遍历
    const dfsFind = (nowKey, aimKey) => {
        if(nowKey === aimKey) {
            return 1;
        }
        let nowKeyArr = eMap.get(nowKey);
        for(let i = 0; i < nowKeyArr.length; i++) {
            // 找到该键的映射的value数组
            let nowArr = nowKeyArr[i];
            if(!visitMap.get(nowArr[0])) {
                // 设为true，避免后续判断循环进去
                visitMap.set(nowArr[0], true);
                let tempRes = dfsFind(nowArr[0], aimKey);
                visitMap.set(nowArr[0], false);
                if(tempRes !== -1) {
                    // 当前的路径值*后续的路径值
                    return nowArr[1] * tempRes;
                }
            }
        }
        return -1;
    };
    // 构建图
    for(let i = 0; i < equations.length; i++) {
        let e = equations[i],
            v = values[i];
        if(!eMap.has(e[0])) {
            eMap.set(e[0], []);
            visitMap.set(e[0], false);
        }
        if(!eMap.has(e[1])) {
            eMap.set(e[1], []);
            visitMap.set(e[1], false);
        }
        let edjs1 = eMap.get(e[0]);
        let edjs2 = eMap.get(e[1]);
        edjs1.push( [ e[1], v ] );
        edjs2.push( [ e[0], 1 / v ] );
    }
    for(let i = 0; i < queries.length; i++) {
        let q = queries[i];
        // 前面构建图的时候已经会加入两个除数被除数的节点了，如果有出现一定会成为map的key
        // 因此两者必须同时存在才可以保证互相连接到
        if(eMap.has(q[0]) && eMap.has(q[1])) {
            visitMap.set(q[0], true);
            resArr.push(dfsFind(q[0], q[1]));
            visitMap.set(q[0], false);
        } else {
            resArr.push(-1);
        }
    }
    return resArr;
};
```