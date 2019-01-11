# beego学习笔记
## 一、beego简介
### 1、beego基本介绍
beego是一个作用于Golang语言的一个Web框架，能够简化Golang语言对Web的开发，避免重复造轮子的过程，相当于java中的SpringMVC框架。

### 2、beego框架特性
- 简单化：RESTful 支持、MVC 模型，可以使用 bee 工具快速地开发应用，包括监控代码修改进行热编译、自动化测试代码以及自动化打包部署。

- 智能化：支持智能路由、智能监控，可以监控 QPS、内存消耗、CPU 使用，以及 goroutine 的运行状况，让您的线上应用尽在掌握。
- 模块化：beego 内置了强大的模块，包括 Session、缓存操作、日志记录、配置解析、性能监控、上下文操作、ORM 模块、请求模拟等强大的模块，足以支撑你任何的应用。
- 高性能：beego 采用了 Go 原生的 http 包来处理请求，goroutine 的并发效率足以应付大流量的 Web 应用和 API 应用，目前已经应用于大量高并发的产品中。

## 二、beego环境搭建
### 第一步：克隆源码
安装了Git以后，直接进入Git Push页面，依次输入地址：
```
go get -u github.com/astaxie/beego

go get -u github.com/beego/bee
```

### 第二步：测试是否安装成功
在命令行窗口先输入命令：在GOPATH路径下创建一个project1的web项目
```
bee new project1
```

再进入project1项目，在命令行窗口输入命令：即可启动这个项目
```
bee run
```

最后使用浏览器访问这个项目：输入网址即可
```
localhost:8080
```

## 三、beego路由设置
### 1、路由设置官方文档
https://beego.me/docs/mvc/controller/router.md

### 2、路由基本设置
```
为了用户更加方便的路由设置，beego 参考了 sinatra 的路由实现，支持多种方式的路由：

beego.Router(“/api/?:id”, &controllers.RController{})
默认匹配 //例如对于URL”/api/123”可以匹配成功，此时变量”:id”值为”123”

beego.Router(“/api/:id”, &controllers.RController{})
默认匹配 //例如对于URL”/api/123”可以匹配成功，此时变量”:id”值为”123”，但URL”/api/“匹配失败

beego.Router(“/api/:id([0-9]+)“, &controllers.RController{})
自定义正则匹配 //例如对于URL”/api/123”可以匹配成功，此时变量”:id”值为”123”

beego.Router(“/user/:username([\\w]+)“, &controllers.RController{})
正则字符串匹配 //例如对于URL”/user/astaxie”可以匹配成功，此时变量”:username”值为”astaxie”

beego.Router(“/download/*.*”, &controllers.RController{})
*匹配方式 //例如对于URL”/download/file/api.xml”可以匹配成功，此时变量”:path”值为”file/api”， “:ext”值为”xml”

beego.Router(“/download/ceshi/*“, &controllers.RController{})
*全匹配方式 //例如对于URL”/download/ceshi/file/api.json”可以匹配成功，此时变量”:splat”值为”file/api.json”

beego.Router(“/:id:int”, &controllers.RController{})
int 类型设置方式，匹配 :id为int 类型，框架帮你实现了正则 ([0-9]+)

beego.Router(“/:hi:string”, &controllers.RController{})
string 类型设置方式，匹配 :hi 为 string 类型。框架帮你实现了正则 ([\w]+)

beego.Router(“/cms_:id([0-9]+).html”, &controllers.CmsController{})
带有前缀的自定义正则 //匹配 :id 为正则类型。匹配 cms_123.html 这样的 url :id = 123


可以在 Controller 中通过如下方式获取上面的变量：

this.Ctx.Input.Param(":id")
this.Ctx.Input.Param(":username")
this.Ctx.Input.Param(":splat")
this.Ctx.Input.Param(":path")
this.Ctx.Input.Param(":ext")
```

