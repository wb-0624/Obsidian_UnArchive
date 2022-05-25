# 什么是Spring？

spring boot 一种框架

spring cloud 基于boot的云应用开发工具

# 为什么要用spring？

行业标准。

两大特性 IoC，AOP。

从代码上讲：一个Spring容器就是某个实现了ApplicationContext接口的类的实例。也就是说，从代码层面，Spring容器其实就是一个ApplicationContext（一个实例化对象）。

通常是对xml配置文件来实例化。

通过类路径下寻找或系统文件路径下寻找配置文件。

# IoC

不再是开发者手动创建对象。由IoC根据需求，配置文件自动创建对象。

创建对象的容器。

IoC容器管理一个或多个bean，bean根据提供给容器的配置元数据(.xml)创建。xml通常放置在resource文件夹下。

Bean 定义实质上是创建一个或多个对象的方法。

容器内，bean定义表示为BeanDefinition对象。包含以下属性：

xml里

| Property                 | Expalined in ... |
| ------------------------ | ---------------- |
| Class                    |                  |
| Name                     |                  |
| Scope                    |                  |
| Constructor arguments    |                  |
| Properties               |                  |
| Autowiring mode          |                  |
| Lazy initialization mode |                  |
| Initialization method    |                  |
| Destruction method       |                  |

bean 命名：小写开头，遵循驼峰规则。id最多一个，name任意个。

bean实例化的三种方式:

1. 用构造函数实例化
``` xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="accountDao"
        class="org.springframework.samples.jpetstore.dao.jpa.JpaAccountDao">
        <!-- additional collaborators and configuration for this bean go here -->
		<property name = " " value = " "></property>
		<!-- 填成员变量及其值-->
    </bean>

    <bean id="itemDao" class="org.springframework.samples.jpetstore.dao.jpa.JpaItemDao">
        <!-- additional collaborators and configuration for this bean go here -->
    </bean>

    <!-- more bean definitions for data access objects go here -->

</beans>

```

``` java
ApplicationContext context = new ClassPathXmlApplicationContext("xxx.xml")
System.out.println(context.getBean("accountDao"))//打印accountDao
```

2. 使用静态工厂方法实例化



3. 使用实例工厂方法实例化




# AOP

## 概念

借助于IoC

重复的代码片段，抽象成对象，统一处理。相似位置使用相似的代码。

![AOP](../img/AOP.excalidraw)

和平时使用的对象类（Student，Teacher，Admin）相比，更为抽象，描述的不是具体的对象，而是某些相似位置的流程。抽象化的面向对象编程。【将非业务重复代码抽离出来？】保留核心的业务代码。核心业务和非业务代码的解耦合。

和函数的区别？

函数需要调用，AOP只需要配置。

## 实现

1. 创建切面类，需要声明@Aspect
2. 注入IoC
3. 目标方法和切面对象连接
    Joinpoint，获取目标方法的包括名称，参数等数据。
4. 方法执行前，方法执行后等区分。【方法执行中间是否可以？】



# 注解

1.   生成文档。
2.   跟踪代码依赖性，实现替代配置文件功能。
3.   在编译时进行格式检查。 如@Override，是否覆盖超类的方法。

