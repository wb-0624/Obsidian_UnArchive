# 注册中心

## 作用

将服务注册到注册中心，方便消费者调用。

## 怎么注册

添加需要的Nacos依赖

```xml
<dependency>
    <groupId>com.alibaba.cloud</groupId>
    <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
</dependency>
```

在模块的配置文件里配置好注册中心地址+端口即可。

```yml
spring:
	cloud:
		nacos:
			discovery:
				server-addr = localhost:8848
```

Dubbo框架下，可以直接配置或者挂载到Spring Cloud注册中心。

```yml
dubbo:
	registry:
		address: nacos://localhost:8848或spring-cloud://localhost
```

主启动类上添加@EnableDiscoveryClient，以便注册中心可以发现该服务。

## Nacos带来的便利

不使用注册中心时，如果消费者需要调用服务者提供的服务就需要使用rest访问服务者的URL来实现服务的调用。

```java
@RestController
@Slf4j
public class DeptController_Consumer {
    @Resource
    private RestTemplate restTemplate;
    @Value("${service-url.nacos-user-service}")
    private String serverURL; //服务提供者的服务名
    @GetMapping("/consumer/dept/nacos/{id}")
    public String paymentInfo(@PathVariable("id") Long id) {
        return restTemplate.getForObject(serverURL + "/dept/nacos/" + id, String.class);
    }
}
```



## Nacos功能介绍



# 配置中心

