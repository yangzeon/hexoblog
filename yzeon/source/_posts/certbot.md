---
title: 树莓派安装Let's Encrypt SSL证书
date: 2018-02-27 10:35:36
tags:
---
为了给梯子升级.需要加入TLS传输层安全协议.就有了如下的一篇文章.

## Let's Encrypt 简介
如果要启用HTTPS.我们就需要从证书授权机构(以下简称CA) 处获取一个证书.Let's Encrypt 就是一个 CA.我们可以从 Let's Encrypt 获得网站域名的免费的证书.

## Certbot 简介
Certbot 是Let's Encrypt官方推荐的获取证书的客户端.可以帮我们获取免费的Let's Encrypt 证书.Certbot 是支持所有 Lunix 内核的操作系统的.这里系统是UBUNTU16.04 硬件是 树莓派3.
```
https://github.com/certbot/certbot
```
### 绑定域名
* 一级域名哪申请的哪解析.绑定IP
* 二级域名可以直接使用DNS方式申请证书.这里不讲了.我也用不上
* [教你申请.tk/.ml/.cf/.gq/.ga等免费域名](https://doub.io/dbwz-3/)
### 用CERTBOT申请证书
#### Install
升级系统.安装PPA.添加安装包到仓库.
```
sudo apt-get update
sudo apt-get install software-properties-common
sudo add-apt-repository ppa:certbot/certbot
sudo apt-get update
sudo apt-get install python-certbot-nginx 
```
#### 使用nginx插件获取证书
```
sudo certbot --nginx
```
这种方式会更改Nginx的配置.之前用ACME.SH一直出错.那种是不更改配置的快速部署方式.反正我又没配置什么.加上也是闲置域名.就无所谓改配置的问题了.

```
sudo certbot --nginx certonly
```
这个是手动模式.开发者文档.所以没事别用这个了.[To learn more about how to use Certbot read our documentation](https://certbot.eff.org/docs)
#### 自动更新
```
sudo certbot renew --dry-run
#添加CRONTAB定时任务.每隔2个月的凌晨2点15分运行一次.注意ROOT权限
15 2 * */2 * certbot renew --pre-hook "service nginx stop" --post-hook "service nginx start"
```
这个命令是给快到90天的证书续约的.证书地址 /etc/letsencrypt/live/DOMAIN/fullchain.pem  and /etc/letsencrypt/live/DOMAIN/privkey.pem
#### 验证证书
[To learn more about how to use Certbot read our documentation](https://www.ssllabs.com/ssltest/index.html)
查验证书的信息.证书有效期是从 2018 年 2 月 27 号到 2018 年的 4 月 26 号.密钥是 RSA 2048 bits.证书签发机构是 Let's Encrypt Authority X3 .Trusted 为 Yes 表明我这个证书可信.