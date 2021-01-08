**Introduction**

RabbitMQ is a message broker: it accepts and forwards messages. 

Think about it as a post office: you put a mail in post box and the postman will deliver the mail to your recipient. In this analogy, RabbitMQ is a post box, a post office and a postman.

RMQ accepts, stores and forwards binary blobs of data ‒ messages. 

RMQ jargon: 

- Producing means nothing more than sending. A program that sends messages is a producer.
- A queue is the name for a post box which lives inside RMQ. Although messages flow through RabbitMQ and your applications, they can only be stored inside a queue. producers can send messages that go to one queue, and consumers can try to receive data from one queue.
- Consuming has a similar meaning to receiving. A consumer is a program that mostly waits to receive messages.

> **The Advanced Message Queuing Protocol (AMQP)** is an open standard application layer protocol for message-oriented middleware. The defining features of AMQP are message orientation, queuing, routing (including point-to-point and publish-and-subscribe), reliability and security.

> Unlike JMS, which merely defines an API, AMQP is a wire-level protocol. A wire-level protocol is a description of the format of the data that is sent across the network as a stream of octets. Consequently any tool that can create and interpret messages that conform to this data format can interoperate with any other compliant tool irrespective of implementation language

1. AMQP is a messaging technology that do not implement the JMS API.
2. JMS is API and AMQP is a protocol.
3. JMS is only a API spec. It doesn't use any protocol. A JMS provider (like ActiveMQ) could be using any underlying protocol to realize the JMS API. For ex: Apache ActiveMQ can use any of the following protocols: AMQP, MQTT, OpenWire, REST(HTTP), RSS and Atom, Stomp, WSIF, WS Notification, XMPP. 

**Setup**

```sh
 docker run -d -p 5672:5672 -p 15672:15672 --name rabbitmq rabbitmq
 
docker ps -a
0423ceeaf80c        rabbitmq                                              "docker-entrypoint.s…"   3 minutes ago       Up 3 minutes                    4369/tcp, 0.0.0.0:5672->5672/tcp, 5671/tcp, 25672/tcp, 0.0.0.0:15672->15672/tcp   rabbitmq
```

```xml
<dependency>
    <groupId>com.rabbitmq</groupId>
    <artifactId>amqp-client</artifactId>
    <version>4.0.0</version>
</dependency>
```
```java
ConnectionFactory factory = new ConnectionFactory();
factory.setHost("localhost");
Connection connection = factory.newConnection();
Channel channel = connection.createChannel();

// optional as 5672 is the default port
factory.setPort(5672);
// optional if secured
factory.setUsername("user1");
factory.setPassword("MyPassword");
```

**Basic example**

```java
public class Publisher {

    private final static String QUEUE_NAME = "hello";
    public static void main(String[] argv) throws Exception {

        ConnectionFactory factory = new ConnectionFactory();
        factory.setHost("localhost");
        factory.setPort(5672);

        try (Connection connection = factory.newConnection(); 
             Channel channel = connection.createChannel()) 
        {
            channel.queueDeclare(QUEUE_NAME, false, false, false, null);
            String message = "Hello !";
            channel.basicPublish("", QUEUE_NAME, null, message.getBytes(StandardCharsets.UTF_8));
            System.out.println(" [x] Sent '" + message + "'");
        }
    }
}


/**		
// same without try resources
ConnectionFactory factory = new ConnectionFactory();
factory.setHost("localhost");
Connection connection = factory.newConnection();
Channel channel = connection.createChannel();

String message = "product details";
channel.queueDeclare(QUEUE_NAME, false, false, false, null);

channel.basicPublish("", QUEUE_NAME, null, message.getBytes());
System.out.println(" [x] Sent '" + message + "'");

channel.close();
connection.close();
*/
```

```java
public class Consumer {

    private final static String QUEUE_NAME = "hello";

    public static void main(String[] argv) throws Exception {

        ConnectionFactory factory = new ConnectionFactory();
        factory.setHost("localhost");
        factory.setPort(5672);

        Connection connection = factory.newConnection();
        Channel channel = connection.createChannel();

        channel.queueDeclare(QUEUE_NAME, false, false, false, null);
        System.out.println(" [*] Waiting for messages. To exit press CTRL+C");

        DeliverCallback deliverCallback = (consumerTag, delivery) -> {
            String message = new String(delivery.getBody(), "UTF-8");
            System.out.println(" [x] Received '" + message + "'");
        };

        channel.basicConsume(QUEUE_NAME, true, deliverCallback, consumerTag -> {
        });
    }
}

/**
// same
ConnectionFactory factory = new ConnectionFactory();
factory.setHost("localhost");
Connection connection = factory.newConnection();
Channel channel = connection.createChannel();

channel.queueDeclare(QUEUE_NAME, false, false, false, null);

Consumer consumer = new DefaultConsumer(channel) {
	@Override
    public void handleDelivery(String consumerTag, 
    						   Envelope envelope,
                               AMQP.BasicProperties properties, 
                               byte[] body) 
	{
    	String message = new String(body, StandardCharsets.UTF_8);
    	System.out.println(" [x] Received '" + message + "'");
    }
};
channel.basicConsume(QUEUE_NAME, true, consumer);
*/
```

<https://github.com/rabbitmq/rabbitmq-tutorials/tree/master/java>

<https://www.baeldung.com/rabbitmq>

