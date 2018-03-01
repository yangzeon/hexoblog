---
title: Lnmp
date: 2017-12-28 16:35:36
tags:
---

1.1 安装Nginx


$sudo apt-get install nginx



配置在/etc/nginx下，每个虚拟主机已经安排在了/etc/nginx/sites-available下
程序在/usr/sbin/nginx

日志在/var/log/nginx
并在/etc/init.d/下创建了启动脚本nginx


默认的虚拟主机的目录设置在了/var/www/nginx-default (有的版本 默认的虚拟主机的目录设置在

了/var/www, 请参考/etc/nginx/sites-available里的配置)



$sudo /etc/init.d/nginx start