## ARTS - Algorithm 补2019.3.13

## [5. 最长回文子串](https://leetcode-cn.com/problems/longest-palindromic-substring/)

### 题目

给定一个字符串 s，找到 s 中最长的回文子串。你可以假设 s 的最大长度为 1000。

示例 1：

输入: "babad"
输出: "bab"
注意: "aba" 也是一个有效答案。
示例 2：

输入: "cbbd"
输出: "bb"

### 分析

这个回文子串，要分两步，1.分割，2.判断。判断的逻辑可以是，从两边往中间挤压，只要不相等，就不是，判断代码如下：

```java
private static boolean isPalindrome(String subString) {
        boolean isPalindrom = true;
        int length = subString.length();
        for (int i = 0; i < length; i++) {
            char si = subString.charAt(i);
            char sj = subString.charAt(length - i - 1);
            if (si != sj) {
                return false;
            }
        }
        return isPalindrom;

    }
```

那么，分割可以是迭代从第1位开始，每次都比较到最后一位子串：

```java
public static String longestPalindrome(String s) {

        int lenth = s.length();
        int max = 0;
        String result = "";
        for (int i = 0; i < lenth; i++) {
            for (int j = i; j < lenth; j++) {
                String subString = s.substring(i, j + 1);
                boolean is_palindrom = isPalindrome(subString);
                if (is_palindrom) {
                    if (subString.length() >= max) {
                        result = subString;
                    }
                    max = Math.max(max, subString.length());

                }
            }

        }
        return result;

    }
```

这个确实可以求到结果，但是提交leetcode超时，仔细分析这个过程，发现时间复杂度是大于O(n2)的。那么有没有一个更好的方法？ 当然有，对于每一位，可以向两边扩散，代码如下：



### 代码

```java
 public static String longestPalindrome5(String s) {

        if (s == null || s.length() < 2) {
            return s;
        }

        // 对称，和顶点
        int max = 0;
        int left = 0;
        int right = 0;
        for (int i = 0; i < s.length(); i++) {
            // 顶点
            int a = i - 1;
            int b = i + 1;

            int m = 1;
            while (a > -1 && b < s.length() && s.charAt(a) == s.charAt(b)) {

                m += 2;

                a--;
                b++;

            }

            if (m > max) {
                max = m;
                left = a + 1;
                right = b - 1;
            }

            // 对称
            a = i;
            b = i +1;
            m = 0;
            while (a > -1 && b < s.length() && s.charAt(a) == s.charAt(b)) {

                m += 2;
                a--;
                b++;
            }

            if (m > max) {
                max = m;
                left = a + 1;
                right = b - 1;
            }


        }


        return s.substring(left, right + 1);

    }
```

