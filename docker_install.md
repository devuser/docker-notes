#. 配置PTS开发的Docker环境

在.bash_profile中增加如下几行
```
export YOUR_COMPANY=XXXX
alias zero="docker run -m 2048m -t -i --rm -v $HOME/working:/data/$YOUR_COMPANY -p 58081:8081 -p 59001:9001 -p 50080:80 -p 58080:8080 -p 50021:21 -p 50022:22 --name=gopherzero --link some-mysql:mysql --link javazero:tomcat xiehuanang/gopher /bin/bash"
# ptsenvzero容器作为PTS开发使用，端口号加30000
alias javazero="docker run -m 4096m -it --rm -v $HOME/working:/data/$YOUR_COMPANY -v $HOME/.m2:/root/.m2 -v $HOME/.ivy2:/root/.ivy2 -p 28080:8080 -p 29000:9000 --link some-mysql:mysql --name javazero xiehuanang/ptsenv /bin/bash"
alias alpinezero="docker run -m 4096m -it --rm -v $HOME/working:/data/$YOUR_COMPANY -v $HOME/.m2:/root/.m2 -v $HOME/.ivy2:/root/.ivy2 -p 38080:8080 -p 39000:9000 --link some-mysql:mysql --name alpinezero xiehuanang/ptsenvalpinegrails3 /bin/sh"
```


装载容器或镜像

```
cat some-mysql.tar | sudo docker import - mysql:latest
docker load < xiehuanang_ptsenv.tar

docker load < xiehuanang_gopher.tar

docker images
```

启动MySQL容器
```
docker run --name some-mysql -e MYSQL_ROOT_PASSWORD=root -v $HOME/working/PythonBillProcess/sql:/data/sql -d mysql:5.6
```

进入容器创建数据库
```
docker exec -it some-mysql bash
mysql -u root --password=root
create database rbbocpts_development;
create database rbbocpts_test;
create database rbbocpts_production;
```

在PythonBillProcess/sql文件夹找到对应的创建数据库表的sql文件，比如`foo.sql`
回到MySQL容器，注意启动容器时把PythonBillProcess/sql映射到`data/$YOUR_COMPANY/sql`
```
cd /data/$YOUR_COMPANY/sql
mysql -u root --password=root
use rbbocpts_development;
\. foo.sql
use rbbocpts_test;
\. foo.sql
use rbbocpts_production;
\. foo.sql
```
