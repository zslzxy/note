# WebService学习文档
## 1. Apache CXF
### 1.1  基于WS方式
#### 第一步：引入jar包
> 在客户端和服务端程序都添加相应的jar
```xml
<dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.11</version>
      <scope>test</scope>
    </dependency>

    <dependency>
      <groupId>org.apache.cxf</groupId>
      <artifactId>cxf-rt-frontend-jaxws</artifactId>
      <version>3.0.1</version>
    </dependency>

    <dependency>
      <groupId>org.apache.cxf</groupId>
      <artifactId>cxf-rt-transports-http-jetty</artifactId>
      <version>3.0.1</version>
    </dependency>

    <dependency>
      <groupId>org.slf4j</groupId>
      <artifactId>slf4j-log4j12</artifactId>
      <version>1.7.12</version>
      <scope>test</scope>
    </dependency>
  </dependencies>
```

#### 第二步：创建server服务器
> 1. 创建接口
```java
package com.zsl.service;
import javax.jws.WebService;

@WebService
public interface HelloService {
    public String sayHello(String name);
}
```
> 2. 创建实现类
```java
package com.zsl.service.impl;
import com.zsl.service.HelloService;
import javax.jws.WebService;

public class HelloSercviceImpl implements HelloService {
    @Override
    public String sayHello(String name) {
        System.out.println("服务调用成功");
        return name + " Welcome to CQ.";
    }
}
```
> 3. 发布服务
```java
package com.zsl.server;

import com.zsl.service.impl.HelloSercviceImpl;
import org.apache.cxf.jaxws.JaxWsServerFactoryBean;

/**
 * 发布服务
 */
public class Server {

    public static void main(String[] args) {
        //创建服务发布的工厂
        JaxWsServerFactoryBean factory = new JaxWsServerFactoryBean();
        //设置服务地址
        factory.setAddress("http://localhost:8000/ws/hello");
        //设置服务类
        factory.setServiceBean(new HelloSercviceImpl());
        //发布服务
        factory.create();
        System.out.println("服务发布成功，端口8000");
    }
}

```
> 4. 访问生成的wsdl文件，直接在后面添加 ?wsdl 即可。
```http
http://localhost:8000/ws/hello?wsql
```

#### 第三步：创建Client端
> 1. 获取对应的接口，不能修改对应的包名等数据。
```java
package com.zsl.service;
import javax.jws.WebService;

@WebService
public interface HelloService {
    public String sayHello(String name);
}
```
> 2. 调用暴露的方法
```java
package com.zsl.client;

import com.zsl.service.HelloService;
import org.apache.cxf.jaxws.JaxWsProxyFactoryBean;

/**
 * 客户端
 */
public class Client {

    public static void main(String[] args) {
        //拿到对应的server的地址

        //创建cxf代理工厂
        JaxWsProxyFactoryBean factory = new JaxWsProxyFactoryBean();
        factory.setAddress("http://localhost:8000/ws/hello");
        //设置该服务对应的接口类型
        factory.setServiceClass(HelloService.class);
        //生成代理对象
        HelloService helloService = factory.create(HelloService.class);
        //远程访问服务端方法
        String hello = helloService.sayHello("zsl");
        System.out.println("返回的结果："+hello);
    }
}
```

### 1.2 WS方式整合Spring
> 配置web.xml文件
```xml
 <servlet>
    <servlet-name>cxfServlet</servlet-name>
    <servlet-class>org.apache.cxf.transport.servlet.CXFServlet</servlet-class>
  </servlet>

  <servlet-mapping>
    <servlet-name>cxfServlet</servlet-name>
    <url-pattern>/ws/*</url-pattern>
  </servlet-mapping>

  <context-param>
    <param-name>contextConfigLocation</param-name>
    <param-value>classpath*:applicationContext.xml</param-value>
  </context-param>
  <listener>
    <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
  </listener>

```
> 配置server端的applicationContext.xml
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns="http://www.springframework.org/schema/beans"
       xmlns:jaxws="http://cxf.apache.org/jaxws"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://cxf.apache.org/jaxws http://cxf.apache.org/schemas/jaxws.xsd">

    <jaxws:server address="/hello">
        <jaxws:serviceBean>
            <bean class="com.zsl.service.impl.HelloServiceImpl"></bean>
        </jaxws:serviceBean>
    </jaxws:server>

</beans>****
```
> 配置client端的applicationContext.xml文件
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns="http://www.springframework.org/schema/beans"
       xmlns:jaxws="http://cxf.apache.org/jaxws"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://cxf.apache.org/jaxws http://cxf.apache.org/schemas/jaxws.xsd">

    <jaxws:client
          id="helloService"
          serviceClass="com.zsl.service.HelloService"
          address="http://localhost:8080/ws/hello">
    </jaxws:client>

</beans>
```


### 1.3 基于RESTFUL方式---RS方式
#### 第一步，加入jar
```xml
 <properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <maven.compiler.source>1.7</maven.compiler.source>
    <maven.compiler.target>1.7</maven.compiler.target>
  </properties>

  <dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.11</version>
      <scope>test</scope>
    </dependency>

    <!-- https://mvnrepository.com/artifact/org.apache.cxf/cxf-rt-frontend-jaxrs -->
    <dependency>
      <groupId>org.apache.cxf</groupId>
      <artifactId>cxf-rt-frontend-jaxrs</artifactId>
      <version>3.0.1</version>
    </dependency>

    <!-- https://mvnrepository.com/artifact/org.apache.cxf/cxf-rt-transports-http-jetty -->
    <dependency>
      <groupId>org.apache.cxf</groupId>
      <artifactId>cxf-rt-transports-http-jetty</artifactId>
      <version>3.0.1</version>
    </dependency>

    <!-- https://mvnrepository.com/artifact/org.slf4j/slf4j-log4j12 -->
    <dependency>
      <groupId>org.slf4j</groupId>
      <artifactId>slf4j-log4j12</artifactId>
      <version>1.7.12</version>
      <scope>test</scope>
    </dependency>

    <!-- https://mvnrepository.com/artifact/org.apache.cxf/cxf-rt-rs-client -->
    <dependency>
      <groupId>org.apache.cxf</groupId>
      <artifactId>cxf-rt-rs-client</artifactId>
      <version>3.0.1</version>
    </dependency>

    <!-- https://mvnrepository.com/artifact/org.codehaus.jettison/jettison -->
    <dependency>
      <groupId>org.codehaus.jettison</groupId>
      <artifactId>jettison</artifactId>
      <version>1.3.7</version>
    </dependency>

        <!-- https://mvnrepository.com/artifact/org.apache.cxf/cxf-rt-rs-extension-providers -->
    <dependency>
      <groupId>org.apache.cxf</groupId>
      <artifactId>cxf-rt-rs-extension-providers</artifactId>
      <version>3.0.1</version>
    </dependency>

  </dependencies>

  <build>
    <plugins>
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-compiler-plugin</artifactId>
        <version>3.2</version>
        <configuration>
          <source>1.8</source>
          <target>1.8</target>
          <encoding>UTF-8</encoding>
          <showWarnings>true</showWarnings>
        </configuration>
      </plugin>
      <plugin>
        <groupId>org.apache.tomcat.maven</groupId>
        <artifactId>tomcat7-maven-plugin</artifactId>
        <version>2.2</version>
        <configuration>
          <port>8080</port>
          <path>/</path>
        </configuration>
      </plugin>
    </plugins>
  </build>
</project>

```

#### 第二步，创建对象
```java
package com.zsl.bean;

import javax.xml.bind.annotation.XmlRootElement;
import java.util.Date;

/**
 * 就要RestFul风格的传递方式，可以传递xml或者json文件
 */
/**
 * 传递根节点的值为 xml
 *  @XmlRootElement(name = "User")
 * 传递根节点的值为 json
 *  @XmlRootElement(name = "User")
 */
@XmlRootElement(name = "User")
public class User {

    private int id;

    private String name;

    private String desc;

    private Date date;

    public User() {
    }

    public User(int id, String name, String desc, Date date) {
        this.id = id;
        this.name = name;
        this.desc = desc;
        this.date = date;
    }

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getDesc() {
        return desc;
    }

    public void setDesc(String desc) {
        this.desc = desc;
    }

    public Date getDate() {
        return date;
    }

    public void setDate(Date date) {
        this.date = date;
    }

    @Override
    public String toString() {
        return "User{" +
                "id=" + id +
                ", name='" + name + '\'' +
                ", desc='" + desc + '\'' +
                ", date=" + date +
                '}';
    }
}

```

#### 第三步：创建接口
```java
package com.zsl.service;

import com.zsl.bean.User;

import javax.ws.rs.*;
import java.util.List;

@Path("/userService")
@Produces("*/*")
public interface UserService {

    @POST
    @Path("/user")
    //服务器支持的请求的格式数据类型
    @Consumes({"application/xml", "application/json"})
    @Produces({"application/xml", "application/json"})
    void saveUser(User user);

    @PUT
    @Path("/user")
    @Consumes({"application/xml", "application/json"})
    void updateUser(User user);

    @GET
    @Path("/user")
    //服务器支持的返回的数据类型
    @Produces({"application/xml", "application/json"})
    List<User> findAll();

    @GET
    @Path("/user/{id}")
    @Consumes({"application/xml", "application/json"})
    @Produces({"application/xml", "application/json"})
    User findUserById(@PathParam("id")int id);

    @DELETE
    @Path("/user/{id}")
    @Consumes({"application/xml", "application/json"})
    void deleteUserById(@PathParam("id")int id);

}

```

#### 第四步：创建实现类
```java
package com.zsl.service.impl;

import com.zsl.bean.User;
import com.zsl.service.UserService;

import java.util.ArrayList;
import java.util.Date;
import java.util.List;

public class UserServiceImpl implements UserService {
    @Override
    public void saveUser(User user) {
        System.out.println("save user success:" + user);
    }

    @Override
    public void updateUser(User user) {
        System.out.println("update user success:" + user);
    }

    @Override
    public List<User> findAll() {
        List<User> list = new ArrayList<>();
        User user1 = new User(1,"zsl","shuaige",new Date());
        User user2 = new User(2,"ttt","shuaige",new Date());
        User user3 = new User(3,"bbb","shuaige",new Date());
        User user4 = new User(4, "ccc", "shuaige", new Date());
        list.add(user1);
        list.add(user2);
        list.add(user3);
        list.add(user4);
        return list;
    }

    @Override
    public User findUserById(int id) {
        User user = new User(1,"zsl","tytytytyt",new Date());
        return user;
    }

    @Override
    public void deleteUserById(int id) {
        System.out.println("delete user success:" + id);
    }
}

```

#### 第五步：发布服务
```java
    public static void main(String[] args) {
        JAXRSServerFactoryBean factory = new JAXRSServerFactoryBean();
        factory.setAddress("http://localhost:8083/ws/");
        factory.setServiceBean(new UserServiceImpl());
        factory.getInInterceptors().add(new LoggingInInterceptor());
        factory.getOutInterceptors().add(new LoggingOutInterceptor());
        factory.create();
        System.out.println("服务发布成功");
    }
```

#### 第六步：创建客户端
```java
    @Test
    public void addtest() {
        User user = new User(7, "zsl", "adf", new Date());
        WebClient.create("http://localhost:8083/ws/userService/user")
                 .post(user);
    }

    @Test
    public void selectAlltest() {
        WebClient.create("http://localhost:8083/ws/userService/user/1")
                .type(MediaType.APPLICATION_JSON) //指定请求数据格式为json
        .get();
    }
```

### 1.4 RESTFUL整合Spring
#### 第一步：添加jar
```xml
<dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.11</version>
      <scope>test</scope>
    </dependency>

    <!-- https://mvnrepository.com/artifact/org.apache.cxf/cxf-rt-frontend-jaxrs -->
    <dependency>
      <groupId>org.apache.cxf</groupId>
      <artifactId>cxf-rt-frontend-jaxrs</artifactId>
      <version>3.0.1</version>
    </dependency>

    <!-- https://mvnrepository.com/artifact/org.apache.cxf/cxf-rt-rs-client -->
    <dependency>
      <groupId>org.apache.cxf</groupId>
      <artifactId>cxf-rt-rs-client</artifactId>
      <version>3.0.1</version>
    </dependency>

    <!-- https://mvnrepository.com/artifact/org.apache.cxf/cxf-rt-rs-extension-providers -->
    <dependency>
      <groupId>org.apache.cxf</groupId>
      <artifactId>cxf-rt-rs-extension-providers</artifactId>
      <version>3.0.1</version>
    </dependency>

    <!-- https://mvnrepository.com/artifact/org.codehaus.jettison/jettison -->
    <dependency>
      <groupId>org.codehaus.jettison</groupId>
      <artifactId>jettison</artifactId>
      <version>1.3.7</version>
    </dependency>

    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-context</artifactId>
      <version>4.1.6.RELEASE</version>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-web</artifactId>
      <version>4.1.6.RELEASE</version>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-webmvc</artifactId>
      <version>4.1.6.RELEASE</version>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-beans</artifactId>
      <version>4.1.6.RELEASE</version>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-expression</artifactId>
      <version>4.1.6.RELEASE</version>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-test</artifactId>
      <version>4.1.6.RELEASE</version>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-context-support</artifactId>
      <version>4.1.6.RELEASE</version>
    </dependency>
  </dependencies>

  <build>
    <plugins>
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-compiler-plugin</artifactId>
        <version>3.2</version>
        <configuration>
          <source>1.8</source>
          <target>1.8</target>
          <encoding>UTF-8</encoding>
          <showWarnings>true</showWarnings>
        </configuration>
      </plugin>
      <plugin>
        <groupId>org.apache.tomcat.maven</groupId>
        <artifactId>tomcat7-maven-plugin</artifactId>
        <version>2.2</version>
        <configuration>
          <port>8080</port>
          <path>/</path>
        </configuration>
      </plugin>
    </plugins>
  </build>
</project>

```
#### 第二步：配置web.xml
```xml
  <servlet>
    <servlet-name>cxfServlet</servlet-name>
    <servlet-class>org.apache.cxf.transport.servlet.CXFServlet</servlet-class>
  </servlet>

  <servlet-mapping>
    <servlet-name>cxfServlet</servlet-name>
    <url-pattern>/ws/*</url-pattern>
  </servlet-mapping>

  <context-param>
    <param-name>contextConfigLocation</param-name>
    <param-value>classpath*:applicationContext.xml</param-value>
  </context-param>
  <listener>
    <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
  </listener>

```

#### 第三步：创建接口
```java
package com.zsl.service;

import com.zsl.bean.User;

import javax.ws.rs.*;
import java.util.List;

@Path("/userService")
@Produces("*/*")
public interface UserService {

    @POST
    @Path("/user")
    //服务器支持的请求的格式数据类型
    @Consumes({"application/xml", "application/json"})
    @Produces({"application/xml", "application/json"})
    void saveUser(User user);

    @PUT
    @Path("/user")
    @Consumes({"application/xml", "application/json"})
    void updateUser(User user);

    @GET
    @Path("/user")
    //服务器支持的返回的数据类型
    @Produces({"application/xml", "application/json"})
    List<User> findAll();

    @GET
    @Path("/user/{id}")
    @Consumes({"application/xml", "application/json"})
    @Produces({"application/xml", "application/json"})
    User findUserById(@PathParam("id")int id);

    @DELETE
    @Path("/user/{id}")
    @Consumes({"application/xml", "application/json"})
    void deleteUserById(@PathParam("id")int id);

}

```


#### 第四步：创建接口
```java
package com.zsl.service;

import com.zsl.bean.User;

import javax.ws.rs.*;
import java.util.List;

@Path("/userService")
@Produces("*/*")
public interface UserService {

    @POST
    @Path("/user")
    //服务器支持的请求的格式数据类型
    @Consumes({"application/xml", "application/json"})
    @Produces({"application/xml", "application/json"})
    void saveUser(User user);

    @PUT
    @Path("/user")
    @Consumes({"application/xml", "application/json"})
    void updateUser(User user);

    @GET
    @Path("/user")
    //服务器支持的返回的数据类型
    @Produces({"application/xml", "application/json"})
    List<User> findAll();

    @GET
    @Path("/user/{id}")
    @Consumes({"application/xml", "application/json"})
    @Produces({"application/xml", "application/json"})
    User findUserById(@PathParam("id")int id);

    @DELETE
    @Path("/user/{id}")
    @Consumes({"application/xml", "application/json"})
    void deleteUserById(@PathParam("id")int id);

}

```

#### 第五步：创建实现类
```java
package com.zsl.service.impl;

import com.zsl.bean.User;
import com.zsl.service.UserService;

import java.util.ArrayList;
import java.util.Date;
import java.util.List;

public class UserServiceImpl implements UserService {
    @Override
    public void saveUser(User user) {
        System.out.println("save user success:" + user);
    }

    @Override
    public void updateUser(User user) {
        System.out.println("update user success:" + user);
    }

    @Override
    public List<User> findAll() {
        List<User> list = new ArrayList<>();
        User user1 = new User(1,"zsl","shuaige",new Date());
        User user2 = new User(2,"ttt","shuaige",new Date());
        User user3 = new User(3,"bbb","shuaige",new Date());
        User user4 = new User(4, "ccc", "shuaige", new Date());
        list.add(user1);
        list.add(user2);
        list.add(user3);
        list.add(user4);
        return list;
    }

    @Override
    public User findUserById(int id) {
        User user = new User(1,"zsl","tytytytyt",new Date());
        return user;
    }

    @Override
    public void deleteUserById(int id) {
        System.out.println("delete user success:" + id);
    }
}

```

#### 第六步：配置applicationContext.xml
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns="http://www.springframework.org/schema/beans"
       xmlns:jaxrs="http://cxf.apache.org/jaxrs"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://cxf.apache.org/jaxrs
        http://cxf.apache.org/schemas/jaxrs.xsd">

    <jaxrs:server address="/userService">
        <jaxrs:serviceBeans>
            <bean class="com.zsl.service.impl.UserServiceImpl"></bean>
        </jaxrs:serviceBeans>
    </jaxrs:server>

</beans>
```

#### 第七步：配置客户端
> 客户端直接使用WebClient对象即可访问。
```java
    @Test
    public void addtest() {
        User user = new User(7, "zsl", "adf", new Date());
        WebClient.create("http://localhost:8080/ws/userService/userService/user")
                .type("application/json") //指定请求数据格式为json
                .post(user);

    }

    @Test
    public void selectAlltest() {
        WebClient.create("http://localhost:8080/ws/userService/userService/user/1")
                .type(MediaType.APPLICATION_JSON) //指定请求数据格式为json
                .get();
    }

```