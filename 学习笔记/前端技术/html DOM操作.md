[TOC]

# 一. HTML DOM

## 1.1 HTML DOM方法
> 方法是我们可以在节点（HTML元素）上执行的动作。
- `document.getElementBId("元素id")` 返回带有指定id的元素。
- `getElementByName("name值")` 返回包含带有指定name的元素的节点列表。
- `getElementBTagName("标签属性")` 返回带有指定标签的所有元素节点。
- `getElementByClassName("class属性值")` 返回包含带有指定类名的所有元素的节点列表。
- `appendChild(节点元素)` 将新的节点添加到指定的节点。
- `removeChild(节点元素)` 删除子节点。
- `replaceChild(新节点元素,旧节点元素)` 替换子节点。
- `insertBefore()` 在指定的子节点前面插入新的子节点。
- `createAttribute("属性","value")` 创建属性节点。
- `createElement("标签名")` 创建元素节点。
- `createTextNode("文本内容")` 创建文本节点。
- `getAttribute()` 返回指定的属性值。
- `setAttribute("属性","value")` 将指定属性或修改为指定的值。

## 1.2 HTML DOM属性
> 属性是节点（HTML元素）的值，能够获取或设置。
- `innerHTML` 获取对应元素的内容信息。
  - 使用 innerHTML 获取对应的文本内容：selector.innerHTML;
  - 使用 innerHTML 重新设置对应的文本内容：selector.innerHTML="张世林";
- `nodeName` nodeName 属性规定节点的名称。
  - nodeName 是只读的
  - 元素节点的 nodeName 与标签名相同
  - 属性节点的 nodeName 与属性名相同
  - 文本节点的 nodeName 始终是 #text
  - 文档节点的 nodeName 始终是 #document
- `nodeValue` 属性规定节点的值。
  - 元素节点的 nodeValue 是 undefined 或 null
  - 文本节点的 nodeValue 是文本本身
  - 属性节点的 nodeValue 是属性值
- `nodeType` 属性
  - nodeType 属性返回节点的类型。nodeType 是只读的。

| 元素类型 | NodeType
|-----|-----|
| 元素 | 1 |
| 属性 | 2
| 文本 | 3
| 注释 | 8
| 文档 | 9

## 1.3 改变HTML内容
- `selector.innerHTML = "new text!"` 使用 innerHTML实现给对应的标签附新值。
- `selector.style.color="red"` 修改对应的元素的样式。

## 1.4 节点元素
```html
<script>
    //创建一个p节点
    var ps = document.createElement("p");
    //创建一个文本数据
    var data = document.createTextNode("this is a message");
    //将文本数据添加到创建的p节点中
    ps.appendChild(data)

    //将创建的p节点加入到div最后面
    document.getElementById("mydiv").appendChild(ps)

    //删除节点
    var divs = document.getElementById("mydiv")
    var thep =document.getElementById("mya")
    divs.removeChild(thep,ps)
</script>    
```

## 1.4事件
### 1.4.1 onClick事件
- `onclick="this.innerHTML='hello!'` 为对应的标签创建点击事件。
- `onclick="changetext(this)` 为标签绑定事件，并将this当前对象传入到方法中。
- `onclick="displayDate()` 为标签单纯的绑定一个事件。
- `document.getElementById("myBtn").onclick=function(){displayDate()};` 使用js为标签绑定事件。

### 1.4.2 onload、onunload事件
> 当用户进入或者离开页面时，会触发该事件。
> onload事件可用于检查访客的浏览器类型和版本，用于加载不同的网页。
> onload与onunload方法可用于检查cookie。
```html
    <body onload="checkCookies()" unonload="list()">
```

### 1.4.3 onChange事件
```html
<input type="text" id="fname" onchange="upperCase()">
```

### 1.4.4 onmouseover 和 onmouseout 事件
```html

```




# JavaScript 语言
## 1. JS对象
### 1.1 JS数据类型
- `string` 字符串类型
- `number` 数值类型
- `Undefined` 未定义类型
- `obejct` 对象类型
- `Null` Null数据类型
- `""` 空值类型

### 1.2 获取数据类型
`typeof` 获取对象的类型。一下是其返回类型：
  - string
  - number
  - boolean
  - undefeined

### 1.3 对象的比较
- `typeof undefined` 值为undefined。
- `typeof null` 值为object。
- `null === undefined` false
- `null == undefined` true

## 2. === 与 == 的区别
- `===` 对象类型和对象的值都相等，才返回true。
- `==` 对象的值相等，就返回true。

## 3. string字符串
### 3.1 string创建方式
- `var str1 = "张世林";` 直接赋值的方式创建。
- `var str2 = new String("张世林");` 以创建对象的方式创建值。

### 3.2 字符串的比较
- `typeof str1` 返回 string
- `typeof str2` 返回 object
- `str1 === str2` 返回false， 因为对象不同
- `str1 == str2` 返回true，因为值相等

### 3.3 字符串的方法
- `indexOf("regex", startIndex)` 根据regex查找第一个相匹配的数据，返回索引值。如果没有匹配到，则返回 -1；
  - Regex：表达式（可以为正则表达式）。
  - startIndex：从字符串该下标开始扫描。

- `lastIndexOf("regex", startIndex)` 根据regex查找最后一个相匹配的数据，返回索引值。如果没有匹配到，则返回 -1；
  - Regex：表达式（可以为正则表达式）。
  - startIndex：从字符串该下标开始扫描。

- `search("regex")` 跟indexOf()方法一致（无法使用正则表达式）。
- `slice(startindex, endindex)` 提取字符串的某一个部分，并返回新的字符串。
- `substring(startindex, endindex)` 提取字符串的某一个部分，并返回新的字符串。
- `substr(startindex, length)` 从该下标中获取长度为length的数据。
- `replace()` 用于替换字符串中指定的值。
- `toUpperCase()` 将字符串转换为大写。
- `toLowerCase()` 将字符串转换为小写。
- `concat()` 字符串连接。
- `String.trim()` 去除字符串两端的空白符。
- `String.charAt(index)` 返回对应下标的字符。
- `String.charCodeAt(index)` 返回对应下标的字符的Unicode编码。
- `String.split("表达式")` 根据表达式将字符串转换为数组。

## 4 var、let、const
- **`var`** 创建变量，具有函数作用于与全局作用域两种。
- **`let`** 创建变量：
  - 在函数中创建变量，则属于 函数作用域；
  - 在全局中创建变量，则属于 全局作用域；
    - var创建的全局变量，属于window对象，可以使用window.xxx 获取变量的值。
    - let创建的全局变量，不属于window对象，只能直接使用 xxx 获取变量的值。
  - 在代码块中创建变量，则属于块作用域；代码块作用域不会影响原本申明的变量的值。
```html
<script>
    var x = 10;
    // 此处 x 为 10
    { 
    var x = 6;
    // 此处 x 为 6
    }
    // 此处 x 为 6


    let i = 7;
    for (let i = 0; i < 10; i++) {
    // 一些语句
    }
    // 此处 i 为 7
</script>
```
- **`const`** 创建一个常量对象引用，无法修改其引用对象，但是能够修改引用对象中的属性值。

## 5. 最佳实践
### 5.1 开启debug模式
> 在需要开启debug的位置输入： **`debugger;`**，打开控制台即可实现代码的停止。

### 5.2 变量最佳设置
- 请使用 {} 来代替 new Object()
- 请使用 "" 来代替 new String()
- 请使用 0 来代替 new Number()
- 请使用 false 来代替 new Boolean()
- 请使用 [] 来代替 new Array()
- 请使用 /()/ 来代替 new RegExp()
- 请使用 function (){}来代替 new Function()

### 5.3 函数闭包
```html
<script>
var add = (function () {
    var counter = 0;
    return function () {return counter += 1;}
})();

add();
add();
add();

// 计数器目前是 3 
</script>
```
> 变量 add 的赋值是自调用函数的返回值。  
> 这个自调用函数只运行一次。它设置计数器为零（0），并返回函数表达式。  
> 这样 add 成为了函数。最“精彩的”部分是它能够访问父作用域中的计数器。  
> 这被称为 JavaScript 闭包。它使函数拥有“私有”变量成为可能。  
> 计数器被这个匿名函数的作用域保护，并且只能使用 add 函数来修改。  
> 闭包指的是有权访问父作用域的函数，即使在父函数关闭之后。  

## 6 JavaScript window--浏览器对象模型
> 浏览器对象模型（Browser Object Model(BOM)），允许JavaScript与浏览器进行对话。

### 6.1 window对象
- 所有浏览器都支持window对象。
- 所有的全局JavaScript对象，函数和变量自动成为window对象的成员。
- 全局变量是window对象中的属性。
- 全局函数是window对象的方法。
- document也是window对象的属性。
```html
<script>
  window.document.getElementById("myp").innerHTML;
</script>
```
#### 6.1.1 浏览器窗口
> 浏览器窗口尺寸使用的是window的两个属性；
- `window.innerHeight` 返回浏览器窗口的内高度。
- `window.innerWidth` 返回浏览器窗口的内宽度。

> 操作window窗口的方法：
- `window.open()` 打开新窗口。
- `window.close()` 关闭新窗口。
- `window.moveTo()` 移动当前窗口。
- `window.resizeTo()` 重新调整当前窗口。

### 6.1.2 window Screen 
> window Screen对象包含了用户屏幕的信息。
- `window.screen.width` 屏幕的宽度（像素）。
- `window.screen.height` 屏幕的高度（像素）。
- `window.screen.availWidth` 屏幕的可用宽度（像素）。
- `window.screen.availHeight` 屏幕的可用高度（像素）。
- `window.screen.colorDepth` 返回用于显示一种颜色的比特数（显示以位计的屏幕色彩深度）。
  - 24bits = 16,777,216种不同的“True Colors”。
  - 32bits = 4,294,967,296种不同的“Deep Colors”。
- `window.screen.pixelDepth` 返回屏幕的像素深度。

### 6.1.3 window Location
> window Location 用于返回当前页面的URL、主机域名等数据。
- `window.location.href` 返回当前页面的href。
- `window.location.hostname` 返回web主机的域名。
- `window.location.pathname` 返回当前页面的路径或文件名。
- `window.location.protocol` 返回使用的web协议（http或https）。
- `window.location.assign(URL)` 加载新文档（也就是加载新的页面）。
- `window.location.port` 加载当前互联网主机端口号。

### 6.1.4 window History
> window history对象保安浏览器历史。
- `window.history.back()` 加载历史列表中前一个URL，等同于在浏览器点击后退按钮。
- `window.history.forward()` 加载历史列表中下一个URL，等同于在浏览器中点击前进按钮。
- `window.history.go(number)` 
  - number = -1，表示返回上一个界面。
  - number = -2，表示返回上上个界面。

### 6.1.5 window Navigator
> window Navigator对象包含有关访问者的信息。
- `window.navigator.cookieEnabled` 
  - true：表示cookie已启用。
  - false：表示cookie未启用。
- `window.navigator.appName` 返回浏览器的应用程序名称。
- `window.navigator.appCodeName` 返回浏览器的应用程序代码名称。
- `window.navigator.product` 返回浏览器引擎的产品名称。
- `window.navigator.platform` 返回浏览器平台。
- `window.navigator.language` 返回浏览器语言。

### 6.1.6 window 弹出框
- `window.alert()` 警告框。
- `window.confirm()` 确认框
  - true：点击确认。
  - false：点击取消。
- `window.prompt()`提示框。

### 6.1.7 window Time事件
> window time事件，就是定时事件。
- `window.setTimeOut(function, milliseconds)` 多少时间以后，执行该函数。
  - function：需要执行的函数。
  - millseconds：设置函数延迟执行的时间，单位为毫秒。
- `window.clearTimeout(定时事件对象)` 对对应的定时事件对象进行停止执行的操作。
```html
<script>
myVar = setTimeout(function, milliseconds);
clearTimeout(myVar);
</script>
```
- `window.setInterval(function, milliseconds);` 方法在指定的时间间隔重复执行给定的函数。
  - function：需要执行的函数。
  - milliseconds：函数执行的时间间隔（单位为毫秒）。
- `window.clearInterval(timerVariable)` 停止执行对应的定时任务。
```html
<p id="demo"></p>
<button onclick="clearInterval(myVar)">停止时间</button>
<script>
var myVar = setInterval(myTimer, 1000);
 function myTimer() {
    var d = new Date();
    document.getElementById("demo").innerHTML = d.toLocaleTimeString();
}
</script>
```

### 6.1.8 window cookie
> Cookie 是在您的计算机上存储在小的文本文件中的数据。
- `var cookies = window.document.cookie` 获取cookie。
- 删除cookie：直接将cookie的日期设置为一个过期时间即可。
- 创建cookie： `document.cookie = "username=Bill Gates; expires=Sun, 31 Dec 2017 12:00:00 UTC; path=/";`

- 设置cookie的函数：
```html
<script>
//cookie 的名字（cname），cookie 的值（cvalue），以及知道 cookie 过期的天数（exdays）。
function setCookie(cname, cvalue, exdays) {
    var d = new Date();
    d.setTime(d.getTime() + (exdays*24*60*60*1000));
    var expires = "expires="+ d.toUTCString();
    document.cookie = cname + "=" + cvalue + ";" + expires + ";path=/";
} 
</script>
```
- 获取cookie的函数：
```html
<script>
function getCookie(cname) {
    var name = cname + "=";
    var decodedCookie = decodeURIComponent(document.cookie);
    var ca = decodedCookie.split(';');
    for(var i = 0; i <ca.length; i++) {
        var c = ca[i];
        while (c.charAt(0) == ' ') {
            c = c.substring(1);
         }
         if (c.indexOf(name) == 0) {
            return c.substring(name.length, c.length);
         }
     }
    return "";
} 
</script>
```