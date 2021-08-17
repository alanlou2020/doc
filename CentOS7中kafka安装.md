### CentOS7中kafka安装及详细说明

##### 1、安装zookeeper

###### 1.1、解压apache-zookeeper-3.5.9-bin.tar.gz

 ```shell
 tar -zxvf apache-zookeeper-3.5.9-bin.tar.gz
 ```

 ###### 1.2、创建及修改配置文件

 ```shell
 cd  apache-zookeeper-3.5.9-bin/conf/
 cp zoo_sample.cfg zoo.cfg
 vim zoo.cfg
 ```

> 修改zoo.cfg配置如下：
>
> ```properties
> # The number of milliseconds of each tick
> tickTime=2000
> # The number of ticks that the initial 
> # synchronization phase can take
> initLimit=10
> # The number of ticks that can pass between 
> # sending a request and getting an acknowledgement
> syncLimit=5
> # the directory where the snapshot is stored.
> # do not use /tmp for storage, /tmp here is just 
> # example sakes.
> dataDir=/opt/zookeeper/apache-zookeeper-3.5.9-bin
> # the port at which the clients will connect
> clientPort=2181
> # the maximum number of client connections.
> # increase this if you need to handle more clients
> #maxClientCnxns=60
> #
> # Be sure to read the maintenance section of the 
> # administrator guide before turning on autopurge.
> #
> # http://zookeeper.apache.org/doc/current/zookeeperAdmin.html#sc_maintenance
> #
> # The number of snapshots to retain in dataDir
> #autopurge.snapRetainCount=3
> # Purge task interval in hours
> # Set to "0" to disable auto purge feature
> #autopurge.purgeInterval=1
> ```

###### 1.3、开放防火墙端口

```shell
firewall-cmd --permanent --add-port=2181/tcp --zone=public
firewall-cmd --reload
```

###### 1.4、增加环境变量

```shell
vim /etc/profile
```

> 在配置文件最下方添加配置如下：
>
> ```properties
> #zookeeper config start
> export ZOOKEPPER_HOME=/opt/zookeeper/apache-zookeeper-3.5.9-bin
> export PATH=$ZOOKEPPER_HOME/bin:$PATH
> #zookeeper config end
> ```
>
> 使配置文件生效：
>
> ```shell
> source /etc/profile
> ```

###### 1.5、启动zookeeper服务

```shell
zkServer.sh start
zkServer.sh status #查看zookeeper服务状态
zkServer.sh stop #停止zookeeper服务
```

##### 2、安装kafka

###### 2.1、解压kafka_2.13-2.8.0.tgz

```shell
tar -zxvf kafka_2.13-2.8.0.tgz
```

###### 2.2、修改配置文件

```shell
cd kafka_2.13-2.8.0/
mkdir logs #创建日志文件夹
mkdir data #创建数据文件夹
cd config
vim server.properties
```

```properties
# Licensed to the Apache Software Foundation (ASF) under one or more
# contributor license agreements.  See the NOTICE file distributed with
# this work for additional information regarding copyright ownership.
# The ASF licenses this file to You under the Apache License, Version 2.0
# (the "License"); you may not use this file except in compliance with
# the License.  You may obtain a copy of the License at
#
#    http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

# see kafka.server.KafkaConfig for additional details and defaults

############################# Server Basics #############################

# The id of the broker. This must be set to a unique integer for each broker.
broker.id=0

#删除 topic 功能使能 
delete.topic.enable=true

############################# Socket Server Settings #############################

# The address the socket server listens on. It will get the value returned from 
# java.net.InetAddress.getCanonicalHostName() if not configured.
#   FORMAT:
#     listeners = listener_name://host_name:port
#   EXAMPLE:
#     listeners = PLAINTEXT://your.host.name:9092
#listeners=PLAINTEXT://:9092
listeners=PLAINTEXT://localhost:9092

# Hostname and port the broker will advertise to producers and consumers. If not set, 
# it uses the value for "listeners" if configured.  Otherwise, it will use the value
# returned from java.net.InetAddress.getCanonicalHostName().
#advertised.listeners=PLAINTEXT://your.host.name:9092

# Maps listener names to security protocols, the default is for them to be the same. See the config documentation for more details
#listener.security.protocol.map=PLAINTEXT:PLAINTEXT,SSL:SSL,SASL_PLAINTEXT:SASL_PLAINTEXT,SASL_SSL:SASL_SSL

# The number of threads that the server uses for receiving requests from the network and sending responses to the network
num.network.threads=3

# The number of threads that the server uses for processing requests, which may include disk I/O
num.io.threads=8

# The send buffer (SO_SNDBUF) used by the socket server
socket.send.buffer.bytes=102400

# The receive buffer (SO_RCVBUF) used by the socket server
socket.receive.buffer.bytes=102400

# The maximum size of a request that the socket server will accept (protection against OOM)
socket.request.max.bytes=104857600


############################# Log Basics #############################

# A comma separated list of directories under which to store log files
log.dirs=/opt/kafka/kafka_2.13-2.8.0/data

# The default number of log partitions per topic. More partitions allow greater
# parallelism for consumption, but this will also result in more files across
# the brokers.
num.partitions=1

# The number of threads per data directory to be used for log recovery at startup and flushing at shutdown.
# This value is recommended to be increased for installations with data dirs located in RAID array.
num.recovery.threads.per.data.dir=1

############################# Internal Topic Settings  #############################
# The replication factor for the group metadata internal topics "__consumer_offsets" and "__transaction_state"
# For anything other than development testing, a value greater than 1 is recommended to ensure availability such as 3.
offsets.topic.replication.factor=1
transaction.state.log.replication.factor=1
transaction.state.log.min.isr=1

############################# Log Flush Policy #############################

# Messages are immediately written to the filesystem but by default we only fsync() to sync
# the OS cache lazily. The following configurations control the flush of data to disk.
# There are a few important trade-offs here:
#    1. Durability: Unflushed data may be lost if you are not using replication.
#    2. Latency: Very large flush intervals may lead to latency spikes when the flush does occur as there will be a lot of data to flush.
#    3. Throughput: The flush is generally the most expensive operation, and a small flush interval may lead to excessive seeks.
# The settings below allow one to configure the flush policy to flush data after a period of time or
# every N messages (or both). This can be done globally and overridden on a per-topic basis.

# The number of messages to accept before forcing a flush of data to disk
#log.flush.interval.messages=10000

# The maximum amount of time a message can sit in a log before we force a flush
#log.flush.interval.ms=1000

# The following configurations control the disposal of log segments. The policy can
# be set to delete segments after a period of time, or after a given size has accumulated.
# A segment will be deleted whenever *either* of these criteria are met. Deletion always happens
# from the end of the log.

# The minimum age of a log file to be eligible for deletion due to age
log.retention.hours=168

# A size-based retention policy for logs. Segments are pruned from the log unless the remaining
# segments drop below log.retention.bytes. Functions independently of log.retention.hours.
#log.retention.bytes=1073741824

# The maximum size of a log segment file. When this size is reached a new log segment will be created.
log.segment.bytes=1073741824

# The interval at which log segments are checked to see if they can be deleted according
# to the retention policies
log.retention.check.interval.ms=300000

############################# Zookeeper #############################

# Zookeeper connection string (see zookeeper docs for details).
# This is a comma separated host:port pairs, each corresponding to a zk
# server. e.g. "127.0.0.1:3000,127.0.0.1:3001,127.0.0.1:3002".
# You can also append an optional chroot string to the urls to specify the
# root directory for all kafka znodes.
zookeeper.connect=localhost:2181

# Timeout in ms for connecting to zookeeper
zookeeper.connection.timeout.ms=18000


############################# Group Coordinator Settings #############################

# The following configuration specifies the time, in milliseconds, that the GroupCoordinator will delay the initial consumer rebalance.
# The rebalance will be further delayed by the value of group.initial.rebalance.delay.ms as new members join the group, up to a maximum of max.poll.interval.ms.
# The default value for this is 3 seconds.
# We override this to 0 here as it makes for a better out-of-the-box experience for development and testing.
# However, in production environments the default value of 3 seconds is more suitable as this will help to avoid unnecessary, and potentially expensive, rebalances during application startup.
group.initial.rebalance.delay.ms=0
```

###### 2.3、增加环境变量

```shell
vim /etc/profile
```

> 在配置文件最下方添加配置如下：
>
> ```properties
> #kafka config start
> export KAFKA_HOME=/opt/kafka/kafka_2.13-2.8.0
> export PATH=$KAFKA_HOME/bin:$PATH
> #kafka config end
> ```
>
> 使配置文件生效：
>
> ```shell
> source /etc/profile
> ```

###### 2.4、启动kafka服务

```shell
kafka-server-start.sh -daemon /opt/kafka/kafka_2.13-2.8.0/config/server.properties
```

> ==daemon参数表示以守护进程的方式运行==

###### 2.5、停止服务

```shell
kafka-server-stop.sh stop
```

###### 2.6、kafka 群起脚本

```shell
#!/bin/bash
for i in hadoop102 hadoop103 hadoop104 
do 
echo "========== $i =========="  
ssh $i '/opt/kafka/kafka_2.13-2.8.0/bin/kafka-server-start.sh -daemon /opt/kafka/kafka_2.13-2.8.0/config/server.properties' 
done
```

###### 2.7、发放防火墙端口

```shell
firewall-cmd --permanent --add-port=9092/tcp --zone=public
firewall-cmd --reload
```

##### 3、Kafka 命令行操作

###### 3.1、查看当前服务器中的所有 topic

```shell
kafka-topics.sh --zookeeper localhost:2181 --list
```

###### 3.2、创建 topic

```shell
kafka-topics.sh --zookeeper localhost:2181 --create --replication-factor 1 --partitions 1 --topic first
```

> replication-factor参数：副本数量（必须小于集群中broker的总数量）
>
> partitions参数：分区数量
>
> topic参数： 定义topic名

###### 3.3、删除 topic

```shell
kafka-topics.sh --zookeeper localhost:2181 --delete --topic first
```

###### 3.4、发送消息

```shell
kafka-console-producer.sh --broker-list localhost:9092 --topic first
>hello world
>test test
```

###### 3.5、消费消息

```shell
kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic first
kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic first --consumer.config config/consumer.properties #consumer.properties配置使用了消费者组
kafka-console-consumer.sh --bootstrap-server localhost:9092 --from-beginning --topic first
```

> from-beginning参数：会把主题中以往所有的数据都读取出来

###### 3.6、查看主题的属性信息

```shell
kafka-topics.sh --describe --topic first --zookeeper localhost:2181
```

