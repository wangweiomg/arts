## ARTS - Algorithm 补11.26
### [141. 环形链表](https://leetcode-cn.com/problems/linked-list-cycle/)

### 题目
给定一个链表，判断链表中是否有环。

为了表示给定链表中的环，我们使用整数 pos 来表示链表尾连接到链表中的位置（索引从 0 开始）。 如果 pos 是 -1，则在该链表中没有环。

 

示例 1：

```
输入：head = [3,2,0,-4], pos = 1
输出：true
解释：链表中有一个环，其尾部连接到第二个节点。
```

![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/07/circularlinkedlist.png)

示例 2：

```
输入：head = [1,2], pos = 0
输出：true
解释：链表中有一个环，其尾部连接到第一个节点。
```
![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/07/circularlinkedlist_test2.png)

示例 3：

```
输入：head = [1], pos = -1
输出：false
解释：链表中没有环。
```
![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/07/circularlinkedlist_test3.png)

进阶：

你能用 O(1)（即，常量）内存解决此问题吗？

### 分析
这道题是要判断链表中有没有存在环。是否存在环，特点是有两个指针都指向同一个元素，就根据这个特点来判断，于是用最暴力的手段，把节点存到集合里，每取一个就去和集合中的比较，如果相同就代表存在环，于是代码如下：

```
public static boolean hasCycle(ListNode head) {

        if (head == null || head.next == null) {
            return false;
        }

        List<ListNode> list = new ArrayList<>();

        while (head != null) {
            if (list.contains(head)) {
                return true;
            }
            list.add(head);
            head = head.next;
        }


        return false;
    }
    
    
```

或者考虑用Set集合来接收，判断是否添加成功来确定是否有环

```
		Set<ListNode> set = new HashSet<>();
        while (head != null) {
            boolean flag = set.add(head);
            if (!flag) {
                return true;
            }
            head = head.next;
        }


        return false;
```

### 代码

如果不引入集合，即空间复杂度为O(1),那么该如何解决呢？

如果有环，那么一定会引起指针循环，根据这个特点，可以设计个快慢指针(双指针思想)，一个跳两步，一个跳一步，只要存在循环，那么一定会追上，如果不存在环，那么一定追不上。于是解答代码如下：

```
public class Solution {
    public boolean hasCycle(ListNode head) {
        if (head == null || head.next == null) {
            return false;
        }

        ListNode fast = head;
        ListNode slow = head;

        do {
            fast = fast.next.next;
            slow = slow.next;

        } while (fast != null && fast.next != null && fast != slow);

        return fast == slow ? true : false;
        
    }
}
```
