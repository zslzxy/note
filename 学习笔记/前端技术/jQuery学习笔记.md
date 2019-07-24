[TOC]

### Maven命令将maven项目中依赖的jar全部拷贝到lib目录。 dependency:copy-dependencies -DoutputDirectory=lib



# jQuery学习笔记
## 1. jQuery选择器
| 选择器 | 实例 | 选取|
|:-----:|:-----:|:-----:|
| * | $("*") | 所有元素
| #id | $("#my_id") | id = my_id 的元素
| .class | $(".my_class") | class中有 my_class的元素
| .class,.class | $(".class1, .class2") | class中包含了 class1类 和 class2类的元素
| element | $("span") | 所有的span元素
| el1, el2 | $("span, p") | 所有的span和p元素
| :first | $("span:first") | 得到第一个span元素
| :last | $("span:last") | 得到最后一个span元素
| :even | $("tr:even") | 得到偶数的tr元素
| :odd | $("tr:odd") | 得到基数的tr元素
| :eq(index) | $("ul li:eq(3)") | 得到li元素的第四个元素
| :gt(num) | $("ul li:gt(3)") | 列举index大于3的元素
| :header | $(":header") | 所有的标题元素（h1、h2等）
| :contains(text) | $("contains('hello')") | 所有包含文本 hello 的元素。
| :empty | $(":empty") | 所有的空元素
| :hidden | $(":hidden") | 获取所有的被隐藏的元素 
| :visible | $("table:visible") | 获取所有可见的表格
| :input | $(":input") | 所有的input标签
| :text | $(":text") | 所有type=text 的input标签。
| :password :checkbox、:submit、:reset、:button | $(":password") | 所有type=对应类型 的input标签。
| :checked、:enabled、:selected、:disabled || 所有针对性操作的元素



## 2. jQuery事件
- `$(document).ready(function)`——将函数绑定到文档的就绪事件。
- `$(selector).click(function)` —— 触发或将函数绑定到被选元素的点击事件。
- `$(selector).dbclick(function)`——触发或将函数绑定到被选元素的双击事件。
- `$(selector).focus(function)`——触发或将函数绑定到被选元素的获得焦点事件。
- `$(selector).mouseover(function)`——触发获奖函数绑定到被选元素的鼠标悬停事件。


## 3. jQuery效果
### 3.1 显示、隐藏
- `$(selector).hide(speed, callback)` 隐藏对应的元素。
- `$(selector).show(speed, callback)` 显示对应的元素。
  - **speed**：（可选）显示、隐藏的速度，单位为毫秒。
  - **callback**：（可选）隐藏或显示完成以后需要还行的函数。

### 3.2 显示、隐藏切换
- `$(selector).toggle(speed,callback)` 用于显示与隐藏两种方式的切换。例如：
```html
    <button id="mybtn">切换</button>
    <div id="myp">
        <p>
            显示隐藏切换
        </p>
    </div>

    <script>
        $("#mybtn").click(function() {
            $("#myp").toggle(1000)
        })
    </script>
```
### 3.3 淡入、淡出
- `$(selector).fadeIn(speed, callback)` 用于淡入已隐藏的元素。
```html
    <button id="mybtn">切换</button>
    <div id="myp" hidden>
        <p>
            显示隐藏切换
        </p>
    </div>

    <script>
        $("#mybtn").click(function() {
            $("#myp").fadeIn(2000)
        })
    </script>
```
- `$(selector).fadeOut(speed,callback)` 用于淡出可见元素。
```html
    <button id="mybtn">切换</button>
    <div id="myp">
        <p>
            显示隐藏切换
        </p>
    </div>

    <script>
        $("#mybtn").click(function() {
            $("#myp").fadeOut(2000)
        })
    </script>
```
- `$(selector).fadeToggle(speed, callback)` 该方法用于fadeIn与fadeOut之间的切换。
  - 如果元素已经淡出，则会向元素添加淡入效果。
  - 如果元素已经淡入，则会向元素添加淡出效果。
```html
    <button id="mybtn">切换</button>
    <div id="myp">
        <p>
            显示隐藏切换
        </p>
    </div>

    <script>
        $("#mybtn").click(function() {
            $("#myp").fadeToggle(2000)
        })
    </script>
```
- `$(selector).fadeTo(speed,opacity,callback)` 允许渐变为给定的不透明度。
  - **opacity**：设置不透明度，值在0-1之间。
```html
    <button id="mybtn">切换</button>
    <div id="myp" hidden>
        <p>
            显示隐藏切换
        </p>
    </div>

    <script>
        $("#mybtn").click(function() {
            $("#myp").fadeTo(2000,0.3)
        })
    </script>
```
### 3.4 滑动
- `$(selector).slideDown(speed,callback)` 用于向下滑动元素(也就是将该元素从上往下展开)。
```html
    <button id="mybtn">切换</button>
    <div id="myp" hidden>
        <p>显示隐藏切换</p>
        <p>显示隐藏切换</p>
        <p>显示隐藏切换</p>
    </div>

    <script>
        $("#mybtn").click(function() {
            $("#myp").slideDown(2000)
        })
    </script>
```
- `$(selector).slideUp(speed, callback)` 用于向上滑动元素（也就是将该元素从下网上展开）。
- `$(selector).slideToggle(speed, callback)` 用于向上、向下间替地滑动元素。
```html
    <div id="myp" >
        <p>显示隐藏切换</p>
        <p>显示隐藏切换</p>
        <p>显示隐藏切换</p>
    </div>
    <button id="mybtn">切换</button>


    <script>
        $("button").click(function(){
            //让元素先上后下
            $("div").slideDown(2000).slideUp(2000)
        })
    </script>
```

### 3.5 动画
- `$(selector).animate({params}, speed, callback)` 用于创建元素动画。
  - **params**：（必填）参数定义形成动画的CSS属性。
```javaScript
    <script>
        $("button").click(function(){
            $("div").animate({
                left:'250px',
                opacity:'0.5',
                height:'150px',
                width:'150px'
            });
        }); 
    </script>
```
### 3.6 停止动画
- `$(selector).stop(stopAll, goToEnd)` 用于停止动画。
  - **stopAll**：（可选）是否清除动画队列，默认为false。
  - **GoToEnd**：（可选）是否立即完成当前动画，默认为false。


## 4. jQuery DOM
DOM，全称 Document Object Model，文档对象模型。
### 4.1 获取、设置内容
- text()——设置或返回所选元素的文本内容。
- html()——设置或返回所选元素的内容（包括HTML标记）。
- val()——设置或返回表单字段的值。
- text(data)——将data文本赋值到元素中
- html(data)——将data文本赋值到元素中（包含HTML标记）
- val(data)——赋值表单字段的值
- text(callback)
- html(callback)
- val(callback)

### 4.2 获取、设置属性
- `$(selector).attr(标签)`——获取对应元素的标签的属性。 
```html
    <script>
        $("button").click(function(){
            //让元素先上后下
            console.log($("div").attr("style"))
        })
    </script>
```
- `$(selector).attr(标签,属性)`——为对应元素的标签添加属性。
```html
<script>
    $("button").click(function(){
        //让元素先上后下
        $("div").attr("style", "margin-top: 500px")
    })
</script>
```

### 4.3 添加元素
- `$(selector).append("some thing")` 在被选元素中的后面插入内容。
```html
<div id="myp" style="margin-top: 20px;color:blue">
    <p>已有元素</p>

</div>
<button id="mybtn">切换</button>

<script>
    $("button").click(function(){
        //让元素先上后下
        $("div").append("张世林大帅哥")
    })
</script>
```
- `$(selector).prepend("some thing")` 在被选元素中的开头插入内容。
- `$(selector).after("some thing")` 在被选元素之后插入内容（也就是在该元素范围外最开始的地方插入元素）。
- `$(selector).before("some thing")` 在被选元素之前插入内容（也就是在该元素范围外最后的地方插入元素）。

### 4.4 删除元素
- `$(selector).remove()` 删除被选元素（及其子元素）。
- `$(selector).empty()` 从被选元素中删除子元素。

### 4.5 操作CSS类
- `$(selector).addClass("类名")` 向被选元素添加对应的class类。
```html
<script>
    $("#tab_btn").click(function() {
        $("#tab tbody tr td").addClass("blue")
    })
</script>
```
- `$(selector).removeClass("属性")` 从被选元素中移除对应的class属性。
- `$(selector).toggleClass("属性")` 添加属性与移除属性的方法之间切换。

### 4.6 操作css()方法
- `$(selector).css("propertoryName")` 获取被选元素对应css样式的属性值。
```html
<script>
    alert($("#datap").css("color"))
</script>
```
- `$(selector).css({"propertoryName": "value"})` 设置被选元素对应的css样式，可以多对象设置。

### 4.7 获取、设置元素尺寸
- `$(selector).width()` 设置或返回元素的宽度（不包括内边距、边框和外边框）。
- `$(selector).height()` 设置或返回元素的高度（不包括内边距、边框和外边框）。
- `$(selector).innerWidth()` 设置或返回元素的宽度（包括内边距）。
- - `$(selector).innerHeight()` 设置或返回元素的高度（包括内边距）。
- `$(selector).outerWidth()` 设置或返回元素的宽度（包括内边距、边框）。
- `$(selector).outerHeight()` 设置或返回元素的高度（包括内边距、边框）。
  
## 5 jQuery遍历
### 5.1 遍历-祖先
> 从子元素往上进行遍历，查找对应的父级元素或祖先元素。
- `$(selector).parent()` 获取被选中元素的父级元素。
```html
<script>
    $(document).ready(function(){
        $("span").parent();
    });
</script>
```
- `$(selector).parents()` 返回被选元素的所有祖先元素，它一路向上到文档的根元素（<thml>）。
```html
<script>
    //查找到父级元素中的 ul 元素
    console.log($(this).parents("ul").text())
    //一直查找到 html文档 元素
    console.log($(this).parents().text())
</script>    
```
- `$(selector).parentsUtil()` 返回对应两个标签之间的文档内容。

### 5.2 遍历-后代
> 从父元素向下扫描，得到对应的后代元素。
- `$(selector).children()` 返回被选元素的所有直接子元素（只会向下一级的DOM树进行遍历）。
```html
<script>
    //为div下一级元素添加css样式
    $("div").children().css({"color":"red","border":"2px solid red"});

    //查找到下一级元素span中含有类名为 blue 的孩子
    $("#thisp").children("span.blue").text()
</script>    
```
- `$(selector).find()` 方法返回被选的后代元素，一路向下直到最后一个后代。
```html
<script>
    //得到子元素中的所有span元素
    $("#thisp").find("span").text()
    //得到所有子元素
    $("#thisp").find("*").text()
</script>
```

### 5.3 遍历-同胞
> 跟当前被选元素同一级的素有元素。
- `$(selector).siblings()` 返回被选元素的所有同胞元素。
```html
<script>
    //得到所有的同级元素
    $("span").siblings().text()

    //得到所有同级元素中的所有 p 标签元素
    $("span").siblings("p").text()
</script>
```
- `$(selector).next()` 返回被选中元素的下一个同胞元素。
```html
<script>
    $(".blue").next().text()
</script>
```

- `$(selector).nextAll()` 返回被选中元素后面的所有同胞元素。
```html
<script>
    $(".blue").nextAll().text()
</script>
```

- `$(selector).nextUnit()` 返回被选中元素与标签中的元素之间的所有同胞元素。
```html
<script>
    $(".blue").nextUnit("span").text()
</script>
```
- `$(selector).prev()` 返回被选中元的素同级元素的前一个元素。
- `$(selector).prevAll()` 返回被选中元素前面的所有同级元素。
- `$(selector).prevUnit()` 返回被选中元素与标签间的前面的所有元素。

### 5.4 遍历-过滤
- `$(selector).first()` 返回被选中元素的首个元素。
```html
<script>
    //得到div中的第一个 p 元素
    $("div p").first()
</script>    
```
- `$(selector).last()` 返回被选中元素的首个元素。
- `$(selector).eq(num)` 返回被选中元素中带有指定索引号的元素。
- `$(selector).filter(元素)` 从被选中的元素中进行过滤，留下能够匹配上filter的元素。
```html
<script>
//得到所有span中类名包含了 ss 的元素
$("#thisp span").filter(".ss").text()
</script>
```
- `$(selector).not(元素)` 从被选中的元素中进行过滤，留下不能匹配上元素的所有元素。
```html
<script>
//得到所有span中类名不包含 ss 的元素
$("#thisp span").not(".ss").text()
</script>
```

## 6. jQuery Ajax
> AJAX = 异步JavaScript和XML（Asynchronous JavaScript and XML）。其特性是在不重新加载整个网页的情况下， AJAX通过向后台发送请求加载数据，并在网页中进行显示。
- `$(selector).load(URL, data, callback)` 从服务器中加载数据，并将返回的数据放到被选中的元素中。
```html
<script>
    $(".thisli").click(function() {
       $(this).load("http://localhost:8080/goods_order_war_exploded/list.action")
    })
</script>
```
- `$.get(URL,callback)` 发送get请求，请求完成以后执行回调函数。
```html
<script>
    $("button").click(function(){
    $.get("demo_test.php",function(data,status){
        alert("数据: " + data + "\n状态: " + status);
    });
    });
</script>
```
- `$post(URL, data, callback)` 发送post请求，可以携带数据。
```html
<script>
    $("button").click(function(){
    $.post("demo_test_post.html",
    {
        name:"Donald Duck",
        city:"Duckburg"
    },
    function(data,status){
        alert("Data: " + data + "nStatus: " + status);
    });
    });
</script>
```
- `$.ajax({name:value, name:value, ... })` ajax方法用于执行发送请求的操作。


| 名称        | 值    | 描述  |
| --------   | -----:   | :-----: |
| async        | 默认值为true      |  布尔值，表示请求是否异步处理    |
| beforeSend(xhr)  |    |  发送请求前运行的函数。|
|complete(xhr,status) |   | 请求完成时运行的函数(成功失败都会执行) |
|contentType |  "application/x-www-form-urlcoded"  |  发送数据到服务器时所使用的的内容格式 |
| context	| 的 |为所有 AJAX 相关的回调函数规定 "this" 值。
| data|  |	规定要发送到服务器的数据。
|dataFilter(data,type)||	用于处理 XMLHttpRequest 原始响应数据的函数。
|dataType	||预期的服务器响应的数据类型。
|error(xhr,status,error)||	如果请求失败要运行的函数。
|global	||布尔值，规定是否为请求触发全局 AJAX 事件处理程序。默认是 true。
|ifModified	| |布尔值，规定是否仅在最后一次请求以来响应发生改变时才请求成功。默认是 false。
|jsonp||	在一个 jsonp 中重写回调函数的字符串。
|jsonpCallback	||在一个 jsonp 中规定回调函数的名称。
|password	||规定在 HTTP 访问认证请求中使用的密码。
|processData	||布尔值，规定通过请求发送的数据是否转换为查询字符串。默认是 true。
|scriptCharset||	规定请求的字符集。
|success(result,status,xhr)||	当请求成功时运行的函数。
|timeout||	设置本地的请求超时时间（以毫秒计）。
|traditional||	布尔值，规定是否使用参数序列化的传统样式。
|type||	规定请求的类型（GET 或 POST）。
|url||	规定发送请求的 URL。默认是当前页面。
|username||	规定在 HTTP 访问认证请求中使用的用户名。
|xhr||	用于创建 XMLHttpRequest 对象的函数。

## 7. jQuery数组
### 7.1 定义数组
#### 7.1.1 定义一维数组
```html
<script>
//创建一个空数组
var array = new Array()

//创建一个非空数组
var array = new Array(6,3,8,7)
var array = [1,2,3,4,5]
</script>
```

#### 7.1.2 定义二维数组
```html
<script>
var i = 0;
var arr = new Array()
arr[i++] = new Array()
</script>
```

### 7.2 遍历数组
```html
<script>
//遍历方式一
for(var i = 0; i < arr.length; i++) {
    alert(arr[i])
}

//遍历方式二
for(var e in arr) {
    alert(e)
}
</script>
```
### 7.3 数组常用函数
- `concat(...items: ConcatArray<T>[]): T[];` 进行数组的拼接功能，返回新的数组。
```html
<script>
    var arr1 = new Array(1,2,3)
    var arr2 = new Array(4,5,6)
    var arr3 = new Array(7,8,9)
    alert(arr1.concat(arr2,arr3,arr1))
</script>
```

- `join("分隔符")` 将数组拼接为一个字符串，每个字符串都是使用分隔符进行分隔。
```html
<script>
    var arr1 = new Array(1,2,3)
    alert(arr1.join(","))
</script>
```

- `pop()` 删除数组中最后一个元素，并返回。
- `push(arr[])` 网数组后面添加新的数组，不会返回新的数组，而是创建数组。
- `reverse()`颠倒数组中元素的顺序。
- `shift()`删除并返回数组的第一个元素
- `slice()`从某个已有的数组返回选定的元素
- `sort()`对数组的元素进行排序splice()删除元素，并向数组添加新元素。
- `toSource()`返回该对象的源代码。toString()把数组转换为字符串，并返回结果。
- `toLocaleString()`把数组转换为本地数组，并返回结果。
- `unshift()`向数组的开头添加一个或更多元素，并返回新的长度。
- `valueOf()`返回数组对象的原始值。
