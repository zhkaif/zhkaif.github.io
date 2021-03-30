---
title: LeetCode移除元素
date: 2021-03-25 13:39:46
tags:
  - LeetCode
  - 数组
categories: [后端, 算法]
---

## LeetCode 移除元素

### 题目描述

给你一个数组 nums 和一个值 val，你需要原地移除所有数值等于 val 的元素，并返回移除后数组的新长度。
不需要使用额外的数组空间，你必须仅使用 O(1)额外空间并原地修改输入数组。
元素的顺序可以改变。你不需要考虑数组中超出新长度后面的元素。

示例：

```Text
输入：nums = [3,2,2,3], val = 3
输出：2, nums = [2,2]
解释：函数应该返回新的长度 2, 并且 nums 中的前两个元素均为 2。你不需要考虑数组中超出新长度后面的元素。例如，函数返回的新长度为 2 ，而 nums = [2,2,3,3] 或 nums = [2,2,0,0]，也会被视作正确答案。
```

```Text
输入：nums = [0,1,2,2,3,0,4,2], val = 2
输出：5, nums = [0,1,4,0,3]
解释：函数应该返回新的长度 5, 并且 nums 中的前五个元素为 0, 1, 3, 0, 4。注意这五个元素可为任意顺序。你不需要考虑数组中超出新长度后面的元素。
```

### Java 解法

```Java
/**
     * 给你一个数组 nums 和一个值 val，你需要 原地 移除所有数值等于 val 的元素，并返回移除后数组的新长度。
     * 不要使用额外的数组空间，你必须仅使用 O(1) 额外空间并 原地 修改输入数组。
     * 元素的顺序可以改变。你不需要考虑数组中超出新长度后面的元素。
     *
     * @param nums 数组
     * @param val  判断值
     * @return 新数组的长度
     */
    public static int removeElement(int[] nums, int val) {
        int len = nums.length;
        if (len == 0) {
            return 0;
        }
        int i = 0;
        for (i = 0; i < len; i++) {
            if (nums[i] == val) {
                for (int j = i; j < len - 1; j++) {
                    nums[j] = nums[j + 1];
                }
                i--;
                len--;
            }
        }
        return i;
    }

    /**
     * 给你一个数组 nums 和一个值 val，你需要 原地 移除所有数值等于 val 的元素，并返回移除后数组的新长度。
     * 不要使用额外的数组空间，你必须仅使用 O(1) 额外空间并 原地 修改输入数组。
     * 元素的顺序可以改变。你不需要考虑数组中超出新长度后面的元素。
     *
     * @param nums 数组
     * @param val  判断值
     * @return 新数组的长度
     */
    public static int removeElementTwo(int[] nums, int val) {
        int len = nums.length;
        if (len == 0) {
            return 0;
        }
        int i = 0;
        for (int j = 0; j < nums.length; j++) {
            if (nums[j] == val) {
                continue;
            }
            nums[i++] = nums[j];
        }
        return i;
    }
```

### Java 解法效率对比

```Text
输入：nums = {1, 2, 3, 4, 5, 6, 7, 8, 9};val = 2;
方法一：2666500ns
方法二：15300ns
```

### Python 解法

```Python
from typing import List


def remove_element(nums: List[int], val: int) -> int:
    """
    给你一个数组 nums 和一个值 val，你需要 原地 移除所有数值等于 val 的元素，并返回移除后数组的新长度。
    不要使用额外的数组空间，你必须仅使用 O(1) 额外空间并 原地 修改输入数组。
    元素的顺序可以改变。你不需要考虑数组中超出新长度后面的元素。
    :param nums: 数组
    :param val: 判断值
    :return: 新数组的长度
    """
    n = len(nums)
    for i in range(n):
        if nums[i] == val:
            for j in range(n - 1):
                nums[j] = nums[j + 1]
            i -= 1
            n -= 1
    return i


def remove_element_two(nums: List[int], val: int) -> int:
    """
    给你一个数组 nums 和一个值 val，你需要 原地 移除所有数值等于 val 的元素，并返回移除后数组的新长度。
    不要使用额外的数组空间，你必须仅使用 O(1) 额外空间并 原地 修改输入数组。
    元素的顺序可以改变。你不需要考虑数组中超出新长度后面的元素。
    :param nums: 数组
    :param val: 判断值
    :return: 新数组的长度
    """
    n = len(nums)
    i = 0
    for j in range(n):
        if nums[j] == val:
            continue
        else:
            nums[i] = nums[j]
            i += 1
    return i

```

### Python 解法效率对比

```Text
输入：nums = {1, 2, 3, 4, 5, 6, 7, 8, 9};val = 2;
方法一：60700ns
方法二：36200ns
```
