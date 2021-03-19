## ARTS - Algorithm

### [6. Z 字形变换](https://leetcode-cn.com/problems/zigzag-conversion/)

### 题目

将一个给定字符串根据给定的行数，以从上往下、从左到右进行 Z 字形排列。

比如输入字符串为 "LEETCODEISHIRING" 行数为 3 时，排列如下：

L   C   I   R
E T O E S I I G
E   D   H   N
之后，你的输出需要从左往右逐行读取，产生出一个新的字符串，比如："LCIRETOESIIGEDHN"。

请你实现这个将字符串进行指定行数变换的函数：

string convert(string s, int numRows);
示例 1:

输入: s = "LEETCODEISHIRING", numRows = 3
输出: "LCIRETOESIIGEDHN"s
示例 2:

输入: s = "LEETCODEISHIRING", numRows = 4
输出: "LDREOEIIECIHNTSG"
解释:

L     D     R
E   O E   I I
E C   I H   N
T     S     G



### 分析

字符串字符从上到下从左到右之字型排列，其实就是类似回文数作为行数顺序  0 1 2 3 2 1 0 12 3 2 1 0 ... ， 

然后存下每一行的数据，最后按行拼接字符串。

如何生成回文数？类似如下算法：

```java
/**
     * 生成zigzag 数字，
     * @param max 最大值
     */
    private static void generateZigzag(int max) {

        // flag == true ++,  flag == false --
        // 标记， 如果到大边界就开始反转，到小边界也要反转
        boolean flag = true;
      	// 最小边界记号
        int low = 0;
      	// 最大边界记号
        int index = 0;
        for (int i = 0; i < 3*max; i++) {

            System.out.print(index + " ");
            if (index == max) {
                flag = false;
            }
            if (index == low) {
                flag = true;
            }
          	if (low == max) {
              // 如果最大最小边界相等，就是只有一行
              continue;
            }

            if (flag) {
                index ++;
            } else {
                index --;
            }


        }


    }
```

有了这个算法，然后把每一行当做一个List，来存取数据就可以了，整体代码如下：

```java
public String convert(String s, int numRows) {
        List<List<String>> result = new ArrayList<>();

        // 每一行都加一个list
        for (int i = 0; i < numRows; i++) {
            result.add(new ArrayList<>());
        }

        // 然后拆分字符串，

        int min = 0;
        int max = numRows - 1;
        int index = 0;
        boolean flag = true;
        for (int i = 0; i < s.length(); i++) {
            char ch = s.charAt(i);
            result.get(index).add(String.valueOf(ch));

            if (index == max) {
                flag = false;
            }

            if (index == min) {
                flag = true;
            }

            if (min == max) {
                continue;
            }


            if (flag) {
                index++;
            } else {
                index--;
            }

        }

        StringBuilder sb = new StringBuilder();
        result.forEach(i -> i.forEach(j -> sb.append(j)));

        return sb.toString();
    }
```



虽然以上算法解决了这个问题，但是还是有优化空间的，比如，我们其实实际只需要找出各个字母的行数就行，然后根据行数取字符，无需增加中间容器来保存。所以，问题就变成了，位置和行数的关系。

【代码未实现】



