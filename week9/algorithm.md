## ARTS - Algorithm
[28. 实现strStr()](https://leetcode-cn.com/problems/implement-strstr/description/)


实现 strStr() 函数。

给定一个 haystack 字符串和一个 needle 字符串，在 haystack 字符串中找出 needle 字符串出现的第一个位置 (从0开始)。如果不存在，则返回  -1。


### 思路
本人用Java 实现， 直接使用 indexOf 函数就可实现，但是这道题考察的就是indexOf实现，所以写了一下实现过程. 我的思路是 

遍历haystack, 如果碰到和 needle 第一位的字符相等，就进入子循环，遍历needle字符串，看剩下的haystack 是否和needle相等。代码如下：


```
public int strStr(String haystack, String needle) {

        if (haystack == null || needle == null) {
            return  -1;
        }

        if (needle.length() == 0) {
            return 0;
        }


        int len1 = haystack.length();
        int len2 = needle.length();

        for (int i = 0; i < len1; i++) {

            if (haystack.charAt(i) == needle.charAt(0)) {

                // 如果needle 就1位
                if (len2 == 1) {
                    return i;
                }

                // 如果haystack剩余没有needle长，返回 -1
                if (len1 - i < len2) {
                    return -1;
                }

					// 记录 两个串相等的个数
                int j = 1;
                for (int k = 1; k < needle.length(); k++) {

                    if (needle.charAt(k) != haystack.charAt(i + k)) {
                        break;
                    }

                    j++;

                }

                if (j == len2) {
                    return i;
                }

            }

        }



        return -1;
    }

```


### github 实现

我们来看下github 的实现：
https://github.com/haoel/leetcode/blob/master/algorithms/java/src/strStr/strStr.java

```

public int strStr(String haystack, String needle) {
        if (haystack == null || needle == null) {
            return -1;
        }
        int i, j = 0;
        for (i = 0; i < haystack.length() - needle.length() + 1; i++) {
            for (j = 0; j < needle.length(); j++) {
                if (haystack.charAt(i + j) != needle.charAt(j)) {
                    break;
                }
            }
            if (j == needle.length()) {
                return i;
            }
        }
        return -1;
    }

```


这样实现就简练一些， 避开了多余一步的 needle 和 haystack剩余长度的检查。

### JDK 实现
我们再来看看JDK 实现, 就是String的 indexOf方法

```

	public int indexOf(String str) {
	        return indexOf(str, 0);
	}

	public int indexOf(String str, int fromIndex) {
        return indexOf(value, 0, value.length,
                str.value, 0, str.value.length, fromIndex);
    }
    
    static int indexOf(char[] source, int sourceOffset, int sourceCount,
            char[] target, int targetOffset, int targetCount,
            int fromIndex) {
        if (fromIndex >= sourceCount) {
            return (targetCount == 0 ? sourceCount : -1);
        }
        if (fromIndex < 0) {
            fromIndex = 0;
        }
        if (targetCount == 0) {
            return fromIndex;
        }

        char first = target[targetOffset];
        int max = sourceOffset + (sourceCount - targetCount);

        for (int i = sourceOffset + fromIndex; i <= max; i++) {
            /* Look for first character. */
            if (source[i] != first) {
                while (++i <= max && source[i] != first);
            }

            /* Found first character, now look at the rest of v2 */
            if (i <= max) {
                int j = i + 1;
                int end = j + targetCount - 1;
                for (int k = targetOffset + 1; j < end && source[j]
                        == target[k]; j++, k++);

                if (j == end) {
                    /* Found whole string. */
                    return i - sourceOffset;
                }
            }
        }
        return -1;
    }


```

### JDK 实现strStr

JDK实现，使用了

```
if (haystack.charAt(i) != first) {
                while(++i <= max && haystack.charAt(i) != first);
            }
```
这个样就不用每次取元素对比，直到元素相等再比。


```

public int strStr4(String haystack, String needle) {

        if (haystack == null || needle == null) {
            return -1;
        }

        if (needle.length() == 0) {
            return 0;
        }

        int max = haystack.length() - needle.length();

        char first = needle.charAt(0);
        for (int i = 0; i <= max; i++) {

            if (haystack.charAt(i) != first) {
                while(++i <= max && haystack.charAt(i) != first);
            }

            /* Found first character, now look at the rest of v2 */
            if (i <= max) {
                int j = i + 1;
                int end = j + needle.length() - 1;
                for (int k = 1; j < end && haystack.charAt(j)
                        == needle.charAt(k); j++, k++);

                if (j == end) {
                    /* Found whole string. */
                    return i;
                }
            }

        }



        return -1;
    }
```