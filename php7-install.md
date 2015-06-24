php7-install.md  

使用源码编译安装PHP7

2015年6月11日，PHP官网发布消息，正式公开发布PHP7第一版的alpha版本.

### PHP7特性：

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

### 系统环境：
```
Mac-mini:~ WangTom$ uname -mnprs
Darwin Mac-mini.local 14.3.0 x86_64 i386

Mac-mini:~ WangTom$ sw_vers
ProductName:	Mac OS X
ProductVersion:	10.10.3
BuildVersion:	14D136
```

### 源码安装PHP7:

PHP7下载地址：https://downloads.php.net/~ab/

```
$ wget https://downloads.php.net/~ab/php-7.0.0alpha1.tar.bz2
$ tar jxf php-7.0.0alpha1.tar.bz2
$ cd php-7.0.0alpha1

$ ./configure
... ...
checking size of long... (cached) 8
checking size of long long... (cached) 8
checking for iconv support... yes
checking for iconv... no
checking for libiconv... no
configure: error: Please specify the install prefix of iconv with --with-iconv=<DIR>

```

安装 libiconv (字符编码转换库)
网站地址: http://www.gnu.org/software/libiconv/
当前版本: http://ftp.gnu.org/pub/gnu/libiconv/libiconv-1.14.tar.gz

```
$ wget http://ftp.gnu.org/pub/gnu/libiconv/libiconv-1.14.tar.gz
$ tar zxf libiconv-1.14.tar.gz
$ cd libiconv-1.14
$ ./configure --prefix=/usr/local/lib/libiconv
$ make
$ sudo make install
```


配置参数
```
$ ./configure --prefix=/usr/local/php7 \
--enable-fpm \
--with-config-file-path=/usr/local/php7/etc \
--with-iconv=/usr/local/lib/libiconv \

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
WangTomdeMacBook-Pro:php-7.0.0alpha1 wangtom$ /usr/local/php7/bin/php -v
PHP 7.0.0alpha1 (cli) (built: Jun 20 2015 00:04:19) 
Copyright (c) 1997-2015 The PHP Group
Zend Engine v3.0.0-dev, Copyright (c) 1998-2015 Zend Technologies

Mac-mini:~ WangTom$ sudo ln -s /usr/local/php7/bin/php /usr/bin/php7

Mac-mini:~ WangTom$ php -v
PHP 5.5.20 (cli) (built: Feb 25 2015 23:30:53)
Copyright (c) 1997-2014 The PHP Group
Zend Engine v2.5.0, Copyright (c) 1998-2014 Zend Technologies

Mac-mini:~ WangTom$ php7 -v
PHP 7.0.0alpha1 (cli) (built: Jun 23 2015 17:24:34)
Copyright (c) 1997-2015 The PHP Group
Zend Engine v3.0.0-dev, Copyright (c) 1998-2015 Zend Technologies

Mac-mini:php-fpm.d WangTom$ which php
/usr/bin/php
Mac-mini:php-fpm.d WangTom$ which php7
/usr/bin/php7
```

### 配置PHP-FPM： 

Nginx不支持对外部程序的直接调用或者解析，所有的外部程序（包括PHP）必须通过FastCGI接口来调用。
PHP-FPM是一个PHP FastCGI管理器,新版的PHP已经集成了php-fpm，在./configure的时候带 –enable-fpm参数即可开启PHP-FPM.
FPM (FastCGI Process Manager) is an alternative PHP FastCGI implementation with some additional features (mostly) useful for heavy-loaded sites.

启动 PHP-FPM: 
```
Mac-mini:php7 WangTom$ /usr/local/php7/sbin/php-fpm
[23-Jun-2015 15:33:01] WARNING: Nothing matches the include pattern '/usr/local/php7/etc/php-fpm.d/*.conf' from /usr/local/php7/etc/php-fpm.conf at line 125.
[23-Jun-2015 15:33:01] ERROR: failed to open error_log (/usr/local/php7/var/log/php-fpm.log): Permission denied (13)
[23-Jun-2015 15:33:01] ERROR: failed to post process the configuration
[23-Jun-2015 15:33:01] ERROR: FPM initialization failed
```
提示错误说/usr/local/php7/var/log/php-fpm.log 没权限，就给777权限：
$ chmod 777 /usr/local/php7/var/log/

修改 php-fpm 配置文件：
$ cd /usr/local/php7/etc/
$ cp php-fpm.conf.default php-fpm.conf
$ vim php-fpm.conf
  > 打开 error_log这一行的注释，默认该项被注释掉，若不修改会出现提示log文件路径不存在
  > error_log = /usr/local/php7/var/log/php-fpm.log 
  > 打开inclue这一行的注释
  > include=/usr/local/php7/etc/php-fpm.d/*.conf

修改 /usr/local/php7/etc/php-fpm.d/www.conf 文件：
如果这个文件不存在,就从default复制一份：
$ cd /usr/local/php7/etc/php-fpm.d/
$ cp www.conf.default www.conf
将配置文件中的 user 和 group 部分的 nobody 改成 www:
$ vim /usr/local/php7/etc/php-fpm.d/www.conf
  > user  = www  
  > group = www  

开始启动 php-fpm：
```
Mac-mini:php-7.0.0alpha1 WangTom$ /usr/local/php7/sbin/php-fpm
[23-Jun-2015 18:30:48] NOTICE: [pool www] 'user' directive is ignored when FPM is not running as root
[23-Jun-2015 18:30:48] NOTICE: [pool www] 'group' directive is ignored when FPM is not running as root
[23-Jun-2015 18:30:48] ERROR: unable to bind listening socket for address '127.0.0.1:9000': Address already in use (48)
[23-Jun-2015 18:30:48] ERROR: FPM initialization failed
```
这个错误问题有两个：(1)没有使用root账户执行启动命令 (2)端口9000被占用
解决方法：
使用root账户执行php-fpm启动，或 sudo /usr/local/php7/sbin/php-fpm  
关闭 PHP-fpm, 并重新启动：
```
Mac-mini:~ WangTom$ lsof -P | grep ':9000' | awk '{print $2}' | xargs kill -9
Mac-mini:php-7.0.0alpha1 WangTom$ /usr/local/php7/sbin/php-fpm -t
[23-Jun-2015 18:30:25] NOTICE: configuration file /usr/local/php7/etc/php-fpm.conf test is successful
Mac-mini:~ WangTom$ sudo /usr/local/php7/sbin/php-fpm
Mac-mini:~ WangTom$ 
```

修改Nginx 配置： 
在 nginx.conf 配置文件server 部分增加fastcgi配置，并重新加载配置文件：
```
Mac-mini:~ WangTom$ sudo vim /usr/local/nginx/conf/nginx.conf

> location ~ \.php$ {
>  root html;
>  fastcgi_pass 127.0.0.1:9000;
>  fastcgi_index index.php;
>  fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
>  include fastcgi_params;
> }

Mac-mini:~ WangTom$ sudo /usr/local/nginx/sbin/nginx -t
nginx: the configuration file /usr/local/nginx/conf/nginx.conf syntax is ok
nginx: configuration file /usr/local/nginx/conf/nginx.conf test is successful
Mac-mini:~ WangTom$ sudo /usr/local/nginx/sbin/nginx -s reload
Mac-mini:~ WangTom$
```

用到的一些命令：

uname: 用来获取电脑和操作系统的相关信息
sw_vers: Mac下查看系统版本信息
lsof: 列出当前系统打开文件（list open files）
which: 指令会在环境变量$PATH设置的目录里查找符合条件的文件


参考链接：

​http://php.net/archive/2015.php#id2015-06-11-3



[END]
