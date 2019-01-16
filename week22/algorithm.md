## ARTS - Algorithm 补12.3
### [142. 环形链表 II](https://leetcode-cn.com/problems/linked-list-cycle-ii/)

### 题目
给定一个链表，返回链表开始入环的第一个节点。 如果链表无环，则返回 null。

为了表示给定链表中的环，我们使用整数 pos 来表示链表尾连接到链表中的位置（索引从 0 开始）。 如果 pos 是 -1，则在该链表中没有环。

说明：不允许修改给定的链表。

 

示例 1：

```
输入：head = [3,2,0,-4], pos = 1
输出：tail connects to node index 1
解释：链表中有一个环，其尾部连接到第二个节点。
```
![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/07/circularlinkedlist.png)

示例 2：

```
输入：head = [1,2], pos = 0
输出：tail connects to node index 0
解释：链表中有一个环，其尾部连接到第一个节点
```
![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/07/circularlinkedlist_test2.png)

示例 3：

```
输入：head = [1], pos = -1
输出：no cycle
解释：链表中没有环。
```
![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/07/circularlinkedlist_test3.png)

进阶：

你是否可以不用额外空间解决此题？

### 分析
这道题是[141.环形链表](https://leetcode-cn.com/problems/linked-list-cycle/)的进阶版，141只要判断是否存在环，这个要求找出进环位置。

首先考虑还是使用HashSet Key的唯一性来做判断，只要第一次存在重复的元素，就说明这个元素为进环位置，代码如下：

```
public ListNode detectCycle2(ListNode head) {
        if (head == null || head.next == null) {
            return null;
        }

        Set<ListNode> set = new HashSet<>();

        while (head != null) {
            boolean flag = set.add(head);
            if (!flag) {
                return head;
            }

            head = head.next;
        }

        return null;

    }
```

我们需要考虑不增加额外空间来实现这个功能。也参照141环形链表的双指针方案。
思考这样一个链表：

![](https://img-blog.csdn.net/20151009091556303)


我们假设这个链表 起点、环入口、第一次相遇点 分别是 X, Y, Z, 之间的距离分别是a,b,c

那么，在 Z 点第一次相遇，此时 

* 慢指针走的距离为 a+b, 
* 快指针走的距离为  a+b+c+b ,

由于快指针速度是慢指针的2倍，也就是说快指针走过的距离为慢指针的距离的2倍，于是就存在以下等式：

2* (a+b) = a+b+c+b 

也就是
2a + ab = a+c + 2b,

也就是 
**a == c**
 
也就是，两个指针第一次相遇点到环入口的距离 和 起点到入口的距离相等，那么我们就可以利用这个特点来写代码。

### 代码

```
public ListNode detectCycle(ListNode head) {

        if (head == null || head.next == null) {
            return null;
        }

        ListNode fast = head;
        ListNode slow = head;

        do {
            fast = fast.next.next;
            slow = slow.next;
        } while (fast != null && fast.next  != null && fast != slow);

        if (fast != slow) {
            return null;
        }

        ListNode p1 = head;
        ListNode p2 = slow;

        while (p1 != p2) {
            p1 = p1.next;
            p2 = p2.next;
        }
        return p1;



    }

```