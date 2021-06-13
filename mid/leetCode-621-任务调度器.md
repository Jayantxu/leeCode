*原题链接：👉：[任务调度器](https://leetcode-cn.com/problems/task-scheduler/description/)*

题目描述:

1. 给你一个用字符数组 tasks 表示的 CPU 需要执行的任务列表。其中每个字母表示一种不同种类的任务。任务可以以任意顺序执行，并且每个任务都可以在 1 个单位时间内执行完。在任何一个单位时间，CPU 可以完成一个任务，或者处于待命状态。

2. 然而，两个 相同种类 的任务之间必须有长度为整数 n 的冷却时间，因此至少有连续 n 个单位时间内 CPU 在执行不同的任务，或者在待命状态。

你需要计算完成所有任务所需要的 最短时间 。

示例 1：

```
输入：tasks = ["A","A","A","B","B","B"], n = 2
输出：8
解释：A -> B -> (待命) -> A -> B -> (待命) -> A -> B
     在本示例中，两个相同类型任务之间必须间隔长度为 n = 2 的冷却时间，而执行一个任务只需要一个单位时间，所以中间出现了（待命）状态。 
```

```
输入：tasks = ["A","A","A","A","A","A","B","C","D","E","F","G"], n = 2
输出：16
解释：一种可能的解决方案是：
     A -> B -> C -> A -> D -> E -> A -> F -> G -> A -> (待命) -> (待命) -> A -> (待命) -> (待命) -> A

```
思路：

> 查看注释，维护一个类型类型的`keValue`对象，记录任务剩余数及冷却剩余数

```
var leastInterval = function(tasks, n) {
    // 如果不需要调度的间隔时间，那么任务执行时间就是任务数
    if(!n) return tasks.length;
    let totalTime = 0; // 执行的总时长
    let finishTaskNum = 0; // 完成任务的数量
    // 统计任务类型及相应数量
    // 每个类型对应一个数组，0：任务数量，1：等待的冷却时间
    let taskType = {};
    for(let iTask of tasks) {
        taskType[iTask] ? taskType[iTask][0]++ : taskType[iTask] = [1, 0];
    }
    while(finishTaskNum < tasks.length) {
        let maxTaskNum = 0,
            selTask = -1;
        // 遍历任务，选出当前任务数还有最多的任务，并且减少冷却时间
        for(let i in taskType) {
            // 减少冷却时间
            if(taskType[i][1]) {
                taskType[i][1] -= 1;
            } else {
                // 识别任务数剩最多的任务
                if(taskType[i][0] > maxTaskNum) {
                    maxTaskNum = taskType[i][0];
                    selTask = i;
                }
            }
        }
        if(selTask !== -1) {
            if(taskType[selTask][0] === 1) {
                delete taskType[selTask];
            } else {
                taskType[selTask][0] -= 1;
                taskType[selTask][1] = n;
            }
            finishTaskNum += 1;
        }
        totalTime += 1;
    }
    return totalTime;
};
```