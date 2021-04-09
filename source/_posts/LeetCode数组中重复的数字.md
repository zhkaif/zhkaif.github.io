---
title: LeetCode数组中重复的数字
date: 2021-04-08 10:07:10
tags:
  - LeetCode
  - 数组
categories: [后端, 算法]
---

## LeetCode 数组中重复的数字

### 题目描述

在一个长度为 n 的数组 nums 里的所有数字都在 0~n-1 的范围内。数组中某些数字是重复的，但不知道有几个数字重复了，也不知道每个数字重复了几次。请找出数组中任意一个重复的数字。

示例 1：

```Text
输入：[2,3,1,0,2,5,3]
输出：2或3
```

### 一得之见（Java/Python）

使用双循环，index 不等且 value 相等时，即重复。
时间复杂度 O(n²)，空间复杂度 O(1)

```Java
    /**
     * 在一个长度为 n 的数组 nums 里的所有数字都在 0~n-1 的范围内。数组中某些数字是重复的，但不知道有几个数字重复了，也不知道每个数字重复了几次。请找出数组中任意一个重复的数字
     *
     * @param nums 数组
     * @return 任意一个重复的数字
     */
    public static int findRepeatNumber(int[] nums) {
        int result = -1;
        int numsLen = nums.length;
        for (int i = 0; i < numsLen; i++) {
            for (int j = 0; j < numsLen; j++) {
                if (i != j && nums[i] == nums[j]) {
                    result = nums[i];
                    break;
                }
            }
        }
        return result;
    }
```

```Python
def find_repeat_number(nums: List[int]) -> int:
    """
    在一个长度为 n 的数组 nums 里的所有数字都在 0~n-1 的范围内。数组中某些数字是重复的，但不知道有几个数字重复了，也不知道每个数字重复了几次。请找出数组中任意一个重复的数字
    :param nums: 数组
    :return: 任意一个重复的数字
    """
    nums_len = len(nums)
    for i in range(nums_len):
        for j in range(nums_len):
            if i != j and nums[i] == nums[j]:
                return nums[i]

    return -1
```

### 他山之石（Java/Python）

#### 使用集合 Set

把数组中的元素循环加入到集合 Set，如果加入时有重复，则返回。
时间复杂度：O(n)，空间复杂度：O(n)

```Java
    /**
     * 在一个长度为 n 的数组 nums 里的所有数字都在 0~n-1 的范围内。数组中某些数字是重复的，但不知道有几个数字重复了，也不知道每个数字重复了几次。请找出数组中任意一个重复的数字
     *
     * @param nums 数组
     * @return 任意一个重复的数字
     */
    public static int findRepeatNumberTwo(int[] nums) {
        Set<Integer> set = new HashSet<Integer>();
        int repeat = -1;
        for (int num : nums) {
            if (!set.add(num)) {
                repeat = num;
                break;
            }
        }
        return repeat;
    }
```

```Python
def find_repeat_number_two(nums: List[int]) -> int:
    """
    在一个长度为 n 的数组 nums 里的所有数字都在 0~n-1 的范围内。数组中某些数字是重复的，但不知道有几个数字重复了，也不知道每个数字重复了几次。请找出数组中任意一个重复的数字
    :param nums: 数组
    :return: 任意一个重复的数字
    """
    temp = set()
    for num in nums:
        if num not in temp:
            temp.add(num)
        else:
            return num
    return -1
```

#### 先排序再查找

先排序再查找，排序之后有重复的肯定是挨着的，然后前后两两比较，如果有重复的直接返回。
时间复杂度：O(n)，空间复杂度：O(1)

```Java
    /**
     * 在一个长度为 n 的数组 nums 里的所有数字都在 0~n-1 的范围内。数组中某些数字是重复的，但不知道有几个数字重复了，也不知道每个数字重复了几次。请找出数组中任意一个重复的数字
     *
     * @param nums 数组
     * @return 任意一个重复的数字
     */
    public static int findRepeatNumberFour(int[] nums) {
        Arrays.sort(nums);
        for (int i = 1; i < nums.length; i++) {
            if (nums[i] == nums[i - 1]) {
                return nums[i];
            }
        }
        return -1;
    }
```

```Python
def find_repeat_number_three(nums: List[int]) -> int:
    """
    在一个长度为 n 的数组 nums 里的所有数字都在 0~n-1 的范围内。数组中某些数字是重复的，但不知道有几个数字重复了，也不知道每个数字重复了几次。请找出数组中任意一个重复的数字
    :param nums: 数组
    :return: 任意一个重复的数字
    """
    nums.sort()
    nums_len = len(nums)
    for i in range(1, nums_len):
        if nums[i] == nums[i - 1]:
            return nums[i]
    return -1
```

#### 使用临时数组

这道题有个很明显的特点，就是数字的大小在 0~n-1 之间，所以使用上面两种方法肯定不是最好的选择。这里我们可以申请一个临时数组 temp，因为 nums 元素中的每个元素的大小都在 0~n-1 之间，所以我们可以把 nums 中的元素的值和临时数组 temp 建立映射关系，就是 nums 中的元素的值是几，我们就把 temp 中对应的位置的值加 1，当 temp 某个位置的值大于 1 的时候，我们直接返回即可。
时间复杂度：O(n)，空间复杂度：O(n)

```Java
    /**
     * 在一个长度为 n 的数组 nums 里的所有数字都在 0~n-1 的范围内。数组中某些数字是重复的，但不知道有几个数字重复了，也不知道每个数字重复了几次。请找出数组中任意一个重复的数字
     *
     * @param nums 数组
     * @return 任意一个重复的数字
     */
    public static int findRepeatNumberFive(int[] nums) {
        int length = nums.length;
        int[] temp = new int[length];
        for (int i = 0; i < length; i++) {
            temp[nums[i]]++;
            if (temp[nums[i]] > 1) {
                return nums[i];
            }
        }
        return -1;
    }
```

```Python
def find_repeat_number_four(nums: List[int]) -> int:
    """
    在一个长度为 n 的数组 nums 里的所有数字都在 0~n-1 的范围内。数组中某些数字是重复的，但不知道有几个数字重复了，也不知道每个数字重复了几次。请找出数组中任意一个重复的数字
    :param nums: 数组
    :return: 任意一个重复的数字
    """
    nums_len = len(nums)
    temp = [0 for i in range(nums_len)]
    for num in nums:
        temp[num] += 1
        if temp[num] > 1:
            return num
    return -1
```

#### 原地置换

如果没有重复数字，那么正常排序后，数字 i 应该在下标为 i 的位置，所以思路是重头扫描数组，遇到下标为 i 的数字如果不是 i 的话，（假设为 m），那么我们就拿与下标 m 的数字交换。在家换过程中，如果有重复数字，那么终止返回。
时间复杂度：O(n)，空间复杂度：O(1)

```Java
    /**
     * 在一个长度为 n 的数组 nums 里的所有数字都在 0~n-1 的范围内。数组中某些数字是重复的，但不知道有几个数字重复了，也不知道每个数字重复了几次。请找出数组中任意一个重复的数字
     *
     * @param nums 数组
     * @return 任意一个重复的数字
     */
    public static int findRepeatNumberThree(int[] nums) {
        int temp;
        for (int i = 0; i < nums.length; i++) {
            while (nums[i] != i) {
                if (nums[i] == nums[nums[i]]) {
                    return nums[i];
                }
                temp = nums[i];
                nums[i] = nums[temp];
                nums[temp] = temp;
            }
        }
        return -1;
    }
```

```Python
def find_repeat_number_five(nums: List[int]) -> int:
    """
    在一个长度为 n 的数组 nums 里的所有数字都在 0~n-1 的范围内。数组中某些数字是重复的，但不知道有几个数字重复了，也不知道每个数字重复了几次。请找出数组中任意一个重复的数字
    :param nums: 数组
    :return: 任意一个重复的数字
    """
    nums_len = len(nums)
    for i in range(nums_len):
        if nums[i] != i:
            if nums[i] == nums[nums[i]]:
                return nums[i]
            else:
                temp = nums[i]
                nums[i] = nums[temp]
                nums[temp] = temp
    return -1
```

#### 效率对比（Java）

```Text
输入：nums = {2, 3, 1, 0, 2, 5, 3};
方法一：1990900ns （个人笨比解法😂）
方法二：238000ns  （使用集合Set）
方法三：12600ns   （先排序再查找🤗）
方法四：589800ns  （使用临时数组）
方法五：17600ns   （原地置换）
```

#### 效率对比（Python）

```Text
输入：nums = {2, 3, 1, 0, 2, 5, 3};
方法一：6600ns （个人笨比解法 😂）
方法二：4500ns （使用集合 Set）
方法三：8100ns （先排序再查找 🤗）
方法四：28300ns （使用临时数组）
方法五：6600ns （原地置换）
```
