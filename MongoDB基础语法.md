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

  
