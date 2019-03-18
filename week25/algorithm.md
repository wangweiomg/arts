## ARTS - Algorithm 补12.24
## [205. 同构字符串](https://leetcode-cn.com/problems/isomorphic-strings/)

### 题目

给定两个字符串 s 和 t，判断它们是否是同构的。

如果 s 中的字符可以被替换得到 t ，那么这两个字符串是同构的。

所有出现的字符都必须用另一个字符替换，同时保留字符的顺序。两个字符不能映射到同一个字符上，但字符可以映射自己本身。

示例 1:

输入: s = "egg", t = "add"
输出: true
示例 2:

输入: s = "foo", t = "bar"
输出: false
示例 3:

输入: s = "paper", t = "title"
输出: true
说明:
你可以假设 s 和 t 具有相同的长度。

### 分析
找相同结构的字符串，那么，可以把字符串转换为数字，比如第一个1开始，如果和之前的字符有重复，就设置为相同的数字，如果不是，就把前一个加1.

那么代码如下：

```
public static boolean isIsomorphic(String s, String t) {

        if (s.length() != t.length()) {
            return false;
        }


        String r1 = convert(s);
        String r2 = convert(t);

        return Objects.equals(r1, r2);


    }

    private static String convert(String s) {

        int idx = 1;
        StringBuilder sb = new StringBuilder();
        sb.append(idx);



        for (int i = 1; i < s.length(); i++) {

            char ch1 = s.charAt(i);
            boolean flag = false;
            for (int j = 0; j < i; j++) {
                char ch2 = s.charAt(j);
                if (ch2 == ch1) {
                    flag = true;
                    sb.append(sb.charAt(j));

                    break;
                }

            }

            if (!flag) {
                sb.append(++idx);
            }

        }

        return sb.toString();
    }
```


这个解法能通过，但是不是最优解法，耗费内存多且效率都不高，我们想办法优化这个。

### 代码
以下使用了codepoint来进行比较的。


```
public boolean isIsomorphic(String s, String t) {

        if (s.length() != t.length()) {
            return false;
        }

        int MAXCHAR = 256;
        char maps[] = new char[MAXCHAR];
        char mapt[] = new char[MAXCHAR];

        for (int i = 0; i < s.length(); i++) {
            if (maps[s.charAt(i)] == 0 && mapt[t.charAt(i)] == 0) {
                maps[s.charAt(i)] = t.charAt(i);
                mapt[t.charAt(i)] = s.charAt(i);
                continue;
            }

            if (maps[s.charAt(i)] == t.charAt(i) && mapt[t.charAt(i)] == s.charAt(i)) {
                continue;
            }
            return false;
        }

        return true;
    }
```