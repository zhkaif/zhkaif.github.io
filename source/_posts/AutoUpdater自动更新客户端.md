---
title: AutoUpdater自动更新客户端
date: 2021-03-19 11:11:06
tags:
  - AutoUpdater
  - 客户端

categories: [.Net, Winform]
---

## 安装 NuGet 包

    在Visio studio中右击解决方案，选择管理NuGet包，搜索安装Autoupdater.NET.Official。

## 工作简介

        从服务器下载包含更新文件的XML文件，从中获取软件的最新版本信息。如果软件的最新版本大于用户PC上安装的当前软件版本，则会向用户显示更新对话框。当然，也可以设置按钮事件进行点击下载更新文件。如果文件是压缩包，会自动将压缩包的内容解压缩到应用程序目录。

## XML 文件

    ``` xml
        <?xml version = "1.0" encoding = "UTF-8"?>
        < item >
            < version > 2.0.0.0 </ version >
            < url > https://www.cnblogs.com</ url >
            < changelog > https://www.cnblogs.com </ changelog >
            < mandatory > false </ mandatory >
        </ item >
    ```

    如上所示：
        version(必填)：格式为X.X.X.X的版本标记。
        url(必填)：最新版本安装程序文件的url。
        changelog(可选)：程序更改日志的url。
        mandatory(可选)：强制更新，将跳过信息和稍后更新按钮隐藏。
    选择使用以下代码将跳过update对话框，自动下载更新：

    ``` xml
        <mandatory mode="2">true</mandatory>
    ```

        args(可选)：为安装提供命令行参数，参数可以包含%path%，用以替换正在执行的应用程序所在目录的路径。
        checksum(可选)：更新文件的校验和，用以检验文件的完整性，algorithm属性指定算法，支持 MD5,SHA1,SHA256,SHA384,SHA512。

    ```xml
    <checksum algorithm="MD5">Update file Checksum</checksum>
    ```

## 窗体代码

    ``` c#
    using AutoUpdaterDotNET;

    private void button1_Click(object sender, EventArgs e)
            {
                //XML文件地址
                AutoUpdater.Start("https://www.cnblogs.com");
            }
    ```

## 参考地址

<https://github.com/ravibpatel/AutoUpdater.NET>
