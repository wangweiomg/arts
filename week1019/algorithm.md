## ARTS - Algorithm
## 补 11.12
## [4.寻找两个有序数组的中位数](https://leetcode-cn.com/problems/median-of-two-sorted-arrays/description/)

### 题目
给定两个大小为 m 和 n 的有序数组 nums1 和 nums2。

请你找出这两个有序数组的中位数，并且要求算法的时间复杂度为 O(log(m + n))。

你可以假设 nums1 和 nums2 不会同时为空。

示例 1:

nums1 = [1, 3]
nums2 = [2]

则中位数是 2.0

示例 2:

nums1 = [1, 2]
nums2 = [3, 4]

则中位数是 (2 + 3)/2 = 2.5

### 分析
由于和 [88. 合并两个有序数组](https://leetcode-cn.com/problems/merge-sorted-array/description/) 有相似之处，可以直接借鉴下解法。

可以认为是合并有序数组，找中位数，那么可以倒着把两个数组放入新数组，不用全放，到中位数哪里就可以了，于是代码如下：

```
class Solution {
    public double findMedianSortedArrays(int[] nums1, int[] nums2) {
        int m = nums1.length;
        int n = nums2.length;


        int len = (m + n) / 2;

        int[] ints = new int[len + 1];
        int ia = m - 1;
        int ib = n - 1;

        for (int i = len; i >= 0; i--) {

            if (ia >= 0 && ib < 0) {
                ints[i] = nums1[ia--];
                continue;

            }

            if (ia < 0 && ib >= 0) {
                ints[i] = nums2[ib--];
                continue;
            }

            if (ia >= 0 && ib >= 0) {
                if (nums1[ia] > nums2[ib]) {
                    ints[i] = nums1[ia--];

                } else {
                    ints[i] = nums2[ib--];
                }
            }

        }

        double res = 0;
        if ((m + n) % 2 == 0) {
            res = (ints[0] + ints[1]) * 1.0 /2;
        } else {
            res = ints[0];
        }
        return res;
    }
}
```

但是，**要求算法的时间复杂度为 O(log(m + n))**， 所以这种写法是不行的。 看了答案后，觉得这种道题还是挺有难度的，使用了复合的二分查找法，具体思路就是从寻找到 在 num1 和num2中都是中位数的元素，那么合并后也是中位数，具体代码如下：

### 代码

```
public static double findMedianSortedArrays3(int[] nums1, int[] nums2) {
        int m = nums1.length;
        int n = nums2.length;
        if (m == 0 && n == 0) {
            return 0;
        }

        if (m == 0) {
            return n % 2 == 1 ? nums2[n / 2] : (nums2[n / 2 - 1] + nums2[n / 2]) / 2.0;
        }

        if (n == 0) {
            return m % 2 == 1 ? nums1[m / 2] : (nums1[m / 2 - 1] + nums1[m / 2]) / 2.0;
        }

        if (m > n) {
            return findMedianSortedArrayHelper(nums1, nums2);
        }

        return findMedianSortedArrayHelper(nums2, nums1);

    }

    private static double findMedianSortedArrayHelper(int[] nums1, int[] nums2) {
        int m = nums1.length;
        int n = nums2.length;
        int low1 = 0;
        int high1 = m - 1;
        int low2 = 0;
        int high2 = n - 1;

        int mid = low1 + (high1 - low1) / 2;
        int pos = binarySearch(nums2, nums1[mid]);
        int num = mid + pos;
        // 如果 nums1[mid] 在 num2 中，
        if (num == (m + n) / 2) {
            if ((m + n) % 2 == 1) {
                return nums1[mid];
            }

            int next = 0;
            if ((mid > 0 && pos > 0)) {
                next = nums1[mid - 1] > nums2[pos - 1] ? nums1[mid - 1] : nums2[pos - 1];
            } else if (pos > 0) {
                next = nums2[pos - 1];
            } else if (mid > 0) {
                next = nums1[mid - 1];
            }
            return (nums1[mid] + next) / 2.0;

        }

        if (num < (m + n) / 2) {
            low1 = mid + 1;
            low2 = pos;
            if (high1 - low1 > high2 - low2) {
                return findMedianSortedArrayHelper(nums1, nums2);
            }
            return findMedianSortedArrayHelper(nums2, nums1);
        }

        if (num > (m + n) / 2) {
            high1 = mid - 1;
            high2 = pos - 1;
            if (high1 - low1 > high2 - low2) {
                return findMedianSortedArrayHelper(nums1, nums2);
            }
            return findMedianSortedArrayHelper(nums2, nums1);
        }


        return 0;
    }
```
