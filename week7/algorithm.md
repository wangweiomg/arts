## ARTS - Algorithm
## [26. 删除排序数组中的重复项](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-array/description/)

给定一个排序数组，你需要在原地删除重复出现的元素，使得每个元素只出现一次，返回移除后数组的新长度。

不要使用额外的数组空间，你必须在原地修改输入数组并在使用 O(1) 额外空间的条件下完成。

### 示例 1:

给定数组 nums = [1,1,2], 

函数应该返回新的长度 2, 并且原数组 nums 的前两个元素被修改为 1, 2。 

你不需要考虑数组中超出新长度后面的元素。
### 示例 2:

给定 nums = [0,0,1,1,1,2,2,3,3,4],

函数应该返回新的长度 5, 并且原数组 nums 的前五个元素被修改为 0, 1, 2, 3, 4。

你不需要考虑数组中超出新长度后面的元素。

### 分析
考虑用两个指示器， i,j ， 分别初始化为 0, 1， 如果 i,j 相等，就把 j 之后的元素都往前挪一位，最后一位 设置为数组第一位元素 -1  ，实现如下：

```
public int removeDuplicates(int[] nums) {

        int i = 0, j = 1;
        int len = nums.length;

        while (j < nums.length ) {

            int a = nums[i];
            int b = nums[j];

            if (a > b) {
                // 到头了
                len = j;
                break;
            }


            if (a == b) {
                // 把 b 挪到最后
                for (int k = j + 1; k < nums.length; k++) {
                    nums[k - 1] = nums[k];
                }

                // 最后一位是b
                nums[nums.length - 1] = nums[0] - 1;

            } else {
                i++;
                j++;
            }

        }
       
        return len;
    }

```

### 优化
在参考了别人实现后，发现可以继续优化， 只需要遍历一次，同样是设置一个 指示器，该指示器从0开始，
遍历从 1 开始，碰到和指示器位置不同的元素，就把该元素放到指示器位置后边， 指示器向前移动一位，直到遍历结束, 返回指示器+1。代码如下：

```
public int removeDuplicates2(int[] nums) {
        if (nums == null || nums.length == 0) {
            return 0;
        }
        int len = 0;
        for (int i = 1; i < nums.length; i++) {

            if (nums[i] != nums[len]) {
                nums[len + 1] = nums[i];
                len++;
            }

        }


               
        return len + 1;
    }

```


