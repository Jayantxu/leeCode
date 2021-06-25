*原题链接：👉：[每日温度](https://leetcode-cn.com/problems/daily-temperatures/description/)*

题目描述:

1. 请根据每日 气温 列表，重新生成一个列表。对应位置的输出为：要想观测到更高的气温，至少需要等待的天数。如果气温在这之后都不会升高，请在该位置用 0 来代替。

2. 例如，给定一个列表 temperatures = [73, 74, 75, 71, 69, 72, 76, 73]，你的输出应该是 [1, 1, 4, 2, 1, 1, 0, 0]。

3. 提示：气温 列表长度的范围是 [1, 30000]。每个气温的值的均为华氏度，都是在 [30, 100] 范围内的整数。

思路:

> 维护一个单调递减栈，

- 当 `i=0` 时，单调栈为空，因此将 `0` 进栈。
    - {stack}=[0(73)]
    - ans=[0,0,0,0,0,0,0,0]

- 当 `i=1` 时，由于`74 > 73`，因此移除栈顶元素 `0`，赋值 `ans[0]:=1-0`，将 `1` 进栈。
    - {stack}=[1(74)]
    - {ans}=[1,0,0,0,0,0,0,0]

- 当 `i=2` 时，由于 `75 > 74`，因此移除栈顶元素 `1`，赋值 `ans[1]:=2-1`，将 `2` 进栈。
    - {stack}=[2(75)]
    - ans=[1,1,0,0,0,0,0,0]

- ...


**code**

```
var dailyTemperatures = function(temperatures) {
    // 单调栈，栈内存放温度索引
    let tempStack = [],
        resArr = [];
    for(let i = 0; i < temperatures.length; i++) {
        while(
            temperatures[tempStack[tempStack.length - 1]] < temperatures[i]) {
            let topIdx = tempStack.pop();
                resArr[topIdx] = i - topIdx;
        }
        tempStack.push(i);
    }
    while(tempStack.length) {
        let idx = tempStack.pop();
        resArr[idx] = 0;
    }
    return resArr;
};
```