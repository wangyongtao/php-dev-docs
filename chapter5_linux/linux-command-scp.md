linux-command-scp.md

scp -- secure copy (remote file copy program)


* 上传文件到远程服务器:

```
// 默认
$ scp ~/demo/homestead_2017-06-12.sql root@wang123.net:/wwwdata/demo/

// 指定端口号，并重命名文件
$ scp -P 123456 ~/demo/homestead_2017-06-12.sql root@wang123.net:/wwwdata/demo/homestead.sql
```

scp -P 2233 root@106.14.34.47:/tmp/test.txt ./test111.txt 


* 从远程服务器下载文件: 

```
// 默认: 下载文件到 `~/demo/` 目录中 
$ scp root@wang123.net:/wwwdata/demo/homestead.sql ~/demo/
 
// 指定端口号，并重命名文件
$ scp -P 123456 root@wang123.net:/wwwdata/demo/homestead.sql ~/demo/homestead_20170620.sql 
```
 
常用参数: 


`-r`: Recursively copy entire directories. 递归拷贝(拷贝目录)   
`-P`: Specifies the port to connect to on the remote host. 指定远程服务器的端口号  
`-v`: Verbose mode. 显示进度. 可用来查看连接、认证或是配置问题  



