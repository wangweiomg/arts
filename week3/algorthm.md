## ARTS - Algorthm

### [9. 回文数](https://leetcode-cn.com/problems/palindrome-number/description/)
判断一个整数是否是回文数。回文数是指正序（从左向右）和倒序（从右向左）读都是一样的整数。

### 分析：
负数比如 -121， 从左到右是 -121, 从右到左是 121-， 所以不是回文数。
这道题算法和[7.反转整数](https://leetcode-cn.com/problems/reverse-integer/description/) 类似，反转整数是取得每一位倒置输出，而这个是取得每一位倒置后和原值进行比较。

代码如下：

```
public boolean isPalindrome(int x) {

        if (x < 0) {
            return false;
        }

        int y = 0;
        int z = x;
        while(x != 0) {

            y = x % 10 + y * 10;

            x /= 10;

        }
        return z == y;

    }
```
