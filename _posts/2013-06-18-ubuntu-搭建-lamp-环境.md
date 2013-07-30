---
layout: post
title: Ubuntu 搭建 LAMP(Linux+Apache+MySQL+Php) 环境
permalink: ubuntu-install-lamp
category: 记录
tags: ubuntu install lamp
---

LAMP 是指一组通常一起使用来运行动态网站或者服务器的自由软件名称首字母缩写：

-   Linux, 操作系统
-   Apache, 网页服务器
-   MySQL, 数据库管理系统（或者数据库服务器）
-   PHP 和有时 Perl 或 Python, 脚本语言

——wikipedia

- - -

下面是安装步骤（顺序不固定）

-   安装 Apache

        sudo apt-get install apache2

    测试：在浏览器中输入 http://localhost, 若出现 It works!, 请继续下一步

-   安装 MySQL

        sudo apt-get install mysql-server

    安装的过程中会提示你输入密码，不要忘记了  
    测试：输入命令 mysql -uroot -p, 然后输入安装的时候设置的密码，
    若出现欢迎界面（Welcome to the MySQL monitor.），请继续下一步

-   安装 Php 及一些必要模块

        sudo apt-get install php5 libapache2-mod-php5 php5-mysql

    测试：在 apache 的 www 文件夹下（默认在 /var）新建文件 phpinfo.php
    内容为 <?php phpinfo(); ?>,
    在浏览器中输入 http://localhost/phpinfo.php, 若出现 php 信息, 请继续下一步
