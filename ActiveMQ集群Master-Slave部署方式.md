## ActiveMQ集群Master-Slave部署方式

#### 解决问题场景：自动主备切换（热备），提供broker节点高可用（基于共享存储的Master-Slave；多个broker共用同一数据源，谁拿到锁谁就是master,其他处于待启动状态，如果master挂掉了，某个抢到文件锁的slave变成master）

##### 一、配置方法：

###### 1、打开HostA的ActiveMQ配置文件activemq.xml（路径：HostA\apache-activemq-5.16.0\conf\activemq.xml），配置内容如下：

* 找到原始配置如下：

~~~xml
<transportConnectors>
    <!-- DOS protection, limit concurrent connections to 1000 and frame size to 100MB -->
    <transportConnector name="openwire" uri="tcp://0.0.0.0:61616?maximumConnections=1000&amp;wireFormat.maxFrameSize=104857600"/>
    <transportConnector name="amqp" uri="amqp://0.0.0.0:5672?maximumConnections=1000&amp;wireFormat.maxFrameSize=104857600"/>
    <transportConnector name="stomp" uri="stomp://0.0.0.0:61613?maximumConnections=1000&amp;wireFormat.maxFrameSize=104857600"/>
    <transportConnector name="mqtt" uri="mqtt://0.0.0.0:1883?maximumConnections=1000&amp;wireFormat.maxFrameSize=104857600"/>
    <transportConnector name="ws" uri="ws://0.0.0.0:61614?maximumConnections=1000&amp;wireFormat.maxFrameSize=104857600"/>
    <!--<transportConnector name="ssl" uri="ssl://localhost:61617?maximumConnections=1000&amp;wireFormat.maxFrameSize=104857600"/>-->
</transportConnectors>
~~~

* 只留下name="openwire"的transportConnector，其他的删掉，配置如下：

~~~xml
  <transportConnectors>
      <!-- DOS protection, limit concurrent connections to 1000 and frame size to 100MB -->
      <transportConnector name="openwire" uri="tcp://0.0.0.0:61616?maximumConnections=1000&amp;wireFormat.maxFrameSize=104857600"/>           
  </transportConnectors>
~~~

==说明：把HostA的ActiveMQ端口设置为61616==

###### 2、打开HostA的Jetty配置文件jetty.xml（路径：HostA\apache-activemq-5.16.0\conf\jetty.xml）,配置端口如下：

~~~xml
<bean id="jettyPort" class="org.apache.activemq.web.WebConsolePort" init-method="start">
    <!-- the default port number for the web console -->
    <property name="host" value="127.0.0.1"/>
    <property name="port" value="8161"/>
</bean>
~~~

==说明：把HostA的Jetty端口设置为8161==

###### 3、HostB的配置同上，ActiveMQ端口设置为61617，Jetty端口设置为8162。

##### 二、测试：

###### 1、分别启动HostA和HostB文件夹中ActiveMQ服务，服务启动批处理路径（apache-activemq-5.16.0\bin\win64\activemq.bat），HostA和HostB都能正常运行则正常。

==注意：需配合客户端发送方式和接受方式（"failover:(tcp://127.0.0.1:61616,tcp://127.0.0.1:61617)"）才能实现==



