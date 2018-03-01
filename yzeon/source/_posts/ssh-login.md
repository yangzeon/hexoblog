---
title: 使用ssh登录
date: 2017-12-27 16:47:10
tags:
---
##### SSH
这个坑到现在还在困扰着我.简单来讲就是这东西就是一个钥匙.一捅.开了.
捅不开就被服务器拒绝了.有很多提高安全的方式.比如

* 更改SSH端口号.
* 禁止密码登录.
* 禁止ROOT登录.

别相信那些大的VPS提供商.国内最大的云.我开了防火墙.130多次就试出来密码了...8位大小写混合密码啊!!!

SSH之所以能够保证安全.原因在于它采用了公钥加密.整个过程是这样的：

* 远程主机收到用户的登录请求.把自己的公钥发给用户.
* 用户使用这个公钥.将登录密码加密后.发送回来.
* 远程主机用自己的私钥.解密登录密码.如果密码正确.就同意用户登录.

### 客户端
* Windows（SecureCRT / Xshell / Putty） 
* Linux （ssh）
* MAC (那个命令行叫啥来着)

<!-- more -->

### 登录

```bash
如果你是第一次通过ssh登录远程主机，会出现下面的提示：
ssh user@host
The authenticity of host 'host (120.18.49.21)' can't be established.
RSA key fingerprint is RSA key.
Are you sure you want to continue connecting (yes/no)?
```
这段话的意思是.无法确认host主机的真实性.只知道它的公钥指纹.问你还想继续连接吗？

所谓"公钥指纹".是指公钥长度较长（这里采用RSA算法，长达1024位）.所以对其进行MD5计算.将它变成一个128位的指纹.再进行比较.就容易多了.
很自然的一个问题就是.用户怎么知道远程主机的公钥指纹应该是多少？回答是：没有好的办法.远程主机必须在自己的网站上贴出公钥指纹.以便用户自行核对.

#### 制作钥匙
生成钥匙的命令都一样.后面带的命令没啥区别.反正个人用够了
```bash
ssh-keygen -t rsa
```
回车
```bash
Generating public/private rsa key pair.
Enter file in which to save the key (/root/.ssh/id_rsa): #<== 按 Enter
Created directory '/root/.ssh'.
Enter passphrase (empty for no passphrase): #<== 输入密钥锁码，或直接按 Enter 留空
Enter same passphrase again: #<== 再输入一遍密钥锁码
Your identification has been saved in /root/.ssh/id_rsa. #<== 私钥
Your public key has been saved in /root/.ssh/id_rsa.pub. #<== 公钥
The key fingerprint is: null
root@host
```

私钥设置口令（passphrase）这个嘛...如果是被害妄想症....你就设一个呗.本来刚刚你就生成了一个256位的密码.（设置私钥口令的目的是防止私钥被盗用）.但是设置之后.利用私钥连接也需要输入密码.

一般在$ROOT/.ssh/目录下.会生成两个文件：id_rsa.pub 公钥 和 id_rsa 私钥..ssh 目录的权限必须是 700.

sshd 服务器端默认的公钥文件是根目录.ssh下的 authorized_keys 文件.

```bash
cd /.ssh #进入目录
cat id_rsa.pub >> authorized_keys #钥匙给服务器
chmod 700 ~/.ssh #否则ssh服务器会拒绝登录。
chmod 600 ~/.ssh/authorized_keys #必须是600权限.否则ssh服务器会拒绝用户登陆。
```

经过以上两步之后.从此你再登录.就不需要输入密码了...此命令执行后.远程主机直接将用户的公钥保存在 ~/.ssh/authorized_keys 文件中

### 更改SSH CONFIG文件提高安全性
```bash
nano /etc/ssh/sshd_config
```

我比较喜欢用NANO.跟记事本一样的软件.VI是一个神奇的编辑器.至今不会用.

```bash
PermitRootLogin no  #禁止root登录 
RSAAuthentication yes  #开启证书登录
PubkeyAuthentication yes  #开启证书登录
AuthorizedKeysFile    .ssh/authorized_keys   #证书地址
PasswordAuthentication no   #禁用密码登陆.测试好再用这个.自己吃亏多次 
PermitEmptyPasswords no   #禁止空密码登陆
 
# 在确定你的公钥加入到authorzed_keys后重启sshd服务
/etc/init.d/sshd restart
```

### 下载私钥转换为 PuTTY 能使用的格式

用 WinSCP SFTP 等工具把私钥文件 id_rsa 下载回来.然后打开 PuTTYGen.Load私钥如果你刚才设置了密钥锁码.这时则需要输入.

在 Key comment 中键入对密钥的说明信息.然后单击 Save private key 按钮即可将私钥文件存放为 PuTTY 能使用的PPK格式.

当使用 PuTTY 登录时.在左侧的 Connection -> SSH -> Auth 中的 Private key file for authentication: 处选择你的私钥文件.登录过程中只需输入密钥锁密码.

PS:
* 出问题最多的地方就是钥匙地址.为了省事我喜欢用ROOT账户.如果是普通账户的话.SSH地址在~/用户名/.ssh
* PasswordAuthentication YES 小心别把自己关外面了.等登录的时候可以用密匙后.再改成NO.