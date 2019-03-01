## 一 消息基本介绍

### 1 消息中间件

​	消息（Message）使之在应用之间传送的数据，消息包含了文本数据、json数据等格式。

​	消息队列中间件（Message Queue Middleware），是指利用高效可靠的消息传递机制进行与平台无关的数据交流，并给予数据通信来进行分布式系统的集成，能够在分布式的环境下尽心进程之间的通信。

### 2 消息传递方式

#### 2.1 点对点（P2P）

>  	点对点的消息传递方式是基于队列来实现的，消息生产者发送消息到队列中，消息消费者从队列中接收待消息，队列的存在使得消息的异步传输成为了可能。

#### 2.2 发布/订阅（Pub/Sub）

> ​	发布订阅模式定义了如何向一个内容节点发布和订阅消息，这个内容节点被称为： ***主题(`Topic`)***。
>
> ​	***`Topic`***可以类比为消息传递过程中的中介，消息发布者发布消息到某一个主题中，而消息订阅者则从主题中订阅消息。主题是的消息的订阅者与消息的发布者互相之间保持独立，不需要进行接触即可保证消息的传递。

#### 2.3 中间件作用

- **解耦** ：应用程序之间通过实现消息中间件提供的统一的API，实现数据在不同的微服务之间的异步传递功能，同时能够触发对应的应用程序执行相应的操作，实现的是一种事件驱动模型。
- **冗余（存储）**：为了避免消息在数据的传递或者处理中失败的情况，将数据进行持久化的存储，直到其完全被正确处理以后再进行删除。
- **扩展性**：消息中间件解耦了应用的处理过程，所以提高消息入队和处理的效率能够更加方便。只需要新增对应的节点即可实现扩容效果。
- **削峰**：在访问量急剧增长的情况下，需要大量的资源来保证高峰的用户的体验。使用消息中间件能够节省项目的资源的投入，让中间件进行消息的分流操作，保证项目能够正常完整运行的操作。
- **顺序保证**：数据处理的顺序是非常重要的，中间件的实现方式包括了队列的功能，先进先出。
- **缓冲**：消息中间件能够作为缓冲层来帮助任务最高效率的去执行计算等操作。
- **异步通信**：消息异步通信

## 二 RabbitMQ基本介绍

### 1 基本特征

- 可靠性: RabbitMQ 使用一些机制来保证可靠性， 如持久化、传输确认及发布确认等。
- 令灵活的路由: 在消息进入队列之前，通过交换器来路由消息。对于典型的路由功能，
  RabbitMQ 己经提供了一些内置的交换器来实现。针对更复杂的路由功能，可以将多个
  交换器绑定在一起， 也可以通过插件机制来实现自己的交换器。
- 扩展性: 多个RabbitMQ 节点可以组成一个集群，也可以根据实际业务情况动态地扩展
  集群中节点。
- 令高可用性: 队列可以在集群中的机器上设置镜像，使得在部分节点出现问题的情况下队
  列仍然可用。
- 令多种协议: RabbitMQ 除了原生支持AMQP 协议，还支持STOMP ， MQTT 等多种消息
  中间件协议。
- 令多语言客户端:R甜bitMQ 几乎支持所有常用语言，比如Java、Python 、Ruby 、PHP 、
  C# 、JavaScript 等。
- 管理界面: RabbitMQ 提供了一个易用的用户界面，使得用户可以监控和管理消息、集
  群中的节点等。
- 令插件机制: RabbitMQ 提供了许多插件， 以实现从多方面进行扩展，当然也可以编写自
  己的插件。

### 2 基本命令

- **`rabbitmq-server -detached`**  后台守护进程的方式启动rabbitmq。
- **`rabbitmqctl status`**  查看rabbitmq的启动状态。
- **`rabbitmqctl cluster_status`**  查看rabbitmq的集群信息。

- **`rabbitmqctl add_user root root`**  新增用户root，密码为 root
- **`rabbitmqctl set_permissions -p / root ".*" ".*" ".*"`**  设置用户权限
- **`rabbitmqctl set_user_tags root adminstrator`**  设置root用户为管理员

###  3 基本概念

#### 3.1 基本模型

​	RabbitMQ主要是一个生产者与一个消费者模型，负责接收、存储和转发消息的操作。RabbitMQ可以理解为一种**交换机模型**。图例为：

![1551404685777](C:\Users\张世林\AppData\Roaming\Typora\typora-user-images\1551404685777.png)

#### 3.2 生产者

> ​	生产者Producer，就是投递消息的一方。生产者将消息发送到RabbitMQ中，主要是分为两个部分：**`消息体（payload）与标签（label）`**。
>
> - 消息体一般是带有结构化的数据，比如一个JSON字符串。
>
> - 标签用于描述这条消息，比如交换机的名称和一个路由键。

#### 3.3 消费者

> ​	 消费者Consumer，接收消息的一方。消费者通过连接到RabbitMQ服务器，订阅到对应的队列上，进行消息的消费操作。

#### 3.4 Broker

> ​        消息中间件服务节点Broker。一个RabbitMQ Broker可以看做是一个RabbitMQ的服务节点，或者RabbitMQ的服务实例。大多数情况下，可以将一个RabbitMQ看做是一台RabbitMQ服务器。

![1551405326403](C:\Users\张世林\AppData\Roaming\Typora\typora-user-images\1551405326403.png)

#### 3.5 队列

> - **`Queue`**队列,是RabbitMQ的内部对象，**用于存储消息**。
>
>   ![1551405659564](C:\Users\张世林\AppData\Roaming\Typora\typora-user-images\1551405659564.png)
>
> -   RabbitMQ的消息只能够存储在队列中，消费者从队列中获取消息进行消费。
>
> - 多个消费者可以订阅同一个主题，这是队列中的消息就会被平均分摊（即：**Round-Robin轮询算法**）。
>
>   ![1551405684980](C:\Users\张世林\AppData\Roaming\Typora\typora-user-images\1551405684980.png)

#### 3.6 交换器、路由键、绑定

##### 3.6.1 Exchange：交换器

> ​	生产者将消息发送到Exchange中，再由交换器Exchange将消息路由到一个或者多个队列中。如果路由无法连接到对应的队列中，则会将消息返回给生产者、或者直接丢弃消息。

**交换器的模型：**

![1551405998236](C:\Users\张世林\AppData\Roaming\Typora\typora-user-images\1551405998236.png)

**交换器类型：**

> - **`fanout`** ：将所有发送到该Exchange的消息路由到与该Exchange绑定的队列中。
> - **`direct`** ：将消息路由到那些RoutingKey与BindingKey完全匹配的队列中。
> - **`topic`**：将消息路由到BindingKey与RoutingKey相匹配的队列中。
>   - RoutingKey 以一个 “ . ” 为分割的字符串，每分割的字符串，就是对应的一个单词，例如（”com.rabbitmq.client“、“java.util.concurrent”）。
>   - BindingKey与RoutingKey一样也是 “ . ” 进行分割字符串。
>   - **BindingKey中可以存在两种特殊的字符 `*` 和 `#` ，用作模糊匹配：**
>     - `*` ：模糊匹配单个单词。例如：（“ com.*.java ” ，“ \*.com.\* ”） 。
>     - `#` ：模糊匹配多个单词，也可以是零个。
> - **`headers`** ：header类型的交换器不依赖于路由键的匹配规则来进行消息的路由，而是根据发送的消息内容中的headers属性进行匹配。在绑定队列和交换器时制定一组键值对， 当发送消息到交换器时，RabbitMQ 会获取到该消息的headers (也是一个键值对的形式) ，对比其中的键值对是否完全匹配队列和交换器绑定时指定的键值对，如果完全匹配则消息会路由到该队列，否则不会路由到该队列。headers 类型的交换器性能会很差，而且也不实用，基本上不会看到它的存在。

##### 3.6.2 RoutingKey：路由键

> ​	路由键表示的是生产者将消息发送给交换器Exchange的时候，**设置这个消息的路由规则**。而这个**RoutingKey需要与交换器类型和绑定键（BingingKey）联合使用才能生效。**

##### 3.6.3 BindingKey：绑定键

> ​	RabbitMQ中通过绑定交换器与队列关联起来，在绑定的时候通常会绑定一个绑定键（ BindingKey），这样子RabbitMQ就能够正确的将消息路由到队列中了。
>
> ​	生产者将消息发送给交换机的时候，需要一个RoutingKey；
>
> ​	消息通过Exchange发送给对应的队列时，需要BindingKey；
>
> ​	只有RoutingKey与BindingKey能够正常匹配的话，才能够正确生效。RoutingKey相当于邮寄的包裹上面的地址，BindingKey相当于是实际的收货地址，如果存在实际的收货地址，则RoutingKey才能够实现真正的效果。
>
> ​	**通常情况下，一般是将BindingKey与RoutingKey不做区分，统称：RoutingKey。**

![1551406362430](C:\Users\张世林\AppData\Roaming\Typora\typora-user-images\1551406362430.png)

#### 3.7 运行流程

##### 3.7.1 生产者发送消息

运行流程为：

> - 第一步：生产者与RabbitMQ建立一个Collection链接，开启一个信道（Channel）。
> - 第二步：生产者声明一个交换器，设置交换器的属性，比如交换器的类型、是否持久化等。
> - 第三步：生产者声明一个队列，并设置队列的相关属性，比如：是否排他、是否持久化、是否自动删除等。
> - 第四步：生产者通过路由键将交换器和队列进行绑定起来。
> - 第五步：生产者将消息发送到RabbitMQ Broker中。
> - 第六步：相应的交换器根据接收到的路由键查找相匹配的队列。
>   - 存在对应的消息队列：将消息存入到消息队列中。
>   - 不存在对应的消息队列：将消息进行回退或者直接丢弃。
> - 第七步：关闭信道，关闭连接。

代码实现为：

- 第一步：创建连接工厂

```java
public class ConnectionFactoryBean {

	public static final String IP_ADDRESS = "192.168.3.131";
	public static final String USERNAME = "root";
	public static final String PASSWORD = "root";
	public static final int PORT = 5672;

	public static ConnectionFactory getConnectionFactory() {
		com.rabbitmq.client.ConnectionFactory connectionFactory = new com.rabbitmq.client.ConnectionFactory();
		connectionFactory.setHost(IP_ADDRESS);
		connectionFactory.setPort(PORT);
		connectionFactory.setUsername(USERNAME);
		connectionFactory.setPassword(PASSWORD);
		return connectionFactory;
	}

	public static Connection getConnection() throws IOException, TimeoutException {
		Connection connection = ConnectionFactoryBean.getConnectionFactory().newConnection();
		return connection;
	}

}
```

- 第二步：生产者发送消息

```java
public class Producer {

	//设置交换机的名称
	public static final String EXCHANGE_NAME = "exchange_demo";
	public static final String ROUTING_KEY = "routingkey_demo";
	public static final String QUEUE_NAME = "queue_demo";

	public static void main(String[] args) throws IOException, TimeoutException {
		//得到对应的rabbitmq连接
		Connection connection = ConnectionFactoryBean.getConnection();

		//得到Channel
		Channel channel = connection.createChannel();
		//创建一个 type="direct"，持久化的、非自动删除的交换器
		channel.exchangeDeclare(EXCHANGE_NAME, "direct", true, false, null);
		//创建一个持久化的、非排他的、非自动删除的队列
		channel.queueDeclare(QUEUE_NAME, true, false, false, null);
		//将交换器与队列通过路由进行绑定在一起
		channel.queueBind(QUEUE_NAME, EXCHANGE_NAME, ROUTING_KEY);

		//发送一条消息
		String message = "我的第一个rabbitmq项目";
		channel.basicPublish(EXCHANGE_NAME, ROUTING_KEY, MessageProperties.PERSISTENT_TEXT_PLAIN, message.getBytes());

		//关闭对应的资源数据
		channel.close();
		connection.close();
	}

}
```

##### 3.7.2 消费者接收消息

运行流程为：

> 第一步：消费者连接到RabbitMQ Broker，建立连接Connection，开启信道Channel。
>
> 第二步：消费者向RabbitMQ Broker请求消费相关的队列的消息，设置相应的回调函数。
>
> 第三步：等待RabbitMQ Broker回应并投递相应队列中的消息，消费者接受消息。
>
> 第四步：等待消费者确认（ack）已经正确的接受并处理了消息。
>
> 第五步：RabbitMQ从队列中删除对应的已经被确认的消息。
>
> 第六步：关闭channel、关闭connection。

代码实现为：

第一步：创建连接工厂

```java
public class ConnectionFactoryBean {

	public static final String IP_ADDRESS = "192.168.3.131";
	public static final String USERNAME = "root";
	public static final String PASSWORD = "root";
	public static final int PORT = 5672;

	public static ConnectionFactory getConnectionFactory() {
		com.rabbitmq.client.ConnectionFactory connectionFactory = new com.rabbitmq.client.ConnectionFactory();
		connectionFactory.setHost(IP_ADDRESS);
		connectionFactory.setPort(PORT);
		connectionFactory.setUsername(USERNAME);
		connectionFactory.setPassword(PASSWORD);
		return connectionFactory;
	}

	public static Connection getConnection() throws IOException, TimeoutException {
		Connection connection = ConnectionFactoryBean.getConnectionFactory().newConnection();
		return connection;
	}

}
```

第二步：创建消费者进行消息的消费。

```java
public class Consumer {

	//设置交换机的名称
	public static final String EXCHANGE_NAME = "exchange_demo";
	public static final String ROUTING_KEY = "routingkey_demo";
	public static final String QUEUE_NAME = "queue_demo";
	private static final String IP_ADDRESS = "192.168.3.131";
	private static final int PORT = 5672;
	public static final String USERNAME = "root";
	public static final String PASSWORD = "root";

	public static void main(String[] args) throws IOException, TimeoutException {
		//得到对应的链接
		Address[] addresses = new Address[]{new Address(IP_ADDRESS, PORT)};

		Connection connection = ConnectionFactoryBean.getConnection();
		Channel channel = connection.createChannel();
		//设置客户端最多接收未被ack的消息的个数
		channel.basicQos(64);
		com.rabbitmq.client.Consumer consumer = new DefaultConsumer(channel) {
			@Override public void handleDelivery(String consumerTag, Envelope envelope, AMQP.BasicProperties properties,
												 byte[] body) throws IOException {
				System.out.println("recve message:" + new String(body));
				try {
					TimeUnit.SECONDS.sleep(1);
				} catch (InterruptedException e) {
					e.printStackTrace();
				}
				channel.basicAck(envelope.getDeliveryTag(), false);
			}
		};

		channel.basicConsume(QUEUE_NAME,consumer);
		try {
			TimeUnit.SECONDS.sleep(5);
		} catch (InterruptedException e) {
			e.printStackTrace();
		}
		channel.close();
		connection.close();

	}

}

```

#### 3.8  Connection

>  	生产者与消费者之间，都需要连接到RabbitMQ Broker。而建立的TCP连接就是Connection。

#### 3.9 Channel

> ​	当Connection连接起以后，需要建立起一条AMQP信道（也就是Channel），每一条指令都是通过创建的信道来完成的。**建立的TCP连接，采用的是NIO的非阻塞式的IO 多路复用模型。**

![1551409361315](C:\Users\张世林\AppData\Roaming\Typora\typora-user-images\1551409361315.png)

### 4 AMQP协议

​	