---
title: LeetCode缺失的第一个正数
date: 2021-03-29 11:43:21
tags:
  - LeetCode
  - 数组
categories: [后端, 算法]
---

## LeetCode 缺失的第一个正数

### 题目描述

给你一个未排序的整数数组 nums，请你找出其中没有出现的最小的正整数。

进阶：你可以实现时间复杂度为 O(n)并且只使用常数级别额外空间的解决方案吗？

示例 1：

```Text
输入：nums = [1,2,0]
输出：3
```

示例 2：

```Text
输入：nums = [3,4,-1,1]
输出：2
```

示例 3：

```Text
输入：nums = [7,8,9,11,12]
输出：1
```

### Java 解法

```Java
/**
 * @author zhkai
 * @date 2021年3月29日13:51:23
 */
public class FirstMissingPositive {
    /**
     * 给你一个未排序的整数数组 nums，请你找出其中没有出现的最小的正整数
     *
     * @param nums 未排序的整数数组 nums
     * @return 没有出现的最小的正整数
     */
    public static int firstMissingPositive(int[] nums) {
        int numsLen = nums.length;
        if (numsLen == 0) {
            return 1;
        }
        int[] res = new int[numsLen + 1];
        int resLen = res.length;
        for (int x : nums) {
            if (x > 0 && x < resLen) {
                res[x] = x;
            }
        }
        for (int i = 1; i < resLen; i++) {
            if (i != res[i]) {
                return i;
            }
        }
        return resLen;
    }

    /**
     * 给你一个未排序的整数数组 nums，请你找出其中没有出现的最小的正整数
     *
     * @param nums 未排序的整数数组 nums
     * @return 没有出现的最小的正整数
     */
    public static int firstMissingPositiveTwo(int[] nums) {
        int numsLen = nums.length;
        if (numsLen == 0) {
            return 1;
        }
        for (int i = 0; i < numsLen; i++) {
            while (nums[i] > 0 && nums[i] < numsLen + 1 && nums[i] != i + 1 && nums[i] != nums[nums[i] - 1]) {
                swap(nums, i, nums[i] - 1);
            }
        }
        for (int i = 0; i < numsLen; i++) {
            if (nums[i] != i + 1) {
                return i + 1;
            }
        }
        return numsLen + 1;
    }

    /**
     * 交换数组元素位置
     *
     * @param nums 未排序的整数数组 nums
     * @param i 需交换元素数组index
     * @param j 与需交换元素进行交换的数组index
     */
    public static void swap(int[] nums, int i, int j) {
        if (i != j) {
            nums[j] ^= nums[j];
            nums[j] ^= nums[i];
            nums[i] ^= nums[j];
        }
    }
}

```

#### Java 解法效率对比

```Text
输入：nums = {1, 3, 6, 7, 9};
方法一：2708900ns
方法二：15400ns
```

### Python 解法

```Python
from typing import List


def first_missing_positive(nums: List[int]) -> int:
    """
    给你一个未排序的整数数组 nums，请你找出其中没有出现的最小的正整数
    :param nums: 未排序的整数数组
    :return: 没有出现的最小的正整数
    """
    n = len(nums)
    res = [0 for i in range(n + 1)]
    for x in nums:
        if 0 < x < len(res):
            res[x] = x
    for i in range(len(res)):
        if res[i] != i:
            return i
    return len(res)


def first_missing_positive_two(nums: List[int]) -> int:
    """
    给你一个未排序的整数数组 nums，请你找出其中没有出现的最小的正整数
    :param nums: 未排序的整数数组
    :return: 没有出现的最小的正整数
    """
    n = len(nums)
    for i in range(n):
        if 0 < nums[i] < n + 1 and nums[1] != i + \
                1 and nums[i] != nums[nums[i] - 1]:
            swap(nums, i, nums[i] - 1)
    for i in range(n):
        if nums[i] != i + 1:
            return i + 1
    return n + 1


def swap(nums: List[int], i: int, j: int):
    if i != j:
        nums[i] ^= nums[j]
        nums[j] ^= nums[i]
        nums[i] ^= nums[j]

```

#### Python 解法效率对比

```Text
输入：nums = {1, 3, 6, 7, 9};
方法一：13900ns
方法二：17100ns
```
