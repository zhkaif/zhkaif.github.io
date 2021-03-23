---
title: 维护java项目之==和equals
date: 2021-03-19 09:54:38
tags:
  - equals

categories: [后端, Java]
---

最近发生一件很尴尬的事情，在维护一个 Java 项目的时候，发现有使用 `==` 来比较两个对象的属性，
于是顺手就把 `==` 改成了 equals。悲剧发生......🤣🤣🤣

## `==` 和 equals 的区别

`==` ：对于基本类型来说是值比较，对于引用类型来说是引用比较
equals：引用比较，但一些类重写了 equals 方法，如 String、Integer 等，变成了值比较。

## 使用 equals 的前提

使用 equals 进行比较，如：
a 和 b 是两个对象

```java
  a.getId().equals(b.getId())
```

需要确保 a.getId()不为 null，因为 null 是没有.equals()方法的。

## 各种对象使用 equals

String 类型：
可以使用 StringUtils.equals()进行比较，该方法内置非空校验
其余封装类型：
可以使用 Objects.equals()进行比较，该方法内置非空校验
使用三目运算符：

```java
  a.getId() == null ? b.getId() == null ? false : true : a.getId().equals(b.getId())
```
