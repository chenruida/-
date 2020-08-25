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



