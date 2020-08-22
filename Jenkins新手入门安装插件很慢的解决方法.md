# Jenkins新手入门安装插件很慢的解决方法

### （一）Linux下的Jenkins

linux系统的Jenkins，cd {你的Jenkins工作目录}/updates

比如修改 /var/lib/jenkins/updates/default.json，进入/var/lib/jenkins/updates 目录，执行修改命令：

```
cd /var/lib/jenkins/updates
sed -i 's/http:\/\/updates.jenkins-ci.org\/download/https:\/\/mirrors.tuna.tsinghua.edu.cn\/jenkins/g' default.json && sed -i 's/http:\/\/www.google.com/https:\/\/www.baidu.com/g' default.json
```

然后重启Jenins

### （二）windows下的Jenkins

打开文件 C:\Windows\System32\config\systemprofile\AppData\Local\Jenkins.jenkins\updates\default.json


1. 全局搜索  www.google.com  替换成  http://www.baidu.com

2. 全局搜索  updates.jenkins-ci.org/download  替换成  mirrors.tuna.tsinghua.edu.cn/jenkins

3.重启Jenkins，打开cmd窗口

net stop jenkins

net start jenkins

4.刷新Jenkins首页