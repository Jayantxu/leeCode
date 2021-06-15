*原题链接：👉：[剑指 Offer 10- I. 斐波那契数列](https://leetcode-cn.com/problems/fei-bo-na-qi-shu-lie-lcof/)*

题目描述:

1. 写一个函数，输入 n ，求斐波那契（Fibonacci）数列的第 n 项（即 F(N)）。斐波那契数列的定义如下：
```
F(0) = 0,   F(1) = 1
F(N) = F(N - 1) + F(N - 2), 其中 N > 1.
```
2. 斐波那契数列由 0 和 1 开始，之后的斐波那契数就是由之前的两数相加而得出。
3. 答案需要取模 1e9+7（1000000007），如计算初始结果为：1000000008，请返回 1。

示例：

```
输入：n = 2
输出：1
```

```
输入：n = 5
输出：5
```

思路：

> 递归，优化的话，保存一些值。

```
var fib = function(n) {
    const resRecord = {};
    const dfsNres = (N) => {
        if(N===0) return 0;
        if(N===1) return 1;
        let N1, N2;
        if(resRecord[N-1]) {
            N1 = resRecord[N-1];
        } else {
            N1 = dfsNres(N-1);
            resRecord[N-1] = N1;
        }
        if(resRecord[N-2]) {
            N2 = resRecord[N-2];
        } else {
            N2 = dfsNres(N-2);
            resRecord[N-2] = N2;
        }
        return (N1 + N2) % 1000000007;
    }
    return dfsNres(n) ;
};
```