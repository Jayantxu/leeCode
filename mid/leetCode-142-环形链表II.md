*原题链接：👉：[环形链表Ⅱ](https://leetcode-cn.com/problems/linked-list-cycle-ii/description/)*

题目描述:
1. 给定一个链表，返回链表开始入环的第一个节点。 如果链表无环，则返回 null。
2. 为了表示给定链表中的环，我们使用整数 pos 来表示链表尾连接到链表中的位置（索引从 0 开始）。 如果 pos 是 -1，则在该链表中没有环。注意，pos 仅仅是用于标识环的情况，并不会作为参数传递到函数中。

说明：不允许修改给定的链表。

进阶：

你是否可以使用 O(1) 空间解决此题？

示例：

![示例1](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/07/circularlinkedlist.png)
```
输入：head = [3,2,0,-4], pos = 1
输出：返回索引为 1 的链表节点
解释：链表中有一个环，其尾部连接到第二个节点。
```

![示例2](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/07/circularlinkedlist_test2.png)

```
输入：head = [1,2], pos = 0
输出：返回索引为 0 的链表节点
解释：链表中有一个环，其尾部连接到第一个节点。
```

进阶：

你能用 O(1)（即，常量）内存解决此问题吗？

思路：

- 题解，[👉题解链接](https://leetcode-cn.com/problems/linked-list-cycle-ii/solution/linked-list-cycle-ii-kuai-man-zhi-zhen-shuang-zhi-/)

看了题解的，构筑快慢指针，先让快慢指针第一次相遇（如果有），相遇之后，相遇即意味着是有环结构的，相遇之后，令快指针回到head起点，这回，快慢指针都一步一步走，直到再次相遇即为环交点。


```
var detectCycle = function(head) {
    if(!head) return null;
    if(!head.next) return null;
    let p1 = head,
        p2 = head.next;
    while(p1 !== p2) {
        if(!p2) return null;
        if(!p2.next) return null;
        p1 = p1.next;
        p2 = p2.next.next;
    }
    p2 = head;
    while(p1 !== p2) {
        p1 = p1.next;
        p2 = p2.next;
    }
    return p1;
};
```