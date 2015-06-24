php7-install.md

2015年6月11日，PHP官网发布消息，正式公开发布PHP7第一版的alpha版本.

PHP7特性：

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

源码安装PHP7:

$ cat /System/Library/CoreServices/SystemVersion.plist
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
	<key>ProductBuildVersion</key>
	<string>14D136</string>
	<key>ProductCopyright</key>
	<string>1983-2015 Apple Inc.</string>
	<key>ProductName</key>
	<string>Mac OS X</string>
	<key>ProductUserVisibleVersion</key>
	<string>10.10.3</string>
	<key>ProductVersion</key>
	<string>10.10.3</string>
</dict>
</plist>

PHP7下载地址：https://downloads.php.net/~ab/

```
$ wget https://downloads.php.net/~ab/php-7.0.0alpha1.tar.bz2
$ tar jxf php-7.0.0alpha1.tar.bz2
$ ./configure
... ...
checking size of long... (cached) 8
checking size of long long... (cached) 8
checking for iconv support... yes
checking for iconv... no
checking for libiconv... no
configure: error: Please specify the install prefix of iconv with --with-iconv=<DIR>
WangTomdeMacBook-Pro:php-7.0.0alpha1 wangtom$ 
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
$ ./configure --prefix=/usr/local/php7 \
--with-config-file-path=/usr/local/php7/etc \
--with-iconv=/usr/local/lib/libiconv \

配置成功后，可以看到有如下结果：
```
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

````
WangTomdeMacBook-Pro:php-7.0.0alpha1 wangtom$ /usr/local/php7/bin/php -v
PHP 7.0.0alpha1 (cli) (built: Jun 20 2015 00:04:19) 
Copyright (c) 1997-2015 The PHP Group
Zend Engine v3.0.0-dev, Copyright (c) 1998-2015 Zend Technologies
```

参考链接：

​http://php.net/archive/2015.php#id2015-06-11-3






[END]
