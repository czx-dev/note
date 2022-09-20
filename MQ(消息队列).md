# MQ(消息队列)

账号密码：admin

### 	什么是中间件

###### 					实现跨平台通讯，高可用，持久化

## 	RabbitMq六种工作模式

#### 			Simple简单模式

###### 						一对一

#### 			Work模式

###### 						一对多

##### 				两种消费模式

###### 						轮询模式

###### 						公平分发模式：能者多多劳	

#### 			layout模式	

###### 						一对多+交换机（不是默认的交换机）

#### 			Routing模式

###### 						增加route key 对队列进行分组，消息分组发送

#### 			Topics 模式

###### 						支持模糊查询 # 多少都可以 , * 一级

### Springboot 整合

###### 		1.导入依赖 amqp

###### 		2.声明交换机，队列与关系

###### 		3.利用 rabbitmqTemplate 发送消息

###### 		4. @RabbitMqListener(queue="") 类 @RabbitHandler 方法 参数需要有个Sting

### 参数说明

###### 		TTL：消息过期时间

###### 		死信队列： 当消息过期，被拒绝时存放的队列

### 分布式应用

###### 	可靠生产问题：跨平台事务回滚

###### 		定时重发 ：利用消息回执机制

###### 		消费者消费时出现异常会出现无限循环  

###### 		解决方案：1.配置重试次数

 ```java
  BindingBuilder.bind(queue).to(exchange).with(bindingKey)
  参数说明:
 	queue:队列实例对象
 	exchange:交换机实例对象
     bindingKey: 绑定关系
 ```



``

