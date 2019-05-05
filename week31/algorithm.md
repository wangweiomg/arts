## ARTS - Algorithm 补2019.2.6

## [[24. 两两交换链表中的节点](https://leetcode-cn.com/problems/swap-nodes-in-pairs/)]

### 题目

给定一个链表，两两交换其中相邻的节点，并返回交换后的链表。

**你不能只是单纯的改变节点内部的值**，而是需要实际的进行节点交换

**示例:**

```c
给定 1->2->3->4, 你应该返回 2->1->4->3.
```

### 分析

这道题要两两交换，且是节点实际交换，那么就是操作指针了。交换节点操作指针时候的重点是不能丢了指向，就是 1、2交换不能丢失他们后继 3，而且，交换后也不能忘了3 的前驱是1，因此我们要保留四个节点：

```Prev -> node -> node.next -> node.next.next```

可以给开头一个假的节点，作为第一个前驱。所以操作步骤如下：

1. 建立假头部指向链表，这个假头部当做prev。
2. 建立三个指针指向当前、当前的后继、当前的后继的后继
3. 操作指向
   1. 前驱指向第二个节点
   2. 第一个节点指向第三个节点
   3. 第二个节点指向第一个节点
4. 当前指针向前推进

### 代码

```java

public static ListNode swapPairs(ListNode head) {

        ListNode prev = new ListNode(0);
        prev.next = head;
        head = prev;
        ListNode node = prev.next;

        while (node != null && node.next != null) {
            // 保存三个
            ListNode first = node;
            ListNode second = node.next;
            ListNode third = node.next.next;

            prev.next = second;
            first.next = third;
            second.next = first;

						// 这时的node已经是交换过的node了，所以推动前驱就是指向这个第二个node
            prev = node;
            // 此时只需要往前推动一步就跳过了这一对节点
            node = node.next;
        }

        return head.next;

    }
```

