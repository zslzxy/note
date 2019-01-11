[toc]
# Mybatis学习笔记
## Mybatis常见问题
### 1、Mybatis中的占位符 #{} 与 ${} 的区别？
##### #{} ：
在mybatis中，#{} 的作用是以预编译的方式将数据加载到PreparedStatement中的，也就是在打印SQL语句时，是使用 ? 来实现占位，真正执行的时候才将数据加入进去。
##### ${} ：
在mybatis中，${} 的作用是直接将参数的值加载到SQL语句中，也就是在打印SQL时，参数的值已经设置在了SQL语句里面；这种方式有安全问题。

### 2、xml映射文件中，除了增删改查四大标签以外的其它标签？
还有 <resultMap> <sql> <include> <selectKey> 等其它标签。

还有9个动态SQL标签：trim | where | set | foreach | if |choose | when |otherwise | bind

### 3、在mybatis中会有一个就对应这个一个xml樱映射文件，实现的方式是什么？
Dao接口也就是通常说的Mapper接口，Mapper接口的全类名就是xml映射文件的namespace的值。  

映射文件中的每一个SQL语句都是一个preparedStatement对象，Mapper中的每一个方法分别对应xml映射文件中的一个preparedStatement对象。  

而通过一个 namespace + 方法名 就可以找到对应的唯一的一个preparedStatement对象。  

再使用java语言的动态代理，Mybatis运行时使用jdk动态代理动态的生成Mapper接口的Proxy对象，Proxy对象来拦截方法，转而执行preparedStatement对象，并返回结果集。

### 4、Mybatis如何进行分页的？
##### 内存分页：
Mybatis使用RowBounds对象来进行分页，是针对的ResultSet结果集执行的内存分页。
##### 物理分页：
mybatis可以使用编写SQL语句或者分页插件来实现分页的效果。分页插件的原理是拦截SQL语句，并重写SQL语句，添加对应的物理分页语句与分页参数。

### 5、Mybatis的四大插件运行原理，如何编写插件？
##### Mybatis自带的四中插件：
ParameterHandler、ResultSetHandler、StatementHandler、Executor四大插件。每当代理对象执行到这四种插件时，就会进入拦截方法。

##### 编写插件：
实现Mybatis的Interceptor接口并复写intercept()方法，然后在给插件编写注解，指定要拦截哪一个接口的哪些方法即可，记住，别忘了在配置文件中配置你编写的插件。

### 6、Mybatis的Executor执行器的分类与选择？
##### Executor基本执行器：SimpleExecutor、ReuseExecutor、BatchExecutor 三类；

- SimpleExecutor：每执行一次update或select，就开启一个Statement对象，用完立刻关闭Statement对象。

- ReuseExecutor：执行update或select，以sql作为key查找Statement对象，存在就使用，不存在就创建，用完后，不关闭Statement对象，而是放置于Map<String, Statement>内，供下一次使用。简言之，就是重复使用Statement对象。

- BatchExecutor ：执行update（没有select，JDBC批处理不支持select），将所有sql都添加到批处理中（addBatch()），等待统一执行（executeBatch()），它缓存了多个Statement对象，每个Statement对象都是addBatch()完毕后，等待逐一执行executeBatch()批处理。与JDBC批处理相同。

##### 执行器的指定：
在Mybatis配置文件中，可以指定默认的ExecutorType执行器类型，也可以手动给DefaultSqlSessionFactory的创建SqlSession的方法传递ExecutorType类型参数。

配置代码为：
```
SqlSessionFactory sqlSessionFactory = getSqlSessionFactory();
SqlSession openSession = sqlSessionFactory.openSession(ExecutorType.BATCH);

```


### 7、为什么Mybatis是半自动ORM框架？
Mybatis在进行关系对象映射的时候，需要手动编写SQL语句，而hibernate则是自动完成查询与封装，而这之间存在的不同导致一个为全自动ORM，一个为半自动ORM框架。

---

## 一、Mybatis学习笔记

### 1、Mybatis缓存
#### 1）一级缓存--默认是自动开启的
**简介：**  
Mybatis的一级缓存，也称为本地缓存，**适用范围是SQLSession级别**。

指的是与数据库一次会话期间查询到的数据会放到一级缓存中，以后如果在同一次会话期间需要获取相同的数据，直接从缓存中获取，而不需要再去查询数据库。

使用代码：  
```
@Test
	public void test1() throws Exception
	{		
		SqlSessionFactory sqlSessionFactory = getSqlSessionFactory();
		SqlSession openSession = sqlSessionFactory.openSession();
		EmployeeMapper employeeMapper = openSession.getMapper(EmployeeMapper.class);
		Employee employee = employeeMapper.selectEmployeeById(1);
		
		//中间操作
		
		Employee employee1 = employeeMapper.selectEmployeeById(1);

		System.out.println(employee); 
		openSession.close();
	}
```

**关闭缓存的方式：**  
第一种：在select标签中添加 flushCache="true" ，意思是每一次查询都需要刷新缓存，所以都会去查询数据库。
```
<select id="getUser" parameterType="java.lang.String" resultMap="result"  flushCache="true">  
select * from usert where name =#{name}  
</select> 
```

**一级缓存失效情况：**  
- 使用的sqlsession不一样（因为一级缓存是使用sqlsession来执行的）。
- sqlsession相同，但是查询条件不一致。

- 同一次的sqlsession中，如果两次查询中间执行了 **增删改** 操作，一级缓存就会失效，重新从数据库中查询。
- 清空缓存。代码为： openSession.clearCache();


#### 2）二级缓存--默认是关闭的
**简介：**  
二级缓存也称为全局缓存，基于namespace级别的缓存，一个namespace对应着一个二级缓存。

**工作机制：**  
一个会话，查询一条数据，这个数据就会被放在当前会话的一级缓存中。  
如果会话关闭，一级缓存中的数据就会被保存到二级缓存中，新的会话能够从二级缓存中获取数据。

**使用步骤：**  
第一步：在配置文件中开启二级缓存。
```
<setting name="cacheEnabled" value="true"/>
```
第二步：去mapper.xml文件中配置二级缓存。
```xml
<!-- 
	eviction : 缓存回收策略--默认为LRU
		- LRU ： 最近最少使用的，移除最长时间不使用对象
		- FIFO ： 先进先出，按照对象进入缓存的顺序来移除它们。
		- SOFT ： 软引用，移除基于垃圾回收器状态和软引用规则的对象。
		- WEAK ： 弱引用，更加积极的移除基于垃圾回收器状态和弱引用规则的对象。
 	flushInterval : 指定缓存刷新时间。
 		flushInterval="6000" 表示6秒一次刷新
 	readOnly : 是否只读--默认为false
 		- true ： 只读策略，mybatis从缓存中获取数据的操作是只读操作，不会修改数据，
 						只读策略能够加快数据的获取速度，是直接将数据的引用发送给用户，速度更快，不安全。
 		- false ： 非只读策略，mybatis从缓存中获取的数据不是只读的，那么数据可能会被修改。
 						非只读策略是将缓存中的数据通过序列化与反序列化的操作来克隆一份新的数据发送给用户，速度稍慢，安全。
 	size ： 指定二级缓存存储数据的多少。
 	type ： 指定使用什么缓存技术。这里是使用的ehcache来作为缓存技术。
 -->
<cache size="1024" readOnly="true" eviction="FOFO" flushInterval="6000"  type="org.mybatis.caches.ehcache.EhcacheCache"></cache>

```
第三步：将POJO实现序列化接口即可使用二级缓存。

第四步：如果使用ehcache作为缓存的话，必须添加ehcache.xml文件：
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<ehcache xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
xsi:noNamespaceSchemaLocation="../config/ehcache.xsd">
    <diskStore path="F:\\ehcache"/>
<defaultCache
        maxElementsInMemory="10000"
        maxElementsOnDisk="10000"
        eternal="false"
        timeToIdleSeconds="300"
        timeToLiveSeconds="300"
        overflowToDisk="true"
        diskPersistent="true"
        memoryStoreEvictionPolicy="LRU">
   </defaultCache>
</ehcache>
```

**关闭二级缓存：** 在select标签中添加如下内容：    
```
useCache="false"
```

#### Mybatis的缓存浏览图
![image](https://note.youdao.com/yws/api/personal/file/ECADD177E5FF4E949827734CC8D3CA0F?method=download&shareKey=747ecfbe107a1627692f8fe25104a332)




## Nybatis工作原理
### Mybatis的整体层次
- **接口层**：也就是与用户直接交互的层次，用户可以自定义mapper接口或者使用sqlSession的API来执行持久化的操作。

- **数据处理层**：参数的映射、SQL解析、SQL执行、SQL结果集映射等操作。
- **框架支持层**：用于事务管理、连接池管理、缓存机制的操作。
- **引导层**：使用全局配置文件或者java API的方式进行sqlsession的创建、启动。
![image](https://note.youdao.com/yws/api/personal/file/6829991601CB461FA46DD3B2C115ABA4?method=download&shareKey=83292489462b5e14a55f4a63edd7e08d)

### Mybatis运行流程
#### 流程代码：
```java
public class mybatisTest {
	
	private SqlSessionFactory getSqlSessionFactory() throws IOException {
		String resource = "mybatis-config.xml";
		InputStream inputStream = Resources.getResourceAsStream(resource);
		SqlSessionFactory sqlSessionFactory =
		 new SqlSessionFactoryBuilder().build(inputStream);
		return sqlSessionFactory;
	}
		
	/**
	 * 第一步:先创建SqlSessionFactory。
	 * 		通过build()方法去调用parser()方法进行解析，来将配置文件的所有配置全部加载到了configuration对象中，
	 * 		也将mapper.xml文件所有的详细信息以及对应的接口的SQL预编译的语句保存到里面。
	 *
	 * 第二步：创建SqlSession对象。
	 * 		通过sqlsessionfactory对象来开启sqlsession。
	 *		先获取到 Executor 执行器，根据上面传来的configuration来创建 Batch/Reuse/Simple执行器.
	 *		得到的Executor执行器其作用是用来执行 增删改查 操作的执行器。
	 *		创建Executor的时候，会判断是否有二级缓存，有的话，使用CachingExecutor()来包装Executor。
	 *		然后将每一个拦截器重新包装Executor。
	 *
	 *
	 * 第三步:获得 EmployeeMapper.class 类的代理对象。
	 * 		当调用getMapper的时候，会使用MapperProxyFactory闯将一个MapperProxy代理对象。
	 * 		会使用java反射的invocationHandle接口来实现动态代理，并返回这个创建出来的代理对象。
	 *
	 * 第四步：使用 EmployeeMapper 的代理对象来调用接口的方法。
	 *		使用MapperProxy对象的invoke()方法，
	 *		接着在invoke()方法中创建一个mapperMethod对象，
	 *		再调用mapperMethod对象的execute()方法，来判断是执行增、删、改还是查的操作。
	 *		接着在调用query()方法，来创建BoundSql对象那个，保存SQL详细信息。
	 *		然后一直到BaseExecutor类，调用该类的doquery()方法。
	 *		在doquery()方法中创建了四大对象之一的 StatementHandler。
	 *		创建一个默认为PreparedStatementHandler，来创建PreparedStatement的SQL预编译对象。
	 *		在创建一个parameterHandler来设置参数。
	 *		在创建一个TypeHandler来给SQL预编译设置参数。
	 *		接着使用 resultHandler 来处理结果。
	 *		又使用TypeHandler来进行数据库与pojo结果的映射。
	 */
	@Test
	public void test1() throws Exception
	{		
		SqlSessionFactory sqlSessionFactory = getSqlSessionFactory();
		SqlSession openSession = null;
		try {
			openSession = sqlSessionFactory.openSession();
			EmployeeMapper employeeMapper = openSession.getMapper(EmployeeMapper.class);
			Employee employee = employeeMapper.selectEmployeeById(1);
			System.out.println(employeeMapper);
			System.out.println(employeeMapper.getClass());
			System.out.println(employee);
		} catch (Exception e) {
			e.printStackTrace();
		} finally {
			openSession.close();
		}
	}

}

```

#### 流程简介：
- 第一步：创建SqlSessionFactory对象的时候，会创建一个configuration对象。configuration对象中包含了全局配置文件的所有配置，以及所有mapper.xml与mapper接口的信息。

- 第二步：创建了一个SqlSession对象，里面包含了上面创建的sqlsession对象，以及包含了Mybatis的四大对象之一的Executor(执行器)对象(这个对象的创建类型时根据configuration中的配置来进行选择的)。

- 第三步：调用sqlsession对象的getMapper()方法，来创建这个mapper接口的代理对象。这个MapperProxy对象包含了sqlsession对象，也即是说包含了configuration以及Executor等。

- 第四步：使用代理对象MapperProxy来执行增删改查的方法。
    - 1）调用sqlsession的增删改查，再使用sqlsession中的Executor对象来执行。  
    - 2）创建StatementHandler、ParameterHandler、ResultSetHandler三个对象，分别执行各自的功能。  
    - 3）调用StatementHandler的预编译参数，以及使用ParameterHandler来设置参数值。  
    - 4）调用StatementHandler的增删改查方法。  
    - 5）ResultSetHandler来封装结果集。  
    - 6）前面的过程都会使用TypeHandler来进行参数类型的处理。



#### 流程过程图
##### 第一步：创建SqlSessionFactory对象。
![image](https://note.youdao.com/yws/api/personal/file/B259AB159CD8419AA33F96550B09ED29?method=download&shareKey=1a30e5c8eed76dbccdd61e87be6d7c4c)

##### 第二步：创建SqlSession对象。
![image](https://note.youdao.com/yws/api/personal/file/AABBFCFFB7D349C6825F27AD21877AC1?method=download&shareKey=e0bb2ee7bdb7fbcb82c13968151a11a8)

##### 第三步：调用getMapper(Mapper接口)来得到mapper的代理对象。
![image](https://note.youdao.com/yws/api/personal/file/D29DB0E4B9384FBBAF09AB2E0F0985A5?method=download&shareKey=160b3c34378fb5ce43ea1c2064217b9a)

##### 第四步：调用代理对象来执行需要调用的代码---Mybatis的四大对象的用武之地。
![image](https://note.youdao.com/yws/api/personal/file/EB6F42FCF7124460A6DACFD11BB0B13D?method=download&shareKey=73a5a4efba844806c912bc2ba0a37977)

#### Mybatis整体流程图
![image](https://note.youdao.com/yws/api/personal/file/808B1FAB5FBE4F3D85538BED5B606C84?method=download&shareKey=6d7093a2e5dea8a56ccb1c84fda837d6)



## Mybatis插件开发
### 简介：
Mybatis在四大队向的创建过程中，都会有插件的介入。插件能够利用动态代理机制来一层层的包装目标对象。而实现的目标对象执行目标方法之前进行拦截的效果。

### Mybatis的自带的四大插件：
- **Executor** (update, query, flushStatements, commit, rollback, getTransaction, close, isClosed) 

- **ParameterHandler** (getParameterObject, setParameters) 
- **ResultSetHandler** (handleResultSets, handleOutputParameters) 
- **StatementHandler** (prepare, parameterize, batch, update, query)

### 插件原理
当四大对象创建的时候，每一个创建出来的对象不是直接返回的，而是执行 interceptorChain.pluginAll(parameterHandler);

接下来是获取所有的Interceptor（拦截器）（插件需要实现的接口）；调用interceptor.plugin(target);返回target包装后的对象。





---

## 二、通用mapper学习笔记

### 1、通用Mapper的依赖：

```
<!-- https://mvnrepository.com/artifact/tk.mybatis/mapper -->
<dependency>
    <groupId>tk.mybatis</groupId>
    <artifactId>mapper</artifactId>
    <version>4.0.3</version>
</dependency>
<!-- https://mvnrepository.com/artifact/org.mybatis/mybatis -->
<dependency>
    <groupId>org.mybatis</groupId>
    <artifactId>mybatis</artifactId>
    <version>3.4.6</version>
</dependency>
<!-- https://mvnrepository.com/artifact/org.mybatis/mybatis-spring -->
<dependency>
    <groupId>org.mybatis</groupId>
    <artifactId>mybatis-spring</artifactId>
    <version>1.3.2</version>
</dependency>
```


### 2、spring整合通用mapper：

```
<!-- 整合MyBatis -->
	<bean id="sqlSessionFactoryBean" 
class="org.mybatis.spring.SqlSessionFactoryBean">
		<property name="configLocation" value="classpath:mybatis-config.xml"/>
		<property name="dataSource" ref="dataSource"/>
	</bean>

	<!-- 整合通用Mapper所需要做的配置修改： -->
	<!-- 原始全类名：org.mybatis.spring.mapper.MapperScannerConfigurer -->
	<!-- 通用Mapper使用：tk.mybatis.spring.mapper.MapperScannerConfigurer -->
	<bean class="tk.mybatis.spring.mapper.MapperScannerConfigurer">
		<property name="basePackage" value="com.atguigu.mapper.mappers"/>
	</bean>
```


### 3、注解 @Table:
使用方法：标注在类名上     @Table(name="tb_employee")

作用：对应到数据库的表名。
### 4、注解 @Column：
使用方法：标注在属性上      @Column(name="last_name")

作用：用于对应到数据库的字段。
### 5、注解 @Id：
使用方法：标注在属性的主键上，表明为主键。

作用：使用@Id 主键明确标记和数据库表中主键字段对应的实体类字段。

注意：如果使用 xxxByPrimaryKey() 方法的话，必须要标记有 @Id 注解的实体类，不然通用mapper就会将所有的字段理解为主键。
### 6、注解 @GeneratedValue：
使用方法：标注在标注有 @Id 注解的主键上。

##### 自增（mysql）：

```
@GeneratedValue(strategy=GenerationType.IDENTITY)
```

##### 序列（oracle）：

```
@GeneratedValue(
strategy=GenerationType.IDENTITY,
generator="seletc SEQ_ID.nextval from dual")
```


作用：让通用mapper执行插入以后，将自动生成的id的值返回到实体类对象中。
### 7、注解 @Transient：
使用方法：标注在不需要进行映射的属性上。

作用：将标注有这个注解的属性不进行ORM关系映射。


---

## 三、Mybatis-Plus学习笔记
### 1、加入jar：

```
<dependency>
    <groupId>com.baomidou</groupId>
    <artifactId>mybatis-plus</artifactId>
    <version>2.3</version>
</dependency>
```

注意：这个依赖会自动依赖Mybatis以及Mybatis-Spring的依赖
### 2、在spring配置文件整合mybatis-plus
在配置文件中将mybatis的sqlsessionfactorybean的class改为mybatis-plus的sqlsessionfactorybean；

org.mybatis.spring.SqlSessionFactoryBean     改为
com.baomidou.mybatisplus.spring.MybatisSqlSessionFactoryBean

### 3、使用mapper接口去 继承 BaseMapper接口即可使用mybatis-plus的CRUD；

```
public interface EmployeeMapper extends BaseMapper<Employee> {
}
```

### 4、指定表的主键 @TableId

```
@TableId(value = "id",type = IdType.AUTO)
```

注意：value指的是对应到数据库表的id的name，mybatis-plus自动支持驼峰命名法
    type 指的是主键的生成策略，共四种类型：
    
- IdType.AUTO    主键自增              			   value=0   主键自增
- IdType.INPUT   用户输入ID	    			   value=1   用户输入ID
- IdType.ID_WORKER 全局唯一ID,为空自动填    value=2   全局唯一数字id
- IdType.UUID 全局唯一ID，内容为空自动填充   value=3   全局唯一id

### 5、将pojo对应到数据库的表 @TableName()

```
@TableName(value = "tbl_employee",resultMap = "")
```

注意：value 指的是对应到数据库的哪一张表
    resultMap 指的是对应到返回的哪一个结果集。
### 6、配置mybatis-plus的全局配置

```
<bean id="globalConfiguration"  
class="com.baomidou.mybatisplus.entity.GlobalConfiguration">
    <!--制定个mybatis-plus开启驼峰命名法====默认自动开启的-->
    <property name="dbColumnUnderline" value="true"></property>
<!--配置全局的主键策略
value=0   主键自增
value=1   用户输入ID
value=2   全局唯一数字id
value=3   全局唯一id-->
<property name="idType" value="0"></property>
<!--全局表前缀策略配置-->
<property name="tablePrefix" value="tbl_"></property>
</bean>
<!--然后需要将全局配合放到sqlsessionfactorybean中-->
<bean id="sqlSessionFactoryBean" class="com.baomidou.mybatisplus.spring.MybatisSqlSessionFactoryBean">
    <property name="dataSource" ref="dataSource"></property>
    <property name="configLocation" value="classpath:mybatis-config.xml"></property>
    <property name="typeAliasesPackage" value="com.zsl.mp.bean"></property>
    <!--注入mybatis-plus的全局配置，让其生效-->
    <property name="globalConfig" ref="globalConfiguration"></property>
</bean>
```

### 7、指定属性对应的数据库的字段 @TableField
##### 1）用于指定对应数据库表字段的name

```
@TableField(value = "last_name")
private String lastName;
```

##### 2）指定这个自独胆数据库不存在，不需要自动映射

```
@TableField(exist = false)
private double salary;
```

### 8、获取插入的这条数据的主键的值
注意：mybatis-plus自动会返回主键值到插入的对象里。

使用方法：在插入一条数据以后，直接调用对象的getId()方法就可以得到主键。
### 9、插入方法insert：
insert()方法-----根据有数据的字段来进行插入，没字段的数据不执行到SQL语句中。

insertAllColumn()方法---将所有的字段都进行插入，都执行到SQL语句中。
### 10、更新方法update：
updateById()方法---根据id来修改信息，且是根据拥有的字段来进行选择性的插入。

updateAllColumnById()方法---根据id来修改所有的信息
### 11、查询方法select：
selectById()---根据id来进行查找，返回一个pojo对象。

selectOne(Emp emp)---根据一个pojo对象来进行查找一条数据(将pojo对象里面的非空字段来进行SQL查询)。

selectBatchIds()---根据多个(Collection类型或者子类型)的id来进行查询，返回集合类型。

selectByMap()---根据map封装条件查询，返回集合类型。

selectPage()---分页方法

### 12、删除方法delete：
deleteById()---根据id进行删除

deleteBatchIds()---根据多个(Collection类型或者子类型)的id来进行删除。

deleteByMap()---根据map封装条件删除。
