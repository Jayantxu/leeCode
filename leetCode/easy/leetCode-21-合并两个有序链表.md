
*原题链接：👉：[合并两个有序链表](https://leetcode-cn.com/problems/merge-two-sorted-lists/)*

题目描述:

1. 将两个升序链表合并为一个新的 升序 链表并返回。
2. 新链表是通过拼接给定的两个链表的所有节点组成的。 


示例：

① -> ② -> ④

① -> ③ -> ④

*result:*

① -> ① -> ② -> ③ -> ④ -> ④

```
输入：l1 = [1,2,4], l2 = [1,3,4]
输出：[1,1,2,3,4,4]
```

```
输入：l1 = [], l2 = []
输出：[]
```

思路：
> 双指针，同时走，然后比大小，在连接即可。

- 链表不好调试，放弃了书写...看一下题解，递归方式求解，脑门一亮💥，tql！

```
/*
* function ListNode(val, next) {
*     this.val = (val===undefined ? 0 : val)
*     this.next = (next===undefined ? null : next)
* }
*/

var mergeTwoLists = function(l1, l2) {
    if(l1 === null){
        return l2;
    }
    if(l2 === null){
        return l1;
    }
    if(l1.val < l2.val){
        l1.next = mergeTwoLists(l1.next, l2);
        return l1;
    }else{
        l2.next = mergeTwoLists(l1, l2.next);
        return l2;
    }
};

```