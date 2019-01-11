[toc]
# Git学习笔记

### 一、Git本地库操作
#### 1.本地库初始化：
##### 简介：
创建一个文件夹，然后使用命令创建为git本地库
##### 步骤： 
进入新建的文件夹，进入git bash界面，输入命令 ：  git init
#### 2.设置签名：
##### 简介：
设置用户名与email地址（用于区分不同开发人员的身份）
##### 步骤：
- 设置目录级别的用户名： git config user.name zsl 

- 设置目录级别的email：  git config user.email 1083837617@qq.com
- 设置系统级别的用户名： git config --global user.name zsl
- 设置系统级别的email：  git config --global user.email 1083837617@qq.com
##### 注意：
一般设置系统级别的签名就行。

#### 3.查看git版本库的状态
##### 步骤 ： 
进入到版本库目录，然后输入命令：  git status  
#### 4.将文件提交到暂存区
##### 步骤 ：
进入到版本库，然后对里面的文件提交到暂存区，
加入到暂存区输入命令 ： **git add good.txt**    

取出将已经加入到暂存区的文件输入命令(一般不用)： **git rm --cached good.txt**
#### 5.将暂存区的内容提交到本地库
##### 方法1：
步骤：      
**1）** 将文件指向上面的步骤，提交到暂存区以后，  
输入命令 ：**git commit good.txt**  

**2）** 进入到提交的注释的界面：这就是vim编辑器界面  
![image](https://note.youdao.com/yws/api/personal/file/6BB5CE30B1A945678F7EDA1177947180?method=download&shareKey=f2a7655d08406687a11f07d0bba0245c)

**3）** 输入提交的备注与注释信息后，保存（ESC后，再输入:wq），提交到本地库。

##### 方法2：
步骤： 
将文件指向上面的步骤，提交到暂存区以后，直接在命令行里面添加提交注释。   
输入命令：**git commit -m "commit last one" good.txt**       提交成功


### 二、Git实现版本的前进与后退
#### 1、查看所有commit的版本
##### 步骤：
进入版本库，查看所有的commit，下面的命令都可以使用，显示的信息内容详悉度不同；   
- 输入命令 ： git log      最完整的形式

- 或者： git log --pretty=oneline    每次commit显示一行
- 或者： git log --oneline     相对而言比上面的内容更简洁
- 或者： git  reflog    （一般是使用在版本的回退与前进）

#### 2、执行版本前进与后退
##### 方式一：
基于索引值的方式---使用git内置的head指针来指向各个版本（推荐使用）  

**步骤：** 
使用 git reflog 命令的索引值来进行版本的选择。   
输入命令：**git reset --hard [版本的索引值]**  如： git reset --hard 57c172a
##### 方法二：
使用异或符号（只能后退，一般不使用）
##### 方法三：
使用~符号（只能后退，一般不使用）


#### 3、找回删除的文件
当删除某一个文件以后，可以通过版本库来找回文件，使用的是版本的前进与后退命令。


#### 4、文件修改以后进行比较
当文件已经提交到了本地库，然后又再次修改了文件，需要比较两个的差异性的话，  

输入命令 ： 
- git diff apple.txt     工作区的文件与暂存区的文件进行比较  

- git diff [本地库的历史版本] apple.txt   与本地库对应的的历史版本相比较


### 三、Git分支管理
#### 1、查看所有分支
git中，需要开发很多分支，查看所有分支，
输入命令 ：**git branch -v**
#### 2、创建分支
git中，如果需要创建分支，
输入命令 : **git branch [分支名]**   例如：  git branch hot_fix
#### 3、切换使用分支
git中，如果需要切换到其他分支，
输入命令：** git checkout [分支名字]**  例如 ：  git checkout hot_fix
#### 4、合并分支
git中，如果两个分支之间需要合并（将 A 分支的内容合并到 B 分支上面），那么：  
##### 步骤：  
1）进入到需要被合并的分支(进入到B分支)  
2）输入命令 ： git merge [分支名字]  例如： git merge A


### 四、git连接远程仓库
#### 1、为远程仓库的地址取别名
远程仓库的地址需要和本地仓库库相关联，有一个https或者ssh地址，因为地址太长，所以为地址取别名，方便管理：  
输入命令： **git remote add origin [远程仓库的地址]**     
    如： git remote add origin https://github.com/zslzxy/huashan.git
#### 2、查看远程仓库的别名
查看别名，输入命令： **git remote -v**
#### 3、推送本地库数据到远程GitHub
将本地库的数据推送到远程仓库，
输入命令： **git push [远程仓库地址的别名] [分支角色]**   
    如：git push origin master
#### 4、将远程仓库的数据克隆到本地---直接将远程的数据克隆下来，不需要有本地库
当需要克隆远程仓库的数据到本地的时候，需要执行：  
##### 步骤：
- 1）创建一个新的文件夹来接收需要克隆下来的数据 

- 2）需要在 Git Bash 界面进入到新创建的文件夹
- 3）输入命令 ： git clone [github上面的地址]  
如 ： git clone https://github.com/zslzxy/huashan.git

##### 效果：
- 1）新创建的文件夹不需要初始化

- 2）新创建的文件夹克隆下来数据以后，会自动生成远程仓库的别名
- 3）克隆完成以后，会自动将这个文件夹设置为本地库

#### 5、将远程库的数据抓取下来（pull）---将远程库抓取下来，本地需要有本地库，且抓取下来的数据会与之前的数据合并
git里面，pull = fetch + merge  

##### 输入命令：
git fetch [远程库地址别名] [远程分支名]  git fetch origin master   
git merge [远程库地址别名/远程分支名] 

**或者：**  

git pull [远程库地址别名] [远程分支名]


### 五、使用ssh免密登陆
#### 步骤：
1、打开Git Bash界面，进入到用户的家目录，输入命令 :  cd ~  

2、删除之前的 .ssh目录，保证生成新的ssh目录，输入命令： rm -r .ssh/  

3、输入命令生成 .ssh目录 ： ssh-keygen -t rsa -C [github邮箱]    
例如： ssh-keygen -t rsa -C 1083837617@qq.com  

4、重新更换github账号里面的ssh秘钥  

5、使用ssh地址来重新生成origin_ssh别名，调用别名来实现免密登陆



### 七、centos7安装与操作Gitlab
#### 安装gitlab：
##### 步骤：
1、安装centos7，并配置ip地址。  

2、将gitlab-ce-10.8.2-ce.0.el7.x86_64.rpm包放到 /opt/ 目录下  

3、vim install.sh，然后里面存放的内容为：  
```
sudo rpm -ivh /opt/gitlab-ce-10.8.2-ce.0.el7.x86_64.rpm
sudo yum install -y curl policycoreutils-python openssh-server cronie
sudo lokkit -s http -s ssh
sudo yum install postfix
sudo service postfix start
sudo chkconfig postfix on
curl https://packages.gitlab.com/install/repositories/gitlab/gitlab-ce/script.rpm.sh | sudo bash
sudo EXTERNAL_URL="http://gitlab.example.com" yum -y install gitlab-ce
```

4、为 install.sh文件添加执行权限 ： chmod u+x install.sh  

5、执行 install.sh 文件  ./install.sh  


#### 配置Gitlab：
##### 步骤：
1、初始化配置gitlab,输入命令 ：  gitlab-ctl reconfigure  

2、启动gitlab 服务，输入命令 ：  gitlab-ctl start

如果需要停止gitlab服务器，输入命令 ： gitlab-ctl stop

3、关闭防火墙，在浏览器输入虚拟机的ip地址 : 192.168.21.130

4、直接设置gitlab的root账号的密码，设置为 ： 123466789


### 八、Linux安装git客户端
#### 1、安装编译 git 时需要的包 

```
yum install -y curl-devel expat-devel gettext-devel openssl-devel zlib-devel 
yum install -y gcc perl-ExtUtils-MakeMaker
```

#### 2、删除已有的 git 

```
yum remove git
```

#### 3、Git 官网下载 Git 最新版 tar 包，移动到/usr/local 目录下 

```
cd /usr/local
tar -zxvf git-2.9.3.tar.gz
```

#### 4、进入解压目录以后，编译安装 

```
1）make prefix=/usr/local/git all
2）make prefix=/usr/local/git install 		
3）echo "export PATH=$PATH:/usr/local/git/bin">>/etc/bashrc 
4）source /etc/bashrc
```

