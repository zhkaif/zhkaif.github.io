---
title: LeetCode最富有客户的资产总量
date: 2021-04-06 11:13:37
tags:
  - LeetCode
  - 数组
categories: [后端, 算法]
---

## 最富有客户的资产总量

### 题目描述

给你一个 m \* n 的整数网格 accounts，其中 account[i][j]是第 i 位客户在第 j 家银行托管的资产数量。返回最富有客户所拥有的资产总量。
客户的资产总量就是他们在各家银行托管的资产数量之和。最富有客户就是资产总量最大的客户。

示例 1：

```Text
输入：accounts = [[1,2,3],[3,2,1]]
输出：6
解释：
第1位客户的资产总量 = 1+2+3=6
第2位客户的资产总量 = 3+2+1=6
两位客户都是最富有的，资产总量都是6，所以返回6。
```

示例 2：

```Text
输入：accounts = [[1,5],[7,3],[3,5]]
输出：10
解释：
第1位客户的资产总量 = 6
第2位客户的资产总量 = 10
第3位客户的资产总量 = 8
第2为客户是最富有的，资产总量是10
```

示例 3：

```Text
输入：accounts = [[2,8,7],[7,1,3],[1,9,5]]
输出：17
```

### 一得之见（Java）

```Java
/**
 * @author zhkai
 * @date 2021年4月6日11:28:27
 */
public class MaximumWealth {
    /**
     * 给你一个 m * n 的整数网格 accounts，其中 account[i][j]是第 i 位客户在第 j 家银行托管的资产数量。返回最富有客户所拥有的资产总量。
     *
     * @param accounts 整数网格
     * @return 最富有客户的资产总量
     */
    public static int maxWealth(int[][] accounts) {
        int accountsLen = accounts.length;
        int sum = 0;
        int result = 0;
        for (int i = 0; i < accountsLen; i++) {
            for (int j = 0; j < accounts[i].length; j++) {
                sum += accounts[i][j];
            }
            result = Math.max(result, sum);
            sum = 0;
        }
        return result;
    }
}

```

### 他山之石（Java）

```Java
/**
     * 给你一个 m * n 的整数网格 accounts，其中 account[i][j]是第 i 位客户在第 j 家银行托管的资产数量。返回最富有客户所拥有的资产总量。
     *
     * @param accounts 整数网格
     * @return 最富有客户的资产总量
     */
    public static int maxWealthTwo(int[][] accounts) {
        return Arrays.stream(accounts).map(ints -> Arrays.stream(ints).sum()).max(Integer::compareTo).get();
    }
```

### 效率分析（Java）

```Text
输入：accounts = {{1,2,3},{1,4,5},{1,4,7}}
方法一：2241700ns
方法二：153366300ns
```

### 一得之见（Python）

```Python
from typing import List


def max_wealth(accounts: List[List[int]]) -> int:
    """
    给你一个 m * n 的整数网格 accounts，其中 account[i][j]是第 i 位客户在第 j 家银行托管的资产数量。返回最富有客户所拥有的资产总量。
    :param accounts: 整数网格
    :return: 最富有客户的资产总量
    """
    accounts_len = len(accounts)
    result = 0
    for i in range(accounts_len):
        len_two = len(accounts[i])
        sum_one = 0
        for j in range(len_two):
            sum_one += accounts[i][j]
            result = max(result, sum_one)
    return result
```

### 他山之石（Python）

```Python
def max_wealth_two(accounts: List[List[int]]) -> int:
    """
    给你一个 m * n 的整数网格 accounts，其中 account[i][j]是第 i 位客户在第 j 家银行托管的资产数量。返回最富有客户所拥有的资产总量。
    :param accounts: 整数网格
    :return: 最富有客户的资产总量
    """
    return max(sum(accounts[i]) for i in range(len(accounts)))
```

### 效率分析（Python）

```Text
输入：accounts = {{1,2,3},{1,4,5},{1,4,7}}
方法一：13600ns
方法二：9100ns
```
