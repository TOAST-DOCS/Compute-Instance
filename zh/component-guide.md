## Compute > Instance > Installation Component Guide

## NAT Instance
NAT instance is an instance that allows you to access internet over a specific IP address band in the private network instance.
This feature is available only in the Pyeongchon region, Korea.

### Key Features
* The instance of a private network not connect with an internet gateway can access the internet via the NAT instance.
* Accesses the Internet by changing the floating IP of the NAT instance to source IP.
* The packets delivered to the NAT instance are delivered according to the routing setting of the routing table connected to the subnet of NAT instance.
* Do not add a route setting that specifies the NAT instance as a gateway in the routing table connected to the subnet of the NAT instance.
* The instance of a private network cannot receive the inbound traffic coming from the internet.
* Specifies IP address bandwidth of destination to allow Internet access through the NAT instance.
* Allows access to another instance by accessing the NAT instance.
* Sets security group.
* Sets network ACL.
* Redundancy not supported.
* The option of checking network source/target must be disabled in the Network Interface settings for the NAT instance to work.
* If a private image is created with NAT instance, the function might not work normally.

> [Note] Difference with NAT gateway
>
> | Classification | NAT gateway | NAT instance |
> |--|--|--|
> |Availability| Redundancy supported | Redundancy not supported |
> |Maintenance|Maintained by NHN Cloud| Directly maintained by users|
> |Security group|Not settable| Settable|Amount
> |Network ACL| Settable | Settable|
> |SSH|Unavailable| Available|

### Source/target check setting
For the NAT instance to work normally, the option of checking network source/target must be disabled in the Network Interface settings.

### Routing setting
Specifies the NAT instance as a route gateway. The packets delivered to the NAT instance are delivered according to the routing setting of the routing table connected to the subnet of NAT instance.

### Caution on settings
* Do not add a routing setting that specifies the NAT instance as a gateway in the routing table connected to the subnet of the NAT instance.
* It is strongly recommended to separate the NAT instance subnet from the subnet of an instance that is going to use the NAT instance as a gateway and use a different routing table.
* If the NAT instance subnet and the subnet of an instance that is going to use the NAT instance as a gateway are the same or they are connected to the same routing table, and you have to add a routing setting that specifies the NAT instance as a gateway, then a floating IP must be set for the NAT instance. Also, the target CIDR must be set to IP Prefix 0 (/0). Any other settings will only cause the communication to fail, and might impact the communication of all instances that use the routing table.

> [Note] Routing setting for NAT instance
>
> * If the subnet of NAT instance is connected to Routing Table 1 and the instances that are going to use the NAT instance as a gateway are connected to Routing Table 2
>     * In the routing setting of Routing Table 2, the NAT instance can be specified as a gateway for a specific CIDR (e.g. 8.8.8.8/32).
>     * You should specify the NAT instance as a gateway in the route setting of Routing Table 1 except in the case of the NAT instance connected to a floating IP, in which case IP prefix 0 (/0) can be set for the target CIDR.
> * If the NAT instance subnet and the subnet of instances that are going to use the NAT instance as a gateway are both connected to Routing Table 1
>     * If NAT instance is connected to a floating IP, IP Prefix 0 (/0) can be set for the target CIDR to route.
>     * Without using the above settings, you should not specify the NAT instance as a gateway in the routing setting of Routing Table 1.


## MS-SQL Instance
创建实例完成后，利用RDP（Remote Desktop Protocol）访问实例。
Floating IP应连接至实例，安全组中应允许使用TCP端口3389（RDP）。
单击**+确认密码**按钮创建实例时，使用设置的密钥对确认密码。

![mssqlinstance_02_201812](https://static.toastoven.net/prod_ms_sql/mssqlinstance_02_201812.png)

单击**连接**按钮，下载.rdp文件后，利用获得的密码连接到实例。

### 创建MS-SQL镜像后进行初始设置

#### 1.设置SQL验证模式

服务器的默认验证模式为“Windows验证模式”。 
为使用MS-SQL的数据库账户，需要更改为SQL验证模式。 

执行Microsoft SQL Server Management Studio，通过实例名连接到对象。

![mssqlinstance_03_201812](https://static.toastoven.net/prod_ms_sql/mssqlinstance_03_201812.png)

1.右击对象。
2.在菜单中选择**属性**。
3.在服务器属性窗口中选择**安全**菜单。
4.将**服务器验证**方式更改为“SQL Server及Windows验证模式”。

※ 设置SQL验证模式后，为进行应用，应重启MS-SQL服务。 

#### 2.更改MS-SQL服务端口

MS-SQL的默认服务端口1433是广为人知的端口，所以有可能成为安全漏洞。
建议更改为其他端口。
※ Express的情况无法指定默认端口。 

运行SQL Server配置管理器。

![mssqlinstance_04_201812](https://static.toastoven.net/prod_ms_sql/mssqlinstance_04_201812.png)

1.在左侧面板中选择**SQL Server网络配置**的下级项目**MSSQLSERVER相关协议**。
2.右击协议名中的**TCP/IP**。
3.在菜单中选择**属性**。
4.单击**IP地址**标签。
5.在下拉菜单中选择**IP ALL**后，更改为其他端口号。

※ 更改MS-SQL服务端口后，为进行应用，应重启MS-SQL服务。 

#### 3.设置允许从外部连接MS-SQL数据库

为从外部连接MS-SQL数据库，应在**Network > Security Group**中将MS-SQL服务端口添加到Security Group。 
添加Security Group时，注册允许连接的MS-SQL服务端口（默认端口：1433）及远程IP。 

### 分配数据卷

MS-SQL的数据/日志文件（MDF/LDF）、备份文件建议使用另外的Block Storage。
若欲创建Block Storage，在**Compute > Instance > Block Storage**标签中单击+创建Block Storage按钮。

![mssqlinstance_05_201812](https://static.toastoven.net/prod_ms_sql/mssqlinstance_05_201812.png)

创建Block Storage时，为保证性能，Volume类型推荐“通用SSD”。

![mssqlinstance_06_201812](https://static.toastoven.net/prod_ms_sql/mssqlinstance_06_201812.png)

完成创建Block Storage并选择Storage后，单击**连接管理**按钮，连接到实例。

<br/>

使用RDP连接到实例，执行**计算机管理**，跳转至**存储位置>磁盘管理**。

![mssqlinstance_07_201812](https://static.toastoven.net/prod_ms_sql/mssqlinstance_07_201812.png)

可以确认是否检测到已连接的Block Storage。为进行使用，应先对磁盘执行初始化。
1.右击**磁盘1**块后，单击**初始化磁盘**。
2.选择分区格式后，单击**确定**按钮。

<br/>

初始化完成后，创建磁盘卷。

![mssqlinstance_08_201812](https://static.toastoven.net/prod_ms_sql/mssqlinstance_08_201812.png)

右击未分配的磁盘后，单击**新建简单卷**，运行新建简单卷程序。

<br/>

在Microsoft SQL Server Management Studio服务器属性的数据库设置中，将数据库默认位置更改为创建的卷的目录。

![mssqlinstance_09_201812](https://static.toastoven.net/prod_ms_sql/mssqlinstance_09_201812.png)

※ 更改MS-SQL数据库默认位置后，为进行应用，应重启MS-SQL服务。 

### 重启MS-SQL服务
更改MS-SQL设置时，有时需要重启MS-SQL服务。
为应用更改的设置，重启MS-SQL服务。 

选择SQL Server配置管理器的**SQL Server配置管理器（本地）>SQL Server服务>SQL Server（MSSQLSERVER）**后，右击，在显示的菜单中选择“重启”重启MS-SQL服务。

![mssqlinstance_10_201812](https://static.toastoven.net/prod_ms_sql/mssqlinstance_10_201812.png)

### 确认/设置自动执行MS-SQL服务
确认MS-SQL的服务是否设置为OS驱动时自动启动。 

在SQL Server配置管理器的SQL Server配置管理器（本地）>SQL Server服务中可确认“启动模式”。 

![mssqlinstance_11_201812](https://static.toastoven.net/prod_ms_sql/mssqlinstance_11_201812.png)

**SQL SERVER (MSSSQLSERVER)**及**SQL Server代理程序（MSSQLSERVER）**等的服务启动模式不是**自动**时：
1.右击相应服务后，选择**属性**。
2.在**服务**标签中将**General>启动模式**更改为**自动**。

> 【参考】
> MS-SQL Instance的发布现状请参考【实例发布说明】(/Compute/Compute/ko/release-notes/)。

## MySQL Instance
### 如何启动/停止MySQL

```
#启动mysql服务
shell> service mysqld start

#停止mysql服务
shell> service mysqld stop

#重启mysql服务
shell> service mysqld restart
```

### 访问MySQL

镜像创建初始，请按照以下方式访问。

```
shell> mysql -uroot
```

### MySQL镜像创建后的初始设置

#### 1\. 更改密码

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

#### 2\. 更改端口(port)

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

### my.cnf描述

my.cnf的默认路径为/etc/my.cnf，并设置有NHN Cloud推荐变量(variable)，内容如下。

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

### MySQL列表描述

MySQL列表和文件描述如下所示。

| 名称 | 描述 |
| --- | --- |
| my.cnf | /etc/my.cnf |
| DATADIR | MySQL数据文件路径 - /var/lib/mysql/ |
| ERROR_LOG | MySQL error_log文件路径 - /var/log/mysqld.log |
| SLOW_LOG | MySQL Slow Query文件路径 -  <span style="color:#333333">/var/lib/mysql/*slow.log</span> |

## PostgreSQL Instance

## CUBRID Instance
### How to Start/Stop the CUBRID service

You can start or stop the CUBRID service as follows by logging in with the “cubrid” Linux account.
```
# Start the CUBRID service/server
shell> sudo su - cubrid
shell> cubrid service start 
shell> cubrid server start demodb

# Stop the CUBRID service/server
shell> sudo su - cubrid
shell> cubrid server stop demodb
shell> cubrid service stop 

# Restart the CUBRID service/server
shell> sudo su - cubrid
shell> cubrid server restart demodb
shell> cubrid service restart 

# Start/stop/restart the CUBRID broker
shell> sudo su - cubrid
shell> cubrid broker start
shell> cubrid broker stop
shell> cubrid broker restart
```

### Connect to CUBRID

After creating an instance, initially connect as follows.
```
shell> sudo su - cubrid
shell> csql -u dba demodb@localhost
```

### Initial Setup After Creating a CUBRID Instance

#### 1\. Set the Password

After initial installation, the CUBRID dba account password is not set. Therefore, you must set a password after installation.
```
shell> csql -u dba -c "ALTER USER dba PASSWORD 'new_password'" demodb@localhost
```

#### 2\. Change the Broker Port

The broker port for **query_editor** defaults to **30000**, and the broker port for **broker1** defaults to **33000**.
For security reasons, it is recommended to change the port.

###### 1) Modify the broker file

Open the following file and enter the port address to change as shown below.
```
shell> vi /opt/cubrid/conf/cubrid_broker.conf

[%query_editor]
BROKER_PORT             =[port address to change]

[%BROKER1]
BROKER_PORT             =[port address to change]
```

###### 2) Restart the broker

Restart the broker for the port change to take effect.
```
shell> cubrid broker restart
```

#### 3\. Change the Manager Server Port

The manager server port defaults to **8001**.
For security reasons, it is recommended to change the port.

###### 1)  Modify the cm.conf file

Open the following file and enter the port address to change as shown below.
```
shell> vi /opt/cubrid/conf/cm.conf

cm_port =[port address to change]
```

###### 2) Restart the manager server

Restart the manager for the port change to take effect.
```
shell> cubrid manager stop
shell> cubrid manager start
```

### CUBRID Directory Description

The CUBRID directory and file descriptions are as follows.

| Name | Description |
| --- | --- |
| database.txt | CUBRID database location information file path - /opt/cubrid/databases |
| CONF PATH | CUBRID server, broker, manager environment variable file path - /opt/cubrid/conf |
| LOG PATH | CUBRID process log file path - /opt/cubrid/log |
| SQL\_LOG | CUBRID SQL Query file path /opt/cubrid/log/broker/sql\_log |
| ERROR\_LOG | CUBRID ERROR SQL Query file path - /opt/cubrid/log/broker/error\_log |
| SLOW\_LOG | CUBRID Slow Query file path - /opt/cubrid/log/broker/sql\_log |

#### cubrid.conf Description

A server configuration file that allows you to configure the memory of the database you want to operate, the number of threads according to the number of concurrent users, and the communication port between the broker and the server.

| Name | Description |
| --- | --- |
| service  | A parameter to register processes that start automatically when the CUBRID service starts.<br>By default, server, broker, and manager processes are registered. |
| cubrid\_port\_id | The port used by the master process. |
| max\_clients | The maximum number of concurrently connected clients per database server process. |
| data\_buffer\_size | A parameter to set the size of the data buffer that the database server caches in memory.<br>It is recommended to set the required memory size to a value within 2/3 of the system memory. |

#### broker.conf Description

A broker configuration file that allows you to set the port used by the broker you want to operate, the number of application servers (CAS), SQL LOG, etc.

| Name | Description |
| --- | --- |
| BROKER\_PORT | The port used by the broker. The port seen by the actual driver such as JDBC is the port of the broker. |
| MAX\_NUM\_APPL\_SERVER | A parameter to set the maximum number of CASs that can be connected to the broker at the same time. |
| MIN\_NUM\_APPL\_SERVER | A parameter to set the minimum number of CAS processes waiting by default even if there is no connection request to the broker. |
| LOG\_DIR | A parameter that specifies the directory where SQL logs are stored. |
| ERROR\_LOG\_DIR | A parameter that specifies the directory where error logs for the broker are stored. |

#### cm.conf Description

A CUBRID manager configuration file that allows you to set the port used by the manager server process you want to operate, the monitoring collection cycle, etc.

| Name | Description |
| --- | --- |
| cm\_port | The port used by the manager server process. |
| cm\_process\_monitor\_interval | A cycle for monitoring information collection. |
| support\_mon\_statistic | A parameter to set whether to use cumulative monitoring. |
| server\_long\_query\_time | A parameter that specifies the threshold (in seconds) for a late query when the slow\_query item among the server's diagnostic items is set. |



## MariaDB Instance
### How to Start/Stop MariaDB

``` sh
# Start the MariaDB service
shell> sudo systemctl start mariadb.service

# Stop the MariaDB service
shell> sudo systemctl stop mariadb.service

# Restart the MariaDB service
shell> sudo systemctl restart mariadb.service
```

### Connect to MariaDB

After creating an instance, initially connect to MariaDB as follows.

``` sh
shell> mysql -u root
```

After changing the password, connect to MySQL as follows.

``` sh
shell> mysql -u root -p
Enter password:
```

### Initial Setup After Creating a MariaDB Instance

#### 1\. Set the Password

After initial installation, the MariaDB root account password is not set. Therefore, you must set a password after installation.

```
SET PASSWORD [FOR user] = password_option

MariaDB> SET PASSWORD = PASSWORD('password');
```

#### 2\. Change the Port

After initial installation, the port is 3306, which is MariaDB's default port. For security reasons, it is recommended to change the port.

##### 1) Modify the `/etc/my.cnf.d/server.cnf` file

Open the `/etc/my.cnf.d/server.cnf` file and enter the port address to change under [mariadb] as follows.

```
shell> sudo vi /etc/my.cnf.d/server.cnf
```

```
[mariadb]
port=[port address to change]
```

##### 2) Restart the instance
Restart the instance for the port change to take effect.
```
sudo systemctl restart mariadb.service
```
## Tibero Instance

## JEUS Instance

The images provided by default include CentOS 7.8 with JEUS8Fix1 (Domain Administrator Server 2022.03.22) and CentOS 7.8 with JEUS8Fix1 (Managed Server 2022.03.22). To install Domain Administrator Server, use CentOS 7.8 with JEUS8Fix1 (Domain Administrator Server 2022.03.22) image. To install Managed Server, use the CentOS 7.8 with JEUS8Fix1 (Managed Server 2022.03.22) image.

JEUS is installed in `~/apps/jeus8`.

The following properties are set during installation.

| Property | Default value |
| --- | --- |
| Domain name | jeus_domain |
| WebAdmin port | 9736 |
| Admin server name | adminServer |
| Admin user ID | administrator |
| Admin user password | jeusadmin |
| Node manager | java |


### Check the Startup

To configure or control JEUS, you must start the node manager and then control it through WebAdmin or jeusadmin.

### Connect to the Instance

After the instance creation is complete, use SSH to access the instance.
The instance must have a floating IP associated and TCP port 22 (SSH) must be allowed in the security group.

### Start the Node Manager

Connect to the shell and run the node manager with the startNodeManager command.
Since the node managers need to communicate with each other, add an allow rule for the default port 7730 to the security group.

### Start JEUS

To run Domain Administrator Server, use the startDomainAdminServer command.

```
startDomainAdminServer -uadministrator -pjeusadmin
```

### JEUS WebAdmin

Run WebAdmin as follows:

1. Set a floating IP on the instance where DAS is installed.
2. Add an allow rule for port 9736 to that instance's security group.
3. If you connect to in http://{floatingIP}:9736/webadmin in a web browser, you can see the WebAdmin screen.


## WebtoB Instance

The image provided by default is CentOS 7.8 with WebtoB5Fix4 (2022.03.22).
WebtoB is installed in `~/apps/webtob`.


### Check the Startup

#### Connect to the Instance

After the instance creation is complete, use SSH to access the instance.
The instance must have a floating IP associated and TCP port 22 (SSH) must be allowed in the security group.

#### Compile the Configuration File

Compile the configuration file using the wscfl command.

```
wscfl -i http.m
```

#### Start WebtoB

Start WebtoB using wsboot.

```
wsboot
```

You can use wsadmin to view or control the status.
