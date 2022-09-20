# Dubbo 使用

## 	1.整合springboot 

​			1.引入pom依赖				

```java
        <dependency>
            <groupId>com.alibaba.spring.boot</groupId>
            <artifactId>dubbo-spring-boot-starter</artifactId>
            <version>2.0.0</version>
        </dependency>
        <!-- 引入zookeeper的依赖 -->
        <dependency>
            <groupId>com.101tec</groupId>
            <artifactId>zkclient</artifactId>
            <version>0.10</version>
        </dependency>
```

​			2.添加配置

```java 
#别名  一般与项目或者模块名一致
spring.dubbo.application.name=dubbo-provider
#注册中心 所在ip 和协议
spring.dubbo.application.registry=zookeeper://192.168.0.128:2181
```

​			3.启动类添加注解  @EnableDubboConfiguration

​			4.提供方@Service  dubbo下的

​			5.消费方@Reference  远程调用

​		**注意： 先启动提供方再启动消费方**

## 	2.附加知识点

​			1.序列化：提供方与消费方的对象传输  需要对象 实现Serialized接口

​			2.本地缓存：dubbo会在首次访问进行缓存，当服务地址修改时 注册中心会通知

​			3.超时与重试:@Service(timeout=,retries=)

​			4.多版本：@Service(vsersion=)  @Reference（vsersion=）

​			5.负载均衡：@Reference(loadbalance=)  #random 随机 roundrobin 权重轮询 consistentHash 相同参数 发给同一个提供者

​			6.集群容错：@Reference(cluster=)

​			7.服务降级：@Reference(mock=)

