## ARTS - Algorithm  补12.10
### [143. 重排链表](https://leetcode-cn.com/problems/reorder-list/)

### 题目

> 给定一个单链表 L：L0→L1→…→Ln-1→Ln ，
> 
> 将其重新排列后变为： L0→Ln→L1→Ln-1→L2→Ln-2→…

你不能只是单纯的改变节点内部的值，而是需要实际的进行节点交换。

示例 1:

给定链表 1->2->3->4, 重新排列为 1->4->2->3.

示例 2:

给定链表 1->2->3->4->5, 重新排列为 1->5->2->4->3.

### 分析
还是先用最简单的方法实现。我们发现最后的排列结果其实就是取一个首，取一个尾，用栈这种数据结构很合适，于是就想到了如下代码：

```
public void reorderList(ListNode head) {

        LinkedList<Integer> list = new LinkedList<>();

        while (head != null) {
            list.add(head.val);
            head = head.next;
        }


        ListNode node = new ListNode(0);
        ListNode n = node;

        while (list.size() > 0) {

            int a = list.pollFirst();
            n.next = new ListNode(a);

            Integer x = list.pollLast();
            if (x == null) {
                n.next.next = null;
            } else {
                n.next.next = new ListNode(x);
            }
            n = n.next.next;

        }

        head = node.next;

    }


```

但是，题目要求： 

**你不能只是单纯的改变节点内部的值，而是需要实际的进行节点交换。**

所以，改变了值的是不行的。

