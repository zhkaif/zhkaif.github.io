---
title: Windows端口被占用解决方法
date: 2021-03-19 13:40:56
tags:
  - 端口占用

categories: [操作系统, Windows]
---

## Error 场景

启动 Java 项目失败，控制台显示

```text

Error starting ApplicationContext. To display the conditions report`re-run your application with 'debug' enabled.

***************************
APPLICATION FAILED TO START
***************************

Description:

The Tomcat connector configured to listen on port 8080 failed to start. The port may already be in use or the connector may be misconfigured.

Action:

Verify the connector's configuration, identify and stop any process that's listening on port 8080, or configure this application to listen on another port.
```

## 解决方法

- 查看那些进程占用了我们的端口号`8080`

  打开 Windows 控制台，输入命令

`netstat -nao | findstr "8080"`

可以看到占用 `8080` 端口的进程 PID 为 `8404`

- 杀死相应进程：

  在 Windows 控制台，继续输入命令

`taskkill /pid 8404 /f`

![avatar](https://img.imgdb.cn/item/605440cc524f85ce2903d864.jpg)
