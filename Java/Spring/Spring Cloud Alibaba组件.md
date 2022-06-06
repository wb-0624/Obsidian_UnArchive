# Spring Cloud Alibaba

```xml
<dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>com.alibaba.cloud</groupId>
            <artifactId>spring-cloud-alibaba-dependencies</artifactId>
            <version>2.2.7.RELEASE</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>
    </dependencies>
</dependencyManagement>
```



Nacos 一个alibaba开源的平台。使用Spring Cloud Alibaba Nacos Discovery，可基于Spring Cloud 的变成模型快速ieruNacos服务注册功能。

# Nacos Discovery 服务注册/发现

服务发现是微服务架构体系中最关系的组件之一。手动配置太复杂，利用Nacos Discovery 可以将服务自动注册到Nacos

## 如何引入Nacos Discovery进行服务发现

```xml
<dependency>
    <groupId>com.alibaba.cloud</groupId>
    <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
</dependency>
```

