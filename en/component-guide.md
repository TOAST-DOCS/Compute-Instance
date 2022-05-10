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
After instance is created, access the instance by using Remote Desktop Protocol (RDP). 
To that end, an instance must be associated with a floating IP and TCP port 3389 (RDP) must be allowed for security group. 

![mssqlinstance_02_201812](https://static.toastoven.net/prod_ms_sql/mssqlinstance_02_201812_en.png)

Click **+ Check Password** to check password by using key pair configured along with instance creation. 
Click **Associate** and download .rdp file, to access the instance by using the acquired password. 

### Initial Settings after Microsoft SQL Image is Created  

#### 1. Set SQL Certification Mode  

The default certification mode of the server is set with"**Windows Certification Mode**". 
To use Microsoft SQL database account, the mode must be changed to SQL Certification Mode. 

Execute Microsoft SQL Server Management Studio and associate to an object under the instance name. 

![mssqlinstance_03_201812](https://static.toastoven.net/prod_ms_sql/mssqlinstance_03_201812_en.png)

1. Select an object, right-click it, and choose **Attributes**. 
2. On **Server Attributes** , click **Security**. 
3. Change the **Server Certification** type into **SQL Server and Windows Certification Mode**.

※ To apply the changed SQL certification mode, restart Microsoft SQL. 

#### 2. Change Microsoft SQL Service Port 

The default port 1433 for Microsoft SQL is widely known and might serve as a security vulnerability.   
A change is recommended to another port. 
※ For Express, no default port is specified. 

Execute SQL Server configuration manager as below. 

![mssqlinstance_04_201812](https://static.toastoven.net/prod_ms_sql/mssqlinstance_04_201812_en.png)

1. Click **Protocol for MSSQLSERVER** below **SQL Server Network Configuration** from menu on the left. 
2. Click **TCP/IP** for **Protocol Name** and right click it. When the menu shows, select **Attributes**.  
3. Select the **IP Address** tab. 
4. Select **IP ALL** on the list and change the port number to another.  

※ To apply the changed service port, restart Microsoft SQL. 

#### 3. Allow External Access to Microsoft SQL Database 

To allow external access to Microsoft SQL Database, go to the **Security Group** tab of **Network > VPC** and add Microsoft SQL service port for security rules. 
Also, register Microsoft SQL service port (default port: 1433) to allow access, as well as remote IP.  

### Data Volume Assignment  

Microsoft SQL data/log files (MDF/LDF) and backup files are recommended to be applied with separate block storages.  

![mssqlinstance_05_201812](https://static.toastoven.net/prod_ms_sql/mssqlinstance_05_201812_en.png)

Go to **Compute > Instance > Block Storage** and create a block storage.  
**Universal SSD** is a recommended volume type for improved performance.

After a block storage is created, select the storage and click **Association Management** and associate it to an instance.  

![mssqlinstance_06_201812](https://static.toastoven.net/prod_ms_sql/mssqlinstance_06_201812_en.png)

<br/>

Access instance with RDP and execute **Computer Management**, and go to **Storage>Disk Management**. 

![mssqlinstance_07_201812](https://static.toastoven.net/prod_ms_sql/mssqlinstance_07_201812_en.png)

You can find the associated block storage is detected. To use it, initialize disk first. 
1. Right-click the **Disk 1** block and click **Initialize Disk**. 
2. Select a partition type and click **OK**. 

<br/>

After initialization is completed, create disk volume. 

![mssqlinstance_08_201812](https://static.toastoven.net/prod_ms_sql/mssqlinstance_08_201812_en.png)

Click unassigned disk and right-click it. Select **New Simple Volume** and proceed with wizard for new simple volume. 

<br/>

In the **Database Setting** of **Server Attributes** of Microsoft SQL Server Management Studio,  change **Default Database Location** into the directory where the volume has been created. 

![mssqlinstance_09_201812](https://static.toastoven.net/prod_ms_sql/mssqlinstance_09_201812_en.png)

※ To apply the changed default database location, restart Microsoft SQL. 

### Restart Microsoft SQL 
Change of Microsoft SQL settings sometimes requires a restart of the service.  
To apply changed settings, restart Microsoft SQL.   

From SQL Server Configuration Manager, go to **SQL Server Configuration Manager (local) > SQL Server Service > SQL Server (MSSQLSERVER)** and right click it. When the menu shows, click **Restart** to restart the service. 

![mssqlinstance_10_201812](https://static.toastoven.net/prod_ms_sql/mssqlinstance_10_201812_en.png)

### Check/Set Automatic Microsoft SQL  Service Execution
Check if Microsoft SQL is set for automatic start with OS running.  

Go to **SQL Server Configuration Manager (local) > SQL Server** in the SQL Server Configuration Manager to find **Start Mode**.  

![mssqlinstance_11_201812](https://static.toastoven.net/prod_ms_sql/mssqlinstance_11_201812_en.png)

When the service start mode for **SQL SERVER (MSSSQLSERVER) and SQL Server Agent (MSSQLSERVER)** are not **automatic**, do the followings: 

1. Click the service and right-click it. Select **Attributes** on the menu. 
2. Change **Service** on **General > Start Mode** to **Automatic**.

> [Note]
> For the release status of Microsoft SQL Instance, see [Instance Release Note](/Compute/Compute/ko/release-notes/).

## MySQL Instance
### Starting/Stopping MySQL 

```
#Start mysql Service 
shell> service mysqld start

#Stop mysql Service 
shell> service mysqld stop

#Restart mysql Service 
shell> service mysqld restart
```

### Connecting to MySQL 

For initial connection, connect to MySQL with default user name. 

```
shell> mysql -uroot
```

### Initial Settings for MySQL Instance

#### 1\. Setting Password

There's no password on root user on initial installation. Therefore, it is required to set password as soon as possible.  

* Set Password for MySQL Version 5.6  

```
SET PASSWORD [FOR user] = password_option

mysql> set password=password('password');
```

* Set Password for MySQL Version 5.7  

```
ALTER USER USER() IDENTIFIED BY 'auth_string';

mysql> ALTER USER 'root'@'localhost' IDENTIFIED BY 'New Password';
```

Default MySQL validate_password_policy is as below:

* validate\_password\_policy=MEDIUM
* Must be more than 8 characters, and include numbers, lower/upper cases, and special characters.

#### 2\. Changing Port Number

The default MySQL port number is 3306. It is recommended to change the port number for security reasons. 

```
shell> vi /etc/my.cnf


# Specify a port to use in the my.cnf file. 

port = Port name to use 


# Save vi editor Save editor 


# Restart mysql service  


shell> service mysqld restart


#Connect with the changed port number


shell> mysql -uroot -P[changed port number]
```

### Description of my.cnf 

The default path of my.cnf is /etc/my.cnf, and NHN Cloud recommended variables are set as below: 

| Name | Description |
| --- | --- |
| default\_storage\_engine | Specify a default storage engine: Default is InnoDB with Online-DDL and transactions available. |
| expire\_logs\_days | Set log expiration period for logs provided by binlog settings. Default is three days. |
| innodb\_log\_file\_size | Specify the size of log files which save redo logs of transactions. <br>Recommended size is 256MB or higher in actual environment, and it is set as 512MB by default. In order for the changes to take effect, please restart the database. |
| innodb\_file\_per\_table | When a table is deleted or truncated, the table space is immediately returned to the OS. |
| innodb\_log\_files\_in\_group | Set the number of innodb\_log\_file files and use them in circular fashion: requires at least two. |
| log_timestamps | Default log time of MySQL 5.7 is displayed in UTC time format; therefore, change log time to system local time. |
| slow\_query\_log | Enable the slow\_query log option. Queries taking more than 10 seconds in accordance with long_query_time will be logged to the slow_query_log. |
| sysdate-is-now | For sysdate, SQL with sysdate() used for replication results in discrepant time between Master and Slave, so sysdate() and now() functions will behave the same. |

### Description of MySQL Directory 

Directory and file description of MySQL are as below: 

| Name | Description |
| --- | --- |
| my.cnf | /etc/my.cnf |
| DATADIR | Path for MySQL Data File  - /var/lib/mysql/ |
| ERROR_LOG | Path for MySQL error_log File  - /var/log/mysqld.log |
| SLOW_LOG | Path for MySQL Slow Query File -  <span style="color:#333333">/var/lib/mysql/*slow.log</span> |


> For detailed release status of MySQL Instance, please refer to [Instance Release Notes](/Compute/Compute/en/release-notes/).


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
