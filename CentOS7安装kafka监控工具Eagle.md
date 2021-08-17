#### CentOS7安装kafka监控工具Eagle

##### 1、修改 kafka 启动命令

> 修改 kafka-server-start.sh 命令中：
>
> ```properties
> if [ "x$KAFKA_HEAP_OPTS" = "x" ]; then
>  export KAFKA_HEAP_OPTS="-Xmx1G -Xms1G"
> fi
> ```
>
> 为：
>
> ```properties
> if [ "x$KAFKA_HEAP_OPTS" = "x" ]; then
>  export KAFKA_HEAP_OPTS="-server -Xms2G -Xmx2G -XX:MetaspaceSize=128m -XX:+UseG1GC -XX:MaxGCPauseMillis=200 -XX:ParallelGCThreads=8 -XX:ConcGCThreads=5 -XX:InitiatingHeapOccupancyPercent=70"
>  export JMX_PORT="9999"
> fi
> ```
>
> ==注意：每台安装了kafka的机器都要改。==
>
> ==开启JMX_PORT防火墙端口：==
>
> ```shell
> firewall-cmd --permanent --add-port=9999/tcp --zone=public
> firewall-cmd --reload
> ```

##### 2、解压kafka-eagle-web-2.0.6-bin.tar.gz

```shell
tar -xvf kafka-eagle-web-2.0.6-bin.tar.gz
```

##### 3、给启动文件执行权限

```shell
cd kafka-eagle-web-2.0.6/
cd bin
chmod 777 ke.sh
```

##### 4、修改配置文件

```shell
cd ..
cd conf
vim system-config.properties
```

```properties
# kafka eagle webui port
######################################
kafka.eagle.webui.port=8048

######################################
# kafka jmx acl and ssl authenticate
######################################
cluster1.kafka.eagle.jmx.acl=false
cluster1.kafka.eagle.jmx.user=keadmin
cluster1.kafka.eagle.jmx.password=keadmin123
cluster1.kafka.eagle.jmx.ssl=false
cluster1.kafka.eagle.jmx.truststore.location=/Users/dengjie/workspace/ssl/certificates/kafka.truststore
cluster1.kafka.eagle.jmx.truststore.password=ke123456

######################################
# kafka offset storage
######################################
cluster1.kafka.eagle.offset.storage=kafka
#cluster2.kafka.eagle.offset.storage=zookeeper

######################################
# kafka jmx uri
######################################
cluster1.kafka.eagle.jmx.uri=service:jmx:rmi:///jndi/rmi://%s/jmxrmi

######################################
# kafka metrics, 15 days by default
######################################
kafka.eagle.metrics.charts=true
kafka.eagle.metrics.retain=15

######################################
# kafka sql topic records max
######################################
kafka.eagle.sql.topic.records.max=5000
kafka.eagle.sql.topic.preview.records.max=10

######################################
######################################
# multi zookeeper & kafka cluster list
######################################
#kafka.eagle.zk.cluster.alias=cluster1,cluster2
#cluster1.zk.list=tdn1:2181,tdn2:2181,tdn3:2181
#cluster2.zk.list=xdn10:2181,xdn11:2181,xdn12:2181

kafka.eagle.zk.cluster.alias=cluster1
cluster1.zk.list=192.168.18.8:2181,192.168.18.9:2181

######################################
# zookeeper enable acl
######################################
cluster1.zk.acl.enable=false
cluster1.zk.acl.schema=digest
cluster1.zk.acl.username=test
cluster1.zk.acl.password=test123

######################################
# broker size online list
######################################
cluster1.kafka.eagle.broker.size=20

######################################
# zk client thread limit
######################################
kafka.zk.limit.size=32

######################################
# kafka eagle webui port
######################################
kafka.eagle.webui.port=8048

######################################
# kafka jmx acl and ssl authenticate
######################################
cluster1.kafka.eagle.jmx.acl=false
cluster1.kafka.eagle.jmx.user=keadmin
cluster1.kafka.eagle.jmx.password=keadmin123

######################################
# zookeeper enable acl
######################################
cluster1.zk.acl.enable=false
cluster1.zk.acl.schema=digest
cluster1.zk.acl.username=test
cluster1.zk.acl.password=test123

######################################
# broker size online list
######################################
cluster1.kafka.eagle.broker.size=20

######################################
# zk client thread limit
######################################
kafka.zk.limit.size=32

######################################
kafka.eagle.sql.topic.records.max=5000
kafka.eagle.sql.topic.preview.records.max=10

######################################
# delete kafka topic token
######################################
kafka.eagle.topic.token=keadmin

######################################
# kafka sasl authenticate
######################################
cluster1.kafka.eagle.sasl.enable=false
cluster1.kafka.eagle.sasl.protocol=SASL_PLAINTEXT
cluster1.kafka.eagle.sasl.mechanism=SCRAM-SHA-256
cluster1.kafka.eagle.sasl.jaas.config=org.apache.kafka.common.security.scram.ScramLoginModule required username="kafka" password="kafka-eagle";
cluster1.kafka.eagle.sasl.client.id=
cluster1.kafka.eagle.blacklist.topics=
cluster1.kafka.eagle.sasl.cgroup.enable=false
cluster1.kafka.eagle.sasl.cgroup.topics=
cluster2.kafka.eagle.sasl.enable=false
cluster2.kafka.eagle.sasl.protocol=SASL_PLAINTEXT
cluster2.kafka.eagle.sasl.mechanism=PLAIN
cluster2.kafka.eagle.sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="kafka" password="kafka-eagle";
cluster2.kafka.eagle.sasl.client.id=
cluster2.kafka.eagle.blacklist.topics=
cluster2.kafka.eagle.sasl.cgroup.enable=false
cluster2.kafka.eagle.sasl.cgroup.topics=

######################################
# kafka ssl authenticate
######################################
cluster3.kafka.eagle.ssl.enable=false
cluster3.kafka.eagle.ssl.protocol=SSL
cluster3.kafka.eagle.ssl.truststore.location=
cluster3.kafka.eagle.ssl.truststore.password=
cluster3.kafka.eagle.ssl.keystore.location=
cluster3.kafka.eagle.ssl.keystore.password=
cluster3.kafka.eagle.ssl.key.password=
cluster3.kafka.eagle.ssl.endpoint.identification.algorithm=https
cluster3.kafka.eagle.blacklist.topics=
cluster3.kafka.eagle.ssl.cgroup.enable=false
cluster3.kafka.eagle.ssl.cgroup.topics=

######################################
# kafka sqlite jdbc driver address
######################################
#kafka.eagle.driver=org.sqlite.JDBC
#kafka.eagle.url=jdbc:sqlite:/hadoop/kafka-eagle/db/ke.db
#kafka.eagle.username=root
#kafka.eagle.password=www.kafka-eagle.org

######################################
# kafka mysql jdbc driver address
######################################
#ke这个数据库自己会创建，只要mysql原来没这库就OK，否则会覆盖
kafka.eagle.driver=com.mysql.cj.jdbc.Driver
kafka.eagle.url=jdbc:mysql://192.168.18.8:3306/ke?useUnicode=true&characterEncoding=UTF-8&zeroDateTimeBehavior=convertToNull
kafka.eagle.username=root
kafka.eagle.password=alan123456
```

##### 5、添加环境变量（必须）

```shell
vim /etc/profile
```

```properties
export KE_HOME=/opt/module/eagle 
export PATH=$PATH:$KE_HOME/bin
```

> ==刷新环境变量：==
>
> ```shell
> source /etc/profile
> ```
>
> ==解决每次重启centos都要手动刷新环境变量的问题：==
>
> ```shell
> vim ~/.bashrc
> ```
>
> 在最下面加入：
>
> ```properties
> # Auto deploy profile
> source /etc/profile
> ```

##### 6、启动服务

```shell
[root@alanCentos01 kafka-eagle-web-2.0.6]# bin/ke.sh start
```

