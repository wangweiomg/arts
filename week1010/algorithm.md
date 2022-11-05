## ARTS - Algorithm

[35. 搜索插入位置](https://leetcode-cn.com/problems/search-insert-position/description/)


给定一个排序数组和一个目标值，在数组中找到目标值，并返回其索引。如果目标值不存在于数组中，返回它将会被按顺序插入的位置。

你可以假设数组中无重复元素。

示例 1:

输入: [1,3,5,6], 5
输出: 2
示例 2:

输入: [1,3,5,6], 2
输出: 1

### 分析
这个题很容易想到遍历数组，找出最后一个比target小的元素，返回这个位置。代码如下：

```

class Solution {
    public int searchInsert(int[] nums, int target) {
    	if (nums == null) return 0;
    	
        for (int i = 0; i < nums.length; i++) {

            if (nums[i] >= target) {
                return i;
            }

        }

        return nums.length;

    }
}

```

### 改进
参考了答案后，知道查找这个位置时间复杂度是O(n), 我们可以优化查找过程，使用二分查找实现：

```

public static int searchInsert2(int[] nums, int target) {

        if (nums == null || nums.length == 0) {
            return 0;
        }

        int start = 0;
        int end = nums.length - 1;

		// 如果target 在nums 数组中，那么在循环里就能找到位置
        while (start + 1 < end) {
            int mid = start + (end - start) / 2;
            if (nums[mid] == target) {
                return mid;
            } else if (nums[mid] < target){
                start = mid;
            } else {
                end = mid;
            }
        }


		// target 不在nums数组中，比较获得位置
        if (nums[start] >= target) {
            return start;
        }

        if (nums[end] >= target) {
            return end;
        }

        return end + 1;


    }

```