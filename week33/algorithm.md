## ARTS - Algorithm 补 2019.2.20

## [[61. 旋转链表](https://leetcode-cn.com/problems/rotate-list/)](https://leetcode-cn.com/problems/rotate-list/submissions/)

#### 题目

给定一个链表，旋转链表，将链表每个节点向右移动 *k* 个位置，其中 *k* 是非负数。

**示例 1:**

```java
输入: 1->2->3->4->5->NULL, k = 2
输出: 4->5->1->2->3->NULL
解释:
向右旋转 1 步: 5->1->2->3->4->NULL
向右旋转 2 步: 4->5->1->2->3->NULL
```

**示例 2:**

```java
输入: 0->1->2->NULL, k = 4
输出: 2->0->1->NULL
解释:
向右旋转 1 步: 2->0->1->NULL
向右旋转 2 步: 1->2->0->NULL
向右旋转 3 步: 0->1->2->NULL
向右旋转 4 步: 2->0->1->NULL
```



#### 分析

这道题实际是找到新头部，和新头部的前驱，把新头部的前驱的后继指向NULL，老尾部指向老头部。

我们知道向前推动了k步，其实就是链表头部向前走了(len - k)步，k < len,  如果k > len, 那么就是 k = k % len。

于是步骤为：

1. 找出链表长度 len.
2. 头部向前走 len - (k % len) 步，且记录前驱prev, 此时的头部就是新头部
3. 记录新头部，然后prev 指向null
4. 继续往后走到老队尾，老队尾next指向老头部。

继续思考下，如果该链表是环形的，那么我们知道找到新头部，然后断开就行了，所以第4步是可以优化的。



#### 代码

```java
public class RotateRightTest {

    private static void print(ListNode node) {
        System.out.println();
        while (node != null) {
            System.out.print(node.val + "->");
            node = node.next;
        }
        System.out.println();
    }



    public static ListNode rotateRight(ListNode head, int k) {

        ListNode node = head;

        int len = 1;
        while (node.next != null) {
            len++;
            node = node.next;
        }
        node.next = head;

        k %= len;

        for (int i = 0; i < len - k; i++) {

            node = node.next;
        }

        head = node.next;
        node.next = null;
        return head;

    }

    public static void main(String[] args) {
        ListNode n1 = new ListNode(1);
        ListNode n2 = new ListNode(2);
//        ListNode n3 = new ListNode(3);
//        ListNode n4 = new ListNode(4);
//        ListNode n5 = new ListNode(5);
        n1.next = n2;
//        n2.next = n3;
//        n3.next = n4;
//        n4.next = n5;

        print(n1);

        print(rotateRight(n1, 5));

    }
}
```

