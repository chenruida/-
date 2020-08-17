# Java 基础知识

## JVM、JDK和JRE的关系

- JVM :运行java程序的虚拟机 各个系统都有自己的虚拟机 Windows jvm 和 Linux jvm等
- JRE：Java runtime environment java程序运行环境 **包括 jvm**和各种类库
- JDK: Java Development Kit   java程序开发工具包 **包括 JRE**和开发使用的工具

## Javac 和 java

- javac classname.java 编译程序 编译得到 classname.class 
- java classname 运行程序

## 编译器

- 对于byte/short/char三种类型，如果右侧赋值没有超过范围，那么javac编译器将会自动隐含为我们补上一个（byte)(short)(char)
  - 如果没有超过左侧的范围，强转
  - 如果超过，报错
- 使用表达式时，所有的byte型. short型和char型将被提升到int型
- **编译器的常量优化**：在给变量进行赋值时，如果右侧表达式全是常量，没有任何变量，那么编译器javac将会先对常量表达式运算

## switch

匹配哪一个case就从哪一个位置向下执行，直到遇到break或者整体结束为止。

## IDEA 
### 项目目录
Project

Module

Package(英文小写，.,数字)
### 快捷键

|       快捷键       |                  功能                  |
| :----------------: | :------------------------------------: |
|     Alt+Enter      |          导入包，自动修正代码          |
|       Ctrl+Y       |            删除光标所在的行            |
|       Ctrl+D       | 复制光标所在行的内容，插入光标位置下面 |
|      Alt+Ins       |              自动生成代码              |
| Alt+Shift+上下箭头 |             移动当前代码行             |
|       .fori        |                遍历元素                |

## 重载

- 只改变参数的类型和顺序

## 数组

特点

- 引用类型         所有的引用类型变量，都可以赋值为null值。但是代表其中什么都没有。
- 数组长度在程序运行期间不可改变

初始化方式：

- 动态初始化（指定长度） 数据类型[] 数组名称 = new 数据类型[数组长度]；
- 静态初始化（指定内容）
- 可以拆分成两个部分，但是静态出
- 数组必须new初始化才能使用其中的元素，如果只是赋值一个null,没有new创建，那么将会发生空指针异常 NullPointException

元素获取

- 直接打印室得到数组对应的，内存地址哈希值

赋值

- 默认值
  - 整数 0
  - 浮点 0.0
  - 字符类型 ‘/u0000’
  - 布尔类型 false
  - 引用类型 null
- 静态初始化其实也有一个默认值过程，只不过系统马上将默认值替换为了大括号中的具体数值。

方法

- array.length   获取数组长度

内存图

![](D:\用户目录\我的文档\java_learning_notes\picture\数组内存.jpg)



## java内存

Java的内存划分为5个部分

1. ==栈(stack): 存放得我都是方法中的局部变量。==
   - 局部变量：方法的参数，或者方法中的变量
   - 作用域：一旦超出作用域，立刻从栈内存中消失
   - ==方法一定要在栈中运行==
2. ==堆（Heap）:凡是new 出来的东西，都在堆当中。==
   - 堆内存里的东西都有一个地址值：16进制
   - 堆内存中的数据都有默认值
3. 方法去（Method Area）:存储class相关信息，包含方法的信息
4. 本地方法栈（Native Method Stack）:与操作系统相关
5. 寄存器(pc Register):与内存相关

## 面向对象

当需要实现一个功能时，不关系具体的步骤，而是找到一个具有该功能的对象，实现该功能

三大特征

- 封装
  - 方法就是一种封装
  - private也是一种封装，将需要保护的成员变量进行修饰
- 继承
- 多态

使用

- 导包 import包名称.类名

标准类组成部分：

1. 成员变量都要使用private关键字修饰
2. 为每一个成员变量编写一对 getter/setter方法
3. 编写一个无参数的构造方法
4. 编写一个全参数的构造方法

## Scanner 类

java.lang包下的内容不要导包，其他都需要import语句。

## 匿名对象

- 只有右边的对象，没有左边的名字和赋值运算符

- new 类名称（）;

- 只能使用唯一的一次
- 使用建议：如果确定有一个对象只需要使用唯一的一次，就可以用匿名对象。

## ArrayList 

- 可变大小

- 从JDK 1.7+开始，右侧的尖括号内部可以不写内容，但是<>本身还是要写的

- 对于arraylist 直接打印不是地址值，而是内容。

- 如果是空的，则为[]

- 泛型只能是引用类型，不能是基本类型

  - 存储基本类型，必须使用基本类型的包装类

    - | 基本类型 |    包装类     |
      | :------: | :-----------: |
      |   byte   |     Byte      |
      |  short   |     Short     |
      |   int    |  ==Integer==  |
      |   long   |     Long      |
      |  float   |     Float     |
      |  double  |    Double     |
      |   char   | ==Character== |
      | boolean  |    Boolean    |

- 从JDK1.5 支持自动装箱，自动拆箱

常用方法

1. 添加元素 public boolean add(E e);
2. 获取元素 public E get(int index)
3. 删除元素 public E remove( int index);
4. 获取大小 public int size();

## 字符串

特点：

- 字符串的内容永不改变
- 因为不可改变，所以可以共享使用
- 字符串效果上相当于char[]字符数组，但是底层原理是byte[]字节数组。

创建方法：

1. String() 创建一个空白字符串，不含任何内容
2. String(char[] array),根据字符数组的内容，来创建对应的字符串
3. String(byte[] array)，根据字节数据的内容，来创建对应的字符串

字符串常量池

- 程序当中直接写上的双引号字符串，就在字符串常量池中
  - 基本类型，==比较的是值
  - 引用类型，==比较的是地址
- 在堆当中

![](D:\用户目录\我的文档\java_learning_notes\picture\字符串内存.png)

方法

- public boolean equals(object obj)参数可以是任何对象，只有参数是一个字符串并且内容相同的，才能true
  - 任何对象都能被equal接受
  - 一个常量一个变量，推荐把常亮放在前面，避免空指针
  - 区分大小写，只有英文区分大小写
- public String concat(String str):拼接字符串
- public char charAt(int index):获取指定索引位置的的那个字符
- public int indexOf(String str):查找参数字符串在本字符串当中首次出现的索引值，没有返回-1值
- 截取方法
  - public String substring(int index),从参数位置一直到字符串末尾，返回新字符串
  - public String substring(int begin,int end),截取从begin开始，一直到end结束
- 转换
  - public char[] toCharArray()，拆分成字符数组
  - public byte[] getBytes(),获得当前字符串底层的字节数组
  - public String replace(CharSequence oldString,CharSequence newString);
- 分割方法
  - public String[] split(String regex)按照参数规则，将字符串切分为若干个部分
  - 规则 ：正则表达式，如果用"."切分，要用“//.”

## 静态方法

关键字 static

- 可以用于对象计数器
- 静态方法不能访问非静态
  - 因为在内存当中，现有的静态内容，后有的非静态内容
- 静态方法不能用this关键字
- 方法区中有一块单独的静态区

![](D:\用户目录\我的文档\java_learning_notes\picture\静态成员内存.jpg)

## 数组工具类 Arrays

与数组相关的工具类，提供了大量静态方法，用来实现数组常见的操作

- public static String toString(数组):将参数数组变成字符串（按照默认格式：【元素1，元素2。。。】）
- public static void sort(数组)：按照默认升序（从小到大）对数组的元素进行排序
  - 如果是数值，sort默认按照升序从小到大
  - 如果是字符串，sort默认按照字母升序
  - 如果是自定义类，需要Comparable或者Comparator接口的支持

## 数学工具类 Math

- abs 绝对值
- ceil 向上取整
- floor 向下取证
- round 四舍五入

## 继承

继承是多态的前提，若果没有继承，就没有多态



