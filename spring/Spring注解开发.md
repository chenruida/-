# Spring注解开发

## 概念理解

### 反射

**反射就是在运行时才知道要操作的类是什么，并且可以在运行时获取类的完整构造，并调用对应的方法。**

~~~java
Apple apple = new Apple(); //直接初始化，「正射」
apple.setPrice(4);
~~~

~~~java
Class clz = Class.forName("com.example.reflect.Apple"); //获取类的 Class 对象实例
Method method = clz.getMethod("setPrice", int.class);//获取方法的 Method 对象
Constructor constructor = clz.getConstructor();//根据 Class 对象实例获取 Constructor 对象
Object object = constructor.newInstance();//使用 Constructor 对象的 newInstance 方法获取反射类对象
method.invoke(object, 4);//利用 invoke 方法调用方法
~~~



## 原始注解

|      注解      |                      说明                      |
| :------------: | :--------------------------------------------: |
|   @Component   |            使用在类上用于实例化Bean            |
|  @Controller   |                web层实例化Bean                 |
|    @Service    |              service层实例化Bean               |
|  @Repository   |                dao层实例化Bean                 |
|   @Autowired   |      使用在字段上用于根据数据类型依赖注入      |
|   @Qualifier   | 结合@Autowired一起使用用于根据名称进行依赖注入 |
|   @Resource    |  相当于@Autowired+@Qualifier按照名称进行注入   |
|     @Value     |                  注入普通属性                  |
|     @Scope     |               标注Bean的作用范围               |
| @PostConstruct |    使用在方法上标注该方法是Bean的初始化方法    |
|  @PreDestroy   |     使用在方法上标注该方法是Bean的销毁方法     |



## 新注解

|      注解       |                             说明                             |
| :-------------: | :----------------------------------------------------------: |
| @Configuration  | 用于指定当前类是一个Spring配置类，当创建容器时会从该类上加载注解 |
| @ComponentScan  | 用于指定Spring在初始化容器时要扫描的包。<br>作用和在Spring的xml配置文件中的<br><context:component-scan base-package="com.example" /> 一样 |
|      @Bean      |     使用在方法上，标注将该方法的返回值存储到Spring容器中     |
| @PropertySource |                用于加载properties文件中的配置                |
|     @Import     |                      用于导入其他配置类                      |

# Spring 集成Junit

- 让SpringJunit负责创建Spring容器，但是需要将配置文件的名称告诉它
- 将需要进行测试Bean直接在测试类中进行注入

## 步骤：

1. 导入spring继承junit的坐标
2. 使用@Runwith注解替换原来的运行期
3. 使用@contextConfiguration指定配置文件或配置类
4. 使用@Autowired注入需要测试的对象
5. 创建测试方法进行测试