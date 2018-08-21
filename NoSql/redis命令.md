#### 安装redis

sudo apt install redis

sudo yum install redis

查看版本

redis-server -v

#### 局域网链接

1. 修改配置文件

   1. 注释：bind 127.0.0.1
   2. 修改保护模式：protected-mode no

2. 设置密码

   vim /etc/redis/6379.conf

   修改：requirepass  youpassword

3. 关闭防火墙

   1. CentOS

      /sbin/iptables -I INPUT -p tcp --dport 6379 -j ACCEPT  

      /etc/rc.d/init.d/iptables save

   2. Ubuntu

      sudo apt install ufw

      sudo ufw allow 6379

4. 重启

   service redis stop

   service redis start 



#### redis简介

Redis是一个开源，高级的键值存储和一个适用的解决方案，用于构建高性能，可扩展的Web应用程序。

Redis有三个主要特点，使它优越于其它键值数据存储系统 -

- Redis将其数据库完全保存在内存中，仅使用磁盘进行持久化。
- 与其它键值数据存储相比，Redis有一组相对丰富的数据类型。
- Redis可以将数据复制到任意数量的从机中。

####Redis的优点

以下是**Redis**的一些优点。

- **异常快** - Redis非常快，每秒可执行大约`110000`次的设置(`SET`)操作，每秒大约可执行`81000`次的读取/获取(`GET`)操作。
- **支持丰富的数据类型** - Redis支持开发人员常用的大多数数据类型，例如列表，集合，排序集和散列等等。这使得Redis很容易被用来解决各种问题，因为我们知道哪些问题可以更好使用地哪些数据类型来处理解决。
- **操作具有原子性** - 所有Redis操作都是原子操作，这确保如果两个客户端并发访问，Redis服务器能接收更新的值。
- **多实用工具** - Redis是一个多实用工具，可用于多种用例，如：缓存，消息队列(Redis本地支持发布/订阅)，应用程序中的任何短期数据，例如，web应用程序中的会话，网页命中计数等。



####一、String

>  概述：String是redis最基本的类型，最大能存储512MB的数据，String类型是二进制安全的，即可以存储任何数据、比如数字、图片、序列化对象等

#####1、设置

a、设置键值
set key value

> 例如：

```mysql
set name 'xiaoming'
```

 b、设置键值及过期时间，以秒为单位
  setex key seconds value

> 例如：

```mysql
setex name  10 'xiaoming' 
```

c、设置多个键值
 mset key value [key value ……]

> 例如：

```mysql
mset name "lili"  age 18  sex 'girl'
```

##### 2、获取

a、根据键获取值，如果键不存在则返回None(null 0 nil)
     get key

```mysql
get name
```

 b、根据多个键获取多个值
 mget key [key ……]

```mysql
mget name age
```

#####3、运算

> 要求：值是字符串类型的数字

a、将key对应的值加1
       incr key

> 例如：

```mysql
incr score
```

b、将key对应的值减1
       decr key

```mysql
decr score
```

 c、将key对应的值加整数
       incrby key intnum

```mysql
incrby score 10
```

 d、将key对应的值减整数
       decrby key intnum

```mysql
decrby score  20
```

##### 4、其它

a、追加值
     append key value

```mysql
append name "hello"
```

b、获取值长度
      strlen key

```mysql
strlen name
```

####二、key

#####1、查找键，参数支持正则

​      keys pattern

#####2、判断键是否存在，如果存在返回1，不存在返回0

​        exists key

#####3、查看键对应的value类型

​        type key

#####4、删除键及对应的值

​        del key [key ……]

##### 5、设置过期时间，以秒为单位

​        expire key seconds

##### 6、查看有效时间，以秒为单位

​        ttl key

####三、hash

>  概述：hash用于存储对象

​    {
        naem:"tom",
        age:18
    }

##### 1、设置

a、设置单个值

```mysql
#语法
hset key field value

hset people name "Tom"
hset people age 100

hget people name
hget people age
```

b、设置多个值

```mysql
#语法
hmset key field value [field value ……]

hmset people name "Alice" age 100 sex 1
hgetall people
```

##### 2、获取

 a、获取一个属性的值

```mysql
#语法
hget key field
```

 b、获取多个属性的值

```mysql
 #语法
 hmget key filed [filed ……]
```

 c、获取所有属性和值

```mysql
  #语法
  hgetall key
```

 d、获取所有属性

```mysql
  hkeys key
  
  hkeys people
```

 e、获取所有值

```mysql
 hvals key
 
 hvals people
```

 f、返回包含数据的个数

```mysql
 hlen key
 
 hlen people
```



#####3、其它

a、判断属性是否存在，存在返回1，不存在返回0

```mysql
hexists key field

hexists people name
hexists people nice
```

b、删除属性及值

```mysql
hdel key field [field ……]

hdel people name
hget people name
hget people age
```

  

####四、list

> 概述：列表的元素类型为string，按照插入顺序排序，在列表的头部或尾部添加元素

##### 1、设置

a、在头部插入

```mysql
lpush key value [vlaue ……]

lpush person name "Alice"
lpush person name "Bob"
lpush person name "Jim"
```

 b、在尾部插入

```mysql
 rpush key value [vlaue ……]
 
 rpush person name "a"
```

 c、在一个元素的前|后插入新元素

```mysql
 linsert key before|after pivot value
 
 linsert person before "Jim" "Gim"
```

 d、设置指定索引的元素值

```mysql
  lset key index value
  
  lset person 6 "Tina"
  lrange person 0 16
```

  注意：index从0开始
  注意：索引值可以是负数，表示偏移量是从list的尾部开始，如-1表示最后一个元素

##### 2、获取

a、移除并返回key对应的list的第一个元素     

```mysql
lpop key

lpop person
```

b、移除并返回key对应的list的最后一个元素

```mysql
 rpop key
 
 rpop person
```

c、返回存储在key的列表中的指定范围的元素

```mysql
 lrange key start end
 
 lrange person 0 10
```

​        注意：start end都是从0开始
        注意：偏移量可以是负数

##### 3、其它

a、裁剪列表，改为原集合的一个子集

```mysql
 ltrim key start end
 
 ltrim person 0 5
 lrange person 0 -1
```

​        注意：start end都是从0开始
        注意：偏移量可以是负数
b、返回存储在key里的list的长度

```mysql
 llen key
 
 llen person
```

c、返回列表中索引对应的值  

```mysql
 lindex key index
 
 lindex person 3
```

#####五、set

> 概述：无序集合，元素类型为String类型，元素具有唯一性，不重复

##### 1、设置

​    a、添加元素

```mysql
  sadd key member [member ……]
  
  sadd animal dog cat
```

#####2、获取
a、返回key集合中所有元素


```
 smembers key
 
 smembers animal
```


b、返回集合元素个数

```mysql
scard key

scard animal

```

##### 3、交集

 a、求多个集合的交集     

```mysql
 sinter key [key ……]
 
 sadd animal1 dog cat pig
 sadd animal2 dog pig
 sinter animal animal2
```


  b、求多个集合的差集      

```mysql
sdiff key [key ……]

sdiff animal animal2
```


c、求多个集合的合集       

```mysql
sunion key [key ……]

sunion animal animal2
```


d、判断元素是否在集合中，存在返回1，不存在返回0

```mysql
 sismember key member
 
 sismember animal pig
 sismember animal dog
```



####六、zset

> 概述：a、有序集合，元素类型为Sting，元素具有唯一性，不能重复
>
> b、每个元素都会关联一个double类型的score(表示权重),通过权重的大小排序，元素的score可以相同

#####1、设置

​    a、添加   

```mysql
#语法
zadd key score member [score member ……]
#示例
zadd z1 1 a 5 b 3 c 2 d 4 e
```



#####2、获取

a、返回指定范围的元素

```mysql
zrange key start end

zrange z1 0 -1
```

 b、返回元素个数

```mysql
 zcard key
 
 zcard z1
```

c、返回有序集合key中，score在min和max之间的元素的个数

```mysql
 zcount key min max
 
 zcount z1 1 7
```

 d、返回有序集合key中，成员member的score值

```mysql
zscore key member

zscore z1 f
```



#### 发布订阅机制

- 发布者不是计划发送消息给特定的接收者（订阅者），而是发布的消息分到不同的频道，不需要知道什么样的订阅者订阅
- 订阅者对一个或多个频道感兴趣，只需接收感兴趣的消息，不需要知道什么样的发布者发布的
- 发布者和订阅者的解耦合可以带来更大的扩展性和更加动态的网络拓扑
- 客户端发到频道的消息，将会被推送到所有订阅此频道的客户端
- 客户端不需要主动去获取消息，只需要订阅频道，这个频道的内容就会被推送过来

##### 消息的格式

- 推送消息的格式包含三部分
- part1:消息类型，包含三种类型
  - subscribe，表示订阅成功
  - unsubscribe，表示取消订阅成功
  - message，表示其它终端发布消息
- 如果第一部分的值为subscribe，则第二部分是频道，第三部分是现在订阅的频道的数量
- 如果第一部分的值为unsubscribe，则第二部分是频道，第三部分是现在订阅的频道的数量，如果为0则表示当前没有订阅任何频道，当在Pub/Sub以外状态，客户端可以发出任何redis命令
- 如果第一部分的值为message，则第二部分是来源频道的名称，第三部分是消息的内容

##### 命令

- 订阅

```
SUBSCRIBE 频道名称 [频道名称 ...]
```

- 取消订阅
- 如果不写参数，表示取消所有订阅

```
UNSUBSCRIBE 频道名称 [频道名称 ...]
```

- 发布

```
PUBLISH 频道 消息
```

#### 主从配置

- 一个master可以拥有多个slave，一个slave又可以拥有多个slave，如此下去，形成了强大的多级服务器集群架构
- 比如，将ip为192.168.1.10的机器作为主服务器，将ip为192.168.1.11的机器作为从服务器
- 设置主服务器的配置

```
# 这一句可以注释：bind 10.36.143.139
```

- 设置从服务器的配置
- 注意：在slaveof后面写主机ip，再写端口，而且端口必须写

```
# 这一句可以注释：bind 10.36.143.93
slaveof 10.36.143.139 6379
```

- 在master和slave分别执行info或者info replication命令，查看输出信息
- 在master上写数据

```
set python world
```

- 在slave上读数据

```
get python
```



#### 七、与python交互

1.安装redis客户端模块

pip install redis

```python
import redis

#连接
r = redis.StrictRedis(host="localhost", port=6379, password="sunck")


#方法1：根据数据类型的不同，调用响应的方法
#写
#r.set("p1", "good")
#读
#print(r.get("p1"))

#方法2：pipeline
#缓冲多条命令，然后依次执行，减少服务器-客户端之间的TCP数据包
pipe = r.pipeline()
pipe.set("p2", "nice")
pipe.set("p3", "handsom")
pipe.set("p4", "cool")
pipe.execute()
```

对redis进行封装

```python
import redis


class SunckRedis():
    def __init__(self, passwd, host="localhost", port=6379):
        self.__redis = redis.StrictRedis(host=host, port=port, password=passwd)

    def set(self, key, value):
        self.__redis.set(key, value)
    def get(self,key):
        if self.__redis.exists(key):
            return self.__redis.get(key)
        else:
            return ""
        
        
pipe = SunckRedis(passwd, host, port)
```



