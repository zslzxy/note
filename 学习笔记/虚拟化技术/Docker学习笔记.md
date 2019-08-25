# Docker学习笔记

[TOC]

### docker的安装
#### 步骤：
1.从网络下载docker： yum install docker

2.启动docker：systemctl start docker 

3.设置docker开机自动启动： systemctl enable docker

4.关闭防火墙：
service firewalld status ；查看防火墙状态 
service firewalld stop：关闭防火墙

5.拉取镜像：docker pull mysql

6、进入docker的交互式容器（进入创建的容器中）docker exec -it tomcat01 /bin/bash


### 二.docker创建容器：
1.mysql ： docker run ‐p 3306:3306 ‐‐name mysql02 ‐e MYSQL_ROOT_PASSWORD=123456 ‐d  mysql 

2.Tomcat ： docker run -d -p 8080:8080 --name tomcat01 tomcat

3.RabbitMQ:docker run -d --name myrabbitmq -p 5672:5672 -p 15672:15672 docker.io/rabbitmq:3-management

4.ActiveMQ : docker run -d --name myactivemq -p 61617:61616 -p 8162:8161 docker.io/webcenter/activemq:latest

5.elasticsearch ： docker run -e ES_JAVA_OPTS="-Xms256m -Xmx256m" -d -p 9200:9200 -p 9300:9300 --name es01 0ba66712c1f9


### docker镜像加速 
docker pull registry.docker-cn.com/library/ubuntu:16.04


### 三.docker连接上idea
#### 步骤：
1.下载插件

2.Linux的docker和Linux的idea 输入：（在这个网址）   
https://www.jetbrains.com/help/idea/2018.1/docker-connection-settings.html?utm_medium=link&utm_source=product&utm_campaign=IU&utm_content=2018.1


https://my.oschina.net/u/1776714/blog/1605259

![image](https://note.youdao.com/yws/api/personal/file/432BE17B5216463C8E429F551C7C7F46?method=download&shareKey=48812586f8d32d439f2054f1832db95b)

---

#### docker端口映射报错：
Error response from daemon: Cannot start container eb9d501f56bc142d9bf75ddfc7ad88383b7388ca6a5959309af2165f1fff6292: iptables failed: iptables --wait -t nat -A DOCKER -p tcp -d 0/0 --dport 8081 -j DNAT --to-destination 

172.17.0.164:8080 ! -i docker0: iptables: No chain/target/match by that name.

 (exit status 1)


解决办法：

```
pkill docker
iptables -t nat -F
ifconfig docker0 down
brctl delbr docker0
```

重启docker后解决


#### 报错  ARNING: IPv4 forwarding is disabled. Networking will not work.

解决办法：

```
# vi /etc/sysctl.conf
或者
# vi /usr/lib/sysctl.d/00-system.conf
添加如下代码：
    net.ipv4.ip_forward=1

重启network服务
# systemctl restart network

查看是否修改成功
# sysctl net.ipv4.ip_forward
```


如果返回为“net.ipv4.ip_forward = 1”则表示成功了



#### Centos7安装docker

```
yum -y install epel-release
yum install python-pip
pip install --upgrade pip
pip install docker-compose    如果报错，就执行下面一句话
pip install docker-compose --ignore-installed requests
```



### docker使用docker-compose
#### 以下是在 Ubuntu 系统中安装 Docker Compose 的说明：

> curl -L https://github.com/docker/compose/releases/download/1.17.0/docker-compose-`unam e -s`-`uname -m` -o /usr/local/bin/docker-compose

以下是下载进度：

% Total    % Received % Xferd  Average Speed   Time    Time     Time  Current                                 Dload  Upload   Total   Spent    Left  Speed 100   617    0   617    0     0    168      0 --:--:--  0:00:03 --:--:--   168 100 8649k  100 8649k    0     0  15651      0  0:09:25  0:09:25 --:--:-- 10383


##### 给  docker-compose  添加执行的权限：
sudo chmod +x /usr/local/bin/docker-compose

##### 查看  docker-compose  的版本
docker-compose version

##### Docker Compose 使用

前台运行  
docker-compose up

后台运行  
docker-compose up -d

启动  
docker-compose start

停止  
docker-compose stop

停止并移除容器  
docker-compose down


#### docker-compose创建gitlab


```
version: '3'
services:
    gitlab:
      image: 'twang2218/gitlab-ce-zh:9.4'
      restart: always
      hostname: '192.168.21.140'
      environment:
        TZ: 'Asia/Shanghai'
        GITLAB_OMNIBUS_CONFIG: |
          external_url 'http://192.168.21.140:8080'
          gitlab_rails['gitlab_shell_ssh_port'] = 2222
          unicorn['port'] = 8888
          nginx['listen_port'] = 8080
      ports:
        - '8080:8080'
        - '8443:443'
        - '2222:22'
      volumes:
        - /usr/local/docker/gitlab/config:/etc/gitlab
        - /usr/local/docker/gitlab/data:/var/opt/gitlab
        - /usr/local/docker/gitlab/logs:/var/log/gitlab
```


#### docker-compose创建mysql 


```
version: '3'
services:
  mysql:
    restart: always
    image: mysql
    container_name: mysql
    ports:
      - 3306:3306
    environment:
      TZ: Asia/Shanghai
      MYSQL_ROOT_PASSWORD: 123456
    command:
      --character-set-server=utf8mb4
      --collation-server=utf8mb4_general_ci
      --explicit_defaults_for_timestamp=true
      --lower_case_table_names=1
      --max_allowed_packet=128M
      --sql-mode="STRICT_TRANS_TABLES,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION,NO_ZERO_DATE,NO_ZERO_IN_DATE,ERROR_FOR_DIVISION_BY_ZERO"
    volumes:
      - /usr/local/docker/mysql/mysql:/var/lib/mysql
```


#### dockker-compose创建redis

**第一步.先在redis文件夹创建yml文件（redis的端口可以不指定，系统自动分配）**


```
version: '3.1'
services:
  master:
    image: redis:3
    ports:
      - 6379:6379

  slave:
    image: redis:3
    ports:
      - 6380:6379
    command: redis-server --slaveof redis-master 6379
    links:
      - master:redis-master

  sentinel:
    build: sentinel
    ports:
      - 26379:26379
    environment:
      - SENTINEL_DOWN_AFTER=5000
      - SENTINEL_FAILOVER=5000
    links:
      - master:redis-master
      - slave
```



注意：在上面不指定映射端口的情况下可执行以下命令：  
docker-compose scale sentienl=3      表示创建三个sentienl   
docker-compose scale master=2      表示创建两个主redis，然后由三个sentienl来制定谁为备用节点   
docker-compose scale slave=3   表示指定6个从redis   


**第二步：在redis文件夹下创建sentinel文件夹**

输入 vim Dockerfile  
再   

```
FROM redis:3
MAINTAINER zsl <1083837617@qq.com>
EXPOSE 26379
ADD sentinel.conf /etc/redis/sentinel.conf
RUN chown redis:redis /etc/redis/sentinel.conf
ENV SENTINEL_QUORUM 2
ENV SENTINEL_DOWN_AFTER 30000
ENV SENTINEL_FAILOVER 180000
COPY sentinel-entrypoint.sh /usr/local/bin/
RUN chmod +x /usr/local/bin/sentinel-entrypoint.sh
ENTRYPOINT ["sentinel-entrypoint.sh"]
```


输入 vim sentinel.conf  


```
#Exmple sentinel.conf can be downloaded from http://download.redis.io/redis-stable/sentinel.conf
port 26379
dir /tmp
sentinel monitor mymaster redis-master 6379 $SENTINEL_QUORUM
sentinel down-after-milliseconds mymaster $SENTINEL_DOWN_AFTER
sentinel parallel-syncs mymaster 1
sentinel failover-timeout mymaster $SENTINEL_FAILOVER
```


输入 vim sentinel-entrypoint.sh

```
#!/bin/sh
sed -i "s/\$SENTINEL_QUORUM/$SENTINEL_QUORUM/g" /etc/redis/sentinel.conf
sed -i "s/\$SENTINEL_DOWN_AFTER/$SENTINEL_DOWN_AFTER/g" /etc/redis/sentinel.conf
sed -i "s/\$SENTINEL_FAILOVER/$SENTINEL_FAILOVER/g" /etc/redis/sentinel.conf
exec docker-entrypoint.sh redis-server /etc/redis/sentinel.conf --sentinel
```



#### docker-compose创建tomcat

```
version: '3'
services:
  tomcat:
    restart: always
    image: tomcat
    container_name: tomcat
    ports:
      - 8080:8080
    volumes:
      - /usr/local/docker/tomcat/webapps/:/usr/local/tomcat/webapps/
    environment:
      TZ: Asia/Shanghai
```



#### docker-compose创建zookeeper

```
version: '3.1'
services:
    zoo1:
        image: zookeeper
        restart: always
        hostname: zoo1
        ports:
            - 2181:2181
        environment:
            ZOO_MY_ID: 1
            ZOO_SERVERS: server.1=zoo1:2888:3888 server.2=zoo2:2888:3888 server.3=zoo3:2888:3888

    zoo2:
        image: zookeeper
        restart: always
        hostname: zoo2
        ports:
            - 2182:2181
        environment:
            ZOO_MY_ID: 2
            ZOO_SERVERS: server.1=zoo1:2888:3888 server.2=zoo2:2888:3888 server.3=zoo3:2888:3888

    zoo3:
        image: zookeeper
        restart: always
        hostname: zoo3
        ports:
            - 2183:2181
        environment:
            ZOO_MY_ID: 3
            ZOO_SERVERS: server.1=zoo1:2888:3888 server.2=zoo2:2888:3888 server.3=zoo3:2888:3888
```


#### Dockerfile的关键字

FROM    基础镜像，指定当前镜像是基于哪个镜像的。

MAINTAINER     镜像维护者的姓名月邮箱地址。

RUN    容器构建过程中需要执行的命令。

EXPOSE   暴露镜像服务的端口号。

WORKDIR   指定进入创建的容器时，会跳转到什么位置。

ENV   用于设置构建镜像过程中的环境变量。镜像的其他位置需要取这个变量的值，可以使用
$镜像名。  
例如：  
定义变量： ENV MY_PATH /usr/local   
获得变量的值： WORKSPACE $MY_PATH  

ADD   将宿主机的目录下的文件拷贝进入到镜像，且会自动处理URL以及解压tar压缩包

COPY   将宿主机的目录下的文件拷贝到镜像。

VOLUME   容器数据卷的持久化工作。

CMD   制定一个容器启动时需要执行的命令。Dockerfile文件中可以有多个 CMD 命令，但是只有最后一个 CMD 的命令生效；且 CMD 会被 docker run 命令之后的参数替换掉。

ENTRYPOINT   指定一个容器运行时需要执行的命令。ENTRYPOINT  不会被 docker run 命令之后的参数替换掉。

ONBUILD   当构建一个被继承的Dockerfile时运行命令，父镜像被子继承后父镜像的onbuild被触发。

##### 例如：

```
from centos

ENV mypath /emp
WORKDIR $mypath

RUN yum install vim -y
EXPOSE 80
CMD /bin/bash
```





# Docker复学

## 一 Docker常见命令

### 1. Docker帮助命令

- docker version  docker版本信息
- docker info  docker基本信息
- docker --help  docker帮助信息

### 2. Docker镜像命令

- **docker images**  列出所有镜像

  - **`-a`**  列出本地所有镜像

  - **`-q`**  只显示镜像id
  - **`--digests`**  显示镜像的摘要信息
  - **`--no-trunc`**  显示完整的镜像信息

- **docker search 镜像名称**   查询对应的镜像

  - **`--no-trunc`** 显示完整的镜像信息
  - **`-s`**  列出收藏数不小于指定值的镜像

- **docker pull 镜像名称:tag**   下载对应tag版本的镜像

- **docker rmi  镜像名字**  

  - **`docker rmi -f 镜像id`**  删除单个镜像

  - **`docker rmi -f 镜像名1:tag  镜像名2:tag `**  删除多个镜像
  - **`docker rmi -f ${docker images -qa}  `** 删除所有镜像

### 3. Docker容器命令

- **docker run [OPTIONS]  IMAGE  [COMMAND]**   新建并启动容器
  - OPTIONS说明：
    - **`--name=容器名称`**  为容器指定一个名称；
    - **`-d`**  后台运行容器，并返回容器id，即以守护式方式启动容器；
    - **`-i`**  以交互模式运行容器，通常与 **`-t`** 同时使用；例如：创建一个交互式容器，在容器中执行/bin/bash命令：docker run -it mysql -p 3307:3306 --name mysql_3307  /bin/bash   
    - **`-t`**  为容器重新分配一个伪输入终端；
    - **`-p`**  端口映射：
      - ip:hostPort:containerPort
      - ip::containerPort
      - `hostPort:containerPort` 左边是宿主机，右边是容器
      - containerPort

- **docker ps [OPTIONS]**  列出所有的容器：

  - **`-a`**  列出当前所有正在运行的容器 + 历史上运行过的容器；
  - **`-l`**  显示最近创建的容器；
  - **`-n`**  显示最近n个创建的容器；
  - **`-q`**  静默模式，只显示容器编号；
  - **`--no-trunc`**  不截断输出。

- **退出容器：**

  - **`exit`**  如果是以交互式启动的容器，则会停止容器并退出到宿主界面；如果是以守护方式启动的容器，则不会停止容器并退出到宿主界面；
  - **`ctrl + p + q`**  容器不停止，并退出到宿主机界面；

- **docker start  容器id或者容器名**   启动容器；

- **docker restart 容器id或者容器名**  重启容器；

- **docker  kill  容器名或者容器id **  强制停止容器；

- **docker stop 容器id或者容器名** 停止容器；

- **docker rm  容器id**  删除已经停止了的容器：

  - **`docker rm -f ${docker ps -a -q}`**  删除多个已经停止了的容器；

  -  **`docker rm 容器id1 容器id2`**  删除指定容器；











