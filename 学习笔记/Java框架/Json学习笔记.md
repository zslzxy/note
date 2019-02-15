[TOC]
# Gson学习笔记

## Gson的使用场景：
- 将 Json 数据转换为 POJO 对象。

- 将 POJO 对象转换为 Json 数据。


## 简单示例：
### 第一步：添加jar包

```
<dependency>
	    <groupId>com.google.code.gson</groupId>
	    <artifactId>gson</artifactId>
	    <version>2.8.2</version>
	</dependency>
```
### 第二步：创建一个 POJO 对象。

```
@AllArgsConstructor
@NoArgsConstructor
@Data
public class Student {

	private String name;
	private int age;
	
	
}
```

### 执行json数据与pojo之间的转换。
将json数据转换为pojo对象：  

```java
	@Test
	public void testJsonToPojo() {
		//json数据
		String json = "{\"name\":\"zsl\",\"age\":23}";
		
		//创建GsonBuilder
		GsonBuilder builder = new GsonBuilder();
		
		//创建Gson对象
		Gson gson = builder.create();
		
		//将json数据转换为对象
		Student student = gson.fromJson(json, Student.class);
		
		System.out.println(json);
		
		System.out.println(student);
		
	}
```
将pojo对象转换为json数据：  

```
	@Test
	public void testPojoToJson() {
		Student student = new Student("张世林",25);
		
		//创建GsonBuilder
		GsonBuilder builder = new GsonBuilder();
		
		//创建Gson对象
		Gson gson = builder.create();
		
		//将pojo转换为json数据
		String json = gson.toJson(student);
		
		System.out.println(student);
		System.out.println(json);
		
	}
```
将json文件中的数据获取出来：  
```
	@Test
	public void testJsonReader() {
		Student student = new Student("张世林",25);
		
		//创建GsonBuilder
		GsonBuilder builder = new GsonBuilder();
		
		//创建Gson对象
		Gson gson = builder.create();
		
		try {
			Student fromJson = gson.fromJson(new JsonReader(new FileReader("student.json")), Student.class);
			System.out.println("数据为："+fromJson);
		} catch (JsonIOException e1) {
			// TODO Auto-generated catch block
			e1.printStackTrace();
		} catch (JsonSyntaxException e1) {
			// TODO Auto-generated catch block
			e1.printStackTrace();
		} catch (FileNotFoundException e1) {
			// TODO Auto-generated catch block
			e1.printStackTrace();
		}
	}
```

### fromJson()方法与toJson()方法详解
编号 |	方法 |	描述
---|---|---
1 |	<T> T fromJson(JsonElement json, Class<T> classOfT) | 此方法将从指定分析树读取的Json反序列化为指定类型的对象。
2|	<T> T fromJson(JsonElement json, Type typeOfT)|	此方法将从指定分析树读取的Json反序列化为指定类型的对象。
3	|<T> T fromJson(JsonReader reader, Type typeOfT)|	从reader中读取下一个JSON值并将其转换为typeOfT类型的对象。
4|	<T> T fromJson(Reader json, Class<T> classOfT)|	此方法将从指定Reader读取的Json反序列化为指定类的对象。
5|	<T> T fromJson(Reader json, Type typeOfT)|	此方法将从指定reader读取的Json反序列化为指定类型的对象。
6|	<T> T fromJson(String json, Class<T> classOfT)|	此方法将指定的Json反序列化为指定类的对象。
7|	<T> T fromJson(String json, Type typeOfT)|	此方法将指定的Json反序列化为指定类型的对象。
8|	<T> TypeAdapter<T> getAdapter(Class<T> type)|	返回type的类型适配器。
9	|<T> TypeAdapter<T> getAdapter(TypeToken<T> type)|	返回type的类型适配器。
10|	<T> TypeAdapter<T> getDelegateAdapter(TypeAdapterFactory skipPast, TypeToken<T> type)	|此方法用于获取指定类型的备用类型适配器。
11 | String toJson(JsonElement jsonElement)|	将JsonElements树转换为其等效的JSON表示形式。
12	|void toJson(JsonElement jsonElement, Appendable writer)|	为JsonElements树写出等价的JSON。
13	|void toJson(JsonElement jsonElement, JsonWriter writer)|	将jsonElement的JSON写入writer。
14	|String toJson(Object src)	|此方法将指定的对象序列化为其等效的Json表示形式。
15	|void toJson(Object src, Appendable writer)|	此方法将指定的对象序列化为其等效的Json表示形式。
16	|String toJson(Object src, Type typeOfSrc)|	此方法将指定对象(包括泛型类型的对象)序列化为其等效的Json表示形式。
17	|void toJson(Object src, Type typeOfSrc, Appendable writer)|	此方法将指定对象(包括泛型类型的对象)序列化为其等效的Json表示形式。
18	|void toJson(Object src, Type typeOfSrc, JsonWriter writer)|	将typeOfSrc类型的src的JSON表示写入writer。
19	|JsonElement toJsonTree(Object src)|	此方法将指定对象序列化为与JsonElements树相同的表示形式。
20	|JsonElement toJsonTree(Object src, Type typeOfSrc)|	此方法将指定对象(包括泛型类型的对象)序列化为与JsonElements树相同的表示形式。
21	|String toString()	|转化为字符串的形式。



### List类型的数据的转换：
使用 Type type = new TypeToken<List<Student>>(){}.getType(); 来获取json的数据的类型。  
```
import java.lang.reflect.Type;
import java.util.ArrayList;
import java.util.List;

import org.junit.Test;

import com.google.gson.Gson;
import com.google.gson.GsonBuilder;
import com.google.gson.reflect.TypeToken;

import lombok.Getter;

public class GsonTest3 {
	
	@Test
	public void testPojoToJson() {
		Student student0 = new Student("张世林",21);
		Student student1 = new Student("张世红",22);
		Student student2 = new Student("张世刚",30);
		Student student3 = new Student("张世强",29);
		
		List<Student> list = new ArrayList<>();
		list.add(student0);
		list.add(student1);
		list.add(student2);
		list.add(student3);
		
		//创建GsonBuilder
		GsonBuilder builder = new GsonBuilder();
		
		//创建Gson对象
		Gson gson = builder.create();
		
		//将pojo转换为json数据
		String json = gson.toJson(list);
		System.out.println(json);
		
		//获取返回的类型
		Type type = new TypeToken<List<Student>>(){}.getType();
		
		List<Student> sts = gson.fromJson(json, type);
		
		for (Student student : sts) {
			System.out.println(student);
		}
		
	}

}

```

### Gson转换内部类：

```
public class GsonTester { 
   public static void main(String args[]) { 
      Student student = new Student();  
      student.setRollNo(1); 
      Student.Name name = student.new Name(); 

      name.firstName = "Mahesh"; 
      name.lastName = "Kumar"; 
      student.setName(name); 
      Gson gson = new Gson(); 

      String jsonString = gson.toJson(student); 
      System.out.println(jsonString);  
      student = gson.fromJson(jsonString, Student.class);  

      System.out.println("Roll No: "+ student.getRollNo()); 
      System.out.println("First Name: "+ student.getName().firstName); 
      System.out.println("Last Name: "+ student.getName().lastName);  

      String nameString = gson.toJson(name); 
      System.out.println(nameString);  

      name = gson.fromJson(nameString,Student.Name.class); 
      System.out.println(name.getClass()); 
      System.out.println("First Name: "+ name.firstName); 
      System.out.println("Last Name: "+ name.lastName); 
   }      
}  
class Student { 
   private int rollNo; 
   private Name name;  

   public int getRollNo() { 
      return rollNo; 
   }  
   public void setRollNo(int rollNo) { 
      this.rollNo = rollNo; 
   }  
   public Name getName() { 
      return name; 
   } 
   public void setName(Name name) { 
      this.name = name; 
   } 
   class Name { 
      public String firstName; 
      public String lastName; 
   } 
}
```

### serializeNulls()方法
Gson中默认是优化Json内容的，将数据转换为Json数据的时候，会忽略掉 NULL 的值。  
但是提供了 serializeNulls() 方法来实现将 NULL 对象也转换为 json 数据里面去。

代码为：
```
//创建GsonBuilder
GsonBuilder builder = new GsonBuilder();
builder.serializeNulls();
```
差异：当pojo的name属性为 null 的时候，不使用 serializeNulls()方法的结果为：
```
{"age":25}
```
##### 当使用 serializeNulls()方法方法以后，结果为：

```
{"name":null,"age":25}
```


### Gson进行版本控制--setVersion() 方法
在Gson中，为每个pojo的属性的字段添加注解 @Since ,根据系统后面版本的更新，添加更多的字段，来实现版本的控制。当我将pojo转换为Json数据的时候，能够根据字段的版本来选择什么字段来执行json的转换。

注意：默认是使用最新版本的字段组合来进行json转换。

例如：  
先创建一个pojo，并指定字段的版本
```
public class Person {
	
	/**
	 * 使用@Since注解来标记某个字段是在什么版本的时候就存在了的，
	 * 可以适用于后面执行json的转换的时候，进行版本控制
	 */
	@Since(value = 1.0)
	private String name;
	
	@Since(value = 1.0)
	private Integer id;
	
	@Since(value = 1.2)
	private Boolean dd; 
	
	@Since(value = 1.1)
	private Boolean active; 
}
```
再使用版本控制来限制版本的选择：
```
    @Test
	public void testPojoToJson() {
		Person person = new Person();
		person.setId(1);
		person.setName("张世林");
		person.setActive(true);
		person.setDd(true);
		
		//创建GsonBuilder
		GsonBuilder builder = new GsonBuilder();
		builder.serializeNulls()
			   .setVersion(1.1);
		
		//创建Gson对象
		Gson gson = builder.create();
		
		String json = gson.toJson(person);
		System.out.println(json);
	}
```


##### 使用 @Expose 注解与 builder.excludeFieldsWithoutExposeAnnotation(); 来指定pojo的部分字段进行json转换。

代码为：先创建一个pojo，并使用@Expose注解来指定哪几个字段来进行json的转换。
```java
/**
*指定 name、id两个属性来进行json的转换
*/
public class Person {
	@Expose
	@Since(value = 1.0)
	private String name;
	
	@Expose
	@Since(value = 1.0)
	private Integer id;
	
	@Since(value = 1.2)
	private Boolean dd; 
	
	@Since(value = 1.1)
	private Boolean active; 
	
```
调用方法的时候，需指定 builder.excludeFieldsWithoutExposeAnnotation(); 来启动这个功能：
```
	@Test
	public void testPojoToJson() {
		Person person = new Person();
		person.setId(1);
		person.setName("张世林");
		person.setActive(true);
		person.setDd(true);
		
		//创建GsonBuilder
		GsonBuilder builder = new GsonBuilder();
		builder.serializeNulls()
			   .excludeFieldsWithoutExposeAnnotation();
		
		//创建Gson对象
		Gson gson = builder.create();
		
		String json = gson.toJson(person);
		System.out.println(json);
	}
```

### Gson 排除字段的控制
GsonBuilder使用序列化/反序列化过程中的excludeFieldsWithModifiers()方法提供对使用特定修饰符排除字段的控制。

注意：在Gson中，默认是排除静态或者瞬间的字段。

代码为：builder.excludeFieldsWithModifiers(Modifier.TRANSIENT); 用以实现静态数据也能够进行json的转换。
```
@Test
	public void testPojoToJson() {
		Person person = new Person();
		person.setId(1);
		person.setName("张世林");
		person.setActive(true);
		person.setDd(true);
		
		//创建GsonBuilder
		GsonBuilder builder = new GsonBuilder();
		builder.serializeNulls()
			   .excludeFieldsWithoutExposeAnnotation()
			   .excludeFieldsWithModifiers(Modifier.TRANSIENT);
		
		//创建Gson对象
		Gson gson = builder.create();
		
		String json = gson.toJson(person);
		System.out.println(json);
	}
```

### 重命名pojo的属性字段--@SerializedName() 注解
在进行json转换的时候，会将这个注解的值来代替之前的属性的属性名。

```
@SerializedName("new")
private Integer id;
```


### 复杂类型转换--pojo中有数组、List、对象、属性等
代码为：
```
@AllArgsConstructor
@NoArgsConstructor
@Data
public class Person {
	
	/**
	 * 使用@Since注解来标记某个字段是在什么版本的时候就存在了的，
	 * 可以适用于后面执行json的转换的时候，进行版本控制
	 */
	//@Expose
	@Since(value = 1.0)
	private String name;
	
	@SerializedName("new")
	//@Expose
	@Since(value = 1.0)
	private Integer id;
	
	@Since(value = 1.2)
	private Boolean dd; 
	
	@Since(value = 1.1)
	private Boolean active; 
	
	private int[] a = new int[3];
	
	private Student st;
	
	private List<Student> list;
}
```
```
@Test
	public void testPojoToJson() {
		int a[] = {1,2,3};
		Student student0 = new Student("张世林",21);
		Student student1 = new Student("张世红",22);
		Student student2 = new Student("张世刚",30);
		Student student3 = new Student("张世强",29);
		
		List<Student> list = new ArrayList<>();
		list.add(student0);
		list.add(student1);
		list.add(student2);
		list.add(student3);
		
		Person person = new Person();
		person.setId(1);
		person.setName("张世林");
		person.setActive(true);
		person.setDd(true);
		person.setA(a);
		person.setSt(new Student("zsl",22));
		person.setList(list);
		
		//创建GsonBuilder
		GsonBuilder builder = new GsonBuilder();
		builder.serializeNulls();
		
		//创建Gson对象
		Gson gson = builder.create();
		
		String json = gson.toJson(person);
		System.out.println(json);
		
		Person fromJson = gson.fromJson(json, Person.class);
		System.out.println(fromJson);
	}
```

### Gson设置日期的格式 --- setDateFormat() 方法
```
GsonBuilder builder = new GsonBuilder();
builder.setDateFormat("yyyy-MM-dd");
```


# FastJson学习笔记

## FastJson的使用场景：
- 将 Json 数据转换为 POJO 对象。

- 将 POJO 对象转换为 Json 数据。

## 简单示例
### 第一步：添加jar
```
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>fastjson</artifactId>
    <version>1.2.45</version>
</dependency>
```

### 第二步：创建一个简单对象
```
@AllArgsConstructor
@NoArgsConstructor
@Data
public class Student {

	private String name;
	private int age;
	private Date date;
	
}
```
### 第三步：执行pojo与json之间的转化
将json数据转换为pojo：
```java
@Test
public void test2() {
	String json = "{\"name\":\"zsl\",\"age\":23}";
	
	//将json转换为pojo
	Student student = JSON.parseObject(json, Student.class);
	System.out.println(student);
	
	//将pojo转换为json
	Student student2 = new Student("lll",333,new Date());
	String jsonString = JSON.toJSONString(student2);
	System.out.println(jsonString);
}
```

将pojo转换为json数据：
```java
@Test
public void test3() {
	
	//将pojo转换为json
	Student student2 = new Student("lll",333,new Date());
	String jsonString = JSON.toJSONString(student2);
	System.out.println(jsonString);
	
}
```

将list转换为json数据：
```java
@Test
public void test3() {
	Student student = new Student("qqq",123,new Date());
	Student student2 = new Student("lll",333,new Date());
	
	List<Student> list = new ArrayList<>();
	list.add(student);
	list.add(student2);
	
	String jsonString = JSON.toJSONString(list);
	System.out.println(jsonString);
}
```

将json数据转换为List数据：
```java
@Test
public void test4() {
	//创建一个List集合类型的json数据
	Student student = new Student("qqq",123,new Date());
	Student student2 = new Student("lll",333,new Date());
	List<Student> list = new ArrayList<>();
	list.add(student);
	list.add(student2);
	String jsonString = JSON.toJSONString(list);

	//将List类型的json数据转换为集合
	List<Student> array = JSON.parseArray(jsonString,Student.class);
	System.out.println(array);
}
```

将复杂数据类型转换为json：
```
@Test
public void test5() {
	int []arr = {1,2,3};
	
	Person person = new Person();
	person.setName("张世林");
	person.setArr(arr);
	person.setActive(true);
	person.setId(345);
	
	//创建一个List集合类型的json数据
	Student student = new Student("qqq",123,new Date());
	Student student2 = new Student("lll",333,new Date());
	List<Student> list = new ArrayList<>();
	list.add(student);
	list.add(student2);
	person.setList(list);
	
	String jsonString = JSON.toJSONString(person);
	System.out.println(jsonString);
}
```

### FastJson注解 @JSONField 
@JSONField适用于标注在实体类上，注解的属性：  
- ordinal ： 指定执行序列化的顺序。

-  name ：为序列化取别名。
-  format ： 指定序列化的格式。
-  serialize ： 指定是否参与序列化的过程。
-  deserialize ： 指定是否参与反序列化的过程。
```
@AllArgsConstructor
@NoArgsConstructor
@Data
public class Student {

	//ordinal表示序列化的顺序
	//serialize=true表示是否序列化
	//deserialize=true表示是否反序列化
	@JSONField(ordinal=1,serialize=true,deserialize=true)
	private String name;
	
	@JSONField(ordinal=2,name="Age")
	private int age;
	
	//format表示序列化使用的格式
	@JSONField(format=("yyyy-MM-dd"),ordinal=3)
	private Date date;

}

```