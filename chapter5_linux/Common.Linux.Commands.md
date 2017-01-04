Common.Linux.Commands.md

常用的Linux命令



cd 

ls 

ls -al
ls -alh 

df -h



wget

curl




.tar.gz    
格式解压为 tar   -zxvf   xx.tar.gz

.tar.bz2   
格式解压为 tar   -jxvf    xx.tar.bz2


grep 

ps -ef |grep nginx

文件权限：

使用chown命令来改变文件所有者。chown命令是change owner（改变拥有者）的缩写
chown [-R] 账号名称 文件或目录
chown [-R] 账号名称:用户组名称 文件或目录

使用chgrp命令来改变文件所属用户组，该命令就是change group（改变用户组）的缩写：
chgrp [-R] 用户组名称 dirname/filename

chmod(变动文件属性)
文件属性的设置方式有两种，，别离是数字和标记。


find   ~   -name   "*.txt"   -print    #在$HOME中查.txt文件并显示

查看文件中的内容，显示其上下各3行：
$ cat ~/orp/webserver/conf/nginx.conf |grep -3 8248

添加所有的新增文件到SVN中
```
svn st | grep '^\?' | tr '^\?' ' ' | sed 's/[ ]*//' | sed 's/[ ]/\\ /g' | xargs svn add

svn st | awk '{if ( $1 == "?") { print $2}}' | xargs svn add
```

$ svn up --username wangyongtao --password 123456


```
//分行显示sql备份文件
perl -p -i -e "s/\),\(/\r\n/g" tb_channel.sql
```



Ptyhon: SimpleHTTPServer
```
//可以使用http://[IP地址]:8001/来访问web页面或共享文件
//http://127.0.0.1:8000/
$ sudo python -m SimpleHTTPServer 8001
```

PHP: 从5.4.0起， CLI SAPI 提供了一个内置的Web服务器

```
//启动Web服务器,当前目录
$ php -S localhost:8000
//指定目录 -t
$ php -S localhost:8000 -t foo/
//指定目录,可以通过ip远程访问 
$ php -S 0.0.0.0:8080:8000 -t foo/
```


### 在Mac中，在命令行中用sublime打开文件

//打开当前文件夹: 
$ /Applications/Sublime\ Text\ 2.app/Contents/SharedSupport/bin/subl ./

为Sublime做个软连接：
$ ls /Applications |grep "Sublime"
Sublime Text 2.app 
Sublime Text.app （这个是Sublime Text 3）

//比如为Sublime Text 2.app做软件接：
$ sudo ln -s /Applications/Sublime\ Text\ 2.app/Contents/SharedSupport/bin/subl /usr/local/bin/sublime
//比如为Sublime Text.app做软件接：
$ sudo ln -s /Applications/Sublime\ Text.app/Contents/SharedSupport/bin/subl /usr/local/bin/sublime
$ /usr/local/bin/sublime ./
$ sublime ./



使用dos2unix, unix2dos 命令实现 DOS <=> UNIX text file 转换

dos2unix 是将 Windows 格式文件转换为 Unix/Linux 格式的实用命令。
unix2dos 则是将 Unix/Linux 格式文件转换为Windows格式文件的命令。

Windows格式文件的换行符为\r\n ,而 Unix/Linux 文件的换行符为\n.
dos2unix 命令其实就是将文件中的\r\n 转换为 \n。
unix2dos 命令其实就是将文件中的\n 转换为 \r\n。

