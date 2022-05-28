# AOP

## 概念



借助于IoC

重复的代码片段，抽象成对象，统一处理。相似位置使用相似的代码。

![AOP](AOP.excalidraw.md)

和平时使用的对象类（Student，Teacher，Admin）相比，更为抽象，某些相似位置的流程。保留核心的业务代码。核心业务和非业务代码的解耦合。

和函数的区别？

函数需要调用，AOP只需要配置。



## AOP术语

---

- 建议、通知(Advice)

在特定的连接点，AOP框架执行的动作。

| Advice          | Description                                                  |
| :-------------- | :----------------------------------------------------------- |
| Before          | 目标方法被调用前                                             |
| After           | 目标方法被调用后                                             |
| After-returning | 目标方法成功执行后                                           |
| After-throwing  | 目标方法抛出异常后                                           |
| Around          | 目标方法被调用前和调用后<br />决定这个方法什么时候执行，如何执行，是否执行。 |

使用建议时，使用最简单的。

order，优先级高的后运行。

- 连接点(Join point)

允许使用通知的地方，都算连接点。

包括方法调用前，调用后，返回，抛出异常等。

- 切点(Point cut)

你的一个类里，有15个方法，那就有几十个连接点，但是并不想在所有方法附近都使用通知，你只想让其中的几个，在调用这几个方法之前，之后或者抛出异常时干点什么，那么就用切点来定义这几个方法，让切点来筛选连接点，选中那几个你想要的方法。

Advice通过Point cut来连接和接入Join Point。

- 切面(Aspect)

通知和切入点组成切面。

定义，在哪些方法，什么时候进行通知。

- 引入(Introduction)

给一个现有的类，在不改变现有代码的情况下，添加方法或字段属性。

- 目标对象(Target object)

目标类，核心业务逻辑方法。

- AOP代理(AOP proxy)

一个类被AOP织入通知后，就产出了一个结果类，它是融合了原类和通知逻辑的代理类。

- 织入(Weaving)

将通知添加到目标类具体连接点上的过程。



## 实现大致步骤

1. 声明切面类，并把业务逻辑组件和切面类都加入到容器中。
2. 在切面类上的每一个通知方法都标注通知注解，和切入点表达式。
3. 开启基于注解的aop模式。

## 声明
``` java
package io.binghe.spring.aop;

import org.aspectj.lang.JoinPoint;
import org.aspectj.lang.annotation.*;

import java.util.Arrays;

/**
 * @author binghe
 * @version 1.0.0
 * @description Spring的切面类
 */

@Aspect                                         //声明切面类
public class LogAspect {

    @Pointcut("execution(public int io.binghe.spring.aop.MatchCalculator.*(..))")  //切入点表达式(指定切面作用于哪些包下的哪些方法)
    public void pointCut(){}
     
    @Before("pointCut()")
    public void logStart(JoinPoint joinPoint){
        System.out.println(joinPoint.getSignature().getName() + "运行...参数列表是：{"+ Arrays.asList(joinPoint.getArgs()) +"}");
    }
     
    @After("pointCut()")
    public void logEnd(JoinPoint joinPoint){
        System.out.println(joinPoint.getSignature().getName() + "结束...");
    }
     
    @AfterReturning(value = "pointCut()", returning = "result")
    public void logReturn(JoinPoint joinPoint, Object result){
        System.out.println(joinPoint.getSignature().getName() + "正常返回...运行结果：{"+result+"}");
    }
     
    @AfterThrowing(value = "pointCut()", throwing = "e")
    public void logException(JoinPoint joinPoint, Exception e){
        System.out.println(joinPoint.getSignature().getName() + "异常...异常信息：{"+e+"}");
    }
}
```

注入IoC

```java
package io.binghe.spring.config;

import io.binghe.spring.aop.LogAspect;
import io.binghe.spring.aop.MatchCalculator;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.EnableAspectJAutoProxy;

/**

 * @author binghe

 * @version 1.0.0

 * @description Spring配置类
   */
   @Configuration
   @EnableAspectJAutoProxy      //开启Aspect
   public class MainConfigOfAOP {

   @Bean
   public MatchCalculator matchCalculator(){
       return new MatchCalculator();
   }

   @Bean
   public LogAspect logAspect(){
       return new LogAspect();
   }
   }
```

测试

```java
package io.binghe.spring.test;
 
import io.binghe.spring.aop.MatchCalculator;
import io.binghe.spring.config.MainConfigOfAOP;
import org.junit.Before;
import org.junit.Test;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;
 
/**
 * @author binghe
 * @version 1.0.0
 * @description AOP实例测试类
 */
public class IOCTestAOP {
 
    private AnnotationConfigApplicationContext applicationContext = null;
 
    @Before
    public void initTest(){
        applicationContext = new AnnotationConfigApplicationContext(MainConfigOfAOP.class);
    }
 
    @Test
    public void test01(){
        MatchCalculator calculator = applicationContext.getBean(MatchCalculator.class);
        calculator.div(1, 0);
    }
}
```



