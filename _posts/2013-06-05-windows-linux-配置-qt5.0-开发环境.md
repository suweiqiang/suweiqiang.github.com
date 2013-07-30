---
layout: post
title: Windows &amp; Ubuntu 配置 Qt5.0 开发环境
permalink: windows-ubuntu-install-qt5
category: 记录
tags: windows ubuntu install qt
---

## Windows

有很多种方式，看自己喜好了

-   MinGW 工具链 + Qt Creator IDE
    1.  安装 [Qt 5.0.2 for Windows 32-bit (MinGW 4.7, 650 MB)][qt-downloads]
        （集成了 [MinGW 4.7][mingw] 和 [Qt Creator][qt-downloads]，很方便，安装的时候记得勾选上）
-   MS 工具链 + Qt Creator IDE
    1.  安装 [vs2012][vs]（Express 版就可以了，注册一下就能免费使用了）
    2.  安装 [Qt 5.0.2 for Windows 64-bit (VS 2012, 500 MB)][qt-downloads]
        （集成了 [Qt Creator][qt-downloads]，安装的时候记得勾选）
    3.  安装 [控制台调试器 (CDB)][cdb]（Qt Creator 中要使用的调试器）
-   MS 工具链 + VS IDE
    1.  安装 [vs2012][vs] （非 Express 版）
    2.  安装 [Qt 5.0.2 for Windows 64-bit (VS 2012, 500 MB)][qt-downloads]（使用编译好的就是方便）
    3.  安装 [Visual Studio Add-in 1.2.1 for Qt5][qt-downloads]（与VS的集成插件）

[qt-downloads]: http://qt-project.org/downloads
[mingw]: http://sourceforge.net/projects/mingw/files/
[vs]: http://www.microsoft.com/visualstudio/chs/products/visual-studio-overview
[cdb]: http://msdn.microsoft.com/zh-CN/windows/hardware/gg463009/

## ubuntu

1.  安装 C/C++ 编译环境

        sudo apt-get install build-essential

2.  [安装 Qt 5.0.2 for Linux 64-bit (394 MB)][qt-downloads]

3.  设置环境变量  
    假设 Qt 安装在 /usr/local

        sudo vim /etc/profile (所有用户) 或者 vim ~/.bashrc (当前用户)

    在末尾加入如下内容

    ```bash
    export QTDIR=/usr/local/Qt5.0.2
    export PATH=$QTDIR/5.0.2/gcc_64/bin:$PATH
    export PATH=$QTDIR/Tools/QtCreator/bin:$PATH
    ```

    `source /etc/profile` 使之生效

4.  构建程序的时候会如下出现错误  
    /usr/bin/ld: cannot find -lGL  
    collect2: ld returned 1 exit status  
    这是因为 Qt5.0 默认将 OpenGL 加入了工程，但是在机器上没有安装 OpenGL  
    解决办法就是安装 OpenGL Library

        sudo apt-get install libgl1-mesa-dev

- - -

参考：  
[QtDoc](http://qt-project.org/doc/qt-5.0/qtdoc/platform-details.html)  
[Ubuntu Qt5.0 安装后运行失败提示 can't find -Igl](http://blog.sina.com.cn/s/blog_c289bdac0101600z.html)
