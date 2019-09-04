## Docker-Compose

### 1. Docker-Compose常用命令

> **docker-compose  [-f <arg>...]  [options]  [COMMAND]  [ARGS...]**

#### 1.1 命令选项  [-f \<arg\>...]

> 这个命令选项主要是用于指定一些命令参数：
>
> - **-f , --file FILE** :用于指定Compose模板文件，默认名称为 docker-compose.yml文件。
>   - 例如： docker-compose  -f docker-compose.yml up -d
> - **-p, --porject-name NAME**：指定项目的名称，默认使用当前所在目录名称作为项目名称
>   - 例如： docker-compose  -f docker-compose.yml -p mysql01 up -d
> - **-x-network-driver** ：使用？Docker可插拔网络后端特性
> - **-x-network-driver DRIVER**  指定网络后端驱动，默认为bridge，需Docker1.9+版本支持。
> - **-verbose** 输出更多调试信息
> - **-v, --version** 打印版本并退出

#### 1.2 命令选项 [options]

- `-d` 指定在后台以守护进程方式运行服务容器
- `-no-color` 设置不使用颜色来区分不同的服务器的控制输出
- `-no-deps` 设置不启动服务所链接的容器
- `-force-recreate` 设置强制重新创建容器，不能与`--no-recreate`选项同时使用。
- `--no-create` 若容器已经存在则不再重新创建，不能与`--force-recreate`选项同时使用。
- `--no-build` 设置不自动构建缺失的服务镜像
- `--build` 设置在启动容器前构建服务镜像
- `--abort-on-container-exit` 若任何一个容器被停止则停止所有容器，不能与选项`-d`同时使用。
- `-t, --timeout TIMEOUT` 设置停止容器时的超时秒数，默认为10秒。
- `--remove-orphans` 设置删除服务中没有在`compose`文件中定义的容器
- `--scale SERVICE=NUM` 设置服务运行容器的个数，此选项将会负载在`compose`中通过`scale`指定的参数。



#### 1.3 COMMOND命令包括

| 命令    | 解释                                    |
| ------- | --------------------------------------- |
| build   | 构建或重建服务                          |
| bundle  | 从compose配置文件中产生一个docker绑定   |
| config  | 验证并查看compose配置文件               |
| create  | 创建服务                                |
| down    | 停止并移除容器、网络、数据卷            |
| events  | 从容器中接收实时的事件                  |
| exec    | 在一个运行中的容器上执行一个命令        |
| help    | 获取命令帮助信息                        |
| images  | 列出所有镜像                            |
| kill    | 通过发送SIGKILL信号来停止指定服务的容器 |
| logs    | 从容器中查看服务日志输出                |
| pause   | 暂停服务                                |
| port    | 打印绑定的公共端口                      |
| ps      | 列出所有运行中的容器                    |
| pull    | 拉去并下载指定镜像                      |
| push    | 推送服务镜像                            |
| restart | 重启yaml文件中定义的服务                |
| rm      | 删除指定已经停止服务的容器              |
| run     | 在一个服务上执行一条命令                |
| scale   | 设置指定服务运行容器的个数              |
| start   | 在容器中启动指定服务                    |
| stop    | 停止已经运行的服务                      |
| top     | 显示各个服务容器内运行的进程            |
| unpause | 回复容器服务                            |
| up      | 创建并启动容器                          |
| version | 显示docker-compose的版本号              |

#### 1.4 常用命令

- **docker-compose up**    启动所有服务
- **docker-compose ps**    列出项目中当前的所有容器
- **docker-compose down [options]**   停止和删除容器、网络、卷、镜像；选项options参数为：
  - `--rmi type`  删除镜像类型，类型可选；
    - `--rmi all`  删除compose文件中定义的所有镜像；
    - `--rmi local`  删除镜像名为空的镜像；
  - `-v,--volumes`  删除已经在compose文件中定义的和匿名的附在容器上的数据卷；
  - `--remove-orphans`  删除服务中没有在compose中定义的容器。
- **docker-comopose logs**  查看服务容器的输出，默认情况下`docker-compose`将对不同的服务输出使用不同的颜色来区分。可以通过`--no-color`来关闭颜色。
- **docker-compose build [options] [--build-arg key=val...] [SERVICE...]**  构建或重构项目中的服务容器，服务容器一旦构建后将会带上一个标记名称，可以随时在项目目录下运行 `docker-compose build` 命令来从新构建服务；选项options参数为：
  - `--compress` 通过gzip压缩构建上下文环境
  - `--force-rm` 删除构建过程中的临时容器
  - `--no-cache` 构建镜像过程中不使用缓存
  - `--pull` 始终尝试通过拉取操作来获取更新版本的镜像
  - `-m, --memory MEM`为构建的容器设置内存大小
  - `--build-arg key=val` 为服务设置`build-time`变量
- **docker-compose restart [OPTIONS] [SERVICE...]** 重启项目中的服务；选项options参数为：
  - `-t, --timeout TIMEOUT`指定重启前停止容器的超时时长，默认为10秒。
- **docker-compose pull**   拉取服务依赖的镜像；选项options参数为：
  - `--ignore-pull-failures` 忽略拉取镜像过程中的错误
  - `--parallel` 同时拉取多个镜像
  - `--quiet` 拉取镜像过程中不打印进度信息
- **docker-compose rm [OPTIONS] [SERVICE...]**  删除所有停止状态的服务容器，推荐先执行`docker-compose stop` 命令；选项options参数为：
  - `-f, --force` 强制直接删除包含非停止状态的容器
  - `-v` 删除容器所挂载的数据卷
- **docker-compose start [SERVICE]**  启动已经存在的服务容器
- **docker-compose run [options] [-v VOLUME...] [-p PORT...] [-e KEY=VAL...] SERVICE [COMMAND] [ARGS...]**   在指定服务上执行一条命令；
  - 例如：`docker-compose run ubuntu ping www.baidu.com -c 10` 在Ubuntu容器中运行ping命令10次；
- **docker-compose  scale** 设置指定服务运行的容器个数；
- **docker-compose pause [SERVICE...]**  暂停一个服务容器；
- **docker-compose kill [OPTIONS] [SERVICE...]**  发送强制停止服务容器，支持通过 `-s` 参数。
- **docker-compose config [OPTIONS]**  验证并查看compose文件配置；选项options参数为：
  - `--resolve-image-digests` 将镜像标签标记为摘要
  - `-q, --quiet` 只验证配置不输出，配置正确时不输出任何容器，配置错误时输出错误信息。
  - `--services` 打印服务名称，一行显示一个。
  - `--volumes` 打印数据卷名称，一行显示一个。
- **docker-compose create [OPTIONS] [SERVICE...]**  为服务创建容器；选项options参数为：
  - `--force-recreate` 重新创建容器，即使配置和镜像没有改变，不兼容`--no-recreate`参数。
  - `--no-recreate` 如果容器已经存在则无需重新创建，不兼容`--force-recreate`参数。
  - `--no-build` 不创建镜像即使缺失
  - `--build` 创建容器前生成镜像
- **docker-compose exec [options] SERVICE COMMAND [ARGS...]**  ，选型options参数为：
  - `-d` 分离模式，以后台守护进程运行命令。
  - `--privileged` 获取特权
  - `-T` 禁用分配TTY，默认`docker-compose exec`分配TTY。
  - `--index=index` 当一个服务拥有多个容器时可通过该参数登录到该服务下的任何服务
- **docker-compose port [options] SERVICE PRIVATE_PORT** 显示某个容器端口所映射的公共端口；选项参数`[options]`为：
  - `--protocol=proto` 指定端口协议，默认为TCP，可选UDP。
  - `--index=index` 若同意服务存在多个容器，指定命令对象容器的索引序号，默认为1。
- **docker-compose push [options] [SERVICE...]** 推送服务依赖的镜像；选项参数options为：
  - `--ignore-push-failure` 忽略推送镜像过程中的错误
- **docker-compose stop**   显示各个容器运行的进程情况
- **docker-compose unpause**   恢复处于暂停状态中的服务
- **docker-compose version**  docker-compose版本号;



### 2. docker使用docker-compose

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



### 3. docker-compose.yml语法

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

```yaml
version: '3.3'

services:
  db:
    image: mysql:5.6
    volumes:
      - db_data:/var/lib/mysql
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: 123456
      MYSQL_DATABASE: wordpress
      MYSQL_USER: wordpress
      MYSQL_PASSWORD: wordpress

  wordpress:
    depends_on:
      - db
    image: wordpress:latest
    ports:
      - "8084:80"
    restart: always
    environment:
      WORDPRESS_DB_HOST: db:3306
      WORDPRESS_DB_USER: wordpress
      WORDPRESS_DB_PASSWORD: wordpress
      WORDPRESS_DB_NAME: wordpress
volumes:
  db_data: {}
```

