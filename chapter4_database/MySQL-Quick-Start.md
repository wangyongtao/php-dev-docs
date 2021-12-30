MySQL-Quick-Start.md

MySQL是一个关系型数据库管理系统，由瑞典 MySQL AB 公司1995年发布，2008年被 Sun Microsystems 公司收购，2010年，Oracle 收购 Sun Microsystems， 目前属于Oracle公司。


$ mysql -u root -p



# 修改表结构：

## 新增表字段:

```
ALTER TABLE `test_invoice`
ADD COLUMN `taxpayer_name` varchar(50) NOT NULL DEFAULT '' COMMENT '纳税人名称',
ADD COLUMN `taxpayer_id` varchar(50) NOT NULL DEFAULT '' COMMENT '纳税人识别号',
```

## 修改表字段:

```
ALTER TABLE `test_invoice` 
CHANGE COLUMN `type` `type` tinyint(1) NOT NULL DEFAULT '0' COMMENT '类型：1-类型1,2-类型2';
```

# mysqldump


* 导出全部数据库及表
$ mysqldump -u root -p homestead > mysql_backup_201706.sql
 

* 导出部分数据库及表
$ mysqldump --databases db1 db2 db3 > /tmp/dump.sql  




# Reference:

https://dev.mysql.com/doc/refman/5.6/en/mysqldump.html