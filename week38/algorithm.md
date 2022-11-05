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

注意点是，第二层循环里， 有多少个元素 = (j - i + 1)

代码如下：

```java
public int lengthOfLongestSubstring1(String s) {

        if (s == null || s.length() == 0) {
            return 0;
        }

        // max 初始化为1，因为为空已经判断过了
        int max = 1;


        Set<Character> set = new HashSet<>();

        // 找出所有子串
        for (int i = 0; i < s.length(); i++) {

            // 找出所有子串,并直接判断重复
            set.add(s.charAt(i));

            for (int j = i+1; j < s.length(); j++) {

                set.add(s.charAt(j));

                // 判断是否重复
                // 如果不重复，那么应该有 (j-i+1) 个元素
                int num = j - i + 1;
                if (set.size() < num) {

                    // 重复，清空set, 跳出循环
                    set.clear();

                    break;
                } else {
                  	// 重置max
                    max = Math.max(max, num);
                }

            }

        }

        return max;

    }
```

再次提交试试，通过，但是还是不够好, 有很大优化空间。

![image-20221104202851953](https://tva1.sinaimg.cn/large/008vxvgGly1h7tdb7t05tj31jc0bamy6.jpg)



#### 去掉双重循环

继续优化代码。重新思考这个问题， 题目要求：

>  给定一个字符串 `s` ，请你找出其中不含有重复字符的 **最长子串** 的长度。

假设是如图所示的字符串，要找到无重复最长子串。我们很容易看出来，最长子串是 ```abc``` ,  长度是3。在脑海中过一下执行过程：

1. 拿出a ，结果```a``` ， 无重复
2. 拿出b ,   结果```ab``` ，无重复
3. 拿出c, 结果 ```abc``` ，无重复
4. 拿出a, 结果 ```abca```， 重复，去掉最后的```a```,  当前最长是 ```abc```， 
5. **(关键步骤)**  这时候，我们是返回去，从 下标为1的```b``` 开始下一轮判断，还是往后从下标为3的a开始判断？很明显是后者。

![image-20221104204715144](https://tva1.sinaimg.cn/large/008vxvgGly1h7tduc7ghkj318m0k4q3u.jpg)

想清楚了以上步骤， 回顾我们的代码： 很明显， 我们在第5步骤，应该从下标为3 的a开始，但是却从了 下标为1开始判断，这块是重复无用的，所以需要改造这一块。思路就是， 当右边发现有重复时候， 把左边的指针指向右边重复位置。

```java
for (int i = 0; i < s.length(); i++) {

            // 找出所有子串,并直接判断重复
            set.add(s.charAt(i));

            for (int j = i+1; j < s.length(); j++) {

                set.add(s.charAt(j));

                // 判断是否重复
                // 如果不重复，那么应该有 (j-i+1) 个元素
                int num = j - i + 1;
                if (set.size() < num) {

                    // 重复，清空set, 跳出循环
                    set.clear();

                    break;
                } else {
                    max = Math.max(max, num);
                }

            }

        }

```



根据以上思路，改造后的代码主要逻辑为：

```java
				int left = 0;
        // 找出所有子串
        for (int i = 0; i < s.length(); i++) {

            char ch = s.charAt(i);
            set.add(ch);

            // 无重复的话，长度为 i - left + 1
            int num = i - left + 1;
            if (set.size() < num) {
                // 有重复的, 记录当前位置
                left = i;
                set.clear();
                set.add(ch);

            } else {
                // 无重复，更新max
                max = Math.max(max, num);
            }



        }

        return max;
```



自测通过，提交测试， 发现用例没有通过：

![image-20221104211850962](https://tva1.sinaimg.cn/large/008vxvgGly1h7ter8dyujj30ku0iy0t7.jpg)

是的，在该案例中， ```dvdf```， 我们看出最长子串是```vdf```， 根据上面代码， 执行的结果是```df```， 也就是说，遇到重复后，并不能直接从重复除开始往后找，重复前面的字符开头也可能存在最长子串。回到刚才改造之前的思路，看看问题出在哪？

> 想清楚了以上步骤， 回顾我们的代码： 很明显， 我们在第5步骤，应该从下标为3 的a开始，但是却从了 下标为1开始判断，这块是重复无用的，所以需要改造这一块。思路就是， 当右边发现有重复时候， 把左边的指针指向右边重复位置。

只在上面那个例子```abcabcbb```中， 这么想确实没问题，但是并没有兼容 ```dvdf```这样的情况， 重复字符前面开头的子串还可能存在最长的，再结合我们第一版的暴力算法--- 找到所有子串，  发现，**其实这问题的解题关键，就是要找到每一个字符领头下的，无重复最长子串！**

也就是说，要实现的是这样的过程：

**对于abcabcbb**

1. a领头： 最长是 abc
2. b领头：最长是bca
3. c领头：最长是cab
4. a领头:  最长是 abc
5. b领头：最长是bc
6. c领头: 最长是cb
7. b领头: 最长是b
8. b领头: 最长是b

**对于 dvdf**

1. d领头：最长是dv
2. v领头：最长是vdf
3. d领头：最长是df
4. f领头：最长是f

这时候就引入了一个计算机概念，叫**滑动窗口算法(Sliding Window Algorithm)**， 主要用于数组和字符串处理，逻辑如图所示：

![image-20221104214542954](https://tva1.sinaimg.cn/large/008vxvgGly1h7tfj6b64zj31lc0t4gqc.jpg)

相当于创建一个滑动的窗口，或者队列管道， 碰到重复的，就把前面重复的去掉。 注意上图第 step4 ,碰到 b重复，并不是只去一个， 而是直接去掉了ab, 因此这里我们要知道重复字符所在的位置，方便窗口的左边界指针移动过来，指向之前重复的后一位。既要记录位置，也要判断重复，Java里的合适的数据结构，就是map了，用map的key的无重复特性处理重复， value保存字符的位置，挺合适。

这时候的主要逻辑，就是移动左指针了。如果右边指针(for 循环的 i) 碰到了重复的，就把左边指针往右挪一位，左边指针的位置在map中有保存，取出即可。 **注意，由于碰到所有的重复字符都会操作左指针的位置， 防止出现左指针往左偏移，每次就要取最大位置的左指针**。



#### 最终代码

```java
public int lengthOfLongestSubstring(String s) {

        if (s == null || s.length() == 0) {
            return 0;
        }

        Map<Character, Integer> map = new HashMap<>();
        int max = 0;
        // 左边界指针
        int left = 0;

        for (int i = 0; i < s.length(); i++) {

            char ch = s.charAt(i);
            if (map.containsKey(ch)) {
                // 如果重复，就移动左指针
                left = Math.max(left, map.get(ch) + 1);
            }

            max = Math.max(max, i - left + 1);

            // 保存字符
            map.put(ch, i);

        }

        return max;
    }
```



看执行性能和内存占用：

![image-20221105154345557](http://rkv59lj1r.hb-bkt.clouddn.com/img/20221105154346.png)



还算可以。
