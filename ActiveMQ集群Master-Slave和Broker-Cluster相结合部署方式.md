## ActiveMQ集群Master-Slave和Broker-Cluster相结合部署方式

#### 解决问题场景：支持自动主备切换（热备），提供broker节点高可用，同时可实现负载均衡。

##### 一、配置方法：

###### 1、打开HostA的ActiveMQ配置文件activemq.xml（路径：HostA\apache-activemq-5.16.0\conf\activemq.xml），一般是在persistenceAdapter标签上边，新增networkConnectors标签内容，详细配置如下：

~~~xml
<networkConnectors>
    <networkConnector uri="masterslave:(tcp://0.0.0.0:61617,tcp://0.0.0.0:61618)" duplex="false"/>
</networkConnectors>
~~~

==说明：masterslave:(tcp://0.0.0.0:61617,tcp://0.0.0.0:61618)实现自动准备（热备），提供broker节点高可用==

###### 2、修改transportConnectors标签内容如下：

~~~xml
<transportConnectors>
    <!-- DOS protection, limit concurrent connections to 1000 and frame size to 100MB -->
    <transportConnector name="openwire" uri="tcp://0.0.0.0:61616?maximumConnections=1000&amp;wireFormat.maxFrameSize=104857600"/>           
</transportConnectors>
~~~

==说明：把HostA的ActiveMQ端口设置为61616==

###### 3、打开HostA的Jetty配置文件jetty.xml（路径：HostA\apache-activemq-5.16.0\conf\jetty.xml）,配置端口如下：

~~~xml
<bean id="jettyPort" class="org.apache.activemq.web.WebConsolePort" init-method="start">
    <!-- the default port number for the web console -->
    <property name="host" value="127.0.0.1"/>
    <property name="port" value="8161"/>
</bean>
~~~

==说明：把HostA的Jetty端口设置为8161==

###### 4、打开HostB的ActiveMQ配置文件activemq.xml（路径：HostA\apache-activemq-5.16.0\conf\activemq.xml），一般是在persistenceAdapter标签上边，新增networkConnectors标签内容，详细配置如下（静态配置）：

~~~xml
<networkConnectors>
    <networkConnector uri="static:(tcp://0.0.0.0:61616)" duplex="false"/>
</networkConnectors>
~~~

==说明：这里static:(tcp://0.0.0.0:61616)是指HostA的ActiveMQ的地址和端口号==

```xml
<transportConnectors>
    <!-- DOS protection, limit concurrent connections to 1000 and frame size to 100MB -->
    <transportConnector name="openwire" uri="tcp://0.0.0.0:61617?maximumConnections=1000&amp;wireFormat.maxFrameSize=104857600"/>            
</transportConnectors>
```

==说明：把HostB的ActiveMQ端口设置为61617==

###### 5、设置HostB中的共享文件夹

```xml
<persistenceAdapter>
    <kahaDB directory="f:/data/kahadb"/>
</persistenceAdapter>
```

==说明：只能有一个ActiveMQ服务占用共享文件并且加锁，其他共享该文件夹的ActiveMQ服务都会处于挂起状况（管理客户端界面也无法访问）==

###### 6、Jetty配置文件jetty.xml（路径：HostA\apache-activemq-5.16.0\conf\jetty.xml）,配置端口如下：

```xml
<bean id="jettyPort" class="org.apache.activemq.web.WebConsolePort" init-method="start">
    <!-- the default port number for the web console -->
    <property name="host" value="127.0.0.1"/>
    <property name="port" value="8162"/>
</bean>
```

==说明：把HostA的Jetty端口设置为8162==

###### 7、HostC的ActiveMQ配置类似HostB，详细配置如下：

```xml
<networkConnectors>
    <networkConnector uri="static:(tcp://0.0.0.0:61616)" duplex="false"/>
</networkConnectors>
```

==说明：这里static:(tcp://0.0.0.0:61616)是指HostA的ActiveMQ的地址和端口号==

```xml
<transportConnectors>
    <!-- DOS protection, limit concurrent connections to 1000 and frame size to 100MB -->
    <transportConnector name="openwire" uri="tcp://0.0.0.0:61618?maximumConnections=1000&amp;wireFormat.maxFrameSize=104857600"/>            
</transportConnectors>
```

==说明：把HostB的ActiveMQ端口设置为61618==

```xml
<persistenceAdapter>
    <kahaDB directory="f:/data/kahadb"/>
</persistenceAdapter>
```

==说明：只能有一个ActiveMQ服务占用共享文件并且加锁，其他共享该文件夹的ActiveMQ服务都会处于挂起状况（管理客户端界面也无法访问）==

###### 7、Jetty配置文件jetty.xml（路径：HostC\apache-activemq-5.16.0\conf\jetty.xml）,配置端口如下：

```xml
<bean id="jettyPort" class="org.apache.activemq.web.WebConsolePort" init-method="start">
    <!-- the default port number for the web console -->
    <property name="host" value="127.0.0.1"/>
    <property name="port" value="8163"/>
</bean>
```

###### 二、测试：

###### 1、按顺序启动HostA和HostB和HostC文件夹中ActiveMQ服务，服务启动批处理路径（apache-activemq-5.16.0\bin\win64\activemq.bat），HostA、HostB都能正常运行则正常，HostC的ActiveMQ服务处于挂起状态，当HostB宕机后，HostC会立即激活取代HostB；客户端通过"failover:(tcp://127.0.0.1:61617,tcp://127.0.0.1:61618)"的方式发送消息，正常情况下消息会发送到127.0.0.1:61617；127.0.0.1:61618起到备份的作用。==127.0.0.1:61616不属于接受消息的范畴，客户端发消息只通过"failover:(tcp://127.0.0.1:61617,tcp://127.0.0.1:61618)"发到127.0.0.1:61617或127.0.0.1:61618（那个抢到了共享文件锁并且服务启动了，就发给哪个），127.0.0.1:61616桥接到127.0.0.1:61617或127.0.0.1:61618，起到负载均衡的作用。==

