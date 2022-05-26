# 什么是Spring？

spring boot 一种框架

spring cloud 基于boot的云应用开发工具

# 为什么要用spring？

行业标准。

更为方便。

# IoC

## Bean

不再是开发者手动创建对象。由IoC根据需求，配置文件自动创建对象。

通过类路径下寻找或系统文件路径下寻找xml配置文件。

IoC容器管理一个或多个bean，bean根据提供给容器的配置元数据(.xml)创建。xml通常放置在resource文件夹下。

容器内，bean定义表示为BeanDefinition对象。包含以下属性：

xml里

| Property                 | Expalined in ... |
| ------------------------ | ---------------- |
| Class                    | 创建一个什么类    |
| Name                     | 名称，id         |
| Scope                    |                  |
| Constructor arguments    |                  |
| Properties               | 为对象属性赋值  |
| Autowiring mode          |                  |
| Lazy initialization mode |                  |
| Initialization method    |                  |
| Destruction method       |                  |

bean 命名：小写开头，遵循驼峰规则。id最多一个，name任意个。

bean实例化的三种方式:

1. ==用构造函数实例化==【常用】

直接通过spring工厂返回实例对象。

xml文件配置 放在resource/bean目录下

``` xml
<?xml version="1.0" encoding="UTF-8"?>  
<beans xmlns="http://www.springframework.org/schema/beans"  
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd"> 
	
<bean class = "com.example.demo.ioc.Bean1" id = "bean1">  
    <property name="driverName" value="wb"></property>  
    <property name="url" value="www.baidu.com"></property>  
</bean>  
	
</beans>

```

> IoC在需要用到bean1的时候，进行创建。创建一个类型为Bean1名称为bean1的对象。并对其内的属性进行赋值。

> 参数赋值：name-value形式。指定下标[construtor-arg]。指定类型。

类配置

```java
package com.example.demo.ioc;  
    
import lombok.Data;  
  
@Data  //自动创建一些getter，setter等方法
public class DataConfig {  
    public String url;  
    public String driverName;  
}
```


测试
``` java
ApplicationContext context = new ClassPathXmlApplicationContext("bena/Bean.xml")
System.out.println(context.getBean("bean1"));
```


输出
``` shell
DataConfig(url=www.baidu.com, driverName=wb)
```
2. 使用静态工厂方法实例化

spring工厂通过调用自定义工厂的静态方法去返回类的实例对象。

需要一个bean类，以及一个工厂类(有静态方法可以创建bean的实例)。



class Bean2

```java
package com.example.demo.ioc;

public class Bean2 {

}
```

class Bean2Factory

```java
package com.example.demo.ioc;

public class Bean2Factory {
    public static Bean2 getBean2(){
        System.out.println("use static method");
        return new Bean2();
    }
}
```

xml配置

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id = "bean2" class = "com.example.demo.ioc.Bean2Factory" factory-method="getBean2">

    </bean>
</beans>
```

测试

```java
@Test
public void Bean2(){
    ApplicationContext context = new ClassPathXmlApplicationContext("bean/Bean2.xml");
    Bean2 bean2 = (Bean2)context.getBean("bean2");
    System.out.println(bean2);
}
```

> 调用Bean2Factory类的静态方法，创建一个名为bean2的对象。静态方法可以直接用类名.方法名调用。



3. 使用实例工厂方法实例化

先实例化一个工厂类，通过实例化工厂对象中的非静态方法来创建bean，并注入到容器中。在xml配置文件bean中

> facroty-bean指定拥有该工厂方法的Bean。
> factory-method指定该工厂方法的名称。
> construtor-arg元素为工厂传递方法参数。

class Bean3

```java
package com.example.demo.ioc;

public class Bean3 {
}
```

class Bean3Factory

```java
package com.example.demo.ioc;

public class Bean3Factory {
    public Bean3 getBean3(){
        System.out.println("normal method");
        return new Bean3();
    }
}
```

Bean3.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
<bean class = "com.example.demo.ioc.Bean3Factory" id = "bean3Factory">  </bean>
    <bean id = "bean3" factory-bean="bean3Factory" factory-method="getBean3"></bean>
</beans>
```

> 与静态方法可以直接由类名调用不同，实例方法必须先实例化再调用。
>
> [1]. 先实例化Bean3Factorty类
>
> [2]. 再调用实例化方法来创建bean3对象。

> “ factory bean”是指在 Spring 容器中配置并通过instance或static工厂方法创建对象的 Bean。



测试

```java
@Test
public void Bean3(){
    ApplicationContext context = new ClassPathXmlApplicationContext("bean/Bean3.xml");
    Bean3 bean3 = (Bean3)context.getBean("bean3");
    System.out.println(bean3);
}
```

## DI(依赖注入)

【属性赋值？】

基本使用name-value形式。

### 基于构造函数

<constructor-arg>

name-value

index-value 从0开始

type-value

### 基于setter

<property>

本质还是调用setter方法。

------

由于可以混合使用基于构造函数的 DI 和基于 setter 的 DI，因此将构造函数用于强制性依赖项并将 setter 方法或配置方法用于可选依赖性是一个很好的经验法则。


# AOP

## 概念

借助于IoC

重复的代码片段，抽象成对象，统一处理。相似位置使用相似的代码。

![AOP](AOP.excalidraw.md)

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

