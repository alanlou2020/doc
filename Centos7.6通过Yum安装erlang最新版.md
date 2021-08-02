#### Centos7.6通过Yum安装erlang最新版

#####  1、卸载Erlang和RabbitMQ

##### 1.1、卸载erlang

```shell
yum list | grep erlang
yum -y remove erlang-*
rm -rf /usr/lib64/erlang
```

##### 1.2、卸载RabbitMQ

```shell
yum list | grep rabbitmq
yum -y remove rabbitmq-server.noarch
```

##### 2、添加CentOS本地yum源

```shell
vim /etc/yum.repos.d/erlang_solutions.repo
```

> 内容如下：

```properties
[erlang-solutions]
name=CentOS $releasever - $basearch - Erlang Solutions
baseurl=https://packages.erlang-solutions.com/rpm/centos/$releasever/$basearch
gpgcheck=1
gpgkey=https://packages.erlang-solutions.com/rpm/erlang_solutions.asc
enabled=1
```

##### 3、安装erlang

```shell
yum install erlang -y
```

