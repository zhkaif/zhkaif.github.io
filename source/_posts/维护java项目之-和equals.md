---
title: 维护java项目之==和equals
date: 2021-03-19 09:54:38
tags: Java
categories: 后端
---
最近发生一件很尴尬的事情，在维护一个Java项目的时候，发现有使用`==`来比较两个对象的属性，
于是顺手就把`==`改成了equals。悲剧发生......
## `==`和equals的区别
`==`：对于基本类型来说是值比较，对于引用类型来说是引用比较
equals：引用比较，但一些类重写了equals方法，如String、Integer等，变成了值比较。
## 使用equals的前提
使用equals进行比较，如：
a和b是两个对象
  ```
    a.getId().equals(b.getId())
  ```
需要确保a.getId()不为null，因为null是没有.equals()方法的。
## 各种对象使用equals
String类型：
  可以使用StringUtils.equals()进行比较，该方法内置非空校验
其余封装类型：
  可以使用Objects.equals()进行比较，该方法内置非空校验
使用三目运算符：
  ```
    a.getId() == null ? b.getId() == null ? false : true : a.getId().equals(b.getId())
  ```