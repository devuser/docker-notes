# dockernotes
Note anything during writing docker

## Mac OS
从官网下载boot2docker 安装完毕后，点击屏幕右上角Spotlight搜索boot2docker，回车启动运行。

[![Deploy](https://www.taolch.com/docker-notes/images/Spotlight.png)]

启动后打开两个Terminal窗口，其中一个白色，另外一个黄色。

注意其中一个Terminal窗口中显示一条提示

`eval "$(boot2docker shellinit)"`

记住这条命令，如果你打开新的终端话，需要复制这条命令到新终端，并执行， 实现环境变量的初始化。

有哪些环境变量呢
- 主机名称
- 授权文件夹
- 是否进行TLS校验

```
To connect the Docker client to the Docker daemon, please set:
    export DOCKER_HOST=tcp://192.168.59.103:2376
    export DOCKER_CERT_PATH=/Users/devuser/.boot2docker/certs/boot2docker-vm
    export DOCKER_TLS_VERIFY=1

Or run: `eval "$(boot2docker shellinit)"`
```

RUN yum install -y wget

# 腾讯云服务器Cent OS 6安装Docker指南
# 开始私人定制

```
docker build -t xiehuanang/sshd .
cd ../dockermysql;docker build -t xiehuanang/mysql .
```

常用的软件包

```
yum install -y vim curl wget
```

# 常用的镜像

```
docker pull debian
docker pull centos
docker pull ubuntu
docker pull mysql
docker pull mongo
docker pull redis
docker pull r-base
```

# 爬虫

```
docker pull binux/pyspider
```

# 玩点新鲜的

```
docker pull schickling/rust
```
