---
title: Java交换数组元素
date: 2021-03-30 13:53:53
tags:
  - 数组
categories: [后端, Java]
---

## Java 交换数组元素

### 代码示例

```Java
import java.util.Arrays;
import java.util.Collections;
import java.util.List;
import java.util.stream.Collectors;

/**
 * @author zhkai
 * @date 2021年3月30日14:09:29
 */
public class SwapElement {
    /**
     * 将数组下标为i和数组下标为j的两个数组元素进行交换
     *
     * @param nums 待交换数组
     * @param i    需交换下标
     * @param j    需交换下标
     * @return 交换后的数组
     */
    public static int[] swapElementOne(int[] nums, int i, int j) {
        int item = nums[i];
        nums[i] = nums[j];
        nums[j] = item;
        return nums;
    }

    /**
     * 将数组下标为i和数组下标为j的两个数组元素进行交换
     *
     * @param nums 待交换数组
     * @param i    需交换下标
     * @param j    需交换下标
     * @return 交换后的数组
     */
    public static int[] swapElementTwo(int[] nums, int i, int j) {
        List<Integer> item = Arrays.stream(nums).boxed().collect(Collectors.toList());
        Collections.swap(item, i, j);
        int[] result = item.stream().mapToInt(Integer::valueOf).toArray();
        return result;
    }

    /**
     * 将数组下标为i和数组下标为j的两个数组元素进行交换
     *
     * @param nums 待交换数组
     * @param i    需交换下标
     * @param j    需交换下标
     * @return 交换后的数组
     */
    public static int[] swapElementThree(int[] nums, int i, int j) {
        nums[i] ^= nums[j];
        nums[j] ^= nums[i];
        nums[i] ^= nums[j];
        return nums;
    }
}

```

### 效率对比

```Text
输入：nums = {1, 2, 3, 4}; i=1; j=3;
方法一：2420500ns
方法二：163113800ns
方法三：20200ns
```

### 总结

- 方法一：
    使用中间变量进行交换，不能直接进行交换。
- 方法二：
    使用Collections.swap()方法进行交换，需要先将数组转换成List，交换完成后再转换成数组返回。
- 方法三：
    使用位运算符进行交换。
    ^：如果相对应位值相同，则结果为0，否则为1
    `C ^= C1 等价于 C = C^C1`
