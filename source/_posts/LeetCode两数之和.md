---
title: LeetCode两数之和
date: 2021-03-23 14:09:47
tags:
  - LeetCode
  - 数组
categories: [后端, 算法]
---

## LeetCode 两数之和

### 题目描述

给定一个整数数组 nums 和一个目标值 target，请你在该数组中找出和为目标值的那两个整数，并返回他们的数组下标。
你可以假设每种输入只会对应一个答案。但是，数组中同一个元素不能使用两遍。

示例：

```Text
给定nums = [2,7,11,15],target=9
因为nums[0] + nums[1] = 2 + 7 = 9 所以返回[0, 1]
```

### Java 解法

```Java
import java.util.HashMap;

/**
 * @author zhkai
 * @date 2021年3月23日14:27:28
 */
public class TwoNumSum {
    private final static int NUM_NUMS = 2;

    /**
     * 给定一个整数数组 nums 和一个目标值 target，请你在该数组中找出和为目标值的那两个整数，并返回他们的数组下标
     *
     * @param nums   整数数组
     * @param target 目标值
     * @return 数组中和为目标值的两个整数的数组下标
     */
    public static int[] twoNumSum(int[] nums, int target) {
        int len = nums.length;
        if (len < NUM_NUMS) {
            return new int[0];
        }
        int[] result = new int[2];
        for (int i = 0; i < len; i++) {
            for (int j = i + 1; j < len; j++) {
                if (nums[i] + nums[j] == target) {
                    result[0] = i;
                    result[1] = j;
                }
            }
        }
        return result;
    }

    /**
     * 给定一个整数数组 nums 和一个目标值 target，请你在该数组中找出和为目标值的那两个整数，并返回他们的数组下标
     *
     * @param nums   整数数组
     * @param target 目标值
     * @return 数组中和为目标值的两个整数的数组下标
     */
    public static int[] twoNumSumMap(int[] nums, int target) {
        HashMap<Integer, Integer> map = new HashMap<Integer, Integer>(16);
        int len = nums.length;
        for (int i = 0; i < len; i++) {
            if (map.containsKey(target - nums[i])) {
                return new int[]{map.get(target - nums[i]), i};
            }
            map.put(nums[i], i);
        }
        return new int[0];
    }
}

```

#### Java解法效率对比

```Text
输入：nums = {1, 3, 5, 7, 9, 12, 13, 19, 20};target = 23;
方法一：5074400ns
方法二：264300ns
```

### Python 解法

```Python
from typing import List


def two_sum_dict(nums: List[int], target: int) -> List[int]:
    """
   给定一个整数数组 nums 和一个目标值 target，请你在该数组中找出和为目标值的那两个整数，并返回他们的数组下标
   :param nums: 整数数组
   :param target: 目标值
   :return: 数组中和为目标值的两个整数的数组下标
   """
    dict_item = dict()
    for i, num in enumerate(nums):
        if target - num in dict_item:
            return [dict_item[target - num], i]
    return []


def two_sum(nums: List[int], target: int) -> List[int]:
    """
    给定一个整数数组 nums 和一个目标值 target，请你在该数组中找出和为目标值的那两个整数，并返回他们的数组下标
    :param nums: 整数数组
    :param target: 目标值
    :return: 数组中和为目标值的两个整数的数组下标
    """
    n = len(nums)
    for i in range(n):
        for j in range(i + 1, n):
            if nums[i] + nums[j] == target:
                return [i, j]
    return []

```

#### Python解法效率对比

```Text
输入：nums = {1, 3, 5, 7, 9, 12, 13, 19, 20};target = 23;
方法一：10200ns
方法二：6800ns
```
