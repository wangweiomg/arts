## ARTS - Algorithm

[20. 有效的括号](https://leetcode-cn.com/problems/valid-parentheses/description/)

给定一个只包括 '('，')'，'{'，'}'，'['，']' 的字符串，判断字符串是否有效。

有效字符串需满足：

左括号必须用相同类型的右括号闭合。
左括号必须以正确的顺序闭合。
注意空字符串可被认为是有效字符串。

### 思考
如果是程序代码中，很容易想到， 该字符是成对出现，排成一行后，就是偶数个， 对称的，可以用这个特性来比较。但是还有一种情况，是这样的： "()[]{}" ，这样的组合也是正确的，这时候就要有不同的判断逻辑，可以将事情分为这两类，参考答案后发现作者巧妙的使用栈这个数据结构，左边的负号入栈，右边的符号就出栈对比，完美的解决这个问题。以下是代码：

```

public boolean isValid(String s) {

        if (s == null || s.length() == 0) {
            return true;
        }
        Stack<Character> stack = new Stack<>();

        char[] chars = s.toCharArray();
        for (char ch : chars) {
            if (ch == '(' || ch == '[' || ch == '{') {
                stack.push(ch);
            } else if (ch == ')' || ch == ']' || ch == '}') {
                if (stack.isEmpty()) {
                    return false;
                }
                char top = stack.peek();
                if ((top == '(' && ch == ')') || (top == '[' && ch == ']') || (top == '{' && ch == '}')) {
                    stack.pop();
                } else {
                    return false;
                }
            } else {
                return false;
            }
        }

        return stack.isEmpty();


    }
    
```