#### yum安装erlang

##### 1、#卸载erlang

```shell
yum -y remove erlang-*
```

##### 2、使用存储库安装

```shell
wget https://packages.erlang-solutions.com/erlang-solutions-2.0-1.noarch.rpm
rpm -Uvh erlang-solutions-2.0-1.noarch.rpm
```

##### 3、手动添加存储库条目

```shell
rpm --import https://packages.erlang-solutions.com/rpm/erlang_solutions.asc
```

##### 4、添加到“/etc/yum.repos.d/”目录文件中

```shell
vim erlang_solutions.repo
```

###### 4.1、内容如下：

```properties
[erlang-solutions]
name=CentOS $releasever - $basearch - Erlang Solutions
baseurl=https://packages.erlang-solutions.com/rpm/centos/$releasever/$basearch
gpgcheck=1
gpgkey=https://packages.erlang-solutions.com/rpm/erlang_solutions.asc
enabled=1
```

##### 5、查看erlang可安装版本

```shell
yum list |grep erlang
yum list erlang --showduplicates | sort -r
```

##### 6、安装erlang,也可安装指定版本

```shell
yum install -y erlang
```

##### 7、安装erlang指定版本

```shell
yum install erlang-24.0-1.el7.aarch64
```

