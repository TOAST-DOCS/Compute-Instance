## Compute > Instance > 专业实例使用指南 > MySQL Instance指南
## MySQL version

我们提供以下2种MySQL版本。

* MySQL Community Server 5.6.38
    * mysql-community-server-5.6.38-2.el6.x86_64
* MySQL Community Server 5.7.20
    * mysql-community-server-5.7.20-1.el6.x86_64

## 如何启动/停止MySQL

```
#启动mysql服务
shell> service mysqld start

#停止mysql服务
shell> service mysqld stop

#重启mysql服务
shell> service mysqld restart
```

## 访问MySQL

镜像创建初始，请按照以下方式访问。

```
shell> mysql -uroot
```

## MySQL镜像创建后的初始设置

### 1\. 更改密码

初始安装后MySQL ROOT账号密码未被指定。因此，安装后必须及时更改密码。

* MySQL 5.6版本更改密码

```
SET PASSWORD [FOR user] = password_option

mysql> set password=password('密码');
```

* MySQL 5.7版本更改密码

```
ALTER USER USER() IDENTIFIED BY 'auth_string';

mysql> ALTER USER 'root'@'localhost' IDENTIFIED BY '新密码';
```

MySQL默认validate\_password\_policy如下。\.

* validate\_password\_policy=MEDIUM
* 必须包含**至少8个字符、数字、小写，大写文字、特殊符号**

### 2\. 更改端口(port)

所提供的镜像端口为MySQL默认端口3306。出于安全性的考虑，我们建议您更改端口。

```
shell> vi /etc/my.cnf


#在my.cnf文件中指定要使用的端口。

port = 端口号


#vi 保存编辑器


#重启mysql服务


shell> service mysqld restart


#请按照以下方式连接变更后的端口


shell> mysql -uroot -P[变更后的端口号]
```

## my.cnf描述

my.cnf的默认路径为/etc/my.cnf，并设置有TOAST推荐变量(variable)，内容如下。

| 名称 | 描述 |
| --- | --- |
| default\_storage\_engine | 指定默认储存引擎(stroage engine)。 它被指定为InnoDB，可以使用Online-DDL和事务(transaction)。|
| expire\_logs\_days | 为binlog堆积日志设置日志保存时间。默认设置为3天。 |
| innodb\_log\_file\_size | 指定存储事务(transaction)的redo log的日志文件大小。 <br><br>在实际运营环境中,建议使用256MB以上，当前设置为512MB。如果要更改设置，需重启DB。|
| innodb\_file\_per\_table | 当表被删除或TRUNCATE时，空间将被释放到OS。|
| innodb\_log\_files\_in\_group | 设置innodb\_log\_file文件数，并循环\(circular\)使用。\. 至少由2个构成。\. |
| log_timestamps | MySQL 5.7的默认日志时间以UTC显示。因此，需将日志时间更改为SYSTEM本地时间。|
| slow\_query\_log | 使用slow\_query log选项\. 基于long\_query\_time的默认10秒以上的查询将被记录到slow\_query\_log中。\. |
| sysdate-is-now | sysdate使用sysdate()和now()函数，因为在replication中使用sysdate()的SQL语句时，主服务器与从服务器之间的时间不同。|

## MySQL列表描述

MySQL列表和文件描述如下所示。

| 名称 | 描述 |
| --- | --- |
| my.cnf | /etc/my.cnf |
| DATADIR | MySQL数据文件路径 - /var/lib/mysql/ |
| ERROR_LOG | MySQL error_log文件路径 - /var/log/mysqld.log |
| SLOW_LOG | MySQL Slow Query文件路径 -  <span style="color:#333333">/var/lib/mysql/*slow.log</span> |
