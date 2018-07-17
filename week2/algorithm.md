
## Algorithm

### leecode [7. 翻转整数](https://leetcode-cn.com/problems/reverse-integer/description/)
给定一个 32 位有符号整数，将整数中的数字进行反转。

### 分析
这道题把数字翻转，思路是使用模运算和直接除以10运算获得每一位数字，然后要考虑越界问题，比较笨的方式是转为字符数组，然后倒序排列后转为int，本人不才，使用了第二种。代码如下:

```

public int reverse(int x) {

        if (x == Integer.MIN_VALUE) {
            return 0;
        }

        boolean positive = x < 0;

        if (positive) {
            x = -x;
        }
        String n = String.valueOf(x);

        char[] chars = n.toCharArray();

        StringBuilder sb = new StringBuilder();
        for (int len = chars.length, i = len - 1; i >= 0; i--) {
            sb.append(chars[i]);
        }
        String result = sb.toString();

        try {
            if (positive) {

                return -Integer.parseInt(result);
            } else {
                return Integer.parseInt(result);
            }

        } catch (NumberFormatException e) {
            return 0;
        }

    }
```

当然，高效的代码就是模运算取每一位，然后循环乘以10，代码如下：

```
public int reverse(int x) {

        int y = 0;

        while (x != 0) {

            if (y > Integer.MAX_VALUE / 10 || y < Integer.MIN_VALUE / 10) {
                return 0;
            }

            y = y * 10 + x % 10;

            x = x / 10;
        }


        return y;

    }
```

