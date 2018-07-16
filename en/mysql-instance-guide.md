## Compute > Instance > Instance Guide > MySQL Instance Guide
## MySQL version

##### MySQL of TOAST Cloud is classified into the following two versions: 

* MySQL Community Server 5.6.38
    * mysql-community-server-5.6.38-2.el6.x86_64
* MySQL Community Server 5.7.20
    * mysql-community-server-5.7.20-1.el6.x86_64

## How to Start/Stop MySQL 

```
#Start mysql Service 
shell> service mysqld start

#Stop mysql Service 
shell> service mysqld stop

#Restart mysql Service 
shell> service mysqld restart
```

## Access MySQL 

Access as below initially after image is created.

```
shell> mysql -uroot
```

## Initial Setting after MySQL Image Created 

### 1\. Change Passwords 

Passwords for MySQL Root account is not specified after initial installation. Therefore, password change is required after installation.  

* Change Passwords for MySQL 5.6 Version  

```
SET PASSWORD [FOR user] = password_option

mysql> set password=password('password');
```

* Change Passwords for MySQL 5.7  

```
ALTER USER USER() IDENTIFIED BY 'auth_string';

mysql> ALTER USER 'root'@'localhost' IDENTIFIED BY 'New Password';
```

### Default MySQL validate_password_policy is as below:

* validate\_password\_policy=MEDIUM
* Must be more than 8 characters, including numbers, lower/upper cases, and special characters.

### 2\. Change Ports

#### The default image port is 3306. It is recommended to change port for security reasons. 

```
shell> vi /etc/my.cnf


# Specify a port to use in the my.cnf file. 

port = Port name to use 


# Save vi editor Save editor 


# Restart mysql service  


shell> service mysqld restart


#Access as below  with changed port


shell> mysql -uroot -P[changed port number]
```

## Description of my.cnf 

The default route of my.cnf is /etc/my.cnf, where variables are set as recommended by ToastCloud, like below: 

| Name | Description |
| --- | --- |
| default\_storage\_engine | Specify a default storage engine: specified in InnoDB and transaction with online DDL is available. |
| expire\_logs\_days | Set log saving dates accumulated with binlog setting. Default is three days. |
| innodb\_log\_file\_size | Specify the size of log files which save redo logs of transaction. <br>Recommended for 256MB or higher in actual environment, while it is currently set at 512MB. When setting changes, DB restart is required. |
| innodb\_file\_per\_table | When a table is deleted or truncated, the table space is immediately returned to OS. |
| innodb\_log\_files\_in\_group | Set the number of innodb\_log\_file files and use them for circular purpose: composed with at least two. |
| log_timestamps | Default log time of MySQL 5.7 is displayed as UTC; therefore, change log time to system local time. |
| slow\_query\_log | Enable the slow\_query log option. Queries of more than 10 seconds in accordance with long_query_time remains in the slow_query_log. |
| sysdate-is-now | For sysdate, SQL with sysdate() used for replication results in different time between Master and Slave, and functions for sysdate() and now() shall be applied the same. |

## Description of MySQL Directory 

Directory and file description of MySQL are as below: 

| Name | Description |
| --- | --- |
| my.cnf | /etc/my.cnf |
| DATADIR | Route for MySQL Data File  - /var/lib/mysql/ |
| ERROR_LOG | Route for MySQL error_log File  - /var/log/mysqld.log |
| SLOW_LOG | Route for MySQL Slow Query File -  <span style="color:#333333">/var/lib/mysql/*slow.log</span> |

