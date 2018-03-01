---
title: 接触LINUX并登录
date: 2017-12-27 13:13:20
tags:
---

当你找到这篇文章的时候.肯定是被WINDOWS弄的苦不堪言了.哦~也许就是我自己回来看看而已...

估计只有2018年的WEB USB一统天下了.才会让我对WINDOWS好一点...

除开MAC不说.那个使用太少了.系统又不给瞎折腾.折腾坏了又贵...所以我转投了LINUX阵营

之前的那堆黑历史日志应该不更新了.没啥意思.扔WORDPRESS封存吧.老早就想用静态页面了.你看那个MYSQL居然占微型服务器的60%多资源...

LINUX wiki https://zh.wikipedia.org/wiki/Linux

这里就说说系统吧.
* Debian GNU/Linux 8/9
* CentOS 6/7
* Ubuntu 14/16/17

常见的就这几个.新手选UBUNTU.图形界面不LOW.从WINDOWS过度简单.

LINUX的命令也很简单粗暴.
``` bash
XXX -X 
```
命令我都不记.要用的时候去GOOGLE.后来发现要GOOGLE的地方太多.就有了写一个学习日志的地方.

目前接触的都是Ubuntu系统.之前买的各种VPS都选的这个.口碑好的那几家我都用过了.性价比来说.有卷肯定选GOOGLE啊.没卷就选有2.5刀的

### 选啥版本
选Ubuntu 16.04 LTS/64位.反正我也不知道32位有啥区别.版本别选最高.一般降一档都可以用.只要不做开发.什么版本什么都无所谓啊

树莓派				Ubuntu Mate
VPS				Ubuntu 16 好像现在17都出来了.

## 虚拟机
没啥好说的.进去就是图形界面

## VPS
申请之后会给你IP和账号密码.用SSH软件进行连接.WIN系统里我习惯用PUTTY.MAC直接用命令行.

``` bash
ssh 服务器IP -p 端口号(如果默认22的话.-p也不用打了)
```
我早就用快捷方式连服务器了.多省事...

``` bash
C:\Users\zy\Desktop\putty\PUTTY.EXE -i "C:\Users\zy\Desktop\putty\pi\pi.ppk" -P 8530 zeonpi@192.168.1.88
```

PUTTY地址 + 钥匙文件 + 端口号 + 用户名@服务器地址
做一个快捷方式...双击~刚开始没那么复杂~直接用户名@服务器地址.

成功后就应该是
``` bash
Using username "用户名".
Authenticating with public key "钥匙名"
Welcome to Ubuntu 16.04.3 LTS (GNU/Linux 4.4.38-v7+ armv7l)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

8 packages can be updated.
0 updates are security updates.

*** System restart required ***
Last login: Tue Dec 26 16:26:56 2017 from 192.168.1.74
用户名@计算机名:~$
```

如果你用管理员身份登录的话就是ROOT@XXX之类的.管理员权限很大.

普通用户换成管理员需要用SUDO命令
```bash
sudo -i
```

如果你手贱的话~可以打上升级命令....看见那个
8 packages can be updated.
0 updates are security updates.
没?会告诉你可以升级多少个~如果你新装的话.好几百个可以升级.

```bash
apt-get update && apt-get dist-upgrade -y
```

网速不好的话.呵呵...本篇完.