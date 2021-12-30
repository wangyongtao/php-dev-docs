源码编译安装Nginx服务器

Compiling and Installing From the Sources


## 1 安装前准备：

1-1 检测本地环境:
```
$ sw_vers
ProductName:    Mac OS X
ProductVersion: 10.11.3
BuildVersion:   15D21

$ uname -mnprs
Darwin MacWangYongTao.local 15.3.0 x86_64 i386
```

1-2 检测GCC版本：
```
$ gcc -v
Configured with: 
--prefix=/Applications/Xcode.app/Contents/Developer/usr 
--with-gxx-include-dir=/Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX10.11.sdk/usr/include/c++/4.2.1
Apple LLVM version 7.0.2 (clang-700.1.81)
Target: x86_64-apple-darwin15.3.0
Thread model: posix
```

1-3 创建安装目录/webserver：

为了方便管理，在/usr/local/下新建一个目录webserver，以后软件安装都可以加"—prefix=/usr/local/webserver"来安装到这个目录下：

```
$sudo mkdir webserver
$pwd
/usr/local/webserver
```

# 2 安装nginx的依赖库

### 安装pcre:

PCRE(Perl Compatible Regular Expressions)是一个Perl库，包括 Perl兼容的 正则表达式库，由菲利普.海泽(Philip Hazel)编写。


the PCRE library – required by NGINX Core and Rewrite modules and provides support for regular expressions:

```
wget ftp://ftp.csx.cam.ac.uk/pub/software/programming/pcre/pcre-8.41.tar.bz2
tar jxvf pcre-8.41.tar.bz2
cd pcre-8.41
./configure --prefix=/usr/local/webserver/pcre-8.41
make
make test
sudo make install
```

### 下载 zlib 库:

zlib是提供数据压缩用的函式库，目前zlib是一种事实上的业界标准，最新版本是:

    zlib 1.2.11（January 15, 2017）。

zlib was written by Jean-loup Gailly (compression) and Mark Adler (decompression).

zlib库是 ngx_http_gzip_module 模块必须安装的，如果不需要nginx gzip压缩的话可以不开启(建议安装并开启)。
本模块不需要编译安装，只需要讲源码地址告诉 Nginx 即可(使用参数--with-zlib=zlib-path)，由 Nginx 来编译安装。


```
# 下载
wget http://www.zlib.net/zlib-1.2.11.tar.gz
tar zxvf zlib-1.2.11.tar.gz
cd zlib-1.2.11

# 如果需要安装，可以执行以下命令
./configure 
make
make test
sudo make install  
```

### 下载 PCRE 库: 

PCRE(Perl Compatible Regular Expressions)是一个轻量级的函数库Perl库，包括 perl 兼容的正则表达式库。
PCRE 库是 ngx_http_rewrite_module 模块必须安装的，主要用来页面重定向和正则替换URI的。
同 zlib 库一样，本模块不需要编译安装，只需要讲源码地址告诉 Nginx 即可(使用参数--with-pcre=pcre-path)，由 Nginx 来编译安装。


官方下载地址: https://ftp.pcre.org/pub/pcre/

目前最新的版本是 pcre-8.41 和 pcre2-10.30 两个，由于 Nginx 只支持(version 4.4 — 8.41)， 这里我们下载的是 pcre-8.41 版本 。 

    wget https://ftp.pcre.org/pub/pcre/pcre-8.41.tar.gz
    tar -zxf pcre-8.41.tar.gz
 

### 安装 OpenSSL 库:

OpenSSL 是一个强大的安全套接字层密码库，包括主要的密码算法、常用的密钥和证书封装管理功能及SSL协议，并提供丰富的应用程序供测试或其它目的使用。

the OpenSSL library – required by NGINX SSL modules to support the HTTPS protocol:

```
$ wget http://www.openssl.org/source/openssl-1.0.2e.tar.gz
$ tar -zxf openssl-1.0.2e.tar.gz
$ cd openssl-1.0.2e
$ ./Configure darwin64-x86_64-cc --prefix=/usr/local/webserver/openssl --openssldir=/usr/local/webserver/openssl
$ make
$ sudo make install

查看版本(检测是否安装成功):  
$ /usr/local/webserver/openssl/bin/openssl version  
OpenSSL 1.0.2h  3 May 2016  


查看OpenSSL支持的平台列表：
$./Configure LIST

```

注意事项:
> If configuring for 64-bit OS X, then use a command similar to:
> $./Configure darwin64-x86_64-cc shared enable-ec_nistp_64_gcc_128 no-ssl2 no-ssl3 no-comp --openssldir=/usr/local/ssl/macos-x86_64
> $make depend
> $sudo make install
>
> If configuring for 32-bit OS X, then use a command similar to:
> ./Configure darwin-i386-cc shared no-ssl2 no-ssl3 no-comp --openssldir=/usr/local/ssl/macosx-i386
> $make depend
> $sudo make install



## 3 安装Nginx

下载并安装Nginx:

```
// 指定安装在/usr/local/webserver/目录下：
$ wget http://nginx.org/download/nginx-1.8.1.tar.gz
$ tar zxf nginx-1.8.1.tar.gz
$ export KERNEL_BITS=64
$ ./configure --prefix=/usr/local/webserver/nginx \
    --sbin-path=/usr/local/webserver/nginx/nginx \
    --conf-path=/usr/local/webserver/nginx/nginx.conf \
    --pid-path=/usr/local/webserver/nginx/nginx.pid \
    --with-http_ssl_module \
    --with-openssl=../openssl-1.0.2h \
    --with-pcre=../pcre-8.41 \
    --with-zlib=../zlib-1.2.11
```


./configure
    --sbin-path=/usr/local/nginx/nginx
    --conf-path=/usr/local/nginx/nginx.conf
    --pid-path=/usr/local/nginx/nginx.pid
    --with-http_ssl_module
    --with-http_gzip_static_module
    --with-http_v2_module
    --with-pcre=../pcre-8.41
    --with-zlib=../zlib-1.2.11

参数说明：

--sbin-path=path — 设置可执行文件名. 默认是:prefix/sbin/nginx.
--conf-path=path — 设置配置文件名(nginx.conf). 默认是:prefix/conf/nginx.conf.
--pid-path=path  — 设置存放进程ID的nginx.pid文件名.默认是:prefix/logs/nginx.pid.
--with-http_ssl_module - 启用支持https协议，依赖OpenSSL库.
--with-pcre=path - sets the path to the sources of the PCRE library.(注意是源码的路径)
--with-zlib=path — sets the path to the sources of the zlib library.(注意是源码的路径)


## 4 配置Nginx:

配置文件路径： /usr/local/webserver/nginx/nginx.conf

$ sudo /usr/local/webserver/nginx/nginx -t
nginx: the configuration file /usr/local/webserver/nginx/nginx.conf syntax is ok
nginx: configuration file /usr/local/webserver/nginx/nginx.conf test is successful

## 5 启动Nginx:

查看版本：
```
$ /usr/local/webserver/nginx/nginx -v
nginx version: nginx/1.8.1
```

执行启动命令：
```
// 启动Nginx,没有报错可以认为启动成功  
$ sudo /usr/local/webserver/nginx/nginx
```


命令查看:

```
$ curl -I 127.0.0.1
HTTP/1.1 200 OK
Server: nginx/1.8.1
Date: Mon, 01 Feb 2016 12:29:30 GMT
Content-Type: text/html
Content-Length: 612
Last-Modified: Mon, 01 Feb 2016 10:25:21 GMT
Connection: keep-alive
ETag: "56af3291-264"
Accept-Ranges: bytes
```

浏览器查看：

打开浏览器输入：http://127.0.0.1/ 或者 http://localhost/
可以看到："Welcome to nginx! " 页面

其实，这个页面是在 /usr/local/webserver/nginx/html/ 目录下，
我们尝试新建一个html静态页面：
```
$ pwd
/usr/local/webserver/nginx/html
$ sudo vi hello.html
...(随便写入一下内容，比如"Hello,Nginx!"，然后保存)..

```

打开浏览器输入：
http://127.0.0.1/hello.html 或者 http://localhost/hello.html
可以看到我们刚刚写的静态页面hello.html的内容。

现在Nginx只支持一些静态的内容，如果需要动态内容，还需要安装PHP等。

重新加载：
$ sudo /usr/local/webserver/nginx/nginx -s reload


## 遇到的问题与解决办法  

### 报错1： 没有指定 OpenSSL 库.  

内容:
```
./configure: error: SSL modules require the OpenSSL library.
You can either do not enable the modules, or install the OpenSSL library
into the system, or build the OpenSSL library statically from the source
with nginx by using --with-openssl=<path> option.
```
分析: 启用with-http_ssl_module模块，但没有启用openssl, 启用的SSL模块需要依赖openssl库  
解决: configure时增加参数 [with-openssl=../openssl-1.0.2e].


### 报错2: 参数配置路径出错

参数with-pcre,如果指定的是with-pcre=/usr/local/webserver/pcre-8.41,则执行 make 时会报错：  

```
/Applications/Xcode.app/Contents/Developer/usr/bin/make -f objs/Makefile
cd /usr/local/pcre-8.41 \
    && if [ -f Makefile ]; then /Applications/Xcode.app/Contents/Developer/usr/bin/make distclean; fi \
    && CC="cc" CFLAGS="-O2 -pipe " \
    ./configure --disable-shared
/bin/sh: ./configure: No such file or directory  
make[1]: *** [/usr/local/webserver/pcre-8.41/Makefile] Error 127  
make: *** [build] Error 2
```

分析： set path to PCRE library sources, 注意是PCRE的源代码的路径，不是编译安装后的路径  
```
查看配置帮助：
$  ./configure --help | grep 'pcre'
  --without-pcre                     disable PCRE library usage
  --with-pcre                        force PCRE library usage
  --with-pcre=DIR                    set path to PCRE library sources
  --with-pcre-opt=OPTIONS            set additional build options for PCRE
  --with-pcre-jit                    build PCRE with JIT compilation support
```

注意这里的路径是 PCRE library sources, 是PCRE的源代码。  
直接去官网下载PCRE, 解压至与 nginx-1.8.1 平级的目录中(或者其他的路径)。  

解决： 将PCRE路径指定为源代码的路径，比如：with-pcre=/softwares/pcre-8.41  

### 报错3 ：
$ sudo ./config  --prefix=/usr/local/openssl
Operating system: i686-apple-darwinDarwin Kernel Version 15.3.0: Thu Dec 10 18:40:58 PST 2015;   
root:xnu-3248.30.4~1/RELEASE_X86_64
WARNING! If you wish to build 64-bit library, then you have to
         invoke './Configure darwin64-x86_64-cc' *manually*.
         You have about 5 seconds to press Ctrl-C to abort.

解决： 配置命令使用 Configure, 而不是config, 加参数平台参数：darwin64-x86_64-cc
$ sudo ./Configure darwin64-x86_64-cc  --prefix=/usr/local/openssl
备注： 查看支持的平台列表:$./Configure LIST

### 报错4：Undefined symbols for architecture x86_64

```
Undefined symbols for architecture x86_64:
  "_SSL_CTX_set_alpn_select_cb", referenced from:
      _ngx_http_ssl_merge_srv_conf in ngx_http_ssl_module.o
  "_SSL_CTX_set_next_protos_advertised_cb", referenced from:
      _ngx_http_ssl_merge_srv_conf in ngx_http_ssl_module.o
  "_SSL_select_next_proto", referenced from:
      _ngx_http_ssl_alpn_select in ngx_http_ssl_module.o
  "_X509_check_host", referenced from:
      _ngx_ssl_check_host in ngx_event_openssl.o
ld: symbol(s) not found for architecture x86_64
clang: error: linker command failed with exit code 1 (use -v to see invocation)
make[1]: *** [objs/nginx] Error 1
make: *** [build] Error 2

```
分析： 这个报错在网上有好多种原因造成的，因此也有许多的解决办的。
只要系统是64位系统，然后configure前在指明一下是64位系统，就没问题了。
解决：
(1)将openssl-1.0.2e目录中， 文件Makefile中的[darwin-i386-cc]全部替换成[darwin64-x86_64-cc]
(2)检测自己的系统是以32位模式运行，还是以64位模式运行，确保是以64位模式运行。
(3)要为当前启动磁盘选择 64 位内核，请在“终端”中使用下列命令：sudo systemsetup -setkernelbootarchitecture x86_64
```
// 但是设置没有生效：
$ sudo systemsetup -setkernelbootarchitecture x86_64
Password:
setting kernel architecture to: x86_64
changes to kernel architecture failed to save!
```
(4) 执行配置NGINX命令前，增加 export KERNEL_BITS=64   命令，指定系统的运行模式为64位的。

### 报错5: 重启NGIXN报错：
内容:
nginx: [error] open() "/usr/local/webserver/nginx/nginx.pid" failed (2: No such file or directory)

分析:
解决：使用nginx -c的参数指定nginx.conf文件的位置:
/usr/local/webserver/nginx/nginx -c /usr/local/webserver/nginx/nginx.conf

注意这里的nginx路径，在编译时有指定，可能像网上的有些不同，默认应该是：
/usr/local/webserver/nginx/sbin/nginx -c /usr/local/webserver/nginx/conf/nginx.conf

```
检测配置文件是否正确:  (出错了，没有找到nginx.conf文件)
$ sudo /usr/local/webserver/nginx/nginx -t  
nginx: [emerg] open() "/usr/local/webserver/nginx/nginx.conf" failed (2: No such file or directory)  
nginx: configuration file /usr/local/webserver/nginx/nginx.conf test failed  

复制配置文件：(复制一份)
$sudo cp nginx.conf.default nginx.conf

再次检测： (OK)
$ sudo /usr/local/webserver/nginx/nginx -t  
nginx: the configuration file /usr/local/webserver/nginx/nginx.conf syntax is ok  
nginx: configuration file /usr/local/webserver/nginx/nginx.conf test is successful  
```


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
Update 2016/05/17: 更新到openssl-1.0.2h,修改一些内容.

[END]
