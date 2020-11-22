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

   ### B/S资源

   1. 静态资源
      1. 使用静态网页开发技术发布的资源
         1. 特点
            1. 所有用户访问，得到的结果一样
            2. 如果用户请求的是静态资源，那么服务器直接将静态资源发送给浏览器。浏览器内置静态资源的解析引擎，可以展示静态资源
   2. 动态资源
      1. 使用动态网页及时发布的资源
         1. 特点：
            1. 服务器将动态资源转换成静态资源，再发送给浏览器

   ### HTML

   #### 文件标签

   1. html html文档的根标签
   2. head 头标签。用于指定html文档的一些属性。引进外部资源
   3. title 标题标签
   4. body 体标签

   #### 文本标签

   1. 注释<!--注释内容-->

   ## javascript

   ### DOM

<<<<<<< HEAD:基础知识/Jave Web.md
   文档对象模型，将标记语言文档的各个组成部分，封装成对象，可以使用这些对象，对标记语言文档进行CURD的操作。
   
=======
   文档对象模型

   将标记语言的各个组成部分，封装成对象。可以使用这些对象，对标记语言进行CRUD的操作

   W3C DOM标准被分为3个部分。定义了访问HTML和XML的标准

   1. 核心DOM 针对任何结构化文档的标准模型
      - Doucment 文档对象
      - Element 元素对象
      - Attribute 属性对象
      - Text 文本对象
      - Comment 注释对象
      - Node 节点对象 其他五个的父对象
   2. XML DOM 针对XML文档的标准模型
   3. HTML DOM 针对HTML文档的标准模型

   #### 核心DOM模型

   ##### Document

>>>>>>> a2f1fd47615cff6bc9f24312bb6564b660aaa501:Jave Web.md
   - 功能：控制html文档内容
   - 方法：
     1. 获取Element对象：
        1. document.getElementById("id值") 根据id值获取页面标签（元素）对象 Element
        2. getElementsByTagName("Tag值")  返回值是一个元素数组
        3. getElementsByClassName(calss) 根据class属性值获取元素对象们
     2. 创建其他DOM对象
        1. createAttribute(name)
        2. createComment()
        3. createElement()
        4. createTextNode()

   ##### Element

   - 操作Element对象
     - 明确获取的对象是哪一个
     - 查看API文档，找其中有哪些属性可以设置
<<<<<<< HEAD:基础知识/Jave Web.md
- 修改标签体内容
     - 属性：innerHTML
- 
   
### 事件
   
=======
       - removeAttribute() 删除属性
       - setAttribute() 设置属性

   ##### Node

   节点对象可以使元素节点、属性节点、文本节点、

   - 特点：所有DOM对象都可以被认为是一个节点
   - 方法
     - CRUD DOM树
       - appendChild()：向节点的子节点列表的结尾添加心得子节点
       - removeChild()：删除（并返回）当前节点的指定子节点
       - replaceChild()：用新节点替换一个子节点
   - 属性
     - 返回当前节点的父节点

   #### HTML DOM

   修改标签体内容

   - 属性：innerHTML

   使用html元素的属性

   ### 事件

>>>>>>> a2f1fd47615cff6bc9f24312bb6564b660aaa501:Jave Web.md
   - 功能：某些组件被执行了某些操作后，触发某些代码的执行
   
- 如何绑定事件
     1. 直接再html标签上，指定事件的属性（操作），属性值就是js代码
     1. 时间：onclick--单击事件
   
### BOM
   
   ![](.\picture\BOM.png)
   
   1. 组成：
      1. ==Window 窗口对象==
      2. Navigator浏览器对象
      3. Screen 显示器
      4. ==History 历史记录==
      5. ==Location 地址栏对象==
   2. 用法
      1. Window 窗口对象 
         1. 创建
         2. 方法
            1. 与弹出框有关的方法
               1. alter 警告框
               2. confirm 确认取消对话框
                  - 返回 true 或 false
               3. prompt 输入对话框
                  - 返回用户输入的值
            2. 与打开关闭有关的方法
               1. close()
               2. open()
                  - 返回一个新的Windwows对象
            3. 与定时器有关的方法
               1. setTimeOut("method()"，时间) setTimeOut(method，时间)
                  - 返回唯一标识，用于取消
               2. clearTimeout(id)
               3. setInterval("method()"，时间) setInterval(method，时间)
               4. clearInterval
         3. 属性
            1. 获取其他BOM对象
               2. history
               2. location
               3. Navigator
               4. Screen
            2. 获取document对象
               1. 
      4. 特点
            1. Window对象不用创建，window使用。window.方法名();
         2. windows 可以省略 方法名
      2. History 历史记录
<<<<<<< HEAD:基础知识/Jave Web.md
   3. Location对象
         1. 包含当前的URL信息 
      2. 方法
            1. reload() 刷新方法
         3. 属性
            1. herf 获取herf属性
   
   
   
   
   
   
   
   
   
   
   
   
   
=======

   #### 事件监听机制

   某些组件被执行了某些操作后，触发某些代码的执行

   - 事件：某些操作，单击，双击。。。。
     1. 点击事件
        1. onclick 单击事件
        2. ondblclick 双击事件
     2. 焦点事件
        1. onblur 失去焦点
           - 一般用于表单校验
        2. onfocus 获得焦点
     3. 加载时间
        1. onload 页面或图像加载完成
           - 
     4. 鼠标事件
        1. onmousedown 鼠标被按下
        2. onmouseup 鼠标被松开
        3. onmousemove 鼠标被移动
        4. onmouseover 鼠标一到某元素上
        5. onmouseout 鼠标从某元素移开
     5. 键盘事件
        1. onkeydown 某个键盘按键被按下
        2. onkeyup 某个键盘按键被松开
        3. onkeypress 某个键盘按键陪按下并松开
     6. 选中和改变
        1. onchange 与内容被改变
        2. onselect 文本被选中
     7. 表单事件
        1. onsubmit 确认按钮被提交
           - 可以组织表单提交
             - 方法返回false则表单被组织提交
        2. onreset 重置按钮被点击
   - 事件源：组件
   - 监听器：代码
   - 注册监听：将事件，事件源，监听器结合在仪器

   ### Bootstrap

   一个前端开发的框架

   好处

   1. 定义了很多css样式和js插件。我们开发人员可以直接使用这些样式和插件
   2. 响应式布局
      1. 同一套页面可以兼容不同分辨率的内容
   3. 快速入门
      1. 下载BootStrap
      2. 在项目中将三个文件夹复制
      3. 创建HTML

   #### 响应式布局

   依赖于栅格系统：将一行平均分为12个格子，可以指定元素占几个格子

   步骤：

   1. 定义容器

      1. container 两边有一定量的留白
      2. container-fluid 每一种设备都是100%宽度

   2. 定义行

   3. 定义元素。制定该元素在不同的设备设备上，所占的格子数目。样式：col-设备代号-格子数目

      1. 设备代号

         1. xs 超小屏幕（）

         2. sm 平板

         3. md 中等屏幕

         4. lg 大屏幕

   注意事项

      1. 一行中如果格子数超过12，则超出部分自动换行
      2. 栅格类属性可以向上兼容
      3.  如果真实设备宽度小雨了设置栅格类属性的设备代码最小值，会一个元素占满一整行

   CSS样式和插件

   按钮

   

## XML

   ### 功能

存储数据

1. 配置文件
2. 在网络中传输

### 与HTML的区别

1. xml标签都是自定义的，html标签是预定义的
2. xml语法严格 html语法松散
3. xml是存储数据，html是展示数据

### 语法

#### 基本语法

1. xml文档后缀名为 .xml
2. xml第一行必须定义为文档声明
3. xml文档有且仅有一个根标签
4. 属性值必须使用引号（单双号都可）引起来
5. 标签必须关闭
6. xml标签名区别大小写

#### 组成部分

1. 文档声明

   1. 格式：<?xml 属性列表 ?>
   2. 属性列表：
      1. version 版本号 必须
      2. encoding 编码方式 告知解析引擎，当前文档时用的编码方式，默认为ISO-8859-1
      3. standalone 独立方式
         1. YES 不依赖其他文件

2. 指令

3. 标签

   1. 数字不能开头
   2. 不能以数字xml开始
   3. 名称不能有空格

4. 属性

   1. id属性值唯一

5. 文本

   1. CDATA区 该区域的数据被原样展示

      1. ```xml
         <![CDATA][数据]>
         ```

### 约束

规定XML文档的书写规则

分类：

1. DTD 一种简单的约束技术
2. Schema 一种复杂的约束技术

#### DTD

引入

- 内部dtd 约束规则在xml文档中
- 外部dtd约束的规则定义在外部的dtd文件中
  - 本地```  <!DOCTYPE 根标签名SYSTEM "约束文件名"> ```
  - 网络```  <!DOCTYPE 根标签名PUBLIC "约束文件名" "DTD文件的位置URL"> ```

缺陷

- 无法规定属性的内容

#### Schema

引入

1. 填写xml文档的根元素
2. 引入xsi前缀
3. 引入xsd文件命名空间
4. 为每一个xsd约束声明一个前缀，作为前缀

### 解析

思想：

1. DOM 将标记语言文档一次性加载进内存，想成DOM树，可以快速查找
   - 优点：操作方便，可以对文档进行CRUD所有操作
   - 缺点：消耗内存
2. SAX 逐行读取，基于事件驱动的。

   1. 优点：不占内存
   2. 缺点：只能读取

XML解析器

1. JAXP Sun提供，一般不用
2. DOM4J
3. Jsoup
4. PULL 安卓内置解析器

#### Jsoup

- 步骤
  1. 导入jar包
  2. 获取Doucment对象
     1. parse（URL url,int timeoutMillis）通过网络路径获取指定的html或xml的文档对象
     2. parse（file path , encoding）
  3. 获取对应的标签
- 对象使用
  - Jsoup 工具类，可以解析html或xml文档，返回Document
  - Document 文档对象。代表内存中的dom树
  - Elements 元素Element对象的集合。可以当做ArryList<Element>来适应
    - 获取子元素对象
    - 回去属性值
    - 获取文本内容
  - Node节点对象
- 快捷查询
  - Selector 选择器
    - Element select(String cssQuery)
      - 参考
  - Xpath

## Tomcat

接受用户的请求，处理请求，做出响应

常用的Web服务器

1. webLogic Oracle 大型javaEE服务器 收费
2. webSphere IBM 大型javaEE服务器 收费
3. JBOSS JBOSS 大型javaEE服务器 收费
4. Tomcat Apache基金组织 中小型 仅支持少量的JaveEE规范 Servlet/jsp

### 部署

1. 直接将项目放到webapps目录下
   - 将项目打包成war包放置到webapp项目
2. 配置conf/server.xml文件
   1. 在<host> 标签体中配置```<Context docBase='basePath' path='path'>```
3. 配置conf/catalina/localhost文件 创建任意名称的xml文件。

### 项目

#### Java动态项目目录结构

- 项目的根目录
  - WEB-INF目录
  - web.xml:web项目的核心配置文件
  - classes目录:放置字节码文件的目录
  - lib目录 放置依赖的jar包

#### 与IDEA集成，并且创建JavaEE的项目





## Servlet

Server applet 运行在服务器端的小程序

- severlet 就是一个接口，定义了Java类被浏览器访问到的规则
- 将来我们自定义一个类，实现Servlet 覆写方法

### 原理

![image-20200902102653742](\picture\image-20200902102653742.png)

1. 当服务器接收到客户端浏览器的请求后，会解析请求URL路径，获取访问的Servlet的资源路径
2. 查找Web.xml文件，是否有对应的<url-pattern>标签体内容
3. 如果有，则在找到对应的<servlet-class>全类名
4. tomcat会将字节码文件加载进内存，并且创建其对象
5. 调用其方法

### Servlet 生命周期（servlet里面的方法）

   ![img](\picture\生命周期.png)

- init 
  - 何时执行
    - 默认情况下，第一次被访问时，Servlet被创建
      - <load-on-startup>的值为负数
    - 第二种，服务器启动时
      - <load-on-startup>的值为0或正整数
    - 在web.xml下修改
  - init方法只执行一次，说明Servlet在内存中只存在一个对象，说明是单例的
    - 多个用户访问时，可能存在线程安全问题
    - 解决：尽量不要在servlet中定义成员变量。即使定义了成员变量，也不要修改该值。
- service
  - 每次访问Servlet时，Service方法都会被调用一次
- destroy
  - 只有服务器正常关闭时，才会执行destroy方法

### Servlet3.0

- 好处
  - 支持注解配置，可以不需要web.xml
- 步骤
  - 创建JAVAEE项目，选择Servlet版本在3.0上，可以不创建web.xml
  - 定义一个类，实现servlet接口
  - 覆写方法
  - 在类上使用@WebServlet注解，进行配置
    - @WebServlet("路径")

### Servlet的体系结构

Servlet--接口

​	|

GenricServlet--抽象类

​	|

HttpServlet--抽象类

- GenericServlet 将servlet接口中其他方法做了默认实现，只将service（）方法作为抽象
  - 将来定义Servlet类时，可以继承GenericServlet,实现service（）方法即可
- HttpServlet

![image-20200902131712004](\picture\service.png)

对HTTP协议的封装，可以简化操作

1. 定义类继承HttpServlet
2. 覆写doget() 和 dopost() 方法

### Servlet配置

1. urlPartten:Servlet访问路径
   1. 一个Servlet可以设置多个路径
   2. /* 通配符 优先级最低

### IDEA 与 Tomcat

1. IDEA会为每一个Tomcat部署项目单独建立一份配置
2. 工作空间项目和Tomcat部署的webx项目
   1. tomcat真正访问的是‘’Tomcat部署的web项目“，“Tomcat部署的web项目”对应着“工作空间项目”的web目录下的所有资源
   2. WEB-INF目录下的资源不能被浏览器直接访问



## HTTP

超文本传输协议

### 特点

1. 基于TCP/IP的高级协议
2. 默认端口号：80
   1. 基于请求/响应模型的 一次请求对应一次响应
   2. 无状态的：每次请求之间相互独立，不能交互数据

### 请求消息数据格式

1. 请求行

   ```
   请求方式 请求url 请求协议/版本
   GET /login.html HTTP/1.1
   ```

   - 请求方式
     - HTTP有七种请求方式，常用的有两种
       - GET
         - 请求参数在请求行中，在URL后
         - 请求的URL长度有限制
         - 不太安全
       - POST
         - 请求参数在请求体中
         - 长度没限制，比较安全

2. 请求头

   ```
   请求头名称：请求头值
   ```

   User-Agent :浏览器信息，解决浏览器的兼容性问题

   Referer:告诉服务器，当前请求从哪里来

   1. 防盗链
   2. 统计

   Connection: 链接可以被复用

3. 请求空行

   ```
   空行
   ```

   分割请求头和请求体

4. 请求体

   ```
   key=values
   ```

   封装POST 请求消息的请求参数的

### 响应消息格式

1. 响应行

   ``````
   协议/版本 响应状态码 状态码描述
   HTTP/1.1 200 ok
   ``````

2. 响应头

   ``````
   响应名称：响应头值
   ``````

   content-type 服务器告诉客户端本次响应体数据格式以及编码格式

   Content-disposition 服务器告诉客户端以什么格式打开响应体数据

3. 响应空行

4. 响应体

   ``````
   传输数据
   ``````

## Request和Respose

原理：

![image-20200902142923486](.\picture\request和response.png)

1. request对象和response对象是由服务器创建的，我们来使用他们
2. request对象是来获取请求消息，response对象是来设置响应消息

### Requset

#### 继承体系结构

ServletRequest --接口

​				|继承

HttpServletRequest --接口

​				|实现

org.apache.catalina.connector.RequestFacade 类（tomcat）

#### 功能

1. 获取请求消息数据

   1. 获取请求行
      1. 获取虚拟目录
         - String getContextPath()
      2. 获取请求URI
         - String getRequestURI()
   2. 获取请求头
      1. ==String getHeader(String headname) 通过请求头的名称获取请求头的值==
      2. Enumeration<String> getHeaderNames() 获取所有的请求头的名称
   3. 获取请求体
      - 请求体只有POST请求方式才有请求体，在请求体中封装了POST请求的请求参数
      - 步骤
        1. 获取流对象（字节流和字符流）
           1. BufferedReader getReader() 获取字符输入流，只能操作字符数据
           2. ServletInputStream getInputStream() 
        2. 再从流对象中获取数据

2. 其他功能

   1. 获取请求数据

      1. ==String getParameter(String name)根据参数名称获取参数值==
      2. ==string[] getParameterValues(String name) 根据参数名称获取参数值的数组==
      3. Enumeration<string> getParameterNames()获取所有轻轻地参数名称
      4. Map<string,string[]> getParameterMap()获取所有参数的map集合

      - 中文乱码
        - get方式：tomcat8 已经将get方式乱码问题解决了
        - POST会乱码
          - 在获取参数前，设置编码request.setCharacterEncoding("utf-8")

   2. 请求转发

      1. 一种在服务器内部资源跳转的方式
      2. 步骤：
         1. 通过request对象获取请求转发器对象 
            - request.getRequsetDispatcher(==path==).forward()
      3. ==特点==
         1. 浏览器地址栏不发生变化
         2. 只能转发到服务器内部资源中
         3. 转发是一次请求

   3. 共享数据

      - 域对象 一个有作用范围的对象，可以在范围内共享数据
      - request域：代表一次请求的范围，一般用于请求转发的多个资源中共享数据
      - 方法：
        - setAttribute(String name.object obj)存储数据
        - object getAttitude(String name) 通过建获取值
        - void removeAttribute(String name) 通过键

   4. 获取sevletContext

#### BeanUtila

用于封装JavaBean的

1. JavaBean 标准的Java类
   1. 要求
      1. 类必须被public修饰
      2. 必须提供空参的构造器
      3. 成员变量必须被Private修饰
      4. 提供公共setter和getter方法
   2. 功能：封装数据
2. 概念
   1. 属性： setter和getter方法截取后的产物；
3. 方法：
   1. setProperty()
   2. getProperty()
   3. populate

### Response

#### 方法

1. 设置响应头
   1. 设置状态码 setStatus(int sc)
2. 设置响应行
   1. setHeader(String name,String value);
3. 设置响应体
   1. 使用步骤
      1. 获取输出流
      2. 使用输出流，将数据输出到客户端

#### 功能

1. 重定向

   1. 资源跳转的方式
2. response.sendRedirect(path);
   3. 重定向的特点： redirect

      1. 地址栏路径变换
   2. 可以访问其他服务器下的资源
      3. 两次请求
   4. 转发的特点：forward
   1. 地址栏路径不变
   2. 只能访问当前服务器下的资源
      3. 一次请求，可以使用request对象共享数据
5. 路径分类
      1. 相对路径 通过相对路径不可以确定唯一资源
      2. 绝对路径 通过绝对路径可以确定唯一资源
         1. 规则：判断是给谁用的
            1. 给客户端浏览器使用：需要加虚拟目录
               1. 虚拟目录动态获取 request.getContextPath()
            2. 给服务器用 不用加虚拟目录
      
      
>>>>>>> a2f1fd47615cff6bc9f24312bb6564b660aaa501:Jave Web.md
   
   ​     
   
2. 输出数据

   1. 解决乱码
      1. response.setContentType("txt/html;charset=utf-8")
      2. 在获取流之前设置

3. 验证码

### ServletContext 对象

代表整个web应用，可以和程序的容器（服务器）来通信

功能：

1. 获取MIME类型
   1. MIME类型：在互联网通信中定义的一种文件数据类型
   2. String getMimeType(String file)
2. 域对象：共享数据
   1. 方法
      1. setAttribute(String name,Object value)
      2. getAttribute(String name)
      3. removeAttribute(String name)
   2. 范围
      1. 所有用户所有请求的数据
      2. 生命周期特别长
3. 获取文件的真实（服务器）路径
   1. 方法：getRealPath(String path)
   2. 

## 会话

- 概念:

  一次会话中包含多次请求和响应

  一次会话：浏览器第一次给服务器资源发送请求，会话建立，知道有一方断开为止

- 功能：在一次会话的范围内的多次请求间，共享数据
- 方式：
  1. 客户端会话技术 Cookie
  2. 服务器端会话技术 Session

### Cookie

#### 概念

- 客户端会话技术，将数据保存到客户端

- 快速入门：

1. 创建Cookie对象，绑定数据

   new Cookie(String name,String value)

2. 发送Cookie对象

   response.addCookie(Cookie cookie)

3. 获取Cookie，拿到数据

   1. Cookie[] cookie = request.getCookies();

#### 原理：

​	基于响应头set-cookie和请求头cookie实现

#### 细节：

- 可以创建多个cookie对象，使用response调用多次addCookie方法发送cookie即可
- 默认浏览器关闭cookie销毁
- 持久化存储
  - setMaxAge(int seconds)
    - 正数 ：将cookies数据写到硬盘的文件中。持久化存储。cookie存活时间
    - 负数 ：默认值
    - 零：删除cookie信息
- 是否支持中文
  - 在Tomcat 8之前不支持中文，存储会报错
    - 需要将中文数据转码--一般采用URL编码
  - Tomcat8之后，可以存储，但是还是不支持特殊字符
- 共享问题
  - 同一tomcat服务器
    - 默认cookie不能共享
    - setPath("/") 则可共享
  - 不同cookie能共享吗
    - setDomain(String path)如果设置一级域名相同，那么多个服务器之间cookie可以共享

#### 特点

1. cookie储存在客户端，不太安全
2. 浏览器对于单个cookie的大小限制以及同一域名下的数量有限制（20）

#### 作用

1. cookie一般存储少量不太敏感的数据
2. 在不登录的情况下，完成服务器对客户端的身份识别

### Session

服务器端会话技术，在一次会话的多次请求共享数据

#### 原理

Session的是现实依赖于Cookie的

#### 细节

当客户端关闭后，服务器端不关闭，两次获取session是否是同一个？

- 默认情况不是
- 如果需要相同，则可以创建Cookie,键为jsessionid,设置最大存活时间，让cookie持久化保存

客户端不关闭，服务器关闭后，两次获取的session是同一个吗？

- 不是同一个，但是要确保数据不丢失
- session钝化
  - 在服务器正常关闭之前，将session对象系列化到硬盘上
- session活化
  - 在服务器启动后，将senssion文件转化为内存中的session对象即可

#### session什么时候被销毁

1. 服务器关闭
2. session对象调用30分钟后被自动销毁
3. 在web.xml中修改
4. 调用invalidate（）

#### session特点

1. session用于存储一次会话的多次请求的数据，存在服务器端
2. session可以存储任意类型，任意大小的数据

## JSP

Java server pages Java服务器端页面

- 可以理解为：一个特殊的标签，既可以定义html页面，也可以定义java代码

![image-20200903153335367](.\picture\image-20200903153335367.png)



==本质是一个Servlet==

### JSP的脚本

jsp定义JAVA代码

1. <% 代码 %>定义Java代码，在service方法中。service方法中可以定义什么，该脚本就可以定义什么

2. <%! 代码 %>定义的java代码，在jsp转换后的java类的成员位置，用的较少

3. <%= 代码 %>定义的java代码，会输出到页面上。输出语句可以定义什么，该脚本就可以定义什么

### 内置对象

在jsp页面上不需要多去和创建，可以直接使用的对象

一共九个对象

1. request
2. response
3. out：字符输出流对象。可以将数据输出到页面上。和response.getwriter() 类似
   1. response.getwriter() 和 out.writer（）的区别
   2. 在tomcat服务器真正给客户端做出响应前，会先找到response缓冲区数据，再找out缓冲区数据
   3. response.getWriter() 数据输出永远在out.write()之前
4. application
5. pageContext
6. config
7. exception

![](D:\用户目录\我的文档\java_learning_notes\picture\微信截图_20200904103318.png)



### 指令

- 作用：用于配置JSP页面，导入资源文件
- 格式：
  - <%@ 指令名称 键1=值1 键2=值2  ... %>
- 分类
  - page
    - 配置JSP 页面
    - contentType:等同于response.setContentType()
      - 设置响应体的mime类型及字符集
      - 设置当前jsp页面的编码（）
    - import 导包
    - erropage 当前页面发生异常后，会自动跳转到指定的错误页面
    - iserrorpage 标志当前页面是否是错误页面
  - include
    - 页面包含的。导入页面的资源文件
  - taglib
    - 导入资源

### 注释

<%----%>

## MVC

M

V

C

### EL表达式

- 概念expression language 表达式语言
- 作用 替换和简化jsp页面中java代码的编写
- 语法：${}
- 注意 jsp中，默认是支持el表达式的
  - 直接展示：isElIgnore
  - \
- 运算：
  - 运算符
    - 空运算符 empty
  - 获取值
    - el表达式只能从域对象中获取值
    - 语法：
      - ${域名称.键名}：从指定域中获取指定键的值

### JSTL

概念：JSP标准标签库

作用：用于简化和替换jsp页面上的Java代码

步骤：

1. 导入jstl相关的jar包
2. 引入标签库：taglib指令 <% taglib %>
3. 使用标签

常用的JSTL标签：

1. if 相当于Java代码的if语句
2. choose 相当于java代码的switch语句
3. foreach 相当于java代码的if语句



## 三层架构

1. 界面层（表示层）：用户看得到的界面。用户可以通过界面上的组件和服务器进行交互
2. 业务逻辑层：处理业务逻辑。
3. 数据访问层：操作数据存储文件

## JQuery

js框架

### JQuery对象和Js对象的区别

1. JQuery对象在操作时，更加方便
2. JQuery对象和js对象方法不通用
3. 两者相互转换
   1. jq-------->js ： jq对象[索引]或者 jq对象.get(索引)
   2. js --------->jq ： $(js对象)

### 选择器

筛选具有相似特征的元素（标签）

## AjAX

1. 概念：
   - Asynchronous JavaScript And XML  异步的JavaScript和XML
   - 异步和同步
     - 同步：客户端必须等待服务器端的响应。在等待的期间客户端不能做其他操作
     - 异步：客户端不需要等待服务器端的响应。在服务器处理请求的过程中，客户端可以做其他的操作
   - 无需重新加载页面，更新部分页面内容
2. 实现方式：

## JSON

存储和交换信息的语法

进行数据的传输

比XML更小更容易解析

### 语法

1. 数据在名称/值对中：json数据是由键值对构成的
   - 键用引号（单双都行）引起来的，也可以不使用引号
   - 值取值类型：
     - 数字
     - 字符串
     - 逻辑值（true或false）
     - 数组
     - 对象

## Json转java 对象

常见的json解析器

jsonlib 、gson、fastjson、jackson



## redis

特殊的数据库软件

### 概念

高性能的NOSQL的菲关系型数据库



