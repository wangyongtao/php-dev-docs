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
//可以使用http://[IP地址]:8000/来访问web页面或共享文件
$ sudo python -m SimpleHTTPServer 80
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




