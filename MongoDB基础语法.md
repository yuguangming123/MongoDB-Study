# MongoDB基础语法

## 第二章 MongoDB的安装与配置

### 2.1.1MongoDB安装

下载安装包，直接点下一步即可

### 2.1.2配置环境变量

系统属性——环境变量——path——变量值后添加MongoDB安装路径\bin,

例：E:\MongoDB\bin

### 2.1.3创建数据库存放文件夹

在MongoDB\data目录下创建db文件夹，用于存放数据库文件

### 2.1.4启动MongoDB

- 命令窗口输入：MomgoDB\bin>mongod --dbpath D:\MongoDB\Server\data\db，可查看MongoDB信息。
- 浏览器输入：127.0.0.1:27017，看到提示英文即表示安装成功

#### 2.1.5配置本地MongoDB服务

如果在安装程序的过程中，已经安装过本地服务，那么这个步骤可以省略。

1. data目录下创建log文件夹，用于存放日志

2. 根目录下创建配置文件mongo.config，用于保存配置

3. 编辑配置文件：

   ```
   dbpath=D:\MongoDB\Server\data\db
   logpath=D:\MongoDB\Server\data\log\mongo.log
   ```

4. 管理员身份运行CMD，并进入bin目录

   ```
   mongod -dbpath "D:\MongoDB\Server\data\db" -logpath "D:\MongoDB\Server\data\log\mongo.log" -install -serviceName "MongoDB"
   ```

​		如果命令报错，用sc.exe delet MongoDB指令删除服务，再重复第4步。

5. 基本命令（管理员启动CMD）：

   ```
   启动：net start MongoDB
   停止：net stop MongoDB
   ```

2.1.6 创建一个数据库

1. 安装MongoDB shell
   - 6.0以后得版本不在自带MongoDB shell，需要自己去官网下载安装，网址：https://www.mongodb.com/try/download/shell
   - 将安装包解压到MongoDB跟目录，然后把MongoDB shell文件夹中bin目录的路径添加到系统环境变量。
   - 在CMD中键入mongosh命令后，即可使用CMD命令操作MongoDB数据库了。

2. 数据库的增删改查命令

   ```
   增：use DATABASE_NAME
   检增：db
   查看：show dbs
   ```

## 2.2MongoDB可视化工具MongoDB Compass

### 2.2.1下载与安装

下一步到底

### 2.2.2连接MongoDB

本地ip：127.0.0.1:27017

### 2.2.3创建数据库

+create database——填写database name、connection name(类似Mysql的表名)

### 2.2.4插入文档（数据）

右键点击+add data，选择Insert document

## 第三章 数据库程序的操作——MongoDB数据库使用

### 3.1MongoDB shell

1. 连接shell

```mysql
mongosh
```

2. 创建管理员账户

```mysql
use admin
db.createUser({
  user: "adminUser",
  pwd: "adminPassword",
  roles: [{ role: "userAdminAnyDatabase", db: "admin" }]
})
```

3. 创建登录账户

```mysql
use yourDatabaseName
db.createUser({
  user: "dbUser",
  pwd: "dbPassword",
  roles: [{ role: "readWrite", db: "yourDatabaseName" }]
})
```

### 3.2 MongoDB基本操作

- 新增数据库

  ~~~mysql
  use DATABASE_NAME
  ~~~

- 切换数据库

  ~~~
  use DATABASE_NAME
  ~~~

- 查看数据库

  ~~~mysql
  show dbs
  ~~~

- 删除数据库：先选择进入要删除的数据库，然后运行删除指令

  ~~~
  db.dropDatabase()
  ~~~

### 3.3集合

- 创建集合

  ~~~
  db.createCollection(name,options)
  ~~~

  name:要创建的集合名称

  options：可选参数，具体如下

  | 字段        | 类型   | 描述                                                         |
  | ----------- | ------ | ------------------------------------------------------------ |
  | capped      | bool   | （可选）若为true，创建固定集合，必须指定size参数。固定集合有固定的大小，达到最大值时，自动覆盖最早的文档 |
  | autoIndexId | bool   | （可选）若为true，自动在_id字段创建索引，默认为false         |
  | size        | number | （可选）指定集合的最大值（byte），capped为true，必须指定size参数 |
  | max         | number | （可选）指定集合中包含文档的最大数量                         |

  示例：创建一个名为test的集合。先选择要创建集合的数据库

  ~~~
  db.createCollection("test",{capped:true,autoIndexId:true,size:5000000,max:1000})
  ~~~

- 查看集合：进入要查看的数据库运行以下指令

  ~~~mysql
  show collections
  ~~~

- 删除集合：先选择进入要删除的数据库，然后运行以下指令

  ~~~mysql
  db.要删除的集合名称.drop（）
  ~~~


### 3.4文档

文档是一组键值（key-value）对，是MongoDB的核心概念，是数据的基本单元。类似mysql表中的行，与关系型数据库有很大的区别，也是MongoDB的核心优势所在。

创建文档是需要注意：

- 文档的键值是有序的；
- 键值可以是各种数据类型，甚至是整个嵌入的文档。
- MongoDB区分类型和大小写
- 键名不能重复，否则会报错
- 键名是字符串类型，可采用UTF-8的任意字符

**1.插入文档**

方法：insert（）、save（）

~~~
db.collection_name.insert(document)
~~~

示例：

~~~
db.newdb.insert({
	"name":"张三",
	"sex":"男",
	"age":52
})
~~~

为了方便管理，可以将数据定义为一个变量，例如“

~~~
document=({
	"name":"张三",
	"sex":"男",
	"age":52
})
~~~

**2.查询文档**

 - 方法：find()、findone()

   ~~~\
   db.collection.find(query,projection)
   ~~~

   - query：可选参数，适用查询操作符指定查询条件。
   - projection：可选参数，适用投影操作符指定返回键。如果需要返回文档中所有的键值，可省略该参数（默认省略）。
   

- 如果需要格式化读取数据，可使用pretty（）方法：

  ~~~mysql
  db.collection.find().pretty()
  ~~~

- 条件查询语句

  |   操作   |         格式          |                     示例                      |
  | :------: | :-------------------: | :-------------------------------------------: |
  |   等于   |   {<key>:<valiue>}    |   db.newdb.find({"by":"数据库"}).pretty（）   |
  |   小于   | {<key>:{$lt:<value>}  | db.newdb.find({"likes":{$lt:50}}).pretty（）  |
  | 小于等于 | {<key>:{$lte:<value>} | db.newdb.find({"likes":{$lte:50}}).pretty（） |
  |   大于   | {<key>:{$gt:<value>}  | db.newdb.find({"likes":{$gt:50}}).pretty（）  |
  | 大于等于 | {<key>:{$gte:<value>} | db.newdb.find({"likes":{$gte:50}}).pretty（） |
  |  不等于  | {<key>:{$ne:<value>}  | db.newdb.find({"likes":{$ne:50}}).pretty（）  |

  - AND条件

  ```mysql
  db.newdb.find({key1:value1,key2:value2}).pretty()
  ```

  - OR条件

  ```mysql
  db.newdb.find($or[{key1:value1},{key2:value2}]).pretty()
  ```

**3.更新文档**

方法：update()、save()

- update()方法：

  ~~~
  db.collection.update(
  	<query>,
  	<update>,
  	{
  		upsert:<boolean>,
  		multi:<boolean>,
  		writeConcern:<document>
  	}
  )
  ~~~

  query：update查询条件；

  update：update的对象和一些更新操作符；

  upsert：可选，默认false，如果查询的update不存在，是否插入objNew；

  multi：可选，默认false，只更新查询到的第一条记录，改为true，则会更新查询到的所有记录；

  writeConcern：可选，抛出异常级别。

- 示例：

  ~~~
  db.newdb.insert({
  	"name":"张三",
  	"sex":"男",
  	"age":52
  })
  ~~~

  更新上面文档的姓名

  ~~~mysql
  db.newdb.update({"name":"张三"},{$set{"name":"李四"}})
  ~~~

- save()方法：通过传入的新文档来替换数据库中已有文档

  ~~~mysql
  db.collection.save(
  	<document>,
      {
      	writeConcern:<document>
      }
  )
  ~~~

- 示例：将上面插入的张三的信息文档增加_id信息

  ~~~mysql
  db.newdb.save(
  	"_id":ObjectId("5d69f455545gd5s545w4d4")
      "name":"张三",
  	"sex":"男",
  	"age":52
  )
  ~~~

**4.sh删除文档**

- remove()方法：

  ~~~mysql
  db.collection.remove(
  	<query>,
      {
      	justOne:<boolean>,
      	writeConcern:<document>
      }
  )
  ~~~

  query:删除文档的条件

  justOne:默认false，删除所有匹配条件的文档，设为true或1，只删除1个文档

  writeConcern：可选，抛出异常级别。

- 删除所有数据：

  ~~~
  db.collection.remove（{}）
  ~~~

### 3.5数据类型

1. ObjectId:文档自动生成的_id，类似mysql的主键，可以快速生成和排序，包含24B。

   示例：

   ~~~mysql
   db.collection.find（
   	{"_id":ObjectId("5d69cf296693871a67f19983")}
   ）
   ~~~

   - 0~88B（5d69cf29）：时间戳，这条数据生成的时间。
   - 9~14B（669387）：机器标志服，存储这条数据时的机器编码。
   - 15~18B（1a67）：进程id。
   - 19~24B（f19983）：计数器。

2. String：任意UTF-8字符组成。
3. Boolean：布尔值true或者false，首字母必须小写
4. Integer：整数，包括32位和64位，不同编程语言支撑情况不同，注意转化。
5. Double：浮点，MongoDB没有float，只支持双精Double
6. Arrays：数组，值的集合或者列表都可以表述为数组，数组元素可以是不同类型厄数据。
7. Null：空数据类型，表示不存在的字段。
8. Date：日期类型，不存储时区。

### 3.6索引

索引是一种对应数据库表中一列或多列值进行排序的一种结构。

通过创建索引，MongoDB不用扫描所有集合进行查询，大大提高数据查询效率。

- createIndex()方法

  ~~~mysql
  db.collection.createIndex(keys,options)
  ~~~

  - keys：要创建索引的字段
  - option：1为升序索引，-1为降序索引。

