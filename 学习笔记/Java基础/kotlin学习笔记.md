# Kotlin学习笔记

[TOC]

## 一、Kotlin基础

### 1、Number数值类型

| 分类   | 类型   | 位宽         |        |
| ------ | ------ | :----------- | ------ |
| 浮点型 | double | 64（双精度） | 123.5  |
|        | float  | 32（单精度） | 123.5F |
| 整型   | Long   | 64           | 123L   |
|        | Int    | 32           | 123    |
|        | Short  | 16           |        |
| 字节   | Byte   | 8            |        |

#### 1）数值类型特征：

- 在Kotlin中，可以使用下划线使数字变得更加通俗易懂。例如：val oneMillion = 1_000_000；

- kotlin中默认自动对应着java中的拆箱包装类型的。**kotlin使用(如 Int?) 或者 泛型，就会自动将数据进行装箱**的操作。

```kotlin
	// 数字装箱操作，就不一定保留了同一性
	val a: Int = 10000
    println(a === a) // 输出“true”
    val boxedA: Int? = a
    val anotherBoxedA: Int? = a
    println(boxedA === anotherBoxedA) // ！！！输出“false”！！！

	// 数字的装箱操作，保留了数字的相等性
    val a: Int = 10000
    println(a == a) // 输出“true”
    val boxedA: Int? = a
    val anotherBoxedA: Int? = a
    println(boxedA == anotherBoxedA) // 输出“true”
```

#### 2）数据的显式转换

- Kotlin中的基本数据类型不能够进行隐式的转换，只能够进行显式的转换操作。

- 每个数字类型支持如下的转换:
  - `toByte(): Byte`
  - `toShort(): Short`
  - `toInt(): Int`
  - `toLong(): Long`
  - `toFloat(): Float`
  - `toDouble(): Double`
  - `toChar(): Char`

```kotlin
    var a1: Int = 100
    var a2 = a1.toInt()
    println(a2)
```

- 使用运算符进行上下文变量的转换：

```kotlin
    var a3 = 100L + 100
    println(a3)
```

### 2、运算

​	Kotlin支持数字运算的标准集，运算被定义为相应的类成员。

#### 1）位运算

- `shl(bits)` – 有符号左移 (Java 的 `<<`)

- `shr(bits)` – 有符号右移 (Java 的 `>>`)

- `ushr(bits)` – 无符号右移 (Java 的 `>>>`)

- `and(bits)` – 位与

- `or(bits)` – 位或

- `xor(bits)` – 位异或

- `inv()` – 位非

  例如：

  ```kotlin
      var a4 = (1 ushr 2) and 0x00FF000
      println(a4)
  ```

### 3、浮点数比较

- 相等性检测：`a == b` 与 `a != b`
- 比较操作符：`a < b`、 `a > b`、 `a <= b`、 `a >= b`
- 区间实例以及区间检测：`a..b`、 `x in a..b`、 `x !in a..b`
- 认为 `NaN` 与其自身相等
- 认为 `NaN` 比包括正无穷大（`POSITIVE_INFINITY`）在内的任何其他元素都大
- 认为 `-0.0` 小于 `0.0`

```kotlin
    var a5: Float? = 1.2f
    var a6: Float? = 1.2F
    println(a5 == a6) // true
	println(a5 === a6) // false
    var a7: Double = Double.NaN
    var a8: Float = Float.NaN
    println(a7)  //NAN
    println(a8)  //NAN
```

### 4、字符

​	Kotlin中的字符，是使用 **单引号包括起来的单个字符**。特殊字符可以使用反斜杠进行转义： 

```kotlin
\t   \b   \n  \r   \'  \"  \\  \$  等等
```

​	将一个字符转换为数据：

```kotlin
    //将字符转换为数字
    var a9: Char = 'c'
    val toInt = a9.toInt()
    println(toInt)
```

​	kotlin中使用 可空引用时，会将对象进行包装。

```kotlin
	var a10: Char? = 'a'
```



### 5、布尔类型

​	布尔类型使用的是 Boolean 类型表示，具有两个值，包括true与false。

​	当对象使用可空引用时，会将对象进行包装的操作。

```kotlin
    var a11: Boolean = true
    var a12: Boolean? = false
```

​	内置的布尔运算：

- `||` – 短路逻辑或
- `&&` – 短路逻辑与
- `!` - 逻辑非

### 6、数组

​	数组在 Kotlin 中使用的是 **Array** 类来表示，它定义了 **get** 与 **set** 函数，以及一些成员函数：

- 基本数据类型的数组的创建：

```kotlin
//创建Long类型的数组
var long_array:LongArray = longArrayOf(1, 2, 3)
//创建Float类型的数组
var float_array:FloatArray = floatArrayOf(1.0f, 2.0f, 3.0f)
//创建Double类型的数组
var double_array:DoubleArray = doubleArrayOf(1.0, 2.0, 3.0)
//创建Boolean类型的数组
var boolean_array:BooleanArray = booleanArrayOf(true, false, true)
//创建Char类型的数组
var char_array:CharArray = charArrayOf('a', 'b', 'c')
```

- String类型数组的创建：

```kotlin
    //创建String类型的数组
    var string_array:Array<String> = arrayOf("How", "Are", "You")
    for (index in string_array) {
        println(index)
    }
```

### 7、无符号整型(1.3版本才能实验性的使用)

#### 1）无符号整型的类型：

- Kotlin 为无符号整数引入了以下类型：
  - `kotlin.UByte`: 无符号 8 比特整数，范围是 0 到 255
  - `kotlin.UShort`: 无符号 16 比特整数，范围是 0 到 65535
  - `kotlin.UInt`: 无符号 32 比特整数，范围是 0 到 2^32 - 1
  - `kotlin.ULong`: 无符号 64 比特整数，范围是 0 到 2^64 - 1
- 无符号类型都有相应的为该类型特化的表示数组的类型：
  - `kotlin.UByteArray`: 无符号字节数组
  - `kotlin.UShortArray`: 无符号短整型数组
  - `kotlin.UIntArray`: 无符号整型数组
  - `kotlin.ULongArray`: 无符号长整型数组

- 区间与数列也支持 UInt和ULong的无符号整型：
  - `kotlin.ranges.UIntRange`
  - `kotlin.ranges.UIntProgression`
  -  `kotlin.ranges.ULongRange`
  -  `kotlin.ranges.ULongProgression`

#### 2）字面值

​	为了让无符号整型更易于使用，Kotlin中使用了提供后缀标记的方式拥有表示指定的无符号类型。

- 使用 `u` 与 `U` 将字面值标记为无符号。确定类型使用的是预期类型确定。
- 如果没有指定预期类型数据，默认是使用 `UInt` 或者 `ULong` 类型。

```kotlin
    val b: UByte = 1u  // UByte，已提供预期类型
    val s: UShort = 1u // UShort，已提供预期类型
    val l: ULong = 1u  // ULong，已提供预期类型

    val a1 = 42u // UInt：未提供预期类型，常量适于 UInt
    val a2 = 0xFFFF_FFFF_FFFFu // ULong：未提供预期类型，常量不适于 UInt
```

- 后缀使用 `uL` 与 `UL` 显式的将字面值标记为无符号长整型。

```kotlin
	val a = 1UL // ULong，即使未提供预期类型并且常量适于 UInt
```

### 8、字符串

#### 1）两种类型的字符串字面值

- 转义字符串可以拥有转义字符。

- 原始字符串可以包含换行以及任意文本------  使用    """  """    来进行字符串的包含。

```kotlin
	//创建一个可以拥有转义字符的String字符串
	var str1:String = "adsfadf\n"
	
	//创建一个不拥有转义字符的String字符串
    var str2:String = """
        adsfasdf\na
        adsf
        \t
    """

    println(str1)
    println(str2)
```

#### 2）函数-- trimMargin()

​	函数 trimMargin() 用于去除前导空格，默认是使用  |  来进行去掉这一行字符串的前面的空格。

```kotlin
    var str2:String = """
        |adsfasdf\na
        |adsf
        |\t
    """.trimMargin()

//输出的结果为----前面没空格了
adsfasdf\na
adsf
\t
```

​	换种方式去掉当前字符串的前导空格，例如使用   ' < '  进行分割。

```kotlin
    var str2:String = """
        <adsfasdf\na
        |adsf
        |\t
    """.trimMargin("<")
//输出的结果为：
adsfasdf\na
        |adsf
        |\t
```

#### 3）字符串模板

​	字符串模板中能够包含模板表达式，也就是一小段代码，然后将这一小段代码的值合并到字符串中。

- 模板表达式使用的是 **$** 符号进行表示，例如：

```kotlin
    var b5: Int = 23
    var str5: String = "张世林的年龄为：$b5，他是个帅哥"
    println(str5)  //输出的结果为：  张世林的年龄为：23，他是个帅哥
```

- 使用花括号将表达式进行概括：

```kotlin
    var b5: Int = 23
    var b6: Int = 23
    var str5: String = "张世林的年龄为：${b5 + b6}，他是个帅哥"
    println(str5)  //输出的结果为：  张世林的年龄为：46，他是个帅哥
```

- **在原始字符串以及转义字符串中，都支持模板**。

```kotlin
    var b5: Int = 23
    var b6: Int = 23
	//在原始字符中使用 字符串模板。
    var str6: String = """
            张世林的年龄为：
            ${b5 + b6}，
            他是个帅哥
        """
    println(str6)
    
//输出的结果为：
            张世林的年龄为：
            46，
            他是个帅哥
```

- 如果在字符串中需要使用  **$** 的话：

```kotlin
val price = """
${'$'}9.99
"""
```



# 基本类型 学习完毕



### 2、Kotlin装箱拆箱

​	在Kotlin中，数据类型不区分装箱与拆箱。编译器会自动根据数据类型来进行不同的选择进行封装。例如：kotlin中的Int类型，编译器会自动根据数据的value来进行选择使用int还是Integer来进行封装。

### 3、基本数据类型转换

- **不可隐式转换** ---- 在Kotlin中，基本数据类型不可以进行隐式的转换，只能使用方法进行显式转换。例如：

```Kotlin
	val t9: Int = 23
    val t10: Long = t9.toLong()
    println(t10)
```

### 4、字符

​	字符在Kotlin中，是使用的Unicode编码，占用两个字节，可以存放汉字。

```kotlin
	val t7: Char = 'z'
    val t8: Char = '咋'
    println(t7)
    println(t8)
```

### 5、字符串

​	字符串在Kotlin中，相当于java中的字符串。

​	字符串中的 == 相当于java中的 equals() 方法比较；字符串中的 === 相当于java中的 == 比较。

```kotlin
	val Str1: String = "zsds"
    val Str2: String = String(StringBuffer("zsds"))
    //== 相当于java中的 equals() 方法
    println(Str1 == Str2)
    // === 相当于java中的 == 表达式
    println(Str1 === Str2)
```

​	字符串保留固定格式：使用三个双引号来保证字符串里面的数据格式不会发生改变。

```kotlin
    val str3: String = """张世林
        啊擦
        \t
        adf
    """
    println(str3)
```

​	获取字符串的长度：调用字符串的 length 属性来获取字符串的长度。

```kotlin
	println(str3.length)
```



### 6、使用${}取值

```kotlin
    //使用 ${} 来进行在字符串中的取变量的值
    val i1: Int = 2
    val i2: Int = 4
    println("$i1 + $i2 = ${i1 + i2}")

    val i3: String = "bac${t7}cde "
    println(i3)
    
    //输出的结果为:
    2 + 4 = 6
	baczcde 
```

### 7、val与var修饰符

#### 1）val修饰符：

​	val 修饰符主要是用于修饰**只读的、不能够被修改**的变量。例如:：

```kotlin
	val str: String = "zsl"
	//如果再进行修改的话，程序会报错
```

#### 2）	var修饰符：

​	var 修饰符主要是用于修饰 **可变的，能够被修改**的变量。例如：

```kotlin
	var str: String = "zsl"
	str = "tsl"
	//修改变量的值，程序不会报错
```

### 8、区间

​	kotlin中，可以定义一定的区间。

```kotlin
	var range1: IntRange = 0..1024  //表示[0--1024]
    var range2: IntRange = 0 until 1024 //表示[0--1024)

    //遍历range区间
    for (i in range1) {
        println(i)
    }

    //判断区间是否为空
    println(range2.isEmpty())
    //查看区间是否包含某一个数
    println(range1.contains(23))
    //查案去见是否包含某一个数
    println(50 in range1)
```

### 9、数组

```kotlin
	var arr1: Array<Person1> = arrayOf(Person1(1, "zsl"),
                                       Person1(2, "tsl"))
    var arr2: IntArray = intArrayOf(1,2,3,4)
    var arr3: CharArray = charArrayOf('z','a','张','r')
    var arr4: LongArray = longArrayOf(1,2)
    var arr5: Array<String> = arrayOf("我是","帅哥")
```

### 10、kotlin类型推导

在kotlin中，类型推导，是根据所赋值的类型来自动进行类型的推导的。

```kotlin
fun main(args: Array<String>) {
    var i = "zsxl"
    println(i)

    var i1 = 123
    println(i1)

    var i2 = Person1(1,"zsl")
    println(i2)
}
```



## 二、Kotlin函数

### 1、普通函数：

```kotlin
/**
* 具有两个形参，然后返回值为 Int 类型。
*/
fun sum (arg1: Int,arg2: Int): Int {
    return arg1 + arg2
}

fun main(args: Array<String>) {
    println(sum(1,2))
}
```

### 2、表达式作为函数体、返回值

```kotlin
fun sum1(a: Int, b: Int) = a+ b

fun main(args: Array<String>) {
    val sum = sum(1, 2)
    println(sum)
}
```

### 3、函数无返回值

```kotlin
/**
 * 函数返回无意义的值
 */
fun noreturn1 (a: Long,b: Int): Unit {
    println("函数返回无意义的值：$a+$b=${a + b}")
}

/**
 * 函数无返回值，跟返回值为 Unit意义一样
 */
fun noreturn2 (a: Long,b: Long) {
    println("函数无返回值：$a+$b=${a+b}")
}

fun main(args: Array<String>) {
    noreturn1(2,3)
    noreturn2(2,3)
}
```

### 4、使用可空值及 null 检测

​	**在kotlin中，变量的值可以为null的时候，必须在声明的类型后面添加  ?  来标识该引用能够为空。**

#### 1）函数的形参可以为空：

```kotlin
/**
* 形参中的第一个参数表示可以为 null，第二个形参不能够为null
*/
fun returnmull (str1: String?, str2: String) {
    println(str1+str2)
}

/**
* 传递数据的时候，第一个数据可以传null，第二个数据不能传null
*/
fun main(args: Array<String>) {
    noreturn1(2,3)
    noreturn2(2,3)
    returnmull(null,"张世林")
}
```

#### 2）函数的返回值可以为空：

```kotlin
fun parseInt(str: String): Int? {

}
```

#### 3）自动类型转换：

​	使用  is  算术运算符能够将数据自动转换成 is 后面对应的数据类型。

```kotlin
/**
* 直接将 obj 在条件分支中转换为 String类型的数据了
*/
fun getStringLength(obj: Any): Int? {
    if (obj is String) {
        // `obj` 在该条件分支内自动转换成 `String`
        return obj.length
    }

    // 在离开类型检测分支后，`obj` 仍然是 `Any` 类型
    return null
}

/**
* 直接在返回的时候，调用length会将 obj转换为  String类型的数据
*/
fun getStringLength(obj: Any): Int? {
    if (obj !is String) return null

    // `obj` 在这一分支自动转换为 `String`
    return obj.length
}
```



## 三、循环

### 1、for循环

​	for 循环是循环一个集合或者一个数组。主要是包含下标遍历和数据遍历。例如：

```kotlin
    var items = listOf<String>("张世林","周小燕","肖国树")
    //遍历集合
    for (item in items) {
        println(item)
    }

    //遍历集合下标
    for (index in items.indices) {
        println("根据集合下标进行遍历:"+items[index])
    }
```

### 2、while循环

​	while循环跟java循环一致。例如：

```kotlin
//while 循环
var i = 0
println("items的size为："+items.size)
while (i < items.size) {
println("while循环的结果：${items[i]}")
i++
}

```

### 3、when循环

​	when循环，相当于在java中的switch语句，根据多条语句进行匹配的操作，匹配到值就进行对应的操作。

```kotlin
fun testWhen(obj: Any): Any =
    when(obj) {
        1         ->  1
        "张世林"   ->  "张世林"
        true      ->  true
        else      ->  "没有匹配的数据"
    }

fun main(args: Array<String>) {
    val any = testWhen("adf")
    println(any)
}
```

### 4、range区间循环

```kotlin
    var x = 1
    var y = 9
    //判断是否在这个区间内
    if (5 in x..y) {
        println("5在这个区间中")
    }

    //判断是否被没有在这个区间中
    if (55 !in x..y) {
        println("55在这个区间中")
    }

    //区间迭代
    for (p in 1..5) {
        println(p)
    }

```























