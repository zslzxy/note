[toc]
# JVM学习笔记

### JVM总结的问题：

#### 1、java虚拟机的理解？
虚拟机：虚拟机指的是通过软件模拟具有完整硬件系统功能的、运行在一个完全隔离环境中的完整计算机系统。  
java虚拟机，专门用于加载与运行 .class 文件的。它运行在一个完全隔离的环境的虚拟操作系统。它是通过这实际的计算机上模拟计算机的功能来实现的。拥有着完整的架构，如处理器、堆栈、寄存器等。java虚拟机是运行在操作系统之上的，所有只要操作系统安装了java虚拟机，就能够运行java文件以及java项目。实现的是：一次编译，处处运行的道理。

#### 2、对JVM的ClassLoader类加载器的了解？
JVM本身自带三种类加载器，分别是 BootStrap跟加载器---负责加载java的核心类库，Extension扩展类加载器---负责加载<JAVA_HOME>/lib/ext下的类库，APP ClassLoader系统类加载器---负责加载classpath下的java文件。  
JVM也可以自定义ClassLoader来实现类的加载。  
JVM中，类的加载、连接、初始化以及类的卸载都是在JVM中实现的，且类的加载采用的是双亲委派机制。

#### 3、什么是OOM？
OOM 简称 “Out Of Menmery”，意思为“内存用完了”。来源于java.lang.OutOfMemoryError，当JVM中没有足够的内存空间来给对象进行分配，且垃圾回收器已经没有可以回收的空间以后，就会抛出这个Error。  

**造成 OOM 的常见的原因有两种：**
- 内存泄漏：事情做完以后内存未能够得到释放，导致虚拟机一直不能够再次使用这部分内存空间，造成内存泄漏。  
- 内存溢出：申请的内存空间超出了JVM提供的内存大小，则内存溢出。  

**常见的 OOM 的情况**：  
- java.lang.OutofMemoryError : java heap space  ----堆内存溢出。  
- java.lang.OutOfMenoryError : PermGen space ----java永久带溢出，即方法区溢出。
- java.lang.StackOverFlowError : ---java虚拟机栈内存溢出。   

**解决办法：**  
使用软引用与弱引用来解决OOM的问题。如软引用来加载大量数据，当内存不够用的时候，则会自动清理内存中的其他数据来释放内存空间。  

#### 4、什么是虚拟机？


#### 强引用、软引用、弱引用、虚引用与GC的关系？
**强引用：** new 出来的对象之间的引用---只要强引用存在，则GC永远不回收。  
  例如：Person person = new Person();   这个per就是一个强引用，如果一个对象具有强引用，那Java虚拟机宁愿抛出out of memory也不会对这个对象进行回收。

**软引用：** 如果一个对象只具有软引用。那么java虚拟机内存足够的话，就不会回收它，如果java虚拟机内存不足的时候，GC就会将其内存进行回收。软引用可以用来实现内存敏感的高速缓存。  
例如：SoftReference<User> sr = new SoftReference<User>(new User());  

**弱引用：** 如果对象只具有弱引用，则不管JVM内存是否足够，只要在垃圾回收器的管理范围之内，一旦GC发现了对象为 弱引用，就会将其放在ReferenceQueue队列中，等待下次GC来对其进行回收。所以，一般使用弱引用的时候，需要通过 XXX.get() 来判断对象是否为null；  
例如：WeakReference<User> wr = new WeakReference<User>(new User());  

**虚引用：**  “虚引用”顾名思义，就是形同虚设，与其他几种引用都不同，虚引用并不会决定对象的生命周期。如果一个对象 仅持有虚引用，那么它就和没有任何引用一样，在任何时候都可能被垃圾回收。 虚引用主要用来跟踪对象被垃圾回收的活动。虚引用与软引用和弱引用的一个区别在于：虚引用必须和引用队列 （ReferenceQueue）联合使用。当垃圾回收器准备回收一个对象时，如果发现它还有虚引用，就会在回收对象的内存之前，把这个虚引用加入到与之 关联的引用队列中。程序可以通过判断引用队列中是否已经加入了虚引用，来了解被引用的对象是否将要被垃圾回收。程序如果发现某个虚引用已经被加入到引用队 列，那么就可以在所引用的对象的内存被回收之前采取必要的行动。  


### 一、JVM的体系结构：
![image](https://note.youdao.com/yws/api/personal/file/CB94FE2781B34988AB50B04D877994DC?method=download&shareKey=413bded35326a910c80f62d61b40a313)  

#### 1、JVM的内存划分为：
- 方法区（Method Area）
- 堆区（Heap）
- 虚拟机栈（JVM Stack）
- 本地方法栈（Native Method Stack）
- 程序计数器（Program Counter Register）

#### 2、方法区(Method Area)   
- **方法区，也就是通常说的 “持久带”。**  
- 方法区主要是存放了类的信息、静态变量、构造函数、final自定义常量、类中的字段与方法等信息。  
- 方法区是共享的，所有线程都能够访问。  
- 方法区存在着少量的GC垃圾回收，当方法区占用的内存超出以后，就会抛出OutOfMemoryError异常。  
- 方法区包含 **运行时异常**，用于存储编译器生成的常量与引用。  

**注意：JDK1.7中的运行时常量池、静态变量的在方法区中，然后JDK1.8的运行时常量池、静态变量的在堆中的元空间中。**

#### 3、堆(Heap)
##### 1）简介：
堆中是专门用于存放new出来的东西（new Person(),就是对象的实体，数组的实体等），以及 成员变量等。  
堆中是GC最频繁调用的位置，堆中是所有线程共享，在虚拟机启动的时候创建。  
##### 2）java7堆内存结构：
逻辑上：新生区 + 养老区 + 永久区  
实际上：新生区 + 杨老区，而永久区则称为 非堆内存。  

![image](https://note.youdao.com/yws/api/personal/file/0A3FBBE9A12B4BFA9D9C7BD3051E8CB1?method=download&shareKey=637ec7fd47d4efcf0c257a484b9dd467)

![image](https://note.youdao.com/yws/api/personal/file/F48F98B7992942019E96D1C9D87ADB14?method=download&shareKey=036bd5c193e22c539314234ea0fa783e)

##### 3）java8堆内存逻辑结构：
逻辑上是：新生+养老+元空间。  
实际上是：新生+养老区，而元空间称为 非堆内存。  

![image](https://note.youdao.com/yws/api/personal/file/AEAAF240A5824166A237D645CEF69F9B?method=download&shareKey=27c6c7d9ed3c5d0c22af9738cf4ac266)


##### 4）对象在新生区、养老区的移动：
如果对象在Eden中经过了一次Minor GC以后仍然存活，也能够被容纳仅如Survivor区中，并且为期设置value为1；然后在S0与S1中熬过一次次的Minor GC，直到value为15以后仍然存活，那么就将其晋升到 老年代 中。  


#### 4、虚拟机栈(VM Stack)
##### 1）简介：
虚拟机栈主要存放的是 局部变量、声明的对象的引用名、数组的引用名。虚拟机栈是私有的，每个线程都会创建一个虚拟机栈，生命周期与线程的生命周期一致。   

每个方法执行时都会产生一个**栈帧**，栈帧用于存储局部变量表、动态链接、操作数和方法出口等信息。当方法被调用时，栈帧入栈，方法结束调用时，栈帧出栈。  

栈帧内部的 **局部变量表** 存储着相关的局部变量，包括各种基本数据类型以及对象的引用地址等。因此其内存空间在编译的时候就能够确定，运行时不在改变。


#### 5、本地方法栈(Native Method Stack)
本地方法栈适用于支持native方法的执行。存储了每个native方法执行的状态。本地方法栈的执行机制与虚拟机栈的执行机制相同，区别在于：本地方法栈执行的是native方法，虚拟机栈执行的是java方法。在JVM中，常用的HotSpot虚拟机是将虚拟机栈与本地方法栈结合在一起使用的。


#### 6、程序计数器(Program Counter Register)
程序计数器是在一个很小的内存空间，直接划分在CPU上面。主要作用是：JVM在解释字节码（.class）文件时，存储当前线程执行的字节码行号，只是一种概念模型，各种JVM所采用的方式不一样。字节码解释器工作时，就是通过改变程序计数器的值来取下一条要执行的指令，分支、循环、跳转等基础功能都是依赖此技术区完成的。  
程序计数器是线程私有的。


#### 7、注意事项：
1. 方法区与堆的内存区域市是共有的，程序计数器、虚拟机栈、本地方法栈、是私有的。
2. GC执行垃圾回收主要是在堆中执行，其次是在方法去中进行调用，其他地方不执行，都是随着线程的生命周期同步创建、销毁。


### 二、JVM的垃圾回收机制--GC
#### 1、GC垃圾回收器的使用场景：
随着程序的运行，内存占用越来越多，为了及时释放资源，java提供了GC垃圾回收机制。垃圾回收机制主要是作用域 方法区 与 堆中。  

#### 2、GC算法
##### 1）引用计算算法：
每个对象都添加到引用计数器，每被引用一次就 +1，如果失去引用，计数器 -1；当一段时间内计数器的值为0时，即默认对象可以被回收了。但是这个算法的缺陷在于：两个对象之间的相互引用，而两者的状态都为0时，都是被回收的对象。但是两者之间的相互引用关系导致不符合回收的条件，无法对其进行回收。  

##### 2）跟搜索算法
从一个叫 GC ROOTs 的根节点开始，向下进行搜索，如果发现有对象无法到达 GC ROOTs 的时候，就表明这个对象以及与其相互之间的引用的对象都符合了被回收的条件，被执行垃圾回收。   
![image](https://note.youdao.com/yws/api/personal/file/A08D1C6A89F44E438F988C35C38152B5?method=download&shareKey=615f8e7229e2f3d01b98f965fcabfe2a)

#### 3、垃圾收集器
简介：在JVM中，GC是由垃圾收集器来进行执行的，所以，实际场景中，我们需要选择合适的垃圾收集器来执行垃圾的回收。

##### 1）串行收集器(Serial GC)
Serial GC是最古老的也是最基本的收集器，在串行处理器中minor与major GC过程是一个线程串行执行的。它进行垃圾回收时，需要对所有正在实行的线程暂停。所以，该收集器适用于单CPU、新生代空间较小且对暂停时间要求不是特别高的应用上，是client级别的默认GC方式。

##### 2）ParNew GC
基本与Serial GC类似，但是本质的区别在于加入了多线程，提高了效率，可以在服务端使用。

##### 3）Parallel Scavenge GC
在整个扫描赋值过程中采用多线程的方式进行，适用于多CPU、对暂停空间时间要求较短的应用。**==是服务器端的默认垃圾收集器。==**

##### 4）CMS(Concurrent Mark Sweep)收集器
该收集器的主要目标是为了解决Serial GC停顿的问题，已达到最短回收时间。常见的B/S架构的应用适合于这种收集器，因为其具有高并发、高响应的特点，CMS是基于 标记-清除 算法。    
  
**优势：** 实现并发收集、低停顿的效果。  
**劣势：** 
1. CMS对CPU资源非常敏感，在并发阶段不会导致用户停顿，但是会占用CPU资源，导致应用程序变慢，吞吐量下降。
2. CMS无法清除浮动垃圾，也会导致垃圾碎片的产生。

##### 5）G1 收集器
在CMS上进行了改进，使用 标记-压缩 算法，不会产生内存碎片，可以比较精确的控制停顿。

##### 6）Serial Old收集器
Serial Old是Serial收集器的老年代版本，它同样使用一个单线程执行收集，使用“标记-整理”算法。主要使用在Client模式下的虚拟机。
       
##### 7） Parallel Old收集器
Parallel Old是Parallel Scavenge收集器的老年代版本，使用多线程和“标记-整理”算法。
       
##### 8） RTSJ垃圾收集器
RTSJ垃圾收集器，用于Java实时编程。


### 三、类加载器--ClassLoader

#### 1、类加载器的种类
##### 1）虚拟机自带的类加载器：
- Bootstrap类装载器 ： 启动类加载器  （老汉）---<JAVA_HOME>/lib路径下的核心类库，C++编写的。

- Extension类加载器： 扩展类加载器   （自己）---它负责加载<JAVA_HOME>/lib/ext目录下 或者 由系统变量-Djava.ext.dir指定位路径中的类库

- APP ClassLoader：应用程序类加载器 / 系统类加载器  （儿子）---默认加载classpath路径下的文件。

##### 2）用户自定义类加载器
- java.lang.ClassLoader 的子类，用户可自己制定类加载器。



### 四、类的加载过程

#### 1、类的加载、连接与初始化
##### 1）类的加载：
从硬盘中查找并加载类的二进制文件到JVM中(也就是.class文件到JVM中)。  

##### 2）类的连接：
步骤：
- 验证：为了确保被加载的类的正确性。
- 为类的静态变量分配内存，并将其初始化为系统默认值。
- 将类中的符号引用转换为直接引用。

##### 3）类的初始化：
为类的静态变量赋以正确的初始值。


![image](https://note.youdao.com/yws/api/personal/file/EB87B69B05244650BAB8F6E348CFF699?method=download&shareKey=b172048063a473bd8c52896ae16104d4)



#### 2、java程序对类的初始化的使用方式：
##### 1）主动使用---会执行初始化：
- 创建类的实例。
- 访问某各类或者接口的静态变量，或者对该静态变量赋值。
- 调用类的静态方法。
- 反射。
- 初始化一个类的子类。
- java虚拟机的启动的时候被表明为启动类。

##### 被动使用---不会执行初始化：
除了主动使用以外，其他都是被动使用。

##### 3）注意：
java虚拟机对类的初始化的时间是在----类或者接口 “首次主动” 使用的时候才会初始化他们。


#### 3、类的加载与加载方式---最终得到Class对象。
##### 1）简介：
类的加载指的是将类的.class文件中的二进制数据读入到内存中，将其放在 **运行时数据区的方法区** 内，然后再 **在堆中创建一个 java.lang.Class 对象**，**用来封装类在方法区的数据结构**。

##### 2）加载方式：
- 从本地系统中直接加载。 

- 将Java源文件动态编译为.class文件。
- 通过网络下载.class文件(使用的是java的 URLClassLoader 类)
- 从 zip、jar等归档文档中加载.class文件。
- 从专有数据库中提取.class文件。

==**类的加载的最终产品就是 位于堆中的 Class对象；**==  
Class对象封装了类在方法区内的数据结构，并且向java程序员提供了访问方法区内 的数据结构的接口。


#### 4、类的连接----验证
##### 1）简介：
类在加载以后，会进入连接阶段，连接阶段就是将已经读入到内存中的类的二进制数据合并到虚拟机的运行时环境中去，这个阶段需要先对类的结构进行验证，来判断是否符合java语言规范。
##### 2）验证内容：
- 类文件的结构检查：确保类文件遵从 java类文件的固定格式。

- 语义检查：确保类本身的内容复合java语言的语法规定。
- 字节码检查：确保字节码流可以被java虚拟机安全地执行。
- 二进制兼容检查：确保相互引用的类之间是协调一致的。


#### 5、类的准备与解析：
##### 1）在准备阶段
java虚拟机对类的静态变量进行非配内存，并且为其value设置为默认值。
##### 2）在解析阶段
java虚拟机会把类的二进制数据中的符号引用替换为直接引用；


#### 6、类的初始化：
##### 1）步骤：
- 假如这个类还没有被加载和连接，那就先进行加载和连接。

- 假如这个类存在直接的父类，且这个父类没有被初始化，那就先初始这个化直接的父类。
- 假如类中存在初始化语句，那就一次执行这些初始化语句。
##### 初始化时机：
- 在调用子类时，必须要先初始化其父类。

- 在调用一个类时，只会初始化其对应的接口，而不会影响其父接口。
- 只有当程序访问的静态变量或静态方法确 是在当前类或当前接口中定义时，才可以 认为是对类或接口的主动使用。


#### 7、类加载器的父亲委托机制---双亲委托机制：
##### 1）特征：
类的加载过程采用父亲委托机制，这种机制能够更好的保证java平台的安全。除了java虚拟机自带的根加载器(BootStrap)以外，其余的类加载器有且仅有一个父类。
##### 2）过程：
在加载器实现加载的过程中，类加载器会先委托自己的所有父类(从上(BootStrap)到下(用户自定义类加载器)来进行选择) 来加载器来加载这个类，依次向上委托父类进行加载，如若父类加载器完成了类加载的功能，则完成任务；如若父类加载器无法完成类加载的功能，则这个类加载器本身来自己加载这个类。


#### 8、沙箱机制：

##### 1）简介：
为了避免用户收到一塔程序或者代码对本java的侵犯，java提供了一个沙箱机制，用来保护java程序的安全性；

##### 2）包括四部分：
- 类装载器---作用：
    - 防止恶意代码对程序的干扰。
    - 守护被信任的类库边界。
    - 将代码归入到保护区。

- Class文件检验器---作用：
    - 检验文件内部的结构是否正确以及是否安全编译。
    - 检验被引用的类、属性等是否正确。

- 内置于java虚拟机的安全特性---作用：
    - 类型安全的引用转换
    - 结构化的内存访问
    - 自动垃圾回收
    - 数组边界检查
    - 空引用检查

- 安全管理及 java API---作用：
    - 它在访问控制对于外部资源的访问中起中枢的作用。专门针对于权限进行限定。

##### 3）例如：
java里面的一些类（.class）文件，比如String.class等，不能够自己再去从新写一个String.class文件进行编译执行。


#### 9、定义类加载器 与 初始类加载器：
##### 1）定义类加载器：
如果某个类加载器能够加载一个类，那么这个类加载器就称为 定义类加载器；
##### 2）初始类加载器：
定义类加载器以及所有的子加载器都称为 初始类加载器。



#### 10、类加载器的父子关系：
##### 1）简介：
类加载器之间的父子关系指的是 类加载器之间的包装关系，而不是类之间的继承关系。  
比如：父加载器Loader1能够加载Person类，则Loader2包装了Loader1，拥有loader1所有的功能，但是却不是继承关系，也能够加载Person类。
##### 2）注意：
当自定义一个类加载器的时候，如果未指定父类加载器，则系统会自动指定BootStrap加载器来作为这个自定义类的类加载器。


#### 11、类加载器的命名空间：
##### 1）简介：
类加载器的命名空间，是由该类加载器以及其父类加载器所加载的类组成，同一个命名空间中，累的名字不会重复出现，只会加载一次。


#### 12、创建自定义类加载器：
继承ClassLoader抽象类，从写方法实现类加载器。


#### 13、类的卸载：
##### 1）简介：
在类初始化以后，其生命周期就开始了。如果当Class对象不再被引用，即不可触及的时候，Class对象就会结束生命周期。  

##### 2）注意：
由java虚拟机自带的类加载器所加载的类，在虚拟机的生命周期中是不会被卸载的。所以，用户自定义的类加载器加载的Class对象就能够在JVM运行期间卸载。



### 五、JVM参数

#### 1、垃圾回收参数
-Xnoclassgc 是否对类进行回收  
-verbose:class -XX:+TraceClassUnloading 查看类加载和卸载信息
 
-XX:SurvivorRatio Eden和其中一个survivor的比值  
-XX:PretenureSizeThreshold 大对象进入老年代的阈值，Serial和ParNew生效  
-XX:MaxTenuringThreshold 晋升老年代的对象年龄，默认15, CMS默认是4   
-XX:HandlePromotionFailure 老年代担保   
-XX:+UseAdaptiveSizePolicy动态调整Java堆中各个区域大小和进入老年代年龄  
-XX:ParallelGCThreads 并行回收的线程数  
-XX:MaxGCPauseMillis Parallel Scavenge参数，设置GC的最大停顿时间  
-XX:GCTimeRatio  Parallel Scavenge参数，GC时间占总时间的比率，默认99%，即1%的GC时间  
-XX:CMSInitiatingOccupancyFraction，old区触发cms阈值，默认68%  
-XX:+UseCMSCompactAtFullCollection(CMS完成后是否进行一次碎片整理，停顿时间加长)  
-XX:CMSFullGCsBeforeCompaction(执行多少次不进行碎片整理的FullGC后进行一次带压缩的)  
-XX:+ScavengeBeforeFullGC，在fullgc前触发一次minorGC  

#### 2、垃圾回收统计信息
-XX:+PrintGC 输出GC日志  
-verbose:gc等同于上面那个  
-XX:+PrintGCDetails 输出GC的详细日志  

#### 3、堆大小设置
-Xmx:最大堆大小  
-Xms:初始堆大小(最小内存值)  
-Xmn:年轻代大小  
-XX:NewSize和-XX:MaxNewSize 新生代大小  
-XX:SurvivorRatio:3 意思是年轻代中Eden区与两个Survivor区的比值。注意Survivor区有两个。如：3，表示Eden：Survivor=3：2，一个Survivor区占整个年轻代的1/5  
-XX:NewRatio=4:设置年轻代（包括Eden和两个Survivor区）与年老代的比值（除去持久代）。设置为4，则年轻代与年老代所占比值为1：4，年轻代占整个堆栈的1/5  
-Xss栈容量 默认256k  
-XX:PermSize永久代初始值  
-XX:MaxPermSize 永久代最大值  


#### 4、生产环境参数配置(CMS-GC)


##### Java < 8 
    -server    
    -Xms<heap size>[g|m|k] -Xmx<heap size>[g|m|k]  
    -XX:PermSize=<perm gen size>[g|m|k] -XX:MaxPermSize=<perm gen size>[g|m|k]  
    -Xmn<young size>[g|m|k]  
    -XX:+DisableExplicitGC  
    -XX:SurvivorRatio=<ratio>  
    -XX:+UseConcMarkSweepGC   
    -XX:+CMSParallelRemarkEnabled  
    -XX:+CMSScavengeBeforeRemark  
    -XX:+UseCMSInitiatingOccupancyOnly   
    -XX:CMSInitiatingOccupancyFraction=<percent>  
    -XX:+PrintGCDateStamps -verbose:gc -XX:+PrintGCDetails -Xloggc:"<path to log>"  
    -XX:+UseGCLogFileRotation -XX:NumberOfGCLogFiles=10 -XX:GCLogFileSize=100M  
    -XX:+HeapDumpOnOutOfMemoryError -XX:HeapDumpPath=<path to dump>`date`.hprof  
    -Dsun.net.inetaddr.ttl=<TTL in seconds>  
    -Djava.rmi.server.hostname=<external IP>  
    -Dcom.sun.management.jmxremote.port=<port>   
    -Dcom.sun.management.jmxremote.authenticate=false   
    -Dcom.sun.management.jmxremote.ssl=false  

##### Java >= 8
    -server  
    -Xms<heap size>[g|m|k] -Xmx<heap size>[g|m|k]  
    -XX:MaxMetaspaceSize=<metaspace size>[g|m|k]  
    -Xmn<young size>[g|m|k]  
    -XX:+DisableExplicitGC  
    -XX:SurvivorRatio=<ratio>  
    -XX:+UseConcMarkSweepGC   
    -XX:+CMSParallelRemarkEnabled  
    -XX:+CMSScavengeBeforeRemark  
    -XX:+UseCMSInitiatingOccupancyOnly  
    -XX:CMSInitiatingOccupancyFraction=<percent>  
    -XX:+PrintGCDateStamps -verbose:gc -XX:+PrintGCDetails -Xloggc:"<path to log>"  
    -XX:+UseGCLogFileRotation -XX:NumberOfGCLogFiles=10 -XX:GCLogFileSize=100M  
    -XX:+HeapDumpOnOutOfMemoryError -XX:HeapDumpPath=<path to dump>`date`.hprof  
    -Dsun.net.inetaddr.ttl=<TTL in seconds>  
    -Djava.rmi.server.hostname=<external IP>  
    -Dcom.sun.management.jmxremote.port=<port>   
    -Dcom.sun.management.jmxremote.authenticate=false   
    -Dcom.sun.management.jmxremote.ssl=false  


#### 5、理解GC日志


DefNew：Serial收集器新生代名称 - Tenured - Perm  
ParNew：ParNew收集器新生代名称 -   
PSYoungGen：Parallel Scavenge收集器新生代名称  


### 六、JVM调优

#### 1、调优的方向：
* 让GC的时间足够的小   
* 让GC的次数足够的少  
* 让发生Full GC的周期足够的长  

**注意：**  
前两个目前是相悖的，要想GC时间小必须要一个更小的堆，要保证GC次数足够少，必须保证一个更大的堆，我们只能取其平衡。
   
##### 方式一：针对JVM堆的设置：
一般可以通过-Xms   -Xmx限定其最小、最大值，为了防止垃圾收集器在最小、最大之间收缩堆而产生额外的时间，我们通常把最大、最小设置为相同的值。

##### 方式二：
年轻代和年老代将根据默认的比例（1：2）分配堆内存，可以通过调整二者之间的比率NewRadio来调整二者之间的大小，也可以针对回收代。

年轻代，通过 -XX:newSize -XX:MaxNewSize来设置其绝对大小。同样，为了防止年轻代的堆收缩，我们通常会把-XX:newSize -XX:MaxNewSize设置为同样大小。-XX:PermSize，-XX:MaxPermSize设置为一样防止老年代收缩。

-XX:NewRatio=4:设置年轻代（包括Eden和两个Survivor区）与年老代的比值（除去持久代）。设置为4，则年轻代与年老代所占比值为1：4，年轻代占整个堆栈的1/5

-XX:MaxTenuringThreshold=0：设置垃圾最大年龄。如果设置为0的话，则年轻代对象不经过Survivor区，直接进入年老代。对于年老代比较多的应用，可以提高效率。如果将此值设置为一个较大值，则年轻代对象会在Survivor区进行多次复制，这样可以增加对象再年轻代的存活时间，增加在年轻代即被回收的概论。

##### 方式三：调整年轻代与老龄代的大小：
年轻代和年老代设置多大才算合理？这个我问题毫无疑问是没有答案的，否则也就不会有调优。我们观察一下二者大小变化有哪些影响
年轻代老年代设置：整个JVM内存大小=年轻代大小 + 年老代大小 + 持久代大小。持久代一般固定大小为64m，所以增大年轻代后，将会减小年老代大小。此值对系统性能影响较大，Sun官方推荐年轻代配置为整个堆的3/8。

* 更大的年轻代必然导致更小的年老代，大的年轻代会延长普通GC的周期，但会增加每次GC的时间；小的年老代会导致更频繁的Full GC。
* 更小的年轻代必然导致更大年老代，小的年轻代会导致普通GC很频繁，但每次的GC时间会更短；大的年老代会减少Full GC的频率。
* 如何选择应该依赖应用程序对象生命周期的分布情况：如果应用存在大量的临时对象，应该选择更大的年轻代；如果存在相对较多的持久对象，年老代应该适当增大。但很多应用都没有这样明显的特性，在抉择时应该根据以下两点：
    - （A）本着Full GC尽量少的原则，让年老代尽量缓存常用对象，JVM的默认比例1：2也是这个道理
    - （B）通过观察应用一段时间，看其他在峰值时年老代会占多少内存，在不影响Full GC的前提下，根据实际情况加大年轻代，比如可以把比例控制在1：1。但应该给年老代至少预留1/3的增长空间

##### 方式四：
在配置较好的机器上（比如多核、大内存），可以为年老代选择并行收集算法： -XX:+UseParallelOldGC ，默认为Serial收集

##### 方式五：线程堆栈的设置：
每个线程默认会开启1M的堆栈，用于存放栈帧、调用参数、局部变量等，对大多数应用而言这个默认值太了，一般256K就足用。理论上，在内存不变的情况下，减少每个线程的堆栈，可以产生更多的线程，但这实际上还受限于操作系统。 -Xss256k：设置每个线程的堆栈大小。JDK5.0以后每个线程堆栈大小为1M，以前每个线程堆栈大小为256K。更具应用的线程所需内存大小进行调整。在相同物理内存下，减小这个值能生成更多的线程。但是操作系统对一个进程内的线程数还是有限制的，不能无限生成，经验值在3000~5000左右。

##### 方式六：通过下面的参数打Heap Dump信息

```
  -XX:HeapDumpPath
  -XX:+PrintGCDetails
  -XX:+PrintGCTimeStamps
  -Xloggc:/usr/aaa/dump/heap_trace.txt
  通过下面参数可以控制OutOfMemoryError时打印堆的信息
  -XX:+HeapDumpOnOutOfMemoryError
```
**注意：**  
通过分析dump文件可以发现，每个1小时都会发生一次Full GC，经过多方求证，只要在JVM中开启了JMX服务，JMX将会1小时执行一次Full GC以清除引用.


#### 2、性能分析工具

##### 1）jps
```
-m 主类的参数 
-l 主类的全名，如果执行的是jar包，输出jar路径
-v 虚拟机参数
```

##### 2）jstat
```
监视虚拟机运行状态信息，包括类装载、GC、运行期编译（JIT）
用于输出java程序内存使用情况，包括新生代、老年代、元数据区容量、垃圾回收情况
 jstat -gcutil 52670 2000 5 进程号 2s输出一次一共5次

S0：幸存1区当前使用比例
S1：幸存2区当前使用比例
E：伊甸园区使用比例
O：老年代使用比例
M：元数据区使用比例
CCS：压缩使用比例
YGC：年轻代垃圾回收次数
YGCT：年轻代垃圾回收消耗时间
FGC：老年代垃圾回收次数
FGCT：老年代垃圾回收消耗时间
GCT：垃圾回收消耗总时间
```

##### 3）jmap
```
jmap：用于生成堆转储快照。一般称为dump或heapdump文件。
jmap -histo 3618
上述命令打印出进程ID为3618的内存情况，包括有哪些对象，对象的数量。但我们常用的方式是将指定进程的内存heap输出到外              部文件，再由专门的heap分析工具进行分析,例如mat（Memory Analysis Tool），所以我们常用的命令是：
jmap -dump:live,format=b,file=heap.hprof 3618
-F 强制生成dump快照
```

##### 4）jstack
```
jstack：用户输出虚拟机当前时刻的线程快照，常用于定位因为某些线程问题造成的故障或性能问题。一般称为threaddump文件。
参数
-F当正常输出没有响应的时候强制打印栈信息，一般情况不需要使用
-l长列表. 打印关于锁的附加信息，一般情况不需要使用
-m 如果调用本地方法的话，可打印c/c++的堆栈
```


### 七、内存泄漏及解决方法

#### 1、系统崩溃前的一些现象：

- 每次垃圾回收的时间越来越长，由之前的10ms延长到50ms左右，FullGC的时间也有之前的0.5s延长到4、5s。

- FullGC的次数越来越多，最频繁时隔不到1分钟就进行一次FullGC。

- 年老代的内存越来越大并且每次FullGC后年老代没有内存被释放。

- 之后系统会无法响应新的请求，逐渐到达OutOfMemoryError的临界值。

#### 2、生成堆的dump文件
通过JMX的MBean生成当前的Heap信息，大小为一个3G（整个堆的大小）的hprof文件，如果没有启动JMX可以通过Java的jmap命令来生成该文件。

#### 3、分析dump文件

下面要考虑的是如何打开这个3G的堆信息文件，显然一般的Window系统没有这么大的内存，必须借助高配置的Linux。当然我们可以借助X-Window把Linux上的图形导入到Window。我们考虑用下面几种工具打开该文件：

- Visual VM
- IBM HeapAnalyzer
- JDK 自带的Hprof工具

使用这些工具时为了确保加载速度，建议设置最大内存为6G。使用后发现，这些工具都无法直观地观察到内存泄漏，Visual VM虽能观察到对象大小，但看不到调用堆栈；HeapAnalyzer虽然能看到调用堆栈，却无法正确打开一个3G的文件。因此，我们又选用了Eclipse专门的静态内存分析工具：Mat。

#### 4、分析内存泄漏

通过Mat我们能清楚地看到，哪些对象被怀疑为内存泄漏，哪些对象占的空间最大及对象的调用关系。针对本案，在ThreadLocal中有很多的JbpmContext实例，经过调查是JBPM的Context没有关闭所致。

另，通过Mat或JMX我们还可以分析线程状态，可以观察到线程被阻塞在哪个对象上，从而判断系统的瓶颈。

#### 5、回归问题

Q：为什么崩溃前垃圾回收的时间越来越长？

A:根据内存模型和垃圾回收算法，垃圾回收分两部分：内存标记、清除（复制），标记部分只要内存大小固定，时间是不变的，变的是复制部分，因为每次垃圾回收都有一些回收不掉的内存，所以增加了复制量，导致时间延长。所以，垃圾回收的时间也可以作为判断内存泄漏的依据

Q：为什么Full GC的次数越来越多？

A：因此内存的积累，逐渐耗尽了年老代的内存，导致新对象分配没有更多的空间，从而导致频繁的垃圾回收

Q:为什么年老代占用的内存越来越大？

A:因为年轻代的内存无法被回收，越来越多地被Copy到年老代


#### 6、调优方法

一切都是为了这一步，调优，在调优之前，我们需要记住下面的原则：

1、多数的Java应用不需要在服务器上进行GC优化；

2、多数导致GC问题的Java应用，都不是因为我们参数设置错误，而是代码问题；

3、在应用上线之前，先考虑将机器的JVM参数设置到最优（最适合）

4、减少创建对象的数量；

5、减少使用全局变量和大对象；

6、GC优化是到最后不得已才采用的手段；

7、在实际使用中，分析GC情况优化代码比优化GC参数要多得多；

GC优化的目的有两个

1、将转移到老年代的对象数量降低到最小；

2、减少full GC的执行时间；

为了达到上面的目的，一般地，你需要做的事情有：

1、减少使用全局变量和大对象；

2、调整新生代的大小到最合适；

3、设置老年代的大小为最合适；

4、选择合适的GC收集器