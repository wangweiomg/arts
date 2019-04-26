## ARTS - Algorithm 补2019.1.30
## [23. 合并K个排序链表](https://leetcode-cn.com/problems/merge-k-sorted-lists/submissions/)

### 题目
合并 k 个排序链表，返回合并后的排序链表。请分析和描述算法的复杂度。

示例:

```
输入:
[
  1->4->5,
  1->3->4,
  2->6
]
输出: 1->1->2->3->4->4->5->6
```

### 分析

相当于合并多个有序数组，也可以先合并两个有序数组，再合并剩下的，于是代码如下：

### 代码：

```
public class MergeKLists {


    public static ListNode mergeKLists(ListNode[] lists) {


        int len = lists.length;

        if (len == 0) {
            return null;
        }

        ListNode res = lists[0];
        for (int i = 1; i < len; i++) {

            res = mergeNode(res, lists[i]);

        }

        return res;

    }

    private static ListNode mergeNode(ListNode node1, ListNode node2) {

        ListNode fake = new ListNode(0);
        ListNode head = fake;

        while (node1 != null && node2 != null) {
            if (node1.val < node2.val) {
                head.next = node1;
                node1 = node1.next;
            } else {
                head.next = node2;
                node2 = node2.next;
            }
            head = head.next;
        }

        if (node1 == null) {
            head.next = node2;
        }
        if (node2 == null) {
            head.next = node1;
        }

        return fake.next;
    }

    public static void main(String[] args) {
        ListNode n1 = new ListNode(1);
        ListNode n2 = new ListNode(4);
        ListNode n3 = new ListNode(5);
        n1.next = n2;
        n2.next = n3;

        ListNode n4 = new ListNode(2);
        ListNode n5 = new ListNode(4);
        n4.next = n5;

        System.out.println("------");
        print(n1);
        System.out.println("\n------");
        print(n4);
        System.out.println("\n------");
        print(mergeKLists(new ListNode[]{null}));
//        print(mergeKLists(new ListNode[]{n1, n4}));


    }


    private static void print(ListNode node) {
        while (node != null) {
            System.out.print(node.val + "->");

            node = node.next;


        }
    }
}
```