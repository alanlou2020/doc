#### Redis详细说明

##### 1、Linux安装Redis

###### 1.1、官网下载：https://www.redis.io/

###### 1.2、安装编译环境、解压、编译：

```shell
yum install gcc-c++ 
tar -zxvf redis-6.2.5.tar.gz
cd redis-6.2.5
make
```

###### 1.3、安装编译完成的redis：

```shell
make install
```

> 安装完成后，默认的安装路径为：`/usr/local/bin`

###### 1.4、复制配置文件到默认的安装路径`/usr/local/bin`：

```shell
mkdir /usr/local/bin/config
cp /opt/redis/redis-6.2.5/redis.conf /usr/local/bin/config
```

###### 1.5、修改配置文件：服务以守护进程的方式启动

```properties
# daemonize no
# 服务以守护进程的方式启动（后台启动）
daemonize yes
# 生产环境注释 bind 127.0.0.1 -::1 ，并把protected-mode改为no，否则其他机器无法连接该服务器
# bind 127.0.0.1 -::1
protected-mode no
# 配置数据保存点
save 3600 1
save 300 100
save 60 10000
```

###### 1.6、启动服务：

```shell
[root@alanCentos01 bin]# pwd
/usr/local/bin
[root@alanCentos01 bin]# ./redis-server config/redis.conf
[root@alanCentos01 bin]# ps -ef | grep redis
root      31393      1  0 16:54 ?        00:00:00 ./redis-server 127.0.0.1:6379
root      31464  19954  0 16:55 pts/0    00:00:00 grep --color=auto redis
```

###### 1.7、测试连接：

```shell
# 如果不加--raw参数，中文显示会乱码
[root@alanCentos01 bin]# ./redis-cli --raw -h 127.0.0.1 -p 6379
127.0.0.1:6379> ping
PONG
127.0.0.1:6379> set name alan
OK
127.0.0.1:6379> get name
"alan"
127.0.0.1:6379> keys *
1) "name"
127.0.0.1:6379> shutdown
not connected> exit
[root@alanCentos01 bin]#
```

> ==注意：shutdown命令是关闭停止redis服务==

##### 2、性能（压力）测试：

```bash
# 测试100个并发连接、100000个请求
[root@alanCentos01 bin]# ./redis-benchmark -h 127.0.0.1 -p 6379 -c 100 -n 100000
```

##### 3、常用命令：

###### 3.1、选取数据库（SELECT）

```bash
localhost:6379> SELECT 3
OK
localhost:6379[3]> SELECT 0
OK
```

###### 3.2、查看数据库大小（DBSIZE）

```bash
localhost:6379> DBSIZE
(integer) 5
```

###### 3.3、查看当前数据库中所有的键（KEYS *）

```bash
localhost:6379> KEYS *
1) "mylist"
2) "counter:__rand_int__"
3) "name"
4) "key:__rand_int__"
5) "myhash"
```

###### 3.4、清空当前数据库（FLUSHDB）

```bash
localhost:6379> FLUSHDB
OK
localhost:6379> KEYS *
(empty array)
localhost:6379> GET name
(nil)
```

###### 3.5、清空所有数据库（FLUSHALL）

```bash
localhost:6379> FLUSHALL
OK
```

###### 3.6、判断键值对是否存在(EXISTS：返回1存在、0不存在)

```bash
localhost:6379> EXISTS name
(integer) 1
localhost:6379> EXISTS name1
(integer) 0
```

###### 3.7、移除键值对（MOVE：1是指当前数据库）

```bash
localhost:6379> MOVE name 1
(integer) 1
localhost:6379> EXISTS name
(integer) 0
```

###### 3.8、设置过期时间（EXPIRE：设置键为name的键值对10秒过期）

```bash
localhost:6379> EXPIRE name 10
(integer) 1
```

###### 3.9、查看距离过期时间剩余的秒数（TTL：-2代表已经过期失效）

```bash
localhost:6379> TTL name
(integer) -2
localhost:6379> GET name
(nil)
```

###### 3.10、查看键对应值得数据类型（TYPE）

```bash
localhost:6379> TYPE name
string
localhost:6379> TYPE age
string
```

##### 4、String类型

###### 4.1、追加（APPEND）

```bash
localhost:6379> set key1 v1
OK
# 追加
localhost:6379> APPEND key1 "hello"
(integer) 7
localhost:6379> GET key1
"v1hello"
```

###### 4.2、获取字符串长度（STRLEN）

```bash
localhost:6379> STRLEN key1
(integer) 7
```

###### 4.3、数值增长（INCR）

```bash
localhost:6379> SET views 0
OK
localhost:6379> KEYS *
1) "key1"
2) "views"
localhost:6379> GET views
"0"
# 数值增长1
localhost:6379> INCR views
(integer) 1
localhost:6379> GET views
"1"
# 数值增长1
localhost:6379> INCR views
(integer) 2
localhost:6379> GET views
"2"
```

###### 4.4、数值减（DECR）

```bash
# 数值减1
localhost:6379> DECR views
(integer) 1
localhost:6379> GET views
"1"
# 数值减1
localhost:6379> DECR views
(integer) 0
localhost:6379> GET views
"0"
```

###### 4.5、自定义步长增减（INCRBY、DECRBY）

```bash
localhost:6379> GET views
"0"
# 增长（步长为10）
localhost:6379> INCRBY views 10
(integer) 10
localhost:6379> GET views
"10"
# 减（步长为10）
localhost:6379> DECRBY views 10
(integer) 0
localhost:6379> GET views
"0"
# 减（步长为10）
localhost:6379> DECRBY views 10
(integer) -10
localhost:6379> GET views
"-10"
```

###### 4.6、字符串范围（GETRANGE）

```bash
localhost:6379> KEYS *
(empty array)
localhost:6379> SET key1 hello,alan
OK
localhost:6379> GET key1
"hello,alan"
localhost:6379> GETRANGE key1 3 8
"lo,ala"
# -1指到结尾
localhost:6379> GETRANGE key1 0 -1
"hello,alan"
localhost:6379> GETRANGE key1 3 100
"lo,alan"
localhost:6379> GETRANGE key1 1 100
"ello,alan"
localhost:6379> GETRANGE key1 0 100
"hello,alan"
localhost:6379> GETRANGE key1 -1 100
"n"
localhost:6379> GETRANGE key1 -2 100
"an"
```

###### 4.7、修改字符串（指定范围：SETRANGE）

```bash
localhost:6379> SET key2 abcdefg
OK
localhost:6379> GET key2
"abcdefg"
# 指定修改范围（偏移量：1，指从1开始修改，666长度为3，则替换1后面的3个字符为666；注：字符串从0开始计数）
localhost:6379> SETRANGE key2 1 666
(integer) 7
localhost:6379> GET key2
"a666efg"
```

###### 4.8、设置键值及过期时间（SETEX）

```bash
# 60为过期时间（秒）
localhost:6379> SETEX key3 60 hello
OK
localhost:6379> TTL key3
(integer) 10
localhost:6379> TTL key3
(integer) 9
localhost:6379> TTL key3
(integer) 8
```

###### 4.9、数据库中键不存在的情况下设置键值（SETNX：在分布式锁中会经常使用，可以去重）

```bash
localhost:6379> KEYS *
1) "key2"
2) "key1"
# 如果键不存在，则在当前库中新增这个键值对
localhost:6379> SETNX myKey redis
(integer) 1
localhost:6379> KEYS *
1) "key2"
2) "myKey"
3) "key1"
localhost:6379> GET myKey
"redis"
localhost:6379> GET key1
"hello,alan"
localhost:6379> SETNX key1 aaa
(integer) 0
localhost:6379> GET key1
"hello,alan"
```

###### 4.10、批量设置多个键值对（MSET）及批量获取（MGET）

```bash
localhost:6379> MSET k1 v1 k2 v2 k3 v3
OK
localhost:6379> MGET k1 k2 k3
1) "v1"
2) "v2"
3) "v3"
localhost:6379> MSET person:1:name "zhangsan" person:1:age 18 
OK
localhost:6379> MGET person:1:name person:1:age
1) "zhangsan"
2) "18"
```

###### 4.11、批量设置不存在的键值对（MSETNX）

```bash
localhost:6379> MGET k1 k2 k3 k4
1) "v1"
2) "v2"
3) "v3"
4) (nil)
# MSETNX中只要有一个键在数据库中是存在的，则整条命令都不能成功，无论其他键是不是存在
localhost:6379> MSETNX k1 vv1 k2 vv2 k3 vv3 k4 vv4
(integer) 0
localhost:6379> MGET k1 k2 k3 k4 k5
1) "v1"
2) "v2"
3) "v3"
4) (nil)
5) (nil)
localhost:6379> MSETNX k4 vv4 k5 vv5
(integer) 1
localhost:6379> MGET k1 k2 k3 k4 k5
1) "v1"
2) "v2"
3) "v3"
4) "vv4"
5) "vv5"
```

###### 4.12、获取设置键值对组合命令（getset）

```bash
localhost:6379> GET key1
"hello,alan"
# 先获取键key1的值返回，再给key1设置新的值
localhost:6379> GETSET key1 "hi,jerry"
"hello,alan"
localhost:6379> GET key1
"hi,jerry"
localhost:6379> GET key6
(nil)
localhost:6379> GETSET key6 "hello"
(nil)
localhost:6379> GET key6
"hello"
```

###### 4.13、删除键值对（DEL）

```bash
localhost:6379> KEYS *
 1) "key6"
 2) "k4"
 3) "key1"
 4) "k5"
 5) "person:1:age"
 6) "k2"
 7) "key2"
 8) "person:2:age"
 9) "myKey"
10) "k1"
11) "k3"
12) "person:1:name"
# 删除键为k4的键值对
localhost:6379> DEL k4
(integer) 1
localhost:6379> KEYS *
 1) "key6"
 2) "key1"
 3) "k5"
 4) "person:1:age"
 5) "k2"
 6) "key2"
 7) "person:2:age"
 8) "myKey"
 9) "k1"
10) "k3"
11) "person:1:name"
localhost:6379> 
```

##### 5、List类型

###### 5.1、定义List并从左边插入一个值（LPUSH：从左边插入）

```bash
localhost:6379> FLUSHDB
OK
localhost:6379> KEYS *
(empty array)
# 定义一个名为list1的List，并插入值"one"
localhost:6379> LPUSH list1 "one"
(integer) 1
localhost:6379> LPUSH list1 "two"
(integer) 2
localhost:6379> LPUSH list1 "three"
(integer) 3
localhost:6379> LRANGE list1 0 -1
1) "three"
2) "two"
3) "one"
```

###### 5.2、获取List中的值（LRANGE：从左边开始，索引从0开始）

```bash
localhost:6379> LRANGE list1 0 0
1) "three"
localhost:6379> LRANGE list1 0 1
1) "three"
2) "two"
# 这里-1是指最后一个
localhost:6379> LRANGE list1 0 -1
1) "three"
2) "two"
3) "one"
```

###### 5.3、定义List并从右边插入一个值（RPUSH：从右边插入）

```bash
localhost:6379> RPUSH list1 four
(integer) 4
localhost:6379> RPUSH list1 five
(integer) 5
# 注：没有RRANGE这个指令哈
localhost:6379> LRANGE list1 0 -1
1) "three"
2) "two"
3) "one"
4) "four"
5) "five"
```

###### 5.4、从List左边移除（LPOP：从左边移除）

```bash
localhost:6379> LRANGE list1 0 -1
1) "three"
2) "two"
3) "one"
4) "four"
5) "five"
# 从list1左边移除1个
localhost:6379> LPOP list1 1
1) "three"
localhost:6379> LRANGE list1 0 -1
1) "two"
2) "one"
3) "four"
4) "five"
# 从list1左边移除2个
localhost:6379> LPOP list1 2
1) "two"
2) "one"
localhost:6379> LRANGE list1 0 -1
1) "four"
2) "five"
# 不谢数量则默认移除1个
localhost:6379> LPOP list1
"four"
localhost:6379> LRANGE list1 0 -1
1) "five"
```

###### 5.4、从List右边移除（RPOP：从右边移除）

```bash
localhost:6379> RPOP list1
"five"
localhost:6379> LRANGE list1 0 -1
(empty array)
```

###### 5.5、通过List索引获取对应的值（LINDEX：只是取值，并不会从List中移除）

```bash
localhost:6379> LPUSH list1 "one"
(integer) 1
localhost:6379> LPUSH list1 "two"
(integer) 2
localhost:6379> LPUSH list1 "three"
(integer) 3
localhost:6379> LPUSH list1 "five"
(integer) 4
localhost:6379> LPUSH list1 "six"
(integer) 5
localhost:6379> LRANGE list1 0 -1
1) "six"
2) "five"
3) "three"
4) "two"
5) "one"
localhost:6379> LINDEX list1 0
"six"
localhost:6379> LINDEX list1 1
"five"
localhost:6379> LINDEX list1 3
"two"
localhost:6379> LRANGE list1 0 -1
1) "six"
2) "five"
3) "three"
4) "two"
5) "one"
localhost:6379> 
```

###### 5.6、获取List长度（LLEN）

```bash
localhost:6379> LLEN list1
(integer) 5
```

###### 5.7、移除指定的值（LREM）

```bash
localhost:6379> LRANGE list1 0 -1
1) "one"
2) "six"
3) "five"
4) "three"
5) "two"
6) "one"
localhost:6379> LREM list1 2 one
(integer) 2
localhost:6379> LRANGE list1 0 -1
1) "six"
2) "five"
3) "three"
4) "two"
localhost:6379> LREM list1 3 one
(integer) 0
localhost:6379> LRANGE list1 0 -1
1) "six"
2) "five"
3) "three"
4) "three"
5) "three"
# 0表示从List中删除所有的指定值
localhost:6379> LREM list1 0 three
(integer) 3
localhost:6379> LRANGE list1 0 -1
1) "six"
2) "five"
```

###### 5.8、截取List（LTRIM：通过下标从左开始截取指定范围内的List）

```bash
localhost:6379> LRANGE myList 0 -1
1) "hello"
2) "hello1"
3) "hello2"
4) "hello3"
localhost:6379> LTRIM myList 1 2
OK
localhost:6379> LRANGE myList 0 -1
1) "hello1"
2) "hello2"
```

###### 5.9、组合命令（RPOPLPUSH）从当前List尾部删除一个值，并添加（不存在的话创建）到另一个List中

```bash
localhost:6379> KEYS *
(empty array)
localhost:6379> RPUSH myList "hello"
(integer) 1
localhost:6379> RPUSH myList "hello1"
(integer) 2
localhost:6379> RPUSH myList "hello2"
(integer) 3
localhost:6379> LRANGE myList 0 -1
1) "hello"
2) "hello1"
3) "hello2"
localhost:6379> RPOPLPUSH myList newList
"hello2"
localhost:6379> KEYS *
1) "newList"
2) "myList"
localhost:6379> LRANGE newList 0 -1
1) "hello2"
localhost:6379> LRANGE myList 0 -1
1) "hello"
2) "hello1"
localhost:6379> RPOPLPUSH myList newList
"hello1"
localhost:6379> LRANGE newList 0 -1
1) "hello1"
2) "hello2"
localhost:6379> LRANGE myList 0 -1
1) "hello"
```

###### 5.10、判断List是否存在（EXISTS）

```bash
localhost:6379> KEYS *
1) "newList"
2) "myList"
localhost:6379> EXISTS list1
(integer) 0
localhost:6379> EXISTS myList
(integer) 1
localhost:6379> EXISTS newList
(integer) 1
```

###### 5.11、在指定List的下标位置插入（LSET）

```bash
localhost:6379> LRANGE list1 0 -1
1) "one"
2) "two"
3) "three"
# 在index为1的位置插入
localhost:6379> LSET list1 1 "aaa"
OK
localhost:6379> LRANGE list1 0 -1
1) "one"
2) "aaa"
3) "three"
localhost:6379> KEYS *
1) "list1"
# 如果数据中不存在指定的List，就会报错“ERR no such key”
localhost:6379> LSET list2 0 "bbb"
(error) ERR no such key
# 超出索引长度也会报错
localhost:6379> LSET list1 3 "bbb"
(error) ERR index out of range
```

###### 5.12、在指定的List的值得前面或后面插入一个值（LINSERT）

```bash
localhost:6379> KEYS *
(empty array)
localhost:6379> RPUSH list1 "hello"
(integer) 1
localhost:6379> RPUSH list1 "world"
(integer) 2
localhost:6379> LRANGE list1 0 -1
1) "hello"
2) "world"
localhost:6379> LINSERT list1 before "hello" "hi,"
(integer) 3
localhost:6379> LRANGE list1 0 -1
1) "hi,"
2) "hello"
3) "world"
localhost:6379> LINSERT list1 after "hello" "-"
(integer) 4
localhost:6379> LRANGE list1 0 -1
1) "hi,"
2) "hello"
3) "-"
4) "world"
# 返回-1表示插入失败，这里导致的原因是值"work"在"list1"中不存在
localhost:6379> LINSERT list1 after "work" "!"
(integer) -1
localhost:6379> LINSERT list1 after "world" "!"
(integer) 5
localhost:6379> LRANGE list1 0 -1
1) "hi,"
2) "hello"
3) "-"
4) "world"
5) "!"
```

##### 6、Set类型：Set中的值不能重复

###### 6.1、往Set集合中新增元素（SADD：集合不存在则自动创建）

```bash
localhost:6379> KEYS *
(empty array)
localhost:6379> SADD set1 "aaa"
(integer) 1
localhost:6379> SADD set1 6
(integer) 1
# Set中的值不能重复
localhost:6379> SADD set1 "aaa"
(integer) 0
localhost:6379> SMEMBERS set1
1) "aaa"
2) "6"
# 同时插入多个值
localhost:6379> SADD set3 "ccc" "eee"
(integer) 2
```

###### 6.2、查看Set集合中的所有元素（SMEMBERS）

```bash
localhost:6379> SMEMBERS set1
1) "aaa"
2) "6"
```

###### 6.3、判断元素在Set中是否存在（SISMEMBER）

```bash
# 返回1则表示存在
localhost:6379> SISMEMBER set1 "aaa"
(integer) 1
# 返回0则表示不存在
localhost:6379> SISMEMBER set1 "bbb"
(integer) 0
```

###### 6.4、获取Set中元素的个数（SCARD）

```bash
localhost:6379> SCARD set1
(integer) 2
```

###### 6.5、删除Set中的指定元素(SREM)

```bash
localhost:6379> SREM set1 "aaa"
(integer) 1
localhost:6379> SMEMBERS set1
1) "6"
localhost:6379> SREM set1 6
(integer) 1
localhost:6379> SMEMBERS set1
(empty array)
# 删除失败返回0（这里是删除了不存在的元素）
localhost:6379> SREM set1 "bbb"
(integer) 0
```

###### 6.6、从Set中随机取值（SRANDMEMBER）

```bash
localhost:6379> SCARD set1
(integer) 0
localhost:6379> SADD set1 "hello"
(integer) 1
localhost:6379> SADD set1 "jerry"
(integer) 1
localhost:6379> SADD set1 "aaa"
(integer) 1
localhost:6379> SADD set1 "bbb"
(integer) 1
localhost:6379> SADD set1 "ccc"
(integer) 1
# 在集合中随机取1个值
localhost:6379> SRANDMEMBER set1 1
1) "ccc"
# 在集合中随机取2个值
localhost:6379> SRANDMEMBER set1 2
1) "jerry"
2) "bbb"
localhost:6379> SRANDMEMBER set1 2
1) "bbb"
2) "hello"
```

###### 6.6、随机从Set中删除元素（SPOP）

```bash
localhost:6379> SMEMBERS set1
1) "aaa"
2) "jerry"
3) "ccc"
4) "bbb"
5) "hello"
localhost:6379> SPOP set1
"hello"
localhost:6379> SPOP set1 1
1) "aaa"
localhost:6379> SPOP set1 2
1) "bbb"
2) "ccc"
localhost:6379> SMEMBERS set1
1) "jerry"
```

###### 6.7、从Set中移动指定的元素到另一个Set（SMOVE：目标Set不存在则创建）

```bash
localhost:6379> KEYS *
(empty array)
localhost:6379> SADD set1 "hello"
(integer) 1
localhost:6379> SADD set1 "bbb"
(integer) 1
localhost:6379> SADD set1 "ccc"
(integer) 1
localhost:6379> KEYS *
1) "list1"
# 从list1中移动"hello"到list2（此处list2是移动时新创建的）
localhost:6379> SMOVE set1 set2 "hello"
(integer) 1
localhost:6379> KEYS *
1) "list1"
2) "list2
localhost:6379> SMEMBERS set1
1) "ccc"
2) "bbb"
localhost:6379> SMEMBERS set2
1) "hello"
localhost:6379> SMOVE set1 set2 "bbb"
(integer) 1
localhost:6379> SMEMBERS set1
1) "ccc"
localhost:6379> SMEMBERS set2
1) "bbb"
2) "hello"
```

###### 6.8、第一个Set与其他Set的差集（SDIFF：即从第一个Set中去除掉在后面Set中出现过的元素，返回剩下的）

```bash
localhost:6379> SMEMBERS set1
1) "ddd"
2) "ccc"
3) "aaa"
4) "bbb"
5) "eee"
localhost:6379> SMEMBERS set2
1) "ccc"
2) "fff"
3) "ggg"
4) "bbb"
localhost:6379> SMEMBERS set3
1) "eee"
2) "ccc"
# 以第一个集合为参照物，去除掉后面集合中出现过的元素
localhost:6379> SDIFF set1 set2
1) "eee"
2) "ddd"
3) "aaa"
localhost:6379> SDIFF set1 set2 set3
1) "ddd"
2) "aaa"
```

###### 6.9、第一个Set与其他Set的交集（SINTER：取所有集合中相同的元素）

```bash
localhost:6379> SMEMBERS set1
1) "ddd"
2) "ccc"
3) "aaa"
4) "bbb"
5) "eee"
localhost:6379> SMEMBERS set2
1) "ccc"
2) "fff"
3) "ggg"
4) "bbb"
localhost:6379> SMEMBERS set3
1) "eee"
2) "ccc"
# 取所有集合中相同的元素
localhost:6379> SINTER set1 set2
1) "ccc"
2) "bbb"
localhost:6379> SINTER set1 set2 set3
1) "ccc"
```

###### 6.10、多个Set的并集（SUNION）

```bash
localhost:6379> SMEMBERS set1
1) "ddd"
2) "ccc"
3) "aaa"
4) "bbb"
5) "eee"
localhost:6379> SMEMBERS set2
1) "ccc"
2) "fff"
3) "ggg"
4) "bbb"
localhost:6379> SMEMBERS set3
1) "eee"
2) "ccc"
localhost:6379> SUNION set1 set2
1) "ddd"
2) "fff"
3) "aaa"
4) "ggg"
5) "ccc"
6) "eee"
7) "bbb"
localhost:6379> SUNION set1 set2 set3
1) "ddd"
2) "fff"
3) "aaa"
4) "ggg"
5) "ccc"
6) "eee"
7) "bbb"
```

##### 7、Hash类型

###### 7.1、往Hash中添加元素（HSET）

```bash
localhost:6379> HSET hash1 "name" "alan"
(integer) 1
localhost:6379> HSET hash1 "tel" "138***"
(integer) 1
localhost:6379> HSET hash1 "address" "street18" "age" 25 "remark" "test"
(integer) 3
localhost:6379> HGET hash1 "name"
"alan"
localhost:6379> HGET hash1 "tel"
"138***"
# 如果Hash中field已存在，则会更新field对应的值，返回0
localhost:6379> HSET hash1 "name" "jerry"
(integer) 0
localhost:6379> HGET hash1 "name"
"jerry
```

###### 7.2、通过field获取Hash中的值（HGET）

```bash
localhost:6379> HGET hash1 "name"
"alan"
localhost:6379> HGET hash1 "tel"
"138***"
localhost:6379> HGET hash1 "address"
"street18"
localhost:6379> HGET hash1 "age"
"25"
localhost:6379> HGET hash1 "remark"
"test"
```

###### 7.3、批量添加Hash元素（HMSET：HSET也可以添加多个，但另个指令返回结果不同）

```bash
# 这里结果返回“OK”，HSET执行返回成功插入的元素个数
localhost:6379> HMSET hash1 "dept" "aaa" "height" "171"
OK
localhost:6379> HGET hash1 "dept"
"aaa"
localhost:6379> HGET hash1 "height"
"171"
# 如果Hash中field已存在，则会更新field对应的值
localhost:6379> HMSET hash1 "dept" "bbb" "height" "181"
OK
localhost:6379> HMGET hash1 "dept" "height"
1) "bbb"
2) "181"
```

###### 7.4、同时获取多个Hash中的值（HMGET）

```bash
localhost:6379> HMGET hash1 "dept" "height"
1) "aaa"
2) "171"
```

###### 7.5、获取Hash中所有的值（HGETALL：以键值对换行形式返回）

```bash
localhost:6379> HGETALL hash1
 1) "name"
 2) "jerry"
 3) "tel"
 4) "138***"
 5) "address"
 6) "street18"
 7) "age"
 8) "25"
 9) "remark"
10) "test"
11) "dept"
12) "bbb"
13) "height"
14) "181"
```

###### 7.5、通过field删除Hash中的元素（HDEL）

```bash
localhost:6379> HDEL hash1 "remark"
(integer) 1
localhost:6379> HGET hash1 "remark"
(nil)
localhost:6379> HGETALL hash1
 1) "name"
 2) "jerry"
 3) "tel"
 4) "138***"
 5) "address"
 6) "street18"
 7) "age"
 8) "25"
 9) "dept"
10) "bbb"
11) "height"
12) "181"
```

###### 7.6、获取Hash的长度（HLEN）

```bash
localhost:6379> HGETALL hash1
 1) "name"
 2) "jerry"
 3) "tel"
 4) "138***"
 5) "address"
 6) "street18"
 7) "age"
 8) "25"
 9) "dept"
10) "bbb"
11) "height"
12) "181"
localhost:6379> HLEN hash1
(integer) 6
```

###### 7.7、判断指定的field在Hash中是否存在（HEXISTS）

```bash
# 存在返回1
localhost:6379> HEXISTS hash1 name
(integer) 1
# 不存在返回0
localhost:6379> HEXISTS hash1 remark
(integer) 0
```

###### 7.8、获取Hash中所有的field（HKEYS）

```bash
localhost:6379> HKEYS hash1
1) "name"
2) "tel"
3) "address"
4) "age"
5) "dept"
6) "height"
```

###### 7.9、获取Hash中所有的value（HVALS）

```bash
localhost:6379> HVALS hash1
1) "jerry"
2) "138***"
3) "street18"
4) "25"
5) "bbb"
6) "181"
```

###### 7.10、Hash按指定步长值增长（HINCRBY）

> ==注：Hash类型没有专门的按步长减的指令，如果要按字长减，则用负数==

```bash
localhost:6379> HINCRBY hash1 age 1
(integer) 26
# 注：该指令只能用于整数
localhost:6379> HINCRBY hash1 name 2
(error) ERR hash value is not an integer
# 注：Hash类型没有专门的按步长减的指令，如果要按字长减，则用负数
localhost:6379> HINCRBY hash1 age -2
(integer) 24
localhost:6379> HINCRBY hash1 age -1
(integer) 23
```

###### 7.11、Hash中判断如果不存在则新增键值对（HSETNX）

```bash
localhost:6379> HKEYS hash1
1) "name"
2) "tel"
3) "address"
4) "age"
5) "dept"
6) "height"
localhost:6379> HGET hash1 "name"
"jerry"
# 如果存在则不能更新键值对的值
localhost:6379> HSETNX hash1 "name" "kelly"
(integer) 0
localhost:6379> HGET hash1 "name"
"jerry"
# 如果不存在则新增键值对
localhost:6379> HSETNX hash1 "memo" "beizhu"
(integer) 1
localhost:6379> HGET hash1 "memo"
"beizhu"
localhost:6379> HKEYS hash1
1) "name"
2) "tel"
3) "address"
4) "age"
5) "dept"
6) "height"
7) "memo"
```

##### 8、Zset有序集合

> 说明：在set的基础上，加了一个score属性（二维）。

###### 8.1、Zset新增（ZADD）

```bash
localhost:6379> ZADD zset1 1 "one"
(integer) 1
localhost:6379> ZADD zset1 2 "two"
(integer) 1
localhost:6379> ZADD zset1 3 "three" 4 "four" 5 "five"
(integer) 3
localhost:6379> ZRANGE zset1 0 -1
1) "one"
2) "two"
3) "three"
4) "four"
5) "five"
```

###### 8.2、获取Zset中的值（ZRANGE）

```bash
localhost:6379> ZRANGE zset1 0 -1
1) "one"
2) "two"
3) "three"
4) "four"
5) "five"
```

###### 8.3、Zset按score升序排序（ZRANGEBYSCORE）

```bash
localhost:6379> ZADD salary 20000 xiaohong
(integer) 1
localhost:6379> ZADD salary 30000 zhangsan
(integer) 1
localhost:6379> ZADD salary 16000 jerry
(integer) 1
# -inf表示负无穷，+inf表示正无穷
localhost:6379> ZRANGEBYSCORE salary -inf +inf
1) "jerry"
2) "xiaohong"
3) "zhangsan"
# withscores表示显示score的值
localhost:6379> ZRANGEBYSCORE salary -inf +inf withscores
1) "jerry"
2) "16000"
3) "xiaohong"
4) "20000"
5) "zhangsan"
6) "30000"
localhost:6379> ZRANGEBYSCORE salary -inf 20000
1) "jerry"
2) "xiaohong"
```

###### 8.3、Zset按score降序排序（ZREVRANGEBYSCORE）

```bash
localhost:6379> ZREVRANGEBYSCORE salary +inf -inf
1) "zhangsan"
2) "xiaohong"
3) "jerry"
localhost:6379> ZREVRANGEBYSCORE salary +inf -inf withscores
1) "zhangsan"
2) "30000"
3) "xiaohong"
4) "20000"
5) "jerry"
6) "16000"
localhost:6379> ZREVRANGEBYSCORE salary 20000 -inf withscores
1) "xiaohong"
2) "20000"
3) "jerry"
4) "16000"
```

###### 8.4、移除Zset中指定的元素（ZREM）

```bash
localhost:6379> ZRANGE salary 0 -1
1) "jerry"
2) "xiaohong"
3) "zhangsan"
localhost:6379> ZREM salary "jerry"
(integer) 1
localhost:6379> ZRANGE salary 0 -1
1) "xiaohong"
2) "zhangsan"
```

###### 8.5、获取Zset的长度（ZCARD）

```bash
localhost:6379> ZCARD salary
(integer) 2
```

###### 8.6、统计指定Zset区间内元素总数（ZCOUNT）

```bash
localhost:6379> ZRANGE salary 0 -1 withscores
1) "xiaohong"
2) "20000"
3) "zhangsan"
4) "30000"
localhost:6379> ZCOUNT salary 1000 15000
(integer) 0
localhost:6379> ZCOUNT salary 1000 25000
(integer) 1
```

##### 9、Geospatial地理位置

###### 9.1、将指定的地理空间位置（经度、纬度、名称）添加到指定的`key`中（ GEOADD）

> ==注意：有效的经度从-180度到180度。有效的纬度从-85.05112878度到85.05112878度。==

```bash
# 无效的经纬度（超过了限制）
localhost:6379> GEOADD china:city 39.90 116.40 beijing
(error) ERR invalid longitude,latitude pair 39.900000,116.400000
# 键china:city，经度116.40，纬度39.90，名称beijing
localhost:6379> GEOADD china:city 116.40 39.90 beijing
(integer) 1
localhost:6379> GEOADD china:city 121.47 31.23 shanghai
(integer) 1
localhost:6379> GEOADD china:city 106.50 29.53 chongqing
(integer) 1
localhost:6379> GEOADD china:city 114.05 22.52 shenzhen 120.16 30.24 hangzhou 108.96 34.26 xian
(integer) 3
localhost:6379> KEYS *
1) "china:city"
```

###### 9.2、从key里返回所有给定位置元素的位置（经度和纬度）（GEOPOS）

```bash
localhost:6379> GEOPOS china:city beijing
1) 1) "116.39999896287918091"
   2) "39.90000009167092543"
# 获取多个城市的经纬度   
localhost:6379> GEOPOS china:city beijing chongqing
1) 1) "116.39999896287918091"
   2) "39.90000009167092543"
2) 1) "106.49999767541885376"
   2) "29.52999957900659211"   
```

###### 9.3、返回两个给定位置之间的距离（GEODIST）

> 单位：m 表示单位为米。km 表示单位为千米。mi 表示单位为英里。ft 表示单位为英尺；==默认使用米作为单位==。

```bash
# 默认使用米作为单位
localhost:6379> GEODIST china:city beijing chongqing
"1464070.8051"
localhost:6379> GEODIST china:city beijing chongqing km
"1464.0708"
```

###### 9.4、以给定的经纬度为中心， 在Geospatial集合中找出某一半径内的元素（GEORADIUS）

```bash
# 获取以经度110，纬度30位中心，半径1000km为半径的周边的城市
localhost:6379> GEORADIUS china:city 110 30 1000 km
1) "chongqing"
2) "xian"
3) "shenzhen"
4) "hangzhou"
localhost:6379> GEORADIUS china:city 110 30 600 km
1) "chongqing"
2) "xian"
# withdist返回值包含直线距离
localhost:6379> GEORADIUS china:city 110 30 600 km withdist
1) 1) "chongqing"
   2) "341.9374"
2) 1) "xian"
   2) "483.8340"
# withcoord返回值包含经度和纬度   
localhost:6379> GEORADIUS china:city 110 30 600 km withcoord
1) 1) "chongqing"
   2) 1) "106.49999767541885376"
      2) "29.52999957900659211"
2) 1) "xian"
   2) 1) "108.96000176668167114"
      2) "34.25999964418929977"   
# count返回结果集中的个数（这里count为1，则只返回一个结果，如果没有则返回空）       
localhost:6379> GEORADIUS china:city 110 30 600 km withdist count 1
1) 1) "chongqing"
   2) "341.9374"
localhost:6379> GEORADIUS china:city 110 30 100 km withdist count 1
(empty array)
localhost:6379> GEORADIUS china:city 110 30 500 km withcoord withdist withhash
1) 1) "chongqing"
   2) "341.9374"
   3) (integer) 4026042091628984
   4) 1) "106.49999767541885376"
      2) "29.52999957900659211"
2) 1) "xian"
   2) "483.8340"
   3) (integer) 4040115445396757
   4) 1) "108.96000176668167114"
      2) "34.25999964418929977"
```

###### 9.5、找出位于指定范围内的元素，中心点是由指定的Geospatial集合中的位置元素决定（GEORADIUSBYMEMBER）

```bash
# 在Geospatial的china:city集合中以beijing为中心，半径小于等于1000km的城市
localhost:6379> GEORADIUSBYMEMBER china:city beijing 1000 km
1) "beijing"
2) "xian"
localhost:6379> GEORADIUSBYMEMBER china:city beijing 1000 km withdist
1) 1) "beijing"
   2) "0.0000"
2) 1) "xian"
   2) "910.0565"
```

###### 9.6、返回一个或多个位置元素的 Geohash 表示

```bash
localhost:6379> GEOHASH china:city beijing
1) "wx4fbxxfke0"
# 两个hash字符串长得越像，则距离越近
localhost:6379> GEOHASH china:city shanghai hangzhou
1) "wtw3sj5zbj0"
2) "wtmkn31bfb0"
```

###### 9.7、GEO没有提供删除成员的命令，但是因为GEO的底层实现是zset，所以可以借用zrem命令实现对地理位置信息的删除

```bash
localhost:6379> ZREM china:city hangzhou
(integer) 1
localhost:6379> ZRANGE china:city 0 -1
1) "chongqing"
2) "xian"
3) "shenzhen"
4) "shanghai"
5) "beijing"
localhost:6379> ZCARD china:city
(integer) 5
# ZADD命令不能对GEO进行操作
localhost:6379> ZADD china:city 100 60 hangzhou
(error) MISCONF Redis is configured to save RDB snapshots, but it is currently not able to persist on disk. Commands that may modify the data set are disabled, because this instance is configured to report errors during writes if RDB snapshotting fails (stop-writes-on-bgsave-error option). Please check the Redis logs for details about the RDB error.
# 以上报错需执行下面语句，否则一直会报错
localhost:6379> config set stop-writes-on-bgsave-error no
OK
```

##### 10、Hyperloglog基数统计

> ==说明：只会根据输入的元素来计算基数，而不会储存输入元素本身（所以HyperLogLog不能像集合那样,返回输入的各个元素）。==

> 应用场景举例：网页访问量（UV）。传统的解决方案是使用Set来保存用户id，然后统计Set中的元素数量来获取页面UV。但这种方案只能承载少量用户，一旦用户数量大起来就需要消耗大量的空间来存储用户id。我们的目的是统计用户数量而不是保存用户，这简直是个吃力不讨好的方案！而使用RedisHyperLogLog最多需要12k就可以统计大量的用户数，尽管它大概有0.81%的错误率，但对于统计UV这种不需要很精确的数据是可以忽略不计的。

###### 10.1、Hyperloglog新增元素（PFADD）

```bash
localhost:6379> PFADD hplog1 a b c d e f g h i j
(integer) 1
localhost:6379> PFCOUNT hplog1
(integer) 10
localhost:6379> PFADD hplog2 i j z x c v b n m
(integer) 1
localhost:6379> PFCOUNT hplog2
(integer) 9
```

###### 10.2、将多个 HyperLogLog 合并为一个 HyperLogLog（并集）（PFMERGE）

```bash
# 将hplog1、hplog2合并成hplog3
localhost:6379> PFMERGE hplog3 hplog1 hplog2
OK
localhost:6379> PFCOUNT hplog3
(integer) 15
```

##### 11、Bitmap位图场景

> 应用场景：统计疫情感染人数。比如：中国有12亿人口，用Bitmap位图表示就是创建一个12亿个0的二进制数，谁感染了就标记成1，表示的形式大概如下：0101000111000111...........................，这样用二进制表示的好处是可以节省内存。

###### 11.1、设置Bitmap的值（SETBIT）

```bash
localhost:6379> SETBIT bitmap1 0 1
(integer) 0
# 报错原因：值不能用2，只能用0或1（二进制）
localhost:6379> SETBIT bitmap1 1 2
(error) ERR bit is not an integer or out of range
localhost:6379> SETBIT bitmap1 1 0
(integer) 0
localhost:6379> SETBIT bitmap1 3 0
(integer) 0
localhost:6379> SETBIT bitmap1 4 1
(integer) 0
localhost:6379> SETBIT bitmap1 5 1
(integer) 0
localhost:6379> SETBIT bitmap1 6 0
(integer) 0
localhost:6379> GETBIT bitmap1 0
(integer) 1
localhost:6379> GETBIT bitmap1 1
(integer) 0
localhost:6379> GETBIT bitmap1 2
(integer) 0
```

###### 11.2、获取Bitmap的值（GETBIT）

```bash
localhost:6379> GETBIT bitmap1 0
(integer) 1
localhost:6379> GETBIT bitmap1 1
(integer) 0
localhost:6379> GETBIT bitmap1 2
(integer) 0
```

###### 11.3、统计Bitmap中值为1的数量（BITCOUNT）

```bash
localhost:6379> BITCOUNT bitmap1
(integer) 3
localhost:6379> BITCOUNT bitmap1 0 -1
(integer) 3
```

###### 11.4、对一个或多个保存二进制位的字符串key进行位元操作，并将结果保存到destkey上（BITOP）

```bash
localhost:6379> SETBIT 20010101 0 1
(integer) 0
localhost:6379> SETBIT 20010102 0 0
(integer) 0
localhost:6379> SETBIT 20010103 0 1
(integer) 0
localhost:6379> BITOP and 200101 20010101 20010102 20010103
(integer) 1
localhost:6379> GETBIT 200101 0
(integer) 0
```

##### 12、事务操作（此事务非彼事务）

> ==注意：Redis中，单条命令是原子性执行的，但事务不保证原子性，且没有回滚。事务中任意命令执行失败，其余的命令仍会被执行（Redis的事务更像是命令批处理）。==
>
> 事务执行三个步骤：1、开启事务（MULTI）；2、命令入队；3、执行事务（exec）。

###### 12.1、执行一个事务（MULTI...EXEC）

```bash
# 开启事务
localhost:6379> MULTI
OK
# 命令入队
localhost:6379(TX)> SET k1 v1
QUEUED
localhost:6379(TX)> SET k2 v2
QUEUED
localhost:6379(TX)> GET k2
QUEUED
localhost:6379(TX)> SET k3 v3
QUEUED
# 执行事务
localhost:6379(TX)> EXEC
1) OK
2) OK
3) "v2"
4) OK
```

###### 12.2、取消执行事务（MULTI...DISCARD）

```bash
localhost:6379> KEYS *
(empty array)
localhost:6379> MULTI
OK
localhost:6379(TX)> SET k1 v1
QUEUED
localhost:6379(TX)> SET k2 v2
QUEUED
localhost:6379(TX)> SET k4 v4
QUEUED
# 取消事务（事务队列中的命令都不会执行了）
localhost:6379(TX)> DISCARD
OK
localhost:6379> KEYS *
(empty array)
```

###### 12.3、异常处理（编译型错误）：命令有错，则事务中所有的命令都不会执行。

```bash
localhost:6379> KEYS *
(empty array)
localhost:6379> MULTI
OK
localhost:6379(TX)> SET k1 v1
QUEUED
localhost:6379(TX)> SET k2 v2
QUEUED
# 这里模拟错误的命令（把k3和v3写一起了）
localhost:6379(TX)> SET k3v3
(error) ERR wrong number of arguments for 'set' command
localhost:6379(TX)> SET k3 v3
QUEUED
# 事务中存在命令错误的语句，则事务会放弃执行
localhost:6379(TX)> EXEC
(error) EXECABORT Transaction discarded because of previous errors.
localhost:6379> KEYS *
(empty array)
```

###### 12.4、运行时异常（如：对字符串进行加1操作）

> ==注意：运行时异常只有有问题的命令执行无法成功（抛出异常），正常的语句均可执行成功。==

```bash
localhost:6379> KEYS *
(empty array)
localhost:6379> MULTI
OK
localhost:6379(TX)> SET k1 "aaa"
QUEUED
# 这里将引发运行时异常（对字符串进行加1操作）
localhost:6379(TX)> INCR k1
QUEUED
localhost:6379(TX)> SET k2 "bbb"
QUEUED
localhost:6379(TX)> GET k1
QUEUED
localhost:6379(TX)> GET k2
QUEUED
localhost:6379(TX)> EXEC
1) OK
2) (error) ERR value is not an integer or out of range
3) OK
4) "aaa"
5) "bbb"
```

##### 13、锁（Redis采用的是乐观锁）

> * 悲观锁：认为什么时候都可能出问题，无论做什么都加锁（影响性能）。传统的关系型数据库里面就用到了很多这种锁机制，比如行锁，表锁等，读锁，写锁等，都是在操作之前先上锁。
>
> * 乐观锁：认为什么时候都不会出问题，无论做什么都不加锁，更新的时候会判断一下在此期间别人有没有去更新这个数据，可以使用版本号等机制，乐观锁适用于多读的应用类型，这样可以提高吞吐量。乐观锁策略：提交版本必须大于记录当前版本才能执行更新。

###### 13.1、Redis乐观锁（Watch）：1、获取Version；2、更新的时候比较Version。

> 正常流程：

```bash
# 初始化钱money为100
localhost:6379> SET money 100
OK
# 初始化花费out为0
localhost:6379> SET out 0
OK
# 对money添加监视器
localhost:6379> WATCH money
OK
# 开启事务
localhost:6379> MULTI
OK
# 钱花掉了20，money减20
localhost:6379(TX)> DECRBY money 20
QUEUED
# 钱花掉了20，out增20
localhost:6379(TX)> INCRBY out 20
QUEUED
# 执行事务
localhost:6379(TX)> EXEC
1) (integer) 80
2) (integer) 20
```

> 以下模拟两个线程的执行:
>
> 线程1：

```bash
localhost:6379> WATCH money
OK
localhost:6379> MULTI 
OK
localhost:6379(TX)> DECRBY money 10
QUEUED
localhost:6379(TX)> INCRBY out 10
QUEUED
# 这些先不要EXEC执行线程
localhost:6379(TX)> 
```

> 线程2：

```bash
localhost:6379> GET money
"80"
# 这里更改了money的值，那么线程1后续EXEC执行事务会报错（Watch的作用）
localhost:6379> SET money 1000
OK
```

> 线程1：

```bash
localhost:6379> WATCH money
OK
localhost:6379> MULTI 
OK
localhost:6379(TX)> DECRBY money 10
QUEUED
localhost:6379(TX)> INCRBY out 10
QUEUED
# 现在EXEC执行事务，返回为空，表示事务执行失败
# 失败原因：EXEC执行之前，另外一个线程修改了Watch监视的值，就会导致事务的执行失败
localhost:6379(TX)> EXEC
(nil)
# 此时money的值为线程2修改的数值
localhost:6379> GET money
"1000"
```

> ==注意：一但执行 EXEC 开启事务的执行后，无论事务是否执行成功， WARCH对变量的监控都将被取消。因此，当事务执行完成后，需重新执行WATCH命令对变量进行监控，并开启新的事务进行操作。==

##### 14、Jedis

###### 14.1、导入redis的依赖

```xml
<dependencies>
    <dependency>
        <groupId>redis.clients</groupId>
        <artifactId>jedis</artifactId>
        <version>3.2.0</version>
    </dependency>
    <dependency>
        <groupId>com.alibaba</groupId>
        <artifactId>fastjson</artifactId>
        <version>1.2.67</version>
    </dependency>
</dependencies>
```

###### 14.2、编写测试代码

```java
import redis.clients.jedis.Jedis;

public class TestPing {

    public static void main(String[] args) {

        String host = "192.168.18.7";
        Integer port = 6379;

        //1.创建Jedis对象
        Jedis jedis = new Jedis(host, port);
        System.out.println("连接成功");

        System.out.println(jedis.ping());
        System.out.println(jedis.ping("aaa"));

        jedis.close();

    }
}
```

> 测试结果如下：

```properties
连接成功
PONG
aaa
```

###### 14.3、对key操作的命令

```java
import redis.clients.jedis.Jedis;

import java.util.Set;

public class TestKey {

    public static void main(String[] args) {

        String host = "192.168.18.7";
        Integer port = 6379;

        Jedis jedis = new Jedis(host, port);
        System.out.println("清空数据：" + jedis.flushDB());
        System.out.println("判断某个键是否存在：" + jedis.exists("username"));
        System.out.println("新增<'username','kuangshen'>的键值对：" + jedis.set("username", "kuangshen"));
        System.out.println("新增<'password','password'>的键值对：" + jedis.set("password", "password"));
        System.out.print("系统中所有的键如下：");
        Set<String> keys = jedis.keys("*");
        System.out.println(keys);
        System.out.println("删除键password:" + jedis.del("password"));
        System.out.println("判断键password是否存在：" + jedis.exists("password"));
        System.out.println("查看键username所存储的值的类型：" + jedis.type("username"));
        System.out.println("随机返回key索引中的一个：" + jedis.randomKey());
        System.out.println("重命名key：" + jedis.rename("username", "name"));
        System.out.println("取出改后的name：" + jedis.get("name"));
        System.out.println("按索引查询：" + jedis.select(0));
        System.out.println("删除当前选择数据库中的所有key：" + jedis.flushDB());
        System.out.println("返回当前数据库中key的数目：" + jedis.dbSize());
        System.out.println("删除所有数据库中的所有key：" + jedis.flushAll());
        jedis.close();
    }
}
```

> 结果如下：

```properties
清空数据：OK
判断某个键是否存在：false
新增<'username','kuangshen'>的键值对：OK
新增<'password','password'>的键值对：OK
系统中所有的键如下：[password, username]
删除键password:1
判断键password是否存在：false
查看键username所存储的值的类型：string
随机返回key索引中的一个：username
重命名key：OK
取出改后的name：kuangshen
按索引查询：OK
删除当前选择数据库中的所有key：OK
返回当前数据库中key的数目：0
删除所有数据库中的所有key：OK
```

###### 14.4、对String操作的命令

```java
import redis.clients.jedis.Jedis;

import java.util.concurrent.TimeUnit;

public class TestString {
    public static void main(String[] args) {

        String host = "192.168.18.7";
        Integer port = 6379;

        Jedis jedis = new Jedis(host, port);
        jedis.flushDB();
        System.out.println("===========增加数据===========");
        System.out.println(jedis.set("key1", "value1"));
        System.out.println(jedis.set("key2", "value2"));
        System.out.println(jedis.set("key3", "value3"));

        System.out.println("删除键key2:" + jedis.del("key2"));
        System.out.println("获取键key2:" + jedis.get("key2"));
        System.out.println("修改key1:" + jedis.set("key1", "value1Changed"));
        System.out.println("获取key1的值：" + jedis.get("key1"));
        System.out.println("在key3后面加入值：" + jedis.append("key3", "End"));
        System.out.println("key3的值：" + jedis.get("key3"));
        System.out.println("增加多个键值对：" + jedis.mset("key01", "value01", "key02", "value02", "key03", "value03"));
        System.out.println("获取多个键值对：" + jedis.mget("key01", "key02", "key03"));
        System.out.println("获取多个键值对：" + jedis.mget("key01", "key02", "key03", "key04"));
        System.out.println("删除多个键值对：" + jedis.del("key01", "key02"));
        System.out.println("获取多个键值对：" + jedis.mget("key01", "key02", "key03"));
        jedis.flushDB();
        System.out.println("===========新增键值对防止覆盖原先值==============");
        System.out.println(jedis.setnx("key1", "value1"));
        System.out.println(jedis.setnx("key2", "value2"));
        System.out.println(jedis.setnx("key2", "value2-new"));
        System.out.println(jedis.get("key1"));
        System.out.println(jedis.get("key2"));
        System.out.println("===========新增键值对并设置有效时间=============");
        System.out.println(jedis.setex("key3", Long.valueOf(2), "value3"));
        System.out.println(jedis.get("key3"));
        try {
            TimeUnit.SECONDS.sleep(3);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        System.out.println(jedis.get("key3"));
        System.out.println("===========获取原值，更新为新值==========");
        System.out.println(jedis.getSet("key2", "key2GetSet"));
        System.out.println(jedis.get("key2"));
        System.out.println("获得key2的值的子串：" + jedis.getrange("key2", 2, 4));
    }
}
```

> 结果如下：

```properties
===========增加数据===========
OK
OK
OK
删除键key2:1
获取键key2:null
修改key1:OK
获取key1的值：value1Changed
在key3后面加入值：9
key3的值：value3End
增加多个键值对：OK
获取多个键值对：[value01, value02, value03]
获取多个键值对：[value01, value02, value03, null]
删除多个键值对：2
获取多个键值对：[null, null, null]
===========新增键值对防止覆盖原先值==============
1
1
0
value1
value2
===========新增键值对并设置有效时间=============
OK
value3
null
===========获取原值，更新为新值==========
value2
key2GetSet
获得key2的值的子串：y2G
```

###### 14.5、对List操作的命令

```java
import redis.clients.jedis.Jedis;

public class TestList {

    public static void main(String[] args) {
        String host = "192.168.18.7";
        Integer port = 6379;

        Jedis jedis = new Jedis(host, port);
        jedis.flushDB();
        System.out.println("===========添加一个list===========");
        jedis.lpush("collections", "ArrayList", "Vector", "Stack", "HashMap", "WeakHashMap", "LinkedHashMap");
        jedis.lpush("collections", "HashSet");
        jedis.lpush("collections", "TreeSet");
        jedis.lpush("collections", "TreeMap");
        System.out.println("collections的内容：" + jedis.lrange("collections", 0, -1));//-1代表倒数第一个元素，-2代表倒数第二个元素,end为-1表示查询全部
        System.out.println("collections区间0-3的元素：" + jedis.lrange(" collections", 0, 3));
        System.out.println("===============================");
        // 删除列表指定的值 ，第二个参数为删除的个数（有重复时），后add进去的值先被删，类似于出栈
        System.out.println("删除指定元素个数：" + jedis.lrem("collections", 2, "HashMap"));
        System.out.println("collections的内容：" + jedis.lrange("collections", 0, -1));
        System.out.println("删除下表0-3区间之外的元素：" + jedis.ltrim(" collections", 0, 3));
        System.out.println("collections的内容：" + jedis.lrange("collections", 0, -1));
        System.out.println("collections列表出栈（左端）：" + jedis.lpop(" collections"));
        System.out.println("collections的内容：" + jedis.lrange("collections", 0, -1));
        System.out.println("collections添加元素，从列表右端，与lpush相对应：" + jedis.rpush(" collections", " EnumMap"));
        System.out.println("collections的内容：" + jedis.lrange("collections", 0, -1));
        System.out.println("collections列表出栈（右端）：" + jedis.rpop(" collections"));
        System.out.println("collections的内容：" + jedis.lrange("collections", 0, -1));
        System.out.println("修改collections指定下标1的内容：" + jedis.lset("collections", 1, "LinkedArrayList"));
        System.out.println("collections的内容：" + jedis.lrange("collections", 0, -1));
        System.out.println("===============================");
        System.out.println("collections的长度：" + jedis.llen("collections"));
        System.out.println("获取collections下标为2的元素：" + jedis.lindex(" collections", 2));
        System.out.println("===============================");
        jedis.lpush("sortedList", "3", "6", "2", "0", "7", "4");
        System.out.println("sortedList排序前：" + jedis.lrange("sortedList", 0, -1));
        System.out.println(jedis.sort("sortedList"));
        System.out.println("sortedList排序后：" + jedis.lrange("sortedList", 0, -1));
    }
}
```

> 结果如下：

```properties
===========添加一个list===========
collections的内容：[TreeMap, TreeSet, HashSet, LinkedHashMap, WeakHashMap, HashMap, Stack, Vector, ArrayList]
collections区间0-3的元素：[]
===============================
删除指定元素个数：1
collections的内容：[TreeMap, TreeSet, HashSet, LinkedHashMap, WeakHashMap, Stack, Vector, ArrayList]
删除下表0-3区间之外的元素：OK
collections的内容：[TreeMap, TreeSet, HashSet, LinkedHashMap, WeakHashMap, Stack, Vector, ArrayList]
collections列表出栈（左端）：null
collections的内容：[TreeMap, TreeSet, HashSet, LinkedHashMap, WeakHashMap, Stack, Vector, ArrayList]
collections添加元素，从列表右端，与lpush相对应：1
collections的内容：[TreeMap, TreeSet, HashSet, LinkedHashMap, WeakHashMap, Stack, Vector, ArrayList]
collections列表出栈（右端）： EnumMap
collections的内容：[TreeMap, TreeSet, HashSet, LinkedHashMap, WeakHashMap, Stack, Vector, ArrayList]
修改collections指定下标1的内容：OK
collections的内容：[TreeMap, LinkedArrayList, HashSet, LinkedHashMap, WeakHashMap, Stack, Vector, ArrayList]
===============================
collections的长度：8
获取collections下标为2的元素：null
===============================
sortedList排序前：[4, 7, 0, 2, 6, 3]
[0, 2, 3, 4, 6, 7]
sortedList排序后：[4, 7, 0, 2, 6, 3]
```

###### 14.6、对Set的操作命令

```java
import redis.clients.jedis.Jedis;

public class TestSet {

    public static void main(String[] args) {

        String host = "192.168.18.7";
        Integer port = 6379;

        Jedis jedis = new Jedis(host, port);
        jedis.flushDB();
        System.out.println("============向集合中添加元素（不重复）============");
        System.out.println(jedis.sadd("eleSet", "e1", "e2", "e4", "e3", "e0", "e8", "e7", "e5"));
        System.out.println(jedis.sadd("eleSet", "e6"));
        System.out.println(jedis.sadd("eleSet", "e6"));
        System.out.println("eleSet的所有元素为：" + jedis.smembers("eleSet"));
        System.out.println("删除一个元素e0：" + jedis.srem("eleSet", "e0"));
        System.out.println("eleSet的所有元素为：" + jedis.smembers("eleSet"));
        System.out.println("删除两个元素e7和e6：" + jedis.srem("eleSet", "e7", "e6"));
        System.out.println("eleSet的所有元素为：" + jedis.smembers("eleSet"));
        System.out.println("随机的移除集合中的一个元素：" + jedis.spop("eleSet"));
        System.out.println("随机的移除集合中的一个元素：" + jedis.spop("eleSet"));
        System.out.println("eleSet的所有元素为：" + jedis.smembers("eleSet"));
        System.out.println("eleSet中包含元素的个数：" + jedis.scard("eleSet"));
        System.out.println("e3是否在eleSet中：" + jedis.sismember("eleSet", "e3"));
        System.out.println("e1是否在eleSet中：" + jedis.sismember("eleSet", "e1"));
        System.out.println("e1是否在eleSet中：" + jedis.sismember("eleSet", "e5"));
        System.out.println("=================================");
        System.out.println(jedis.sadd("eleSet1", "e1", "e2", "e4", "e3", "e0", "e8", "e7", "e5"));
        System.out.println(jedis.sadd("eleSet2", "e1", "e2", "e4", "e3", "e0", "e8"));
        System.out.println("将eleSet1中删除e1并存入eleSet3中：" + jedis.smove("eleSet1", "eleSet3", "e1"));//移到集合元素
        System.out.println("将eleSet1中删除e2并存入eleSet3中：" + jedis.smove("eleSet1", "eleSet3", "e2"));
        System.out.println("eleSet1中的元素：" + jedis.smembers("eleSet1"));
        System.out.println("eleSet3中的元素：" + jedis.smembers("eleSet3"));
        System.out.println("============集合运算=================");
        System.out.println("eleSet1中的元素：" + jedis.smembers("eleSet1"));
        System.out.println("eleSet2中的元素：" + jedis.smembers("eleSet2"));
        System.out.println("eleSet1和eleSet2的交集:" + jedis.sinter("eleSet1 ", "eleSet2"));
        System.out.println("eleSet1和eleSet2的并集:" + jedis.sunion("eleSet1 ", "eleSet2"));
        System.out.println("eleSet1和eleSet2的差集:" + jedis.sdiff("eleSet1 ", "eleSet2"));//eleSet1中有，eleSet2中没有
        jedis.sinterstore("eleSet4", "eleSet1", "eleSet2");//求交集并将交集保存到dstkey的集合
        System.out.println("eleSet4中的元素：" + jedis.smembers("eleSet4"));
    }
}
```

> 结果如下：

```properties
============向集合中添加元素（不重复）============
8
1
0
eleSet的所有元素为：[e4, e3, e5, e1, e2, e0, e7, e8, e6]
删除一个元素e0：1
eleSet的所有元素为：[e5, e1, e2, e7, e8, e3, e4, e6]
删除两个元素e7和e6：2
eleSet的所有元素为：[e8, e3, e4, e5, e2, e1]
随机的移除集合中的一个元素：e5
随机的移除集合中的一个元素：e3
eleSet的所有元素为：[e8, e4, e2, e1]
eleSet中包含元素的个数：4
e3是否在eleSet中：false
e1是否在eleSet中：true
e1是否在eleSet中：false
=================================
8
6
将eleSet1中删除e1并存入eleSet3中：0
将eleSet1中删除e2并存入eleSet3中：0
eleSet1中的元素：[e8, e4, e3, e5, e1, e2, e0, e7]
eleSet3中的元素：[]
============集合运算=================
eleSet1中的元素：[e8, e4, e3, e5, e1, e2, e0, e7]
eleSet2中的元素：[e2, e1, e8, e4, e3, e0]
eleSet1和eleSet2的交集:[]
eleSet1和eleSet2的并集:[]
eleSet1和eleSet2的差集:[]
eleSet4中的元素：[e8, e1, e2, e4, e3, e0]
```

###### 14.6、对Hash的操作命令

```java
import redis.clients.jedis.Jedis;

import java.util.HashMap;
import java.util.Map;

public class TestHash {

    public static void main(String[] args) {

        String host = "192.168.18.7";
        Integer port = 6379;

        Jedis jedis = new Jedis(host, port);
        jedis.flushDB();
        Map<String, String> map = new HashMap<>();
        map.put("key1", "value1");
        map.put("key2", "value2");
        map.put("key3", "value3");
        map.put("key4", "value4");
        //添加名称为hash（key）的hash元素
        jedis.hmset("hash", map);
        //向名称为hash的hash中添加key为key5，value为value5元素
        jedis.hset("hash", "key5", "value5");
        System.out.println("散列hash的所有键值对为：" + jedis.hgetAll("hash"));//return Map<String,String>
        System.out.println("散列hash的所有键为：" + jedis.hkeys("hash"));//return Set<String>
        System.out.println("散列hash的所有值为：" + jedis.hvals("hash"));//return List<String>
        System.out.println("将key6保存的值加上一个整数，如果key6不存在则添加key6：" + jedis.hincrBy("hash", "key6", 6));
        System.out.println("散列hash的所有键值对为：" + jedis.hgetAll("hash"));
        System.out.println("将key6保存的值加上一个整数，如果key6不存在则添加key6：" + jedis.hincrBy("hash", "key6", 3));
        System.out.println("散列hash的所有键值对为：" + jedis.hgetAll("hash"));
        System.out.println("删除一个或者多个键值对：" + jedis.hdel("hash", "key2"));
        System.out.println("散列hash的所有键值对为：" + jedis.hgetAll("hash"));
        System.out.println("散列hash中键值对的个数：" + jedis.hlen("hash"));
        System.out.println("判断hash中是否存在key2：" + jedis.hexists("hash", "key2"));
        System.out.println("判断hash中是否存在key3：" + jedis.hexists("hash", "key3"));
        System.out.println("获取hash中的值：" + jedis.hmget("hash", "key3"));
        System.out.println("获取hash中的值：" + jedis.hmget("hash", "key3", "key4"));
    }
}
```

> 结果如下：

```properties
散列hash的所有键值对为：{key1=value1, key2=value2, key5=value5, key3=value3, key4=value4}
散列hash的所有键为：[key1, key2, key5, key3, key4]
散列hash的所有值为：[value1, value2, value4, value3, value5]
将key6保存的值加上一个整数，如果key6不存在则添加key6：6
散列hash的所有键值对为：{key1=value1, key2=value2, key5=value5, key6=6, key3=value3, key4=value4}
将key6保存的值加上一个整数，如果key6不存在则添加key6：9
散列hash的所有键值对为：{key1=value1, key2=value2, key5=value5, key6=9, key3=value3, key4=value4}
删除一个或者多个键值对：1
散列hash的所有键值对为：{key1=value1, key5=value5, key6=9, key3=value3, key4=value4}
散列hash中键值对的个数：5
判断hash中是否存在key2：false
判断hash中是否存在key3：true
获取hash中的值：[value3]
获取hash中的值：[value3, value4]
```

##### 15、事务

```java
import com.alibaba.fastjson.JSONObject;
import redis.clients.jedis.Jedis;
import redis.clients.jedis.Transaction;

public class TestMulti {

    public static void main(String[] args) {

        String host = "192.168.18.7";
        Integer port = 6379;

        JSONObject jsonObject = new JSONObject();
        jsonObject.put("name", "zhangSan");
        jsonObject.put("age", 18);
        String jsonString = jsonObject.toJSONString();

        Jedis jedis = new Jedis(host, port);

        jedis.flushDB();//清空当前数据库

        jedis.watch(jsonString);//watch要在事务multi前使用，否则会报错
        Transaction multi = jedis.multi();
        try {
            multi.set("user1", jsonString);
            multi.set("user2", jsonString);

            //int i = 1 / 0;//代码抛出异常，事务执行失败
            multi.incrBy("user1", 8);//代码不会抛出异常，事务执行完成

            multi.exec();
        } catch (Exception e) {
            multi.discard();
            e.printStackTrace();
        } finally {
            System.out.println(jedis.get("user1"));
            System.out.println(jedis.get("user2"));
            jedis.close();
        }
    }
}
```

> 结果如下：

```pr
{"name":"zhangSan","age":18}
{"name":"zhangSan","age":18}
```

##### 16、SpringBoot整合

###### 16.1、pom配置

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.5.4</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>
    <groupId>com.kuang</groupId>
    <artifactId>redis-02-springboot</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <name>redis-02-springboot</name>
    <description>redis-02-springboot</description>
    <properties>
        <java.version>1.8</java.version>
    </properties>
    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-redis</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <scope>runtime</scope>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-configuration-processor</artifactId>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
                <configuration>
                    <excludes>
                        <exclude>
                            <groupId>org.projectlombok</groupId>
                            <artifactId>lombok</artifactId>
                        </exclude>
                    </excludes>
                </configuration>
            </plugin>
        </plugins>
    </build>

</project>
```

###### 16.2、application.properties

```properties
server.port=8888
# SpringBoot所有的配置类都有一个自动配置类，这里是RedisAutoConfiguration
# 自动配置类都会绑定一个properties配置文件，这里是RedisProperties
spring.redis.host=192.168.18.7
spring.redis.port=6379
```

###### 16.3、测试代码

```java
import lombok.extern.slf4j.Slf4j;
import org.junit.jupiter.api.Test;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.data.redis.connection.RedisConnection;
import org.springframework.data.redis.connection.RedisConnectionFactory;
import org.springframework.data.redis.core.RedisTemplate;

import javax.annotation.Resource;

@Slf4j
@SpringBootTest
class Redis02SpringbootApplicationTests {

    @Resource
    private RedisTemplate redisTemplate;

    @Test
    void contextLoads() {

        RedisConnectionFactory connectionFactory = redisTemplate.getConnectionFactory();
        RedisConnection connection = connectionFactory.getConnection();
        connection.flushDb();

        redisTemplate.opsForValue().set("k1", "测试一下哈");
        log.info(redisTemplate.opsForValue().get("k1").toString());

    }

}
```

##### 17、Redis.conf

###### 17.1、Redis的配置文件位于 Redis 安装目录下，文件名为 redis.conf，我们一般情况下，会单独拷贝出来一份进行操作。来保证初始文件的安全。

```bash
# 获取全部的配置
config get *
```

###### 17.2、Units单位

> * 配置大小单位，开头定义了一些基本的度量单位，只支持bytes，不支持bit
> * 对大小写不敏感

```properties
# Note on units: when memory size is needed, it is possible to specify
# it in the usual form of 1k 5GB 4M and so forth:
#
# 1k => 1000 bytes
# 1kb => 1024 bytes
# 1m => 1000000 bytes
# 1mb => 1024*1024 bytes
# 1g => 1000000000 bytes
# 1gb => 1024*1024*1024 bytes
#
# units are case insensitive so 1GB 1Gb 1gB are all the same.
```

###### 17.3、INCLUDES包含：和Spring配置文件类似，可以通过includes包含，redis.conf可以作为总文件，可以包含其他文件！

```properties
################################## INCLUDES ###################################

# Include one or more other config files here.  This is useful if you
# have a standard template that goes to all Redis servers but also need
# to customize a few per-server settings.  Include files can include
# other files, so use this wisely.
#
# Note that option "include" won't be rewritten by command "CONFIG REWRITE"
# from admin or Redis Sentinel. Since Redis always uses the last processed
# line as value of a configuration directive, you'd better put includes
# at the beginning of this file to avoid overwriting config change at runtime.
#
# If instead you are interested in using includes to override configuration
# options, it is better to use include as the last line.
#
# include /path/to/local.conf
# include /path/to/other.conf
```

###### 17.4、NETWORK网络配置

```properties
# 绑定的ip（只能通过这个IP来访问Redis）
bind 127.0.0.1 -::1
# 保护模式
protected-mode yes
# 默认端口
port 6379          
```

###### 17.5、GENERAL通用

```properties
# 默认情况下，Redis不作为守护进程运行。需要开启的话，改为 yes
daemonize yes
# 可通过upstart和systemd管理Redis守护进程
supervised no
# 以后台进程方式运行redis，则需要指定pid 文件
pidfile /var/run/redis_6379.pid
# 日志级别。可选项有：
# debug（记录大量日志信息，适用于开发、测试阶段）；  
# verbose（较多日志信息）；
# notice（适量日志信息，使用于生产环境）；
# warning（仅有部分重要、关键信息才会被记录）。
loglevel notice
# 日志文件的位置，当指定为空字符串时，为标准输出
logfile ""
# 设置数据库的数目。默认的数据库是DB 0
databases 16
# 是否总是显示logo
always-show-logo yes
```

###### 17.6、SNAPSHOPTING 快照

```properties
# 900秒（15分钟）内至少1个key值改变（则进行数据库保存--持久化）
save 900 1
# 300秒（5分钟）内至少10个key值改变（则进行数据库保存--持久化）
save 300 10
# 60秒（1分钟）内至少10000个key值改变（则进行数据库保存--持久化）
save 60 10000
# 持久化出现错误后，是否依然进行继续进行工作
stop-writes-on-bgsave-error yes
# 使用压缩rdb文件 yes：压缩，但是需要一些cpu的消耗。no：不压缩，需要更多的磁盘空间
rdbcompression yes
# 是否校验rdb文件，更有利于文件的容错性，但是在保存rdb文件的时候，会有大概10%的性能损耗
rdbchecksum yes
# dbfilename rdb文件名称
dbfilename dump.rdb
# dir 数据目录，数据库的写入会在这个目录。rdb、aof文件也会写在这个目录
dir ./
```

###### 17.7、SECURITY安全

> 访问密码的查看，设置和取消

```properties
# IMPORTANT NOTE: starting with Redis 6 "requirepass" is just a compatibility
# layer on top of the new ACL system. The option effect will be just setting
# the password for the default user. Clients will still authenticate using
# AUTH <password> as usually, or more explicitly with AUTH default <password>
# if they follow the new protocol: both will work.
#
# The requirepass is not compatable with aclfile option and the ACL LOAD
# command, these will cause requirepass to be ignored.
#
# requirepass foobared
requirepass 123456
```

> 也可以通过命令行设置

```bash
[root@alanCentos01 bin]# ./redis-cli --raw -h localhost -p 6379
localhost:6379> ping
PONG
# 获得和设置密码
config get requirepass
config set requirepass "123456"
#测试ping，发现需要验证
127.0.0.1:6379> ping
NOAUTH Authentication required.
# 验证
127.0.0.1:6379> auth 123456
OK
127.0.0.1:6379> ping
PONG
```

###### 17.8、客户端限制（CLIENTS）

```properties
# 设置能连上redis的最大客户端连接数量
maxclients 10000
# redis配置的最大内存容量
maxmemory <bytes>
# maxmemory-policy 内存达到上限的处理策略
# volatile-lru：利用LRU算法移除设置过过期时间的key。
# volatile-random：随机移除设置过过期时间的key。
# volatile-ttl：移除即将过期的key，根据最近过期时间来删除（辅以TTL）
# allkeys-lru：利用LRU算法移除任何key。
# allkeys-random：随机移除任何key。
# noeviction：不移除任何key，只是返回一个写错误。
maxmemory-policy noeviction
```

##### 18、Redis的持久化

###### 18.1、什么是RDB：在指定的时间间隔内将内存中的数据集快照写入磁盘，也就是行话讲的Snapshot快照，它恢复时是将快照文件直接读到内存里。

> * 说明：Redis会单独创建（fork）一个子进程来进行持久化，会先将数据写入到一个临时文件中，待持久化过程都结束了，再用这个临时文件替换上次持久化好的文件。整个过程中，主进程是不进行任何IO操作的。这就确保了极高的性能。如果需要进行大规模数据的恢复，且对于数据恢复的完整性不是非常敏感，那RDB方式要比AOF方式更加的高效。==RDB的缺点是最后一次持久化后的数据可能丢失。==
> * Fork：Fork的作用是复制一个与当前进程一样的进程。新进程的所有数据（变量，环境变量，程序计数器等）数值都和原进程一致，但是是一个全新的进程，并作为原进程的子进程。

###### 18.2、Rdb保存的是dump.rdb文件

```bash
[root@alanCentos01 alan]# cd /usr/local/bin/
[root@alanCentos01 bin]# ll
总用量 18888
drwxr-xr-x 2 root root    4096 9月   1 23:59 config
-rw-r--r-- 1 root root     112 9月   2 00:10 dump.rdb
-rwxr-xr-x 1 root root 4829648 8月  25 16:19 redis-benchmark
lrwxrwxrwx 1 root root      12 8月  25 16:19 redis-check-aof -> redis-server
lrwxrwxrwx 1 root root      12 8月  25 16:19 redis-check-rdb -> redis-server
-rwxr-xr-x 1 root root 5003264 8月  25 16:19 redis-cli
lrwxrwxrwx 1 root root      12 8月  25 16:19 redis-sentinel -> redis-server
-rwxr-xr-x 1 root root 9491784 8月  25 16:19 redis-server
```

> Redis.conf配置文件中设置：

```properties
# 文件名
# The filename where to dump the DB
dbfilename dump.rdb
# 路径
# Note that you must specify a directory here, not a file name.
dir ./
```

###### 18.3、如何触发RDB快照

> 1、命令save或者是bgsave
>
> * save时只管保存，其他不管，全部阻塞。
> * bgsave，Redis会在后台异步进行快照操作，快照同时还可以响应客户端请求。可以通过lastsave命令获取最后一次成功执行快照的时间。
>
> ```bash
> localhost:6379> LASTSAVE
> 1630595272
> ```
>
> 2、执行flushall命令，也会产生dump.rdb文件，但里面是空的，无意义。
>
> 3、退出的时候也会产生dump.rdb文件（用kill杀掉redis进程也会产生dump.rdb文件）。

###### 18.4、如何使用RDB恢复Redis数据

> 1、获取目录的命令如下：
>
> ```bash
> localhost:6379> CONFIG GET dir
> dir
> # 如果在这个目录下存在dump.rdb文件，启动就会自动恢复其中的数据
> /usr/local/bin
> ```
>
> 2、将备份文件（dump.rdb）移动到目录/usr/local/bin并启动服务即可

###### 18.5、RDB优点和缺点

> 优点：适合大规模的数据恢复（前提是对数据完整性和一致性要求不高）。
>
> 缺点：
>
> * 在一定间隔时间做一次备份，所以如果redis意外down掉的话，就会丢失最后一次快照后的所有修改。
> * Fork的时候，内存中的数据被克隆了一份，大致2倍的膨胀性需要考虑。

###### 18.6、AOF（Append Only File）

> 以日志的形式来记录每个写操作，==将Redis执行过的所有指令记录下来（读操作不记录）==，只许追加文件但不可以改写文件，redis启动之初会读取该文件重新构建数据，换言之，redis重启的话就根据日志文件的内容将写指令从前到后执行一次以完成数据的恢复工作。

###### 18.7、AOF保存的是 appendonly.aof 文件，配置文件如下：

```bash
############################## APPEND ONLY MODE ###############################

# By default Redis asynchronously dumps the dataset on disk. This mode is
# good enough in many applications, but an issue with the Redis process or
# a power outage may result into a few minutes of writes lost (depending on
# the configured save points).
#
# The Append Only File is an alternative persistence mode that provides
# much better durability. For instance using the default data fsync policy
# (see later in the config file) Redis can lose just one second of writes in a
# dramatic event like a server power outage, or a single write if something
# wrong with the Redis process itself happens, but the operating system is
# still running correctly.
#
# AOF and RDB persistence can be enabled at the same time without problems.
# If the AOF is enabled on startup Redis will load the AOF, that is the file
# with the better durability guarantees.
#
# Please check https://redis.io/topics/persistence for more information.

appendonly no

# The name of the append only file (default: "appendonly.aof")

appendfilename "appendonly.aof"

# The fsync() call tells the Operating System to actually write data on disk
# instead of waiting for more data in the output buffer. Some OS will really flush
# data on disk, some other OS will just try to do it ASAP.
#
# Redis supports three different modes:
#
# no: don't fsync, just let the OS flush the data when it wants. Faster.
# always: fsync after every write to the append only log. Slow, Safest.
# everysec: fsync only one time every second. Compromise.
#
# The default is "everysec", as that's usually the right compromise between
# speed and data safety. It's up to you to understand if you can relax this to
# "no" that will let the operating system flush the output buffer when
# it wants, for better performances (but if you can live with the idea of
# some data loss consider the default persistence mode that's snapshotting),
# or on the contrary, use "always" that's very slow but a bit safer than
# everysec.
#
# More details please check the following article:
# http://antirez.com/post/redis-persistence-demystified.html
#
# If unsure, use "everysec".

# appendfsync always
appendfsync everysec
# appendfsync no

# When the AOF fsync policy is set to always or everysec, and a background
# saving process (a background save or AOF log background rewriting) is
# performing a lot of I/O against the disk, in some Linux configurations
# Redis may block too long on the fsync() call. Note that there is no fix for
# this currently, as even performing fsync in a different thread will block
# our synchronous write(2) call.
#
# In order to mitigate this problem it's possible to use the following option
# that will prevent fsync() from being called in the main process while a
# BGSAVE or BGREWRITEAOF is in progress.
#
# This means that while another child is saving, the durability of Redis is
# the same as "appendfsync none". In practical terms, this means that it is
# possible to lose up to 30 seconds of log in the worst scenario (with the
# default Linux settings).
#
# If you have latency problems turn this to "yes". Otherwise leave it as
# "no" that is the safest pick from the point of view of durability.

no-appendfsync-on-rewrite no

# Automatic rewrite of the append only file.
# Redis is able to automatically rewrite the log file implicitly calling
# BGREWRITEAOF when the AOF log size grows by the specified percentage.
#
# This is how it works: Redis remembers the size of the AOF file after the
# latest rewrite (if no rewrite has happened since the restart, the size of
# the AOF at startup is used).
#
# This base size is compared to the current size. If the current size is
# bigger than the specified percentage, the rewrite is triggered. Also
# you need to specify a minimal size for the AOF file to be rewritten, this
# is useful to avoid rewriting the AOF file even if the percentage increase
# is reached but it is still pretty small.
#
# Specify a percentage of zero in order to disable the automatic AOF
# rewrite feature.

auto-aof-rewrite-percentage 100
auto-aof-rewrite-min-size 64mb

# An AOF file may be found to be truncated at the end during the Redis
# startup process, when the AOF data gets loaded back into memory.
# This may happen when the system where Redis is running
# crashes, especially when an ext4 filesystem is mounted without the
# data=ordered option (however this can't happen when Redis itself
# crashes or aborts but the operating system still works correctly).
#
# Redis can either exit with an error when this happens, or load as much
# data as possible (the default now) and start if the AOF file is found
# to be truncated at the end. The following option controls this behavior.
#
# If aof-load-truncated is set to yes, a truncated AOF file is loaded and
# the Redis server starts emitting a log to inform the user of the event.
# Otherwise if the option is set to no, the server aborts with an error
# and refuses to start. When the option is set to no, the user requires
# to fix the AOF file using the "redis-check-aof" utility before to restart
# the server.
#
# Note that if the AOF file will be found to be corrupted in the middle
# the server will still exit with an error. This option only applies when
# Redis will try to read more data from the AOF file but not enough bytes
# will be found.
aof-load-truncated yes

# When rewriting the AOF file, Redis is able to use an RDB preamble in the
# AOF file for faster rewrites and recoveries. When this option is turned
# on the rewritten AOF file is composed of two different stanzas:
#
#   [RDB file][AOF tail]
#
# When loading, Redis recognizes that the AOF file starts with the "REDIS"
# string and loads the prefixed RDB file, then continues loading the AOF
# tail.
aof-use-rdb-preamble yes
```

> 详细说明：

```properties
# 是否以append only模式作为持久化方式，默认使用的是rdb方式持久化，这种方式在许多应用中已经足够用了
appendonly no

# appendfilename AOF 文件名称
appendfilename "appendonly.aof"

# appendfsync aof持久化策略的配置
# no表示不执行fsync，由操作系统保证数据同步到磁盘，速度最快。
# always表示每次写入都执行fsync，以保证数据同步到磁盘。
# everysec表示每秒执行一次fsync，可能会导致丢失这1s数据。
appendfsync everysec
```

###### 18.8、AOF修复

```bash
[root@alanCentos01 bin]# ./redis-check-aof --fix appendonly.aof
0x              6a: Expected \r\n, got: 6673
AOF analyzed: size=136, ok_up_to=81, ok_up_to_line=26, diff=55
This will shrink the AOF from 136 bytes, with 55 bytes, to 81 bytes
Continue? [y/N]: y
Successfully truncated AOF
```

###### 18.9、AOF的Rewrite

> AOF采用文件追加方式，文件会越来越大，为避免出现此种情况，新增了重写机制，当AOF文件的大小超过所设定的阈值时，Redis 就会启动AOF 文件的内容压缩，只保留可以恢复数据的最小指令集，可以使用命令：bgrewriteaof
>
> 重写原理：AOF文件持续增长而过大时（默认配置：auto-aof-rewrite-min-size 64mb），会fork出一条新进程来将文件重写（也是先写临时文件最后再rename），遍历新进程的内存中数据，每条记录有一条的Set语句。重写AOF文件的操作，并没有读取旧的AOF文件，这点和快照有点类似。

```properties
# 重写时是否可以运用Appendfsync，用默认no即可，保证数据安全性
no-appendfsync-on-rewrite no

# 设置重写的基准值
auto-aof-rewrite-min-size 64mb

#设置重写的基准值
auto-aof-rewrite-percentage 100
```

###### 18.10、AOF优点和缺点

> 优点：
>
> * 每修改同步：appendfsync always同步持久化，每次发生数据变更会被立即记录到磁盘，性能较差但数据完整性比较好。
> * 每秒同步： appendfsync everysec异步操作，每秒记录 ，如果一秒内宕机，有数据丢失。
> * 不同步： appendfsync no从不同步，效率最高。
>
> 缺点：
>
> * 相同数据集的数据而言，aof文件要远大于rdb文件，恢复速度慢于rdb。
> * AOF运行效率要慢于rdb，每秒同步策略效率较好，不同步效率和rdb相同。

###### 18.11、Redis关闭持久化

> 修改redis配置文件，如下：

```properties
# 注释掉原来的持久化规则
# save 3600 1
# save 300 100
# save 60 10000

# 设置save为空
save ""
```

> 修改完成后重启redis服务。

###### 18.12、性能建议

> * ==只做缓存，如果你只希望你的数据在服务器运行的时候存在，你也可以不使用任何持久化。==
> * 同时开启两种持久化方式：
>   * 在这种情况下，当redis重启的时候会优先载入AOF文件来恢复原始的数据，因为在通常情况下AOF文件保存的数据集要比RDB文件保存的数据集要完整。
>   * RDB的数据不实时，同时使用两者时服务器重启也只会找AOF文件，那要不要只使用AOF呢？作者建议不要，因为RDB更适合用于备份数据库（AOF在不断变化不好备份），快速重启，而且不会有AOF可能潜在的Bug，留着作为一个万一的手段。
>
> * 因为RDB文件只用作后备用途，建议只在Slave上持久化RDB文件，而且只要15分钟备份一次就够了，只保留 save 900 1 这条规则。
> * 如果Enable AOF ，好处是在最恶劣情况下也只会丢失不超过两秒数据，启动脚本较简单只load自己的AOF文件就可以了，代价一是带来了持续的IO，二是AOF rewrite 的最后将 rewrite 过程中产生的新数据写到新文件造成的阻塞几乎是不可避免的。只要硬盘许可，应该尽量减少AOF rewrite 
>   的频率，AOF重写的基础大小默认值64M太小了，可以设到5G以上，默认超过原大小100%大小重写可以改到适当的数值。
> * 如果不Enable AOF，仅靠 Master-Slave Repllcation 实现高可用性也可以，能省掉一大笔IO，也减少了rewrite时带来的系统波动。代价是如果Master/Slave 同时倒掉（断电），会丢失十几分钟的数据，启动脚本也要比较两个 Master/Slave 中的 RDB文件，载入较新的那个，微博就是这种架构。

##### 19、Redis发布订阅

>* Redis发布订阅(pub/sub)是一种消息通信模式：发送者(pub)发送消息，订阅者(sub)接收消息。
>* Redis客户端可以订阅任意数量的频道。

###### 19.1、命令

> 订阅频道

```bash
# 订阅频道filmChannel
localhost:6379> SUBSCRIBE filmChannel
subscribe
filmChannel
1
```

> 向频道发送消息

```bash
localhost:6379> PUBLISH filmChannel "hello everybody!"
# 返回值为1表示有一个消费者接收到了消息
1
localhost:6379> PUBLISH filmChannel "我是一个消息"
# 返回值为2表示有2个消费者收到了消息
2
localhost:6379> PUBLISH filmChannel "hahaha"
# 返回值为0表示没有消费者收到了消息
0
```

```bash
# 注意：订阅频道前的消息都无法收到
localhost:6379> SUBSCRIBE filmChannel
subscribe
filmChannel
1
# 消息
message
# 频道
filmChannel
# 消息的内容
hello everybody!
```

> 取消订阅

```bash
localhost:6379> UNSUBSCRIBE filmChannel
unsubscribe
filmChannel
0
```

###### 19.2、使用场景（简单应用场景，复杂还是用消息中间件（MQ）靠谱点）：

> * Pub/Sub构建实时消息系统。
> * Pub/Sub构建实时聊天系统。
> * 订阅关注系统。

##### 20、Redis主从复制

> 主从复制：是指将一台Redis服务器的数据，复制到其他的Redis服务器。前者称为主节点(master/leader)，后者称为从节点(slave/follower)；==数据的复制是单向的，只能由主节点到从节点。==Master以写为主，Slave 以读为主。
>
> ==默认情况下，每台Redis服务器都是主节点。==一个主节点可以有多个从节点(或没有从节点)，但一个从节点只能有一个主节点。
>
> 主从复制的作用主要包括：
>
> 1. 数据冗余：主从复制实现了数据的热备份，是持久化之外的一种数据冗余方式。
> 2. 故障恢复：当主节点出现问题时，可以由从节点提供服务，实现快速的故障恢复；实际上是一种服务的冗余。
> 3. 负载均衡：在主从复制的基础上，配合读写分离，可以由主节点提供写服务，由从节点提供读服务（即写Redis数据时应用连接主节点，读Redis数据时应用连接从节点），分担服务器负载；尤其是在写少读多的场景下，通过多个从节点分担读负载，可以大大提高Redis服务器的并发量。
> 4. 高可用基石：除了上述作用以外，主从复制还是哨兵和集群能够实施的基础，因此说主从复制是Redis高可用的基础。
>
> 一般来说，要将Redis运用于工程项目中，只使用一台Redis是万万不能的（至少要一主二从），原因如下：
>
> 1. 从结构上，单个Redis服务器会发生单点故障，并且一台服务器需要处理所有的请求负载，压力较大。
> 2. 从容量上，单个Redis服务器内存容量有限，就算一台Redis服务器内存容量为256G，也不能将所有内存用作Redis存储内存，一般来说，==单台Redis最大使用内存不应该超过20G==。

###### 20.1、环境配置

> 如果使用多台主机，则只需配从库，不需要配主库。

```bash
# 查看当前库信息
localhost:6379> INFO replication
# Replication
# 角色：主
role:master
# 连接的从机数：0
connected_slaves:0
master_failover_state:no-failover
master_replid:1f39a693f6335f034799065dc7b7d8aaf3458217
master_replid2:0000000000000000000000000000000000000000
master_repl_offset:0
second_repl_offset:-1
repl_backlog_active:0
repl_backlog_size:1048576
repl_backlog_first_byte_offset:0
repl_backlog_histlen:0
```

> 配置多个不同的配置文件（在相同机器相同路径下，用端口号可方便区分文件名）

```bash
[root@alanCentos01 config]# cp redis.conf redis6379.conf 
[root@alanCentos01 config]# cp redis.conf redis6380.conf 
[root@alanCentos01 config]# cp redis.conf redis6381.conf 
[root@alanCentos01 config]# ll
总用量 368
-rw-r--r-- 1 root root 93810 9月   4 16:38 redis6379.conf
-rw-r--r-- 1 root root 93810 9月   4 16:38 redis6380.conf
-rw-r--r-- 1 root root 93810 9月   4 16:38 redis6381.conf
-rw-r--r-- 1 root root 93810 9月   4 12:22 redis.conf
```

> 修改配置文件（redis端口号.conf）：
>
> 1. 指定端口 6379，依次类推。
> 2. 开启daemonize yes。
> 3. Pid文件名字：pidfile /var/run/redis_6379.pid ，依次类推。
> 4. Log文件名字：logfile "6379.log" , 依次类推。
> 5. rdb文件名字：dbfilename dump6379.rdb，依次类推。
> 6. ==如果主机设置了密码，从机slave还需配置：replicaof 127.0.0.1 6379，这里配置了就不用客户端每次写命令了（slaveof 127.0.0.1 6379）==。
> 7. ==如果主机设置了密码，主机（哨兵选举时需要，否则启动后无法连接新Master）和从机slave都要配置：masterauth 123456==。

> 主机（Redis端口号6379）配置如下：

```bash
[root@alanCentos01 config]# vim redis6379.conf
```

```properties
# Accept connections on the specified port, default is 6379 (IANA #815344).
# If port 0 is specified Redis will not listen on a TCP socket.
port 6379

# 服务以守护进程的方式启动（后台启动）
daemonize yes

pidfile /var/run/redis_6379.pid

logfile "6379.log"

# The filename where to dump the DB
dbfilename dump6379.rdb

# masterauth <master-password>
masterauth 123456
```

> 从机（Redis端口号6380）配置如下：

```bash
[root@alanCentos01 config]# vim redis6380.conf
```

```properties
# Accept connections on the specified port, default is 6379 (IANA #815344).
# If port 0 is specified Redis will not listen on a TCP socket.
port 6380

# 服务以守护进程的方式启动（后台启动）
daemonize yes

pidfile /var/run/redis_6380.pid

logfile "6380.log"

# The filename where to dump the DB
dbfilename dump6380.rdb

# replicaof <masterip> <masterport>
replicaof 127.0.0.1 6379

# masterauth <master-password>
masterauth 123456
```

> 从机（Redis端口号6381）配置如下：

```bash
[root@alanCentos01 config]# vim redis6381.conf
```

```properties
# Accept connections on the specified port, default is 6379 (IANA #815344).
# If port 0 is specified Redis will not listen on a TCP socket.
port 6381

# 服务以守护进程的方式启动（后台启动）
daemonize yes

pidfile /var/run/redis_6381.pid

logfile "6381.log"

# The filename where to dump the DB
dbfilename dump6381.rdb

# replicaof <masterip> <masterport>
replicaof 127.0.0.1 6379

# masterauth <master-password>
masterauth 123456
```

> 开放防火墙端口访问：

```bash
[root@alanCentos01 bin]# firewall-cmd --permanent --add-port=6380/tcp --zone=public
success
[root@alanCentos01 bin]# firewall-cmd --permanent --add-port=6381/tcp --zone=public
success
[root@alanCentos01 bin]# firewall-cmd --reload
success
[root@alanCentos01 bin]# firewall-cmd --list-port
8080/tcp 6868/tcp 20/tcp 21/tcp 22/tcp 80/tcp 8888/tcp 39000-40000/tcp 5762/tcp 15672/tcp 5672/tcp 5673/tcp 15673/tcp 4369/tcp 25672/tcp 25673/tcp 5674/tcp 15674/tcp 25674/tcp 9188/tcp 5670/tcp 2181/tcp 9092/tcp 6379/tcp 6380/tcp 6381/tcp
```

###### 20.2、配置“一主二从”

> 1. 通过上面配置的3个不同的配置文件开启3个服务：

```bash
[root@alanCentos01 bin]# ./redis-server config/redis6379.conf 
[root@alanCentos01 bin]# ./redis-server config/redis6380.conf 
[root@alanCentos01 bin]# ./redis-server config/redis6381.conf 
[root@alanCentos01 bin]# ps -ef | grep redis
root      44809      1  0 17:11 ?        00:00:00 ./redis-server *:6379
root      44833      1  0 17:11 ?        00:00:00 ./redis-server *:6380
root      44849      1  0 17:11 ?        00:00:00 ./redis-server *:6381
root      44864  18133  0 17:11 pts/0    00:00:00 grep --color=auto redis
```

> 2. 客户端通过3个不同的端口连接3个启动的服务：

```bash
# 连接6379端口服务的客户端
[root@alanCentos01 bin]# ./redis-cli --raw -h localhost -p 6379
localhost:6379> auth 123456
OK
localhost:6379> ping
PONG
localhost:6379> keys *

localhost:6379> info replication
# Replication
role:master
connected_slaves:0
master_failover_state:no-failover
master_replid:5dd04aeaf5d9bf9b0edaec4bcdb1c71d5c85496e
master_replid2:0000000000000000000000000000000000000000
master_repl_offset:0
second_repl_offset:-1
repl_backlog_active:0
repl_backlog_size:1048576
repl_backlog_first_byte_offset:0
repl_backlog_histlen:0
localhost:6379>
```

```bash
# 连接6380端口服务的客户端
[root@alanCentos01 bin]# ./redis-cli --raw -h localhost -p 6380
localhost:6380> auth 123456
OK
localhost:6380> ping
PONG
localhost:6380> keys *

localhost:6380> info replication
# Replication
role:master
connected_slaves:0
master_failover_state:no-failover
master_replid:d563cc2d95befdc86ef5ee18beaae74a029dcaba
master_replid2:0000000000000000000000000000000000000000
master_repl_offset:0
second_repl_offset:-1
repl_backlog_active:0
repl_backlog_size:1048576
repl_backlog_first_byte_offset:0
repl_backlog_histlen:0
localhost:6380>
```

```bash
# 连接6381端口服务的客户端
[root@alanCentos01 bin]# ./redis-cli --raw -h localhost -p 6381
localhost:6381> auth 123456
OK
localhost:6381> ping
PONG
localhost:6381> keys *

localhost:6381> info replication
# Replication
role:master
connected_slaves:0
master_failover_state:no-failover
master_replid:e2094cf8044837dbaa079bfcdad65c39997ebcd7
master_replid2:0000000000000000000000000000000000000000
master_repl_offset:0
second_repl_offset:-1
repl_backlog_active:0
repl_backlog_size:1048576
repl_backlog_first_byte_offset:0
repl_backlog_histlen:0
localhost:6381>
```

> 3. 默认情况下三个都是Master主节点，配置为一个Master两个Slave：
>
>    注意：保留一个端口的服务作为Master主节点（我这是用6379做为Master 主节点）。因此，==只要在连接到6380服务和6381服务的客户端，分别执行下面语句即可==。

```bash
# 连接6380端口服务的客户端
# 如果配置文件中配置了“replicaof 127.0.0.1 6379”，就不用写这个命令了，在启动6380端口服务的时候就自动变slave了
localhost:6380> slaveof 127.0.0.1 6379
OK
localhost:6380> info replication
# Replication
# 这里role已经变成slave了
role:slave
master_host:127.0.0.1
master_port:6379
# master连接状况为up，说明已连接到master了
master_link_status:up
master_last_io_seconds_ago:9
master_sync_in_progress:0
slave_repl_offset:280
slave_priority:100
slave_read_only:1
replica_announced:1
connected_slaves:0
master_failover_state:no-failover
master_replid:c633bea8149b230b9a9e62121769346ea2b856e3
master_replid2:0000000000000000000000000000000000000000
master_repl_offset:280
second_repl_offset:-1
repl_backlog_active:1
repl_backlog_size:1048576
repl_backlog_first_byte_offset:1
repl_backlog_histlen:280
localhost:6380>
```

```bash
# 连接6381端口服务的客户端
# 如果配置文件中配置了“replicaof 127.0.0.1 6379”，就不用写这个命令了，在启动6381端口服务的时候就自动变slave了
localhost:6381> slaveof 127.0.0.1 6379
OK
localhost:6381> info replication
# Replication
# 这里role已经变成slave了
role:slave
master_host:127.0.0.1
master_port:6379
# master连接状况为up，说明已连接到master了
master_link_status:up
master_last_io_seconds_ago:6
master_sync_in_progress:0
slave_repl_offset:364
slave_priority:100
slave_read_only:1
replica_announced:1
connected_slaves:0
master_failover_state:no-failover
master_replid:c633bea8149b230b9a9e62121769346ea2b856e3
master_replid2:0000000000000000000000000000000000000000
master_repl_offset:364
second_repl_offset:-1
repl_backlog_active:1
repl_backlog_size:1048576
repl_backlog_first_byte_offset:71
repl_backlog_histlen:294
localhost:6381> 
```

```bash
# 连接6379端口服务的客户端
localhost:6379> info replication
# Replication
role:master
# 有两个slaves连接到master了
connected_slaves:2
slave0:ip=127.0.0.1,port=6380,state=online,offset=532,lag=0
slave1:ip=127.0.0.1,port=6381,state=online,offset=532,lag=0
master_failover_state:no-failover
master_replid:c633bea8149b230b9a9e62121769346ea2b856e3
master_replid2:0000000000000000000000000000000000000000
master_repl_offset:532
second_repl_offset:-1
repl_backlog_active:1
repl_backlog_size:1048576
repl_backlog_first_byte_offset:1
repl_backlog_histlen:532
localhost:6379> 
```

> ==生产环境中主从配置都会在配置文件中配置，直接启动服务就直接是slave从机了，使用命令“slaveof 127.0.0.1 6379”比较麻烦，从机宕机或重启后，重新上线还要手动执行一下命令。==
>
> ==注意：主机可写入可以读取数据，但从机只能读取数据。==

###### 20.3、测试：在主机设置值，在从机都可以获取到！从机不能写值！

```bash
# 连接6379端口服务的客户端
localhost:6379> set k1 v1
OK
#主机能写也能读
localhost:6379> get k1
v1
```

```bash
# 连接6380端口服务的客户端
localhost:6380> keys *
k1
localhost:6380> get k1
v1
# 从机slave只能读不能写
localhost:6380> set k2 v2
READONLY You can't write against a read only replica.
```

```bash
# 连接6381端口服务的客户端
localhost:6381> keys *
k1
localhost:6381> get k1
v1
# 从机slave只能读不能写
localhost:6381> set k3 v3
READONLY You can't write against a read only replica.
localhost:6381> flushdb
READONLY You can't write against a read only replica.
```

> ==问题：如果主机宕机了，从机依然是从机器（即无法进行写操作了），从机不会自动推选新的主机（哨兵模式可实现从机推选主机）；主机从新上线后，从机又可以重新连上主机了（这时可以进行写操作了）。==

###### 20.4、复制原理

> * Slave启动成功连接到Master后会发送一个sync命令，Master接到命令，启动后台的存盘进程，同时收集所有接收到的用于修改数据集命令，在后台进程执行完毕之后，Master将传送整个数据文件到slave，并完成一次完全同步。
> * 全量复制：Slave服务在接收到数据库文件数据后，将其存盘并加载到内存中。
> * 增量复制：Master继续将新的所有收集到的修改命令依次传给Slave，完成同步。但只要是重新连接Master，一次完全同步（全量复制）将被自动执行。

###### 20.5、层层链路

> 上一个Slave 可以是下一个slave和Master，Slave同样可以接收其他Slaves的连接和同步请求，那么该Slave作为了链条中下一个的Master，可以有效减轻 主机Master的写压力！

###### 20.6、谋朝串位

> 一主二从的情况下，如果主机断了，从机可以使用命令“SLAVEOF NO ONE”将自己改为主服务器。这个时候其余的从机链接到这个节点。对一个从属服务器执行命令“SLAVEOF NO ONE”将使得这个从属服务器关闭复制功能，并从从属服务器转变为主服务器，且原来同步所得的数据集不会被丢弃。
>
> 这里模拟主服务器宕机：将6379端口的主服务shutdown。

```bash
# 连接6380端口服务的客户端
localhost:6380> info replication
# Replication
role:slave
master_host:127.0.0.1
master_port:6379
master_link_status:down
master_last_io_seconds_ago:-1
master_sync_in_progress:0
slave_repl_offset:20296
master_link_down_since_seconds:20
slave_priority:100
slave_read_only:1
replica_announced:1
connected_slaves:0
master_failover_state:no-failover
master_replid:c45d1c5a1054800f3a4e57acaf563b5a59a66fe4
master_replid2:0000000000000000000000000000000000000000
master_repl_offset:20296
second_repl_offset:-1
repl_backlog_active:1
repl_backlog_size:1048576
repl_backlog_first_byte_offset:1
repl_backlog_histlen:20296
# 将从属服务器变成主服务器
localhost:6380> slaveof no one
OK
localhost:6380> info replication
# Replication
role:master
connected_slaves:0
master_failover_state:no-failover
master_replid:7974c136568123b02d249a032f518554bda421ad
master_replid2:c45d1c5a1054800f3a4e57acaf563b5a59a66fe4
master_repl_offset:20296
second_repl_offset:20297
repl_backlog_active:1
repl_backlog_size:1048576
repl_backlog_first_byte_offset:1
repl_backlog_histlen:20296
localhost:6380> keys *
k1
localhost:6380> get k1
v1
localhost:6380> set k2 v2
OK
```

> 从属服务器变成主服务器后，也只是一个光杆司令，其他从属服务器可以手动执行命令绑定到这个主服务器上来。

```bash
# 连接6381端口服务的客户端
localhost:6381> slaveof 127.0.0.1 6380
OK
localhost:6381> info replication
# Replication
role:slave
master_host:127.0.0.1
master_port:6380
master_link_status:up
master_last_io_seconds_ago:7
master_sync_in_progress:0
slave_repl_offset:20362
slave_priority:100
slave_read_only:1
replica_announced:1
connected_slaves:0
master_failover_state:no-failover
master_replid:7974c136568123b02d249a032f518554bda421ad
master_replid2:c45d1c5a1054800f3a4e57acaf563b5a59a66fe4
master_repl_offset:20362
second_repl_offset:20297
repl_backlog_active:1
repl_backlog_size:1048576
repl_backlog_first_byte_offset:1
repl_backlog_histlen:20362
localhost:6381>
```

```bash
# 连接6380端口服务的客户端
localhost:6380> info replication
# Replication
role:master
connected_slaves:1
slave0:ip=127.0.0.1,port=6381,state=online,offset=20376,lag=0
master_failover_state:no-failover
master_replid:7974c136568123b02d249a032f518554bda421ad
master_replid2:c45d1c5a1054800f3a4e57acaf563b5a59a66fe4
master_repl_offset:20376
second_repl_offset:20297
repl_backlog_active:1
repl_backlog_size:1048576
repl_backlog_first_byte_offset:1
repl_backlog_histlen:20376
localhost:6380> 
```

##### 21、哨兵模式（自动选举主服务器的模式）

> 主从切换技术的方法是：当主服务器宕机后，需要手动把一台从服务器切换为主服务器，这就需要人工干预，费事费力，还会造成一段时间内服务不可用。这不是一种推荐的方式，更多时候，我们优先考虑“哨兵模式”。Redis从2.8开始正式提供了Sentinel（哨兵）架构来解决这个问题。

###### 21.1、在config目录下创建哨兵配置文件“sentinel.conf”

 ```bash
 [root@alanCentos01 config]# touch sentinel.conf
 [root@alanCentos01 config]# ll
 总用量 368
 -rw-r--r-- 1 root root 93822 9月   4 16:51 redis6379.conf
 -rw-r--r-- 1 root root 93864 9月   4 17:51 redis6380.conf
 -rw-r--r-- 1 root root 93865 9月   4 17:54 redis6381.conf
 -rw-r--r-- 1 root root 93810 9月   4 12:22 redis.conf
 -rw-r--r-- 1 root root     0 9月   5 11:46 sentinel.conf
 ```

###### 21.2、编写哨兵配置文件“sentinel.conf”

```bash
[root@alanCentos01 config]# vim sentinel.conf
```

```properties
# Example sentinel.conf

# 哨兵sentinel实例运行的端口 默认26379
port 26379

# 哨兵sentinel的工作目录
dir "tmp"

# 服务以守护进程的方式启动（后台启动）
daemonize yes

logfile "sentinel26379.log"

# 哨兵sentinel监控的redis主节点的 ip port 
# master-name  可以自己命名的主节点名字 只能由字母A-z、数字0-9 、这三个字符".-_"组成。
# quorum 配置多少个sentinel哨兵统一认为master主节点失联 那么这时客观上认为主节点失联了
# sentinel monitor <master-name> <ip> <redis-port> <quorum>
sentinel monitor myRedisMaster 127.0.0.1 6379 1

# 当在Redis实例中开启了“requirepass foobared”授权密码，这样所有连接Redis实例的客户端都要提供密码
# 设置哨兵sentinel连接主从的密码,注意必须为主从设置一样的验证密码
# sentinel auth-pass <master-name> <password>
sentinel auth-pass myRedisMaster 123456

# 指定多少毫秒之后，主节点没有应答哨兵sentinel,此时,哨兵主观上认为主节点下线,默认30秒
# sentinel down-after-milliseconds <master-name> <milliseconds>
sentinel down-after-milliseconds myRedisMaster 30000

# 这个配置项指定了在发生failover主备切换时最多可以有多少个slave同时对新的master进行同步，
# 这个数字越小，完成failover所需的时间就越长，
# 但是如果这个数字越大，就意味着越多的slave因为replication而不可用。
# 可以通过将这个值设为 1 来保证每次只有一个slave 处于不能处理命令请求的状态。
# sentinel parallel-syncs <master-name> <numslaves>
sentinel parallel-syncs myRedisMaster 1

# 故障转移的超时时间 failover-timeout 可以用在以下这些方面： 
# 1.同一个sentinel对同一个master两次failover之间的间隔时间。
# 2.当一个slave从一个错误的master那里同步数据开始计算时间。直到slave被纠正为向正确的master那里同步数据时。
# 3.当想要取消一个正在进行的failover所需要的时间。  
# 4.当进行failover时，配置所有slaves指向新的master所需的最大时间。不过，即使过了这个超时，slaves依然会被正确配置为指向master，但是就不按parallel-syncs所配置的规则来了
# 5.默认三分钟
# sentinel failover-timeout <master-name> <milliseconds>
sentinel failover-timeout myRedisMaster 180000

# SCRIPTS EXECUTION
# 配置当某一事件发生时所需要执行的脚本，可以通过脚本来通知管理员，例如当系统运行不正常时发邮件通知相关人员。
# 对于脚本的运行结果有以下规则：
# 若脚本执行后返回1，那么该脚本稍后将会被再次执行，重复次数目前默认为10
# 若脚本执行后返回2，或者比2更高的一个返回值，脚本将不会重复执行。
# 如果脚本在执行过程中由于收到系统中断信号被终止了，则同返回值为1时的行为相同。
# 一个脚本的最大执行时间为60s，如果超过这个时间，脚本将会被一个SIGKILL信号终止，之后重新执行。

# 通知型脚本:当sentinel有任何警告级别的事件发生时（比如说redis实例的主观失效和客观失效等等），将会去调用这个脚本，
# 这时这个脚本应该通过邮件，SMS等方式去通知系统管理员关于系统不正常运行的信息。
# 调用该脚本时，将传给脚本两个参数，一个是事件的类型，一个是事件的描述。
# 如果sentinel.conf配置文件中配置了这个脚本路径，那么必须保证这个脚本存在于这个路径，并且是可执行的，否则sentinel无法正常启动成功。
# 通知脚本
# sentinel notification-script <master-name> <script-path>
sentinel notification-script myRedisMaster /var/redis/notify.sh

# 客户端重新配置主节点参数脚本
# 当一个master由于failover而发生改变时，这个脚本将会被调用，通知相关的客户端关于master地址已经发生改变的信息。
# 以下参数将会在调用脚本时传给脚本:
# <master-name> <role> <state> <from-ip> <from-port> <to-ip> <to-port>
# 目前<state>总是“failover”,<role>是“leader”或者“observer”中的一个。 
# 参数 from-ip, from-port, to-ip, to-port是用来和旧的master和新的master(即旧的slave)通信的
# 这个脚本应该是通用的，能被多次调用，不是针对性的。
# sentinel client-reconfig-script <master-name> <script-path>
sentinel client-reconfig-script myRedisMaster /var/redis/reconfig.sh
```

```bash
# 创建哨兵工作目录，存放日志和数据
[root@alanCentos01 bin]# mkdir /usr/local/bin/tmp
```

> ==注意：如果要创建哨兵集群，只需创建新的sentinel.conf，如sentinel26380.conf，在配置文件中更改port和logfile即可。==

###### 21.3、创建通知脚本“notify.sh”

```bash
[root@alanCentos01 config]# cd /var
[root@alanCentos01 var]# mkdir redis
[root@alanCentos01 var]# cd redis
[root@alanCentos01 redis]# touch notify.sh
[root@alanCentos01 redis]# ll
总用量 0
-rw-r--r-- 1 root root 0 9月   5 13:02 notify.sh
[root@alanCentos01 redis]# cd /var
[root@alanCentos01 var]# cd log/
[root@alanCentos01 log]# mkdir redis-logstash
[root@alanCentos01 redis-logstash]# cd /var/redis/
[root@alanCentos01 redis]# vim notify.sh
```

```properties
echo "redis master failovered at `date` " > /var/log/redis-logstash/redis_failover.log
```

```bash
[root@alanCentos01 redis]# chmod 777 notify.sh
```

###### 21.4、创建通知脚本“reconfig.sh”

```bash
[root@alanCentos01 redis]# cd /var/redis/
[root@alanCentos01 redis]# touch reconfig.sh
[root@alanCentos01 redis]# ll
总用量 4
-rwxrwxrwx 1 root root 100 9月   5 13:18 notify.sh
-rw-r--r-- 1 root root   0 9月   5 13:20 reconfig.sh
[root@alanCentos01 redis]# vim reconfig.sh
```

```properties
#!/bin/bash

check_time=$(date +"%F-%T")
master_name="$1"
from_ip="$4"
from_port="$5"
to_ip="$6"
to_port="$7"

# 填写自己正确的机器人链接
# curl 'https://qyapi.weixin.qq.com/cgi-bin/webhook/send?key=xxxx' \
#   -H 'Content-Type: application/json' \
#   -d '
#   {
#        "msgtype": "text",
#        "text": {
#            "content": "【'$check_time' '$master_name' redis failover】\nfrom\n'$from_ip:$from_port'\nto\n'$to_ip:$to_port'",
#            "mentioned_list":["xiaodongl"]
#        }
#   }'

echo -e "redis master failover master reconfig 【'$check_time' '$master_name' redis failover】\nfrom\n'$from_ip:$from_port'\nto\n'$to_ip:$to_port'" > /var/log/redis-logstash/redis_failover_reconfig.log
```


```bash
[root@alanCentos01 redis]# chmod 777 reconfig.sh
```

###### 21.5、启动哨兵

```bash
[root@alanCentos01 bin]# ./redis-sentinel config/sentinel.conf
# 如果有哨兵集群
[root@alanCentos01 bin]# ./redis-sentinel config/sentinel26380.conf
```

###### 21.6、哨兵模式的优缺点

> 优点：
>
> 1. 哨兵集群模式是基于主从模式的，所有主从的优点，哨兵模式同样具有。
> 2. 主从可以切换，故障可以转移，系统可用性更好。
> 3. 哨兵模式是主从模式的升级，系统更健壮，可用性更高。
>
> 缺点：
>
> 1. Redis较难支持在线扩容，在集群容量达到上限时在线扩容会变得很复杂。
> 2. 实现哨兵模式的配置也不简单，甚至可以说有些繁琐。

##### 22、缓存穿透

> Redis缓存的使用，极大的提升了应用程序的性能和效率，特别是数据查询方面。但同时，它也带来了一些问题。其中，最要害的问题，就是数据的一致性问题，从严格意义上讲，这个问题无解。如果对数据的一致性要求很高，那么就不能使用缓存。另外的一些典型问题就是，缓存穿透、缓存雪崩和缓存击穿。

> 缓存穿透：缓存穿透的概念很简单，用户想要查询一个数据，发现redis内存数据库没有，也就是缓存没有命中，于是向持久层数据库查询。发现也没有，于是本次查询失败。当用户很多的时候，缓存都没有命中，于是都去请求了持久层数据库。这会给持久层数据库造成很大的压力，这时候就相当于出现了缓存穿透。

> 解决方案：
>
> 1. 布隆过滤器：布隆过滤器是一种数据结构，对所有可能查询的参数以hash形式存储，在控制层先进行校验，不符合则丢弃，从而避免了对底层存储系统的查询压力。
> 2. 缓存空对象：当存储层不命中后，即使返回的空对象也将其缓存起来，同时会设置一个过期时间，之后再访问这个数据将会从缓存中获取，保护了后端数据源。

> 但是这种方法会存在两个问题：
>
> 1. 如果空值能够被缓存起来，这就意味着缓存需要更多的空间存储更多的键，因为这当中可能会有很多的空值的键；
> 2. 即使对空值设置了过期时间，还是会存在缓存层和存储层的数据会有一段时间窗口的不一致，这对于需要保持一致性的业务会有影响。

##### 23、缓存击穿

> 这里需要注意和缓存击穿的区别，缓存击穿，是指一个key非常热点，在不停的扛着大并发，大并发集中对这一个点进行访问，当这个key在失效的瞬间，持续的大并发就穿破缓存，直接请求数据库，就像在一个屏障上凿开了一个洞。
>
> 当某个key在过期的瞬间，有大量的请求并发访问，这类数据一般是热点数据，由于缓存过期，会同时访问数据库来查询最新数据，并且回写缓存，会导使数据库瞬间压力过大。

> 解决方案：
>
> 1. 设置热点数据永不过期，从缓存层面来看，没有设置过期时间，所以不会出现热点 key 过期后产生的问题。
> 2. 加互斥锁：使用分布式锁，保证对于每个key同时只有一个线程去查询后端服务，其他线程没有获得分布式锁的权限，因此只需要等待即可。这种方式将高并发的压力转移到了分布式锁，因此对分布式锁的考验很大。

##### 24、缓存雪崩

> 缓存雪崩，是指在某一个时间段，缓存集中过期失效，或者Redis宕机。
>
> 产生雪崩的原因之一，比如马上就要到双十二零点，很快就会迎来一波抢购，这波商品时间比较集中的放入了缓存，假设缓存一个小时。那么到了凌晨一点钟的时候，这批商品的缓存就都过期了。而对这批商品的访问查询，都落到了数据库上，对于数据库而言，就会产生周期性的压力波峰。于是所有的请求都会达到存储层，存储层的调用量会暴增，造成存储层也会挂掉的情况。

> 解决方案：
>
> 1. redis高可用：这个思想的含义是，既然redis有可能挂掉，那我多增设几台redis，这样一台挂掉之后其他的还可以继续工作，其实就是搭建的集群。
> 2. 限流降级：这个解决方案的思想是，在缓存失效后，通过加锁或者队列来控制读数据库写缓存的线程数量。比如对某个key只允许一个线程查询数据和写缓存，其他线程等待。
> 3. 数据预热：数据加热的含义就是在正式部署之前，我先把可能的数据先预先访问一遍，这样部分可能大量访问的数据就会加载到缓存中。在即将发生大并发访问前手动触发加载缓存不同的key，设置不同的过期时间，让缓存失效的时间点尽量均匀。

##### 25、完结，撒花！！！
