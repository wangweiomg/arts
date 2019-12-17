## ARTS - Algorithm 补2019.3.6

## [92. 反转链表 II](https://leetcode-cn.com/problems/reverse-linked-list-ii/)

#### 题目

反转从位置 m 到 n 的链表。请使用一趟扫描完成反转。

说明:
1 ≤ m ≤ n ≤ 链表长度。

示例:

输入: 1->2->3->4->5->NULL, m = 2, n = 4
输出: 1->4->3->2->5->NULL

#### 分析

要求是反转从m到n的一段，一次扫描。我们需要找到几个临界点：

* 反转起点前一个，pstart
* 反转起点，start
* 反转结束节点，end
* 反转结束节点后一个 aend

注意情况：

1. 起点相同，即m=1
2. m = n

#### 代码

```java
public class ReverseBetweenTest {

    public static ListNode reverseBetween(ListNode head, int m, int n) {

        if (m == n) {
            return head;
        }

        ListNode node = head;

        ListNode prev = null;

        for (int i = 1; i < m; i++) {
            prev = node;
            node = node.next;
        }

        // 起点前一个
        ListNode tail1 = prev;

        // 起点
        ListNode start = node;
        ListNode next = null;
        for (int i = 0; i < n - m + 1; i++) {
            // 逆转
            next = node.next;
            node.next = prev;

            prev = node;
            node = next;

        }

        //prev 是翻转了最后一个， node的是list外的第一个
        start.next = node;

        if (tail1 == null) {
            return prev;
        } else {
            tail1.next = prev;
            return head;
        }


    }


    public static void main(String[] args) {

        ListNode n1 = new ListNode(1);
        ListNode n2 = new ListNode(2);
        ListNode n3 = new ListNode(3);
//        ListNode n4 = new ListNode(4);
//        ListNode n5 = new ListNode(5);
        n1.next = n2;
        n2.next = n3;
//        n3.next = n4;
//        n4.next = n5;

        n1.print();


//        ListNode a1 = new ListNode(3);
//        ListNode a2 = new ListNode(5);
//        a1.next = a2;
//        a1.print();

//        reverseBetween(a1, 1, 2).print(); // 5,3
//        reverseBetween(a1, 1, 1).print(); // 3,5

        // 1,2,3, 1,2   213

    }


}

```

