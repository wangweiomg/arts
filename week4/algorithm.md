## ARTS - Algorithm
[14. 最长公共前缀](https://leetcode-cn.com/problems/longest-common-prefix/description/)

编写一个函数来查找字符串数组中的最长公共前缀。

如果不存在公共前缀，返回空字符串 ""。

示例 1:

输入: ["flower","flow","flight"]
输出: "fl"
示例 2:

输入: ["dog","racecar","car"]
输出: ""
解释: 输入不存在公共前缀。
说明:

所有输入只包含小写字母 a-z 。


### 分析
定义一个指针， 从第一个开始，遍历所有字符串是否相等，遇到不相等的就停止，取得指针经过的一段字符串。

```

public String longestCommonPrefix(String[] strs) {
        if (strs.length == 0) {
            return "";
        }
        if (strs.length == 1) {
            return strs[0];
        }

        int point = -1;

        loop:
        for (int i = 0; i < strs[0].length(); i++) {
            boolean flag = true;
            for (int j = 0; j < strs.length; j++) {

                if (point + 1 > strs[j].length() - 1) {
                    break loop;
                }

                char c = strs[0].charAt(point + 1);

                flag = c == strs[j].charAt(point + 1);
                if (!flag) {
                    break loop;
                }
            }
            point++;
        }


        return strs[0].substring(0, point + 1);

    
    }
    

``` 