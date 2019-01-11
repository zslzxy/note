[toc]
# SpringCloud笔记


## 一、SpringCloud的各类资料

SpringCloud中文API参考使用文档1：https://springcloud.cc/spring-cloud-dalston.html

SpringCloud中午API参考使用文档2：https://springcloud.cc/spring-cloud-netflix.html

SpringCloud中国社区网：http://springcloud.cn/

SpirngCloud中文网：https://springcloud.cc/

SpringCloud的GitHub地址：https://github.com/spring-cloud   
## 二、SpringCloud常见问题
### 1、SpringCloud是什么？
SpringCloud是分布式一站式的解决方案。  

SpringCloud是微服务技术的一种落地的体现和实现。


### 2、SpringCloud和SpringBoot的区别和关系？

1.SpringBoot专注于快速方便的开发单个个体微服务。是微服务的微观概念，主要体现的是一个个的微服务。

2.SpringCloud是关注全局的微服务协调整理治理框架以及一整套的落地解决方案，它将SpringBoot开发的一个个单体微服务整合并管理起来，为各个微服务之间提供：配置管理，服务发现，断路器，路由，微代理，事件总线等的集成服务。是微服务的宏观概念，主要指的是一个个的微服务。

3.SpringBoot可以离开SpringCloud独立使用，但是SpringCloud离不开SpringBoot，属于依赖的关系。

4.SpringBoot专注于快速，方便的开发单个微服务个体，SpringCloud关注全局的服务治理框架。

### 3、什么是微服务？
理解：微服务也是一种服务架构，他提倡的是将单一应用划分为多个小的服务，运行在各自独立的进程中，服务与服务之间桶过通信技术（RPC、RESTful API）来进行通信。  

主要的优势在于将专一的事情交给专一的一个项目区完成，降低耦合。


### 4、什么是分布式？
理解：不同的服务器上部署不同的服务工程模块，而他们之间通过RPC等通信技术来进行通信与调用。  

官方说法：分布式系统就是多态计算机和通信软件组件通过计算机网络连接起来。分布式系统是建立在网络之上的软件系统，能够实现高度内聚与透明性。分布式系统主要用于局域网、广域网以及PC端上等。

### 5、什么是集群？
不同的多态服务器上部署相同的服务模块，通过分布式部署调度软件来进行统一的调度，对外提供服务。

### 6、微服务之间的通信的方式？
主要是使用 API Gateway 方式进行通信。

### 7、SpringCloud与Dubbo之间的差别？
##### dubbo：https://github.com/dubbo  

- Zookeeper 服务注册中心  
- RPC 请求方式  
- Dubbo-monitor  服务监控中心  

##### springCloud： https://github.com/spring-cloud     
- Erueka 服务注册中心，  
- Rest API 请求方式  
- Spring Boot Admin 服务监控中心  
- Spring Cloud Netflix Hystrix  断路器（相当于保险丝）  
- Spring Cloud Netflix Zuul 服务网关  
- Spring Cloud Config 分布式配置（主要是使用Git）  
- Spring Cloud Sleuth 服务跟踪  
- Spring Cloud Bus 消息总线  
- Spring Cloud Stream 数据流  
- Spring Cloud Task 批量任务  

##### 其他主要的不同之处：
- SpringCloud主要目标是微服务架构下的一站式解决方案
- Dubbo主要是专注于RPC框架

##### 总结：
1）SpringCloud功能比Dubbo更加强大，涵盖面更广，并且作为Spring的拳头项目，它能够与Spring Framework，SpringBoot，Spring Data等其他Spring项目完美整合，这些对于微服务而言是至关重要的。   

2）而使用Dubbo构建的微服务架构就像组装电脑，各环节我们的选择自由度很高，但是最终很可能因为一条内存质量不行就点不亮了，而SpringCloud就像品牌机，在Spring Source的整合下，做了大量的兼容性测试，保证了机器拥有更高的稳定性，但是如果要在使用非原装组件外的东西，就需要对其基础有足够的了解。

3）那么最大的区别在于：SpringCloud抛弃了Dubbo的RPC通信，采用的是基于HTTP的REST方式。

### 8.什么是服务熔断？什么是服务降级？
##### 服务熔断：
在Hystrix中，当请求后端服务失败数量超过一定的比例，断路器就会将这条线断开，所有的请求会直接失败二不会发送给后端服务，避免造成服务雪崩。当断开一段时间以后，自动切换为半断开阶段，发送请求，如果请求成功，则重新连接服务，如果请求失败，则继续断开该服务。
##### 服务降级：
springcloud提供fallback方法来实现服务降级，当我们请求的服务发生故障，会自动调用这个故障方法所对应的fallback方法来进行返回。fallback方法的返回值一般是设置的默认值或者来自缓存.告知后面的请求服务不可用了，不要再来了。


### 9、微服务的优缺点？
##### 微服务的优点：
- 每个服务内聚，聚焦的是指定的业务的功能或需求。
- 开发效率高。
- 专注于松耦合
- 可以使用不同的开发语言
- 便于融入新技术，便于维护
- 微服务你是业务逻辑的代码，不会和HTML、css等混合。
- 数据的独立性，数据的共享性能够灵活的控制。
##### 微服务的缺点：
- 维护困难。

### 10.微服务的技术站有哪些？
微服务技术栈主要有：服务注册、服务发现、服务熔断、负载均衡、服务开发、服务调用、服务治理、消息队列、服务监控、服务路由、


### 11.Erueka与Zookeeper的区别？
##### Eureka： 
**优点：**  
eureka主要采取的是CAP原则的AP，高可用与分区容错性。   
eureka的工作模式就好比是我的注册者可以注册在多个相互关联的eureka服务器上，如果某		  一个eureka服务端挂掉，其他的服务端也能够正常的为注册者提供服务。   
**缺点：**
eureka没使用CAP的C原则，数据的获取可能不是最新的。

##### Zookeeper：
**优点：**
Zookeeper主要采取的是CAP原则的CP原则，数据的更新快，主要体现在强一致性。  
**缺点：**
Zookeeper的集群模式，如果主Zookeeper坏掉，其他的备用机需要执行投票决定安排来执行服务。中间这段时间不能够为注册者提供服务。   

##### 总结：
eureka可以很好的应对各种故障，不会因故障而停止服务。Zookeeper却会因为故障导致整个服务瘫痪。


### 12.Ribbon，Feign，Nginx的区别？
 Ribbon和Feign都是用于调用其他服务的，不过方式不同。
 
1.启动类使用的注解不同，Ribbon用的是@RibbonClient，Feign用的是@EnableFeignClients。

2.服务的指定位置不同，Ribbon是在@RibbonClient注解上声明，Feign则是在定义抽象方法的接口中使用@FeignClient声明。

3.调用方式不同，Ribbon需要自己构建http请求，模拟http请求然后使用RestTemplate发送给其他服务，步骤相当繁琐。

4.Feign则是在Ribbon的基础上进行了一次改进，采用接口的方式，将需要调用的其他服务的方法定义成抽象方法即可，
不需要自己构建http请求。不过要注意的是抽象方法的注解、方法签名要和提供服务的方法完全一致。






## Rest微服务项目实战：

### 1.创建整体父工程

1.在做案例之前，先在eclipse/IDEA里面创建一个maven的整体父工程项目名为：microservicecloud

**备注：在创建整体父工程的时候Packaging要选择pom而不是选择jar**

2.将需要用到的mavenjar包配置好


```
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>

	<groupId>com.hhf.springcloud</groupId>
	<artifactId>microservicecloud</artifactId>
	<version>0.0.1-SNAPSHOT</version>
	<packaging>pom</packaging>


	<properties>
		<project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
		<maven.compiler.source>1.8</maven.compiler.source>
		<maven.compiler.target>1.8</maven.compiler.target>
		<junit.version>4.12</junit.version>
		<log4j.version>1.2.17</log4j.version>
		<lombok.version>1.16.18</lombok.version>
	</properties>

	<dependencyManagement>
		<dependencies>
			<dependency>
				<groupId>org.springframework.cloud</groupId>
				<artifactId>spring-cloud-dependencies</artifactId>
				<version>Dalston.SR1</version>
				<type>pom</type>
				<scope>import</scope>
			</dependency>
			<dependency>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-dependencies</artifactId>
				<version>1.5.9.RELEASE</version>
				<type>pom</type>
				<scope>import</scope>
			</dependency>
			<dependency>
				<groupId>mysql</groupId>
				<artifactId>mysql-connector-java</artifactId>
				<version>5.0.4</version>
			</dependency>
			<dependency>
				<groupId>com.alibaba</groupId>
				<artifactId>druid</artifactId>
				<version>1.0.31</version>
			</dependency>
			<dependency>
				<groupId>org.mybatis.spring.boot</groupId>
				<artifactId>mybatis-spring-boot-starter</artifactId>
				<version>1.3.0</version>
			</dependency>
			<dependency>
				<groupId>ch.qos.logback</groupId>
				<artifactId>logback-core</artifactId>
				<version>1.2.3</version>
			</dependency>
			<dependency>
				<groupId>junit</groupId>
				<artifactId>junit</artifactId>
				<version>${junit.version}</version>
				<scope>test</scope>
			</dependency>
			<dependency>
				<groupId>log4j</groupId>
				<artifactId>log4j</artifactId>
				<version>${log4j.version}</version>
			</dependency>
		</dependencies>
	</dependencyManagement>

	<build>
		<finalName>microservicecloud</finalName>
		<resources>
			<resource>
				<directory>src/main/resources</directory>
				<filtering>true</filtering>
			</resource>
		</resources>
		<plugins>
			<plugin>
				<groupId>org.apache.maven.plugins</groupId>
				<artifactId>maven-resources-plugin</artifactId>
				<configuration>
					<delimiters>
						<delimit>$</delimit>
					</delimiters>
				</configuration>
			</plugin>
		</plugins>
	</build>
</project>
```





### 2.创建第一个子工程（公共子模块）

3.创建好父工程并且将相应的maven配置好以后，开始创建第一个子工程的项目。

在已经创建好的父工程下创建公共子模块工程名为：microservicecloud-api（maven Module）

**备注：创建子工程时Packaging要选择jar的打包方式。**


```
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>

  	<!-- 子类里面显示声明才能有明确的继承表现，无意外就是父类的默认版本否则自己定义 -->
	<parent>
		<groupId>com.hhf.springcloud</groupId>
		<artifactId>microservicecloud</artifactId>
		<version>0.0.1-SNAPSHOT</version>
	</parent>
	<!-- 当前Module我自己叫什么名字 -->
	<artifactId>microservicecloud-api</artifactId>

  <!-- 当前Module需要用到的jar包，按自己需求添加，如果父类已经包含了，可以不用写版本号 -->
	<dependencies>
		<dependency>
			<groupId>org.projectlombok</groupId>
			<artifactId>lombok</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework.cloud</groupId>
			<artifactId>spring-cloud-starter-feign</artifactId>
		</dependency>
	</dependencies>
</project>
```

4.然后在microservicecloud-api这个子项目的src/main/java下创建一个aip公共模块名为Dept的类，并且在这个类里面添加以下代码。

```
//@AllArgsConstructor //全参构造注解
@Data	//set设置值/get获取值注解
@NoArgsConstructor //无参数构造注解
@Accessors(chain=true) //链式访问
public class Dept {
	//主键
	private Long deptno; 
	//部门名称
	private String 	dname; 
	//来自那个数据库，因为微服务架构可以一个服务对应一个数据库，同一个信息被存储到不同数据库。
	private String 	db_source;
	
	public Dept(String dname)
	{
		super();
		this.dname = dname;
	}
```



5.编写完以上代码以后，在microservicecloud-api这个子项目右键点击Run As里面列表下的maven clean和maven install，让我们的工程重新在本地库生成最新的jar包使其给其他模块引用，达到通用之目的。



### 3.创建第二个子工程（部门微服务提供者）

6.我们在父项目microservicecloud中再创建一个maven的module名为：microservicecloud-provider-dept-8001

1.这个microservicecloud-provider-dept-8001用来做部门服务提供者

2.在microservicecloud-provider-dept-8001这个项目的pom文件中加入以下jar包：

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>

	<parent>
		<groupId>com.hhf.springcloud</groupId>
		<artifactId>microservicecloud</artifactId>
		<version>0.0.1-SNAPSHOT</version>
	</parent>

	<artifactId>microservicecloud-provider-dept-8001</artifactId>

	<dependencies>
		<!-- 引入自己定义的api通用包，可以使用Dept部门Entity -->
		<dependency>
			<groupId>com.hhf.springcloud</groupId>
			<artifactId>microservicecloud-api</artifactId>
			<version>${project.version}</version>
		</dependency>
		<!-- actuator监控信息完善 -->
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-actuator</artifactId>
		</dependency>
		<!-- 将微服务provider侧注册进eureka -->
		<dependency>
			<groupId>org.springframework.cloud</groupId>
			<artifactId>spring-cloud-starter-eureka</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework.cloud</groupId>
			<artifactId>spring-cloud-starter-config</artifactId>
		</dependency>
		<dependency>
			<groupId>junit</groupId>
			<artifactId>junit</artifactId>
		</dependency>
		<dependency>
			<groupId>mysql</groupId>
			<artifactId>mysql-connector-java</artifactId>
		</dependency>
		<dependency>
			<groupId>com.alibaba</groupId>
			<artifactId>druid</artifactId>
		</dependency>
		<dependency>
			<groupId>ch.qos.logback</groupId>
			<artifactId>logback-core</artifactId>
		</dependency>
		<dependency>
			<groupId>org.mybatis.spring.boot</groupId>
			<artifactId>mybatis-spring-boot-starter</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-jetty</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-test</artifactId>
		</dependency>
		<!-- 修改后立即生效，热部署 -->
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>springloaded</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-devtools</artifactId>
		</dependency>
	</dependencies>
</project>
```

6.然后在microservicecloud-provider-dept-8001这个项目的resources文件下创建一个application.yml文件

将如下代码添加到application.yml中

```yaml
server:
  port: 8001
  
mybatis:
  config-location: classpath:mybatis/mybatis.cfg.xml        # mybatis配置文件所在路径
  type-aliases-package: com.hhf.springcloud.entities    # 所有Entity别名类所在包
  mapper-locations:
  - classpath:mybatis/mapper/**/*.xml                       # mapper映射文件
    
spring:
   application:
    name: microservicecloud-dept 
   datasource:
    type: com.alibaba.druid.pool.DruidDataSource            # 当前数据源操作类型
    driver-class-name: org.gjt.mm.mysql.Driver              # mysql驱动包
    url: jdbc:mysql://localhost:3306/cloudDB01              # 数据库名称
    username: root                                                
    password: 520hhf
    dbcp2:
      min-idle: 5                                           # 数据库连接池的最小维持连接数
      initial-size: 5                                       # 初始化连接数
      max-total: 5                                          # 最大连接数
      max-wait-millis: 200                                  # 等待连接获取的最大超时时间
```

7.然后在resources文件夹里再创建一个mybatis文件夹，在mybatis文件夹里面创建一个mybatis.cfg.xml文件。

8.在mysql中创建部门数据库脚本

​		备注：得提前在mysql中创建好cloudDB01这个库

```sql
DROP DATABASE IF EXISTS cloudDB01;
CREATE DATABASE cloudDB01 CHARACTER SET UTF8;
USE cloudDB01;

CREATE TABLE dept
(
  deptno BIGINT NOT NULL PRIMARY KEY AUTO_INCREMENT,
  dname VARCHAR(60),
  db_source VARCHAR(60)
 );
 
INSERT INTO dept(dname,db_source) VALUES('开发部',DATABASE());
INSERT INTO dept(dname,db_source) VALUES('人事部',DATABASE());
INSERT INTO dept(dname,db_source) VALUES('财务部',DATABASE());
INSERT INTO dept(dname,db_source) VALUES('市场部',DATABASE());
INSERT INTO dept(dname,db_source) VALUES('运维部',DATABASE());
```

9.然后在microservicecloud-provider-dept-8001这个项目下的dao包下创建一个dao接口名为：DeptDao

```java
import com.hhf.springcloud.entities.Dept;
import org.apache.ibatis.annotations.Mapper;

import java.util.List;
//整合mybatis
@Mapper
public interface DeptDao {

    public boolean addDept(Dept dept);

    public Dept findById(Long id);

    public List<Dept> findAll();
}

```

10.然后在从resources文件夹里再mybatis文件夹里面再创建一个mapper文件夹，并在mapper文件夹下创建DeptMapper.xml这个文件

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="com.hhf.springcloud.dao.DeptDao">

	<select id="findById" resultType="Dept" parameterType="Long">
		select deptno,dname,db_source from dept where deptno=#{deptno};
	</select>
	<select id="findAll" resultType="Dept">
		select deptno,dname,db_source from dept;
	</select>
	<insert id="addDept" parameterType="Dept">
		INSERT INTO dept(dname,db_source) VALUES(#{dname},DATABASE());
	</insert>
</mapper>
```

11.然后在service包下创建DeptService部门服务接口类

```java
package com.hhf.springcloud.service;

import com.hhf.springcloud.entities.Dept;

import java.util.List;

public interface DeptService {
    public boolean add(Dept dept);
    public Dept  get(Long id);
    public List<Dept> list();
}
```

12.当然，有了创建DeptService部门服务接口，肯定也要有它的实现类，那么在service包下在创建一个包名为：impl，在这个impl包下创建DeptServiceimpl实现类

```java
package com.hhf.springcloud.service.impl;

import com.hhf.springcloud.dao.DeptDao;
import com.hhf.springcloud.entities.Dept;
import com.hhf.springcloud.service.DeptService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import java.util.List;

@Service
public class DeptServiceimpl implements DeptService {
    @Autowired
    private DeptDao dao;

    @Override
    public boolean add(Dept dept)
    {
        return dao.addDept(dept);
    }

    @Override
    public Dept get(Long id)
    {
        return dao.findById(id);
    }

    @Override
    public List<Dept> list()
    {
        return dao.findAll();
    }

}
```

13.好，接下来我们在controller包下创建一个DeptController的类来实现页面数据显示

```java
package com.hhf.springcloud.controller;

import com.hhf.springcloud.entities.Dept;
import com.hhf.springcloud.service.DeptService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.*;

import java.util.List;

@RestController
public class DeptController {
    @Autowired
    private DeptService service;

    @RequestMapping(value="/dept/add",method=RequestMethod.POST)
    public boolean add(@RequestBody Dept dept)
    {
        return service.add(dept);
    }

    @RequestMapping(value="/dept/get/{id}",method=RequestMethod.GET)
    public Dept get(@PathVariable("id") Long id)
    {
        return service.get(id);
    }

    @RequestMapping(value="/dept/list",method=RequestMethod.GET)
    public List<Dept> list()
    {
        return service.list();
    }
}
```

14.接下来我们进入一个测试阶段，看看能不能把数据显示到页面中

在主包下创建一个名为：DeptProvider8001_App的启动类进行测试

```java
package com.hhf.springcloud;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class DeptProvider8001_App {
    public static void main(String[] args) {
        SpringApplication.run(DeptProvider8001_App.class,args);
    }
}
```

15.好，激动人心的时刻到来了！如果无误，那么我们从数据库读取的数据能够全部在页面中进行一个显示。

在浏览器输入：http://localhost:8001/dept/list    进行页面测试！


## 4.创建第三个子工程（部门微服务消费者）

1.在整体父工程下再创建一个子工程，名为：microservicecloud-consumer-dept-80

2.在microservicecloud-consumer-dept-80子工程pom文件下添加jar包

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <parent>
        <groupId>com.hhf.springcloud</groupId>
        <artifactId>microservicecloud</artifactId>
        <version>0.0.1-SNAPSHOT</version>
    </parent>

    <artifactId>microservicecloud-consumer-dept-80</artifactId>
    <description>部门微服务消费者</description>

    <dependencies>
        <dependency>
          <!-- 自己定义的api -->
            <groupId>com.hhf.springcloud</groupId>
            <artifactId>microservicecloud-api</artifactId>
            <version>${project.version}</version>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <!-- 修改后立即生效，热部署 -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>springloaded</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
        </dependency>
    </dependencies>
</project>
```

3.然后在resources资源文件下创建一个application.yml文件，并添加如下内容：

```yaml
server:
  port: 80
```

4.然后在java包下创建一个com.hhf.springcloud.cfgbeans包，并在此包创建一个ConfigBean类

```java
package com.hhf.springcloud.cfgbeans;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.client.RestTemplate;

@Configuration
public class ConfigBean {
	/**
     * RestTemplate提供了多种便捷访问远程Http服务的方法，
     * 是一种简单便捷的访问restful服务模板类，是Spring提供的用于访问Rest服务的客户端模板工具集。
     */
  
    @Bean
    public RestTemplate getRestTemplate(){
            return new RestTemplate();
    }

}
```

5.并在com.hhf.springcloud包下创建一个controller包，在controller包下创建一个名为：DeptController_Consumer的类

```java
package com.hhf.springcloud.controller;

import com.hhf.springcloud.entities.Dept;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.client.RestTemplate;

import java.util.List;

@RestController
public class DeptController_Consumer {

    private static final String REST_URL_PREFIX = "http://localhost:8001";

    @Autowired
    private RestTemplate restTemplate;

    @RequestMapping(value="/consumer/dept/add")
    public boolean add(Dept dept)
    {
        return restTemplate.postForObject(REST_URL_PREFIX+"/dept/add", dept, Boolean.class);
    }

    @RequestMapping(value="/consumer/dept/get/{id}")
    public Dept get(@PathVariable("id") Long id)
    {
        return restTemplate.getForObject(REST_URL_PREFIX+"/dept/get/"+id, Dept.class);
    }

    @SuppressWarnings("unchecked")
    @RequestMapping(value="/consumer/dept/list")
    public List<Dept> list()
    {
        return restTemplate.getForObject(REST_URL_PREFIX+"/dept/list", List.class);
    }
}
```

6.创建一个主启动类名为：DeptConsumer80_App

```java
package com.hhf.springcloud;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class DeptConsumer80_App {
    public static void main(String[] args) {
        SpringApplication.run(DeptConsumer80_App.class, args);
    }
}
```

7.激动人心的时候又到啦！！！

​	我们先启动microservicecloud-provider-dept-8001这个子工程下的主启动类：DeptProvider8001_App。

8.启动了DeptProvider8001_App成功以后，我们再启动microservicecloud-consumer-dept-80这个子工程下的主启动类：DeptConsumer80_App

9.然后在浏览器中进行测试即可。

# SpringCloud微服务架构之Eureka

**最近听到说SpringCloud的Eureka2.0要闭源了，那么即使闭源也没事，我相信Spring那么多强大，肯定会有其他的来代替的，所以不用担心，而且我们只要掌握一个，那么其他基本上原理都是一样的。**

# Eureka是什么？

#### Netflix在设计Eureka时遵守的就是AP原则

Spring Cloud 封装了 Netflix 公司开发的 Eureka 模块来实现服务注册和发现

Eureka 采用了 C-S 的设计架构。Eureka Server 作为服务注册功能的服务器，它是服务注册中心。

而系统中的其他微服务，使用 Eureka 的客户端连接到 Eureka Server并维持心跳连接。这样系统的维护人员就可以通过 Eureka Server 来监控系统中各个微服务是否正常运行。SpringCloud 的一些其他模块（比如Zuul）就可以通过 Eureka Server 来发现系统中的其他微服务，并执行相关的逻辑。

#### Eureka包含两个组件：
Eureka Server和Eureka Client  

#### Eureka Server提供服务注册服务：
各个节点启动后，会在EurekaServer中进行注册，这样EurekaServer中的服务注册表中将会存储所有可用服务节点的信息，服务节点的信息可以在界面中直观的看到

#### EurekaClient客户端：
是一个Java客户端，用于简化Eureka Server的交互，客户端同时也具备一个内置的、使用轮询(round-robin)负载算法的负载均衡器。在应用启动后，将会向Eureka Server发送心跳(默认周期为30秒)。如果Eureka Server在多个心跳周期内没有接收到某个节点的心跳，EurekaServer将会从服务注册表中把这个服务节点移除（默认90秒)。

现在我们要把我们部门微服务提供者模块注册进Eureka，然后从Eureka在去访问。



# Eureka的三大角色

1.Eureka Server 提供服务注册和发现。

2.Service Provider服务提供方将自身服务注册到Eureka，从而使服务消费方能够找到。

3.Service Consumer服务消费方从Eureka获取注册服务列表，从而能够消费服务。



# Eureka的构建和使用

## 1.创建 Eureka服务注册中心的Module

1）.在我们已经构建好的四个工程的父工程里面再创建一个名为：microservicecloud-eureka-7001的Eureka服务注册中心Module

2）.pom文件添加jar包

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>

  <parent>
   <groupId>com.hhf.springcloud</groupId>
   <artifactId>microservicecloud</artifactId>
   <version>0.0.1-SNAPSHOT</version>
  </parent>

  <artifactId>microservicecloud-eureka-7001</artifactId>

  <dependencies>
   <!--eureka-server服务端 -->
   <dependency>
     <groupId>org.springframework.cloud</groupId>
     <artifactId>spring-cloud-starter-eureka-server</artifactId>
   </dependency>
   <!-- 修改后立即生效，热部署 -->
   <dependency>
     <groupId>org.springframework</groupId>
     <artifactId>springloaded</artifactId>
   </dependency>
   <dependency>
     <groupId>org.springframework.boot</groupId>
     <artifactId>spring-boot-devtools</artifactId>
   </dependency>
  </dependencies>

</project>
```

3).然后在resources资源文件夹下创建application.yml

```yaml
server: 
  port: 7001
 
eureka:
  instance:
    hostname: localhost #eureka服务端的实例名称
  client:
    register-with-eureka: false #false表示不向注册中心注册自己。
    fetch-registry: false #false表示自己端就是注册中心，我的职责就是维护服务实例，并不需要去检索服务
    service-url:
      defaultZone: http://${eureka.instance.hostname}:${server.port}/eureka/        #设置与Eureka Server交互的地址查询服务和注册服务都需要依赖这个地址。
 
```

4).然后创建一个启动类进行测试

```java
package com.hhf.springcloud;


import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.netflix.eureka.server.EnableEurekaServer;

@SpringBootApplication
@EnableEurekaServer//EurekaServer服务器端启动类,接受其它微服务注册进来
public class EurekaServer7001_App {
    public static void main(String[] args) {
        SpringApplication.run(EurekaServer7001_App.class, args);
    }
}

```

5).然后启动microservicecloud-eureka-7001这个项目进行一个测试

在浏览器上输入：http://localhost:7001/


## 2.将已有的部门微服务注册进Eureka服务中心

我们将microservicecloud-provider-dept-8001工程注册进microservicecloud-eureka-7001工程Eureka服务中心

#### 1).修改microservicecloud-provider-dept-8001工程的pom文件

添加pom配置文件如下：

```xml
<!-- 将微服务provider侧注册进eureka -->
   <dependency>
     <groupId>org.springframework.cloud</groupId>
     <artifactId>spring-cloud-starter-eureka</artifactId>
   </dependency>
   <dependency>
     <groupId>org.springframework.cloud</groupId>
     <artifactId>spring-cloud-starter-config</artifactId>
   </dependency>
```

#### 2).在yml文件的最后添加内容：

添加内容如下：

```yaml
eureka:
  client: #客户端注册进eureka服务列表内
    service-url: 
      defaultZone: http://localhost:7001/eureka

```

#### 3).在DeptProvider8001_App主启动类添加@EnableEurekaClient注解进行自动注册。

```java
@SpringBootApplication
@EnableEurekaClient //本服务启动后会自动注册进eureka服务中
public class DeptProvider8001_App
{
  public static void main(String[] args)
  {
   SpringApplication.run(DeptProvider8001_App.class, args);
  }
}
 

```

#### 4).测试最终的注册结果

​		1.先启动microservicecloud-eureka-7001这个工程主类，然后再启动microservicecloud-provider-dept-8001这个工程的主类。

​		2.在浏览器中输入：http://localhost:7001/  进行一个测试

​		3.如无意外，那么我们的部门微服务就成功注册进Eureka服务中心了


#### 注意：
我们看到图中的Application下面已经存在了一个服务名字，那么这个服务名字是哪里来的呢？

那么就是从我们的microservicecloud-provider-dept-8001工程下的resources资源文件中写的一个配置

```yaml
spring:
   application:
    name: microservicecloud-dept    //这个就是我们在页面中看到的微服务名字
```



## 3.完善我们的微服务

目前我们已经完成了注册功能，但是此时我们刷新一下http://localhost:7001/ 这个页面就会看到一段红色的字：  

> EMERGENCY! EUREKA MAY BE INCORRECTLY CLAIMING INSTANCES ARE UP WHEN THEY'RE NOT. RENEWALS ARE LESSER THAN THRESHOLD AND HENCE THE INSTANCES ARE NOT BEING EXPIRED JUST TO BE SAFE.


这个是Eureka的自我保护的一个机制。

那么为什么Eureka会有这种自我保护的机制呢？

答：
1.因为长时间没有另外的一些服务访问，也就是说没有心跳。

2.服务名没有变更，已经有的服务现在没了。

某时刻某一个微服务不可用了，eureka不会立刻清理，依旧会对该微服务的信息进行保存




微服务info内容详细信息

​	1.先修改microservicecloud-provider-dept-8001这个工程下的Pom文件

添加内容：

```xml
 <!-- actuator监控信息完善 -->
   <dependency>
     <groupId>org.springframework.boot</groupId>
     <artifactId>spring-boot-starter-actuator</artifactId>
   </dependency>   
```

​	2.修改总工程microservicecloud下的pom文件添加构建build信息

添加内容：

```xml
<build>
   <finalName>microservicecloud</finalName>
   <resources>
     <resource>
       <directory>src/main/resources</directory>
       <filtering>true</filtering>
     </resource>
   </resources>
   <plugins>
     <plugin>
       <groupId>org.apache.maven.plugins</groupId>
       <artifactId>maven-resources-plugin</artifactId>
       <configuration>
         <delimiters>
          <delimit>$</delimit>
         </delimiters>
       </configuration>
     </plugin>
   </plugins>
  </build>

```

​	3.再修改microservicecloud-provider-dept-8001工程下的application.yml文件，来添加我们这个微服务的一些描述。

添加内容：

```yaml
info:
  app.name: hhf-microservicecloud
  company.name: https://www.jianshu.com/u/c7adbc6b595c  //我的博客地址
  build.artifactId: $project.artifactId$
  build.version: $project.version$
 
```

添加成功后的效果图：	![](C:\Users\Administrator\Desktop\springcloud笔记\img\777.png)

那么目前我们微服务的功能已经基本上完善了！！



# Eureka的自我保护机制介绍

**默认情况下，如果EurekaServer在一定时间内没有接收到某个微服务实例的心跳，EurekaServer将会注销该实例（默认90秒）。但是当网络分区故障发生时，微服务与EurekaServer之间无法正常通信，以上行为可能变得非常危险了——因为微服务本身其实是健康的，此时本不应该注销这个微服务。Eureka通过“自我保护模式”来解决这个问题——当EurekaServer节点在短时间内丢失过多客户端时（可能发生了网络分区故障），那么这个节点就会进入自我保护模式。一旦进入该模式，EurekaServer就会保护服务注册表中的信息，不再删除服务注册表中的数据（也就是不会注销任何微服务）。当网络故障恢复后，该Eureka Server节点会自动退出自我保护模式。**

**在自我保护模式中，Eureka Server会保护服务注册表中的信息，不再注销任何服务实例。当它收到的心跳数重新恢复到阈值以上时，该Eureka Server节点就会自动退出自我保护模式。它的设计哲学就是宁可保留错误的服务注册信息，也不盲目注销任何可能健康的服务实例。一句话讲解：好死不如赖活着**

**综上，自我保护模式是一种应对网络异常的安全保护措施。它的架构哲学是宁可同时保留所有微服务（健康的微服务和不健康的微服务都会保留），也不盲目注销任何健康的微服务。使用自我保护模式，可以让Eureka集群更加的健壮、稳定。**

**在Spring Cloud中，可以使用eureka.server.enable-self-preservation = false 禁用自我保护模式。**

 如果你真的想禁用，那么在我们的服务注册中心microservicecloud-eureka-7001工程的application.yml里面添加禁用模式即可。

添加禁用自我保护的内容：

```yaml
server:
   enable-self-preservation: false
```

 但是eureka自我保护模式希望大家在没有其他特殊业务需求的话就不要去禁用它的自我保护模式



# Eure的服务发现

对于注册进eureka里面的微服务，可以通过服务发现来获得该服务的信息

​	1.修改microservicecloud-provider-dept-8001工程的DeptController

我们添加一个服务发现接口名为：DiscoveryClient

在DeptController中添加内容：

```java
import org.springframework.cloud.client.discovery.DiscoveryClient;
@Autowired
  private DiscoveryClient client;
```

我们要做一个DiscoveryRest风格的服务，让大家知道这个Discovery服务发现的信息

在DeptController中再添加内容：

```java
 @RequestMapping(value = "/dept/discovery", method = RequestMethod.GET)
    public Object discovery()
    {
        //client.getServices()的意思是获取eureka里面有多少个微服务
        //那么我们如果只有一个微服务是部门，后续我们在工作中可能会有越来越多的微服务
        //那么List查出来的就会有很多的微服务，然后进行遍历然后打印出来
        List<String> list = client.getServices();
        System.out.println("**********" + list);
        //client.getInstances()的意思就是在那么多的微服务里面，你指定只找哪一个微服务
        List<ServiceInstance> srvList = client.getInstances("MICROSERVICECLOUD-DEPT");
        for (ServiceInstance element : srvList) {
            //然后打印你指定要找的微服务的ID和主机和端口以及访问地址等信息
            System.out.println(element.getServiceId() + "\t" + element.getHost() + "\t" + element.getPort() + "\t"
                    + element.getUri());
        }
        return this.client;
    }
```

然后添加内容成功后，我们在主启动类上添加一个注解

```java
@EnableDiscoveryClient //服务发现
```

然后进入测试

​	1.先启动microservicecloud-eureka-7001工程

​	2.再启动microservicecloud-provider-dept-8001工程

​	3.然后在浏览器中输入：http://localhost:8001/dept/discovery 进行测试

测试的效果图：

![](C:\Users\Administrator\Desktop\springcloud笔记\img\888.png)然后在控制台也可以看到我们注册的微服务信息

![](C:\Users\Administrator\Desktop\springcloud笔记\img\999.png)



​	4.我们使用consumer来访问该服务

​		1).修改microservicecloud-consumer-dept-80工程下的DeptController_Consumer类

添加内容：

```java
//测试@EnableDiscoveryClient,消费端可以调用服务发现
  @RequestMapping(value="/consumer/dept/discovery") 
  public Object discovery()
  {
   return restTemplate.getForObject(REST_URL_PREFIX+"/dept/discovery", Object.class);
  } 
```

添加成功后，我们直接再启动microservicecloud-consumer-dept-80进行测试。

启动成功后在浏览器输入：http://localhost/consumer/dept/discovery 进行测试

测试效果图：

![](C:\Users\Administrator\Desktop\springcloud笔记\img\100.png)

说明从消费者也能成功的访问到



# Eureka集群配置

经过我们上面的一步步操作和深入的理解，我们基本上已经理解了Eureka的服务注册与发现。

那么接下来我们进入重点，那就是Eureka的集群配置。

首先先新建microservicecloud-eureka-7002/microservicecloud-eureka-7003这两个工程来实现集群之配置

​	1.先在我们的总父工程下创建microservicecloud-eureka-7002的maven Module工程

​		1).按照microservicecloud-eureka-7001为模板粘贴Pom文件到microservicecloud-eureka-7002工程下的Pom文件

```xml
<dependencies>
        <!--eureka-server服务端 -->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-eureka-server</artifactId>
        </dependency>
        <!-- 修改后立即生效，热部署 -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>springloaded</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
        </dependency>
    </dependencies>

</project>
```

​	2.再从总父工程下再创建microservicecloud-eureka-7003的maven Module工程

​		1).按照microservicecloud-eureka-7001为模板粘贴Pom文件到microservicecloud-eureka-7003工程下的Pom文件

```xml
<dependencies>
        <!--eureka-server服务端 -->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-eureka-server</artifactId>
        </dependency>
        <!-- 修改后立即生效，热部署 -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>springloaded</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
        </dependency>
    </dependencies>

</project>
```

​	3.修改各自的主启动类

​		1).将microservicecloud-eureka-7001的主启动类分别给microservicecloud-eureka-7002和microservicecloud-eureka-7003都复制一份即可并分别重新起名字，避免混乱。

效果图：

​	![](C:\Users\Administrator\Desktop\springcloud笔记\img\200.png)

​	

​	4.接下来，为了区别集群域名和映射，我们在C:\Windows\System32\drivers\etc下的hosts文件进行修改

在hosts文件中添加以下内容：

```xml
127.0.0.1  eureka7001.com
127.0.0.1  eureka7002.com
127.0.0.1  eureka7003.com
```

​	5.修改3台eureka服务器的application.yml配置

​		1).先修改第一台Eureka服务器microservicecloud-eureka-7001工程下的application.yml配置

直接替换原先的：

```xml
server: 
  port: 7001

eureka: 
  instance:
    hostname: eureka7001.com #Eureka服务端的实例名称
  client: 
    register-with-eureka: false     #false表示不向注册中心注册自己。
    fetch-registry: false     #false表示自己端就是注册中心，我的职责就是维护服务实例，并不需要去检索服务
    service-url: 
      #单机 defaultZone: http://${eureka.instance.hostname}:${server.port}/eureka/       
	 #设置与Eureka Server交互的地址查询服务和注册服务都需要依赖这个地址（单机）。
      defaultZone: http://eureka7002.com:7002/eureka/,http://eureka7003.com:7003/eureka/
      

```

​		2).然后在microservicecloud-eureka-7001工程的application.yml文件复制两份分别给microservicecloud-eureka-7002工程和microservicecloud-eureka-7003工程的resources文件夹中，并且修改microservicecloud-eureka-7002工程和microservicecloud-eureka-7003工程的application.yml的端口和服务器端的实例名称。

microservicecloud-eureka-7002工程的application.yml文件修改内容如下：

```xml
server:
  port: 7002

eureka:
  instance:
    hostname: eureka7002.com #eureka服务端的实例名称
  client:
    register-with-eureka: false     #false表示不向注册中心注册自己。
    fetch-registry: false     #false表示自己端就是注册中心，我的职责就是维护服务实例，并不需要去检索服务
    service-url:
      #单机 defaultZone: http://${eureka.instance.hostname}:${server.port}/eureka/  #设置与Eureka Server交互的地址查询服务和注册服务都需要依赖这个地址（单机）。
      defaultZone: http://eureka7001.com:7001/eureka/,http://eureka7003.com:7003/eureka/
```

microservicecloud-eureka-7003工程的application.yml文件修改内容如下：

```xml
server:
  port: 7003

eureka:
  instance:
    hostname: eureka7003.com #eureka服务端的实例名称
  client:
    register-with-eureka: false     #false表示不向注册中心注册自己。
    fetch-registry: false     #false表示自己端就是注册中心，我的职责就是维护服务实例，并不需要去检索服务
    service-url:
      #单机 defaultZone: http://${eureka.instance.hostname}:${server.port}/eureka/  
	#设置与Eureka Server交互的地址查询服务和注册服务都需要依赖这个地址（单机）。
      defaultZone: http://eureka7001.com:7001/eureka/,http://eureka7002.com:7002/eureka/
```

​	6.microservicecloud-provider-dept-8001微服务发布到上面3台eureka集群配置中

​		1.修改microservicecloud-provider-dept-8001的application.yml文件

```xml
server:
  port: 8001
  
mybatis:
  config-location: classpath:mybatis/mybatis.cfg.xml  #mybatis所在路径
  type-aliases-package: com.hhf.springcloud.entities #entity别名类
  mapper-locations:
  - classpath:mybatis/mapper/**/*.xml #mapper映射文件
    
spring:
   application:
    name: microservicecloud-dept 
   datasource:
    type: com.alibaba.druid.pool.DruidDataSource
    driver-class-name: org.gjt.mm.mysql.Driver
    url: jdbc:mysql://localhost:3306/cloudDB01
    username: root
    password: 520hhf
    dbcp2:
      min-idle: 5
      initial-size: 5
      max-total: 5
      max-wait-millis: 200
      
eureka:
  client: #客户端注册进eureka服务列表内
    service-url: 
      defaultZone:  #注册进这三台集群
http://eureka7001.com:7001/eureka/,http://eureka7002.com:7002/eureka/,http://eureka7003.com:7003/eureka/
  instance:
    instance-id: microservicecloud-dept8001   #自定义服务名称信息
    prefer-ip-address: true     #访问路径可以显示IP地址
      
info:
  app.name: hhf-microservicecloud
  company.name: https://www.jianshu.com/u/c7adbc6b595c  #我的博客地址
  build.artifactId: $project.artifactId$
  build.version: $project.version$
```

好，集群配置完成，我们来测试一下

​	1.先启动microservicecloud-eureka-7001

​	2.再启动microservicecloud-eureka-7002

​	3.再启动microservicecloud-eureka-7003

​	4.然后再启动microservicecloud-provider-dept-8001

​	5.依次打开浏览器：http://eureka7001.com:7001/  

7001的效果图：

![](C:\Users\Administrator\Desktop\springcloud笔记\img\1532245800(1).jpg)

​	

打开 http://eureka7002.com:7002/  进行测试

7002的效果图：

![](C:\Users\Administrator\Desktop\springcloud笔记\img\1532245904(1).jpg)



 打开http://eureka7003.com:7003/   进行测试

7003的效果图：

![](C:\Users\Administrator\Desktop\springcloud笔记\img\1532245981(1).jpg)

​	

最终，我们的集群配置成功！！！



# Eureka比Zookeeper好在哪里？

著名的CAP理论指出，一个分布式系统不可能同时满足C(一致性)、A(可用性)和P(分区容错性)。由于分区容错性P在是分布式系统中必须要保证的，因此我们只能在A和C之间进行权衡。
因此
Zookeeper保证的是CP。
Eureka则是AP。

1.Zookeeper保证CP
当向注册中心查询服务列表时，我们可以容忍注册中心返回的是几分钟以前的注册信息，但不能接受服务直接down掉不可用。也就是说，服务注册功能对可用性的要求要高于一致性。但是zk会出现这样一种情况，当master节点因为网络故障与其他节点失去联系时，剩余节点会重新进行leader选举。问题在于，选举leader的时间太长，30 ~ 120s, 且选举期间整个zk集群都是不可用的，这就导致在选举期间注册服务瘫痪。在云部署的环境下，因网络问题使得zk集群失去master节点是较大概率会发生的事，虽然服务能够最终恢复，但是漫长的选举时间导致的注册长期不可用是不能容忍的。

2.Eureka保证AP
Eureka看明白了这一点，因此在设计时就优先保证可用性。Eureka各个节点都是平等的，几个节点挂掉不会影响正常节点的工作，剩余的节点依然可以提供注册和查询服务。而Eureka的客户端在向某个Eureka注册或时如果发现连接失败，则会自动切换至其它节点，只要有一台Eureka还在，就能保证注册服务可用(保证可用性)，只不过查到的信息可能不是最新的(不保证强一致性)。除此之外，Eureka还有一种自我保护机制，如果在15分钟内超过85%的节点都没有正常的心跳，那么Eureka就认为客户端与注册中心出现了网络故障，此时会出现以下几种情况： 
1. Eureka不再从注册列表中移除因为长时间没收到心跳而应该过期的服务 
2. Eureka仍然能够接受新服务的注册和查询请求，但是不会被同步到其它节点上(即保证当前节点依然可用) 
3. 当网络稳定时，当前实例新的注册信息会被同步到其它节点中

因此， Eureka可以很好的应对因网络故障导致部分节点失去联系的情况，而不会像zookeeper那样使整个注册服务瘫痪。



# Ribbon是什么?

Ribbon是基于Netflix Ribbon实现的一套客户端  负载均衡的工具。

简单的说，Ribbon是Netflix发布的开源项目，主要功能是提供客户端的软件负载均衡算法，将Netflix的中间层服务连接在一起。Ribbon客户端组件提供一系列完善的配置项如连接超时，重试等。简单的说，就是在配置文件中列出Load Balancer（简称LB）后面所有的机器，Ribbon会自动的帮助你基于某种规则（如简单轮询，随机连接等）去连接这些机器。我们也很容易使用Ribbon实现自定义的负载均衡算法。



#  Ribbon能干什么？

LB，即负载均衡(Load Balance)，在微服务或分布式集群中经常用的一种应用。
负载均衡简单的说就是将用户的请求平摊的分配到多个服务上，从而达到系统的HA。
常见的负载均衡有软件Nginx，LVS，硬件 F5等。
相应的在中间件，例如：dubbo和SpringCloud中均给我们提供了负载均衡，SpringCloud的负载均衡算法可以自定义。 

集中式LB：

即在服务的消费方和提供方之间使用独立的LB设施(可以是硬件，如F5, 也可以是软件，如nginx), 由该设施负责把访问请求通过某种策略转发至服务的提供方。

进程内LB：

将LB逻辑集成到消费方，消费方从服务注册中心获知有哪些地址可用，然后自己再从这些地址中选择出一个合适的服务器。

Ribbon就属于进程内LB，它只是一个类库，集成于消费方进程，消费方通过它来获取到服务提供方的地址。



#  Ribbon的相关资料

https://github.com/Netflix/ribbon/wiki/Getting-Started



# Ribbon初步的配置

1.修改microservicecloud-consumer-dept-80工程

​	1).修改pom文件,增加以下内容

```xml
 <!-- Ribbon相关 -->
   <dependency>
     <groupId>org.springframework.cloud</groupId>
     <artifactId>spring-cloud-starter-eureka</artifactId>
   </dependency>
   <dependency>
     <groupId>org.springframework.cloud</groupId>
     <artifactId>spring-cloud-starter-ribbon</artifactId>
   </dependency>
   <dependency>
     <groupId>org.springframework.cloud</groupId>
     <artifactId>spring-cloud-starter-config</artifactId>
   </dependency>

```

2.修改application.yml   追加eureka的服务注册地址

​	2).修改application.yml资源文件，增加以下内容：

```yaml
eureka:
  client:
    register-with-eureka: false
    service-url: 
      defaultZone: http://eureka7001.com:7001/eureka/,http://eureka7002.com:7002/eureka/,http://eureka7003.com:7003/eureka/

```

3.对ConfigBean类进行新注解@LoadBalanced    获得Rest时加入Ribbon的配置。

​	3).修改ConfigBean类，增加以下内容：

```java
@Bean
    @LoadBalanced //要求客户端通过Rest去访问微服务的时候自带负载均衡
    public RestTemplate getRestTemplate(){
            return new RestTemplate();
    }
```

4.主启动类DeptConsumer80_App添加@EnableEurekaClient注解。

​	4).修改主启动类DeptConsumer80_App，增加以下内容：

```
@SpringBootApplication
@EnableEurekaClient  
public class DeptConsumer80_App {
    public static void main(String[] args) {
        SpringApplication.run(DeptConsumer80_App.class, args);

    }
}
```

5.修改DeptController_Consumer客户端访问类。

​	5).修改DeptController_Consumer，增加以下内容：

```java
  //private static final String REST_URL_PREFIX = "http://localhost:8001";
	//从Eureka上面有一个叫MICROSERVICECLOUD-DEPT微服务名字，按名字访问的微服务
    private static final String REST_URL_PREFIX = "http://MICROSERVICECLOUD-DEPT"; 
```

6.测试：

​	1).先启动3个eureka集群，也就是microservicecloud-eureka-7001，microservicecloud-eureka-7002和microservicecloud-eureka-7003。

​	2).再启动microservicecloud-provider-dept-8001并注册进eureka。

​	3).再启动microservicecloud-consumer-dept-80。

​	输入到浏览器： http://localhost/consumer/dept/get/1        进行测试          

​		![](C:\Users\Administrator\Desktop\springcloud笔记\img\1532263588(1).jpg)

​			

​		输入到浏览器: http://localhost/consumer/dept/list        进行测试

![](C:\Users\Administrator\Desktop\springcloud笔记\img\1532263655(1).jpg)

​                            

​			输入到浏览器: http://localhost/consumer/dept/add?dname=大数据部    	 进行测试			![](C:\Users\Administrator\Desktop\springcloud笔记\img\1532263800(1).jpg)

查看我们的mysql数据库看是否添加了大数据部：

添加成功效果图：

​			![](C:\Users\Administrator\Desktop\springcloud笔记\img\1532263924(1).jpg)



总结：Ribbon和Eureka整合后Consumer可以直接调用服务而不用再关心地址和端口号。



# Ribbon负载均衡

Ribbon负载均衡架构图：![](C:\Users\Administrator\Desktop\springcloud笔记\img\1532264483(1).jpg)

Ribbon在工作时分成两步
第一步先选择 EurekaServer ,它优先选择在同一个区域内负载较少的server.
第二步再根据用户指定的策略，在从server取到的服务注册列表中选择一个地址。
其中Ribbon提供了多种策略：比如轮询、随机和根据响应时间加权。



1.在总父工程下创建microservicecloud-provider-dept-8002的maven Module

​	1）.然后再参考microservicecloud-provider-dept-8001的pom文件，将pom文件里的配置复制一份到microservicecloud-provider-dept-8002 pom文件中



2.在总父工程下再创建microservicecloud-provider-dept-8003的maven Module

​	1）.然后再参考microservicecloud-provider-dept-8001的pom文件，将pom文件里的配置复制一份到microservicecloud-provider-dept-8003 pom文件中



3.将microservicecloud-provider-dept-8001的java源码复制两份分别给microservicecloud-provider-dept-8002和microservicecloud-provider-dept-8003，并且分别修改主启动类名字

修改成功后的效果图：

​	![](C:\Users\Administrator\Desktop\springcloud笔记\img\1532266665(1).jpg)



4.然后再将microservicecloud-provider-dept-8001工程下的resources资源文件夹里面的两个配置文件都复制到microservicecloud-provider-dept-8002和microservicecloud-provider-dept-8003

效果图如下：

![](C:\Users\Administrator\Desktop\springcloud笔记\img/1532267410(1).jpg)

然后我们修改microservicecloud-provider-dept-8002和microservicecloud-provider-dept-8003的application.yml文件里面的端口，都分别改为8002和8003端口。



5.新建两个数据库给microservicecloud-provider-dept-8002和microservicecloud-provider-dept-8003这两个工程连接和使用

备注：创建cloudDB02数据库，执行以下sql语句

```sql
DROP DATABASE IF EXISTS cloudDB02;
 
CREATE DATABASE cloudDB02 CHARACTER SET UTF8;

USE cloudDB02;

CREATE TABLE dept
(
  deptno BIGINT NOT NULL PRIMARY KEY AUTO_INCREMENT,
  dname VARCHAR(60),
  db_source   VARCHAR(60)
);
 
INSERT INTO dept(dname,db_source) VALUES('开发部',DATABASE());
INSERT INTO dept(dname,db_source) VALUES('人事部',DATABASE());
INSERT INTO dept(dname,db_source) VALUES('财务部',DATABASE());
INSERT INTO dept(dname,db_source) VALUES('市场部',DATABASE());
INSERT INTO dept(dname,db_source) VALUES('运维部',DATABASE());
 
SELECT * FROM dept;
 
```

再创建一个名为cloudDB03的数据库，执行以下sql语句。

```sql
DROP DATABASE IF EXISTS cloudDB03;

CREATE DATABASE cloudDB03 CHARACTER SET UTF8;

USE cloudDB03;


CREATE TABLE dept
(
  deptno BIGINT NOT NULL PRIMARY KEY AUTO_INCREMENT,
  dname VARCHAR(60),
  db_source   VARCHAR(60)
);

INSERT INTO dept(dname,db_source) VALUES('开发部',DATABASE());
INSERT INTO dept(dname,db_source) VALUES('人事部',DATABASE());
INSERT INTO dept(dname,db_source) VALUES('财务部',DATABASE());
INSERT INTO dept(dname,db_source) VALUES('市场部',DATABASE());
INSERT INTO dept(dname,db_source) VALUES('运维部',DATABASE());

SELECT * FROM dept;
```

然后再改microservicecloud-provider-dept-8002和microservicecloud-provider-dept-8003这两个工程的application.yml文件下的数据库源的url指定的数据库名字

microservicecloud-provider-dept-8002工程连 cloudDB02，

microservicecloud-provider-dept-8003工程连 cloudDB03。



好了，我们做了那么多配置和修改，现在要进入测试我们的负载均衡了！！！！

测试的步骤：

​			1.启动3个eureka集群配置（microservicecloud-eureka-7001系列）

​			2.启动3个Dept微服务并各自测试通过（microservicecloud-provider-dept-8001系列）

​				**启动3个Dept微服务成功后先进行一次测试**

​					**1).先在浏览器输入第一个网址：http://localhost:8001/dept/list     进行测试**

​					**2).再从浏览器中输入第二个网址：http://localhost:8002/dept/list    进行测试**

​					**3).再从浏览器中输入第三个网址：http://localhost:8003/dept/list   进行测试**

​	**如果三个网址都成功出现JSON字符串，并且db_source的数据都不同，那么说明成功了！！！**

​			3.启动microservicecloud-consumer-dept-80

​				启动成功以后，在浏览器输入：http://localhost/consumer/dept/list

​				 就可以体现负载均衡的效果了。

​	启动成功后的负载均衡效果图：  这是第一次访问的结果，

![](C:\Users\Administrator\Desktop\springcloud笔记\img/1532270846(1).jpg)

我们在http://localhost/consumer/dept/list 这个页面上刷新一次该页面就会看到clouddb03会变成clouddb02再刷新一次就会变成clouddb01。依次这样，这种模式采用的是轮询算法，这里就不演示了。

**备注：注意观察看到返回的数据库名字，各不相同，负载均衡实现**

然后再打开http://eureka7001.com:7001/  你就可以看到在一个微服务下面挂着三个实例

![](C:\Users\Administrator\Desktop\springcloud笔记\img/1532272323(1).jpg)

**总结：Ribbon其实就是一个软负载均衡的客户端组件，他可以和其他所需请求的客户端结合使用，和eureka结合只是其中的一个实例。**



# Ribbon核心组件之IRule

根据特定算法中从服务列表中选取一个要访问的服务。

SpringCloud结合Ribbon，它默认出厂自带了7种算法。

​	第一种是：RoundRobinRule 轮询

​	第二种是：RandomRule 随机

​	第三种是：AvailabilityFilteringRule 会先过滤掉由于多次访问故障而处于断路器跳闸状态的服务，
还有并发的连接数量超过阈值的服务，然后对剩余的服务列表按照轮询策略进行访问。

​	第四种是：WeightedResponseTimeRule 根据平均响应时间计算所有服务的权重，响应时间越快服务权重越大被选中的概率越高。刚启动时如果统计信息不足，则使用RoundRobinRule策略，等统计信息足够，
会切换到WeightedResponseTimeRule。

​	第五种是：RetryRule 先按照RoundRobinRule的策略获取服务，如果获取服务失败则在指定时间内会进行重试，获取可用的服务。

​	第六种是：BestAvailableRule 会先过滤掉由于多次访问故障而处于断路器跳闸状态的服务，然后选择一个并发量最小的服务。

​	第七种是：ZoneAvoidanceRule 默认规则,复合判断server所在区域的性能和server的可用性选择服务器。

那么目前，我们实现了第一种算法RoundRobinRule 轮询的方式。

那我们就来做第二种随机算法的实现吧。

在microservicecloud-consumer-dept-80工程ConfigBean类下做一个随机算法的方法

```java
 	@Bean
    public IRule myRule(){
        //用重新选择的随机算法替代默认的轮询算法。
        return new RandomRule();
    }
```

我们来测试一下：

​			1.启动3个eureka集群配置（microservicecloud-eureka-7001系列）。

​			2.启动3个Dept微服务并各自测试通过（microservicecloud-provider-dept-8001系列）。

​			3.启动microservicecloud-consumer-dept-80工程进行随机算法的测试。

请在浏览器输入：http://localhost/consumer/dept/list  

随机的效果我不截图了，因为用了随机算法，随机的效果的体现你们就能体会的到。

那么其他的一些算法，我就不一一进行展示了。



# Ribbon自定义算法

如果上面的7种算法，都不够出来业务逻辑，那么可以来自定义算法。

1.修改microservicecloud-consumer-dept-80的主启动类

​	1).在主启动类上添加一个注解@RibbonClient()

```java
/*在启动该微服务的时候就能去加载我们的自定义Ribbon配置类，从而使配置生效。
并且官方文档明确给出了警告：
	这个自定义配置类不能放在@ComponentScan所扫描的当前包下以及子包下，
	否则我们自定义的这个配置类就会被所有的Ribbon客户端所共享，也就是说
	我们达不到特殊化定制的目的了。*/

//那么解决方案是重新在java包下再建一个包名，并把MySelfRule类放入该包内。
@RibbonClient(name="MICROSERVICECLOUD-DEPT",configuration=MySelfRule.class)
```

请看下面的效果图：

![](C:\Users\Administrator\Desktop\springcloud笔记\img\1532357425(1).jpg)



编写MySelfRule自定义配置类的：

```java
@Configuration
public class MySelfRule{
  @Bean
  public IRule myRule()
  {
   return new RandomRule();//Ribbon默认是轮询，我自定义为随机
  }
}
```

然后进行测试：

​			1.启动3个eureka集群配置（microservicecloud-eureka-7001系列）。

​			2.启动3个Dept微服务并各自测试通过（microservicecloud-provider-dept-8001系列）。

​			3.启动microservicecloud-consumer-dept-80工程进行随机算法的测试。

请在浏览器输入：http://localhost/consumer/dept/get/1

测试的效果我不截图了，因为用的是自定义随机算法，随机的效果的实现你们就能体会的到。



# 自定义规则深度解析

需求：依旧轮询策略，但是加上新需求，每个服务器要求被调用5次。也即以前是每台机器一次，现在是每台机器5次。

需求代码实现：  在MySelfRule类中的myRule方法里面添加以下内容：

```java
@Configuration
public class MySelfRule {
    @Bean
    public IRule myRule()
    {
        //return new RandomRule();//Ribbon默认是轮询，自定义为随机
        return new RandomRule();
    }
```

然后在myrule包下创建一个RandomRule_hhf的类，并且添加以下内容：

解析源码：https://github.com/Netflix/ribbon/blob/master/ribbon-loadbalancer/src/main/java/com/netflix/loadbalancer/RandomRule.java

```java
import com.netflix.client.config.IClientConfig;
import com.netflix.loadbalancer.AbstractLoadBalancerRule;
import com.netflix.loadbalancer.ILoadBalancer;
import com.netflix.loadbalancer.Server;

import java.util.List;
import java.util.concurrent.ThreadLocalRandom;

//需求是5次，但是微服务只有8001，8002，8003三台机器
public class RandomRule_hhf extends AbstractLoadBalancerRule {
	
  	//总共被调用的次数，目前要求每台被调用5次	
    private int total = 0;    
  	//当前提供服务的机器号
    private int currentIndex = 0;

    public Server choose(ILoadBalancer lb, Object key) {
        //ILoadBalancer哪一种负载均衡算法如果等于null的话就返回null，那么自然而然，它肯定会加载一种算法，所以它不会变成null。
        if (lb == null) {
            return null;
        }
        //现在还不知道是哪个算法来响应server
        Server server = null;

        //如果说server等于null，那么就看线程是否中断了，如果被中断的话就返回null
        while (server == null) {
            if (Thread.interrupted()) {
                return null;
            }
            //upList的意思就是现在活着的可以对外提供的机器，然后.get()方法通过
            //int index = rand.nextInt(serverCount); 那么就是随机到几就返回第几的值
            List<Server> upList = lb.getReachableServers();
            List<Server> allList = lb.getAllServers();

            //如果serverCount目前有三台，那么就不等于0，那么就是flase。
            int serverCount = allList.size();
            if (serverCount == 0) {
                /*
                 * No servers. End regardless of pass, because subsequent passes
                 * only get more restrictive.
                 */
                return null;
            }
            //这个的意思就是说如果serverCount有三台，那么index就得到了从下标0和1和2数组
           // int index = rand.nextInt(serverCount);
           // server = upList.get(index);
          
          //当第一次total < 5的时候
         //当第二次total < 5的时候
         //当第五次total < 5的时候（那么第五次就不小于5），那么if(total < 5)这段里面的代码就不执行了
           	if(total < 5){
              //那么第一次的server是0号机
              //那么第二次的server也是0号机
              
            	server = upList.get(currentIndex);
              //第一次的总的计数次数是加一个1
              //第二次的总的计数次数是再加一个1
            	total++;
              //当第五次total < 5的时候就走else
            }else {
              //那么total等于0
            	total = 0;
              //而currentIndex就加一个1
            	currentIndex++;
              //那么1大于等于upList.size()，目前假设有三台机器，那么1就不大于等于upList.size()
              //那么现在就是给0号机给1号机进行服务了，以此类推。。
              //但是如果currentIndex等于下标3的时候并且>= upList.size()，但我们按照数组下标来算的话只				//有0和1和2的下标，那么当currentIndex等于下标3的时候这样就是超过第三台了，那么						//currentIndex就重新等于0。以此类推。。
            if(currentIndex >= upList.size())
            {
              currentIndex = 0;
            }
            }

            //如果这个server等于null，那么线程中断一会，下一轮继续
            if (server == null) {
                /*
                 * The only time this should happen is if the server list were
                 * somehow trimmed. This is a transient condition. Retry after
                 * yielding.
                 */
                Thread.yield();
                continue;
            }
            //如果活着好好的，那么就返回server回去
            if (server.isAlive()) {
                return (server);
            }

            // Shouldn't actually happen.. but must be transient or a bug.
            server = null;
            Thread.yield();
        }

        //返回对应该响应服务是8001，还是8002还是8003
        return server;

    }

    protected int chooseRandomInt(int serverCount) {
        return ThreadLocalRandom.current().nextInt(serverCount);
    }

    @Override
    public Server choose(Object key) {
        return choose(getLoadBalancer(), key);
    }

    @Override
    public void initWithNiwsConfig(IClientConfig clientConfig) {
        

    }
}
```

那么我们的自定义算法定义好了以后，我们再回到MySelfRule类中添加RandomRule_hhf类进行自定义算法的测试了

在MySelfRule类中添加RandomRule_hhf类：

```java
@Configuration
public class MySelfRule {
    @Bean
    public IRule myRule()
    {
        //return new RandomRule();//Ribbon默认是轮询，自定义为随机
        //return new RandomRule();
        return new RandomRule_hhf();  //自定义每台机器5次
    }
}
```

进入测试：

​			1.启动3个eureka集群配置（microservicecloud-eureka-7001系列）。

​			2.启动3个Dept微服务并各自测试通过（microservicecloud-provider-dept-8001系列）。

​			3.启动microservicecloud-consumer-dept-80工程进行自定义负载均衡算法的测试。

请在浏览器输入：http://localhost/consumer/dept/get/1

测试的效果我不截图了，只要在页面刷新5次即可看出效果！！



# Feign是什么？

Feign是一个声明式WebService客户端。使用Feign能让编写Web Service客户端更加简单, 它的使用方法是定义一个接口，然后在上面添加注解，同时也支持JAX-RS标准的注解。Feign也支持可拔插式的编码器和解码器。Spring Cloud对Feign进行了封装，使其支持了Spring MVC标准注解和HttpMessageConverters。Feign可以与Eureka和Ribbon组合使用以支持负载均衡。

 总结：Feign是一个声明式的Web服务客户端，使得编写Web服务客户端变得非常容易，只需要创建一个接口，然后在上面添加注解即可。



# Feign能干什么？

Feign旨在使编写Java Http客户端变得更容易。
前面在使用Ribbon+RestTemplate时，利用RestTemplate对http请求的封装处理，形成了一套模版化的调用方法。但是在实际开发中，由于对服务依赖的调用可能不止一处，往往一个接口会被多处调用，所以通常都会针对每个微服务自行封装一些客户端类来包装这些依赖服务的调用。所以，Feign在此基础上做了进一步封装，由他来帮助我们定义和实现依赖服务接口的定义。在Feign的实现下，我们只需创建一个接口并使用注解的方式来配置它(以前是Dao接口上面标注Mapper注解,现在是一个微服务接口上面标注一个Feign注解即可)，即可完成对服务提供方的接口绑定，简化了使用Spring cloud Ribbon时，自动封装服务调用客户端的开发量。



# Feign之工程构建

1.参考microservicecloud-consumer-dept-80 来新建microservicecloud-consumer-dept-feign工程

​	1).将microservicecloud-consumer-dept-80工程下的java包下的所有类以及配置文件复制一份到microservicecloud-consumer-dept-feign

​	2).在microservicecloud-consumer-dept-feign工程下修改主启动类名字：DeptConsumer80_Feign_App

```java
package hhf.springcloud;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.netflix.eureka.EnableEurekaClient;

@SpringBootApplication
@EnableEurekaClient
public class DeptConsumer80_Feign_App {
    public static void main(String[] args) {
        SpringApplication.run(DeptConsumer80_Feign_App.class, args);

    }
}
```



2.microservicecloud-consumer-dept-feign工程pom.xml修改，主要添加对feign的支持

​	1).添加microservicecloud-consumer-dept-feign工程下pomjar包：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>microservicecloud</artifactId>
        <groupId>com.hhf.springcloud</groupId>
        <version>0.0.1-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>

    <artifactId>microservicecloud-consumer-dept-feign</artifactId>

    <dependencies>
        <dependency><!-- 自己定义的api -->
            <groupId>com.hhf.springcloud</groupId>
            <artifactId>microservicecloud-api</artifactId>
            <version>${project.version}</version>
        </dependency>

        <!-- feign相关 -->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-feign</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <!-- 修改后立即生效，热部署 -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>springloaded</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
        </dependency>

        <!-- Ribbon相关 -->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-eureka</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-ribbon</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-config</artifactId>
        </dependency>
    </dependencies>
</project>
```



3.修改microservicecloud-api工程

​	1).在microservicecloud-api工程下添加pom文件的jar包(其他jar包不变):

```xml
<dependency>
     <groupId>org.springframework.cloud</groupId>
     <artifactId>spring-cloud-starter-feign</artifactId>
   </dependency>
```

​	2).在microservicecloud-api工程下新建DeptClientService接口并新增注解@FeignClient

```java
package com.hhf.springcloud.service;
import org.springframework.cloud.netflix.feign.FeignClient;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;

import com.hhf.springcloud.entities.Dept;

import java.util.List;

@FeignClient(value = "MICROSERVICECLOUD-DEPT") //向哪个微服务进行面向接口feign的编码
public interface DeptClientService {
    @RequestMapping(value = "/dept/get/{id}",method = RequestMethod.GET)
    public Dept get(@PathVariable("id") long id);

    @RequestMapping(value = "/dept/list",method = RequestMethod.GET)
    public List<Dept> list();

    @RequestMapping(value = "/dept/add",method = RequestMethod.POST)
    public boolean add(Dept dept);

}
```



4.在microservicecloud-consumer-dept-feign工程下修改Controller的DeptController_Consumer类，添加上一步新建的DeptClientService接口。

​	1).修改Controller的DeptController_Consumer类，并将原先的代码删除，添加以下内容：

```java
package hhf.springcloud.controller;

import com.hhf.springcloud.entities.Dept;
import com.hhf.springcloud.service.DeptClientService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;


import java.util.List;

@RestController
public class DeptController_Consumer {

    @Autowired
    private DeptClientService service;

    @RequestMapping(value = "/consumer/dept/get/{id}")
    public Dept get(@PathVariable("id") Long id)
    {
        return this.service.get(id);
    }

    @RequestMapping(value = "/consumer/dept/list")
    public List<Dept> list()
    {
        return this.service.list();
    }

    @RequestMapping(value = "/consumer/dept/add")
    public Object add(Dept dept)
    {
        return this.service.add(dept);
    }

}

```



5.在microservicecloud-consumer-dept-feign工程下修改DeptConsumer80_Feign_App主启动类：

​	1）.在DeptConsumer80_Feign_App主启动类添加以下内容：

```java
package hhf.springcloud;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.netflix.eureka.EnableEurekaClient;
import org.springframework.cloud.netflix.feign.EnableFeignClients;
import org.springframework.context.annotation.ComponentScan;

@SpringBootApplication
@EnableEurekaClient
@EnableFeignClients(basePackages= {"com.hhf.springcloud"})
@ComponentScan("com.hhf.springcloud")
public class DeptConsumer80_Feign_App {
    public static void main(String[] args) {
        SpringApplication.run(DeptConsumer80_Feign_App.class, args);

    }
```



6.终于到测试阶段了：

​			1).启动3个eureka集群配置（microservicecloud-eureka-7001系列）。

​			2).启动3个Dept微服务并各自测试通过（microservicecloud-provider-dept-8001系列）。

​			3).启动microservicecloud-consumer-dept-feign工程。

请在浏览器输入：http://localhost/consumer/dept/list



# Hystrix是什么？

Hystrix是一个用于处理分布式系统的延迟和容错的开源库，在分布式系统里，许多依赖不可避免的会调用失败，比如超时、异常等，Hystrix能够保证在一个依赖出问题的情况下，不会导致整体服务失败，避免级联故障，以提高分布式系统的弹性。

“断路器”本身是一种开关装置，当某个服务单元发生故障之后，通过断路器的故障监控（类似熔断保险丝），向调用方返回一个符合预期的、可处理的备选响应（FallBack），而不是长时间的等待或者抛出调用方无法处理的异常，这样就保证了服务调用方的线程不会被长时间、不必要地占用，从而避免了故障在分布式系统中的蔓延，乃至雪崩。



# 服务的熔断

熔断机制是应对雪崩效应的一种微服务链路保护机制。
当扇出链路的某个微服务不可用或者响应时间太长时，会进行服务的降级，进而熔断该节点微服务的调用，快速返回"错误"的响应信息。当检测到该节点微服务调用响应正常后恢复调用链路。在SpringCloud框架里熔断机制通过Hystrix实现。Hystrix会监控微服务间调用的状况，当失败的调用到一定阈值，缺省是5秒内20次调用失败就会启动熔断机制。熔断机制的注解是@HystrixCommand。

1.创建新的子工程，来演示服务熔断，子工程名为microservicecloud-provider-dept-hystrix-8001

2.在microservicecloud-provider-dept-hystrix-8001工程的pom文件里添加如下jar包：

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>
 
  <parent>
   <groupId>com.hhf.springcloud</groupId>
   <artifactId>microservicecloud</artifactId>
   <version>0.0.1-SNAPSHOT</version>
  </parent>
 
  <artifactId>microservicecloud-provider-dept-hystrix-8001</artifactId>
 
 
  <dependencies>
   <!--  hystrix -->
   <dependency>
     <groupId>org.springframework.cloud</groupId>
     <artifactId>spring-cloud-starter-hystrix</artifactId>
   </dependency>
   <!-- 引入自己定义的api通用包，可以使用Dept部门Entity -->
   <dependency>
     <groupId>com.hhf.springcloud</groupId>
     <artifactId>microservicecloud-api</artifactId>
     <version>${project.version}</version>
   </dependency>
   <!-- 将微服务provider侧注册进eureka -->
   <dependency>
     <groupId>org.springframework.cloud</groupId>
     <artifactId>spring-cloud-starter-eureka</artifactId>
   </dependency>
   <dependency>
     <groupId>org.springframework.cloud</groupId>
     <artifactId>spring-cloud-starter-config</artifactId>
   </dependency>
   <!-- actuator监控信息完善 -->
   <dependency>
     <groupId>org.springframework.boot</groupId>
     <artifactId>spring-boot-starter-actuator</artifactId>
   </dependency>
   <dependency>
     <groupId>junit</groupId>
     <artifactId>junit</artifactId>
   </dependency>
   <dependency>
     <groupId>mysql</groupId>
     <artifactId>mysql-connector-java</artifactId>
   </dependency>
   <dependency>
     <groupId>com.alibaba</groupId>
     <artifactId>druid</artifactId>
   </dependency>
   <dependency>
     <groupId>ch.qos.logback</groupId>
     <artifactId>logback-core</artifactId>
   </dependency>
   <dependency>
     <groupId>org.mybatis.spring.boot</groupId>
     <artifactId>mybatis-spring-boot-starter</artifactId>
   </dependency>
   <dependency>
     <groupId>org.springframework.boot</groupId>
     <artifactId>spring-boot-starter-jetty</artifactId>
   </dependency>
   <dependency>
     <groupId>org.springframework.boot</groupId>
     <artifactId>spring-boot-starter-web</artifactId>
   </dependency>
   <dependency>
     <groupId>org.springframework.boot</groupId>
     <artifactId>spring-boot-starter-test</artifactId>
   </dependency>
   <!-- 修改后立即生效，热部署 -->
   <dependency>
     <groupId>org.springframework</groupId>
     <artifactId>springloaded</artifactId>
   </dependency>
   <dependency>
     <groupId>org.springframework.boot</groupId>
     <artifactId>spring-boot-devtools</artifactId>
   </dependency>
  </dependencies>
 
</project>
```

3.参考microservicecloud-provider-dept-8001工程的java包中的类，将其microservicecloud-provider-dept-8001所有的类包都复制一份到microservicecloud-provider-dept-hystrix-8001的java包下，然后进行修改。

microservicecloud-provider-dept-hystrix-8001工程修改和添加内容如下：

​		1).添加microservicecloud-provider-dept-hystrix-8001工程的application.yml文件内容：

```xml
sserver:
   port: 8001

 mybatis:
   config-location: classpath:mybatis/mybatis.cfg.xml  #mybatis所在路径
   type-aliases-package: com.hhf.springcloud.entities #entity别名类
   mapper-locations:
   - classpath:mybatis/mapper/**/*.xml #mapper映射文件

 spring:
    application:
     name: microservicecloud-dept
    datasource:
     type: com.alibaba.druid.pool.DruidDataSource
     driver-class-name: org.gjt.mm.mysql.Driver
     url: jdbc:mysql://localhost:3306/cloudDB01
     username: root
     password: 520hhf
     dbcp2:
       min-idle: 5
       initial-size: 5
       max-total: 5
       max-wait-millis: 200

 eureka:
   client: #客户端注册进eureka服务列表内
     service-url:
       defaultZone: http://eureka7001.com:7001/eureka/,http://eureka7002.com:7002/eureka/,http://eureka7003.com:7003/eureka/
   instance:
     instance-id: microservicecloud-dept8001-hystrix   #自定义服务名称信息
     prefer-ip-address: true     #访问路径可以显示IP地址

 info:
   app.name: atguigu-microservicecloud
   company.name: www.hhf.com
   build.artifactId: $project.artifactId$
   build.version: $project.version$
```



​		2).修改microservicecloud-provider-dept-hystrix-8001工程下的DeptController类，把原先DeptController类里面的所有方法以及属性删除，并添加以下内容：

```java
package com.hhf.springcloud.controller;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;
import org.springframework.web.bind.annotation.RestController;

import com.hhf.springcloud.entities.Dept;
import com.hhf.springcloud.service.DeptService;
import com.netflix.hystrix.contrib.javanica.annotation.HystrixCommand;

@RestController
public class DeptController
{
	@Autowired
	private DeptService service = null;

	@RequestMapping(value="/dept/get/{id}",method=RequestMethod.GET)
	@HystrixCommand(fallbackMethod = "processHystrix_Get")
	public Dept get(@PathVariable("id") Long id)
	{
		Dept dept =  this.service.get(id);
		if(null == dept)
		{
			throw new RuntimeException("该ID："+id+"没有没有对应的信息");
		}
		return dept;
	}

	public Dept processHystrix_Get(@PathVariable("id") Long id) {
		return new Dept().setDeptno(id).setDname("该ID："+id+"没有没有对应的信息,null--@HystrixCommand").setDb_source("no this database in MySQL");
	}
}

```

​		3).修改主启动类名，名为：DeptProvider8001_Hystrix_App并添加新注解@EnableCircuitBreaker

```java
@SpringBootApplication
@EnableEurekaClient //本服务启动后会自动注册进eureka服务中
@EnableDiscoveryClient //服务发现
@EnableCircuitBreaker//对hystrixR熔断机制的支持
public class DeptProvider8001_Hystrix_App
{
	public static void main(String[] args)
	{
		SpringApplication.run(DeptProvider8001_Hystrix_App.class, args);
	}
}

```

4.测试：

​			1).启动3个eureka集群配置（microservicecloud-eureka-7001系列）。

​			2).启动microservicecloud-provider-dept-hystrix-8001的DeptProvider8001_Hystrix_App类。

​			3).启动microservicecloud-consumer-dept-80。

在浏览器中输入：  如果在7001页面中显示了[microservicecloud-dept8001-hystrix](http://192.168.31.231:8001/info)这个服务，说明成功！！

![](C:\Users\Administrator\Desktop\springcloud笔记\img\1532527609(1).jpg)



我们再来看消费者来调用

在浏览器再输入：http://localhost/consumer/dept/get/1 先看看能不能获取到正确的信息

如果可以，那么我们再从浏览器输入一个在数据库里面毫无id的数据，看看是不是能达到我们代码中写的效果一样：

在浏览器再输入：http://localhost/consumer/dept/get/4444 

如果没有找到这个id值的信息，那么就页面就会显示一条信息，如下图所示：

![](C:\Users\Administrator\Desktop\springcloud笔记\img/1532528083(1).jpg)

这页面显示的信息，就说明我们的熔断调用成功！！！

总结：向调用方返回一个符合调用预期的，可处理的备选响应，快速返回错误的响应信息。



# 服务的降级

整体资源快不够了，忍痛将某些服务先关掉，待渡过难关，再开启回来。

服务降级处理是在客户端实现完成的，与服务端没有关系。

1.修改microservicecloud-api工程，service包下再创建一个类：名为：DeptClientServiceFallbackFactory

并实现FallbackFactory这个接口。

```java
ackage com.hhf.springcloud.service;

import java.util.List;

import org.springframework.stereotype.Component;

import com.hhf.springcloud.entities.Dept;

import feign.hystrix.FallbackFactory;

@Component
public class DeptClientServiceFallbackFactory implements FallbackFactory<DeptClientService>
{
    @Override
    public DeptClientService create(Throwable throwable)
    {
        return new DeptClientService() {
            @Override
            public Dept get(long id)
            {
                return new Dept().setDeptno(id)
                        .setDname("该ID："+id+"没有没有对应的信息,Consumer客户端提供的降级信息,此刻服务Provider已经关闭")
                        .setDb_source("no this database in MySQL");
            }

            @Override
            public List<Dept> list()
            {
                return null;
            }

            @Override
            public boolean add(Dept dept)
            {
                return false;
            }
        };
    }
}
```

2.再修改microservicecloud-api工程，在DeptClientService接口的注解@FeignClient中添加fallbackFactory属性值

```java
import org.springframework.cloud.netflix.feign.FeignClient;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;

import com.hhf.springcloud.entities.Dept;

import java.util.List;


@FeignClient(value = "MICROSERVICECLOUD-DEPT",fallbackFactory=DeptClientServiceFallbackFactory.class)
public interface DeptClientService {

    @RequestMapping(value = "/dept/get/{id}",method = RequestMethod.GET)
    public Dept get(@PathVariable("id") long id);

    @RequestMapping(value = "/dept/list",method = RequestMethod.GET)
    public List<Dept> list();

    @RequestMapping(value = "/dept/add",method = RequestMethod.POST)
    public boolean add(Dept dept);

}

```

3.在microservicecloud-consumer-dept-feign工程下添加application.yml文件内容

添加内容为：（而其他内容不变）

```yaml
feign: 
  hystrix: 
    enabled: true
```



4.测试：

​	1).启动3个eureka集群配置（microservicecloud-eureka-7001系列）。

​	2).microservicecloud-provider-dept-8001启动。

​	3).microservicecloud-consumer-dept-feign启动。

​	4).在浏览器中输入：http://localhost/consumer/dept/get/1   进行测试。

​	效果图：

​	![](C:\Users\Administrator\Desktop\springcloud笔记\img\1532612075(1).jpg)

​	5).故意关闭微服务microservicecloud-provider-dept-8001。

​	6).再浏览器中输入：http://localhost/consumer/dept/get/1   进行重新测试。

​	效果图：

​	![](C:\Users\Administrator\Desktop\springcloud笔记\img\1532612178(1).jpg)此时服务端provider已经down了，但是我们做了服务降级处理，让客户端在服务端不可用时也会获得提示信息而不会挂起耗死服务器。



# 服务监控hystrixDashboard

除了隔离依赖服务的调用以外，Hystrix还提供了准实时的调用监控（Hystrix Dashboard），Hystrix会持续地记录所有通过Hystrix发起的请求的执行信息，并以统计报表和图形的形式展示给用户，包括每秒执行多少请求多少成功，多少失败等。Netflix通过hystrix-metrics-event-stream项目实现了对以上指标的监控。Spring Cloud也提供了Hystrix Dashboard的整合，对监控内容转化成可视化界面。



1.在总父工程microservicecloud下新建工程microservicecloud-consumer-hystrix-dashboard子工程。

2.在microservicecloud-consumer-hystrix-dashboard工程下的pom文件里面添加jar包：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>microservicecloud</artifactId>
        <groupId>com.hhf.springcloud</groupId>
        <version>0.0.1-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>

    <artifactId>microservicecloud-consumer-hystrix-dashboard</artifactId>
    
    <dependencies>
        <!-- 自己定义的api -->
        <dependency>
            <groupId>com.hhf.springcloud</groupId>
            <artifactId>microservicecloud-api</artifactId>
            <version>${project.version}</version>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <!-- 修改后立即生效，热部署 -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>springloaded</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
        </dependency>
        <!-- Ribbon相关 -->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-eureka</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-ribbon</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-config</artifactId>
        </dependency>
        <!-- feign相关 -->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-feign</artifactId>
        </dependency>
        <!-- hystrix和 hystrix-dashboard相关-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-hystrix</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-hystrix-dashboard</artifactId>
        </dependency>
    </dependencies>
</project>

```

3.在microservicecloud-consumer-hystrix-dashboard工程下的application.yml文件里面添加配置：

```xml
server:
  port: 9001
```



4.创建主启动类，名为：DeptConsumer_DashBoard_App 并在方法上添加注解@EnableHystrixDashboard

```java
@SpringBootApplication
@EnableHystrixDashboard
public class DeptConsumer_DashBoard_App
{
    public static void main(String[] args)
    {
        SpringApplication.run(DeptConsumer_DashBoard_App.class,args);
    }
}
```



好，我们开始进行测试：

​	1).启动microservicecloud-consumer-hystrix-dashboard该微服务监控消费端。

​		在浏览器中输入：http://localhost:9001/hystrix   进行测试。

​	效果图：  出现以下效果说明成功！！！

![](C:\Users\Administrator\Desktop\springcloud笔记\img/1532617175(1).jpg)

​	2).启动3个eureka集群配置（microservicecloud-eureka-7001系列）。

​	3).启动microservicecloud-provider-dept-hystrix-8001。

​		1.在浏览器中输入：http://localhost:8001/dept/get/1  进行测试

​		2.再从浏览器中输入：http://localhost:8001/hystrix.stream  进行测试

​	效果图：

![](C:\Users\Administrator\Desktop\springcloud笔记\img\1532694784.jpg)

3).我们把上面的效果图以图形化的界面进行展示，那么我们回到http://localhost:9001/hystrix页面中，然后在Hystrix Dashboard下面的一个框里输入http://localhost:8001/hystrix.stream 这个网址。

​	效果图1：  然后点击那个按钮进入一个图形化界面.

![](C:\Users\Administrator\Desktop\springcloud笔记\img\1532695119(1).jpg)



效果图2：  目前我们9001工程监控8001工程。

![](C:\Users\Administrator\Desktop\springcloud笔记\img/1532695277(1).jpg)

然后我们来使用这个监控页面，在页面的左边有一个1圈和1线和7色的图，我们来试着狂刷新http://localhost:8001/dept/get/1这个页面，然后再回到图形化监控页面的变化。

说明：实心圆：共有两种含义。它通过颜色的变化代表了实例的健康程度，它的健康度从绿色<黄色<橙色<红色递减。该实心圆除了颜色的变化之外，它的大小也会根据实例的请求流量发生变化，流量越大该实心圆就越大。所以通过该实心圆的展示，就可以在大量的实例中快速的发现故障实例和高压力实例。

曲线：用来记录2分钟内流量的相对变化，可以通过它来观察到流量的上升和下降趋势。

效果图1：狂刷新以后就再迅速回到图形化监控页面来查看变化。

​	![](C:\Users\Administrator\Desktop\springcloud笔记\img\1532695707(1).jpg)

效果图2：看到监控页面的变化了吧？

​	![](C:\Users\Administrator\Desktop\springcloud笔记\img\1532695858(1).jpg)

# zuul路由网关

Zuul包含了对请求的路由和过滤两个最主要的功能：
其中路由功能负责将外部请求转发到具体的微服务实例上，是实现外部访问统一入口的基础而过滤器功能则负责对请求的处理过程进行干预，是实现请求校验、服务聚合等功能的基础.Zuul和Eureka进行整合，将Zuul自身注册为Eureka服务治理下的应用，同时从Eureka中获得其他微服务的消息，也即以后的访问微服务都是通过Zuul跳转后获得。

注意：Zuul服务最终还是会注册进Eureka。

提供=代理+路由+过滤三大功能。



# Zuul路由网关的工程构建

1.新建子工程Module模块microservicecloud-zuul-gateway-9527。

2.在microservicecloud-zuul-gateway-9527工程的pom文件添加jar包。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>microservicecloud</artifactId>
        <groupId>com.hhf.springcloud</groupId>
        <version>0.0.1-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>

    <artifactId>microservicecloud-zuul-gateway-9527</artifactId>


    <dependencies>
        <!-- zuul路由网关 -->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-zuul</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-eureka</artifactId>
        </dependency>
        <!-- actuator监控 -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
        </dependency>
        <!--  hystrix容错-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-hystrix</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-config</artifactId>
        </dependency>
        <!-- 日常标配 -->
        <dependency>
            <groupId>com.hhf.springcloud</groupId>
            <artifactId>microservicecloud-api</artifactId>
            <version>${project.version}</version>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-jetty</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
        </dependency>
        <!-- 热部署插件 -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>springloaded</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
        </dependency>
    </dependencies>

</project>
```



3.在microservicecloud-zuul-gateway-9527工程的resources资源文件包下创建application,yml文件并添加以下内容：

```yaml
server: 
  port: 9527
 
spring: 
  application:
    name: microservicecloud-zuul-gateway
 
eureka: 
  client: 
    service-url: 
      defaultZone: http://eureka7001.com:7001/eureka,http://eureka7002.com:7002/eureka,http://eureka7003.com:7003/eureka  
  instance:
    instance-id: gateway-9527.com
    prefer-ip-address: true 


info:
  app.name: hhf-microcloud
  company.name: www.hhf.com
  build.artifactId: $project.artifactId$
  build.version: $project.version$
```

4.在我们电脑的hosts文件里面添加如下域名：

```xml
127.0.0.1  myzuul.com
```

5.在java包下创建主启动类，名为：Zuul_9527_StartSpringCloudApp

```java
@SpringBootApplication
@EnableZuulProxy
public class Zuul_9527_StartSpringCloudApp
{
    public static void main(String[] args)
    {
        SpringApplication.run(Zuul_9527_StartSpringCloudApp.class, args);
    }
}
```

6.测试：

​	1).启动3个eureka（microservicecloud-eureka-7001系列）。

​	2).启动microservicecloud-provider-dept-8001工程。

​	3).启动microservicecloud-zuul-gateway-9527工程。

​	在浏览器输入：http://eureka7001.com:7001/    来看看初步的配置

效果图1：

![](C:\Users\Administrator\Desktop\springcloud笔记\img/1532697619(1).jpg)





​	4).先不用路由来测试，在浏览器输入：http://localhost:8001/dept/get/2

​	5).启动路由来测试，在浏览器输入：http://myzuul.com:9527/microservicecloud-dept/dept/get/2

​	通过网关也可以访问到我们的微服务。



# Zuul路由访问映射规则

我们对http://myzuul.com:9527/microservicecloud-dept/dept/get/2这个页面做一下路由访问映射的规则

在microservicecloud-zuul-gateway-9527工程下修改以下内容：

​	1.在application.yml文件里面添加以下内容（其他不变）

```yaml
zuul: 
  routes: 
    mydept.serviceId: microservicecloud-dept
    mydept.path: /mydept/**
```

然后我们来测试一下能不能用：

​		1).重新启动microservicecloud-zuul-gateway-9527工程

​		2).在浏览器输入： http://myzuul.com:9527/mydept/dept/get/1     进行测试	

​		最终结果还是能访问成功（这里就不截图了）。



​		3).再从浏览器中输入：http://myzuul.com:9527/microservicecloud-dept/dept/get/2 进行测试

​			microservicecloud-dept这个名字结果也还是能访问成功，那么这个就有点不太安全了，那么最好是单入口单出口。那么我们用代理名字来对外暴露该服务。

​	2).在application.yml文件里面的zuul下面再添加一句内容（其他不变）

```yaml
ignored-services: microservicecloud-dept 

完整配置：
	zuul:
  ignored-services: microservicecloud-dept     #只是添加了这一句！！
  routes:
    mydept.serviceId: microservicecloud-dept
    mydept.path: /mydept/**
```

我们再测试一下：

​		1).重新启动microservicecloud-zuul-gateway-9527工程

​		2).在浏览器输入： http://myzuul.com:9527/mydept/dept/get/1     进行测试	

​		最终结果还是能访问成功（这里就不截图了）。



​		3).再从浏览器中输入：http://myzuul.com:9527/microservicecloud-dept/dept/get/2 进行测试

​		结果却访问不到了，原因是原路径已经被我们封了！！！


