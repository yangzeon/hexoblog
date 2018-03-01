---
title: 磁盘相关
date: 2017-12-28 14:35:36
tags:
---
```
cd /var/
dd if=/dev/zero of=swapfile bs=1M count=2048  #获取1GB的文件块（一般设置为内存的2倍）
/sbin/mkswap swapfile       #创建Swap文件
/sbin/swapon swapfile       #激活Swap文件
chmod 0644 /var/swapfile    #为了安全，建议修改一下权限
echo "/var/swapfile swap swap defaults 0 0" >> /etc/fstab    #将swapfile添加到fstab文件中，开机自动启动
free -m                     #搞定了。此时查看内存信息
```
出现 “Swap: 1023” 字样表示设置成功。

CentOS 7上创建swap分区的步骤

<!-- more -->

：
①使用dd命令创建一个swap分区

dd if=/dev/zero of=/home/swap bs=1024 count=1048576
count的值是：size（多少M）* 1024，我这里设置的1G虚拟内存，也就是count=1024000.
②格式化swap分区

mkswap /home/swap
③把格式化后的文件分区设置为swap分区
swapon /home/swap
（关闭SWAP分区命令为：[root@localhost Desktop]#swapoff /home/swap）
此时，swap分区已经创建好了，使用free命令查看，可见多了一个挂载分区。
④swap分区自动挂载
vi /etc/fstab
在文件末尾加上
/home/swap swap swap default 0 0


1. 查看磁盘空间使用情况：df -h
2. 进入空间占用最多的目录：cd /
3. 使用命令 ： du -sh  *   查看根目录下每个文件夹的大小
4. 进入占用空间比较大的文件夹，然后再使用步骤2,3中命令查找大文件的方法依次查找。

常用命令
查找大文件：find / -type f -size +10G
文件大小排序：ls -lhS 或者 du -h * | sort -n