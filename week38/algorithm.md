## Algorithm - [3. 无重复字符的最长子串](https://leetcode.cn/problems/longest-substring-without-repeating-characters/description/)

给定一个字符串 `s` ，请你找出其中不含有重复字符的 **最长子串** 的长度。

 

**示例 1:**

```text
输入: s = "abcabcbb"
输出: 3 
解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。
```

### 分析

一开始毫无头绪，就从能想到的最简单的方法来实现，不考虑空间、时间，只考虑能够解决。

#### 方法1、穷举

既然要找出无重复字符最长子串，那么就把所有子串找出来，排除重复的，找到无重复的最长子串

#### 方法2、解决

```java
public int lengthOfLongestSubstring(String s) {

        if (s == null || s.length() == 0) {

            return 0;
        }

        int max = 1;


        HashSet<Character> set = new HashSet<>();

        for (int i = 0, len = s.length(); i < len; i++) {

            char ch = s.charAt(i);
            set.clear();
            set.add(ch);

            // 找出vision 开始无重复最长子串
            for (int j = i+1 ; j < s.length(); j++) {


                char tmp = s.charAt(j);
                set.add(tmp);

                int length = j - i + 1;

                if (set.size() < length) {
                    // 有重复的，
                    max = Math.max(max, length-1);
                    break;
                } else {
                    max = Math.max(max, length);
                }

            }


        }

        return max;

    }
```





