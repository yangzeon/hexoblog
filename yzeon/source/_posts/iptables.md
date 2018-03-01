---
title: 防火墙
date: 2017-12-28 17:35:36
tags:
---
## iptables.
```
sudo apt-get install iptables            # ubuntu
service iptables  (start|stop|restart|status)
```
规则列表
iptables -L -n
清初列表
iptables -F
```
iptables -A INPUT -p tcp --dport 22 -j ACCEPT
iptables -A INPUT -p tcp --dport 80 -j ACCEPT
iptables -A INPUT -p tcp --dport 443 -j ACCEPT
iptables -A INPUT -p tcp --dport 5830 -j ACCEPT
iptables -A INPUT -p tcp --dport 14151 -j ACCEPT
iptables -A INPUT -p tcp --dport 14152 -j ACCEPT
iptables -A INPUT -p tcp --dport 14153 -j ACCEPT
```
#允许ping
iptables -A INPUT -p icmp -m icmp --icmp-type 8 -j ACCEPT
#禁止其他未允许的规则访问
iptables -A INPUT -j REJECT  #（注意：如果22端口未加入允许规则，SSH链接会直接断开。）
iptables -A FORWARD -j REJECT

<!-- more -->

#屏蔽单个IP的命令是
iptables -I INPUT -s 123.45.6.7 -j DROP
#封整个段即从123.0.0.1到123.255.255.254的命令
iptables -I INPUT -s 123.0.0.0/8 -j DROP
#封IP段即从123.45.0.1到123.45.255.254的命令
iptables -I INPUT -s 124.45.0.0/16 -j DROP
#封IP段即从123.45.6.1到123.45.6.254的命令是
iptables -I INPUT -s 123.45.6.0/24 -j DROP

删除已添加的iptables规则,以序号标记显示
iptables -L -n --line-numbers
比如要删除INPUT里序号为8的规则(要删除OUTPUT的话就改成OUTPUT，以此类推)
iptables -D INPUT 8

允许35.197.104.169访问本机的8530端口
iptables -I INPUT -s 35.197.104.169 -p tcp --dport 8530 -j ACCEPT
iptables -I INPUT -s 35.185.163.45 -p tcp --dport 8530 -j ACCEPT
iptables -I INPUT -s 192.168.1.0/24 -p tcp --dport 8530 -j ACCEPT


启用iptables的日志
$ iptables -A INPUT -j LOG
查看iptables的日志
启用iptables日志后。根据您的操作系统，检查以下日志文件来查看iptables日志的生成。
在Ubuntu和Debian
iptables的日志由内核生成的。因此，检查以下内核日志文件。
$ tailf /var/log/kern.log
在CentOS / RHEL和Fedora
# cat /var/log/messages
更改iptables的日志文件名
要更改iptables的日志文件名编辑 etc/rsyslog.conf 文件，并添加下列配置到文件中。
# vi /etc/syslog.conf 添加以下行
kern.warning /var/log/iptables.log 现在，使用以下命令重新启动rsyslog服务。
$ service rsyslog restart

iptables的开机启动及规则保存
CentOS上可以执行：service iptables save保存规则。
另外更需要注意的是Debian/Ubuntu上iptables是不会保存规则的。
需要按如下步骤进行，让网卡关闭是保存iptables规则，启动时加载iptables规则：
创建/etc/network/if-post-down.d/iptables 文件，添加如下内容：
#!/bin/bash
iptables-save > /etc/iptables.rules
执行：chmod +x /etc/network/if-post-down.d/iptables 添加执行权限。
创建/etc/network/if-pre-up.d/iptables 文件，添加如下内容：
#!/bin/bash
iptables-restore < /etc/iptables.rules
执行：chmod +x /etc/network/if-pre-up.d/iptables 添加执行权限。
关于更多的iptables的使用方法可以执行：iptables --help或网上搜索一下iptables参数的说明。

