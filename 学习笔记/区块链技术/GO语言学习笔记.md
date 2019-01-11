[toc]
# Go语言学习笔记
**Go 语言中文网站API：https://studygolang.com/pkgdoc**
## 一、GO语言的特点：
> GO 语言既能够达到**静态编译语言的安全与性能**，又能够达到**动态语言开发维护的高效率**。如果使用一句话来表达GO语言的话：GO = C + Python；*它既能够达到C静态语言程序的运行速度，又能够达到P原合同动态语言的快速开发。*
>- 继承了C语言的很多语法，控制结构，基础数据类型，指针等。
>- 引入**包的概念** ，用于组织程序结构，GO语言的一个文件属于一个包，不能单独存在。
>- 垃圾回收机制，内存自动回收，不需要开发人员管理。
>- 天然并发(重要特征)。
>   - 语言层面支持并发，实现简单。
>   - goroutine，轻量级线程，可实现大并发处理，高效的利用多核。
>   - 使用CPS并发模型实现并发。
>- 吸收了管道通信机制，形成Go语言特有的管道之间的通信。
>- 函数能够实现多个返回值。
>- 新的特性：比如切片 slice 、延时执行 defer。
>- Golang中没有构造函数，当需要创建构造器的时候，可以使用 工厂模式 来进行创建。

### 1、简介：
>- 一个程序就是一个世界。
>- 变量是程序的基本组成单位。无论是哪种高级程序语言编写程序，变量都使其程序的基本组成单位。
### 2、变量的介绍：
1）**变量的概念**：变量相当于内存中一个数据存储空间的表示，通过变量名可以来访问到变量值。  
2）**变量的使用步骤**(*声明的变量必须要被使用*)：
>- 声明变量
>- 给变量赋值
>- 使用变量

### 3、变量的使用：
```Go
package main
import "fmt"
func main(){
	//定义了一个int类型的变量i
	var i int
	var id = 10
	var age int = 18
	//给变量 i 赋值
	i = 10
	//输出
	fmt.Println(i);
}
```

### 4、变量使用的注意事项
**1）变量表示内存中的一个存储区域，该区域有自己的名称(变量名)与类型(数据类型)。**
![image](https://note.youdao.com/yws/api/personal/file/428A9E0E15E24134BF0A3C3F137530FB?method=download&shareKey=7ca822cecfe229390dabdde51f44f1bd)

**2）Golang变量声明及使用的三种方式：**
>- 指定变量类型，声明以后若不赋值，使用默认值。例如：var num int，int类型的初始值为0，string类型值为null，小数类型值为0。
>- 根据值自行判断变量类型(类型推导)。例如：var num = 10.11
>- 省略 var 关键字，注意：:= 左边的变量不应该是已经声明过了的，否则会导致编译错误。例如：name = "tom"
>- 一次性多变量声明：
>   - 例1：var n1,n2,n3 int
>   - 例2：var id,name = 100,"张世林"
>   - 例3：id,name,password := 100,"张世林","123456"
>- 声明全局变量：声明在函数外的变量
>   - 在函数外定义单个全局变量: var id = 10;
>   - 在函数外定义多个全局变量: var(
                                	text = "啦啦啦"
                                	sum = 0
                                )
>- 一个变量的数据值可以在同一类型范围内不断变化value。如果将int类型的数据赋值为 12.3类型的话，就会报错。
### 5、变量之间使用 + 连接
>- 两边都是数值类型时，则做加法运算。
>- 两边都是字符串时，则做字符串拼接。
### 6、Go语言基本数据类型
![image](https://note.youdao.com/yws/api/personal/file/BBD2964EF1494372823E6298E7CC7214?method=download&shareKey=da61b953bda67753d0b6c953419d3afa)
### 7、整数类型基本介绍：
<font color=#D2691E  size=5>有符号的整数类型：</font>
![image](https://note.youdao.com/yws/api/personal/file/43DAFF6DF8A941F986651DABAFE799BF?method=download&shareKey=dce6d064ebd3e112e2b56795163cb67f)  
<font color=#D2691E  size=5>无符号的整数类型：</font>
![image](https://note.youdao.com/yws/api/personal/file/239D29D2623346B38D76CA5497BFF7D8?method=download&shareKey=11ade671d24b652e3c122d3d272f5e29)  
<font color=#D2691E  size=5>int、unit、rune、byte四种类型：</font>
![image](https://note.youdao.com/yws/api/personal/file/5F99E26E328049E9937FF4AAD05BCD5E?method=download&shareKey=29afff542ea7645833345a6870f3bbcf)
> **注意：**
>- Golang的整数类型分为：有符号与无符号类型，int uint32 类型和操作系统有关。
>- Golang的整型类型默认声明为 int 类型。
>- 查看某个变量的字节大小与数据类型：调用 printf() 方法  
>   - 查看数据类型： printf("数据类型为 %T",i)
>   - 查看数据占用字节数：先引入包unsafe，再调用 printf("字节大小为 %d",unsafe.Sizeof(i))
>- Golang程序中，遵守保小不保大的原则，即：在保证数据正常运行的情况下，使用的空间更小更好。
>- 计算机中最小存储单位为 bit ，byte 则是计算机中基本存储单元。8 bit = 1 byte。   

### 8、小数类型/浮点型
<font color=#D2691E  size=5>小数类型：float32与float64  </font>  
![image](https://note.youdao.com/yws/api/personal/file/CCDB951DD1AE4A57BDA10CD18A49A9A4?method=download&shareKey=f99edeeaad514cd643607af554572736)  
> **注意：**
>- Golang浮点类型有固定的范围与长度，不受操作系统的影响。
>- Golang的浮点型默认声明为float64类型。
>- 通常使用float64类型来更加精确的保证数据的精确性。
>- 浮点数的两种计数方式：
>   - 十进制数形式 ： 5.123   而 0.123 可以表示为 .123
>   - 科学计数法表示 ：(5.1234e2)---表示的是(5.1234 * 10的二次方) |||| (5.1234e-2)---表示的是(5.1234 * 10的负二次方)

### 9、字符类型 -- byte
**基本介绍：** 
>- Golang中没有专门的字符类型，如果需要存储单个字符，一般使用 byte 来进行存储。  
>- Golang中的自妇产室友一串固定长度的字符连接起来的字符序列。也即是说 **Go语言中的字符串是由单个字节** 连接起来的(传统的字符串是由单个字符组成的)。   

>**注意：**  
>- 如果保存的字符在ASCLL表中，则可以直接使用 byte 数据类型来今夕你保存。
>- 如果保存的字符超出了ASCLL表的范围，则可以使用 int 类型来进行保存。
>   - 例如：保存ASCLL表中的单个字符--- var c1 byte = 'a'
>   - 例如：保存超出了ASCLL表中的单个字符--- var c2 int = '中'
>- 如果需要输出字符，则：fmt.Printf("%c",c1)
>- Go语言中使用的是UTF-8编码格式。而字符的本质解释一个整数，直接输出的话，就是对应着UTF-8编码的码值。
>- 字符类型是可以进行运算的，它是对应的Unicode码。

>**字符转换流程：**
>- 字符的存储：字符->对应码值->二进制->存储
>- 字符的读取：二进制->对应码值->字符->读取

### 9、布尔类型 -- bool
**基本介绍：** 
>- 布尔类型也叫bool类型。bool类型只允许类型的只有 true 与 false 。
>- 布尔类型占用一个字节。
>- 布尔类型适用于逻辑运算，一般用于流程控制等。比如：if条件控制语句、for循环控制语句。
>- 使用方法 ： var b bool = false

### 10、字符串类型 -- string
**基本介绍：** 
>- **字符串就是一串固定长度的字符连接起来的字符序列。Go语言的字符串是由单个字符组合连接而成的。** Go语言的字符串的自己使用的是UTF-8编码标识Unicode文本。  

>**注意：** 
>- 使用方式 ： var c3 string = "阿瑟东 asdf45a"
>- Go语言中的字符串一旦赋值以后，字符串的value就不能够修改。
>- 字符串的两种表现形式：
>   - 双引号，嫩公仔双引号里面识别转义字符。
>   - 反引号。以字符串的原生形态来进行输出，避免了安全问题。 例如： c5 := `asd\nadsaas`
>- 字符串的拼接方式：
>   - 第一种方式：var c6 = c5 + c3 （将 c3 与 c5 字符串拼接在一起）
>   - 第二种方式：c6 += "haha"    （意思是在c6字符串后面添加 ‘haha’ 字符串）
>- 添加的字符串太多，需要换行的时候，加号必须放在上一行的末尾。  
### 11、Golang中基本数据类型的默认值：
在程序中，数据类型都有一个默认值，在程序员没有赋值的时候，就会保留默认值，又称为**零值**。
数据类型 | 默认值(零值)
---|---
整型 | 0
浮点型 | 0
字符串 | ""
布尔类型 | false

### 12、基本数据类型的相互转换
**1）简介：** 
> Golang和java不同，Go语言在不同类型的变量之间赋值的时候需要显示转换。也就是说**Golang中数据类型不能自动转换。**  

**2）基本语法：**
> 表达式： T(v) --- 将 v 的数据类型转换为 类型T。 
>   - 例1：将整型转换为float32类型
>```Go
>var c1 int = 123455
>c2 := float32(c1)
>```
>   - 例2：将float32型转换为整型类型
>```Go
>var c1 float32 = 123455
>c2 := int(c1)
>```

> **注意：**
>- 数据类型的转换：小范围--->大范围   大范围--->小范围
>- 被转换的变量不会发生变化，只是将转换后的数据赋值给其他变量。
>- 被转换的数据不能够超出范围转换的类型的数据范围，否则会造成数据溢出，会造成数据的结果的不一致。

### 13、基本数据类型与string类型的转换
**1）简介：** 
> 在程序开发中，经常将基本数据类型转换为string类型；或者将string类型转换为基本数据类型。  

**2）基本数据类型-->string类型：** 
>- <font color=#D2691E>方法一：fmt.Sprintf()方法</font>  
![image](https://note.youdao.com/yws/api/personal/file/CA79BE1C293F44F59FCFC300F9BF304E?method=download&shareKey=2bc5cc38d79eb04e766d0aa9458dcdb5)  
>- <font color=#D2691E>方法二：使用 strconv包 里面的四个函数</font>   
>   - func FormatBool(b bool) string
>   - func FormatInt(i int64, base int) string
>   - func FormatUint(i uint64, base int) string
>   - func FormatFloat(f float64, fmt byte, prec, bitSize int) string
![image](https://note.youdao.com/yws/api/personal/file/CB433BBB91E14A8F9F9E5FE0030AE100?method=download&shareKey=9481c4373c68bd922f1d152b7f360bac)  
>- <font color=#D2691E>方法三：使用 strconv包 里面的Itoa函数，直接将int类型转换为字符串</font>   
>   - func Itoa(i int) string 例如：str := Itoa(num)

**3）string类型-->基本数据类型：**  
>- <font color=#D2691E>方法一：使用 strconv包 里面的四个函数</font> 
>   - func ParseBool(str string) (value bool, err error)
>   - func ParseInt(s string, base int, bitSize int) (i int64, err error)
>   - func ParseUint(s string, base int, bitSize int) (n uint64, err error)
>   - func ParseFloat(s string, bitSize int) (f float64, err error)

> **注意：**
>- string转向基本数据类型，必须确定数据的有效性。

### 14、标准输出的 %X 所代表的的意思：
> 通用：
>- %v	值的默认格式表示
>- %+v	类似%v，但输出结构体时会添加字段名
>- %#v	值的Go语法表示
>- %T	值的类型的Go语法表示
>- %%	百分号  

> 布尔值：
>- %t	单词true或false  

> 整数：
>- %b	表示为二进制
>- %c	该值对应的unicode码值
>- %d	表示为十进制
>- %o	表示为八进制
>- %q	该值对应的单引号括起来的go语法字符字面值，必要时会采用安全的转义表示
>- %x	表示为十六进制，使用a-f
>- %X	表示为十六进制，使用A-F
>- %U	表示为Unicode格式：U+1234，等价于"U+%04X"

> 浮点数与复数的两个组分：
>- %b	无小数部分、二进制指数的科学计数法，如-123456p-78；参见strconv.FormatFloat
>- %e	科学计数法，如-1234.456e+78
>- %E	科学计数法，如-1234.456E+78
>- %f	有小数部分但无指数部分，如123.456
>- %F	等价于%f
>- %g	根据实际情况采用%e或%f格式（以获得更简洁、准确的输出）
>- %G	根据实际情况采用%E或%F格式（以获得更简洁、准确的输出）

> 字符串和[]byte：
>- %s	直接输出字符串或者[]byte
>- %q	该值对应的双引号括起来的go语法字符串字面值，必要时会采用安全的转义表示
>- %x	每个字节用两字符十六进制数表示（使用a-f）
>- %X	每个字节用两字符十六进制数表示（使用A-F） 

### 15、指针
**1）简介：**
>- 基本数据类型，变量存的就是指，也叫值类型。
>- 获取变量的地址，使用 & 符号。例如：&num 。下面是变量的内存模型。  
![image](https://note.youdao.com/yws/api/personal/file/9F9F171C94EC49798771F00CB7CA6162?method=download&shareKey=a6921e643a7534be14d8a308a4863549)
>- 指针类型，就是指 **指针变量存的是一个内存地址**，而这个指针也具有一个**一个内存地址**。下面是指针内存图：指针 ptr 的变量值为 变量i的地址，而指针也具有一个内存地址。 
![image](https://note.youdao.com/yws/api/personal/file/92D9360372EC4A769154CCD25A669B17?method=download&shareKey=174241c8903c8ee3f489e81504577bd9)
>- 指针的使用方式：
>   - 创建变量：var num int =12
>   - 创建指针： var ptr *int= &num
>   - 输出指针所存放的值：fmt.Printf("指针ptr的value为 %v  \n",**ptr**) 
>   - 输出指针对应的内存的地址：fmt.Printf("指针ptr自己的地址为 %v  \n",**&ptr**)
>   - 输出指针存放的地址对应的value：fmt.Printf("指针ptr存放的地址的那个值为 %v",***ptr**)
>   - 使用指针存放的地址对应的value(也就是 *ptr)来改变value的num变量的值。 *ptr--  

**2）指针数据类型：**
> 值类型，都有对应的指针类型，形式为 *数据类型。比如int类型的指针为 *int，float32对应的指针为 *float32。
> 值类型包括：基本数据类型 int系列、float系列、bool、string、数组和结构体struct。

### 16、值类型与引用类型
**1）值类型：** 基本数据类型 int系列、float系列、bool、string、数组与结构体struct。  
**2）引用类型：**  指针、slice切片、map、管道chan、interface等都是引用类型。  
**3）值类型的特点：**
> 在值类型中，变量直接存储值，内存通常在栈中分配。内存中的模型为：  
![image](https://note.youdao.com/yws/api/personal/file/C47F094DBDDE43F6AE7B4AEB858C8A1F?method=download&shareKey=5b9a99c44a9a323e3f149ab8ae5a59ab)   

> 在引用类型中，变量存储的是一个地址，这个地址对应的空间才是真正存放的value。内存通常在堆中分配。当没有任何变量引用这个跟地址是，改地址就变成了一堆垃圾，由GC负责回收。  
![image](https://note.youdao.com/yws/api/personal/file/050E4C8924A745E5B6C2FA583001A35B?method=download&shareKey=b5cef3db76324b193fe5dc1724c157e4)   

> 堆、栈的模型为：  
![image](https://note.youdao.com/yws/api/personal/file/B5B228247DF342F3B95E3B52A0E2C799?method=download&shareKey=e1a1b17ea1fd225365e96e39f442515c)  

### 17、标识符
**1）概念：**  
>- Golang对各种变量、方法、函数等命名时使用的字符序列称为标识符。
>- 范式自己可以起名字的地方都叫标识符。  

**2）标识符命名规则：**
>- 由26个英文字母大小写，0-9，_组成。
>- 数字不可以开头。
>- Golang中严格区分大小写。
>- 标识符不能包含空格。
>- 下划线“_”本身就是Go语言中的一个特殊标识符，称为“空标识符”。可以代表任何其他的标识符，然而它的值会被忽略(比如用作某个不重要的返回值)。所以仅能被作为占位符使用，而不能被作为标识符使用。
>- 不能够一系统**保留关键字**作为标识符。  
系统中保留关键字有：  
![image](https://note.youdao.com/yws/api/personal/file/521C2E9F10884C5FB8FF3BDFF97610C8?method=download&shareKey=f1a1aeab8fca99e9b571866ec7e0fe13)  
系统中预定义标识符：  
![image](https://note.youdao.com/yws/api/personal/file/D178A79B2FDA430C9770B0845227BDB2?method=download&shareKey=7f2bd7c30ad88a5377142614faf19761)

**3）注意：**
>- 包名：保持package的名字与目录保持一致。尽量采取有意义的包名，且需要避免与标准库冲突。
>- 变量名、函数名、常量名都采用驼峰命名法。
>- **如果变量名、函数名、常量名首字母大写，则可以被其他的包访问；如果首字母小写，则只能在本包中使用。简单的理解就是：首字母大写就是公开的，首字母小写就是私有的。**   

**4、两个包之间的数据引用方法:**
>- 第一步：创建被调用的包里面的数据：创建utils包下的 util.go 文件
```Go
package utils
 import (
	 "fmt"
 )
var Num int = 8
func Test(){
	fmt.Println("测试成功")
}
```
>- 第二步：创建主包 main 下的 main.go 文件，然后再引入utils包下的资源，最后对utils包中的资源进行调用。  
>   注意：  
>   - ① 是直接使用 包名+变量名/函数名/常量名 来对其他包的资源进行调用。
>   - ② import的路径是GOPATH环境变量的src目录后的目录所有路径需要写入到import中。
```Go
package main
import (
	"fmt"
	"go_code/project01/utils"
)
func main(){
	fmt.Println(utils.Num);
	utils.Test()
}
```

### 18、常量const
#### 1）语法格式：
格式一： const 常量名 常量类型 = 常量的value

格式二： const 常量名 = 常量的value

例如：
```
const name = "张世林"
const text string = "come to here"
```

#### 2）常量使用细节：
- 常量是使用 const 关键字修饰。

- 常量在初始化的时候，必须附初始值。
- 常量不能够修改。
- 常量只能修饰 bool、string、int、float类型。
- **常量仍然是通过首字母大小写来控制访问的权限。**

#### 3）常量简单写法：
```
const (
    a = 1
    b = 2
)
//输出结果为： a=1,b=2

const (
    a = iota
    b
    c
)
//输出结果为： a=0,b=1,c=2
```

## 二、Go语言运算符
运算符是一种特殊的符号，用以表示数据的运算、赋值、比较等。运算符的种类为：
>- 算术运算符
>- 赋值运算符
>- 比较运算符/关系运算符
>- 逻辑运算符
>- 位运算符
>- 其他运算符  

### 1、算数运算符
**1）简介：** 算术运算符是对数字运算符的变量进行运算的。比如：加减乘除。  

**2）算术运算符预览：**  
![image](https://note.youdao.com/yws/api/personal/file/1E65A04A99144282B117B47EBECFAE2A?method=download&shareKey=9d86931642b584a36851981d77f2e72c)  

**3）注意：**  
>- 对于除号 "/"，它除以整数的话，就只保留整数；它除以小数的话，就会保留小数点后面的数据。
>- 对于取模 "%"，它的公式为：a % b = **a - a / b * b**  
>- Go语言中的自增自减只能够当做一个独立语言使用(也就是只能在单行执行语句 i++ )。例如：i++ 正确；而 a = i++ 错误。
>- Go语言中的 ++ 与 -- 运算符只能够放在变量的后面，不能够写在变量前面。

### 2、关系运算符
**1）简介：** 
>- 关系运算符的结果都是bool类型，也就是true或者false。
>- 关系表达式经常用在 if结构 **的条件中** 或者 **循环结构** 中。
 
**2）关系运算符预览：**  
![image](https://note.youdao.com/yws/api/personal/file/BA0DC0EE86744739A56147DA54E9BB5D?method=download&shareKey=b54b0247ef35ca72ecb011c5afaa76f6)   

**3）注意：**
>- 关系运算符只有true或者false两个值的类型。
>- 关系运算符组成的表达式称为 关系表达式。
>- 比较运算符 "==" 不能够写成 "="。

### 3、逻辑运算符
**1）简介：** 用于连接多个表达式，最终的结果也是一个true或者false。
**2）逻辑运算符预览：**
>- **&&** ---逻辑与运算，如果两边的操作数都为真，结果为真，否则为false。也称为**短路与**，如果第一个条件为假，后面的条件不会去继续进行判断。
>- **||** ---逻辑或运算，如果两边的操作数有一个为true，则结果为true，否则为false。也称为**多路或**，如果第一个条件为true，则第二个条件不会继续进行判断。
>- **！** ---逻辑非运算，如果条件为真，则值为false，条件为false，则值为true。

### 4、赋值运算符
**1）简介：** 将某个运算后的值，赋值给指定变量。
**2）赋值运算符概览：**  
![image](https://note.youdao.com/yws/api/personal/file/1E5197922E7A4017B8CB1C38E682E648?method=download&shareKey=eb76ea8bfddbbc7b085739fd8f217988)  
![image](https://note.youdao.com/yws/api/personal/file/639CCBD27BC64744A668BEAE892D192B?method=download&shareKey=740fd7ce1b29118aeb5e530508b44500)  
**3）注意：**  
>- 运算顺序从左到右，赋值运算符左边只能是变量，右边可以是变量、常量、表达式。


### 5、位运算符

**2）位运算符概览：**
![image](https://note.youdao.com/yws/api/personal/file/4F4274C619DF411BBE10FA3A1066F3E2?method=download&shareKey=f7a42932fe5d3b037026944aa5c52efb)

### 6、其他运算符

运算符 | 描述 | 实例
---|---|---
& | 返回变量的存储地址 | &a  返回 a 的存储地址
* | 指针变量 | *a 是一个指针变量

### 7、运算符优先级序列：
![image](https://note.youdao.com/yws/api/personal/file/ED39107C97344EEB88E62884217F83C9?method=download&shareKey=cd1f50235f2f77f5daa1bd27888ad4fe)

### 8、键盘输入语句：
方法一：使用fmt包中的 func Scanln() 方法。
```
package main
import "fmt"

func main(){
	var i int
    fmt.Scanln(&i)
	fmt.Println(i)
}
```
方法二：使用fmt包中的 func Scanf() 来标准的执行输入，根据空格、逗号等方式来进行隔断输入数据。
```
package main
import "fmt"

func main(){
	var i int
	var j int
	var k int
    fmt.Scanf("%d,%d,%d",&i,&j,&k)
	fmt.Println(i+j+k)
}
```

### 9、位运算
#### Golang中有三个位运算符：

运算符 | 符号 | 运算规则
---|---|---
按位与 | & | 同一为1，否则为0
按位或 | 一个竖线 | 有一为一，否则为0
按位异或 | ^ | 两者必须有一个为1，一个为0才为1，否则为0。

#### 注意：
- 计算机在做位运算的时候，都是**使用补码来进行位运算**的。
- **补码之间的运算，算出来的结果也是补码**，如果需要得到结果，那么可以必须将补码转换为原码。

---

例如：2&3 的结果为 2  
> 解析过程：  
> 2的补码为：0000 0010  
> 3的补码为：0000 0011  
> 根据 与 的规则，同一为一，则结果为： 0000 0010 == 2 ，所以结果为2.

---

例如：2|3 的结果为 3  
> 解析过程：
> 2的补码为： 0000 0010
> 3的补码为： 0000 0011
> 根据 或 的规则，有一为一，则结果为： 0000 0011 == 3 ，所以结果为3.

---

例如：-2^3 的结果为 -3   
> 解析过程：
> -2的补码为： 1111 1110
> 3的补码为：  0000 0011
> 根据 异或 的规则，两个中有一个0与一个1，则为1，否则为0： 1111 1101    
> 然后再将补码转换为反码： 1111 1100
> 最后将反码转换为原码： 1000 0011 == -3

#### Golang中两个移位运算符

运算符 | 符号 | 运算规则
---|---|---
右移运算符 | >> | 低位溢出，符号位不变，并用符号位补溢出的高位。
左移运算符 | << | 符号位不变，低位补0.

**注意：**
其实也就是移动数据多少位而已。


### 10、程序流程控制
#### 程序流程控制的分类：
- 顺序控制

- 分支控制
- 循环控制

#### 1）顺序控制
从上到下逐行的执行，中间没有任何判断与跳转。

#### 2）分支控制
##### if单分支控制
执行代码为：
```
if 条件表达式 {

    //执行代码块
}
```
##### if双分支控制
执行代码为：
```
if (a > 3) {
	fmt.Println(a)
} else {
	fmt.Println(a-1)
}
```

##### if多分支控制
执行代码为：
```
if a > 3 {
		fmt.Println(1)
	} else if a == 3 {
		fmt.Println(2)
	} else {
		fmt.Println(3)
	}
```


#### 注意：
- if 后面的条件表达式尽量不使用小括号。

- else 不能够单独占一行，只能够跟在 if 标签的反大括号的后面。
- if 标签与 else 标签都必须添加 大括号。
- if 标签的 条件表达式 中能够申明一个变量，只能够在条件表达式中使用。例如：
```
if age := 20;age > 18 {
    
    
}
```

##### switch分支结构
**基本介绍：** switch语句用于基于不同条件执行的不同动作，每一个case分支都是唯一的，从上下文中逐一尝试，直到匹配位置。

**注意：**  
- switch的case后面不需要break，默认自带了break。

- 如果switch无匹配到的数据，则执行default中的代码。
- case后面的表达式能够有多个，使用逗号分开。
- default不是必须存在的。
- switch后面可以不带表达式，直接执行case中的代码。

执行结构为:
```
switch 条件表达式 {
	case 表达式一,表达式二 : 
	fmt.Println(1)

	case 表达式一,表达式二 : 
	fmt.Println(2)

	default:
	fmt.Println(3)
}
```

执行代码为：
```
switch a/2 {
	case 1,2 : 
	fmt.Println(1)

	case 3,4 : 
	fmt.Println(2)

	default:
	fmt.Println(3)
}
```

#### 循环控制
操作模式一：
```
    for 初始化变量/变量; 循环条件表达式 ; 循环变量迭代 {
		//中间操作。
	}
	
	//例如：
	for a = 3;a < 10;a++ {
		fmt.Println(a)
	}
```

操作模式二：
```
    for 循环条件表达式 {
        //中间操作    
    }
    
    //例如：
    for a < 10 {
		fmt.Println(a)
		a++
	}
```

操作模式三：
```
    for {
        //中间操作，相当于无限循环，需要与break结合来停止循环。
    }
    
    //例如：
    for {
		if a < 10 {
			fmt.Println(a)
			a++
		} else{
			fmt.Println("循环终止")
			break;
		}
	}
```

#### break的完整使用
##### 1、直接跳出当前循环
使用方式：
```
for i:=1;i<9;i++ {
    if i ==7 {
        break
    }
}
```

##### 2、跳出指定的某个循环
使用方式：
```
	a1:
	for k:=1;k<5;k++ {
		fmt.Printf("k的值为：%d\n",k)

		a2:
		for j:=1;j<=k;j++ {
			fmt.Printf("J的值为：%d\n",j)
			if j == 2 {
				break a2
			}
		}

		if(k == 4) {
			break a1
		}
	}
```

#### continue 完整使用
##### 1、基本介绍：
continue语句用于结束本次循环，继续执行下一次循环。continue语句出现在多层嵌套的循环语句体中时，可以通过标签指明要跳过哪一层循环。

##### 2、结束本次循环
使用方式为：
```
for o :=0;o<10;o++ {
	if o ==2 {
		continue;
	} 
	fmt.Println(o)
}
```

##### 3、跳过某一层循环
使用方式为：
```
a1:
for p := 0;p<5;p++ {
	fmt.Println(p)

	for q := 0;q<=p;q++ {
		if q ==2 {
			continue a1;
		} 
		fmt.Println(q)
	}
}
```

#### goto 完整使用
##### 1、基本介绍：
Go语言中的 goto语句能够实现无条件的转移到程序中指定的标签的位置。

##### 2、注意：
Go语言中不推荐使用 goto语句，以免造成程序混乱。

##### 3、使用方式：
执行代码为：
```
if a<10 {
    goto lable;
}

var k int;
lable;

k++;

```



## 三、Go语言的函数、包与错误处理
### 1、Go语言函数
#### 1）函数的基本语法
##### 格式：
```
func 函数名 (形参列表) (返回值列表) {
    
    //中间操作
    
    return 返回值列表
}
```

##### 注意：
- 形参列表能够有多个数据，中间使用逗号隔开。
- 形参的形式是 参数名在前，参数类型在后。
- 返回值列表可以有多个，使用逗号隔开。
- 返回值只需要有参数的类型即可。

##### 简单代码：
```
func main(){
	var n1 int
	var n2 int
	fmt.Println("请输入两个整数：")
	fmt.Scanf("%d %d",&n1,&n2 )

	n3 := a1(n1,n2)
	fmt.Println(n3)
} 

func a1 (n1 int,n2 int) int {
	return n1+n2
}
```

#### 函数返回多个值
使用多个变量来接收函数的返回值。
```
func main(){
	var n1 int
	var n2 int
	fmt.Println("请输入两个整数：")
	fmt.Scanf("%d %d",&n1,&n2 )

	n4,n5 := A2(n1,n2)
	
	fmt.Println(n4)
	fmt.Println(n5)
} 

func A2 (n1 int,n2 int) (int,int) {
	return n1+n2,n1-n2
}
```
**注意：** 如果不需要接收那么多的返回值，可以使用 _ 占位。而 _ 占位的那个返回值无任何意义。
```
func main(){
	var n1 int
	var n2 int
	fmt.Println("请输入两个整数：")
	fmt.Scanf("%d %d",&n1,&n2 )

	n3 := utils.A1(n1,n2)
	fmt.Println(n3)

	_,n5 := A2(n1,n2)

	fmt.Println(n5)
} 

func A2 (n1 int,n2 int) (int,int) {
	return n1+n2,n1-n2
}
```



### 2、Go语言包
#### 1）简介：
包的概念是将多个 .go 文件进行逻辑的分开，避免集中在一起混乱。

#### 2）操作方式：
第一步：创建一个utils包，包里面创建一个 util1.go 文件，文件中存放一个函数：
```
package utils

func A1 (n1 int,n2 int) int {
	return n1+n2
}
```

第二步：创建一个主函数的main包，里面存放的是主函数 main.go 文件。如果需要调用utils包下面的函数，则需要在import中引入这个对应的包的路径。
```
package main
import(
	"fmt"
	"go_code/chapter03/func1"
)

```

第三步：真正调用函数的时候，需要使用 **包名+函数名** 的方式进行调用。
```
func main(){
	var n1 int
	var n2 int
	fmt.Println("请输入两个整数：")
	fmt.Scanf("%d %d",&n1,&n2 )

	n3 := func1.A1(n1,n2)
	fmt.Println(n3)
}
```

#### 注意：
- 当需要引入外界的函数的时候，外界的函数名必须要大写，否则访问权限不够。
- 当文件进行打包的时候，一个包对应的就是一个文件夹。
- 当使用 import 导入包的时候，路径是从 GOPATH 路径下的 src 路径后面的路径。 
- 同一个包名下，不能够有相同的 函数名 以及 全局变量名。
- 可以为包名取别名，当调用的时候直接使用别名即可。例如：
```
import(
	"fmt"
	utils "go_code/chapter03/func1" 
)
```

### 3、Go语言内存结构
#### 栈区：
基本数据类型一般存储在栈区（使用的是编译器的逃逸机制来进行区分的）。

#### 堆区：
引用数据类型一般存储在堆区（也是使用编译器的逃逸机制来进行区分的）。

#### 代码区：
代码存放的位置。

### 4、函数的递归调用
#### 1）基本介绍：
一个函数在函数体内又调用了本身，我们称为递归调用。

### 5、函数的注意事项
- 函数的形参列表与返回值都可以为多个。
- 函数列表和返回值列表的数据类型可以是值类型和引用类型。
- 函数的命名遵循标识符命名规范，首字母不能使数字，注意首字母的大小写的使用场景。
- 函数中的变量是局部变量，函数外面不生效。
- **基本数据类型和数组默认是值传递** 的，即进行值拷贝操作，在函数中对数组进行操作，不会影响原来的数据。
- 如果希望函数内的变量能够修改函数外的变量，可以传入变量的地址&，函数内以指针的方式进行操作变量，效果类似于引用。
- Go语言不支持函数重载
- Go语言中 **函数也是一种数据类型**，可以复制给一个变量，则该变量的数据类型就是一个函数类型的变量。
例如:
```
func getsum(n1 int ,n2 int) int {
    return n1 + n2
}

main(){
    //这个操作就是将 getsum 的数据类型传递给 变量a。
    a := getsum
    
    n3 := a(10,20)
    
}
```

-Go语言中，函数是一种数据类型，所以可以作为形参传入。
```
func getsum(n1 int ,n2 int) int {
    return n1 + n2
}

func orther(sum getsum(int,int) , n1 int , n2 int)  {
    return n1 + n2
}
```

- Go语言种支持自定义数据类型。
    - 基本语法为： type 自定义数据类型名 数据类型。 例如： type myint int
    - 函数类型： type mysum func(int , int) int 。意思是 mysum 类型时这个func函数的数据类型。
- Go语言支持可变形参。代码为：
```
func main(){
	n1 := A2(1,2,3,4,5)
	fmt.Println(n1)
} 

func A2 (n1 int,args... int) int {
	sum := n1
	
	for i := 0 ; i<len(args); i++ {
		sum += args[i]
	}	

	return sum
}
```

### 6、init函数
#### 1）基本介绍：
每一个源文件中都可以包含一个init函数，该函数会在 main 函数执行之前执行，被GO运行框架调用。也就是说init函数会在main函数之前执行。

#### 2）注意事项：
- 当main.go文件中与引入的util.go文件中都有init()函数的时候，会先执行util.go文件中init()函数，再执行main.go文件中的init()函数。

- 完整的执行顺序为： **全局变量的执行--》导入的文件的init()方法的执行--》main.go文件中的init()方法的执行--》main()方法的执行**。
- init()函数最主要的工作就是做一些初始化的工作。


### 7、匿名函数
#### 1）介绍：
Go语言支持匿名函数，匿名函数就是没有名字的函数，如果我们某个函数只是希望使用一次，那么我们就可以考虑使用匿名函数，匿名函数也可以实现多次调用。

#### 2）匿名函数使用方式一：
在定义匿名函数的时候直接调用，在后面添加数据传入匿名函数的值即可。这种方式只能对匿名函数调用一次。
```
n6 := func (n1 int,n2 int) int {
		return n1+n2
	}(10,20)
```

#### 3）匿名函数的使用方式二：
将匿名函数赋给一个变量，然后使用该变量来调用匿名函数。
```
a := func (n1 int,n2 int) int {
		return n1+n2
	}

n7 = a(10,20)  //调用 变量a 即可对匿名函数的调用。

```

#### 4）全局匿名函数
如果将匿名函数赋值给一个全局变量，那么这个匿名函数既能够称为一个全局匿名函数。
```
var (
	Pk = func (n1 int,n2 int) int {
		return n1+n2
	}
)

//调用方式：
op := Pk(10,20);


```

### 8、闭包
#### 1）基本介绍：
闭包就是**一个函数** 和其**相关的引用环境**组合的一个整体。


### 9、函数defer--延时机制
#### 1）为什么需要defer？
在函数中，程序员经常需要创建资源（比如：数据库的连接、文件句柄、锁等），为了在函数执行完成之后，能够及时释放资源，GO语言设计者提供了defer。

#### 2）使用方式：
使用defer标记的语句，暂时不执行，会将defer后面的语句亚入岛独立的栈中暂存。  
待到函数执行完毕以后，再从defer栈中，将栈中的语句按先入后出的顺序进行出栈并执行。  
也就是说，defer在函数执行完毕后执行。
```
defer fmt.Println("第一个defer")
defer fmt.Println("第二个defer")
```

#### 3）defer使用细节：
- 当 Go 保存defer的语句的时候，会保存使用到的相关的值也写入到语句中进行保存。
- defer很好的使用场景是用于函数执行完毕以后，及时释放资源。


### 10、函数的参数传递方式
#### 1）值传递：
基本数据类型 int系列、float系列、bool、string、数组与结构体struct。

#### 2）引用传递：
指针、slice切片、map、管道chan、interface等都是引用类型。

#### 3）值传递与引用传递的特点：
- 其实不管是值传递还是引用传递，值传递的都是进行值得拷贝，引用传递是地址的拷贝。
- 值类型默认是值传递，变量直接存储值，内存通常在栈中分配。

- 引用类型默认是引用传递。变量存储的是一个地址，这个地址对应的空间才真正存储数据。没存通常分配在堆上，当没有任何变量引用这个地址时，改地址对应的数据空间就回城为一个垃圾，待GC来进行回收。
- 如果希望函数内的变量拉力修改函数外的变量，可以穿入变量的地址&，函数内以指针的方式操作变量，从效果上看类似引用。

### 11、变量的作用域
- 在函数内部声明的变量就是局部变量，作用域仅限于函数内部；
- 函数外部声明的变量叫做全局变量，作用域在整个包中都有效，如果首字母大写，则用用于在整个程序都有效。

- 如果变量是一个代码块，那么这个变量的作用域就在该代码块中。例如if、for循环定义的变量。


### 12、字符串常用的系统函数
#### 1）统计字符串的长度，按照字节的方式: len(str)
```
//输出字符串的长度。
fmt.Println(len(str))
```

#### 2）字符串的遍历，同时能够处理中文问题：r := []rune(str)
```
r := []rune(str)
for i:=0 ;i<len(r) ;i++ {
    fmt.Println(r[i])
}
```

#### 3）字符串转整数
使用 strconv 包的函数。
```
n,err := strconv.Atoi(str)

if err != nil {
    fmt.Println("转换错误",err)
} else {
    fmt.Println("转换成功",n)
}
```

#### 4）整数转字符串
使用 strconv 包的函数。
```
str = strconv.Itoa(123)
```

#### 5）字符串转 byte[]：
```
var bytes = []byte(str)
```

#### 6）byte[]转换为字符串：
```
var bytes = []byte{12,13,14}
str := string(bytes)
```

#### 7）查看字符串是否在指定的字符串中：
strings.Contains("长串",子串"); 返回值为true或者false。
```
bl = strings.Contains("asdfg","asd")
```

#### 8）统计字符串有几个指定的子串
```
num := strings.Count("adsfaf","as")
```

#### 9）返回字符串中第一次出现的子串的index值，如果不存在，则返回-1：
```
index := strings.Index("asdfdf","af")
```

#### 10）返回字符串中最后一次出现的子串的index值，如果不存在，则返回-1：
```
index := strings.LastIndex("asdfdf","af")
```

#### 11）将一个字符串中的指定的字段替换为其他的字段：
```
//第一个参数：将对哪个字符串进行操作
//第二个参数：字符串中的哪个子串需要被替换
//第三个参数：将字符串中需要被替换的子串替换为这个串
//第四个参数：替换多少次，如果为 -1 的话，就是全部替换
str := strings.Replace("adsfgad","as","qq"，-1)
```

#### 12）按照指定的某个字符进行拆分为多个字符串：
```
strArr := strings.Split("sdfaf,tyru,afs",",")

for i := 0;i<len(strArr);i++ {
    fmt.Println(strArr[i])
}
```

#### 13）将字符串进行整体的大小写替换：
```
//全部转换为小写
str := strings.ToLower("GofsgT")

//全部转换为大写
str1 := strings.ToUpper("GadTtyY")
```

#### 14）去掉字符串左右两边的空格：
```
str := strings.TrimSpace("  adf法 按时打发 adr  ")
```

#### 15）去掉字符串左右两边指定的字符：
```
//去掉两边的字符
str1 := strings.Trim("!adsgf!","!")

//去掉字符串左边指定的字符
str2 != strings.TrimLeft("!adsf!","!")

//去掉字符串右边指定的字符
str2 != strings.TrimRight("!adsf!","!")
```

#### 16）判断字符串以什么开头与以什么结尾：
```
//判断字符串以什么开头，返回值为true/false
str := strings.HasPrefix("!!!afa","!!!") //返回值为真

//判断字符串以什么结束，返回值为true/false
str := strings.HasSuffix("!!!afa","!!!") //返回值为假

```

### 13、时间与日期函数
#### 1）基本介绍：
程序时常用到时间与日期的相关的函数。需要引入 time 包。

#### 2）获取当前时间，以及格式化日期：
```
    //获取当前时间
	now := time.Now()

	fmt.Printf("%v\n",now.Year())
	//将月份转换为数字
	fmt.Printf("%v\n",int(now.Month()))
	fmt.Printf("%v\n",now.Day())
	fmt.Printf("%v\n",now.Hour())
	fmt.Printf("%v\n",now.Minute())
	fmt.Printf("%v\n",now.Second())

	//使用time包下的Format()方法来进行格式化时间，
	//注意：Format里面的数据不可改变，这即使模板
	fmt.Printf(now.Format("2006-01-02 15:04:05"))

```

#### 3）时间的常量：

时间 | 中文 | 差距
---|---|---
Nanosecond | 1纳秒 |
Microsecond | 1微秒 | 1000 纳秒
Millisecond | 1毫秒 | 1000 微秒
Second | 1秒 | 1000 毫秒
Minute | 1分 | 60秒
Hour | 1小时 | 60分钟

##### 常量的作用：
能够得到一个固定的精确的时间段。比如： myTime := 100*time.Millsecond

能够结合 Sleep 来使用时间常量。例如：time.Sleep(100*time.Millsecond)

#### 4）time的Unix和UnixNano方法：
Unix()方法的时间:从1970年到时间点t所经过的时间，以秒为单位。
```
now := time.Now().Unix();
```

UnixNano()方法的时间：从1970年到时间点t所经过的时间，以纳秒为单位。
```
now := time.Now().UnixNano();
```


### 14、内置函数
#### 1）简介：
Go语言中为了编程方便，提供了一些函数，这些函数能够直接使用。称为内置函数。

#### 2）常用的内置函数：
- len() ： 用于求长度。比如：string、数组、slice、map、channel
- new() ： 从1970年到时间点t所经过的时间，以秒为单位。
- make():  用来分配内存，主要是用来分配引用类型，比如channel、map、slice等。


### 15、错误处理
#### 1）简介：
Go语言中，默认是发生错误以后，程序就会崩溃。  

Go语言中追求简洁优雅，所以，Go语言不支持 try...catch...finally 这种处理方式。  

Go语言中引入的处理方式为：defer、panic、recover  

**2）使用流程：**  
Go语言可以抛出一个panic的异常，然后在defer中通过recover来捕获这个异常，然后进行正常处理。


**3）使用 defer + recover 来进行异常的捕获与处理：**  
```

func main(){
	//使用 defer + recover 方式来进行异常的捕获与处理
	defer func() {
		//先使用recover()方法来捕获到异常
		err := recover()
		if err != nil {
			fmt.Println("异常问题为：",err)
		}
	}()

	num1 := 10
	num2 := 0
	resu := num1/num2
	fmt.Println(resu)
}
```

**4）错误处理的好处：**  
进行错误处理后，程序不会轻易挂掉，如果加入预警代码，就可以让程序更加的健壮。

##### 自定义错误：
Go 程序中，也支持自定义错误， 使用 errors.New 和 panic 内置函数。  
- errors.New("错误说明") ,  会返回一个 error 类型的值，表示一个错误
- panic 内置函数 ,接收一个 interface{}类型的值（也就是任何值了）作为参数。可以接收 error 类型的变量，输出错误信息，并退出程序.

**使用方式：**  
```
//这个方法是自定义的异常处理方法，可以根据传入的参数来进行异常的控制
func err1(name string) error {
	if name == "abc.txt" {
		//返回 nil 表示程序没有问题，返回以后程序继续执行
		return nil
	} else{
		//返回自定义异常
		return errors.New("出现错误")
	}
}

func main(){
	//绑定自定义异常
	err := err1("ac.txt")

	if err != nil {
		panic(err)
	}

	fmt.Println("主函数继续执行")
}
```


## 四、数组与切片与map
### 1、数组
#### 1）简介：
数组可以存放多个同一个类型的数据。数组是一种数据类型，在Go语言中，**数组是值类型**。

#### 2）快速入门：
```
	var a [7] int

	for i := 0;i<7;i++ {
		a[i] = i+5
		fmt.Println(a[i])
	}
```
 
#### 3）数组定义:
数组的定义：  
> var 数组名 [数组大小] 数据类型

例如：

```
//第一种方式
var a [7] int

//第二种方式
var arr [5]int = [...]int {1,2.3}
```

#### 4）数组的内存布局：
- 数组的地址可以通过数组名来获取 &a
- 数组的第一个元素的地址，就是数组的首地址
- 数组的各个元素的地址间隔是依据数组的类型决定，比如 int64 -> 8	int32->4...


#### 5）数组的使用：
数组的遍历：
- 方式一：使用 for循环
- 方式二：使用 for-range循环

for-range循环遍历数组：
```
    //index 存放的是数组的下标
	//value 存放的是这个数组下标对应的数组的值
	//range 不变的固定单词
	//a 需要遍历的数组
	for index,value := range a {
		fmt.Println("数组的下标：",index)
		fmt.Println("数组对应的下标的值：",value)
		fmt.Println("数组a[index]的值：",a[index])
	}
```

#### 6）数组的使用细节：
- 数组是多个相同类型数据的组合,一个数组一旦声明/定义了,其长度是固定的, 不能动态变化。

- var arr []int	这时 arr 就是一个 slice 切片。
- 数组中的元素可以是任何数据类型，包括值类型和引用类型，但是不能混用。
- 数组创建后，如果没有赋值，有默认值(零值)。
    - 数值类型数组：默认值为 0。
    - 字符串数组：	默认值为 ""。
    - bool 数组： 默认值为 false。
- 数组下标必须在指定范围内使用，否则报 panic。
- Go 的数组属值类型， 在默认情况下是值传递， 因此会进行值拷贝。数组间不会相互影响。
- 长度是数组类型的一部分，在传递函数参数时 需要考虑数组的长度。意思就是：传递的数组的长度必须与形参的长度定义的一致。
- 如想在其它函数中，去修改原来的数组，可以使用引用传递(指针方式)。
```
func test02(arr *[3]int){
    (*arr)[0] = 0;
}
```

### 2、切片
#### 1）简介：
- 切片的英文是 slice。
- 切片是数组的一个引用，因为**切片是引用数据类型**，在进行传递的时候，遵守的是引用传递机制。

- 欺骗的使用方式与数组类似，遍历切片、访问切片的元素和求切片长度的方式都是一样的。

- **切片的长度是可以变化的，因此切片是一个可以动态变化的数组。**

#### 2）基本语法：
**切片的定义：**  var 切片名 []类型

例如:
```
var a []int
```

#### 3）切片的基本demo：
```
    //创建一个数组
	var a [6] int = [...]int{1,2,3,4,5,6}

	//声明切片
	//表示 slice 这个切片 引用到 a 这个数组，
	//引用的 a 数组 的起始下标为1，结束下标为3（但是不包括3）
	//slice := a[1:3]  //创建切片的第一种方式
	var slice [] int = a[1:3] //创建切片的第二种方式

	fmt.Println("a数组的值为：",a)
	fmt.Println("slice切片的元素是：",slice)
	fmt.Println("slice的元素个数为：",len(slice))
	fmt.Println("slice的容量为：",cap(slice))
	
	//输出结果为：
	// a数组的值为： [1 2 3 4 5 6]
	// slice切片的元素是： [2 3]
	// slice的元素个数为： 2
	// slice的容量为： 5
```

#### 3）切片的内存形式：
![image](https://note.youdao.com/yws/api/personal/file/AD2FCFA2EC854D0C96C76195982DE526?method=download&shareKey=a8f14fdb9ef985542b3c7acc244fa2eb)

**对上面的分析图总结:**  
1.	slice 的确是一个引用类型  
2.	slice 从底层来说，其实就是一个数据结构(struct 结构体)  

```java
type slice struct { 
    ptr	*[2]int
    len	int
    cap	int

}
```

#### 4）切片的使用
##### 方式一：
简介：定义一个切片，然后让切片去引用一个已经创建好了的数组。
```
    slice := a[1:3]  //创建切片的第一种方式
	var slice [] int = a[1:3] //创建切片的第二种方式
	var slice = a[1:4] //创建切片的第三种方式
```

##### 方式二：
**简介：** 通过make来创建切片。   

**基本语法：** var 切片名 []type = make([]type,len,[cap])  
**参数介绍：**  
> type ： 指的是数据类型    
> len : 大小    
> cap : 指定切片容量，可选。如果你分配了 cap,则要求 cap>=len.  

**代码示例为：**
```
    var slice []int = make([]int,5,10)
	slice[1] = 2;
	slice[2] = 3;

	fmt.Println("slice的元素个数为：",len(slice))
	fmt.Println("slice的容量为：",cap(slice))
```

**总结为：**  
- 通过 make 方式创建切片可以指定切片的大小和容量。

- 如果没有给切片的各个元素赋值，那么就会使用默认值[int , float=> 0	string =>””	bool =>
false]
- 通过 make 方式创建的切片对应的数组是由 make 底层维护，对外不可见，即只能通过 slice 去访问各个元素.

##### 方式三
**简介：**  
定义一个切片，直接就指定具体数组，使用原理与make类似。

**使用代码：**  
```
	var slice []int = []int{1,12,3,4,5}
	
	fmt.Println("slice的元素个数为：",len(slice))
	fmt.Println("slice的容量为：",cap(slice))
```

#### 5）切片的遍历
##### 方式一：
**简介：**   
使用for 循环常规方式遍历。

**代码操作为：**  
```
	var slice []int = []int{1,12,3,4,5}

	for i:=0 ; i <len(slice) ; i++ {
		fmt.Println(slice[i])
	}
```

##### 方式二：
**简介：**   
使用for 循环常规方式遍历。

**代码操作为：**  
```
	var slice []int = []int{1,12,3,4,5}

	for i,v := range slice {
		fmt.Println(slice[i])
		fmt.Println(v)
	}
```

#### 6）切片使用细节
1)	切片初始化时 var slice = arr[startIndex:endIndex]  
说明：从 arr 数组下标为 startIndex，取到 下标为 endIndex 的元素(不含 arr[endIndex])。

2)	切片初始化时，仍然不能越界。范围在 [0-len(arr)] 之间，但是可以动态增长.

3)	cap 是一个内置函数，用于统计切片的容量，即最大可以存放多少个元素。

4)	切片定义完后，还不能使用，因为本身是一个空的，需要让其引用到一个数组，或者 make 一个空间供切片来使用。

5)	切片可以继续切片，使用方式相同。

6)	用 append 内置函数，可以对切片进行动态追加。代码为：
```
	var slice []int = []int{1,12,3,4,5}
	slice = append(slice,55,66,88,99)
```
 
7)	切片的拷贝操作。具体代码为：
```
    // copy(需要将数据存入的slice, 需要将数据复制出来的slice)
    var slice []int = []int{1,12,3,4,5}
	slice = append(slice,55,66,88,99)

	var slice1 []int = make([]int,5,10)
	copy(slice1,slice)

```
8）切片是引用类型，所以在传递时，遵守引用传递机制。（也就是说传递的是地址）


### 3、string与slice
#### 1）使用细节：
**①** **string底层是一个byte数组**，因此string也可以进行切片处理。其实string也就是一个切片，可以使用切片的方法。

使用代码为：
```
var a string = "ddsfdsaf"

slice1 := a[1:3]

for i,_ := range slice1 {
	fmt.Printf("%c\n",slice1[i])
}
```
**②** string 是不可变的，也就说不能通过 str[0] = 'z' 方式来修改字符串。

**③** 如果需要修改字符串，可以先将 string -> []byte / 或者 []rune -> 修改 -> 重写转成 string 。


### 4、二维数组
#### 1）二维数组语法：
var 数组名 [数组的行][数组的列] 数组类型 。 例如： var arr [3][4] int

#### 2）使用demo：
```
var arr[3][4] int
for i :=0;i<3;i++ {
	for j := 0 ;j<4;j++ {
		arr[i][j] = i + j;
	}
}
for i :=0;i<3;i++ {
	for j := 0 ;j<4;j++ {
		fmt.Print(arr[i][j],"  ")
	}
	fmt.Println()
}
```

#### 3）使用方式：
##### ① 使用方式一：
语法: var 数组名 [大小][大小]类型

比如: var arr [2][3]int  ， 再赋值。

##### ② 使用方式 2: 直接初始化
声明方式：
- var 数组名 [大小][大小]类型 = [大小][大小]类型{{初值..},{初值..}}

- var 数组名 [大小][大小]类型 = [...][大小]类型{{初值..},{初值..}}
- var 数组名	= [大小][大小]类型{{初值..},{初值..}}
- var 数组名	= [...][大小]类型{{初值..},{初值..}}

例如：
```
var arr [2][3] int = [2][3]int{{1,2,3},{4,5,6}}

var arr1 = [2][3]int{{1,2,3},{4,5,6}}
```

#### 4）二维数组的遍历
##### ① 使用for循环遍历
```
for i :=0;i<3;i++ {
	for j := 0 ;j<4;j++ {
		fmt.Print(arr[i][j],"  ")
	}
	fmt.Println()
}
```
##### ② 使用for-range遍历
```
for i,value1 := range arr {
	for j,value2 := range value1 {
		fmt.Printf("下标为%d %d \n",i,j)
		fmt.Println(value2)
	}
}
```


### 5、map
#### 1）基本介绍：
map就是key-value的数据结构，又称为字段或者关联数组。

#### 2）map的声明：
var 变量名 map[key的类型] value的类型

例如：
```
var a map[string]string 
var a map[string]int 
var a map[int]string
var a map[string]map[string]string

```

#### 3）map的key与value的类型：
 key与value 可以是很多种类型，比如 bool,  数字，string,  指针, channel ,  还可以是只包含前面几个类型的 接口, 结构体, 数组

#### 4）map的使用前提：
***当我们声明了map以后*，不能够直接使用这个map，需要使用make方法来分配内存以后才能够对map进行使用。**

使用案例：
```
func main() {

	var map1 map[string]string
    //在使用map之前，需要使用make方法来分配空间，10表示的是分配的空间大小
	map1 = make(map[string]string,10)

	map1["1"] = "zsl"
	map1["2"] = "tsl"

	fmt.Println(map1)
}

```

#### 5）map使用说明：
- map使用之前必须要先 make。
- map的key不能够重复。
- map的value时无序的数据结构。
- map的value恶意是重复的。


#### 6）map的使用方式：
##### ①方法一：
```
var map1 map[string]string

map1 = make(map[string]string,10) 

map1["1"] = "zsl"
map1["2"] = "tsl"
```

##### ②方法二（推荐使用）：
```
map1 := make(map[string]string)

map1["1"] = "zsl"
map1["2"] = "tsl"
```

##### ③方式三：
```
map1 := map[string]string{
    "1":"zsl",
    "2":"tsl",
}
```

#### 7）map的增删改查：
##### ① map的增加与更新：
map["key"] = value ;  

**当key已经存在的时候，就是执行更新的操作。**

代码为：
```
map1 := make(map[string]string)

map1["1"] = "zsl"
map1["2"] = "tsl"
```

##### ② map的删除：
delete(map,"key")  

**delete是一个内置函数，如果key存在，就删除key-value这个键值对，如果不存在，那么就不执行操作，且不会报错。**

代码为：

```
delete(map1,"1")
```

使用细节：  
> 如果我们要删除 map 的所有 key ,没有一个专门的方法一次删除，可以遍历一下 key, 逐个删除。

> 或者 map = make(...)，make 一个新的，让原来的成为垃圾，被 gc 回收。

##### ③ map的查找
使用方式： val findresult = map1["1"]   
注释说明：   
- 如果在这个map中存在 "1" 这个key，那么findresult就会返回的结果是true，否则为false。
- val中存的是这个 key 的值。

代码执行：
```
var map1 map[string]string

map1 = make(map[string]string,10)

map1["1"] = "zsl"
map1["2"] = "tsl"

val,findresult := map1["1"]

fmt.Println(val)  //结果为 zsl
fmt.Println(findresult)  //结果为 true
```

#### 8）map的遍历：
map的遍历是使用的 for-range 来进行遍历的。

简单的map遍历：
```
//最简单的遍历
var map1 map[string]string

map1 = make(map[string]string,10)

map1["1"] = "zsl"
map1["2"] = "tsl"

//遍历
for k,v := range map1 {
	fmt.Print(k," ")
	fmt.Println(v)
}

```
map中包含map的遍历：
```
//map中包括一个map
	map1 := make(map[string]map[string]string)

	map1["1"] = make(map[string]string,2)

	map1["1"]["name"] = "zsl"
	map1["1"]["age"] = "123"
	//遍历map中的map
	for k1,v1 := range map1 {
		fmt.Println("k1=",k1)
		for k2,v2 := range v1  {
			fmt.Print(k2," ")
			fmt.Println(v2)
		}
	}

```

#### 9）获得map的长度
```
len(map1)
```

#### 10）map排序
##### 基本介绍：
- golang 中没有一个专门的方法针对 map 的 key 进行排序；
- golang 中的 map 默认是无序的，注意也不是按照添加的顺序存放的，你每次遍历，得到的输出可能不一样；
- golang 中 map 的排序，是先将 key 进行排序，然后根据 key 值遍历输出即可。

##### 代码演示：
```
    map1 := make(map[int]int)

	map1[0] = 88;
	map1[1] = 23;
	map1[5] = 45;
	map1[3] = 21;
	map1[4] = 99;
	map1[2] = 0;
	map1[6] = 534;
	map1[7] = 23;
	map1[8] = 56;
	map1[9] = 12;

	fmt.Println(map1)

	//将所以的key放入到切片中
	var keys []int
	for k,_ := range map1 {
		keys = append(keys,k)
	}

	//排序
	sort.Ints(keys)

	fmt.Println(keys)

	//按照key的顺序进行排序输出
	for _,k := range keys {
		fmt.Println(k," = ",map1[k])
	}
```

### 11）map使用细节：
- map是引用类型，遵守引用类型传递机制。也就是说函数接受一个map，修改以后，会直接修改原来的map数据。
- map的容量达到扩容以后，在想map增加元素，会自动扩容，并且不会报错。
- map 的 value 也经常使用 struct 类型，更适合管理复杂的数据(比前面 value 是一个 map 更好)。

### 6、map切片
####  1）基本介绍：
切片的数据类型如果是map，那么就是称为 map切片。这样子**map也就可以动态变化**了。

#### 2）基本语法：
var 切片名 []map[数据类型]数据类型  

例如：var map1 []map[string]string

#### 3）案例演示：
**要求：** 使用一个 map 来记录 妖怪 的信息 name 和 age, 也就是说一个 妖怪 对应一个 map,并且妖怪的个数可以动态的增加。

```
	var map1 []map[string]string
	map1 = make([]map[string]string, 2)

	if map1[0] ==nil {
		map1[0] = make(map[string]string,2)
		map1[0]["name"] = "zszl"
		map1[0]["age"] = "123"
	}

	if map1[1] ==nil {
		map1[1] = make(map[string]string,2)
		map1[1]["name"] = "weqer"
		map1[1]["age"] = "33"
	}

	moremap := map[string]string {
		"name":"ttt",
		"age":"878",
	}

	//动态的添加map切片
	map1 = append(map1,moremap)

	fmt.Println(map1)
```

## 五、排序与查找与设计模式
### 1、排序
#### 1）介绍：
排序是将一组数据，依指定的顺序进行排序的过程。

#### 2）排序的分类：
##### 内部排序：
指的是将需要处理的所有数据都加载到内部存储中进行排序。包括（交换式排序法、选择式排序法、插入式排序法等）。
##### 外部排序：
经常用作数据量过大、无法加载全部数据到内存中的场景。需要借助外部存储进行排序。包括（合并排序法、直接合并排序法等）。

#### 3）交换式排序法包括--冒泡排序（Bubble sort）

##### 基本思想：
让前面的数与后面的数相比较，如果根据需求来进行位置的调换操作。

##### 基本操作：
![image](https://note.youdao.com/yws/api/personal/file/683DC2BF6F1E41D180D780983BD5B8FD?method=download&shareKey=aa0b40df61c5d4d326b72dd452a41782)

##### 基本概念：
一共会经过数组长度n的（n-1）次；每一轮比较就会确定一个数的位置；每一轮比较的次数再逐渐的减少（第一次是比较n-1次，第二次就是比较到n-2次，一直到最后一次的比较即可）。

##### 代码实现：
```
/**
 * 冒泡排序
 * @param args
 */
public static void main(String[] args) {
	int a[] = {12,95,56,82,73,91,106,22,19,64};
	int i = 0; 
	int j = 0;
	int temp = 0;
	
	//第一个for循环，是专门针对于for循环是需要进行多少次循环
	for ( i=0;i<a.length-1;i++ ) {
		//第二个for循环，是为了让这个冒泡排序进行多少个数的比较
		for( j=0;j<a.length-i-1;j++ ) {
			if( a[j]>a[j+1] ) {
				temp =a[j];
				a[j] = a[j+1];
				a[j+1] = temp;
			}
		}
		
	}
	
	for (int k=0;k<a.length;k++) {
		System.out.println(a[k]);
	}
}
```

### 2、查找
#### 1）二分法查找
##### 实现原理：
二分法查找是在一个**有序的数组**中进行查询。

##### 实现流程：
![image](https://note.youdao.com/yws/api/personal/file/062F1733F6194BA9B662FBC2E51F25B6?method=download&shareKey=0655afbc697aaf8e53ada4c0c97716f6)

##### 代码实现：
```
public static void main(String[] args) {
		int[] a = new int[] { 12, 23, 34, 45, 56, 67, 77, 89, 90 };

		int aa = rank(3,a);
		if(aa == -1) {
			System.out.println("没查找到结果");
		} else {
			System.out.println("结果查找到了");
		}
	}
	
	/**
	 * 二分法查找
	 * @param key
	 * @param nums
	 * @return
	 */
	public static int rank(int key,int nums[])
	{
		int low=0;
		int high=nums.length-1;
		int notFind=-1;
		while(low<=high)
		{
			int mid=low+(high-low)/2;

			if(nums[mid]>key)
			{
				high=mid-1;
			}else if(nums[mid]<key)
			{
				low=mid+1;
			}
			else
			{
				return mid;
			}
		}
		return notFind;
	}
	
```
### 3、设计模式
#### 1）工厂模式
##### 基本介绍：
在Golang中没有构造函数，当我们需要创建结构体的时候，可以使用工厂模式来创建结构体实例。

创建一个结构体，里面提供一个工厂方法，然后提供变量的Get方法，让外界能够调用这个Get方法来实现对属性的调用。提供Set方法，为这个变量赋值。  

##### 代码实现：
先创建一个go文件：
```
//创建一个结构体
type student struct {
	name string
	age int
}

//使用工厂模式来创建struct实例
func NewStudent(name string,age int) *student {
	return &student{name,age}
}

//提供get方法
func (std student) GetName() string {
	return std.name
}
func (std student) GetAge() int {
	return std.age
}

//提供set方法
func (std *student) SetName(name1 string) {
	(*std).name = name1
}
func (std *student) SetAge(age1 int) {
	(*std).age = age1
}
```

再创建一个主函数，来实现对其的调用：
```
import (
	"go_code/project3/utils"
	"fmt"
)

func main() {
	var std = utils.NewStudent("zsl",24)
	std.SetName("sss")
	std.SetAge(555)
	fmt.Println(std.GetName())
	fmt.Println(std.GetAge())
}
```




## 六、面向对象
### 1、面向对象的特征
>- Go语言中的面向对象，不是类的概念，而是使用 **结构体** 来代替类的概念。

>- Go语言也支持面向对象编程（OOP）。但是和传统的面向对象编程有区别，并不是纯粹的面向对象语言。所以Golang支持面向对象编程特性是比较准确的。

>- Golang面向对象编程是非常简洁的，去掉了传统的OOP语言的继承、方法重载、构造函数和析构函数、隐匿的this指针等。

>- Golang仍然有面向对象的继承、封装、和多态的特性，只是实现的方式和其他OOP语言不一样。

>- Golang面向对象很优雅，OOP本身就是语言类型系统（type system）的一部分，通过接口（interface）关联，耦合降低，非常灵活。Golang中对接口的使用非常重要，非常灵活。

### 2、结构体
#### 1）结构体的特征：
- 将一类事务的特性提取出来，形成一个新的数据类型，就是一个结构体。

- 通过结构体，来创建多个变量（相当于是多个 实例/对象）。
- 这个结构体，可以是猫、person、或者是工具类等。

#### 2）结构体的使用

##### 结构体创建的基本语法
```
//基本语法
type 结构体名称 struct {
    field1 type  //字段、属性
    field2 type  //字段、属性
}

```

##### 结构体变量创建的基本语法
**方式一：** 直接声明变量。
```
type Cat struct {
	Name string
	Age int8
	Color string
	Hobby string
}

//创建结构体变量
var cat Cat

//为变量赋值
```


**方式二：** 在创建的时候直接赋值。  
```
type Cat struct {
	Name string
	Age int8
	Color string
	Hobby string
}

//创建结构体变量
var cat Cat = Cat{"白猫",8,"red","asdf"}

//返回值为直接变量
var cat2 = Cat {
    Name:"sad",
    Age:32,
    Color:"ff",
    Hobby:"we",
}

cat3 := Cat {
    Name:"sad",
    Age:32,
    Color:"ff",
    Hobby:"we",
}
```

**方式三：** 使用指针的方式来创建结构体变量。
```
type Cat struct {
	Name string
	Age int8
	Color string
	Hobby string
}

//为指针赋值
//方式一
var cat1 *Cat = new(Cat)
cat1.Name = "苯二八年"
cat1.Age = 23
fmt.Println(cat1.Name)
fmt.Println(cat1.Age)

//方式二
var cat2 *Cat = new(Cat)
(*cat2).Name = "asdf"
(*cat2).Age = 2
fmt.Println(cat2.Name)
fmt.Println(cat2.Age)

var cat3 *Cat = &Cat{"asd",23,"asd","yty"}

cat4 := &Cat{"asd",23,"asd","yty"}

cat5 := &Cat {
    Name:"sad",
    Age:32,
    Color:"ff",
    Hobby:"we",
}
```

**方式四：** 使用地址符方式进行赋值。
```
//创建一个猫的结构体
type Cat struct {
	Name string
	Age int8
}

var cat1 *Cat = &Cat{}

cat1.Name = "asdf"
cat1.Age = 2
fmt.Println(cat1.Name)
fmt.Println(cat1.Age)

```

**注意：**  
- 第三种与第四种的方式返回的是 *结构体指针*。
- 结构体指针访问字段的标准方式是 ： (*结构体指针).字段名; 例如：(*cat1).Name = "as"

- Go语言也支持另外一种访问结构体指针的方式： 结构体指针.字段名; 例如：cat1.Name = "a"



##### 结构体的创建
```
//创建一个猫的结构体
type Cat struct {
	Name string
	Age int8
	Color string
	Hobby string
}
```

##### 结构体的使用
```
//创建一个猫的结构体
type Cat struct {
	Name string
	Age int8
	Color string
	Hobby string
}

//创建一个Cat变量
var cat1 Cat
cat1.Name = "小白"
cat1.Age = 2
cat1.Color = "red"
cat1.Hobby = "吃鱼"

fmt.Println(cat1.Name)
fmt.Println(cat1.Age)
fmt.Println(cat1.Color)
fmt.Println(cat1.Hobby)
```

#### 3）结构体与结构体变量的异同
- 结构体是自定义的数据类型，代表的是一类事物。

- 结构体变量（也就是创建的结构体实例）是具体的一个事物的实例。

#### 4）结构体变量（实例）的内存结构
![image](https://note.youdao.com/yws/api/personal/file/F53C9E09B89F4D3BB195AFCD1998F400?method=download&shareKey=fe1c27c1812dff49b479999137c80020)

#### 5）结构体中使用结构体
```
//定义一个结构体
type point struct {
    x int
    y int
    
}

//在结构体中引用结构体
type rect struct {
    leftdown point
    rightdown point
    
}

```

#### 6）trg标签的使用
##### 简介：
struct 的每个字段上，可以写上一个 tag, 该 tag 可以通过反射机制获取，常见的使用场景就是序列化和反序列化。
##### 使用方式：
在 struct结构体 的字段后面添加 **\`json:"别名"\`** 的方式来实现。 

例如：
```
//当我们使用结构体的时候，一般是将字段进行大写，否则经常无法访问。
//放我们序列化的时候（也就是转换为json格式的数据的时候），一般是将字段更改为小写
//所以需要使用 tag 来实现相当于是在转换json的时候实现 为字段取别名。
	
//创建一个猫的结构体,使用 tag标签来进行取别名，专门用于序列化与反序列化
type Cat struct {
	Name string  `json:"name"`
	Age int8 `json:"age"`
}

var cat1 *Cat = &Cat{}

cat1.Name = "asdf"
cat1.Age = 2
fmt.Println(cat1.Name)
fmt.Println(cat1.Age)

//使用json来转换字符串,
//第一个返回值 jsonStr：这个值是一个byte切片
//第二个返回值 err：这个是返回错误信息
byteStr,err := json.Marshal(cat1)
jsonStr := string(byteStr)
if ( err != nil ) {
	fmt.Println("err为：",err)
} else {
	fmt.Println(jsonStr)
}
```


### 3、字段/属性
#### 1）基本属性
- 从概念上看：**结构体字段 = 属性 = filed** （通常是统一叫做字段）

- 字段是结构体一个组成部分，一般是基本数据类型、数组、也可以是引用类型。


#### 2）注意事项
- 字段的类型可以为：基本数据类型、数组、引用数据类型

- 在创建一个结构体变量以后，如果未给字段赋值，则对应着一个同类型的默认值。
    - 布尔值为false，
    - 数值为0，
    - 字符串为""，
    - 数组类型的默认值和它的元素类型相关，
    - 指针，slice，和 map 的零值都是 nil ，即还没有分配空间。
- 不同的结构体变量是互不影响的，一个结构体变量字段的更改，不会影响另外一个，**结构体是值类型**。

- 结构体的所有字段在内存中是连续的。
- 结构体是用户单独定义的类型，和其他类型进行转换时需要有完全相同的字段（名字、类型与个数）。
- 结构体进行 type 重新定义(相当于取别名)，Golang 认为是新的数据类型，但是相互间可以强转
- struct 的每个字段上，可以写上一个 tag, 该 tag 可以通过反射机制获取，常见的使用场景就是序列化和反序列化。
- **结构体的复制，默认是值拷贝，所以当我们将一个结构体赋值给另外一个结构体的时候，知识将结构体的数据进行拷贝，复制到另外个结构体中。**
- **结构体指针的复制，就是进行的地址的复制。那么对每一个结构体的改变，都会对那个数据做出改变。**



### 4、方法
#### 1）基本介绍：
在某些情况下，我们要需要声明(定义)方法。比如 Person 结构体:除了有一些字段外( 年龄，姓名..),Person 结构体还有一些行为比如:可以说话、跑步..,通过学习，还可以做算术题。这时就要用方法才能完成。

Golang中的方法是作用在指定的数据类型上的（即：和指定的数据类型绑定），因此自定义类型，都可以有方法，而不仅仅是struct。也就是说，方法是与数据类型绑定在一起的。

#### 2）方法与函数的区别
- 函数的调用方式:	函数名(实参列表)

- 方法的调用方式:	变量.方法名(实参列表)

- 对于普通函数，接收者为值类型时，不能将指针类型的数据直接传递，反之亦然

- 对于方法（如 struct 的方法），接收者为值类型时，可以直接用指针类型的变量调用方法，反过来同样也可以。

**函数的声明方式：**   
```
func 函数名 (形参列表) (返回值列表) {
    
    //中间操作
    
    return 返回值列表
}
```
**方法的声明方式：**  
- 参数列表：表示方法输入
-	recevier type : 表示这个方法和 type 这个类型进行绑定，或者说该方法作用于 type 类型
-	receiver type : type 可以是结构体，也可以其它的自定义类型
-	receiver :  就是 type 类型的一个变量(实例)，比如 ：Person 结构体 的一个变量(实例)
-	返回值列表：表示返回的值，可以多个
-	方法主体：表示为了实现某一功能代码块
-	return 语句不是必须的。
```
func (recevier type) methodName（参数列表） (返回值列表){
    方法体
    return 返回值

}
```
**注意：**  
方法的声明是需要与数据类型相绑定的，而函数却不需要与数据类型进行绑定。  

**方法的调用必须要使用 绑定的参数类型的对象 来进行调用。其他不能够调用。**


#### 3）方法的声明与调用demo
```
type Cat struct {
	Name string  `json:"name"`
	Age int8 `json:"age"`
}

func (cat Cat) test(){
	fmt.Println(cat.Name)
	fmt.Println(cat.Age)
}

func main() {

	var cat1 *Cat = &Cat{}
	cat1.Name = "asdf"
	cat1.Age = 2

	//调用绑定了 Cat数据类型 的 test() 方法
 	cat1.test()
}
```

#### 4）方法的调用和传参机制原理
- 在通过一个变量去调用方法时，其调用机制和函数一样。
- **不一样的地方时，变量调用方法时，该变量本身也会作为一个参数传递到方法(如果变量是值类型，则进行值拷贝，如果变量是引用类型，则进行地质拷贝)**


#### 5）方法的使用细节
-	结构体类型是值类型，在方法调用中，遵守值类型的传递机制，是值拷贝传递方式
-	如程序员希望在方法中，修改结构体变量的值，可以通过结构体指针的方式来处理

-	Golang 中的方法作用在指定的数据类型上的(即：和指定的数据类型绑定)，因此自定义类型， 都可以有方法，而不仅仅是 struct， 比如 int , float32 等都可以有方法。
- 	**方法的访问范围控制的规则，和函数一样。方法名首字母小写，只能在本包访问，方法首字母大写，可以在本包和其它包访问。**
- 	如果一个类型实现了 String()这个方法，那么 fmt.Println 默认会调用这个变量的 String()进行输出。


### 5、面向对象---抽象
#### 1）抽象简介
把一类事物的共有的属性（字段）和行为（方法）提取出来，形成一个物理模型（结构体），这种研究问题的方法就是**抽象**。

#### 2）银行账号案例
很多银行，很多银行卡，都具有账号，密码、余额等共有的属性，以及存款、取款等行为，将其抽取出来，实现一个结构体，就是实现一个抽象。
```
//账户的结构体
type Account struct {
	AccountNum string
	Passwd string
	Balance float64
}

func main(){

	account := Account{"123","123",64.4}
	account.Output("123",60)
	account.Input("123",66666)

}



//银行取钱
func (account Account) Output(passwd string,money float64) {

	if passwd != account.Passwd {
		fmt.Println("输入的密码错误")
		return
	}

	//查看银行卡余额是否足够
	if money > account.Balance || money <= 0 {
		fmt.Println("输入的金额不正确")
		return
	}
	account.Balance = account.Balance - money
	fmt.Println("取款成功，余额为：",account.Balance)
}

//银行存钱
func (account Account) Input(passwd string ,money float64) {
	if passwd != account.Passwd {
		fmt.Println("输入的密码错误")
		return
	}
	//查看输入的金额是否大于0
	if  money <= 0 {
		fmt.Println("输入的金额不正确")
		return
	}
	account.Balance = account.Balance + money
	fmt.Println("存款成功，账户余额为：",account.Balance)
}
```


### 6、面向对象---封装（encapsulation）
#### 1）基本介绍：
封装就是把抽象出来的字段和对字段的操作封装在一起，数据被保护在内部，程序的其他包只能通过被授权的操作（也就是方法）来对字段进行操作。也就是说，我们为字段封装到内部，让外界无法访问，然后提供绑定了字段的对外开放的方法，来实现对字段的操作。

#### 2）封装特性：
- 隐藏了细节操作。
- 可以对数据进行校验，保证数据的安全合理。
- 实现了高内聚，低耦合的操作，避免数据之间发生冲突。

#### 3）封装的实现原理：
- 对结构体中的属性进行封装（也即是将结构体中的属性小写，实现本包能够访问，其他包不能访问的操作）。

- 通过 包、方法 来实现封装的效果。

#### 4）封装的实现步骤：
第一步：将结构体、字段（属性）的首字母小写（这样子在其他包中就无法使用了，类似有private）。

第二步：给结构体所在包提供一个工厂模式函数，首字母大写（类似于构造函数）。

第三步：提供一个首字母大写的Set方法（类似于public），用于对属性判断并赋值。
```
func (var 结构体类型名) SetXXX(参数列表)(返回值列表) {
    //加入数据验证的业务逻辑
    var.属性 = 参数
}
```

第四步：提供一个首字母大写的 Get 方法(类似于public)，用于获取属性的值
```
func (var 结构体类型名) GetXxx() {
return var.age;

}
```

#### 5）封装的代码实现：
先创建一个 person.go 文件，里面创建一个 person 类型的结构体，提供这个结构体的工厂方法，以及这个结构体的 Get/Set方法（类似于java中的pojo）。

```
type person struct {
	name string
	age int8
}

//使用工厂方式创建一个person结构体对象
func NewPerson() *person{
	return &person{}
}

func (p *person) SetName(name1 string){
	if( len(name1)<10 && len(name1)>3 ) {
		p.name = name1
	} else {
		fmt.Println("姓名的长度只能在3-10之间，请重新输入。")
	}
}

func (p *person) SetAge(age1 int8){
	if( age1<120 && age1>0 ){
		p.age = age1
	} else {
		fmt.Println("年龄范围不正确")
	}
}

func (p *person) GetName() string {
	return p.name
}

func (p *person) GetAge() int8 {
	return p.age
}
```

在主函数中创建这个 person 对象，调用方法。
```
func main(){

	person := model.NewPerson()
	person.SetAge(92)
	person.SetName("df33")
	name := person.GetName()
	age := person.GetAge()
	fmt.Println("姓名为：",name,"年龄为：",age)
}
```

### 7、面向对象---继承
#### 1）基本介绍：
继承能够解决代码复用，让编程更接近人类思维。

当多个结构体存在相同的属性（字段）和方法的时候，只需要 **嵌套一个通用的结构体** 即可。

#### 2）继承的代码实现：
先创建一个结构体，然后让 Man 与 Woman 来继承 person 这个结构体：
```
package people

type person struct {
	Name string
	Age int8
}

type Man struct {
	//Man继承了 person 的属性
	person
	LastName string
}

type Woman struct {
	//woman 继承了 person 的属性
	person
	FirstName string
}
```
使用main函数来调用这个具有继承属性的结构体：
```
func main(){

	man := people.Man{}
	man.Age = 1
	man.LastName = "世林"
	man.Name = "张世林"

	fmt.Println(man)

	woman:= people.Woman{}
	woman.FirstName = "张"
	woman.Name = "张海霞"
	woman.Age = 2

	fmt.Println(woman)
}

```

#### 3）继承的特点：
- 结构体使用嵌套匿名结构体的所有字段与方法（无论是属性与方法，无论是大写还是小写开头的，都会有对应的继承的效果）。

- 父结构体的方法、属性，子结构体都具有对应的属性以及方法。
- 当操作子结构体的时候，编译器会先去子结构体中寻找对应的属性与方法，如果子类没有就查询其父类，如果父类也不存在，则报错（编译器是就近原则来对其属性进行访问，子类->父类->报错）。
- 当子结构体的多个父类存在相同的字段的时候，对其调用的话，必须指定父结构体的名字来对其属性进行调用。
例如：下创建两个结构体，每个结构体都有name属性，让BigStudent这个结构体嵌套两个结构体；
```

type bigStudent struct {
	student
	person
	zy string
}

func (bigs bigStudent) NewBigStudent() *bigStudent {
	bigs.person.name = "zsl"
	bigs.student.name = "zsl"
	bigs.zy = "计算机科学与技术"
	return &bigs
}
```
- 如果struct潜逃了一个**有名结构体**，则使用时必须使用结构体的名字。例如：
- 嵌套匿名结构体在创建变量的时候，可以直接对各个匿名结构体的字段进行赋值。


#### 4）多重继承
##### 基本介绍:
如果一个struct嵌套了多个匿名结构体，那么该结构体可以直接访问嵌套的匿名结构体的字段和方法，从而实现多重继承。

##### 代码演示：
```
type bigStudent struct {
	student
	person
	zy string
}

func (bigs bigStudent) NewBigStudent() *bigStudent {
	bigs.person.name = "zsl"
	bigs.zy = "计算机科学与技术"
	return &bigs
}

```

##### 使用细节：
- 多重继承，在匿名结构体的方式下，如果有相同的字段名或者方法名，必须要指定对应的结构体，否则编译器会报错。
- 通常情况下，为了保证代码的简洁性，尽量少用多重继承。

### 8、面向对象---接口
#### 1）基本介绍：
interface类型可以定义一组方法，但是这些方法不需要实现。并且interface中不可以包含任何变量。待到某个自定义类型（比如结构体）需要使用的时候，可以根据对应的情况将其具体实现。

在Golang中，多态的特性主要是使用接口来实现的。

#### 2）接口的demo
```
/**
定义一个接口
 */
type Usb interface {
	//声明两个未实现的方法
	Start()
	Stop()
}

//定义一个Phone结构体
type Phone struct {

}
//让Phone实现Usb接口的方法
func (p Phone) Start() {
	fmt.Println("手机开始工作")
}
func (p Phone) Stop(){
	fmt.Println("手机结束工作")
}

//定义一个Camera结构体
type Camera struct {

}
//让Camera实现Usb接口的方法
func (p Camera) Start() {
	fmt.Println("Camera开始工作")
}
func (p Camera) Stop(){
	fmt.Println("Camera结束工作")
}

//定义了一个Computer
type Computer struct {

}
func (c Computer) Working(usb Usb) {
	usb.Start()
	usb.Stop()
}

func main() {
	computer := Computer{}
	camera := Camera{}
	computer.Working(camera)
}
```

#### 3）接口的基本语法：
```
type 接口名 interface {
    方法名1(参数列表) (返回值列表)
    方法名2(参数列表) (返回值列表)
    ...
}
```

#### 4）接口的特点：
- 接口中的所有方法都没有方法体，即接口中的方法只有方法头，没有方法体。
- 接口主要是在程序中体现了高内聚，低耦合的特点。

- **Golang中的接口，不需要显式的调用，只要一个变量（比如结构体），含有了接口类型中的所有方法，那么这个变量就是实现了这个接口。所以，Golang中没有implements关键字。**

#### 5）接口的注意事项
- **接口本身不能够创建实例，但是可以指向一个实现了该接口的自定义类型的变量（实例）。** 也就是说，接口不能够直接创建变量，但是可以指向含有该接口的结构体的变量。

- 接口中所有的方法都没有方法体,即都是没有实现的方法。
- 在 Golang 中，一个自定义类型需要将某个接口的所有方法都实现，我们说这个自定义类型实现了该接口。
- 一个自定义类型只有实现了某个接口，才能将该自定义类型的实例(变量)赋给接口类型。例如：
```
//先定义一个实现了 A接口 的数据类型的变量
var stud Student

//现在才能够定义一个接口的变量来指向接口的实现类
var a A = stud
```

- 只要是自定义数据类型，就可以实现接口，不仅仅是结构体类型。
- **一个自定义数据类型能够实现多个接口。**
- 接口中一定不能够有变量。
- **一个接口可以多继承多个接口，那么实现这个接口的话，必须要实现这个接口以及父类接口定义的所有方法。**
```
type A interface {
    say()
}

type B interface {
    hello()
}
//接口来继承多个接口
type C interface {
    A
    B
}
```

- interface默认是一个指针类型（引用类型），那么如果没有对interface进行初始化就使用的话，会输出 nil。

- 空接口 interface{} 没有任何方法，所以所有数据类型都是实现了空接口，即我们可以将任何一个变量赋给空接口。例如：
```
var student Student
//将任何变量都能够给赋值给一个空接口
var stu interface{} = stu
```

- **接口多继承的多个接口，不允许有相同的方法名。**

#### 6）实现接口来自定义数据类型的排序
```
import (
	"fmt"
	"math/rand"
)

type Hero struct {
	Name string
	Age int
}

//创建一个Hero类型的切片数据
type HeroSlice []Hero

func (hs HeroSlice) Len() int {
	return len(hs)
}

// Less方法报告索引i的元素是否比索引j的元素小
func (hs HeroSlice) Less(i, j int) bool {
	//按照年龄排序
	return hs[i].Age < hs[j].Age
}

// Swap方法交换索引i和j的两个元素
func (hs HeroSlice) Swap(i, j int){
	//进行元素的交换操作
	//temp := hs[i]
	//hs[i] = hs[j]
	//hs[j] = temp
	//上面三句代码相当于下面这一行代码，主要是说明的是将 hs[i]与hs[j]相交换。
	hs[i],hs[j] = hs[i],hs[i]
}

func main() {

	//创建一个Hero切片
	var heros HeroSlice

	//为HeroSlice赋值
	for i:=0;i<3 ;i++  {
		hero := Hero{
			//使用Sprintf()方法返回一个值
			Name: fmt.Sprintf("英雄|%d", rand.Intn(100)),
			Age : rand.Intn(100),
		}
		heros = append(heros,hero)
	}

	//使用for_range来进行输出,将按照上面自定义的规则进行排序
	fmt.Println("---------------------")
	for _,v:=range heros {
		fmt.Println(v)
	}
	fmt.Println("---------------------")
}
```

#### 7）接口与继承的关系
- 当 A 结构体继承了 B 结构体，那么 A 结构就自动的继承了 B 结构体的字段和方法，并且可以直接使用
- 当 A 结构体需要扩展功能，同时不希望去破坏继承关系，则可以去实现某个接口即可，因此我们可以认为：实现接口是对继承机制的补充.
- 实现接口可以看作是对 继承的一种补充。

- 继承的价值主要在于：解决代码的复用性和可维护性。
- 接口的价值主要在于：设计，设计好各种规范(方法)，让其它自定义类型去实现这些方法。

- 接口在一定程度上实现代码解耦。

### 9、面向对象---多态
#### 1）基本介绍：
变量（实例）具有多种形态，面向对象的第三大特征。Go语言中的多态主要是使用接口来实现的。**可以按照统一的借口来通过调用不同的接口的实现来实现不同的功能**。

#### 2）多态参数：
##### 基本介绍：
将接口作为形参，然后传入不同的这个接口的实现类来指明需要执行哪个实现类的功能。

##### 案例演示：
Usb结构体作为形参的时候，传入不同的实现了这个接口的对象，就会具体执行不同的功能。
```
/**
定义一个接口
 */
type Usb interface {
	//声明两个未实现的方法
	Start()
	Stop()
}

//定义一个Phone结构体
type Phone struct {

}
//让Phone实现Usb接口的方法
func (p Phone) Start() {
	fmt.Println("手机开始工作")
}
func (p Phone) Stop(){
	fmt.Println("手机结束工作")
}

//定义一个Camera结构体
type Camera struct {

}
//让Camera实现Usb接口的方法
func (p Camera) Start() {
	fmt.Println("Camera开始工作")
}
func (p Camera) Stop(){
	fmt.Println("Camera结束工作")
}

//定义了一个Computer
type Computer struct {

}
func (c Computer) Working(usb Usb) {
	usb.Start()
	usb.Stop()
}

func main() {
	computer := Computer{}
	camera := Camera{}
	computer.Working(camera)
}

```

#### 3）多态数组
##### 基本介绍：
多态数组就是定义一个接口数组，然后多个这个接口的实现类，分别赋值给这个接口数组。

##### 案例演示：
```
/**
定义一个接口
 */
type Usb interface {
	//声明两个未实现的方法
	Start()
	Stop()
}

//定义一个Phone结构体
type Phone struct {

}
//让Phone实现Usb接口的方法
func (p Phone) Start() {
	fmt.Println("手机开始工作")
}
func (p Phone) Stop(){
	fmt.Println("手机结束工作")
}

//定义一个Camera结构体
type Camera struct {

}
//让Camera实现Usb接口的方法
func (p Camera) Start() {
	fmt.Println("Camera开始工作")
}
func (p Camera) Stop(){
	fmt.Println("Camera结束工作")
}

func main() {
	var usb [2]Usb
	usb[0] = Phone{}
	usb[1] = Camera{}
}

```

### 10、类型断言
#### 1）基本介绍：
类型断言，由于接口是一般类型，不知道其具体类型，如果需要转换成具体类型，就回去使用到类型断言。

#### 2）基本语法：
```
//类型断言的检测机制语法
if 名称,断言返回值 := 被检测的数据.(数据类型); 断言返回值 {
    
}

//断言使用方式与一：
//类型断言进行检测机制，如果类型判断正确，那么久执行下面的代码，否则不执行代码
if y,value := x.(float64); value {
	fmt.Printf("y的类型为：%T ,值为 %v",y,y)
}

//断言检测使用方式二：
y,value := x.(float64)
if value == true {
	fmt.Printf("y的类型为：%T ,值为 %v",y,y)
} else {
    fmt.Println("类型不正确")
}
```

#### 3）断言说明：
在进行类型断言的时候，如果类型不匹配。那么就会报错 panic，因此进行类型断言的时候，要确保原来的空接口指向的就是断言的类型。

#### 4）断言检测：
在断言中，使用下面的方式进行数据类型的判断，如果数据类型与断言的数据类型一致的话，就会执行标签中的代码，如果数据类型判断不一致的话，就不会执行标签中的方法，但是程序也不会报错。
```
func main() {
	var x interface{}
	var k float64
	k = 4

	//将 k 的值赋值给空接口 x
	x = k
	
	//断言使用方式与一：
	//类型断言进行检测机制，如果类型判断正确，那么久执行下面的代码，否则不执行代码
	if y,value := x.(float64); value {
		fmt.Printf("y的类型为：%T ,值为 %v",y,y)
	}
	
	//断言检测使用方式二：
	y,value := x.(float64)
	if value == true {
		fmt.Printf("y的类型为：%T ,值为 %v",y,y)
	} else {
	    fmt.Println("类型不正确")
	}
}
```

#### 5）断言最佳实践：
##### 最佳实践一：
根据传入的参数的类型来进行断言的验证，确定是否执行代码
```
/**
定义一个接口
 */
type Usb interface {
	//声明两个未实现的方法
	Start()
	Stop()
}

//定义一个Phone结构体
type Phone struct {

}
//让Phone实现Usb接口的方法
func (p Phone) Start() {
	fmt.Println("手机开始工作")
}
func (p Phone) Stop(){
	fmt.Println("手机结束工作")
}
func (p Phone) Call(){
	fmt.Println("手机可以打电话")
}

//定义一个Camera结构体
type Camera struct {

}
//让Camera实现Usb接口的方法
func (p Camera) Start() {
	fmt.Println("Camera开始工作")
}
func (p Camera) Stop(){
	fmt.Println("Camera结束工作")
}

type Computer struct {

}
func (c Computer) Working(usb Usb){
	usb.Start()
	usb.Stop()
	
	//根据传入的参数的不同，来确定是否执行标签中的代码
	if t,value := usb.(Phone); value {
	    t.Call()
	}
}

func main(){
	var usb Usb
	var phone Phone
	usb = phone
	var computer Computer
	computer.Working(usb)
}
```

##### 最佳实践二：
```
//先定义一个检测可变参数类型数据的函数
//返回传入的参数的类型
func TypeReturn(items ...interface{}) {
	for i,value := range items {
		switch value.(type) {
		case bool:
			fmt.Println("输出的参数为bool类型，值为",value,"下标为：",i)
		case int:
			fmt.Println("输出的参数为int类型，值为",value,"下标为：",i)
		case float64:
			fmt.Println("输出的参数为float64类型，值为",value,"下标为：",i)
		case string:
			fmt.Println("输出的参数为string类型，值为",value,"下标为：",i)
		}
	}
}

//传入参数，进行判断
func main(){
	var n1 float64 = 1.1
	var n2 string = "adf"
	main3.TypeReturn(n1,n2)
}
```

## 七、Golang文件操作
### 1、文件的基本介绍
#### 1）文件的概念
文件，我们可以理解为数据源（保存数据的地方）的一种，比如大家经常使用的Word文档、txt文件、excel文件等。文件最主要的作用就是保存数据，它即可以保存一张图片，也可以保存视频、声音等。

#### 2）流与输入流与输出流
- 流：数据在数据源（文件）和程序（内存）之间经历的路径。
- 输入流：数据从数据源（文件）到程序（内存）中。
- 输出流：数据从程序（内存）到数据源（文件）中。

```
sequenceDiagram
Go内存->>外部文件: 输出流
外部文件->>Go内存: 输入流
```

#### 3）操作文件的方法一：
os.File 包中封装了Golang语言对文件的相关操作，File是一个结构体。

#### 4）普通的打开文件
##### 基本介绍：
Open打开一个文件用于读取。如果操作成功，返回的文件对象的方法可用于读取数据；对应的文件描述符具有O_RDONLY模式。如果出错，错误底层类型是*PathError。

##### 注意事项：
- 返回的 file，可以称为  file对象/file指针/file文件句柄,但会的实际就是一个指针。
- 判断err是否为空来确定是否打开文件成功。

##### 方法原型：
```
func Open(name string) (file *File, err error)

```

#### 5）关闭文件
##### 基本介绍：
Close关闭文件file，使文件不能用于读写。它返回可能出现的错误。

##### 方法原型：
```
func (f *File) Close() error
```

##### 使用方法：
```
//方法一：直接使用defer来进行文件流的关闭
defer file.Close();

//方法二：在不需要文件流的位置进行文件流的关闭
//比如：在方法的末尾关闭文件流
err := file.Close()
if err == nil {
	fmt.Println("文件关闭成功")
} else {
	fmt.Println("文件关闭失败",err)
}

```

#### 6）带缓冲的Reader读取文件：
##### 使用步骤：
第一步：先使用 Open()函数打开文件流  
第二步：使用 defer 方式来关闭文件流  
第三步：创建一个 reader，调用的是 bufio的NewReader()方法  
第四步：判断返回的err信息，来查看是否读到了文件的末尾，来循环输出内容。  

##### 代码实现：
```
func main() {
	//打开文件，调用的是os包下面的Open函数
	file, err := os.Open("E:/test.txt")

	//如果没有错误信息的话，就可以继续操作 file对象/file指针/file文件句柄
	if err == nil {
		fmt.Println(file)
	} else {
		fmt.Println("打开文件失败")
	}

	//关闭这个文件流
	defer file.Close()

	//创建一个带缓冲区的 Reader 来读取文件
	reader := bufio.NewReader(file)

	for {
		//调用reader的ReadString函数来实现当遇到换行符的话，一行就结束
		str, err1 := reader.ReadString('\n')
		//当 err1 == io.EOF 的时候，说明文件读取到了末尾
		if err1 ==io.EOF {
			break
		}
		fmt.Println(str)
	}
}
```

#### 7）直接将一个文件读入到内存中---小文件
##### 基本介绍：
ReadFile函数 从filename指定的文件中读取数据并返回文件的内容。成功的调用返回的err为nil而非EOF。因为本函数定义为读取整个文件，它不会将读取返回的EOF视为应报告的错误。

##### 方法原型：
```
func ReadFile(filename string) ([]byte, error)
```

##### 代码实现：
```
//使用的是 ioutil 包下面的 ReadFile 函数，实现小文件的直接读取功能
func main() {
	filePath := "E:/test.txt"
	file, err := ioutil.ReadFile(filePath)

	if err == nil{
		fmt.Println(string(file))
	} else {
		fmt.Println("错误，",err)
	}
}
```

##### 注意事项：
- ReadFile()函数不需要显式的Open()文件和Close()文件。

- 一次性的将整个文件读取到内存中，所以文件不能过大，多大内存空间不够。

#### 8）直接将一个文件写入到文件中---小文件
##### 基本介绍：
函数向filename指定的文件中写入数据。如果文件不存在将按给出的权限创建文件，否则在写入数据之前清空文件。

##### 代码原型：
```
func WriteFile(filename string, data []byte, perm os.FileMode) error
```

##### 代码实现：
```
err2 := ioutil.WriteFile(file2, "sadfasff", 0666)
if err2 != nil {
	fmt.Println("写入到文件失败",err2)
}
```

#### 9）带缓冲区的Writer写入文件
##### 使用步骤：
第一步：打开一个可以写入的文件流    
第二步：使用 defer 方式来关闭文件流    
第三步：创建一个 writer，调用的是 bufio的NewWriter()方法。   
第四步：调用writer的WriteString()方法写入数据到缓冲区。    
第五步：调用writer的Flush()方法来实现数据从缓冲区中刷入的文件中。  

##### 代码实现：
```
//创建一个带缓存的Writer
writer := bufio.NewWriter(file)

//向writer中写入数据
for i:=0;i<5;i++ {
	writer.WriteString("hello World \n")

}

//调用Flush()方法将缓存中数据刷入到文件中
writer.Flush()
```


#### 10）将数据写入文件
##### 基本介绍：
使用OpenFile()函数打开文件，能够进行打开的方式的控制，能够进行文件权限的控制（文件权限的控制在Linux操作系统下才能够实现，Windows操作系统下无效）。

OpenFile是一个更一般性的文件打开函数，大多数调用者都应用Open或Create代替本函数。它会使用指定的选项（如O_RDONLY等）、指定的模式（如0666等）打开指定名称的文件。如果操作成功，返回的文件对象可用于I/O。如果出错，错误底层类型是*PathError。

##### 代码原型：
```
//name string，表示的是文件名
//flag int，表示的是打开的模式，多种模式可以组合在一起
//perm FileMode，表示打开的文件的权限操作。
func OpenFile(name string, flag int, perm FileMode) (file *File, err error)

```

##### 方法参数的选择：
**flag int参数的选择：**  
参数名 | 参数对应文件打开的方式
---|---
O_RDONLY int = syscall.O_RDONLY | 只读模式打开文件
O_WRONLY int = syscall.O_WRONLY | 只写模式打开文件(覆盖)
O_RDWR   int = syscall.O_RDWR   | 读写模式打开文件(覆盖)
O_APPEND int = syscall.O_APPEND | 写操作时将数据附加到文件尾部(追加)
O_CREATE int = syscall.O_CREAT  | 如果不存在将创建一个新文件
O_EXCL   int = syscall.O_EXCL   | 和O_CREATE配合使用，文件必须不存在
O_SYNC   int = syscall.O_SYNC   | 打开文件用于同步I/O
O_TRUNC  int = syscall.O_TRUNC  | 如果可能，打开时清空文件


**perm FileMode参数的选择：**
参数 | 参数的解释
---|---
1 | 读r
2 | 写w
4 | 可执行x

##### 代码实现：
```
func main() {
	//定义一个文件名
	fileName := "E:/test1.txt"
	//第一个参数：指定文件的文件名
	//第二个参数：组合模式，打开文件的方式是 读写 或者 创建 两种模式
	//第三个参数：指定在Linux下这个文件的权限
 	file, err := os.OpenFile(fileName, os.O_RDWR | os.O_CREATE, 4)

 	if err != nil {
 		fmt.Println("open文件失败",err)
	}

 	//关闭这个流
 	defer file.Close()

 	//创建一个带缓存的Writer
	writer := bufio.NewWriter(file)

	//向writer中写入数据
	for i:=0;i<5;i++ {
		writer.WriteString("hello World \n")

	}

	//调用Flush()方法将缓存中数据刷入到文件中
	writer.Flush()
}

```

#### 11）将一个文件的数据转入到另外一个文件中
```
func main() {

	file1 := "E:/test.txt"
	file2 := "E:/test1.txt"

	//得到这个文件的内容
	file1_value, err1 := ioutil.ReadFile(file1)
	if err1 != nil {
		fmt.Println("文件读取失败",err1)
		return
	}

	err2 := ioutil.WriteFile(file2, file1_value, 0666)
	if err2 != nil {
		fmt.Println("写入到文件失败",err2)
	}
}
```

#### 12）判断文件/文件夹是否存在

##### 基本介绍：
Golang中判断文件或者文件夹是否存在的方法是 os.Stat()函数。

##### 详细介绍：

- 如果返回的错误为 nil，则说明文件或者文件夹存在。
- 如果返回的错误类型使用os.IsNotExists()函数来进行判断为true，则说明文件夹不存在。

- 如果返回其他类型，则不确定文件是否存在。

##### 代码实现：
```
func main() {

	file1 := "E:/test1.txt"

	b := PathExists(file1)
	fmt.Println(b)
}

//返回文件是否存在
func PathExists(path string) (bool){
	_, err := os.Stat(path)
	if err == nil {
		return true
	}
	if os.IsNotExist(err) {
		return false
	}
	return false
}
```

#### 13）文件的拷贝
##### 拷贝图片。
**使用方法：**  
使用的是 io包下面的 Copy() 方法来实现文件的拷贝操作。

**代码实现：**  
```
//拷贝图片
func main() {

	file := "E:/照片/周小燕艺术照片/P001.jpg"
	file1 := "E:/P001.jpg"

	copyfile(file,file1)
}

/**
实现原理是得到一个reader对象与一个writer对象，然后执行copy操作
 */
//srcPath：源文件的路径
//DSTPath：目标文件的路径
func copyfile(srcPath string,dstPath string) (written int64, err error)  {
	//打开源文件路径
	srcFile, err1 := os.Open(srcPath)
	if err1 != nil {
		fmt.Println("打开源文件失败",err1)
		return
	}
	//拿到Reader对象
	reader := bufio.NewReader(srcFile)

	//创建或者打开一个文件
	dstFile, err2 := os.OpenFile(dstPath, os.O_CREATE|os.O_RDWR, 0666)

	//得到这个文件的Writer对象
	if err2 != nil {
		fmt.Println("创建或者打开目标文件失败",err2)
		return
	}
	writer := bufio.NewWriter(dstFile)

    defer srcFile.Close()
	defer dstFile.Close()

	//执行文件的Copy操作，并且返回
	return io.Copy(writer, reader)
}
```

#### 14）统计文件的各类字符数量
##### 案例需求：
统计文件中的英文、数字、空格以及其他字符的个数。

##### 代码实现：
```
//定义这个结构体，来实现文件中数据的统计
type Num struct {
	intSize int
	charSize int
	spaceSize int
	ortherSize int
}
func main(){
	var num Num
	var path string = "E:/test3.txt"
	num1 := tongji(path, num)
	fmt.Println(num1.intSize)
	fmt.Println(num1.charSize)
	fmt.Println(num1.spaceSize)
	fmt.Println(num1.ortherSize)
}

//统计文件中的各个字段的数据
func tongji(filepath string,num Num) *Num {

	file, err1 := os.Open(filepath)
	if err1 != nil {
		fmt.Println("文件打开失败",err1)
	}

	defer file.Close()

	//得到这个reader对象，让偶能够实现读取功能
	reader := bufio.NewReader(file)

	for {
		//一行一行的读取的意思
		str, err2 := reader.ReadString('\n')
		if err2 == io.EOF {
			fmt.Println("读取文件完成",err2)
			break
		}
		for _,v :=range str {
			switch {
				case v >= 'a' && v <= 'z': num.charSize++
				case v >= 'A' && v <= 'Z': num.charSize++
				case v >='0' && v <='9': num.intSize++
				case v >=' ' && v <='\t': num.intSize++
				default:
					num.ortherSize++
			}
		}
	}
	return &num
}
```

## 八、Golang命令行参数
### 1、os.Args解析命令行参数：
os.Args 是一个 string 的切片，用来存储所有的命令行参数。

#### 1）简单demo：
查看所有的命令行参数。
```
//查看命令行参数
func main(){
	fmt.Println("命令行的参数有:",len(os.Args),"个")

	//得到所有的命令行参数
	for i,v :=range os.Args {
		fmt.Printf("第%个参数，值为%v\n",i+1,v)
	}
}
```

### 2、flag包来解析命令行参数
#### 简单demo：
查看所有的命令行参数
```
//flag包查看命令行参数
func main(){
	var user string
	var pwd string
	var host string
	var port int

	flag.StringVar(&user,"u","root","用户名默认为root")
	flag.StringVar(&pwd,"pwd","123456","密码默认为123456")
	flag.StringVar(&host,"h","127.0.0.1","主机名默认为本机")
	flag.IntVar(&port,"p",3306,"端口号默认为3306")

	//必须将flag进行转换
	flag.Parse()

	fmt.Println(user,pwd,host,port)
}
```
## 九、Go语言Json
### 1、Json简介：
Json全称 JavaScript Object Notation，是一种轻量级的数据交换格式，有利于阅读与编写。

Json基于机器解析与生成，并有效地提升网络传输效率，通常程序在网络传输时先将数据（结构体、数组、切片、map等）序列华为Json字符串；待到接收方收到Json字符串时，在反序列化恢复成原来的数据类型。

### 2、Json格式：
**在JS中，一切皆为对象**，所以任何数据都可以通过Json类型数据来表示。

Json键值对是用来保存数据的一种方式，key/value对组合中的key写在前面并且使用双引号""包裹住，然后使用 : 分隔，最后连接着value。例如：
```
//最简单的json
{"name":"zsl","age":66,"address":"cq"}

//带数组的json
{"name":"zsl","age":66,"address":["cq","sh"]}

//多个数组对象
[
    {"name":"zsl","age":66,"address":"cq"}
    {"name":"thl","age":67,"address":"wuxi"}
]
```

### 3、Json序列化
#### 1）基本介绍：
Json序列化是指：将 key/value 结构的数据类型（比如结构体、map、切片等）序列化为Json字符串的操作。

#### 2）序列化使用的方法：
```
func Marshal(v interface{}) ([]byte, error)
```

#### 3）注意事项：
Json序列化必**须使用大写的属性（字段）与大写的结构体**，才能够被 json.Marsual()函数使用。

#### 4）代码演示:
```
//定义一个结构体
type Monster struct {
	Name string `json:"monster_name"` //反射机制
	Age int `json:"monster_age"`
	Birthday string //....
	Sal float64
	Skill string
}



func testStruct() {
	//演示
	monster := Monster{
		Name :"牛魔王",
		Age : 500 ,
		Birthday : "2011-11-11",
		Sal : 8000.0,
		Skill : "牛魔拳",
	}

	//将monster 序列化
	data, err := json.Marshal(&monster) //..
	if err != nil {
		fmt.Printf("序列号错误 err=%v\n", err)
	}
	//输出序列化后的结果
	fmt.Printf("monster序列化后=%v\n", string(data))

}

//将map进行序列化
func testMap() {
	//定义一个map
	var a map[string]interface{}
	//使用map,需要make
	a = make(map[string]interface{})
	a["name"] = "红孩儿"
	a["age"] = 30
	a["address"] = "洪崖洞"

	//将a这个map进行序列化
	//将monster 序列化
	data, err := json.Marshal(a)
	if err != nil {
		fmt.Printf("序列化错误 err=%v\n", err)
	}
	//输出序列化后的结果
	fmt.Printf("a map 序列化后=%v\n", string(data))

}

//演示对切片进行序列化, 我们这个切片 []map[string]interface{}
func testSlice() {
	var slice []map[string]interface{}
	var m1 map[string]interface{}
	//使用map前，需要先make
	m1 = make(map[string]interface{})
	m1["name"] = "jack"
	m1["age"] = "7"
	m1["address"] = "北京"
	slice = append(slice, m1)

	var m2 map[string]interface{}
	//使用map前，需要先make
	m2 = make(map[string]interface{})
	m2["name"] = "tom"
	m2["age"] = "20"
	m2["address"] = [2]string{"墨西哥","夏威夷"}
	slice = append(slice, m2)

	//将切片进行序列化操作
	data, err := json.Marshal(slice)
	if err != nil {
		fmt.Printf("序列化错误 err=%v\n", err)
	}
	//输出序列化后的结果
	fmt.Printf("slice 序列化后=%v\n", string(data))

}

//对基本数据类型序列化，对基本数据类型进行序列化意义不大
func testFloat64() {
	var num1 float64 = 2345.67

	//对num1进行序列化
	data, err := json.Marshal(num1)
	if err != nil {
		fmt.Printf("序列化错误 err=%v\n", err)
	}
	//输出序列化后的结果
	fmt.Printf("num1 序列化后=%v\n", string(data))
}

func main() {
	//演示将结构体, map , 切片进行序列号
	testStruct()
	testMap()
	testSlice()//演示对切片的序列化
	testFloat64()//演示对基本数据类型的序列化
}
```

### 4、Json使用tag标签
#### 1）基本介绍:
在Json序列化的过程中，如果需要将属性（字段）取别名或者转换为小写字母开头，那么就需要使用tag标签。

#### 2）基本写法：

```
type Person struct {
    Name string `json:"name"`
}
```


#### 3）代码演示：
```
//定义一个结构体，使用tag标签取别名
type Person struct {
	Name string `json:"name"`
	Age int `json:"age"`
	Birthday string `json:"birthday"`
}

//演示将一个结构体转换为一个字符串
func ser(p Person) (string,error) {
	bytes, err := json.Marshal(p)
	return string(bytes),err

}

func main() {
	per := Person{"zsl",23,"2344"}
	str, err := ser(per)
	if err != nil {
		fmt.Println("序列化失败,",err)
	}
	fmt.Println(str)
}

//输出结果为：
{"name":"zsl","age":23,"birthday":"2344"}
```

### 5、Json反序列化
#### 1）基本介绍：
Json反序列化是指：将Json字符串反序列化成对应的数据类型（比如：及饿哦固体、map、切片等）的操作。

#### 2）反序列化使用的方法：
```
func Unmarshal(data []byte, v interface{}) error
```

#### 3）代码实现：
```
//定义一个结构体
type Monster struct {
	Name string  
	Age int 
	Birthday string //....
	Sal float64
	Skill string
}


//演示将json字符串，反序列化成struct
func unmarshalStruct() {
	//说明str 在项目开发中，是通过网络传输获取到.. 或者是读取文件获取到
	str := "{\"Name\":\"牛魔王~~~\",\"Age\":500,\"Birthday\":\"2011-11-11\",\"Sal\":8000,\"Skill\":\"牛魔拳\"}"

	//定义一个Monster实例
	var monster Monster

	err := json.Unmarshal([]byte(str), &monster)
	if err != nil {
		fmt.Printf("unmarshal err=%v\n", err)
	}
	fmt.Printf("反序列化后 monster=%v monster.Name=%v \n", monster, monster.Name)

}
//将map进行序列化
func testMap() string {
	//定义一个map
	var a map[string]interface{}
	//使用map,需要make
	a = make(map[string]interface{})
	a["name"] = "红孩儿~~~~~~"
	a["age"] = 30
	a["address"] = "洪崖洞"

	//将a这个map进行序列化
	//将monster 序列化
	data, err := json.Marshal(a)
	if err != nil {
		fmt.Printf("序列化错误 err=%v\n", err)
	}
	//输出序列化后的结果
	//fmt.Printf("a map 序列化后=%v\n", string(data))
	return string(data)

}

//演示将json字符串，反序列化成map
func unmarshalMap() {
	//str := "{\"address\":\"洪崖洞\",\"age\":30,\"name\":\"红孩儿\"}"
	str := testMap()
	//定义一个map
	var a map[string]interface{} 

	//反序列化
	//注意：反序列化map,不需要make,因为make操作被封装到 Unmarshal函数
	err := json.Unmarshal([]byte(str), &a)
	if err != nil {
		fmt.Printf("unmarshal err=%v\n", err)
	}
	fmt.Printf("反序列化后 a=%v\n", a)

}

//演示将json字符串，反序列化成切片
func unmarshalSlice() {
	str := "[{\"address\":\"北京\",\"age\":\"7\",\"name\":\"jack\"}," + 
		"{\"address\":[\"墨西哥\",\"夏威夷\"],\"age\":\"20\",\"name\":\"tom\"}]"
	
	//定义一个slice
	var slice []map[string]interface{}
	//反序列化，不需要make,因为make操作被封装到 Unmarshal函数
	err := json.Unmarshal([]byte(str), &slice)
	if err != nil {
		fmt.Printf("unmarshal err=%v\n", err)
	}
	fmt.Printf("反序列化后 slice=%v\n", slice)
}

func main() {

	unmarshalStruct()
	unmarshalMap()
	unmarshalSlice()
}
```

#### 4）反序列化注意事项：
- 反序列化的 map或者切片不需要进行make操作，因为在UnMarsual()方法能够自动make操作。


## 十、Golang单元测试
### 1、基本介绍
Go 语言中自带有一个轻量级的测试框架 testing 和自带的 go test 命令来实现单元测试和性能测试，testing 框架和其他语言中的测试框架类似，可以基于这个框架写针对相应函数的测试用例，也可以基于该框架写相应的压力测试用例。

### 2、实现功能：
- 确保每个函数都是可运行的，并且结果运行都是正确的。
- 确保写出来的代码性能优秀。
- 单元测试能够及时发现程序设计与实现的逻辑错误，使错误及时暴露，便于问题的定位与解决。
- 性能测试着重点在于发自按程序设计上面的一些问题，能够让程序在高并发的情况下还能够保持稳定运行。

### 3、快速入门：
在utils包下面创建一个 util1.go 文件，里面放置一个函数：
```
func Add(a int,b int) int {
	return a+b
}
```

在utils包下面创建一个命名为 test_util1.go 的文件，专门用于测试 util1.go 文件的函数。
```
func TestAdd(t *testing.T){
	add := Add(1, 2)
	if add != 3 {
		t.Fatalf("函数执行错误，立即自动退出")
	} else {
		fmt.Println("函数执行成功")
	}
}
```

### 4、注意事项：
- 测试用例的文件名字必须以 **XXX_test.go** 方式命名。一般这个 XXX 都是使用被测试的那个文件的文件名来代替。例如：需要测试 util.go 文件，那么可以在同一个包中创建一个 util_test.go 文件来实现对util.go文件的测试。

- 测试用例函数必须以 **TestXXX** 方式来命名，一般是使用 Test+被测试的函数名。比如：TestAdd。
- TestAdd(t *testing.T) 的形参必须要是 **t *testing.T**。
- 一个测试用例文件可以同时测试多个用例函数。
- cmd运行命令：go test -v（程序测试用例运行，且打印日志）。
- 当出现错误的时候，可以使用 t.Fatalf(str string) 来输出错误信息，并停止程序运行。
- t.Logf(str string) 可以输出相应的日志。
- 测试用例中，PASS表示测试正确，FAIL表示测试失败。


## 十一、goroutine（协程）

### 1、进程与线程
- 进程就是程序在操作系统中的一次执行过程，是系统进行资源分配和调度的基本单位。
- 线程是进程的一个执行实例，是程序执行的最小单元，它是一个比进程更小的能够独立运行的基本单位。

- 一个进程能够创建多个线程，同一个进程中的多个线程可以并发的执行任务。
- 一个程序至少一个进程，一个进程至少有一个线程。

###  2、并行与并发
#### 1）并行：
多线程程序在多核上同时运行，这就是并行。

并行是在多个CPU上面有多分线程运行，每个CPU在相同的时间上都是能够执行一个线程，所以多个线程能够同时执行。

#### 2）并发：
多线程程序在单核上同时运行，这就是并发。

并发在同一个CPU上面有多个线程运行，其实是多个线程抢占CPU资源，然后进行线程任务的执行。

### 3、Go协程与Go主线程
#### 1）基本介绍：
在一个Golang主线程上面，可以起多个协程，协程是一个轻量级的线程。也就是说，Golang主线程能够开启多个协程。

#### 2）Go协程的特点：
- 有独立的栈空间。
- 共享程序堆空间。
- 调度由用户控制。
- 协程使用一个轻量级的线程。

示意图：
```
graph LR
A[GO主线程] -->B(协程1)
A[Go主线程] -->C(协程2)
A[Go主线程] -->D(协程3)
A[Go主线程] -->E(协程...)
```

### 4、goroutine（协程）快速入门
#### 1）开启协程方式：
开启协程，就是说在Go主程序上开启一个协程，与主线程一起同时执行，相当于java中的多线程执行方式。

#### 2）使用方式：
go 方法名 --- 这样就能够开启协程。

#### 3）代码演示：
```
func syso() {
	for i:=0;i<10;i++ {
		fmt.Println("hello goroutine!!",i)
		//睡眠一秒
		time.Sleep(time.Second)
	}
}

func main(){
	//go 方法名  --- 这样子就是开启了一个协程
	go syso()

	for i :=0;i<10;i++ {
		fmt.Println("hello Goland",i)
		time.Sleep(time.Second)
	}
}
```
#### 4）注意事项：
- 如果主线程执行完毕退出了以后，则协程即是未完成，也会退出。
- 如果协程可以在主线程未完成之前就完成了，那么也不会对主线程有影响。
- 主线程是一个物理线程，直接作用于CPU上面，是重量级的，非常耗费资源。
- 协程是从主线程开启的，是轻量级的线程，消耗资源非常少。

- Golang的协程机制是重要的特点，可以轻松突破上万个协程，并发能力强悍。


### 5、Golang设置运行的CPU数目
调用函数查看机器的CPU数目，设置使用的CPU数目.
```
func main(){
	//查看机器的CPU数目
	cpu := runtime.NumCPU()
	fmt.Println("cpu数目为：",cpu)

	//自定义需要使用的CPU个数
	runtime.GOMAXPROCS(6)
}
```

## 十二、channel（管道）
### 1、channel使用原因
在程序的运行过程中，开启了多个协程goroutine同时操作同一个数据，那么可能会导致数据冲突的问题出现。所以需要使用 channel管道 来解决这个问题。也就是说，为了让程序之间能够相互之间进行通信的操作，避免发生资源的抢占。

### 2、channel基本介绍
- channel本质是一个数据结构---队列。

- 数据是先进先出的操作方式。
- 线程安全，多goroutine访问的时候，不需要加锁，也能够保证线程安全。
- channel是有类型的，一个string的channel只能存放string类型的数据。

### 3、channel声明定义
#### 1）声明方式：
```
var 变量名 chan 数据类型
```

#### 2）举例说明：
```
var intChan chan int  （定义了一个存放int类型的channel）

var stringChan chan string  （定义了一个存放string类型的channel）

var personChan chan Person  （定义了一个存放结构体Person类型的channel）

var personChan1 chan *Person  （定义了一个存放结构体Person指针类型的channel）
```
#### 3）注意事项：
- **channel是引用类型**。

- **channel必须初始化才能够写入数据，即 make 以后才能够正常使用**。

- 管道是有数据类型的规定的，只能存放对应的数据类型的数据。
- channel数据放满以后，就不能够继续存放数据。
- 在没有协程的情况下，如果管道中没有数据以后，继续取数据，会报出 dead lock。



### 4、channel的初始化与使用

#### 1）channel的初始化：
初始化的时候，可以定义channel可以存放的数据量的大小。

```
//定义一个channel
var intChan chan int

//初始化channel,意思是 创建了一个可以存放3个int数据类型的管道
intChan = make(chan int,3)
```

#### 2）channel写入、读取数据
```
func main() {

	//定义一个channel
	var intChan chan int

	//初始化channel,意思是 创建了一个可以存放3个int数据类型的管道
	intChan = make(chan int, 3)

	//查看 intChan 是什么
	fmt.Printf("intChan的值为 %v,本身的地址为 %p\n", intChan, &intChan)

	//向管道中存入数据
	intChan <- 10
	intChan <- 20


	//查看管道定义的长度：
	fmt.Println("管道的长度为：",cap(intChan))
	//查看管道存入的数据量：
	fmt.Println("管道写入的数据有：",len(intChan))

	//从管道中读取数据
	num0 := <-intChan
	fmt.Println("取出的第一个数据为：",num0)
}
```

### 5、channel更多案例：
#### 1）创建map[string]string的channel
```
func main() {

	//定义一个channel
	var mapChan chan map[string]string
	mapChan = make(chan map[string]string,10)

	map1 := make(map[string]string,2)
	map1["name"]="zsl"
	map1["age"]="23"
	map1["birth"]="1996-11-22"
	mapChan <- map1

	//取出数据
	map2 := <- mapChan
	fmt.Println(map2)
}
```


#### 2）创建一个结构体的channel
```
type person struct {
	name string
	age int
}

func main() {
	var per chan person
	per =make(chan person,3)

	cat1 := person{
		name:"zsl",
		age:23,
	}

	per <- cat1

	cat2 := <- per
	fmt.Println(cat2)
}
```

### 6、channel的遍历与关闭
#### 1）channel的关闭
调用 close()函数来关闭管道
```
func main() {

	var intChan chan int
	intChan = make(chan int,10)
	
	intChan <- 100
	intChan <- 200
	
	//关闭管道，这个时候不能继续写入数据到管道中，但是能够继续遍历管道中的数据
	close(intChan)
}
```

#### 2）channel的遍历
##### 遍历channel的方式：
使用的是 for-range 的方式进行遍历，模式为：
```
for value :=range 遍历的channel {
    fmr.Println(value)
}
```

##### 代码演示：
```
func main() {

	var intChan chan int
	intChan = make(chan int,10)

	intChan <- 100
	intChan <- 200

	//关闭管道，这个时候不能继续写入数据到管道中，但是能够继续遍历管道中的数据
	close(intChan)

	//调用for-range来进行遍历的操作
	//在遍历的时候，如果管道没有关闭，则会出现 deadback错误。
	for v :=range intChan {
		fmt.Println(v)
	}
}
```

#### 3）channel关闭与遍历的关系：
- **遍历之前必须先要关闭管道，否则会报错 deadback错误。**

- 关闭管道以后，就不能够继续向channel中写入数据。
- channe对象的遍历是使用 for-range 来进行遍历。

### 7、goroutine与channel综合案例一
使用协程与管道进行数据的写入，保证了程序的协作性，提高程序效率，又保证了程序的线程按加权问题，也使用 exitChan 来确保协程执行完毕以后，主线程才停止执行。
```
//写数据的函数
func writeData(intChan chan int) {
	for i:=0;i<50;i++ {
		intChan <- i+1
		fmt.Println("写入的数据，",i+1)
	}
	close(intChan)
}
//读数据的函数
func readData(intChan chan int,exitChan chan bool) {

	for value :=range intChan {
		fmt.Println("读取的数据，",value)
	}

	//读的任务完成
	exitChan <- true
	close(exitChan)
}

func main() {
	//用于存储数据
	intChan := make(chan int, 50)
	//用于来判断是否读取intChan的数据结束，结束的话，才让程序结束
	exitChan := make(chan bool, 1)

	//开启协程
	go writeData(intChan)
	go readData(intChan, exitChan)

	//如果协程结束，才让程序结束
	for v :=range exitChan {
		if v == true {
			break
		}
	}
}
```

### 8、channel使用细节
#### 1）channel只读、只写模式
可以声明为只读，或者只写的模式。使用场景为：当我发送消息的话，可以定义一个只写模式的形参，接收消息的时候， 可以定义一个只读的形参。

```
func main(){

	//只读channel
	var intChan <-chan int
	
	//只写channel
	var stringChan chan<- string
}
```

#### 2）select处理管道取数据阻塞
当在程序运行过程中不知道什么时候关闭管道，那么久可以使用select来进行通道的数据的读取等操作。
```
func main(){

	//定义一个管道
	intChan := make(chan int,10)
	for i:=0;i<10;i++ {
		intChan <-i
	}

	//定义一个管道 ，五个数据的string、
	stringChan := make(chan string,5)
	for i:=0;i<5;i++ {
		stringChan <- "hello"+fmt.Sprintf("%d",i)
	}

	//在实际开发中，如果不知道该什么时候关闭管道的话，需要用select来解决
	for {
		select {
			case v := <-intChan :
				fmt.Printf("从intChannel中取得数据：%d\n",v)
			case v := <-stringChan:
				fmt.Printf("从stringChannel中取得数据：%s\n",v)
		default:
			time.Sleep(2000)
			fmt.Printf("都取不到数据了\n")
		}
	}
}
```

#### 3）recover在goroutine中的使用
在程序中的多个协程进行工作，如果一个协程出现错误，那么就会报错panic，如果我们不捕获这个panic的话，会造成整个程序的崩溃。如果在goroutine中使用recover来捕获panic的话，就可以进行处理异常问题，而程序不会出现错误，其他协程继续执行。

##### 代码实现：
```
//函数
func sayHello() {
for i := 0; i < 10; i++ { time.Sleep(time.Second) fmt.Println("hello,world")
}
}
//函数
func test() {
    //这里我们可以使用 defer + recover 
    defer func() {
        //捕获 test 抛出的 panic
        if err := recover(); err != nil {
            fmt.Println("test() 发生错误", err)
        }
    }()
    
    //定义了一个 map
    var myMap map[int]string 
    myMap[0] = "golang" //error
}


func main() {
    go sayHello() 
    go test()

    for i := 0; i < 10; i++ { 
        fmt.Println("main() ok=", i) 
        time.Sleep(time.Second)
    }
}

```

## 十三、Golang反射
### 1、基本介绍：
> reflect包实现了运行时反射，允许程序操作任意类型的对象。典型用法是用静态类型interface{}保存一个值，通过调用TypeOf获取其动态类型信息，该函数返回一个Type类型值。调用ValueOf函数返回一个Value类型值，该值代表运行时的数据。Zero接受一个Type类型参数并返回一个代表该类型零值的Value类型值。

- 反射可以在运行时动态的获取变量的各种信息，比如变量的类型（type）、类别（kind）等。
- 如果是结构体变量的话，还可以获取到结构体本身的信息（结构体的字段、方法等）。
- 通过反射，可以修改变量的值，可以调用与其相关联的方法。

- 使用反射，可以修改变量的值，可以调用关联的方法。
- 反射的使用，需要导入包 reflect。

![image](https://note.youdao.com/yws/api/personal/file/4CA89708D50B4071A0E8FB0C8B585F3D?method=download&shareKey=9a9205a1a8ce1b7042e5c260705f0a69)


### 2、反射的使用场景：
- 当不知道调用哪个函数，根据传入的参数在运行时确定调用的具体结构，这种需要对函数或者方法进行反射的操作。
- 当结构体需要使用 tag进行序列化的时候，可以使用反射生成相应的字符串。

### 3、反射的函数与概念：
#### 1）函数---reflect.TypeOf(变量名)
##### 基本介绍：
获取变量的类型，返回的是 reflect.Type 类型。

##### 方法原型：
```
func TypeOf(i interface{}) Type

TypeOf返回接口中保存的值的类型，TypeOf(nil)会返回nil。
```

##### reflect.Type 介绍：
reflect.Type是一个接口类型，而已通过这个Type调用其已经完成了的方法：
```
type Type interface {
    // Kind返回该接口的具体分类
    Kind() Kind
    // Name返回该类型在自身包内的类型名，如果是未命名类型会返回""
    Name() string
    // PkgPath返回类型的包路径，即明确指定包的import路径，如"encoding/base64"
    // 如果类型为内建类型(string, error)或未命名类型(*T, struct{}, []int)，会返回""
    PkgPath() string
    // 返回类型的字符串表示。该字符串可能会使用短包名（如用base64代替"encoding/base64"）
    // 也不保证每个类型的字符串表示不同。如果要比较两个类型是否相等，请直接用Type类型比较。
    String() string
    // 返回要保存一个该类型的值需要多少字节；类似unsafe.Sizeof
    Size() uintptr
    // 返回当从内存中申请一个该类型值时，会对齐的字节数
    Align() int
    // 返回当该类型作为结构体的字段时，会对齐的字节数
    FieldAlign() int
    // 如果该类型实现了u代表的接口，会返回真
    Implements(u Type) bool
    // 如果该类型的值可以直接赋值给u代表的类型，返回真
    AssignableTo(u Type) bool
    // 如该类型的值可以转换为u代表的类型，返回真
    ConvertibleTo(u Type) bool
    // 返回该类型的字位数。如果该类型的Kind不是Int、Uint、Float或Complex，会panic
    Bits() int
    // 返回array类型的长度，如非数组类型将panic
    Len() int
    // 返回该类型的元素类型，如果该类型的Kind不是Array、Chan、Map、Ptr或Slice，会panic
    Elem() Type
    // 返回map类型的键的类型。如非映射类型将panic
    Key() Type
    // 返回一个channel类型的方向，如非通道类型将会panic
    ChanDir() ChanDir
    // 返回struct类型的字段数（匿名字段算作一个字段），如非结构体类型将panic
    NumField() int
    // 返回struct类型的第i个字段的类型，如非结构体或者i不在[0, NumField())内将会panic
    Field(i int) StructField
    // 返回索引序列指定的嵌套字段的类型，
    // 等价于用索引中每个值链式调用本方法，如非结构体将会panic
    FieldByIndex(index []int) StructField
    // 返回该类型名为name的字段（会查找匿名字段及其子字段），
    // 布尔值说明是否找到，如非结构体将panic
    FieldByName(name string) (StructField, bool)
    // 返回该类型第一个字段名满足函数match的字段，布尔值说明是否找到，如非结构体将会panic
    FieldByNameFunc(match func(string) bool) (StructField, bool)
    // 如果函数类型的最后一个输入参数是"..."形式的参数，IsVariadic返回真
    // 如果这样，t.In(t.NumIn() - 1)返回参数的隐式的实际类型（声明类型的切片）
    // 如非函数类型将panic
    IsVariadic() bool
    // 返回func类型的参数个数，如果不是函数，将会panic
    NumIn() int
    // 返回func类型的第i个参数的类型，如非函数或者i不在[0, NumIn())内将会panic
    In(i int) Type
    // 返回func类型的返回值个数，如果不是函数，将会panic
    NumOut() int
    // 返回func类型的第i个返回值的类型，如非函数或者i不在[0, NumOut())内将会panic
    Out(i int) Type
    // 返回该类型的方法集中方法的数目
    // 匿名字段的方法会被计算；主体类型的方法会屏蔽匿名字段的同名方法；
    // 匿名字段导致的歧义方法会滤除
    NumMethod() int
    // 返回该类型方法集中的第i个方法，i不在[0, NumMethod())范围内时，将导致panic
    // 对非接口类型T或*T，返回值的Type字段和Func字段描述方法的未绑定函数状态
    // 对接口类型，返回值的Type字段描述方法的签名，Func字段为nil
    Method(int) Method
    // 根据方法名返回该类型方法集中的方法，使用一个布尔值说明是否发现该方法
    // 对非接口类型T或*T，返回值的Type字段和Func字段描述方法的未绑定函数状态
    // 对接口类型，返回值的Type字段描述方法的签名，Func字段为nil
    MethodByName(string) (Method, bool)
    // 内含隐藏或非导出方法
}
```

#### 2）函数---reflect.ValueOf(变量名)
##### 基本介绍：
获取变量的值，返回的reflect.Value是一个结构体类型。

##### 方法原型：
```
func ValueOf(i interface{}) Value

ValueOf返回一个初始化为i接口保管的具体值的Value，ValueOf(nil)返回Value零值。
```

##### reflect.Value介绍：
reflect.Value是一个结构体类型，可以用过这个结构体调用其方法：
```
func ValueOf(i interface{}) Value
func Zero(typ Type) Value
func New(typ Type) Value
func NewAt(typ Type, p unsafe.Pointer) Value
func Indirect(v Value) Value
func MakeSlice(typ Type, len, cap int) Value
func MakeMap(typ Type) Value
func MakeChan(typ Type, buffer int) Value
func MakeFunc(typ Type, fn func(args []Value) (results []Value)) Value
func Append(s Value, x ...Value) Value
func AppendSlice(s, t Value) Value
func (v Value) IsValid() bool
func (v Value) IsNil() bool
func (v Value) Kind() Kind
func (v Value) Type() Type
func (v Value) Convert(t Type) Value
func (v Value) Elem() Value
func (v Value) Bool() bool
func (v Value) Int() int64
func (v Value) OverflowInt(x int64) bool
func (v Value) Uint() uint64
func (v Value) OverflowUint(x uint64) bool
func (v Value) Float() float64
func (v Value) OverflowFloat(x float64) bool
func (v Value) Complex() complex128
func (v Value) OverflowComplex(x complex128) bool
func (v Value) Bytes() []byte
func (v Value) String() string
func (v Value) Pointer() uintptr
func (v Value) InterfaceData() [2]uintptr
func (v Value) Slice(i, j int) Value
func (v Value) Slice3(i, j, k int) Value
func (v Value) Cap() int
func (v Value) Len() int
func (v Value) Index(i int) Value
func (v Value) MapIndex(key Value) Value
func (v Value) MapKeys() []Value
func (v Value) NumField() int
func (v Value) Field(i int) Value
func (v Value) FieldByIndex(index []int) Value
func (v Value) FieldByName(name string) Value
func (v Value) FieldByNameFunc(match func(string) bool) Value
func (v Value) Recv() (x Value, ok bool)
func (v Value) TryRecv() (x Value, ok bool)
func (v Value) Send(x Value)
func (v Value) TrySend(x Value) bool
func (v Value) Close()
func (v Value) Call(in []Value) []Value
func (v Value) CallSlice(in []Value) []Value
func (v Value) NumMethod() int
func (v Value) Method(i int) Value
func (v Value) MethodByName(name string) Value
func (v Value) CanAddr() bool
func (v Value) Addr() Value
func (v Value) UnsafeAddr() uintptr
func (v Value) CanInterface() bool
func (v Value) Interface() (i interface{})
func (v Value) CanSet() bool
func (v Value) SetBool(x bool)
func (v Value) SetInt(x int64)
func (v Value) SetUint(x uint64)
func (v Value) SetFloat(x float64)
func (v Value) SetComplex(x complex128)
func (v Value) SetBytes(x []byte)
func (v Value) SetString(x string)
func (v Value) SetPointer(x unsafe.Pointer)
func (v Value) SetCap(n int)
func (v Value) SetLen(n int)
func (v Value) SetMapIndex(key, val Value)
func (v Value) Set(x Value)
func Copy(dst, src Value) int
func DeepEqual(a1, a2 interface{}) bool
```

##### 注意事项：
**在程序开发中，变量、interface{}、reflect.Value 是可以相互转换的，在实际的开发场景中常见。**

- interface{} 转换为 reflect.Value，可以使用： reflect.ValueOf(变量名的方式)

- reflect.Value 转换为 interface{}，可以使用： 变量名.interface()  
- 将数据转换为 特定类型，使用类型断言，方式为： 变量名.(变量类型) 



例如：
```
func test(in interfacve{}) {
    //将接口类型转换为 reflect.ValueOf(变量名)的方式
    rVal := reflect.ValueOf(in)
    
    //将reflect.Value类型转换为空接口interface{}
    iVal := rVal.interface()
    
    //使用类型断言进行变量的转换
    student := iVal.(Student)
}
```
![image](https://note.youdao.com/yws/api/personal/file/AA13828D595E4668B00976A765B8E036?method=download&shareKey=1920df373802be4eefa5b1036d4ba8f4)

### 4、反射入门案例：
#### 1）使用案例一：基本数据类型的反射操作
```
func main(){

	var i int
	i = 5

	//获得数据类型的value
	valueOf := reflect.ValueOf(i)
	fmt.Println(valueOf)

	//获取到反射的Type
	typeOf := reflect.TypeOf(i)
	//调用kind方法，来获得变量的类型
	kind := typeOf.Kind()
	fmt.Println(kind)

	//将reflect.Value转换为interface
	i2 := valueOf.Interface()
	test(i2)
}

func test(a interface{}) {
	fmt.Println("空接口的值：",a)

	//将空接口的值进行类型断言，实现类型的转换
	i := a.(int)
	fmt.Println("转换为int类型的值为：",i+3)
}

```

#### 2）使用案例二：结构体数据类型的反射操作
```
type Person struct {
	name string
	age int
	card string
	phone string
}

func (p Person) show(per Person) {
	fmt.Println("show方法的输出：",per)
}


//结构体数据类型的反射
func main(){
	person := Person{
		"zsl",
		23,
		"19961122",
		"123456",
	}
	test1(person)

}

func test1(a interface{}) {
	typeOf := reflect.TypeOf(a)
	fmt.Println(typeOf)

	valueOf := reflect.ValueOf(a)

	//得到结构体的第三个字段
	field:= valueOf.Field(2)
	fmt.Println(field)

	//获取类别---这个返回值为struct
	kind := valueOf.Kind()
	fmt.Println("类别为：",kind)

	//获取类型---返回值为Person
	i2 := valueOf.Type()
	fmt.Println("类别为：",i2)

	//结构体与interface、reflect.Value之间的转换
	i := valueOf.Interface()
	person := i.(Person)
	fmt.Println(person)
}

```


### 5、反射使用细节
- reflect.Value.Kind，获取的是变量的类别，返回的是一个常量。

- Type与Kind的区别：Type 是类型, Kind 是类别， Type 和 Kind 可能是相同的，也可能是不同的. 

- 数据的相互转换一定要保证转换的数据类型正确，否则会包panic。

- 通过反射修改变量：获取 reflect.Value的时候，传入变量的地址，
然后再使用 valueOf.Elem().SetInt(200) 进行修改变量的值。例如：
```
func main() {
	var i int
	i = 5

	//获得数据类型的value
	valueOf := reflect.ValueOf(&i)

	valueOf.Elem().SetInt(200)
	fmt.Printf("%v",i)
}
```

### 6、反射最佳实践：
#### 1）反射常用方法：
- valueOf.NumField() ---- 获取结构体总共有多少个字段

- valueOf.Field(num int) ---- 根据下标获取第几个字段，从下标0开始。
- valUeOf.NumMethod() ---- 获取总共有多少个方法
- valueOf.FieldByName(str string) ---- 根据 str来获取struct中的属性
- valueOf.Elum() ---- 将指针类型的reflect.Value地址 转换为 reflect.Value数据，用于反射操作。
- typeOf.Field(1).Tag.Get("json") ---- 获取结构体中第二个字段的tag标签

```
package main
import (
	"fmt"
	"reflect"
)
//定义了一个Monster结构体
type Monster struct {
	Name  string `json:"name"`
	Age   int `json:"monster_age"`
	Score float32 `json:"成绩"`
	Sex   string

}

//方法，返回两个数的和
func (s Monster) GetSum(n1, n2 int) int {
	return n1 + n2
}

//方法， 接收四个值，给s赋值
func (s Monster) Set(name string, age int, score float32, sex string) {
	s.Name = name
	s.Age = age
	s.Score = score
	s.Sex = sex
}

//方法，显示s的值
func (s Monster) Print() {
	fmt.Println("---start~----")
	fmt.Println(s)
	fmt.Println("---end~----")
}

func TestStruct(a interface{}) {
	//获取reflect.Type 类型
	typ := reflect.TypeOf(a)
	//获取reflect.Value 类型
	val := reflect.ValueOf(a)
	//获取到a对应的类别
	kd := val.Kind()
	//如果传入的不是struct，就退出
	if kd !=  reflect.Struct {
		fmt.Println("expect struct")
		return
	}

	//获取到该结构体有几个字段
	num := val.NumField()

	fmt.Printf("struct has %d fields\n", num) //4
	//变量结构体的所有字段
	for i := 0; i < num; i++ {
		fmt.Printf("Field %d: 值为=%v\n", i, val.Field(i))
		//获取到struct标签, 注意需要通过reflect.Type来获取tag标签的值
		tagVal := typ.Field(i).Tag.Get("json")
		//如果该字段于tag标签就显示，否则就不显示
		if tagVal != "" {
			fmt.Printf("Field %d: tag为=%v\n", i, tagVal)
		}
	}

	//获取到该结构体有多少个方法
	numOfMethod := val.NumMethod()
	fmt.Printf("struct has %d methods\n", numOfMethod)

	//var params []reflect.Value
	//方法的排序默认是按照 函数名的排序（ASCII码）
	val.Method(1).Call(nil) //获取到第二个方法。调用它


	//调用结构体的第1个方法Method(0)
	var params []reflect.Value  //声明了 []reflect.Value
	params = append(params, reflect.ValueOf(10))
	params = append(params, reflect.ValueOf(40))
	res := val.Method(0).Call(params) //传入的参数是 []reflect.Value, 返回[]reflect.Value
	fmt.Println("res=", res[0].Int()) //返回结果, 返回的结果是 []reflect.Value*/

}
func main() {
	//创建了一个Monster实例
	var a Monster = Monster{
		Name:  "黄鼠狼精",
		Age:   400,
		Score: 30.8,
	}
	//将Monster实例传递给TestStruct函数
	TestStruct(a)
}
```

#### 2）使用反射调用数据变量的方法
程序进行反射以后，程序的方法是来给程序进行自动排序。根据ASCLL码来进行排序。

反射使用的是 Method( num int) 来进行选择执行第几个方法。

反射使用 Call() 来进行参数的传递。如果没有参数的话，传入 nil 即可。方法格式为：
```
func (v Value) Call(in []Value) []Value

//传入的必须是 reflect.Value 切片或者reflect.Value数组
```

例如：
```
//调用第三个方法，传入参数为10 。
valueOf.Method(2).Call(10)

//调用第二个方法，传入参数为切片
var params []reflect.Value  //声明一个切片
params = append(params, reflect.ValueOf(10)) //向切片中添加参数
params = append(params, reflect.ValueOf(40)) //向切片中添加参数
valueOf.Method(1).Call(params) //传入参数，调用第二个方法
```


## 十四、Golang网络编程
### 1、网络编程基本介绍
Golang的主要设计目标之一就是面向大规后端服务程序，网络通信这块是服务端程序必不可少的一个重要不组成部分。

#### 1）网络编程的两种模式
##### 第一种：TCP socket编程
TCP socket编程，是网络编程的主流。之所以叫TCP socket，是因为底层是基于TCP/ip协议。

##### 第二种：b/s结构的http编程
b/s结构的http编程，我们使用浏览器去访问服务器时，使用的是http协议，而http底层依赖的是tcp socket实现的。

#### 2）网线、网卡、无线网卡
计算机要进行相互之间的通信，需要使用网线、网卡或者是无线网卡。

### 2、TCP/IP协议
TCP/IP（Transmission Control Protocol/Internet Protocol)的简写,中文译名为**传输控制协议/因特网互联协议**，又叫**网络通讯协议**。


这个协议是 Internet 最基本的协议、Internet 国际互联网络的基础，简单地说，就是由网络层的 IP 协议和传输层的 TCP 协议组成的。

### 3、 IP地址与端口
#### 1）IP的地址
##### 基本介绍：
每隔Internet上每台主机和路由器都有一个IP地址，它包括网络号和主机号，ip地址有 IPV4(32位)、IPV6(128位)。

#### 2）端口
##### 基本介绍：
一个IP地址，拥有65536（256*256）个端口。

服务器端监听一个端口，然后进行服务之间的交互，该端口就是其他程序与服务器之间的通信的通道。

一旦一个端口被某个程序监听了以后（也就是端口被占用），其他程序就不能够再次监听该端口。

##### 基本分类:

端口号 | 端口介绍 | 注意事项
---|---|---
0 号端口 | 保留端口 | 程序员不能使用
1-1024 号端口 | 固定端口 | 程序员不能使用
22 号端口 | SSH远程登录协议端口 | 程序员不能使用
23 号端口 | Telnet端口 | 程序员不能使用
21 号端口 | ftp端口 | 程序员不能使用
25 号端口 | smtp服务端口 | 程序员不能使用
80 号端口 | iis服务端口 | 程序员不能使用
1025-65535 号端口 | 动态端口 | 程序员可以使用

##### 注意事项：
- 在计算机中，应该尽量少开启端口。

- 一个端口只能被一个程序监听。
- 使用 netstat -an 可以查看端口那些已经被监听。
- 可以使用 netstat –anb 来查看监听端口的 pid,在结合任务管理器关闭不安全的端口。


### 4、tcp socket 编程的客户端和服务端
#### 1）服务器端处理流程：
第一步：监听端口，比如8888号端口来进行监听。

第二步：接收客户端的tcp连接，建立起客户端与服务器端的连接。

第三步：创建goroutine，处理该链接的请求。

#### 2）客户端处理流程：
第一步：建立起与服务器的连接（首先，先客户端自动为这个程序分配一个端口，然后让这个端口与socket的端口进行交互，形成连接）。

第二步：发送请求数据，接收服务器端返回来的结果数据。

第三步：关闭socket连接。

#### 3）客户端与服务器端的图例：
```
graph TB
H[协程A] --> A(服务器)
F[协程B] --> A(服务器)
G[协程C] --> A(服务器)
I[协程...] --> A(服务器)
A[服务器] -->|连接|B(客户端A)
B -->|连接|A
A[服务器] -->|连接|C(客户端B)
C -->|连接|A
A[服务器] -->|连接|D(客户端C)
D -->|连接|A
A[服务器] -->|连接|E(客户端...)
E -->|连接|A
```

#### 4）服务器端代码实现：
```
func main(){
	fmt.Println("服务器开始进行监听了")

	//使用的协议是使用 tcp协议，且监听的是本机的 8888 端口
	listener, err := net.Listen("tcp", "0.0.0.0:8888")

	//判断是否监听成功,未成功的话，直接程序结束
	if err != nil {
		fmt.Println("端口监听失败")
		return
	}

	//相当于是开启一直监听模式，程序结束才关闭。相当于java中的 io操作
	defer listener.Close()

	for {
		//循环等待客户端来与服务端进行连接,也就是说建立多个连接
		conn, err1 := listener.Accept()
		if err1 != nil {
			fmt.Println("连接失败，请重试",err1)
		} else {
			fmt.Println("连接成功，",conn)
		}
		//当连接成功以后，为客户端监理一个协程操作
		go process(conn)
	}
}

func process(conn net.Conn) {

	//用完以后，关闭连接
	defer conn.Close()

	var bt []byte
	bt = make([]byte,512)

	//循环读取数据，返回的 n 表示数据的长度
	n, err := conn.Read(bt)
	if err != nil {
		fmt.Println("服务器读取客户端发送来的数据失败,",err)
	}
	//将客户端发送来的数据接收到
	fmt.Println(string(bt[:n]))
}
```

#### 4）客户端代码的实现：
```
func main(){

	conn, err := net.Dial("tcp", "127.0.0.1:8888")
	if err != nil {
		fmt.Println("客户端无法实现连接", err)
		return
	}

	//客户端发送单行数据，然后就退出
	//os.Stdin表示标准输入
	reader := bufio.NewReader(os.Stdin)

	//从终端读取一行用户输入，并准备发送给服务器
	readString, err1 := reader.ReadString('\n')
	if err1 != nil {
		fmt.Println("从终端读取数据失败",err1)
	}

	//将终端读取到的数据发送到服务器
	write, err2 := conn.Write([]byte(readString))
	if err2 != nil {
		fmt.Println("发送到服务器失败")
	}

	fmt.Println("客户端发送成功，数据为：",write)
}
```

## 十五、Golang整合redis
### 1、Golang安装Redis第三方库
先进入到GOPATH目录，输入命令：
```
go get github.com/garyburd/redigo/redis
```

然后在GOPATH目录下的src目录下出现了一个专门用于操作Redis的文件。

### 2、Golang连接Redis
直接调用下载的第三方函数来进行redis的连接。

```
func main() {
	//通过远程连接到redis数据库
	//第一个参数是指定什么方式的连接，
	//第二个参数指的是IP地址与端口号
	//第一个返回值conn使用一个结构体，主要是用于存放连接信息的
	//第二个返回值 err 是返回连接的错误信息
	conn, err := redis.Dial("tcp", "192.168.21.133:6379")
	if err != nil {
		fmt.Println("连接失败，",err)
		return
	}
	//连接成功
	fmt.Println("连接成功，",conn)
}
```

### 3、Golang连接Redis的Demo
```
    //操作String数据类型
	//插入数据
	_, err1 := conn.Do("set", "name", "zsl")
	if err1 != nil {
		fmt.Println("set数据失败",err1)
	}
	fmt.Println("set数据成功：")

	//取出数据，使用redis.String()方法来实现数据的转换
	do, err2 := redis.String(conn.Do("Get", "name"))
	if err2 != nil{
		fmt.Println("获取数据失败")
	}
	fmt.Println("取出的数据为：",do)
	
	
	
	//操作Hash数据类型
	//插入数据
	_, err1 := conn.Do("HSet", "user2","name2", "zsl2")
	if err1 != nil {
		fmt.Println("set数据失败",err1)
		return
	}
	fmt.Println("set数据成功：")

	//取出数据，使用redis.String()方法来实现数据的转换
	do, err2 := redis.String(conn.Do("HGet", "user2","name2"))
	if err2 != nil{
		fmt.Println("获取数据失败")
	}
	fmt.Println("取出的数据为：",do)

```

### 4、Golang使用Redis连接池
```
func init(){
	pool = &redis.Pool{
		//最大空闲连接数，也就是创建8个线程
		MaxIdle:8,
		//最大空闲时间，表示和数据库的最大连接数，0表示没有限制。
		MaxActive:0,
		//最大空闲时间，当空闲时间超出以后未使用，就会返回得到连接池
		IdleTimeout:100,
		//初始化连接池的代码
		Dial: func() (redis.Conn, error) {
			return redis.Dial("tcp","192.168.21.133:6379")
		},
	}

	//关闭redis连接池
	//pool.Close()
}

func main(){
	//从数据库中获取一个连接池
	conn := pool.Get()

	//关闭这个连接，让这个连接返回到连接池
	defer conn.Close()

	_, err1 := conn.Do("set", "qq", "zsl")
	if err1 != nil {
		fmt.Println("set数据失败",err1)
	}
	fmt.Println("set数据成功：")

	//取出数据，使用redis.String()方法来实现数据的转换
	do, err2 := redis.String(conn.Do("Get", "qq"))
	if err2 != nil{
		fmt.Println("获取数据失败")
	}
	fmt.Println("取出的数据为：",do)
}
```

## 十六、数据结构
### 1、数据结构基本介绍
数据结构是一门专门研究算法的学科，自从有了编程语言也就有了数据结构，能够编码成更加优美的编程方式。

程序 = 数据结构 + 算法。

### 2、数据结构与算法的关系
算法是程序的灵魂，算法的基石是数据结构。数据结构能够保证算法更加简单，逻辑更加清晰。使用数据结构的算法能够让程序更加坚如磐石。

### 3、稀疏sparsearray数组
#### 1）基本介绍：
当一个数组中的大部分元素为0，或者同一个值的数组时，可以使用稀疏数组来保存该数组。

#### 2）稀疏数组处理方法：
第一步：记录数组一共有几行几列，有多少个不同的值。

第二步：把具有不同的元素的行列及值记录在一个小规模的数组中，从而缩小程序的规模。

#### 3）图像表示：
稀疏矩阵图为：

列行 | 第0列 | 第1列 | 第2列 | 第3列 | 第4列
---|---|---|---|---|---
第0行 | 3 | 0 | 0 | 0 | 0
第1行 | 0 | 0 | 2 | 0 | -9
第2行 | 0 | 0 | 0 | 0 | 0
第3行 | 0 | 7 | 0 | 0 | 0

记录的小规模数组为：

列行 | 行 | 列 | 值
---|---|---|---
（0）| 0 | 0 | 3
（1）| 1 | 2 | 2
（2）| 1 | 4 | -9
（3）| 3 | 1 | 7

#### 4）案例演示

##### 需求情况：

1) 使用稀疏数组，来保留类似前面的二维数组(棋盘、地图等等)

2) 将稀疏矩阵存入到一个结构体中，实现行、列、value的存储。

##### 代码演示：
```
/**
稀疏矩阵的解析与返回
 */

 //定义一个专门用于接受稀疏矩阵不为0 的数据的结构体
 type Node struct {
 	//存储行
 	row int
 	//存储列
 	col int
 	//存储值
 	val int
 }

 func main(){
 	//1、先创建一个原始数组
 	var arr [11][11]int
 	arr[1][2] = 1
 	arr[2][3] = 2

 	//输出原始数组
 	for _,v :=range arr{
 		for _,v2 := range v {
 			fmt.Printf(" %d ",v2)
		}
 		fmt.Println(" ")
	}

 	//将稀疏矩阵转换为一个 行列值 的切片
 	var nodes []Node

 	//初始化节点，也就是需要存储一下这个稀疏矩阵是多少行，多少列
 	node :=Node{
 		row:len(arr),
 		col:len(arr),
 		val:0,
	}
 	nodes = append(nodes,node)

 	//遍历系数矩阵将数据存入Node切片中
	 for i,v :=range arr{
		 for j,v2 := range v {
		 		if v2 != 0 {
		 			node := Node{
		 				row:i,
		 				col:j,
		 				val:v2,
					}
		 			//将数据append到node切片中
					nodes = append(nodes, node)
				}
		 }
	 }

	 //输出这个nodes
	 for i,v :=range nodes {
	 	//因为第一个节点是存储的是这个稀疏矩阵的行数与列数
	 	if i == 0 {
	 		fmt.Printf("这个矩阵一共有%d行，一共有%d列\n",v.row,v.col)
	 		continue
		}
	 	fmt.Printf("第%d个数：行：%d 列：%d 值：%d\n",i,v.row,v.col,v.val)
	 }

	 //恢复这个稀疏矩阵,先创建这个数组
	 var arr3 [11][11]int

	 for i,v :=range nodes {
	 	//因为第一个数据存的是这个矩阵的行和列，不应该放入到稀疏矩阵中
	 	if i != 0 {
			//将数据提供给定义的这个数组
			arr3[v.row][v.col] = v.val
		}
	 }

	 //输出这个返回了的稀疏矩阵
	 fmt.Println(" ")
	 fmt.Println("重组的矩阵为：")
	 for _,v :=range arr3{
		 for _,v2 := range v {
			 fmt.Printf(" %d ",v2)
		 }
		 fmt.Println(" ")
	 }
 }
```


### 4、队列（queue）
#### 1）基本介绍：
队列是一个有序列表，可以使用**数组或者链表来实现**。

队列遵循的是 **先入先出** 原则，先存入的数据先取出来，后存入的数据后取出来。

#### 2）使用数组模拟队列
队列本身是有序列表，若使用数组的结构来存储队列的数据，则队列数组的声明如下：  
- 声明maxSize来表明该队列的最大容量。
- 使用两个变量front与rear分别标记队列前端与后端的下标，front是随着数据输出而改变，rear是随着数据的输入而该表。

第一次：在初始化数组的时候，有一个空的数组，而front与rear均为-1：

队列位置|队列的值 
---|---
maxSize-1 | 
4 | 
3 |
2 |
1 |
0 |

第二次：存入数据的时候，rear会变化为2，而front依然不变：
队列位置|队列的值 
---|---
maxSize-1 | 
4 | 
3 |
2 | 45
1 | 34
0 | 66

注意：这里存入数据的话，是先在0的位置存入66，然后再在1的位置存入34；接着再在2的位置存入45。

第三次：取出数据的时候，先取出的是第一个数据，front的值改变为1，rear的值依然为2：
队列位置|队列的值 
---|---
maxSize-1 | 
4 | 
3 |
2 | 45
1 | 34
0 | 

#### 3）非环形数组队列
##### 需求分析：
第一步：创建一个数组arr，作为一个队列的原型。  
第二步：使用front表示队列头，初始化front的值为-1。   
第三步：使用rear表示队列尾，初始化rear的值为-1。   
第四步：完成队列的基本查找。   

##### 异常情况：
当队列满了以后，取出数据，但是也无法实现继续在前面添加数据。

##### 代码实现：
```
//使用数组实现 非环形队列

//定义一个常量来规定这个队列的长度
const lens int = 4

//创建一个结构体来进行管理一个队列
type Queue struct {
	maxSize int //队列的最大长度
	array [lens]int //数组，需要给这个数组添加一个方法与操作，才能够变成队列
	front int //指向队列的队头
	rear int //执行队列的队尾
}

//向队列中添加数据
func (queue *Queue)addNum(val int)(error){
	//先判断队列是否已经满了
	if queue.rear == queue.maxSize -1 {
		return errors.New("队列已满")
	}
	//队列没满的话，就会将数据添加到队列中
	queue.rear++
	queue.array[queue.rear] = val
	fmt.Println("添加数据成功")
	return nil
}

//从队列中按顺序取出一个数据
func (queue *Queue) getQueue()(int,error) {
	//判断队列是否为空
	if queue.front == queue.rear {
		return -1,errors.New("队列已空")
	}
	queue.front++
	val := queue.array[queue.front]
	return val,nil
}

//显示队列
func (queue Queue)showQueue(){
	fmt.Println("队列的详细信息微：")

	for i:=queue.front+1;i<=queue.rear;i++ {
		fmt.Printf("array[%d] = %d\t",i,queue.array[i])
	}
	fmt.Println(" ")
}

func main(){

	queue := Queue{
		maxSize:lens,
		front:-1,
		rear:-1,
	}
	var key string
	var val int

	for {
		fmt.Println("1、输入add表示添加队列数据")
		fmt.Println("2、输入get表示获取队列数据")
		fmt.Println("3、输入show表示显示队列数据")
		fmt.Println("4、输入exit退出程序")

		fmt.Scanln(&key)
		switch key {
			case "add":
				fmt.Println("请输入你需要添加到队列的数据")
				fmt.Scanln(&val)
				err := queue.addNum(val)
				if err != nil {
					fmt.Println("添加队列数据失败,",err.Error())
				}
			case "get":
				val, err := queue.getQueue()
				if err != nil{
					fmt.Println(err.Error())
				} else {
					fmt.Println("从队列中取出的值为：",val)
				}
		case "show":
				queue.showQueue()
			case "exit":
				fmt.Println("get")
			}
		}
}
```

#### 4）环形数组队列
##### 基本介绍：
将数组形成一个环状，实现非环形数组队列的优化。

##### 分析思路:
1)	什么时候表示队列满 (tail + 1) % maxSize = head   
2)	tail == head  表示空   
3)	初始化时， tail = 0 head = 0   
4)	怎么统计该队列有多少个元素 (tail + maxSize - head ) % maxSize   

##### 代码实现：
```
//使用环形数组实现队列

//定义一个常量，实现设置链表的长度
const lens  int = 5

//定义一个结构体，来实现环形队列的管理
type CirclQueue struct {
	maxSize int  //最大的链表长度
	array [lens]int //数组
	head int //执行队列队首
	tail int //执行队列队尾
}
//入队列
func (queue *CirclQueue) push(val int)(error){
	if queue.isFull() {
		return errors.New("队列已满")
	}
	//将这个值放在最后的尾部
	queue.array[queue.tail] = val
	queue.tail = (queue.tail + 1)%queue.maxSize
	return nil
}

//出队列
func (queue *CirclQueue) pop()(val int,err error){
	if queue.isEmpty(){
		return 0,errors.New("队列已空")
	}

	//取出数据
	val1 := queue.array[queue.head]
	queue.head = (queue.head + 1)%queue.maxSize
	return val1,nil
}

//显示队列
func (queue *CirclQueue) listQueue(){
	size := queue.Size()
	if size == 0 {
		fmt.Println("队列为空")
		return
	}

	//输出队列
	fmt.Println("输出队列")
	tempHead := queue.head
	for i:=0;i<size;i++ {
		fmt.Printf("array[%d]=%d \t",tempHead,queue.array[tempHead])
		tempHead = (tempHead + 1) % queue.maxSize
	}
	fmt.Println(" ")
}

//判断环形队列是否已满
func (queue CirclQueue) isFull() bool{
	return (queue.tail+1) % queue.maxSize == queue.head
}

//判断环形队列是否已空
func (queue CirclQueue) isEmpty() bool{
	return queue.tail == queue.head
}

//获得环形队列有多少个元素
func (queue CirclQueue) Size() int{
	return (queue.tail + queue.maxSize-queue.head) % queue.maxSize
}

func main(){

	queue := &CirclQueue{
		maxSize:lens,
		head:0,
		tail:0,
	}

	var key string
	var val int

	for {
		fmt.Println("1、输入add表示添加队列数据")
		fmt.Println("2、输入get表示获取队列数据")
		fmt.Println("3、输入show表示显示队列数据")
		fmt.Println("4、输入exit退出程序")

		fmt.Scanln(&key)
		switch key {
		case "add":
			fmt.Println("请输入你需要添加到队列的数据")
			fmt.Scanln(&val)
			err := queue.push(val)
			if err != nil {
				fmt.Println("添加队列数据失败,",err.Error())
			}
		case "get":
			val, err := queue.pop()
			if err != nil{
				fmt.Println(err.Error())
			} else {
				fmt.Println("从队列中取出的值为：",val)
			}
		case "show":
			queue.listQueue()
		case "exit":
			fmt.Println("get")
		}
	}
}
```

### 5、链表
#### 1）链表基本介绍
链表是一个物理存储单元上**非连续、非顺序的存储结构**，数据元素的逻辑顺序是通过链表中的指针链接顺序来形成的一系列节点。

#### 2）链表的结构
链表的每个节点包括两部分：一部分是用于存储元素的数据的数据域；一部分是用于存储下一个节点的指针域。

#### 3）链表示意图
![image](https://note.youdao.com/yws/api/personal/file/D3A08363CD1A495B99E17319983F8780?method=download&shareKey=954d35c12893276a21a3321dd02f892f)

**注意：** 通常情况下，为了对单链表进行增删改查的操作，我们都会给他设置一个头结点，这个头结点的作用是用于标记链表头，且这个节点不存放数据。

#### 4）链表的应用实例----单向链表
##### 需求分析：
使用带 head 头的单向链表实现 –水浒英雄排行榜管理。完成对英雄人物的增删改查操作， 注: 删除和修改,查找可以考虑学员独立完成

##### 代码演示：
```
import (
	"fmt"
)

//定义一个结构体
type HeroNode struct {
	no int
	name string
	nickname string
	//指向下一个节点
	next *HeroNode
}

//给链表插入一个节点
//方式一：在单链表的最后加入一个节点。
//第一个形参表示的是将节点插入到哪一个头结点的链表中；第二个形参表示的是新插入的这个节点
func InsertHerNode(head *HeroNode,newHeroNode *HeroNode){
	//先找到该链表的最后节点
	temp := head
	for {
		//如果 temp.next == nil 的话，就查找到了最后一个节点了
		if temp.next == nil {
			break
		}
		//让temp不断的指向下一个节点
		temp = temp.next
	}

	//让新节点加入到链表的结尾
	temp.next = newHeroNode
}

//方式二：根据hero的no编号从小到大进行有序插入。
//第一个形参表示的是将节点插入到哪一个头结点的链表中；第二个形参表示的是新插入的这个节点
func InsertHerNodeByNo(head *HeroNode,newHeroNode *HeroNode){
	//创建一个辅助节点
	temp := head
	//添加一个标识符，默认为允许添加数据的意思
	flag := true

	//让插入的节点的no，与temp的下一个节点的no进行比较
	for {
		//表明该节点进入到了链表的最后
		if temp.next == nil {
			break
		} else if temp.next.no > newHeroNode.no {
			//说明插入的节点应该插入到temp的后面去
			break
		} else if temp.next.no == newHeroNode.no {
			//说明链表中已经存在这个no节点，不能够继续插入
			flag = false
			break
		}
		temp = temp.next
	}

	if flag == false {
		fmt.Printf("已经存在这个no节点%d，不能够重新插入",newHeroNode.no)
		return
	} else {
		//进行节点的插入操作
		newHeroNode.next = temp.next
		temp.next = newHeroNode
	}
}

//单链表的节点删除操作
func deleteHeroNode(head *HeroNode,no int){
	temp := head
	//表示的是没找到这个no
	flag := false
	//使用比较的方式进行查找到需要删除的no节点
	for {
		//表明该节点进入到了链表的最后
		if temp.next == nil {
			break
		}  else if temp.next.no == no {
			//说明链表中已经存在这个no节点，则表示的是找到了这个节点
			flag = true
			break
		}
		temp = temp.next
	}

	//找到了数据，删除这个节点，让这个节点指向下下个数据节点即可
	if flag {
		temp.next = temp.next.next
	} else {
		fmt.Println("没查找到这个no为 %d 的节点",no)
	}
}

//显示链表的所有信息
func ListHeroNode(head *HeroNode){
	temp := head

	//判断是否为空链表
	if temp.next == nil {
		fmt.Println("这个是空链表，请插入数据在遍历。")
		return
	}

	//遍历这个链表
	for {
		//输出链表，实现遍历的效果
		fmt.Printf("[%d ,%s ,%s]\n",temp.next.no,temp.next.name,temp.next.nickname)

		temp = temp.next

		//判断是否在链表的尾部了，如果已经在链表的尾部，那么就停止遍历
		if temp.next == nil {
			fmt.Println("\n链表的节点遍历完成")
			break
		}
	}
}

func main() {
	//第一步：创建一个链表的头结点
	head := &HeroNode{}

	//第二步：创建一个新的HerNode节点
	hero1 := &HeroNode{
		no:1,
		name:"宋江",
		nickname:"及时雨",
	}
	hero2 := &HeroNode{
		no:2,
		name:"卢俊义",
		nickname:"玉麒麟",
	}
	hero3 := &HeroNode{
		no:3,
		name:"林冲",
		nickname:"豹子头",
	}
	//将 英雄 添加到hero这个链表中
	InsertHerNodeByNo(head,hero2)
	InsertHerNodeByNo(head,hero1)
	InsertHerNodeByNo(head,hero3)

	//遍历节点
	ListHeroNode(head)

	//删除一个节点
	deleteHeroNode(head,2)
	ListHeroNode(head)

}
```

#### 5）链表的应用实例----双向链表
##### 基本介绍：
双向链表是链表中的一种，它的每一个数据节点都含有两个指针，分别指向的是前一个节点以及后面一个节点。从表的任意节点开始，都能够方便的访问其前区域后继节点。

##### 示例图：
![image](https://note.youdao.com/yws/api/personal/file/48E7982535B547BBBCD3A9C069006CE6?method=download&shareKey=ac37c438e508c22f720f1eda2030f35a)

##### 代码实现：
```
package main

import (
	"fmt"
)

//单向链表的使用

//定义一个结构体
type HeroNode11 struct {
	no int
	name string
	nickname string
	//指向前一个节点
	pre *HeroNode11
	//指向下一个节点
	next *HeroNode11
}

//给双向链表插入一个节点
//方式一：在单链表的最后加入一个节点。
//第一个形参表示的是将节点插入到哪一个头结点的链表中；第二个形参表示的是新插入的这个节点
func InsertHerNode1(head *HeroNode11,newHeroNode11 *HeroNode11){
	//先找到该链表的最后节点
	temp := head
	for {
		//如果 temp.next == nil 的话，就查找到了最后一个节点了
		if temp.next == nil {
			break
		}
		//让temp不断的指向下一个节点
		temp = temp.next
	}

	//让新节点加入到链表的结尾
	temp.next = newHeroNode11
	//让这个节点的前驱指向的是前一个节点
	newHeroNode11.pre = temp
}


//方式二：根据hero的no编号从小到大进行有序插入。
//第一个形参表示的是将节点插入到哪一个头结点的链表中；第二个形参表示的是新插入的这个节点
func InsertHerNodeByNo1(head *HeroNode11,newHeroNode *HeroNode11){
	//创建一个辅助节点
	temp := head
	//添加一个标识符，默认为允许添加数据的意思
	flag := true

	//让插入的节点的no，与temp的下一个节点的no进行比较
	for {
		//表明该节点进入到了链表的最后
		if temp.next == nil {
			break
		} else if temp.next.no > newHeroNode.no {
			//说明插入的节点应该插入到temp的后面去
			break
		} else if temp.next.no == newHeroNode.no {
			//说明链表中已经存在这个no节点，不能够继续插入
			flag = false
			break
		}
		temp = temp.next
	}

	if flag == false {
		fmt.Printf("已经存在这个no节点%d，不能够重新插入",newHeroNode.no)
		return
	} else {
		//进行节点的插入操作
		newHeroNode.next = temp.next
		newHeroNode.pre = temp
		if temp.next != nil {
			temp.next.pre = newHeroNode
		}
		temp.next = newHeroNode
	}
}



//前序遍历显示链表的所有信息
func ListHeroNode11(head *HeroNode11){
	temp := head

	//判断是否为空链表
	if temp.next == nil {
		fmt.Println("这个是空链表，请插入数据在遍历。")
		return
	}

	//遍历这个链表
	for {
		//输出链表，实现遍历的效果
		fmt.Printf("[%d ,%s ,%s]\n",temp.next.no,temp.next.name,temp.next.nickname)

		temp = temp.next

		//判断是否在链表的尾部了，如果已经在链表的尾部，那么就停止遍历
		if temp.next == nil {
			fmt.Println("\n链表的节点遍历完成")
			break
		}
	}
}

//后序遍历显示链表的所有信息
func ListHeroNode12(head *HeroNode11){
	temp := head

	if temp.next == nil {
		fmt.Println("空空如也。。。。")
		return
	}

	for {
		if temp.next == nil {
			break
		}
		temp = temp.next
	}
	for {
		fmt.Printf("[%d ,%s ,%s]\n",temp.no,temp.name,temp.nickname)

		//判断是否链表头
		temp = temp.pre
		if temp.pre == nil {
			fmt.Println("\n后序遍历链表的节点遍历完成")
			break
		}
	}
}

//双向链表删除一个节点
func deleteHeroNode1(head *HeroNode11,no int){
	temp := head
	//表示的是没找到这个no
	flag := false
	//使用比较的方式进行查找到需要删除的no节点
	for {
		//表明该节点进入到了链表的最后
		if temp.next == nil {
			break
		}  else if temp.next.no == no {
			//说明链表中已经存在这个no节点，则表示的是找到了这个节点
			flag = true
			break
		}
		temp = temp.next
	}

	//找到了数据，删除这个节点，让这个节点指向下下个数据节点即可
	if flag {
		temp.next = temp.next.next
		if temp.next != nil{
			temp.next.pre = temp
		}
	} else {
		fmt.Println("没查找到这个no为 %d 的节点",no)
	}
}

func main() {
	//第一步：创建一个链表的头结点
	head := &HeroNode11{}

	//第二步：创建一个新的HerNode节点
	hero1 := &HeroNode11{
		no:1,
		name:"宋江",
		nickname:"及时雨",
	}
	hero2 := &HeroNode11{
		no:2,
		name:"卢俊义",
		nickname:"玉麒麟",
	}
	hero3 := &HeroNode11{
		no:3,
		name:"林冲",
		nickname:"豹子头",
	}
	//将 英雄 添加到hero这个链表中
	InsertHerNodeByNo1(head,hero2)
	InsertHerNodeByNo1(head,hero1)
	InsertHerNodeByNo1(head,hero3)

	deleteHeroNode1(head,1)
	//遍历节点
	ListHeroNode11(head)
	ListHeroNode12(head)
}

```

#### 6）链表的应用实例----环形单向链表
##### 示例图：
![image](https://note.youdao.com/yws/api/personal/file/9214381A10D84CE5B27C0737CE686727?method=download&shareKey=f3cf25270b47c7349c7edf36d62a2e49)

##### 代码演示：
```
package main

import "fmt"

//环形单向链表

//先定义一个结构体
type CatNode struct {
	no int
	name string
	next *CatNode
}

//插入一个元素
func InsertCatNode(head *CatNode,newCatNode *CatNode){
	//判断是不是插入的第一只猫,如果是第一只猫，那么久将这只猫存放在头节点中
	if head.next == nil {
		head.no = newCatNode.no
		head.name = newCatNode.name
		//让这个链表形成一个环状
		head.next = head
		return
	}

	//如果不是第一只猫，那么久加入到环形队列中
	temp := head
	//判断什么时候是在了队尾
	for {
		if temp.next == head {
			break
		}
		temp = temp.next
	}

	temp.next = newCatNode
	newCatNode.next = head
}

//输出环形链表
func ShowCatNodes(head *CatNode){
	temp := head
	//判断是否是空链表
	if temp.next == nil {
		fmt.Println("空空如也。。。")
		return
	}

	for {
		fmt.Printf("Cat的信息为：%d==%s\n",temp.no,temp.name)
		if temp.next == head {
			fmt.Println("遍历完成")
			break
		}
		temp = temp.next
	}

}

//删除环形队列节点
func deleteCatNode(head *CatNode,no int)(*CatNode){
	temp := head
	helper := head

	//判断是否是空链表
	if temp.next == nil {
		fmt.Println("这个是一个空链表，不能够删除")
		return head
	}

	//判断是否是一个节点
	if temp.next == head {
		temp.next = nil
		return head
	}

	//将helper定位到最后
	for {
		if helper.next == head {
			break
		}
		helper = helper.next
	}


	//两个及两个以上的节点
	flag := true  //表示未删除
	for {
		if temp.next == head {
			//如果比较到这里，就说明没找到
			break
		}
		if temp.no == no {
			//如果删除了头结点，则需要将头结点放置在下一个元素中
			if temp == head {
				head = head.next
			}
			//找到了这个节点
			helper.next = temp.next
			fmt.Println("删除成功")
			flag = false
		}
		temp = temp.next
		helper = helper.next
	}
	//在比较一次最后一个节点
	//如果flag为真，表明上面没有被删除
	if flag {
		if temp.no == no {
			helper.next = temp.next
			fmt.Println("删除成功")
		} else {
			fmt.Println("没查找到这个节点")
		}
	}
	return head
}

func main () {

	//县创建一个头结点，并且这个头结点必须要存放数据
	head := &CatNode{}

	//创建第一只猫
	cat1 := &CatNode{
		no:   1,
		name: "qqq",
	}

	cat2 := &CatNode{
		no:   2,
		name: "www",
	}

	//cat3 := &CatNode{
	//	no:   3,
	//	name: "eee",
	//}


	InsertCatNode(head,cat1)
	InsertCatNode(head,cat2)
	//InsertCatNode(head,cat3)

	head1 := deleteCatNode(head, 1)
	ShowCatNodes(head1)

}

```

#### 7）约瑟夫问题
##### 问题介绍：
设编号为 1，2，…  n 的 n 个人围坐一圈，约定编号为 k（1<=k<=n）的人从 1 开始报数，数到 m 的那个人出列，它的下一位又从 1 开始报数，数到 m 的那个人又出列，依次类推， 直到所有人出列为止，由此产生一个出队编号的序列。