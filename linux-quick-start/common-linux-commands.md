Common.Linux.Commands.md

常用的Linux命令


kill -- terminate or signal a process

Some of the more commonly used signals:
     1       HUP (hang up)
     2       INT (interrupt)
     3       QUIT (quit)
     6       ABRT (abort)
     9       KILL (non-catchable, non-ignorable kill)
     14      ALRM (alarm clock)
     15      TERM (software termination signal)
cd 

* ls 

    ls -al
    ls -alh 

* 检查文件系统的磁盘空间占用情况 df  

    $ df -h
    Filesystem      Size   Used  Avail Capacity iused      ifree %iused  Mounted on
    /dev/disk2     1.0Ti  348Gi  681Gi    34% 5524742 4289442537    0%   /
    devfs          191Ki  191Ki    0Bi   100%     661          0  100%   /dev
    map -hosts       0Bi    0Bi    0Bi   100%       0          0  100%   /net
    map auto_home    0Bi    0Bi    0Bi   100%       0          0  100%   /home

* 查询目录文件的磁盘使用空间 du 
    
    // 查看当前目录下的大小
    $ du -sh
    355M    .
    
    // 查看指定目录 vendor 的大小 
    $ du -sh vendor 
    46M    vendor

    // 安装文件大小排序
    $ du ./app/ | sort -nr | more


wget

curl



## 压缩与解压  

.tar.gz    
格式解压为 tar   -zxvf   xx.tar.gz

.tar.bz2   
格式解压为 tar   -jxvf    xx.tar.bz2

解压
tar –xvf file.tar //解压 tar包
tar -xzvf file.tar.gz //解压tar.gz
tar -xjvf file.tar.bz2   //解压 tar.bz2
tar –xZvf file.tar.Z   //解压tar.Z
unrar e file.rar //解压rar
unzip file.zip //解压zip

unzip accounts.zip
gunzip logs.jsonl.gz



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



### 简单的WEB服务器

* Ptyhon: SimpleHTTPServer
    
可以使用http://[IP地址]:8001/来访问web页面或共享文件

    ```
    //http://127.0.0.1:8000/
    $ sudo python -m SimpleHTTPServer 8001
    ```

* PHP 内置的Web服务器

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


* [dos2unix / unix2dos]

使用dos2unix, unix2dos 命令实现 DOS <=> UNIX text file 转换

  dos2unix 将 Windows 文件格式(\r\n) 转换为 Unix/Linux 文件格式(\n)。
  unix2dos 将 Unix/Linux 文件格式(\n)转换为 DOS/Windows 文件格式(\r\n)。

  UNIX/Linux 使用 `0x0A`（LF)作为换行符(\n); DOS/Windows 使用 `0x0D0A`（CRLF）作为换行符 (\r\n).


| Command                            | Description                              |
| ---------------------------------- | ---------------------------------------- |
| cat [filename]                     | Display file’s contents to the standard output device(usually your monitor). |
| cd /directorypath                  | Change to directory.                     |
| chmod [options] mode filename      | Change a file’s permissions.             |
| chown [options] filename           | Change who owns a file.                  |
| clear                              | Clear a command line screen/window for a fresh start. |
| cp [options] source destination    | Copy files and directories.              |
| date [options]                     | Display or set the system date and time. |
| df [options]                       | Display used and available disk space.   |
| du [options]                       | Show how much space each file takes up.  |
| file [options] filename            | Determine what type of data is within a file. |
| find [pathname] [expression]       | Search for files matching a provided pattern. |
| grep [options] pattern [filesname] | Search files or output for a particular pattern. |
| kill [options] pid                 | Stop a process. If the process refuses to stop, use kill -9 pid. |
| less [options] [filename]          | View the contents of a file one page at a time. |
| ln [options] source [destination]  | Create a shortcut.                       |
| locate filename                    | Search a copy of your filesystem for the specifiedfilename. |
| lpr [options]                      | Send a print job.                        |
| ls [options]                       | List directory contents.                 |
| man [command]                      | Display the help information for the specified command. |
| mkdir [options] directory          | Create a new directory.                  |
| mv [options] source destination    | Rename or move file(s) or directories.   |
| passwd [name [password]]           | Change the password or allow (for the system administrator) tochange any password. |
| ps [options]                       | Display a snapshot of the currently running processes. |
| pwd                                | Display the pathname for the current directory. |
| rm [options] directory             | Remove (delete) file(s) and/or directories. |
| rmdir [options] directory          | Delete empty directories.                |
| ssh [options] user@machine         | Remotely login to another Linux machine.Leave an ssh session by typing exit. |
| su [options] [user [arguments]]    | Switch to another user account.          |
| tail [options] [filename]          | Display the last *n* lines of a file (the default is10). |
| tar [options] filename             | Store and extract files from a tarfile (.tar) or tarball (.tar.gz or.tgz). |
| top                                | Displays the resources being used on your system. Press q toexit. |
| touch filename                     | Create an empty file with the specified name. |
| who [options]                      | Display who is logged on.                |

-- from [COMMON LINUX COMMANDS](http://www.dummies.com/computers/operating-systems/linux/common-linux-commands/)



[END]