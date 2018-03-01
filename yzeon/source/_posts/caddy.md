---
title: Caddy安装和配置
date: 2018-02-28 10:35:36
tags:
---
前面那个没搞定WebSocket+TLS+Web.卡在NGINX认证成功后与V2RAY共享钥匙那块.各种出错.
别人的nginx可以用acme.sh直接获取钥匙并输出给V2RAY.但是我这个一直报错.

## 停用其他WEB服务
```
sudo apt-get remove nginx nginx-common	 	#卸载删除除了配置文件以外的所有文件。
sudo apt-get purge nginx nginx-common       #卸载所有东东，包括删除配置文件。
sudo apt-get autoremove					    #在上面命令结束后执行，主要是卸载删除Nginx的不再被使用的依赖包。
sudo apt-get remove nginx-full nginx-common #卸载删除两个主要的包。
sudo apt-get remove apache2 				#APACHE也一样的.
```


## 安装与配置个人版CADDY
```
curl https://getcaddy.com | bash -s personal
```
在网站也可以下载其他版本的.[https://caddyserver.com](https://caddyserver.com)

### 生产环境
本身Caddy是没有固定文件夹的.可以随意在任何位置配置网站.
```
mkdir /etc/caddy                  #为了记忆方便还是放在ETC下.
sudo mkdir -p /var/www/localhost       #当然VAR三W方便.

sudo chown -R root:www-data /etc/caddy
```

### 配置文件
```
sudo nano /etc/caddy/Caddyfile

:80 localhost:80 {
    root /var/www/localhost
    gzip
}
```
监听本地80端口.文件地址.开启GZIP.比起NGINX来.它的配置简单很多.

## 命令行运行caddy
没了...就这么简单粗暴...

# 重点是这鬼玩意居然报错.不搞了
sudo rm -rf /etc/caddy




