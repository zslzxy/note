## 1 流Stream

### 1.1 基本介绍

> ​	将原有的数据源(比如 数组、集合、对象等) 转换成流，然后根据 Stream API 来进行一系列的中间操作，执行完以后，会产生一个新的流来存储新的结果，而原有的数据不会改变。
>
> ​	编程设计思想中，对一串数据进行多个中间操作以后，最后再获得结果。这个操作在没有流的情况下通常会涉及多次循环，效率低下。
>
> ​	流是为了处理一串数据（sequence），而不需要多次循环的一种处理方式。
>
> ​	流在操作序列的时候，会将数据存放在 Stream Pipeline 中，主要包含三部分：
>
> - 源（通常是指集合、数组等）
>
> - 0或者多个中间操作（一般为惰性操作，不会直接操作数据）
>
> - 终止操作（一般为求最终值，这时流的整个流程结束）
>
>   **Stream支持并行的操作，在多核处理上具有着强大的优势。**

![img](https://upload-images.jianshu.io/upload_images/1112615-cf1b08a3a58deb6b.png?imageMogr2/auto-orient/)

### 1.2 创建流

- 方法一：创建数据源,然后将数据源转换为 Stream 流。
- 方法二：通过 Arrays 中的 Stream() 方法来获取数组流。 
- 方法三：通过Stream类中的 of() 方法获取流。
- 方法四：创建无限流---项目不停，流就一直不断，可以使用 limit(num) 方法来限定生成多少流。
  - 迭代  --使用迭代来创建无限流。
  - 生成  --使用生成来创建无限流。

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

    //生成区间的流,创建一个区间为1~10的流
    IntStream range = IntStream.range(1, 10);

    //生成
    Stream<Double> stream5 = Stream.generate(() -> Math.random());
    stream5.limit(12).forEach(System.out::println);
}
```

### 1.3 Stream类型

- **`IntStream`**：支持串行、并行操作的序列，元素只有int类型的流。
- **`DoubleStream`**：支持串行、并行操作的序列，元素只有double类型的流。
- **`Stream`**：支持串行、并行操作的序列，元素是根据传递的泛型来确定。
- **`LongStream`**：支持串行、并行操作的序列，元素只有long类型的流。

### 1.4 常用方法

```java
//filter操作时过滤的操作
stream.filter((e) -> e > 3)
    //distinct操作是去重的操作
    .distinct()
    //sorted操作是进行升序排序的操作
    .sorted()
    //map操作是进行函数式计算的操作
    .map(e -> e + e)
    //转换为DoubleStream操作
    .asDoubleStream()
    //flatMap主要使用于操作集合中的集合等操作，将集合中的集合转换为Stream进行操作
    .flatMap(i -> DoubleStream.of(i + 1))
    //集合遍历
	.forEach(System.out::println);
```

```

```



