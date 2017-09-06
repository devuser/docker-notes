#. 在Docker环境中交叉编译Gopher

假定您已经成功在Mac或Linux机器上安装了Docker环境。

使用如下命令`docker pull golang:1.6`下载golang 1.6版本的容器，目前具体是`1.6.2`

如下源代码保存在`$GOPATH`，尝试编译`$GOPATH`下的一个具体应用程序名字叫`BillProcess`

```
docker run --rm -it -v "$GOPATH":/usr/src/myapp -w /usr/src/myapp/src/goBillProcess -e GOOS=windows -e GOARCH=386 -e GOPATH=/usr/src/myapp -e GO15VENDOREXPERIMENT=1 golang:1.6 bash

for GOOS in windows;do for GOARCH in 386 amd64; do go build -v -o BillProcess-$GOOS-$GOARCH-v20160506.exe; done; done
```

如果您仅有一个应用程序在`$GOPATH`下，即有一个`main.go`在`$GOPATH/src`可以使用如下命令

```
docker run --rm -it -v "$GOPATH":/usr/src/myapp -w /usr/src/myapp/src -e GOOS=windows -e GOARCH=386 -e GOPATH=/usr/src/myapp -e GO15VENDOREXPERIMENT=1 golang:1.6 bash

export VERSION=`date -d today +"v2.4.0.%-Y%-m%-d%-k%M%S"`
for GOOS in windows;do for GOARCH in 386 amd64; do go build -v -o main-$GOOS-$GOARCH-vyyyymmdd.exe; done; done
for GOOS in windows;do for GOARCH in 386 amd64; do go build -v -ldflags "-s -w -X main._VERSION_="$VERSION  -o BillProcess-$GOOS-$GOARCH-v20160506.exe; done; done
```

本人使用编译命令如下

```
docker run --rm -it -v "$GOPATH":/usr/src/myapp -w /usr/src/myapp/src -e GOOS=windows -e GOARCH=386 -e GOPATH=/usr/src/myapp -e GO15VENDOREXPERIMENT=1 golang:1.6 bash

export VERSION=`date -d today +"v2.4.0.%-Y%-m%-d%-k%M%S"`
export TS=`date -d today +"%-Y%-m%-d %k%M%S"`
for GOOS in windows;do for GOARCH in 386 amd64; do go build -v -ldflags "-s -w -X main._VERSION_="$VERSION  -o BillProcess-$GOOS-$GOARCH-v$TS.exe; done; done


for GOOS in windows;do for GOARCH in 386 amd64; do go build -v -ldflags "-s -w -X main._VERSION_="$VERSION  -o dp-mock-server-$GOOS-$GOARCH-v$TS.exe; done; done
```

在启动容器时，默认设定目标操作系统Windows，系统架构386，可以简单理解成编译出来的程序适用于Windows XP 32位机器。
