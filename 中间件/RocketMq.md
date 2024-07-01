1. 为什么要保持订阅关系一致性
	 定义:  一个GroupId 下 要保证 topic 和 tag 一致 
	 后果:  容易造成消息丢失
	 原因:  根据TopicGroup 注册到brocker 的时候 会循环覆盖。

2. 手动ACK 应答机制
```java 
//生产者
import org.apache.rocketmq.spring.core.RocketMQTemplate;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.CommandLineRunner;
import org.springframework.stereotype.Component;

@Component
public class RocketMQProducer implements CommandLineRunner {

    @Autowired
    private RocketMQTemplate rocketMQTemplate;

    @Override
    public void run(String... args) throws Exception {
        // 发送消息到指定的topic
        String topic = "your_topic";
        String message = "Hello, RocketMQ!";
        rocketMQTemplate.convertAndSend(topic, message);
        System.out.println("Message sent: " + message);
    }
}


```

```java
//消费者
import org.apache.rocketmq.client.consumer.DefaultMQPushConsumer;
import org.apache.rocketmq.client.consumer.listener.ConsumeConcurrentlyContext;
import org.apache.rocketmq.client.consumer.listener.ConsumeConcurrentlyStatus;
import org.apache.rocketmq.client.consumer.listener.MessageListenerConcurrently;
import org.apache.rocketmq.client.exception.MQClientException;
import org.apache.rocketmq.common.message.MessageExt;
import org.springframework.context.annotation.Bean;
import org.springframework.stereotype.Component;

import java.util.List;

@Component
@RocketMQMessageListener( topictopic = ,  consumerGroup = ,  messageModel =  )
public class RocketMQConsumer implements RocketMQListener<>, MessageListenerConcurrently {
    @Override
    public void onMessage(RocketMQWebSocketMessage message) {
        rocketMQWebSocketMessageSender.send(message.getSessionId(),
                message.getUserType(), message.getUserId(),
                message.getMessageType(), message.getMessageContent());
    }
    
}


```