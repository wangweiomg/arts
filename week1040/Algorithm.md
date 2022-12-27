## Algorithm - [169. 多数元素](https://leetcode.cn/problems/majority-element/)

169. 多数元素

给定一个大小为 `n` 的数组 `nums` ，返回其中的多数元素。多数元素是指在数组中出现次数 **大于** `⌊ n/2 ⌋` 的元素。

你可以假设数组是非空的，并且给定的数组总是存在多数元素。

**示例 1：**

```
输入：nums = [3,2,3]
输出：3
```

**示例 2：**

```
输入：nums = [2,2,1,1,1,2,2]
输出：2
```



### 分析

老规矩，先从想到的最笨的的方法解决。 只要把数组中每个元素放入map里， value作为次数，就能找出多数元素。

代码如下：

```java
class Solution {
    public int majorityElement(int[] nums) {
				if (nums.length == 1) {
            return nums[0];
        }

        Map<Integer, Integer> map = new HashMap<>(nums.length/2+1);

        for (int num : nums) {
            if (map.get(num) == null) {
                map.put(num, 1);
            } else {

                int cnt = map.get(num);
                map.put(num, cnt + 1);

                if (cnt + 1 > nums.length/2) {
                    return num;
                }
            }
        }



        return 0;
    }
}
```

结果是通过的, 但是用时和空间都耗费多，**进阶：**尝试设计时间复杂度为 O(n)、空间复杂度为 O(1) 的算法解决此问题。

进阶要求， 设计时间复杂度O(n)、空间复杂度O(1) 的算法。也就是说一次遍历，不引入容器或容器大小固定来解决问题。

加入元素有序，那么只用取n/2 +1位置的元素，就是对的值。

```java
class Solution {
    public int majorityElement(int[] nums) {
				if (nums.length == 1) {
            return nums[0];
        }

        Arrays.sort(nums);


        return nums[nums.length/2];
    }
}
```



看题解，一个比较巧妙的解决方式是，选举法。

我们假设这样的场景，每个元素都认为自己能竞选成功，被推上台时，支持的票(相同元素)加1， 反对的票(不同的元素)减一 ， 当票数为0，重新选举。我们知道，众数最后一定能成功。

代码如下：

```java
public int majorityElement(int[] nums) {
        // 候选人得票数
        int count = 0;
        // 候选人
        int candidate = 0;

        for (int num : nums) {

          	// 当票数为0， 选出一个候选人
            if (count == 0) {
                candidate = num;
            }
						// 是候选人的话，票数+1，否则-1		
 	     	    count += (candidate == num) ? 1 : -1;

        }

        return candidate;

    }
```



这种构思真是巧妙。

