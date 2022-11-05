## ARTS - Algorithm
## [67. 二进制求和](https://leetcode-cn.com/problems/add-binary/description/)
### 题目
给定两个二进制字符串，返回他们的和（用二进制表示）。

输入为非空字符串且只包含数字 1 和 0。

示例 1:

输入: a = "11", b = "1"
输出: "100"
示例 2:

输入: a = "1010", b = "1011"
输出: "10101"

### 分析
很容易想到，倒序遍历，取得两个数字相加，取得和后放入栈结构，最后取出，注意进位和最最高位进位问题

代码如下：

```
public static String addBinary(String a, String b) {

        if (a == null || a.length() == 0) {
            return b;
        }
        if (b == null || b.length() == 0) {
            return a;
        }

        int i = a.length() - 1;
        int j = b.length() - 1;


        Stack<Integer> stack = new Stack();
        int max = Math.max(i, j);


        boolean add = false;
        int x, y = 0;
        for (int k = max; k >= 0; k--, i--, j--) {

            if (i < 0) {
                x = 0;
            } else {

                x = Integer.parseInt(String.valueOf(a.charAt(i)));
            }

            if (j < 0) {

                y = 0;
            } else {
                y = Integer.parseInt(String.valueOf(b.charAt(j)));

            }


            int r = x + y;

            if (add) {
                r++;
            }
            if (r > 1) {
                add = true;
            } else {
                add = false;
            }
            stack.push(r % 2);

        }


        if (add) {
            stack.push(1);
        }

        StringBuilder sb = new StringBuilder();
        while (!stack.isEmpty()) {
            sb.append(stack.pop());
        }

        return sb.toString();

    }

```

