## ARTS - Algorithm 补2019.1.2
## [155. 最小栈](https://leetcode-cn.com/problems/min-stack/submissions/)

### 题目
设计一个支持 push，pop，top 操作，并能在常数时间内检索到最小元素的栈。

* push(x) -- 将元素 x 推入栈中。
* pop() -- 删除栈顶的元素。
* top() -- 获取栈顶元素。
* getMin() -- 检索栈中的最小元素。

**示例:**

```
MinStack minStack = new MinStack();
minStack.push(-2);
minStack.push(0);
minStack.push(-3);
minStack.getMin();   --> 返回 -3.
minStack.pop();
minStack.top();      --> 返回 0.
minStack.getMin();   --> 返回 -2.
```


### 分析
两个要求，一是设计一个栈的数据结构，第二是要求取得最小元素时间复杂度为常数。

对于一，我们可以根据栈的先进后出的特点，使用数组来实现，维护一个移动指针，指向栈顶，也就是数组尾巴， 要考虑数组扩容问题。

对于二，要常数时间获得最小元素，就要提前保存下这个最小元素，最初想法就是把最小的单独保存起来，遇到更小的元素就刷新最小的值，但是在做pop操作时候，无法获得倒数第二小的元素，于是就需要用数组维护起来。比如维护一个有序的队列，但是这个明显不需要保存那么多元素，只保存小的就行，这时候就有保存最小元素还是保存索引的问题，如果保存最小元素，可能会出现重复现象，保存索引就不存在，于是最后选定使用保存索引方式。
代码如下。

### 代码

```
class MinStack {
    private int[] stack;
    private int[] mins;
    private int index;
    private int minIndex;
    private int capacity;

    /** initialize your data structure here. */
    public MinStack() {
        index = -1;
        minIndex = 0;
        capacity = 10;
        stack = new int[capacity];
        mins = new int[capacity];
    }
    
    public void push(int x) {
        index++;

        if (index > capacity -1) {
            // 扩容
            expand();

        }
        stack[index] = x;

       
        if (x < getMin()) {
            minIndex++;

            mins[minIndex] = index;
        }

    }
    
    public void pop() {
      

        if (index == mins[minIndex]) {
            mins[minIndex] = 0;
            minIndex--;
        }

        stack[index] = 0;
        index--;



    }
    
    public int top() {
        
        if (index < 0) {
            return 0;
        }

        return stack[index];
    }
    
    public int getMin() {
        
        if (minIndex < 0) {
            return stack[0];
        }
        return stack[mins[minIndex]];
    }
    
    
  
    private void expand() {
 
        int[] tmp = new int[2 * capacity];
        System.arraycopy(stack, 0, tmp, 0, capacity);
        stack = tmp;

        tmp = new int[2 * capacity];
        System.arraycopy(mins, 0, tmp, 0, capacity);
        mins = tmp;

        capacity = capacity * 2;

    }
}

/**
 * Your MinStack object will be instantiated and called as such:
 * MinStack obj = new MinStack();
 * obj.push(x);
 * obj.pop();
 * int param_3 = obj.top();
 * int param_4 = obj.getMin();
 */

```