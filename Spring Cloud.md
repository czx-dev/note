### Spring Cloud

#### 	1.Eureka 注册中心（现使用Nacos）

#### Eueka使用

###### 1.引入pom依赖

服务端:

```java
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-eureka-server</artifactId>
</dependency>  
```

客户端:

```java
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-eureka</artifactId>
</dependency>
```

###### 2.启动类上增加 服务端:@EnableEurekaServer  客户端:@EnableEurekaClient

###### 3.配置文件增加

```
spring:
	application:
		name: 注册名
server:
	port: 8001
eureka:
	client:
		serviceUrl:
			defaultZone: Eureka 服务端ip 和端口/eureka
		#开启注册
		register-with-eureka: false
		#开启抓取
		fetch-registry = false
```

###### 4.注入

```java
@Bean#注入springboot
@LoadBalanced# 负载均衡
public RestTemplate restTemplate(){return new RestTemplate();}
```

###### 5.调用

```java
String url = "服务名/路由"
restTemplate.getForobject(url,接受类)
```



#### 			Nacos 使用

##### 				1.安装

###### 					下载解压包，解压，进入bin ，cmd startup.cmd 启动  一定要配置JAVA_HOME

##### 				2.检验是否成功

###### 					进入localhost:8848/nacos 账号/密码 ： nacos/nacos 

##### 				3.使用

###### 					pom 文件中添加依赖

###### 					配置文件中加上 

###### 						spring.cloud.cacos.discovery.server-addr=127.0.0.1:8848

###### 						spring.application.name=服务别名

###### 					启动类上添加 @EnableDiscoveryClient

##### 				4.刷新nacos 可到看服务即注册成功

### 	2.Fegin 服务调用

##### 				1.pom 引入依赖

##### 				2.在调用端（消费者）启动类上添加注解@EnabelFeginClients

##### 				3.调用端添加接口  @Component  @FeginClient(name="被调用端的注册名字")

##### 				4.定义（声明）你要调用方法的路径@RequetsMapping 需要全路径@Pathvariable需指定名称

##### 				5.使用时把接口注入即可

### 	3.Hystrix 熔断器(生产者挂掉的时候，拒绝请求，现在使用阿里的Sentinel)

##### 				1.pom 引入依赖

##### 				2.配置文件中  

###### 						fegin.hystrix.enable = true//开启熔断机制

###### 						hystrix.command.deufalt.execution.isolation.timeoutinMilliseconds=6000//设置超时时间

##### 				3.实现  Fegin 的自定义接口 //出错后执行

##### 				4.Fegin 接口注解@FeginClient(name="被调用端的注册名字",fallback=指定接口实现类)

### 4.GateWay(服务网关)

##### 	1.pom引入依赖

#####     2.application.properties配置文件

###### 		spring.cloud.gateway.locator.enable = ture//找到其他服务

###### 		spring.cloud.gateway.routes[0].id = 服务器名 

###### 		spring.cloud.gateway.routes[0].uri = lb://Nacos中注册的服务名    

###### 		spring.cloud.gateway.routes[0].predicates = Path =/访问路径  //哪些服务走这个 

## 5.Spring Cloud Config(配置中心，现使用 Nacos)

##### 	1.到Nacos 页面中配置

##### 	2.引入pom依赖

##### 	3.创建配置文件

###### 		spring.cloud.nacos.config.server-addr= 配置中心:8848

###### 		spring.application.name = 配置中心的dataId

​	

​		



​		





​	



​		