### Centos7单机搭建多实例RabbitMQ高可用集群

> ==注意：此集群方案主节点不能挂，主节点挂了，所有的从节点也都不可用了。==

##### 1、在Centos上安装好RabbitMQ，并在防火墙中开放端口：5672,15672等端口，此过程略过。。。

##### 2、安装完成后停止RabbitMQ服务：

```shell
systemctl stop rabbitmq-server
```

##### 3、启动第一个节点rabbit-1：

```shell
sudo RABBITMQ_NODE_PORT=5672 RABBITMQ_NODENAME=rabbit-1 rabbitmq-server start &
```

> 结束命令为：rabbitmqctl -n rabbit-1 stop

##### 4、启动第二个节点rabbit-2：

> 注意：web管理插件端口占用，所以还要指定其web插件占用的端口号：
>
> RABBITMQ_SERVER_START_ARGS="-rabbitmq_management listener [{port,15673}]"

```shell
sudo RABBITMQ_NODE_PORT=5673 RABBITMQ_SERVER_START_ARGS="-rabbitmq_management listener [{port,15673}]" RABBITMQ_NODENAME=rabbit-2 rabbitmq-server start &
```

> 结束命令为：rabbitmqctl -n rabbit-2 stop

##### 5、启动第三个节点rabbit-3：

> 注意：web管理插件端口占用，所以还要指定其web插件占用的端口号：
>
> RABBITMQ_SERVER_START_ARGS="-rabbitmq_management listener [{port,15674}]"

```shell
sudo RABBITMQ_NODE_PORT=5674 RABBITMQ_SERVER_START_ARGS="-rabbitmq_management listener [{port,15674}]" RABBITMQ_NODENAME=rabbit-3 rabbitmq-server start &
```

> 结束命令为：rabbitmqctl -n rabbit-3 stop

##### 6、验证启动是否正常：

```shell
[root@alanCentos01 /]# ps aux | grep rabbit
root      74830  0.0  0.3 274508  5680 pts/0    S    17:55   0:00 sudo RABBITMQ_NODE_PORT=5672 RABBITMQ_NODENAME=rabbit-1 rabbitmq-server start
root      74831  0.0  0.0 146400  1612 pts/0    S    17:55   0:00 /sbin/runuser -u rabbitmq -- /usr/lib/rabbitmq/bin/rabbitmq-server start
rabbitmq  74839  0.0  0.0 113176  1544 pts/0    S    17:55   0:00 /bin/sh /usr/lib/rabbitmq/bin/rabbitmq-server start
rabbitmq  74846  1.0  4.5 2274216 84748 pts/0   Sl   17:55   0:11 /usr/lib64/erlang/erts-12.0.3/bin/beam.smp -W w -MBas ageffcbf -MHas ageffcbf -MBlmbcs 512 -MHlmbcs 512 -MMmcs 30 -P 1048576 -t 5000000 -stbt db -zdbbl 128000 -sbwt none -sbwtdcpu none -sbwtdio none -B i -- -root /usr/lib64/erlang -progname erl -- -home /var/lib/rabbitmq -- -pa  -noshell -noinput -s rabbit boot -boot start_sasl -lager crash_log false -lager handlers [] start
rabbitmq  74853  0.0  0.0   4352   560 ?        Ss   17:55   0:00 erl_child_setup 1024
rabbitmq  74878  0.0  0.0  11628   340 ?        S    17:55   0:00 /usr/lib64/erlang/erts-12.0.3/bin/epmd -daemon
rabbitmq  74901  0.0  0.0  11588   456 ?        Ss   17:55   0:00 inet_gethost 4
rabbitmq  74902  0.0  0.0  41536  1228 ?        S    17:55   0:00 inet_gethost 4
root      76905  0.0  0.3 274508  5680 pts/0    S    18:12   0:00 sudo RABBITMQ_NODE_PORT=5673 RABBITMQ_SERVER_START_ARGS=-rabbitmq_management listener [{port,15673}] RABBITMQ_NODENAME=rabbit-2 rabbitmq-server start
root      76906  0.0  0.0 146400  1612 pts/0    S    18:12   0:00 /sbin/runuser -u rabbitmq -- /usr/lib/rabbitmq/bin/rabbitmq-server start
rabbitmq  76914  0.0  0.0 113176  1548 pts/0    S    18:12   0:00 /bin/sh /usr/lib/rabbitmq/bin/rabbitmq-server start
rabbitmq  76921  7.4  4.4 2281168 82360 pts/0   Sl   18:12   0:07 /usr/lib64/erlang/erts-12.0.3/bin/beam.smp -W w -MBas ageffcbf -MHas ageffcbf -MBlmbcs 512 -MHlmbcs 512 -MMmcs 30 -P 1048576 -t 5000000 -stbt db -zdbbl 128000 -sbwt none -sbwtdcpu none -sbwtdio none -B i -- -root /usr/lib64/erlang -progname erl -- -home /var/lib/rabbitmq -- -pa  -noshell -noinput -s rabbit boot -boot start_sasl -rabbitmq_management listener [{port,15673}] -lager crash_log false -lager handlers [] start
rabbitmq  76928  0.0  0.0   4352   556 ?        Ss   18:12   0:00 erl_child_setup 1024
rabbitmq  76976  0.0  0.0  11588   456 ?        Ss   18:12   0:00 inet_gethost 4
rabbitmq  76977  0.0  0.0  41536  1228 ?        S    18:12   0:00 inet_gethost 4
root      77228  0.0  0.0 112724   988 pts/0    R+   18:14   0:00 grep --color=auto rabbit
```

##### 7、停止、重置、启动第一个节点（rabbit-1）的应用：

> 目的是清除节点上的历史数据（如果不清除，无法将节点加入到集群）

```shell
[root@alanCentos01 /]# sudo rabbitmqctl -n rabbit-1 stop_app
Stopping rabbit application on node rabbit-1@alanCentos01 ...
[root@alanCentos01 /]# sudo rabbitmqctl -n rabbit-1 reset
Resetting node rabbit-1@alanCentos01 ...
[root@alanCentos01 /]# sudo rabbitmqctl -n rabbit-1 start_app
Starting node rabbit-1@alanCentos01 ...

  ##  ##      RabbitMQ 3.8.19
  ##  ##
  ##########  Copyright (c) 2007-2021 VMware, Inc. or its affiliates.
  ######  ##
  ##########  Licensed under the MPL 2.0. Website: https://rabbitmq.com

  Erlang:      24.0.4 [emu]
  TLS Library: OpenSSL - OpenSSL 1.0.2k-fips  26 Jan 2017

  Doc guides:  https://rabbitmq.com/documentation.html
  Support:     https://rabbitmq.com/contact.html
  Tutorials:   https://rabbitmq.com/getstarted.html
  Monitoring:  https://rabbitmq.com/monitoring.html

  Logs: /var/log/rabbitmq/rabbit-1@alanCentos01.log
        /var/log/rabbitmq/rabbit-1@alanCentos01_upgrade.log

  Config file(s): (none)

  Starting broker... completed with 4 plugins.
[root@alanCentos01 /]# 
```

##### 8、停止、重置、将节点rabbit-2加入主节点rabbit-1中（组成集群），其中[alanCentos01是服务器的主机名]，最后启动节点rabbit-2的应用：

> 目的是清除节点上的历史数据（如果不清除，无法将节点加入到集群）
>
> ==注意：每台机器的主机名都不一样，可自行根据[root@alanCentos01 /]中“@”后面的就是主机名了==

```shell
[root@alanCentos01 /]# sudo rabbitmqctl -n rabbit-2 stop_app
Stopping rabbit application on node rabbit-2@alanCentos01 ...
[root@alanCentos01 /]# sudo rabbitmqctl -n rabbit-2 reset
Resetting node rabbit-2@alanCentos01 ...
[root@alanCentos01 /]# sudo rabbitmqctl -n rabbit-2 join_cluster rabbit-1@alanCentos01
Clustering node rabbit-2@alanCentos01 with rabbit-1@alanCentos01
[root@alanCentos01 /]# sudo rabbitmqctl -n rabbit-2 start_app
Starting node rabbit-2@alanCentos01 ...

  ##  ##      RabbitMQ 3.8.19
  ##  ##
  ##########  Copyright (c) 2007-2021 VMware, Inc. or its affiliates.
  ######  ##
  ##########  Licensed under the MPL 2.0. Website: https://rabbitmq.com

  Erlang:      24.0.4 [emu]
  TLS Library: OpenSSL - OpenSSL 1.0.2k-fips  26 Jan 2017

  Doc guides:  https://rabbitmq.com/documentation.html
  Support:     https://rabbitmq.com/contact.html
  Tutorials:   https://rabbitmq.com/getstarted.html
  Monitoring:  https://rabbitmq.com/monitoring.html

  Logs: /var/log/rabbitmq/rabbit-2@alanCentos01.log
        /var/log/rabbitmq/rabbit-2@alanCentos01_upgrade.log

  Config file(s): (none)

  Starting broker... completed with 4 plugins.
[root@alanCentos01 /]# 
```

##### 9、停止、重置、将节点rabbit-3加入主节点rabbit-1中（组成集群），其中[alanCentos01是服务器的主机名]，最后启动节点rabbit-3的应用：

```shell
[root@alanCentos01 /]# sudo rabbitmqctl -n rabbit-3 stop_app
Stopping rabbit application on node rabbit-2@alanCentos01 ...
[root@alanCentos01 /]# sudo rabbitmqctl -n rabbit-3 reset
Resetting node rabbit-2@alanCentos01 ...
[root@alanCentos01 /]# sudo rabbitmqctl -n rabbit-3 join_cluster rabbit-1@alanCentos01
Clustering node rabbit-2@alanCentos01 with rabbit-1@alanCentos01
[root@alanCentos01 /]# sudo rabbitmqctl -n rabbit-3 start_app
Starting node rabbit-3@alanCentos01 ...

  ##  ##      RabbitMQ 3.8.19
  ##  ##
  ##########  Copyright (c) 2007-2021 VMware, Inc. or its affiliates.
  ######  ##
  ##########  Licensed under the MPL 2.0. Website: https://rabbitmq.com

  Erlang:      24.0.4 [emu]
  TLS Library: OpenSSL - OpenSSL 1.0.2k-fips  26 Jan 2017

  Doc guides:  https://rabbitmq.com/documentation.html
  Support:     https://rabbitmq.com/contact.html
  Tutorials:   https://rabbitmq.com/getstarted.html
  Monitoring:  https://rabbitmq.com/monitoring.html

  Logs: /var/log/rabbitmq/rabbit-3@alanCentos01.log
        /var/log/rabbitmq/rabbit-3@alanCentos01_upgrade.log

  Config file(s): (none)

  Starting broker... completed with 4 plugins.
[root@alanCentos01 /]#
```

##### 10、查看集群状态

```shell
[root@alanCentos01 /]# sudo rabbitmqctl cluster_status -n rabbit-1
Cluster status of node rabbit-1@alanCentos01 ...
Basics

Cluster name: rabbit-1@alanCentos01

Disk Nodes

rabbit-1@alanCentos01
rabbit-2@alanCentos01
rabbit-3@alanCentos01

Running Nodes

rabbit-1@alanCentos01
rabbit-2@alanCentos01
rabbit-3@alanCentos01

Versions

rabbit-1@alanCentos01: RabbitMQ 3.8.19 on Erlang 24.0.4
rabbit-2@alanCentos01: RabbitMQ 3.8.19 on Erlang 24.0.4
rabbit-3@alanCentos01: RabbitMQ 3.8.19 on Erlang 24.0.4

Maintenance status

Node: rabbit-1@alanCentos01, status: not under maintenance
Node: rabbit-2@alanCentos01, status: not under maintenance
Node: rabbit-3@alanCentos01, status: not under maintenance

Alarms

(none)

Network Partitions

(none)

Listeners

Node: rabbit-1@alanCentos01, interface: [::], port: 15672, protocol: http, purpose: HTTP API
Node: rabbit-1@alanCentos01, interface: [::], port: 25672, protocol: clustering, purpose: inter-node and CLI tool communication
Node: rabbit-1@alanCentos01, interface: [::], port: 5672, protocol: amqp, purpose: AMQP 0-9-1 and AMQP 1.0
Node: rabbit-2@alanCentos01, interface: [::], port: 15673, protocol: http, purpose: HTTP API
Node: rabbit-2@alanCentos01, interface: [::], port: 25673, protocol: clustering, purpose: inter-node and CLI tool communication
Node: rabbit-2@alanCentos01, interface: [::], port: 5673, protocol: amqp, purpose: AMQP 0-9-1 and AMQP 1.0
Node: rabbit-3@alanCentos01, interface: [::], port: 15674, protocol: http, purpose: HTTP API
Node: rabbit-3@alanCentos01, interface: [::], port: 25674, protocol: clustering, purpose: inter-node and CLI tool communication
Node: rabbit-3@alanCentos01, interface: [::], port: 5674, protocol: amqp, purpose: AMQP 0-9-1 and AMQP 1.0

Feature flags

Flag: drop_unroutable_metric, state: enabled
Flag: empty_basic_get_metric, state: enabled
Flag: implicit_default_bindings, state: enabled
Flag: maintenance_mode_status, state: enabled
Flag: quorum_queue, state: enabled
Flag: user_limits, state: enabled
Flag: virtual_host_metadata, state: enabled
[root@alanCentos01 /]# 
```

##### 11、开发防火墙端口：

```shell
[root@alanCentos01 /]# firewall-cmd --permanent --add-port=5672/tcp --zone=public
success
[root@alanCentos01 /]# firewall-cmd --permanent --add-port=5673/tcp --zone=public
success
[root@alanCentos01 /]# firewall-cmd --permanent --add-port=5674/tcp --zone=public
success
[root@alanCentos01 /]# firewall-cmd --permanent --add-port=15672/tcp --zone=public
success
[root@alanCentos01 /]# firewall-cmd --permanent --add-port=15673/tcp --zone=public
success
[root@alanCentos01 /]# firewall-cmd --permanent --add-port=15674/tcp --zone=public
success
[root@alanCentos01 /]# firewall-cmd --permanent --add-port=25672/tcp --zone=public
success
[root@alanCentos01 /]# firewall-cmd --permanent --add-port=25673/tcp --zone=public
success
[root@alanCentos01 /]# firewall-cmd --permanent --add-port=25674/tcp --zone=public
success
[root@alanCentos01 /]# firewall-cmd --permanent --add-port=4369/tcp --zone=public
success
[root@alanCentos01 /]# firewall-cmd --reload
success
[root@alanCentos01 /]# firewall-cmd --list-ports
8080/tcp 6868/tcp 20/tcp 21/tcp 22/tcp 80/tcp 8888/tcp 39000-40000/tcp 5762/tcp 15672/tcp 5672/tcp 5673/tcp 15673/tcp 4369/tcp 25672/tcp 25673/tcp
```

##### 12、需重新添加用户及授权

###### 12.1、创建账号

```shell
rabbitmqctl -n rabbit-1 add_user admin 123456
rabbitmqctl -n rabbit-2 add_user admin 123456
rabbitmqctl -n rabbit-3 add_user admin 123456
```

###### 12.2、设置用户角色

```shell
rabbitmqctl -n rabbit-1 set_user_tags admin administrator
rabbitmqctl -n rabbit-2 set_user_tags admin administrator
rabbitmqctl -n rabbit-3 set_user_tags admin administrator
```

###### 12.3、设置用户权限

```shell
rabbitmqctl -n rabbit-1 set_permissions -p "/" admin ".*" ".*" ".*"
rabbitmqctl -n rabbit-2 set_permissions -p "/" admin ".*" ".*" ".*"
rabbitmqctl -n rabbit-3 set_permissions -p "/" admin ".*" ".*" ".*"
```

###### 12.4、显示当前用户和角色

```shell
rabbitmqctl -n rabbit-1 list_users
rabbitmqctl -n rabbit-2 list_users
rabbitmqctl -n rabbit-3 list_users
```

##### 13、启动rabbitmq的web图形化界面管理插件

```shell
rabbitmq-plugins -n rabbit-1 enable rabbitmq_management
rabbitmq-plugins -n rabbit-2 enable rabbitmq_management
rabbitmq-plugins -n rabbit-3 enable rabbitmq_management

rabbitmq-plugins -n rabbit-1 enable rabbitmq_delayed_message_exchange
rabbitmq-plugins -n rabbit-2 enable rabbitmq_delayed_message_exchange
rabbitmq-plugins -n rabbit-3 enable rabbitmq_delayed_message_exchange
```

> ==注意：Centos重启后，要再次启动集群，需执行下面语句（如果启动过程中一直提示：“Starting broker...”，不用管继续执行后面的语句，即可）：==
>
> ``` shell
> sudo RABBITMQ_NODE_PORT=5672 RABBITMQ_NODENAME=rabbit-1 rabbitmq-server start &
> sudo RABBITMQ_NODE_PORT=5673 RABBITMQ_SERVER_START_ARGS="-rabbitmq_management listener [{port,15673}]" RABBITMQ_NODENAME=rabbit-2 rabbitmq-server start &
> sudo RABBITMQ_NODE_PORT=5674 RABBITMQ_SERVER_START_ARGS="-rabbitmq_management listener [{port,15674}]" RABBITMQ_NODENAME=rabbit-3 rabbitmq-server start &
> ```

#### 14、配置镜像队列

~~~shell
rabbitmqctl -n rabbit-1 set_policy ha-all "^mirror" '{"ha-mode":"all","ha-sync-mode":"automatic"}'
rabbitmqctl -n rabbit-2 set_policy ha-all "^mirror" '{"ha-mode":"all","ha-sync-mode":"automatic"}'
rabbitmqctl -n rabbit-2 set_policy ha-all "^mirror" '{"ha-mode":"all","ha-sync-mode":"automatic"}'
~~~

> ==注意：以上语句为每个节点都做了一个镜像，新建的交换机和队列命名一定以“mirror”开头，以匹配正则表达式"^mirror"，否则镜像无效。==

##### 15、多机器集群多做下面几部，其他和上述一样，最多改下@的后面的主机名

###### 15.1、修改 3 台机器的主机名称 

```shell
vim /etc/hostname 
```

###### 15.2、配置各个节点的 hosts 文件，让各个节点都能互相识别对方

```shell
vim /etc/hosts 
10.211.55.74 node1 
10.211.55.75 node2 
10.211.55.76 node3
```

##### 15.3、以确保各个节点的 cookie 文件使用的是同一个值，在 node1 上执行远程操作命令，如下：

```shell
scp /var/lib/rabbitmq/.erlang.cookie root@node2:/var/lib/rabbitmq/.erlang.cookie 
scp /var/lib/rabbitmq/.erlang.cookie root@node3:/var/lib/rabbitmq/.erlang.cookie
```

##### 15.4、后面只要把第二、三台机器的rabbit-1节点都加到第一个主节点上去就可以了（每台机器的rabbit-2、rabbit-3都加到各自的rabbit-1中）。

##### 完成！！！

