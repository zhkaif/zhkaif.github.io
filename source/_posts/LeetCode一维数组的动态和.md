---
title: LeetCode一维数组的动态和
date: 2021-03-31 09:47:34
tags:
  - LeetCode
  - 数组
categories:
  - 后端
  - 算法
---

## 一维数组的动态和

### 题目描述

给你一个数组 nums。数组「动态和」的计算公式为：runningSum[i] = sum(nums[0]...nums[i])。
请返回 nums 的动态和。

示例 1：

```Text
  输入：nums = [1,2,3,4]
  输出：[1,3,6,10]
  解释：动态和计算过程为 [1, 1+2, 1+2+3, 1+2+3+4]
```

示例 2：

```Text
  输入：nums = [1,1,1,1,1]
  输出：[1,2,3,4,5]
  解释：动态和计算过程为 [1, 1+1, 1+1+1, 1+1+1+1, 1+1+1+1+1]
```

示例 3：

```Text
  输入：nums = [3,1,2,10,1]
  输出：[3,4,5,16,17]
  解释：动态和计算过程为 [3, 3+1, 3+1+5, 3+1+5+10, 3+1+5+10+1]
```

### 一得之见（Java）

```Java
/**
 * @author zhkai
 * @date 2021年3月31日10:07:10
 */
public class RunningSum {
    /**
     * 给你一个数组 nums 。数组「动态和」的计算公式为：runningSum[i] = sum(nums[0]…nums[i]) 。
     * 请返回 nums 的动态和
     *
     * @param nums 数组
     * @return nums 的动态和
     */
    public static int[] runningSum(int[] nums) {
        int numsLen = nums.length;
        if (numsLen == 0) {
            return null;
        }
        int[] result = new int[numsLen];
        for (int i = 0; i < numsLen; i++) {
            for (int j = 0; j <= i; j++) {
                result[i] += nums[j];
            }
        }
        return result;
    }
}
```

### 他山之石（Java）

```Java
 /**
     * 给你一个数组 nums 。数组「动态和」的计算公式为：runningSum[i] = sum(nums[0]…nums[i]) 。
     * 请返回 nums 的动态和
     *
     * @param nums 数组
     * @return nums 的动态和
     */
    public static int[] runningSumTwo(int[] nums){
        for( int i = 1 ; i < nums.length ; i++ ){
            nums[i] += nums[i-1];
        }
        return nums;
    }
```

```Java
/**
     * 给你一个数组 nums 。数组「动态和」的计算公式为：runningSum[i] = sum(nums[0]…nums[i]) 。
     * 请返回 nums 的动态和
     *
     * @param nums 数组
     * @return nums 的动态和
     */
    public static int[] runningSumThree(int[] nums) {
        int numsLen = nums.length;
        int[] result = new int[numsLen];
        result[0] = nums[0];
        for (int i = 1; i < numsLen; i++) {
            result[i] = result[i - 1] + nums[i];
        }
        return result;
    }
```

#### 效率分析(Java)

```Text
输入：nums = {1, 2, 3, 4};
方法一用时：1820300ns
方法二用时：8600ns
方法三用时：11300ns
```

### 一得之见（Python）

```Python
from typing import List


def running_sum(nums: List[int]) -> List[int]:
    """
    给你一个数组 nums 。数组「动态和」的计算公式为：runningSum[i] = sum(nums[0]…nums[i]) 。
    请返回 nums 的动态和
    :param nums: 数组
    :return: 数组的动态和
    """
    nums_len = len(nums)
    result = [0 for i in range(nums_len)]
    for i in range(nums_len):
        for j in range(i + 1):
            result[i] += nums[j]
    return result
```

### 他山之石（Python）

```Python
def running_sum_two(nums: List[int]) -> List[int]:
    """
    给你一个数组 nums 。数组「动态和」的计算公式为：runningSum[i] = sum(nums[0]…nums[i]) 。
    请返回 nums 的动态和
    :param nums: 数组
    :return: 数组的动态和
    """
    nums_len = len(nums)
    res = [nums[0]]
    for i in range(1, nums_len):
        total = res[i - 1] + nums[i]
        res.append(total)
    return res
```

#### 效率分析(Python)

```Text
输入：nums = {1, 2, 3, 4};
方法一用时：15700ns
方法二用时：6500ns
```
