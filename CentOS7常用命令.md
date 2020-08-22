# CentOS7常用命令

### 1、vi/vim编辑器

```CentOS
# 1、:i命令：插入
# 2、:q!命令：强制退出
# 3、:wq命令：保存退出
# 4、快捷键shift+insert：粘贴
```

### 2、防火墙命令

```Centos
# 1、启动防火墙服务：systemctl restart firewalld 或 service firewalld restart
# 2、开机自动启动：systemctl enable firewalld.service
# 3、关闭防火墙：systemctl stop firewalld 或 service firewalld stop
# 4、开放防火墙端口：firewall-cmd --permanent --add-port=8888/tcp
```