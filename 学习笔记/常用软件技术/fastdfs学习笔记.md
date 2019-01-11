# fastdfs学习笔记
1. 安装gcc   yum install make cmake gcc gcc-c++  （先查询是否安装了gcc）
2. 上传四个文件到  /usr/local/  目录：  
![image](https://note.youdao.com/yws/api/personal/file/74ECC2B0BD984FFD965F64FAC7244202?method=download&shareKey=b9634f88fec71f5143cedc80d7063025)
3. cd /usr/local 
4. unzip libfastcommon-master.zip 
5. cd libfastcommon-master 
6. ./make.sh 
7. ./make.sh install
8. cd /usr/local 
9. unzip fastdfs-master.zip 
10. cd fastdfs-master 
11. ./make.sh 
12. ./make.sh install 
13. cp /etc/fdfs/tracker.conf.sample /etc/fdfs/tracker.conf
14. vim /etc/fdfs/tracker.conf 修改内容为：  
![image](https://note.youdao.com/yws/api/personal/file/3B3C4E61FD994874BA557DF3BD310753?method=download&shareKey=3ef19d999a679578a07aba89cd8fc3d5)
15. mkdir -p /fastdfs/tracker
16. /etc/init.d/fdfs_trackerd start
17. cp /etc/fdfs/storage.conf.sample /etc/fdfs/storage.conf
18. vi /etc/fdfs/storage.conf     修改内容如下：  
![image](https://note.youdao.com/yws/api/personal/file/D92638D50D1B4F07A546F717DCAB707C?method=download&shareKey=9883eda50e4de75f9a1b54e7a00a0db0)
19. mkdir -p /fastdfs/storage 
20. /etc/init.d/fdfs_storaged start
21. cp /etc/fdfs/client.conf.sample /etc/fdfs/client.conf 
22. vim /etc/fdfs/client.conf    修改内容如下：
![image](https://note.youdao.com/yws/api/personal/file/4EEF97302A7B447D8A71E1D09144ABE2?method=download&shareKey=ee9574570848f4294acef10e0653b314)
23. 测试文件上传 ---  /usr/bin/fdfs_upload_file /etc/fdfs/client.conf /root/logo.png   
![image](https://note.youdao.com/yws/api/personal/file/F42D56D464824967BF8FACB9344442E7?method=download&shareKey=8b657ef2e34c29ae557f8b57b3408b20)
24. cd /usr/local 
25. unzip fastdfs-nginx-module-master.zip
26. wget http://nginx.org/download/nginx-1.10.0.tar.gz  
然后解压下载下来的nginx， tar xf nginx-1.10.0.tar.gz
27. yum -y install gcc pcre pcre-devel zlib zlib_devel openssl openssl-devel
28. **第一步：** 进入到nginx的目录执行 ---   ./configure --prefix=/usr/local/nginx --sbin-path=/usr/bin/nginx --add-module=/usr/local/fastdfs-nginx-module-master/src  
**第二步：** 然后输入  make && make install  将nginx安装成功
29. cp /usr/local/fastdfs-nginx-module-master/src/mod_fastdfs.conf /etc/fdfs/
30. vi /etc/fdfs/mod_fastdfs.conf    修改内容为：  
![image](https://note.youdao.com/yws/api/personal/file/C83A57168E4D4D938EE2ECCB7A29AF24?method=download&shareKey=27b0f36997d43cf40dd114921bab7d4c)
31. cd /usr/local/fastdfs-master/conf
32. cp http.conf mime.types /etc/fdfs/
33. ln -s /fastdfs/storage/data/ /fastdfs/storage/data/M00  
34. vim /usr/local/nginx/conf/nginx.conf   修改内容为：

```
第一行的  user 放出来    user  root;
   第二处 ：    
server {
        listen       80;
        server_name  localhost;
        location ~/group([0-9])/M00 {
            ngx_fastdfs_module;
        }
        #charset koi8-r;
```
然后启动nginx，  直接输入  nginx   ，启动nginx成功。
35. 浏览器访问 ： 192.168.21.124  现出nginx。
36. 192.168.21.124/(图片生成的id)就可以访问到。