php7-install.md  

使用源码编译安装PHP7

2015年6月11日，PHP官网发布消息，正式公开发布PHP7第一版的alpha版本.

## PHP7特性：

PHP 7.0.0 Alpha 1使用新版的ZendEngine引擎，带来了许多新的特性，以下是不完全列表：
（1）性能提升：PHP7比PHP5.6性能提升了两倍。 Improved performance: PHP 7 is up to twice as fast as PHP 5.6
（2）全面一致的64位支持。 Consistent 64-bit support
（3）以前的许多致命错误，现在改成抛出异常。Many fatal errors are now Exceptions
（4）移除了一些老的不在支持的SAPI（服务器端应用编程端口）和扩展。Removal of old and unsupported SAPIs and extensions
（5）新增了空接合操作符。The null coalescing operator (??)
（6）新增加了结合比较运算符。Combined comparison Operator (<=>)
（7）新增加了函数的返回类型声明。Return Type Declarations
（8）新增加了标量类型声明。Scalar Type Declarations
（9）新增加匿名类。Anonymous Classes

## 系统环境：
```
$ uname -mnprs
Darwin Mac-mini.local 14.3.0 x86_64 i386

$ sw_vers
ProductName:	Mac OS X
ProductVersion:	10.10.3
BuildVersion:	14D136
```

## 源码安装PHP7:

PHP7下载地址：https://downloads.php.net/~ab/
最新版本(10-May-2016 15:44): https://downloads.php.net/~ab/php-7.0.7RC1.tar.bz2  
```
$ wget https://downloads.php.net/~ab/php-7.0.7RC1.tar.bz2
$ tar jxf php-7.0.7RC1.tar.bz2
$ cd php-7.0.7RC1

$ ./configure --prefix=/usr/local/webserver/php7 \
--with-config-file-path=/usr/local/webserver/php7 \
--with-mysqli \
--enable-pdo \
--with-pdo-mysql \
--with-mysql-sock=/usr/local/mysql/data/WangTomdeMBP.pid \
--enable-cgi \
--enable-fpm \
--enable-sockets \
--enable-mbstring \
--enable-mbregex \
--enable-bcmath \
--enable-xml \
--enable-zip \
--enable-opcache \
--with-zlib=/usr/local/lib/zlib \
--with-gd \
--with-curl=/usr/local/webserver/curl \
--disable-iconv

--with-iconv=/usr/local/libiconv \


参数选项"--enable-mbstring" ：激活 mbstring 函数。 要使用 mbstring 函数必须启用这个选项。

--disable-iconv

$ ./configure
... ...
checking size of long... (cached) 8
checking size of long long... (cached) 8
checking for iconv support... yes
checking for iconv... no
checking for libiconv... no
configure: error: Please specify the install prefix of iconv with --with-iconv=<DIR>

```

CFLAGS='-arch x86_64' CCFLAGS='-arch x86_64' CXXFLAGS='-arch x86_64'

ARCHFLAGS="-arch x86_64"

参数with-mysql-sock： 
获取MySQL的pid路径：  
$ ps -ef |grep mysql  
可以看到结果中：“--pid-file=/usr/local/mysql/data/WangTomdeMBP.pid”

参数“enable-opcache”，此参数选项在php5.5及以后版本自带


安装 libiconv (字符编码转换库)
网站地址: http://www.gnu.org/software/libiconv/
当前版本: http://ftp.gnu.org/pub/gnu/libiconv/libiconv-1.14.tar.gz

```
$ wget http://ftp.gnu.org/pub/gnu/libiconv/libiconv-1.14.tar.gz
$ tar zxf libiconv-1.14.tar.gz
$ cd libiconv-1.14
$ ./configure --prefix=/usr/local/webserver/libiconv
$ make
$ sudo make install

$ /usr/local/webserver/libiconv/bin/iconv --version
iconv (GNU libiconv 1.14)
Copyright (C) 2000-2011 Free Software Foundation, Inc.

```


配置参数
```
$ ./configure --prefix=/usr/local/php7 \
--enable-fpm \
--with-config-file-path=/usr/local/php7 \
--with-iconv=/usr/local/webserver/libiconv \

执行configure配置后，可以看到有如下结果：
... ...
Thank you for using PHP.

config.status: creating php7.spec
config.status: creating main/build-defs.h
config.status: creating scripts/phpize
config.status: creating scripts/man1/phpize.1
config.status: creating scripts/php-config
config.status: creating scripts/man1/php-config.1
config.status: creating sapi/cli/php.1
config.status: creating sapi/cgi/php-cgi.1
config.status: creating ext/phar/phar.1
config.status: creating ext/phar/phar.phar.1
config.status: creating main/php_config.h
config.status: executing default commands  
WangTomdeMacBook-Pro:php-7.0.0alpha1 wangtom$ 
```
$ make
$ make test
$ sudo make install

查看PHP7是否安装成功
````
$ /usr/local/webserver/php7/bin/php -v
PHP 7.0.7RC1 (cli) (built: May 22 2016 19:14:03) ( NTS )
Copyright (c) 1997-2016 The PHP Group
Zend Engine v3.0.0, Copyright (c) 1998-2016 Zend Technologies

设置环境变量php7
$ sudo ln -s /usr/local/webserver/php7/bin/php /usr/local/bin/php7
注意新版本的OSX系统不能指定到/usr/bin/下，指定到/usr/local/bin/下即可。  

> //原来可以使用：
> //$ sudo ln -s /usr/local/webserver/php7/bin/php /usr/bin/php7 
> 因为系统默认开启了“系统完整性保护,System Integrity Protection (SIP)” 
> $ csrutil status
> System Integrity Protection status: enabled.


$ php -v
PHP 5.5.20 (cli) (built: Feb 25 2015 23:30:53)
Copyright (c) 1997-2014 The PHP Group
Zend Engine v2.5.0, Copyright (c) 1998-2014 Zend Technologies

$ php7 -v
PHP 7.0.7RC1 (cli) (built: May 22 2016 19:14:03) ( NTS )
Copyright (c) 1997-2016 The PHP Group
Zend Engine v3.0.0, Copyright (c) 1998-2016 Zend Technologies

$ which php
/usr/bin/php
$ which php7
/usr/bin/php7
```

报错:Undefined symbols for architecture x86_64 
错误信息: 
```
Undefined symbols for architecture x86_64:
  "_libiconv", referenced from:
      _zif_iconv_substr in iconv.o
      _zif_iconv_mime_encode in iconv.o
      _php_iconv_string in iconv.o
      __php_iconv_strlen in iconv.o
      __php_iconv_strpos in iconv.o
      __php_iconv_appendl in iconv.o
      _php_iconv_stream_filter_append_bucket in iconv.o
      ...
  "_libiconv_close", referenced from:
      _zif_iconv_substr in iconv.o
      _zif_iconv_mime_encode in iconv.o
      _php_iconv_string in iconv.o
      __php_iconv_strlen in iconv.o
      __php_iconv_strpos in iconv.o
      __php_iconv_mime_decode in iconv.o
      _php_iconv_stream_filter_factory_create in iconv.o
      ...
  "_libiconv_open", referenced from:
      _zif_iconv_substr in iconv.o
      _zif_iconv_mime_encode in iconv.o
      _php_iconv_string in iconv.o
      __php_iconv_strlen in iconv.o
      __php_iconv_strpos in iconv.o
      __php_iconv_mime_decode in iconv.o
      _php_iconv_stream_filter_factory_create in iconv.o
      ...
ld: symbol(s) not found for architecture x86_64
clang: error: linker command failed with exit code 1 (use -v to see invocation)
make: *** [sapi/cli/php] Error 1
```
解决办法: 目前还没有解决,先将选项参数"--with-iconv=/usr/local/libiconv"改成"--disable-iconv"。


## 配置PHP-FPM： 

Nginx不支持对外部程序的直接调用或者解析，所有的外部程序（包括PHP）必须通过FastCGI接口来调用。
PHP-FPM是一个PHP FastCGI管理器,新版的PHP已经集成了php-fpm，在./configure的时候带 –enable-fpm参数即可开启PHP-FPM.
FPM (FastCGI Process Manager) is an alternative PHP FastCGI implementation with some additional features (mostly) useful for heavy-loaded sites.

/usr/local/webserver/php7/
/usr/local/webserver/php7/sbin/php-fpm

启动 PHP-FPM: 
```
$ sudo /usr/local/webserver/php7/sbin/php-fpm
[23-Jun-2015 15:33:01] WARNING: Nothing matches the include pattern '/usr/local/webserver/php7/etc/php-fpm.d/*.conf' from /usr/local/webserver/php7/etc/php-fpm.conf at line 125.
[23-Jun-2015 15:33:01] ERROR: failed to open error_log (/usr/local/webserver/php7/var/log/php-fpm.log): Permission denied (13)
[23-Jun-2015 15:33:01] ERROR: failed to post process the configuration
[23-Jun-2015 15:33:01] ERROR: FPM initialization failed
```
提示错误说/usr/local/webserver/php7/var/log/php-fpm.log 没权限，就给777权限：
$ chmod 777 /usr/local/webserver/php7/var/log/

FPM配置文件：
复制一份
$ cd /usr/local/webserver/php7/etc/
$ sudo cp php-fpm.conf.default php-fpm.conf
$ vim php-fpm.conf
  > 打开 error_log这一行的注释，默认该项被注释掉，若不修改会出现提示log文件路径不存在
  > error_log = log/php-fpm.log
  > 可以写成绝对路径
  > error_log = /usr/local/webserver/php7/var/log/php-fpm.log 
  > 查看最后一行:这一行用来加载php-fpm.d目录下的配置文件
  > 如果这一行被注释掉了，则需要打开注释（本次实验这行是打开注释的）
  > include=/usr/local/webserver/php7/etc/php-fpm.d/*.conf

修改 /usr/local/webserver/php7/etc/php-fpm.d/www.conf 文件：
如果这个文件不存在,就从default复制一份：
$ cd /usr/local/webserver/php7/etc/php-fpm.d/
$ sudo cp www.conf.default www.conf
将配置文件中的 user 和 group 部分的 nobody 改成 www:
$ sudo vim /usr/local/webserver/php7/etc/php-fpm.d/www.conf
  > 修改内容： 
  > user  = www  
  > group = www
  > 
  > 修改监听端口：默认9000端口，可以按需修改成别的，比如改成9001：  
  > listen = 127.0.0.1:9000 改成 listen = 127.0.0.1:9001  
  > 
  > 注意： 如果此处修改了FPM端口号，Nginx也要相应的修改:
  > Nginx配置文件: /usr/local/webserver/nginx/nginc.conf
  > fastcgi_pass   127.0.0.1:9001;
  
检测配置文件:(检测正常)
$ sudo /usr/local/webserver/php7/sbin/php-fpm -t
Password:
[22-May-2016 20:28:33] NOTICE: configuration file /usr/local/webserver/php7/etc/php-fpm.conf test is successful

启动FPM:
如果配置文件检测正常，则可以启动FPM：
$ sudo /usr/local/webserver/php7/sbin/php-fpm

查看FPM进程：
$ ps -ef|grep fpm

查看FPM版本
$ /usr/local/webserver/php7/sbin/php-fpm -v
PHP 7.0.7RC1 (fpm-fcgi) (built: May 22 2016 19:14:11)
Copyright (c) 1997-2016 The PHP Group
Zend Engine v3.0.0, Copyright (c) 1998-2016 Zend Technologies


开始启动 php-fpm：
```
$ /usr/local/webserver/php7/sbin/php-fpm
[23-Jun-2015 18:30:48] NOTICE: [pool www] 'user' directive is ignored when FPM is not running as root
[23-Jun-2015 18:30:48] NOTICE: [pool www] 'group' directive is ignored when FPM is not running as root
[23-Jun-2015 18:30:48] ERROR: unable to bind listening socket for address '127.0.0.1:9000': Address already in use (48)
[23-Jun-2015 18:30:48] ERROR: FPM initialization failed
```
这个错误问题有两个：(1)没有使用root账户执行启动命令 (2)端口9000被占用
解决方法：
使用root账户执行php-fpm启动，或 sudo /usr/local/webserver/php7/sbin/php-fpm  
关闭 PHP-fpm, 并重新启动：
```
$ lsof -P | grep ':9000' | awk '{print $2}' | xargs kill -9
$ /usr/local/webserver/php7/sbin/php-fpm -t
[23-Jun-2015 18:30:25] NOTICE: configuration file /usr/local/webserver/php7/etc/php-fpm.conf test is successful
$ sudo /usr/local/webserver/php7/sbin/php-fpm
$ 
```

## 修改Nginx 配置： 

注意本示例中Nginx安装路径有修改，与默认的安装的路径可能不同：
> 修改后的具体路径如下
> prefix=/usr/local/webserver/nginx  
> sbin-path=/usr/local/webserver/nginx/nginx  
> conf-path=/usr/local/webserver/nginx/nginx.conf  
> pid-path=/usr/local/webserver/nginx/nginx.pid  

在 nginx.conf 配置文件server 部分增加fastcgi配置，并重新加载配置文件：
```
$ sudo vim /usr/local/webserver/nginx/nginx.conf

> location ~ \.php$ {
>  root html;
>  fastcgi_pass 127.0.0.1:9000;
>  fastcgi_index index.php;
>  fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
>  include fastcgi_params;
> }

$ sudo /usr/local/webserver/nginx/nginx -t
nginx: the configuration file /usr/local/webserver/nginx/nginx.conf syntax is ok
nginx: configuration file /usr/local/webserver/nginx/nginx.conf test is successful
$ sudo /usr/local/webserver/nginx/nginx -s reload
```


错误：Primary script unknown
页面打开直接显示"File not found."
打开Nginx的error.log看到报错"Primary script unknown":
```
$ tail /usr/local/webserver/nginx/logs/error.log 
2016/05/22 21:17:42 [error] 24165#0: *54 
FastCGI sent in stderr: "Primary script unknown" while reading response header from upstream, 
client: 127.0.0.1, server: localhost, request: "GET /index.php HTTP/1.1", 
upstream: "fastcgi://127.0.0.1:9001", host: "localhost"
```
原因：
直接修改Nginx默认的配置，脚本路径不对。

> //Nginx默认的：
> fastcgi_param  SCRIPT_FILENAME  /scripts$fastcgi_script_name;
> //自己修改后的：
> fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;


用到的一些命令：

uname: 用来获取电脑和操作系统的相关信息
sw_vers: Mac下查看系统版本信息
lsof: 列出当前系统打开文件（list open files）
which: 指令会在环境变量$PATH设置的目录里查找符合条件的文件


## 参考链接：

- http://php.net/archive/2015.php#id2015-06-11-3
- http://www.hashbangcode.com/blog/compiling-and-installing-php7-ubuntu

## 更新记录
20150521 更新PHP版本到php-7.0.7RC1.


[END]
