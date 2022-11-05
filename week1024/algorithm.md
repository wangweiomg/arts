## ARTS - Algorithm 补12.17
## [206. 反转链表](https://leetcode-cn.com/problems/reverse-linked-list/submissions/)

### 题目
反转一个单链表。

示例:

输入: 1->2->3->4->5->NULL

输出: 5->4->3->2->1->NULL

进阶:
你可以迭代或递归地反转链表。你能否用两种方法解决这道题？

### 分析

翻转链表，就是让当前节点指向前面的节点，假设如下节点：

A->B->C

当前指针指向 B， 我们要把B指向A，也就是  B.next = A, 但是目前 B.next 是C，把当前指针curr向前推进时候，需要保存C，防止链表断裂。

于是就有了如下思路：

1. 保存curr.next 
2. 将 curr.next 指向prev
3. 将prev指向curr, curr指向 第1步保存的，
4. 最后循环结束，记得将curr指向prev,

### 代码

```
public static ListNode reverse(ListNode head) {

        ListNode prev = null;
        ListNode curr = head;
        ListNode next = null;


        while (curr != null) {
            // 1.保存 curr.next,
            // 2.将curr.next 指向 prev
            // 3. curr 翻转到curr.next,prev前进

            next = curr.next;
            curr.next = prev;

            prev = curr;
            curr = next;

        }
        // 循环结束
        curr = prev;

        return curr;

    }
    
    
    
    public ListNode reverseListRecursion(ListNode head) {
		if (head == null) {
			return head;
		}
		ListNode newHead = recursion(head);
		head.next = null;
		return newHead;
	}
	/**
	 * The recursive solution
	 */
	public ListNode recursion(ListNode p) {
		if (p.next == null) {
			return p;
		} else {
			ListNode next = p.next;
			ListNode newHead = recursion(next);
			next.next = p;
			return newHead;
		}
	}
```

