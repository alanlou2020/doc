### CentOS7下源码包方式安装Erlang

##### 1、官网上下载源码包：OTP 24 Source File 

地址：https://www.erlang.org/downloads/24.0

##### 2、把源码放在/opt目录中，解压，如下： 

```shell
tar -zxvf otp_src_24.0.tar.gz
```

##### 3、准备环境：

```shell
yum -y install make gcc gcc-c++ kernel-devel m4 ncurses-devel openssl-devel
```

##### 4、设置配置项：

```shell
cd /opt/otp_src_24.0
mkdir /usr/local/erlang
./configure --prefix=/usr/local/erlang --without-javac
```

##### 5、编译：

```shell
make && make install
```

##### 6、创建链接：

```shell
ln -s /usr/local/erlang/bin/erl /usr/local/bin/erl
```

##### 7、配置环境变量：

```shell
vim /etc/profile
```

###### 7.1、在profile文件最后加入如下配置：

```properties
#set erlang environment
ERL_PATH=/usr/local/erlang/bin
PATH=$ERL_PATH:$PATH
```

###### 7.2、使配置生效，执行命令如下：

```shell
source /etc/profile
```

##### 8、检查是否安装成功，执行命令如下：

```shell
[root@alancentos /]# erl
Erlang/OTP 24 [erts-12.0] [source] [64-bit] [smp:4:4] [ds:4:4:10] [async-threads:1]

Eshell V12.0  (abort with ^G)
1> 
```

