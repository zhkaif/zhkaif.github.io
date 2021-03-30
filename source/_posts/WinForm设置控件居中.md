---
title: WinForm设置控件居中
date: 2021-03-19 11:10:36
tags:
  - 控件
  - 居中

categories: [.Net, Winform]
---

## 简单阐述

    在C#的WinForm里面，原生控件是没有居中属性的，故通过重写OnResize(EventArgs e)方法，通过计算，重新定位控件位置。

## 以 Label 控件为例

    (1)将label的AutoSize属性设置为false；Dock属性设置为fill；TextAlign属性设置为MiddleCenter。
    (2)重写居中的代码如下：

``` C#
protected override void OnResize(EventArgs e)
    {
            base.OnResize(e);
            int x = (int)(0.5 * (this.Width - label1.Width));
            int y = label1.Location.Y;
            label1.Location = new System.Drawing.Point(x,y);
    }
```

## 参考地址

<https://blog.csdn.net/mingyueyixi/article/details/55035935>
