# dockernotes
Note anything during writing docker

作者在Github的博客列表
- [Spark 笔记](http://devuser.github.io/spark-notes)
- [Docker 笔记](http://devuser.github.io/docker-notes)
- [Golang 笔记](http://devuser.github.io/golang-notes)

[Docker源代码深度探索](depth-of-docker.md)

[在腾讯云安装Docker](dockerontencentcloud.md)

Docker是Docker.Inc公司开源的一个基于轻量级虚拟化技术的容器引擎项目,整个项目基于Go语言开发，并遵从Apache 2.0协议。通过分层镜像标准化和内核虚拟化技术，Docker使得应用开发者和运维工程师可以以统一的方式跨平台发布应用，并且以几乎没有额外开销的情 况下提供资源隔离的应用运行环境。由于众多新颖的特性以及项目本身的开放性，Docker在不到两年的时间里迅速获得诸多IT厂商的参与，其中更是包括 Google、Microsoft、VMware等业界行业领导者。同时，Docker在开发者社区也是一石激起千层浪，许多如我之码农纷纷开始关注、学 习和使用Docker，许多企业，尤其是互联网企业，也在不断加大对Docker的投入，大有掀起一场容器革命之势。

Docker镜像命名解析

镜像是Docker最核心的技术之一，也是应用发布的标准格式。无论你是用docker pull image，或者是在Dockerfile里面写FROM image，从Docker官方Registry下载镜像应该是Docker操作里面最频繁的动作之一了。那么在我们执行docker pull image时背后到底发生了什么呢？在回答这个问题前，我们需要先了解下docker镜像是如何命名的，这也是Docker里面比较容易令人混淆的一块概念：Registry，Repository, Tag and Image。

下面是在本地机器运行docker images的输出结果：

![docker-images](http://static.oschina.net/uploads/img/201412/13141513_WXg7.jpg)

## Mac OS
从官网下载boot2docker 安装完毕后，点击屏幕右上角Spotlight搜索boot2docker，回车启动运行。

![Deploy](http://www.i5life.com:8090/docker-notes/images/Spotlight.png)

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

## 尝试第一个命令
- 从hub.docker下载或拉取centos镜像 `docker pull centos`
- 尝试启动第一个容器 `docker run -it --rm --name=firstcentos centos /bin/bash`
- 稍后进入容器的终端界面 `[root@bb40f7672bc6 /]#`
- 输入命令 `ls -all`看看下面有哪些文件或目录呢

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

## 安装curl
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

# 启动容器的参数说明
`docker run -it --rm --name=firstcentos centos /bin/bash`

# 推荐国内的DaoCloud
## 从国内的DaoCloud拉去Postgres的最新版本
`docker pull daocloud.io/library/postgres:latest`

启动一个 Postgres 实例

`docker run --name some-postgres -e POSTGRES_PASSWORD=mysecretpassword -d daocloud.io/postgres`

这个镜像会导出 Postgres 的 5432 端口, 因此通过标准的link机制就可以方便的访问 Postgres 数据库实例。 容器启动时会通过initdb自动创建默认的 postgres用户和数据库。 数据库postgres是可以被用户，工具和第三方应用程序访问的默认数据库。

从应用中连接数据库

   `docker run --name some-app --link some-postgres:postgres -d application-that-uses-postgres`

或者通过 psql

`docker run -it --link some-postgres:postgres --rm postgres sh -c 'exec psql -h "$POSTGRES_PORT_5432_TCP_ADDR" -p "$POSTGRES_PORT_5432_TCP_PORT" -U postgres`

环境变量

Postgres 镜像通过一系列环境变量来配置容器，虽然这些环境变量都不是必须的，但是它们会极大的方便您使用镜像。

`POSTGRES_PASSWORD`

推荐您使用镜像时指定这个环境变量，它用来设置超级用户的密码。 默认的超级用户是由环境变量POSTGRES_USER指定的。 在开始的例子中，超级用户密码被设置为 "mysecretpassword"。

`POSTGRES_USER`

这个可选的环境变量是搭配`POSTGRES_PASSWORD` 一起来设置用户名和密码的，它会创建一个指定名称的超级管理员和同名数据库。 如果没有设置这个环境变量，将使用默认值postgres。

如何扩展这个镜像

如果您希望在这个镜像的派生镜像中执行额外的初始化工作，可以在`/docker-entrypoint-init.d`目录下增加_.sh脚本 （如果该目录不存在则创建目录），在初始化过程调用initdb创建默认的postgres用户和数据库后，它会执行该目录下的所有_.sh脚本完成额外初始化操作再启动服务。

如果您希望在初始化中执行 SQL 语句，强烈建议您使用 `Postgres` 单用户模式。 您还可以通过一个简单的Dockerfile设置locale，下面这个例子将设置默认的locale为de_DE.utf8：

```
FROM daocloud.io/postgres:9.4
RUN localedef -i de_DE -c -f UTF-8 -A /usr/share/locale/locale.alias de_DE.UTF-8
ENV LANG de_DE.utf8
```

因为数据库初始化仅发生在容器启动时，因此您可以在数据库创建前设置语言。

注意

如果容器启动时没有数据库，Postgres 会为您创建一个默认数据库。虽然这是 Postgres 正常的行为，但它意味着在这个阶段数据库是不接受连接请求的。 这个行为会对一些自动化工具产生影响，比如 docker-compose 会同时启动多个容器。

支持的Docker版本

这个镜像在 Docker 1.7.0 上提供最佳的官方支持，对于其他老版本的 Docker（1.0 之后）也能提供基本的兼容。

# 尝试一把RabbitMQ
RabbitMQ 是开源的消息队列系统（或称消息中间件），它实现了高级的消息队列协议（AMQR）。RabbitMQ 服务端是由 Erlang 编写的，同时也是基于开放电信平台框架（OTP）开发的。客户端的接口则几乎兼容所有的主流语言。

`docker pull daocloud.io/library/rabbitmq:latest`

启动一个实例

RabbitMQ 通过节点名（通常是主机名）存储数据。所以我们启动 Docker 时需要设置`-h/--hostname`参数，这样可以让我们知道数据存在哪里。

```
=INFO REPORT==== 6-Jul-2015::20:47:02 ===
node           : rabbit@my-rabbit
home dir       : /var/lib/rabbitmq
config file(s) : /etc/rabbitmq/rabbitmq.config
cookie hash    : UoNOcDhfxW9uoZ92wh6BjA==
log            : tty
sasl log       : tty
database dir   : /var/lib/rabbitmq/mnesia/rabbit@my-rabbit
```

Erlang Cookie

节点之间使用 `cookie（关于`RabbitMQ`集群） 来判断是否通信，唯有`cookie` 相同的两个节点才能通信。

你可使用 `RABBITMQ_ERLANG_COOKIE` 来设置 `RabbitMQ` 实例的 `cookie` ：

```
docker run -d --hostname my-rabbit --name some-rabbit -e RABBITMQ_ERLANG_COOKIE='secret cookie here' daocloud.io/rabbitmq:3
```

设置完成后，通过`docker link`连接此 RabbitMQ 实例：

```
$ docker run -it --rm --link some-rabbit:my-rabbit -e RABBITMQ_ERLANG_COOKIE='secret cookie here' daocloud.io/rabbitmq:3 bash
root@f2a2d3d27c75:/# rabbitmqctl -n rabbit@my-rabbit list_users Listing users ...
guest   [administrator]
```

当然，你也可以设置 `RABBITMQ_NODENAME`,这样你就可以更好的使用 `rabbitmqctl` 命令了。

```
$ docker run -it --rm --link some-rabbit:my-rabbit -e RABBITMQ_ERLANG_COOKIE='secret cookie here' -e RABBITMQ_NODENAME=rabbit@my-rabbit daocloud.io/rabbitmq:3 bash
root@f2a2d3d27c75:/# rabbitmqctl list_users Listing users ...
guest   [administrator]
```

管理你的 RabbitMQ 服务

RabbitMQ 已经有一些自带管理插件的镜像。用这些镜像创建的容器实例可以直接使用默认的 15672 端口访问，默认账号密码是guest/guest：

`docker run -d --hostname my-rabbit --name some-rabbit daocloud.io/rabbitmq:3-management`

然后打开浏览器访问 [http://容器](http://容器) IP:15672 ，就可以管理你的 RabbitMQ 实例了，或者你可以暴露主机端口来访问：

`docker run -d --hostname my-rabbit --name some-rabbit -p 8080:15672 daocloud.io/rabbitmq:3-management`

这时，你可访问 [http://localhost:8080](http://localhost:8080) 或者 [http://宿主](http://宿主) IP:8080 管理 RabbitMQ 服务了。

连接 Daemon

`docker run --name some-app --link some-rabbit:rabbit -d application-that-uses-rabbitmq`

支持的Docker版本

这个镜像在 Docker 1.7.1 上提供最佳的官方支持，对于其他老版本的 Docker（1.0 之后）也能提供基本的兼容。
