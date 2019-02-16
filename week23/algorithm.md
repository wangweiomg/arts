## ARTS - Algorithm  补12.10
### [143. 重排链表](https://leetcode-cn.com/problems/reorder-list/)

### 题目

> 给定一个单链表 L：L0→L1→…→Ln-1→Ln ，
> 
> 将其重新排列后变为： L0→Ln→L1→Ln-1→L2→Ln-2→…

你不能只是单纯的改变节点内部的值，而是需要实际的进行节点交换。

示例 1:

给定链表 1->2->3->4, 重新排列为 1->4->2->3.

示例 2:

给定链表 1->2->3->4->5, 重新排列为 1->5->2->4->3.

### 分析
还是先用最简单的方法实现。我们发现最后的排列结果其实就是取一个首，取一个尾，用栈这种数据结构很合适，于是就想到了如下代码：

```
public void reorderList(ListNode head) {

        LinkedList<Integer> list = new LinkedList<>();

        while (head != null) {
            list.add(head.val);
            head = head.next;
        }


        ListNode node = new ListNode(0);
        ListNode n = node;

        while (list.size() > 0) {

            int a = list.pollFirst();
            n.next = new ListNode(a);

            Integer x = list.pollLast();
            if (x == null) {
                n.next.next = null;
            } else {
                n.next.next = new ListNode(x);
            }
            n = n.next.next;

        }

        head = node.next;

    }


```

但是，题目要求： 

**你不能只是单纯的改变节点内部的值，而是需要实际的进行节点交换。**

所以，改变了值的是不行的。那么应该把ListNode对象放进队列，然后再取头取尾。

如下：

```
public void reorderList2(ListNode head) {

        LinkedList<ListNode> list = new LinkedList<>();

        while (head != null) {
            list.add(head);
            head = head.next;
        }

        for (ListNode node:
             list) {
            node.next = null;
        }

        ListNode first = list.pollFirst();
        ListNode node = first;

        boolean flag = true;
        while (list.size() > 0) {
            if (flag) {
                node.next = list.pollLast();
                flag = false;
            } else {
                node.next = list.pollFirst();
                flag = true;

            }
            node = node.next;

        }

        

    }

```


这个提交结果是超出时间限制，超出内存限制。我们再优化下，如下：

```
public static ListNode reorderList2(ListNode head) {

        LinkedList<ListNode> list = new LinkedList<>();

        ListNode node = head;
        while (node != null) {
            list.add(node);
            node = node.next;
        }
        node = list.pollFirst();
        boolean flag = true;
        while (!list.isEmpty()) {
            if (flag) {
                node.next = list.pollLast();
                flag = false;
            } else {
                node.next = list.pollFirst();
                flag = true;

            }
            node = node.next;

        }

        if (node != null) {
            node.next  = null;
        }


        return node;
    }

```
然后可以了。

我们继续思考，肯定有更好的解决方法，不用引入队列，那么该如何做呢？想象一下，用双指针，找到中点，然后list截成两段，后一段翻转，再合并。代码如下

### 代码

#### C++版
```
// Source : https://oj.leetcode.com/problems/reorder-list/
// Author : Hao Chen
// Date   : 2014-06-17

/********************************************************************************** 
* 
* Given a singly linked list L: L0→L1→…→Ln-1→Ln,
* reorder it to: L0→Ln→L1→Ln-1→L2→Ln-2→…
* 
* You must do this in-place without altering the nodes' values.
* 
* For example,
* Given {1,2,3,4}, reorder it to {1,4,2,3}.
* 
*               
**********************************************************************************/

#include <stdio.h>
#include <stdlib.h>
/**
 * Definition for singly-linked list.
 */
class ListNode {
public:
    int val;
    ListNode *next;
    ListNode():val(0), next(NULL) {}
    ListNode(int x) : val(x), next(NULL) {}
};

class Solution {
public:
    void reorderList(ListNode *head) {
        ListNode *pMid = findMidPos(head);
        pMid = reverseList(pMid);
        head = Merge(head, pMid);
    }
    
private:
    ListNode* findMidPos(ListNode *head){

        ListNode *p1, *p2, *p=NULL;
        p1 = p2 = head;
        
        while(p1!=NULL && p2!=NULL && p2->next!=NULL){
            p = p1;
            p1 = p1->next;
            p2 = p2->next->next;
        }

        if(p!=NULL){
            p->next = NULL;
        }
        
        return p1;
    }
    
    ListNode* reverseList(ListNode *head){
        ListNode* h = NULL;
        ListNode *p;
        while (head!=NULL){
            p = head;
            head = head->next;
            p->next = h;
            h = p;
        }
        return h;
    }
    
    ListNode* Merge(ListNode *h1, ListNode* h2) {
        ListNode *p1=h1, *p2=h2, *p1nxt, *p2nxt;
        while(p1!=NULL && p2!=NULL){
            p1nxt = p1->next;
            p2nxt = p2->next;
            
            p1->next = p2;
            p2->next = p1nxt;
            
            if (p1nxt==NULL){
                p2->next = p2nxt;
                break;
            }
            p1=p1nxt;
            p2=p2nxt;
        }
    }
};

void printList(ListNode *h){
    while(h!=NULL){
        printf("%d->", h->val);
        h = h->next;
    }
    printf("nil\n");
}

int main(int argc, char** argv)
{
    int size = atoi(argv[1]);
    ListNode* n = new ListNode[size] ;

    for(int i=0; i<size; i++){
        n[i].val = i;
        if( i+1 < size) {
            n[i].next = &n[i+1];
        }
    }


    Solution s;
    s.reorderList(&n[0]);
    printList(&n[0]);
    
    
    
    return 0;
}
```

#### Python版

```
def reorderList(self, head):
    if not head or not head.next: return
    
    # Step 1: find the middle node
    middle = None
    slow, fast = head, head
    while fast and fast.next:
        middle = slow
        slow = slow.next
        fast = fast.next.next
    middle.next = None
    
    # Step 2: reverse the second half
    prev = None
    while slow:
        nextNode = slow.next
        slow.next = prev
        prev, slow = slow, nextNode
        
    # Step 3: merge two lists
    while head and prev:
        first, second = head.next, prev.next
        head.next = prev
        if first: prev.next = first
        head, prev = first, second
```



