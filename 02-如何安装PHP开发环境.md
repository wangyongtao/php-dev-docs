
- 如何安装PHP开发环境

-- PHP官网： http://php.net/manual/zh/install.php

在 Unix/Linux 平台下安装 PHP 有几种方法：使用配置和编译过程，或是使用各种预编译的包。
本文主要针对配置和编译 PHP 的过程。
很多 Unix 类系统都有包安装系统，可以用它来设置一个有着标准配置的 PHP。
但是若需要与标准配置不同的功能（例如一个安全服务器，或者不同的数据库驱动扩展模块），可能需要编译 PHP 和／或 web 服务器。
如果不熟悉编译软件，可以考虑搜索一下是否有人已经编译了包含所需要功能的预编译包。

# PHP开发环境包括什么？


# 如何安装PHP开发环境？


# 如何使用源码编译安装PHP环境？


### 安装 OpenSSl :

> # 官网：
> http://www.openssl.org/
> # 简介：
> OpenSSL 是一个强大的安全套接字层密码库，囊括主要的密码算法、常用的密钥和证书封装管理功能及SSL协议，并提供丰富的应用程序供测试或其它目的使用。

> 下载：wget http://www.openssl.org/source/openssl-1.0.2a.tar.gz

> Mac:
> If configuring for 64-bit OS X, then use a command similar to:
> ./Configure darwin64-x86_64-cc enable-ec_nistp_64_gcc_128 no-ssl2 no-ssl3 no-comp --openssldir=/usr/local/ssl/macos-x86_64
> make
> sudo make install

> 参见：http://wiki.openssl.org/index.php/Compilation_and_Installation#Mac

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

> wget http://zlib.net/zlib-1.2.8.tar.gz

### 安装libpng:
> # 网站：
> http://libmng.com/pub/png/libpng.html
> #下载：
> http://sourceforge.net/projects/libpng/
> ftp://ftp.simplesystems.org/pub/libpng/png/src/libpng16/libpng-1.6.17.tar.gz
