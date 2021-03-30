---
title: CefSharp创建客户端
date: 2021-03-19 11:10:53
tags:
  - CefSharp
  - 客户端

categories: [.Net, Winform]
---

## 安装 NuGet 包

    在Visio studio中右击解决方案，选择管理NuGet包，搜索安装CefSharp.WinForms。

## 配置工作

    (1)首先右击项目选择属性，在"生成"选项中将"首选32位"勾上。

    (2)其次在项目文件目录下找到"项目名称.csproj"文件，在第一个PropertyGroup中添加以下代码：

    ``` xml
        <CefSharpAnyCpuSupport>true</CefSharpAnyCpuSupport>
    ```

    (3)最后修改App.config文件，和<startup>标签并列地位，添加以下代码：

    ``` xml

    <runtime>
        <assemblyBinding xmlns="urn:schemas-microsoft-com:asm.v1">
            <probing privatePath="x86"/>
        </assemblyBinding>
    </runtime>
    ```

## 窗体代码

    ``` c#

    using System;
    using System.Collections.Generic;
    using System.ComponentModel;
    using System.Data;
    using System.Drawing;
    using System.Linq;
    using System.Text;
    using System.Threading.Tasks;
    using System.Windows.Forms;
    using CefSharp;
    using CefSharp.WinForms;

    namespace embebbedChromium
    {
        public partial class Form1 : Form
        {
            public ChromiumWebBrowser chromeBrowser;

            public Form1()
            {
                InitializeComponent();
                // 初始化全局组件后启动浏览器
                InitializeChromium();
            }

            private void Form1_Load(object sender, EventArgs e)
            {

            }

            public void InitializeChromium()
            {
                CefSettings settings = new CefSettings();
                //按照设置初始化cef
                Cef.Initialize(settings);
                // 创建一个浏览器组件
                chromeBrowser = new ChromiumWebBrowser("http://baidu.com");
                // 将其添加到表单并将其填充到表单窗口
                this.Controls.Add(chromeBrowser);
                chromeBrowser.Dock = DockStyle.Fill;
            }

            private void Form1_FormClosing(object sender, FormClosingEventArgs e)
            {
                Cef.Shutdown();
            }
        }
    }
    ```

## 参考地址

<https://ourcodeworld.com/articles/read/173/how-to-use-cefsharp-chromium-embedded-framework-csharp-in-a-winforms-application>
