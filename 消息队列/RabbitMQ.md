## 常见面试题

#### 1、什么是rabbitmq

采用AMQP高级消息队列协议的一种消息队列技术，最大的特点就是消费并不需要确保提供方存在，实现了服务之间的高度解耦。



#### 2、为什么要使用rabbitmq

在分布式系统下具备异步、削峰、负载均衡等一系列高级功能；

拥有持久化的机制，进程消息、队列中的消息也可以保存下来；

实现消费者和生产者之间的解耦；

对于高并发场景下，利用消息队列可以使得同步访问变为串行访问达到一定量的限流，利于数据库的操作；

可以使用异步队列达到异步下单的效果，排队中，后台进行逻辑下单。



#### 3、使用rabbitmq的场景

服务间异步通信；

顺序消费；

定时任务；

流量削峰；

解耦。



#### 4、如何确保消息正确的发送至rabbitmq，如何确保消息消费方消费了消息

**发送方确认模式**

将信道设置成confirm模式(发送方确认模式)，则所有在信道上发布的消息都会被指派一个唯一的ID；

一旦消息被投递到目的队列后，或者消息被写入磁盘后(可持久化的消息)，信道会发送一个确认给生产者(包含消息唯一ID)；

如果rabbitmq发生内部错误从而导致消息丢失，会发送一条nack消息；

发送方确认模式是异步的，生产者应用程序在等待确认的同时，可以继续发送消息。当确认消息到达生产者应用程序，生产者应用程序的回调方法就会被触发来处理确认消息。



**接收方确认机制**

消费者接收每一条消息后都必须进行确认(消息接收和消息确认是两个不同的操作)。只有消费者确认了消息，rabbitmq才能安全的把消息从队列中删除；

这里并没有用到超时机制，rabbitmq仅通过cunsumer的连接中断来确认是否需要重新发送消息。也就是说，只要连接不中断，rabbitmq给了consumer足够长的时间来处理消息，保证数据的最终一致性。



**几种特殊情况**

如果消费者接收到消息，在确认之前断开了连接或取消订阅，rabbitmq会认为消息没有被分发，然后重新分发给下一个订阅的消费者(可能存在消息重复消费的隐患，需要去重)；

如果消费者接收到消息却没有确认消息，连接也未断开，则rabbitmq认为该消费者繁忙，将不会给该消费者分发更多的消息。



#### 5、rabbitmq怎样避免消息丢失

消息持久化

ack确认机制

设置集群镜像模式

消息补偿机制



#### 6、rabbitmq有哪些重要角色

生产者：消息的创建者，负责创建和推送数据到消息服务器；

消费者：消息的接收方，用于处理数据和确认消息；

代理：就是rabbitmq本身，用于扮演“快递”的角色，本身不生产消息，只是扮演“快递”的角色；



#### 7、要保证消息持久化成功的条件是什么

声明队列必须设置持久化durable为true；

消息推送投递模式必须设置持久化，deliveryMode设置为2；

消息已经到达持久化交换器；

消息已经到达持久化队列。

以上四个条件都满足才能保证消息持久化成功。



#### 8、rabbitmq持久化有什么缺点

持久化的缺点就是降低了服务器的吞吐量，因为使用的是磁盘而非内存存储，从而降低了吞吐量。可尽量使用ssd硬盘来缓解吞吐量的问题。



#### 9、消息基于什么传输

由于TCP连接创建和销毁开销较大，且并发数受系统资源限制，会造成性能瓶颈。rabbitmq使用信道的方式来传输数据，信道是建立在真实的TCP连接内的虚拟连接，且每条TCP连接上的信道数量没有限制。



#### 10、rabbitmq的缺点

**系统可用性降低**

系统引入的外部依赖越多越容易挂掉。

**系统复杂性提高**

需要考虑保证消息没有重复消费，处理消息丢失，保证消息顺序等问题。

**一致性问题**



#### 11、rabbitmq的工作模式

工作模式（work）

发布订阅模式（publish/subscribe）

路由模式（routing）

主题模式（topic）



#### 12、如何保证rabbitmq消息的顺序性

拆分多个队列，每个队列一个消费者，就是多一些队列而已，确实是麻烦点；这样也会造成吞吐量下降，可以在消费者内部采用多线程的方式去消费；

> 生产者发送n个消息到一个队列里面，这n个消息具有先后顺序，使用关键值哈希等操作，将具有先后顺序的发送到同一个队列里面，比如订单根据订单id发送到同一个queue。
>
> 消费者去不同的队列里面消费消息。

一个队列对应一个消费者，这个消费者内部用内存队列做排队，然后分发给底层不同的worker来处理。



#### 13、rabbitmq有哪些重要的组件

ConnectionFactory(连接管理器)：应用程序与rabbit之间建立连接的管理器，程序代码中使用；

Channel(信道)：消息推送使用的通道；

Exchange(交换机)：用于接受分配消息；

Queue(队列)：用于存储生产者的消息；

RoutingKey(路由键)：用于把生产者的数据分配到交换机上；

BindingKey(绑定键)：用于把交换机的消息绑定到队列上。



## RabbitMQ

#### 1.MQ引言

###### 1.1 什么是MQ

<font color='red'>MQ</font>是翻译为消息队列，通过典型的生产者和消费者模型，生产者不断向消息队列中生产消息，消费者不断从队列中获取消息。因为消息的生产者和消费者都是异步的，而且只关心消息的发送和接收，没有业务逻辑的侵入，轻松实现系统间解耦。别名为<font color='red'>消息中间件</font>，通过利用高效可靠的消息传递机制进行平台无关的数据交流，并基于数据通信来进行分布式系统的集成。

###### 1.2 MQ有哪些

当今市面上有很多主流的消息中间件，如老牌的ActiveMQ、RabbitMQ、炙手可热的Kafka，阿里巴巴自主研发的RocketMQ等。

###### 1.3 不同MQ特点

*1.ActiveMQ*

ActiveMQ是Apache出品，最流行的，能力强劲的开源消息总线。它是一个完全支持JMS规范的消息中间件。丰富的API，多种集群架构模式让ActiveMQ在业界成为老牌的消息中间件，在中小型企业颇受欢迎。

*2. Kafka*

Kafka是LinkedIn开源的分布式发布-订阅消息系统，目前归属于Apache顶级项目。Kafka主要特点是基于Pull的模式来处理消息消费。追求高吞吐量，一开始的目的就是用于日志收集和传输。0.8版本开始支持复制，不支持事务，对消息的重复、丢失、错误没有严格要求，适合产生大量数据的互联网服务的数据收集业务。

*3. RocketMQ*

RocketMQ是阿里开源的消息中间件，它是纯java开发，具有高吞吐量、高可用性、适合大规模分布式系统应用的特点。RocketMQ思路起源于Kafka，但并不是Kafka的一个复制，它对消息的可靠传输及事务性做了优化，目前在阿里集团被广泛用于交易、充值、流计算、消息推送、日志流式处理、binglog分发等场景。

*4. RabbitMQ*

RabbitMQ是使用Erlang语言开发的开源消息队列系统，基于AMQP协议来实现。AMQP的主要特征是面向消息、队列、路由（包括点对点和发布/订阅）、可靠性、安全。AMQP协议更多用在企业系统内对数据一致性、稳定性和可靠性要求很高的场景，对性能和吞吐量的要求还在其次。

#### 2.实战 hello world

###### 2.1 Windows下载安装

###### 2.2 Hello World

*发送端操作流程*

创建连接===创建通道===声明队列===发送消息

```java
package com.southwind.test.rabbitmq;

import com.rabbitmq.client.Channel;
import com.rabbitmq.client.Connection;
import com.rabbitmq.client.ConnectionFactory;

import java.io.IOException;
import java.util.concurrent.TimeoutException;

/**
 * rabbitmq的入门程序
 */
public class Producer01 {

    // 队列
    private static final String QUEUE = "hello world";
    public static void main(String[] args) {

        // 通过连接工厂和mq创建连接
        ConnectionFactory connectionFactory = new ConnectionFactory();
        connectionFactory.setHost("127.0.0.1");
        connectionFactory.setPort(5672);
        connectionFactory.setUsername("guest");
        connectionFactory.setPassword("guest");
        // 设置虚拟机，一个mq服务可以设置多个虚拟机，每个虚拟机就相当于一个mq
        connectionFactory.setVirtualHost("/");
        // 建立新连接
        Connection connection = null;
        try {
            connection = connectionFactory.newConnection();
            // 创建会话通道,生产者和mq服务所有通信都在channel
            Channel channel = connection.createChannel();
            // 声明队列,如果队列在mq中没有则要创建
            // 参数：String queue, boolean durable, boolean exclusive, boolean autoDelete, Map<String, Object> arguments
            /**
             * 1. 队列名称
             * 2. durable是否持久化，如果持久化，mq服务重启后队列还在
             * 3. exclusive是否独占连接，队列只允许在该连接中访问，如果连接关闭队列自动删除,如果将此参数设为true可以用于临时队列的创建
             * 4. autoDelete自动删除，队列不再使用时自动删除此队列，如果将此参数和exclusive参数设置为true就可以实现临时队列
             * 5. arguments参数，可以设置一个队列的扩展参数，存活时间等
             */
            channel.queueDeclare(QUEUE, true, false, false, null);
            // 发送消息
            // 参数：String exchange, String routingKey, BasicProperties props, byte[] body
            /**
             * 1. exchange，交换机，如果不指定使用mq默认的交换机,设置为“”
             * 2. routingKey， 路由key来将消息发送到指定的队列,如果使用默认交换机，routingKey设置为队列的名称
             * 3. props，消息的属性
             * 4. body, 消息内容
             */
            String message = "hello world 黑马程序员";
            channel.basicPublish("", QUEUE, null, message.getBytes());
            System.out.println("send to mq" + message);

        } catch (Exception e) {
            e.printStackTrace();
        } finally {

        }
    }
}
```

*接收端操作流程*

创建连接===创建通道===声明队列===监听队列===接收消息===ack回复

```java
package com.southwind.test.rabbitmq;

import com.rabbitmq.client.*;

import java.io.IOException;
import java.util.concurrent.TimeoutException;

/**
 * 入门程序消费者
 */
public class consumer01 {

    private static final String QUEUE = "hello world";

    public static void main(String[] args) throws IOException, TimeoutException {
        // 通过连接工厂和mq创建连接
        ConnectionFactory connectionFactory = new ConnectionFactory();
        connectionFactory.setHost("127.0.0.1");
        connectionFactory.setPort(5672);
        connectionFactory.setUsername("guest");
        connectionFactory.setPassword("guest");
        // 设置虚拟机，一个mq服务可以设置多个虚拟机，每个虚拟机就相当于一个mq
        connectionFactory.setVirtualHost("/");
        // 建立新连接
        Connection connection = connectionFactory.newConnection();
        // 创建会话通道
        Channel channel = connection.createChannel();

        // 监听队列
        /**
         * 参数明细
         * String queue, boolean autoAck, Consumer callback
         * 1. 队列名称
         * 2. 自动回复,当消费者接收到消息后要告诉mq消息已接收，如果将此参数设为true表示会自动回复，否则需要编程实现回复
         * 3. 消费方法，当消费者接收到消息后要执行的方法
         */
        channel.queueDeclare(QUEUE, true, false, false, null);

        // 实现消费方法
        DefaultConsumer defaultConsumer = new DefaultConsumer(channel){
            // 接收到消息后此方法将被调用

            /**
             *
             * @param consumerTag 消费者标签，标识消费者，在监听队列时设置
             * @param envelope 信封
             * @param properties 属性
             * @param body  消息内容
             * @throws IOException
             */
            @Override
            public void handleDelivery(String consumerTag, Envelope envelope, AMQP.BasicProperties properties, byte[] body) throws IOException {
                String exchange = envelope.getExchange();
                long deliveryTag = envelope.getDeliveryTag();

                String message = new String(body, "utf-8");
                System.out.println("receive message: " + message);
            }
        };
        channel.basicConsume(QUEUE, true, defaultConsumer);
    }
}
```

#### 3.工作模式

###### 3.1 工作队列模式

![image-20200912222555260](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200912222555260.png)

工作队列与入门程序相比，多了一个消费端，两个消费端共同消费同一个队列中的消息，它的特点如下：

一个生产者将消息发给一个队列

多个消费者共同监听一个队列的消息

消息不能被重复消费

rabbit采用轮询的方式将消息平均发送给消费者



###### 3.2 发布-订阅模式

![image-20200912223401068](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200912223401068.png)

1.一个生产者将消息发送给交换机

2.与交换机绑定的有多个队列，每个消费者监听自己的队列

3.生产者将消息发给交换机，由交换机将消息转发给绑定此交换机的每个队列，每个绑定交换机的序列都将接收到消息

4.如果消息发给没有绑定队列的交换机上消息将丢失



###### 3.3 Routing路由模式

1.一个交换机绑定多个队列，每个队列设置routingkey，并且一个队列可以设置多个routingkey

2.每个消费者监听自己的队列

3.生产者将消息发给交换机，发送消息时需要指定routingkey的值，交换机来判断该routingkey的值和哪个队列的routingkey相等，如果相等则将消息转发给该队列



###### 3.4 Topics通配符工作模式

1.一个交换机可以绑定多个队列，每个队列可以设置一个或多个带通配符的routingKey

2.生产者将消息发给交换机，交换机根据routingKey的值来匹配队列，匹配时采用通配符方式，匹配成功的将消息转发到指定的队列



###### 3.5 Header模式

header模式与routing不同的地方在于，header模式取消routingKey，使用header中的键值对匹配队列。



###### 4.6 RPC

RPC即客户端远程调用服务端的方法，使用MQ可以实现RPC的异步调用，基于Direct交换机实现，流程如下：

1.客户端既是生产者又是消费者，向RPC请求队列发送RPC调用消息，同时监听RPC响应队列

2.服务端监听RPC请求队列的消息，收到消息后执行服务端的方法，得到方法返回的结果

3.服务端将RPC方法的结果发送到RPC响应队列

#### 4.Spring整合RabbitMQ

###### 4.1 搭建springboot环境

选择基于Spring-rabbit去操作RabbitMQ

使用spring-boot-starter-amqp会自动添加spring-rabbit依赖，如下：

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-amqp</artifactId>
    <version>2.3.3.RELEASE</version>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-test</artifactId>
    <version>2.3.0.RELEASE</version>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-logging</artifactId>
    <version>1.5.9.RELEASE</version>
</dependency>
```

###### 4.2 配置

1、配置application.yml

```yaml
server:
  port: 44000
spring:
  application:
    name: test-rabbitmq-producer
  rabbitmq:
    host: 127.0.0.1
    port: 5672
    username: guest
    password: guest
    virtual-host: /
```

2、配置config.java

```java
package rabbitmq.config;

import org.springframework.context.annotation.Configuration;

@Configuration
public class RabbitmqConfig {

    //声明交换机

    //声明队列

    //绑定交换机和队列

}
```

3、logback-spring.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<configuration>
    <!--包含Spring boot对logback日志的默认配置-->
    <include resource="org/springframework/boot/logging/logback/defaults.xml" />
    <property name="LOG_FILE" value="${LOG_FILE:-${LOG_PATH:-${LOG_TEMP:-${java.io.tmpdir:-/tmp}}}/spring.log}"/>
    <include resource="org/springframework/boot/logging/logback/console-appender.xml" />

    <!--重写了Spring Boot框架 org/springframework/boot/logging/logback/file-appender.xml 配置-->
    <appender name="TIME_FILE"
              class="ch.qos.logback.core.rolling.RollingFileAppender">
        <encoder>
            <pattern>${FILE_LOG_PATTERN}</pattern>
        </encoder>
        <file>${LOG_FILE}</file>
        <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
            <fileNamePattern>${LOG_FILE}.%d{yyyy-MM-dd}.%i</fileNamePattern>
            <!--保留历史日志一个月的时间-->
            <maxHistory>30</maxHistory>
            <!--
            Spring Boot默认情况下，日志文件10M时，会切分日志文件,这样设置日志文件会在100M时切分日志
            -->
            <timeBasedFileNamingAndTriggeringPolicy class="ch.qos.logback.core.rolling.SizeAndTimeBasedFNATP">
                <maxFileSize>10MB</maxFileSize>
            </timeBasedFileNamingAndTriggeringPolicy>

        </rollingPolicy>
    </appender>

    <root level="INFO">
        <appender-ref ref="CONSOLE" />
        <appender-ref ref="TIME_FILE" />
    </root>

</configuration>
        <!--
            1、继承Spring boot logback设置（可以在appliaction.yml或者application.properties设置logging.*属性）
            2、重写了默认配置，设置日志文件大小在100MB时，按日期切分日志，切分后目录：
                blog.2017-08-01.0   80MB
                blog.2017-08-01.1   10MB
                blog.2017-08-02.0   56MB
                blog.2017-08-03.0   53MB
                ......
        -->
```



###### 4.3 生产端

###### 4.4 消费端

https://github.com/strawqqhat/rabbitMq-practice