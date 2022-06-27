# OpenFeign

Feign是一个声明式的Web服务客户端，让编写Web服务客户端变得非常容易，只需创建一个接口并在一个接口上添加注解即可。

# 如何使用

添加依赖

```xml
    <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-starter-openfeign</artifactId>
    </dependency>
```

主启动类上添加@EnableFeignClients

声明该项目是Feign客户端，扫描对应的feign client。

使用OpenFeign的好处：不需要自己去拼接url，交由feign去做。