[TOC]
# BIO学习笔记

## 一. 基本介绍

> ​	BIO在java中统称为 IO，是一种**阻塞IO**；通常情况下，使用BIO处理客户端请求，那么客户端有多少个连接，那么服务器就需要创建多少个线程，否则会导致客户端连接被阻塞或失效；

## 二. BIO的Socket实例

- 使用BIO来创建 ServerSocket对象，那么该Socket请求，会在 socket.accept()处被阻塞，直到有客户端请求过来以后，才会执行后面的代码；相当于执行了  `telnet 127.0.0.1  7777`   
- accept.getInputStream()方法在没有接收到该客户端发送的请求的情况下，也会阻塞该线程，直到客户端有数据发送过来以后，才会执行后面的代码。相当于执行了  `send 张世林大帅哥`  

```java
    public static void main(String[] args) throws IOException {
        ServerSocket serverSocket = new ServerSocket(7777);
        System.out.println("-------服务器已启动------");
        while (true) {
            Socket socket = serverSocket.accept();
            System.out.println("连接到了一个客户端");
            try {
                byte[] bytes = new byte[1024];
                InputStream inputStream = socket.getInputStream();
                int read = 0;
                System.out.println("线程id：" + Thread.currentThread().getId());
                while ((read = inputStream.read(bytes)) != -1) {
                    System.out.println(new String(bytes, 0, read) + ",线程id：" + Thread.currentThread().getId());
                }
            } catch (Exception ex) {
                ex.printStackTrace();
            } finally {
                try {
                    socket.close();
                } catch (Exception ex) {
                    ex.printStackTrace();
                }
            }
        }
    }
```

- 通常情况下，BIO采用的是server与client一一对应的方式，将两者进行关联；服务器端采用线程池；

```java
    public static void main(String[] args) throws IOException {
        ExecutorService executorService = Executors.newCachedThreadPool();
        ServerSocket serverSocket = new ServerSocket(7777);
        System.out.println("-------服务器已启动------");
        while (true) {
            Socket socket = serverSocket.accept();
            System.out.println("连接到了一个客户端");
            executorService.submit(() -> {
                handler(socket);

            });
        }
    }

    public static void handler(Socket socket) {
        try {
            byte[] bytes = new byte[1024];
            InputStream inputStream = socket.getInputStream();
            int read = 0;
            System.out.println("线程id：" + Thread.currentThread().getId());
            while ((read = inputStream.read(bytes)) != -1) {
                System.out.println(new String(bytes, 0, read) + ",线程id：" + Thread.currentThread().getId());
            }
        } catch (Exception ex) {
            ex.printStackTrace();
        } finally {
            try {
                socket.close();
            } catch (Exception ex) {
                ex.printStackTrace();
            }
        }
    }
```





# NIO学习笔记

## 一、NIO概况
### 1. 简介：
> NIO是java1.4版本以后投入的一个新的API，NIO与IO的作用与目的是一致的，专注于文件的读取、写入的IO操作。  
>
> NIo的操作方式：**面向缓冲区的、基于通道的操作**。NIO是以更加高效的方式进行文件的读写操作。

### 2. 原理：
> NIO是由一个或者多个专门的线程来处理多有的IO事件。  
>
> 处理IO事件是到了被触发的时候才触发，不会一直占着线程来进行监听，避免资源浪费。


### 3. NIO与IO的区别

IO | NIO
---|---
面向流（Stream Oriented） | 面向缓冲区（Buffer Oriented）
阻塞IO（Blocking IO） | 非阻塞IO（Non Blocking IO）
无选择器    | 选择器（Selectors）

### 4. 面向流与面向缓冲区的区别
> ​	IO面向流的操作，是每一次从流中读取出一个或者多个字节的数据，直到流中的数据被读取完成。在这个过程中，数据无法前后移动，如果需要进行数据的前后移动，则需要将其缓存到新的缓冲区。
>
> ​	NIO是面向缓冲区的，它将数据读取到缓冲区，数据在缓冲区中能够钱够移动，增加数据处理的灵活性。在这个过程中，读取数据先要判断缓冲区是否有这个数据，且需要注意数据量太大而覆盖掉了之前缓冲区的数据。  

### 5. NIO的核心
> - 缓冲区--`Buffer`：用于存储于容纳数据，操作缓冲区的数据，对数据进行处理。  
> - 通道--`Channel`：用于与IO设备的连接，用于负责承载缓冲区进行数据的传输。
> - 选择器--`Selectors`：用于对各个Channel的监控与线程的连接。

### 6. 通道与缓冲区的联系与运行模式：
##### 联系：
通道必须结合着缓冲区进行使用，通道本身无法进行数据的传输。  
缓冲区位于通道中，专门用于承载数据。

##### 运行模式：
![image](https://note.youdao.com/yws/api/personal/file/802CF83C17FC4D858990C3F80A418752?method=download&shareKey=16d957cde70f4a2bfcaf4e597232b850)

### 7. 缓冲区
#### 7.1 简介：
> 1）缓冲区底层数据模型是数组，用于存储不同类型的数据。  
> 2）缓冲区存储数据的不同，所以分为不同的缓冲区类型，常见的基本数据类型的缓冲区有：  
> `ByteBuffer`、`CharBuffer`、`ShortBuffer`、`IntBuffer`、`LongBuffer`、`FloatBuffer`、`DoubleBuffer`。

#### 7.2 创建缓冲区：
> 使用 `allocate()` 方法来创建缓冲区。  
>
> 例如：创建一个大小为1024字节的字节缓冲区。

```java
ByteBuffer buf = ByteBuffer.allocate(1024);
```

#### 7.3 缓冲区存数据的核心方法：

##### 7.3.1 put() ---存入数据到缓冲区中。
> - put(byte b)：将给定单个字节写入缓冲区的当前位置
>
> - put(byte[] src)：将src 中的字节写入缓冲区的当前位置
>
> - put(int index, byte b)：将指定字节写入缓冲区的索引位置(不会移动position)

如：存入一个字符串  

```java
String str = "abc";
buf.put( str.getBytes() );
```

##### 7.3.2 get() ---获取缓冲区的数据。
> - get() ：读取单个字节
>
> - get(byte[] dst)：批量读取多个字节到dst 中
>
> - get(int index)：读取指定索引位置的字节(不会移动position)
>
> **步骤：**  
> 第一步：先切换到读数据模式    buf.flip();
>
> 第二步：利用get()方法读取缓冲区的数据。   

##### 7.3.3 操作缓冲区的其他方法：
> - buf.rewind() ---返回到最开始读取数据的状态，可重复读取	
>
> - buf.clear() ---缓冲区的几大核心的指针的位置为初始状态，但数据没有被清除。
>
> - buf.hasRemaining ---获取缓冲区中可以操作的数据的数量(也就是缓冲区内还有多少数据存在)。
>
> - buf.isDirect() ---返回值为true，则为直接缓冲区；返回值为false，则为非直接缓冲区。


#### 7.4 缓冲区中的四个核心属性：
> - capacity ---容量，表示缓冲区中存储数据的容量，一旦声明就不能够改变。
> - limit ---界限，表示缓冲区总可以操作数据的大小(limit后面的数据不能够进行读写操作)
> - position ---位置，表示缓冲区中正在操作数据的位置。
> - mark ---标记，用于记录当前position的位置。通过 reset() 方法恢复到mark的位置。

![image](https://note.youdao.com/yws/api/personal/file/294A6C3C7B8543A8B9BD86FBF108A743?method=download&shareKey=d42efb4f97bd3d92bbe0c5d351eeb078)


#### 7.5 直接缓冲区与非直接缓冲区
##### 7.5.1 非直接缓冲区
通过 allocate() 方法分配的缓冲区，将缓冲区建立在JVM内存中。代码为：  

```
ByteBuffer buf = ByteBuffer.allocate(1024);
```
![image](https://note.youdao.com/yws/api/personal/file/CEA55A6DE86D44A7AB741A99A1713EDF?method=download&shareKey=1f1edddc49915f07001c534dec932fed)  

##### 7.5.2 直接缓存区
通过 allocateDirect() 方法直接分配缓冲区，将缓冲区建立在物理内存中，提高效率。代码为：  

```
ByteBuffer byteBuffer = ByteBuffer.allocateDirect(1024);
```
![image](https://note.youdao.com/yws/api/personal/file/1D67C18EE33F4E77A9B16D066F6DD571?method=download&shareKey=16b25547a2431211bc72cce25935b6ed)


### 8. 通道（Channel）
#### 8.1 简介
> 由 java.nio.channel 包定义的。Channel表示IO源与目标对象之间的连接通道。  
>
> Channel 类似于传统的“流”，但是Channel本身无法访问数据，只能与Buffer进行交互。

#### 8.2 作用
> ​	通道Channel主要适用于源节点与目标节点的连接。在 NIO 中负责缓冲区中的数据的传输工作。Channel本身不存储数据，需要配合缓冲区Buffer来实现数据的传输。

#### 8.3 通道的主要实现类：
> java.nio.channels.Channel 接口：  
>       |---FileChannel   
>       |---SocketChannel   
>       |---ServerSocketChannel  
>       |---DataGramChannel  


#### 8.4 获取通道的三种方法：
> **方法1：**
>
> java针对 支持通道的类 提供了 getChannel() 方法来获取通道。  
>
> 支持通道的类有：   
> 本地 IO：FileInputStream / FileOutPutStream / RandomAccessFile  
> 网络 IO：Socket / ServerSocket / DaragramSocket
>
> **方法2：**
>
> 在JDK1.7中的 NIO.2 针对各个通道提供了静态方法 open() 来获取通道。
>
> **方法3：**
>
> 在JDK1.7中的NIO.2中的 Files 工具类的 newByteChannel() 来获取通道。


#### 8.5 通道与缓冲区的协作操作数据：

##### 8.5.1 直接缓冲区与channel合作操作数据
```java
/**
	 * 使用直接缓冲区与channel合作操作数据，程序更加迅速
	 * @throws IOException
	 */
	@Test
	public void test2() throws IOException{//2127-1902-1777
		long start = System.currentTimeMillis();
		
		FileChannel inChannel = FileChannel.open(Paths.get("d:/1.mkv"), StandardOpenOption.READ);
		FileChannel outChannel = FileChannel.open(Paths.get("d:/2.mkv"), StandardOpenOption.WRITE, StandardOpenOption.READ, StandardOpenOption.CREATE);
		
		//内存映射文件
		MappedByteBuffer inMappedBuf = inChannel.map(MapMode.READ_ONLY, 0, inChannel.size());
		MappedByteBuffer outMappedBuf = outChannel.map(MapMode.READ_WRITE, 0, inChannel.size());
		
		//直接对缓冲区进行数据的读写操作
		byte[] dst = new byte[inMappedBuf.limit()];
		inMappedBuf.get(dst);
		outMappedBuf.put(dst);
		
		inChannel.close();
		outChannel.close();
		
		long end = System.currentTimeMillis();
		System.out.println("耗费时间为：" + (end - start));
	}
```

##### 8.5.2 非直接缓冲区与channel合作操作数据

```java
/**
	 * 非直接缓冲区来与channel合作，创建通道与缓冲区，实现数据的操作
	 */
	@Test
	public void test1 ()
	{
		//创建能够使用通道的类
		FileInputStream fis = null;
		FileOutputStream fos = null;
		//得到通道
		FileChannel inchannel = null;
		FileChannel outchannel = null;
		try {
			fis = new FileInputStream("1.jpg");
			fos = new FileOutputStream("2.jpg");
			
			inchannel = fis.getChannel();
			outchannel = fos.getChannel();
			
			//创建缓冲区
			ByteBuffer buf = ByteBuffer.allocate(10240);
			
			//将通道的数据写入到缓冲区中
			while(inchannel.read(buf) != -1)
			{
				outchannel.write(buf);
				// 清空缓冲区
				buf.clear();  
			}
		} catch (IOException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		} finally {
			if( outchannel != null)
			{
				try {
					outchannel.close();
				} catch (IOException e) {
					// TODO Auto-generated catch block
					e.printStackTrace();
				}
			}
			if (inchannel != null)
			{
				try {
					inchannel.close();
				} catch (IOException e) {
					// TODO Auto-generated catch block
					e.printStackTrace();
				}
			}
			if ( fos != null )
			{
				try {
					fos.close();
				} catch (IOException e) {
					// TODO Auto-generated catch block
					e.printStackTrace();
				}
			}
			if( fis != null )
			{
				try {
					fis.close();
				} catch (IOException e) {
					// TODO Auto-generated catch block
					e.printStackTrace();
				}
			}
		}	
	}
```

#### 8.6 通道之间的数据传输：
##### 8.6.1 使用 TransferFrom() 与 TransferTo() 方法

```
@Test
	public void test3() throws IOException{
		long start = System.currentTimeMillis();
		
		FileChannel inChannel = FileChannel.open(Paths.get("e:/1.jpg"), StandardOpenOption.READ);
		FileChannel outChannel = FileChannel.open(Paths.get("e:/2.jpg"), StandardOpenOption.WRITE, StandardOpenOption.READ, StandardOpenOption.CREATE);
		
		//将inChannel通道的数据传输到outChannel通道中。
		//inChannel.transferTo(0, inChannel.size(), outChannel);
		
		//将outChannel通道的数据传输到inChannel通道中。
		outChannel.transferFrom(inChannel, 0, inChannel.size());
		
		inChannel.close();
		outChannel.close();
		
		long end = System.currentTimeMillis();
		System.out.println("耗费时间为：" + (end - start));
	}
```


#### 8.7 分散(Scatter)与聚集(Gather)
##### 8.7.1 分散读取(Scatter Reads)：
将通道中的数据按照顺序，依次分散到多个缓冲区中。  
![image](https://note.youdao.com/yws/api/personal/file/7F0ABAB31068430F8BB4866EBD972271?method=download&shareKey=c2e3503a7351acebac59e24f3f0bd816)

##### 8.7.2 聚集写入(Gathering Writes)：
将多个缓冲区中的数据聚集到通道中。  
![image](https://note.youdao.com/yws/api/personal/file/CD06F78BD9E54F3E884827788A1A18A6?method=download&shareKey=819d04ac8223410094b67815ac729506)

##### 代码操作为：

```java
/**
* 对分散于聚集
*/
@Test
	public void test4() throws IOException{
		RandomAccessFile raf1 = new RandomAccessFile("1.jpg", "rw");
		
		//1. 获取通道
		FileChannel channel1 = raf1.getChannel();
		
		//2. 分配指定大小的缓冲区
		ByteBuffer buf1 = ByteBuffer.allocate(100);
		ByteBuffer buf2 = ByteBuffer.allocate(1024);
		
		//3. 分散读取
		ByteBuffer[] bufs = {buf1, buf2};
		channel1.read(bufs);
		
		for (ByteBuffer byteBuffer : bufs) {
			byteBuffer.flip();
		}
		
		System.out.println(new String(bufs[0].array(), 0, bufs[0].limit()));
		System.out.println("-----------------");
		System.out.println(new String(bufs[1].array(), 0, bufs[1].limit()));
		
		//4. 聚集写入
		RandomAccessFile raf2 = new RandomAccessFile("3.jpg", "rw");
		FileChannel channel2 = raf2.getChannel();
		
		channel2.write(bufs);
	}
```

#### 8.8 字符集：Charset
编码：字符串--->字节数组

解码：字节数组--->字符串


```
@Test
	public void test6() throws IOException{
		Charset cs1 = Charset.forName("GBK");
			
		//获取编码器
		CharsetEncoder ce = cs1.newEncoder();
			
		//获取解码器
		CharsetDecoder cd = cs1.newDecoder();
		
		CharBuffer cBuf = CharBuffer.allocate(1024);
		cBuf.put("尚硅谷威武！");
		cBuf.flip();
			
		//编码
		ByteBuffer bBuf = ce.encode(cBuf);
			
		for (int i = 0; i < 12; i++) {
			System.out.println(bBuf.get());
		}
			
		//解码
		bBuf.flip();
		CharBuffer cBuf2 = cd.decode(bBuf);
		System.out.println(cBuf2.toString());
			
		System.out.println("------------------------------------------------------");
			
		Charset cs2 = Charset.forName("GBK");
		bBuf.flip();
		CharBuffer cBuf3 = cs2.decode(bBuf);
		System.out.println(cBuf3.toString());
	}
```


### 9. 选择器Selector
##### 简介：
选择器允许一个单独的线程来见识多个通道，也即是将多个通道注册并使用一个选择器，来进行通道读写的监控，选择器监视到通道正在进行读写的操作的时候，就会为这个通道提供线程来解决通道的读写请求。这样子来实现一个线程来管理多个通道。

这有一幅示意图，描述了单线程处理三个channel的情况：  
![image](https://note.youdao.com/yws/api/personal/file/8973994CF1C340E485D90EFAE8F49A89?method=download&shareKey=4da3d2eebddffca077f668f9992cdcfe)

##### 创建与开启Selector：

```java
//创建selector
Selector selector = Selector.open();
//开启非阻塞IO
channel.configureBlocking(false);
SelectionKey key = channel.register(selector, Selectionkey.OP_READ);

```

### 10. 选择器、通道、Buffer的组合实例
#### 10.1 实例一：

```java
//客户端
	@Test
	public void client() throws IOException{
		//1. 获取通道
		SocketChannel sChannel = SocketChannel.open(new InetSocketAddress("127.0.0.1", 9898));
		
		//2. 切换非阻塞模式
		sChannel.configureBlocking(false);
		
		//3. 分配指定大小的缓冲区
		ByteBuffer buf = ByteBuffer.allocate(1024);
		
		//4. 发送数据给服务端
		Scanner scan = new Scanner(System.in);
		
		while(scan.hasNext()){
			String str = scan.next();
			buf.put((new Date().toString() + "\n" + str).getBytes());
			buf.flip();
			sChannel.write(buf);
			buf.clear();
		}
		
		//5. 关闭通道
		sChannel.close();
	}

	//服务端
	@Test
	public void server() throws IOException{
		//1. 获取通道
		ServerSocketChannel ssChannel = ServerSocketChannel.open();
		
		//2. 切换非阻塞模式
		ssChannel.configureBlocking(false);
		
		//3. 绑定连接
		ssChannel.bind(new InetSocketAddress(9898));
		
		//4. 获取选择器
		Selector selector = Selector.open();
		
		//5. 将通道注册到选择器上, 并且指定“监听接收事件”
		ssChannel.register(selector, SelectionKey.OP_ACCEPT);
		
		//6. 轮询式的获取选择器上已经“准备就绪”的事件
		while(selector.select() > 0){
			
			//7. 获取当前选择器中所有注册的“选择键(已就绪的监听事件)”
			Iterator<SelectionKey> it = selector.selectedKeys().iterator();
			
			while(it.hasNext()){
				//8. 获取准备“就绪”的是事件
				SelectionKey sk = it.next();
				
				//9. 判断具体是什么事件准备就绪
				if(sk.isAcceptable()){
					//10. 若“接收就绪”，获取客户端连接
					SocketChannel sChannel = ssChannel.accept();
					
					//11. 切换非阻塞模式
					sChannel.configureBlocking(false);
					
					//12. 将该通道注册到选择器上
					sChannel.register(selector, SelectionKey.OP_READ);
				}else if(sk.isReadable()){
					//13. 获取当前选择器上“读就绪”状态的通道
					SocketChannel sChannel = (SocketChannel) sk.channel();
					
					//14. 读取数据
					ByteBuffer buf = ByteBuffer.allocate(1024);
					
					int len = 0;
					while((len = sChannel.read(buf)) > 0 ){
						buf.flip();
						System.out.println(new String(buf.array(), 0, len));
						buf.clear();
					}
				}
				
				//15. 取消选择键 SelectionKey
				it.remove();
			}
		}
	}
```

#### 10.2 实例二：

```java
@Test
	public void send() throws IOException{
		DatagramChannel dc = DatagramChannel.open();
		
		dc.configureBlocking(false);
		
		ByteBuffer buf = ByteBuffer.allocate(1024);
		
		Scanner scan = new Scanner(System.in);
		
		while(scan.hasNext()){
			String str = scan.next();
			buf.put((new Date().toString() + ":\n" + str).getBytes());
			buf.flip();
			dc.send(buf, new InetSocketAddress("127.0.0.1", 9898));
			buf.clear();
		}
		
		dc.close();
	}
	
	@Test
	public void receive() throws IOException{
		DatagramChannel dc = DatagramChannel.open();
		
		dc.configureBlocking(false);
		
		dc.bind(new InetSocketAddress(9898));
		
		Selector selector = Selector.open();
		
		dc.register(selector, SelectionKey.OP_READ);
		
		while(selector.select() > 0){
			Iterator<SelectionKey> it = selector.selectedKeys().iterator();
			
			while(it.hasNext()){
				SelectionKey sk = it.next();
				
				if(sk.isReadable()){
					ByteBuffer buf = ByteBuffer.allocate(1024);
					
					dc.receive(buf);
					buf.flip();
					System.out.println(new String(buf.array(), 0, buf.limit()));
					buf.clear();
				}
			}
			
			it.remove();
		}
	}
```



### 11. 阻塞IO--传统IO 与 非阻塞IO--NIO   主要是用来进行网络通信
#### 11.1 阻塞IO：
在传统的IO中，服务端与客户端之间的读写请求的线程一直被占用，线程一直连通着，当客户端发送读写到服务端的时候，线程才会被使用，其他时候线程是一直被占用，等待下一次的读写请求，而不能执行其他的操作；线程的阻塞意味着资源的大量浪费，造成性能下降。
解决办法：使用多线程来解决少量的客户端请求。

#### 11.2 非阻塞IO：
Java NIO是非阻塞式的。当线程从某通道进行读写数据时，如果没有其他读写操作的请求时，该线程可以执行其他线程。线程通常将非阻塞式IO的空闲时间用于其他通道的IO操作，实现单线程管理多个通道的功能。  
**实现原理：**  
非阻塞式IO是使用 选择器 来操作的。所有的通道都注册到选择器上，来实现对通道状况的监控。再由选择器来为需要进行读写操作的通道分配线程来执行读写操作。

![image](https://note.youdao.com/yws/api/personal/file/2BA5DC94EFC54206A98795B10B6F51CB?method=download&shareKey=c4066e6b3d8053c9489e66c2e1c052cb)

### 12. 同步IO 与 异步IO
实际上同步IO与异步IO是针对应用程序的交互而言的。

#### 12.1 同步IO：
线程启动一个IO操作以后立即进入等待状态，直到IO操作完成以后才来继续执行其他操作(导致请求进程阻塞，直到IO操作完成)。-常用于较小的IO操作。
#### 12.2 异步IO：
线程发送一个请求到内核以后，不需要等待IO的完成即可进入后面的操作，而内核进行IO操作完成以后，就会通知线程IO已经执行完毕(不导致进程阻塞)。-常用于较大的IO操作。

### 13. 多路复用IO
多路复用IO也是阻塞IO。select/epoll的好处就在于单个process就可以同时处理多个网络连接的IO。它的基本原理是select/epoll这个函数会不断轮询所负责的IO操作，当某个IO操作有数据到达时，就通知用户进程。然后由用户进程去操作IO。