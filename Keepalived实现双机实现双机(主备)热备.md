### Keepalived 实现双机实现双机(主备)热备

##### 1、下载keepalived

> 直接从 Keepalived 官方下载所需版本，这里我下载的为 2.x 的版本。下载后进行解压：

```shell
wget https://www.keepalived.org/software/keepalived-2.0.18.tar.gz
tar -zxvf keepalived-2.0.18.tar.gz
```

##### 2、编译

> 安装相关依赖后进行编译：

```shell
# 安装依赖
yum -y install libnl libnl-devel
cd  keepalived-2.0.18
# 编译安装(注意：这里需要进入解压后的文件夹)
./configure --prefix=/usr/app/keepalived-2.0.18
make && make install
```

##### 3、环境配置

> 由于不是采用 yum 的方式进行安装，而是采用压缩包的方式进行安装，此时需要进行环境配置，具体如下：
> Keepalived 默认会从 /etc/keepalived/keepalived.conf 路径读取配置文件，所以需要将安装后的配置文件拷贝到该路径：

```shell
mkdir /etc/keepalived
cp /usr/app/keepalived-2.0.18/etc/keepalived/keepalived.conf /etc/keepalived/
```

##### 4、将所有 Keepalived 脚本拷贝到 /etc/init.d/ 目录下

```shell
# 编译目录中的脚本
cp /opt/keepalived/keepalived-2.0.18/keepalived/etc/init.d/keepalived /etc/init.d/
# 安装目录中的脚本
cp /usr/app/keepalived-2.0.18/etc/sysconfig/keepalived /etc/sysconfig/
cp /usr/app/keepalived-2.0.18/sbin/keepalived /usr/sbin/
```

##### 5、设置开机自启动（可选）

```shell
chmod +x /etc/init.d/keepalived
chkconfig --add keepalived
systemctl enable keepalived.service
```

##### 6、配置第一台节点（node1）的 keepalived.conf

```shell
vim /etc/keepalived/keepalived.conf
```

> 完整内容如下：

```properties
global_defs {
   # 路由id,主备节点不能相同
   router_id node1
}

# 自定义监控脚本
vrrp_script chk_haproxy {
    # 脚本位置
    script "/etc/keepalived/haproxy_check.sh"
    # 脚本执行的时间间隔
    interval 5
    weight 10
}

vrrp_instance VI_1 {
    # Keepalived的角色，MASTER 表示主节点，BACKUP 表示备份节点
    state MASTER
    # 指定监测的网卡，可以使用 ifconfig 进行查看
    interface ens33
    # 虚拟路由的id，主备节点需要设置为相同
    virtual_router_id 1
    # 优先级，主节点的优先级需要设置比备份节点高
    priority 100
    # 设置主备之间的检查时间，单位为秒
    advert_int 1
    # 定义验证类型和密码
    authentication {
        auth_type PASS
        auth_pass 123456
    }

    # 调用上面自定义的监控脚本
    track_script {
        chk_haproxy
    }

    virtual_ipaddress {
        # 虚拟IP地址，可以设置多个
        192.168.18.200
    }
}
```

##### 7、以上配置定义了 第一台节点（node1）上的 Keepalived 节点为 MASTER 节点，并设置对外提供服务的虚拟 IP 为 192.168.18.200。此外最主要的是定义了通过 haproxy_check.sh 来对 HAProxy 进行监控，这个脚本需要我们自行创建（主、备两台节点机器都要创建）

```shell
vim /etc/keepalived/haproxy_check.sh
```

> 内容如下：

```shell
#!/bin/bash

# 判断haproxy是否已经启动
if [ ${ps -C haproxy --no-header |wc -l} -eq 0 ] ; then
    #如果没有启动，则启动
    haproxy -f /etc/haproxy/haproxy.cfg
fi

#睡眠3秒以便haproxy完全启动
sleep 3

#如果haproxy还是没有启动，此时需要将本机的keepalived服务停掉，以便让VIP自动漂移到另外一台haproxy
if [ ${ps -C haproxy --no-header |wc -l} -eq 0 ] ; then
    systemctl stop keepalived
fi
```

> 创建后为其赋予执行权限：

```shell
chmod +x /etc/keepalived/haproxy_check.sh
```

> ==说明：这个脚本主要用于判断 HAProxy 服务是否正常，如果不正常且无法启动，此时就需要将本机 Keepalived 关闭，从而让虚拟IP漂移到备份节点。==

##### 9、启动第一台节点服务

```shell
systemctl start  keepalived
systemctl status  keepalived
```

> `ip a` 命令查看到虚拟 IP 的情况：

```shell
[root@alanCentos01 keepalived]# ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host 
       valid_lft forever preferred_lft forever
2: ens33: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 00:0c:29:db:05:74 brd ff:ff:ff:ff:ff:ff
    inet 192.168.18.8/24 brd 192.168.18.255 scope global noprefixroute ens33
       valid_lft forever preferred_lft forever
    inet 192.168.18.200/32 scope global ens33
       valid_lft forever preferred_lft forever
    inet6 fe80::d886:4d18:f97f:b5c2/64 scope link noprefixroute 
       valid_lft forever preferred_lft forever
3: virbr0: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500 qdisc noqueue state DOWN group default qlen 1000
    link/ether 52:54:00:43:56:cb brd ff:ff:ff:ff:ff:ff
    inet 192.168.122.1/24 brd 192.168.122.255 scope global virbr0
       valid_lft forever preferred_lft forever
4: virbr0-nic: <BROADCAST,MULTICAST> mtu 1500 qdisc pfifo_fast master virbr0 state DOWN group default qlen 1000
    link/ether 52:54:00:43:56:cb brd ff:ff:ff:ff:ff:ff
```

##### 10、配置第二台节点（node2）的 keepalived.conf

> ==注意：1、需要修改global_defs 的 router_id，如：node2；2、其次要修改 vrrp_instance_VI 中 state 为"BACKUP"；3、最后要将priority 设置为小于 100 的值。==

```shell
vim /etc/keepalived/keepalived.conf
```

> 完整内容如下：

```properties
global_defs {
   # 路由id,主备节点不能相同    
   router_id node2  
}

vrrp_script chk_haproxy {
    script "/etc/keepalived/haproxy_check.sh" 
    interval 5 
    weight 10
}

vrrp_instance VI_1 {
    # BACKUP 表示备份节点
    state BACKUP 
    interface ens33
    virtual_router_id 1
    # 优先级，备份节点要比主节点低
    priority 50 
    advert_int 1 
    authentication { 
        auth_type PASS
        auth_pass 123456
    }
    
    track_script {
        chk_haproxy
    }

    virtual_ipaddress {
        192.168.18.200  
    }
}
```

##### 11、启动第二台节点服务

```shell
systemctl start  keepalived
systemctl status  keepalived
```

##### 12、验证故障转移

> 这里我们验证一下故障转移，因为按照我们上面的检测脚本，如果 HAProxy 已经停止且无法重启时 KeepAlived 服务就会停止，这里我们直接使用以下命令停止 Keepalived 服务：

```shell
systemctl stop keepalived
```

> 结论：两台机器只要有一台keepalived服务是正常启动的，就能正常访问：http://192.168.18.200:15672/
