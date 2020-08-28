# Java Web

## 网络编程入门

#### 软件结构

C/S 结构 全成为client/Server结构 ，是指客户端和服务器结构

B/S结构 全称为Browser/Server结构 浏览器和服务器结构

## 网络通信协议

连接和通信的规则称为网络通信协议，他对数据的传输格式，传输速率，传输步骤，等做了统一的规定

#### TCP/IP协议

传输控制协议/因特网互联协议，采用4层模型，每一层都呼叫他的下一层所提供的协议来完成自己的需求

应用层-->传输层-->网络层（核心，将数据分组）-->物理层（数据链路层）

### 协议分类

#### UDP

用户数据报协议，无连接的通信协议，消耗资源少，通信效率高

#### TCP

传输控制协议，面向连接的通信协议

### 三次握手

1. 第一次握手，客户端向服务器放出连接请求，等待服务器确认
2. 第二次握手，服务器向客户端回送一个响应，通知客户端接收到了连接请求
3. 第三次握手，客户端再次向服务器端发送确认信息，确认连接

### 网络编程三要素

#### 协议

#### IP地址

互联网协议地址，计算机设备的唯一编号

#### 端口号

由两个字节组成，取值范围在0-65535之间，其中1024之前不能使用，

mysql 3306 oracle 1521 tomcat 8080

## TCP通信程序

### 通信步骤

1. 服务器端启动
2. 客户端请求服务器端
3. 服务器端和客户端建立一个逻辑连接
4. 连接中包含一个IO对象（字节流对象）
5. 客户端给服务器端发送数据
6. 服务器端读取客户端发送的数据
7. 服务器端给客户端发送数据
8. 客户端读取服务器端发送的数据

- 多个客户端同时和服务器进行交互，服务器必须明确和哪个客户端进行交互
- 多个客户端同时和服务器进行交互，就需要使用多个IO流对象
  - 服务器端没有IO流，服务器可以获取到请求的客户端对象Socket，使用每个客户端Socket中提供的IO流和客户端进行交互
    - 服务器使用客户端的字节输入流读取客户端发送的数据
    - 服务器使用客户端的字节输出流给客户端回写数据
  - 简单记：服务器使用客户端的流和客户端进行交互

### Socket类

#### 上传文件

上传完文件要给一个结束标志。

void shutdownOutput

#### 改进

1. 文件名随机+毫秒值
2. 一直处于监听的状态（accept 死循环）
3. 多线程技术，有一个客户端上传文件，就开启一个线程

### 模拟B/S

## 数据库

1.什么是SQL？
	Structured Query Language：结构化查询语言
	其实就是定义了操作所有关系型数据库的规则。每一种数据库操作的方式存在不一样的地方，称为“方言”。
	
2.SQL通用语法
	1. SQL 语句可以单行或多行书写，以分号结尾。
	2. 可使用空格和缩进来增强语句的可读性。
	3. MySQL 数据库的 SQL 语句不区分大小写，关键字建议使用大写。
	4. 3 种注释
		* 单行注释: -- 注释内容 或 # 注释内容(mysql 特有) 
		* 多行注释: /* 注释 */
	

3. SQL分类
	1) DDL(Data Definition Language)数据定义语言
		用来定义数据库对象：数据库，表，列等。关键字：create, drop,alter 等
	2) DML(Data Manipulation Language)数据操作语言
		用来对数据库中表的数据进行增删改。关键字：insert, delete, update 等
	3) DQL(Data Query Language)数据查询语言
		用来查询数据库中表的记录(数据)。关键字：select, where 等
	4) DCL(Data Control Language)数据控制语言(了解)
		用来定义数据库的访问权限和安全级别，及创建用户。关键字：GRANT， REVOKE 等

### DML：增删改表中数据

1. 添加数据：
	* 语法：
		* insert into 表名(列名1,列名2,...列名n) values(值1,值2,...值n);
	* 注意：
		1. 列名和值要一一对应。
		2. 如果表名后，不定义列名，则默认给所有列添加值
			insert into 表名 values(值1,值2,...值n);
		3. 除了数字类型，其他类型需要使用引号(单双都可以)引起来
2. 删除数据：
	* 语法：
		* delete from 表名 [where 条件]
	* 注意：
		1. 如果不加条件，则删除表中所有记录。
		2. 如果要删除所有记录
			1. delete from 表名; -- 不推荐使用。有多少条记录就会执行多少次删除操作
			2. ==TRUNCATE TABLE 表名; -- 推荐使用，效率更高 先删除表，然后再创建一张一样的表。==
3. 修改数据：
	* 语法：
		* update 表名 set 列名1 = 值1, 列名2 = 值2,... [where 条件];

	* 注意：
		1. ==如果不加任何条件，则会将表中所有记录全部修改。==

### DQL：查询表中的记录

* select * from 表名;

1. 语法：
	select
		字段列表
	from
		表名列表
	where
		条件列表
	group by
		分组字段
	having
		分组之后的条件
	order by
		排序
	limit
		分页限定

2. 基础查询
	1. 多个字段的查询
		select 字段名1，字段名2... from 表名；
		* 注意：
			* 如果查询所有字段，则可以使用*来替代字段列表。
	2. 去除重复：
		* distinct
	3. 计算列
		* 一般可以使用四则运算计算一些列的值。（一般只会进行数值型的计算）
		* ifnull(表达式1,表达式2)：null参与的运算，计算结果都为null
			* 表达式1：哪个字段需要判断是否为null
			* 如果该字段为null后的替换值。
	4. 起别名：
		* as：as也可以省略

### 约束

对表中数据进行限定，保证数据的正确性，有效性和完整性。

分类：

1. 主键约束：primary key
   1. 非空且唯一
   2. 一张表只能有一个
   3. 删除主键
      1. Alter table 表名 drop primary key;
   4. 表创建完成后添加主键
      1. 通过修改表 alter table 表名 modify name 列名 primary key
   5. 自动增长
      - 如果某一列是数值类型的，使用 auto_increment 可以完成值的自动增长
      - 只和上一条记录有关系
      - 删除自动增长
        - alter table 表名 modify name 列名 （不会删除主键）

1. 非空约束 not null
   1. 添加约束
      1. 创建表时创建
      2. 通过修改表 alter table 表名 modify name 列名
   2. 修改约束
      1. 通过修改表 alter table 表名 modify name 列名 not null
2. 唯一约束 unique
   1. 注意MySQL中唯一约束的限定的值可以有多个null
   2. 删除约束
      1. alter table 表名 drop index 列名
      2. 不能用 modify
   3. 创建完成后添加主键
      1. 通过修改表 alter table 表名 modify name 列名 unique
3. 外键约束 foreign key
   1. 外键列 contraint 外键名称 foreign key （外键列名称） reference 主表名称（主表列名称）
   2. 删除外键
      1. alter table 表名 drop foreign key  外键名称
   3. 创建表后，添加外键
      1. alter table 表名 Add constraint 外键名称 foreign key (外键名称)  references 主表名称（主表列名称）
   4. 级联操作
      1. 级联更新：添加表时，设置外键后面添加 on update cascade
      2. 级联删除 on delete cascade
      3. 谨慎操作

#### 数据库设计

多表之间的关系

1. 1对1
   1. 一对一的实现，可在任意一方田间唯一外键指向对方的主键
2. 1对多（多对一）
   1. 在多的一方建立外键，指向一的一方的主键
3. 多对多
   1. 需要借助中间表，中间表至少包含两个字段，这两个字段作为第三张表的外键，分别指向两张表的主键



### 范式

第一范式：每一列都是不可分割的原子数据项

第二范式：在1NF的基础上，非码属性必须完全依赖于候选码（在1NF基础上消除非属性对主码的部分函数依赖项）

1. 函数依赖：A-->B,如果通过A属性（属性组）的值，可以唯一确定B属性的值。则称B依赖于A
2. 完全函数依赖：A-->B,如果A是一个属性组，则B属性值值得确定需要依赖于A属性组中所有的属性值
3. 部分函数依赖：A-->B,如果A是一个属性组，则B属性值得确定只需要依赖于A属性组中某一些值即可
4. 传递函数依赖：A-->B，B-->c,如果通过A属性（属性组）的值，可以确定唯一B属性的值，在通过B属性（属性组）的值可以唯一确定C属性的值，则称C传递函数依赖于A
5. 码：如果一个表中，一个属性或属性组，被其他所有属性完全依赖，则称这个属性（属性组）为该表的码

第三范式：在2NF基础上，任何非主属性不依赖于其他非主属性（在2NF基础上消除传递依赖）



### 数据库的备份和还原

1. 命令行的方式
   1. 备份语法：mysqldump -u用户名 -p密码 > 保存路径
   2. 还原：
      1. 登录数据库
      2. 创建数据库
      3. 使用数据库
      4. 执行文件 source 文件路径
1. 图像化工具

### 多表查询

笛卡尔积：两个集合A、B的所有组成情况

分类：

1. 内连接查询
   1. 隐式内联接
      1. 用where
   2. 显式内连接
      1. select  字段列表 from 表名1【 inner】 join 表2 on 条件
   3. 注意事项：
      1. 从哪些表中查数据
      2. 查询条件是什么
      3. 查询哪些字段
2. 外连接查询
   1. 左外连接
      1. ==语法：select 字段列表 from 表1 left outer join 表2 on 条件==
      2. 查询的是左表所有数据以及交集部分
   2. 右外连接
      1. 语法：select 字段列表 from 表1 right 【outer】 join 表2 on 条件

1. 子查询
   1. 查询中嵌套查询，称为子查询
   2. 不同情况
      1. 子查询结果单行单列 
         1. 子查询作为条件，使用运算符去判断。运算符：> >= < <= =
      2. 子查询结果多行多列的
         1. 子查询可以作为一个虚拟表。
      3. 子查询结果为多行单列的
         1. 子查询可以作为条件，使用运算符in来判断



## 事务

概念：

事务执行是一个整体，所有的SQL语句都必须执行成功。如果其其中有一条执行失败，所有的SQL语句都要回滚，整个业务失败。

操作：

1. 开启事务：start transaction
2. 回滚：roolback
3. 提交：commit
4. MySQL数据库中事务自动提交
   - 事务提交方式
     - 自动提交
       - MySQL
     - 手动提交
       - 需要先开始事务再提交
     - 修改该事务的默认提交方式
       - select @@autocommit； --1自动提交 --2手动提交

事务的四大特征

1. 原子性：是不可分割的最小操作单位i，要么同时成功，要么同时失败
2. 持久性：当事务提交或回滚后，数据库会持久化的保存数据
3. 隔离性：多个事物之间相互独立
4. 一致性。事务操作前后，数据总量不变



## DCL

管理用户，授权

- 管理用户
  1. 切换到MySQL数据库
     1. user MySQL
  2. 查询user表
     1. select * from user；
  3. 创建用户
     1. create user ‘用户名’@‘主机名’ IDENTIFIED BY '密码'；
  4. 删除用户
     1. deop user '用户名'@’主机名‘
  5. 改用户密码
     1. set password for ’用户名‘@'主机名'=PASSWORD('新密码)



## JDBC

定义了一套操作所有关系型数据库的接口

### 步骤：

1. 导入驱动jar包
2. 注册驱动
3. 获取数据库连接对象
4. 定义sql
5. 获取执行sql语句的对象 statement
6. 执行SQL，接受返回结果
7. 处理结果
8. 释放资源

### 详解对象

1. DriverManager 驱动管理对象

   - 功能：

     - 注册驱动 数据库该使用哪个JAR包 

       - static void registerDriver(Driver driver)：注册与给定的驱动程序 DriverManager

       - ```java
         Class.forName("com.mysql.jdbc.Driver");
         ```

       - 源码中，存在静态代码块

       - ==MySQL5版本之后可以省略注册驱动==

     - 数据库连接：

       - ```java
         Connection conn = DriverManager.getConnection("jdbc:mysql://localhost:3306/db3","root","555777");
         ```

2. Connection 数据库连接对象

   - 功能
     1. 获取执行SQL对象
        1. Statement createStatement（）
        2. PreparedStatement      preparedStatement (string sql);
     2. 管理事务
        1. 开启事务 void setAutoCommit(boole) 
           1. 在connection 之后，sql执行之前
        2. 提交事务 commit()
           1. sql执行结束后
        3. 回滚事务rollback()
           1. 在catch里面

3. Statement 执行SQL的对象

   1. 执行SQL
      1. int executeUpdate(string sql) 执行DML语句（insert update delete)语句、DDL（create alter drop)语句
         - 返回值：影响的行数 >0则执行成果，反之，则执行失败
      2. ResultSet execuQuery（String sql);

4. ResultSet 结果集对象

   1. next() 获取下一行数据
   2. getxxx(参数)获取类型
      1. XXX代表数据类型
      2. 参数：
         1. int 代表列的编号，从 1 开始
         2. String 代表列的名称

5. PrepareStatement 执行SQL对象

   1. SQL注入问题
      1. 在拼接SQL时，有一些SQL的特殊关键字参与字符串的拼接。会造成安全性问题
      2. 使用PrepareStatement 对象来解决
   2. 预编译SQL：参数使用？占位符替代
   3. 用法：
      1. 导入驱动jar包
      2. 注册驱动
      3. 获取数据库连接对象
      4. 定义sql
         1. 参数使用？占位符替代
      5. 获取执行sql语句的对象 connectio.PrepareStatement (string sql)
      6. 给？赋值
         1. 方法setxxx（index.value）
      7. 执行SQL，接受返回结果
      8. 处理结果
      9. 释放资源



## 数据库连接池

一个存放数据连接的容器（集合）

当系统初始化后，容器被创建，容器会申请一些连接对象，当用户来访问数据库时，从容其中获取连接对象，用户访问完后，会将连接归还给容器。

好处

- 节约资源
- 用户访问高效

实现：

1. 标准接口DataSource javax.sql包下
   1. 方法：
      1. 获取连接 getconnection
      2. 归还连接 connection.close()
2. 由厂商实现
   1. C3P0
   2. Druid

C3P0使用：

- 步骤

1. 导入jar包
2. 配置文件
   1. c3p0.properties或c3p0-config.xml
   2. 直接放在src目录下
3. 创建核心对象 comboPooledDataSource
4. 获取连接 getconnection

Driud:

1. 导入jar包 Druid
2. 定义配置文件
   1. 是properties形式的
   2. 可以任意名称，任意目录
3. 加载配置文件
4. 获取数据库连接池对象 通过工厂类获取 DriuoData
5. 获取连接 getConnection

### JDBCTemplate

Spring框架对JDBC的简单封装。提供一个JDBCTemplate对象简化JDBC的开发

步骤：

1. 导入Jar包

2. 创建JdbcTemplate对象。依赖于数据源DataSource

3. 调用JdbcTemplate的方法来完成CURD的操作

   1. update（）执行DML语句。增删改语句
   2. queryForMap() 查询结果将结果集封装到map集合
      1. 只能有一条
   3. queryForList()  查询结果将结果封装为list集合
      1. 将每一条记录封装成map，然后装到List中
      2. query() 查询结果，将结果封装为Javabean对象、
         1. query 使用数据到new BeanPropertyRowMapper<类型>（类型.class）
   4. queryForObject 查询结果，将结果封装为对象
      1. 聚合函数

   ## Web概念基础

   

