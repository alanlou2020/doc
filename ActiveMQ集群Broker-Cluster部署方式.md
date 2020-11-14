## ActiveMQ集群Broker-Cluster部署方式

#### 解决问题场景：可实现负载均衡，但不支持主备切换（热备），也就是说，只要有一个ActiveMQ的服务宕了，这个服务中的队列的消息只有等它再次启动才能被消费。

##### 一、配置方法（静态）

###### 1、打开HostA的ActiveMQ配置文件activemq.xml（路径：HostA\apache-activemq-5.16.0\conf\activemq.xml），一般是在persistenceAdapter标签上边，新增networkConnectors标签内容，详细配置如下：

~~~xml
<transportConnectors>
    <!-- DOS protection, limit concurrent connections to 1000 and frame size to 100MB -->
    <transportConnector name="openwire" uri="tcp://0.0.0.0:61616?maximumConnections=1000&amp;wireFormat.maxFrameSize=104857600"/>           
</transportConnectors>
~~~

~~~xml
<networkConnectors>
    <networkConnector uri="static:(tcp://127.0.0.1:61617)" />
</networkConnectors>
~~~

==说明：把HostA的ActiveMQ端口设置为61616，配置桥接的networkConnector需其他服务的地址及端口，如：127.0.0.1:61617，可以多个==

###### 2、打开HostA的Jetty配置文件jetty.xml（路径：HostA\apache-activemq-5.16.0\conf\jetty.xml）,配置端口如下：

~~~xml
<bean id="jettyPort" class="org.apache.activemq.web.WebConsolePort" init-method="start">
    <!-- the default port number for the web console -->
    <property name="host" value="127.0.0.1"/>
    <property name="port" value="8161"/>
</bean>
~~~

==说明：把HostA的Jetty端口设置为8161==

3、HostB的配置同上，ActiveMQ端口设置为61617，Jetty端口设置为8162，详细配置如下：

~~~xml
<transportConnectors>
    <!-- DOS protection, limit concurrent connections to 1000 and frame size to 100MB -->
    <transportConnector name="openwire" uri="tcp://0.0.0.0:61617?maximumConnections=1000&amp;wireFormat.maxFrameSize=104857600"/>           
</transportConnectors>
~~~

~~~xml
<networkConnectors>
    <networkConnector uri="static:(tcp://127.0.0.1:61616)" />
</networkConnectors>
~~~

~~~xml
<bean id="jettyPort" class="org.apache.activemq.web.WebConsolePort" init-method="start">
    <!-- the default port number for the web console -->
    <property name="host" value="127.0.0.1"/>
    <property name="port" value="8162"/>
</bean>
~~~

##### 二、测试：

###### 1、分别启动HostA和HostB文件夹中ActiveMQ服务，服务启动批处理路径（apache-activemq-5.16.0\bin\win64\activemq.bat），HostA和HostB都能正常运行则正常。

~~~powershell
jvm 1    |  INFO | Establishing network connection from vm://localhost to tcp://127.0.0.1:61617
jvm 1    |  INFO | Connector vm://localhost started
jvm 1    |  INFO | localhost Shutting down NC
jvm 1    |  INFO | localhost bridge to Unknown stopped
jvm 1    |  INFO | Error with pending local brokerInfo on: vm://localhost#8 (peer (vm://localhost#9) stopped.)
jvm 1    |  INFO | Connector vm://localhost stopped
jvm 1    |  WARN | Could not start network bridge between: vm://localhost and: tcp://127.0.0.1:61617 due to: Connection refused: connect
jvm 1    |  INFO | Establishing network connection from vm://localhost to tcp://127.0.0.1:61617
jvm 1    |  INFO | Connector vm://localhost started
jvm 1    |  INFO | Network connection between vm://localhost#10 and tcp:///127.0.0.1:61617@3458 (localhost) has been established.
~~~

==注意：当只开启HostA的ActiveMQ服务时，报错：Could not start network bridge between: vm://localhost and: tcp://127.0.0.1:61617 due to: Connection refused: connect；当HostB的ActiveMQ服务也开启时，成功建立连接：Network connection between vm://localhost#10 and tcp:///127.0.0.1:61617@3458 (localhost) has been established.==

##### 三、配置方法（动态）：

###### 1、打开HostA的ActiveMQ配置文件activemq.xml（路径：HostA\apache-activemq-5.16.0\conf\activemq.xml），配置内容如下：

~~~xml
<networkConnectors>
    <networkConnector uri="multicast://default" dynamicOnly="true" networkTTL="3" prefetchSize="1" decreaseNetworkConsumerPriority="true"/>
</networkConnectors>
~~~

~~~xml
<transportConnectors>
    <!-- DOS protection, limit concurrent connections to 1000 and frame size to 100MB -->
    <transportConnector name="openwire" uri="tcp://0.0.0.0:61616?maximumConnections=1000&amp;wireFormat.maxFrameSize=104857600" discoveryUri="multicast://default"/>           
</transportConnectors>
~~~

###### 2、HostB的配置同上，ActiveMQ端口设置为61617，Jetty端口设置为8162。

##### 四、测试：

###### 1、分别启动HostA和HostB文件夹中ActiveMQ服务，服务启动批处理路径（apache-activemq-5.16.0\bin\win64\activemq.bat），HostA和HostB都能正常运行则正常。

~~~powershell
jvm 1    |  INFO | Establishing network connection from vm://broker1 to tcp://Alan:61617
jvm 1    |  INFO | Connector vm://broker1 started
jvm 1    |  INFO | Network connection between vm://broker1#4 and tcp://Alan/192.168.56.1:61617@16168 (broker2) has been established.
~~~





