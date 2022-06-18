幂等：相同的参数得到相同的返回结果。

非幂等：相同的参数可以有不同的返回结果。



# Dubbo属性配置
```properties
dubbo.protocol.host 设置主机地址


```


# @DubboReference、@DubboService

可以设置如下属性

## loadblance 负载均衡

负载均衡也是一种路由控制。

+ random，加权随机。
    + 按权重设置进行随机，权重越高，被随机到的概率越大，调用量越大分布越均匀。
    + 缺点：存在慢的提供者累计请求的问题。比如：第二台机器很慢，但没挂，当请求调到第二台时，就卡住了，久而久之，所有请求都卡在第二台机器了。
+ roundrobin，加权轮询。
    + 按公约后的权重设置轮询比率。
    + 缺点：同样存在慢的提供者累计请求的问题。

+ leastactive，加权最少活跃调用优先。
    + 活跃数越低越先调用。相同活跃数进行加权随机。
    + **这里的活跃数不是指调用次数，而是请求发送数-响应返回书。表示特定提供者的任务堆积量。**
    + 活跃数越低表示该提供者处理能力越强。
    + 使慢的提供者收到更少的请求，处理能力越强的节点，处理更多的请求。
+ shortestresponse，加权最短响应优先。
    + 在最近一个滑动窗口中，响应时间越短，越优先调用。相同时间进行加权随机。
    + 缺点：可能会造成流量过于集中于高性能节点。如果高性能节点出现问题，就会造成大量请求报错。
    + 这里的响应时间：某个提供者在窗口时间内的平均响应时间。窗口时间默认30s。
+ consistenthash，一致性Hash。
    + 相同参数的请求总是发到同一提供者。
    + 当某一平台提供者挂掉时，发往该提供者的请求会基于虚拟节点，平摊到其他提供者，不会引起剧烈变动。
    + 默认只对传入的第一个参数Hash，如要修改，配置`<dubbo:parameter key = "hash.argumets" value = "0,1"`
    + 默认用160份虚拟节点，如要修改，配置`<dubbo:parameter key  = "hash.nodes" value = "320"`


## cluster 容错方式

+ failover，失败自动切换
  + 服务调用失败后，会切换到其他集群的其他机器重试，默认重试次数为2。可以修改属性retries = 2。
  + 这种容错模式通常用于读操作，如果是事务型操作会带来数据重复错误。

+ failfast，快速失败
  + 服务调用失败后，立即报错，只发起一次调用。通常用于幂等操作。
  + 比如：新增数据时，可能请求在服务器中已经处理成功，因为网络的原因延迟导致相应失败，为了在不确定的情况下导致数据重复插入的问题，可以使用这种容错机制。


+ failsafe，失败安全。
  + 出现异常时，直接忽略。


+ failback，失败后自动回复。
  + 出现异常后，在后台记录这条失败的请求，定时重发。
  + 适用于消息通知操作，保证请求一定发送成功。


+ forking，并行调用集群中的多个服务。
  + 只要其中一个成功就返回，可以通过forks = 2进行设置最大并行数。


+ broadcast，广播调用所有的服务提供者。
  + 任意一个服务报错则表示服务调用失败。
  + 通常用于通知所有的服务提供者更新缓存或者本地资源信息。

实际应用中，查询语句容错建议使用默认的Failover Cluster，而增删改操作建议使用Failfast Cluster或者使用Failover Cluster(retries ="0")策略，防止数据重复添加。

建议在设计接口时把查询方法单独做成一个接口提供查询。

## mock 服务降级

所谓降级，就是把一些非必要的功能在流量较大的时间段暂时关闭。从而释放更多的资源来保障核心服务的运行。
```java

public class MockHelloService implements IHelloService{
    public String say(String s){
    return "Sorry,服务无法访问，返回降级数据";
    }
}


@RestController
public class HelloController{
    @DubboReference(mock = "MockHelloService",cluster = "failfast)
    
    private IHelloServide iHelloService;
    @GetMapping("/say")
    public String sayHello(){
    retutn iHelloService.sayHello();
    }
}

```
返回值超过默认时间时，访问/say接口得到就是`MockHelloService`中返回的数据。
有点类似fallback和BlockHandler。(在@SentinelResource)

