[TOC]
# java设计模式

## 一、设计模式概述
### 1、设计模式的类型
- 创建型模式（五种）：工厂方法模式、抽象工厂模式、单例模式、建造者模式、原型模式

- 结构型模式（七种）：适配器模式、装饰器模式、代理模式、外观模式、桥接模式、组合模式、享元模式
- 行为型模式（十一种）：策略模式、模板方法模式、观察者模式、迭代子模式、责任链模式、命令模式、备忘录模式、访问者模式、中介者模式、解释器模式

### 2、创建型模式

<font color=red size=3>Singleton--单例模式</font>：保证一个类只有一个实例，并提供一个访问它的全局访问点。

<font color=red size=3>Abstract Factory--抽象工厂模式</font>：提供一个创建一系列相关或者相互依赖对象的接口，而无需指定他们的具体实现类。

<font color=red size=3>Factory Method--工厂方法模式</font>：定义一个用于创建对象的接口，让子类决定实例化哪一个类，Factory Method使一个类的实例化延迟加载到子类中。

<font color=red size=3>Builder--建造者模式</font>：将一个复杂对象的构建与染得表示相分离，是的相同的构建过程可以创建不同的表示。

Prototype 原型模式：用原型实例指定创建对象的种类，并且通过拷贝这些原型来创建	新的对象。

### 3、结构型模式

Composite 组合模式：将对象组合成树形结构以表示部分整体的关系，composite使得用户对单个对象和组合对象的使用具有一致性。

<font color=red size=3>Facade--外观模式</font>：为子系统的一组接口提供一致的界面，facade提供了高层接口，这给借口让子系统更容易被使用。

<font color=red size=3>Proxy--代理模式</font>：为其他对象提供一种代理以控制这个对象的访问。

<font color=red size=3>Adapter--适配器模式</font>：将一类接口转换为客户希望的另一个接口，Adapter使得原来由于接口不兼容而不能一起工作的类可以一起执行。

<font color=red size=3>Decrator--装饰模式</font>：动态的给对象增加一些额外的职责，或者抽取出相同的功能来增强。

<font color=red size=3>Bridge--桥接模式</font>：将抽象部分与它显示部分相分离，使他们可以独立的变化。

Flyweight 享元模式：

### 4、行为型模式

Iterator 迭代器模式：提供一个方法熟顺序来访问一个聚合对象的各个元素，而不需要暴露该对象的内部表示。

<font color=red size=3>Observer--观察者模式</font>：定义对象间一对多的依赖关系，当一个对象的状态发生改变时，所有依赖于它的对象都会得到通知。

<font color=red size=3>Template Method--模板方法模式</font>：定义一个算法的骨架，而将一些步骤延迟到子类中，使得子类可以不改变一个算法的结构就能重新定义算法的特定步骤。

Commond 命令模式：将一个请求封装为一个对象，从而你可以使不同的请求对客户进行参数化，对请求排队和记录请求日志，以及支持可撤销得我操作。

State 状态模式：用于对象在其内部状态改变时改变他的行为。

Strategy 策略模式：定义一系列的算法，把他们一个个的封装起来，并且让其能够相互替换。策略模式能够使得算法可以独立于使用他们的客户。

职责链模式：使多个对象都有机会处理请求，从而避免了请求发送者与请求接受者之间的耦合关系。

Mediator 中介者模式：用一个中介对象封装一系列的对象交互。

Visitor 访问者模式：表示一个作用与某个对象结构中的各个元素间的操作，它是你可以再不改变各元素类的前提下定义作用域这个元素的新操作。

Interpreter 解释器模式：给定一个语言，定义他的语法的表示，并给定一个解释器，用这个解释器来杰斯语句中的句子。

Memento 备忘录模式：在不破坏前提的情况下，捕获一个对象的内部转改，并在该对象之外保存这个状态。

---

## 二、设计模式遵循的六大原则
#### 总原则：开闭原则
​	对扩展开放，对修改关闭。程序在需要扩展的是时候，不去修改源代码，而是去执行源代码的扩展，实现一个热插拔的效果。

#### 1、单一职责原则
每个类应该实现单一的职责(功能)，如果不然，需将这个类进行拆分。

#### 2、里氏替换原则(LSP)
​	里氏替换原则是面向对象设计的基本原则之一。里氏替换原则的中定义，任何类可以出现的地方，子类一定可以出现。LSP是继承复用的基石，只有当延伸类可以替换掉基类，且软件功能不受影响的情况下，基类才能实现真正的复用。相当于是 开闭原则 的补充。
#### 3、依赖倒转原则
体现在面向接口编程，依赖与抽象类而不依赖于具体的实现类。写代码需要用到具体的实现类，可直接依赖接口进行交互。
#### 4、接口隔离原则
接口中不应该存在子类用不到却必须要实现的方法。所以，接口必须要进行拆分。
#### 5、迪米特法则(最少知道法则)
一个类对自己依赖的类越少越好，降低耦合。
#### 6、合成复用原则
原则时尽量使用合成/聚合的方式，而不是使用继承。

---

## 三、具体设计模式
### 1、工厂方法模式
#### 原理：
工厂方法模式中，创建一个工厂接口，以及为每一个需要被创建的类创建一个工厂方法类，多个工厂方法类实现工厂接口，来实现更好的扩展。  
类比为：一个吃饭的功能，鸡、鸭、狗、人吃饭的方式都不同，那么我们就需要来分别为每种生物创建一个类来实现吃饭的功能。这个时候，如果有个统一的接口，来调用不同的实现类来创建对应的生物物种，那么就是使用工厂方法模式。
![image](https://note.youdao.com/yws/api/personal/file/3C2A999A1F7540B8A28221FC679D6410?method=download&shareKey=9d79f25f052f6de8331010932e7537e7)

#### 实现步骤：
创建一个sender接口，让MailSender与SMSSender来实现Sender接口。在创建一个工厂接口Provider，然后创建与Sender实现类相同个数的类来实现Provider接口。

1）创建Sender接口：

```java
public interface Sender { 
    public void Send();  
    
}   
```
2）创建Sender接口的第一个实现类：

```java
public class MailSender implements Sender { 
     @Override
     public void Send() { 
     System.out.println("this is mailsender!");  
     }  
} 
```

3、创建Sender接口的第二个实现类：

```java
public class SMSSender implements Sender { 
     @Override
     public void Send() { 
     System.out.println("this is mailsender!");  
    }  
    
} 
```
4）创建Provider接口：

```java
public interface Provider {  
    public Sender produce(); 
    
}
```
5）创建Provider工厂接口的第一个实现类：

```java
public class SendMailFactory implements Provider {
    @Override
    public Sender produce(){ 
    return new MailSender(); 
    }
    
}
```

6）创建Provider工厂接口的第二个实现类：

```java
public class SendSMSFactory implements Provider {
    @Override
    public Sender produce(){ 
    return new MailSender(); 
    }
    
}
```

7、测试方法：

```java
public static void main(String[] args) {
    Provider provider = new SendMailFactory();
    Sender sender = provider.produce(); 
    sender.Send(); 
}
```

#### 优点：
- 当需要创建更多的生物物种的吃饭功能的时候，只需要创建一个类来实现吃饭的接口，在创建一个实现了工厂的接口的类即可，能够在不修改源代码的前提下对类进行扩展。

- 一个抽象产品类，能够扩展多个具体产品类。
- 一个抽象工厂类，能够派生多个具体工厂类。
- 一个工厂类只能够对应一个具体产品类。


### 2、抽象工厂模式
#### 简介：
抽象工厂模式中，一个抽象工厂实现类中，定义了多个同一类的产品，而创建产品是根据传入的参数来创建不同的产品。
#### 代码实现：
步骤一：创建一个颜色接口

```java
public interface Color {
    void fill();
}
```
第二步：创建多个颜色接口的实现类

```javascript
public class Red implements Color {
@Override
public void fill() {
System.out.println("Inside Red::fill() method.");
}
}
```

```java
public class Green implements Color {
@Override
public void fill() {
System.out.println("Inside Green::fill() method.");
}
}
```

```java
public class Blue implements Color {
@Override
public void fill() {
System.out.println("Inside Blue::fill() method.");
}
}
```
第三步：为这三个同一类产品创建一个抽象工厂：

```java
public abstract class AbstractFactory {
    public abstract Color getColor(String color);

}
```
第四步:创建抽象工厂的实现类

```java
public class ColorFactory extends AbstractFactory {

    @Override
    Color getColor(String color) {
        if(color == null){
        return null;
        }
        
        if(color.equalsIgnoreCase("RED"))
        {
            return new Red();
        } 
        
        else if(color.equalsIgnoreCase("GREEN"))
        {
            return new Green();
        } 
        
        else if(color.equalsIgnoreCase("BLUE"))
        {
            return new Blue();
        }
        
        return null;
    }
}
```
第五步：测试代码

```java
public class AbstractFactoryPatternDemo {
public static void main(String[] args) {

    ColorFactory colorFactory = new ColorFactory();
    //获取颜色为 Red 的对象
    Color color1 = colorFactory.getColor("RED");
    //调用 Red 的 fill 方法
    color1.fill();
    //获取颜色为 Green 的对象
    Color color2 = colorFactory.getColor("Green");
    //调用 Green 的 fill 方法
    color2.fill();
    //获取颜色为 Blue 的对象
    Color color3 = colorFactory.getColor("BLUE");
    //调用 Blue 的 fill 方法
    color3.fill();
    }
}
```
#### 优势：
- 在同一个工厂类中根据传入的参数来选择创建什么样的对象。  

- 在一个抽象工厂中，能够有多个抽象方法，来实现多个类型的产品的选择，再根据传入的参数调用对应的代码来创建对象。
- 每一个具体的工厂类能够创建一个具体的产品实例。
- 每个抽象工厂类，能够有多个不同的具体工厂类。



### 3、单例模式
#### 简介：
在程序的运行过程中，对一个类的多次调用的话，重复类的 创建-> 调用 -> 销毁 的过程，对内存的开销大，导致程序运行过慢。

而使用单例模式的话，就是将一个类在程序的启动或者调用这个类的时候创建这个类的对象，而这个类的对象会一直存在，每次对这个类的是由只需要去调用创建的这个单例对象即可，在程序运行结束的时候被销毁。 

#### 代码实现：
创建饿汉式单例模式一：

```java
public class SingleDemo {
	
	private static SingleDemo single = null;
	
	private SingleDemo() {}
	
	public static SingleDemo getSingleDemo() {
		single = new SingleDemo();
		return single;
	}
}
```

创建饿汉式单例模式二：
```java
public class SingleDemo {
    //第一：先将构造器私有化，实现外界无法创建这个类的对象
    //第二：使用static修饰，让一个静态变量来接收这个实例化对象
    //第三：使用public修饰，让这个静态变量能够向外提供这个实例
    //第四：使用final修饰，更加明确地表示这个是一个单例
    public static final SingleDemo single = new SingleDemo();

    private SingleDemo(){

    }
}
```

创建饿汉式单例模式三：
```java
/**
 * 创建一个变量名为 single 的单例枚举类
 */
public enum SingleDemo1 {
    single
}

//调用这个每句单例类
public static void main(String[] args) {
    SingleDemo1 demo1 = SingleDemo1.single;
}
```


创建懒汉式单例模式方法一：这种双重锁机制能够实现只有在第一次需要同步，创建以后不再需要同步
```java
private static volatile SingleDemo single = null;
	
	private SingleDemo() {}
	
	public static SingleDemo getSingleDemo() {
		
		if(single == null) {
			synchronized (SingleDemo.class) {
				if(single == null) {
					single = new SingleDemo();
				}
			}
		}
		return single;
	}
```
创建懒汉式单例模式方法二：效率较低
```java
	private static volatile SingleDemo single = null;
	
	private SingleDemo() {}
	
	public static synchronized SingleDemo getSingleDemo() {
		
		if(single == null) {
			single = new SingleDemo();
		}
		return single;
	}
```

创建懒汉式单例模式方法三：使用静态内部类来进行实例化对象
```java
public class SingleDemo2 {
    private SingleDemo2(){

    }

    private static class Inner{
        private static final SingleDemo2 single = new SingleDemo2();
    }

    public static SingleDemo2 newInstance(){
        return Inner.single;
    }
}
```


### 4 建造者模式
#### 4.1 简介：
建造者模式是使用多个简单的对象来一步步的创建一个复杂的对象。它提供了一种非常优雅的创建对象的方式。  
也就是说，一个Builder类会一步一步构造最终的对象，该Builder类是独立于其他对象的。

#### 4.2 使用场景：
主要是将一个复杂的构建过程拆分，当一个构建过程会经常面临着变化的元素，使用构造者模式将其组合起来的话，能够实现相同的构建也能构建出不同的表示。例如：肯德基的食谱不会变，但是各项菜单的组合是经常变换的。

#### 4.3 代码执行：
步骤 1  
创建一个表示食物条目和食物包装的接口。

```java
Item.java
public interface Item {
public String name();
public Packing packing();
public float price();
}
Packing.java
public interface Packing {
public String pack();
}
```

步骤 2  
创建实现 Packing 接口的实体类。


```java
Wrapper.java
public class Wrapper implements Packing {
@Override
public String pack() {
return "Wrapper";
}
}
```


```java
Bottle.java
public class Bottle implements Packing {
@Override
public String pack() {
return "Bottle";
}
}
```

步骤 3   
创建实现 Item 接口的抽象类，该类提供了默认的功能。


```java
Burger.java
public abstract class Burger implements Item {
@Override
public Packing packing() {
return new Wrapper();
}
@Override
public abstract float price();
}
```


```java
ColdDrink.java
public abstract class ColdDrink implements Item {
@Override
public Packing packing() {
return new Bottle();
}
@Override
public abstract float price();
}
```

步骤 4  
创建扩展了 Burger 和 ColdDrink 的实体类。


```java
VegBurger.java
public class VegBurger extends Burger {
@Override
public float price() {
return 25.0f;
}
@Override
public String name() {
return "Veg Burger";
}
}
```


```java
ChickenBurger.java
public class ChickenBurger extends Burger {
@Override
public float price() {
return 50.5f;
}
@Override
public String name() {
return "Chicken Burger";
}
}
```


```java
Coke.java
public class Coke extends ColdDrink {
@Override
public float price() {
return 30.0f;
}
@Override
public String name() {
return "Coke";
}
}
```


```java
Pepsi.java
public class Pepsi extends ColdDrink {
@Override
public float price() {
return 35.0f;
}
@Override
public String name() {
return "Pepsi";
}
}
```

步骤 5  
创建一个 Meal 类，带有上面定义的 Item 对象。


```java
Meal.java
import java.util.ArrayList;
import java.util.List;
public class Meal {
private List<Item> items = new ArrayList<Item>();
public void addItem(Item item){
items.add(item);
}
public float getCost(){
float cost = 0.0f;
for (Item item : items) {
cost += item.price();
}
return cost;
}
public void showItems(){
for (Item item : items) {
System.out.print("Item : "+item.name());
System.out.print(", Packing : "+item.packing().pack());
System.out.println(", Price : "+item.price());
}
}
}
```

步骤 6  
创建一个 MealBuilder 类，实际的 builder 类负责创建 Meal 对象。


```java
MealBuilder.java
public class MealBuilder {
public Meal prepareVegMeal (){
Meal meal = new Meal();
meal.addItem(new VegBurger());
meal.addItem(new Coke());
return meal;
}
public Meal prepareNonVegMeal (){
Meal meal = new Meal();
meal.addItem(new ChickenBurger());
meal.addItem(new Pepsi());
return meal;
}
}
```

步骤 7  
BuiderPatternDemo 使用 MealBuider 来演示建造者模式（Builder Pattern）。


```java
BuilderPatternDemo.java

public class BuilderPatternDemo {

public static void main(String[] args) {

    MealBuilder mealBuilder = new MealBuilder();
    
    Meal vegMeal = mealBuilder.prepareVegMeal();
    
    System.out.println("Veg Meal");
    
    vegMeal.showItems();
    
    System.out.println("Total Cost: " +vegMeal.getCost());
    
    Meal nonVegMeal = mealBuilder.prepareNonVegMeal();
    
    System.out.println("\n\nNon-Veg Meal");
    
    nonVegMeal.showItems();
    
    System.out.println("Total Cost: " +nonVegMeal.getCost());
    }
}
```

步骤 8  
执行程序，输出结果：


```java
Veg Meal
Item : Veg Burger, Packing : Wrapper, Price : 25.0
Item : Coke, Packing : Bottle, Price : 30.0
Total Cost: 55.0


Non-Veg Meal
Item : Chicken Burger, Packing : Wrapper, Price : 50.5
Item : Pepsi, Packing : Bottle, Price : 35.0
Total Cost: 85.5
```


### 5 代理模式
#### 5.1 简介：
>  	一个类代表另外一个类的功能。也就是在代理类中创建被代理类对象，来实现在代理类中对被代理类的方法的调用。

#### 5.2 概念：

- 真实对象：需要进行功能扩展的对象。
- 真实方法：需要进行功能扩展的方法。
- 代理对象：功能扩展以后的对象。
- 代理方法：功能扩展以后的方法。

#### 5.3 优缺点：

> - **优点**：
>   - 职责清晰，高扩展性、智能化。能够在不修改源码的基础之上完成功能扩展。
>   - 能够在一定的情况下，提供一部分的安全控制等。
>
> - **缺点**：
>   - 客户端与真实主题之间增加了代理对象，因此有些类型的代理模式会变慢。

#### 5.4 静态代理

> ​	 由程序员自己编写的代理对象与代理方法称为静态代理方法。
>
> ​	代理设计模式中，能够在不修改真实方法的源码基础之上进行功能的扩展，但是我们的需要改变调用方式由直接调用真实对象转换为直接调用代理对象，再由代理对象去调用真实对象，实现一个功能扩展的功能。**通常情况下，代理方法与真实方法的设计，形参与返回值是与源代码的形参与返回值相同的。** 可以进行规范性的操作：
>
> - 代理方法与真实方法实现相同的接口。
> - 代理方法继承真实方法。

##### 5.4.1 代码操作一：
步骤 1  
创建一个接口。


```java
Image.java
public interface Image {
void display();
}
```

步骤 2  
创建实现接口的代理对象与被代理对象。


```java
RealImage.java
public class RealImage implements Image {
    private String fileName;
    public RealImage(String fileName){
    this.fileName = fileName;
    loadFromDisk(fileName);
    }
    @Override
    public void display() {
    System.out.println("Displaying " + fileName);
    }
    private void loadFromDisk(String fileName){
    System.out.println("Loading " + fileName);
    }
}
```


```java
ProxyImage.java
public class ProxyImage implements Image{
    private RealImage realImage;
    private String fileName;
    public ProxyImage(String fileName){
    this.fileName = fileName;
    }
    @Override
    public void display() {
    if(realImage == null){
    realImage = new RealImage(fileName);
    }
    realImage.display();
    }
}
```

步骤 3  
当被请求时，使用 ProxyImage 来获取 RealImage 类的对象。


```java
ProxyPatternDemo.java
public class ProxyPatternDemo {
    public static void main(String[] args) {
    Image image = new ProxyImage("test_10mb.jpg");
    // 图像将从磁盘加载
    image.display();
    System.out.println("");
    // 图像不需要从磁盘加载
    image.display();
    }
}
```

##### 5.4.2 代码操作二：

1）创建接口

```java
public interface Sourceable {  
    public void method(); 
    
}
```

2）创建被代理类  

```java
public class Source implements Sourceable {
    @Override
     public void method() {
    System.out.println("the original method!");
     } 
}
```

3）创建代理类，里面创建被代理类的对象。

```java
public class Proxy implements Sourceable {
    private Source source;
    public Proxy(){
    	super();
    	this.source = new Source();
    }
    @Override
    public void method() {
    	before();
    	source.method();
    }
    private void before() {
    	System.out.println("before proxy!");
    }
}
```

#### 5.5 JDK动态代理--基于接口

##### 5.5.1 基本介绍

> - **java.lang.reflect.Proxy** ：用于创建代理对象。
> - **InvocationHandler**：JDK动态代理动态创建的代理对象须要实现这个接口。

##### 5.5.2 实现流程

> - 创建一个实现了InvocationHandler接口，并重写了 invoke 方法，实现代理的功能。
> - 使用Proxy调用其newProxyIntance() 方法完成代理对象和代理方法的动态生成。
> - 调用动态生成的代理对象来代理完成业务操作。

##### 5.5.3 代码实现

第一步

创建实现真实功能需要的接口：

```java
public interface Gongneng {
	void eat();
}
```

第二步：创建真实功能的实现：

```java
public class Boss implements Gongneng {
	public void eat() {
		System.out.println("这里是老板吃饭的功能");
	}
}
```

第三步：创建需要扩展的功能：

```java
public class Before {
	public void before() {
		System.out.println("前置before方法");
	}
}

public class After {
	public void after() {
		System.out.println("后置after方法");
	}
}
```

第四步

自定义动态代理的方法：

```java
public class JDKProxy implements InvocationHandler {
	/**
	 * 自定义动态代理的方法
	 * Object proxy : 代理对象。
	 * Method method ： 接口类型的切点方法。
	 * Object[] args ：代理方法接收的实参的数组。
	 * return ：一般直接返回真实业务逻辑完成以后的返回值。
	 */
	public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
		//前置方法
		Before before = new Before();
		before.before();
		
		//真正的业务逻辑
		Gongneng gongneng = new Boss();
		gongneng.eat();
		
		//后置方法
		After after = new After();
		after.after();
		return null;
	}
}
```

第五步

实现动态代理测试类

```java
import java.lang.reflect.Proxy;

public class TestProxy {
	public static void main(String[] args) {
		Gongneng gongneng = (Gongneng)Proxy.newProxyInstance(TestProxy.class.getClassLoader(),
				new Class[] {Gongneng.class}, new JDKProxy());
		gongneng.eat();
	}
}
```

#### 5.6 Cglib动态代理--基于继承

##### 5.6.1 基本介绍

> ​	Cgilb是基于继承实现的，底层是基于字节码文件反向动态生成代理对象的。是一种第三方包提供的动态代理方式。

##### 5.6.2 代码实现

第一步

创建实现真实功能需要的接口：

```java
public interface Gongneng {
	void eat();
}
```

第二步：创建真实功能的实现：

```java
public class Boss implements Gongneng {
	public void eat() {
		System.out.println("这里是老板吃饭的功能");
	}
}
```

第三步：创建需要扩展的功能：

```java
public class Before {
	public void before() {
		System.out.println("前置before方法");
	}
}

public class After {
	public void after() {
		System.out.println("后置after方法");
	}
}
```

第四步

创建Cglib动态代理的类

```java
public class CgilbProxy implements MethodInterceptor {
	public Object intercept(Object arg0, Method arg1, Object[] arg2, MethodProxy arg3) throws Throwable {
		//前置方法
		Before before = new Before();
		before.before();
		
		//真正的业务逻辑
		Boss boss = new Boss();
		boss.eat();
		
		//后置方法
		After after = new After();
		after.after();
		return null;
	}
}
```

第五步：测试调用

```java
import net.sf.cglib.proxy.Enhancer;

public class TestCglib {
	public static void main(String[] args) {
		//設置cglib的翻譯對象
		Enhancer en = new Enhancer();
		//設置需要继承的真实对象
		en.setSuperclass(Boss.class);
		//设置代理对象需要回调的方法
		en.setCallback(new CgilbProxy());
		//创建并返回创建出来的代理对象
		Boss boss = (Boss) en.create();
		boss.eat();
	}
}
```

### 6、外观模式

#### 简介：
外观模式是隐藏系统的复杂性，并向客户端提供了一个可以访问的系统的类型的接口，这样的设计就是外观模式。主要是用于屏蔽系统内部的复杂性。  
这种模式涉及到一个单一的类，该类提供了客户端请求的简化方法和对现有系统类方法的委托调用。

#### 作用：
为系统中的一组接口提供一个一致的界面，外观模式定义了一个高层接口，这个接口使得该系统更加容易使用。降低访问复杂系统的复杂度，简化操作。  
客户端不需要与系统耦合，外观类来与系统进行耦合的操作。  
能够减少系统之间的相互依赖，提高灵活度。
将多个类之间的依赖关系由外观类来控制。

#### 实现代码一：
步骤 1  
创建一个接口。


```
Shape.java
public interface Shape {
   void draw();
}
```

步骤 2  
创建实现接口的实体类。


```
Rectangle.java
public class Rectangle implements Shape {
 
   @Override
   public void draw() {
      System.out.println("Rectangle::draw()");
   }
}
```


```
Square.java
public class Square implements Shape {
 
   @Override
   public void draw() {
      System.out.println("Square::draw()");
   }
}
```


```
Circle.java
public class Circle implements Shape {
 
   @Override
   public void draw() {
      System.out.println("Circle::draw()");
   }
}
```

步骤 3  
创建一个外观类。


```
ShapeMaker.java
public class ShapeMaker {
   private Shape circle;
   private Shape rectangle;
   private Shape square;
 
   public ShapeMaker() {
      circle = new Circle();
      rectangle = new Rectangle();
      square = new Square();
   }
 
   public void drawCircle(){
      circle.draw();
   }
   public void drawRectangle(){
      rectangle.draw();
   }
   public void drawSquare(){
      square.draw();
   }
}
```

步骤 4  
使用该外观类画出各种类型的形状。


```
FacadePatternDemo.java
public class FacadePatternDemo {
   public static void main(String[] args) {
      ShapeMaker shapeMaker = new ShapeMaker();
 
      shapeMaker.drawCircle();
      shapeMaker.drawRectangle();
      shapeMaker.drawSquare();      
   }
}
```

#### 实现代码二：
1）创建多个类，分别有各自的方法

```
public class cat1{
    public void cat()
    {
    System.out.println("cat----satrt");
    }
}
```


```
public class dog1{
    public void dog()
    {
    System.out.println("dog----satrt");
    }
}
```

2)创建一个类，来将这两个类加入到一起。

```
public class anmail{
    private cat1 cat1;
    private dog1 dog1;
    public anmail()
    {
    cat1 = new cat1();
    dog1 = new dog1();
    }
    public void show ()
    {
    cat1.cat();
    dog1.dog();
    }
}
```


### 7、适配器模式
#### 简介：
适配器模式是解决两个不兼容的类的接口之间类的不匹配的问题，用于处理兼容性问题。主要分为：类的适配器模式、对象的适配器模式、接口的适配器模式

#### 1）类的适配器模式
实例：一个Source类，有一个方法，一个Taegetable接口，需要使用Adapter代理来将二者进行关联。

a：创建Source类：

```
public class Source { 
    public void method1() {
    System.out.println("this is original method!");
    }
    
}
```

b、创建接口Targetable：

```
public interface Targetable {
    public void method1();
    public void method2();
 }
```

c、创建适配器将二者关联上

```
public class Adapter extends Source implements Targetable { 
    @Override
    public void method2() {
        System.out.println("this is the targetable method!");
    }
}
```

d、测试用例

```
public static void main(String[] args) { 
    Targetable target = new Adapter();
    target.method1(); 
    target.method2(); 
}
```

#### 2）对象适配器模式
实例：一个Source类，有一个方法，一个Taegetable接口，需要使用Adapter代理来将二者进行关联。

a：创建Source类：

```
public class Source { 
    public void method1() {
    System.out.println("this is original method!");
    }
    
}
```

b、创建接口Targetable：

```
public interface Targetable {
    public void method1();
    public void method2();
 }
```

c、创建适配器将二者关联上

```
public class Adapter extends Source implements Targetable { 
    private Source source; 
    public Wrapper(Source source){
        super();
        this.source = source;   
    }
    
    @Override
    public void method1() {
        source.method1();
    }
    
    @Override
    public void method2() {
        System.out.println("this is the targetable method!");
    }
}
```

d、测试用例

```
public static void main(String[] args) { 
    Targetable target = new Adapter();
    target.method1(); 
    target.method2(); 
}
```

#### 3)接口适配器模式
场景：在实际开发中，实现接口就必须实现接口的所有方法，但是当我们实现所有的方法dehumidifier，容易造成资源浪费，所以我们可以引入接口的适配器模式，**借助于抽象类，让抽象类实现改接口，实现所有的方法，而我们就直接与抽象类打交道，写一个类来继承抽象类就行**。例如：Spring的WebMvcConfigurerAdapter抽象类就是实现了WebMvcConfigurer接口，然后当我们需要去实现某一个方法的时候，直接继承WebMvcConfigurerAdapter类即可。

a）创建接口：

```
public interface Sourceable {
 	public void method1(); 
    public void method2();  
}
```

b）创建接口适配器(抽象类)来实现接口：

```
public abstract class Wrapper2 implements Sourceable{
 public void method1(){}
 public void method2(){}  
}
```

c）当需要是调用某一个方法的时候，直接继承抽象类，重写里面需要操作的方法即可：

```
public class SourceSub2 extends Wrapper2 { 
    public void method2(){
        System.out.println("the sourceable interface's second Sub2!");
    }   
}
```


### 8、装饰模式
#### 简介：
装饰设计模式就是给一对象增加一些新的功能，而且是动态添加的。被装饰对象与装饰对象都需要实现同一个接口，装饰对象持有被装饰对象的实例。主要用于动态的扩展类的功能。

#### 代码操作：
步骤 1  
创建一个接口：


```
Shape.java
public interface Shape {
   void draw();
}
```

步骤 2  
创建实现接口的实体类。


```
Rectangle.java
public class Rectangle implements Shape {
 
   @Override
   public void draw() {
      System.out.println("Shape: Rectangle");
   }
}
```

```
Circle.java
public class Circle implements Shape {
 
   @Override
   public void draw() {
      System.out.println("Shape: Circle");
   }
}
```

步骤 3  
创建实现了 Shape 接口的抽象装饰类。


```
ShapeDecorator.java
public abstract class ShapeDecorator implements Shape {
   protected Shape decoratedShape;
 
   public ShapeDecorator(Shape decoratedShape){
      this.decoratedShape = decoratedShape;
   }
 
   public void draw(){
      decoratedShape.draw();
   }  
}
```

步骤 4   
创建扩展了 ShapeDecorator 类的实体装饰类。


```
RedShapeDecorator.java
public class RedShapeDecorator extends ShapeDecorator {
 
   public RedShapeDecorator(Shape decoratedShape) {
      super(decoratedShape);     
   }
 
   @Override
   public void draw() {
      decoratedShape.draw();         
      setRedBorder(decoratedShape);
   }
 
   private void setRedBorder(Shape decoratedShape){
      System.out.println("Border Color: Red");
   }
}
```

步骤 5   
使用 RedShapeDecorator 来装饰 Shape 对象。


```
DecoratorPatternDemo.java
public class DecoratorPatternDemo {
   public static void main(String[] args) {
 
      Shape circle = new Circle();
 
      Shape redCircle = new RedShapeDecorator(new Circle());
 
      Shape redRectangle = new RedShapeDecorator(new Rectangle());
      System.out.println("Circle with normal border");
      circle.draw();
 
      System.out.println("\nCircle of red border");
      redCircle.draw();
 
      System.out.println("\nRectangle of red border");
      redRectangle.draw();
   }
}
```

步骤 6   
执行程序，输出结果：


```
Circle with normal border
Shape: Circle

Circle of red border
Shape: Circle
Border Color: Red

Rectangle of red border
Shape: Rectangle
Border Color: Red
```


### 9、桥接模式
#### 简介：
桥接模式是用于把抽象化与现实解耦，使二者可以独立化，这类设计是实现虚拟雨现实化之间的桥梁，来实现二者之间的解耦。

#### 实现代码一：
步骤 1  
创建桥接实现接口。


```
DrawAPI.java
public interface DrawAPI {
public void drawCircle(int radius, int x, int y);
}
```

步骤 2  
创建实现了 DrawAPI 接口的实体桥接实现类。


```
RedCircle.java
public class RedCircle implements DrawAPI {
    @Override
    public void drawCircle(int radius, int x, int y) {
    System.out.println("Drawing Circle[ color: red, radius: "
    + radius +", x: " +x+", "+ y +"]");
    }
}
```


```
GreenCircle.java
public class GreenCircle implements DrawAPI {
    @Override
    public void drawCircle(int radius, int x, int y) {
    System.out.println("Drawing Circle[ color: green, radius: "
    + radius +", x: " +x+", "+ y +"]");
    }
}
```

步骤 3     
使用 DrawAPI 接口创建抽象类 Shape。


```
Shape.java
public abstract class Shape {
    protected DrawAPI drawAPI;
    protected Shape(DrawAPI drawAPI){
        this.drawAPI = drawAPI;
    }
    public abstract void draw();
}
```

步骤 4    
创建实现了 Shape 接口的实体类。


```
Circle.java
public class Circle extends Shape {
    private int x, y, radius;
    public Circle(int x, int y, int radius, DrawAPI drawAPI) {
        super(drawAPI);
        this.x = x;
        this.y = y;
        this.radius = radius;
    }
    public void draw() {
        drawAPI.drawCircle(radius,x,y);
    }
}
```

步骤 5  
使用 Shape 和 DrawAPI 类画出不同颜色的圆。


```
BridgePatternDemo.java
public class BridgePatternDemo {
    public static void main(String[] args) {
        Shape redCircle = new Circle(100,100, 10, new RedCircle());
        Shape greenCircle = new Circle(100,100, 10, new GreenCircle());
        redCircle.draw();
        greenCircle.draw();
    }
}
```

步骤 6  
执行程序，输出结果：


```
Drawing Circle[ color: red, radius: 10, x: 100, 100]
Drawing Circle[  color: green, radius: 10, x: 100, 100]
```

#### 示例代码二：
1）创建一个接口


```
public interface Sourceable { 
    public void method();
    
}
```

2）两个实现类

```
public class SourceSub1 implements Sourceable {
    @Override
    public void method() {
        System.out.println("this is the first sub!");
    }
}
```


```
public class SourceSub2 implements Sourceable {
    @Override
    public void method() {
        System.out.println("this is the two sub!");
    }
}
```

3)定义一个桥，里面有这个接口的实例

```
public abstract class Bridge {
    private Sourceable source;
    
    public void method(){   
        source.method();
    } 
    
    public Sourceable getSource() {
        return source;
    }
    
    public void setSource(Sourceable source) {
        this.source = source;
    }
}
```

4）创建一个桥的子类

```
public class MyBridge extends Bridge {

    public void method(){
        getSource().method(); 
    } 
} 
```
5）测试

```
public class BridgeTest {

    public static void main(String[] args) {
    
        Bridge bridge = new MyBridge();
        
        Sourceable source1 = new SourceSub1();
        bridge.setSource(source1);
        bridge.method();
        
        Sourceable source2 = new SourceSub2();
        bridge.setSource(source2);
        bridge.method();
    }
}
```


### 10、观察者模式
#### 简介：
当对象之间存在一对多的关系的情况下，使用观察者模式，能够实现当一个对象被修改，会立即通知其他对象说明该对象被修改了。

#### 作用：
当一个对象的状态发生改变，所有依赖的对象都将收到其发生变化的消息。  
使用的是面向对象化的技术，实现降低耦合的操作。  

#### 使用场景：
- 一个抽象模型有两个方面，其中一个方面依赖于另一个方面。将这些方面封装在独立的对象中使它们可以各自独立地改变和复用。

- 一个对象的改变将导致其他一个或多个对象也发生改变，而不知道具体有多少对象将发生改变，可以降低对象之间的耦合度。
- 一个对象必须通知其他对象，而并不知道这些对象是谁。
- 需要在系统中创建一个触发链，A对象的行为将影响B对象，B对象的行为将影响C对象……，可以使用观察者模式创建一种链式触发机制。

#### 实现代码：






### 11、模板方法模式
#### 简介：
在模板模式中，一个抽象类公开定义了执行它的方法的方式/模板，子类能够对其进行重写等操作。但是调用是将以抽象类中定义的方式与顺序进行执行。

也就是说，在一个抽象类中，有一个主方法，再定义多个方法（抽象方法、真实方法都行），能够实现按照主方法的执行顺序进行执行，且能够使用继承的方式进行扩展。

#### 代码示例：
1）创建一个抽象类：

```
public abstract class AbstractCalculator {

    public final int calculate(String exp,String opt) {
    int array[] = split(exp,opt);
    return calculate(array[0],array[1]);
    }
    
    abstract public int calculate(int num1,int num2);
    
    public int[] split(String exp,String opt){
        String array[] = exp.split(opt);
        int arrayInt[] = new int[2];
        arrayInt[0] = Integer.parseInt(array[0]);
        arrayInt[1] = Integer.parseInt(array[1]);
        return arrayInt;
    }
}
```


2）创建一个类来继承抽象类，重写抽象方法

```
public class Plus extends AbstractCalculator {
    @Override
    public int calculate(int num1,int num2) {
    return num1 + num2;
    }
}
```

3）通过调用抽象父类来实现对子类的调用。

```
public class StrategyTest {
    public static void main(String[] args) {
        String exp = "8+8";
        AbstractCalculator cal = new Plus();
        int result = cal.calculate(exp, "\\+");
        System.out.println(result);
    }
}
```
