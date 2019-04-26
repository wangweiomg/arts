## ARTS - Algorithm 补 2019.1.23
## [19. 删除链表的倒数第N个节点](https://leetcode-cn.com/problems/remove-nth-node-from-end-of-list/)

### 题目
给定一个链表，删除链表的倒数第 n 个节点，并且返回链表的头结点。

示例：

```
给定一个链表: 1->2->3->4->5, 和 n = 2.

当删除了倒数第二个节点后，链表变为 1->2->3->5.
```
说明：

给定的 n 保证是有效的。

进阶：

你能尝试使用一趟扫描实现吗？


### 分析

如果没有一趟扫描实现的限制，那么通过两次扫描就能实现： 第一次算出总长度，第二次删除指定节点。但是要求一次扫描就完成，我们有没有更好的思路？

我们根据条件知道 队尾和删除节点相距为 n, 我们可以使用两个指针，距离为n， 然后同步走，先走的指针到达队尾，那么后出发的指针就到了要删除的节点，代码如下：

### 代码

```
public ListNode removeNthFromEnd(ListNode head, int n) {

        if (head == null || n <= 0) {
            return null;
        }

        ListNode first = head;
        ListNode second = head;
        for (int i = 0; i < n; i++) {
            second = second.next;

        }

        if (second == null) {
            // 第一个first
            return head.next;
        }

        while (second.next != null) {
            first = first.next;
            second = second.next;
        }
        first.next = first.next.next;

        return head;

    }
```