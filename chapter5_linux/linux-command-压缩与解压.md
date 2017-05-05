linux-command-压缩与解压.md

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

根据php官网下载地址，可以看到目前提供了三种下载格式：


http://php.net/downloads.php

* php-7.1.4.tar.bz2 (sig) [15,343Kb] 13 Apr 2017

* php-7.1.4.tar.gz (sig) [19,843Kb] 13 Apr 2017

* php-7.1.4.tar.xz (sig) [12,494Kb] 13 Apr 2017

```
$ ls -al
total 95368
drwxr-xr-x  5 WangTom  staff       170  5  3 11:06 .
drwxr-xr-x  6 WangTom  staff       204  5  3 11:06 ..
-rw-r-----@ 1 WangTom  staff  15710808  5  3 11:03 php-7.1.4.tar.bz2
-rw-r-----@ 1 WangTom  staff  20319716  5  3 11:04 php-7.1.4.tar.gz
-rw-r-----@ 1 WangTom  staff  12793840  5  3 11:03 php-7.1.4.tar.xz
```

tar.xz 文件最小
tar.bz2 文件最小
tar.xz 文件最小

// 解压 tar.bz2

```
$ tar -xjf php-7.1.4.tar.bz2 
```

//解压 tar.gz

```
$ tar -xzf php-7.1.4.tar.gz
```

//解压 tar.xz

```
$ tar -xzf php-7.1.4.tar.xz
```