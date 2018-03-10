---
title: 搞定NGINX SSL了
date: 2018-03-01 14:35:36
tags:
---
```
并创建 一个 bash 的 alias, 方便你的使用: acme.sh=~/.acme.sh/acme.sh
```
之前问题出在ALIAS这里了.直接定义地址后.如果不给权限运行.还是会出错.
所以别省事了.还是加上地址打命令吧
```
sudo ~/.acme.sh/acme.sh 参数
```

nginx的配置文件出错好多次.也不知道是什么机制.一直在调用默认文件DEFAULT.
我已经在SITES-AVAILABLE中建立新的配置文件.并且建立连接去ENABLE中
```
sudo cp /etc/nginx/sites-available/XXX.COM /etc/nginx/sites-enable/XXX.COM
```
后来我直接删了default文件.多省事.

<!-- more -->

```
[2018年 03月 01日 星期四 11:52:36 CST] Your cert is in  /home/user/.acme.sh/XXX.com/XXX.com.cer
[2018年 03月 01日 星期四 11:52:36 CST] Your cert key is in  /home/user/.acme.sh/XXX.com/XXX.com.key
[2018年 03月 01日 星期四 11:52:37 CST] The intermediate CA cert is in  /home/user/.acme.sh/XXX.com/ca.cer
[2018年 03月 01日 星期四 11:52:37 CST] And the full chain certs is there:  /home/user/.acme.sh/XXX.com/fullchain.cer
#上面是成功后给的地址
sudo ~/.acme.sh/acme.sh --installcert -d XXX.com --fullchainpath /etc/v2ray/v2ray.crt --keypath /etc/v2ray/v2ray.key -ecc   #ecc 证书 失败
sudo ~/.acme.sh/acme.sh --installcert -d XXX.com --fullchainpath /etc/v2ray/v2ray.crt --keypath /etc/v2ray/v2ray.key        #RSA 证书 OK

sudo ~/.acme.sh/acme.sh \
 --installcert  -d  XXX.com  \
 --key-file   /etc/nginx/ssl/XXX.com.key  \
 --fullchain-file /etc/nginx/ssl/fullchain.cer  \
 --reloadcmd  "service nginx force-reload"
```
本来要做这个证书导入的.后来直接在NGINX里指定了证书地址.
搞定后再去配置NGINX的网站信息
```
listen 443 ssl;         #开启443 SSL
listen [::]:443 ssl;

return 301 https://XXX.com$request_uri;   #重新定向HTTPS.但是我这边出错.就删了

ssl_certificate     /etc/nginx/ssl/fullchain.cer;     #证书地址.不过我给定义在V2RAY里了
ssl_certificate_key /etc/nginx/ssl/XXX.com.key;

server {
  listen  443 ssl;
  ssl on;
  ssl_certificate       /etc/v2ray/v2ray.crt;
  ssl_certificate_key   /etc/v2ray/v2ray.key;
  ssl_protocols         TLSv1 TLSv1.1 TLSv1.2;
  ssl_ciphers           HIGH:!aNULL:!MD5;
  server_name           mydomain.me;
        location /ray {
        proxy_redirect off;
        proxy_pass http://127.0.0.1:14151;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
        proxy_set_header Host $http_host;
        }
}
```
开个定时任务.搞定收工.没事就别管了...真的好容易报错
```
crontab -l
0 0 * * * "/home/ubuntu/.acme.sh"/acme.sh --cron --home "/home/ubuntu/.acme.sh" > /dev/null
```
#最后走一下流程看看能否正确执行
```
sudo ~/.acme.sh/acme.sh --cron 
```

* 一些注意事项
	* ssl_dhparam 未配置，将导致 ssllabs.com 的评分降到 B，并给 This server supports weak Diffie-Hellman (DH) key exchange parameters. Grade capped to B. 的警告。
	* ssl_prefer_server_ciphers on 也是一个必要的配置，否则会 A+ 变成 A-;
	* 如果你需要兼容老系统或老浏览器的话，你需要配置 ssl_ciphers，详见 Mozilla Server_Side_TLS 的介绍，Nginx 里面 ssl_ciphers 默认值是 HIGH:!aNULL:!MD5; ref

#### 验证证书
[To learn more about how to use Certbot read our documentation](https://www.ssllabs.com/ssltest/index.html)
查验证书的信息.证书有效期是从 2018 年 2 月 27 号到 2018 年的 4 月 26 号.密钥是 RSA 2048 bits.证书签发机构是 Let's Encrypt Authority X3 .Trusted 为 Yes 表明我这个证书可信.

what?一个大大的B？
原因人家在下面都给你写清楚了嘛.你的diffie-hellman算法强度太低.作用就是让访问的者浏览器和你的服务器能安全的交换密钥.nginx默认采用1024位的diffie-hellman.
```
cd ~/.acme.sh/XXX.COM  #证书放一起.省事
openssl dhparam -out dhparam.pem 2048 # 如果你的机器性能足够强大，可以用 4096 位加密
```
考验cpu性能的时候到了.树莓派用了半个小时..........你的目录下面应该已经出现了一个dhparam.pem文件.接下来我们就要修改nginx的配置文件.让他启用我们新生成的diffie-hellman.并且屏蔽容易被攻击的协议.

```
server {
  listen 443 ssl http2;
  server_name www.example.com;
  ssl on;
  ssl_certificate /etc/ssl/certs/ssl-bundle.crt;#证书文件
  ssl_certificate_key /etc/ssl/private/www_example_com.key;#私钥
  ssl_dhparam /etc/ssl/certs/dhparam.pem;#刚刚生成的那个pem文件的路径
  ssl_session_cache shared:SSL:10m;#开启缓存，有利于减少ssl握手开销
  ssl_session_timeout 10m;#SSL会话过期时间，有利于减少服务器开销
  ssl_protocols TLSv1 TLSv1.1 TLSv1.2;#指定可用的ssl协议，排除sslV3等容易被攻击的协议
  ssl_stapling on;#开启证书吊销状态检查
  ssl_trusted_certificate  /etc/ssl/certs/ssl-bundle.crt;#这个证书路径跟上面一样
  ssl_ciphers "ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES128-SHA256:ECDHE-ECDSA-AES128-SHA256:ECDHE-RSA-AES128-SHA:ECDHE-ECDSA-AES128-SHA:ECDHE-RSA-AES256-SHA384:ECDHE-ECDSA-AES256-SHA384:ECDHE-RSA-AES256-SHA:ECDHE-ECDSA-AES256-SHA:AES128-GCM-SHA256:AES256-GCM-SHA384:AES128-SHA256:AES256-SHA256:AES128-SHA:AES256-SHA:AES:CAMELLIA:DES-CBC3-SHA:!aNULL:!eNULL:!EXPORT:!DES:!RC4:!MD5:!PSK:!aECDH:!EDH-DSS-DES-CBC3-SHA:!EDH-RSA-DES-CBC3-SHA:!KRB5-DES-CBC3-SHA";#屏蔽不安全的加密方式
  ssl_prefer_server_ciphers on;
  add_header Strict-Transport-Security "max-age=63072000; includeSubdomains; preload";#开启HSTS，强制全站加密，如果你的网站要引用不加密的资源，或者考虑将来取消加密，就不要开这个
  location / {
    ……………………
  }
}
```

执行nginx -t
nginx会自动检测你的配置文件语法是否正确.看见 successful就执行service nginx reload