## CentOS7.6详细说明

### 一、Linux目录结构

1. /bin：是 Binary 的缩写, 这个目录存放着最经常使用的命令。==[常用]==

2. /sbin：就是 Super User 的意思，这里存放的是系统管理员使用的系统管理程序。==[常用]==

3. /home：存放普通用户的主目录，在 Linux 中每个用户都有一个自己的目录，一般该目录名是以用户的账号命名。==[常用]==

4. /root：该目录为系统管理员，也称作超级权限者的用户主目录。==[常用]==

5. /lib：系统开机所需要最基本的动态连接共享库，其作用类似于 Windows 里的 DLL 文件。几乎所有的应用程序都需要
   用到这些共享库。

6. /lost+found：这个目录一般情况下是空的，当系统非法关机后，这里就存放了一些文件。==[隐藏目录]==

7. /etc：所有的系统管理所需要的==配置文件==和子目录, 比如安装 mysql 数据库 my.conf。==[常用]==

8. /usr：这是一个==非常重要的目录==，用户的很多==安装的应用程序==和文件都放在这个目录下，类似与 windows 下的 program files 目录。==[常用]==

9. /boot：==非常重要的目录==，存放的是启动 Linux 时使用的一些核心文件，包括一些连接文件以及镜像文件。==[常用]==

10. /proc：这个目录是一个虚拟的目录，它是系统内存的映射，访问这个目录来获取系统信息。==[不能动]==

11. /srv：service 缩写，该目录存放一些服务启动之后需要提取的数据。==[不能动]==

12. /sys：这是 linux2.6 内核的一个很大的变化。该目录下安装了 2.6 内核中新出现的一个文件系统 sysfs。==[不能动]==
13. /tmp：这个目录是用来存放一些临时文件的。
14. /dev：类似于 windows 的设备管理器，把所有的硬件用文件的形式存储。
15. /media：linux 系统会自动识别一些设备，例如 U 盘、光驱等等，当识别后，linux 会把识别的设备挂载到这个
    目录下。==[常用]==
16. /mnt ：系统提供该目录是为了让用户临时==挂载别的文件系统==的，我们可以将外部的存储挂载在/mnt/上，然后进入该目录就可以查看里的内容了。 如：d:/myshare。==[常用]==
17. /opt：这是给主机安装软件的==安装程序==所存放的目录。如ORACLE数据库的安装程序就可放到该目录下。默认为空。
18. /usr/local：这是另一个给主机额外安装软件所安装到的目录。一般是通过编译源码方式安装的程序。==[常用]==
19. /var：这个目录中存放着在不断扩充着的东西，习惯将经常被修改的目录放在这个目录下。包括各种日志文件。==[常用]==
20. /selinux [security-enhanced linux]：SELinux 是一种安全子系统,它能控制程序只能访问特定文件, 有三种工作模式，可以自行设置。==类似windows的360安全卫士，该文件夹需功能启用才会有，否则没有==。

### 二、Vi 和 Vim 编辑器

1. 正常模式：以 vim 打开一个档案就直接进入一般模式了(这是默认的模式默认的模式)。
2. 插入模式：按下 i, I, o, O, a, A, r, R 等任何一个字母之后才会进入编辑模式, 一般来说按 i 即可。
3. 命令行模式：输入 esc 再输入”:“。
4. vi 和 vim 快捷键：
   * 拷贝当前行yy , 拷贝当前行向下的 5 行5yy，并粘贴（输入 p）。
   * 删除当前行dd, 删除当前行向下的 5 行 5dd。
   * 在文件中查找某个单词 [命令行下 /关键字 ， 回车 查找 ,输入 n 就是查找下一个 ]。
   * 设置文件的行号，取消文件的行号[命令行下: set nu 和:set nonu]。
   * 编辑 /etc/profile 文件，在一般模式下, 使用快捷键到该文档的最末行[G]和最首行[gg]。
   * 在一个文件中输入 "hello" ,在一般模式下, 然后又撤销这个动作[u]。
   * 编辑/etc/profile 文件，在一般模式下,并将光标移动到第20行：输入 20,再输入[shift+g]。

### 三、开机、重启

1. shutdown –h now 说明：立该进行关机。
2. shutdown -h 1 "hello, 1 分钟后会关机了" 说明：1 分钟后会关机，并提示说明“hello, 1 分钟后会关机了”。
3. shutdown –r now 说明：现在重新启动计算机。
4. halt 说明：关机，作用和上面一样。
5. reboot 说明：现在重新启动计算机。
6. sync 说明：把内存的数据同步到磁盘。
7. ==注意细节：==
   * 不管是重启系统还是关闭系统，首先要运行 sync 命令，把内存中的数据写到磁盘中。
   * 目前的 shutdown/reboot/halt 等命令均已经在关机前进行了 sync ，提醒: 小心驶得万年船。

### 四、用户登录、注销   

1. 切换成系统管理员身份：
   * su - root，然后输入root用户的密码。
   * sudo su，然后输入root用户的密码==（要求：当前用户名要在sudoers文件中）==。
2. 注销用户：logout。
3. ==注意细节：==
   * logout 注销指令在图形运行级别无效，在运行级别 3 下有效。

### 五、用户管理

1. 显示当前用户所在路径：

   ```shell
   [root@alanCentos01 ~]# pwd
   /root
   ```

2. 添加用户：useradd 用户名，例如：

   ~~~shell
   useradd milan
   ~~~

   ==注意细节：==添加一个用户 milan, 默认该用户的家目录在 /home/milan。

3. 添加用户：useradd -d 目录名 用户名。

   ==注意细节：==新的用户名指定目录名，给新创建的用户指定家目录。

4. 指定/修改密码：passwd 用户名，例如：

   ~~~shell
   [root@alanCentos01 ~]# passwd milan
   更改用户 milan 的密码 。
   新的 密码：
   无效的密码： 密码少于 8 个字符
   重新输入新的 密码：
   passwd：所有的身份验证令牌已经成功更新。
   ~~~

   ==注意：如果passwd后面没有跟用户名，就是给当前登录用户修改密码。==

5. 删除用户：

   * 保留家目录:

   ~~~shell
   [root@alanCentos01 ~]# userdel milan
   [root@alanCentos01 ~]# ls -l /home
   总用量 8
   drwx------. 5 alan alan 4096 4月   5 23:17 alan
   drwx------. 3 1001 1001 4096 4月   6 21:45 milan
   ~~~

   * 不保留家目录:

   ```shell
   userdel -r milan
   ```

   ==注意细节：==是否保留家目录的讨论? 一般情况下，建议保留。

6. 查询用户信息指令：

   ~~~shell
   [root@alanCentos01 ~]# id alan
   uid=1000(alan) gid=1000(alan) 组=1000(alan)
   [root@alanCentos01 ~]# id jerry
   id: jerry: no such user
   ~~~

   ==注意细节：==当用户不存在时，返回无此用户。

7. 切换用户：

   ~~~shell
   [root@alanCentos01 ~]# su - alan
   上一次登录：一 4月  5 23:13:40 CST 2021从 192.168.18.1pts/0 上
   [alan@alanCentos01 ~]$ 
   ~~~

   ==注意细节：==

   * 从权限高的用户切换到权限低的用户，不需要输入密码，反之需要。
   * 当需要返回到原来用户时，使用 exit/logout 指令。

8. 查看当前用户/登录用户：

   ~~~shell
   [root@alanCentos01 ~]# whoami
   root
   [alan@alanCentos01 ~]$ who am i
   root     pts/0        2021-04-06 21:44 (192.168.18.1)
   ~~~

9. 新增用户组：

   ~~~shell
   [root@alanCentos01 ~]# groupadd wudang
   [root@alanCentos01 ~]# 
   ~~~

10. 删除用户组：

    ~~~shell
    [root@alanCentos01 ~]# groupdel wudang
    [root@alanCentos01 ~]#
    ~~~

11. 增加用户时直接加上组：

    ~~~shell
    [root@alanCentos01 ~]# groupadd wudang
    [root@alanCentos01 ~]# useradd -g wudang zwj
    [root@alanCentos01 ~]# id zwj
    uid=1001(zwj) gid=1001(wudang) 组=1001(wudang)
    ~~~

12. 修改用户的组：

    ~~~shell
    [root@alanCentos01 ~]# groupadd mojiao
    [root@alanCentos01 ~]# usermod -g mojiao zwj
    [root@alanCentos01 ~]# id zwj
    uid=1001(zwj) gid=1002(mojiao) 组=1002(mojiao)
    ~~~

13. 用户和组相关文件：

    * /etc/passwd文件：用户（user）的配置文件，记录用户的各种信息。

      ```shell
      [root@alanCentos01 ~]# cat /etc/passwd |grep "zwj"
      zwj:x:1001:1002::/home/zwj:/bin/bash
      ```

      ==每行的含义==：用户名:口令:用户标识号:组标识号:注释性描述:主目录:登录Shell。

    * /etc/shadow文件：口令的配置文件。

      ~~~shell
      [root@alanCentos01 ~]# cat /etc/shadow |grep "zwj"
      zwj:!!:18723:0:99999:7:::
      ~~~

      ==每行的含义==：登录名:加密口令:最后一次修改时间:最小时间间隔:最大时间间隔:警告时间:不活动时间:失效时间:标志。==注意：”!!“表示没有设置密码。==

    * /etc/group文件：组(group)的配置文件，记录 Linux 包含的组的信息。

      ~~~shell
      [root@alanCentos01 ~]# cat /etc/group |grep "mojiao"
      mojiao:x:1002:
      ~~~

      ==每行的含义==：组名:口令:组标识号:组内用户列表。

### 六、运行级别指令

1. 运行级别：

   * ==运行级别说明(常用运行级别是 3 和 5 ，也可以指定默认运行级别)==：

     - 0 ：关机
     - 1 ：单用户【找回丢失密码】
     - 2：多用户状态没有网络服务
     - 3：多用户状态有网络服务
     - 4：系统未使用保留给用户
     - 5：图形界面
     - 6：系统重启

   * 命令：init [0123456]

     ~~~shell
     init 3
     ~~~

2. 指定默认运行级别：

   - 在 centos7 以前版本，/etc/inittab 文件中修改数字（运行级别）。

   - 在 centos7版本，对/etc/inittab 文件进行了简化，如下：

     multi-user.target: analogous to runlevel 3
     graphical.target: analogous to runlevel 5

   - 查看当前运行级别：

     ```shell
     [root@alancentos ~]# systemctl get-default
     graphical.target
     ```

   - 通过命令行设置默认运行级别：

     ~~~shell
     [root@alancentos ~]# systemctl set-default multi-user.target
     Removed symlink /etc/systemd/system/default.target.
     Created symlink from /etc/systemd/system/default.target to /usr/lib/systemd/system/multi-user.target.
     
     [root@alancentos ~]# systemctl set-default graphical.target
     Removed symlink /etc/systemd/system/default.target.
     Created symlink from /etc/systemd/system/default.target to /usr/lib/systemd/system/graphical.target.
     ~~~

### 七、CentOS7.6找回root密码

1. 首先，启动系统，进入开机界面，在界面中按“e”进入编辑界面。如图：

   ![image-20210410190950485](assets/CentOS7.6详细说明/image-20210410190950485.png)

   2. 进入编辑界面，使用键盘上的上下键把光标往下移动，找到以““Linux16”开头内容所在的行数”，在行的最后面输入：init=/bin/sh。如图：

      ![image-20210410191117688](assets/CentOS7.6详细说明/image-20210410191117688.png)

3. 接着，输入完成后，直接按快捷键：Ctrl+x 进入**单用户模式**。

4. 接着，在光标闪烁的位置中输入：mount -o remount,rw /（注意：各个单词间有空格），完成后按键盘的回车键（Enter）。如图：

   ![image-20210410191208429](assets/CentOS7.6详细说明/image-20210410191208429.png)

5. 在新的一行最后面输入：passwd， 完成后按键盘的回车键（Enter）。输入密码，**然后再次确认密码即**可(**提示:** **密码长度最好8****位以上,****但不是必须的**), 密码修改成功后，会显示passwd.....的样式，说明密码修改成功。

   ![image-20210410191301500](assets/CentOS7.6详细说明/image-20210410191301500.png)

6. 接着，在鼠标闪烁的位置中（最后一行中）输入：touch /.autorelabel（注意：touch与 /后面有一个空格），完成后按键盘的回车键（Enter）

7. 继续在光标闪烁的位置中，输入：exec /sbin/init（注意：exec与 /后面有一个空格），完成后按键盘的回车键（Enter）,等待系统自动修改密码(**提示：这个过程时间可能有点长，耐心等待**)，完成后，系统会自动重启, **新的密码生效了**。

   ![image-20210410191424175](assets/CentOS7.6详细说明/image-20210410191424175.png)

### 八、帮助指令

1. man 获得帮助信息：

   基本语法：man [命令或配置文件]（功能描述：获得帮助信息）

   ~~~shell
   [root@alancentos home]# man ls
   ~~~

   说明：==在 linux 下，隐藏文件是以 .开头== , 选项可以组合使用比如 ls -al, 比如 ls-al /root

2. help 指令：

   基本语法：help 命令 （功能描述：获得 shell 内置命令的帮助信息）

   ~~~shell
   [root@alancentos home]# help cd
   ~~~

### 九、文件目录类

1. pwd指令：

   基本语法 ：pwd(功能描述：显示当前工作目录的绝对路径)

   ~~~shell
   [root@alancentos home]# pwd
   /home
   ~~~

2. ls指令：

   基本语法：ls [选项：目录或是文件]
   常用选项：
   -a ：显示当前目录所有的文件和目录，包括隐藏的。
   -l：以列表的方式显示信息。

   -h：以方便人类读取的方式显示信息。

   ~~~shell
   [root@alanCentos01 /]# ls -lh
   总用量 68K
   lrwxrwxrwx.   1 root root    7 4月   2 00:24 bin -> usr/bin
   dr-xr-xr-x.   6 root root 4.0K 4月   5 22:41 boot
   drwxr-xr-x.  19 root root 3.3K 4月  11 12:35 dev
   drwxr-xr-x. 144 root root  12K 4月  11 12:37 etc
   drwxr-xr-x.   3 root root 4.0K 4月  11 13:55 home
   lrwxrwxrwx.   1 root root    7 4月   2 00:24 lib -> usr/lib
   lrwxrwxrwx.   1 root root    9 4月   2 00:24 lib64 -> usr/lib64
   drwx------.   2 root root  16K 4月   2 00:23 lost+found
   drwxr-xr-x.   2 root root 4.0K 4月  11 2018 media
   drwxr-xr-x.   3 root root 4.0K 4月   3 22:08 mnt
   drwxr-xr-x.   4 root root 4.0K 4月   3 22:07 opt
   dr-xr-xr-x. 227 root root    0 4月  11 12:35 proc
   dr-xr-x---.  16 root root 4.0K 4月  11 13:16 root
   drwxr-xr-x.  42 root root 1.3K 4月  11 12:35 run
   lrwxrwxrwx.   1 root root    8 4月   2 00:24 sbin -> usr/sbin
   drwxr-xr-x.   2 root root 4.0K 4月  11 2018 srv
   dr-xr-xr-x.  13 root root    0 4月  11 12:35 sys
   drwxrwxrwt.  41 root root 4.0K 4月  11 13:55 tmp
   drwxr-xr-x.  13 root root 4.0K 4月   2 00:24 usr
   drwxr-xr-x.  21 root root 4.0K 4月   2 00:34 var
   ~~~

   

3. cd指令：

   基本语法：cd[参数-功能描述：切换到指定目录]

   cd ~ 或者 cd ：回到自己的家目录, 比如 你是 root ， cd ~ 到 /root。
   cd .. 回到当前目录的上一级目录。

   ```shell
   [root@alancentos home]# cd /root
   [root@alancentos ~]# 
   ```

4. mkdir指令

   说明：mkdir 指令用于创建目录

   基本语法：mkdir[选项]要创建的目录

   创建一级目录：

   ~~~shell
   [root@alancentos ~]# mkdir /home/dog
   ~~~

   创建多级目录：

   ~~~shell
   [root@alancentos ~]# mkdir /home/animal/tiger
   mkdir: 无法创建目录"/home/animal/tiger": 没有那个文件或目录
   [root@alancentos ~]# mkdir -p /home/animal/tiger
   ~~~

5. rmdir指令

   说明：删除空目录

   基本语法：rmdir[选项]要删除的空目录。

   ==使用细节==：
   rmdir删除的是空目录，如果目录下有内容时无法删除的。
   提示：如果需要删除非空目录，需要使用rm -rf要删除的目录。
   比如： rm -rf /home/animal

   ~~~shell
   [root@alancentos ~]# rmdir /home/dog
   [root@alancentos ~]# rmdir /home/animal
   rmdir: 删除 "/home/animal" 失败: 目录非空
   [root@alancentos ~]# rm -rf /home/animal
   [root@alancentos ~]#
   ~~~

6. touch指令

   说明：touch指令创建空文件
   基本语法：touch 文件名称

   ~~~shell
   [root@alancentos /]# touch /home/hello.txt
   [root@alancentos /]# ls /home
   alan  hello.txt
   ~~~

7. cp指令

   说明：cp指令拷贝文件到指定目录

   基本语法：cp [选项] source dest
   常用选项：
   -r ：递归复制整个文件夹

   应用实例：
   案例 1: 将 /home/hello.txt 拷贝到/home/bbb目录下（==注：如果/home/bbb文件夹不存在，则会把hello.txt拷贝并改名为bbb文件==）。

   ~~~shell
   [root@alancentos /]# cp /home/hello.txt /home/bbb
   ~~~

   案例 2: 递归复制整个文件夹，举例, 比如将 /home/bbb 整个目录， 拷贝到 /opt。

   ~~~shell
   [root@alancentos /]# cp -r /home/bbb /opt
   [root@alancentos /]# ls /opt
   bbb  rh  VMwareTools-10.3.10-13959562.tar.gz  vmware-tools-distrib
   ~~~

   使用细节：
   强制覆盖不提示的方法：\cp，如： \cp -r /home/bbb /opt

8. rm指令

   说明：rm 指令移除文件或目录

   基本语法：

   rm [选项] 要删除的文件或目录
   常用选项：
   -r ：递归删除整个文件夹
   -f ： 强制删除不提示

   使用细节：
   强制删除不提示的方法：带上 -f 参数即可

   ~~~shell
   [root@alancentos /]# rm /home/bbb
   rm：是否删除普通空文件 "/home/bbb"？y
   [root@alancentos /]# rm -rf /opt/bbb
   ~~~

9. mv指令

   说明：mv移动文件与目录或重命名

   基本语法：
   mv oldNameFile newNameFile(功能描述：重命名)
   mv /temp/movefile /targetFolder (功能描述：移动文件)

   ~~~shell
   [root@alancentos /]# mv /home/hello.txt /home/cat.txt
   [root@alancentos /]# ls /home
   alan  bbb  cat.txt
   [root@alancentos /]# mv /home/cat.txt /home/bbb/
   [root@alancentos /]# ls /home/bbb
   cat.txt
   ~~~

10. cat 指令

    说明：cat查看文件内容

    基本语法：cat [选项] 要查看的文件

    常用选项：

    -n ：显示行号

    使用细节：

    cat只能浏览文件，而不能修改文件，为了浏览方便，一般会带上管道命令(==管道命令的作用：把前面的结果交给“|”后面的命令继续处理==)|more

    ~~~shell
    [root@alancentos /]# cat -n /etc/profile |more
    ~~~

11. more指令

    说明：more 指令是一个基于 VI 编辑器的文本过滤器，它以全屏幕的方式按页显示文本文件的内容。more 指令中内置了若
    干快捷键(交互的指令)。

    操作说明：

    * 空白键：向下翻一页
    * Enter：向下翻一行
    * q：代表立刻离开more，不再显示该文件内容
    * =：输出当前行行号
    * :f：显示输出文件名和当前行行号
    * Ctrl+f：向下滚动一屏

    ~~~shell
    [root@alanCentos01 home]# more /etc/profile
    ~~~

12. less指令

    说明：less 指令用来分屏查看文件内容，它的功能与 more 指令类似，但是比 more 指令更加强大，支持各种显示终端。less指令在显示文件内容时，并不是一次将整个文件加载之后才显示，而是根据显示需要加载内容，对于显示大型文件具有较高的效率。

    基本语法：

    less 要查看的文件

    操作说明：

    * 空白键：向下翻一页
    * [PageDown]按钮：向下翻一页
    * [PageUp]按钮：向上翻一页
    * “/”字串：向下搜索【字串】的功能，n：向下查找，N：向上查找
    * “?”字串：向上搜索【字串】的功能，n：向上查找，N：向下查找
    * q：离开这个less程序

    ~~~shell
    [root@alanCentos01 home]# less /etc/profile
    [root@alanCentos01 home]# cat -n /etc/profile | less
    ~~~

13. echo指令

    说明：echo输出内容到控制台。

    基本语法：

    echo [选项：输出内容]

    ~~~shell
    [root@alanCentos01 home]# echo $path
    
    [root@alanCentos01 home]# echo $PATH
    /usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/root/bin
    [root@alanCentos01 home]# echo $HOSTNAME
    alanCentos01
    [root@alanCentos01 home]# echo hello,world!
    hello,world!
    ~~~

14. head指令

    说明：head 用于显示文件的开头部分内容，默认情况下 head 指令显示文件的前 10 行内容。

    基本语法：

    head 文件（功能描述：查看文件头10行内容）

    head -n 5 文件（功能描述：查看文件头 5 行内容，5 可以是任意行数）

    ~~~shell
    [root@alanCentos01 home]# head -n 5 /etc/profile
    # /etc/profile
    
    # System wide environment and startup programs, for login setup
    # Functions and aliases go in /etc/bashrc
    ~~~

15. tail指令

    说明：tail用于输出文件中尾部的内容，默认情况下 tail 指令显示文件的前 10 行内容。

    基本语法：

    * tail文件（功能描述：查看文件尾10行内容）
    * tail -n 5 文件（功能描述：查看文件尾5行内容，5可以是任意行数）
    * tail -f 文件（功能描述：实时追踪该文档的所有更新，==一般用于追踪显示日志==）

    ~~~shell
    [root@alanCentos01 home]# tail -n 5 /etc/profile
        fi
    done
    
    unset i
    unset -f pathmunge
    [root@alanCentos01 /]# tail -f /var/log/vmware-install.log 
    The configuration of VMware Tools 10.3.10 build-13959562 for Linux for this 
    running kernel completed successfully.
    
    Found VMware Tools CDROM mounted at /run/media/root/VMware Tools. Ejecting 
    device /dev/sr0 ...
    Enjoy,
    
    --the VMware team
    
    Sat Apr  3 22:09:16 2021 vmware-install.pl end
    ~~~

16. “>” 指令 和 “>>” 指令

    说明：“>” 输出重定向和 “>>” 追加

    基本语法：

    * ls -l >文件（功能描述：列表的内容写入文件a.txt中（覆盖写））
    * ls -al >>文件 （功能描述：列表的内容追加到文件aa.txt的末尾）
    * cat 文件1 > 文件2（功能描述：将文件1的内容覆盖到文件2）
    * echo "内容">> 文件 （追加）

    ~~~shell
    [root@alanCentos01 /]# ls -l /home > /home/info.txt
    [root@alanCentos01 /]# cat -n /home/info.txt | more
         1	总用量 4
         2	drwx------. 5 alan alan 4096 4月   5 23:17 alan
         3	-rw-r--r--. 1 root root    0 4月  11 13:40 info.txt
    [root@alanCentos01 home]# ls
    alan  info.txt
    [root@alanCentos01 home]# cat info.txt > cat.txt
    [root@alanCentos01 home]# ls
    alan  cat.txt  info.txt
    [root@alanCentos01 home]# cat cat.txt -n | more
         1	总用量 4
         2	drwx------. 5 alan alan 4096 4月   5 23:17 alan
         3	-rw-r--r--. 1 root root    0 4月  11 13:40 info.txt
    [root@alanCentos01 home]# echo "我是追加的内容，哈哈哈" >> info.txt 
    [root@alanCentos01 home]# cat -n info.txt | more
         1	总用量 4
         2	drwx------. 5 alan alan 4096 4月   5 23:17 alan
         3	-rw-r--r--. 1 root root    0 4月  11 13:40 info.txt
         4	我是追加的内容，哈哈哈
    [root@alanCentos01 home]# cal >> info.txt
    [root@alanCentos01 home]# cat -n info.txt | more
         1	总用量 4
         2	drwx------. 5 alan alan 4096 4月   5 23:17 alan
         3	-rw-r--r--. 1 root root    0 4月  11 13:40 info.txt
         4	我是追加的内容，哈哈哈
         5	      四月 2021     
         6	日 一 二 三 四 五 六
         7	             1  2  3
         8	 4  5  6  7  8  9 10
         9	11 12 13 14 15 16 17
        10	18 19 20 21 22 23 24
        11	25 26 27 28 29 30     
    ~~~

17. ln指令

    说明：软链接也称为符号链接，类似于 windows 里的快捷方式，主要存放了链接其他文件的路径。

    基本语法：

    ln -s [原文件或目录] [软链接名] （功能描述：给原文件创建一个软链接）

    ~~~shell
    [root@alanCentos01 home]# ln -s /root /home/myroot
    [root@alanCentos01 home]# ls -l /home
    总用量 12
    drwx------. 5 alan alan 4096 4月   5 23:17 alan
    -rw-r--r--. 1 root root  114 4月  11 13:43 cat.txt
    -rw-r--r--. 1 root root  302 4月  11 13:49 info.txt
    lrwxrwxrwx. 1 root root    5 4月  11 13:51 myroot -> /root
    ~~~

    删除软连接：

    ~~~shell
    [root@alanCentos01 home]# rm /home/myroot
    rm：是否删除符号链接 "/home/myroot"？y
    [root@alanCentos01 home]# ls -l /home/
    总用量 12
    drwx------. 5 alan alan 4096 4月   5 23:17 alan
    -rw-r--r--. 1 root root  114 4月  11 13:43 cat.txt
    -rw-r--r--. 1 root root  302 4月  11 13:49 info.txt
    [root@alanCentos01 home]#
    ~~~

    ==细节说明==：当我们使用 pwd 指令查看目录时，仍然看到的是软链接所在目录。

    ```shell
    [root@alanCentos01 home]# ln -s /root /home/myroot
    [root@alanCentos01 home]# ls
    alan  cat.txt  info.txt  myroot
    [root@alanCentos01 home]# cd /home/myroot
    [root@alanCentos01 myroot]# pwd
    /home/myroot
    ```

18. history指令

    说明：查看已经执行过历史命令,也可以执行历史指令。

    基本语法：

    history （功能描述：查看已经执行过历史命令）

    ```shell
    [root@alanCentos01 home]# history
        1  ls
        2  exit
        3  sync
        4  shutdown -h now
        5  sync
        6  reboot
        7  logout
        8  cd /root
        9  ls
       10  cd /
       11  ls
       12  logout
       13  ls
       14  cd /root
       15  logout
       16  ls
       17  cd /home
       18  ls
       19  userdel milan
       20  userdel zwj
       21  ls
       22  userdel -r
       23  userdel -r zwj
       24  useradd milan
       25  userdel -r milan
       26  ls
       27  useradd zwj
       28  userdel -r zwj
       29  ls
       30  ls -l /opt
       31  cat -n /etc/profile
       32  cat -n /etc/profile |more
       33  man more
       34  cat -n /etc/profile |more
       35  more /etc/profile
       36  more /etc/profiled
       37  more /etc/profile
       38  cat -n /etc/profile | more
       39  more /etc/profile
       40  cat -n /etc/profile | less
       41  less /etc/profile
       42  echo $path
       43  echo $PATH
       44  echo $HOSTHOME
       45  echo $HOSTNAME
       46  echo hello,world
       47  echo hello,world!
       48  cat /etc/profile | head
       49  cat /etc/profile | head 1
       50  cat /etc/profile | head -n 1
       51  cat /etc/profile | head -n 2
       52  cat /etc/profile | head -n 3
       53  head -n 5 /etc/profile
       54  tail -n 5 /etc/profile
       55  ls
       56  cd /
       57  ls
       58  ls -l /var
       59  ls /var/account
       60  tail -f /var/account/pacct 
       61  ls /var/cache
       62  ls /var/cache/opt
       63  ls /var/cache
       64  ls /var/opt
       65  ls /var/tmp
       66  ls /var
       67  ls /var/local
       68  ls /var/adm
       69  ls /var/db
       70  ls /var/db/Makefile 
       71  less /var/db/Makefile 
       72  less /var/
       73  ls -l /var/
       74  ls -l /var/log
       75  tail -f /var/log/vmware-install.log 
       76  ls -l /home > /home/info.txt
       77  cat -n /home/info.txt
       78  cat -n /home/info.txt | more
       79  cd /home
       80  ls
       81  cat info.txt > cat.txt
       82  cat -n cat.txt | more
       83  ls
       84  cat cat.txt -n | more
       85  echo "我是追加的内容，哈哈哈" >> info.txt 
       86  cat -n info.txt | more
       87  cal
       88  cal >> info.txt
       89  cat -n info.txt | more
       90  ln -s /root /home/myroot
       91  ls -l /home
       92  rm /home/myroot
       93  ls -l /home/
       94  ln -s /root /home/myroot
       95  ls
       96  cd /home/myroot
       97  pwd
       98  rm /home/myroot
       99  pwd
      100  cd ..
      101  ls
      102  cd /home
      103  ls
      104  ls -l
      105  history
    [root@alanCentos01 home]# history 10
       97  pwd
       98  rm /home/myroot
       99  pwd
      100  cd ..
      101  ls
      102  cd /home
      103  ls
      104  ls -l
      105  history
      106  history 10
    ```

    ==执行历史编号为 97 的指令==：

    ```shell
    [root@alanCentos01 home]# !97
    pwd
    /home
    ```


### 十、时间日期类

1. date指令

   说明：显示当前日期

   基本语法：

   * date（功能描述：显示当前时间）
   * date +%Y（功能描述：显示当前年份）
   *  date +%m（功能描述：显示当前月份）
   *  date +%d （功能描述：显示当前是哪一天）
   *  date "+%Y-%m-%d %H:%M:%S"（功能描述：显示年月日时分秒）

   ```shell
   [root@alanCentos01 home]# date
   2021年 04月 11日 星期日 15:33:06 CST
   [root@alanCentos01 home]# date "+%Y-%m-%d"
   2021-04-11
   [root@alanCentos01 home]#  date "+%Y-%m-%d %H:%M:%S"
   2021-04-11 15:34:28
   ```


2. date指令

      说明：设置日期

      基本语法：

      date -s 字符串时间

      ~~~shell
        date -s “2020-11-03 20:02:10”
      ~~~

3. cal指令

      说明：查看日历指令cal

      基本语法：

      cal [选项]（功能描述：不加选项，显示本月日历）

      ~~~shell
      [root@alanCentos01 home]# cal
            四月 2021     
      日 一 二 三 四 五 六
                   1  2  3
       4  5  6  7  8  9 10
      11 12 13 14 15 16 17
      18 19 20 21 22 23 24
      25 26 27 28 29 30
      
      [root@alanCentos01 home]# cal 2020
                                     2020                               
      
              一月                   二月                   三月        
      日 一 二 三 四 五 六   日 一 二 三 四 五 六   日 一 二 三 四 五 六
                1  2  3  4                      1    1  2  3  4  5  6  7
       5  6  7  8  9 10 11    2  3  4  5  6  7  8    8  9 10 11 12 13 14
      12 13 14 15 16 17 18    9 10 11 12 13 14 15   15 16 17 18 19 20 21
      19 20 21 22 23 24 25   16 17 18 19 20 21 22   22 23 24 25 26 27 28
      26 27 28 29 30 31      23 24 25 26 27 28 29   29 30 31
      
              四月                   五月                   六月        
      日 一 二 三 四 五 六   日 一 二 三 四 五 六   日 一 二 三 四 五 六
                1  2  3  4                   1  2       1  2  3  4  5  6
       5  6  7  8  9 10 11    3  4  5  6  7  8  9    7  8  9 10 11 12 13
      12 13 14 15 16 17 18   10 11 12 13 14 15 16   14 15 16 17 18 19 20
      19 20 21 22 23 24 25   17 18 19 20 21 22 23   21 22 23 24 25 26 27
      26 27 28 29 30         24 25 26 27 28 29 30   28 29 30
                             31
              七月                   八月                   九月        
      日 一 二 三 四 五 六   日 一 二 三 四 五 六   日 一 二 三 四 五 六
                1  2  3  4                      1          1  2  3  4  5
       5  6  7  8  9 10 11    2  3  4  5  6  7  8    6  7  8  9 10 11 12
      12 13 14 15 16 17 18    9 10 11 12 13 14 15   13 14 15 16 17 18 19
      19 20 21 22 23 24 25   16 17 18 19 20 21 22   20 21 22 23 24 25 26
      26 27 28 29 30 31      23 24 25 26 27 28 29   27 28 29 30
                             30 31
              十月                  十一月                 十二月       
      日 一 二 三 四 五 六   日 一 二 三 四 五 六   日 一 二 三 四 五 六
                   1  2  3    1  2  3  4  5  6  7          1  2  3  4  5
       4  5  6  7  8  9 10    8  9 10 11 12 13 14    6  7  8  9 10 11 12
      11 12 13 14 15 16 17   15 16 17 18 19 20 21   13 14 15 16 17 18 19
      18 19 20 21 22 23 24   22 23 24 25 26 27 28   20 21 22 23 24 25 26
      25 26 27 28 29 30 31   29 30                  27 28 29 30 31
      [root@alanCentos01 home]#  
      ~~~

### 十一、搜索查找类

1. find指令

   说明：find指令将从指定目录向下递归地遍历其各个子目录，将满足条件的文件或者目录显示在终端。

   基本语法：

   find[搜索范围：选项]

   选项说明：

   * -name<查询方式>：对照指定的文件名查找文件
   * -user<用户名>：查找属于指定用户名的所有文件
   * -size<文件大小>：按指定的文件大小查找文件（+n 大于-n 小于n 等于, 单位有 k,M,G）

   ```shell
   [root@alanCentos01 /]# find /home -name info.txt
   /home/info.txt
   [root@alanCentos01 /]# find /home -name *.txt
   /home/info.txt
   /home/cat.txt
   [root@alanCentos01 /]# find /home -user root
   /home
   /home/info.txt
   /home/cat.txt
   [root@alanCentos01 /]# find /opt -size +10M
   /opt/VMwareTools-10.3.10-13959562.tar.gz
   ```

2. locate指令

   说明：locate 指令可以快速定位文件路径。locate 指令利用事先建立的系统中所有文件名称及路径的 locate 数据库实现快速定位给定的文件。Locate 指令无需遍历整个文件系统，查询速度较快。为了保证查询结果的准确度，管理员必须定期更新locate数据库。

   基本语法：

   locate 搜索文件

   特别说明：由于 locate 指令基于数据库进行查询，所以第一次运行前（==或系统里面有新的文件==），必须使用 updatedb 指令创建 locate 数据库。

   ~~~shell
   [root@alanCentos01 /]# updatedb
   [root@alanCentos01 /]# locate info.txt
   /home/info.txt
   /usr/share/doc/git-1.8.3.1/git-mailinfo.txt
   /usr/share/doc/git-1.8.3.1/git-update-server-info.txt
   /usr/share/doc/lshw-B.02.18/proc_usb_info.txt
   ~~~

   ==which或whereis指令，可以查看某个指令在哪个目录下，比如 ls 指令在哪个目录==

   ~~~shell
   [root@alanCentos01 home]# which ls
   alias ls='ls --color=auto'
   	/usr/bin/ls
   [root@alanCentos01 home]# whereis ls
   ls: /usr/bin/ls /usr/share/man/man1/ls.1.gz /usr/share/man/man1p/ls.1p.gz
   ~~~

3. grep指令和管道符号“|”

   说明：grep过滤查找 ，管道符“|”，表示将前一个命令的处理结果输出传递给后面的命令处理。

   基本语法：

   grep [选项] 查找内容 源文件

   常用选项：

   * -n：显示匹配行及行号
   * -i：忽略字母大小写

   ~~~shell
   [root@alanCentos01 home]# cat -n hello.txt | grep -i "yes"
        1	yes no yees y yes ok any kyes
   [root@alanCentos01 home]# grep -n -i "yes" hello.txt
   1:yes no yees y yes ok any kyes     
   ~~~


### 十二、压缩和解压类

1. gzip/gunzip指令

   说明：gzip用于压缩文件， gunzip用于解压的。

   基本语法：

   * gzip文件（功能描述：压缩文件，只能将文件压缩为*.gz 文件）

   * gunzip文件.gz （功能描述：解压缩文件命令）

   ~~~shell
   [root@alanCentos01 home]#  gzip /home/hello.txt
   [root@alanCentos01 home]# ls
   alan  cat.txt  dog.txt  hello.txt.gz  info.txt
   [root@alanCentos01 home]# gunzip /home/hello.txt.gz
   [root@alanCentos01 home]# ls
   alan  cat.txt  dog.txt  hello.txt  info.txt
   ~~~

2. zip/unzip指令

   说明：zip 用于压缩文件， unzip 用于解压的，这个在项目打包发布中很有用的。

   基本语法：

   * zip [选项] XXX.zip将要压缩的内容（功能描述：压缩文件和目录的命令）
   * unzip [选项] XXX.zip（功能描述：解压缩文件）

   zip常用选项：

   -r：递归压缩，即压缩目录

   unzip 的常用选项：

   -d<目录> ：指定解压后文件的存放目录

   ~~~shell
   [root@alanCentos01 home]# zip -r myhome.zip /home/
     adding: home/ (stored 0%)
     adding: home/info.txt (deflated 25%)
     adding: home/cat.txt (deflated 20%)
     adding: home/dog.txt (deflated 20%)
     adding: home/alan/ (stored 0%)
     adding: home/alan/.bashrc (deflated 23%)
     adding: home/alan/.config/ (stored 0%)
     adding: home/alan/.config/abrt/ (stored 0%)
     adding: home/alan/.bash_profile (deflated 21%)
     adding: home/alan/.bash_history (deflated 64%)
     adding: home/alan/.mozilla/ (stored 0%)
     adding: home/alan/.mozilla/plugins/ (stored 0%)
     adding: home/alan/.mozilla/extensions/ (stored 0%)
     adding: home/alan/.cache/ (stored 0%)
     adding: home/alan/.cache/abrt/ (stored 0%)
     adding: home/alan/.cache/abrt/lastnotification (stored 0%)
     adding: home/alan/.bash_logout (stored 0%)
     adding: home/alan/.Xauthority (stored 0%)
     adding: home/hello.txt (deflated 7%)
   [root@alanCentos01 home]# ls
   alan  cat.txt  dog.txt  hello.txt  info.txt  myhome.zip
   [root@alanCentos01 home]# mkdir /opt/tmp
   [root@alanCentos01 home]# ls /opt/
   rh  tmp  VMwareTools-10.3.10-13959562.tar.gz  vmware-tools-distrib
   [root@alanCentos01 home]# unzip -d /opt/tmp /home/myhome.zip
   Archive:  /home/myhome.zip
      creating: /opt/tmp/home/
     inflating: /opt/tmp/home/info.txt  
     inflating: /opt/tmp/home/cat.txt   
     inflating: /opt/tmp/home/dog.txt   
      creating: /opt/tmp/home/alan/
     inflating: /opt/tmp/home/alan/.bashrc  
      creating: /opt/tmp/home/alan/.config/
      creating: /opt/tmp/home/alan/.config/abrt/
     inflating: /opt/tmp/home/alan/.bash_profile  
     inflating: /opt/tmp/home/alan/.bash_history  
      creating: /opt/tmp/home/alan/.mozilla/
      creating: /opt/tmp/home/alan/.mozilla/plugins/
      creating: /opt/tmp/home/alan/.mozilla/extensions/
      creating: /opt/tmp/home/alan/.cache/
      creating: /opt/tmp/home/alan/.cache/abrt/
    extracting: /opt/tmp/home/alan/.cache/abrt/lastnotification  
    extracting: /opt/tmp/home/alan/.bash_logout  
    extracting: /opt/tmp/home/alan/.Xauthority  
     inflating: /opt/tmp/home/hello.txt  
   [root@alanCentos01 home]# ls /opt/tmp
   home
   [root@alanCentos01 home]#
   ~~~

3. tar指令

   说明：tar指令是打包指令，最后打包后的文件是 .tar.gz的文件。

   基本语法：

   tar [选项] XXX.tar.gz 打包的内容 (功能描述：打包目录，压缩后的文件格式.tar.gz)

   选项说明：

   * -c：产生.tar打包文件
   * -v：显示详细信息
   * -f：指定压缩后的文件名
   * -z：打包同时压缩
   * -x：解包.tar文件

   ```shell
   [root@alanCentos01 /]# tar -zcvf /home/pc.tar.gz home/cat.txt home/dog.txt
   home/cat.txt
   home/dog.txt
   [root@alanCentos01 /]# ls -lh /home/
   总用量 24K
   drwx------. 5 alan alan 4.0K 4月   5 23:17 alan
   -rw-r--r--. 1 root root  114 4月  11 13:43 cat.txt
   -rw-r--r--. 1 root root  114 4月  11 22:49 dog.txt
   -rw-r--r--. 1 root root   30 4月  11 22:55 hello.txt
   -rw-r--r--. 1 root root  302 4月  11 13:49 info.txt
   -rw-r--r--. 1 root root  235 4月  12 22:56 pc.tar.gz
   [root@alanCentos01 /]# tar -zcvf /opt/myhome.tar.gz home/
   home/
   home/info.txt
   home/cat.txt
   home/dog.txt
   home/pc.tar.gz
   home/alan/
   home/alan/.bashrc
   home/alan/.config/
   home/alan/.config/abrt/
   home/alan/.bash_profile
   home/alan/.bash_history
   home/alan/.mozilla/
   home/alan/.mozilla/plugins/
   home/alan/.mozilla/extensions/
   home/alan/.cache/
   home/alan/.cache/abrt/
   home/alan/.cache/abrt/lastnotification
   home/alan/.bash_logout
   home/alan/.Xauthority
   home/hello.txt
   [root@alanCentos01 /]# ls -lh /opt
   总用量 54M
   -rw-r--r--. 1 root root 1.6K 4月  12 23:03 myhome.tar.gz
   drwxr-xr-x. 2 root root 4.0K 10月 31 2018 rh
   -rw-r--r--. 1 root root  54M 6月  13 2019 VMwareTools-10.3.10-13959562.tar.gz
   drwxr-xr-x. 9 root root 4.0K 6月  13 2019 vmware-tools-distrib
   [root@alanCentos01 home]# ls
   alan  cat.txt  dog.txt  hello.txt  info.txt  pc.tar.gz
   [root@alanCentos01 home]# tar -zxvf pc.tar.gz
   home/cat.txt
   home/dog.txt
   [root@alanCentos01 home]# ls
   alan  cat.txt  dog.txt  hello.txt  home  info.txt  pc.tar.gz
   [root@alanCentos01 opt]# tar -zxvf myhome.tar.gz -C /opt/tmp/
   home/
   home/info.txt
   home/cat.txt
   home/dog.txt
   home/pc.tar.gz
   home/alan/
   home/alan/.bashrc
   home/alan/.config/
   home/alan/.config/abrt/
   home/alan/.bash_profile
   home/alan/.bash_history
   home/alan/.mozilla/
   home/alan/.mozilla/plugins/
   home/alan/.mozilla/extensions/
   home/alan/.cache/
   home/alan/.cache/abrt/
   home/alan/.cache/abrt/lastnotification
   home/alan/.bash_logout
   home/alan/.Xauthority
   home/hello.txt
   ```


### 十三、组管理和权限管理

1. 查看文件的所有者

   指令：ls –ahl

   ~~~shell
   [root@alanCentos01 home]# ls -alh
   总用量 28K
   drwxr-xr-x.  3 root root 4.0K 4月  12 23:14 .
   dr-xr-xr-x. 18 root root 4.0K 4月  12 22:59 ..
   drwx------.  5 alan alan 4.0K 4月   5 23:17 alan
   -rw-r--r--.  1 root root  114 4月  11 13:43 cat.txt
   -rw-r--r--.  1 root root  114 4月  11 22:49 dog.txt
   -rw-r--r--.  1 root root   30 4月  11 22:55 hello.txt
   -rw-r--r--.  1 root root  302 4月  11 13:49 info.txt
   ~~~

   ![image-20210413222656406](assets/CentOS7.6详细说明/image-20210413222656406.png)

2. 修改文件所有者

   指令：chown 用户名 文件名

   ~~~shell
   [root@alanCentos01 home]# ls -lh
   总用量 20K
   drwx------. 5 alan alan 4.0K 4月   5 23:17 alan
   -rw-r--r--. 1 root root    0 4月  13 22:29 apple.txt
   -rw-r--r--. 1 root root  114 4月  11 13:43 cat.txt
   -rw-r--r--. 1 root root  114 4月  11 22:49 dog.txt
   -rw-r--r--. 1 root root   30 4月  11 22:55 hello.txt
   -rw-r--r--. 1 root root  302 4月  11 13:49 info.txt
   [root@alanCentos01 home]# chown alan apple.txt 
   [root@alanCentos01 home]# ls -lh
   总用量 20K
   drwx------. 5 alan alan 4.0K 4月   5 23:17 alan
   -rw-r--r--. 1 alan root    0 4月  13 22:29 apple.txt
   -rw-r--r--. 1 root root  114 4月  11 13:43 cat.txt
   -rw-r--r--. 1 root root  114 4月  11 22:49 dog.txt
   -rw-r--r--. 1 root root   30 4月  11 22:55 hello.txt
   -rw-r--r--. 1 root root  302 4月  11 13:49 info.txt
   [root@alanCentos01 home]# 
   ~~~

3. 组的创建

   指令：groupadd 组名

   ~~~shell
   [root@alanCentos01 home]# groupadd monster
   [root@alanCentos01 home]# useradd -g monster fox
   [root@alanCentos01 home]# id fox
   uid=1001(fox) gid=1003(monster) 组=1003(monster)
   ~~~

4. 文件/目录所在组

   说明：当某个用户创建了一个文件后，这个文件的所在组就是该用户所在的组(默认)。

5. 查看文件/目录所在组

   指令：ls -ahl

   ~~~shell
   [root@alanCentos01 home]# ls -ahl
   总用量 32K
   drwxr-xr-x.  4 root root    4.0K 4月  13 22:34 .
   dr-xr-xr-x. 18 root root    4.0K 4月  12 22:59 ..
   drwx------.  5 alan alan    4.0K 4月   5 23:17 alan
   -rw-r--r--.  1 alan root       0 4月  13 22:29 apple.txt
   -rw-r--r--.  1 root root     114 4月  11 13:43 cat.txt
   -rw-r--r--.  1 root root     114 4月  11 22:49 dog.txt
   drwx------.  3 fox  monster 4.0K 4月  13 22:34 fox
   -rw-r--r--.  1 root root      30 4月  11 22:55 hello.txt
   -rw-r--r--.  1 root root     302 4月  11 13:49 info.txt
   [root@alanCentos01 home]# 
   ~~~

   ![image-20210413223853905](assets/CentOS7.6详细说明/image-20210413223853905.png)

6. 修改文件/目录所在的组

   指令：chgrp 组名 文件名

   ~~~shell
   [root@alanCentos01 home]# groupadd fruit
   [root@alanCentos01 home]# touch orange.txt
   [root@alanCentos01 home]# ls -alh
   总用量 32K
   drwxr-xr-x.  4 root root    4.0K 4月  13 22:40 .
   dr-xr-xr-x. 18 root root    4.0K 4月  12 22:59 ..
   drwx------.  5 alan alan    4.0K 4月   5 23:17 alan
   -rw-r--r--.  1 alan root       0 4月  13 22:29 apple.txt
   -rw-r--r--.  1 root root     114 4月  11 13:43 cat.txt
   -rw-r--r--.  1 root root     114 4月  11 22:49 dog.txt
   drwx------.  3 fox  monster 4.0K 4月  13 22:34 fox
   -rw-r--r--.  1 root root      30 4月  11 22:55 hello.txt
   -rw-r--r--.  1 root root     302 4月  11 13:49 info.txt
   -rw-r--r--.  1 root root       0 4月  13 22:40 orange.txt
   [root@alanCentos01 home]# chgrp fruit orange.txt 
   [root@alanCentos01 home]# ls -alh
   总用量 32K
   drwxr-xr-x.  4 root root    4.0K 4月  13 22:40 .
   dr-xr-xr-x. 18 root root    4.0K 4月  12 22:59 ..
   drwx------.  5 alan alan    4.0K 4月   5 23:17 alan
   -rw-r--r--.  1 alan root       0 4月  13 22:29 apple.txt
   -rw-r--r--.  1 root root     114 4月  11 13:43 cat.txt
   -rw-r--r--.  1 root root     114 4月  11 22:49 dog.txt
   drwx------.  3 fox  monster 4.0K 4月  13 22:34 fox
   -rw-r--r--.  1 root root      30 4月  11 22:55 hello.txt
   -rw-r--r--.  1 root root     302 4月  11 13:49 info.txt
   -rw-r--r--.  1 root fruit      0 4月  13 22:40 orange.txt
   [root@alanCentos01 home]# 
   ~~~

7. 其它组

   说明：除文件的所有者和所在组的用户外，系统的其它用户都是文件的其它组。

8. 改变用户所在组

   指令：

   * usermod –g 新组名 用户名

   * usermod –d 目录名 用户名 改变该用户登陆的初始目录。特别说明特别说明：用户需要有进入到新目录的权限。

   ~~~shell
   [root@alanCentos01 idea]# id zwj
   id: zwj: no such user
   [root@alanCentos01 idea]# useradd -g mojiao zwj
   [root@alanCentos01 idea]# id zwj
   uid=1002(zwj) gid=1002(mojiao) 组=1002(mojiao)
   [root@alanCentos01 idea]# cat /etc/group | grep wudang
   wudang:x:1001:
   [root@alanCentos01 idea]# usermod -g wudang zwj
   [root@alanCentos01 idea]# id zwj
   uid=1002(zwj) gid=1001(wudang) 组=1001(wudang)
   ~~~

9. 权限的基本介绍

   ls -l 中显示的内容如下：

   -rwxrw-r-- 1 root root 1213 Feb 2 09:39 abc

   0-9 位说明：

   第 0 位确定文件类型(d, - , l , c , b)

   * l是链接，相当于 windows 的快捷方式的快捷方式
* d是目录，相当于 windows 的文件夹的文件夹
  
   * c是字符设备文件，鼠标，键盘
* b是块设备，比如硬盘
   * -代表是一个普通文件

   第1-3位确定所有者（该文件的所有者）拥有该文件的权限---User

   第4-6位确定所属组（同用户组的）拥有该文件的权限---Group

   第7-9位确定其他用户拥有该文件的权限---Other

   ![image-20210414221527911](assets/CentOS7.6详细说明/image-20210414221527911.png)
   
   其它说明：
   
   * 1 - 文件：硬连接数（目录：子目录数）
   * root - 用户
   * root - 组
   * 1213文件大小(字节)，如果是文件夹，显示 4096 字节
   * Feb 2 09:39最后修改日期
   * abc文件名
10. rwx权限详解

    rwx作用到文件：

    * [ r ]代表可读(read)：可以读取、查看。
    * [ w ]代表可写(write)：可以修改，==但是不代表可以删除该文件，删除一个文件的前提条件是对该文件所在的目录有写权限，才能删除该文件==。
    * [ x ]代表可执行(execute)：可以被执行。

    rwx作用到目录：

    * [ r ]代表可读(read)：可以读取，ls 查看目录内容。
    * [ w ]代表可写(write)：可以修改, ==对目录内创建+删除+重命名目录==。
    * [ x ]代表可执行(execute)：可以进入该目录。

    ==说明：可用数字表示为: r=4,w=2,x=1 因此 rwx=4+2+1=7 , 数字可以进行组合（如7就表示权限rwx）。==

11. 修改权限

    基本说明：通过 chmod 指令，可以修改文件或者目录文件或者目录的权限。

    - 第一种方式：+ 、-、= 变更权限

      u：所有者，g：所有组，o：其他人，a：所有人(u、g、o 的总和)

      1) chmod u=rwx,g=rx,o=x 文件/目录名

      2) chmod o+w 文件/目录名

      3) chmod a-x 文件/目录名

      ```shell
      chmod u=rwx,g=rx,o=rx abc.txt
      chmod u-x,g+w abc.txt
      chmod a+r abc.txt
      ```

    - 第二种方式：通过数字变更权限

      r=4，w=2，x=1，rwx=4+2+1=7

      chmod u=rwx,g=rx,o=x 文件目录名：相当于 chmod 751 文件/目录名

      ```shell
      chmod 755 /home/abc.txt
      ```

12. 修改文件所有者

    基本介绍：

    chown newowner 文件/目录 改变所有者

    chown newowner:newgroup 文件/目录 改变所有者和所在组

    -R 如果是目录 则使其下所有子文件或目录递归生效

    ```shell
    chown tom /home/abc.txt
    chown -R tom /home/test
    ```

13. 修改文件/目录所在组

    基本介绍：

    chgrp newgroup 文件/目录

    ```shell
    groupadd shaolin
    chgrp shaolin /home/abc.txt
    chgrp -R shaolin /home/test
    ```


### 十四、定时任务调度

1. crond任务调度

   基本语法：

   crontab [选项]

   常用选项：

   * -e：编辑crontab定时任务
   * -l：查询当前用户crontab任务
   * -r：删除当前用户所有的crontab任务

   快速入门：

   * 设置任务调度文件：/etc/crontab

   * 设置个人任务调度：执行 crontab –e 命令，接着输入任务到调度文件。

     如：*/1 * * * * ls –l/etc/ > /tmp/to.txt（意思说每小时的每分钟执行 ls –l /etc/ > /tmp/to.txt 命令）

   参数细节说明：

   ![image-20210417114946984](assets/CentOS7.6详细说明/image-20210417114946984.png)

   特殊符号的说明：

   ![image-20210417115049322](assets/CentOS7.6详细说明/image-20210417115049322.png)

   特殊时间执行案例：

   ![image-20210417115147076](assets/CentOS7.6详细说明/image-20210417115147076.png)

   应用实例：

   案例 1：每隔 1 分钟，就将当前的日期信息，追加到 /tmp/mydate 文件中

   */1 * * * * date >> /tmp/mydate

   案例 2：每隔 1 分钟， 将当前日期和日历都追加到 /home/mycal 文件中

   步骤:

   (1) vim /home/my.sh写入内容 date >> /home/mycal 和 cal >> /home/mycal

   (2) 给 my.sh 增加执行权限，chmod u+x /home/my.sh

   (3) crontab -e增加 */1 * * * */home/my.sh

   案例 3:每天凌晨 2:00 将 mysql 数据库 testdb ，备份到文件中。

   提示: 指令为mysqldump -u root -p 密码 数据库 > /home/db.bak

   步骤(1) crontab -e

   步骤(2) 0 2 * * * mysqldump -u root -proot testdb > /home/db.bak

   crond相关指令：

   * conrtab –e：编辑crontab定时任务

   * conrtab –r：删除当前用户所有的crontab任务

   * crontab –l：列出当前用户有那些任务调度

   * 重启任务调度：

     ```shell
     [root@alancentos /]# systemctl restart crond
     ```

2. at 定时任务

   基本介绍：

   1)at 命令是一次性定时计划任务，at 的守护进程 atd 会以后台模式运行，检查作业队列来运行。

   2)默认情况下，atd 守护进程每 60 秒检查作业队列，有作业时，会检查作业运行时间，如果时间与当前时间匹配，则运行此作业。

   3)at 命令是一次性定时计划任务，执行完一个任务后不再执行此任务了。

   4)在使用 at 命令的时候，一定要保证 atd 进程的启动 , 可以使用相关指令来查看：

   ```shell
   [root@alancentos /]# ps -ef| grep atd
   root       7718      1  0 11:18 ?        00:00:00 /usr/sbin/atd -f
   root       9278   9093  0 12:01 pts/0    00:00:00 grep --color=auto atd
   [root@alancentos /]# 
   ```

   at 命令格式：

   * at [时间]
   * Ctrl + D结束 at 命令的输入， 输出两次。
   * 强制退出（不保存）：Ctrl + C

   at 时间定义：

   1)接受在当天的 hh:mm（小时:分钟）式的时间指定。假如该时间已过去，那么就放在第二天执行。 例如：04:00。

   2)使用 midnight（深夜），noon（中午），teatime（饮茶时间，一般是下午 4 点）等比较模糊的词语来指定时间。

   3)采用 12 小时计时制，即在时间后面加上 AM（上午）或 PM（下午）来说明是上午还是下午。 例如：12pm。

   4)指定命令执行的具体日期，指定格式为 month day（月 日）或 mm/dd/yy（月/日/年）或 dd.mm.yy（日.月.年），指定的日期必须跟在指定时间的后面。 例如：04:00 2021-03-1。

   5)使用相对计时法。指定格式为：now + count time-units ，now 就是当前时间，time-units 是时间单位，这里能够是 minutes（分钟）、hours（小时）、days（天）、weeks（星期）。count 是时间的数量，几天，几小时。 例如：now + 5 minutes（5分钟以后）。

   6)直接使用 today（今天）、tomorrow（明天）来指定完成命令的时间。

   应用实例：

   * 案例 1：2 天后的下午 5 点执行 /bin/ls /home

     ```shell
     [root@alancentos ~]# at 5pm + 2day
     at> /bin/ls /home<EOT>
     job 2 at Mon Apr 19 17:00:00 2021
     ```

   * 案例 2：atq 命令来查看系统中没有执行的工作任务

     ```shell
     [root@alancentos ~]# atq
     2	Mon Apr 19 17:00:00 2021 a root
     1	Mon Apr 19 17:00:00 2021 a root
     ```

   * 案例 3：明天 17 点钟，输出时间到指定文件内 比如 /home/date100.log

     ```shell
     [root@alancentos ~]# at 5pm tomorrow
     at> date > /home/date100.log<EOT>
     job 7 at Sun Apr 18 17:00:00 2021
     ```

   * 案例 4：2 分钟后，输出时间到指定文件内 比如 /home/date200.log

     ```shell
     [root@alancentos ~]# at now + 2 minutes
     at> date > /home/date200.log<EOT>
     job 5 at Sat Apr 17 16:17:00 2021
     ```

   * 案例 5：删除已经设置的任务 , atrm 编号

     ```shell
     [root@alancentos ~]# atrm 2
     [root@alancentos ~]# atrm 7
     ```

### 十五、Linux 磁盘分区、挂载

1. Linux 分区：

   原理介绍：

   1)Linux 来说无论有几个分区，分给哪一目录使用，它归根结底就只有一个根目录，一个独立且唯一的文件结构 , Linux中每个分区都是用来组成整个文件系统的一部分。

   2)Linux 采用了一种叫“载入”的处理方法，它的整个文件系统中包含了一整套的文件和目录，且将一个分区和一个目录联系起来。这时要载入的一个分区将使它的存储空间在一个目录下获得。

2. 硬盘说明：

   1)Linux 硬盘分 IDE 硬盘和 SCSI 硬盘，目前基本上是 SCSI 硬盘

   2)对于 IDE 硬盘，驱动器标识符为“hdx~”,其中“hd”表明分区所在设备的类型，这里是指 IDE 硬盘了。“x”为盘号（a 为基本盘，b 为基本从属盘，c 为辅助主盘，d 为辅助从属盘）,“~”代表分区，前四个分区用数字 1 到 4 表示，它们是主分区或扩展分区，从 5 开始就是逻辑分区。例，hda3 表示为第一个 IDE 硬盘上的第三个主分区或扩展分区,hdb2表示为第二个 IDE 硬盘上的第二个主分区或扩展分区。

   3)对于 SCSI 硬盘则标识为“sdx~”，SCSI 硬盘是用“sd”来表示分区所在设备的类型的，其余则和 IDE 硬盘的表示方法一样。

   ```shell
   [root@alancentos /]# lsblk
   NAME   MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
   sda      8:0    0   20G  0 disk 
   ├─sda1   8:1    0    1G  0 part /boot
   ├─sda2   8:2    0    2G  0 part [SWAP]
   └─sda3   8:3    0   17G  0 part /
   sr0     11:0    1  4.3G  0 rom
   ```

3. 查看所有设备挂载情况

   命令：lsblk 或者 lsblk -f

   ```shell
   [root@alancentos /]# lsblk
   NAME   MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
   sda      8:0    0   20G  0 disk 
   ├─sda1   8:1    0    1G  0 part /boot
   ├─sda2   8:2    0    2G  0 part [SWAP]
   └─sda3   8:3    0   17G  0 part /
   sr0     11:0    1  4.3G  0 rom
   [root@alancentos /]# lsblk -f
   NAME   FSTYPE LABEL UUID                                 MOUNTPOINT
   sda                                                      
   ├─sda1 ext4         6a2b7463-ff43-489c-82dd-e6b1626396f1 /boot
   ├─sda2 swap         a29ce550-1829-4db8-b6b5-85742a02a526 [SWAP]
   └─sda3 ext4         7b19ed3e-a667-475e-91f9-bf9cb964b98f /
   ```

4. 挂载的经典案例：

   * 说明：下面我们以增加一块硬盘增加一块硬盘为例来熟悉下磁盘的相关指令和深入理解磁盘分区、挂载、卸载的概念。

   * 如何增加一块硬盘：

     1)虚拟机添加硬盘

     2)分区

     3)格式化

     4)挂载

     5)设置可以自动挂载

   * 虚拟机增加硬盘步骤1：

     在【虚拟机】菜单中，选择【设置】，然后设备列表里添加硬盘，然后一路【下一步】，中间只有选择磁盘大小的地方需要修改，至到完成。==然后重启系统（才能识别）==。
   ![image-20210417171147139](assets/CentOS7.6详细说明/image-20210417171147139.png)

   * 虚拟机增加硬盘步骤2：

     分区命令：fdisk /dev/sdb

     开始对/sdb 分区

     * m：显示命令列表
     * p：显示磁盘分区 同 fdisk –l
     * n：新增分区
     * d：删除分区
     * w：写入并退出
     * 说明： 开始分区后输入 n，新增分区，然后选择 p ，分区类型为主分区。两次回车默认剩余全部空间。最后输入 w写入分区并退出，若不保存退出输入 q。
    ![image-20210417171453396](assets/CentOS7.6详细说明/image-20210417171453396.png)

   * 虚拟机增加硬盘步骤3：

     格式化磁盘：

     分区命令：mkfs -t ext4 /dev/sdb1（其中 ext4 是分区类型）

   * 虚拟机增加硬盘步骤4：

     挂载：将一个分区与一个目录联系起来

     命令：mount 设备名称 挂载目录

     例如：mount /dev/sdb1 /newdisk

     取消挂载：umount 设备名称 或者挂载目录

     例如：umount /dev/sdb1 或者 umount /newdisk

     ==注意: 用命令行挂载,重启后会失重启后会失效==

   * 虚拟机增加硬盘步骤5
     永久挂载: 通过修改/etc/fstab 实现挂载，添加完成后，执行 mount –a 即刻生效。
     ![image-20210418001537359](assets/CentOS7.6详细说明/image-20210418001537359.png)

5. 磁盘情况查询

   * 查询系统整体磁盘使用情况

     基本语法：df -h

     应用实例：查询系统整体磁盘使用情况

     ```shell
     [root@alanCentos01 /]# df -h
     文件系统        容量  已用  可用 已用% 挂载点
     /dev/sda3        17G  7.8G  8.1G   50% /
     devtmpfs        895M     0  895M    0% /dev
     tmpfs           910M     0  910M    0% /dev/shm
     tmpfs           910M   11M  900M    2% /run
     tmpfs           910M     0  910M    0% /sys/fs/cgroup
     /dev/sda1       976M  144M  766M   16% /boot
     .host:/         601G  517G   84G   87% /mnt/hgfs
     tmpfs           182M   12K  182M    1% /run/user/42
     tmpfs           182M     0  182M    0% /run/user/0
     ```

   * 查询指定目录的磁盘占用情况

     基本语法：du -h

     查询指定目录的磁盘占用情况，默认为当前目录
     参数：

     -s 指定目录占用大小汇总

     -h 带计量单位

     -a 含文件

     --max-depth=1子目录深度

     -c 列出明细的同时，增加汇总值

     应用实例：查询 /opt 目录的磁盘占用情况，深度为 1

     ```shell
     [root@alanCentos01 /]# du -hac --max-depth=1 /opt
     54M	/opt/VMwareTools-10.3.10-13959562.tar.gz
     2.3G	/opt/idea
     163M	/opt/vmware-tools-distrib
     4.0K	/opt/rh
     2.6G	/opt
     2.6G	总用量
     [root@alanCentos01 /]#
     ```

   * 磁盘情况-工作实用指令

     1)统计/opt 文件夹下文件的个数

     ```shell
     [root@alanCentos01 /]# ls -l /opt | grep "^-" | wc -l
     1
     ```

     2)统计/opt 文件夹下目录的个数

     ```shell
     [root@alanCentos01 /]# ls -l /opt | grep "^d" | wc -l
     3
     ```

     3)统计/opt 文件夹下文件的个数，包括子文件夹里的

     ```shell
     [root@alanCentos01 /]# ls -lR /opt | grep "^-" | wc -l
     6061
     ```

     4)统计/opt 文件夹下目录的个数，包括子文件夹里的

     ```shell
     [root@alanCentos01 /]# ls -lR /opt | grep "^d" | wc -l
     1887
     ```

     5)以树状显示目录结构 tree 目录 ， 注意，如果没有 tree ,则使用 yum install tree 安装

     ```shell
     [root@alanCentos01 /]# tree /home
     /home
     ├── alan
     ├── apple.txt
     ├── cat.txt
     ├── dog.txt
     ├── fox
     ├── hello.txt
     ├── info.txt
     ├── orange.txt
     ├── position.sh
     └── zwj
     ```


### 十六、网络配置

1. 查看虚拟网络编辑器和修改IP地址

   ![image-20210418133131035](assets/CentOS7.6详细说明/image-20210418133131035.png)

2. 查看网关

   ![image-20210418133329287](assets/CentOS7.6详细说明/image-20210418133329287.png)

3. 查看 windows 环境的中 VMnet8 网络配置 (ipconfig 指令)

   ```powershell
   以太网适配器 VMware Network Adapter VMnet8:
   
      连接特定的 DNS 后缀 . . . . . . . :
      本地链接 IPv6 地址. . . . . . . . : fe80::c59c:7fd8:7caf:20e%14
      IPv4 地址 . . . . . . . . . . . . : 192.168.18.1
      子网掩码  . . . . . . . . . . . . : 255.255.255.0
      默认网关. . . . . . . . . . . . . :
   ```

4. 查看 linux 的网络配置(ifconfig指令)

   ```shell
   ens33: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
           inet 192.168.18.128  netmask 255.255.255.0  broadcast 192.168.18.255
           inet6 fe80::d886:4d18:f97f:b5c2  prefixlen 64  scopeid 0x20<link>
           ether 00:0c:29:75:8c:9a  txqueuelen 1000  (Ethernet)
           RX packets 4248173  bytes 5947975593 (5.5 GiB)
           RX errors 0  dropped 0  overruns 0  frame 0
           TX packets 368539  bytes 41417102 (39.4 MiB)
           TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
   ```

5. ping测试主机之间网络连通性

   基本语法：

   ping 目的主机（功能描述：测试当前服务器是否可以连接目的主机）

   应用实例：

   测试当前服务器是否可以连接百度

   ```shell
   [root@alanCentos01 idea]# ping www.baidu.com
   PING www.a.shifen.com (14.215.177.38) 56(84) bytes of data.
   64 bytes from 14.215.177.38 (14.215.177.38): icmp_seq=1 ttl=128 time=18.4 ms
   64 bytes from 14.215.177.38 (14.215.177.38): icmp_seq=2 ttl=128 time=19.1 ms
   64 bytes from 14.215.177.38 (14.215.177.38): icmp_seq=3 ttl=128 time=19.0 ms
   64 bytes from 14.215.177.38 (14.215.177.38): icmp_seq=4 ttl=128 time=19.1 ms
   64 bytes from 14.215.177.38 (14.215.177.38): icmp_seq=5 ttl=128 time=19.0 ms
   64 bytes from 14.215.177.38 (14.215.177.38): icmp_seq=6 ttl=128 time=18.9 ms
   ```

6. linux网络环境配置

   * 第一种方法(自动获取)：

     说明：登陆后，通过界面的来设置自动获取 ip，特点：linux 启动后会自动获取 IP,缺点是每次自动获取的 ip 地址可能不一样。

   * 第二种方法(指定 ip)：

     说明：直接修改配置文件来指定 IP,并可以连接到外网(程序员推荐)。

     编辑：vim /etc/sysconfig/network-scripts/ifcfg-ens33

     要求：将 ip 地址配置的静态的，比如: ip 地址为 192.168.200.130

     ifcfg-ens33 文件说明

     DEVICE=eth0																   #接口名（设备,网卡）

     HWADDR=00:0C:2x:6x:0x:xx										#MAC 地址

     TYPE=Ethernet															#网络类型（通常是 Ethemet）

     UUID=7a32ed1e-a44b-4679-a535-5c6935e5e6f4	 #随机 id

     #系统启动的时候网络接口是否有效（yes/no）

     ONBOOT=yes

     #IP 的配置方法[none|static|bootp|dhcp]（引导时不使用协议|静态分配 IP|BOOTP 协议|DHCP 协议）
     
     BOOTPROTO=static
     
     #IP 地址
     
     IPADDR=192.168.18.128
     
     #网关
     
     GATEWAY=192.168.18.2
     
     #域名解析器
     
     DNS1=192.168.18.2
     
     完整配置如下：
     
     ```shell
     TYPE=Ethernet
     PROXY_METHOD=none
     BROWSER_ONLY=no
     BOOTPROTO=static
     DEFROUTE=yes
     IPV4_FAILURE_FATAL=no
     IPV6INIT=yes
     IPV6_AUTOCONF=yes
     IPV6_DEFROUTE=yes
     IPV6_FAILURE_FATAL=no
     IPV6_ADDR_GEN_MODE=stable-privacy
     NAME=ens33
     UUID=7a32ed1e-a44b-4679-a535-5c6935e5e6f4
     DEVICE=ens33
     ONBOOT=yes
     IPADDR=192.168.18.7
     PREFIX=24
     GATEWAY=192.168.18.2
     DNS1=192.168.18.2
     ```
     
     ==重启网络服务或者重启系统生效==
     
     ```shell
     [root@alanCentos01 idea]# systemctl restart network
     [root@alanCentos01 idea]# reboot
     ```
   
7. 设置主机名

   1)为了方便记忆，可以给 linux 系统设置主机名, 也可以根据需要修改主机名。

   2)指令hostname ：查看主机名

   ```shell
   [root@alanCentos01 opt]# hostname
   alanCentos01
   ```

   3)修改文件在/etc/hostname，指定主机名。

   ==4)修改后，重启重启生效。==

   ```shell
   [root@alanCentos01 opt]# sync
   [root@alanCentos01 opt]# reboot
   ```

8. 设置 hosts 映射

   思考：如何通过主机名能够找到(比如ping) 某个linux 系统？

   windows的方法：

   在C:\Windows\System32\drivers\etc\hosts文件指定即可。

   案例: 192.168.200.130 hspedu100

   linux的方法：

   在/etc/hosts 文件指定。

   案例: 192.168.200.1 ThinkPad-PC

9. 主机名解析过程分析(Hosts、DNS)

   * Hosts是什么？

     一个文本文件，用来记录记录 IP 和 Hostname(主机名主机名)的映射关系。

   * DNS是什么？

   * DNS，就是 Domain Name System 的缩写，翻译过来就是域名系统；是互联网上作为域名和 IP 地址相互映射的一个分布式数据分布式数据库。

   * 应用实例: 用户在浏览器输入了www.baidu.com，分析其域名解析过程。

     1)浏览器先检查浏览器缓存中有没有该域名解析 IP 地址，有就先调用这个 IP 完成解析；如果没有，就检查 DNS 解析器缓存，如果有直接返回 IP 完成解析。这两个缓存，可以理解为本地解析器缓存。

     2)一般来说，当电脑第一次成功访问某一网站后，在一定时间内，浏览器或操作系统会缓存他的 IP 地址（DNS 解析记录）。如 在 cmd 窗口中输入

     * ipconfig /displaydns //DNS 域名解析缓存

     * ipconfig /flushdns //手动清理 dns 缓存

     3)如果本地解析器缓存没有找到对应映射，检查系统中hosts文件中有没有配置对应的域名 IP 映射，如果有，则完成解析并返回。

     4)如果本地DNS解析器缓存和hosts文件中均没有找到对应的IP，则到域名服务DNS进行解析域。

     5)示意图

     ![image-20210418220522246](assets/CentOS7.6详细说明/image-20210418220522246.png)

### 十七、进程管理(重点)

1. 显示系统执行的进程

   * 基本介绍：

     ps 命令是用来查看目前系统中，有哪些正在执行，以及它们执行的状况。可以不加任何参数。

     参数说明：

     * ps -a：显示当前终端的所有进程信息

     * ps -u：以用户的格式显示进程信息

     * ps -x：显示后台进程运行的参数

       ```shell
       [root@alanCentos01 ~]# ps -aux | more
       USER        PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
       root          1  2.1  0.3 128268  6888 ?        Ss   23:15   0:02 /usr/lib/systemd/systemd --switched-root --system --deserialize 22
       root          2  0.0  0.0      0     0 ?        S    23:15   0:00 [kthreadd]
       root          3  0.0  0.0      0     0 ?        S    23:15   0:00 [ksoftirqd/0]
       root          4  0.0  0.0      0     0 ?        S    23:15   0:00 [kworker/0:0]
       root          5  0.0  0.0      0     0 ?        S<   23:15   0:00 [kworker/0:0H]
       root          6  0.0  0.0      0     0 ?        S    23:15   0:00 [kworker/u256:0]
       root          7  0.0  0.0      0     0 ?        S    23:15   0:00 [migration/0]
       root          8  0.0  0.0      0     0 ?        S    23:15   0:00 [rcu_bh]
       root          9  0.3  0.0      0     0 ?        S    23:15   0:00 [rcu_sched]
       root         10  0.0  0.0      0     0 ?        S<   23:15   0:00 [lru-add-drain]
       root         11  0.0  0.0      0     0 ?        S    23:15   0:00 [watchdog/0]
       root         12  0.0  0.0      0     0 ?        S    23:15   0:00 [watchdog/1]
       root         13  0.0  0.0      0     0 ?        S    23:15   0:00 [migration/1]
       root         14  0.0  0.0      0     0 ?        S    23:15   0:00 [ksoftirqd/1]
       root         15  0.1  0.0      0     0 ?        S    23:15   0:00 [kworker/1:0]
       root         16  0.0  0.0      0     0 ?        S<   23:15   0:00 [kworker/1:0H]
       root         18  0.0  0.0      0     0 ?        S    23:15   0:00 [kdevtmpfs]
       root         19  0.0  0.0      0     0 ?        S<   23:15   0:00 [netns]
       root         20  0.0  0.0      0     0 ?        S    23:15   0:00 [khungtaskd]
       root         21  0.0  0.0      0     0 ?        S<   23:15   0:00 [writeback]
       root         22  0.0  0.0      0     0 ?        S<   23:15   0:00 [kintegrityd]
       root         23  0.0  0.0      0     0 ?        S<   23:15   0:00 [bioset]
       root         24  0.0  0.0      0     0 ?        S<   23:15   0:00 [bioset]
       root         25  0.0  0.0      0     0 ?        S<   23:15   0:00 [bioset]
       root         26  0.0  0.0      0     0 ?        S<   23:15   0:00 [kblockd]
       root         27  0.0  0.0      0     0 ?        S<   23:15   0:00 [md]
       root         28  0.0  0.0      0     0 ?        S<   23:15   0:00 [edac-poller]
       root         29  0.0  0.0      0     0 ?        S<   23:15   0:00 [watchdogd]
       root         30  0.0  0.0      0     0 ?        S    23:15   0:00 [kworker/0:1]
       root         35  0.0  0.0      0     0 ?        S    23:15   0:00 [kswapd0]
       root         36  0.0  0.0      0     0 ?        SN   23:15   0:00 [ksmd]
       root         37  0.0  0.0      0     0 ?        SN   23:15   0:00 [khugepaged]
       root         38  0.0  0.0      0     0 ?        S<   23:15   0:00 [crypto]
       root         46  0.0  0.0      0     0 ?        S<   23:15   0:00 [kthrotld]
       root         47  0.0  0.0      0     0 ?        S    23:15   0:00 [kworker/u256:1]
       root         48  0.0  0.0      0     0 ?        S<   23:15   0:00 [kmpath_rdacd]
       root         49  0.0  0.0      0     0 ?        S<   23:15   0:00 [kaluad]
       root         50  0.0  0.0      0     0 ?        S    23:15   0:00 [kworker/1:1]
       root         51  0.0  0.0      0     0 ?        S<   23:15   0:00 [kpsmoused]
       root         52  0.0  0.0      0     0 ?        S    23:15   0:00 [kworker/0:2]
       ```

     指令说明：

     * System V 展示风格
     * USER：用户名称
     * PID：进程号
     * %CPU：进程占用 CPU 的百分比
     * %MEM：进程占用物理内存的百分比
     * VSZ：进程占用的虚拟内存大小（单位：KB）
     * RSS：进程占用的物理内存大小（单位：KB）
     * TT：终端名称（缩写）。
     * STAT：进程状态，其中 S-睡眠，s-表示该进程是会话的先导进程，N-表示进程拥有比普通优先级更低的优先级，R-正在运行，D-短期等待，Z-僵死进程，T-被跟踪或者被停止等等。
     * STARTED：进程的启动时间
     * TIME：CPU 时间，即进程使用 CPU 的总时间
     * COMMAND：启动进程所用的命令和参数，如果过长会被截断显示

   * ps详解：

     1)指令：ps -aux|grep xxx ，比如我看看有没有sshd服务

     ```shell
     [root@alanCentos01 ~]# ps -aux | grep sshd
     root       7713  0.0  0.2 112756  4312 ?        Ss   18:23   0:00 /usr/sbin/sshd -D
     root      17427  0.0  0.2 158712  5556 ?        Ss   18:26   0:00 sshd: root@pts/0
     root      20225  0.0  0.0 112728   988 pts/0    S+   23:05   0:00 grep --color=auto sshd
     ```

   * 应用实例

     要求：以全格式显示当前所有的进程，查看进程的父进程。 查看 sshd 的父进程信息。

     ps -ef 是以全格式显示当前所有的进程

     -e 显示所有进程。-f 全格式

     ps -ef|grep sshd

     ```shell
     [root@alanCentos01 ~]# ps -ef
     UID         PID   PPID  C STIME TTY          TIME CMD
     root          1      0  0 23:15 ?        00:00:02 /usr/lib/systemd/systemd --switched-root --system --deserialize 22
     root          2      0  0 23:15 ?        00:00:00 [kthreadd]
     root          3      2  0 23:15 ?        00:00:00 [ksoftirqd/0]
     root          5      2  0 23:15 ?        00:00:00 [kworker/0:0H]
     root          7      2  0 23:15 ?        00:00:02 [migration/0]
     root          8      2  0 23:15 ?        00:00:00 [rcu_bh]
     root          9      2  0 23:15 ?        00:00:00 [rcu_sched]
     root         10      2  0 23:15 ?        00:00:00 [lru-add-drain]
     root         11      2  0 23:15 ?        00:00:00 [watchdog/0]
     root         12      2  0 23:15 ?        00:00:00 [watchdog/1]
     root         13      2  0 23:15 ?        00:00:00 [migration/1]
     root         14      2  0 23:15 ?        00:00:00 [ksoftirqd/1]
     root         15      2  0 23:15 ?        00:00:01 [kworker/1:0]
     root         16      2  0 23:15 ?        00:00:00 [kworker/1:0H]
     
     ```

     UID：用户 ID

     PID：进程 ID

     PPID：父进程 ID

     C：CPU 用于计算执行优先级的因子。数值越大，表明进程是 CPU 密集型运算，执行优先级会降低；数值越小，表明进程是 I/O 密集型运算，执行优先级会提高。
     STIME：进程启动的时间

     TTY：完整的终端名称

     TIME：CPU 时间

     CMD：启动进程所用的命令和参数

2. 终止进程 kill 和 killall

   介绍：若是某个进程执行一半需要停止时，或是已消了很大的系统资源时，此时可以考虑停止该进程。使用 kill 命令来完成此项任务。

   基本语法：

   kill [选项] 进程号（功能描述：通过进程号杀死/终止进程）。

   killall 进程名称 （功能描述：通过进程名称杀死进程，也支持通配符，这在系统因负载过大而变得很慢时很有用）。

   常用选项：

   -9 :表示强迫进程立即停止

   最佳实践：

   1)案例 1：踢掉某个非法登录用户

   kill 进程号 , 比如 kill 11421

   2)案例 2： 终止远程登录服务sshd，在适当时候再次重启 sshd 服务

   kill sshd对应的进程号；/bin/systemctl start sshd.service

   3)案例 3：终止多个gedit 

   killall gedit

   4)案例 4：强制杀掉一个终端

   kill - 9 bash对应的进程号

   ==（注意：bash是本机的命令行，需在本机运行kill命令）==

3. 查看进程树pstree

   pstree [选项] ,可以更加直观的来看进程信息。

   常用选项：

   -p :显示进程的 PID

   -u :显示进程的所属用户

   应用实例：

   案例 1：请你树状的形式显示进程的 pid

   pstree -p

   案例 2：请你树状的形式进程的用户

   pstree -u

### 十八、服务(service)管理

1. 介绍：服务(service) 本质就是进程，但是是运行在后台的，通常都会监听某个端口，等待其它程序的请求，比如(mysqld , sshd防火墙等)，因此我们又称为守护进程，是 Linux 中非常重要的知识点。

2. service 管理指令：

   1)service 服务名 [start | stop | restart | reload | status]

   2)在CentOS7.0后很多服务不再使用service ,而是 systemctl（后面专门讲）

   3)service指令管理的服务在/etc/init.d查看（==其他的服务都要用systemctl指令来管理了==）

   ```shell
   [root@alanCentos01 ~]# ls -l /etc/init.d/
   总用量 88
   -rw-r--r--. 1 root root 18281 8月  24 2018 functions
   -rwxr-xr-x. 1 root root  4569 8月  24 2018 netconsole
   -rwxr-xr-x. 1 root root  7923 8月  24 2018 network
   -rw-r--r--. 1 root root  1160 10月 31 2018 README
   -rwxr-xr-x. 1 root root 45702 4月   3 22:07 vmware-tools
   [root@alanCentos01 ~]# 
   ```

3. service管理指令案例：

   请使用 service 指令，查看，关闭，启动 network [注意：在虚拟系统演示，因为网络连接会关闭]

   service network status

   service network stop

   service network start

4. 查看服务名：

   使用 setup -> 系统服务 就可以看到全部。

   执行指令：setup

   ![image-20210421215514478](assets/CentOS7.6详细说明/image-20210421215514478.png)

   ==注意：图中带有“*”的，会在centos启动时一并启动。==

5. 服务的运行级别(runlevel)：

   Linux 系统有 7 种运行级别(runlevel)：常用的是级别级别 3 和 5。

   运行级别 0：系统停机状态，系统默认运行级别不能设为 0，否则不能正常启动。

   运行级别 1：单用户工作状态，root 权限，用于系统维护，禁止远程登陆。

   运行级别 2：多用户状态(没有 NFS)，不支持网络。

   运行级别 3：完全的多用户状态(有 NFS)，无界面，登陆后进入控制台命令行模式。

   运行级别 4：系统未使用，保留。

   运行级别 5：X11 控制台，登陆后进入图形 GUI 模式。

   运行级别 6：系统正常关闭并重启，默认运行级别不能设为 6，否则不能正常启动。

   开机的流程说明：

   ![image-20210421220303444](assets/CentOS7.6详细说明/image-20210421220303444.png)

7. CentOS7运行级别说明：

   在 /etc/initab
   
   进行了简化 ，如下:
   
   multi-user.target: analogous to runlevel 3

   graphical.target: analogous to runlevel 5
   
   init 0
   
   #To view current default target, run:
   
   systemctl get-default
   
   #To set a default target, run:
   
   systemctl set-default TARGET.target
   
7. chkconfig 指令

   介绍：

   通过 chkconfig 命令可以给服务的各个运行级别设置自启动/关闭

   chkconfig 指令管理的服务在 /etc/init.d 查看

   注意: Centos7.0 后，很多服务使用 systemctl 管理 

   chkconfig 基本语法：

   1)查看服务 chkconfig--list [| grepxxx]

   ```shell
   [root@alanCentos01 ~]# chkconfig --list
   
   注：该输出结果只显示 SysV 服务，并不包含
   原生 systemd 服务。SysV 配置数据
   可能被原生 systemd 配置覆盖。 
   
         要列出 systemd 服务，请执行 'systemctl list-unit-files'。
         查看在具体 target 启用的服务请执行
         'systemctl list-dependencies [target]'。
   
   netconsole     	0:关	1:关	2:关	3:关	4:关	5:关	6:关
   network        	0:关	1:关	2:开	3:开	4:开	5:开	6:关
   vmware-tools   	0:关	1:关	2:开	3:开	4:开	5:开	6:关
   [root@alanCentos01 ~]# 
   
   ```

   2)chkconfig 服务名 --list

   3)chkconfig --level 5 服务名 on/off

   案例演示 ：对 network 服务进行各种操作, 把 network 在 3 运行级别,关闭自启动

   chkconfig --level 3 network off

   chkconfig --level 3 network on

   ==注意：chkconfig 重新设置服务后自启动或关闭，需要重启机器 reboot 生效。==

8. systemctl管理指令：

   * 基本语法： systemctl [start | stop | restart | status] 服务名

     systemctl指令管理的服务在/usr/lib/systemd/system查看

     ```shell
     [root@alanCentos01 ~]# ls -l /usr/lib/systemd/system/
     总用量 1568
     -rw-r--r--. 1 root root  275 11月 14 2018 abrt-ccpp.service
     -rw-r--r--. 1 root root  380 11月 14 2018 abrtd.service
     -rw-r--r--. 1 root root  361 11月 14 2018 abrt-oops.service
     -rw-r--r--. 1 root root  266 11月 14 2018 abrt-pstoreoops.service
     -rw-r--r--. 1 root root  262 11月 14 2018 abrt-vmcore.service
     -rw-r--r--. 1 root root  311 11月 14 2018 abrt-xorg.service
     -rw-r--r--. 1 root root  729 10月 31 2018 accounts-daemon.service
     ```

   * systemctl设置服务的自启动状态：

     systemctl list-unit-files [ | grep 服务名] (查看服务开机启动状态, grep 可以进行过滤)

     systemctl enable 服务名 (设置服务开机启动)

     systemctl disable 服务名 (关闭服务开机启动)

     systemctl is-enabled 服务名 (查询某个服务是否是自启动的)

   * 应用案例：

     查看当前防火墙的状况，关闭防火墙和重启防火墙。=> firewalld.service

     systemctl status firewalld

     systemctl stop firewalld

     systemctl start firewalld

   * ==注意：==

     关闭或者启用防火墙后，立即生效。[telnet 测试某个端口即可]

     这种方式只是临时生效，当重启系统后，还是回归以前对服务的设置。

     如果希望设置某个服务自启动或关闭永久生效，要使用 systemctl [enable|disable] 服务名。

9. 打开或者关闭指定端口：

   * firewall指令：

     1)打开端口：firewall-cmd --permanent --add-port=端口号/协议

     2)关闭端口：firewall-cmd --permanent --remove-port=端口号/协议

     3)重新载入,才能生效：firewall-cmd --reload

     4)查询端口是否开放：firewall-cmd --query-port=端口/协议

     5)查询已经开放的全部端口：firewall-cmd --list-ports或firewall-cmd --zone=public --list-ports

   * 应用案例：

     1)启用防火墙， 测试111端口是否能 telnet(windows需通过“启用或关闭windows功能”安装telnet客户端), 不行

     ```powershell
     PS C:\Users\Alan> telnet 192.168.18.128 111
     正在连接192.168.18.128...
     ```
   ```
     
   2)开放111端口
     
   ​```shell
     [root@alanCentos01 ~]# firewall-cmd --permanent --add-port=111/tcp
   success
     [root@alanCentos01 ~]# firewall-cmd --reload
     success
   ```

     3)再次关闭111端口

     ```shell
     [root@alanCentos01 ~]# firewall-cmd --permanent --remove-port=111/tcp
     success
     [root@alanCentos01 ~]# firewall-cmd --reload
     success
     [root@alanCentos01 ~]# firewall-cmd --query-port=111/tcp
     no
     ```

10. 动态监控进程：

    * 介绍：

      top 与 ps 命令很相似。它们都用来显示正在执行的进程。Top 与 ps 最大的不同之处，在于 top 在执行一段时间可以更新正在运行的的进程。

    * 基本语法：

      top [选项]

      ![image-20210424173900438](assets/CentOS7.6详细说明/image-20210424173900438.png)

    * 选项说明：

      -d 秒数：指定top命令每隔几秒更新，默认是3秒。

      -i：使top不显示任何闲置或者僵死进程。

      -p：通过指定监控进程ID来仅仅监控某个进程的状态==（注意大小写）==。

    * 交互操作说明：

      P：以CPU使用率排序，默认就是次项

      M：以内存的使用率排序

      N：以PID排序

      q：退出top

    * 应用实例：

      案例 1.监视特定用户, 比如我们监控 tom 用户

      top：输入此命令，按回车键，查看执行的进程；
    
      u：然后输入“u”回车，再输入用户名，即可。
    
      案例 2：终止指定的进程, 比如我们要结束 tom 登录
    
      top：输入此命令，按回车键，查看执行的进程；
    
      k：然后输入“k”回车，再输入要结束的进程 ID 号，然后输入信号量：9==（强制杀进程）==
    
      案例 3:指定系统状态更新的时间(每隔 10 秒自动更新), 默认是 3 秒
    
      top -d 10
    
11. 监控网络状态

    * 查看系统网络情况指令：netstat

    * 基本语法：

      netstat [选项]

    * 选项说明：

      -an：按一定顺序排列输出

      -p：显示哪个进程在调用

    * 应用案例：

      请查看服务名为 sshd 的服务的信息。

      ```shell
      [root@alanCentos01 ~]# netstat -anp | grep sshd
      tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      7742/sshd           
      tcp        0      0 127.0.0.1:6010          0.0.0.0:*               LISTEN      17485/sshd: root@pt 
      tcp        0      0 192.168.18.128:22       192.168.18.1:4007       ESTABLISHED 17485/sshd: root@pt 
      tcp6       0      0 :::22                   :::*                    LISTEN      7742/sshd           
      tcp6       0      0 ::1:6010                :::*                    LISTEN      17485/sshd: root@pt 
      unix  3      [ ]         STREAM     CONNECTED     42978    7742/sshd            
      unix  2      [ ]         DGRAM                    60100    17485/sshd: root@pt  
      [root@alanCentos01 ~]# 
      ```

### 十九、RPM与YUM

1. rpm 包的简单查询指令

   查询已安装的 rpm 列表rpm –qa|grep xx

   举例：看看当前系统，是否安装了 firefox

   ```shell
   [root@alanCentos01 ~]# rpm -qa|grep firefox
   firefox-60.2.2-1.el7.centos.x86_64
   [root@alanCentos01 ~]#
   ```

2. rpm 包名基本格式

   一个 rpm 包名：firefox-60.2.2-1.el7.centos.x86_64

   名称：firefox

   版本号：60.2.2-1

   适用操作系统：el7.centos.x86_64

   表示 centos7.x 的 64 位系统

   如果是 i686、i386 表示 32 位系统，noarch 表示通用

3. rpm 包的其它查询指令：

   rpm -qa：查询所安装的所有 rpm 软件包

   rpm -qa | more

   rpm -qa | grep X [rpm -qa | grep firefox ]

   rpm -q 软件包名 :查询软件包是否安装

   ```shell
   [root@alanCentos01 ~]# rpm -q firefox
   firefox-60.2.2-1.el7.centos.x86_64
   [root@alanCentos01 ~]# 
   ```

   rpm -qi 软件包名 ：查询软件包信息

   ```shell
   [root@alanCentos01 ~]# rpm -qi firefox
   Name        : firefox
   Version     : 60.2.2
   Release     : 1.el7.centos
   Architecture: x86_64
   Install Date: 2021年04月02日 星期五 00时28分21秒
   Group       : Unspecified
   Size        : 216144933
   License     : MPLv1.1 or GPLv2+ or LGPLv2+
   Signature   : RSA/SHA256, 2018年10月09日 星期二 20时51分59秒, Key ID 24c6a8a7f4a80eb5
   Source RPM  : firefox-60.2.2-1.el7.centos.src.rpm
   Build Date  : 2018年10月09日 星期二 08时33分46秒
   Build Host  : x86-01.bsys.centos.org
   Relocations : (not relocatable)
   Packager    : CentOS BuildSystem <http://bugs.centos.org>
   Vendor      : CentOS
   URL         : https://www.mozilla.org/firefox/
   Summary     : Mozilla Firefox Web browser
   Description :
   Mozilla Firefox is an open-source web browser, designed for standards
   compliance, performance and portability.
   [root@alanCentos01 ~]# 
   ```

   rpm -ql 软件包名：查询软件包中的文件

   ```shell
   [root@alanCentos01 ~]# rpm -ql firefox
   /etc/firefox
   /etc/firefox/pref
   /usr/bin/firefox
   /usr/lib64/firefox
   /usr/lib64/firefox/LICENSE
   /usr/lib64/firefox/application.ini
   /usr/lib64/firefox/browser/blocklist.xml
   /usr/lib64/firefox/browser/chrome
   /usr/lib64/firefox/browser/chrome.manifest
   /usr/lib64/firefox/browser/chrome/icons
   /usr/lib64/firefox/browser/chrome/icons/default
   /usr/lib64/firefox/browser/chrome/icons/default/default128.png
   /usr/lib64/firefox/browser/chrome/icons/default/default16.png
   /usr/lib64/firefox/browser/chrome/icons/default/default32.png
   /usr/lib64/firefox/browser/chrome/icons/default/default48.png
   /usr/lib64/firefox/browser/chrome/icons/default/default64.png
   /usr/lib64/firefox/browser/defaults/preferences
   /usr/lib64/firefox/browser/extensions
   /usr/lib64/firefox/browser/extensions/{972ce4c6-7e08-4474-a285-3208198ce6fd}.xpi
   /usr/lib64/firefox/browser/features/activity-stream@mozilla.org.xpi
   /usr/lib64/firefox/browser/features/aushelper@mozilla.org.xpi
   /usr/lib64/firefox/browser/features/firefox@getpocket.com.xpi
   /usr/lib64/firefox/browser/features/followonsearch@mozilla.com.xpi
   /usr/lib64/firefox/browser/features/formautofill@mozilla.org.xpi
   /usr/lib64/firefox/browser/features/jaws-esr@mozilla.org.xpi
   /usr/lib64/firefox/browser/features/onboarding@mozilla.org.xpi
   /usr/lib64/firefox/browser/features/screenshots@mozilla.org.xpi
   /usr/lib64/firefox/browser/features/webcompat@mozilla.org.xpi
   /usr/lib64/firefox/browser/omni.ja
   /usr/lib64/firefox/chrome.manifest
   /usr/lib64/firefox/defaults/pref/channel-prefs.js
   /usr/lib64/firefox/defaults/preferences/all-redhat.js
   /usr/lib64/firefox/dependentlibs.list
   /usr/lib64/firefox/dictionaries
   /usr/lib64/firefox/distribution/distribution.ini
   /usr/lib64/firefox/distribution/extensions
   /usr/lib64/firefox/distribution/extensions/langpack-ach@firefox.mozilla.org.xpi
   /usr/lib64/firefox/distribution/extensions/langpack-af@firefox.mozilla.org.xpi
   /usr/lib64/firefox/distribution/extensions/langpack-an@firefox.mozilla.org.xpi
   /usr/lib64/firefox/distribution/extensions/langpack-ar@firefox.mozilla.org.xpi
   /usr/lib64/firefox/distribution/extensions/langpack-as@firefox.mozilla.org.xpi
   /usr/lib64/firefox/distribution/extensions/langpack-ast@firefox.mozilla.org.xpi
   /usr/lib64/firefox/distribution/extensions/langpack-az@firefox.mozilla.org.xpi
   /usr/lib64/firefox/distribution/extensions/langpack-be@firefox.mozilla.org.xpi
   /usr/lib64/firefox/distribution/extensions/langpack-bg@firefox.mozilla.org.xpi
   /usr/lib64/firefox/distribution/extensions/langpack-bn-BD@firefox.mozilla.org.xpi
   /usr/lib64/firefox/distribution/extensions/langpack-bn-IN@firefox.mozilla.org.xpi
   /usr/lib64/firefox/distribution/extensions/langpack-bn@firefox.mozilla.org.xpi
   /usr/lib64/firefox/distribution/extensions/langpack-br@firefox.mozilla.org.xpi
   /usr/lib64/firefox/distribution/extensions/langpack-bs@firefox.mozilla.org.xpi
   /usr/lib64/firefox/distribution/extensions/langpack-ca@firefox.mozilla.org.xpi
   /usr/lib64/firefox/distribution/extensions/langpack-cak@firefox.mozilla.org.xpi
   /usr/lib64/firefox/distribution/extensions/langpack-cs@firefox.mozilla.org.xpi
   /usr/lib64/firefox/distribution/extensions/langpack-cy@firefox.mozilla.org.xpi
   /usr/lib64/firefox/distribution/extensions/langpack-da@firefox.mozilla.org.xpi
   /usr/lib64/firefox/distribution/extensions/langpack-de@firefox.mozilla.org.xpi
   /usr/lib64/firefox/distribution/extensions/langpack-dsb@firefox.mozilla.org.xpi
   /usr/lib64/firefox/distribution/extensions/langpack-el@firefox.mozilla.org.xpi
   /usr/lib64/firefox/distribution/extensions/langpack-en-GB@firefox.mozilla.org.xpi
   /usr/lib64/firefox/distribution/extensions/langpack-en-ZA@firefox.mozilla.org.xpi
   /usr/lib64/firefox/distribution/extensions/langpack-eo@firefox.mozilla.org.xpi
   /usr/lib64/firefox/distribution/extensions/langpack-es-AR@firefox.mozilla.org.xpi
   /usr/lib64/firefox/distribution/extensions/langpack-es-CL@firefox.mozilla.org.xpi
   /usr/lib64/firefox/distribution/extensions/langpack-es-ES@firefox.mozilla.org.xpi
   /usr/lib64/firefox/distribution/extensions/langpack-es-MX@firefox.mozilla.org.xpi
   /usr/lib64/firefox/distribution/extensions/langpack-es@firefox.mozilla.org.xpi
   /usr/lib64/firefox/distribution/extensions/langpack-et@firefox.mozilla.org.xpi
   /usr/lib64/firefox/distribution/extensions/langpack-eu@firefox.mozilla.org.xpi
   /usr/lib64/firefox/distribution/extensions/langpack-fa@firefox.mozilla.org.xpi
   /usr/lib64/firefox/distribution/extensions/langpack-ff@firefox.mozilla.org.xpi
   /usr/lib64/firefox/distribution/extensions/langpack-fi@firefox.mozilla.org.xpi
   /usr/lib64/firefox/distribution/extensions/langpack-fr@firefox.mozilla.org.xpi
   /usr/lib64/firefox/distribution/extensions/langpack-fy-NL@firefox.mozilla.org.xpi
   /usr/lib64/firefox/distribution/extensions/langpack-fy@firefox.mozilla.org.xpi
   /usr/lib64/firefox/distribution/extensions/langpack-ga-IE@firefox.mozilla.org.xpi
   /usr/lib64/firefox/distribution/extensions/langpack-ga@firefox.mozilla.org.xpi
   /usr/lib64/firefox/distribution/extensions/langpack-gd@firefox.mozilla.org.xpi
   /usr/lib64/firefox/distribution/extensions/langpack-gl@firefox.mozilla.org.xpi
   /usr/lib64/firefox/distribution/extensions/langpack-gn@firefox.mozilla.org.xpi
   /usr/lib64/firefox/distribution/extensions/langpack-gu-IN@firefox.mozilla.org.xpi
   /usr/lib64/firefox/distribution/extensions/langpack-gu@firefox.mozilla.org.xpi
   /usr/lib64/firefox/distribution/extensions/langpack-he@firefox.mozilla.org.xpi
   /usr/lib64/firefox/distribution/extensions/langpack-hi-IN@firefox.mozilla.org.xpi
   /usr/lib64/firefox/distribution/extensions/langpack-hi@firefox.mozilla.org.xpi
   /usr/lib64/firefox/distribution/extensions/langpack-hr@firefox.mozilla.org.xpi
   /usr/lib64/firefox/distribution/extensions/langpack-hsb@firefox.mozilla.org.xpi
   /usr/lib64/firefox/distribution/extensions/langpack-hu@firefox.mozilla.org.xpi
   /usr/lib64/firefox/distribution/extensions/langpack-hy-AM@firefox.mozilla.org.xpi
   /usr/lib64/firefox/distribution/extensions/langpack-hy@firefox.mozilla.org.xpi
   /usr/lib64/firefox/distribution/extensions/langpack-ia@firefox.mozilla.org.xpi
   /usr/lib64/firefox/distribution/extensions/langpack-id@firefox.mozilla.org.xpi
   /usr/lib64/firefox/distribution/extensions/langpack-is@firefox.mozilla.org.xpi
   /usr/lib64/firefox/distribution/extensions/langpack-it@firefox.mozilla.org.xpi
   /usr/lib64/firefox/distribution/extensions/langpack-ja@firefox.mozilla.org.xpi
   /usr/lib64/firefox/distribution/extensions/langpack-ka@firefox.mozilla.org.xpi
   /usr/lib64/firefox/distribution/extensions/langpack-kab@firefox.mozilla.org.xpi
   /usr/lib64/firefox/distribution/extensions/langpack-kk@firefox.mozilla.org.xpi
   /usr/lib64/firefox/distribution/extensions/langpack-km@firefox.mozilla.org.xpi
   /usr/lib64/firefox/distribution/extensions/langpack-kn@firefox.mozilla.org.xpi
   /usr/lib64/firefox/distribution/extensions/langpack-ko@firefox.mozilla.org.xpi
   /usr/lib64/firefox/distribution/extensions/langpack-lij@firefox.mozilla.org.xpi
   /usr/lib64/firefox/distribution/extensions/langpack-lt@firefox.mozilla.org.xpi
   /usr/lib64/firefox/distribution/extensions/langpack-lv@firefox.mozilla.org.xpi
   /usr/lib64/firefox/distribution/extensions/langpack-mai@firefox.mozilla.org.xpi
   /usr/lib64/firefox/distribution/extensions/langpack-mk@firefox.mozilla.org.xpi
   /usr/lib64/firefox/distribution/extensions/langpack-ml@firefox.mozilla.org.xpi
   /usr/lib64/firefox/distribution/extensions/langpack-mr@firefox.mozilla.org.xpi
   /usr/lib64/firefox/distribution/extensions/langpack-ms@firefox.mozilla.org.xpi
   /usr/lib64/firefox/distribution/extensions/langpack-my@firefox.mozilla.org.xpi
   /usr/lib64/firefox/distribution/extensions/langpack-nb-NO@firefox.mozilla.org.xpi
   /usr/lib64/firefox/distribution/extensions/langpack-nb@firefox.mozilla.org.xpi
   /usr/lib64/firefox/distribution/extensions/langpack-ne-NP@firefox.mozilla.org.xpi
   /usr/lib64/firefox/distribution/extensions/langpack-nl@firefox.mozilla.org.xpi
   /usr/lib64/firefox/distribution/extensions/langpack-nn-NO@firefox.mozilla.org.xpi
   /usr/lib64/firefox/distribution/extensions/langpack-nn@firefox.mozilla.org.xpi
   /usr/lib64/firefox/distribution/extensions/langpack-oc@firefox.mozilla.org.xpi
   /usr/lib64/firefox/distribution/extensions/langpack-or@firefox.mozilla.org.xpi
   /usr/lib64/firefox/distribution/extensions/langpack-pa-IN@firefox.mozilla.org.xpi
   /usr/lib64/firefox/distribution/extensions/langpack-pa@firefox.mozilla.org.xpi
   /usr/lib64/firefox/distribution/extensions/langpack-pl@firefox.mozilla.org.xpi
   /usr/lib64/firefox/distribution/extensions/langpack-pt-BR@firefox.mozilla.org.xpi
   /usr/lib64/firefox/distribution/extensions/langpack-pt-PT@firefox.mozilla.org.xpi
   /usr/lib64/firefox/distribution/extensions/langpack-pt@firefox.mozilla.org.xpi
   /usr/lib64/firefox/distribution/extensions/langpack-rm@firefox.mozilla.org.xpi
   /usr/lib64/firefox/distribution/extensions/langpack-ro@firefox.mozilla.org.xpi
   /usr/lib64/firefox/distribution/extensions/langpack-ru@firefox.mozilla.org.xpi
   /usr/lib64/firefox/distribution/extensions/langpack-si@firefox.mozilla.org.xpi
   /usr/lib64/firefox/distribution/extensions/langpack-sk@firefox.mozilla.org.xpi
   /usr/lib64/firefox/distribution/extensions/langpack-sl@firefox.mozilla.org.xpi
   /usr/lib64/firefox/distribution/extensions/langpack-son@firefox.mozilla.org.xpi
   /usr/lib64/firefox/distribution/extensions/langpack-sq@firefox.mozilla.org.xpi
   /usr/lib64/firefox/distribution/extensions/langpack-sr@firefox.mozilla.org.xpi
   /usr/lib64/firefox/distribution/extensions/langpack-sv-SE@firefox.mozilla.org.xpi
   /usr/lib64/firefox/distribution/extensions/langpack-sv@firefox.mozilla.org.xpi
   /usr/lib64/firefox/distribution/extensions/langpack-ta@firefox.mozilla.org.xpi
   /usr/lib64/firefox/distribution/extensions/langpack-te@firefox.mozilla.org.xpi
   /usr/lib64/firefox/distribution/extensions/langpack-th@firefox.mozilla.org.xpi
   /usr/lib64/firefox/distribution/extensions/langpack-tr@firefox.mozilla.org.xpi
   /usr/lib64/firefox/distribution/extensions/langpack-uk@firefox.mozilla.org.xpi
   /usr/lib64/firefox/distribution/extensions/langpack-ur@firefox.mozilla.org.xpi
   /usr/lib64/firefox/distribution/extensions/langpack-uz@firefox.mozilla.org.xpi
   /usr/lib64/firefox/distribution/extensions/langpack-vi@firefox.mozilla.org.xpi
   /usr/lib64/firefox/distribution/extensions/langpack-xh@firefox.mozilla.org.xpi
   /usr/lib64/firefox/distribution/extensions/langpack-zh-CN@firefox.mozilla.org.xpi
   /usr/lib64/firefox/distribution/extensions/langpack-zh-TW@firefox.mozilla.org.xpi
   /usr/lib64/firefox/distribution/extensions/langpack-zh@firefox.mozilla.org.xpi
   /usr/lib64/firefox/firefox
   /usr/lib64/firefox/firefox-bin
   /usr/lib64/firefox/fonts/EmojiOneMozilla.ttf
   /usr/lib64/firefox/gmp-clearkey
   /usr/lib64/firefox/gmp-clearkey/0.1
   /usr/lib64/firefox/gmp-clearkey/0.1/libclearkey.so
   /usr/lib64/firefox/gmp-clearkey/0.1/manifest.json
   /usr/lib64/firefox/gtk2/libmozgtk.so
   /usr/lib64/firefox/liblgpllibs.so
   /usr/lib64/firefox/libmozavcodec.so
   /usr/lib64/firefox/libmozavutil.so
   /usr/lib64/firefox/libmozgtk.so
   /usr/lib64/firefox/libmozsandbox.so
   /usr/lib64/firefox/libmozsqlite3.so
   /usr/lib64/firefox/libxul.so
   /usr/lib64/firefox/omni.ja
   /usr/lib64/firefox/pingsender
   /usr/lib64/firefox/platform.ini
   /usr/lib64/firefox/plugin-container
   /usr/lib64/firefox/run-mozilla.sh
   /usr/lib64/mozilla/extensions/{ec8030f7-c20a-464f-9b0e-13a3a9e97384}
   /usr/share/appdata/firefox.appdata.xml
   /usr/share/applications/firefox.desktop
   /usr/share/icons/hicolor/16x16/apps/firefox.png
   /usr/share/icons/hicolor/22x22/apps/firefox.png
   /usr/share/icons/hicolor/24x24/apps/firefox.png
   /usr/share/icons/hicolor/256x256/apps/firefox.png
   /usr/share/icons/hicolor/32x32/apps/firefox.png
   /usr/share/icons/hicolor/48x48/apps/firefox.png
   /usr/share/icons/hicolor/symbolic/apps/firefox-symbolic.svg
   /usr/share/man/man1/firefox.1.gz
   /usr/share/mozilla/extensions/{ec8030f7-c20a-464f-9b0e-13a3a9e97384}
   [root@alanCentos01 ~]#
   ```

   rpm -qf 文件全路径名 查询文件所属的软件包

   ```shell
   [root@alanCentos01 ~]# rpm -qf /etc/passwd
   setup-2.8.71-10.el7.noarch
   ```

4. 卸载 rpm 包

   基本语法：

   rpm -e RPM 包的名称

   应用案例：

   删除firefox软件包

   rpm -e firefox

5. 细节讨论

   1)如果其它软件包依赖于您要卸载的软件包，卸载时则会产生错误信息。

   如：rpm -e foo

   removing these packages would break dependencies:foo is needed by bar-1.0-1

   2)如果我们就是要删除 foo 这个 rpm 包，可以增加参数 --nodeps ,就可以强制删除，但是一般不推荐这样做，因为依赖于该软件包的程序可能无法运行；

   如：rpm -e --nodeps foo

6. 安装rpm包

   基本语法：

   rpm -ivh RPM包全路径名称

   参数说明：

   i=install 安装

   v=verbose 提示

   h=hash 进度条

   应用实例：

   演示卸载和安装firefox浏览器

   rpm -e firefox

   rpm -ivh /opt/firefox-60.2.2-1.el7.centos.x86_64.rpm

7. yum介绍：
   yum是一个 Shell 前端软件包管理器。基于 RPM 包管理，能够从指定的服务器自动下载 RPM 包并且安装，==可以自动处理依赖性关系，并且一次安装所有依赖的软件包==。

8. yum 的基本指令：

   查询 yum 服务器是否有需要安装的软件

   yum list|grep xxx

9. 安装指定的 yum 包：

   yum install xxx

10. yum 应用实例：

    案例：请使用 yum 的方式来安装 firefox

    rpm -e firefox

    yum list | grep firefox

    yum install firefox

### 二十、搭建 JavaEE 环境

1. 安装 JDK

   * 安装步骤：

     1)mkdir /opt/jdk

     2)通过 xftp6 上传到 /opt/jdk 下

     3)cd /opt/jdk

     4)解压 tar-zxvfjdk-8u261-linux-x64.tar.gz

     5)mkdir /usr/local/java

     6)mv /opt/jdk/jdk1.8.0_261/usr/local/java

     7)配置环境变量的配置文件 vim /etc/profile

     8)export JAVA_HOME=/usr/local/java/jdk1.8.0_261

     9)export PATH=\$JAVA_HOME/bin:$PATH

     10) source /etc/profile[让新的环境变量生效]
     
   * 测试是否安装成功：
   
     ```shell
     [root@alanCentos01 ~]# java -version
     openjdk version "1.8.0_181"
     OpenJDK Runtime Environment (build 1.8.0_181-b13)
     OpenJDK 64-Bit Server VM (build 25.181-b13, mixed mode)
     ```
   
     编写一个简单的 Hello.java 输出"hello,world!"
     
     ```java
     public class Hello{
     
             public static void main(String[] args){
     
                     System.out.println("hello,java");
     
             }
     }
     ```
     
     ```shell
     [root@alanCentos01 home]# javac Hello.java
     [root@alanCentos01 home]# java Hello
     hello,java
     [root@alanCentos01 home]# 
     ```
   
2. tomcat的安装

   步骤 :

   1)上传安装文件，并解压缩到/opt/tomcat

   2)进入解压目录/bin , 启动 tomcat./startup.sh

   3)开放端口 8080 , 回顾 firewall-cmd

   16.3.2 测试是否安装成功：

   在 windows、Linux 下 访问http://linuxip:8080

   ![image-20210424230314204](assets/CentOS7.6详细说明/image-20210424230314204.png)

   ```shell
   [root@alanCentos01 /]# cd /opt/tomcat/apache-tomcat-8.5.59/webapps/ROOT/
   [root@alanCentos01 bin]# firewall-cmd --permanent --add-port=8080/tcp
   success
   [root@alanCentos01 bin]# firewall-cmd --reload
   success
   [root@alanCentos01 bin]# firewall-cmd --query-port=8080/tcp
   yes
   [root@alanCentos01 ROOT]# vim alan.html
   [root@alanCentos01 ROOT]# 
   ```

   ```html
   <html>
           <h1>Hello,Alan!</h1>
   </html>
   ```

3. idea2020 的安装

   步骤：

   1)下载地址: https://www.jetbrains.com/idea/download/#section=windows

   2)解压缩到/opt/idea

   3)启动 idea bin 目录下 ./idea.sh，配置 jdk

   4)编写 Hello world 程序并测试成功！

4. mysql5.7 的安装

   1)新建文件夹/opt/mysql，并cd进去

   2)运行wget http://dev.mysql.com/get/mysql-5.7.26-1.el7.x86_64.rpm-bundle.tar，下载mysql安装包

   ```shell
   [root@alanCentos01 mysql]# wget http://dev.mysql.com/get/mysql-5.7.26-1.el7.x86_64.rpm-bundle.tar
   --2021-04-25 23:11:50--  http://dev.mysql.com/get/mysql-5.7.26-1.el7.x86_64.rpm-bundle.tar
   正在解析主机 dev.mysql.com (dev.mysql.com)... 137.254.60.11
   正在连接 dev.mysql.com (dev.mysql.com)|137.254.60.11|:80... 已连接。
   已发出 HTTP 请求，正在等待回应... 301 Moved Permanently
   位置：https://dev.mysql.com/get/mysql-5.7.26-1.el7.x86_64.rpm-bundle.tar [跟随至新的 URL]
   --2021-04-25 23:11:50--  https://dev.mysql.com/get/mysql-5.7.26-1.el7.x86_64.rpm-bundle.tar
   正在连接 dev.mysql.com (dev.mysql.com)|137.254.60.11|:443... 已连接。
   已发出 HTTP 请求，正在等待回应... 302 Found
   位置：https://cdn.mysql.com//archives/mysql-5.7/mysql-5.7.26-1.el7.x86_64.rpm-bundle.tar [跟随至新的 URL]
   --2021-04-25 23:11:52--  https://cdn.mysql.com//archives/mysql-5.7/mysql-5.7.26-1.el7.x86_64.rpm-bundle.tar
   正在解析主机 cdn.mysql.com (cdn.mysql.com)... 96.16.173.94
   正在连接 cdn.mysql.com (cdn.mysql.com)|96.16.173.94|:443... 已连接。
   已发出 HTTP 请求，正在等待回应... 200 OK
   长度：530882560 (506M) [application/x-tar]
   正在保存至: “mysql-5.7.26-1.el7.x86_64.rpm-bundle.tar”
   
   100%[==============================================================================================================================>] 530,882,560 5.95MB/s 用时 87s    
   
   2021-04-25 23:13:20 (5.82 MB/s) - 已保存 “mysql-5.7.26-1.el7.x86_64.rpm-bundle.tar” [530882560/530882560])
   
   [root@alanCentos01 mysql]# 
   ```

   3)运行：tar -xvf mysql-5.7.26-1.el7.x86_64.rpm-bundle.tar

   ```shell
   [root@alanCentos01 mysql]# tar -xvf mysql-5.7.26-1.el7.x86_64.rpm-bundle.tar 
   mysql-community-embedded-devel-5.7.26-1.el7.x86_64.rpm
   mysql-community-libs-5.7.26-1.el7.x86_64.rpm
   mysql-community-embedded-5.7.26-1.el7.x86_64.rpm
   mysql-community-test-5.7.26-1.el7.x86_64.rpm
   mysql-community-embedded-compat-5.7.26-1.el7.x86_64.rpm
   mysql-community-common-5.7.26-1.el7.x86_64.rpm
   mysql-community-devel-5.7.26-1.el7.x86_64.rpm
   mysql-community-client-5.7.26-1.el7.x86_64.rpm
   mysql-community-server-5.7.26-1.el7.x86_64.rpm
   mysql-community-libs-compat-5.7.26-1.el7.x86_64.rpm
   ```

   4)运行rpm -qa|grep mari，查询mariadb相关安装包==（centos7.6自带的类mysql数据库是mariadb，会跟mysql冲突，要先删除）==

   ```shell
   [root@alanCentos01 mysql]# rpm -qa|grep mari
   mariadb-libs-5.5.60-1.el7_5.x86_64
   marisa-0.2.4-4.el7.x86_64
   ```

   5)运行rpm -e --nodeps mariadb-libs，卸载

   ```shell
   [root@alanCentos01 mysql]# rpm -e --nodeps mariadb-libs
   [root@alanCentos01 mysql]# rpm -e --nodeps marisa
   [root@alanCentos01 mysql]#
   ```

   6)然后开始真正安装mysql，依次运行以下几条：

   rpm -ivh mysql-community-common-5.7.26-1.el7.x86_64.rpm

   rpm -ivh mysql-community-libs-5.7.26-1.el7.x86_64.rpm

   rpm -ivh mysql-community-client-5.7.26-1.el7.x86_64.rpm

   rpm -ivh mysql-community-server-5.7.26-1.el7.x86_64.rpm

   ```shell
   [root@alanCentos01 mysql]# rpm -ivh mysql-community-common-5.7.26-1.el7.x86_64.rpm
   警告：mysql-community-common-5.7.26-1.el7.x86_64.rpm: 头V3 DSA/SHA1 Signature, 密钥 ID 5072e1f5: NOKEY
   准备中...                          ################################# [100%]
   正在升级/安装...
      1:mysql-community-common-5.7.26-1.e################################# [100%]
   [root@alanCentos01 mysql]# rpm -ivh mysql-community-libs-5.7.26-1.el7.x86_64.rpm
   警告：mysql-community-libs-5.7.26-1.el7.x86_64.rpm: 头V3 DSA/SHA1 Signature, 密钥 ID 5072e1f5: NOKEY
   准备中...                          ################################# [100%]
   正在升级/安装...
      1:mysql-community-libs-5.7.26-1.el7################################# [100%]
   [root@alanCentos01 mysql]# rpm -ivh mysql-community-client-5.7.26-1.el7.x86_64.rpm
   警告：mysql-community-client-5.7.26-1.el7.x86_64.rpm: 头V3 DSA/SHA1 Signature, 密钥 ID 5072e1f5: NOKEY
   准备中...                          ################################# [100%]
   正在升级/安装...
      1:mysql-community-client-5.7.26-1.e################################# [100%]
   [root@alanCentos01 mysql]# rpm -ivh mysql-community-server-5.7.26-1.el7.x86_64.rpm
   警告：mysql-community-server-5.7.26-1.el7.x86_64.rpm: 头V3 DSA/SHA1 Signature, 密钥 ID 5072e1f5: NOKEY
   准备中...                          ################################# [100%]
   正在升级/安装...
      1:mysql-community-server-5.7.26-1.e################################# [100%]
   [root@alanCentos01 mysql]# 
   ```

   7)运行systemctl start mysqld.service，启动mysql

   ```shell
   [root@alanCentos01 mysql]# systemctl start mysqld.service
   [root@alanCentos01 mysql]#
   ```

   8)然后开始设置root用户密码，Mysql自动给root用户设置随机密码，运行grep "password" /var/log/mysqld.log可看到当前密码

   ```shell
   [root@alanCentos01 mysql]# cat /var/log/mysqld.log | grep password
   2021-04-25T15:27:29.972582Z 1 [Note] A temporary password is generated for root@localhost: sud5=xsCghld
   [root@alanCentos01 mysql]# grep "password" /var/log/mysqld.log
   2021-04-25T15:27:29.972582Z 1 [Note] A temporary password is generated for root@localhost: sud5=xsCghld
   [root@alanCentos01 mysql]# 
   ```

   9)运行mysql -u root -p，用root用户登录，提示输入密码可用上述的，可以成功登陆进入mysql命令行

   ```shell
   [root@alanCentos01 mysql]# mysql -uroot -p
   Enter password: 
   Welcome to the MySQL monitor.  Commands end with ; or \g.
   Your MySQL connection id is 2
   Server version: 5.7.26
   
   Copyright (c) 2000, 2019, Oracle and/or its affiliates. All rights reserved.
   
   Oracle is a registered trademark of Oracle Corporation and/or its
   affiliates. Other names may be trademarks of their respective
   owners.
   
   Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
   
   mysql>
   ```

   10)设置root密码，对于个人开发环境，如果要设比较简单的密码（**生产环境服务器要设复杂密码**），可以运行：

   set global validate_password_policy=0; 提示密码设置策略

   （validate_password_policy默认值1，）

   ![image-20210425234159833](assets/CentOS7.6详细说明/image-20210425234159833.png)

   ```mysql
   mysql> set global validate_password_policy=0;
   Query OK, 0 rows affected (0.00 sec)
   ```

   11)set password for 'root'@'localhost' =password('123456');

   ```mysql
   mysql> set password for 'root'@'localhost' =password('123456');
   ERROR 1819 (HY000): Your password does not satisfy the current policy requirements
   mysql> set password for 'root'@'localhost' =password('alan123456');
   Query OK, 0 rows affected, 1 warning (0.00 sec)
   ```

   12)运行flush privileges;使密码设置生效

   ```mysql
   mysql> flush privileges;
   Query OK, 0 rows affected (0.01 sec)
   mysql> show databases;
   +--------------------+
   | Database           |
   +--------------------+
   | information_schema |
   | mysql              |
   | performance_schema |
   | sys                |
   +--------------------+
   4 rows in set (0.00 sec)
   
   mysql> quit
   Bye
   [root@alanCentos01 mysql]#
   ```


### 二十一、Shell编程

1. 脚本格式要求

   1)脚本以#!/bin/bash 开头

   2)脚本需要有可执行权限

2. 编写第一个 Shell 脚本

   需求说明：创建一个 Shell 脚本，输出 hello world!

   vim hello.sh

   #!/bin/bash

   echo "hello,world~"

3. 脚本的常用执行方式

   * 方式1：(输入脚本的绝对路径或相对路径)

     说明：首先要赋予 helloworld.sh 脚本的+x 权限， 再执行脚本；

     比如： ./hello.sh 或者使用绝对路径/root/shcode/hello.sh

   * 方式2：(sh+脚本)
     说明：不用赋予脚本+x 权限，直接执行即可。
     比如：sh hello.sh，也可以使用绝对路径

4. Shell 的变量

   * Shell变量介绍

     1)Linux Shell 中的变量分为，系统变量和用户自定义变量。

     2)系统变量：\$HOME、\$PWD、​\$SHELL、​\$USER 等等，比如： echo $HOME等等..

     3)==显示当前 shell 中所有变量：set==

     ```bash
     [root@alanCentos01 ~]# set | grep JAVA_HOME
     JAVA_HOME=/usr/local/java/jdk1.8.0_261
     ```

   * shell 变量的定义

     基本语法

     1)定义变量：变量名=值

     2)撤销变量：unset 变量

     3)声明静态变量：readonly 变量，==注意：不能被unset==

     快速入门

     1)案例 1：定义变量 A

     2)案例 2：撤销变量 A

     3)案例 3：声明静态的变量 B=2，不能unset

     实现如下（多行注释方式为：==:<<!  内容  !==）

     ```shell
     #!/bin/bash
     #案例 1：定义变量 A
     A=100
     #输出变量需要加上$
     echo A=$A
     echo "A=$A"
     #案例 2：撤销变量 A
     unset A
     echo "A=$A"
     #案例 3：声明静态的变量 B=2，不能 unset
     readonly B=2
     echo "B=$B"
     #unset B
     #将指令返回的结果赋给变量
     :<<!
     C=`date`
     D=$(date)
     echo "C=$C"
     echo "D=$D"
     !
     #使用环境变量 TOMCAT_HOME
     echo "tomcat_home=$TOMCAT_HOME"
     ```

     定义变量的规则

     1)变量名称可以由字母、数字和下划线组成，但是不能以数字开头。5A=200(×)

     2)等号两侧不能有空格

     3)变量名称一般习惯为大写， 这是一个规范，我们遵守即可

     将命令的返回值赋给变量

     1)反引号，运行里面的命令，并把结果返回给变量 A

     ```shell
     A=`date` #反引号，运行里面的命令，并把结果返回给变量 A
     ```
     
     2)A=$(date) 等价于反引号

5. 设置环境变量

   * 基本语法

     1)export 变量名=变量值 （功能描述：将 shell 变量输出为环境变量/全局变量）

     2)source 配置文件（功能描述：让修改后的配置信息立即生效）

     3)echo $变量名（功能描述：查询环境变量的值）

   * 快速入门

     1)在/etc/profile 文件中定义 TOMCAT_HOME 环境变量

     2)查看环境变量 TOMCAT_HOME 的值

     3)在另外一个 shell 程序中使用 TOMCAT_HOME

     注意：在输出 TOMCAT_HOME 环境变量前，需要让其生效，执行命令如下

     source /etc/profile

     ![image-20210426211328173](assets/CentOS7.6详细说明/image-20210426211328173.png)

     ==shell 脚本的多行注释，格式如下：==

     :<<!  内容  !

6. 位置参数变量

   * 介绍

     当我们执行一个 shell 脚本时，如果希望获取到命令行的参数信息，就可以使用到位置参数变量，

     比如 ： ./myshell.sh 100 200 , 这个就是一个执行 shell 的命令行，可以在 myshell脚本中获取到参数信息。

   * 基本语法
     \$n （功能描述：n 为数字，\$0 代表命令本身，\$1-​\$9 代表第一到第九个参数，十以上的参数，十以上的参数需要用大括号包含，如​\${10}）

     \$* （功能描述：这个变量代表命令行中所有的参数，​\$*把所有的参数看成一个整体）

     \$@（功能描述：这个变量也代表命令行中所有的参数，不过​\$@把每个参数区分对待）

     \$#（功能描述：这个变量代表命令行中所有参数的个数）

   * 位置参数变量

     案例：编写一个 shell 脚本 position.sh ， 在脚本中获取到命令行的各个参数信息。

     ```shell
     #!/bin/bash
     echo "0=$0 1=$1 2=$2"
     echo "所有的参数=$*"
     echo "$@"
     echo "参数的个数=$#"
     ```
   
7. 预定义变量

   * 基本介绍

     就是 shell 设计者事先已经定义好的变量，可以直接在 shell 脚本中使用。

   * 基本语法

     1)$$ （功能描述：当前进程的进程号（PID））

     2)\$! （功能描述：后台运行的最后一个进程的进程号（PID））

     3)​\$？（功能描述：最后一次执行的命令的返回状态。如果这个变量的值为 0，证明上一个命令正确执行；如果这个变量的值为非 0（具体是哪个数，由命令自己来决定），则证明上一个命令执行不正确了。）

   * 应用实例

     在一个 shell 脚本中简单使用一下预定义变量

     ```shell
     #!/bin/bash
     echo "当前执行的进程 id=$$"
     #以后台的方式运行一个脚本，并获取他的进程号
     /root/shcode/myshell.sh &
     echo "最后一个后台方式运行的进程 id=$!"
     echo "执行的结果是=$?"
     ```

8. 运算符

   * 基本语法

     1)“\$((运算式))”或“​\$[运算式]”或者 expr m + n //expression 表达式

     2)==注意 expr 运算符间要有空格, 如果希望将 expr 的结果赋给某个变量，使用 ``（注意：这个不是单引号，是反引号）==

     3)expr m - n

     4)expr \\*, /, %：乘，除，取余

   * 应用实例 oper.sh

     案例 1：计算(2+3)*4 的值
     案例 2：请求出命令行的两个参数[整数]的和：20、50

     ```shell
     #!/bin/bash
     #案例 1：计算（2+3）X4 的值
     #使用第一种方式
     RES1=$(((2+3)*4))
     echo "res1=$RES1"
     #使用第二种方式, 推荐使用
     RES2=$[(2+3)*4]
     echo "res2=$RES2"
     #使用第三种方式 expr
     TEMP=`expr 2 + 3`
     RES4=`expr $TEMP \* 4`
     echo "temp=$TEMP"
     echo "res4=$RES4"
     #案例 2：请求出命令行的两个参数[整数]的和 20 50
     SUM=$[$1+$2]
     echo "sum=$SUM"
     ```

9. 条件判断

   判断语句：

   * 基本语法

     [ condition ]==（注意 condition 前后要有空格）==

     #非空返回 true，可使用$?验证（0 为 true，>1 为 false）

   * 应用实例

     [ hspEdu ]： 返回 true

     [ ]：返回 false==（注意：”[ ]“里面是有一个空格的，否则执行会报错）==

     [ condition ] && echo OK || echo notok：条件满足，执行后面的语句

   * 常用判断条件

     1) 字符串比较：=

     2) 两个整数的比较：

     - -lt 小于

     * -le 小于等于 little equal

     * -eq 等于

     * -gt 大于

     * -ge 大于等于

     * -ne 不等于

     3) 按照文件权限进行判断：

     -r 有读的权限

     -w 有写的权限

     -x 有执行的权限

     4) 按照文件类型进行判断：

     -f 文件存在并且是一个常规的文件

     -e 文件存在

     -d 文件存在并是一个目录

   * 应用实例 ifdemo.sh

     案例 1："ok"是否等于"ok"

     判断语句：使用 =

     案例 2：23 是否大于等于 22

     判断语句：使用 -ge

     案例 3：/root/shcode/aaa.txt 目录中的文件是否存在

     判断语句： 使用 -f

     代码如下:
     
     ```bash
     [root@alanCentos01 home]# vim ifdemo.sh
     ```
     
     ```shell
     #!/bin/bash
     #案例 1："ok"是否等于"ok"
     if [ "ok" = "ok" ]
     then
             echo "equal"
     fi
     #案例 2：23 是否大于等于 22
     if [ 23 -ge 22 ]
     then
             echo "大于等于"
     fi
     #案例 3：/root/shcode/aaa.txt 目录中的文件是否存在
     #判断语句： 使用 -f
     if [ -f /root/shcode/aaa.txt ]
     then
             echo "存在"
     fi
     #看几个案例
     if [ alan ]
     then
             echo "hello,alan"
     fi
     ```

10. 流程控制

    * if 判断

      * 基本语法：

        if [ 条件判断式 ]

        then

        ​	代码

        fi

        ==或者 , 多分支==

        if [ 条件判断式 ]

        then

        ​	代码

        elif [条件判断式]

        then

        ​	代码

        fi

      * 注意事项：[ 条件判断式 ]，==中括号和条件判断式之间必须有空格==

      * 应用实例 ifCase.sh

        案例：请编写一个 shell 程序，如果输入的参数，大于等于 60，则输出 "及格了"，如果小于 60,则输出 "不及格"

        ```bash
        [root@alanCentos01 home]# vim ifCase.sh
        ```

        ```shell
        #!/bin/bash
        #案例：请编写一个 shell 程序，如果输入的参数，大于等于 60，则输出 "及格了">，如果小于 60,则输出 "不及格"
        if [ $1 -ge 60 ]
        then
                echo "及格了"
        elif [ $1 -lt 60 ]
        then
                echo "不及格"
        fi
        ```

    * case 语句

      case $变量名 in

      "值 1"）

      ​	如果变量的值等于值 1，则执行程序 1

      ;;

      "值 2"）

      ​	如果变量的值等于值 2，则执行程序 2

      ;;

      ==…省略其他分支…==

      *）

      ​	如果变量的值都不是以上的值，则执行此程序

      ;;

      esac

      应用实例 testCase.sh

      ```bash
      [root@alanCentos01 home]# vim testCase.sh
      ```

      ```shell
      #!/bin/bash
      #案例 1 ：当命令行参数是 1 时，输出 "周一", 是 2 时，就输出"周二"， 其它情>况输出"other"
      case $1 in
      "1")
              echo "周一"
      ;;
      "2")
              echo "周二"
      ;;
      *)
              echo "other..."
      ;;
      esac
      ```

    * for 循环

      基本语法 1

      for 变量 in 值 1 值 2 值 3…

      do

      程序/代码

      done

      应用实例 testFor1.sh

      案例 1 ：打印命令行输入的参数 [这里可以看出$* 和 $@ 的区别]

      ```bash
      [root@alanCentos01 home]# vim testFor1.sh
      ```

      ```shell
      #!/bin/bash
      #案例 1 ：打印命令行输入的参数 [这里可以看出$* 和 $@ 的区别]
      #注意：$*是把输入的参数，当做一个整体，所以，只会输出一句
      for i in "$*"
      do
              echo "num is $i"
      done
      #使用$@来获取输入的参数，注意，这时是分别对待，所以有几个参数，就输出几个
      echo "========================================"
      for j in "$@"
      do
              echo "num is $j"
      done
      ```

      基本语法 2

      for (( 初始值;循环控制条件;变量变化 ))

      do

      ​	程序/代码

      done

      应用实例 testFor2.sh

      案例 1 ：从 1 加到 100 的值输出显示

      ```bash
      [root@alanCentos01 home]# vim testFor2.sh
      ```

      ```shell
      #!/bin/bash
      #案例 1 ：从 1 加到 100 的值输出显示
      #定义一个变量SUM
      SUM=0
      for(( i=1; i<=$1; i++ ))
      do
              #写上你的业务代码
              SUM=$[$SUM+$i]
      done
      echo "总和SUM=$SUM"0
      ```

    * while 循环

      基本语法 1

      while [ 条件判断式 ]

      do

      ​	程序 /代码

      done

      注意：while 和 [有空格，条件判断式和 [也有空格

      应用实例 testWhile.sh

      案例 1 ：从命令行输入一个数 n，统计从 1+..+ n 的值是多少？

      ```bash
      [root@alanCentos01 home]# vim testWhile.sh
      ```

      ```shell
      #!/bin/bash
      #案例 1 ：从命令行输入一个数 n，统计从 1+..+ n 的值是多少？
      SUM=0
      i=0
      while [ $i -le $1 ]
      do
              SUM=$[$SUM+$i]
              #i自增
              i=$[$i+1]
      done
      echo "执行结果=$SUM"
      ```

11. read 读取控制台输入

    * 基本语法

      read(选项)(参数)

      选项：

      -p：指定读取值时的提示符；

      -t：指定读取值时等待的时间（秒），如果没有在指定的时间内输入，就不再等待了。。

      参数

      变量：指定读取值的变量名

    * 应用实例 testRead.sh

      案例 1：读取控制台输入一个 NUM1 值

      案例 2：读取控制台输入一个 NUM2 值，在 10 秒内输入。

      ```bash
      [root@alanCentos01 home]# vim testRead.sh
      ```

      ```shell
      #!/bin/bash
      #案例 1：读取控制台输入一个 NUM1 值
      read -p "请输入一个数NUM1=" NUM1
      echo "你输入的NUM1=$NUM1"
      #案例 2：读取控制台输入一个 NUM2 值，在 10 秒内输入。
      read -t 10 -p "请输入一个数NUM2=" NUM2
      echo "你输入的NUM2=$NUM2"
      ```

12. 函数

    * 函数介绍
      shell 编程和其它编程语言一样，有系统函数，也可以自定义函数。系统函数中，我们这里就介绍两个。

    * 系统函数

      basename 基本语法

      功能：返回完整路径最后 / 的部分，常用于获取文件名

      basename [pathname] [suffix]

      basename [string] [suffix]（功能描述：basename 命令会删掉所有的前缀包括最后一个（‘/’）字符，然后将字符串显示出来。

      选项：

      suffix 为后缀，如果 suffix 被指定了，basename 会将 pathname 或 string 中的 suffix 去掉。

      应用实例：

      案例 1：请返回 /home/aaa/test.txt 的 "test.txt" 部分

      ```bash
      [root@alanCentos01 home]# basename /home/aaa/test.txt
      test.txt
      ```

      dirname 基本语法

      功能：返回完整路径最后 / 的前面的部分，常用于返回路径部分

      dirname 文件绝对路径 （功能描述：从给定的包含绝对路径的文件名中去除文件名（非目录的部分），然后返回剩下的路径（目录的部分））

      应用实例：

      案例 1：请返回 /home/aaa/test.txt 的 /home/aaa

      ```bash
      [root@alanCentos01 home]# dirname /home/aaa/test.txt
      /home/aaa
      ```

    * 自定义函数

      基本语法

      [ function ] funname[()]

      {

      ​	Action;

      ​	[return int;]

      }

      调用直接写函数名：funname [值]

      应用实例

      案例 1：计算输入两个参数的和(动态的获取)， getSum

      ```bash
      [root@alanCentos01 home]# vim getSum.sh
      ```

      ```shell
      #!/bin/bash
      #案例 1：计算输入两个参数的和(动态的获取)， getSum
      #定义函数 getSum
      function getSum(){
      
              SUM=$[$n1+$n2]
              echo "和市=$SUM"
      }
      
      #输入两个值
      read -p "请输入一个数 n1=" n1
      read -p "请输入一个数 n2=" n2
      #调用自定义函数
      getSum $n1 $n2
      ```

    * Shell 编程综合案例

      * 需求分析

        1)每天凌晨 2:30 备份 数据库 hspedu 到 /data/backup/db

        2)备份开始和备份结束能够给出相应的提示信息

        3)备份后的文件要求以备份时间为文件名，并打包成 .tar.gz 的形式，比如：2021-03-12_230201.tar.gz

        4)在备份的同时，检查是否有 10 天前备份的数据库文件，如果有就将其删除。

      * 代码 /usr/sbin/mysql_db.backup.sh

        ```bash
        [root@alanCentos01 home]# vim /usr/sbin/mysql_db.backup.sh
        ```

        ```shell
        #!/bin/bash
        #备份目录
        BACKUP=/data/backup/db
        #当前时间
        DATETIME=$(date +%Y-%m-%d_%H%M%S)
        echo $DATETIME
        #数据库的地址
        HOST=localhost
        #数据库用户名
        DB_USER=root
        #数据库密码
        DB_PW=XXXXXX
        #备份的数据库名
        DATABASE=my_secretary
        
        #创建备份目录，如果不存在，就创建
        [ ! -d "${BACKUP}/${DATETIME}" ] && mkdir -p "${BACKUP}/${DATETIME}"
        #备份数据库
        mysqldump -u${DB_USER} -p${DB_PW} --host=${HOST} -q -R --databases -B ${DATABASE} | gzip > ${BACKUP}/${DATETIME}/$DATETIME.sql.gz
        
        #将文件处理成tar.gz
        cd ${BACKUP}
        tar -zcvf $DATETIME.tar.gz ${DATETIME}
        #删除对应的备份目录
        rm -rf ${BACKUP}/${DATETIME}
        
        #删除10天前的备份文件
        find ${BACKUP} -atime +10 -name "*.tar.gz" -exec rm -rf {} \;
        echo "备份数据${DATABASE}成功~"
        ```


### 二十二、日志管理

1. 系统常用的日志

   /var/log/ 目录就是系统日志文件的保存位置，看张图

   ![image-20210504104716108](assets/CentOS7.6详细说明/image-20210504104716108.png)

   ![image-20210504104806084](assets/CentOS7.6详细说明/image-20210504104806084.png)

   应用案例：

   查看lastlog日志文件

   ```bash
   [root@alanCentos01 log]# ll lastlog
   -rw-r--r--. 1 root root 292876 5月   4 10:52 lastlog
   [root@alanCentos01 log]# lastlog
   用户名           端口     来自             最后登陆时间
   root             pts/0    192.168.18.1     二 5月  4 10:52:11 +0800 2021
   bin                                        **从未登录过**
   daemon                                     **从未登录过**
   adm                                        **从未登录过**
   lp                                         **从未登录过**
   sync                                       **从未登录过**
   shutdown                                   **从未登录过**
   halt                                       **从未登录过**
   mail                                       **从未登录过**
   operator                                   **从未登录过**
   games                                      **从未登录过**
   ftp                                        **从未登录过**
   nobody                                     **从未登录过**
   systemd-network                            **从未登录过**
   dbus                                       **从未登录过**
   polkitd                                    **从未登录过**
   libstoragemgmt                             **从未登录过**
   colord                                     **从未登录过**
   rpc                                        **从未登录过**
   gluster                                    **从未登录过**
   saslauth                                   **从未登录过**
   abrt                                       **从未登录过**
   rtkit                                      **从未登录过**
   pulse                                      **从未登录过**
   radvd                                      **从未登录过**
   unbound                                    **从未登录过**
   chrony                                     **从未登录过**
   rpcuser                                    **从未登录过**
   nfsnobody                                  **从未登录过**
   qemu                                       **从未登录过**
   tss                                        **从未登录过**
   usbmuxd                                    **从未登录过**
   geoclue                                    **从未登录过**
   ntp                                        **从未登录过**
   sssd                                       **从未登录过**
   setroubleshoot                             **从未登录过**
   saned                                      **从未登录过**
   gdm              :0                        二 5月  4 10:51:31 +0800 2021
   gnome-initial-setup                           **从未登录过**
   sshd                                       **从未登录过**
   avahi                                      **从未登录过**
   postfix                                    **从未登录过**
   tcpdump                                    **从未登录过**
   alan             pts/0                     二 4月  6 22:27:11 +0800 2021
   fox                                        **从未登录过**
   zwj                                        **从未登录过**
   mysql                                      **从未登录过**
   [root@alanCentos01 log]# who
   root     pts/0        2021-05-04 10:52 (192.168.18.1)
   [root@alanCentos01 log]# 
   ```

   使用 root 用户通过 xshell6 登陆, 第一次使用错误的密码，第二次使用正确的密码登录成功看看在日志文件/var/log/secure 里有没有记录相关信息。

   ```bash
   [root@alanCentos01 log]# cat secure
   May  4 00:42:07 alanCentos01 sshd[7983]: error: Received disconnect from 192.168.18.1 port 3448:0:
   May  4 00:42:07 alanCentos01 sshd[7983]: Disconnected from 192.168.18.1 port 3448
   May  4 00:42:07 alanCentos01 sshd[7983]: pam_unix(sshd:session): session closed for user root
   May  4 10:51:21 alanCentos01 polkitd[6861]: Loading rules from directory /etc/polkit-1/rules.d
   May  4 10:51:21 alanCentos01 polkitd[6861]: Loading rules from directory /usr/share/polkit-1/rules.d
   May  4 10:51:22 alanCentos01 polkitd[6861]: Finished loading, compiling and executing 10 rules
   May  4 10:51:22 alanCentos01 polkitd[6861]: Acquired the name org.freedesktop.PolicyKit1 on the system bus
   May  4 10:51:28 alanCentos01 sshd[7966]: Server listening on 0.0.0.0 port 22.
   May  4 10:51:28 alanCentos01 sshd[7966]: Server listening on :: port 22.
   May  4 10:51:31 alanCentos01 gdm-launch-environment]: pam_unix(gdm-launch-environment:session): session opened for user gdm by (uid=0)
   May  4 10:51:34 alanCentos01 polkitd[6861]: Registered Authentication Agent for unix-session:c1 (system bus name :1.66 [/usr/bin/gnome-shell], object path /org/freedesktop/PolicyKit1/AuthenticationAgent, locale zh_CN.UTF-8)
   May  4 10:52:11 alanCentos01 sshd[8921]: Accepted password for root from 192.168.18.1 port 3020 ssh2
   May  4 10:52:11 alanCentos01 sshd[8921]: pam_unix(sshd:session): session opened for user root by (uid=0)
   [root@alanCentos01 log]# 
   ```

2. 日志管理服务rsyslogd

   * 说明：CentOS7.6 日志服务是 rsyslogd ， CentOS6.x 日志服务是 syslogd 。rsyslogd 功能更强大。rsyslogd 的使用、日志文件的格式，和 syslogd 服务兼容的。

     查询 Linux 中的 rsyslogd 服务是否启动==（grep -v：意思是不显示匹配到的行）==

     ```bash
     [root@alanCentos01 log]# ps aux | grep "rsyslog" | grep -v "grep"
     root       7970  0.0  0.2 222748  4820 ?        Ssl  10:51   0:00 /usr/sbin/rsyslogd -n
     [root@alanCentos01 log]# 
     ```

     查询 rsyslogd 服务的自启动状态

     ```bash
     [root@alanCentos01 log]# systemctl list-unit-files | grep rsyslog
     rsyslog.service                               enabled 
     [root@alanCentos01 log]#
     ```

   * 配置文件：/etc/rsyslog.conf

     编辑文件时的格式为：*.*存放日志文件

     其中第一个\*代表日志类型，第二个\*代表日志级别

     1)日志类型分为：

     auth	##pam 产生的日志

     authpriv	##ssh、ftp 等登录信息的验证信息

     corn	##时间任务相关

     kern	##内核

     lpr	##打印

     mail	##邮件

     mark(syslog)-rsyslog	##服务内部的信息，时间标识

     news	##新闻组

     user	##用户程序产生的相关信息

     uucp	##unixtonuixcopy 主机之间相关的通信

     local1-7	##自定义的日志设备

     2)日志级别分为：

     debug	##有调试信息的，日志通信最多

     info	##一般信息日志，最常用

     notice	##最具有重要性的普通条件的信息

     warning	##警告级别

     err	##错误级别，阻止某个功能或者模块不能正常工作的信息

     crit	##严重级别，阻止整个系统或者整个软件不能正常工作的信息

     alert	##需要立刻修改的信息

     emerg	##内核崩溃等重要信息

     none	##什么都不记录

     ==注意：从上到下，级别从低到高，记录信息越来越少==

   * 由日志服务 rsyslogd 记录的日志文件，日志文件的格式包含以下 4 列：

     1)事件产生的时间

     2)产生事件的服务器的主机名

     3)产生事件的服务名或程序名

     4)事件的具体信息

     日志如何查看实例

     查看一下 /var/log/secure 日志，这个日志中记录的是用户验证和授权方面的信息 来分析如何查看

     ![image-20210504115229775](assets/CentOS7.6详细说明/image-20210504115229775.png)

     日志管理服务应用实例

     在/etc/rsyslog.conf 中添加一个日志文件/var/log/hsp.log,当有事件发送时(比如 sshd 服务相关事件)，该文件会接收到信息并保存。

     ![image-20210504115848707](assets/CentOS7.6详细说明/image-20210504115848707.png)

3. 日志轮替

   * logrotate 配置文件

     /etc/logrotate.conf 为 logrotate 的全局配置文件

     \# rotate log files weekly, 每周对日志文件进行一次轮替
   
     weekly
   
     \# keep 4 weeks worth of backlogs, 共保存 4 份日志文件，当建立新的日志文件时，旧的将会被删除
   
     rotate 4
   
     \# create new (empty) log files after rotating old ones, 创建新的空的日志文件，在日志轮替后
   
     create
   
     \# use date as a suffix of the rotated file, 使用日期作为日志轮替文件的后缀
   
     dateext
   
     \# uncomment this if you want your log files compressed, 日志文件是否压缩。如果取消注释，则日志会在转储的同时进行压缩
   
     \#compress
   
     \#RPM packages drop log rotation information into this directory
   
     include /etc/logrotate.d
   
     \# 包含 /etc/logrotate.d/ 目录中所有的子配置文件。也就 是说会把这个目录中所有子配置文件读取进来
   
     \#下面是单独设置，优先级更高。
   
     \# no packages own wtmp and btmp -- we'll rotate them here
   
     /var/log/wtmp {
   
     ​	monthly # 每月对日志文件进行一次轮替
   
     ​	create 0664 root utmp # 建立的新日志文件，权限是 0664 ，所有者是 root ，所属组是 utmp 组
   
     ​	minsize 1M# 日志文件最小轮替大小是 1MB 。也就是日志一定要超过 1MB 才会轮替，否则就算时间达到一个月，也不进行日志转储
   
     ​	rotate 1 # 仅保留一个日志备份。也就是只有 wtmp 和 wtmp.1 日志保留而已
     }
     /var/log/btmp {
   
     ​	missingok # 如果日志不存在，则忽略该日志的警告信息
   
     ​	monthly
   
     ​	create 0600 root utmp
   
     ​	rotate 1
     }
   
     参数说明：
     daily	日志的轮替周期是每天
   
     weekly	日志的轮替周期是每周
   
     monthly	日志的轮替周期是每月
   
     rotate	数字保留的日志文件的个数。0 指没有备份
   
     compress	日志轮替时，旧的日志进行压缩
   
     create mode owner group	建立新日志，同时指定新日志的权限与所有者和所属组。
   
     mail address	当日志轮替时，输出内容通过邮件发送到指定的邮件地址。
   
     missingok	如果日志不存在，则忽略该日志的警告信息
   
     notifempty	如果日志为空文件，则不进行日志轮替
   
     minsize	大小日志轮替的最小值。也就是日志一定要达到这个最小值才会轮替，否则就算时间达到也不轮替
   
     size	大小日志只有大于指定大小才进行日志轮替，而不是按照时间轮替。
   
     dateext	使用日期作为日志轮替文件的后缀。
   
     sharedscripts	在此关键字之后的脚本只执行一次。
   
     prerotate/endscript	在日志轮替之前执行脚本命令。
   
     postrotate/endscript	在日志轮替之后执行脚本命令。
   
   * 把自己的日志加入日志轮替
   
     1)第一种方法是直接在/etc/logrotate.conf 配置文件中写入该日志的轮替策略
   
     2)第二种方法是在/etc/logrotate.d/目录中新建立该日志的轮替文件，在该轮替文件中写入正确的轮替策略，因为该目录中的文件都会被“include”到主配置文件中，所以也可以把日志加入轮替。
   
     3)推荐使用第二种方法推荐使用第二种方法，因为系统中需要轮替的日志非常多，如果全都直接写 入/etc/logrotate.conf 配置文件，那么这个文件的可管理性就会非常差，不利于此文件的维护。
   
     4)在/etc/logrotate.d/ 配置轮替文件一览
   
   ![image-20210504170724353](assets/CentOS7.6详细说明/image-20210504170724353.png)
   
   * 应用实例
   
     看一个案例, 在/etc/logrotate.conf 进行配置, 或者直接在 /etc/logrotate.d/ 下创建文件 hsplog 编写如下内容,具体轮替的效果 可以参考 /var/log 下的 boot.log 情况。
   
     ![image-20210504171000502](assets/CentOS7.6详细说明/image-20210504171000502-1620119425194.png)
   
     ![image-20210504170817788](assets/CentOS7.6详细说明/image-20210504170817788.png)
   
4. 日志轮替机制原理

   日志轮替之所以可以在指定的时间备份日志，是依赖系统定时任务。在 /etc/cron.daily/目录，就会发现这个目录中是有 logrotate 文件(可执行)，logrotate 通过这个文件依赖定时任务执行的。

5. 查看内存日志

   journalctl	可以查看内存日志, 这里我们看看常用的指令

   journalctl	##查看全部

   journalctl-n 3	##查看最新 3 条

   journalctl--since 19:00--until 19:10:10	#查看起始时间到结束时间的日志可加日期

   journalctl-p err	##报错日志

   journalctl-o verbose	##日志详细内容

   journalctl _PID=1245_COMM=sshd	##查看包含这些参数的日志（在详细日志查看）

   或者 journalctl|grep sshd

   ==注意: journalctl查看的是内存日志, 重启清空==

   演示案例:

   使用journalctl|grep sshd来看看用户登录情况, 重启系统，再次查询，看看日志有什么变化没有

   ```bash
   [root@alanCentos01 home]# journalctl|grep sshd
   5月 04 10:51:28 alanCentos01 sshd[7966]: Server listening on 0.0.0.0 port 22.
   5月 04 10:51:28 alanCentos01 sshd[7966]: Server listening on :: port 22.
   5月 04 10:52:11 alanCentos01 sshd[8921]: Accepted password for root from 192.168.18.1 port 3020 ssh2
   5月 04 10:52:11 alanCentos01 sshd[8921]: pam_unix(sshd:session): session opened for user root by (uid=0)
   ```

### 二十三、定制自己的Linux系统

1. 基本原理

   启动流程介绍：

   制作 Linux 小系统之前，再了解一下 Linux 的启动流程：

   1、首先 Linux 要通过自检，检查硬件设备有没有故障

   2、如果有多块启动盘的话，需要在 BIOS 中选择启动磁盘

   3、启动 MBR 中的 bootloader 引导程序

   4、加载内核文件

   5、执行所有进程的父进程、老祖宗 systemd

   6、欢迎界面

   在 Linux 的启动流程中，加载内核文件时关键文件：

   1）kernel 文件:vmlinuz-3.10.0-957.el7.x86_64

   2）initrd 文件: initramfs-3.10.0-957.el7.x86_64.img

2. 制作 min linux 思路分析

   1)在现有的 Linux 系统(centos7.6)上加一块硬盘/dev/sdb，在硬盘上分两个分区，一个是/boot，一个是/，并将其格式化。需要明确的是，现在加的这个硬盘在现有的 Linux 系统中是/dev/sdb，但是，当我们把东西全部设置好时，要把这个硬盘拔除，放在新系统上，此时，就是/dev/sda

   2)在/dev/sdb 硬盘上，将其打造成独立的 Linux 系统，里面的所有文件是需要拷贝进去的

   3)作为能独立运行的 Linux 系统，内核是一定不能少，要把内核文件和 initramfs 文件也一起拷到/dev/sdb 上

   4)以上步骤完成，我们的自制 Linux 就完成, 创建一个新的 linux 虚拟机，将其硬盘指向我们创建的硬盘，启动即可

3. 操作步骤

   1)首先，我们在现有的linux添加一块大小为20G的硬盘

   ![image-20210504181424926](assets/CentOS7.6详细说明/image-20210504181424926.png)

   ![image-20210504182327684](assets/CentOS7.6详细说明/image-20210504182327684.png)

   ![image-20210504182351909](assets/CentOS7.6详细说明/image-20210504182351909.png)

   ![image-20210504182415423](assets/CentOS7.6详细说明/image-20210504182415423.png)

   ![image-20210504182446836](assets/CentOS7.6详细说明/image-20210504182446836.png)

![image-20210504182801762](assets/CentOS7.6详细说明/image-20210504182801762.png)

![image-20210504182855427](assets/CentOS7.6详细说明/image-20210504182855427.png)

点击完成，就OK了， 可以使用 lsblk 查看==（需要重启）。==

```bash
[root@alanCentos01 ~]# lsblk
NAME   MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
sda      8:0    0   20G  0 disk 
├─sda1   8:1    0    1G  0 part /boot
├─sda2   8:2    0    2G  0 part [SWAP]
└─sda3   8:3    0   17G  0 part /
sdb      8:16   0   20G  0 disk 
sr0     11:0    1  4.3G  0 rom  /run/media/root/CentOS 7 x86_64
[root@alanCentos01 ~]# 
```

2)通过fdisk来给我们的/dev/sdb进行分区

```bash
[root@alanCentos01 ~]# fdisk /dev/sdb
欢迎使用 fdisk (util-linux 2.23.2)。

更改将停留在内存中，直到您决定将更改写入磁盘。
使用写入命令前请三思。

Device does not contain a recognized partition table
使用磁盘标识符 0x25f4a8e6 创建新的 DOS 磁盘标签。

命令(输入 m 获取帮助)：n
Partition type:
   p   primary (0 primary, 0 extended, 4 free)
   e   extended
Select (default p): p
分区号 (1-4，默认 1)：
起始 扇区 (2048-41943039，默认为 2048)：
将使用默认值 2048
Last 扇区, +扇区 or +size{K,M,G} (2048-41943039，默认为 41943039)：+500M
分区 1 已设置为 Linux 类型，大小设为 500 MiB

命令(输入 m 获取帮助)：n
Partition type:
   p   primary (1 primary, 0 extended, 3 free)
   e   extended
Select (default p): 
Using default response p
分区号 (2-4，默认 2)：
起始 扇区 (1026048-41943039，默认为 1026048)：
将使用默认值 1026048
Last 扇区, +扇区 or +size{K,M,G} (1026048-41943039，默认为 41943039)：
将使用默认值 41943039
分区 2 已设置为 Linux 类型，大小设为 19.5 GiB

命令(输入 m 获取帮助)：w
The partition table has been altered!

Calling ioctl() to re-read partition table.
正在同步磁盘。
```

```bash
[root@alanCentos01 ~]# lsblk
NAME   MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
sda      8:0    0   20G  0 disk 
├─sda1   8:1    0    1G  0 part /boot
├─sda2   8:2    0    2G  0 part [SWAP]
└─sda3   8:3    0   17G  0 part /
sdb      8:16   0   20G  0 disk 
├─sdb1   8:17   0  500M  0 part 
└─sdb2   8:18   0 19.5G  0 part 
sr0     11:0    1  4.3G  0 rom  /run/media/root/CentOS 7 x86_64
```

3)接下来，我们对/dev/sdb的分区进行格式化

```bash
[root@alanCentos01 ~]# mkfs.ext4 /dev/sdb1
mke2fs 1.42.9 (28-Dec-2013)
文件系统标签=
OS type: Linux
块大小=1024 (log=0)
分块大小=1024 (log=0)
Stride=0 blocks, Stripe width=0 blocks
128016 inodes, 512000 blocks
25600 blocks (5.00%) reserved for the super user
第一个数据块=1
Maximum filesystem blocks=34078720
63 block groups
8192 blocks per group, 8192 fragments per group
2032 inodes per group
Superblock backups stored on blocks: 
	8193, 24577, 40961, 57345, 73729, 204801, 221185, 401409

Allocating group tables: 完成                            
正在写入inode表: 完成                            
Creating journal (8192 blocks): 完成
Writing superblocks and filesystem accounting information: 完成

[root@alanCentos01 ~]# mkfs.ext4 /dev/sdb2
mke2fs 1.42.9 (28-Dec-2013)
文件系统标签=
OS type: Linux
块大小=4096 (log=2)
分块大小=4096 (log=2)
Stride=0 blocks, Stripe width=0 blocks
1281120 inodes, 5114624 blocks
255731 blocks (5.00%) reserved for the super user
第一个数据块=0
Maximum filesystem blocks=2153775104
157 block groups
32768 blocks per group, 32768 fragments per group
8160 inodes per group
Superblock backups stored on blocks: 
	32768, 98304, 163840, 229376, 294912, 819200, 884736, 1605632, 2654208, 
	4096000

Allocating group tables: 完成                            
正在写入inode表: 完成                            
Creating journal (32768 blocks): 完成
Writing superblocks and filesystem accounting information: 完成   

[root@alanCentos01 ~]# 
```

4)创建目录，并挂载新的磁盘

```bash
[root@alanCentos01 ~]# mkdir -p /mnt/boot /mnt/sysroot
[root@alanCentos01 ~]# mount /dev/sdb1 /mnt/boot
[root@alanCentos01 ~]# mount /dev/sdb2 /mnt/sysroot/
[root@alanCentos01 ~]#
```

5)安装grub, 内核文件拷贝至目标磁盘

```bash
[root@alanCentos01 ~]# grub2-install --root-directory=/mnt /dev/sdb
Installing for i386-pc platform.
Installation finished. No error reported.
[root@alanCentos01 ~]# 
```

我们可以来看一下二进制确认我们是否安装成功

```bash
[root@alanCentos01 ~]# hexdump -C -n 512 /dev/sdb
00000000  eb 63 90 00 00 00 00 00  00 00 00 00 00 00 00 00  |.c..............|
00000010  00 00 00 00 00 00 00 00  00 00 00 00 00 00 00 00  |................|
*
00000050  00 00 00 00 00 00 00 00  00 00 00 80 01 00 00 00  |................|
00000060  00 00 00 00 ff fa 90 90  f6 c2 80 74 05 f6 c2 70  |...........t...p|
00000070  74 02 b2 80 ea 79 7c 00  00 31 c0 8e d8 8e d0 bc  |t....y|..1......|
00000080  00 20 fb a0 64 7c 3c ff  74 02 88 c2 52 be 05 7c  |. ..d|<.t...R..||
00000090  b4 41 bb aa 55 cd 13 5a  52 72 3d 81 fb 55 aa 75  |.A..U..ZRr=..U.u|
000000a0  37 83 e1 01 74 32 31 c0  89 44 04 40 88 44 ff 89  |7...t21..D.@.D..|
000000b0  44 02 c7 04 10 00 66 8b  1e 5c 7c 66 89 5c 08 66  |D.....f..\|f.\.f|
000000c0  8b 1e 60 7c 66 89 5c 0c  c7 44 06 00 70 b4 42 cd  |..`|f.\..D..p.B.|
000000d0  13 72 05 bb 00 70 eb 76  b4 08 cd 13 73 0d 5a 84  |.r...p.v....s.Z.|
000000e0  d2 0f 83 de 00 be 85 7d  e9 82 00 66 0f b6 c6 88  |.......}...f....|
000000f0  64 ff 40 66 89 44 04 0f  b6 d1 c1 e2 02 88 e8 88  |d.@f.D..........|
00000100  f4 40 89 44 08 0f b6 c2  c0 e8 02 66 89 04 66 a1  |.@.D.......f..f.|
00000110  60 7c 66 09 c0 75 4e 66  a1 5c 7c 66 31 d2 66 f7  |`|f..uNf.\|f1.f.|
00000120  34 88 d1 31 d2 66 f7 74  04 3b 44 08 7d 37 fe c1  |4..1.f.t.;D.}7..|
00000130  88 c5 30 c0 c1 e8 02 08  c1 88 d0 5a 88 c6 bb 00  |..0........Z....|
00000140  70 8e c3 31 db b8 01 02  cd 13 72 1e 8c c3 60 1e  |p..1......r...`.|
00000150  b9 00 01 8e db 31 f6 bf  00 80 8e c6 fc f3 a5 1f  |.....1..........|
00000160  61 ff 26 5a 7c be 80 7d  eb 03 be 8f 7d e8 34 00  |a.&Z|..}....}.4.|
00000170  be 94 7d e8 2e 00 cd 18  eb fe 47 52 55 42 20 00  |..}.......GRUB .|
00000180  47 65 6f 6d 00 48 61 72  64 20 44 69 73 6b 00 52  |Geom.Hard Disk.R|
00000190  65 61 64 00 20 45 72 72  6f 72 0d 0a 00 bb 01 00  |ead. Error......|
000001a0  b4 0e cd 10 ac 3c 00 75  f4 c3 00 00 00 00 00 00  |.....<.u........|
000001b0  00 00 00 00 00 00 00 00  a1 3c d3 39 00 00 00 20  |.........<.9... |
000001c0  21 00 83 dd 1e 3f 00 08  00 00 00 a0 0f 00 00 dd  |!....?..........|
000001d0  1f 3f 83 d4 a2 32 00 a8  0f 00 00 58 70 02 00 00  |.?...2.....Xp...|
000001e0  00 00 00 00 00 00 00 00  00 00 00 00 00 00 00 00  |................|
000001f0  00 00 00 00 00 00 00 00  00 00 00 00 00 00 55 aa  |..............U.|
00000200
[root@alanCentos01 ~]#
```

```bash
[root@alanCentos01 ~]# rm -rf /mnt/boot/*
[root@alanCentos01 ~]# cp -rf /boot/*  /mnt/boot/
[root@alanCentos01 ~]# 
```

6)修改 /mnt/boot/grub2/grub.cfg 文件, 标红的部分 是需要使用 指令来查看的

![image-20210504213359791](assets/CentOS7.6详细说明/image-20210504213359791.png)

```bash
[root@alanCentos01 grub2]# vim /mnt/boot/grub2/grub.cfg
```

```shell
### BEGIN /etc/grub.d/10_linux ###
menuentry 'CentOS Linux (3.10.0-957.el7.x86_64) 7 (Core)' --class centos --class gnu-linux --class gnu --class os --unrestricted $menuentry_id_option 'gnulinux-3.10.0-957.el7.x86_64-advanced-2eef594e-68fc-49a0-8b23-07cf87dda424' {
	load_video
	set gfxpayload=keep
	insmod gzio
	insmod part_msdos
	insmod ext2
	set root='hd0,msdos1'
	if [ x$feature_platform_search_hint = xy ]; then
	  search --no-floppy --fs-uuid --set=root --hint-bios=hd0,msdos1 --hint-efi=hd0,msdos1 --hint-baremetal=ahci0,msdos1
--hint='hd0,msdos1'  6ba72e9a-19ec-4552-ae54-e35e735142d4
	else
	  search --no-floppy --fs-uuid --set=root 6ba72e9a-19ec-4552-ae54-e35e735142d4
	fi
	linux16 /vmlinuz-3.10.0-957.el7.x86_64 root=UUID=d2e0ce0f-e209-472a-a4f1-4085f777d9bb ro crashkernel=auto rhgb quiet LANG=zh_CN.UTF-8  selinux=0 init=/bin/bash
	initrd16 /initramfs-3.10.0-957.el7.x86_64.img
}
menuentry 'CentOS Linux (0-rescue-5bd4fb8d8e9d4198983fc1344f652b5d) 7 (Core)' --class centos --class gnu-linux --class gnu --class os --unrestricted $menuentry_id_option 'gnulinux-0-rescue-5bd4fb8d8e9d4198983fc1344f652b5d-advanced-2eef594e-68fc-49a0-8b23-07cf87dda424' {
	load_video
	insmod gzio
	insmod part_msdos
	insmod ext2
	set root='hd0,msdos1'
	if [ x$feature_platform_search_hint = xy ]; then
	  search --no-floppy --fs-uuid --set=root --hint-bios=hd0,msdos1 --hint-efi=hd0,msdos1 --hint-baremetal=ahci0,msdos1 --hint='hd0,msdos1'  6ba72e9a-19ec-4552-ae54-e35e735142d4
	else
	  search --no-floppy --fs-uuid --set=root 6ba72e9a-19ec-4552-ae54-e35e735142d4
	fi
	linux16 /vmlinuz-0-rescue-5bd4fb8d8e9d4198983fc1344f652b5d root=UUID=d2e0ce0f-e209-472a-a4f1-4085f777d9bb ro crashkernel=auto rhgb quiet selinux=0 init=/bin/bash
	initrd16 /initramfs-0-rescue-5bd4fb8d8e9d4198983fc1344f652b5d.img
}

### END /etc/grub.d/10_linux ###	  
```

![image-20210504214347206](assets/CentOS7.6详细说明/image-20210504214347206.png)

==在grub.cfg文件中 , 红色部分用 上面 sdb1 的 UUID替换，蓝色部分用 sdb2的UUID来替换, 紫色部分是添加的，表示selinux给关掉，同时设定一下init，告诉内核不要再去找这个程序了，不然开机的时候会出现错误的==

7)创建目标主机根文件系统

```bash
[root@alanCentos01 grub2]# mkdir -pv /mnt/sysroot/{etc/rc.d,usr,var,proc,sys,dev,lib,lib64,bin,sbin,boot,srv,mnt,media,home,root} 
mkdir: 已创建目录 "/mnt/sysroot/etc"
mkdir: 已创建目录 "/mnt/sysroot/etc/rc.d"
mkdir: 已创建目录 "/mnt/sysroot/usr"
mkdir: 已创建目录 "/mnt/sysroot/var"
mkdir: 已创建目录 "/mnt/sysroot/proc"
mkdir: 已创建目录 "/mnt/sysroot/sys"
mkdir: 已创建目录 "/mnt/sysroot/dev"
mkdir: 已创建目录 "/mnt/sysroot/lib"
mkdir: 已创建目录 "/mnt/sysroot/lib64"
mkdir: 已创建目录 "/mnt/sysroot/bin"
mkdir: 已创建目录 "/mnt/sysroot/sbin"
mkdir: 已创建目录 "/mnt/sysroot/boot"
mkdir: 已创建目录 "/mnt/sysroot/srv"
mkdir: 已创建目录 "/mnt/sysroot/mnt"
mkdir: 已创建目录 "/mnt/sysroot/media"
mkdir: 已创建目录 "/mnt/sysroot/home"
mkdir: 已创建目录 "/mnt/sysroot/root"
[root@alanCentos01 grub2]#
```

8)拷贝需要的bash(也可以拷贝你需要的指令)和库文件给新的系统使用

```bash
[root@alanCentos01 /]# cp /lib64/*.* /mnt/sysroot/lib64/
cp: 略过目录"/lib64/db4.7.25"
cp: 略过目录"/lib64/dleyna-1.0"
cp: 略过目录"/lib64/farstream-0.1"
cp: 略过目录"/lib64/farstream-0.2"
cp: 略过目录"/lib64/gdk-pixbuf-2.0"
cp: 略过目录"/lib64/girepository-1.0"
cp: 略过目录"/lib64/gnome-settings-daemon-3.0"
cp: 略过目录"/lib64/goa-1.0"
cp: 略过目录"/lib64/grilo-0.3"
cp: 略过目录"/lib64/gstreamer-0.10"
cp: 略过目录"/lib64/gstreamer-1.0"
cp: 略过目录"/lib64/gtk-2.0"
cp: 略过目录"/lib64/gtk-3.0"
cp: 略过目录"/lib64/libcanberra-0.30"
cp: 略过目录"/lib64/libpeas-1.0"
cp: 略过目录"/lib64/mission-control-plugins.0"
cp: 略过目录"/lib64/pulse-10.0"
cp: 略过目录"/lib64/python2.7"
cp: 略过目录"/lib64/tracker-1.0"
cp: 略过目录"/lib64/vte-2.91"
cp: 略过目录"/lib64/webkit2gtk-4.0"
[root@alanCentos01 /]# cp /bin/bash /mnt/sysroot/bin/
[root@alanCentos01 /]# 

9)现在我们就可以创建一个新的虚拟机，然后将默认分配的硬盘移除掉，指向我们刚刚创建的磁盘即可。

![image-20210504222142208](assets/CentOS7.6详细说明/image-20210504222142208.png)

![image-20210504222200353](assets/CentOS7.6详细说明/image-20210504222200353.png)

![image-20210504222218487](assets/CentOS7.6详细说明/image-20210504222218487.png)

![image-20210504222231436](assets/CentOS7.6详细说明/image-20210504222231436.png)

![image-20210504222246288](assets/CentOS7.6详细说明/image-20210504222246288.png)

![image-20210504222254540](assets/CentOS7.6详细说明/image-20210504222254540.png)

![image-20210504222304854](assets/CentOS7.6详细说明/image-20210504222304854.png)

![image-20210504222511888](assets/CentOS7.6详细说明/image-20210504222511888.png)

![image-20210504222709074](assets/CentOS7.6详细说明/image-20210504222709074.png)

10)这时，很多指令都不能使用，比如 ls , reboot 等，可以将需要的指令拷贝到对应的目录即可

11)如果要拷贝指令，重新进入到原来的 linux系统拷贝相应的指令即可，比较将 /bin/ls 拷贝到 /mnt/sysroot/bin  将/sbin/reboot 拷贝到 /mnt/sysroot/sbin

​```bash
[root@alanCentos01 ~]# mount /dev/sdb2 /mnt/sysroot/
[root@alanCentos01 /]# cp /bin/ls /mnt/sysroot/bin/
[root@alanCentos01 /]# cp /bin/systemctl  /mnt/sysroot/bin/
[root@alanCentos01 /]# cp /sbin/reboot /mnt/sysroot/sbin/
[root@alanCentos01 /]#
```

12)再重新启动新的min linux系统，就可以使用 ls , reboot 指令了

![image-20210504224204801](assets/CentOS7.6详细说明/image-20210504224204801.png)

### 二十四、linux内核升级

uname -a //查看当前的内核版本

yum info kernel -q //检测内核版本，显示可以升级的内核

yum update kernel //升级内核

yum list kernel -q //查看已经安装的内核

### 二十五、linux系统-备份与恢复

1. 基本介绍

   实体机无法做快照，如果系统出现异常或者数据损坏，后果严重， 要重做系统，还会造成数据丢失。所以我们可以使用备份和恢复技术。

   linux 的备份和恢复很简单 ，有两种方式：

   1)把需要的文件(或者分区)用 TAR 打包就行，下次需要恢复的时候，再解压开覆盖即可

   2)使用 dump 和 restore 命令
   
2. 安装 dump 和 restore

   如果 linux 上没有 dump 和 restore 指令，需要先按照==（安装好了dump，一般restore也会自动安装好）==

   yum -y install dump

   yum -y install restore

3. 使用 dump 完成备份

   * 基本介绍

     dump 支持分卷和增量备份（所谓增量备份是指备份上次备份后 修改/增加过的文件，也称差异备份）。

   * dump 语法说明

     dump \[ -cu] \[-123456789] \[ -f <备份后文件名>] \[-T <日期>] \[ 目录或文件系统]

     dump [] -wW

     -c ：创建新的归档文件，并将由一个或多个文件参数所指定的内容写入归档文件的开头。

     -0123456789：备份的层级。0 为最完整备份，会备份所有文件。若指定 0 以上的层级，则备份至上一次备份以来修改或新增的文件, 到 9 后，可以再次轮替。

     -f <备份后文件名>：指定备份后文件名

     -j： 调用 bzlib 库压缩备份文件，也就是将备份后的文件压缩成 bz2 格式，让文件更小

     -T <日期>：指定开始备份的时间与日期

     -u ：备份完毕后，在/etc/dumpdates 中记录备份的文件系统，层级，日期与时间等。

     -t ：指定文件名，若该文件已存在备份文件中，则列出名称

     -W：显示需要备份的文件及其最后一次备份的层级，时间 ，日期。

     -w：与-W 类似，但仅显示需要备份的文件。

   * dump 应用案例1

     将/boot 分区所有内容备份到/opt/boot.bak0.bz2 文件中，备份层级为“0”

     ```bash
     [root@alanCentos01 ~]# dump -0uj -f /opt/boot.bak0.bz2 /boot
       DUMP: Date of this level 0 dump: Wed May  5 21:49:34 2021
       DUMP: Dumping /dev/sda1 (/boot) to /opt/boot.bak0.bz2
       DUMP: Label: none
       DUMP: Writing 10 Kilobyte records
       DUMP: Compressing output at compression level 2 (bzlib)
       DUMP: mapping (Pass I) [regular files]
       DUMP: mapping (Pass II) [directories]
       DUMP: estimated 144895 blocks.
       DUMP: Volume 1 started with block 1 at: Wed May  5 21:49:34 2021
       DUMP: dumping (Pass III) [directories]
       DUMP: dumping (Pass IV) [regular files]
       DUMP: Closing /opt/boot.bak0.bz2
       DUMP: Volume 1 completed at: Wed May  5 21:49:57 2021
       DUMP: Volume 1 took 0:00:23
       DUMP: Volume 1 transfer rate: 5689 kB/s
       DUMP: Volume 1 145510kB uncompressed, 130867kB compressed, 1.112:1
       DUMP: 145510 blocks (142.10MB) on 1 volume(s)
       DUMP: finished in 23 seconds, throughput 6326 kBytes/sec
       DUMP: Date of this level 0 dump: Wed May  5 21:49:34 2021
       DUMP: Date this dump completed:  Wed May  5 21:49:57 2021
       DUMP: Average transfer rate: 5689 kB/s
       DUMP: Wrote 145510kB uncompressed, 130867kB compressed, 1.112:1
       DUMP: DUMP IS DONE
     [root@alanCentos01 ~]#
     ```

   * dump 应用案例2

     在/boot 目录下增加新文件，备份层级为“1”(只备份上次使用层次“0”备份后发生过改变的数据)，注意比较看看这次生成的备份文件 boot1.bak 有多大

     ```bash
     [root@alanCentos01 ~]# dump -1uj -f /opt/boot.bak1.bz2 /boot
       DUMP: Date of this level 1 dump: Wed May  5 21:57:39 2021
       DUMP: Date of last level 0 dump: Wed May  5 21:49:34 2021
       DUMP: Dumping /dev/sda1 (/boot) to /opt/boot.bak1.bz2
       DUMP: Label: none
       DUMP: Writing 10 Kilobyte records
       DUMP: Compressing output at compression level 2 (bzlib)
       DUMP: mapping (Pass I) [regular files]
       DUMP: mapping (Pass II) [directories]
       DUMP: estimated 31 blocks.
       DUMP: Volume 1 started with block 1 at: Wed May  5 21:57:39 2021
       DUMP: dumping (Pass III) [directories]
       DUMP: dumping (Pass IV) [regular files]
       DUMP: Closing /opt/boot.bak1.bz2
       DUMP: Volume 1 completed at: Wed May  5 21:57:39 2021
       DUMP: 30 blocks (0.03MB) on 1 volume(s)
       DUMP: finished in less than a second
       DUMP: Date of this level 1 dump: Wed May  5 21:57:39 2021
       DUMP: Date this dump completed:  Wed May  5 21:57:39 2021
       DUMP: Average transfer rate: 0 kB/s
       DUMP: Wrote 30kB uncompressed, 10kB compressed, 3.001:1
       DUMP: DUMP IS DONE
     [root@alanCentos01 ~]# ls -lh /opt/
     总用量 182M
     -rw-r--r--. 1 root root 128M 5月   5 21:49 boot.bak0.bz2
     -rw-r--r--. 1 root root  11K 5月   5 21:57 boot.bak1.bz2
     drwxr-xr-x. 3 root root 4.0K 4月  28 23:07 idea
     drwxr-xr-x. 2 root root 4.0K 4月  25 18:48 jdk
     drwxr-xr-x. 2 root root 4.0K 4月  25 23:19 mysql
     drwxr-xr-x. 2 root root 4.0K 10月 31 2018 rh
     drwxr-xr-x. 3 root root 4.0K 4月  25 21:54 tomcat
     -rw-r--r--. 1 root root  54M 6月  13 2019 VMwareTools-10.3.10-13959562.tar.gz
     drwxr-xr-x. 9 root root 4.0K 6月  13 2019 vmware-tools-distrib
     [root@alanCentos01 ~]# 
     ```

     ==提醒: 通过 dump 命令在配合 crontab 可以实现无人值守备份==

   * dump -W

     显示需要备份的文件及其最后一次备份的层级，时间 ，日期

     ```bash
     [root@alanCentos01 ~]# dump -W
     Last dump(s) done (Dump '>' file systems):
     > /dev/sda3	(     /) Last dump: never
       /dev/sda1	( /boot) Last dump: Level 1, Date Wed May  5 21:57:39 2021
     [root@alanCentos01 ~]#
     ```

   * 查看备份时间文件

     ```bash
     [root@alanCentos01 ~]# cat /etc/dumpdates
     /dev/sda1 0 Wed May  5 21:49:34 2021 +0800
     /dev/sda1 1 Wed May  5 21:57:39 2021 +0800
     ```

   * dump 备份文件或者目录

     前面我们在备份分区时，是可以支持增量备份的，如果备份文件或者目录，不再支持增量备份, 即只能使用 0 级别备份

     案例：使用 dump 备份 /etc 整个目录

     ```bash
     [root@alanCentos01 ~]# dump -0j -f /opt/etc.bak.bz2 /etc/
       DUMP: Date of this level 0 dump: Wed May  5 22:23:42 2021
       DUMP: Dumping /dev/sda3 (/ (dir etc)) to /opt/etc.bak.bz2
       DUMP: Label: none
       DUMP: Writing 10 Kilobyte records
       DUMP: Compressing output at compression level 2 (bzlib)
       DUMP: mapping (Pass I) [regular files]
       DUMP: mapping (Pass II) [directories]
       DUMP: estimated 67720 blocks.
       DUMP: Volume 1 started with block 1 at: Wed May  5 22:23:43 2021
       DUMP: dumping (Pass III) [directories]
       DUMP: dumping (Pass IV) [regular files]
       DUMP: Closing /opt/etc.bak.bz2
       DUMP: Volume 1 completed at: Wed May  5 22:23:51 2021
       DUMP: Volume 1 took 0:00:08
       DUMP: Volume 1 transfer rate: 3034 kB/s
       DUMP: Volume 1 77340kB uncompressed, 24272kB compressed, 3.187:1
       DUMP: 77340 blocks (75.53MB) on 1 volume(s)
       DUMP: finished in 8 seconds, throughput 9667 kBytes/sec
       DUMP: Date of this level 0 dump: Wed May  5 22:23:42 2021
       DUMP: Date this dump completed:  Wed May  5 22:23:51 2021
       DUMP: Average transfer rate: 3034 kB/s
       DUMP: Wrote 77340kB uncompressed, 24272kB compressed, 3.187:1
       DUMP: DUMP IS DONE
     [root@alanCentos01 ~]#
     ```

     ==下面这条语句会报错，提示 DUMP: Only level 0 dumps are allowed on a subdirectory==

     ```bash
     [root@alanCentos01 ~]# dump -1j -f /opt/etc.bak.bz2 /etc/
       DUMP: Only level 0 dumps are allowed on a subdirectory
       DUMP: The ENTIRE dump is aborted.
     [root@alanCentos01 ~]#
     ```

   * ==提醒：如果是重要的备份文件， 比如数据区，建议将文件上传到其它服务器保存，不要将鸡蛋放在同一个篮子不要将鸡蛋放在同一个篮子。==

4. 使用 restore 完成恢复

   * 基本介绍

     restore 命令用来恢复已备份的文件，可以从 dump 生成的备份文件中恢复原文件

   * restore 基本语法

     restore \[模式选项] \[选项]

     说明下面四个模式， 不能混用，在一次命令中， 只能指定一种。

     -C：使用对比模式，将备份的文件与已存在的文件相互对比。

     -i：使用交互模式，在进行还原操作时，restors 指令将依序询问用户

     -r：进行还原模式

     -t： 查看模式，看备份文件有哪些文件

     选项

     -f <备份设备>：从指定的文件中读取备份数据，进行还原操作

   * 应用案例1

     restore 命令比较模式，比较备份文件和原文件的区别

     ```bash
     [root@alanCentos01 opt]# restore -C -f boot.bak1.bz2
     Dump tape is compressed.
     Dump   date: Wed May  5 21:57:39 2021
     Dumped from: Wed May  5 21:49:34 2021
     Level 1 dump of /boot on alanCentos01:/dev/sda1
     Label: none
     filesys = /boot
     [root@alanCentos01 opt]#
     ```

     如果比较有区别，显示结果类似下图

     ![image-20210505224608122](assets/CentOS7.6详细说明/image-20210505224608122.png)

   * 应用案例2

     restore 命令查看模式，看备份文件有哪些数据/文件

     ```bash
     [root@alanCentos01 opt]# restore -t -f boot.bak0.bz2
     Dump tape is compressed.
     Dump   date: Wed May  5 21:49:34 2021
     Dumped from: the epoch
     Level 0 dump of /boot on alanCentos01:/dev/sda1
     Label: none
              2	.
             11	./lost+found
             12	./efi
             13	./efi/EFI
             14	./efi/EFI/centos
             22	./efi/EFI/centos/MokManager.efi
             20	./efi/EFI/centos/BOOT.CSV
             21	./efi/EFI/centos/BOOTX64.CSV
             25	./efi/EFI/centos/shimx64-centos.efi
             23	./efi/EFI/centos/mmx64.efi
             24	./efi/EFI/centos/shim.efi
             27	./efi/EFI/centos/fw
             26	./efi/EFI/centos/shimx64.efi
             28	./efi/EFI/centos/fwupia32.efi
             29	./efi/EFI/centos/fwupx64.efi
             16	./efi/EFI/BOOT
             18	./efi/EFI/BOOT/fallback.efi
             17	./efi/EFI/BOOT/BOOTX64.EFI
             19	./efi/EFI/BOOT/fbx64.efi
           8193	./grub2
             38	./grub2/device.map
             39	./grub2/i386-pc
             43	./grub2/i386-pc/videotest.mod
             44	./grub2/i386-pc/blocklist.mod
             45	./grub2/i386-pc/regexp.mod
             53	./grub2/i386-pc/parttool.mod
             54	./grub2/i386-pc/read.mod
             55	./grub2/i386-pc/truecrypt.mod
             56	./grub2/i386-pc/help.mod
             59	./grub2/i386-pc/serial.mod
             60	./grub2/i386-pc/cpio_be.mod
             61	./grub2/i386-pc/newc.mod
             64	./grub2/i386-pc/xfs.mod
             65	./grub2/i386-pc/test.mod
             69	./grub2/i386-pc/loopback.mod
             71	./grub2/i386-pc/loadenv.mod
             73	./grub2/i386-pc/ahci.mod
             74	./grub2/i386-pc/gfxterm.mod
             75	./grub2/i386-pc/gcry_md4.mod
             76	./grub2/i386-pc/ext2.mod
             78	./grub2/i386-pc/dm_nv.mod
             79	./grub2/i386-pc/gcry_rsa.mod
             80	./grub2/i386-pc/elf.mod
             85	./grub2/i386-pc/udf.mod
             87	./grub2/i386-pc/part_dvh.mod
             88	./grub2/i386-pc/part_apple.mod
             91	./grub2/i386-pc/multiboot.mod
             92	./grub2/i386-pc/functional_test.mod
             94	./grub2/i386-pc/gcry_sha512.mod
             98	./grub2/i386-pc/gzio.mod
            101	./grub2/i386-pc/progress.mod
            103	./grub2/i386-pc/geli.mod
            104	./grub2/i386-pc/msdospart.mod
            107	./grub2/i386-pc/ufs1_be.mod
            108	./grub2/i386-pc/cmosdump.mod
            113	./grub2/i386-pc/cmostest.mod
            115	./grub2/i386-pc/uhci.mod
            117	./grub2/i386-pc/gcry_sha256.mod
            118	./grub2/i386-pc/xnu_uuid.mod
            119	./grub2/i386-pc/part_bsd.mod
            121	./grub2/i386-pc/sleep_test.mod
            123	./grub2/i386-pc/archelp.mod
            127	./grub2/i386-pc/legacycfg.mod
            129	./grub2/i386-pc/halt.mod
            131	./grub2/i386-pc/bitmap_scale.mod
            136	./grub2/i386-pc/zfsinfo.mod
            137	./grub2/i386-pc/btrfs.mod
            138	./grub2/i386-pc/gcry_blowfish.mod
            139	./grub2/i386-pc/gcry_tiger.mod
            140	./grub2/i386-pc/play.mod
            141	./grub2/i386-pc/minix_be.mod
            142	./grub2/i386-pc/hashsum.mod
            143	./grub2/i386-pc/gcry_idea.mod
            144	./grub2/i386-pc/bsd.mod
            145	./grub2/i386-pc/sfs.mod
            147	./grub2/i386-pc/mdraid09.mod
            149	./grub2/i386-pc/part_sun.mod
            151	./grub2/i386-pc/hfspluscomp.mod
            155	./grub2/i386-pc/macbless.mod
            156	./grub2/i386-pc/bitmap.mod
            157	./grub2/i386-pc/pxechain.mod
            158	./grub2/i386-pc/priority_queue.mod
            161	./grub2/i386-pc/memrw.mod
            163	./grub2/i386-pc/relocator.mod
            165	./grub2/i386-pc/affs.mod
            166	./grub2/i386-pc/net.mod
            167	./grub2/i386-pc/file.mod
            170	./grub2/i386-pc/adler32.mod
            172	./grub2/i386-pc/legacy_password_test.mod
            175	./grub2/i386-pc/password_pbkdf2.mod
            176	./grub2/i386-pc/setjmp_test.mod
            179	./grub2/i386-pc/cbmemc.mod
            180	./grub2/i386-pc/part_plan.mod
            181	./grub2/i386-pc/linux16.mod
            182	./grub2/i386-pc/echo.mod
            184	./grub2/i386-pc/part_msdos.mod
            190	./grub2/i386-pc/search.mod
            191	./grub2/i386-pc/gfxmenu.mod
            192	./grub2/i386-pc/usb.mod
            196	./grub2/i386-pc/ufs1.mod
            201	./grub2/i386-pc/chain.mod
            202	./grub2/i386-pc/linux.mod
            207	./grub2/i386-pc/minix3.mod
            209	./grub2/i386-pc/cs5536.mod
            212	./grub2/i386-pc/minix.mod
            213	./grub2/i386-pc/crc64.mod
            214	./grub2/i386-pc/gettext.mod
            216	./grub2/i386-pc/video_colors.mod
            218	./grub2/i386-pc/gcry_crc.mod
            222	./grub2/i386-pc/videotest_checksum.mod
            223	./grub2/i386-pc/minix2_be.mod
            226	./grub2/i386-pc/lsapm.mod
            227	./grub2/i386-pc/lsacpi.mod
            229	./grub2/i386-pc/terminfo.mod
            230	./grub2/i386-pc/boot.mod
            231	./grub2/i386-pc/raid6rec.mod
            233	./grub2/i386-pc/part_gpt.mod
            234	./grub2/i386-pc/video.mod
            236	./grub2/i386-pc/gcry_serpent.mod
            239	./grub2/i386-pc/raid5rec.mod
            240	./grub2/i386-pc/usbserial_usbdebug.mod
            241	./grub2/i386-pc/minicmd.mod
            244	./grub2/i386-pc/gcry_sha1.mod
            246	./grub2/i386-pc/squash4.mod
            248	./grub2/i386-pc/cbfs.mod
            250	./grub2/i386-pc/mda_text.mod
            256	./grub2/i386-pc/hfsplus.mod
            257	./grub2/i386-pc/gcry_md5.mod
            258	./grub2/i386-pc/aout.mod
            259	./grub2/i386-pc/verify.mod
            260	./grub2/i386-pc/backtrace.mod
            262	./grub2/i386-pc/mdraid09_be.mod
            264	./grub2/i386-pc/part_sunpc.mod
            265	./grub2/i386-pc/part_dfly.mod
            272	./grub2/i386-pc/usbtest.mod
            277	./grub2/i386-pc/date.mod
            279	./grub2/i386-pc/exfctest.mod
            280	./grub2/i386-pc/usbms.mod
            281	./grub2/i386-pc/tga.mod
            282	./grub2/i386-pc/cpuid.mod
            283	./grub2/i386-pc/crypto.mod
            284	./grub2/i386-pc/cmdline_cat_test.mod
            285	./grub2/i386-pc/cbls.mod
            287	./grub2/i386-pc/ntldr.mod
            289	./grub2/i386-pc/ntfscomp.mod
            293	./grub2/i386-pc/hfs.mod
            301	./grub2/i386-pc/partmap.lst
            200	./grub2/i386-pc/gcry_rfc2268.mod
            198	./grub2/i386-pc/ufs2.mod
             49	./grub2/i386-pc/minix3_be.mod
             96	./grub2/i386-pc/xnu_uuid_test.mod
            232	./grub2/i386-pc/mdraid1x.mod
            168	./grub2/i386-pc/terminal.mod
            199	./grub2/i386-pc/gdb.mod
             95	./grub2/i386-pc/luks.mod
            132	./grub2/i386-pc/signature_test.mod
             93	./grub2/i386-pc/setjmp.mod
            205	./grub2/i386-pc/all_video.mod
            110	./grub2/i386-pc/part_acorn.mod
             82	./grub2/i386-pc/cmp.mod
            114	./grub2/i386-pc/usbserial_pl2303.mod
            152	./grub2/i386-pc/pbkdf2_test.mod
            146	./grub2/i386-pc/vga.mod
            169	./grub2/i386-pc/sendkey.mod
            162	./grub2/i386-pc/usbserial_ftdi.mod
            185	./grub2/i386-pc/ata.mod
            125	./grub2/i386-pc/div_test.mod
             52	./grub2/i386-pc/search_label.mod
             42	./grub2/i386-pc/afs.mod
            134	./grub2/i386-pc/syslinuxcfg.mod
            102	./grub2/i386-pc/reboot.mod
             41	./grub2/i386-pc/hdparm.mod
            228	./grub2/i386-pc/usbserial_common.mod
            159	./grub2/i386-pc/lspci.mod
             97	./grub2/i386-pc/gcry_camellia.mod
            116	./grub2/i386-pc/procfs.mod
            150	./grub2/i386-pc/lvm.mod
             77	./grub2/i386-pc/mmap.mod
            197	./grub2/i386-pc/cryptodisk.mod
            235	./grub2/i386-pc/gfxterm_menu.mod
             86	./grub2/i386-pc/tr.mod
             51	./grub2/i386-pc/sleep.mod
             90	./grub2/i386-pc/video_bochs.mod
            174	./grub2/i386-pc/pxe.mod
            219	./grub2/i386-pc/gcry_rijndael.mod
            128	./grub2/i386-pc/freedos.mod
            210	./grub2/i386-pc/ntfs.mod
            186	./grub2/i386-pc/nilfs2.mod
            105	./grub2/i386-pc/minix2.mod
             57	./grub2/i386-pc/fshelp.mod
            177	./grub2/i386-pc/pata.mod
            195	./grub2/i386-pc/biosdisk.mod
            109	./grub2/i386-pc/video_fb.mod
            173	./grub2/i386-pc/bufio.mod
            204	./grub2/i386-pc/pcidump.mod
            193	./grub2/i386-pc/zfs.mod
            211	./grub2/i386-pc/gcry_des.mod
            100	./grub2/i386-pc/tftp.mod
            120	./grub2/i386-pc/keylayouts.mod
            221	./grub2/i386-pc/datehook.mod
             48	./grub2/i386-pc/macho.mod
             72	./grub2/i386-pc/testspeed.mod
            194	./grub2/i386-pc/mpi.mod
            206	./grub2/i386-pc/gcry_rmd160.mod
             58	./grub2/i386-pc/spkmodem.mod
            238	./grub2/i386-pc/bfs.mod
             62	./grub2/i386-pc/gcry_whirlpool.mod
            130	./grub2/i386-pc/morse.mod
            126	./grub2/i386-pc/pbkdf2.mod
            171	./grub2/i386-pc/hello.mod
            122	./grub2/i386-pc/test_blockarg.mod
            237	./grub2/i386-pc/zfscrypt.mod
            183	./grub2/i386-pc/romfs.mod
             50	./grub2/i386-pc/ehci.mod
            153	./grub2/i386-pc/memdisk.mod
            208	./grub2/i386-pc/extcmd.mod
            112	./grub2/i386-pc/disk.mod
            178	./grub2/i386-pc/gcry_dsa.mod
             84	./grub2/i386-pc/cbtime.mod
            187	./grub2/i386-pc/odc.mod
            111	./grub2/i386-pc/setpci.mod
             83	./grub2/i386-pc/exfat.mod
            215	./grub2/i386-pc/jpeg.mod
            164	./grub2/i386-pc/time.mod
            217	./grub2/i386-pc/gcry_arcfour.mod
            189	./grub2/i386-pc/efiemu.mod
             67	./grub2/i386-pc/xnu.mod
            135	./grub2/i386-pc/cat.mod
             68	./grub2/i386-pc/video_cirrus.mod
            160	./grub2/i386-pc/password.mod
            124	./grub2/i386-pc/configfile.mod
             66	./grub2/i386-pc/http.mod
            188	./grub2/i386-pc/ldm.mod
            154	./grub2/i386-pc/search_fs_file.mod
             46	./grub2/i386-pc/eval.mod
             70	./grub2/i386-pc/ohci.mod
            225	./grub2/i386-pc/probe.mod
            224	./grub2/i386-pc/normal.mod
            203	./grub2/i386-pc/trig.mod
            133	./grub2/i386-pc/true.mod
             89	./grub2/i386-pc/iso9660.mod
             99	./grub2/i386-pc/gcry_twofish.mod
             63	./grub2/i386-pc/hexdump.mod
            148	./grub2/i386-pc/plan9.mod
            220	./grub2/i386-pc/pci.mod
            106	./grub2/i386-pc/usb_keyboard.mod
             81	./grub2/i386-pc/drivemap.mod
             47	./grub2/i386-pc/cbtable.mod
            242	./grub2/i386-pc/search_fs_uuid.mod
            243	./grub2/i386-pc/vga_text.mod
            245	./grub2/i386-pc/jfs.mod
            247	./grub2/i386-pc/xzio.mod
            249	./grub2/i386-pc/iorw.mod
            251	./grub2/i386-pc/diskfilter.mod
            252	./grub2/i386-pc/scsi.mod
            253	./grub2/i386-pc/videoinfo.mod
            254	./grub2/i386-pc/cpio.mod
            255	./grub2/i386-pc/font.mod
            261	./grub2/i386-pc/fat.mod
            263	./grub2/i386-pc/lsmmap.mod
            266	./grub2/i386-pc/ls.mod
            267	./grub2/i386-pc/multiboot2.mod
            268	./grub2/i386-pc/testload.mod
            269	./grub2/i386-pc/vbe.mod
            270	./grub2/i386-pc/tar.mod
            271	./grub2/i386-pc/lzopio.mod
            273	./grub2/i386-pc/offsetio.mod
            274	./grub2/i386-pc/gcry_cast5.mod
            275	./grub2/i386-pc/reiserfs.mod
            276	./grub2/i386-pc/blscfg.mod
            278	./grub2/i386-pc/datetime.mod
            286	./grub2/i386-pc/keystatus.mod
            288	./grub2/i386-pc/gcry_seed.mod
            290	./grub2/i386-pc/gptsync.mod
            291	./grub2/i386-pc/part_amiga.mod
            292	./grub2/i386-pc/at_keyboard.mod
            294	./grub2/i386-pc/nativedisk.mod
            295	./grub2/i386-pc/png.mod
            296	./grub2/i386-pc/acpi.mod
            297	./grub2/i386-pc/gfxterm_background.mod
            298	./grub2/i386-pc/moddep.lst
            299	./grub2/i386-pc/command.lst
            300	./grub2/i386-pc/fs.lst
            302	./grub2/i386-pc/parttool.lst
            303	./grub2/i386-pc/video.lst
            304	./grub2/i386-pc/crypto.lst
            305	./grub2/i386-pc/terminal.lst
            306	./grub2/i386-pc/modinfo.sh
            344	./grub2/i386-pc/core.img
            345	./grub2/i386-pc/boot.img
             40	./grub2/locale
            307	./grub2/locale/pt_BR.mo
            308	./grub2/locale/de.mo
            309	./grub2/locale/es.mo
            310	./grub2/locale/en@cyrillic.mo
            311	./grub2/locale/zh_TW.mo
            312	./grub2/locale/lt.mo
            313	./grub2/locale/eo.mo
            314	./grub2/locale/id.mo
            315	./grub2/locale/en@quot.mo
            316	./grub2/locale/en@hebrew.mo
            317	./grub2/locale/ru.mo
            318	./grub2/locale/fi.mo
            319	./grub2/locale/nl.mo
            320	./grub2/locale/ca.mo
            321	./grub2/locale/gl.mo
            322	./grub2/locale/ast.mo
            323	./grub2/locale/de_CH.mo
            324	./grub2/locale/en@arabic.mo
            325	./grub2/locale/it.mo
            326	./grub2/locale/fr.mo
            327	./grub2/locale/pa.mo
            328	./grub2/locale/hu.mo
            329	./grub2/locale/vi.mo
            330	./grub2/locale/de@hebrew.mo
            331	./grub2/locale/sv.mo
            332	./grub2/locale/ja.mo
            333	./grub2/locale/tr.mo
            334	./grub2/locale/en@piglatin.mo
            335	./grub2/locale/pl.mo
            336	./grub2/locale/sl.mo
            337	./grub2/locale/zh_CN.mo
            338	./grub2/locale/da.mo
            339	./grub2/locale/uk.mo
            340	./grub2/locale/en@greek.mo
            341	./grub2/fonts
            342	./grub2/fonts/unicode.pf2
            343	./grub2/grubenv
            346	./grub2/grub.cfg
           8194	./grub
             15	./grub/splash.xpm.gz
             35	./initramfs-3.10.0-957.el7.x86_64.img
             30	./.vmlinuz-3.10.0-957.el7.x86_64.hmac
             31	./System.map-3.10.0-957.el7.x86_64
             32	./config-3.10.0-957.el7.x86_64
             33	./symvers-3.10.0-957.el7.x86_64.gz
             34	./vmlinuz-3.10.0-957.el7.x86_64
             36	./initramfs-0-rescue-7156b93e99974f91aaaceb7593173fc4.img
             37	./vmlinuz-0-rescue-7156b93e99974f91aaaceb7593173fc4
            347	./initramfs-3.10.0-957.el7.x86_64kdump.img
     [root@alanCentos01 opt]# 
     ```

   * 应用案例3

     restore 命令还原模式，注意细节：如果你有增量备份，需要把增量备份文件也进行恢复，有几个增量备份文件，就要恢复几个，按顺序来恢复即可。

     ```bash
     [root@alanCentos01 opt]# mkdir /opt/boottmp
     [root@alanCentos01 opt]# cd /opt/boottmp
     [root@alanCentos01 boottmp]# restore -r -f /opt/boot.bak0.bz2
     Dump tape is compressed.
     [root@alanCentos01 boottmp]# restore -r -f /opt/boot.bak1.bz2
     Dump tape is compressed.
     [root@alanCentos01 boottmp]# ll
     总用量 128428
     -rw-r--r--. 1 root root   151918 11月  9 2018 config-3.10.0-957.el7.x86_64
     drwx------. 3 root root     4096 11月  9 2018 efi
     drwxr-xr-x. 2 root root     4096 4月   2 00:25 grub
     drwx------. 5 root root     4096 4月   2 00:33 grub2
     -rw-------. 1 root root 74311607 4月   2 00:31 initramfs-0-rescue-7156b93e99974f91aaaceb7593173fc4.img
     -rw-------. 1 root root 28926577 4月   2 00:33 initramfs-3.10.0-957.el7.x86_64.img
     -rw-------. 1 root root 10797022 5月   5 21:05 initramfs-3.10.0-957.el7.x86_64kdump.img
     drwx------. 2 root root     4096 4月   2 00:23 lost+found
     -rw-------. 1 root root   145264 5月   5 22:55 restoresymtable
     -rw-r--r--. 1 root root   314036 11月  9 2018 symvers-3.10.0-957.el7.x86_64.gz
     -rw-------. 1 root root  3543471 11月  9 2018 System.map-3.10.0-957.el7.x86_64
     -rwxr-xr-x. 1 root root  6639904 4月   2 00:31 vmlinuz-0-rescue-7156b93e99974f91aaaceb7593173fc4
     -rwxr-xr-x. 1 root root  6639904 11月  9 2018 vmlinuz-3.10.0-957.el7.x86_64
     [root@alanCentos01 boottmp]# 
     ```

   * 应用案例4

     restore 命令恢复备份的文件，或者整个目录的文件

     基本语法： restore -r -f 备份好的文件

     ```bash
     [root@alanCentos01 /]# mkdir /opt/etctmp
     [root@alanCentos01 /]# cd /opt/etctmp
     [root@alanCentos01 etctmp]# restore -r -f /opt/etc.bak.bz2
     Dump tape is compressed.
     ./lost+found: (inode 11) not found on tape
     ./boot: (inode 655361) not found on tape
     ./dev: (inode 131073) not found on tape
     ./proc: (inode 917505) not found on tape
     ./run: (inode 786433) not found on tape
     ./sys: (inode 262145) not found on tape
     ./root: (inode 524289) not found on tape
     ./var: (inode 393222) not found on tape
     ./tmp: (inode 262146) not found on tape
     ./P: (inode 440) not found on tape
     ./usr: (inode 131074) not found on tape
     ./bin: (inode 17) not found on tape
     ./sbin: (inode 16) not found on tape
     ./lib: (inode 13) not found on tape
     ./lib64: (inode 15) not found on tape
     ./home: (inode 917506) not found on tape
     ./media: (inode 524290) not found on tape
     ./mnt: (inode 655362) not found on tape
     ./opt: (inode 786434) not found on tape
     ./srv: (inode 524291) not found on tape
     ./.jetbrains: (inode 393919) not found on tape
     ./tmpjcef-p19572_scheme.tmp: (inode 2890) not found on tape
     [root@alanCentos01 etctmp]# ll
     总用量 2172
     drwxr-xr-x. 144 root root   12288 5月   5 21:28 etc
     -rw-------.   1 root root 2211664 5月   5 22:59 restoresymtable
     [root@alanCentos01 etctmp]#
     ```

### 二十六、Linux可视化管理webmin和bt运维工具

1. webmin

   * 基本介绍

     Webmin 是功能强大的基于 Web 的 Unix/linux 系统管理工具。管理员通过浏览器访问 Webmin 的各种管理功能并完成相应的管理操作。除了各版本的 linux 以外还可用于：AIX、HPUX、Solaris、Unixware、Irix 和 FreeBSD 等系统。

     ![image-20210505231312859](assets/CentOS7.6详细说明/image-20210505231312859.png)

   * 安装 webmin和配置

     1)下载地址 : http://download.webmin.com/download/yum/ 

     ```bash
     [root@alanCentos01 opt]# wget http://download.webmin.com/download/yum/webmin-1.700-1.noarch.rpm
     --2021-05-05 23:15:33--  http://download.webmin.com/download/yum/webmin-1.700-1.noarch.rpm
     正在解析主机 download.webmin.com (download.webmin.com)... 104.207.151.13, 108.60.199.109
     正在连接 download.webmin.com (download.webmin.com)|104.207.151.13|:80... 已连接。
     已发出 HTTP 请求，正在等待回应... 200 OK
     长度：22287607 (21M) [application/octet-stream]
     正在保存至: “webmin-1.700-1.noarch.rpm”
     
     100%[==============================================================================================================================>] 22,287,607  39.3KB/s 用时 13m 14s
     
     2021-05-05 23:28:49 (27.4 KB/s) - 已保存 “webmin-1.700-1.noarch.rpm” [22287607/22287607])
     
     [root@alanCentos01 opt]#
     ```

     2)安装：rpm -ivh webmin-1.700-1.noarch.rpm

     ```bash
     [root@alanCentos01 opt]# mkdir webmin
     [root@alanCentos01 opt]# cd webmin
     [root@alanCentos01 webmin]# mv /opt/webmin-1.700-1.noarch.rpm /opt/webmin
     [root@alanCentos01 webmin]# ls
     webmin-1.700-1.noarch.rpm
     [root@alanCentos01 webmin]# rpm -ivh webmin-1.700-1.noarch.rpm
     警告：webmin-1.700-1.noarch.rpm: 头V3 DSA/SHA1 Signature, 密钥 ID 11f63c51: NOKEY
     准备中...                          ################################# [100%]
     Operating system is CentOS Linux
     正在升级/安装...
        1:webmin-1.700-1                   ################################# [100%]
     Webmin install complete. You can now login to http://alanCentos01:10000/
     as root with your root password.
     [root@alanCentos01 webmin]#
     ```

     3)重置密码/usr/libexec/webmin/changepass.pl /etc/webmin root test

     root 是 webmin 的用户名，不是 OS 的 , 这里就是把 webmin 的 root 用户密码改成了 test

     ```bash
     [root@alanCentos01 webmin]# /usr/libexec/webmin/changepass.pl /etc/webmin root test
     Updated password of Webmin user root
     [root@alanCentos01 webmin]#
     ```

     4)修改 webmin 服务的端口号（默认是 10000 出于安全目的）

     ```bash
     [root@alanCentos01 webmin]# vim /etc/webmin/miniserv.conf
     ```

     5)将 port=10000 修改为其他端口号，如 port=6868

     ![image-20210505233713833](assets/CentOS7.6详细说明/image-20210505233713833.png)

     ```shell
     port=6868
     root=/usr/libexec/webmin
     mimetypes=/usr/libexec/webmin/mime.types
     addtype_cgi=internal/cgi
     realm=Webmin Server
     logfile=/var/webmin/miniserv.log
     errorlog=/var/webmin/miniserv.error
     pidfile=/var/webmin/miniserv.pid
     logtime=168
     ppath=
     ssl=0
     env_WEBMIN_CONFIG=/etc/webmin
     env_WEBMIN_VAR=/var/webmin
     atboot=1
     logout=/etc/webmin/logout-flag
     listen=6868
     denyfile=\.pl$
     log=1
     blockhost_failures=5
     blockhost_time=60
     syslog=1
     session=1
     premodules=WebminCore
     server=MiniServ/1.700
     userfile=/etc/webmin/miniserv.users
     keyfile=/etc/webmin/miniserv.pem
     passwd_file=/etc/shadow
     passwd_uindex=0
     passwd_pindex=1
     passwd_cindex=2
     passwd_mindex=4
     passwd_mode=0
     preroot=gray-theme
     passdelay=1
     ```

     6)重启 webmin

     /etc/webmin/restart # 重启

     /etc/webmin/start # 启动

     /etc/webmin/stop # 停止

     ```bash
     [root@alanCentos01 webmin]# /etc/webmin/restart
     Stopping Webmin server in /usr/libexec/webmin
     Starting Webmin server in /usr/libexec/webmin
     Pre-loaded WebminCore
     [root@alanCentos01 webmin]# /etc/webmin/stop
     Stopping Webmin server in /usr/libexec/webmin
     [root@alanCentos01 webmin]# /etc/webmin/start
     Starting Webmin server in /usr/libexec/webmin
     Pre-loaded WebminCore
     [root@alanCentos01 webmin]# 
     ```

     7)防火墙放开 6868 端口

     firewall-cmd --zone=public --add-port=6868/tcp --permanent # 配置防火墙开放 6868端口

     firewall-cmd --reload # 更新防火墙配置

     firewall-cmd --zone=public --list-ports # 查看已经开放的端口号

     ```bash
     [root@alanCentos01 webmin]# firewall-cmd --zone=public --add-port=6868/tcp --permanent
     success
     [root@alanCentos01 webmin]# firewall-cmd --reload
     success
     [root@alanCentos01 webmin]# firewall-cmd --zone=public --list-ports
     8080/tcp 6868/tcp
     [root@alanCentos01 webmin]# 
     ```

     8)登录 webmin

     http://ip:6868 可以访问了

     用 root 账号和重置的新密码 test
   
2. bt(宝塔)

   * 基本介绍

     bt 宝塔 Linux 面板是提升运维效率的服务器管理软件，支持一键 LAMP/LNMP/集群/监控/网站/FTP/数据库/JAVA 等多项服务器管理功能。

   * 安装和使用

     1)安装 : yum install -y wget && wget -O install.sh http://download.bt.cn/install/install_6.0.sh && sh install.sh==（如果已经安装wget可以去掉前面的 yum install -y wget &&）==

     2)安装成功后控制台会显示登录地址，账户密码，复制浏览器打开登录

     ![image-20210506233925334](assets/CentOS7.6详细说明/image-20210506233925334.png)

     3)如果 bt 的用户名，密码忘记了，使用 bt default 可以查看

     ```bash
     [root@alanCentos01 bt]# bt default
     ==================================================================
     BT-Panel default info!
     ==================================================================
     外网面板地址: http://106.7.162.209:8888/39719663
     内网面板地址: http://192.168.18.7:8888/39719663
     *以下仅为初始默认账户密码，若无法登录请执行bt命令重置账户/密码登录
     username: ryw01nza
     password: 8fd34f5a
     If you cannot access the panel,
     release the following panel port [8888] in the security group
     若无法访问面板，请检查防火墙/安全组是否有放行面板[8888]端口
     ==================================================================
     ```


### 二十七、Linux应用相关

1. 分析日志 t.log(访问量)，将各个 ip 地址截取，并统计出现次数,并按从大到小排序。

   http://192.168.200.10/index1.html
   http://192.168.200.10/index2.html
   http://192.168.200.20/index1.html
   http://192.168.200.30/index1.html
   http://192.168.200.40/index1.html
   http://192.168.200.30/order.html
   http://192.168.200.10/order.html
   答案: cat t.log| cut -d '/' -f 3| sort | uniq -c | sort -nr

   ```bash
   [root@alanCentos01 home]# vim t.log
   [root@alanCentos01 home]# cat t.log
   http://192.168.200.10/index1.html
   http://192.168.200.10/index2.html
   http://192.168.200.20/index1.html
   http://192.168.200.30/index1.html
   http://192.168.200.40/index1.html
   http://192.168.200.30/order.html
   http://192.168.200.10/order.html
   [root@alanCentos01 home]# cat t.log | cut -d '/' -f 3 | sort | uniq -c | sort -nr
         3 192.168.200.10
         2 192.168.200.30
         1 192.168.200.40
         1 192.168.200.20
   [root@alanCentos01 home]#
   ```

2. 统计连接到服务器的各个 ip 情况，并按连接数从大到小排序。

   ```bash
   [root@alanCentos01 home]# netstat -an| grep ESTABLISHED | awk -F " " '{print $5}' | cut -d ":" -f 1 | sort | uniq -c| sort -nr
         1 192.168.18.1
   [root@alanCentos01 home]#
   ```

3. 写出指令：统计 ip 访问情况，要求分析 nginx 访问日志(access.log)，找出访问页面数量在前 2 位的 ip。

   ```bash
   [root@alanCentos01 home]# cat access.log| awk -F " " '{print $1}' | sort | uniq -c | sort -nr | head -2
   ```

4. 使用 tcpdump 监听本机, 将来自 ip 192.168.18.1，tcp 端口为 22 的数据，保存输出到tcpdump.log , 用做将来数据分析。

   ```bash
   [root@alanCentos01 home]# tcpdump -i ens33 host 192.168.18.1 and port 22 >> /home/tcpdump.log
   tcpdump: verbose output suppressed, use -v or -vv for full protocol decode
   listening on ens33, link-type EN10MB (Ethernet), capture size 262144 bytes
   ```

5. 在进行 Linux 系统权限划分时,应考虑哪些因素?

   1)Linux 权限的主要对象

   ![image-20210509200048568](assets/CentOS7.6详细说明/image-20210509200048568.png)

   ![image-20210509200204855](assets/CentOS7.6详细说明/image-20210509200204855.png)

   2)根据自己实际经验谈考虑因素

   * 注意权限分离，比如: 工作中，Linux 系统权限和数据库权限不要在同一个部门；

   * 权限最小原则(即:在满足使用的情况下最少优先)；

   * 减少使用 root 用户，尽量用普通用户+sudo 提权进行日常操作；

   * 重要的系统文件，比如/etc/passwd，/etc/shadowetc/fstab，/etc/sudoers 等，日常建议使用 chattr(change attribute)锁定，需要操作时再打开【比如:锁定 /etc/passwd 让任何用户都不能随意 useradd，除非解除锁定】；

   * 使用 SUID, SGID, Sticky 设置特殊权限；

   * 可以利用工具，比如 chkrootkit/rootkit hunter 检测 rootkit 脚本（rootkit 是入侵者使用工具,在不察觉的建立了入侵系统途径） 【使用wgetftp://ftp.pangeia.com.br/pub/seg/pac/chkrootkit.tar.gz】；

   * 利用工具 Tripwire 检测文件系统完整性

   ![image-20210509200611309](assets/CentOS7.6详细说明/image-20210509200611309.png)

6. 权限操作思考

   * 用户 tom 对目录 /home/test 有执行 x 和读 r 写 w 权限，/home/test/hello.java 是只读文件，问 tom 对 hello.java 文件能读吗(ok)？能修改吗(no)？能删除吗？(ok)
   * 用户tom 对目录 /home/test 只有读写权限，/home/test/hello.java 是只读文件，问tom对 hello.java文件能读吗(no)？能修改吗(no)？能删除吗(no)？
   * 用户 tom 对目录 /home/test 只有执行权限 x，/home/test/hello.java 是只读文件，问 tom 对 hello.java 文件能读吗(ok)？能修改吗(no)？能删除吗(no)？
   * 用户 tom 对目录 /home/test 只有执行和写权限，/home/test/hello.java 是只读文件，问 tom 对 hello.java 文件能读吗(ok)？能修改吗(no)？能删除(ok)？

7. 列举 Linux 高级命令，至少 6 个

   netstat //网络状态监控

   top //系统运行状态

   lsblk //查看硬盘分区

   find

   ps -aux //查看运行进程

   chkconfig //查看服务启动状态

   systemctl //管理系统服务器

8. Linux 查看内存、io 读写、磁盘存储、端口占用、进程查看命令是什么？

   top, iotop, df -lh , netstat -tunlp , ps -aux | grep 关心的进程

9. 使用 Linux 命令计算 t2.txt 第二列的和并输出。

   ```bash
   [root@alanCentos01 home]# vim t2.txt
   [root@alanCentos01 home]# cat t2.txt | awk -F " " '{sum+=$2} END {print sum}'
   150
   ```

10. Shell 脚本里如何检查一个文件是否存在？并给出提示。

    if [ -f 文件名 ] then echo “存在” else echo “不存在” fi

11. 用 shell 写一个脚本，对文本 t3.txt 中无序的一列数字排序, 并将总和输出。

    9

    8

    7

    6

    5

    4

    3

    2

    10

    ```bash
    [root@alanCentos01 home]# vim t3.txt
    [root@alanCentos01 home]# sort -nr t3.txt | awk '{sum+=$0; print $0} END {print "和="sum}'
    10
    9
    8
    7
    6
    5
    4
    3
    2
    和=54
    [root@alanCentos01 home]#
    ```

12. 请用指令写出查找当前文件夹（/home）下所有的文本文件内容中包含有字符 “cat”的文件名称。

    ```bash
    [root@alanCentos01 home]# grep -r "cat" /home |cut -d ":" -f 1
    /home/IdeaProjects/test/.idea/workspace.xml
    /home/IdeaProjects/test/.idea/workspace.xml
    /home/IdeaProjects/test/.idea/workspace.xml
    /home/IdeaProjects/test/.idea/workspace.xml
    /home/var.sh
    [root@alanCentos01 home]#
    ```

13. 请写出统计/home 目录下所有文件个数和所有文件总行数的指令。

    ```bash
    [root@alanCentos01 home]# find /home -name "*.*"| wc -l
    49
    [root@alanCentos01 home]# find /home -name "*.*"| xargs wc -l
         12 /home/info.txt
          7 /home/IdeaProjects/test/out/production/hello/com/test/Hello.class
    wc: /home/IdeaProjects/test/.idea: 是一个目录
          0 /home/IdeaProjects/test/.idea
          5 /home/IdeaProjects/test/.idea/misc.xml
         73 /home/IdeaProjects/test/.idea/workspace.xml
          7 /home/IdeaProjects/test/.idea/modules.xml
          0 /home/IdeaProjects/test/.idea/.name
          8 /home/IdeaProjects/test/.idea/.gitignore
          8 /home/IdeaProjects/test/src/com/test/Hello.java
         10 /home/IdeaProjects/test/hello.iml
         14 /home/getSum.sh
          3 /home/cat.txt
         11 /home/testWhile.sh
          7 /home/testRead.sh
          3 /home/t2.txt
          5 /home/position.sh
          0 /home/orange.txt
          0 /home/apple.txt
          3 /home/dog.txt
         11 /home/zwj/.bashrc
         12 /home/zwj/.bash_profile
    wc: /home/zwj/.mozilla: 是一个目录
          0 /home/zwj/.mozilla
          2 /home/zwj/.bash_logout
         13 /home/testFor1.sh
         12 /home/tcpdump.log
         11 /home/fox/.bashrc
         12 /home/fox/.bash_profile
    wc: /home/fox/.mozilla: 是一个目录
          0 /home/fox/.mozilla
          2 /home/fox/.bash_logout
          5 /home/myshell.sh
          8 /home/Hello.java
          9 /home/ifCase.sh
         11 /home/alan/.bashrc
    wc: /home/alan/.config: 是一个目录
          0 /home/alan/.config
         12 /home/alan/.bash_profile
         23 /home/alan/.bash_history
    wc: /home/alan/.mozilla: 是一个目录
          0 /home/alan/.mozilla
    wc: /home/alan/.cache: 是一个目录
          0 /home/alan/.cache
          2 /home/alan/.bash_logout
          1 /home/alan/.Xauthority
          7 /home/t.log
         21 /home/ifdemo.sh
         13 /home/testCase.sh
          6 /home/preVar.sh
         23 /home/var.sh
          8 /home/Hello.class
          9 /home/t3.txt
          1 /home/hello.txt
         10 /home/testFor2.sh
        420 总用量
    [root@alanCentos01 home]# 
    ```

14. 18每天晚上 8 点 30 分，打包站点目录/var/spool/mail 备份到/home 目录下（每次备份按时间生成不同的备份包 比如按照 年月日时分秒）

    ![image-20210509202645103](assets/CentOS7.6详细说明/image-20210509202645103.png)

    ![image-20210509202714563](assets/CentOS7.6详细说明/image-20210509202714563.png)

15. 如何优化 Linux 系统？

    (1) 不用 root ,使用 sudo 提示权限

    (2) 定时的自动更新服务时间,使用 nptdate npt1.aliyun.com , 让 croud 定时更新

    (3) 配置 yum 源，指向国内镜像(清华，163)

    (4) 配置合理的防火墙策略,打开必要的端口，关闭不必要的端口

    (5) 打开最大文件数(调整文件的描述的数量) vim /etc/profile ulimit -SHn 65535

    (6) 配置合理的监控策略

    (7) 配置合理的系统重要文件的备份策略

    (8) 对安装的软件进行优化，比如 nginx ,apache

    (9) 内核参数进行优化 /etc/sysctl.conf

    (10) 锁定一些重要的系统文件 chattr /etc/passwd /ect/shadow /etc/inittab

    (11) 禁用不必要的服务 setup , ntsysv

### 二十八、Linux虚拟机(VM)扩容

1. VM上修改磁盘信息，将虚拟机关机。

   ![image-20210509230340098](assets/CentOS7.6详细说明/image-20210509230340098.png)

   之后会收到提示：

   ![image-20210509230409586](assets/CentOS7.6详细说明/image-20210509230409586.png)

2. 开启虚拟机并登录后，使用命令查看当磁盘状态，可看到当前还是原本的20G，并未扩容。

   ```bash
   [root@alanCentos01 ~]# df -h
   文件系统        容量  已用  可用 已用% 挂载点
   /dev/sda3        17G   13G  3.7G   78% /
   devtmpfs        895M     0  895M    0% /dev
   tmpfs           910M     0  910M    0% /dev/shm
   tmpfs           910M   11M  900M    2% /run
   tmpfs           910M     0  910M    0% /sys/fs/cgroup
   /dev/sda1       976M  144M  766M   16% /boot
   .host:/         601G  400G  201G   67% /mnt/hgfs
   tmpfs           182M  8.0K  182M    1% /run/user/42
   tmpfs           182M     0  182M    0% /run/user/0
   [root@alanCentos01 ~]#
   ```

3. 查看到新磁盘的分区

   ```bash
   [root@alanCentos01 ~]# fdisk -l
   
   磁盘 /dev/sda：53.7 GB, 53687091200 字节，104857600 个扇区
   Units = 扇区 of 1 * 512 = 512 bytes
   扇区大小(逻辑/物理)：512 字节 / 512 字节
   I/O 大小(最小/最佳)：512 字节 / 512 字节
   磁盘标签类型：dos
   磁盘标识符：0x000ce340
   
      设备 Boot      Start         End      Blocks   Id  System
   /dev/sda1   *        2048     2099199     1048576   83  Linux
   /dev/sda2         2099200     6293503     2097152   82  Linux swap / Solaris
   /dev/sda3         6293504    41936895    17821696   83  Linux
   [root@alanCentos01 ~]# 
   ```

4. 然后对新加的磁盘进行分区操作，==记住根分区起始位置和结束位置==

   ![image-20210509225240775](assets/CentOS7.6详细说明/image-20210509225240775.png)

5. 删除根分区，==切记不要保存==

   ![image-20210509225323631](assets/CentOS7.6详细说明/image-20210509225323631.png)

6. 创建分区，箭头位置为分区起始位置

   ![image-20210509230900174](assets/CentOS7.6详细说明/image-20210509230900174.png)

7. 保存退出

   ![image-20210509225540710](assets/CentOS7.6详细说明/image-20210509225540710.png)

8. 刷新分区

   ```bash
   [root@alanCentos01 ~]# partprobe /dev/sda
   ```

9. 查看分区状态

   ```bash
   [root@alanCentos01 ~]# lsblk
   NAME   MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
   sda      8:0    0   50G  0 disk 
   ├─sda1   8:1    0    1G  0 part /boot
   ├─sda2   8:2    0    2G  0 part [SWAP]
   └─sda3   8:3    0   47G  0 part /
   sr0     11:0    1  4.3G  0 rom  /run/media/root/CentOS 7 x86_64
   [root@alanCentos01 ~]# df -h
   文件系统        容量  已用  可用 已用% 挂载点
   /dev/sda3        17G   13G  3.7G   78% /
   devtmpfs        895M     0  895M    0% /dev
   tmpfs           910M     0  910M    0% /dev/shm
   tmpfs           910M   11M  900M    2% /run
   tmpfs           910M     0  910M    0% /sys/fs/cgroup
   /dev/sda1       976M  144M  766M   16% /boot
   .host:/         601G  400G  201G   67% /mnt/hgfs
   tmpfs           182M  4.0K  182M    1% /run/user/42
   tmpfs           182M   28K  182M    1% /run/user/0
   /dev/sr0        4.3G  4.3G     0  100% /run/media/root/CentOS 7 x86_64
   ```

10. 刷新根分区并查看状态

    ```bash
    [root@alanCentos01 ~]# resize2fs /dev/sda3
    resize2fs 1.42.9 (28-Dec-2013)
    Filesystem at /dev/sda3 is mounted on /; on-line resizing required
    old_desc_blocks = 3, new_desc_blocks = 6
    The filesystem on /dev/sda3 is now 12320512 blocks long.
    
    [root@alanCentos01 ~]# df -h
    文件系统        容量  已用  可用 已用% 挂载点
    /dev/sda3        47G   13G   32G   28% /
    devtmpfs        895M     0  895M    0% /dev
    tmpfs           910M     0  910M    0% /dev/shm
    tmpfs           910M   11M  900M    2% /run
    tmpfs           910M     0  910M    0% /sys/fs/cgroup
    /dev/sda1       976M  144M  766M   16% /boot
    .host:/         601G  400G  201G   67% /mnt/hgfs
    tmpfs           182M  4.0K  182M    1% /run/user/42
    tmpfs           182M   28K  182M    1% /run/user/0
    /dev/sr0        4.3G  4.3G     0  100% /run/media/root/CentOS 7 x86_64
    [root@alanCentos01 ~]# 
    ```

    

