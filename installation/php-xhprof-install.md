函数级分层性能分析工具-Xhprof的安装与使用


XHProf 是一个轻量级的分层性能测量分析器。

XHProf 包含了一个基于 HTML 的简单用户界面(由 PHP 写成)。 基于浏览器的用户界面使得浏览、分享性能数据结果更加简单方便。 同时也支持查看调用图。

XHProf 的报告对理解代码执行结构常常很有帮助。 比如此分层报告可用于确定在哪个调用链里调用了某个函数。

## 本机环境 

PHP 版本:

```
# 系统默认的php位置：
$ which php
/usr/local/bin/php
# 本机使用的php版本为php5.4(不使用系统自动的)
$ /usr/local/webserver/php54/bin/php -v
PHP 5.4.44 (cli) (built: May 20 2016 10:19:39)
Copyright (c) 1997-2014 The PHP Group
Zend Engine v2.4.0, Copyright (c) 1998-2014 Zend Technologies
```

## 使用 phpize 动态安装扩展

本机的Nginx和PHP等服务软件都安装在"/usr/local/webserver”目录下:

```
# 系统默认phpize
$ which phpize
/usr/local/bin/phpize

# 本机安装的phpize版本(不使用系统自动的)：
$ cd /usr/local/webserver/php54/bin/
$ ls
pear peardev pecl phar phar.phar php php-cgi php-config phpize
$ /usr/local/webserver/php54/bin/phpize
$ /usr/local/webserver/php54/bin/phpize -v
Configuring for:
PHP Api Version:         20100412
Zend Module Api No:      20100525
Zend Extension Api No:   220100525
```

下载与安装 xhprof：

```
$ cd /usr/local/webserver
$ wget http://pecl.php.net/get/xhprof-0.9.4.tgz
$ tar zxf xhprof-0.9.4.tgz
$ cd xhprof-0.9.4
//配置xhprof， 注意指定php-config地址
$ ./configure --with-php-config=/usr/local/webserver/php54/bin/php-config
$ sudo make
$ sudo make test
$ sudo make install
```
注意，--with-php-config 中php-config的位置，不同的环境中可能会有不同的路径。

## 配置 php.ini

本机的 PHP 配置文件 php.ini文件在: /usr/local/webserver/php54/php.ini :
```
$ ls -l
total 136
drwxr-xr-x  11 root  admin    374  5 20 10:26 bin
drwxr-xr-x   5 root  admin    170  5 20 12:53 etc
drwxr-xr-x   3 root  admin    102  5 20 10:22 include
drwxr-xr-x   3 root  admin    102  5 20 10:22 lib
drwxr-xr-x   4 root  admin    136  5 20 10:22 php
-rw-r--r--@  1 root  admin  65690  9  9 13:54 php.ini
drwxr-xr-x   3 root  admin    102  5 20 10:22 sbin
drwxr-xr-x   4 root  admin    136  5 20 10:22 var
```

在 php.ini 中末尾增加如下内容:

```
[xhprof]
extension=xhprof.so;
xhprof.output_dir=/tmp/xhprof
```

注意，xhprof.output_dir指定的目录 /tmp/xhprof/ 一定要有读写权限

然后重启PHP-fpm/Nginx:
```
$ sudo /usr/local/webserver/php54/sbin/php-fpm 
$ sudo /usr/local/webserver/nginx/nginx -s reload
```
注意，不同的环境于配置下，php与nginx的安装路径可能会有不同。

查看是否安装成功:
```
$ /usr/local/webserver/php54/bin/php -i |grep xhprof
xhprof
xhprof => 0.9.2
```

使用XHProf的时候，在点击[View Full Callgraph]查看结果分析图时，会报错.

    failed to execute cmd: " dot -Tpng". stderr: `sh: dot: command not found `  

原因是缺少graphviz绘图软件, 可以使用brew来安装graphviz。

```
$ brew search graphviz
$ brew install graphviz
```

Ubuntu环境可以使用:

```
sudo apt-get install graphviz
```

$ whereis dot
dot: /usr/bin/dot /usr/share/man/man1/dot.1.gz


## Xhprof的使用

嵌入PHP代码

```
<?php
// cpu:XHPROF_FLAGS_CPU 内存:XHPROF_FLAGS_MEMORY
// 如果两个一起：XHPROF_FLAGS_CPU + XHPROF_FLAGS_MEMORY
xhprof_enable(XHPROF_FLAGS_CPU + XHPROF_FLAGS_MEMORY);

//要测试的php代码
// … …

$data = xhprof_disable();   //返回运行数据

// xhprof_lib在下载的包里存在这个目录,记得将目录包含到运行的php代码中
require_once "xhprof_lib/utils/xhprof_lib.php";
require_once "xhprof_lib/utils/xhprof_runs.php";

$objXhprofRun = new XHProfRuns_Default();

// 第一个参数是xhprof_disable()函数返回的运行信息
// 第二个参数是自定义的命名空间字符串(任意字符串),
// 返回运行ID,用这个ID查看相关的运行结果
$run_id = $objXhprofRun->save_run($data, "xhprof");
var_dump($run_id);
```

查看生成的报告：

生成的报告，列表页，链接如下：
  http://localhost/wangyt/xhprof_html/index.php?run=$run_id&source=xhprof

页面中包含如下内容:
```
Existing runs:
    57d277ccf2464.xhprof.xhprof 2016-09-09 16:50:20
    57d277c9e32a3.xhprof.xhprof 2016-09-09 16:50:17
    57d27755dc7f5.xhprof.xhprof 2016-09-09 16:48:21
    ...
```

点击页面中的链接，会进入相应的报告详情页中.
比如点击第一个"57d277ccf2464", 链接如下:
  http://localhost/wangyt/xhprof_html/index.php?run=57d277ccf2464&source=xhprof

页面中会展示一个表格, 表格中会有以下参数指标：

> Function Name   函数名称  
> Calls   调用次数  
> Calls%  调用次数的百分比  
> Incl. Wall Time(microsec) : 包含时间, 包括子函数所有执行时间。  
> IWall%  包含时间所占总时间的百分比  
> Excl. Wall Time(microsec) : 独占时间, 函数本身消耗的时间,不包括子函数执行时间。  
> EWall%  独占时间所占总时间的百分比  

点击页面中的 [View Full Callgraph]，可以查看图像化的内容展示，链接如下:
  http://localhost/wangyt/xhprof_html/callgraph.php?run=57d277ccf2464


## 在 Homestead(PHP7)环境中安装 XHProf 

* 登录到 vagrant 虚拟机

    $ cd ~/homestead
    $ vagrant ssh

* 安装 XHProf 扩展
    
    cd ~/Code
    git clone https://github.com/longxinH/xhprof.git ./xhprof
    cd xhprof/extension/
    /usr/bin/phpize
    ./configure --with-php-config=/usr/bin/php-config
    make && sudo make install

* 创建一个目录，存放分析结果

    mkdir -p /tmp/xhprof/

* 在文件 `/etc/php/7.1/fpm/php.ini` 增加 xhprof 扩展配置: 

    $ sudo vi /etc/php/7.1/fpm/php.ini

    > [xhprof]
    > extension = xhprof.so
    > xhprof.output_dir = /tmp/xhprof 

    (xhprof.output_dir 内容就是我们刚刚创建的目录)

* 安装graphviz绘图软件 (可选)

    $ sudo apt-get install graphviz
    $ whereis dot
    dot: /usr/bin/dot /usr/share/man/man1/dot.1.gz

* 配置访问界面

在本机中增加host记录:

 $ sudo vi /etc/hosts
 192.168.10.10 xhprof.app

配置 Homestead.yaml 新增一个网站:

```
    ...
    sites:
    ...
    - map: xhprof.app
      to: /home/vagrant/Code/xhprof/xhprof_html  
    ...
```

重启 vagrant ：

    $ vagrant provision

访问: `http://xhprof.app` 即可。


## 参考链接：

(1) http://php.net/manual/zh/book.xhprof.php  
(2) http://pecl.php.net/package/xhprof  
(3) http://www.xhprof.com/  
(4) http://my.oschina.net/mickelfeng/blog/496902  
(5) http://avnpc.com/pages/profiler-php-performance-online-by-xhprof  
(6) https://www.goivvy.com/blog/xhprof-php-7

## 更新记录

(1) 20160909 整理本文内容
(2) 20160912 新增安装 graphviz 内容
(3) 20170912 新增Homestead(php7)下安装

[END]