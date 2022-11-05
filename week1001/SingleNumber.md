## ARTS

### Algorithm

[136. 只出现一次的数字](https://leetcode-cn.com/problems/single-number/description/)
### 题目：
给定一个非空整数数组，除了某个元素只出现一次以外，其余每个元素均出现两次。找出那个只出现了一次的元素。

说明：

你的算法应该具有线性时间复杂度。 你可以不使用额外空间来实现吗？

```
class Solution {
    public int singleNumber(int[] nums) {
        int index = 0;

        for (int i = 0; i < nums.length; i++) {
            index = index ^ nums[i];
        }

        return index;
    }
}

```
###分析：
此题目主要考察位运算， 其中 异或 ^ 操作符与相同的数字异或为0， 0与任何数异或是它本身的特性。
如果没有考虑到这一点，实现方法就是嵌套遍历出所有重复的，然后找出不重复的那个，这种方式很差。代码如下，

```
private static int singleNumber(int[] a) {

        List<Integer> list = Lists.newArrayListWithCapacity(a.length);

        int x = 0;
        for (int i = 0; i < a.length - 1; i++) {
            for (int j = i + 1; j < a.length; j++) {

                if (a[i] == a[j]) {

                    list.add(i);
                    list.add(j);
                    break;
                }
            }

        }
        for (int i = 0; i < a.length; i++) {
            if (list.contains(i)) {
                continue;
            }
            x = a[i];
        }

        return x;
    }

```