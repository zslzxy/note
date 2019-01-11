[toc]
# 第一章 SpringBatch 入门
## 第一节 SpringBatch概述

Spring Batch 是一个轻量级的、完善的批处理框架,旨在帮助企业建立健壮、高效的批处理应用。Spring Batch是Spring的一个子项目,使用Java语言并基于Spring框架为基础开发,使得已经使用 Spring 框架的开发者或者企业更容易访问和利用企业服务。

Spring Batch 提供了大量可重用的组件,包括了日志、追踪、事务、任务作业统计、任务重启、跳过、重复、资源管理。对于大数据量和高性能的批处理任务,Spring Batch 同样提供了高级功能和特性来支持,比如分区功能、远程功能。总之,通过 Spring Batch 能够支持简单的、复杂的和大数据量的批处理作业。

Spring Batch 是一个批处理应用框架,不是调度框架,但需要和调度框架合作来构建完成的批处理任务。它只关注批处理任务相关的问题,如事务、并发、监控、执行等,并不提供相应的调度功能。如果需要使用调用框架,在商业软件和开源软件中已经有很多优秀的企业级调度框架(如 Quartz、Tivoli、Control-M、Cron 等)可以使用。

框架主要有以下功能：

Transaction management（事务管理）

Chunk based processing（基于块的处理）

Declarative I/O（声明式的输入输出）

Start/Stop/Restart（启动/停止/再启动）

Retry/Skip（重试/跳过）

![image](http://incdn1.b0.upaiyun.com/2017/08/f892d8d5aa9f25c5591e1bc2a374456e.png)

框架一共有4个主要角色：JobLauncher是任务启动器，通过它来启动任务，可以看做是程序的入口。Job代表着一个具体的任务。Step代表着一个具体的步骤，一个Job可以包含多个Step（想象把大象放进冰箱这个任务需要多少个步骤你就明白了）。JobRepository是存储数据的地方，可以看做是一个数据库的接口，在任务执行的时候需要通过它来记录任务状态等等信息。

## 第二节 搭建SpringBatch项目


```
https://start.spring.io/

<dependency>

  <groupId>com.h2database</groupId>
  
  <artifactId>h2</artifactId>
  
  <scope>runtime</scope>
  
</dependency>
```


## 第三节SpringBatch入门程序


```
@Configuration

@EnableBatchProcessing

public class JobConfiguration {
	
	@Autowired
	private JobBuilderFactory jobBuilderFactory;
	
	@Autowired
	private StepBuilderFactory  stepBuilderFactory;
	
	@Bean
	public Job helloWorldJob()
	{
		return jobBuilderFactory.get("helloWorldJob")
				.start(step1())
				.build();
	}
    
	//需要是public的
	@Bean
	public Step step1() {
		return stepBuilderFactory.get("step1")
				.tasklet(new Tasklet() {
					@Override
					public RepeatStatus execute(StepContribution arg0, ChunkContext arg1) throws Exception {
						System.out.println("Hello World!");
						return RepeatStatus.FINISHED;
					}
				}).build();
	}

}
```


## 第四节替换为MySQL数据库


```
<dependency>
		   
    <groupId>mysql</groupId>
		    
    <artifactId>mysql-connector-java</artifactId>
    
</dependency>

		

<dependency>

    <groupId>org.springframework.boot</groupId>
    
    <artifactId>spring-boot-starter-jdbc</artifactId>
    
</dependency>
```


application.properties:


```
spring.datasource.driverClassName=com.mysql.jdbc.Driver

spring.datasource.url=jdbc:mysql://localhost:3306/springbatch

spring.datasource.username=root

spring.datasource.password=root

spring.datasource.schema=classpath:/org/springframework/batch/core/schema-mysql.sql

spring.batch.initialize-schema=always
```



## 第五节 核心API

![image](http://incdn1.b0.upaiyun.com/2017/08/ec41d0fadf62fa7ee3ff5f9577da4849-300x155.png)


    JobInstance：该领域概念和Job的关系与Java中实例和类的关系一样，Job定义了一个工作流程， JobInstance就是该工作流程的一个具体实例。一个Job可以有多个JobInstance， 多个JobInstance之间的区分就要靠另外一个领域概念JobParameters了。
    
    JobParameters：是一组可以贯穿整个Job的运行时配置参数。不同的配置将产生不同的JobInstance，如果你是使用相同的JobParameters运行同一个Job， 那么这次运行会重用上一次创建的JobInstance。另外，Spring Batch还非常贴心的提供了让JobParameters中的部分参数不参与JobInstance区分的功能。
    
    JobExecution： 该领域概念表示JobInstance的一次运行，JobInstance运行时可能会成功或者失败。每一次JobInstance的运行都会产生一个JobExecution。同一个JobInstance（JobParameters相同）可以多次运行，这样该JobInstance将对应多个Jobexecution。JobExecution记录了一个JobInstance在一次运行时的发生的所有事情，因此，一个JobExecution需要包含很多的属性，并且需要持久化，这样才能很好的支撑Restart等Spring Batch特性。
    
    StepExecution： 类似于JobExecution，该领域对象表示Step的一次运行。Step是Job的一部分，因此一个StepExecution会关联到一个Jobexecution。另外，该对象还会存储很多与该次Ste运行相关的所有数据，因此该对象也有很多的属性，并且需要持久化以支持一些Spring Batch的特性。
    
    ExecutionContext： 从前面的JobExecution，StepExecution的属性介绍中已经提到了该领域概念。说穿了，该领域概念就是一个容器，该容器由Batch框架控制，框架会对该容器持久化，开发人员可以使用该容器保存一些数据，以支持在整个BatchJob或者整个Step中共享这些数据



# 第二章 作业流

## 第一节 Job的创建和使用

Job：作业。批处理中的核心概念，是Batch操作的基础单元。

每个作业Job有1个或者多个作业步Step; 

```
@Configuration
@EnableBatchProcessing

public class JobFlowDemo1 {

	@Autowired
	private JobBuilderFactory jobBuilderFactory;
	
	@Autowired
	private StepBuilderFactory stepBuilderFactory;
	
	@Bean
	public Job JobFlowDemo1()
	{
		return jobBuilderFactory.get("jobFlowDemo1")
//				.start(step1())
//				.next(step2())
//				.next(step3())
//				.build();
				.start(step1())
				.on("COMPLETED").to(step2())
				.from(step2()).on("COMPLETED").to(step3())//fail,stopAndRestart()
				.from(step3()).end()
				.build();
	}

	@Bean
	public Step step1() {
		return stepBuilderFactory.get("step1")
				.tasklet(new Tasklet() {
					@Override
					public RepeatStatus execute(StepContribution arg0, ChunkContext arg1) throws Exception {
						System.out.println("step1");
						return RepeatStatus.FINISHED;
					}
				}).build();
	}
	
	@Bean
	public Step step2() {
		return stepBuilderFactory.get("step2")
				.tasklet(new Tasklet() {
					@Override
					public RepeatStatus execute(StepContribution arg0, ChunkContext arg1) throws Exception {
						System.out.println("step2");
						return RepeatStatus.FINISHED;
					}
				}).build();
	}
	
	@Bean
	public Step step3() {
		return stepBuilderFactory.get("step3")
				.tasklet(new Tasklet() {
					@Override
					public RepeatStatus execute(StepContribution arg0, ChunkContext arg1) throws Exception {
						System.out.println("step3");
						return RepeatStatus.FINISHED;
					}
				}).build();
	}
	
}
```



## 第二节 Flow的创建和使用

1.Flow是多个Step的集合

2.可以被多个Job复用

3.使用FlowBuilder来创建


```
@Configuration

@EnableBatchProcessing

public class JobFlowDemo2 {
	
	@Autowired
	private JobBuilderFactory jobBuilderFactory;
	
	@Autowired
	private StepBuilderFactory stepBuilderFactory;
	
	@Bean
	public Step jobFlowStep1()
	{
		return stepBuilderFactory.get("jobFlowStep1")
				.tasklet(new Tasklet() {
					@Override
					public RepeatStatus execute(StepContribution arg0, ChunkContext arg1) throws Exception {
						System.out.println("jobFlowStep1");
						return RepeatStatus.FINISHED;
					}
				}).build();
	}
	
	@Bean
	public Step jobFlowStep2()
	{
		return stepBuilderFactory.get("jobFlowStep2")
				.tasklet(new Tasklet() {
					@Override
					public RepeatStatus execute(StepContribution arg0, ChunkContext arg1) throws Exception {
						System.out.println("jobFlowStep2");
						return RepeatStatus.FINISHED;
					}
				}).build();
	}
	
	@Bean
	public Step jobFlowStep3()
	{
		return stepBuilderFactory.get("jobFlowStep3")
				.tasklet(new Tasklet() {
					@Override
					public RepeatStatus execute(StepContribution arg0, ChunkContext arg1) throws Exception {
						System.out.println("jobFlowStep3");
						return RepeatStatus.FINISHED;
					}
				}).build();
	}
	
	//创建 Flow:Flow是step的集合
	@Bean
	public Flow jobFlowDemo2Flow()
	{
		return new FlowBuilder<Flow>("jobFlowDemo2Flow")
				.start(jobFlowStep1())
				.next(jobFlowStep2())
				.build();
	}
	
	//创建Job
	@Bean
	public Job jobFlowDemo2Job()
	{
		return jobBuilderFactory.get("jobFlowDemo2Job")
				.start(jobFlowDemo2Flow())
				.next(jobFlowStep3()).end()
				.build();
	}

}
```


## 第三节 split实现并发执行


实现任务中的多个step或多个flow并发执行

1：创建若干个step

2：创建两个flow

3：创建一个任务包含以上两个flow，并让这两个flow并发执行


```
@Configuration
@EnableBatchProcessing
public class JobSplitDemo3 {
	
	@Autowired
	private JobBuilderFactory jobBuilderFactory;
	
	@Autowired
	private StepBuilderFactory stepBuilderFactory;
	
	@Bean
	public Step jobSplitStep1()
	{
		return stepBuilderFactory.get("jobSplitStep1")
				.tasklet(new Tasklet() {
					@Override
					public RepeatStatus execute(StepContribution arg0, ChunkContext chunkContext) throws Exception {
						System.out.println(chunkContext.getStepContext().getStepName()+","+Thread.currentThread().getName());
						return RepeatStatus.FINISHED;
					}
				}).build();
	}
	
	@Bean
	public Step jobSplitStep2()
	{
		return stepBuilderFactory.get("jobSplitStep2")
				.tasklet(new Tasklet() {
					@Override
					public RepeatStatus execute(StepContribution arg0, ChunkContext chunkContext) throws Exception {
						System.out.println(chunkContext.getStepContext().getStepName()+","+Thread.currentThread().getName());
						return RepeatStatus.FINISHED;
					}
				}).build();
	}
	
	@Bean
	public Step jobSplitStep3()
	{
		return stepBuilderFactory.get("jobSplitStep3")
				.tasklet(new Tasklet() {
					@Override
					public RepeatStatus execute(StepContribution arg0, ChunkContext chunkContext) throws Exception {
						System.out.println(chunkContext.getStepContext().getStepName()+","+Thread.currentThread().getName());
						return RepeatStatus.FINISHED;
					}
				}).build();
	}
	
	@Bean
	public Flow jobSplitFLow1()
	{
		return new FlowBuilder<Flow>("jobSplitFLow1")
				.start(jobSplitStep1())
				.build();
	}
	
	@Bean
	public Flow jobSplitFLow2()
	{
		return new FlowBuilder<Flow>("jobSplitFLow2")
				.start(jobSplitStep2())
				.next(jobSplitStep3())
				.build();
	}
	
	
	@Bean Job jobSplitJob()
	{
		return jobBuilderFactory.get("jobSplitJob")
				.start(jobSplitFLow1())
				.split(new SimpleAsyncTaskExecutor()).add(jobSplitFLow2())
				.end()
				.build();//让两个flow分别在各自的线程中异步执行
	}

}
```


## 第四节 决策器的使用

接口：JobExecutionDecider


```
//决策器

public class MyDecider implements JobExecutionDecider {

	private int count;
	
	@Override
	public FlowExecutionStatus decide(JobExecution arg0, StepExecution arg1) {
		count++;
		if(count%2==0)
			return new FlowExecutionStatus("even");
		else
			return new FlowExecutionStatus("odd");
	}

}

@Configuration

@EnableBatchProcessing

public class DecisionDemo4 {
	
	@Autowired
	private JobBuilderFactory jobBuilderFactory;
	
	@Autowired
	private StepBuilderFactory stepBuilderFactory;
	
	
	@Bean
	public Step step1()
	{
		return stepBuilderFactory.get("step1")
				.tasklet(new Tasklet() {
					@Override
					public RepeatStatus execute(StepContribution arg0, ChunkContext arg1) throws Exception {
						System.out.println("step1");
						return RepeatStatus.FINISHED;
					}
				}).build();
	}
	
	@Bean
	public Step step2()
	{
		return stepBuilderFactory.get("step2")
				.tasklet(new Tasklet() {
					@Override
					public RepeatStatus execute(StepContribution arg0, ChunkContext arg1) throws Exception {
						System.out.println("step2");
						return RepeatStatus.FINISHED;
					}
				}).build();
	}
	
	@Bean
	public Step step3()
	{
		return stepBuilderFactory.get("step3")
				.tasklet(new Tasklet() {
					@Override
					public RepeatStatus execute(StepContribution arg0, ChunkContext arg1) throws Exception {
						System.out.println("step3");
						return RepeatStatus.FINISHED;
					}
				}).build();
	}
	
	//创建决策器
	@Bean
	public JobExecutionDecider myDecider()
	{
		return new MyDecider();
	}

	//创建Job
	@Bean
	public Job DecisionDemoJob()
	{
		return jobBuilderFactory.get("DecisionDemoJob")
			   .start(step1())
			   .next(myDecider())
			   .from(myDecider()).on("even").to(step3())
			   .from(myDecider()).on("odd").to(step2())
			   .from(step2()).on("*").to(myDecider())
			   .end()
			   .build();
	}
}
```



## 第五节 Job的嵌套

一个Job可以嵌套在另一个Job中，被嵌套的Job称为子Job，外部Job称为父Job。
子Job不能单独执行，需要由父Job来启动

案例：创建两个Job，作为子Job，再创建一个Job作为父Job

```
@Configuration
@EnableBatchProcessing
class NestedJobDemo5 {
	
	@Autowired
	private JobBuilderFactory jobBuilderFactory;
	
	@Autowired
	private StepBuilderFactory stepBuilderFactory;
	
	@Autowired
	private Job childJobOne;
	
	@Autowired
	private Job childJobTwo;
	
	@Autowired
	private JobLauncher launcher;
	
	@Bean
	public Job parentJob(JobRepository repository,PlatformTransactionManager transactionManager)
	{
		return jobBuilderFactory.get("parentJob")
				.start(childJob1(repository,transactionManager))
				.next(childJob2(repository,transactionManager))
				.build();
	}
    
	//返回的是Job类型的Step
	private Step childJob2(JobRepository repository,PlatformTransactionManager transactionManager) {
		return new JobStepBuilder(new StepBuilder("childJobTwo"))
				   .job(childJobTwo).launcher(launcher)
				   .repository(repository)
				   .transactionManager(transactionManager)
				   .build();
	}

	private Step childJob1(JobRepository repository,PlatformTransactionManager transactionManager) {
		return new JobStepBuilder(new StepBuilder("childJobOne"))
				   .job(childJobOne).launcher(launcher)
				   .repository(repository)
				   .transactionManager(transactionManager)
				   .build();
	}
	
}
```

spring.batch.job.names=parentJob


## 第六节监听器的使用

用来监听批处理作业的执行情况

创建监听可以通过实现接口或使用注解




JobExecutionListener(before,after)

StepExecutionListener(before,after)

ChunkListener(before,after,error)

ItemReadListener,ItemProcessListener,ItemWriteListener(before,after,error)

```
public class MyJobListener implements JobExecutionListener{

	@Override
	public void beforeJob(JobExecution jobExecution) {
		// TODO Auto-generated method stub
		System.out.println(jobExecution.getJobInstance().getJobName()+"before...");
	}
	
	@Override
	public void afterJob(JobExecution jobExecution) {
		// TODO Auto-generated method stub
		System.out.println(jobExecution.getJobInstance().getJobName()+"after...");
	}

}

public class MyChunkListener {
	
	@BeforeChunk
	public void beforeChunk(ChunkContext context){
		System.out.println(context.getStepContext().getStepName()+"before...");
	}
	
	@AfterChunk
	public void afterChunk(ChunkContext context){
		System.out.println(context.getStepContext().getStepName()+"after...");
	}

}
```


## 第七节Job参数


```
@Configuration
@EnableBatchProcessing 
public class JobParametersDemo7 implements StepExecutionListener{
	
	@Autowired
	private JobBuilderFactory jobBuilderFactory;
	
	@Autowired
	private StepBuilderFactory stepBuilderFactory;
	

	private Map<String, JobParameter> parameters;
	 
	
	@Bean
	public Job parametersJob()
	{
		return jobBuilderFactory.get("parametersJob")
				.start(parameterStep())
				.build();
	}

	//在step执行之前接收到参数，因为会在step中用到,所以使用监听
    @Bean
	public Step parameterStep() {
		return stepBuilderFactory.get("parameterStep")
				.listener(this)
				.tasklet(new Tasklet() {
					@Override
					public RepeatStatus execute(StepContribution arg0, ChunkContext arg1) throws Exception {
						System.out.println(parameters.get("name"));
						return RepeatStatus.FINISHED;
					}
				}).build();
	}
    

    @Override
	public void beforeStep(StepExecution stepExecution) {
    	parameters = stepExecution.getJobParameters().getParameters();
	}
    
	@Override
	public ExitStatus afterStep(StepExecution stepExecution) {
		return null;
	}

}
```



# 第三章 数据输入

## 第一节 ItemReader概述


```
@Configuration

@EnableBatchProcessing

public class ItemReaderDemo8 {
	
	@Autowired
	private JobBuilderFactory jobBuilderFactroy;
	
	@Autowired
	private StepBuilderFactory stepBuilderFactory;
	
	@Bean
	public Job itemReaderJob()
	{
		return jobBuilderFactroy.get("itemReaderJob")
			   .start(itemReaderStep())
			   .build();
	}

	@Bean
	public Step itemReaderStep() {
		return stepBuilderFactory.get("itemReaderStep")
			   .<String,String>chunk(2)
			   .reader(itemReaderDemoRead())
			   .writer(list->{
			         for(String item:list)
			         {
			        	 System.out.println(item+"...");
			         }
			   }).build();
			   
	}

	@Bean
	public MyReader itemReaderDemoRead() {
		List<String> data=Arrays.asList("cat","dog","pig","duck");
		return new MyReader(data);
	}

}
```



## 第二节 从数据库中读取数据

JdbcPagingItemReader


```
@Configuration

@EnableBatchProcessing

class DbJdbcDemo9 {
	
	@Autowired
	private JobBuilderFactory jobBuilderFactory;
	
	@Autowired
	private StepBuilderFactory stepBuilderFactory;

	@Autowired
	@Qualifier("dbJdbcWriter")
	private ItemWriter<? super User> dbJdbcWriter;

	@Autowired
	private DataSource dataSource;
	
	@Bean
	public Job dbJdbcJob()
	{
		return jobBuilderFactory.get("dbJdbcJob")
			  .start(dbJdbcStep())
			  .build();
	}

	@Bean
	public Step dbJdbcStep() {
		return stepBuilderFactory.get("dbJdbcStep")
			   .<User,User>chunk(2)
		       .reader(dbJdbcReader())
		       .writer(dbJdbcWriter)
		       .build();
	}

	@Bean
	@StepScope
	public JdbcPagingItemReader<User> dbJdbcReader() {
		
		JdbcPagingItemReader<User> reader=new JdbcPagingItemReader<>();
		//使用JdbcPagingItemReader对象从数据库中读取数据
		reader.setDataSource(dataSource);
		reader.setFetchSize(2);
		reader.setRowMapper(new RowMapper<User>() {
			@Override
			public User mapRow(ResultSet rs, int rowNum) throws SQLException {
				User user=new User();
				user.setId(rs.getInt(1));
				user.setUsername(rs.getString(2));
				user.setPassword(rs.getString(3));
				user.setAge(rs.getInt(4));
				return user;
			}
		});
		//指定sql语句
		MySqlPagingQueryProvider provider =new MySqlPagingQueryProvider();
		provider.setSelectClause("id,username,password,age");
		provider.setFromClause("from User");
		
		Map<String,Order> sort=new HashMap<>(1);
		sort.put("id", Order.ASCENDING);
		provider.setSortKeys(sort);
		
		reader.setQueryProvider(provider);
		return reader;
	}
}
```

## 第三节 从普通文件中读取数据

FlatFileItemReader


```
@Configuration

@EnableBatchProcessing

class FileDemo10 {
	
	@Autowired
	private JobBuilderFactory jobBuilderFactory;
	
	@Autowired
	private StepBuilderFactory stepBuilderFactory;

	@Autowired
	@Qualifier("fileWriter")
	private ItemWriter<? super Customer> fileWriter;


	@Bean
	public Job fileJob()
	{
		return jobBuilderFactory.get("fileJob")
			  .start(fileStep())
			  .build();
	}

	@Bean
	public Step fileStep() {
		return stepBuilderFactory.get("fileStep")
			   .<Customer,Customer>chunk(100)
		       .reader(fileReader())
		       .writer(fileWriter)
		       .build();
	}

	@Bean
	@StepScope
	public FlatFileItemReader<Customer> fileReader() {
		FlatFileItemReader<Customer> reader = new FlatFileItemReader<>();
		reader.setResource(new ClassPathResource("customer.csv"));
		reader.setLinesToSkip(1);//跳过第一行
		
		//如何解析数据
		DelimitedLineTokenizer tokenizer=new DelimitedLineTokenizer();
		//制定四个表头字段
		tokenizer.setNames(new String[]{"id","firstName","lastName","birthday"}); 
		//把一行映射为Customer对象
		DefaultLineMapper<Customer> mapper=new DefaultLineMapper<>();
		mapper.setLineTokenizer(tokenizer);
		mapper.setFieldSetMapper(new FieldSetMapper<Customer>() {
			@Override
			public Customer mapFieldSet(FieldSet fieldSet) throws BindException {
				Customer customer =new Customer();
				customer.setId(fieldSet.readLong("id"));
				customer.setFirstName(fieldSet.readString("firstName"));
				customer.setLastName(fieldSet.readString("lastName"));
				customer.setBirthday(fieldSet.readString("birthday"));
				return customer;
			}
		});
		
		mapper.afterPropertiesSet();
		reader.setLineMapper(mapper);
		
		return reader;
	}
```

	
	
## 第四节 从XML文件中读取数据

StaxEventItemReader



```
<dependency>
	<groupId>org.springframework</groupId>
	<artifactId>spring-oxm</artifactId>
</dependency>
<dependency>
    <groupId>com.thoughtworks.xstream</groupId>
	<artifactId>xstream</artifactId>
	<version>1.4.7</version>
</dependency>
```



```
@Configuration

@EnableBatchProcessing

public class XmlDemo11 {
	
	@Autowired
	private JobBuilderFactory jobBuilderFactory;
	
	@Autowired
	private StepBuilderFactory stepBuilderFactory;
    
	@Autowired
	@Qualifier("xmlWriter")
	private ItemWriter<? super Customer> xmlWriter;

	

	@Bean
	public Job xmlJob()
	{
		return jobBuilderFactory.get("xmlJob")
			  .start(xmlStep())
			  .build();
	}

    @Bean
	public Step xmlStep() {
    	return stepBuilderFactory.get("xmlStep")
 			   .<Customer,Customer>chunk(20)
 		       .reader(xmlReader())
 		       .writer(xmlWriter)
 		       .build();
	}


	@Bean
	@StepScope
	public StaxEventItemReader<Customer> xmlReader() {
		StaxEventItemReader<Customer> reader = new StaxEventItemReader<>();
		reader.setResource(new ClassPathResource("customer.xml"));
		//指定需要处理的根标签
		reader.setFragmentRootElementName("customer");
		//加入oxm的依赖,把xml转成对象
	    XStreamMarshaller unmarshaller=new XStreamMarshaller();
	    Map<String,Class> map = new HashMap<>();
	    map.put("customer", Customer.class);
	    unmarshaller.setAliases(map);
		reader.setUnmarshaller(unmarshaller);
		return reader;
	}
```

	
	
## 第五节 从多个文件中读取数据

MultiResourceItemReader


```
@Configuration

@EnableBatchProcessing

class MultiFileDemo12 {
	
	@Autowired
	private JobBuilderFactory jobBuilderFactory;
	
	@Autowired
	private StepBuilderFactory stepBuilderFactory;

	@Autowired
	@Qualifier("multiFileWriter")
	private ItemWriter<? super Customer> multiFileWriter;

	@Value("classpath:/file*.csv")
	private Resource[] fileResources;


	@Bean
	public Job multiFileJob()
	{
		return jobBuilderFactory.get("multiFileJob")
			  .start(multiFileStep())
			  .build();
	}

	@Bean
	public Step multiFileStep() {
		return stepBuilderFactory.get("multiFileStep")
			   .<Customer,Customer>chunk(100)
		       .reader(multiFileReader())
		       .writer(multiFileWriter)
		       .build();
	}

	@Bean
	@StepScope
	public MultiResourceItemReader<Customer> multiFileReader() {
		MultiResourceItemReader<Customer> reader =new MultiResourceItemReader<Customer>();
		
		reader.setDelegate(fileReader());
		reader.setResources(fileResources);
		return reader;
	}
	
	@Bean
	@StepScope
	public FlatFileItemReader<Customer> fileReader() {
		FlatFileItemReader<Customer> reader = new FlatFileItemReader<>();
		reader.setResource(new ClassPathResource("customer.csv"));
		reader.setLinesToSkip(1);//跳过第一行
		
		//如何解析数据
		DelimitedLineTokenizer tokenizer=new DelimitedLineTokenizer();
		//制定四个表头字段
		tokenizer.setNames(new String[]{"id","firstName","lastName","birthday"}); 
		//把一行映射为Customer对象
		DefaultLineMapper<Customer> mapper=new DefaultLineMapper<>();
		mapper.setLineTokenizer(tokenizer);
		mapper.setFieldSetMapper(new FieldSetMapper<Customer>() {
			@Override
			public Customer mapFieldSet(FieldSet fieldSet) throws BindException {
				Customer customer =new Customer();
				customer.setId(fieldSet.readLong("id"));
				customer.setFirstName(fieldSet.readString("firstName"));
				customer.setLastName(fieldSet.readString("lastName"));
				customer.setBirthday(fieldSet.readString("birthday"));
				return customer;
			}
		});
		
		mapper.afterPropertiesSet();
		reader.setLineMapper(mapper);
		
		return reader;
	}
```

	

## 第六节 ItemReader异常处理及重启


```
@Component("restartReader")

public class RestartReader implements ItemStreamReader<Customer>{
	
	private FlatFileItemReader<Customer> customerFlatFileItemReader=new FlatFileItemReader<>();
    private Long curLine = 0L;
    private boolean restart = false;
    private ExecutionContext executionContext;
    
    public RestartReader()
    {
    	customerFlatFileItemReader.setResource(new ClassPathResource("restartDemo.csv"));
    	customerFlatFileItemReader.setLinesToSkip(1);//跳过第一行
		
		//如何解析数据
		DelimitedLineTokenizer tokenizer=new DelimitedLineTokenizer();
		//制定四个表头字段
		tokenizer.setNames(new String[]{"id","firstName","lastName","birthday"}); 
		//把一行映射为Customer对象
		DefaultLineMapper<Customer> mapper=new DefaultLineMapper<>();
		mapper.setLineTokenizer(tokenizer);
		mapper.setFieldSetMapper(new FieldSetMapper<Customer>() {
			@Override
			public Customer mapFieldSet(FieldSet fieldSet) throws BindException {
				Customer customer =new Customer();
				customer.setId(fieldSet.readLong("id"));
				customer.setFirstName(fieldSet.readString("firstName"));
				customer.setLastName(fieldSet.readString("lastName"));
				customer.setBirthday(fieldSet.readString("birthday"));
				return customer;
			}
		});
		
		mapper.afterPropertiesSet();
		customerFlatFileItemReader.setLineMapper(mapper);
		
    }
    

    @Override
    public Customer read() throws Exception, UnexpectedInputException, ParseException,
            NonTransientResourceException {

        Customer customer = null;

        this.curLine++;

       if(restart){
            customerFlatFileItemReader.setLinesToSkip(this.curLine.intValue()-1);
            restart = false;
            System.out.println("Start reading from line: " + this.curLine);
        }

        customerFlatFileItemReader.open(this.executionContext);
        customer = customerFlatFileItemReader.read();


        if (customer!=null && customer.getFirstName().equals("WrongName")) {
            throw new RuntimeException("Something wrong. Customer id: " + customer.getId());
        }
        return customer;
    }

    @Override
    public void open(ExecutionContext executionContext) throws ItemStreamException {
        this.executionContext = executionContext;
        if (executionContext.containsKey("curLine")){
            this.curLine = executionContext.getLong("curLine");
            this.restart = true;
        }else{
            this.curLine = 0L;
            executionContext.put("curLine",this.curLine);
            System.out.println("Start reading from line: " + this.curLine + 1);
        }

    }

    @Override
    public void update(ExecutionContext executionContext) throws ItemStreamException {
        executionContext.put("curLine",this.curLine);
        System.out.println("currentLine:"+this.curLine);
    }

    @Override
    public void close() throws ItemStreamException {

    }

}
```


# 第四章 数据输出

## 第一节 ItemWriter概述

ItemReader是一个数据一个数据的读，而
ItemWriter是一批一批的输出


```
@Component("myWrite")

public class MyWrite implements ItemWriter<String>{

	@Override
	public void write(List<? extends String> items) throws Exception {
		System.out.println(items.size());
		for(String ss:items){
			System.out.println(ss);
		}
	}

}
```



```
@Configuration
public class ItemWriterDemo {
	
	@Autowired
	private JobBuilderFactory jobBuilderFactory;
	
	@Autowired
	private StepBuilderFactory stepBuilderFactory;

	@Autowired
	@Qualifier("myWrite")
	private ItemWriter<? super String> myWrite;
	
	@Bean
	public Job itemWriterDemoJob()
	{
		return jobBuilderFactory.get("itemWriterDemoJob")
			   .start(itemWriterDemoStep())
			   .build();
	}

	@Bean
	public Step itemWriterDemoStep() {
		
		return stepBuilderFactory.get("itemWriterDemoStep")
			   .<String,String>chunk(5)
			   .reader(myRead())
			   .writer(myWrite)
			   .build();
	}

	@Bean
	public ItemReader<String> myRead() {
		List<String> items=new ArrayList<>();
		for(int i=1;i<=50;i++){
			items.add("java"+i);
		}
		return new ListItemReader<String>(items);
	}
	

}
```



## 第二节 数据输出到数据库

Neo4jItemWriter

MongoItemWriter

RepositoryItemWriter

HibernateItemWriter

JdbcBatchItemWriter

JpaItemWriter

GemfireItemWriter



```
@Configuration

public class ItemWriterDbConfig {
	
	@Autowired
	private DataSource dataSource;
	
	@Bean
	public JdbcBatchItemWriter<Customer> itemWriterDb()
	{
		JdbcBatchItemWriter<Customer> writer=new JdbcBatchItemWriter<Customer>();
		writer.setDataSource(dataSource);
		writer.setSql("insert into customer(id,firstName,lastName,birthday) values "+
				 "(:id,:firstName,:lastName,:birthday)");
		writer.setItemSqlParameterSourceProvider(new BeanPropertyItemSqlParameterSourceProvider<>());
	    return writer;
	}

}
```



## 第三节 数据输出到普通文件

FlatFileItemWriter

案例：从数据库中读取数据写入到文件


```
@Configuration
public class DbFileWriterConfig {
	
	@Bean
	public FlatFileItemWriter<Customer> dbFileWriter() throws Exception
	{
		FlatFileItemWriter<Customer> writer=new FlatFileItemWriter<Customer>();
		String path = "e:\\customer.txt";
		writer.setResource(new FileSystemResource(path)); 
		
		//把一个 Customer对象转成一行字符串
		writer.setLineAggregator(new LineAggregator<Customer>() {
			
			ObjectMapper mapper = new ObjectMapper();
			
			@Override
			public String aggregate(Customer item) {
				String str = null;
				try {
					str=mapper.writeValueAsString(item);
				} catch (JsonProcessingException e) {
					e.printStackTrace();
				}
				return str;
			}
		});
		writer.afterPropertiesSet();
		return writer;
	}

}
```



## 第四节 数据输出到xml文件

StaxEvenItemWriter


```
@Configuration
public class XmlFileWriterConfig {
	
	@Bean
	public StaxEventItemWriter<Customer>  xmlFileWriter() throws Exception
	{
		StaxEventItemWriter<Customer> writer=new StaxEventItemWriter<Customer>();
		
		XStreamMarshaller marshaller = new XStreamMarshaller();
		Map<String,Class> aliases = new HashMap<>();
		aliases.put("customer", Customer.class);
		marshaller.setAliases(aliases);
		
		writer.setRootTagName("customers");
		writer.setMarshaller(marshaller);
		
		String path="e:\\cus.xml";
		writer.setResource(new FileSystemResource(path));
		writer.afterPropertiesSet();
		
		return writer;
	}

}
```


## 第五节 数据输出到多个文件

CompositeItemWriter

ClassifierCompositeItemWriter


    
```
    @Bean
    
	public CompositeItemWriter<Customer> multiFileItemWriter() throws Exception
	{
		CompositeItemWriter<Customer> writer=new CompositeItemWriter<Customer>();
		writer.setDelegates(Arrays.asList(jsonFileWriter(),xmlFileWriter()));
		
		writer.afterPropertiesSet();
		return writer;
	}
	

    //实现分类
    
	@Bean
	
	public ClassifierCompositeItemWriter<Customer> multiFileItemWriter() throws Exception
	{
		ClassifierCompositeItemWriter<Customer> writer=new ClassifierCompositeItemWriter<Customer>();
		
		writer.setClassifier(new Classifier<Customer, ItemWriter<? super Customer>>() {
			@Override
			public ItemWriter<? super Customer> classify(Customer customer) {
				ItemWriter<Customer> write=null;
				try {
					write=customer.getId()%2==0?jsonFileWriter():xmlFileWriter();
				} catch (Exception e) {
					e.printStackTrace();
				} 
				return write;
			}
		});
		return writer;
	}
```


## 第六节 ItemProcessor的使用

ItemProcessor<I,O>用于处理业务逻辑，验证，过滤等功能

CompositeItemProcessor

案例：从数据库中读取数据，然后对数据进行处理，最后输出到普通文件

    
```
@Bean
    
	public CompositeItemProcessor<Customer,Customer> process()
	{
		CompositeItemProcessor<Customer,Customer> processor=new CompositeItemProcessor<>();
		
		List<ItemProcessor<Customer,Customer>> delegates=new ArrayList<>();
		delegates.add(firstNameUpperProcessor);
		delegates.add(idFilterProcessor);
		processor.setDelegates(delegates);
		
		return processor;
	}
```

	
# 第五章 错误处理

## 第一节 错误处理概述

默认情况下当任务出现异常时,SpringBatch会
结束任务，当使相同的参数重启任务时，SpringBatch会去执行未执行的剩余任务.

    
```
@Bean
    
    @StepScope
    
	public Tasklet errorHandling() {
		return new Tasklet() {
			@Override
			public RepeatStatus execute(StepContribution contribution, ChunkContext chunkContext) throws Exception {
				Map<String, Object> stepExecutionContext = chunkContext.getStepContext().getStepExecutionContext();
				if(stepExecutionContext.containsKey("qianfeng")){
					System.out.println("The second run will success");
					return RepeatStatus.FINISHED;
				}
				else{
					System.out.println("The first run will fail");
					chunkContext.getStepContext().getStepExecutionContext().put("qianfeng", true);
					throw new RuntimeException("error ...");
				}
			}
		};
	}
```

	
## 第二节 错误重试(Retry)

    
```
@Bean
    
    public Step retryDemoStep() {
        return stepBuilderFactory.get("retryDemoStep")
                .<String,String>chunk(10)
                .reader(reader())
                .processor(retryItemProcessor)
                .writer(retryItemWriter)
                .faultTolerant()
                .retry(CustomRetryException.class)
                .retryLimit(10)
                .build();
    }
```

    
    
## 第三节 错误跳过(Skip)

```
@Bean
    
    public Step skipDemoStep() {
        return stepBuilderFactory.get("skipDemoStep")
                .<String,String>chunk(10)
                .reader(reader())
                .processor(skipItemProcessor)
                .writer(skipItemWriter)
                .faultTolerant()
                .skip(CustomRetryException.class)
                .skipLimit(10)
                .build();
    }
```

    
## 第四节 错误跳过监听器(Skip Listener) 

    
```
@Bean
 
    public Step skipListenerDemoStep1() {
        return stepBuilderFactory.get("skipListenerDemoStep1")
                .<String,String>chunk(10)
                .reader(reader())
                .processor(skipItemProcessor)
                .writer(skipItemWriter)
                .faultTolerant()
                .skip(CustomRetryException.class)
                .skipLimit(10)
                .listener(mySkipListener)
                .build();
    }
```

    
   
```
@Component
public class MySkipListener implements SkipListener<String, String> {

    @Override
    public void onSkipInRead(Throwable t) {

    }

    @Override
    public void onSkipInWrite(String item, Throwable t) {

    }

    @Override
    public void onSkipInProcess(String item, Throwable t) {
        System.out.println(item + " occur exception：" + t);
    }
}
```


# 第六章 作业调度

## 第一节 JobLauncher的使用

控制任务什么时候启动


```
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-web</artifactId>
</dependency>
```


程序启动时不执行任务:


```
spring.batch.job.enabled=false
```



```
@RestController

@RequestMapping("/job")

public class JobLauncherController {
	
	@Autowired
	private JobLauncher jobLauncher;
	
	@Autowired
	private Job jobLauncherDemoJob;
	
	@GetMapping("/{job1}")
	public String job1Run(@PathVariable String job1) throws JobExecutionAlreadyRunningException, JobRestartException, JobInstanceAlreadyCompleteException, JobParametersInvalidException
	{
		System.out.println(job1);
		JobParameters jobParameters=new JobParametersBuilder()
				                    .addString("job1", job1)
				                    .toJobParameters();
		jobLauncher.run(jobLauncherDemoJob, jobParameters);
		
		return "job1 success";
	}
```

	

## 第二节 JobOperator的使用


```
@Configuration

public class JobOperatorDemo implements StepExecutionListener,ApplicationContextAware{

	@Autowired
	private JobBuilderFactory jobBuilderFactory;
	
	@Autowired
	private StepBuilderFactory stepBuilderFactory;

	
	private Map<String, JobParameter> parameter;
	
	@Autowired
    private JobLauncher jobLauncher;
	
	@Autowired
    private JobRepository jobRepository;
    
    @Autowired
    private JobExplorer jobExplorer;

    @Autowired
    private JobRegistry jobRegistry;
    
    private ApplicationContext context;
    
    
    @Bean
    public JobRegistryBeanPostProcessor jobRegistrar() throws Exception {
        JobRegistryBeanPostProcessor postProcessor = new JobRegistryBeanPostProcessor();

        postProcessor.setJobRegistry(jobRegistry);
        postProcessor.setBeanFactory(context.getAutowireCapableBeanFactory());
        postProcessor.afterPropertiesSet();

        return postProcessor;
    }

	
	@Bean
    public JobOperator jobOperator(){
        SimpleJobOperator operator = new SimpleJobOperator();

        operator.setJobLauncher(jobLauncher);
        operator.setJobParametersConverter(new DefaultJobParametersConverter());
        operator.setJobRepository(jobRepository);
        operator.setJobExplorer(jobExplorer);
        operator.setJobRegistry(jobRegistry);
        
        return operator;
    }
	
	
	@Bean
	public Job jobOperatorDemoJob()
	{
		return jobBuilderFactory.get("jobOperatorDemoJob")
			   .start(jobOperatorDemoStep())
			   .build();
	}

	@Bean
	public Step jobOperatorDemoStep() {
		return stepBuilderFactory.get("jobOperatorDemoStep")
	           .listener(this)
			   .tasklet(new Tasklet() {
				@Override
				public RepeatStatus execute(StepContribution arg0, ChunkContext arg1) throws Exception {
					System.out.println(parameter.get("msg").getValue());
					return RepeatStatus.FINISHED;
				}
			}).build();
	}
	
	

	@Override
	public void beforeStep(StepExecution stepExcution) {
		parameter = stepExcution.getJobParameters().getParameters();
		
	}

	@Override
	public ExitStatus afterStep(StepExecution arg0) {
		
		return null;
	}


	@Override
	public void setApplicationContext(ApplicationContext applicationContext) throws BeansException {
		this.context=applicationContext;
	}

}
```








