*原题链接：👉：[根据身高重建队列](https://leetcode-cn.com/problems/queue-reconstruction-by-height/description/)*

题目描述:

1. 假设有打乱顺序的一群人站成一个队列，数组 people 表示队列中一些人的属性（不一定按顺序）。每个 people[i] = [hi, ki] 表示第 i 个人的身高为 hi ，前面 正好 有 ki 个身高大于或等于 hi 的人。

2. 请你重新构造并返回输入数组 people 所表示的队列。返回的队列应该格式化为数组 queue ，其中 queue[j] = [hj, kj] 是队列中第 j 个人的属性（queue[0] 是排在队列前面的人）。

示例 1：
```
输入：people = [[7,0],[4,4],[7,1],[5,0],[6,1],[5,2]]
输出：[[5,0],[7,0],[5,2],[6,1],[4,4],[7,1]]
解释：
编号为 0 的人身高为 5 ，没有身高更高或者相同的人排在他前面。
编号为 1 的人身高为 7 ，没有身高更高或者相同的人排在他前面。
编号为 2 的人身高为 5 ，有 2 个身高更高或者相同的人排在他前面，即编号为 0 和 1 的人。
编号为 3 的人身高为 6 ，有 1 个身高更高或者相同的人排在他前面，即编号为 1 的人。
编号为 4 的人身高为 4 ，有 4 个身高更高或者相同的人排在他前面，即编号为 0、1、2、3 的人。
编号为 5 的人身高为 7 ，有 1 个身高更高或者相同的人排在他前面，即编号为 1 的人。
因此 [[5,0],[7,0],[5,2],[6,1],[4,4],[7,1]] 是重新构造后的队列。
```

示例 2：
```
输入：people = [[6,0],[5,0],[4,0],[3,2],[2,2],[1,4]]
输出：[[4,0],[5,0],[2,2],[3,2],[1,4],[6,0]]
```

思路：

- 题解

[先排序，再插队](https://leetcode-cn.com/problems/queue-reconstruction-by-height/solution/xian-pai-xu-zai-cha-dui-dong-hua-yan-shi-suan-fa-g/)


**一般这种数对，还涉及排序的，根据第一个元素正向排序，根据第二个元素反向排序，或者根据第一个元素反向排序，第二个元素正向排序，可以简化过程**


```
people.sort((a, b) => b[0] - a[0] || a[1] - b[1]);
```

按照高度降序之后，例如：
`[[9,0],[7,0],[1,9],[3,4],[2,7],[5,3],[6,0],[3,0],[6,2],[5,2]]`降序后：
`[[9,0],[7,0],[6,0],[6,2],[5,2],[5,3],[3,0],[3,4],[2,7],[1,9]]`

①、取`[9,0]`插入`resArr`，`resArr = [[9,0]]`
②、取`[7,0]`插入`resArr`，`resLen = 1 > [1] = 0`，因此`resArr = [[7,0], [9,0]]`
③、取`[1,9]`插入`resArr`，`resLen = 2 < [1] = 9`，直接插入`resArr = [[7,0], [9,0], [1,9]]`
...

**code**

```
var reconstructQueue = function(people) {
    people.sort((a, b) => b[0] - a[0] || a[1] - b[1]);
    let resArr = [];
    for(let i = 0; i < people.length; i++) {
        let nowMan = people[i];
        let resLen = resArr.length;
        if(resLen > nowMan[1]) {
            let nextArr = resArr.splice(nowMan[1]);
            resArr.push(nowMan);
            resArr = resArr.concat(nextArr);
        } else {
            resArr.push(nowMan);
        }
    }
    return resArr;
};
```