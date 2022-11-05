## ARTS - Algorithm
## 补 10月22日
## [83. 删除排序链表中的重复元素](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-list/description/)


给定一个排序链表，删除所有重复的元素，使得每个元素只出现一次。

示例 1:

输入: 1->1->2
输出: 1->2
示例 2:

输入: 1->1->2->3->3
输出: 1->2->3


### 分析

这个就是数据结构的链表结构，相当于对删除链表上的某一个元素，把这个待删除的元素的前驱的后继指向该元素的后继，就行了。 

### 代码1
我第一想到方法是笨方法，首先建立一个新链表，第一个元素是 head.val, 遍历链表，如果比head.val 大就把这个元素拼接到新链表上， 返回新链表的head。代码如下：

```
if (head == null) {
            return null;
        }

        ListNode node = new ListNode(head.val);
        ListNode x = node;

        while (head.next != null) {

            head = head.next;
            if (head.val > x.val) {
                x.next = new ListNode(head.val);
                x = x.next;
            }
        }

        return node;

```

### 代码2
然后就意识到不用新建链表就可以，于是写出了简化版：

```
public ListNode deleteDuplicates(ListNode head) {
        if (head == null) {
            return null;
        }

        ListNode flag = head;

        while (head.next != null) {

            int left = head.val;
            int right = head.next.val;

            if (right == left) {

                // 删除2
                if (head.next.next == null) {
                    head.next = null;
                } else {
                    head.next = head.next.next;
                }

            } else {
                // 往前推进，
                head = head.next;
            }

        }


        return flag;

    }
```


### 代码3 

看了答案后，是在2 的基础上进一步简化：

```
public ListNode deleteDuplicates(ListNode head) {
        ListNode flag = head;

        while (head != null && head.next != null) {

            if (head.val == head.next.val) {
                // 删除2
                head.next = head.next.next;

            } else {
                // 往前推进，
                head = head.next;
            }

        }


        return flag;

    }
```