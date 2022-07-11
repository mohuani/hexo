---
title: "入门RabbitMQ核心概念"
date: 2022-07-10 11:36:39
draft: true
tags: [数据库, RabbitMQ]
categories:
- [数据库, RabbitMQ]
---

### 2-5 AMQP核心概念讲解

![img](https://user-images.githubusercontent.com/21000558/178130345-f57f3ad6-bc48-4d8f-838d-4e9a27bfd110.png)

![img_1](https://user-images.githubusercontent.com/21000558/178130347-d4895fe2-df2b-46ae-8d85-9b291348dc1f.png)

![img_2](https://user-images.githubusercontent.com/21000558/178130355-c5b45f7a-72e2-4790-b04e-3b3d1e50ac48.png)


![img_3](https://user-images.githubusercontent.com/21000558/178130358-976aa6f3-6a31-4793-be3a-b5b3109d412f.png)

![img_4](https://user-images.githubusercontent.com/21000558/178130363-293f8998-e17e-4d3a-b3f8-995e795631a9.png)



### 2-10 生产者消费者模型构建

**Procuder**
```java
package com.bfxy.rabbitmq.quickstart;

import com.rabbitmq.client.Channel;
import com.rabbitmq.client.Connection;
import com.rabbitmq.client.ConnectionFactory;

public class Procuder {

	
	public static void main(String[] args) throws Exception {
		//1 创建一个ConnectionFactory, 并进行配置
		ConnectionFactory connectionFactory = new ConnectionFactory();
		connectionFactory.setHost("192.168.11.76");
		connectionFactory.setPort(5672);
		connectionFactory.setVirtualHost("/");
		
		//2 通过连接工厂创建连接
		Connection connection = connectionFactory.newConnection();
		
		//3 通过connection创建一个Channel
		Channel channel = connection.createChannel();
		
		//4 通过Channel发送数据
		for(int i=0; i < 5; i++){
			String msg = "Hello RabbitMQ!";
			//1 exchange   2 routingKey
			channel.basicPublish("", "test001", null, msg.getBytes());
		}

		//5 记得要关闭相关的连接
		channel.close();
		connection.close();
	}
}


```

**Consumer**
```java
package com.bfxy.rabbitmq.quickstart;

import com.rabbitmq.client.Channel;
import com.rabbitmq.client.Connection;
import com.rabbitmq.client.ConnectionFactory;
import com.rabbitmq.client.Envelope;
import com.rabbitmq.client.QueueingConsumer;
import com.rabbitmq.client.QueueingConsumer.Delivery;

public class Consumer {

	public static void main(String[] args) throws Exception {
		
		//1 创建一个ConnectionFactory, 并进行配置
		ConnectionFactory connectionFactory = new ConnectionFactory();
		connectionFactory.setHost("192.168.11.76");
		connectionFactory.setPort(5672);
		connectionFactory.setVirtualHost("/");
		
		//2 通过连接工厂创建连接
		Connection connection = connectionFactory.newConnection();
		
		//3 通过connection创建一个Channel
		Channel channel = connection.createChannel();
		
		//4 声明（创建）一个队列
		String queueName = "test001";
		channel.queueDeclare(queueName, true, false, false, null);
		
		//5 创建消费者
		QueueingConsumer queueingConsumer = new QueueingConsumer(channel);
		
		//6 设置Channel
		channel.basicConsume(queueName, true, queueingConsumer);
		
		while(true){
			//7 获取消息
			Delivery delivery = queueingConsumer.nextDelivery();
			String msg = new String(delivery.getBody());
			System.err.println("消费端: " + msg);
			//Envelope envelope = delivery.getEnvelope();
		}
		
	}
}

```



### 2-12 交换机Exchanges

direct Exchanges
![image](https://user-images.githubusercontent.com/21000558/178230482-2908be59-891d-48e1-bb39-44128764e69f.png)

![image](https://user-images.githubusercontent.com/21000558/178231287-0080c23c-71b0-43cb-9684-3b09883bf1f7.png)

![image](https://user-images.githubusercontent.com/21000558/178231406-da50cb0e-6216-4896-9f13-46187db4baa0.png)

![image](https://user-images.githubusercontent.com/21000558/178231515-7338999e-0ede-4a84-8bab-6d692fc2a482.png)

![image](https://user-images.githubusercontent.com/21000558/178231568-8ada756c-d80d-4d07-847a-19ace5927d08.png)

topic Exchanges

![image](https://user-images.githubusercontent.com/21000558/178233229-f99b8467-d16f-4396-bd60-a5783ca69d3a.png)

![image](https://user-images.githubusercontent.com/21000558/178233346-7f712292-a811-4ed2-b90a-1686a0bb6364.png)












