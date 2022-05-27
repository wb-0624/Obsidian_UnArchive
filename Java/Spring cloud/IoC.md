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
| Scope                    | singleton，prototype，request，session，application，websocket |
| Constructor arguments    | 构造依赖注入 |
| Properties               | setter依赖注入 |
| Autowiring mode          | 针对属性中的bean属性。autowire = “ ”<br />no：Bean引用由\<ref>定义<br />byName：自动寻找和name相同的beanID<br />byType：type唯一。<br />constructor：类似于byType，适用于构造函数参数。也是通过类型装配。 |
| Lazy initialization mode | lazy-init = true，默认是false。<br />默认是在ApplicationContext启动时就预初始化bean实例。<br />否则在初次使用到时，才进行实例化。 |
| Initialization method    | 1. Lookup Method inject 返回值注入<br />2. Arbitrary method replacement 任意方法注入 |
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

### 基于构造函数

```<constructor-arg name = "" value = ""></constructor-arg>```

name-value

index-value 从0开始

type-value 多种重复类型，不推荐使用。

### 基于setter

```<property name = "	" value = " "> </property>```

本质还是调用setter方法，所以必须是类中实现过setter方法才能使用。

> 基本使用name-value形式。
>
> value改成ref = "beanID"，引用其他bean。

------

由于可以混合使用基于构造函数的 DI 和基于 setter 的 DI，因此将构造函数用于强制性依赖项并将 setter 方法或配置方法用于可选依赖性是一个很好的法则。



依赖项详细配置

| type                         | value              |
| ---------------------------- | ------------------ |
| String，int等直值            | 由Spring自动转化   |
| idref bean = “beanID”        | 仅beanid，检查错误 |
| ref bean = "beanID/beanName" | 传递bean实例对象   |
| List,Set,Map,Property        |                    |

通过注解

```java
@Component
public class Person {
    @Value("Mike")
    private String name;
    @Value("18")
    private int age;

    @Autowired
    private Car car;

}
```

```java
@Component
public class Car {
    @Value("10000")
    private int price;
    @Value("BWM")
    private String logo;

}
```

```java
    @Test
    public void test1(){
        ApplicationContext context = new AnnotationConfigApplicationContext("com.example.demo.ioc");
        System.out.println(context.getBean("person"));
    }
```





## 方法注入

单例bean A依赖于非单例bean B时。

```java
public class User {

    private String name;
    private int age;
    // 依赖于car
    private Car car;

    // 为这个方法进行注入
   	public Car getCar() {
        return car;
    }
    
	// 省略其他setter和getter，以及toString方法
}

public class Car {
    private int speed;
    private double price;

    // 省略setter和getter，以及toString方法
}
```

```java
    Car c1 = user.getCar();
    Car c2 = user.getCar();
    Car c3 = user.getCar();
```

由于创建A时就创建了B，并且缓存在容器中。那么之后基于bean A获得的bean B永远是同一个，就失去了非单例的意义。

### 返回值注入

通过lookup

```xml
<!-- 将user的作用域定义为singleton -->
<bean id="user" class="cn.tewuyiang.pojo.User" scope="singleton">
    <property name="name" value="aaa" />
    <property name="age" value="28" />
    <!--
        配置查找方法注入，替换getCar方法，让他成为从spring容器中查找car的一个工厂方法
    查找名为getCar的函数，并且其返回值用bean car覆盖
    -->
    <lookup-method name="getCar" bean="car" />
</bean>

<!-- 将car的作用域定义为prototype -->
<bean id="car" class="cn.tewuyiang.pojo.Car" scope="prototype">
    <property name="price" value="9999.35" />
    <property name="speed" value="100" />
</bean>
```

```java
@Test
public void testXML() throws InterruptedException {
    // 创建Spring容器
    ClassPathXmlApplicationContext context =
        new ClassPathXmlApplicationContext("classpath:applicationContext.xml");
    // 获取User对象
    User user = context.getBean(User.class);
    // 多次调用getCar方法，获取多个car
    Car c1 = user.getCar();
    Car c2 = user.getCar();
    Car c3 = user.getCar();
    // 分别输出car的hash值，看是否相等，以此判断是否是同一个对象
    System.out.println(c1.hashCode());
    System.out.println(c2.hashCode());
    System.out.println(c3.hashCode());
    // 输出user这个bean所属类型的父类
    System.out.println(user.getClass().getSuperclass());
}
```

虽然三次getCar下的属性值是一样的，但是hashCode是不同的。证明了三次调用，返回的是三个不同的bean。

运用注解

```java
@Component
public class User {
    private String name;
    private int age;
    
    
    private Car car;

    // 使用Lookup注解，告诉Spring这个方法需要使用查找方法注入
    // 这里直接使用@Lookup，则Spring将会依据方法返回值
    // 将它覆盖为一个在Spring容器中获取Car这个类型的bean的方法
    // 但是也可以指定需要获取的bean的名字，如：@Lookup("car")
    // 此时，名字为car的bean，类型必须与方法的返回值类型一致
    @Lookup
    public Car getCar() {
        return car;
    }
    
    // 省略其他setter和getter，以及toString方法
    
}

@Component
@Scope("prototype")	// 声明为多例
public class Car {
    private int speed;
    private double price;

    // 省略setter和getter，以及toString方法
}
```

## 生命周期





# 注解

1.   生成文档。
2.   跟踪代码依赖性，实现替代配置文件功能。
3.   在编译时进行格式检查。 如@Override，是否覆盖超类的方法。

## @Request

用于setter方法，被注解的方法其属性必须在xml被配置。

## @Autowired

标注在属性、方法、构造器上来完成自动装配。通过Type进行注入。

如果同一类型下有多种实现，则可以通过@Qualifier("beanID")来指定注入的实例。

## @Resource

用于注入，@Resource(name = "")，未指定name就根据setter方法派生。

## @Primary

为自动装配下多个type相同的bean中，定出主要候选对象。主要和@Aurowired一起使用。

## @Component

bean注册

告诉spring此类需要被注入IoC。并且可以用过@Value进行依赖注入。

下面三个@Repository，@Service，@Controller是@Component的特化，用于更具体的用例。

## @Repository

持久层。

通常用于注解DAO[数据访问对象]。

## @Service

服务层

## @Controller

表示层。


## @Bean

注解方法，表示返回值就是一个bean，将其注入IoC。配合@Configuration【注解类】使用。

## @Profile

根据需要，配置多套环境，便于切换，不用一直修改配置文件。

