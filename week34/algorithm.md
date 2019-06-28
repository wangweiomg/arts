## ARTS  - Algorithm 补2019.2.27

## [86. 分隔列表](https://leetcode-cn.com/problems/partition-list/)

#### 题目

给定一个链表和一个特定值 x，对链表进行分隔，使得所有小于 x 的节点都在大于或等于 x 的节点之前。

你应当保留两个分区中每个节点的初始相对位置。

示例:

```java
输入: head = 1->4->3->2->5->2, x = 3
输出: 1->2->2->4->3->5
```



#### 分析

这里其实就是把链表分隔成两部分，左边的都小于 x, 右边的都大于或等于 x, 为了保持原来顺序，按照顺序遍历就行了，把小的拼到一个链表， 把大的拼到另一个链表，然后合并链表就行了。



#### 代码如下

```java
public class PartitionTest {


    public static ListNode partition(ListNode head, int x) {

        ListNode node = head;

        ListNode low = new ListNode(0);
        ListNode high = new ListNode(0);
        ListNode l = low;
        ListNode h = high;


        while (node != null) {
            if (node.val < x) {

                // 去左边
                low.next = node;
                low = low.next;

            } else {
                high.next = node;
                high = high.next;

            }
            node = node.next;
        }

      	// 大的链表末尾为空，小的末尾是大的起点
        high.next = null;
        low.next = h.next;

        return l.next;
    }


    public static void main(String[] args) {
        ListNode n1 = new ListNode(1);
        ListNode n2 = new ListNode(4);
        ListNode n3 = new ListNode(3);
        ListNode n4 = new ListNode(2);
        ListNode n5 = new ListNode(5);
        ListNode n6 = new ListNode(2);
        n1.next = n2;
        n2.next = n3;
        n3.next = n4;
        n4.next = n5;
        n5.next = n6;

        print(n1);
        print(partition(n1, 3));


    }

    private static void print(ListNode node) {
        System.out.println();
        while (node != null) {
            System.out.print(node.val + "->");
            node = node.next;
        }
        System.out.println();

    }

}
```



