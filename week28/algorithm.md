## ARTS - Algorithm 补2019.1.16
## [160. 相交链表](https://leetcode-cn.com/problems/intersection-of-two-linked-lists/submissions/)

### 题目

编写一个程序，找到两个单链表相交的起始节点。
如下面的两个链表：

![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/14/160_statement.png)

在节点 c1 开始相交。

 

示例 1：
![](https://assets.leetcode.com/uploads/2018/12/13/160_example_1.png)


>输入：intersectVal = 8, listA = [4,1,8,4,5], listB = [5,0,1,8,4,5], skipA = 2, skipB = 3
输出：Reference of the node with value = 8
输入解释：相交节点的值为 8 （注意，如果两个列表相交则不能为 0）。从各自的表头开始算起，链表 A 为 [4,1,8,4,5]，链表 B 为 [5,0,1,8,4,5]。在 A 中，相交节点前有 2 个节点；在 B 中，相交节点前有 3 个节点。
 

示例 2：
![](https://assets.leetcode.com/uploads/2018/12/13/160_example_2.png)


>输入：intersectVal = 2, listA = [0,9,1,2,4], listB = [3,2,4], skipA = 3, skipB = 1
输出：Reference of the node with value = 2
输入解释：相交节点的值为 2 （注意，如果两个列表相交则不能为 0）。从各自的表头开始算起，链表 A 为 [0,9,1,2,4]，链表 B 为 [3,2,4]。在 A 中，相交节点前有 3 个节点；在 B 中，相交节点前有 1 个节点。
 

示例 3：

![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/14/160_example_3.png)

输入：intersectVal = 0, listA = [2,6,4], listB = [1,5], skipA = 3, skipB = 2
输出：null
输入解释：从各自的表头开始算起，链表 A 为 [2,6,4]，链表 B 为 [1,5]。由于这两个链表不相交，所以 intersectVal 必须为 0，而 skipA 和 skipB 可以是任意值。
解释：这两个链表不相交，因此返回 null。
 

注意：

如果两个链表没有交点，返回 null.
在返回结果后，两个链表仍须保持原有的结构。
可假定整个链表结构中没有循环。
程序尽量满足 O(n) 时间复杂度，且仅用 O(1) 内存。

### 分析

求链表相交，首先暴力算法，对链表 A的每个节点都遍历链表B来比较，知道找到相交的，代码如下：

```
public ListNode getIntersectionNode(ListNode headA, ListNode headB) {

        ListNode p1 = headA;
        ListNode p2 = headB;

        while (p1 != null) {

            while (p2 != null) {
                if (p1 == p2) {
                    return p1;
                }
                p2 = p2.next;
            }
            p1 = p1.next;
            // 重置p2
            p2 = headB;
        }


        return null;

    }

```

然后看到注意里说，尽量满足 O(n) 时间复杂度，且用 O(1) 内存。那么，用上面的算法就是 m x n 的时间复杂度了。

我们继续审题，如果两个链表长度一样，那么共进退就可以找到相逢的地方了，可以先找到两个链表的长度，然后让长的往前走几步，两个长度相等再一起走，就能找到了。

代码如下

### 代码

```
public static ListNode getIntersectionNode2(ListNode headA, ListNode headB) {

        int lenA = getLength(headA);
        int lenB = getLength(headB);

        if (lenA == 0 || lenB == 0) {
            return null;
        }

        if (lenA < lenB) {
            // swap
            ListNode tmp = headA;
            headA = headB;
            headB = tmp;

        }

        for (int i = 0; i < Math.abs(lenA - lenB); i++) {
            headA = headA.next;
        }

        while (headA != headB) {
            headA = headA.next;
            headB = headB.next;
        }


        return headA;


    }

    private static int getLength(ListNode node) {
        if (node == null) {
            return 0;
        }
        int n = 0;
        while (node != null) {
            node = node.next;
            n++;
        }

        return n;

    }

```

