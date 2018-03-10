---
title: PYTHON多版本控制 Anaconda教程
date: 2018-03-01 14:35:36
tags:
---
在GITHUB上有非常多的PYTHON代码.有很多有意思的.不过有的是3.X 有的是2.X 两个版本有明显区别.如何在一台主机上装2个版本.就需要Anaconda了.
打算学习 Python 来做数据分析的你，是不是在开始时就遇到各种麻烦呢？

### 选择Anaconda
## Anaconda介绍
Anaconda是专注于数据分析的Python发行版本.包含了conda、Python等190多个科学包及其依赖项。
## conda 
conda 是开源包（packages）和虚拟环境（environment）的管理系统
## Anaconda 的优点
秒切换版本.自动区分环境.

### 安装Anaconda
[可以anaconda.org从这里下载](https://www.anaconda.com/downloads) Anaconda 的安装程序以及查看安装说明.
支持 Windows、Linux 还是 MAC 的 OSX 系统.

Anaconda Navigtor ：用于管理工具包和环境的图形用户界面，后续涉及的众多管理命令也可以在 Navigator 中手工实现。
Jupyter notebook ：基于web的交互式计算环境，可以编辑易于人们阅读的文档，用于展示数据分析的过程。
qtconsole ：一个可执行 IPython 的仿终端图形界面程序，相比 Python Shell 界面，qtconsole 可以直接显示代码生成的图形，实现多行代码输入执行，以及内置许多有用的功能和函数。
spyder ：一个使用Python语言、跨平台的、科学运算集成开发环境。

安装完成后，我们还需要对所有工具包进行升级，以避免可能发生的错误。打开你电脑的终端，在命令行中输入：
```
conda upgrade --all           #在终端询问是否安装如下升级版本时，输入 y。
```

### 管理Python包
安装一个 package：
```
conda install package_name         #这里 package_name 是需要安装包的名称。
conda install numpy scipy pandas   #你也可以同时安装多个包，比如同时安装numpy 、scipy 和 pandas
conda install numpy=1.10           #你也可以指定安装的版本
conda remove package_name          #移除一个 package
conda update package_name          #升级 package 版本
conda list                         #查看所有的 packages
conda  search search_term          #模糊查询package
```
### 管理Python环境
```
conda create -n env_name  list of packages  #默认的环境是 root.也可以创建一个新环境
```
其中 -n 代表 name，env_name 是需要创建的环境名称，list of packages 则是列出在新环境中需要安装的工具包。

```
conda create -n py2 python=2.7 pandas       #创建 Python 2.7 环境
source activate env_name                    #进入 env_name 环境
source deactivate                           #退出当前环境
conda env remove -n env_name                #删除 env_name 环境
conda env list                              #显示所有环境
```

分享代码需要将运行环境分享给大家.执行export将环境的 package 信息存入名为 environment 的 YAML 文件中.
当执行他人的代码时.需要配置相应的环境.用对方分享的 YAML 文件来创建一摸一样的运行环境。
```
conda env export > environment.yaml
conda env create -f environment.yaml
```




mount -t vboxsf Share /mnt/shared




一个基于命令行的网易云音乐下载器。

安装
Git clone最新版
$ git clone https://github.com/ziwenxie/netease-dl
$ python3 setup.py install
PyPi安装
$ pip3 install netease-dl
p.s: 仅支持Python3.x。

功能特性
通过--help可以查看到所有的功能特性，包括下载单首歌曲，下载一张唱片的所有歌曲，下载一个歌手的前50首热门歌曲，下载一张歌单的所有歌曲，下载一个用户的公开歌单以及登录后可下载个人的私人歌单。

$ netease-dl --help
Usage: netease-dl [OPTIONS] COMMAND [ARGS]...

  A command tool to download NetEase-Music's songs.

Options:
  -t, --timeout INTEGER  Time to wait before giving up, in seconds.
  -p, --proxy TEXT       Use the specified HTTP/HTTPS/SOCKS proxy.
  -o, --output PATH      Specify the storage path.
  -q, --quiet            Automatically select the best one.
  -l, --lyric            Download lyric.
  -a, --again            Login Again.
  --help                 Show this message and exit.

Commands:
  album     Download a album's songs by name or id.
  artist    Download a artist's hot songs by name or id.
  me        Download my playlists.
  playlist  Download a playlist's songs by id.
  song      Download a song by name or id.
  user      Download a user's playlists by id.
使用
下载单首歌曲
使用song命令，在后面通过--name或者-n选项来指定歌曲的名字：

$ netease-dl song --name 歌曲名
上面会返回10条搜索结果，可以在song命令前面加一个--quiet，netease-dl会自动匹配第一个返回的结果：

$ netease-dl --quiet song --name 歌曲名
如果知道歌曲id的话，也可以直接使用--id或者-i选项来指定：

$ netease-dl song --id 歌曲id
netease-dl的所有子命令所支持的特性都可以通过在子命令后面加一个--help选项来查看：

$ netease-dl song --help
Usage: netease-dl song [OPTIONS]

  Download a song by name or id.

Options:
  -n, --name TEXT   Song name.
  -i, --id INTEGER  Song id.
  --help            Show this message and exit.
下载一个歌手的50首热门歌曲
使用artist命令，并且在后面通过--name或者-n选项来指定歌手的姓名：

$ netease-dl artist --name 歌手名
和上面下载歌曲的时候一样，也可以使用--quiet和--id，下面也是一样的原理，接下来我就不重复了。

下载一张唱片的所有歌曲
使用album命令，后面接--name或者-n选项来指定唱片的名字：

$ netease-dl album --name 唱片名
下载一张歌单的所有歌曲
使用playlist命令，后面接--name或者-n选项来指定歌单的名字：

$ netease-dl playlist --name 歌单名
下载指定用户的公开歌单
使用user命令，后面接--name或者-n选项来指定用户的名字：

$ netease-dl user --name 用户名
下载个人收藏以及创建的歌单
使用me命令登录之后可以下载自己的所有歌单包括私人的歌单，以后一段之间之内如果没有修改过密码就不需要重新登录了：

$ netease-dl me
如果要换一个帐号或者登录密码修改了，使用--again或者-a选项重新登录：

$ netease-dl --again me
更多选项
除了上面提到的--quiet选项，正如使用netease-dl --help选项看到的，netease-dl还支持设置代理，设置超时时间，指定下载目录，是否下载歌词等选项，这些都可以通过在子命令前面加上相关的选项来指定。

将歌曲下载到指定路径
使用--output或者-o选项指定下载路径：

$ netease-dl -o 路径名 artist -n 歌手名
设置代理
海外用户可能要设置相关的代理，netease-dl同时支持http和socks协议代理，可以通过--proxy或者-p选项指定，注意要声明代理所使用的协议：

$ netease-dl -p 'http://127.0.0.1:8118' artist -n 歌手名
$ netease-dl -p 'socks5://127.0.0.1:1080' artist -n 歌手名