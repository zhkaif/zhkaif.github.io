---
title: LeetCode替换空格
date: 2021-04-13 13:11:11
tags:
  - LeetCode
  - 数组
categories: [后端, 算法]
---

## LeetCode 替换空格

### 题目描述

请实现一个函数，把字符串 s 中的每个空格替换成"%20"。

实例 1：

```Text
输入：s = "We are happy."
输出："We%20are%20happy."
```

### 一得之见（Java/Python）

使用 Java 的 replaceAll()方法，直接进行替换

```Java
    /**
     * 把字符串 s 中的每个空格替换成"%20"。
     *
     * @param s 原字符串
     * @return 替换后字符串
     */
    public static String replaceSpace(String s) {
        return s.replaceAll(" ", "%20");
    }
```

使用 Python 的 replace()方法，直接进行替换

```Python
def replace_space(s: str) -> str:
    """
    把字符串 s 中的每个空格替换成"%20"。
    :param s: 原字符串
    :return: 替换后字符串
    """
    return s.replace(" ", "%20")
```

### 他山之石（Java/Python）

由于每次替换从 1 个字符变成 3 个字符，使用字符数组课方便进行替换。简历字符数组的长度为 s 的长度的 3 被，这样可保证字符数组可以容纳所有替换后的字符。

- 获得 s 的长度 length
- 创建字符数组 array，其长度为 length\*3
- 初始化 size 为 0，size 表示替换后的字符串的长度
- 从左到右遍历字符串 s
  - 获得 s 的当前字符 c
  - 如果字符 c 是空格，则令`array[size] = '%'`，`array[size + 1] = '2'`，`array[size + 2] = '0'`，并将 size 的值加 3
  - 如果字符 c 不是空格，则令`array[size] = c`，并将 size 的值加 1 -遍历结束之后，size 的值等于替换后的字符串的长度，从 array 的前 size 个字符创建新字符串，并返回新字符串

时间复杂度：O(n) 空间复杂度：O(n)

```Java
    /**
     * 把字符串 s 中的每个空格替换成"%20"。
     *
     * @param s 原字符串
     * @return 替换后字符串
     */
    public static String replaceSpaceTwo(String s) {
        int sLen = s.length();
        char[] sArray = new char[sLen * 3];
        int size = 0;
        for (int i = 0; i < sLen; i++) {
            char c = s.charAt(i);
            if (c == ' ') {
                sArray[size++] = '%';
                sArray[size++] = '2';
                sArray[size++] = '0';
            } else {
                sArray[size++] = c;
            }
        }
        String newStr = new String(sArray, 0, size);
        return newStr;
    }
```

- 初始化一个 list，记为 res
- 遍历列表 s 的每个字符 c：
  - 当 c 为空格时：向 res 后添加字符串"%20"
  - 当 c 不为空格时：向 res 后添加字符串 c
- 将列表 s 转换为字符串并返回。

时间复杂度：O(n) 空间复杂度：O(n)

```Python
def replace_space_two(s: str) -> str:
    """
    把字符串 s 中的每个空格替换成"%20"。
    :param s: 原字符串
    :return: 替换后字符串
    """
    res = []
    for c in s:
        if c == ' ':
            res.append("%20")
        else:
            res.append(c)
    return "".join(res)
```

### 效率对比（Java）

```Text
输入："We are happy"
方法一：3575700ns
方法二：338300ns
```

### 效率对比（Python）

```Text
输入："We are happy"
方法一：3300ns
方法二：7600ns
```
