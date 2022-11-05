## ARTS - Algorithm 补2019.2.13

## [25. k个一组翻转链表](https://leetcode-cn.com/problems/reverse-nodes-in-k-group/)

#### 题目

给出一个链表，每 *k* 个节点一组进行翻转，并返回翻转后的链表。

*k* 是一个正整数，它的值小于或等于链表的长度。如果节点总数不是 *k* 的整数倍，那么将最后剩余节点保持原有顺序。

**示例 :**

给定这个链表：`1->2->3->4->5`

当 *k* = 2 时，应当返回: `2->1->4->3->5`

当 *k* = 3 时，应当返回: `3->2->1->4->5`

**说明 :**

- 你的算法只能使用常数的额外空间。
- **你不能只是单纯的改变节点内部的值**，而是需要实际的进行节点交换。

#### 分析

这道题和上一题[24. 两两交换链表中的节点](https://leetcode-cn.com/problems/swap-nodes-in-pairs/)很是相似，不同的是上道题是交换两个节点，这道题是交换k个节点。我们根据上题的思路，也是要找出列表分段的头和尾，然后转换相邻两个节点的指向，一段遍历完成，把上一段的尾巴指向这一段的新头部，代码如下：

#### 代码

```java


/**
 * 25. k个一组翻转链表
 * https://leetcode-cn.com/problems/reverse-nodes-in-k-group/
 *
 * @author jacklove
 * @date 2019/5/8
 */
public class ReverseKGroup {



    public static ListNode reverseKGroup(ListNode head, int k) {


        ListNode node = head;
        ListNode fake = new ListNode(0);
        fake.next = head;

        //  上一段的尾巴
        ListNode tail = fake;
        while (node != null) {
            if (!checked(node, k)) {
                // 剩下的不符合翻转条件，直接用上一段的尾巴指向node
                tail.next = node;

                break;
            }

          	// 用来记录当前指针的前一个节点
            ListNode prev = null;
          	// 记录当前一段的第一个节点，也就是翻转后的段尾巴
            ListNode first = node;
            for (int i = 0; i < k; i++) {
                ListNode oldnext = node.next;
                // 改变指向
                node.next = prev;

                // prev前进， node前进
                prev = node;
                node = oldnext;

            }
            // 头，尾
            // 新的， 这一段结尾是prev , 下一段开头是 node,
            tail.next = prev;

            // 重置tail为转换后的新尾巴
            tail = first;


        }


        return fake.next;
    }

  	/**
  	* 检查链表是否足够翻转
  	*/
    private static boolean checked(ListNode node, int k) {

        for (int i = 0; i < k; i++) {
            if (node == null) {
                return false;
            }
            node = node.next;
        }

        return true;
    }

    public static void main(String[] args) {
        ListNode n1 = new ListNode(1);
        ListNode n2 = new ListNode(2);
        ListNode n3 = new ListNode(3);
        ListNode n4 = new ListNode(4);
        ListNode n5 = new ListNode(5);
        n1.next = n2;
        n2.next = n3;
        n3.next = n4;
        n4.next = n5;

        print(n1);

//        print(reverseKGroup(n1, 2));
        print(reverseKGroup(n1, 3));
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



