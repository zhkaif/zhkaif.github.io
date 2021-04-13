---
title: LeetCode二维数组中的查找
date: 2021-04-09 10:03:29
tags:
  - LeetCode
  - 数组
categories: [后端, 算法]
---

## LeetCode 二维数组中的查找

### 题目描述

在一个 n\*m 的二维数组中，每一行都按照从左到右递增的顺序排序，每一列都按照从上到下递增。请完成一个搞笑的函数，输入这样的一个二维数组和一个整数，判断数组中是否含有改整数。

示例：
现有矩阵 matrix 如下：

```Text
[
  [1,4,7,11,15],
  [2,5,8,12,19],
  [3,6,9,16,22],
  [10,13,14,17,24],
  [18,21,23,26,30]
]
给定target = 5，返回true
给定target = 20，返回false
```

### 一得之见（Java/Python）

双循环求解。
时间复杂度：O(nm)，空间复杂度：O(1)

```Java
    /**
     * 在一个 n * m 的二维数组中，每一行都按照从左到右递增的顺序排序，每一列都按照从上到下递增的顺序排序。请完成一个高效的函数，输入这样的一个二维数组和一个整数，判断数组中是否含有该整数。
     *
     * @param matrix 二维数组
     * @param target 整数
     * @return 二维数组中是否含有该整数
     */
    public static boolean findNumberInTwoDimenArray(int[][] matrix, int target) {
        if (matrix == null || matrix.length == 0 || matrix[0].length == 0) {
            return false;
        }
        int rows = matrix.length;
        int columns = matrix[0].length;
        for(int i = 0; i < rows; i++){
            for(int j = 0; j < columns; j++){
                if(matrix[i][j] == target){
                    return true;
                }
            }
        }
        return false;
    }
```

```Python
def find_number_in_two_dimen_array(
        matrix: List[List[int]], target: int) -> bool:
    """
    在一个 n * m 的二维数组中，每一行都按照从左到右递增的顺序排序，每一列都按照从上到下递增的顺序排序。请完成一个高效的函数，输入这样的一个二维数组和一个整数，判断数组中是否含有该整数。
    :param matrix: 二维数组
    :param target: 整数
    :return: 二维数组中是否含有该整数
    """
    if matrix is None or len(matrix) == 0 or len(matrix[0]) == 0:
        return False
    else:
        for i in range(len(matrix)):
            for j in range(len(matrix[0])):
                if matrix[i][j] == target:
                    return True
    return False
```

### 他山之石（Java/Python）

由于给定的二维数组具备每行从左到右递增以及每列从上到下递增的特点，当访问到一个元素时，可以排除数组中的部分元素。
从二维数组的右上角开始查找。如果当前元素等于目标值，则返回 true。如果当前元素大于目标值，则移到左边一列。如果当前元素小于目标值，则移到下边一行。
可以证明这种方法不会错过目标值。如果当前元素大于目标值，说明当前元素的下边的所有元素都一定大于目标值，因此往下查找不可能找到目标值，往左查找可能找到目标值。如果当前元素小于目标值，说明当前元素的左边的所有元素都一定小于目标值，因为往左查找不可能找到目标值，往下查找可能找到目标值。

- 若数组为空，返回 false
- 初始化行下标为 0，列下标为二维数组的列数减 1
- 重复下列步骤，知道行下标或者列下标超出边界
  - 获得当前下标位置的元素 num
  - 如果 num 和 target 相等，则返回 true
  - 如果 num 大于 target，列下标减 1
  - 如果 num 小于 target，行下标加 1

时间复杂度：O(n+m)，空间复杂度：O(1)

```Java
    /**
     * 在一个 n * m 的二维数组中，每一行都按照从左到右递增的顺序排序，每一列都按照从上到下递增的顺序排序。请完成一个高效的函数，输入这样的一个二维数组和一个整数，判断数组中是否含有该整数。
     *
     * @param matrix 二维数组
     * @param target 整数
     * @return 二维数组中是否含有该整数
     */
    public static boolean findNumberInTwoDimenArrayTwo(int[][] matrix, int target) {
        if (matrix == null || matrix.length == 0 || matrix[0].length == 0) {
            return false;
        }
        int rows = matrix.length;
        int columns = matrix[0].length;
        int row = 0;
        int column = columns - 1;
        while (row < rows && column >= 0) {
            int num = matrix[row][column];
            if (num == target) {
                return true;
            } else if (num > target) {
                column--;
            } else {
                row++;
            }
        }
        return false;
    }
```

### 效率对比（Java）

```Text
输入： matrix =
  [
    [1, 4, 7, 11, 15],
    [2, 5, 8, 12, 19],
    [3, 6, 9, 16, 22],
    [10, 13, 14, 17, 24],
    [18, 21, 23, 26, 30]
  ]
  target = 5
方法一：2201800ns （个人笨比解法😂）
方法二：8900ns  （线性查找）
```

### 效率对比（Python）

```Text
输入： matrix =
  [
    [1, 4, 7, 11, 15],
    [2, 5, 8, 12, 19],
    [3, 6, 9, 16, 22],
    [10, 13, 14, 17, 24],
    [18, 21, 23, 26, 30]
  ]
  target = 5
方法一：11000ns （个人笨比解法😂）
方法二：5900ns （线性查找）
```
