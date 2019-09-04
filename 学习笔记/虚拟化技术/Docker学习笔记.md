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




## Dockerfile文件编写

Dockerfile主要是用于创建一个镜像的文件，例如创建一个jdk8的镜像：

第一步：创建Dockerfile文件

```dockerfile
FROM debian:stretch

ARG DEBIAN_FRONTEND=noninteractive
ARG JAVA_VERSION=8
ARG JAVA_UPDATE=172
ARG JAVA_BUILD=11
ARG JAVA_PACKAGE=jdk
ARG JAVA_HASH=a58eab1ec242421181065cdc37240b08

ENV LANG C.UTF-8
ENV JAVA_HOME=/opt/jdk
ENV PATH=${PATH}:${JAVA_HOME}/bin

RUN set -ex \
 && apt-get update \
 && apt-get -y install ca-certificates wget unzip \
 && wget -q --header "Cookie: oraclelicense=accept-securebackup-cookie" \
         -O /tmp/java.tar.gz \
         http://download.oracle.com/otn-pub/java/jdk/${JAVA_VERSION}u${JAVA_UPDATE}-b${JAVA_BUILD}/${JAVA_HASH}/${JAVA_PACKAGE}-${JAVA_VERSION}u${JAVA_UPDATE}-linux-x64.tar.gz \
 && CHECKSUM=$(wget -q -O - https://www.oracle.com/webfolder/s/digest/${JAVA_VERSION}u${JAVA_UPDATE}checksum.html | grep -E "${JAVA_PACKAGE}-${JAVA_VERSION}u${JAVA_UPDATE}-linux-x64\.tar\.gz" | grep -Eo '(sha256: )[^<]+' | cut -d: -f2 | xargs) \
 && echo "${CHECKSUM}  /tmp/java.tar.gz" > /tmp/java.tar.gz.sha256 \
 && sha256sum -c /tmp/java.tar.gz.sha256 \
 && mkdir ${JAVA_HOME} \
 && tar -xzf /tmp/java.tar.gz -C ${JAVA_HOME} --strip-components=1 \
 && wget -q --header "Cookie: oraclelicense=accept-securebackup-cookie;" \
         -O /tmp/jce_policy.zip \
         http://download.oracle.com/otn-pub/java/jce/${JAVA_VERSION}/jce_policy-${JAVA_VERSION}.zip \
 && unzip -jo -d ${JAVA_HOME}/jre/lib/security /tmp/jce_policy.zip \
 && rm -rf ${JAVA_HOME}/jar/lib/security/README.txt \
       /var/lib/apt/lists/* \
       /tmp/* \
       /root/.wget-hsts
```

第二步：使用docker build 命令构建镜像：

```dockerfile
docker build -t debain-jdk8:v1.0 . 
```

#### Dockerfile命令

| 指令         | 含义                                                         |
| ------------ | ------------------------------------------------------------ |
| FROM         | FROM debian:stretch表示以debian:stretch作为基础镜像进行构建。 |
| RUN          | RUN后面跟的是一些shell指令，通过 && 将这些脚本连接在了一起执行，多个命令连接在一起构建，能够减少镜像构建层数。 |
| ARG          | 该命令符主要是占位符的意思，在RUN命令中能够使用ARG中的名称来进行value的替换。 |
| ENV          | 指令的作用是在shell中设置一些环境变量。                      |
| FROM...AS... | 给一个阶段的镜像取别名  FROM....（基础镜像）AS...（别名）    |
| COPY         | 用于来回复制文件， `COPY  宿主机目录  容器目录`  表示将当前文件夹的所有文件拷贝到对应的容器文件夹目录中。也可以使用 `--from` 参数来复制前一个阶段镜像中的文件。具体入下面的示例。 |
| WORKDIR      | 在执行`RUN`后面的shell命令前会先`cd`进`WORKDIR`后面的目录    |
| ENTRYPOINT   | 这个参数表示镜像的“入口”，镜像打包完成之后，使用docker run命令运行这个镜像时，其实就是执行这个ENTRYPOINT后面的可执行文件（一般是一个shell脚本文件），也可以通过["可执行文件", "参数1", "参数2"]这种方式来赋予可执行文件的执行参数，这个“入口”执行的工作目录也是WORKDIR后面的那个目录 |
| ADD          | 将文件加入到容器指定位置  `ADD thymeleaf-master-1.0-SNAPSHOT.jar /thymeleaf-master.jar` |

Dickerfile文件可以多阶段构建，其功能是为了解决docker镜像构建的中间冗余文件的处理。例如：

```dockerfile
# Builder container
FROM registry.cn-hangzhou.aliyuncs.com/aliware2018/services AS builder

COPY . /root/workspace/agent
WORKDIR /root/workspace/agent
RUN set -ex && mvn clean package


# Runner container
FROM registry.cn-hangzhou.aliyuncs.com/aliware2018/debian-jdk8

COPY --from=builder /root/workspace/services/mesh-provider/target/mesh-provider-1.0-SNAPSHOT.jar /root/dists/mesh-provider.jar
COPY --from=builder /root/workspace/services/mesh-consumer/target/mesh-consumer-1.0-SNAPSHOT.jar /root/dists/mesh-consumer.jar
COPY --from=builder /root/workspace/agent/mesh-agent/target/mesh-agent-1.0-SNAPSHOT.jar /root/dists/mesh-agent.jar

COPY --from=builder /usr/local/bin/docker-entrypoint.sh /usr/local/bin
COPY start-agent.sh /usr/local/bin

RUN set -ex && mkdir -p /root/logs

ENTRYPOINT ["docker-entrypoint.sh"]
```








