### Haproxy实现rabbitmq集群负载均衡

##### 1、下载haproxy

```shell
yum -y install haproxy
```

##### 2、修改haproxy.cfg

```shell
vim /etc/haproxy/haproxy.cfg
```

> 配置文件内容如下

```properties
#---------------------------------------------------------------------
# Example configuration for a possible web application.  See the
# full configuration options online.
#
#   http://haproxy.1wt.eu/download/1.4/doc/configuration.txt
#
#---------------------------------------------------------------------

#---------------------------------------------------------------------
# Global settings
#---------------------------------------------------------------------
global
    # to have these messages end up in /var/log/haproxy.log you will
    # need to:
    #
    # 1) configure syslog to accept network log events.  This is done
    #    by adding the '-r' option to the SYSLOGD_OPTIONS in
    #    /etc/sysconfig/syslog
    #
    # 2) configure local2 events to go to the /var/log/haproxy.log
    #   file. A line like the following can be added to
    #   /etc/sysconfig/syslog
    #
    #    local2.*                       /var/log/haproxy.log
    #
    log         127.0.0.1 local2

    chroot      /var/lib/haproxy
    pidfile     /var/run/haproxy.pid
    maxconn     4000
    user        haproxy
    group       haproxy
    daemon

    # turn on stats unix socket
    stats socket /var/lib/haproxy/stats

#---------------------------------------------------------------------
# common defaults that all the 'listen' and 'backend' sections will
# use if not designated in their block
#---------------------------------------------------------------------
defaults
    mode                    tcp
    log                     global
    option                  tcplog
    option                  dontlognull
#    option http-server-close
#    option forwardfor       except 127.0.0.0/8
    option                  redispatch
    retries                 3
    timeout http-request    10s
    timeout queue           1m
    timeout connect         10s
    timeout client          1m
    timeout server          1m
    timeout http-keep-alive 10s
    timeout check           10s
    maxconn                 3000

#---------------------------------------------------------------------
# main frontend which proxys to the backends
#---------------------------------------------------------------------
# frontend  main *:5000
#    acl url_static       path_beg       -i /static /images /javascript /stylesheets
#    acl url_static       path_end       -i .jpg .gif .png .css .js

#    use_backend static          if url_static
#    default_backend             app

#---------------------------------------------------------------------
# static backend for serving up images, stylesheets and such
#---------------------------------------------------------------------
#backend static
#    balance     roundrobin
#    server      static 127.0.0.1:4331 check

#---------------------------------------------------------------------
# round robin balancing between the various backends
#---------------------------------------------------------------------
#backend app
#    balance     roundrobin
#    server  app1 127.0.0.1:5001 check
#    server  app2 127.0.0.1:5002 check
#    server  app3 127.0.0.1:5003 check
#    server  app4 127.0.0.1:5004 check

### haproxy 监控页面地址是：http://192.168.18.7:9188/stats
listen admin_stats
    bind *:9188
    mode http
    log 127.0.0.1 local3 err
    stats refresh 60s
    stats uri /stats
    stats realm welcome login\ Haproxy
    stats auth admin:123456
    stats hide-version
    stats admin if TRUE

### rabbitmq 集群配置，转发到
listen rabbitmq_cluster
    bind *:5670
    mode tcp
    balance roundrobin
    server rabbitmq_node1 192.168.18.7:5672 check inter 5000 rise 2 fall 3 weight 1
    server rabbitmq_node2 192.168.18.7:5673 check inter 5000 rise 2 fall 3 weight 1
    server rabbitmq_node3 192.168.18.7:5674 check inter 5000 rise 2 fall 3 weight 1
```

##### 3、开发防火墙端口

```shell
firewall-cmd --permanent --add-port=5670/tcp --zone=public
firewall-cmd --permanent --add-port=9188/tcp --zone=public
firewall-cmd --reload
```

##### 4、启动haproxy

```shell
haproxy -f /etc/haproxy/haproxy.cfg
ps -ef | grep haproxy
```

##### 5、完成，访问地址为：http://192.168.18.7:9188/stats，用户名为配置文件中配置的-admin:123456

