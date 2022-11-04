## Algorithm - [3. 无重复字符的最长子串](https://leetcode.cn/problems/longest-substring-without-repeating-characters/description/)

给定一个字符串 `s` ，请你找出其中不含有重复字符的 **最长子串** 的长度。

 

**示例 1:**

```text
输入: s = "abcabcbb"
输出: 3 
解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。
```

### 分析

一开始毫无头绪，就从能想到的最简单的方法来实现，不考虑空间、时间，只考虑能够解决问题。

#### 方法，穷举

**最直接的思路**，既然要找出无重复字符最长子串，那么就把所有子串找出来，排除重复的，就能找到无重复的最长子串。

**如何获得子串？**String自带的```substr(beginIndex, endIndex)```方法。

**如何查找所有子串？**从第一个字符开始，往后取1个、2个、3个....组成子串， 然后再第二个字符... 

具体代码：

```java
public int lengthOfLongestSubstring(String s) {

        if (s == null || s.length() == 0) {
            return 0;
        }

  			// 包含所有子串的容器
        List<char[]> list = new ArrayList<>();
        // 遍历所有字符
        for (int i = 0; i < s.length(); i++) {

            char head = s.charAt(i);
            list.add(new char[]{head});

            // 找出所有子串
            for (int j = i+1; j < s.length(); j++) {
                list.add(s.substring(i, j + 1).toCharArray());
            }

        }

        // 遍历所有子串，找出无重复的长度
        int max = 0;
        for (int i = 0; i < list.size(); i++) {
            char[] array = list.get(i);

            // 如何检查重复， 类似取所有子串，这次是取所有字符
            boolean duplicate = false;
            label: for (int j = 0; j < array.length; j++) {

                char head = array[j];

                for (int k = j+1; k < array.length; k++) {

                    if (head == array[k]) {
                        // 重复，跳出检查
                        duplicate = true;
                        break label;

                    }

                }

            }

            if (!duplicate) {
                // 存在重复，就不管max
                // 不存在重复，就重置max
                max = Math.max(max, array.length);
            }


        }


        return max;
    }
```



自测没问题，提交leetcode，发现报错，超出内存限制，测试用例基本过了，说明功能基本没问题，下面解决好不好的问题。

![image-20221104102929529](https://tva1.sinaimg.cn/large/008vxvgGly1h7svzkxzg7j31km0mmqb7.jpg)



#### 优化代码

我们分析代码，找出优化空间。从最简单的地方入手，最容易改的地方修改。

考虑代码结构：  **遍历出**所有子串 -> **存起来** -> **遍历取出来**一个个判断 。  从这个角度看，判断重复的逻辑可以在第一次遍历所有子串的时候完成， 这样就不需要一个容易来存起来，我们改代码， 去掉中间容器list。

```java
public int lengthOfLongestSubstring(String s) {

        if (s == null || s.length() == 0) {
            return 0;
        }

        // max 初始化为1，因为为空已经判断过了
        int max = 1;

        // 找出所有子串
        for (int i = 0; i < s.length(); i++) {

            char head = s.charAt(i);

            // 找出所有子串,并直接判断重复
            for (int j = i+1; j < s.length(); j++) {
                char[] array = s.substring(i, j + 1).toCharArray();

                // 判断该array是否存在重复字符
                boolean duplicate = false;
                label: for (int k = 0; k < array.length; k++) {
                    char ch = array[k];
                    for (int l = k+1; l < array.length; l++) {
                        if (ch == array[l]) {
                            // 重复， 跳出比较
                            duplicate = true;
                            break label;
                        }

                    }
                }

                if (!duplicate) {
                    // 不重复就更新max
                    max = Math.max(max, array.length);
                }
            }

        }

        return max;

    }
```



自测通过，提交leetcode,发现还有问题。

![image-20221104111203836](https://tva1.sinaimg.cn/large/008vxvgGly1h7sx7vffxyj31hc09mmyu.jpg)

我们继续优化，这次重点是算法的改进。首先在判断里面有多重循环， 这个是耗时的重点，

关键逻辑步骤是，**取一个字符** -> 找出该字符的后续所有子串 转为数组 -> 判断数组是否重复 -> 更新max ..., 判断数组是否重复这里循环套循环，首先优化这一点。我们知道Java Set结构是不重复的，可以利用这一点来代替我们的是否重复判断，逻辑就是加入Set，有重复就是 set.size < array.length ， 改写代码如下：



#### 引入Set代替重复判断







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





