# Spark学习笔记

## 一. RDD概述

## 1. RDD基本概念

> ​	**`RDD（Resilient Distributed Dataset）`**叫做 **分布式数据集**，是Spark中最基本的数据抽象；代码中存在的形式是一个抽象类，它代表一个不可变、可分区、里面元素可并行计算的集合。

### 2. RDD的属性

> - 一组分区（Partition），即数据集的基本组成单位；
> - 一个计算每个分区的函数；
> - RDD之间的依赖关系；
> - 一个Partitioner，即RDD的分片函数；
> - 一个列表，存储存取每个Partition的优先位置（Preferred location）；

### 3. RDD的特点

> **`RDD表示只读的分区的数据集；`**
>
> **对RDD进行改动，只能通过RDD的转换操作，由一个RDD得到一个新的RDD，新的RDD包含了从其他RDD衍生所必需的信息；**
>
> **RDDS之间存在依赖关系，RDD的执行时按照血缘关系延时计算的；如果血缘关系较长，可通过持久化RDD来切断血缘关系。**

#### 3.1 分区概念

> 

### 4. Spark三大数据结构

> - RDD：分布式数据集
>
> - 广播变量：分布式只读共享变量
>
> - 累加器：分布式只写共享变量  
>
> ```java
>         //创建一个累加器
>         LongAccumulator longAccumulator = javaSparkContext.sc().longAccumulator();
>         parallelize.foreach(x -> longAccumulator.add(Integer.valueOf(x)));
>         System.out.println(longAccumulator.sum());
> 
> 
>         ArrayList<Map<String, Object>> arrayList = new ArrayList<Map<String, Object>>();
>         for (int i = 0; i < 5; i++) {
>             HashMap<String, Object> map = new HashMap<>();
>             map.put(String.valueOf(i), i * i);
>             arrayList.add(map);
>         }
>         //将ArrayList集合创建成共享变量
>         Broadcast<ArrayList<Map<String, Object>>> broadcast = javaSparkContext.broadcast(arrayList);
> 
>         System.out.println("--------------------------------------------");
>         JavaRDD<String> jsonRDD = javaSparkContext.textFile("json.json", 5);
>         JavaRDD<Map<String, Object>> mapJavaRDD = jsonRDD.mapPartitions(datas -> {
>             List<Map<String, Object>> res = new ArrayList<Map<String, Object>>();
>             while (datas.hasNext()) {
>                 res.add(JSON.parseObject(datas.next(), Map.class));
>             }
>             return res.iterator();
>         });
>         //输出多少条数据
>         long count = mapJavaRDD.count();
>         System.out.println(count);
> 
>         mapJavaRDD.filter(x -> {
>             int city_id = Integer.valueOf(x.get("city_id").toString()) % 500000 % 5;
>             System.out.println(broadcast.getValue().get(city_id));
>             return city_id == 2;
>         }).collect();
> 
> ```
>
> 

- **`coalesce(num)`**  合并当前RDD的分区，主要是当每个分区的数据量较小以后，可以缩小分区数目，合并分区；

- **`repartition(num)`** 从新分区，跟coalesce一致；
- **`union(otherDataset)`**  将两个RDD合并成一个RDD；





## 二. SparkSQL

> ​	Spark SQL是Spark用来处理结构化数据的一个模块，它提供了2个编程抽象：`DataFrame`和`DataSet`，并且作为分布式SQL查询引擎的作用。
>
> - **`DataFrame`** : 数据结构
> - **`DataSet`** : 数据集

### 2.1 DataFrame

> DataFrame只有类型，相当于只有表头，但是没有该列数据的数据类型；
>
> DataFrame相当于是结构化的表格，主要是从其他地方读取文件、读取jdbc等，将数据转换为一张视图，根据该视图来实现SQL语句的查询；
>
> - spark.read.xxx  获取数据源
> - spark.createTempView  将数据源创建为视图
> - spark.sql   在该视图中查询数据，使用SQL的方式
> - 数据源.printSchema   获取表头
> - df.rdd  将DataFrame转换为 RDD对象；

### 2.2 DataSet

> DataSet是一个具有表头，该列的数据类型的一个spark的SQL对象











