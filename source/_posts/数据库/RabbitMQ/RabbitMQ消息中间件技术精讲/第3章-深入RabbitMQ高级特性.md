---
title: "深入RabbitMQ高级特性"
date: 2022-07-11 11:36:39
draft: true
tags: [数据库, RabbitMQ]
categories:
- [数据库, RabbitMQ]
---


### 3-2 消息如何保障 100% 的投递成功方案

![image](https://user-images.githubusercontent.com/21000558/178245653-cab112cb-0d21-4b5e-8f43-45e3e8ec5804.png)

![image](https://user-images.githubusercontent.com/21000558/178246138-8ac18475-febb-49f8-99d5-5f2db8dcccc0.png)

![image](https://user-images.githubusercontent.com/21000558/178246178-cc85672b-c1e6-4ba5-be5a-4e827224940d.png)

![image](https://user-images.githubusercontent.com/21000558/178245466-390e8542-21f4-4ee8-b5c5-e464531ad085.png)

![image](https://user-images.githubusercontent.com/21000558/178247709-07387a36-ed30-4e20-baef-a4e3a22289ef.png)


### 3-4 幂等性概念及业界主流解决方案

![image](https://user-images.githubusercontent.com/21000558/178248907-3452d74e-3bd8-45bd-bf5c-7c0f6626fd25.png)

![image](https://user-images.githubusercontent.com/21000558/178248991-f5c8c237-c457-4419-aa44-36d3b5779801.png)

![image](https://user-images.githubusercontent.com/21000558/178250023-4f534f15-d4d2-4b1a-9f67-6194b33c2ae0.png)

### 3-5 Confirm确认消息详解 

![image](https://user-images.githubusercontent.com/21000558/178250530-b31f7a7e-5080-4c8e-a0a4-8b052816ee76.png)

![image](https://user-images.githubusercontent.com/21000558/178250582-200358da-2326-4ff6-b3c4-1ea8874b1da5.png)

![image](https://user-images.githubusercontent.com/21000558/178250482-12780f08-ab39-43e8-801a-7a0c7aaa7d26.png)


### 3-6 Return返回消息详解
![image](https://user-images.githubusercontent.com/21000558/178251148-86d4c4d2-5edb-4152-bd43-2384296b7e71.png)

![image](https://user-images.githubusercontent.com/21000558/178251882-7e841ccf-5bfa-4054-a985-a94863736006.png)

![image](https://user-images.githubusercontent.com/21000558/178252067-120bd2bd-1cf5-4eed-81d6-54c0b250826d.png)


### 3-7 自定义消费者使用 

![image](https://user-images.githubusercontent.com/21000558/178252192-9fce26c8-c970-425e-b3d7-001b3c957889.png)


### 3-8 消费端的限流策略

![image](https://user-images.githubusercontent.com/21000558/178393395-187e00c3-83ac-46b9-8a7b-c7a67b478749.png)

![image](https://user-images.githubusercontent.com/21000558/178393677-67724f12-b8a0-4707-a8e1-ff1b084a5ea4.png)

![image](https://user-images.githubusercontent.com/21000558/178393636-7811fd92-6b3d-47e7-be10-84d31200e162.png)

![image](https://user-images.githubusercontent.com/21000558/178393737-d14e465b-8cc3-4759-b8d3-ceb57cc9c8d7.png)

**Producer**
```java
package com.bfxy.rabbitmq.api.limit;

import com.rabbitmq.client.Channel;
import com.rabbitmq.client.Connection;
import com.rabbitmq.client.ConnectionFactory;

public class Producer {

	
	public static void main(String[] args) throws Exception {
		
		ConnectionFactory connectionFactory = new ConnectionFactory();
		connectionFactory.setHost("192.168.11.76");
		connectionFactory.setPort(5672);
		connectionFactory.setVirtualHost("/");
		
		Connection connection = connectionFactory.newConnection();
		Channel channel = connection.createChannel();
		
		String exchange = "test_qos_exchange";
		String routingKey = "qos.save";
		
		String msg = "Hello RabbitMQ QOS Message";
		
		for(int i =0; i<5; i ++){
			channel.basicPublish(exchange, routingKey, true, null, msg.getBytes());
		}
		
	}
}

```

**Consumer**
```java
package com.bfxy.rabbitmq.api.limit;

import com.rabbitmq.client.Channel;
import com.rabbitmq.client.Connection;
import com.rabbitmq.client.ConnectionFactory;
import com.rabbitmq.client.QueueingConsumer;
import com.rabbitmq.client.QueueingConsumer.Delivery;

public class Consumer {

	
	public static void main(String[] args) throws Exception {
		
		
		ConnectionFactory connectionFactory = new ConnectionFactory();
		connectionFactory.setHost("192.168.11.76");
		connectionFactory.setPort(5672);
		connectionFactory.setVirtualHost("/");
		
		Connection connection = connectionFactory.newConnection();
		Channel channel = connection.createChannel();
		
		
		String exchangeName = "test_qos_exchange";
		String queueName = "test_qos_queue";
		String routingKey = "qos.#";
		
		channel.exchangeDeclare(exchangeName, "topic", true, false, null);
		channel.queueDeclare(queueName, true, false, false, null);
		channel.queueBind(queueName, exchangeName, routingKey);
		
		//1 限流方式  第一件事就是 autoAck设置为 false
		
		channel.basicQos(0, 1, false);
		
		channel.basicConsume(queueName, false, new MyConsumer(channel));
		
		
	}
}

```


**MyConsumer**
```java
package com.bfxy.rabbitmq.api.limit;

import java.io.IOException;

import com.rabbitmq.client.AMQP;
import com.rabbitmq.client.Channel;
import com.rabbitmq.client.DefaultConsumer;
import com.rabbitmq.client.Envelope;

public class MyConsumer extends DefaultConsumer {


	private Channel channel ;
	
	public MyConsumer(Channel channel) {
		super(channel);
		this.channel = channel;
	}

	@Override
	public void handleDelivery(String consumerTag, Envelope envelope, AMQP.BasicProperties properties, byte[] body) throws IOException {
		System.err.println("-----------consume message----------");
		System.err.println("consumerTag: " + consumerTag);
		System.err.println("envelope: " + envelope);
		System.err.println("properties: " + properties);
		System.err.println("body: " + new String(body));
		
		channel.basicAck(envelope.getDeliveryTag(), false);
		
	}


}

```


### 3-10 消费端ACK与重回队列机制 

![image](https://user-images.githubusercontent.com/21000558/178394598-02b3d5a7-b1c9-44f8-b8ab-4f70e246a65e.png)

![image](https://user-images.githubusercontent.com/21000558/178394721-0a9e9fd6-a9c8-4ac2-8e30-e578881a839f.png)


### 3-11 TTL消息详解 

![image](https://user-images.githubusercontent.com/21000558/178395914-5f68477f-3472-422b-afeb-3b432cea4d49.png)


### 3-12 死信队列详解

![image](https://user-images.githubusercontent.com/21000558/178396651-83581010-ccc2-4deb-b2d3-daf0b83a346f.png)

![image](https://user-images.githubusercontent.com/21000558/178396746-1ed6c534-934a-4ec2-a790-b0c06b45b772.png)

![image](https://user-images.githubusercontent.com/21000558/178396809-c87bc853-00fe-40cf-aa3f-0a2cc82db09a.png)

![image](https://user-images.githubusercontent.com/21000558/178397098-5a9554ea-85f4-47fc-a18f-98b52ed6a649.png)

![image](https://user-images.githubusercontent.com/21000558/178397055-acf06906-bda8-480c-8bba-46972d23d29c.png)


**Producer**
```java
package com.bfxy.rabbitmq.api.dlx;

import com.rabbitmq.client.AMQP;
import com.rabbitmq.client.Channel;
import com.rabbitmq.client.Connection;
import com.rabbitmq.client.ConnectionFactory;

public class Producer {

	
	public static void main(String[] args) throws Exception {
		
		ConnectionFactory connectionFactory = new ConnectionFactory();
		connectionFactory.setHost("192.168.11.76");
		connectionFactory.setPort(5672);
		connectionFactory.setVirtualHost("/");
		
		Connection connection = connectionFactory.newConnection();
		Channel channel = connection.createChannel();
		
		String exchange = "test_dlx_exchange";
		String routingKey = "dlx.save";
		
		String msg = "Hello RabbitMQ DLX Message";
		
		for(int i =0; i<1; i ++){
			
			AMQP.BasicProperties properties = new AMQP.BasicProperties.Builder()
					.deliveryMode(2)
					.contentEncoding("UTF-8")
					.expiration("10000")
					.build();
			channel.basicPublish(exchange, routingKey, true, properties, msg.getBytes());
		}
		
	}
}

```

**Consumer**
```java
package com.bfxy.rabbitmq.api.dlx;

import java.util.HashMap;
import java.util.Map;

import com.rabbitmq.client.Channel;
import com.rabbitmq.client.Connection;
import com.rabbitmq.client.ConnectionFactory;
import com.rabbitmq.client.QueueingConsumer;
import com.rabbitmq.client.QueueingConsumer.Delivery;

public class Consumer {

	
	public static void main(String[] args) throws Exception {
		
		
		ConnectionFactory connectionFactory = new ConnectionFactory();
		connectionFactory.setHost("192.168.11.76");
		connectionFactory.setPort(5672);
		connectionFactory.setVirtualHost("/");
		
		Connection connection = connectionFactory.newConnection();
		Channel channel = connection.createChannel();
		
		// 这就是一个普通的交换机 和 队列 以及路由
		String exchangeName = "test_dlx_exchange";
		String routingKey = "dlx.#";
		String queueName = "test_dlx_queue";
		
		channel.exchangeDeclare(exchangeName, "topic", true, false, null);
		
		Map<String, Object> agruments = new HashMap<String, Object>();
		agruments.put("x-dead-letter-exchange", "dlx.exchange");
		//这个agruments属性，要设置到声明队列上
		channel.queueDeclare(queueName, true, false, false, agruments);
		channel.queueBind(queueName, exchangeName, routingKey);
		
		//要进行死信队列的声明:
		channel.exchangeDeclare("dlx.exchange", "topic", true, false, null);
		channel.queueDeclare("dlx.queue", true, false, false, null);
		channel.queueBind("dlx.queue", "dlx.exchange", "#");
		
		channel.basicConsume(queueName, true, new MyConsumer(channel));
		
		
	}
}

```

**MyConsumer**
```java
package com.bfxy.rabbitmq.api.dlx;

import java.io.IOException;

import com.rabbitmq.client.AMQP;
import com.rabbitmq.client.Channel;
import com.rabbitmq.client.DefaultConsumer;
import com.rabbitmq.client.Envelope;

public class MyConsumer extends DefaultConsumer {


	public MyConsumer(Channel channel) {
		super(channel);
	}

	@Override
	public void handleDelivery(String consumerTag, Envelope envelope, AMQP.BasicProperties properties, byte[] body) throws IOException {
		System.err.println("-----------consume message----------");
		System.err.println("consumerTag: " + consumerTag);
		System.err.println("envelope: " + envelope);
		System.err.println("properties: " + properties);
		System.err.println("body: " + new String(body));
	}


}

```







