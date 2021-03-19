---
title: Java类型转换工具类（持续更新）
date: 2021-03-19 11:08:23
tags: 

	- 类型转换
	- Utils

categories: 

	[后端, Java]

---

## 简介

  将项目中用到的类型转换做个记录。

## 详细代码

 ``` java
  @Component
  public class TypeUtil {
   // [start]字符串转各种格式
  
   // 字符串转日期(格式:"yyyyMMdd")
   public static Date StrToDateFirst(String str) {
    DateFormat format = new SimpleDateFormat("yyyyMMdd");
    Date date = null;
    try {
     date = format.parse(str);
    } catch (ParseException e) {
     e.printStackTrace();
    }
    return date;
   }
  
   // 字符串转日期(格式:"dd/MM/yyyy")
   public static Date StrToDateSecond(String str) {
    DateFormat format = new SimpleDateFormat("dd/MM/yyyy");
    Date date = null;
    try {
     date = format.parse(str);
    } catch (ParseException e) {
     e.printStackTrace();
    }
    return date;
   }
  
   // 字符串转日期(格式:"yyyy-MM-dd")
   public static Date StrToDateThird(String str) {
    DateFormat format = new SimpleDateFormat("yyyy-MM-dd");
    Date date = null;
    try {
     date = format.parse(str);
    } catch (ParseException e) {
     e.printStackTrace();
    }
    return date;
   }
  
   // 字符串转日期(格式:"yyyy-MM-dd HH:mm:ss")
   public static Date StrToDateFourth(String str) {
    DateFormat format = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
    Date date = null;
    try {
     date = format.parse(str);
    } catch (ParseException e) {
     e.printStackTrace();
    }
    return date;
   }
  
   // 字符串转Integer
   public static Integer StrToInteger(String str) {
    Integer integer = null;
    try {
     integer = Integer.valueOf(str);
    } catch (Exception e) {
     e.printStackTrace();
    }
    return integer;
   }
  
   // 字符串转Double
   public static Double StrToDouble(String str) {
    Double double1 = 0.00;
    try {
     double1 = Double.parseDouble(str);
    } catch (Exception e) {
     e.printStackTrace();
    }
    return double1;
   }
  
   // 字符串转时间戳
   public static Timestamp StrToTimeStamp(String str) {
    Timestamp timestamp = null;
    try {
     timestamp = Timestamp.valueOf(str);
    } catch (Exception e) {
     e.printStackTrace();
    }
    return timestamp;
   }
  
   // 字符串转BigDecimal
   public static BigDecimal StrTiBigdecimal(String str) {
    BigDecimal bigDecimal = null;
    try {
     bigDecimal = new BigDecimal(str);
    } catch (Exception e) {
     e.printStackTrace();
    }
    return bigDecimal;
   }
   // [end]
  
  }
 ```
