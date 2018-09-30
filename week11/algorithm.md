## ARTS - Algorithm
## [66. 加一](https://leetcode-cn.com/problems/plus-one/description/)

### 描述

给定一个由整数组成的非空数组所表示的非负整数，在该数的基础上加一。

最高位数字存放在数组的首位， 数组中每个元素只存储一个数字。

你可以假设除了整数 0 之外，这个整数不会以零开头。

示例 1:

> 输入: [1,2,3]
> 输出: [1,2,4]
> 解释: 输入数组表示数字 123。

示例 2:

> 输入: [4,3,2,1]
> 输出: [4,3,2,2]
> 解释: 输入数组表示数字 4321。


### 分析
这其实就是考虑数字加1，高位进位的问题，代码如下：

```

public int[] plusOne(int[] digits) {

        // 需要加1
        int plus = 1;
        for (int i = digits.length - 1; i >= 0; i--) {


            int tmp = digits[i] + plus;
            if (tmp >= 10) {

                digits[i] = tmp % 10;
                plus = 1;
            } else {
                digits[i] = tmp;
                plus = 0;
            }

        }
        if (plus == 1) {
            // 需要处理
            digits[0] = digits[0] % 10;
            int[] arr = new int[digits.length + 1];
            System.arraycopy(digits, 0, arr, 1, digits.length);
            arr[0] = 1;
            return arr;
        }


        return digits;


    }
    

```