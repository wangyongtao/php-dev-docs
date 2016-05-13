
- 如何安装PHP开发环境

-- PHP官网： http://php.net/manual/zh/install.php

在 Unix/Linux 平台下安装 PHP 有几种方法：使用配置和编译过程，或是使用各种预编译的包。
本文主要针对配置和编译 PHP 的过程。
很多 Unix 类系统都有包安装系统，可以用它来设置一个有着标准配置的 PHP。
但是若需要与标准配置不同的功能（例如一个安全服务器，或者不同的数据库驱动扩展模块），可能需要编译 PHP 和／或 web 服务器。
如果不熟悉编译软件，可以考虑搜索一下是否有人已经编译了包含所需要功能的预编译包。

### PHP开发环境包括什么？



### 如何安装PHP开发环境？

1 使用一键安装包：  

Windows系统:

PHPStudy:  
http://www.phpstudy.net/

AppServ:  
http://www.appservnetwork.com/en/

WampServer:   
http://www.wampserver.com/

UPUPW: 
http://www.upupw.net/

XAMPP:支持Windows/Linux/MacOS平台   
https://www.apachefriends.org/zh_cn/index.html

Linux系统: 

http://lnmp.org/
http://www.lanmps.com/
XAMPP:支持Windows/Linux/MacOS平台   
https://www.apachefriends.org/zh_cn/index.html

2 使用源码编译安装： 



### 如何使用源码编译安装PHP环境？
> + OpenSSl
> + Mcrypt
> + zlib
> + freetype
> + jpeg
> + 安装libpng
> + MySQL：



##### 安装 OpenSSl :

官网：http://www.openssl.org/  
简介：OpenSSL 是一个强大的安全套接字层密码库，囊括主要的密码算法、常用的密钥和证书封装管理功能及SSL协议，并提供丰富的应用程序供测试或其它目的使用。  
下载：http://www.openssl.org/source/openssl-1.0.2a.tar.gz  

参见：
Mac-64-bit: http://wiki.openssl.org/index.php/Compilation_and_Installation#Mac

```
- wget http://www.openssl.org/source/openssl-1.0.2a.tar.gz
- cd openssl-1.0.2a
- ./Configure darwin64-x86_64-cc enable-ec_nistp_64_gcc_128 no-ssl2 no-ssl3 no-comp --openssldir=/usr/local/ssl/macos-x86_64
- make
- sudo make install
```


### 安装 Mcrypt: 
> # 官网：
> http://mcrypt.hellug.gr/
> # 下载：
> ftp://mcrypt.hellug.gr/pub/crypto/mcrypt/libmcrypt
> wget ftp://mcrypt.hellug.gr/pub/crypto/mcrypt/libmcrypt/libmcrypt-2.5.7.tar.gz


### 安装 zlib:

> # 官网：
> http://www.zlib.net/
> #简介：
> zlib是提供数据压缩用的函式库，由Jean-loup Gailly与Mark Adler所开发，初版0.9版在1995年5月1日发表。zlib使用DEFLATE算法，最初是为libpng函式库所写的，后来普遍为许多软件所使用。此函式库为自由软件，使用zlib授权。

```
$ wget http://zlib.net/zlib-1.2.8.tar.gz
$ ./configure --prefix=/usr/local/lib/zlib
$ make
$ make install
```

### 安装 libiconv (字符编码转换库)

# 网站: http://www.gnu.org/software/libiconv/
# 当前版本: http://ftp.gnu.org/pub/gnu/libiconv/libiconv-1.14.tar.gz

$ wget http://ftp.gnu.org/pub/gnu/libiconv/libiconv-1.14.tar.gz
$ tar zxvf libiconv-1.14.tar.gz
$ cd libiconv-1.14
$ ./configure --prefix=/usr/local/lib/libiconv
$ make
$ sudo make install



### 安装freetype
- tar xzvf freetype-2.1.5.tar.gz (http://freetype.sourceforge.net/download.html#stable)
- cd freetype-2.1.5
- ./configure --prefix=/usr/local/modules/freetype
- make
- make install

# 安装 jpeg:
> http://www.ijg.org/
> http://www.ijg.org/files/
> wget http://www.ijg.org/files/jpegsrc.v9.tar.gz
> 
> - $ wget http://www.ijg.org/files/jpegsrc.v9.tar.gz 
> - $ tar zxf jpegsrc.v9.tar.gz  
> - $ cd jpeg-9/ 
> - $ ./configure --prefix=/usr/local/lib/jpeg9  
> - $ make  
> - $ sudo make install  

### 安装libpng:
> # 网站：
> http://libmng.com/pub/png/libpng.html
> #下载：
> http://sourceforge.net/projects/libpng/
> ftp://ftp.simplesystems.org/pub/libpng/png/src/libpng16/libpng-1.6.17.tar.gz
> 
> 
> 
> 
> 

### 安装 MySQL：
> http://dev.mysql.com/downloads/
> 
> 
> 
> 
>

libevent
https://sourceforge.net/projects/levent/files/libevent/libevent-2.0/libevent-2.0.22-stable.tar.gz

http://pecl.php.net/get/xhprof-0.9.4.tgz




```
 ./configure \
    --prefix=/usr/local/php \
    --with-config-file-path=/usr/local/php \
    --with-mysql \
    --with-mysqli \
    --enable-pdo \
    --with-pdo-mysql \
    --with-mysql-sock=/tmp/mysql.sock \
    --enable-opcache \
    --enable-cgi \
    --enable-fpm \
    --enable-sockets \
    --enable-mbstring \
    --enable-mbregex \
    --enable-bcmath \
    --enable-xml \
    --enable-zip \
    --with-zlib=/usr/local/lib/zlib \
    --with-gd \
    --with-png-dir=/usr/local/lib/libpng \
    --with-jpeg-dir=/usr/local/lib/libjpeg \
    --with-openssl=/usr/local/ssl/macos-x86_64/ \
    --with-curl=/usr/local/curl \
    --with-mhash=/usr/local/lib/libmhash \
    --with-mcrypt=/usr/local/lib/libmcrypt \
    --with-iconv=/usr/local/lib/libiconv \
```

```
# 报错：OpenSSL
... ...  
checking for DSA_get_default_method in -lssl... no  
checking for X509_free in -lcrypto... yes  
checking for RAND_egd... no  
checking for pkg-config... no  
configure: error: Cannot find OpenSSL's <evp.h>  
Mac-mini:php-5.6.9 WangTom$
```


Mac-mini:php-5.6.9 WangTom$ which openssl
/usr/bin/openssl


修改openssl参数地址：
--with-openssl=/usr/local/ssl/macos-x86_64/

```
# 报错：zlib
... ... 
checking bundled sqlite3 library... yes  
checking for ZLIB support... yes  
checking if the location of ZLIB install directory is defined... no  
configure: error: Cannot find libz  
Mac-mini:php-5.6.9 WangTom$  
```
修改openssl参数地址：
--with-openssl=/usr/local/lib/zlib 


```
# 报错： curl
... ... 
checking whether to enable ctype functions... yes
checking for cURL support... yes
checking for cURL in default path... not found
configure: error: Please reinstall the libcurl distribution -
    easy.h should be in <curl-dir>/include/curl/
Mac-mini:php-5.6.9 WangTom$
```




```
# 报错： cURL
checking whether to enable ctype functions... yes
checking for cURL support... yes
checking for cURL in default path... not found
configure: error: Please reinstall the libcurl distribution -
    easy.h should be in <curl-dir>/include/curl/
```


Mac-mini:php-5.6.9 WangTom$ brew --config
HOMEBREW_VERSION: 0.9.5
ORIGIN: https://github.com/Homebrew/homebrew
HEAD: 5b151e438b20f68afadbc9a6df2a01a0cc52b383
Last commit: 3 weeks ago
HOMEBREW_PREFIX: /usr/local
HOMEBREW_CELLAR: /usr/local/Cellar
CPU: 8-core 64-bit ivybridge
OS X: 10.10.3-x86_64
Xcode: 6.3.2
CLT: N/A
Clang: 6.1 build 602
X11: N/A
System Ruby: 2.0.0-p481
Perl: /usr/bin/perl
Python: /usr/bin/python
Ruby: /usr/bin/ruby
Java: 1.8.0

Mac-mini:~ WangTom$ sudo ln -s /Applications/Xcode6-Beta4.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX10.10.sdk/usr/include /usr/include  

Mac-mini:~ WangTom$ ls -l /usr/include  
lrwxr-xr-x  1 root  wheel  118  6 19 14:18 /usr/include -> /Applications/Xcode6-Beta4.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX10.10.sdk/usr/include  












