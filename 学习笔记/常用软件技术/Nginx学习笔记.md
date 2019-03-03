[TOC]
# Nginx 学习笔记
## 一、Nginx优势
Nginx 是一个高性能的HTTP和反向代理服务器，也是一个IMAP/POP3/SMTP服务器。  
- 高新更的 HTTP Server，解决了C10K的问题(也就是说可以同时一万人访问)。
- 高性能的**反向代理**服务器，给网站加速。
- 作为LB集群前端的一个**负载均衡**器。
- nginx采用的是**epoll**模型，能够实现**异步、非阻塞**的操作。

**1）IO多路复用 I/O multiplexing**    

**IO多路复用模型：** 
> 在Nginx中使用单个线程通过**epoll**来记录跟踪每一个IO流的状态来同时管理多个IO流。相当于多个请求过来，使用一个进程来对其进行控制，然后通过epoll像拨开关的方式来分别对应某一个线程来对其进行响应。  
- 第一种方法就是最传统的多进程并发模型 (每进来一个新的I/O流会分配一个新的进程管
理。)   
- 第二种方法就是I/O多路复用 (单个线程，通过记录跟踪每个I/O流(sock)的状态，来同时管理
多个I/O流 。)


## 二、Nginx 安装
### 1、安装方式一：
1. 创建一个repo文件----输入命令： vim /etc/yum.repos.d/nginx.repo  
2. 在创建的文件中添加内容:
```
[nginx]
name=nginx repo
baseurl=http://nginx.org/packages/centos/7/$basearch/
gpgcheck=0
enabled=1
```
3. 查看nginx的yum仓库是否存在----输入命令：yum list nginx  
4. 安装nginx----输入命令： yum install -y nginx
5. 安装其他软件----输入命令： yum install pcre pcre-devel -y
6. 启动nginx(什么位置都可以启动)----输入命令： systemctl start nginx
7. 开机启动nginx----输入命令： systemctl enable nginx
8. 查看nginx版本----输入命令： nginx -V  

### 2、安装方式二：
1. 上传 nginx-1.10.0.tar.gz 文件到 /usr/local 目录中。
2. 安装依赖文件：yum install gcc-c++ pcre pcre-devel zlib-devel openssl openssl-devel -y  
3. 解压 nginx-1.10.0.tar.gz 文件，并进入到 nginx-1.10.0 目录中。
4. 执行命令： ./configure
5. 继续执行命令： make
6. 继续执行命令： make install 
7. 进入到安装的nginx目录 --- /usr/local/nginx
8. 先进入到目录： /usr/local/nginx/sbin
9. 启动nginx： ./nginx 
10. 重启nginx： ./nginx -s reload


## 三、nginx服务器

#### 1、简介：
nginx能够作为一个服务器，来提供http的服务功能。服务器的资源能够通过http方式来进行请求获取。

#### 2、配置案例

操作案例：访问nginx服务器里面配置的资源

1）在nginx的安装目录中，找到 conf 目录。 

2）修改 nginx.conf 文件，修改内容为：
```nginx
    location / {
        root   /opt/temp/miages;
        index  index.html index.htm;
    }

```
3）重启 nginx，输入命令--- ./nginx -s reload 
4）访问服务器资源，比如访问刚配置的location目录下的图片：  
```
    192.168.21.149/P001.jpg
```


### 3、nginx作反向代理
**案例一：使用tomcat来实现两个tomcat的反向代理。**
1）创建两个tomcat，端口分别为 8080 与 8081 。  

2）修改nginx配置文件，在安装目录下的conf目录。例如：/usr/local/nginx/conf/nginx.conf  

3）修改内容：创建多个 server 来反向代理多个tomcat。在最后一个大括号之前添加内容：
```nginx
   upstream tomcatserver1{
       server 192.168.21.149:8080;
    }

    upstream tomcatserver2{
       server 192.168.21.149:8081;
    }

    server {
           listen    80;
           server_name  8080.zsl.com;

           location / {
                proxy_pass http://tomcatserver1;
                index index.html;
           }
    }

    server {
           listen    80;
           server_name  8081.zsl.com;

           location / {
                proxy_pass http://tomcatserver2;
                index index.html;
           }
    }
```

4）重启nginx。  

5）修改本地 hosts 文件： 
```nginx
192.168.21.149 8080.zsl.com
192.168.21.149 8081.zsl.com
```

6）启动两个tomcat。输入网址 8080.zsl.com 与 8081.zsl.com 即可访问这两个tomcat。  

**案例二：使用nginx来反向代理自己做的项目**  

1）修改nginx的配置文件，根据上个案例来进行配置。

2）启动自己做的项目。

3）浏览器中根据nginx配置的网址访问自己做的项目即可。

### 4、Nginx做负载均衡
**1、简介：**  
当相同的项目部署在了多台服务器上，当请求发送过来，则需要nginx来平均处理与转发请求给多台服务器。

##### 1）轮循算法

修改 nginx.conf 文件，添加内容：  

介绍：两个tomcat的请求地址都写在 upstream 中，然后由nginx默认执行**轮循算法**来循环给两个tomcat转发请求。  
```
upstream tomcatserver1{
       server 192.168.21.149:8080;
       server 192.168.21.149:8081;
    }

    server {
           listen    80;
           server_name  8080.zsl.com;

           location / {
                proxy_pass http://tomcatserver1;
                index index.html;
           }
    }

```

启动两个tomcat，输入网址： http://8080.zsl.com 即可。

##### 2）加权轮询

- 为项目指定负载权重---**down**，权重越大，负载就越大，默认权重为1:
```nginx
upstream tomcatserver1{
       server 192.168.21.149:8080;
       server 192.168.21.149:8081 weight=2;
    }
```

- 为项目指定备份项目---**backup**，当其他非backup项目都down或者忙的时候，才会将强求转发给backup对应的项目。
- 允许请求失败的次数---**max_fails**，允许请求失败的默认次数为1，当请求失败超过最大数以后，则返回 proxy_next_upstream 模块定义的错误。
- **fail_timeout**---请求超过最大数皆失败以后，暂停的时间。 
```nginx
upstream tomcatserver1{
       server 192.168.21.149:8080 weight=2 max_fails=2 fail_timeout=30s;
       server 192.168.21.149:8081 weight=2;
       server 192.168.21.149:8082 backup;
    }
```

##### 3）ip_hash轮询

每个请求按访问ip的hash结果分配，这样每个访客固定访问一个后端服务器，可以解决session的问题。 

```nginx
upstream backserver { 
	ip_hash; 
	server 192.168.0.14; 
	server 192.168.0.15; 
} 
```



### 5、Nginx基本配置

```nginx
#运行用户
user nobody;

#启动进程,通常设置成和cpu的数量相等
worker_processes  1;

#全局错误日志及PID文件
#error_log  logs/error.log;
#error_log  logs/error.log  notice;
#error_log  logs/error.log  info;

#pid        logs/nginx.pid;

#工作模式及连接数上限
events {
    #epoll是多路复用IO(I/O Multiplexing)中的一种方式,
    #仅用于linux2.6以上内核,可以大大提高nginx的性能
    use   epoll; 

    #单个后台worker process进程的最大并发链接数    
    worker_connections  1024;

    # 并发总数是 worker_processes 和 worker_connections 的乘积
    # 即 max_clients = worker_processes * worker_connections
    # 在设置了反向代理的情况下，max_clients = worker_processes * worker_connections / 4  为什么
    # 为什么上面反向代理要除以4，应该说是一个经验值
    # 根据以上条件，正常情况下的Nginx Server可以应付的最大连接数为：4 * 8000 = 32000
    # worker_connections 值的设置跟物理内存大小有关
    # 因为并发受IO约束，max_clients的值须小于系统可以打开的最大文件数
    # 而系统可以打开的最大文件数和内存大小成正比，一般1GB内存的机器上可以打开的文件数大约是10万左右
    # 我们来看看360M内存的VPS可以打开的文件句柄数是多少：
    # $ cat /proc/sys/fs/file-max
    # 输出 34336
    # 32000 < 34336，即并发连接总数小于系统可以打开的文件句柄总数，这样就在操作系统可以承受的范围之内
    # 所以，worker_connections 的值需根据 worker_processes 进程数目和系统可以打开的最大文件总数进行适当地进行设置
    # 使得并发总数小于操作系统可以打开的最大文件数目
    # 其实质也就是根据主机的物理CPU和内存进行配置
    # 当然，理论上的并发总数可能会和实际有所偏差，因为主机还有其他的工作进程需要消耗系统资源。
    # ulimit -SHn 65535

}


http {
    #设定mime类型,类型由mime.type文件定义
    include    mime.types;
    default_type  application/octet-stream;
    #设定日志格式
    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    access_log  logs/access.log  main;

    #sendfile 指令指定 nginx 是否调用 sendfile 函数（zero copy 方式）来输出文件，
    #对于普通应用，必须设为 on,
    #如果用来进行下载等应用磁盘IO重负载应用，可设置为 off，
    #以平衡磁盘与网络I/O处理速度，降低系统的uptime.
    sendfile     on;
    #tcp_nopush     on;

    #连接超时时间
    #keepalive_timeout  0;
    keepalive_timeout  65;
    tcp_nodelay     on;

    #开启gzip压缩
    gzip  on;
    gzip_disable "MSIE [1-6].";

    #设定请求缓冲
    client_header_buffer_size    128k;
    large_client_header_buffers  4 128k;


    #设定虚拟主机配置
    server {
        #侦听80端口
        listen    80;
        #定义使用 www.nginx.cn访问
        server_name  www.nginx.cn;

        #定义服务器的默认网站根目录位置
        root html;

        #设定本虚拟主机的访问日志
        access_log  logs/nginx.access.log  main;

        #默认请求
        location / {
            #定义首页索引文件的名称
            index index.php index.html index.htm;   

        }

        # 定义错误提示页面
        error_page   500 502 503 504 /50x.html;
        location = /50x.html {
        }

        #静态文件，nginx自己处理
        location ~ ^/(images|javascript|js|css|flash|media|static)/ {
            #过期30天，静态文件不怎么更新，过期可以设大一点，
            #如果频繁更新，则可以设置得小一点。
            expires 30d;
        }

        #PHP 脚本请求全部转发到 FastCGI处理. 使用FastCGI默认配置.
        location ~ .php$ {
            fastcgi_pass 127.0.0.1:9000;
            fastcgi_index index.php;
            fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
            include fastcgi_params;
        }
        #禁止访问 .htxxx 文件
        location ~ /.ht {
            deny all;
        }

    }
}
```



<<<<<<< HEAD
### Nginx配置静态资源文件

```nginx
	# nginx的自动配置
	upstream yienmall{
       server 127.0.0.1:8081;
    }

    server {
           listen    80;
           server_name  www.yienmall.com;

           location / {
                proxy_pass http://yienmall;
                index index.html;
           }


			location /webroot/static/ {
				expires 30d;
			}
    }
```
=======























>>>>>>> 1a04593b9900f152d33ebdd07f20647b4a434771

