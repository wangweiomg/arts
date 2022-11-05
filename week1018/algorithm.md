## ARTS - Algorithm 
## 补 11.5
## [88. 合并两个有序数组](https://leetcode-cn.com/problems/merge-sorted-array/description/)

### 题目

给定两个有序整数数组 nums1 和 nums2，将 nums2 合并到 nums1 中，使得 num1 成为一个有序数组。

说明:

初始化 nums1 和 nums2 的元素数量分别为 m 和 n。
你可以假设 nums1 有足够的空间（空间大小大于或等于 m + n）来保存 nums2 中的元素。
示例:

输入:

* nums1 = [1,2,3,0,0,0], m = 3
* nums2 = [2,5,6],       n = 3

输出: [1,2,2,3,5,6]


### 分析

首先想出解决方案，再优化解决方案。

这个问题可以先合并数组，再对数组排序，于是就有了如下代码：

```

public static void merge(int[] nums1, int m, int[] nums2, int n) {

        

        for (int i = m, j = 0; i < m + n; i++, j++) {
            nums1[i] = nums2[j];
        }

        for (int i = 0; i < m + n; i++) {
            for (int j = i + 1; j < m + n; j++) {
                if (nums1[i] > nums1[j]) {
                    int tmp = nums1[i];
                    nums1[i] = nums1[j];
                    nums1[j] = tmp;
                }

            }
        }

}
```

这种解法虽说能解决问题，但是肯定存在更好的解法，主要是没有利用上数组的有序特性。考虑另一种解法，遍历两个数组，从前往后，小的放前面，大的放后面，感觉也不太好，因为牵涉到数组整体后移的情况。

看了答案之后豁然开朗， 要点是从后往前填数据，因为从后往前填数据，数据的位置已经是确定的了，不会再移动，所以代码如下


### 代码

```
public static void merge3(int[] nums1, int m, int[] nums2, int n) {

        int ia = m - 1;
        int ib = n - 1;


        for (int i =  m + n - 1; i >= 0 ; i--) {

            if (ia >= 0 && ib < 0) {
                break;
            }

            if (ia < 0 && ib >= 0) {

                nums1[i] = nums2[ib--];
                continue;
            }

            if (nums1[ia] > nums2[ib]) {
                nums1[i] = nums1[ia--];
            } else {
                nums1[i] = nums2[ib--];
            }

        }


    }


```