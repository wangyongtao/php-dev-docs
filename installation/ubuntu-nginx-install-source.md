在Linux上使用源码编译安装 Nginx 服务器(nginx-1.13.6)

Compiling and Installing From the Sources

本文是在 Ubuntu 环境源码安装Nginx的总结。


## 1 安装前准备：

检测本地环境:

```
cat /proc/version
Linux version 3.13.0-86-generic (buildd@lgw01-19) 
(gcc version 4.8.2 (Ubuntu 4.8.2-19ubuntu1) ) 

$ uname -mnprs
Linux iZuf6fu4aqltf25ewmxereZ 3.13.0-86-generic x86_64 x86_64
```

为了方便集中管理，在/usr/local/下新建一个 webserver 目录，以后软件安装都可以加"—prefix=/usr/local/webserver"来安装到这个目录下。

```
$sudo mkdir webserver
$pwd
/usr/local/webserver
```

# 2 安装nginx的依赖库
 
### (2.1)下载 zlib 库:

zlib是提供数据压缩用的函式库，目前zlib是一种事实上的业界标准，最新版本是: zlib 1.2.11（January 15, 2017）。

zlib was written by Jean-loup Gailly (compression) and Mark Adler (decompression).

zlib库是 ngx_http_gzip_module 模块必须安装的，如果不需要nginx gzip压缩的话可以不开启(建议安装并开启)。
本模块不需要编译安装，只需要讲源码地址告诉 Nginx 即可(使用参数--with-zlib=zlib-path)，由 Nginx 来编译安装。

```
# 下载并解压
wget http://www.zlib.net/zlib-1.2.11.tar.gz
tar zxvf zlib-1.2.11.tar.gz

# 如果需要安装，可以执行以下命令
cd zlib-1.2.11
./configure 
make
make test
sudo make install  
```

### (2.2)下载 PCRE 库: 

PCRE(Perl Compatible Regular Expressions)是一个轻量级的函数库Perl库，包括 perl 兼容的正则表达式库。
PCRE 库是 ngx_http_rewrite_module 模块必须安装的，主要用来页面重定向和正则替换URI的。
同 zlib 库一样，本模块不需要编译安装，只需要讲源码地址告诉 Nginx 即可(使用参数--with-pcre=pcre-path)，由 Nginx 来编译安装。

官方下载地址: https://ftp.pcre.org/pub/pcre/

目前最新的版本是 pcre-8.41 和 pcre2-10.30 两个，由于 Nginx 只支持(version 4.4 — 8.41)， 这里我们下载的是 pcre-8.41 版本 。 

```
# 下载并解压
wget https://ftp.pcre.org/pub/pcre/pcre-8.41.tar.gz
tar -zxf pcre-8.41.tar.gz
```

### (2.3) 安装 OpenSSL 库:

OpenSSL 是一个强大的安全套接字层密码库，包括主要的密码算法、常用的密钥和证书封装管理功能及SSL协议，并提供丰富的应用程序供测试或其它目的使用。

the OpenSSL library – required by NGINX SSL modules to support the HTTPS protocol:

```
$ wget http://www.openssl.org/source/openssl-1.0.2m.tar.gz
$ tar -zxf openssl-1.0.2m.tar.gz
$ cd openssl-1.0.2m
$ sudo make
$ sudo make install

# 查看版本(检测是否安装成功):  
$ openssl version  
OpenSSL 1.1.0g  2 Nov 2017
```
 

## 3 安装Nginx

下载并解压:

```
# 指定安装在/usr/local/webserver/目录下：
$ wget http://nginx.org/download/nginx-1.13.6.tar.gz  
$ tar zxf nginx-1.13.6.tar.gz  
```

配置与安装: 指定的参数可以按需调整，[详情参看](https://nginx.org/en/docs/);

```
$ ./configure \
    --prefix=/usr/local/webserver/nginx-1.13.6 \
    --sbin-path=/usr/local/webserver/nginx-1.13.6/nginx \
    --conf-path=/usr/local/webserver/nginx-1.13.6/nginx.conf \
    --pid-path=/usr/local/webserver/nginx-1.13.6/nginx.pid \
    --with-http_ssl_module \
    --with-http_gzip_static_module \
    --with-http_v2_module \
    --with-pcre=../pcre-8.41 \
    --with-zlib=../zlib-1.2.11 
$ sudo make  
$ sudo make install  
```

参数说明：

--sbin-path=path — 设置可执行文件名. 默认是:prefix/sbin/nginx.  
--conf-path=path — 设置配置文件名(nginx.conf). 默认是:prefix/conf/nginx.conf.  
--pid-path=path  — 设置存放进程ID的nginx.pid文件名.默认是:prefix/logs/nginx.pid.  
--with-http_ssl_module - 启用支持https协议，依赖OpenSSL库.  
--with-ngx_http_gzip_static_module - gzip压缩.  
--with-http_v2_module - provides support for HTTP/2 and supersedes the ngx_http_spdy_module module.  
--with-pcre=path - sets the path to the sources of the PCRE library.(注意是源码的路径)  
--with-zlib=path — sets the path to the sources of the zlib library.(注意是源码的路径)  

## 4 配置 Nginx:

配置文件路径： /usr/local/webserver/nginx/nginx.conf

可以看下 Nginx 的默认配置, 可以看到默认端口是80，默认首页是index.html，默认读取的网站目录是html:

```
server {
    listen       80;
    server_name  localhost;
    location / {
        root   html;
        index  index.html index.htm;
    }
}
```

如果修改了配置文件，可以使用 `-t` 参数来检测一些是否有误，`-s reload`来重启服务:  

```
# 检测配置文件是否有错误   
$ /usr/local/webserver/nginx-1.13.6/nginx -t
nginx: the configuration file /usr/local/webserver/nginx-1.13.6/nginx.conf syntax is ok
nginx: configuration file /usr/local/webserver/nginx-1.13.6/nginx.conf test is successful
# 重启服务  
$ sudo /usr/local/webserver/nginx-1.13.6/nginx -s reload  
```

## 5 启动 Nginx:

查看版本，检测是否安装成功：

```
$ /usr/local/webserver/nginx-1.13.6/nginx -v
nginx version: nginx/1.13.6
```

执行启动命令, 没有报错可以认为启动成功：

```
// 启动Nginx 
$ sudo /usr/local/webserver/nginx-1.13.6/nginx
```

## 6 查看 Nginx 运行效果: 


使用curl命令查看, 可以看到返回值"Server: nginx/1.13.6":

```
$ curl -I 127.0.0.1
HTTP/1.1 200 OK
Server: nginx/1.13.6
Date: Tue, 21 Nov 2017 06:03:48 GMT
Content-Type: text/html
Content-Length: 612
Last-Modified: Tue, 21 Nov 2017 05:54:40 GMT
Connection: keep-alive
ETag: "5a13bfa0-264"
Accept-Ranges: bytes
```

浏览器查看：

打开浏览器输入：http://127.0.0.1/ 或者 http://localhost/
可以看到："Welcome to nginx! " 页面

其实，这个页面是在 /usr/local/webserver/nginx-1.13.6/html/ 目录下(这是nginx.conf的默认配置，可以修改的)，
我们尝试新建一个html静态页面：

```
$ pwd
/usr/local/webserver/nginx-1.13.6/html

$ sudo vi hello.html
...(随便写入一下内容，比如"Hello,Nginx!"，然后保存)..

```

打开浏览器输入：`http://127.0.0.1/hello.html` 或者 `http://localhost/hello.html`
可以看到我们刚刚写的静态页面hello.html的内容。

现在Nginx只支持一些静态的内容，如果需要动态内容，还需要安装PHP等。

## 用到的一些命令 

```
$ ps -ef |grep nginx
$ kill -9 [pid]
$ arch
$ uname -a
$ ioreg -l -p IODeviceTree | grep "firmware-abi" | sed -e 's/[^0-9A-Z]//g'

```


## 参考资料

http://nginx.org/en/docs/configure.html

https://www.nginx.com/resources/admin-guide/installing-nginx-open-source/

http://segmentfault.com/a/1190000003822041?_ea=392297

http://lists.apple.com/archives/macnetworkprog/2015/Jun/msg00025.html

http://www.iyunv.com/thread-18789-1-1.html

https://wiki.openssl.org/index.php/Compilation_and_Installation#Mac

http://www.nooidea.com/2011/02/switch-mac-into-64-bit.html


## 更新记录

Update 2016/01/28: Updated for nginx-1.8.1.  
Update 2016/05/17: 更新到openssl-1.0.2h, 修改了一些内容.  
Update 20167/11/21: 更新到nginx-1.13.6、pcre-8.41、zlib-1.2.11， 修改了一些内容.  

[END]