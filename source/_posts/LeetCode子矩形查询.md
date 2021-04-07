---
title: LeetCode子矩形查询
date: 2021-04-07 09:28:57
tags:
  - LeetCode
  - 数组
categories: [后端, 算法]
---

## LeetCode 子矩形查询

### 题目描述

请你实现一个类`SubrectangleQueries`，它的构造函数的参数是一个`rows * cols`的矩形（这里用整数矩阵表示），并支持以下两种操作：

1. `updateSubrectangle(int row1, int col1, int row2, int col2, int newValue)`
   - 用 newValue 更新以`(row1,col1)`为左上角且以`(row2,col2)`为右下角的子矩形。
2. `getValue(int row, int col)`
   - 返回矩形中坐标`(row,col)`的当前值。

### 一得之见（Java）

```Java
/**
 * @author zhkai
 * @date 2021年4月7日09:37:05
 */
public class SubrectangleQueries {
    private int[][] rect = null;

    public SubrectangleQueries(int[][] rectangle) {
        this.rect = rectangle;
    }

    /**
     * 用 newValue 更新以(row1,col1)为左上角且以(row2,col2)为右下角的子矩形。
     *
     * @param row1     子矩形左上角行坐标
     * @param col1     子矩形左上角列坐标
     * @param row2     子矩形右下角行坐标
     * @param col2     子矩形右下角列坐标
     * @param newValue 子矩形新值
     */
    public void updateSubrectangle(int row1, int col1, int row2, int col2, int newValue) {
        if (rect != null) {
            for (int i = row1; i <= row2; i++) {
                for (int j = col1; j <= col2; j++) {
                    rect[i][j] = newValue;
                }
            }
        }
    }

    /**
     * 返回矩形中坐标(row,col)的当前值。
     *
     * @param row 行坐标
     * @param col 列坐标
     * @return 当前值
     */
    public int getValue(int row, int col) {
        if (rect != null) {
            return rect[row][col];
        }
        return -1;
    }
}

```

### 一得之见（Python）

```Python
from typing import List


class SubRectangleQueries:
    def __init__(self, rectangle: List[List[int]]):
        self.data = rectangle

    def update_sub_rectangle(
            self,
            row1: int,
            col1: int,
            row2: int,
            col2: int,
            new_value: int):
        """
        用 newValue 更新以(row1,col1)为左上角且以(row2,col2)为右下角的子矩形。
        :param self:
        :param row1: 子矩形左上角行坐标
        :param col1:子矩形左上角列坐标
        :param row2:子矩形右下角行坐标
        :param col2:子矩形右下角列坐标
        :param new_value:子矩形新值
        """
        if self.data is not None:
            for i in range(row1, row2 + 1):
                for j in range(col1, col2 + 1):
                    self.data[i][j] = new_value

    def get_value(self, row, col) -> int:
        """
        回矩形中坐标(row,col)的当前值
        :param self:
        :param row: 行坐标
        :param col: 列坐标
        :return: 当前值
        """
        if self.data is not None:
            return self.data[row][col]
        else:
            return -1

```
