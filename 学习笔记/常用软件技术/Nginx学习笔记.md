[toc]
# Nginx 学习笔记
## Nginx优势
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


## Nginx 安装
### 方式一：
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
```













```




## Nginx简单学习笔记
### Nginx的安装：
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


### nginx作为http服务器

#### 1、简介：
nginx能够作为一个服务器，来提供http的服务功能。服务器的资源能够通过http方式来进行请求获取。

#### 2、配置案例--访问nginx服务器里面配置的资源
1）在nginx的安装目录中，找到 conf 目录。 

2）修改 nginx.conf 文件，修改内容为：
```
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


### nginx作反向代理
**案例一：使用tomcat来实现两个tomcat的反向代理。**
1）创建两个tomcat，端口分别为 8080 与 8081 。  

2）修改nginx配置文件，在安装目录下的conf目录。例如：/usr/local/nginx/conf/nginx.conf  

3）修改内容：创建多个 server 来反向代理多个tomcat。在最后一个大括号之前添加内容：
```
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
```
192.168.21.149 8080.zsl.com
192.168.21.149 8081.zsl.com
```

6）启动两个tomcat。输入网址 8080.zsl.com 与 8081.zsl.com 即可访问这两个tomcat。  


**案例二：使用nginx来反向代理自己做的项目**  

1）修改nginx的配置文件，根据上个案例来进行配置。

2）启动自己做的项目。

3）浏览器中根据nginx配置的网址访问自己做的项目即可。

### Nginx做负载均衡
**1、简介：**  
当相同的项目部署在了多台服务器上，当请求发送过来，则需要nginx来平均处理与转发请求给多台服务器。

**案例：来将两个tomcat进行负载均衡-----轮循算法**   

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

2）启动两个tomcat，输入网址： http://8080.zsl.com 即可。

**其他负载均衡操作；**   

- 让一个项目暂时不参与负载均衡---**weight**：
```
upstream tomcatserver1{
       server 192.168.21.149:8080 down;
       server 192.168.21.149:8081;
    }
```  

- 为项目指定负载权重---**down**，权重越大，负载就越大，默认权重为1:
```
upstream tomcatserver1{
       server 192.168.21.149:8080;
       server 192.168.21.149:8081 weight=2;
    }
```

- 为项目指定备份项目---**backup**，当其他非backup项目都down或者忙的时候，才会将强求转发给backup对应的项目:
```
upstream tomcatserver1{
       server 192.168.21.149:8080;
       server 192.168.21.149:8081 weight=2;
       server 192.168.21.149:8082 backup;
    }
```

- 允许请求失败的次数---**max_fails**，允许请求失败的默认次数为1，当请求失败超过最大数以后，则返回 proxy_next_upstream 模块定义的错误。

- **fail_timeout**---请求超过最大数皆失败以后，暂停的时间。 

