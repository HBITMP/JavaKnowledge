

#### mongodb简介

> MongoDB是一个跨平台，面向文档的数据库，提供高性能，高可用性和易于扩展。MongoDB是工作在集合和文档上一种概念。

#####数据库

数据库是一个集合的物理容器。每个数据库获取其自己设定在文件系统上的文件。一个单一的MongoDB服务器通常有多个数据库。

##### 集合

集合是一组MongoDB的文件。它与一个RDBMS表是等效的。一个集合存在于数据库中。集合不强制执行模式。集合中的文档可以有不同的字段。通常情况下，在一个集合中的所有文件都是类似或相关目的。

##### 文档

文档是一组键值对。文档具有动态模式。动态模式是指，在同一个集合的文件不必具有相同一组集合的文档字段或结构，并且相同的字段可以保持不同类型的数据。

##### 示例文档

```json
{
   _id: ObjectId(7df78ad8902c)
   title: 'MongoDB Overview', 
   description: 'MongoDB is no sql database',
   by: 'yiibai tutorial',
   url: 'http://www.yiibai.com',
   tags: ['mongodb', 'database', 'NoSQL'],
   likes: 100, 
   comments: [	
      {
         user:'user1',
         message: 'My first comment',
         dateCreated: new Date(2011,1,20,2,15),
         like: 0 
      },
      {
         user:'user2',
         message: 'My second comments',
         dateCreated: new Date(2011,1,25,7,45),
         like: 5
      }
   ]
}
```

##### MongoDB在哪些场景

- 大而复杂的数据
- 移动和社会基础设施数据
- 内容管理和交付
- 用户数据管理
- 数据中心



#### 软件安装

##### [Ubuntu 16.04](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-ubuntu/)

```shell
# Import the public key used by the package management system.
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 9DA31620334BD75D9DCB49F368818C72E52529D4

# Create a list file for MongoDB
echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu xenial/mongodb-org/4.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-4.0.list

# Reload local package database
sudo apt-get update

# Install the MongoDB packages
sudo apt-get install -y mongodb-org --allow-unauthenticated

# Run MongoDB Community Edition
# Start MongoDB
sudo service mongod start

# Stop MongoDB
sudo service mongod stop

# Restart MongoDB
sudo service mongod restart

# Begin using MongoDB
mongo --host 127.0.0.1:27017

# Uninstall MongoDB Community Edition
# Stop MongoDB
sudo service mongod stop

# Remove Packages
sudo apt-get purge mongodb-org*

# Remove Data Directories
sudo rm -r /var/log/mongodb
sudo rm -r /var/lib/mongodb
```



#### 启动MongoDB

如果安装 MongoDB 在不同的位置（建议安装到 D:\software），那么需要设置路径 dbpath 在 mongod.exe 指向 data 备用路径。

```mysql
D:\software\MongoDB\Server\3.0\bin> mongod.exe --dbpath "备用路径[data的路径]" 
```

这将显示在等待连接的控制台输出消息，指示 mongod.exe 成功运行过程。【不要关闭当前的窗口】

另启动一个终端窗口，现在运行的MongoDB，需要打开一个命令提示符，发出以下命令

```mysql
D:\software\MongoDB\Server\3.0\bin>mongo.exe
```

####数据类型

**下列为MongoDB中常用的几种数据类型：**

- Object ID：文档ID
- String：字符串，最常用，必须是有效的UTF-8
- Boolean：存储一个布尔值，true或false
- Integer：整数可以是32位或64位，这取决于服务器
- Double：存储浮点值
- Arrays：数组或列表，多个值存储到一个键
- Object：用于嵌入式的文档，即一个值为一个文档
- Null：存储Null值
- Timestamp：时间戳
- Date：存储当前日期或时间的UNIX时间格式



#### object id

- 每个文档都有一个属性，为_id，保证每个文档的唯一性
- 可以自己去设置_id插入文档
- 如果没有提供，那么MongoDB为每个文档提供了一个独特的_id，类型为objectID
- objectID是一个12字节的十六进制数
  - 前4个字节为当前时间戳
  - 接下来3个字节的机器ID
  - 接下来的2个字节中MongoDB的服务进程id
  - 最后3个字节是简单的增量值

####一、操作mongodb数据库

#####1、创建数据库

​        语法：use 数据库名
        注意：如果数据库不存在则创建数据库，否则切换到指定的数据库
        注意：如果刚刚创建的数据库不在列表内，如果要显示它，我们需要向刚刚创建的数据库中插入一些数据 

```mysql
use  student

db.student.insert({"name":"tom", "age":18,"gender":1,"address":"北京", "isDelete":0}) 
```


   ##### 2、删除数据库
```mysql
#前提：使用当前数据库(use 数据库名)
db.dropDatabase()
```
#####3、查看所有数据
```mysql
show dbs
```

   ##### 4、查看当前正在使用的数据库

```mysql
    a、db
    b、db.getName()
```
#####5、断开连接
```mysql
exit
```

   ##### 6、查看命令api

```mysql
    help
```

####二、集合操作

#####1、查看当前数据库下有哪些集合

```mysql
 show collections
```

#####2、创建集合

 a、 语法：db.createCollection("集合名") 

```mysql
db.createCollection("class")
```

  b、语法：db.集合名.insert(文档)

```mysql
db.student.insert({name:"tom", age:18, gender:1,address:"北京", isDelete:0})
```

>  区别：两者的区别在于前者创建的是一个空的集合，后者创建一个空的集合并添加一个文档

##### 3、删除当前数据库中的集合

语法：db.集合名.drop()

```mysql
db.class.drop()
```

####三、文档操作

#####1、插入文档

 a、使用insert()方法插入文档
  	语法：db.集合名.insert(文档)

​	语法：db.集合名.insert([文档1, 文档2, ……, 文档n])

在插入的文档中，如果不指定`_id`参数，那么 MongoDB 会为此文档分配一个唯一的ObjectId。

`_id`为集合中的每个文档唯一的`12`个字节的十六进制数。

> 插入一个学生

```mysql
db.student.insert({name:"lilei", age:19, gender:1,address:"北京", isDelete:0})
```

> 插入多个学生：

```mysql
db.student.insert([{name:"海妹妹", age:17, gender:0,address:"北京", isDelete:0},{name:"韩梅梅", age:20, gender:0,address:"上海", isDelete:0}])
```

其他的插入方法：

1、`db.collection.insertOne()`方法将单个文档插入到集合中。以下示例将新文档插入到库存集合中。 如果文档没有指定`_id`字段，MongoDB会自动将`_id`字段与`ObjectId`值添加到新文档。

2、`db.collection.insertMany()`方法。以下示例将三个新文档插入到库存集合中。如果文档没有指定`_id`字段，MongoDB会向每个文档添加一个ObjectId值的`_id`字段。

 

b、使用save()方法插入文档
       语法：db.集合名.save(文档)

​	说明：如果不指定_id字段，save()方法类似于insert()方法。如果指定_id字段，则会更新_id字段的数据

```mysql
db.student.save({name:"poi", age:22, gender:1,address:"石家庄", isDelete:0})
```

```mysql
db.student.save({_id:ObjectId("59950962019723fe2a0d8d17"),name:"poi", age:23, gender:1,address:"石家庄", isDelete:0})
```

#####2、文档更新

​    a、update()方法用于更新已存在的文档

> 语法：

```mysql
db.集合名.update(
            query,
            update,
            {
                upset:<boolean>,
                multi:<boolean>,
                writeConcern:<document>
            }
        )
```

参数说明：
 query：update的查询条件，类似于sql里update语句内where后面的内容

 update：update的对象和一些更新的操作符($set,$inc)等，$set直接更新，$inc在原有的基础上累加后更新

 upset：可选，如果不存在update的记录，是否当新数据插入，true为插入，False为不插入，默认为false。

multi：可选，mongodb默认是false，只更新找到的第一条记录，如果这个参数为true,就按照条件查找出来的数据全部更新writeConcern：可选，抛出异常的级别

> 需求：将lilei的年龄更新为25

```mysql
 db.student.update({name:"lilei"},{$set:{age:25}})
```

>累加：

```mysql
 db.student.update({name:"lilei"},{$inc:{age:25}}) 
```

> 全改：

```mysql
db.student.update({name:"poi"},{$set:{age:42}},{multi:true}) 
```

b、save()方法通过传入的文档替换已有文档

> 语法：

```mysql
 db.集合名.save(
                document,
                {
                    writeConcern:<document>
                }
            )
```

参数说明：
 document：文档数据
  writeConcern：可选，抛出异常的级别

#####3、文档删除

> 说明：在执行remove()函数前，先执行find()命令来判断执行的条件是否存在是一个良好习惯

语法：

```mysql
 db.集合名.remove(
            query,
            {
                justOne:<boolean>,
                writeConcern:<document>
            }
        )
```

参数说明：
        query：可选，删除的文档的条件
        justOne：可选，如果为true或1，则只删除一个文档
        writeConcern：可选，抛出异常的级别

```mysql
db.student.remove({name:"poi"})
```

#####4、文档查询

a、find()方法

> 语法：db.集合名.find() 

查询集合下所有的文档(数据)：

```mysql
db.student.find()
```

 b、find()方法查询指定列

```mysql
语法：db.集合名.find(
            query,
            {
                <key>:1,
                <key>:1
            }
        )
```

参数说明：
            query：查询条件
            key：要显示的字段，1表示显示

```mysql
 db.student.find({gender:0},{name:1,age:1})
 db.student.find({},{name:1,age:1})
```

c、pretty()方法以格式化的方式来显示文档

```mysql
db.student.find().pretty()
```

d、findOne()方法查询匹配结果的第一条数据

```mysql
db.student.findOne({gender:0})
```

e、消除重复

> 方法distinct()对数据进行去重

语法：

```
db.集合名称.distinct('去重字段',{条件})
```

> 例:查找年龄大于18的性别（去重）

```
db.student.distinct('gender',{age:{$gt:18}})
```

#####5、查询条件操作符

> 作用：条件操作符用于比较两个表达式并从Mongodb集合中获取数据

 a、大于 - $gt

```mysql
#语法：
db.集合名.find({<key>:{$gt:<value>}})
#示例：
db.student.find({age:{$gt:20}})
```

b、大于等于 - $gte

```mysql
#语法：
db.集合名.find({<key>:{$gte:<value>}})
```

c、小于 - $lt

```mysql
#语法：
db.集合名.find({<key>:{$lt:<value>}})
```

 d、小于等于 - $lte

```mysql
#语法：
db.集合名.find({<key>:{$lte:<value>}})
```

 e、大于等于 和 小于等于 - $gte 和 $lte

```mysql
 #语法：
 db.集合名.find({<key>:{$gte:<value>,$lte:<value>}})
```

 f、等于 - :

```mysql
#语法：
db.集合名.find({<key>:<value>})
```

g、使用_id进行查询

```mysql
#语法：
db.student.find({"_id":ObjectId("id值")})

#示例db.student.find({"_id":ObjectId("5995084b019723fe2a0d8d14")})
```

 h、查询某个结果集的数据条数

```mysql
db.student.find().count()
```

i、查询某个字段的值当中是否包含另一个值

```mysql
#语法：示例：
db.student.find({name:/ile/})
```

 j、查询某个字段的值是否以另一个值开头

```mysql
#示例：
db.student.find({name:/^li/})
```

| 操作     | 语法                     | 示例                                          | RDBMS等效语句         |
| -------- | ------------------------ | --------------------------------------------- | --------------------- |
| 相等     | `{<key>:<value>}`        | `db.mycol.find({"by":"yiibai"}).pretty()`     | `where by = 'yiibai'` |
| 小于     | `{<key>:{$lt:<value>}}`  | `db.mycol.find({"likes":{$lt:50}}).pretty()`  | `where likes < 50`    |
| 小于等于 | `{<key>:{$lte:<value>}}` | `db.mycol.find({"likes":{$lte:50}}).pretty()` | `where likes <= 50`   |
| 大于     | `{<key>:{$gt:<value>}}`  | `db.mycol.find({"likes":{$gt:50}}).pretty()`  | `where likes > 50`    |
| 大于等于 | `{<key>:{$gte:<value>}}` | `db.mycol.find({"likes":{$gte:50}}).pretty()` | `where likes >= 50`   |
| 不等于   | `{<key>:{$ne:<value>}}`  | `db.mycol.find({"likes":{$ne:50}}).pretty()`  | `where likes != 50`   |

 



##### 6、条件查询and 和 or

 a、AND条件

> 在`find()`方法中，如果通过使用’`，`‘将它们分开传递多个键，则 MongoDB 将其视为`AND`条件

```mysql
 #语法：
 db.集合名.find({条件1,条件2,……,条件n})
        
 #示例：
 db.student.find({gender:0,age:{$gt:16}})
```

```mysql
#第二种写法
db.student.find(
   {
      $and: [
         {key1: value1}, {key2:value2}
      ]
   }
)

db.student.find({$and:[{gender:0},{age:20}]})
```

b、OR条件

```mysql
 #语法：
            db.集合名.find(
                {
                    $or:[{条件1},{条件2},……,{条件n}]
                }
            )
```

```mysql
#示例：
db.student.find({$or:[{age:17},{age:{$gte:20}}]})
```

 c、AND和OR联合使用

```mysql
#语法：
            db.集合名.find(
                {
                    条件1,
                    条件2,
                    $or:[{条件3},{条件4}]
                }
            )
```

##### 7、limit、skip

a、limit()：读取指定数量的数据记录

```mysql
db.student.find().limit(3)
```

b、skip()：跳过指定数量的数据

```mysql
db.student.find().skip(3)
```

c、skip与limit联合使用

通常用这种方式来实现分页功能

```mysql
db.student.find().skip(3).limit(3)
```

##### 8、排序

```mysql
#语法：
db.集合名.find().sort({<key>:1|-1})
#示例：
db.student.find().sort({age:1})   
```

 注意：1表示升序，-1表示降序

##### 9.范围运算符

```
使用"$in"，"$nin" 判断是否在某个范围内
```

> 例：查询年龄为18、28的学生

```mysql
db.student.find({age:{$in:[18,28]}})
```

##### 10、支持正则表达式

> 使用//或$regex编写正则表达式

> 查询姓黄的学生

```mysql
db.stu.find({name:/^黄/})
db.stu.find({name:{$regex:'^黄'}}})
```

 ##### 11、自定义查询

> 使用$where后面写一个函数，返回满足条件的数据

> 例：查询年龄大于30的学生

```mysql
db.stu.find({$where:function(){return this.age>20}})
```



#### 三、python and mongodb

##### 1. 安装PyMongo

```python
pip3 install  PyMongo
```

##### 2. 建立连接

使用PyMongo时，第一步是运行 mongod 实例创建一个MongoClient。如下：

```python
from pymongo import MongoClient
client = MongoClient()
```

上述代码将连接默认主机和端口。 也可以明确指定主机和端口，如下所示：

```python
from pymongo import MongoClient
#client = MongoClient()
client = MongoClient('localhost', 27017)
```

或使用MongoDB URI格式：

```python
client = MongoClient('mongodb://localhost:27017/')
```

##### 3. 获取数据库

MongoDB的一个实例可以支持多个独立的数据库。 在使用PyMongo时，可以使用MongoClient实例上的属性的方式来访问数据库：

```python
#client.数据库名
db = client.pythondb
```

如果数据库名称使用属性方式访问无法正常工作(如：`python-db`)，则可以使用字典样式访问：

```python
db = client['python-db']
```

##### 4. 获取集合

集合是存储在MongoDB中的一组文档，可以类似于关系数据库中的表。 在PyMongo中获取集合的工作方式与获取数据库相同：

```python
collection = db.python_collection
```

或(使用字典方式访问)：

```python
collection = db['python-collection']
```

##### 5. 文档

MongoDB中的数据使用JSON方式来表示文档(并存储)。 在PyMongo中使用字典来表示文档。例如，以下字典可能用于表示博客文章：

```python
import datetime
from pymongo import MongoClient
client = MongoClient()

post = {"author": "Mike",
         "text": "My first blog post!",
         "tags": ["mongodb", "python", "pymongo"],
         "date": datetime.datetime.utcnow()}
```

##### 6. 插入文档

> 要将文档插入到集合中，可以使用`insert_one()`方法：

```python
#!/usr/bin/python3
#coding=utf-8

import datetime
from pymongo import MongoClient
client = MongoClient()

db = client.pythondb

post = {"author": "Maxsu",
         "text": "My first blog post!",
         "tags": ["mongodb", "python", "pymongo"],
         "date": datetime.datetime.utcnow()}

posts = db.posts
post_id = posts.insert_one(post).inserted_id
print ("post id is ", post_id)
db.close()
```

插入文档时，如果文档尚未包含“`_id`”键，则会自动添加“`_id`”。 “`_id`”的值在集合中必须是唯一的。 `insert_one()`返回一个`InsertOneResult`的实例



> 或者直接插入

```python
from pymongo import MongoClient

# 连接服务器
conn = MongoClient("localhost", 27017)

# 连接数据库
db = conn.mydb

# 获取集合
collection = db.student

#添加文档
#collection.insert({"name":"abc", "age":19, "gender":1,"address":"北京", "isDelete":0})
collection.insert([{"name":"abc1", "age":19, "gender":1,"address":"北京", "isDelete":0},{"name":"abc2", "age":19, "gender":1,"address":"北京", "isDelete":0}])

#断开
conn.close()
```

##### 7. 查询文档

```python
import pymongo
from pymongo import MongoClient
from bson.objectid import ObjectId#用于ID查询

# 连接服务器
conn = MongoClient("localhost", 27017)

# 连接数据库
db = conn.mydb

# 获取集合
collection = db.student

# 查询文档

# 查询部分文档
res = collection.find({"age":{"$gt":18}})
for row in res:
    print(row)
    print(type(row))


# 查询所有文档
res = collection.find()
for row in res:
    print(row)
    print(type(row))

#统计查询
res = collection.find({"age":{"$gt":18}}).count()
print(res)


#根据id查询

res = collection.find({"_id":ObjectId("5995084b019723fe2a0d8d14")})
print(res[0])


# 排序
res = collection.find().sort("age")#升序
res = collection.find().sort("age", pymongo.DESCENDING)#降序
for row in res:
    print(row)

# 分页查询
res = collection.find().skip(3).limit(5)
for row in res:
    print(row)

#断开
conn.close()
```

##### 8.更新文档

```python
from pymongo import MongoClient

# 连接服务器
conn = MongoClient("localhost", 27017)
# 连接数据库
db = conn.mydb
# 获取集合
collection = db.student

collection.update({"name":"lilei"},{"$set":{"age":25}})

#断开
conn.close()
```

##### 9.删除文档

```python
from pymongo import MongoClient

# 连接服务器
conn = MongoClient("localhost", 27017)
# 连接数据库
db = conn.mydb
# 获取集合
collection = db.student

collection.remove({"name":"lilei"})
#全部删除
collection.remove()
#断开
conn.close()
```





拓展：

给mongodb设置密码

https://segmentfault.com/a/1190000011554055

####聚合 aggregate

> 聚合(aggregate)主要用于计算数据，类似sql中的sum()、avg()

语法

```
db.集合名称.aggregate([{管道:{表达式}}])
```

#### 管道

- 管道在Unix和Linux中一般用于将当前命令的输出结果作为下一个命令的输入

```
ps ajx | grep mongo
```

- 在mongodb中，管道具有同样的作用，文档处理完毕后，通过管道进行下一次处理
- 常用管道
  - $group：将集合中的文档分组，可用于统计结果
  - $match：过滤数据，只输出符合条件的文档
  - $project：修改输入文档的结构，如重命名、增加、删除字段、创建计算结果
  - $sort：将输入文档排序后输出
  - $limit：限制聚合管道返回的文档数
  - $skip：跳过指定数量的文档，并返回余下的文档
  - $unwind：将数组类型的字段进行拆分



#### 表达式

> 处理输入文档并输出

语法

```
表达式:'$列名'
```

常用表达式

- $sum：计算总和，$sum:1同count表示计数
- $avg：计算平均值
- $min：获取最小值
- $max：获取最大值
- $push：在结果文档中插入值到一个数组中
- $first：根据资源文档的排序获取第一个文档数据
- $last：根据资源文档的排序获取最后一个文档数据



#####1.$group

> 将集合中的文档分组，可用于统计结果

> _id表示分组的依据，使用某个字段的格式为'$字段'

> 例：统计男生、女生的总人数

```mysql
db.student.aggregate([
    {$group:
        {
            _id:'$gender',
            counter:{$sum:1}
        }
    }
])
```

#####Group by null

> 将集合中所有文档分为一组

> 例：求学生总人数、平均年龄

```mysql
db.student.aggregate([
    {$group:
        {
            _id:null,
            counter:{$sum:1},
            avgAge:{$avg:'$age'}
        }
    }
])
```



 ##### 2.$match

> 用于过滤数据，只输出符合条件的文档

使用MongoDB的标准查询操作

> 例：查询年龄大于20的学生

```mysql
db.student.aggregate([
    {$match:{age:{$gt:20}}}
])
```

> 例：查询年龄大于20的男生、女生人数

```mysql
db.student.aggregate([
    {$match:{age:{$gt:20}}},
    {$group:{_id:'$gender',counter:{$sum:1}}}
])
```

#####3.$project

> 修改输入文档的结构，如重命名、增加、删除字段、创建计算结果

> 例：查询学生的姓名、年龄

```mysql
db.student.aggregate([
    {$project:{_id:0,name:1,age:1}}
])
```

> 例2：查询男生、女生人数，输出人数

```mysql
db.stu.aggregate([
    {$group:{_id:'$gender',counter:{$sum:1}}},
    {$project:{_id:0,counter:1}}
])
```

#####4.$sort

> 将输入文档排序后输出

> 例：查询学生信息，按年龄升序

```mysql
db.student.aggregate([{$sort:{age:1}}])
```

> 例：查询男生、女生人数，按人数降序

```mysql
db.student.aggregate([
    {$group:{_id:'$gender',counter:{$sum:1}}},
    {$sort:{counter:-1}}
])
```

#####5.$limit

> 限制聚合管道返回的文档数

> 例：查询2条学生信息

```mysql
db.student.aggregate([{$limit:2}])
```

#####6.$skip

> 跳过指定数量的文档，并返回余下的文档

> 例：查询从第3条开始的学生信息

```mysql
db.student.aggregate([{$skip:2}])
```

> 例：统计男生、女生人数，按人数升序，取第二条数据

```mysql
db.student.aggregate([
    {$group:{_id:'$gender',counter:{$sum:1}}},
    {$sort:{counter:1}},
    {$skip:1},
    {$limit:1}
])
```

**注意顺序：先写skip，再写limit**



####超级管理员

> 为了更安全的访问mongodb，需要访问者提供用户名和密码，于是需要在mongodb中创建用户

 采用了角色-用户-数据库的安全管理方式

常用系统角色如下：

- root：只在admin数据库中可用，超级账号，超级权限
- Read：允许用户读取指定数据库
- readWrite：允许用户读写指定数据库



> 创建超级管理用户

```
use admin
db.createUser({
    user:'admin',
    pwd:'123',
    roles:[{role:'root',db:'admin'}]
})
```

####启用安全认证

> 修改配置文件

```
sudo vi /etc/mongod.conf
```

> 启用身份验证

**注意：keys and values之间一定要加空格, 否则解析会报错**

```shell
security:
  authorization: enabled
```

> 重启服务

```shell
sudo service mongod stop
sudo service mongod start
```

> 终端连接

```shell
 mongo -u 'admin' -p '123' --authenticationDatabase 'admin'
```



 #### 普通用户管理

> 使用超级管理员登录，然后进入用户管理操作

> 查看当前数据库的用户

```
use test1
show users
```

> 创建普通用户

```
db.createUser({
    user:'t1',
    pwd:'123',
    roles:[{role:'readWrite',db:'test1'}]
})
```

> 终端连接

```
mongo -u t1 -p 123 --authenticationDatabase test1
```

> 切换数据库，执行命令查看效果

> 修改用户：可以修改pwd、roles属性

```
db.updateUser('t1',{pwd:'456'})
```