## ARTS - Algorithm

[21. 合并两个有序链表](https://leetcode-cn.com/problems/merge-two-sorted-lists/description/)


将两个有序链表合并为一个新的有序链表并返回。新链表是通过拼接给定的两个链表的所有节点组成的。 

示例：

输入：1->2->4, 1->3->4
输出：1->1->2->3->4->4

### 分析
两个ListNode ， 根据l1 、 l2 的val，小的给拼的ListNode. 


```

public ListNode mergeTwoLists(ListNode l1, ListNode l2) {

        ListNode head = null, tail = null;

        while (l1 != null && l2 != null) {

            ListNode node = null;
            if (l1.val < l2.val) {
                node = l1;
                l1 = l1.next;
            } else {
                node = l2;
                l2 = l2.next;
            }

            if (head == null) {

                head = tail = node;

            } else {

                tail.next = node;
                tail = tail.next;

            }

        }

        ListNode rest = l1 == null ? l2 : l1;

        if (rest != null) {
            if (head != null && tail != null) {
                tail.next = rest;
            } else {
                head = rest;
            }
        }
        return head;

    }

```