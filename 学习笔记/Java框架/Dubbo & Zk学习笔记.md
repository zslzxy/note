[toc]
# Dubbo & Zk学习笔记
## 官方文档地址
- Dubbo中文文档：http://dubbo.apache.org/en-us/

- Zookeeper文档：https://zookeeper.apache.org/doc/current/index.html


---



## Dubbo学习笔记
### 一、互联网项目演进架构
#### 1、单一应用架构
##### 简介：
当网站流量较小，只需要一个应用即可满足，将所有的功能部署在一起，减少项目部署成本，这个时候，ORM就变的非常重要。
#### 2、垂直应用架构
##### 简介：
当访问量的增大，单一架构无法满足需求的情况下，将应用拆分为多个互不相干的应用，以提升效率。所以加速前端开发的MVC模式非常关键。
#### 3、分布式服务架构
##### 简介：
当垂直应用越来越多，应用之间的交互不可避免，为了提高业务的复用以及整合的分布式服务框架(RPC、RESTFUL API)非常关键。
#### 4、流动计算架构
##### 简介：
当服务太多的情况下，为了避免出现问题，服务资源浪费等情况，需要增加一个调度中心来基于访问的压力实时管理集群容错，提高利用率，这个时候服务治理中心(SOA)非常关键。


### 二、Dubbo的逻辑架构
![image](https://note.youdao.com/yws/api/personal/file/BF6916785EBA43FD9C3247D6DB3A2893?method=download&shareKey=189783df89b54e6f8949c27906234963)

![image](https://note.youdao.com/yws/api/personal/file/8CCAED4DCDB24B9391A9DDC13D7AC450?method=download&shareKey=3d4ab1cf95f856ea90123bf9ce4f66ab)

**服务提供者（Provider）**：暴露服务的服务提供方，服务提供者在启动时，向注册中心注册自己提供的服务。

**服务消费者（Consumer）**: 调用远程服务的服务消费方，服务消费者在启动时，向注册中心订阅自己所需的服务，服务消费者，从提供者地址列表中，基于软负载均衡算法，选一台提供者进行调用，如果调用失败，再选另一台调用。

**注册中心（Registry）**：注册中心返回服务提供者地址列表给消费者，如果有变更，注册中心将基于长连接推送变更数据给消费者

**监控中心（Monitor）**：服务消费者和提供者，在内存中累计调用次数和调用时间，定时每分钟发送一次统计数据到监控中心

##### 调用关系说明

- 服务容器负责启动，加载，运行服务提供者。

- 服务提供者在启动时，向注册中心注册自己提供的服务。
- 服务消费者在启动时，向注册中心订阅自己所需的服务。
- 注册中心返回服务提供者地址列表给消费者，如果有变更，注册中心将基于长连接推送变更数据给消费者。
- 服务消费者，从提供者地址列表中，基于软负载均衡算法，选一台提供者进行调用，如果调用失败，再选另一台调用。
- 服务消费者和提供者，在内存中累计调用次数和调用时间，定时每分钟发送一次统计数据到监控中心。





### springboot整个dubbo的三种方式
#### 第一种：使用注解@Service与@Reference,加上application.properties
第一步：添加jar
```
<dependency>
	<groupId>com.alibaba.boot</groupId>
	<artifactId>dubbo-spring-boot-starter</artifactId>
	<version>0.1.0</version>
</dependency>
```

第二步：开启dubbo，并执行包扫描： 
```
@EnableDubbo(scanBasePackages="com.atguigu.gmall")
```

第三步：配置application.properties
```
dubbo.application.name=user-service-provider
dubbo.registry.address=127.0.0.1:2181
dubbo.registry.protocol=zookeeper

dubbo.protocol.name=dubbo
#dubbo.protocol.port=20881

dubbo.monitor.protocol=registry
#dubbo.scan.base-packages=com.atguigu.gmall
```

#### 第二种：保留xml文件的方式
第一步：添加jar
```
<dependency>
	<groupId>com.alibaba.boot</groupId>
	<artifactId>dubbo-spring-boot-starter</artifactId>
	<version>0.1.0</version>
</dependency>
```


第二步：使用注解导入xml文件---标注在启动类上
```
@ImportResource(locations="classpath:provider.xml")
```

第三步：配置xml文件
```
<!-- 1、指定当前服务/应用的名字（同样的服务名字相同，不要和别的服务同名） -->
	<dubbo:application name="boot-user-service-provider"></dubbo:application>
	
	<!-- 2、指定注册中心的位置 -->
	<!-- <dubbo:registry address="zookeeper://127.0.0.1:2181"></dubbo:registry> -->
	<dubbo:registry protocol="zookeeper" address="127.0.0.1:2181"></dubbo:registry>
	
	<!-- 3、指定通信规则（通信协议？通信端口） -->
	<dubbo:protocol name="dubbo" port="20882"></dubbo:protocol>
	
	<!-- 4、暴露服务   ref：指向服务的真正的实现对象 -->
	<dubbo:service interface="com.atguigu.gmall.service.UserService" 
		ref="userServiceImpl01" timeout="1000" version="1.0.0">
		<dubbo:method name="getUserAddressList" timeout="1000"></dubbo:method>
	</dubbo:service>
	
	<!--统一设置服务提供方的规则  -->
	<dubbo:provider timeout="1000"></dubbo:provider>
	
	
	<!-- 服务的实现 -->
	<bean id="userServiceImpl01" class="com.atguigu.gmall.service.impl.UserServiceImpl"></bean>
	
	
	<!-- 连接监控中心 -->
	<dubbo:monitor protocol="registry"></dubbo:monitor>
```

#### 第三种：使用添加Bean的方式添加dubbo的配置
第一步：添加jar
```
<dependency>
	<groupId>com.alibaba.boot</groupId>
	<artifactId>dubbo-spring-boot-starter</artifactId>
	<version>0.1.0</version>
</dependency>
```

第二步：示例代码
```
@Configuration
public class MyDubboConfig {
	
	@Bean
	public ApplicationConfig applicationConfig() {
		ApplicationConfig applicationConfig = new ApplicationConfig();
		applicationConfig.setName("boot-user-service-provider");
		return applicationConfig;
	}
	
	//<dubbo:registry protocol="zookeeper" address="127.0.0.1:2181"></dubbo:registry>
	@Bean
	public RegistryConfig registryConfig() {
		RegistryConfig registryConfig = new RegistryConfig();
		registryConfig.setProtocol("zookeeper");
		registryConfig.setAddress("127.0.0.1:2181");
		return registryConfig;
	}
	
	//<dubbo:protocol name="dubbo" port="20882"></dubbo:protocol>
	@Bean
	public ProtocolConfig protocolConfig() {
		ProtocolConfig protocolConfig = new ProtocolConfig();
		protocolConfig.setName("dubbo");
		protocolConfig.setPort(20882);
		return protocolConfig;
	}
	
	/**
	 *<dubbo:service interface="com.atguigu.gmall.service.UserService" 
		ref="userServiceImpl01" timeout="1000" version="1.0.0">
		<dubbo:method name="getUserAddressList" timeout="1000"></dubbo:method>
	</dubbo:service>
	 */
	@Bean
	public ServiceConfig<UserService> userServiceConfig(UserService userService){
		ServiceConfig<UserService> serviceConfig = new ServiceConfig<>();
		serviceConfig.setInterface(UserService.class);
		serviceConfig.setRef(userService);
		serviceConfig.setVersion("1.0.0");
		
		//配置每一个method的信息
		MethodConfig methodConfig = new MethodConfig();
		methodConfig.setName("getUserAddressList");
		methodConfig.setTimeout(1000);
		
		//将method的设置关联到service配置中
		List<MethodConfig> methods = new ArrayList<>();
		methods.add(methodConfig);
		serviceConfig.setMethods(methods);
		
		//ProviderConfig
		//MonitorConfig
		
		return serviceConfig;
	}

}

```

第三步：开启Dubbo

```
@EnableDubbo(scanBasePackages="com.atguigu.gmall")
```

第四步：使用@Service来将服务暴露出来即可。



### Dubbo面试题
#### 1、什么是Dubbo？
Dubbo是一个提供高性能、透明化的RPC远程服务调用框架，以及SOA服务治理框架。其核心部分为：  
1、远程通讯：提供基于长连接的NIO框架抽象封装，包括线程模型、序列化，以及 请求-响应 模式的信息的交换。  
2、集群容错：提供基于接口方法的透明远程调用过程，多协议的支持，提供了负载均衡、集群容错、地址路由、动态配置等集群支持。  
3、自动发现：提供注册中心服务，是消费者能够通过Dubbo注册中心来查找对应的服务提供者。

#### 2、什么是Dubbo服务治理？
大型的分布式项目中，会存在多个服务，各个服务之间的调用、依赖、负载均衡、容错等问题都是Dubbo来提供处理的方案，这就是Dubbo服务治理。

#### 3、Dubbo存在那些协议？
- Dubbo的默认的协议是 Dubbo协议。
- Dubbo有 Dubbo协议、Http协议、RMI协议、Hession协议。
- Dubbo协议是采用 “异步单一长连接” 模式，在通常情况下，服务的提供者的机器数量小于服务服务消费者的机器数量，为了避免太多消费者对提供者施压，保证服务提供者不会受到太大的压迫。  
- Dubbo协议主要适用于传输数据较小的集群。
- Dubbo协议的序列化主要是使用的 Hession 的二进制序列化。



#### 4、Dubbo的整个架构流程？
dubbo分为四大模块，分别为 生产者、消费者、注册中心(zk,redis)、监控中心。  

**流程：**  
1）生产者将服务注册到注册中心(zk)，使用zk持久节点进行存储，消费者订阅zk节点，一旦节点发生变更，zk就会通过通知传递给消费者，消费者来实现对服务的调用。    

2）服务与服务之间的调用，都会将调用的日志记录到监控中心。  


#### 5、Dubbo注册中心全部宕掉以后，Dubbo的工作方式？
Dubbo的注册中心任意一台宕机以后，都会切换到另外一台继续提供服务；服务的提供者与消费者仍能通过本地缓存通讯；当一台服务提供者宕机以后，其他服务提供者仍能正常提供服务，当所有服务提供者都宕机以后，服务消费者将无法得到服务，并无限次重连等待服务恢复。
#### 6、Dubbo实现负载均衡策略？
- Random LoadBalance---随机算法

- RoundRobin LoadBalance---轮循算法
- LeastActive LoadBalance---主要是使使慢的服务提供者收到更少的请求，避免请求过多
- ConsistentHash LoadBalance---一致性Hash，相同参数的请求总是发送给同一个提供者

#### 7、什么是RPC？
RPC(Remote Procedure Call)是指远程调用过程，是一种进程的通信方式，它是一种技术的思想，而不是规范。  
它允许程序调用另一个地址空间的资源，而不需要程序员显式的编码这个远程调用的细节。


















---
---
---


## Zookeeper学习笔记
### 一、Zookeeper的理解：
1）Zookeeper从设计模式来看，是一个基于观察者模式设计的分布式服务管理框架，他负责管理与存储关心的数据，然后接受观察者的注册。当数据状态发生变化，Zookeeper就负责通知已经在Zookeeper上注册的那些观察者做出相应的反应。

2）Zookeeper相当于注册中心，服务提供者，服务消费者都注册到Zookeeper上，Zookeeper来对注册者进行监听，当信息或者状态发生改变，Zookeeper实现监听，对其他的观察者提供这样的信息。


### 二、Zookeeper的概述
#### 1、概述：
1）Zookeeper一个领导者，多个跟随者组成集群

2）Zookeeper集群的领导者是有所有的Zookeeper机器投票产生

3）Zookeeper集群中只要有半数以上的节点存货，集群都能够正常运行(所以Zookeeper集群一般为奇数)

4）数据的原子性，数据的更新要么成功，要么失败

5）实时性，在一定时间范围内，client能够得到最新数据


#### 2、Zookeeper的数据结构
Zookeeper数据模型相当于一棵树，每个节点成为一个ZNode。每一个ZNode默认存储1M数据，么一个ZNode都是通过路径来表示其唯一标识。
![image](https://note.youdao.com/yws/api/personal/file/F0BB86DD78C546F69CD5A59A5117AA1F?method=download&shareKey=d820d605b307c2ffae81a2a0586d8e18)

#### 3、Zookeeper应用场景
提供的服务：统一命名服务、统一配置服务、统一集群管理、服务节点动态上下线、软负载均衡等
#### 4、Zookeeper的选举机制
Zookeeper的配置文件中没有master与slave。Zookeeper在工作的时候，所有的Zookeeper都参与投票，选出一个leader，其他都为salve。所以leader也是临时产生的。
#### 5、Zookeeper节点类型
##### ZNode的两种类型：
- **短暂型：** 客户端与服务端的连接断开以后，创建的节点自动删除

- **持久型：** 客户端与服务端的连接断开以后，创建的节点会保留
##### Zookeeper的四中形式的目录节点（默认使用persistent）：
- **持久化目录节点（persistent）：** 客户端与Zookeeper断开以后，节点仍然保留。

- **持久化顺序编号目录节点(persistent_sequential)：** 断开连接够保留节点，且进行顺序编号

- **临时目录节点（ephemeral）：** 客户端与Zookeeper断开连接以后，该节点被删除

- **临时顺序目录节点(ephemeral_sequential)：** 断开连接，节点删除，Zookeeper也会进行编号