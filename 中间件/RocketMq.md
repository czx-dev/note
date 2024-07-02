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
public class RocketMQConsumer implements MessageListenerConcurrently {

	//自定ACK应答逻辑
        @Override
        public ConsumeConcurrentlyStatus consumeMessage(List<MessageExt> msgs, ConsumeConcurrentlyContext context) {
            for (MessageExt messageExt : msgs) {
                log.debug("received msg: {}", messageExt);
                try {
                    long now = System.currentTimeMillis();
                    handleMessage(messageExt);
                    long costTime = System.currentTimeMillis() - now;
	                //消费逻辑
                    log.debug("consume {} cost: {} ms", messageExt.getMsgId(), costTime);
                } catch (Exception e) {
                    log.warn("consume message failed. messageId:{}, topic:{}, reconsumeTimes:{}", messageExt.getMsgId(), messageExt.getTopic(), messageExt.getReconsumeTimes(), e);
                    context.setDelayLevelWhenNextConsume(delayLevelWhenNextConsume);
                    return ConsumeConcurrentlyStatus.RECONSUME_LATER;
                }
            }

            return ConsumeConcurrentlyStatus.CONSUME_SUCCESS;
        }


}


```