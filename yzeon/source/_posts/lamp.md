---
title: Lamp
date: 2017-12-28 13:35:36
tags:
---

wget -b后台 -C断点续传 
tail -f wget-log 查看日志

apt-get install lamp-server^
mysql !Q....x

apt-get install phpmyadmin
root !Q.....x

ln -s /usr/share/phpmyadmin /var/www/html/

删除链接rm -rf

<!-- more -->

/var/www/html

mkdir 目录名
rm -rf 目录名
mv 目录名 目录名

<?php
   phpinfo();
?>  

/etc/apache2/sites-available    ls -l
/etc/apache2/sites-enabled      ls -l

sites-available 创建文件
xxx.conf


<VirtualHost *:80>
    ServerName www.cczhou.com

    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/html/cczhou


    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined

</VirtualHost>

#ln -s cczhou.conf ../sites-enabled/cczhou.conf
#ln -s /etc/apache2/sites-available/cczhou.conf /etc/apache2/sites-enabled/cczhou.conf

激活CCZHOU
sudo a2ensite cczhou
/etc/init.d/apache2 restart

wget https://wordpress.org/latest.tar.gz
tar -xzvf latest.tar.gz

chmod -R 755 /var/www/html/cczhou
chmod -R 755 /data/www/

find / -name php.ini
脚本时间60秒
max_execution_time = 60
最大内存64MB
memory_limit = 64M

开启Keep-Alive功能

开启Keep-Alive功能可使客户端到服务器端的连接持续有效，当出现对服务器的后继请求时，Keep-Alive功

能避免了建立或者重新建立连接。可见，对访问静态网页时，开启Keep-Alive是很有用的。

因为在进行WordPress管理方面上的优化时，需要安装静态缓存插件，所以，开启Keep-Alive功能十分必要

。

# vi /usr/local/apache/conf/extra/httpd-default.conf

依次修改以下四条：

Timeout 30

KeepAlive On

MaxKeepAliveRequests 100

KeepAliveTimeout 5
保存退出：

# :wq

然后，重启httpd服务：

# service httpd restart

lamp add 创建虚拟主机
lamp del 删除虚拟主机
lamp list 列出虚拟主机

程序目录

MySQL 安装目录: /usr/local/mysql
MySQL 数据库目录：/usr/local/mysql/data（默认，安装时可更改路径）
MariaDB 安装目录: /usr/local/mariadb
MariaDB 数据库目录：/usr/local/mariadb/data（默认，安装时可更改路径）
Percona 安装目录: /usr/local/percona
Percona 数据库目录：/usr/local/percona/data（默认，安装时可更改路径）
PHP 安装目录: /usr/local/php
Apache 安装目录： /usr/local/apache

MySQL 或 MariaDB 或 Percona 命令
/etc/init.d/mysqld (start|stop|restart|status)

Apache 命令
/etc/init.d/httpd (start|stop|restart|status)
/etc/init.d/httpd restart
网站根目录

默认的网站根目录： /data/www/default

增加权限
chown -R apache:apache /data/www/域名/

配置文件
/usr/local/apache/conf/httpd.conf
/usr/local/apache/conf/vhost/域名.conf

serveralias 增加www.

mysql -uroot -h192.168.1.24 -P3306 -p kobe24   # -u:用户名、-h:IP 远程连接数据库、-P:端口(默认

3306)、-p：密码；

阿里云ECS云服务打包。

暂停数据库：service mysqld stop

cd /home/data
tar -czvf /home/databak.tar.gz *

那么/home/data目录下的数据库都会打包备份到/home/databak.tar.gz这个文件