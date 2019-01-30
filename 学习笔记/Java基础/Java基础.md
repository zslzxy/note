[TOC]
### 一、面向对象


#### 1、面向对象的思想

> 面向对象是相对于面向过程而言的，面向过程，强调的是功能行为，是一种以过程为中心的编程思想。  
>
> 面向对象主要是将事物进行对象化，而对象包括了事物的属性与方法，是一种以对象为中心的编程思想。例如：正常的大象进冰箱而言，冰箱使一个类，类里面具有开门、关门的两个方法；大象是一个类，类里面有进入冰箱的方法。正常面向对象而言，就是先创建两个类的对象，在调用对象的方法来实现功能。   
>
> 面向对象更加符合人类日常生活逻辑的方法与原则，具有抽象、封装、继承、多态等。

---

#### 2、面向对象的三大特征
- **封装-Encapsulate**  

> 类中的属性的调用，不应该是 对象.属性 的方式进行调用，不符合程序“高内聚，低耦合”的特点，为了避免程序间的相互依赖产生冲突，一般是使用 对象.getXXX() 方式来对属性进行调用，控制属性的访问权限。  

- **继承-inheritance**    

> 在程序中，为了减少代码的重复性，将需要的类的重复的特征提取出来，放置在一个类里面，让其他类来继承该类，实现代码复用。  

- **多态-ploymorphism**  

> 一个对象，具有多种表现形态，比如重载、重写，子类对象的多态。比如：一个父类两个子类，那么创建父类对象就可以使用子类来创建---Person p = new Man();



---

#### 3、面向对象的六项基本法则
- **单一职责原则：**  
> 一个类只做该做的事情，专注于一项任务去执行，体现 **“高内聚”** 原则。

- **开闭原则：**   
> 对扩展开放，对修改关闭。程序提供接口或者抽象类，实现功能的扩展；将各个可变的因素放到继承体系中，实现代码的扩展性。

- **依赖倒转原则**   
> 面向接口编程

- **里氏替换原则**   
> 任何一个子类都可以替代父类。

- **接口隔离原则**    
> 接口要小而专，不能大而全。

- **合成聚合复用原则**  
> 优先使用聚合或者和复用代码。



---

#### 4、面向对象的一项基本法则：
- **迪米特法则**  
> 一个对象对其他对象尽可能的少依赖。专注于 **“低耦合”** 的特点。





### 二、类

#### 1、类的四大成员

- 属性 
- 方法
- 构造器
- 初始化块(代码块)
    -   静态代码块（static修饰）: 随着类的加载而加载，只能调用静态结构；
    -   非静态代码块（无修饰符）：代码块的执行早于构造器；  

代码块的代码：
```java
        private String name;
        private String email;
        //非静态代码块
        {
        name = "zsl";
        email = "1083837617@qq.com";
        }
        //静态代码块
        static{
        name : "lisi";
        email : "asdasd@qq.com";
        }
```
代码块注意事项：  
>  代码块只能被static修饰；    
 >   可以有输出语句；   
  >   一个类中可以有多个代码块，多个代码块之间按照顺序执行；



---


#### 2、类以及成员变量的修饰符

#### 1） 成员变量（属性）的修饰符：  
public ： 公共的意思，都能访问。  
private ： 私有的意思，只能自己访问，其他类不能访问，包括子类。  
protected ： 指定该变量只能被自己和子类访问，在子类中也可以覆盖这个变量。  
final ： 最终修饰符，指定该变量的值不能变。  
default/friendly ：默认是在包内可以访问。  
static ： 指定被所有对象共享。  


#### 2）类修饰符：  
public ： 将一个类声明为公共类，可以被其他任何对象调用。（一个程序的主类必须是公共类）  
abstract ： 将一个类声明为抽象类。没有实现的方法，需要子类提供方法的实现。  
final ： 将一个类修饰为 不能被其他类继承  
friendly ： 默认修饰符，只能在相同包下才够被对象访问。  

#### 3）方法修饰符：
public ： 公共的。  
private ： 私有，只能自己访问  
protected ： 只能自己及子类访问   
final ： 指定这个方法补鞥能够被覆盖   
static ： 指定不需要实例化就可以激活某一个方法   
native ： java语言使用native的方法是来实现对底层的操作。  
#### 3、局部变量与成员变量
##### 局部变量的分类：
- 方法的形参中时局部变量。
- 方法里面声明的变量是局部变量。
- 非静态代码块中的变量是局部变量。

**注意：** 局部变量的存储位置是在栈中。

##### 成员变量的分类：
- 在类中又没有在方法中的变量是成员变量。
- 静态的成员变量是类变量（使用static修饰）。
- 非静态的成员变量是实例变量（无static修饰）。

**注意：** 类变量是存储在方法区中；实例变量是存储在堆中。

---
#### 3、重载(Overload)与重写(Override)
#### 1）重载
介绍： 在同一个类里面，和方法名相同，且方法的参数不同（参数的个数或者参数的类型）的类里面的方法就叫重载。
注意： 方法的重载有方法的返回值没有关系。
#### 2）重写
介绍： 在继承了父类以后，子类得到了父类的所有功能，在某些情况下父类的某一些功能不适用于子类，就可以将父类的某一些方法进行重写，方法名相同，内容进行修改。  

#### 4、类的初始化
##### 类的初始化过程：
- 一个类需要创建实例的话，需要加载与初始化该类。
    - 主方法main()所在的类需要先加载与初始化。
- 一个子类需要初始化，则需要先加载与初始化该类的父类。
- 一个类的初始化就是执行这个类的 <clinit>() 方法（该方法是编译器自动添加上去的）。
    - 一个类只有一个 <clinit>() 方法，每次初始化只执行一次。
    - <clinit>() 是由静态实例变量显式赋值代码和静态代码块组成，且是按照顺序执行。

#### 5、实例对象的初始化
##### 实例对象的初始化过程：
- 一个实例需要初始化，则对应的需要执行编译器自动添加的 <init>() 方法。
    - <init>()方法是由非静态实例变量显式赋值代码和非静态代码块组成。
    - 一个构造器就有一个对应的 init() 方法，每次调用不同的构造器，就执行不同的 init() 方法。


### 三、I/O流

#### 1、简介：

#### 1）介绍： 
I/O流就是设备（键盘与内存等）与设备之间的数据传输。

##### 2）分类：
   输入流 ： input   从文件中入取数据到内存中  

   输出流 ： output   从内存中将数据写入到文件中  

   标准输入流： System.in     是一个已经与键盘绑定好了的InputStream类型的流对象，接收键盘输入的数据 

- **按照方向分：** 

    - 输入流
    - 输出流

- **按照操作的数据分类：**

    - **字节流：** 操作数据可以为文本，音频，视频，图片  
字节输入流的父类 ： InputStream  
字节输出流的父类 ： OutputStream     

    - **字符流：** 操作的数据只能是文本（在字节流的基础之上融入了ASKLL编码）  
字符输入流的父类 ： Reader（以内存为对象，从文件中读数据到内存里面）  
字符输出流的父类 ： Writer  （以内存为对象，将内存中的数据写到文件中）

### 四、正则表达式 ： 

**使用 matches 方法来进行字符串验证。**

#### 1、简介： 
    专门正对字符串的数据进行验证。
#### 2.使用  matches  方法校验
```
String regex = "[0-9][0-9]{1,5}";
String line = "123";
//然后使用  boolean matches(String regex)  方法进行校验：
boolean bo = line.matches(regex);
```

#### 3.  matches 与 replace 的区别
- matches --- 方法是来判断两个字符串是否符合正则表达式规则。   

- replace --- 方法是来替换符合正则表达式的字符串的内容。

#### 4、字符串切割 ： 

**split方法-使用正则表达式来进行切割。**

**简介：**    
将一个字符串使用正则表达式定义的规则进行切割，使用的方法是：String[] split (String regex);

**代码：**  
```
String line = "zsl,jack,tom";      //使用正则表达式按照逗号进行切割，截取出三个名字
String regex = ",";
String[] arr = line.split(regex);
```


### 五、反射 ：reflect 

#### 1、介绍 ： 
动态获取类（也就是字节文件 : Person.class），并对其进行运行。

#### 2、特点 ：
反射机制允许程序在执行期间借助于Reflection API 任何类的内部信息，并直接调用任意对象的内部睡醒与方法。

#### 3、反射机制提供的功能：
- 在运行时判断任意一个对象所属的类
- 在运行时构造任意一个类对象
- 在运行是判断任意一个类所具有的成员方法与变量
- 在运行时调用任意一个对象的成员变量与方法
- 生成动态代理

### 4、字节码文件： 

**java.lang.Class<T> ---- 反射的源头**。

**1）简介：  **
创建一个类，通过编译(javac.exe)，生成对应的 .class文件，之后我们使用 java.exe加载(JVM类加载器)此 .class 文件，此 .class 文件加载到内存以后，就是一个运行时类，存放在缓存区.而这个.class在内存中就相当于 Class 的实例。  

**2）注意 ：每个运行时类只加载一次。**  

**3）得到字节码文件Class以后，可以执行的操作：**
- 创建运行时类的对象；
- 获取对应的运行时类的完整结构（方法、属性、构造器、父类、异常、内部类等）
- 调用对应大的运行时类的指定结构（方法、属性、构造器）
- 动态代理。


### 5、获取Class对象：
**方法1：调用运行时类本身的 .class 属性。**  

```
Class clazz1 = Person.class;
```

**方法2：通过运行时类的对象获取。**

```
Person p = new Person();
Class clazz2 = p.getClass();
```

**方法3：通过Class的静态方法获取(通过 包+类名 的方式来指定类) 。**

```
Class clazz3 = Class.forName("com.zsl.pojo.Person");
```

**方式4：通过类加载器获取。**

```
ClassLoader classLoader = this.getClass().getClassLoader();
Class clazz4 = classLoader.loadClass("com.zsl.pojo.person");
```


### 6、调用Class，创建运行时类对象：
**方法： 调用 Class 的 newInstance() 方法------使用无参构造器**

```
Object obj = class.newInstance();
```

**注意：** 这个方法是调用无参构造器-----所以无参构造器的权限需要足够。  


### 7、调用Class创建的对象得到其属性，方法
#### 1）获取运行时类的属性：
**方法1：获取运行时类本身所有属性**  

```
Class class1 = Person.class;
Field[] filed = class1.getDeclaredFields();  //所有属性存到了 Field数组 里面
```

**方法2：获取运行时类及其父类的所有生命为public的属性**

```
Field[] field = class1.getFields();  //所有属性存到了 Field数组 里面
```

**获取属性的各种信息(属性的权限修饰符，变量类型，变量名)：**

```
Field[] filed = class1.getDeclaredFields();
for(Field ss : field){
// 1）获取属性的权限修饰符
String str = ss.getModifiers().toString();
// 2）获取属性的类型
String str1 = ss.getType();
}
```


#### 2）获取运行时类的方法：
**方法1：获取运行时类本身与其父类所有的生命为public方法：**

```
Class class1 = Person.class;
Method[] mth = class1.getMethods();
```

**方法2：获取运行时类本身的所有方法:**

```
Method[] mth = class1.getDeclaredMethods();
获取方法的各种信息(属性的权限修饰符，返回类型，形参等)：
Method[] mth = class1.getDeclaredMethods();
for(Method ss : field){
// 1）获取方法的权限修饰符
String str = ss.getModifiers().toString();
// 2）获取形参
Class[] str1 = ss.getParameterTypes();
for(Class str2 : str1){  String name = str2.getName();  }
// 3）获取返回值类型
String sls = ss.getReturnType().toString();
}
```


#### 3）获取运行时类的构造器：

```
Class class1 = Person.class;
Constructor[] cst = class1.getDeclaredConstructors();
```

### 8、调用运行时类的指定的属性：

**调用运行时类的指定的属性：**

```
Class class1 = Person.class;
Person p = class1.newInstance();
//  Field name = class1.getField("name");  //获得Person的name属性（必须为public的权限）
Field name = class1.getDeclaredField("name");  //获得Person的name属性（无权限限制）
name.setAccessible(true);  //将这个属性设置为可以操作（因为属性是私有的，必须赋予权限）
name.set(p,"张世林");  //为name属性赋值
```


**调用运行时类的指定的方法：**

```
Class class1 = Person.class;
Person p = class1.newInstance();
Method met = class1.getMethod("show");  //指定到Person的show方法
Object retnruval = met.invoke(p);  //返回这个方法的值
```


### 六、序列化
#### 1、简介：
将一个pojo序列化。也就是说将一个对象转换为一种可以保持传输的顺序的流的操作。
#### 2、使用方式： 

```
public class Person implements Serializable{.......}
```

#### 3、特点：
实现序列化接口以后，可以自定义序列化的UID，实现如果更改了pojo也不会导致之前的内容进行反序列化失败 ：   private static final long serialVersionUID = -42L;



### 七、进程与线程
#### 1、程序(program)：
完成特定任务、用某一种语言编写的一组指令的集合。即是一段静态的代码，静态对象。
#### 2、进程(process)：
正在进行中的程序，也就是在内存中开辟了一块空间。是一个程序对一个数据集的动态执行过程，是分配资源的基本单位。  

具有一定独立功能的程序关于某个数据集合上的一次运行活动，是操作系统进行资源分配和调度的一个独立单位；
#### 3、线程(thread)：
负责程序执行的一条执行路径，也成为一个执行单元，或者也可以成为一条线路。线程是在进程中被创建，所以不会再另外开辟一个内存空间，而直接使用进程的内存空间。

线程是操作系统中能够进行运算调度的最小单位，它包含在进程中，是进程中的实际运作单位。程序能够通过它来进行多任务的处理，使用多线程能够更好的利用CPU资源。

每一个独立的线程，都有一个程序运行的入口、顺序执行序列、和程序的出口。但是线程不能够独立的执行，必须依存在应用程序中，由应用程序提供多个线程执行控制。
#### 4、进程与线程的关系：
在常见的应用场景中，一个程序的启动，就会创建一个进程。而java启动一个程序，就会创建一个JVM进程。而Java程序可能需要多线程执行，就会在这个进程中创建多个线程来实现程序的多线程运行。  
一个进程至少会有一个线程。  
一个进程有多个线程的时候，就是叫做多线程程序。
#### 5、多线程：
实现不同的功能同时执行,多线程不一定能够提高效率，但是能够合理的使用cup资源。
#### 6、多线程执行方式：
每个线程都是一条线，当这个线程抢到cup资源以后，将执行这个线程的代码，然后当再执行当中的某一个过程，cpu资源被抢夺，然后就会停止执行代码，待又抢到了cpu资源以后，继续执行代码，直到这个线程执行完毕。

#### 7、多线程的创建(总共有四种方式)
**方法1：**   
定义一个类，继承 Thread 类，重写 run() 方法，然后在main函数里面创建对象，使用对象来启动多线程。

代码如下：

```
public class Person extends Thread{
    public void run(){  
        for(int i=100;i<)
        {
            System.out.println("asdas"+i); 
        }
    }
}

public class TT{
    main(){
        Person person = new Person();
        person.start();
    }
}
```

**方法2(主要方式)：**   
定义一个类，实现Runnable接口，重写run()方法，然后在主函数创建 类的对象，再创建Thread的对象并封装 这个类的对象，然后调用start()方法。  

代码如下：  

```
public class Person implements Runnable{
    public void run(){  
        for(int i=100;i<)
        {
            System.out.println("asdas"+i); 
        }
    }
}
   
public class TT{
    main(){
        Person person = new person;
        Thread thread = new Thread(person);
        thread.start();
    }
}
```

**方法3：实现Callable接口的方式。**     

特点：相较于实现Runable接口的方式，其方法可以有返回值，可以抛出异常。  执行Callable方式：需要Future接口下的实现类FutureTask类的支持。   

优点：在主线程需要执行大量的串行运算的时候，将串行运算的内容提交给副线程去执行，而主线程继续去执行其他的操作，到最后使用 get() 方法来将副线程的返回值获取。  

代码如下：

```
/**
 * 创建线程的第三种方式：实现Callable接口
 * 一、Callable接口与runable接口的不同就是Callable能够有返回值，能够抛出异常
 * 二、执行Callable方式：需要Future接口下的实现类FutureTask类的支持。
 */
public class TestCallable {
	public static void main(String[] args) {
		//创建线程
		ThreadDemo1 td = new ThreadDemo1();
		//创建FutureTask实现类来执行多线程
		FutureTask ft = new FutureTask<>(td);
		//使用Thread来启动多线程
		Thread thread = new Thread(ft);
		thread.start();
		System.out.println("测试测试");
		//得到线程运算以后的结果,会在副线程的运算结果结束以后才能够执行
		try {
			System.out.println(ft.get());
		} catch (InterruptedException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		} catch (ExecutionException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
	}
}
class ThreadDemo1 implements Callable<Integer>{
	@Override
	public Integer call() throws Exception {
		Integer sum = 0;
		for (int i = 0;i <100;i++)
		{
			sum = sum + i;
			System.out.println(i);
		}
		return sum;
	}	
}
```


**方法4：线程池**  
线程池--使用线程池来创建以管理线程。  

代码如下：

```
public class TestThreadPool {
	
	public static void main(String[] args) {
		//创建一个固定大小的线程池
		ExecutorService pool = Executors.newFixedThreadPool(5);
		
		ThreadPoolDemo demo = new ThreadPoolDemo();
		
		//为线程池分配任务
		for(int i = 0;i < 10;i++)
		{
			pool.submit(demo);
		}
		
		//关闭线程池:这个是柔和的关闭线程池，执行这条语句以后，不会继续
		//		接收新的任务，待到以前的任务执行完毕以后，就能够关闭线程。
		pool.shutdown();
	}

}

class ThreadPoolDemo implements Runnable{

	@Override
	public void run() {
		System.out.println(Thread.currentThread().getName());
	}
	
}
```

##### 线程的run()与start()方法的区别
start()方法是用来启动线程的，真正实现了多线程运行；会启动其他线程来执行其他多线程的代码。

run()方法是类的普通方法，如果直接调用run()方法的话，程序依然只有主线程在执行，且是按照顺序进行执行代码，当run()方法里面的代码执行完毕以后，才能够继续向下执行。


#### 8、多线程的优点
- 1）提高程序的响应

- 2）提高CPU的利用率

- 3）改善程序结构，将复杂且长的进程分为多个线程


#### 9、线程的生命周期
- 1）新建 ： 创建了对象

- 2）就绪 ： 新线程被start()以后

- 3）运行 ： 线程获得CUP资源以后，执行线程的操作与功能

- 4）阻塞 ： 因特殊原因导致被挂起等，暂且终止了线程的操作

- 5）死亡 ： 当线程工作完毕或者被强制性结束

![image](https://note.youdao.com/yws/api/personal/file/1CD381F13410400EB731B71C66DB76AB?method=download&shareKey=c65ec30f07d8b3146183e43988144a99) 



#### 10、多线程安全(相当于添加锁)

**多线程的安全问题：**  一个共享数据同时被多个线程操作的时候，容易导致数据的异常  

**解决方案 ：**   让一个线程执行完毕以后才能够让其他的线程来操作共享数据

**java解决线程安全的方式：线程同步机制**  

**方式1 ： 同步代码块(synchronized)**

```
synchronized(同步监视器){
//需要被同步的代码块（即为操作共享数据的代码）
}
//1、共享数据：多个线程共同操作的同一个数据
//2、同步监视器：由一个类来充当，哪一个线程获取此监视器，谁就执行同步代码块的代码
//3、可以在类里面随便制造一个对象（不能够在run()方法中定义对象）
```


**方式2 ： 同步方法**    
将操作共享数据的方法声明为同步(synchronized)方法，保证此方法在由线程执行的时候，其他线程不能够进入此方法。

```
public synchronized void show(){
    if(ticket > 0){
    System.out.println("这个票还剩："+(ticket-1));
    }
}
```

**方法3：**  
Lock 同步锁---是一个显式锁，是一种更加灵活的方式，需要通过Lock方法进行上锁，通过unlock()方法进行释放锁。


```
/*
 * 一、用于解决多线程安全问题的方式：
 * jdk 1.5 后：
 *   同步锁 Lock
 * 注意：是一个显示锁，需要通过 lock() 方法上锁，必须通过 unlock() 方法进行释放锁，
 * 		   一般unlock()方法需要放在finally中，保证其方法一直能够要执行成功。
 */
public class TestLock {
	
	public static void main(String[] args) {
		Ticket ticket = new Ticket();
		
		new Thread(ticket, "1号窗口").start();
		new Thread(ticket, "2号窗口").start();
		new Thread(ticket, "3号窗口").start();
	}
}
class Ticket implements Runnable{
	private int tick = 100;
	//创建Lock的实现类对象ReentrantLock来对票进行上锁
	private Lock lock = new ReentrantLock();
	@Override
	public void run() {
		while(true){
			lock.lock(); //上锁
			if ( tick > 0 ) {
				try{
					System.out.println(Thread.currentThread().getName() + " 完成售票，余票为：" + --tick);
				}finally{
					lock.unlock(); //释放锁
				}
			}
		}
	}
}
```


#### 11、线程的死锁

**解释：**   
不同的线程分别占用对方的同步资源不放弃，都在等待对方先放弃，线程线程的死锁。  

**解决办法：**  
- 1）加锁顺序，来对线程进行限制。

- 2）加锁时限，用时间限制来自动释放锁。

####   12、线程通信
**方法：**   

- wait()方法：令当前线程挂起并释放CUP、同步资源，是别的线程可以访问并修改共享资源，当前线程排队等候再次对资源的访问。  

- notify()方法：唤醒正在等待同步资源的县城中优先级最高者结束等待。  

- notifyAll()方法：唤醒正在等待资源的所有县城结束等待。  

注意：这三个方法只能在synchronized方法或者同步代码块中使用。



### 八、super，this关键字

#### 1、super特征 ： 
专门针对父类

#### 2、super作用域：
用于修饰属性、方法、构造器

#### 3、super使用场景：
- 1）当子类中的属性与父类中的属性同名以后，如果需要使用父类的属性，可以使用super。

- 2）当子类重写了父类中的方法以后，仍然需要调用父类中被重写的方法，可以使用super。

- 3）super修饰构造器，通过在子类中使用  super("形参列表") 来显示的调用父类的构造器
    - a)   在子类构造器中使用  super()  构造器，表示调用父类的 无参构造器  
    - b)   在子类构造器中使用  super(“张世林”,23)  ，通过参数来对应到父类的对应的构造器

#### 4、super注意：
- 1）super在构造器内部，super关键字必须要放在首行

- 2）在构造器内部，this关键字与super关键字必须只能二选一   

- 3）在构造器不显示的调用父类的构造器的话，默认调用父类的空参构造器

#### 5、this特征 ： 
专门针对于类本身

#### 6、this作用域：
用于修饰属性、方法、关键字

#### 7、this使用场景：
- 1）在方法中使用this来修饰，this表示的是这个类，然后 this.show() 表示使用这个类里面的show()方法；

- 2） this表示这个类，this.name 表示这个类的属性name，用于区分形参name与类的属性。

- 3）this(name)  表示用于调用这个类里面的形参为 name 的构造器。



### 九、static与final关键字

#### 1、static特征：
静态的，随着类的加载而加载，且独一份；记载的类的静态变量在类的声明之前。

#### 2、static作用域：
属性、方法、代码块、内部类。

#### 3、static来修饰属性的内存图：
static修饰的变量会放在静态域，创建对象以后，会直接将属性找到对应的静态域的值。多个对象的调用，都是使用这一个静态域的值。

#### 4、static来修饰方法：
在内存中独一份，可以通过 类.类方法（Person.show()） 的方式来调用。

#### 5、final特征：
最终的，不可变的。

#### 6、final作用域：
修饰类、属性、方法，final也能够修饰形参，如果修饰的是引用类型的形参，还能够改引用类型的类里面的属性。

#### 7、final作用场景： 
- final修饰类，不能够被继承
- final修饰方法，表示这个方法不能够被重写
- final修饰属性，此属性就是一个常量

#### 8、常量的赋值：
- 在定义常量的时候，必须要附初始值 :  final int A = 12;
- 常量只能够显式的赋值(代码块，构造器);
- 常量一旦初始化以后，不能够再被赋值;


### 十、内部类、枚举类

#### 1、定义：
在类的方法里面再定义一个或者多个类。

#### 2、内部类的分类：
**1）成员内部类：** 在一个类的里面，方法的外面定义的类。具有如下特征：   
- 是外部类里面的成员，可以有修饰符修饰( public、protected、default、private)。、
- 因为是一个类的成员，所以可以使用 static、final 关键字修饰。
- 具有类的特点，可以是abstract类型的类。
- 在内部类里面可以使用构造器、方法、属性等。

**2）局部内部类：**
声明在类的里面，方法的外面的类。


#### 3、内部类掌握三点：
- 如何创建成员内部类对象。
- 如何区分内部类、外部类的变量(外部类与内部类属性重名)。
- 局部内部类的使用。

#### 4、成员内部类的使用：
成员内部类里面能够使用外部类的方法。  

创建静态内部类对象--直接 外部类.内部类  

```
Person.Cat cat = new Person.Cat();
```

创建非静态内部类对象--先创建外部类对象，在创建内部类对象：

```
Person person = new Person();
Person.Brid brid = person.new Brid();
```

#### 5、内部类调用外部类的属性：
方法---外部类.this.属性：

```
name = Person.this.name;
```


#### 6、代码如下：

```java
public class TestInner {
	
	public static void main(String[] args) {
		
		//创建静态内部类的对象：直接使用 外部类.内部类 来指定对应的静态内部类
		Person.Cat cat = new Person.Cat();
		
		//创建非静态的内部类对象：先创建外部类对象，在使用外部类对象来创建内部类对象
		Person person = new Person();
		Person.Brid brid = person.new Brid();
	}
}

class Person {
	String name;
	int age;
	
	public void show()
	{
		System.out.println("外部类的show方法");
	}
	
	//创建一个普通的成员内部类
	class Brid {
		
		String name;
		
		public Brid(){	
		}
		
		//内部类可以调用外部类的方法
		public void test()
		{
			show();
		}
	}
	
	//出构建一个静态内部类
	static class Cat{
		
		String name;
		
		public Cat() {
		}
	}
}
```

#### 7、局部内部类的使用：
**1）局部匿名内部类的创建**  

```java
class Person2 {
	String name;
	//局部匿名内部类
	public Comparable getc() {
		
		return new Comparable() {

			@Override
			public int compareTo(Object o) {
				// TODO Auto-generated method stub
				return 0;
			}
		};
	}
}
```


**2）局部内部类的创建**

```java
class Person1 {
	
	String name;
	//局部内部类
	public Comparable getc() {
		class Mycomp implements Comparable {
			@Override
			public int compareTo(Object o) {
				return 0;
			}	
		}
		return new Mycomp();
	}
}
```

#### 8、枚举类

​	枚举类常用的方法为：

- ###### CourierEnum.values()  --- 返回具有指定类型的枚举类型的常量的数组。

- ###### CourierEnum.HT.ordinal() --- 得到对应的这个枚举类型的常量的 下标（从0开始）。

- ###### CourierEnum.HT.toString()  --- 得到对应的这个常量的值。

```java
public enum  CourierEnum {

	ZT("ZT","中通快递"),
	YT("YT","圆通快递"),
	SF("SF","顺丰快递"),
	YD("YD","韵达快递"),
	YZ("YZ","邮政快递"),
	HT("HT","汇通快递");

	private String code;

	private String msg;

	private CourierEnum(String code, String msg) {
		this.code = code;
		this.msg = msg;
	}

	/**
	 * 根据 code编码获取对应的Enum
	 * @param code
	 * @return
	 */
	public static CourierEnum getCourierEnumByCode(String code) {
		// CourierEnum.values() 得到的是 枚举类的 value
		for (CourierEnum courierEnum : CourierEnum.values()) {
			if (courierEnum.code.equals(code)) {
				return courierEnum;
			}
		}
		return null;
	}

	/**
	 * 根据 msg过去对应的Enum
	 * @param msg
	 * @return
	 */
	public static CourierEnum getCourierEnumByMsg(String msg) {
		// CourierEnum.values() 得到的是 枚举类的 value
		for (CourierEnum courierEnum : CourierEnum.values()) {
			if (courierEnum.msg.equals(msg)) {
				return courierEnum;
			}
		}
		return null;
	}

    /**
    * 得到编码
    */
	public String getCode() {
		return code;
	}

	public void setCode(String code) {
		this.code = code;
	}

    /**
    * 得到描述
    */
	public String getMsg() {
		return msg;
	}

	public void setMsg(String msg) {
		this.msg = msg;
	}
}

class Tes {
	public static void main(String[] args) {
		String yt = CourierEnum.getCourierEnumByCode("YT").getCode();
		String yt1 = CourierEnum.getCourierEnumByMsg("圆通快递").getMsg();
		System.out.println(yt1);
	}
}

```



### 十一、抽象类与接口

#### 1、抽象类 (abstract class) :
将一个父类设计的非常抽象，以至于他没有具体的实例，叫做抽象类。

#### 2、abstract作用域：   
**修饰类**：抽象类 -------- abstract class Person{......}    

**注意：**   
- 1）抽象类不能够被实例化（不能够被new出一个对象来）
- 2）可以拥有构造器
- 3）抽象类里面可以有抽象方法，也可以有具体的已经实现了的方法

**修饰方法**：抽象方法---------   public abstract void eat();   

**注意：**
- 1）抽象方法没有方法体，只有方法名
- 2）抽象方法没有方法体，具体的实现是由子类来重写。
- 
**注意：** 抽象方法所在的类，一定是抽象类。

#### 3、接口(interface) ：
接口类里面只有 常量(public static final修饰)与 抽象方法(public abstract修饰)

#### 4、接口用处：
当多各类需要派生出一个子类来得到他们的所有的属性与方法，但是java不支持多继承，所以接口来实现多继承的效果。

**注意：** 
- 1)接口里面的常量是自动由 public static final来修饰的：故：
   public static final int A = 12;   可省略写成   int A = 12;
- 2)接口里面的抽象方法是由 public abstracct 修饰的，故：
   public abstract String test();  可以省略为  public String test();或者 String test();

#### 5、接口类的定义： 

```
public interface PersonService{
//常量
int IO1 = 12;
//接口里面的方法：
public String test();
}
```

#### 6、接口的实现：

```
public class Test implements PersonService{
//接口里面来实现所有的方法
}
```

**注意：**
- 1）接口的实现必须全部实现所有功能
- 2）接口可以多实现，一个类可以实现多个接口，两个接口类使用逗号分开
- 3）一个类可以继承与实现同时存在，先继承父类，在实现接口
- 4）接口与接口之间可以继承，而且可以是多继承的关系




### 十二、异常处理 Throwable(抓抛模型)
#### 1、java程序异常事件分为两大类：
**1、Error  ：**    
java虚拟机无法解决的严重错误。如JVM内部出现错误，资源耗尽等  
比如：  
栈空间资源溢出错误： main函数里面写 main(args);-----会声明无数个args，
报错：java.lang.StackOverflowError     
堆空间资源溢出错误 ： 定义字节数超过了堆空间的内存，报错
：java.lang.OutOfMemoryError   

#### 2、Exception ：   
因其他编程错误或偶然的外部因素导致的普通的问题，可使用针对性的代码处理异常  

#### 3、Exception异常分类：   

**编译时异常：** 在编译期间，当执行javac.exe命令时出现异常     
异常类型有：
- SQLException , IOException , ClassNotFoundException..........   
- 
**运行时异常：** 在运行期间，当执行java.exe命令时出现异常   
异常类型有：
- 运行时异常(RuntimeException)以及运行时异常下面的子类     
- 常见的异常：数组下标越界：ArrayIndexOutOfBoundsException     
- 算数异常 ：ArithmeticException   
- 类型转换异常：ClassCastException   
- 空指针异常：NullPointerException    

#### 4、异常处理方式:自动抛出，手动抛出
**自动抛出异常：**

```
try-catch-finally方式
try {
   //可能出现异常的代码
} catch(exception1 e1) {
   //出现异常的处理方式
} finally {
   //一定要执行的代码
}
```

**注意：**    
- catch语句可以多次出现，如果try抛出异常，那么会在catch里面找到最适合异常的异常处理catch，执行catch里面的代码。
- 当出现异常被处理以后，程序仍然能够向下执行，不会出现程序终止。  

**在方法的声明出，显示的抛出对象的异常：将编译时异常转换为运行时异常**  

方式 ： 

```
public void fileWrite() throws IOException,FileNotFoundException{.......}
```

**手动抛出异常:**  

方式 ：   

```
throw new  RuntimeException("输入有误，请重新输入");
```

注意：手动抛出异常以后，相当于return，将必须执行的代码执行完毕以后，执行throw的代码

#### 5、自定义异常： 

```java
public class MyException extends RuntimeException{

}
```

#### 6、处理异常的几种方式

<http://www.cnblogs.com/sunshineweb/p/7656463.html>

- 情况一：在try--catch中有return，但是finally中没有return，且finally中没有try和catch需要进行操作的代码。

  ​	结果：程序会直接执行try或者catch中的return代码。

```java
public class Test {
    public static int num=1;
    public static void main(String[] args) throws ParseException {
        int result;
        result = num();
        System.out.println(result);
    }
    private static int num() {
        try{
            int b=4/0;
            return num = num+2;
        }catch(Exception e){
            return num = num+3;
        }finally {
        System.out.println("不管你怎么样，我都是要执行");    
        }        
    }    
}
```

- 情况二：**针对基本数据类型**，在try和catch中有return，finally中没有return，但finally中有对try或catch中要 return数据进行操作的代码。

  ​	结果：返回基本数据类型，finally中对这个方法的返回值没有影响。

```java
public class Test {
    public static int num=1;
    public static void main(String[] args){
        int result;
        result = num();
        System.out.println(result);//结果不受finally影响，输出4
        System.out.println(num);//5
    }
    private static int num() {
        try{
            int b=4/0;
            return num = num+2;
        }catch(Exception e){
            return num = num+3;
        }finally {
            ++num;
        }        
    }    
}
```

- 情况三：**针对引用数据类型**，在try和catch中有return，finally中没有return，但finally中有对try或catch中要 return数据进行操作的代码。

​	结果：返回基本数据类型，finally中对这个方法的返回值有影响。

```java
public class Test {
    public static void main(String[] args) {
        People bride;
        bride = marry();
        System.out.println(bride.getState());//结果受finally影响，输出dead
    }
    private static People marry() {
        People people=new People();
        people.setState("happy");;
        try{
            int b=4/0;
            return people;
        }catch(Exception e){
            return people;
        }finally {
            people.setState("dead");
        }        
    }    
}
```

- 情况四：在try和catch中有return，finally中也有return

  ​		结果：try或catch中return后面的代码会执行，但最终返回的结果为finally中return的值，需要注意的是try或catch中return后面的代码会执行，只是存起来了，并没有返回，让finally捷足先登先返回了。

  ​		简单理解：首先会执行 try、catch中的return中的代码，然后将其结果暂存；接下来执行finally中的代码，然后直接返回finally中的return。

```java
public class Test {
    public static int num=1;
    public static void main(String[] args) throws ParseException {
        int result;
        result = num();
        System.out.println(result);//输出结果为1003
        System.out.println(num);//输出结果为1001
    }
    private static int num() {
        try{
            int b=4/0;
            return num = num+1000;
        }catch(Exception e){
            return num = num+1000;
        }finally {
            return num+2;
        }        
    }    
}
```

- 情况五：在try中有return，在catch中新抛出异常，finally中有return

  ​	结果：如果catch块中捕获了异常, 并且在catch块中将该异常throw给上级调用者进行处理, 但finally中有return, 那么catch块中的throw就失效了, 上级方法调用者是捕获不到异常。

```java
public class Test {
    public static void main(String[] args) throws Exception {
        String result="";
        try {
            result = num();
        } catch (Exception e) {
            System.out.println("青天大老爷在此");
        }
        System.out.println(result);
    }
    public static String num() throws Exception {
        try{
            int b=4/0;
            return "总是异常，反正我又不会执行";
        }catch(Exception e){
            throw new Exception();
        }finally {
            return "用金钱蒙蔽你的双眼";
        }    
    }    
}
```



### 十三、字符串操作方法

#### 1、String字符串

**1）特点：**  
创建字符串，是将字符串保存在字符串常量池(在方法区中)中。修改String类型的数据的时候，会创建新的String对象来保存数据，再改变之前对象的引用，来实现字符串的修改。   

**2）String name = "zsl"; 内存模型：**   
直接在字符串常量池(在方法区中)中创建一个字符串常量，然后栈里面的对象存的是字符串常量池的字符串地址。  
 ![image](https://note.youdao.com/yws/api/personal/file/A5EC4CF4F9CC4D4D977BAD4480B907CD?method=download&shareKey=eadc1d705d999232cf8e5c7636992809)


**3）String name = new String("zsl"); 内存模型：**    
常量池中如果之前存在 "zsl" 常量，则不创建常量；否则创建常量。  

堆中开辟了一个地址空间 new String()， 存储的是常量池中的字符串常量 "zsl" 的地址;   

在栈里面存储的是name这个属性，存的数据是堆中 new String() 分配的地址。  

![image](https://note.youdao.com/yws/api/personal/file/B3D488598EA041FB9764B6EDFD2B47E0?method=download&shareKey=7ff511434aa34970f0cd8458b307a6bb)


**4）String类的操作：**  
- public String length()    ------获取字符串的长度  如 ： String lien="abc";  int length = line.length();
- public char cahrAt(int index)  ----获取字符串中的某一个位置的字符  
- public boolean equals(Object obj)   ---比较两个字符串是否相等，返回值为true或者false
- public int compareTo(String lien)   ----比较两个字符串的大小，返回值为int
- public int indexOf(String s)   ----在一个字符串中查找另外 s字符串 的第一次出现的位置
- public int indexOf(String s,int i)  ---在一个字符串的 i 位置开始查找 s字符串 最后一次出现的位置
- public int lastIndexOf(String s)   ----在一个字符串中查找另外 s字符串 的第一次出现的位置
- public int lastIndexOf(String s,int i)  ---在一个字符串的 i 位置开始查找 s字符串 最后一次出现的位置
- public boolean startswith(String prefix)  ---判断字符串是否是以 prefix 字符串开始的
- public boolean endswith(String surfix)   ---判断字符串是够是以surfix字符串结束的

- public String substring(int startpoint)   ---从startpoint位置开始一直到末尾，截取这个字符串并返回
- public String substring(itn start,int end)  ---从字符串的start位置开始，到end位置结束的字符串截取
- public String replace(char oldahr,char newchar)  ---替换旧的字符为新的字符
- public String replaceAll(String old,String new)   ---字符串替换，将old串替换为new串
- public String trim()   ---去掉某一个字符串的首位两端的空格
- public String concat(String str)   ---连接当前字符串，将str接在当前字符串后面
- public String[] split(String str)   ---根据字符串str来进行餐粉当前字符串，将字符串转换为字符串数组  


**5）将字符串转换为基本数据类型：**  

```
String ss = "123";     
int k = Integer.parseInt(ss);
```

**6）将基本数据类型、包装类转换为字符串**  

方法 1) 

```
String ss = 123 + "";   
//直接加双引号，将数据转换为字符串
```

方法 2) 

```
String ss  = String.valueOf(123);  
//使用String.valueOf(Object object)就能转换为字符串
```


**字符串与字节数组的转换**  

字符串转换为字节数组 ：使用 getByets()  方法

```
String ss = "123";    
byte[] bt = ss.getBytes();
```

字节数组转换为字符串：使用字符串的构造器

```
String ss = new String(bt);
```

**字符串与字符数组的转换**   

字符串转换为字符数组：调用字符串的toCharArray()方法

```
String ss = "asdasd";
char[] cc = ss.toCharArray();
```

字符数组转换为字符串：调用字符串的构造器

```
String ss = new String(cc);
```


#### 2、StringBuffer类

**1）简介：**  

java.lang.StringBuffer，代表的是可变的字符序列，可对字符串内容进行增删，***线程安全***。  

**2）特点：**  

StringBuffer是可变的字符序列。当需要改变或者拼接字符串的时候可以使用。StringBuffer修改数据，不会创建像String一样新的对象，而是直接在原来的对象上进行修改或者追加内容。提高了效率，节省了内存空间的占用。  

**3）使用方法：**  
- 添加： append(Object obj) 方法，在现有的字符串后面添加字符串  

- 插入：insert(int index,String str)  方法  ----在index位置插入 str 字符串  

- 翻转：reverse()方法   ---将一个字符串翻转    用法:   String ss = sb.reverse();  

- 修改：setCharAt(int index,char ch)   ---将字符替换，更新

**4）具体操作： **

```java
StringBuffer sb = new StringBuffer();    
sb.append("abc").append("123");   //可以连续写入数据，且不会覆盖之前的内容
```


**5）注意：**  
StringBuffer的长度有16 个。



#### 3、StringBulider类
**简介：**  

java.lang.StringBulider，可变序列的字符串，对字符串进行增删，**线程不安全，*效率最高***。


### 十四、集合

#### 1、集合的关系图

![image](https://note.youdao.com/yws/api/personal/file/7DEC4AF8A7034C5D9BE0C1A07B4E5C57?method=download&shareKey=63a731bd05d8124e000cd8de1d3499b0)


#### 2、Collection与Map接口的比较

**1）Collection接口的实现类：**  
List接口--存放的是有序的、可重复的数据：ArrayList、LinkedList、Vector    

Set接口 --存放的是无序的、不可重复的数据：Hashset、LinkedHashSet、TreeSet

**2）Map接口的实现类：**  
存放的是具有映射关系 "key-value键值对" 的集合。  

实现类有：HashMap、LinkedHashMap、TreeMap、HashTable(以及hashTable的子类：properties)



#### 3、Collection接口：

**Collection的方法：**  

- size() 方法---返回集合元素个数    Collection coll  = new ArrayList();    int num = coll.size();
- add(Object obj)---向集合添加元素   coll.add("帅哥");
- ifEmpty()----判断集合是否为空   coll.isEmpty();
- clear()---清空集合元素   coll.clear();  

- contains(Object obj)---判断集合是否包含obj这个元素   coll.contains("zsl");
- containsAll(Collection coll)---判断集合是否包含形参coll集合的所有元素
- retainAll(Collection coll)---将当前集合与形参coll集合求交集，并返回给当前集合；
- remove(Object obj)---删除一个元素
- removeAll(Collection coll)---删除当前集合中存在有形参coll元素的元素
- equals(Object obj)--判断两个集合是否相同
- toArray()---将当前集合转换为数组    



#### 4、Collection遍历  

**方法一：** iterator()---迭代器。   
**方法二：** 增强for循环，输出结合结果。


**1）迭代器输出集合的数据：**  

```
Iterator iterator = coll.Iterator();
while(iterator.hasNext()){
    System.out.println(iterator.next());
}
```

**增强for循环输出结合的数据（可以实现对集合、数组的遍历）:**  


```
for(Object it:coll){
    System.out.println(it);
}
```


#### 5、List接口----ArrayList实现类（主要的List实现类，线程不安全，存储有序）

**1）List的主要实现类：ArrayList **     

```
List list = new ArrayList();
```


**2）List的方法**   

- 1）add(Object obj)---向集合添加数据     list.add("zsl")------增  

- 2）add(int index,Object obj)---在集合的指定位置添加数据------插  

- 3）get(int index)---返回指定索引位置的元素------查  

- 4）set(int index,Object obj)---设置指定位置元素的值为obj------改  

- 5）remove(int index)---删除指定位置的元素-----删  

- 6）indexOf(Object obj)---返回obj元素首次出现的位置  

- 7）lastIndexOf(Object obj)---返回obj元素末次出现的位置  

- 8）subList(int start,int end)---返回从start位置开始，到end位置结束的子集合    



**3）ArrayList的三个构造函数：**   

- ArrayList() 构造一个初始容量为 10 的空列表。  

- ArrayList(Collection<? extends E> c) 构造一个包含指定 collection 的元素的列表，这些元素是按照该 collection 的迭代器返回它们的顺序排列的。 

- ArrayList(int initialCapacity) 构造一个具有初始容量的空列表。


**4）ArrayList的实现原理：**  

- 底层是以动态数组来实现的。初始创建时，默认是大小是10的数组，当超出10 的容量的时候，会自动进行扩容操作，增加当前容量的50%，并用System.arraycopy()方法来将数据复制到新的数组中(相当于创建一个新的数组，数组大小为之前数组的150%，然后将旧数据复制到新的数据中)。  

- ArrayList实现了序列化接口，支持序列化功能。  

- ArrayList是**线程不安全的**，尽量只是用在单线程中。多线程可以考虑java.util.concurrent包中的CopyWriteArrayList类。  

- ArrayList直接在末尾添加元素--add(e)的性能高。  
  ​    ArrayList按数组下标访问元素--get(i)/set(i,e)性能高。  
  ​    ArrayList按下标插入、删除等操作--add(i,k)，remove(i)，remove(e)，则需要使用System.arraycopy()来移动部分受到影响的数据，性能较差。  



#### 6、List接口---LinkedList实现类（主要用于频繁更改的集合） 

**1）简介：**  

LinkedList是List接口的实现类，是基于双向链表实现的。所以对数据的插入、删除、修改效率较高，但是对数据的访问效率较低。  

**2）特点：**  

- LinkedList 是非同步的，也即是线程不安全的。

- LinkedList 实现 List 接口，能对它进行队列操作。

- LinkedList 实现 Deque 接口，即能将LinkedList当作双端队列使用。

- LinkedList 实现了Cloneable接口，即覆盖了函数clone()，能克隆。

- LinkedList 实现java.io.Serializable接口，LinkedList支持序列化，能通过序列化去传输。

- LinkedList 是一个继承于AbstractSequentialList的双向链表。它也可以被当作堆栈、队列或双端队列进行操作。

![image](https://note.youdao.com/yws/api/personal/file/22460597B89F4ECDBEEB82A4EB2BD9E9?method=download&shareKey=de22d84b8dade3207204d803314a22ac)  


**3）LinkedList数据结构原理：**  

![image](https://note.youdao.com/yws/api/personal/file/D41ECB974CE04C70BF64D2B836D20D13?method=download&shareKey=cd90ac3cd7e01f718735c488abc48043)   


![image](https://note.youdao.com/yws/api/personal/file/F1C3DE023FC0421BB3FC3A897EB82BE4?method=download&shareKey=ff82efb45773c8db027a22afd5bacfe4)   



#### 7、Set接口---HashSet实现类（主要的Set实现类，存储无序，数据不可重复）

**1）特点：**  

set集合添加的数据，**是使用哈希算法来计算后随机存放的。线程不安全的。**  

**2）Set的主要实现类：HashSet**

```
HashSet set = new HashSet();
set.add("语文");
set.add("数学");
set.add("英语");
set.add("历史");
set.add("政治");
set.add("地理");
set.add("生物");
set.add("化学");
```

**3）Set的主要方法：**  
- add(Object obj)---向set集合添加数据。

- size() 方法---返回集合元素个数

- ......剩下方法与collection接口的方法相似。

**4）HashSet去重的判断：**  

HashSet是在HashMap的基础之上进行了改造。**不能够有重复的数据插入**。当使用add()方法的时候，会先使用hashCode方法来查看是否已经存在，如果存在，则继续调用equals()方法来判断是否存在相同的数据，如果存在，则返回true，数据不插入，如果不存在，则插入数据。  



#### 8、Set接口---LinkedHashSet实现类（增加了记录输入顺序的链表，数据无序不重复）
**特点：**  

使用链表维护了一个添加进如集合的顺序，当我们删改查的时候，可以使用这个存入的顺序链表来进行操作。（意思是存入的时候，会有一个链表来存储存入的顺序，但是存储地址仍然无序），提高了遍历速度，但是插入速度变慢（因为插入数据还要维护链表的关系）。

#### 9、Set接口---TreeSet实现类
**1）Set的实现类： TreeSet**  

```
Set set = new TreeSet();
```

**2）特点：**
- 向TreeSet添加的数据必须为同一类型的数据。

- 可以按照添加进入集合的元素指定的顺序遍历，如String、int类型都是按照从小到大遍历。
- 当添加的封装对象(pojo)时，必须实现CompareTo接口,重写CompareTo(Object o)方法来制定排序的顺序。

**3）方法：**  
方法大致与Collection相似。   


#### 10、Map接口

**特点：**   

- Map与Collection并列存在，具有保存具有映射关系的数据：key-Value  

- Map中的key与Value都可以是任何引用类型的数据
- Map中的key是使用Set来存放的，不能够重复
- Map中的Value是可以重复的  


**实现类：**   

HashMap  、   LinkedHashMap  、  TreeMap 、  Properties


#### 11、Map接口---HashMap实现类（主要的实现类）

**1）方法： Map map = new HashMap();**  
- 1）Object put(Object key,Object value)----向Map中添加数据  

- 2）int size()---返回一个map的大小     int size = map.size();
- 3）Object remove(Object key)---删除key这个元素   map.remove("aa");
- 4）void clear()---清空   map.clear()
- 5）boolean isEmpty()---判断是否为空 
- 6）Object get(Object key)---获取指定的key的值  
- 
**2）HashMap实现原理：**    

HashMap是基于哈希表的Map接口的非同步实现(HashMap是**线程不安全的，效率高**；hashtable与HashMap效果类似，但是hashtable是线程安全的，也是线程同步的)。HashMap存放的可以是null键与null值。  

![image](https://note.youdao.com/yws/api/personal/file/C54B17435FFA4735B7F86FDA200BB922?method=download&shareKey=6605ce5c2f771b47ed03e21b93baecf1)  

注意：上图横排表示数组table[],纵排表示哈希桶bucket(实际上是由多个Entry组成的链表，新加上的Entry放在链头，后加上的放在链尾)。   

![image](https://note.youdao.com/yws/api/personal/file/A0878837DBA4425EA870A8608DC3758D?method=download&shareKey=be806053f98d107ed6f893e9bb6596e9)   

**3）特点：**  

- HashMap底层是以数组的方式进行存储的。数组默认大小是16个。将key-value作为数组中的一个元素进行存储。  

- key-value都是Map.Entry中的属性，将其key的值进行hashcode算法之后进行存储（每一个hash值都有一个数组下标，数组下标是根据hash值和数组长度来计算的）。  

- 由于不同的key可能有相同的hash值，即该位置在数组中的元素出现多个，那么HashMap就采用链表的方式进行存储。  


#### 12、Map遍历
**方法：**    

**1）获得Map所有的key : Set keySet()方法**  

```
Set set = map.ketSet();    //使用迭代器遍历
```

**2）获得Map所有的value ： Collection values()方法**  

```
Collection coll = map.values();   //使用迭代器遍历
```

**3）遍历key-Value数据**


```
Set set = map.keySet();
for (Object obj : set) {
    system.out.println( "key="+obj+",value="+map.get(obj) );
}
```


#### 13、Map接口---LinkedHashMap实现类（常用与经常改变的数据）

**特点 ：**  

主要是在插入的时候，有一个链表来记录插入的顺序，具体与 LinkedHashSet 一致

#### 14、Map接口---TreeMap实现类

**特点：**  

按照添加进map的元素的key的指定属性排序。

**方式：**  

```
Map map = new TreeMap();
```


#### 15、Collections工具类（用于操作Collection与Map集合）
**方法：**  
- 1）reverse(List)---翻转List里面的元素  

- 2）shuffle(List)---对List集合元素进行随机排序
- 3）swap(List list,int i,int j)---对list集合，将指定的 i 的位置的元素与 j 位置的元素进行交换
- 4）Object max(Collection coll)---取得 coll 的最大值
- 5）Object min(Collection coll)---取得 coll 的最小值
- 6）int frequency(Collection coll,Object obj)---返回coll中obj元素出现的次数
- 7）void copy(List dest,List sec)---将sec集合放到dest集合里面


### 十五、网络编程
**1）计算机网络：**

把分布在不同地理区域的计算机与专门的外部设备用通信线路连成一个规模大、功能强的网络系统，从而使众多计算机方便的互相传递信息、共享硬件、软件等。目的是为了直接或者间接的通过网络协议与其他计算机进行通信。

**2）网络通信协议：**  

**定义：**

就是那几网络实现通信必须有一些约定，及通信协议对速率、传输代码、代码结构、传输与控制步骤、出错控制等制定标准。主要是用 TCP/IP模型。  

![image](https://note.youdao.com/yws/api/personal/file/FF6E83D953DA49C88982EFC516BECC22?method=download&shareKey=8097a6cb029c8218261dc69f776d42e8)

**传输层的两大重要协议：**

- 传输控制协议：TCP --- 是一个面向连接的协议。java中java.net的包下来进行网络编程。 
<font color=red size=3>Socket是网络驱动层给应用程序提供的接口与一种机制。</font>
    - ServerSocket：此类用于实现服务端的套接字。
    - Socket：此类用于实现客户端的套接字。
  
- 用户数据报协议：UDP

![image](https://note.youdao.com/yws/api/personal/file/FBB0BC108255478590BD42512BFE7E5D?method=download&shareKey=6358dd41b6931843f308271be7433a9a)


**3）网络通信地址（InetAddress）与 端口号：**

1、InetAddress ：代表着IP地址，一个这个对象就代表一个IP地址  

2、创建InetAddress对象 ：

```
InetAddress inet = InetAddress.getByName("www.baidu.com"); //域名来得到ip地址与域名
String yuming = inet.getHostName();  //得到域名
String address = inet.getHostAddress();   //得到ip
```

**注意：** 端口号 + InetAddress = 网络套接字(Socket);

**4）利用 套接字(Socket) 开发网络程序：**  

通信的两端都需要有Socket，都需要有端口。  
Socket允许程序将网络当成一个流，数据在两个Socket键通过IO传输。  
一般主动发起通信的应用程序术语客户端，等待通信请求的属于服务端。  

**5）使用TCP进行通信**  

需求：客户端发送信息，服务端输出此信息。


**6）程序开发结构：**  
C/S --- 客户端/服务器
B/S --- 浏览器/服务器（常用）




### 十六、URL编程（用于表示具体资源的方式）

**URL定义：**  
统一资源定位符，表示互联网上农业以资源的地址。通过URL我们可以访问Internet上的各种资源，比如www、ftp站点。浏览器通过解析给定的URL可以在网络上查找相应的文件或者其他资源。  

**URL的组成：** <传输协议>://<主机名>:<端口号>/<文件名>   

例如：

```
http://192.168.21.155:8080/Apple/index.jsp
```

**作用:** 
可以通过定位资源来将文件进行下载下来。


### 十七、URI（统一资源标识）
**URL与URI的区别：**  

URI是统一资源的标识方式，是一种更抽象，更高层次的定义，指的是 统一资源标识符。  
URL是具体资源的标识方式，是一种指向具体的资源的标识，指的是统一资源定位符。

### 十八、Java映射

**java中的映射关系：**  
对象-关系映射(Object Relational Mapping，简称ORM)

**简介：**  
对象(Object)   关系(Relational)    映射(Mapping)   三者正好组成了ORM。是为了解决面向对象 与关系型数据库元数据存在互补匹配的现象的技术。ORM通过使用描述对象和数据库之间的映射的元数据，将java中的对象自动持久化到关系型数据库中，本质上是将一种形式转换为另外一种形式

**特点：**  
对象关系映射系统(ORM)一般一中间件的形式存在，主要是实现了程序的属性对应到关系型数据库的映射。

**主流的ORM产品：**  
mybatis。Hibernate



### 十九、MD5加密工具类：
使用 MessageDigest.getInstance("MD5") 方式来创建一个MD5方式的编码规则。 

使用 md.digest() 方法进行MD5编码。

java8中，使用 Base64.getEncoder().encodeToString(bs) 方式将加密的数据以一个字符串的方式进行返回。

```
@Test 
	public void test() {
		String password = "asdf1234";
		try {
			MessageDigest md = MessageDigest.getInstance("MD5");
			byte[] bs = md.digest(password.getBytes("UTF-8"));
			
			//对字节数组进行 BASE64 编码，让其以正常的字符串方式存储到数据库---jdk1.8使用
			String string = Base64.getEncoder().encodeToString(bs);
			
			System.out.println(string);
			
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
```

### 二十、数据结构
#### 1、树
##### 1）树的定义：
树是一种非线性的数据结构，该结构中的一个数据元素可以有两个或者两个以上的直接后继元素。

##### 2）树的基本概念：
- 双亲：子节点的直接上节点，就是树的双亲。
- 孩子：该节点的子树的根称为该节点的孩子。
- 兄弟：具有相同双亲的节点称为兄弟。
- 节点的度：一个节点的子树的个数记为该节点的度。
- 叶子结点:指的是度为0的节点。
- 内部节点：也称为分直接点或者非终端节点。除了根节点意外，分直接点都可以称为内部节点。


### 二十一：java参数传递方式：
#### 1、基本数据类型传递方式：
在Java中，给方法传递形参的时候，当形参为基本数据类型，则是以值的方式进行传递。

也就是说：当是一个简单的基本数据类型的时候，在java栈中直接存储了这个变量的值，直接进行值的传递。

#### 2、引用数据类型传递方式：
在Java中，引用数据类型传递的时候，是将引用数据的地址进行传递。

也就是说：当是一个引用数据类型的时候，java栈中存放的是变量名以及变量指向的堆或者常量池中的地址，传递数据的时候，就是直接将这个引用数据的地址进行传递即可（例如：String类型的数据，栈中存放的是变量名以及常量池中的数据的地址）。

##### 3、包装数据类型、String类型的传递方式：
在包装数据类型中，当更新了当前数据的值，那么会新创建一个这个数据，然后更改栈中变量的引用地址。

也就是说：当进行Integer、String等包装类型的参数进行更改时，会将这个数据存放到另外一个空间中，原来的数据不变，然后更改栈中的地址。

---

---

---

---

---

---


# java8学习笔记

### 一、Lambda表达式

#### 1、Lambda表达式的定义：
Lambda表达式是一个 匿名函数，我们可以把Lambda表达式理解为 一段可以传递的代码 (将代码像数据一样传递)。方便写出一段灵活的代码。作为一种更加紧凑的风格，使java的语言表达能力得到了提升。

#### 2、Lambda表达式的语法：
Lambda表达式在java语言中引用了一个新的语法元素与操作符。这个操作符就是 “ -> ” ，称为Lambda操作符。它将Lambda表达式分为了两个部分：	


- Lambda左侧：指定Lambda表达式所需要的所有参数。  

- Lambda右侧：指定Lambda表达体，即Lambda表达式要执行的功能。


#### 3、Lambda表达式的语法格式：
##### 语法格式一：无参，无返回值，Lambda体只需一条语句。
代码为：

```
Runnable r1 = () -> System.out.println("张世林使用Lambda表达式");
r1.run();
```

##### 语法格式二：Lambda需要一个参数，且无返回值。
代码为：

```
//调用java自带的Consumer类中的 accept()方法
StringBuffer name = new StringBuffer();
name.append("张世林");
Consumer<StringBuffer> con = (x) -> System.out.println(x);
con.accept(name.append("帅哥"));
		
//调用自己创建的接口的方法
TestInterface face = y -> System.out.println(y.append("adsa"));
face.interface2(name);
```

##### 语法格式三：Lambda需要两个以上的参数，并且Lambda体中有多条语句，有返回值。
代码为：

```
@Test
public void test4()
{
	//使用java中Comparator
	Comparator<Integer> com = (x,y) ->{
		x = x + 21;
		y = y + 1 ;
		return Integer.compare(x, y);
	};
System.out.println(com.compare(1, 22));
		
	//使用自及创建的那个函数式接口
	TestInterface test = (p,q) -> {
		p = p+1;
		q = q+3;
		int k = Integer.compare(p, q);
		return k;
	};
	System.out.println(test.interface4(3, 6));	
}
```

##### 语法格式四：Lambda体中如果只有一条语句，则可以不写 大括号 与return；
代码为：

```
//使用自及创建的那个函数式接口
	TestInterface test = (p) -> p = p+1 ;
	int k = test.interface5(3);
	System.out.println(k);
```

##### 语法格式五：Lambda表达式的参数具有参数类型推断的功能，不需要输入参数数据类型。


#### 4、java8中四大核心函数式接口：

- Consumer<T> : 消费型接口   ---- void accept(T t);     传入T类型参数，无返回值的操作。
- Supplier<T> ：供给型接口     ---- T get();                    返回T类型的结果。
- Function<T, R> ：函数型接口  --  R apply(T t);          传入T类型参数，返回R类型的参数。
- Predicate<T> ： 断言型接口  ---- boolean test(T t);    传入T类型参数，返回的是布尔类型数据。


##### 使用案例：

```
//Predicate<T> 断言型接口：
	@Test
	public void test4(){
		List<String> list = Arrays.asList("Hello", "atguigu", "Lambda", "www", "ok");
		List<String> strList = filterStr(list, (s) -> s.length() > 3);
		
		for (String str : strList) {
			System.out.println(str);
		}
	}
	//需求：将满足条件的字符串，放入集合中
	public List<String> filterStr(List<String> list, Predicate<String> pre){
		List<String> strList = new ArrayList<>();
		
		for (String str : list) {
			if(pre.test(str)){
				strList.add(str);
			}
		}
		
		return strList;
	}
```


```
//Function<T, R> 函数型接口：
	@Test
	public void test3(){
		String newStr = strHandler("\t\t\t 我大尚硅谷威武   ", (str) -> str.trim());
		System.out.println(newStr);
		
		String subStr = strHandler("我大尚硅谷威武", (str) -> str.substring(2, 5));
		System.out.println(subStr);
	}
	//需求：用于处理字符串，如何处理留给调用的时候来使用
	public String strHandler(String str, Function<String, String> fun){
		return fun.apply(str);
	}
```


```
//Supplier<T> 供给型接口 :
	@Test
	public void test2(){
		List<Integer> numList = getNumList(10, () -> (int)(Math.random() * 100));
		
		for (Integer num : numList) {
			System.out.println(num);
		}
	}
	//需求：产生指定个数的整数，并放入集合中
	public List<Integer> getNumList(int num, Supplier<Integer> sup){
		List<Integer> list = new ArrayList<>();
		
		for (int i = 0; i < num; i++) {
			Integer n = sup.get();
			list.add(n);
		}
		
		return list;
	}
```


```
//Consumer<T> 消费型接口 :将参数传递进去，不进行值的返回
	@Test
	public void test1(){
		happy(10000, (m) -> System.out.println("你们刚哥喜欢大宝剑，每次消费：" + (m+10) + "元"));
	} 
	
	public void happy(double money, Consumer<Double> con){
		con.accept(money);
	}
```

### 二、方法引用与构造器引用

#### 1、方法引用：
##### 简介：
若lambda体中已经被实现了，就可以使用方法引用。
##### 方法引用的介绍：
使用操作符 “::” 将方法名和对象或类的名字分隔开来。 
##### 方法引用的三种情况：
-  对象::实例方法 

-  类::静态方法
-  类::实例方法

##### 代码如下：


```
@Test
	public void test5(){
		BiPredicate<String, String> bp = (x, y) -> x.equals(y);
		System.out.println(bp.test("abcde", "abcde"));
		
		System.out.println("-----------------------------------------");
		
		BiPredicate<String, String> bp2 = String::equals;
		System.out.println(bp2.test("abc", "abc"));
		
		System.out.println("-----------------------------------------");
		
		
		Function<Employee, String> fun = (e) -> e.show();
		System.out.println(fun.apply(new Employee()));
		
		System.out.println("-----------------------------------------");
		
		Function<Employee, String> fun2 = Employee::show;
		System.out.println(fun2.apply(new Employee()));
		
	}
```

​	

```
//类名 :: 静态方法名
	@Test
	public void test4(){
		Comparator<Integer> com = (x, y) -> Integer.compare(x, y);
		
		System.out.println("-------------------------------------");
		
		Comparator<Integer> com2 = Integer::compare;
	}
	
	@Test
	public void test3(){
		BiFunction<Double, Double, Double> fun = (x, y) -> Math.max(x, y);
		System.out.println(fun.apply(1.5, 22.2));
		
		System.out.println("--------------------------------------------------");
		
		BiFunction<Double, Double, Double> fun2 = Math::max;
		System.out.println(fun2.apply(1.2, 1.5));
	}
```



```
//对象的引用 :: 实例方法名
	@Test
	public void test2(){
		Employee emp = new Employee(101, "张三", 18, 9999.99);
		
		Supplier<String> sup = () -> emp.getName();
		System.out.println(sup.get());
		
		System.out.println("----------------------------------");
		
		Supplier<String> sup2 = emp::getName;
		System.out.println(sup2.get());
	}
```


```
@Test
	public void test1(){
		PrintStream ps = System.out;
		Consumer<String> con = (str) -> ps.println(str);
		con.accept("Hello World！");
		
		System.out.println("--------------------------------");
		
		Consumer<String> con2 = ps::println;
		con2.accept("Hello Java8！");
		
		Consumer<String> con3 = System.out::println;
	}
```

#### 2、构造器引用：
##### 格式：
ClassName::new 
##### 特点：
与函数式接口相结合，自动与函数式接口中方法兼容。    可以把构造器引用赋值给定义的方法，与构造器参数 列表要与接口中抽象方法的参数列表一致。
##### 代码如下：

```
@Test
	public void test7(){
		Function<String, Employee> fun = Employee::new;
		
		BiFunction<String, Integer, Employee> fun2 = Employee::new;
	}
	
	@Test
	public void test6(){
		Supplier<Employee> sup = () -> new Employee();
		System.out.println(sup.get());
		
		System.out.println("------------------------------------");
		
		Supplier<Employee> sup2 = Employee::new;
		System.out.println(sup2.get());
	}
```


### 3、数组引用：
##### 格式： 
type[] :: new

##### 代码如下：

```
@Test
	public void test8(){
		Function<Integer, String[]> fun = (args) -> new String[args];
		String[] strs = fun.apply(10);
		System.out.println(strs.length);
		
		System.out.println("--------------------------");
		
		Function<Integer, Employee[]> fun2 = Employee[] :: new;
		Employee[] emps = fun2.apply(20);
		System.out.println(emps.length);
	}
```

### 4、Lambda表达式使用方式：
##### 集合的遍历：  

```
list.forEach(System.out::println);
```


### 三、Optional 容器类--避免空指针异常
#### 1、简介：
Optional<T>是在java.util.Optional中的一个容器类，用于封装 T 类型的数据来尽量的避免空指针异常。
#### 2、方法：
- Optional.of(T t) : 创建一个T类型的 Optional 实例。  

- Optional.empty() : 创建一个空的 Optional 实例。
- Optional.ofNullable(T t):若 t 不为 null,创建 Optional 实例,否则创建空实例。
- isPresent() : 判断对象是否包含值。
- orElse(T t) :  如果调用对象包含值，返回该值，否则返回T类型的默认值。
- orElseGet(Supplier s) :如果调用对象包含值，返回该值，否则返回 s 获取的值。
- map(Function f): 如果有值对其处理，并返回处理后的Optional，否则返回 Optional.empty()
- flatMap(Function mapper):与 map 类似，要求返回值必须是Optional

#### 3、使用方式：

```
@Test
	public void test1()
	{
		//创建一个实例
		Optional<List<Person>> op1 = Optional.of(list);
		System.out.println(op1.get());
		
		//创建一个空的实例
		Optional<Person> op2 = Optional.empty();
		
		//创建一个实例，若形参为null，则创建空实例，否则创建Optional实例
		Optional<List<Person>> op3 = Optional.ofNullable(list);
		System.out.println(op3);
		
		//判断对象中是否有内容
		boolean present = op3.isPresent();
		System.out.println(present);
		
		//如果对象Optional对象是空的话，返回自己构建的那个值
		Person orElseGet = op2.orElseGet(() -> new Person(3,"3","3",3,true));
		System.out.println(orElseGet);
		
		//
		Optional<Person> person1 = Optional.ofNullable(new Person(3,"3","3",3,true));
		Optional<Integer> map = person1.map( (p) -> p.getAge());
		System.out.println(map);
	}
```


### 四、java8接口中可以编写方法以及方法的实现。
#### 简介：
java8中允许接口中包含具体实现的方法，该方法称为 “默认方法”，默认方法必须使用 default 来修饰。
#### 具体实现：

```
public interface MyFun {
	void test1();
	default void test()
	{
		System.out.println();
	}
}
```


### 五、java8新时间日期API
#### 1、DateTimeFormatter---格式化器
##### 作用：
用于指定选择什么样式的时间、日期转换器以及自定义日期转换格式。
##### 位置：
在 java.time.format 包下面。
#### 2、LocalDate、LocalTime、LocalDateTime三大时间日期格式类
##### 使用方法：

```java
LocalDateTime ldt = LocalDateTime.now();
		System.out.println(ldt);
		
		LocalDateTime ld2 = LocalDateTime.of(2016, 11, 21, 10, 10, 10);
		System.out.println(ld2);
		
		LocalDateTime ldt3 = ld2.plusYears(20);
		System.out.println(ldt3);
		
		LocalDateTime ldt4 = ld2.minusMonths(2);
		System.out.println(ldt4);
		
		System.out.println(ldt.getYear());
		System.out.println(ldt.getMonthValue());
		System.out.println(ldt.getDayOfMonth());
		System.out.println(ldt.getHour());
		System.out.println(ldt.getMinute());
		System.out.println(ldt.getSecond());
```


#### 3、常用方法：
- now() 	静态方法，根据当前时间创建对象 		

```java
LocalDate localDate = LocalDate.now(); 
LocalTime localTime = LocalTime.now(); 
LocalDateTime localDateTime = LocalDateTime.now();
```

- of()	  静态方法，根据指定日期/时间创建对象     

```java
LocalDate localDate = LocalDate.of(2016, 10, 26); 
LocalTime localTime = LocalTime.of(02, 22, 56); 	
LocalDateTime localDateTime = LocalDateTime.of(2016, 10, 26, 12, 10, 55);
```


- getDayOfMonth 	获得月份天数(1-31)   

- getDayOfYear 		获得年份天数(1-366) 
- getDayOfWeek 	获得星期几(返回一个 DayOfWeek 枚举值) 
- getMonth 		获得月份, 返回一个 Month枚举值 
- getMonthValue 	获得月份(1-12) getYear 获得年份

#### 4、更改时区:

```java
//更换时区
LocalDateTime ldt = LocalDateTime.now(ZoneId.of("Asia/Shanghai"));
System.out.println(ldt);

//时区的时间日期格式
ZonedDateTime zdt = ZonedDateTime.now(ZoneId.of("US/Pacific"));
System.out.println(zdt);
```


### 八、java8 -Stream API

#### 1、简单理解：
将原有的数据源(比如 数组、集合、对象等) 转换成流，然后根据 Stream API 来进行一系列的中间操作，执行完以后，会产生一个新的流来存储新的结果，而原有的数据不会改变。

#### 2、什么是流(Stream)？
“流” 就是数据渠道，用于操作数据源(集合、数组等)所生成的元素序列。  
“集合讲的是数据，流讲的是计算”。  

**”流“的特点：**  
Stream 自己不会存储对象。  
Stream 不会改变原来的对象，而是返回一个持有结果的 新的Stream。  
Stream 操作是延迟执行的，这意味着他们会等到需要的结果的时候才执行。  

#### 3、Stream的三个操作步骤：
创建 Stream ： 一个数据源(如：集合、数组等)，获取一个流。  
中间操作 ： 一个中间操作链，对数据源的数据进行处理。  
终止操作： 一个终止操作，执行中间操作链。  

![image](https://note.youdao.com/yws/api/personal/file/CCBB0847D07F41E9831DB0D2C247612C?method=download&shareKey=b8dd944c3143a5a1961be6b07ba0e9f7)


#### 4、对Stream流的代码操作：
##### 1）创建Stream流：
- 方法一：创建数据源,然后将数据源转换为 Stream 流
- 方法二：通过 Arrays 中的 Stream() 方法来获取数组流。 
- 方法三：通过Stream类中的 of() 方法获取流。
- 方法四：创建无限流---项目不停，流就一直不断，可以使用 limit(num) 方法来限定生成多少流。
    - 迭代  --使用迭代来创建无限流。
    - 生成  --使用生成来创建无限流。
    

**代码为：**

```java
@Test
	public void test1()
	{
		//方法一：创建数据源,然后将数据源转换为 Stream 流
		List<String> list = new ArrayList<>();
		Stream stream1 = list.stream();
		
		//方法二：通过 Arrays 中的 Stream() 方法来获取数组流。 
		Person[] persons = new Person[100];
		Stream<Person> stream2 = Arrays.stream(persons);
		
		//方法三：通过Stream类中的 of() 方法获取流
		Stream<Person> stream3 = Stream.of(new Person());

        //迭代
		Stream<Integer> stream4 = Stream.iterate(0, (x) -> x+2 );
		stream4.limit(10).forEach(System.out::println);
		
		//生成
		Stream<Double> stream5 = Stream.generate(() -> Math.random());
		stream5.limit(12).forEach(System.out::println);
	}
```


##### 2）Stream中间操作

**简介：**  
多个中间操作可以连接起来形成一个流水线，除非流水线上出发终止操作，否则中间操作不会执行任何的处理，而在终止操作时一次性全部处理，所以称为 “惰性求值”。

**中间操作的内容：**

**A) 筛选与切片** ---使用方法来对流的数据进行筛选、去重等。  

**方法为：**  

- filter(Predicate p) 	接收 Lambda ， 从流中排除某些元素。  

- distinct() 			筛选，通过流所生成元素的 hashCode() 和 equals() 去除重复元
   素 
- limit(long maxSize)  截断流，使其元素不超过给定数量。
- skip(long n) 		跳过元素，返回一个扔掉了前 n 个元素的流。若流中元素 不足
   n 个，则返回一个空流。与 limit(n) 互补。

**代码为：**

```java
@Test
	public void test1()
	{
		//使用filter来执行中间操作，进行过滤。
		Stream<Person> stream1 = list.stream()
                     .limit(1)
                  // .skip(1)
								 .filter( (e) -> {
									System.out.println("正在执行中间操作");
									return e.getAge() > 30;
								 });
		//终止操作
		stream1.forEach(System.out::println);
	}
```

**B) 映射**  

**方法为：**  

- map(Function f) 					接收一个函数作为参数，该函数会被应用到每个
               		​    元素上，并将其映射成一个新的元素。
- mapToDouble(ToDoubleFunction f) 	接收一个函数作为参数，该函数会被应用到每个
                     ​    ​    元素上，产生一个新的 DoubleStream。 
- mapToInt(ToIntFunction f) 			接收一个函数作为参数，该函数会被应用到每个
                    ​    ​    元 素上，产生一个新的 IntStream。 
- mapToLong(ToLongFunction f) 		接收一个函数作为参数，该函数会被应用到每个
                    ​    ​    元素上，产生一个新的 LongStream。
- flatMap(Function f) 					接收一个函数作为参数，将流中的每个值都换成
                    ​    ​    另 一个流，然后把所有流连接成一个流。

**代码为：**  


```java
@Test
	public void test1(){
		Stream<String> str = emps.stream().map((e) -> e.getName());

		List<String> strList = Arrays.asList("aaa", "bbb", "ccc", "ddd", "eee");
		
		Stream<String> stream = strList.stream()
			   .map(String::toUpperCase);
		stream.forEach(System.out::println);
		
		Stream<Stream<Character>> stream2 = strList.stream()
			   .map(TestStreamAPI1::filterCharacter);
		stream2.forEach((sm) -> {
			sm.forEach(System.out::println);
		});
		
		Stream<Character> stream3 = strList.stream()
			   .flatMap(TestStreamAPI1::filterCharacter);
		
		stream3.forEach(System.out::println);
	}

	public static Stream<Character> filterCharacter(String str){
		List<Character> list = new ArrayList<>();
		
		for (Character ch : str.toCharArray()) {
			list.add(ch);
		}
		return list.stream();
	}
```

**c）排序**  

**方法为：**  

- sorted()——自然排序

- sorted(Comparator com)——定制排序

**代码为：**

```java
    @Test
	public void test2(){
		emps.stream()
			.map(Employee::getName)
			.sorted()
			.forEach(System.out::println);
		
		System.out.println("------------------------------------");
		
		emps.stream()
			.sorted((x, y) -> {
				if(x.getAge() == y.getAge()){
					return x.getName().compareTo(y.getName());
				}else{
					return Integer.compare(x.getAge(), y.getAge());
				}
			}).forEach(System.out::println);
	}
```




##### 3）Stream终止操作
**简介：**
终端操作会从流的流水线生成结果。其结果可以是任何不是流的 值，例如：List、Integer，甚至是 void 。


```java
//终止操作
stream1.forEach(System.out::println);
```




### XML--可扩展标记语言
#### 1、简介：
xml是用文本来描述数据的文档。

#### 2、用途：
- 充当显示数据
- 存储数据
- 以XML方式来描述数据，并在联系服务器与系统的其他部分之间传输。

总之：xml是数据封装和数据消息传递的技术。

#### 3、解析XML传输过来的数据
##### 使用SAX来解析XML数据：
SAX用于读取与操作xml数据的方式，SAX语序在读取文档的时候去处理它，从而不必要在整个文档存储之后才采取数据操作。SAX API是一个基于事件的API，适用于处理数据流，即随着数据的流动而依次处理数据。
