*原题链接：👉：[接雨水](https://leetcode-cn.com/problems/trapping-rain-water/description/)*

题目描述:

1. 给定 n 个非负整数表示每个宽度为 1 的柱子的高度图，计算按此排列的柱子，下雨之后能接多少雨水。

示例：
```
输入：height = [0,1,0,2,1,0,1,3,2,1,2,1]
输出：6
解释：上面是由数组 [0,1,0,2,1,0,1,3,2,1,2,1] 表示的高度图，在这种情况下，可以接 6 个单位的雨水（蓝色部分表示雨水）。
```

思路：
> 把数组或者草图画出来，找一个降了又升的区间，乘上最大最小差值，就是改区域的面积。
> 例如`[2,1,0,1,3]`之间容纳*4*区域的水，因为从*2*降到*0*又升到*3*,所以这个区间是能装水的。
> 能装多少水呢？
> 假设中间没有任何阻挡，也就是去掉*1*，那么数组就是`[2,0,0,0,3]`，最低是*0*，这个区间可以装*6*个面积水，现在再加上两个*1*高度，那么就减去*2*，于是就剩下*4*面积水...
> 应该是这样，试试看🙋‍♂️

🤦‍♂️哭辽没思路
看看官方题解。。


- 单调栈

和我计算先减后增区间差不多，感觉，咋没想到用栈呢。
> 从左向右遍历数组，遍历到下标`i`，如果栈内至少有两个元素时，栈顶元素为*top*，*top*下元素为*left*，则一定**height[left]>=height[top]**(！！因为是减区间)，如果**height[i]>height[top]**，则得到一个接雨水区域，宽度是`i-left-1`，高度是`min(height[left], height[i] - height[top])`。

什么意思呢？例如：

[3, 1, 2].

`left`是3，`top`是1，因此形成一个减区间[3, 1]，然后遍历到`i = 2`，2大于`top`1，所有开始上增了，这个形成的区间就可以装水。

知道思路了，试着写一下。

```
var trap = function(height) {
    let ans = 0;
    let asnArr = [];
    let hL = height.length;
    for(let i = 0; i < hL; i++) {
        let len = asnArr.length - 1;
        while(asnArr.length && height[i] > height[asnArr[len]]) { // i比top大
            let topIdx = asnArr.pop();
            if(asnArr.length === 0) { // [0,1,0,2,1,0,1,3,2,1,2,1], i = 1时， pop了0，没了，所有要判断一下len
                break;
            }
            let left = asnArr[asnArr.length - 1]; // 已经pop了top，所有len - 1 = left
            let width = i -left - 1;
            let ht = Math.min(height[left], height[i]) - height[topIdx];
            ans += width * ht;
            len = asnArr.length - 1;
        }
        asnArr.push(i);
    }
    return ans;
};
```