---
date: 2014-09-09 14:30:00
title: 使用html5＋css3开发PC客户端程序
layout: post
tags:
   - html5
   - 客户端程序
   - node-webkit
---

 # 使用node-webkit 用html5 开发桌面客户端

  + 因为近期写了一个web 应用程序，但个人总觉得该应用做成PC客户端会更加实用。如果在用winform写一个有觉得不太合适，

    1.个人对winform 不是特别熟悉，

    2.费时费力

  + 这个时候我看到了node－webkit

    node－webkit最吸引我得就是可以跨平台了（linux,mac,windows）我不就时我一直向往得东西吗。

## mac node-webkit教程

    1.
    官网：https://github.com/rogerwang/node-webkit

    支持的平台：Windows 32bit，Linux 32/64bit，Mac 32bit(OS X 10.7+)

    选择与平台相对应的版本，下载并安装即可。

    2. 在下载得node-webit 创建一个新得目录helloworld 添加一个 index.html 文件和一个 package.josn文件

       package文件如下

    "main": "main.html",                              /* APP的主入口，文件名任意；必选 */
    "name": "nw-demo",                              /* APP的名称，必须具备唯一性，且符合正常变量命名；必选 */
    "description": "demo app of node-webkit",         /* APP的简单描述 */
    "version": "0.1.0",                               /* APP的版本号 */
    "keywords": [ "demo", "node-webkit"],            /* APP的关键字，搜索APP时用到 */
    "window": {                                       /* APP的窗口属性 */
        "icon": "link.png",                           /* APP图标（windows下，状态栏上可见） */
        "toolbar": true,                              /* 是否显示工具栏 */
        "width": 800,                                 /* 窗口初始化大小 */
        "height": 500,
        "frame": true                                 /* 是否显示外窗体，如最大化、最小化、关闭按钮 */
       },
    }
+ <b>其中，main和name是必选字段，更多配置字段，可参考官方地址：https://github.com/rogerwang/node-webkit/wiki/Manifest-Format</b>

      index.html只要时html页面就行我这个给一个参考

        <html>
        <head>
          <title>Hello World!</title>
        </head>
        <style>
          body{
            -webkit-user-select:none;
            -webkit-app-region:drag;
          }
        </style>
        <body>
        <h1>Hello World!</h1>
        <p><a href="http://iinterest.net">Blog</a></p>
        </body>
        </html>

    最简单得方式是通过 命令行进入 该目录
        zip -r ../${PWD##*/}.nw *
    解压成一个 .nw文件，点击.nw文件就可以运行了，到此我们得mac客户端就已经成功了

## windows .exe 制作

    1
    官网：https://github.com/rogerwang/node-webkit 下载对应得 windows 版本node-webkit

    和之前得创建文件夹一样。

       将之前mac 版本得app.nw文件复制到 node-webkit文件下

       copy /b nw.exe+app.nw app.exe

    之前在网上看到得教程是在windows 下将文件夹压缩成.zip文件

    然后修改.zip为 .nw后缀名 但我这个操作，得到得app.exe都为 nw.exe

## 关于package.josn总属性

+ 在这里写一下关于 package得几个常用得属性(也方便自己记录)


        "resizable": true,   //是否允许窗口调整大小
        "position": "center",      //窗口打开的位置 可设置为 null,center,mouse
        "frame": true        //如果为false 程序将乌边框显示
        "toolbar": false,      //是否显示导航栏
        "show_in_taskbar": true //是否在导航栏种显示图标

+ 鉴于本人也是刚刚学习，就说这么多了，
