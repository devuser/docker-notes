dockernotes
===========

Note anything during writing docker

[在腾讯云安装Docker](dockerontencentcloud.md)

Mac OS
------

从官网下载boot2docker 安装完毕后，点击屏幕右上角Spotlight搜索boot2docker，回车启动运行。

[![Deploy](https://www.i5life.com/docker-notes/images/Spotlight.png)\]

启动后打开两个Terminal窗口，其中一个白色，另外一个黄色。

注意其中一个Terminal窗口中显示一条提示

`eval "$(boot2docker shellinit)"`

记住这条命令，如果你打开新的终端话，需要复制这条命令到新终端，并执行， 实现环境变量的初始化。

有哪些环境变量呢 - 主机名称 - 授权文件夹 - 是否进行TLS校验

```
To connect the Docker client to the Docker daemon, please set:
    export DOCKER_HOST=tcp://192.168.59.103:2376
    export DOCKER_CERT_PATH=/Users/devuser/.boot2docker/certs/boot2docker-vm
    export DOCKER_TLS_VERIFY=1

Or run: `eval "$(boot2docker shellinit)"`
```

尝试第一个命令
--------------

-	从hub.docker下载或拉取centos镜像 `docker pull centos`
-	尝试启动第一个容器 `docker run -it --rm --name=firstcentos centos /bin/bash`
-	稍后进入容器的终端界面 `[root@bb40f7672bc6 /]#`
-	输入命令 `ls -all`看看下面有哪些文件或目录呢

```
[root@bb40f7672bc6 /]# ls -all
total 56
drwxr-xr-x  24 root root 4096 Sep 30 02:08 .
drwxr-xr-x  24 root root 4096 Sep 30 02:08 ..
-rwxr-xr-x   1 root root    0 Sep 30 02:08 .dockerenv
-rwxr-xr-x   1 root root    0 Sep 30 02:08 .dockerinit
lrwxrwxrwx   1 root root    7 Aug 14 21:00 bin -> usr/bin
drwxr-xr-x   5 root root  380 Sep 30 02:08 dev
drwxr-xr-x  46 root root 4096 Sep 30 02:08 etc
drwxr-xr-x   2 root root 4096 Jun 10  2014 home
lrwxrwxrwx   1 root root    7 Aug 14 21:00 lib -> usr/lib
lrwxrwxrwx   1 root root    9 Aug 14 21:00 lib64 -> usr/lib64
drwx------   2 root root 4096 Aug 14 21:00 lost+found
drwxr-xr-x   2 root root 4096 Jun 10  2014 media
drwxr-xr-x   2 root root 4096 Jun 10  2014 mnt
drwxr-xr-x   2 root root 4096 Jun 10  2014 opt
dr-xr-xr-x 120 root root    0 Sep 30 02:08 proc
dr-xr-x---   2 root root 4096 Aug 14 21:05 root
drwxr-xr-x   2 root root 4096 Aug 14 21:00 run
lrwxrwxrwx   1 root root    8 Aug 14 21:00 sbin -> usr/sbin
drwxr-xr-x   2 root root 4096 Jun 10  2014 srv
dr-xr-xr-x  13 root root    0 Sep 30 02:08 sys
drwxrwxrwt   7 root root 4096 Sep 30 02:08 tmp
drwxr-xr-x  13 root root 4096 Aug 14 21:00 usr
drwxr-xr-x  19 root root 4096 Aug 14 21:05 var
```

默认登陆到容器中，是在根目录下，可以看到我们非常熟悉的几个Linux目录 - bin - dev - etc（配置文件可能保存在这里） - home - var (后面安装MySQL会用到这个目录) - root (后面我们会提到修改profile，创建 _ .ssh _ 目录都在这里)

安装curl
--------

在终端中输入命令`yum install -y curl` 下载并安装curl软件包。

BTW，这里的－y参数有什么作用呢？请百度。

```
[root@bb40f7672bc6 /]# yum install -y curl
Loaded plugins: fastestmirror
base                                                                                | 3.6 kB  00:00:00
extras                                                                              | 3.4 kB  00:00:00
systemdcontainer                                                                    | 1.9 kB  00:00:00
updates                                                                             | 3.4 kB  00:00:00
(1/4): extras/7/x86_64/primary_db                                                   |  87 kB  00:00:00
(2/4): base/7/x86_64/group_gz                                                       | 154 kB  00:00:02
(3/4): updates/7/x86_64/primary_db                                                  | 4.0 MB  00:00:06
(4/4): base/7/x86_64/primary_db                                                     | 5.1 MB  00:00:07
systemdcontainer/primary_db                                                         |  20 kB  00:00:00
Determining fastest mirrors
 * base: mirrors.yun-idc.com
 * extras: mirrors.aliyun.com
 * updates: mirrors.yun-idc.com
Package curl-7.29.0-19.el7.x86_64 already installed and latest version
Nothing to do
```

体验一个新的调用yum的方式，一次安装多个软件包`yum install -y vim wget`

输入`wget`可以看到wget已经安装成功。

```
wget
wget: missing URL
Usage: wget [OPTION]... [URL]...

Try `wget --help' for more options.
```

启动容器的参数说明
==================

`docker run -it --rm --name=firstcentos centos /bin/bash`
