## ARTS - Algorithm
## 补 10月29日
## [82. 删除排序链表中的重复元素 II](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-list-ii/description/)

### 题目

给定一个排序链表，删除所有含有重复数字的节点，只保留原始链表中 没有重复出现 的数字。

示例 1:

输入: 1->2->3->3->4->4->5
输出: 1->2->5
示例 2:

输入: 1->1->1->2->3
输出: 2->3


### 分析

此题和 [83. 删除排序链表中的重复元素](https://leetcode-cn.com/problems/remove-      duplicates-from-sorted-list/description/)  相似，属于在这道题的基础上增加了一个难度，不仅是重复的变成不重复的，还要把重复过的元素都删掉。

初始的想法是，首先把遇到的和next相等的元素，把next 删掉，把当前的标记，第二次把标记过的删除，但是这样存在问题，

1. 标记的方式，如果设置当前元素的值为某一个数，如果元素中存在这个，就造成不准确； 
2. 如果存在连续重复的，标记了一个就无法删除掉全部的重复数据。 所以这种方法是不可行的，

还有一个方法，是双向循环链表结构，这样的话可以让前驱直接指向后继的后继，就一次删除了当前的和当前的后继， 但是存在连续重复的问题，需要把重复元素保存起来。

代码如下：

```
public static ListNode deleteDuplicates(ListNode head) {

        // 1. 记录重复值， 2.删除重复值， 3.从第二个开始
        ListNode node = head;
        ListNode prev = new ListNode(head.val - 1);
        ListNode first = prev;
        int duplicate = prev.val;
        prev.next = node;

        while (node != null && node.next != null) {

            // 如果当前和重复的相等，就把当前删掉
            if (node.val == duplicate) {

                prev.next = node.next;

                node = node.next;

            } else {
                // 当前不重复，就判断和下一个重复
                if (node.val == node.next.val) {
                    duplicate = node.val;
                    // 向前推进两个
                    prev.next = node.next.next;
                    node = node.next.next;
                } else {
                    // 不重复

                    prev = node;
                    node = node.next;

                }

            }

        }


        return first.next;


    }


```


这个看似正确，但是如果head.val = Integer.MIN_VALUE d  的话，程序会报错的，所以不是正确答案。


### 代码

参考了答案后，代码如下：

```

public static ListNode deleteDuplicates(ListNode head) {

        ListNode tail = new ListNode(0);
        tail.next = head;

        head = tail;
        boolean flag = false;


        for (ListNode node = head.next; node != null && node.next != null; node = node.next) {

            if (!flag && node.val == node.next.val) {

                flag = true;
                continue;
            }
            if (flag && node.val != node.next.val) {
                flag = false;
                tail.next = node.next;

                continue;
            }

            if (!flag) {
                tail = node;
            }
        }

        if (flag) {
            tail.next = null;
        }
        return head.next;


    }
```