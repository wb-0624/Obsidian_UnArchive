# Nacos是什么

阿里开源的服务管理平台。

用于动态服务发现、配置和管理。

服务注册中心和配置中心的组合体。

# 服务注册中心

![](img/服务注册与发现.png)

- 服务注册中心：Nacos Server，可以为服务提供者和服务消费者提供服务注册和发现功能。
- 服务提供者：Nacos Client，用于对外服务。将自己提供的服务注册到注册中心。以供服务消费者发现和调用。
- 服务消费者：Nacos Client，用于消费服务。从服务注册中心获取服务列表，调用自己需要的服务。

# 使用

[Nacos：Spring Cloud Alibaba服务注册与配置中心（非常详细） (biancheng.net)](http://c.biancheng.net/springcloud/nacos.html)

按照上述网址进行。

```xml
<dependency>
    <groupId>com.alibaba.cloud</groupId>
    <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
</dependency>
```

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-alibaba-nacos-config</artifactId>
    <version>0.9.0.RELEASE</version>
</dependency>
```



# 服务发现

spring-cloud-starter-alibaba-nacos-discovery



