---
layout: post
title: Windows &amp; Linux 配置 JAVA 环境
permalink: windows-linux-install-java
category: 记录
tags: windows linux install java
---

## 下载 JDK

<http://www.oracle.com/technetwork/java/javase/downloads/index.html>

## 介绍几个环境变量

首先，先来介绍一下需要的三个环境变量

JAVA_HOME —— JAVA 的安装目录。
需要 JAVA 环境的软件可能会用到，比如 tomcat, eclipse 等。
配置 PATH, CLASSPATH 变量的时候会用到，增加了方便性，复用性

Path —— 可执行文件搜索路径。
当执行一个可执行文件的时候，如果在当前目录下找不到，系统会依次到 Path 指定的目录下去寻找，直到找到第一个，若找不到则报错。
JAVA 的编译命令（javac），执行命令（java），提取注释工具（javadoc）等，都在 %JAVA_HOME%\bin 目录下

CLASSPATH —— 类搜索路径。
用于搜索 JAVA 程序编译或者运行时用到的类。
自己写的一些类可能放在当前目录下，所以在环境变量中加上 .; 表示当前目录。
还有一些常用的类库 rt.jar（JAVA基础类库。默认加载的，可以不加到 CLASSPATH 中）, dt.jar（关于运行环境的类库）, tools.jar（工具类库）。
jar 就是把一些 java 文件打包起来了，为了方便

## Windows

右键我的电脑，打开属性，然后点击高级系统设置，就能看见环境变量  
假定 JDK 安装在 `C:\Program Files\Java\jdk1.7.0_17`  
修改系统变量，步骤如下:

1.  新建变量 JAVA_HOME，变量值为 `C:\Program Files\Java\jdk1.7.0_17`
2.  找到 Path 变量，在变量值中添加 `;%JAVA_HOME%\bin`
3.  新建变量 CLASSPATH，变量值为 `.;%JAVA_HOME%\lib\dt.jar;%JAVA_HOME%\lib\tools.jar`

点击确定，设置完毕。
在cmd中输入 java -version ，如果出现 java version “1.7.0_17″…，环境变量就设置成功了。

## Linux

**方法一**

首先下载好 jdk 的包，假设下载的是 jdk-7u17-linux-x64.tar.gz  
然后是解压到指定目录

    sudo tar xvf jdk-7u17-linux-x64.tar.gz -C /usr/lib

修改环境变量

    sudo vim /etc/profile

打开 profile 文件，在末尾添加以下内容

```bash
export JAVA_HOME=/usr/lib/jdk1.7.0_17
export PATH=$JAVA_HOME/bin:$PATH
export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
```

**方法二（ubuntu）**

ubuntu 系统中，本身有 OpenJDK 可以使用，
但你如果更喜欢 Oracle JDK，可以通过添加 PPA 的方式，安装和更新都比较方便

    sudo add-apt-repository ppa:webupd8team/java
    sudo apt-get update
    sudo apt-get install oracle-java7-installer

输入命令 java -version ，如果出现 java version …，那么就恭喜你了
