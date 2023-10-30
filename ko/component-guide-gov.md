## Compute > Instance > 설치 구성 요소 가이드


## MS-SQL Instance
### 보안그룹 TCP 포트 3389(RDP) 허용
인스턴스 생성 완료 후 RDP(Remote Desktop Protocol)을 활용하여 인스턴스에 접근합니다.
인스턴스에 Floating IP가 연결되어 있어야 하며 보안그룹에서 TCP 포트 3389(RDP)가 허용되어야 합니다.

![mssqlinstance_02_201812](https://static.toastoven.net/prod_ms_sql/mssqlinstance_02_201812.png)

**+ 비밀번호 확인** 버튼을 클릭하여 인스턴스 생성 시 설정한 키페어를 사용해 비밀번호를 확인합니다.
**연결** 버튼을 클릭하여 .rdp 파일을 다운받은 후 획득한 비밀번호를 사용하여 인스턴스에 접속합니다.

### MS-SQL 이미지 생성 후 초기 설정

#### 1. SQL 인증모드 설정

서버의 기본 인증 모드가 "Windows 인증 모드"로 되어 있습니다.
MS-SQL의 DB 계정을 사용하기 위해 SQL 인증 모드로 변경이 필요합니다.

Microsoft SQL Server Management Studio를 실행하여 인스턴스 이름으로 개체에 연결합니다.

![mssqlinstance_03_201812](https://static.toastoven.net/prod_ms_sql/mssqlinstance_03_201812.png)

1. 개체를 우클릭합니다.
2. 메뉴에서 **속성**을 선택합니다.
3. 서버 속성 창에서 **보안** 메뉴를 선택합니다.
4. **서버 인증** 방식을 "SQL Server 및 Windows 인증 모드"로 변경합니다.

※ SQL 인증모드 설정후 적용을 위해 MS-SQL 서비스를 재시작해야 됩니다.

#### 2. MS-SQL 서비스 포트 변경

MS-SQL의 기본 서비스 포트 1433은 널리 알려진 포트로 보안 취약점이 될 수 있습니다.
다른 포트로 변경하는것을 추천합니다.
※ Express 의 경우 기본포트 지정이 안되어 있습니다.

SQL Server 구성관리자를 실행합니다.

![mssqlinstance_04_201812](https://static.toastoven.net/prod_ms_sql/mssqlinstance_04_201812.png)

1. 좌측 패널에서 **SQL Server 네트워크 구성**의 하위 항목 **MSSQLSERVER에 대한 프로토콜**을 선택합니다.
2. 프로토콜 이름 중 **TCP/IP**를 우 클릭합니다.
3. 메뉴에서 **속성**을 선택합니다.
4. **IP 주소** 탭을 클릭합니다.
5. 드랍다운 메뉴 중 **IP ALL**을 선택한 후 다른 포트 번호로 변경합니다.

※ MS-SQL 서비스 포트 변경후 적용을 위해 MS-SQL 서비스를 재시작해야 됩니다.

#### 3. 외부에서 MS-SQL Database 접속허용 설정

외부에서 MS-SQL Database에 접속하기 위해서 **Network > VPC** 의 **Security Group**탭에서 MS-SQL 서비스 포트를 Security Rule로 추가해야 합니다.
Security Rule 추가시 접속을 허용할 MS-SQL 서비스 포트 (기본포트 : 1433) 및 원격 IP를 등록합니다.

### 데이터 볼륨 할당

MS-SQL의 데이터/로그 파일(MDF/LDF), 백업 파일은 별도의 Block Storage 사용을 권장합니다.

![mssqlinstance_05_201812](https://static.toastoven.net/prod_ms_sql/mssqlinstance_05_201812.png)

**Compute > Instance > Block Storage** 탭으로 이동하여 Block Storage를 생성합니다.
Block Storage 생성시 Volume 타입은 성능을 위해 "범용 SSD"를 추천합니다.

![mssqlinstance_06_201812](https://static.toastoven.net/prod_ms_sql/mssqlinstance_06_201812.png)

Block Storage 생성 완료 후 Storage 선택 후 **연결 관리** 버튼을 클릭하여 인스턴스에 연결합니다.

<br/>

RDP로 인스턴스에 접속하여 **컴퓨터 관리**를 실행하여 **저장소 > 디스크 관리**로 이동합니다.

![mssqlinstance_07_201812](https://static.toastoven.net/prod_ms_sql/mssqlinstance_07_201812.png)

연결 된 Block Storage가 탐지된것을 확인할 수 있습니다. 사용하기 위해서는 먼저 디스크 초기화를 실행해야합니다.
1. **디스크 1** 블럭을 우 클릭한 후 **디스크 초기화**를 클릭합니다.
2. 파티션 형식 선택 후 **확인** 버튼을 클릭합니다.

<br/>

초기화 완료 후 디스크 볼륨을 생성합니다.

![mssqlinstance_08_201812](https://static.toastoven.net/prod_ms_sql/mssqlinstance_08_201812.png)

할당되지 않은 디스크를 우 클릭 후 **새 단순 볼륨**을 클릭하여 새 단순 볼륨 마법사를 진행합니다.

<br/>

Microsoft SQL Server Management Studio 서버 속성의 데이터베이스 설정에서 데이터베이스 기본 위치를 생성한 볼륨의 디렉토리로 변경합니다.

![mssqlinstance_09_201812](https://static.toastoven.net/prod_ms_sql/mssqlinstance_09_201812.png)

※ MS-SQL 데이터베이스 기본 위치 변경후 적용을 위해 MS-SQL 서비스를 재시작해야 됩니다.

### MS-SQL 서비스 재시작
MS-SQL의 설정 변경시 MS-SQL 서비스의 재시작이 필요한 경우가 있습니다.
변경 설정을 적용하기위해 MS-SQL 서비스를 재시작 합니다.

SQL Server 구성관리자의 **SQL Server 구성관리자(로컬) > SQL Server 서비스 > SQL Server(MSSQLSERVER)** 를 선택후 우클릭후 노출되는 메뉴에서 "다시 시작"으로 MS-SQL 서비스를 재시작합니다.

![mssqlinstance_10_201812](https://static.toastoven.net/prod_ms_sql/mssqlinstance_10_201812.png)

### MS-SQL 서비스 자동 실행 확인/설정
MS-SQL의 서비스가 OS 구동시 자동으로 시작하도록 설정되어 있는지 확인합니다.

SQL Server 구성관리자의  SQL Server 구성관리자(로컬) > SQL Server 서비스 에서 "시작모드"를 확인할 수 있습니다.

![mssqlinstance_11_201812](https://static.toastoven.net/prod_ms_sql/mssqlinstance_11_201812.png)

**SQL SERVER (MSSSQLSERVER)** 및 **SQL Server 에이전트 (MSSQLSERVER)** 등의 서비스 시작모드가 **자동**이 아닐 경우:
1. 해당 서비스를 우 클릭한 후 **속성**을 선택합니다.
2. **서비스**탭에서 **General > 시작모드**를 **자동**으로 변경합니다.

> [참고]
> MS-SQL Instance의 릴리스 현황은 [인스턴스 릴리스 노트](/Compute/Compute/ko/release-notes/) 를 참고하시기 바랍니다.
## MySQL Instance
### MySQL 시작/정지 방법

```
#mysql 서비스 시작
shell> service mysqld start

#mysql 서비스 중지
shell> service mysqld stop

#mysql 서비스 재시작
shell> service mysqld restart
```

### MySQL 접속

이미지 생성 후 초기에는 아래와 같이 접속합니다.

```
shell> mysql -uroot
```

### MySQL 인스턴스 생성 후 초기 설정

#### 1\. 비밀번호 설정

초기 설치 후 MySQL ROOT 계정 비밀번호는 지정되어 있지 않습니다. 그러므로 설치 후 반드시 바로 비밀번호를 설정해야 합니다. 비밀번호는 아래와 같이 변경할 수 있습니다.
```
mysql> ALTER USER USER() IDENTIFIED BY '새로운 비밀번호';
```

MySQL의 기본 validate\_password\_policy는 아래와 같습니다.

* validate\_password\_policy=MEDIUM
* 기본 **8자 이상, 숫자, 소문자, 대문자, 특수문자**를 포함해야 함

#### 2\. 포트(port) 변경

제공되는 이미지 포트는 MySQL 기본 포트인 3306입니다. 보안상 포트 변경을 권장합니다.

```
shell> vi /etc/my.cnf


#my.cnf 파일에 사용하고자 하는 포트를 명시해 줍니다.

port =사용하고자 하는 포트명


#vi 편집기 저장


#mysql 서비스 재시작


shell> service mysqld restart


#변경된 포트로 아래와 같이 접속


shell> mysql -uroot -P[변경된 포트 번호]
```

### my.cnf 설명

my.cnf 의 기본 경로는 /etc/my.cnf 이고 NHN Cloud 권장 변수(variable)가 설정되어 있으며, 내용은 아래와 같습니다.

| 이름 | 설명 |
| --- | --- |
| default\_storage\_engine | 기본 스토리지 엔진(stroage engine)을 지정합니다. InnoDB로 지정되며 Online-DDL과 트랜잭션(transaction)을 사용할 수 있습니다. |
| expire\_logs\_days | binlog 설정으로 쌓이는 로그 저장일을 설정합니다. 기본 3일로 지정되어 있습니다. |
| innodb\_log\_file\_size | 트랜잭션(transaction)의 redo log를 저장하는 로그 파일의 크기를 지정합니다. <br><br>실제 운영 환경에서는 256MB 이상을 권장하며, 현재 512MB로 설정되어 있습니다. 설정값 수정 시 DB 재시작이 필요합니다. |
| innodb\_file\_per\_table | 테이블이 삭제되거나 TRUNCATE될 때, 테이블 공간이 OS로 바로 반납됩니다. |
| innodb\_log\_files\_in\_group | innodb\_log\_file 파일의 개수를 설정하며 순환적\(circular\)으로 사용됩니다\. 최소 2개 이상으로 구성됩니다\. |
| log_timestamps | MySQL 5.7의 기본 log 시간은 UTC로 표시됩니다. 그러므로 로그 시간을 SYSTEM 로컬 시간으로 변경합니다. |
| slow\_query\_log | slow\_query log 옵션을 사용합니다\. long\_query\_time에 따른 기본 10초 이상의 쿼리는 slow\_query\_log에 기록됩니다\. |
| sysdate-is-now | sysdate의 경우 replication에서 sysdate() 사용된 SQL문은 복제 시 마스터와 슬레이브 간의 시간이 달라지는 문제가 있어 sysdate()와 now()의 함수를 동일하게 적용합니다. |

### MySQL 디렉터리 설명

MySQL 디렉터리 및 파일 설명은 아래와 같습니다.

| 이름 | 설명 |
| --- | --- |
| my.cnf | /etc/my.cnf |
| DATADIR | MySQL 데이터 파일 경로  - /var/lib/mysql/ |
| ERROR_LOG | MySQL error_log 파일 경로  - /var/log/mysqld.log |
| SLOW_LOG | MySQL Slow Query 파일 경로 -  <span style="color:#333333">/var/lib/mysql/*slow.log</span> |


> MySQL Instance의 릴리스 현황은 [인스턴스 릴리스 노트](/Compute/Compute/ko/release-notes/)를 참고하시기 바랍니다.

## PostgreSQL Instance
### PostgreSQL 시작/정지 방법

```
#postgresql 서비스 시작
shell> sudo systemctl start postgresql-13

#postgresql 서비스 중지
shell> sudo systemctl stop postgresql-13

#postgresql 서비스 재시작
shell> sudo systemctl restart postgresql-13
```

### PostgreSQL 접속

이미지 생성 후 초기에는 아래와 같이 접속합니다.
<br>
```
#postgres로 계정 전환 후 접속
shell> sudo su - postgres
shell> psql
```

### PostgreSQL 인스턴스 생성 후 초기 설정

#### 1\. 포트\(port\) 변경

제공되는 이미지 포트는 PostgreSQL 기본 포트인 5432입니다. 보안상 포트 변경을 권장합니다.
<br>
```
shell> vi /var/lib/pgsql/13/data/postgresql.conf


#postgresql.conf 파일에 사용하고자 하는 포트를 명시해 줍니다.

port =사용하고자 하는 포트명


#vi 편집기 저장


#postgresql 서비스 재시작

shell> sudo systemctl restart postgresql-13


#변경된 포트로 아래와 같이 접속

shell> psql -p[변경된 포트 번호]
```

#### 2\. 서버 로그 타임 존 변경

서버 로그에 기록되는 기본 시간대가 UTC로 설정되어 있습니다. SYSTEM 로컬 시간과 동일하게 변경할 것을 권장합니다.
<br>
```
shell> vi /var/lib/pgsql/13/data/postgresql.conf


#postgresql.conf 파일에 사용하고자 하는 타임 존을 명시해 줍니다.

log_timezone =사용하고자 하는 타임 존


#vi 편집기 저장


#postgresql 서비스 재시작

shell> sudo systemctl restart postgresql-13


#postgresql 접속

shell> psql


#변경한 설정 확인

postgres=# SHOW log_timezone;
```

#### 3\. public 스키마 권한 취소

기본적으로 모든 사용자에게 public 스키마에 대한 CREATE 및 USAGE 권한을 부여하고 있으므로 데이터베이스에 접속할 수 있는 사용자는 public 스키마에서 객체를 생성할 수 있습니다. 모든 사용자가 public 스키마에서 객체를 생성하지 못하도록 권한 취소를 권장합니다.
<br>
```
#postgresql 접속

shell> psql


#권한 취소 명령어 실행

postgres=# REVOKE CREATE ON SCHEMA public FROM PUBLIC;
```

#### 4\. 원격 접속 허용

로컬 호스트 이외의 접속을 허용하려면 listen_addresses 변수와 클라이언트 인증 설정 파일을 변경해야 합니다.
<br>
```
shell> vi /var/lib/pgsql/13/data/postgresql.conf


#postgresql.conf 파일에 허용하고자 하는 주소를 명시해 줍니다.
#IPv4 주소를 모두 허용하는 경우 0.0.0.0
#IPv6 주소를 모두 허용하는 경우 ::
#모든 주소를 허용하는 경우 *

listen_addresses =허용하고자 하는 주소


#vi 편집기 저장


shell> vi /var/lib/pgsql/13/data/pg_hba.conf


#IP주소 형식별로 클라이언트 인증 제어
#오래된 클라이언트 라이브러리는 scram-sha-256 방식이 지원되지 않으므로 md5로 변경 필요

# TYPE  DATABASE        USER            ADDRESS                 METHOD
# IPv4 local connections:
host    all             all             127.0.0.1/32            scram-sha-256
host    허용DB           허용유저          허용주소                   scram-sha-256
# IPv6 local connections:
host    all             all             ::1/128                 scram-sha-256
host    허용DB           허용유저          허용주소                   scram-sha-256


#postgresql 서비스 재시작

shell> sudo systemctl restart postgresql-13
```

### PostgreSQL 디렉터리 설명

PostgreSQL 디렉터리 및 파일 설명은 아래와 같습니다.

| 이름 | 설명 |
| --- | --- |
| postgresql.cnf | /var/lib/pgsql/{version}/data/postgresql.cnf |
| initdb.log | PostgreSQL 데이터베이스 클러스터 생성 log - /var/lib/pgsql/{version}/initdb.log |
| DATADIR | PostgreSQL 데이터 파일 경로 - /var/lib/pgsql/{version}/data/ |
| LOG | PostgreSQL log 파일 경로 - /var/lib/pgsql/{version}/data/log/\*.log |


## CUBRID Instance

### CUBRID 서비스 시작/정지 방법

“cubrid” Linux 계정으로 로그인하여 CUBRID 서비스를 다음과 같이 시작하거나 종료할 수 있습니다.
```
# CUBRID 서비스/서버 시작
shell> sudo su - cubrid
shell> cubrid service start
shell> cubrid server start demodb

# CUBRID 서비스/서버 종료
shell> sudo su - cubrid
shell> cubrid server stop demodb
shell> cubrid service stop

# CUBRID 서비스/서버 재시작
shell> sudo su - cubrid
shell> cubrid server restart demodb
shell> cubrid service restart

# CUBRID 브로커 시작/종료/재시작
shell> sudo su - cubrid
shell> cubrid broker start
shell> cubrid broker stop
shell> cubrid broker restart
```

### CUBRID 접속

이미지 생성 후 초기에는 아래와 같이 접속합니다.
```
shell> sudo su - cubrid
shell> csql -u dba demodb@localhost
```

### CUBRID 인스턴스 생성 후 초기 설정

#### 1\. 비밀번호 설정

초기 설치 후 CUBRID dba 계정 비밀번호는 지정되어 있지 않습니다. 그러므로 설치 후 반드시 비밀번호를 설정해야 합니다.
```
shell> csql -u dba -c "ALTER USER dba PASSWORD 'new_password'" demodb@localhost
```

#### 2\. 브로커 포트\(port\) 변경

**query_editor**의 브로커 포트는 기본값이 **30000**으로 설정되며, **broker1**의 브로커 포트는 기본값이 **33000**으로 설정됩니다.
보안상 포트 변경을 권장합니다.

###### 1) 브로커 파일 수정

아래 파일을 열어서 아래와 같이 변경할 포트 주소를 입력합니다.
```
shell> vi /opt/cubrid/conf/cubrid_broker.conf

[%query_editor]
BROKER_PORT             =[변경할 port 주소]

[%BROKER1]
BROKER_PORT             =[변경할 port 주소]
```

###### 2) 브로커 재시작

포트 변경이 적용되도록 브로커를 재시작합니다.
```
shell> cubrid broker restart
```

#### 3\. 매니저 서버 포트\(port\) 변경

매니저 서버 포트는 기본값이 **8001**으로 설정됩니다. 
보안상 포트 변경을 권장합니다.

###### 1)  cm.conf 파일 수정

아래 파일을 열어서 아래와 같이 변경할 포트 주소를 입력합니다.
```
shell> vi /opt/cubrid/conf/cm.conf

cm_port =[변경할 port 주소]
```

###### 2) 매니저 서버 재시작

포트 변경이 적용되도록 매니저를 재시작합니다.
```
shell> cubrid manager stop
shell> cubrid manager start
```

### CUBRID 디렉터리 설명

CUBRID 디렉터리 및 파일 설명은 아래와 같습니다.

| 이름 | 설명 |
| --- | --- |
| database.txt | CUBRID 데이터베이스 위치 정보 파일 경로 - /opt/cubrid/databases |
| CONF PATH | CUBRID 서버, 브로커, 매니저 환경변수 파일 경로 - /opt/cubrid/conf |
| LOG PATH | CUBRID 프로세스 로그 파일 경로 - /opt/cubrid/log |
| SQL\_LOG | CUBRID SQL Query 파일 경로  /opt/cubrid/log/broker/sql\_log |
| ERROR\_LOG | CUBRID ERROR SQL Query 파일 경로 - /opt/cubrid/log/broker/error\_log |
| SLOW\_LOG | CUBRID Slow Query 파일 경로 - /opt/cubrid/log/broker/sql\_log |

#### cubrid.conf 설명

서버 설정용 파일로, 운영하려는 데이터베이스의 메모리, 동시 사용자 수에 따른 스레드 수, 브로커와 서버 사이의 통신 포트 등을 설정 가능합니다.

| 이름 | 설명 |
| --- | --- |
| service  | CUBRID 서비스 시작 시 자동으로 시작하는 프로세스를 등록하는 파라미터입니다. <br>기본으로 server, broker, manager 프로세스가 등록되어 있습니다. |
| cubrid\_port\_id | 마스터 프로세스가 사용하는 포트입니다. |
| max\_clients | 데이터베이스 서버 프로세스 하나당 동시에 접속할 수 있는 클라이언트의 최대 개수입니다. |
| data\_buffer\_size | 데이터베이스 서버가 메모리 내에 캐시하는 데이터 버퍼의 크기를 설정하기 위한 파라미터입니다. <br>필요한 메모리 크기가 시스템 메모리의 2/3 이내가 되도록 설정할 것을 권장합니다. |

#### broker.conf 설명

브로커 설정 파일로, 운영하려는 브로커가 사용하는 포트, 응용서버(CAS) 수, SQL LOG 등을 설정 가능합니다.

| 이름 | 설명 |
| --- | --- |
| BROKER\_PORT | 브로커가 사용하는 포트이며, 실제 JDBC와 같은 드라이버에서 보는 포트는 해당 브로커의 포트입니다. |
| MAX\_NUM\_APPL\_SERVER | 해당 브로커에 동시 접속할 수 있는 CAS의 최대 개수를 설정하는 파라미터입니다. |
| MIN\_NUM\_APPL\_SERVER | 해당 브로커에 대한 연결 요청이 없더라도 기본적으로 대기하고 있는 CAS 프로세스의 최소 개수를 설정하는 파라미터입니다. |
| LOG\_DIR | SQL 로그가 저장되는 디렉터리를 지정하는 파라미터입니다. |
| ERROR\_LOG\_DIR | 브로커에 대한 에러 로그가 저장되는 디렉터리를 지정하는 파라미터입니다. |

#### cm.conf 설명

CUBRID 매니저 설정 파일로, 운영하려는 매니저 서버 프로세스가 사용하는 포트, 모니터링 수집 주기 등을 설정 가능합니다.

| 이름 | 설명 |
| --- | --- |
| cm\_port | 매니저 서버 프로세스가 사용하는 포트입니다. |
| cm\_process\_monitor\_interval | 모니터링 정보 수집 주기입니다. |
| support\_mon\_statistic | 누적 모니터링을 사용할 것인지 설정하는 파라미터입니다. |
| server\_long\_query\_time | 서버의 진단 항목 중 slow\_query 항목을 설정할 경우 몇 초 이상을 늦은 질의로 판별할지 결정하는 파라미터입니다. |


## MariaDB Instance
### MariaDB 시작/정지 방법

``` sh
# MariaDB 서비스 시작
shell> sudo systemctl start mariadb.service

# MariaDB 서비스 종료
shell> sudo systemctl stop mariadb.service

# MariaDB 서비스 재시작
shell> sudo systemctl restart mariadb.service
```

### MariaDB 접속

이미지 생성 후 초기에는 아래와 같이 접속합니다.

``` sh
shell> sudo mysql -u root
```

패스워드 변경 후에는 아래와 같이 접속합니다.

``` sh
shell> mysql -u root -p
Enter password:
```

### MariaDB 인스턴스 생성 후 초기 설정

#### 1\. 비밀번호 설정

초기 설치 후 MariaDB root 계정 비밀번호는 지정되어 있지 않습니다. 그러므로 설치 후 반드시 비밀번호를 설정해야 합니다.

```
SET PASSWORD [FOR user] = password_option

MariaDB> SET PASSWORD = PASSWORD('비밀번호');
```

#### 2\. 포트\(port\) 변경

초기 설치 후 포트는 MariaDB의 기본 포트인 3306입니다. 보안상 포트 변경을 권장합니다.

##### 1) `/etc/my.cnf.d/server.cnf` 파일 수정

`/etc/my.cnf.d/server.cnf` 파일을 열어서 [mariadb] 밑에 아래와 같이 변경할 포트 주소를 입력합니다.

```
shell> sudo vi /etc/my.cnf.d/server.cnf
```

```
[mariadb]
port=[변경할 port 주소]
```

##### 2) 인스턴스 재시작
포트 변경이 적용되도록 인스턴스를 재시작합니다.
```
sudo systemctl restart mariadb.service
```

## Tibero Instance

### Tibero Instance 생성

#### 최소 권장 사양

- 루트 블록 스토리지 
    - 빠른 속도를 위해 SSD를 권장하며, root disk full 이 발생하지 않도록 **50GB 이상**으로 설정할 것을 권고합니다.

- 최소 권장 사양: 4vCore/8GB
    - **권장 사양 미만 사용 시 DBMS 설치가 제한될 수 있습니다.**
#### 추가 블록 스토리지

- 루트 볼륨 이외의 추가 볼륨을 생성합니다.
    - TMI(Tibero Machine Image)는 추가 볼륨 150GB를 요구하기 때문에 **추가 블록 스토리지 150G 이상**을 반드시 설정해야 합니다.

### 인스턴스 접속

- 인스턴스 생성 완료 후 SSH를 사용하여 인스턴스에 접근합니다.
- 인스턴스에 플로팅 IP가 연결되어 있어야 하며 보안 그룹에서 TCP 포트 22(SSH)가 허용되어야 합니다.
- SSH 클라이언트와 설정한 키페어를 이용해 인스턴스에 접속합니다.
- SSH 연결에 대한 자세한 가이드는 [SSH 연결 가이드](https://docs.toast.com/ko/Compute/Instance/ko/overview/#linux)<span style="color:#313338">를 참고하시기 바랍니다.</span>

### TMI 설치


root 계정으로 /root 경로에서 dbca 명령어를 실행합니다.
```
$ ./dbca OS_ACCOUNT DB_NAME DB_CHARACTERSET DB_TYPE DB_PORT
```

| No | 항목 | 인자값 |
| :---: | --- | --- |
| 1 | OS\_ACCOUNT | Tibero가 구동되는 OS 계정 |
| 2 | DB\_NAME | Tibero에서 사용되는 DB\_NAME(= SID) |
| 3 | DB\_CHARACTERSET | Tibero에서 사용하는 DB 문자 집합 |
| 4 | DB\_TYPE | Tibero Type 지정(16vCore 이하: SE/16vCore 초과: CE) |
| 5 | DB\_PORT | Tibero에서 사용하는 서비스 IP의 포트 |

##### Tibero 7 Cloud Standard Edition
```
[centos@tiberoinstance ~]$ sudo su root
[root@tiberoinstance centos]# cd
[root@tiberoinstance ~]# pwd
/root
[root@tiberoinstance ~]# ./dbca nhncloud tiberotestdb utf8 SE 8639
```


##### Tibero 7 Cloud Enterprise Edition
```
[centos@tiberoinstance ~]$ sudo su root
[root@tiberoinstance centos]# cd
[root@tiberoinstance ~]# pwd
/root
[root@tiberoinstance ~]# ./dbca nhncloud tiberotestdb utf8 CE 8639
```

#### 설치 완료

dbca 명령어 수행 시 진행 상황이 출력되며 nomount 모드에서 데이터베이스가 생성됩니다. 소요 시간은 10분 이하입니다. 
완료되면 아래와 같이 출력됩니다.

```
SQL>
System altered.

SQL>
System altered.

SQL> Disconnected.
[root@tiberoinstance ~]#
```


#### 구동 확인 및 설치 로그 확인

Tibero가 구동 중인지 확인합니다.

```
[root@tiberoinstance ~]# ps -ef |grep tbsvr
nhncloud  9886     1  1 14:14 ?        00:00:00 tbsvr          -t NORMAL -SVR_SID tiberotestdb
nhncloud  9888  9886  0 14:15 ?        00:00:00 tbsvr_MGWP     -t NORMAL -SVR_SID tiberotestdb
nhncloud  9889  9886 36 14:15 ?        00:00:21 tbsvr_FGWP000  -t NORMAL -SVR_SID tiberotestdb
nhncloud  9890  9886  0 14:15 ?        00:00:00 tbsvr_FGWP001  -t NORMAL -SVR_SID tiberotestdb
nhncloud  9891  9886  0 14:15 ?        00:00:00 tbsvr_FGWP002  -t NORMAL -SVR_SID tiberotestdb
nhncloud  9892  9886  0 14:15 ?        00:00:00 tbsvr_FGWP003  -t NORMAL -SVR_SID tiberotestdb
nhncloud  9893  9886  0 14:15 ?        00:00:00 tbsvr_FGWP004  -t NORMAL -SVR_SID tiberotestdb
nhncloud  9894  9886  0 14:15 ?        00:00:00 tbsvr_FGWP005  -t NORMAL -SVR_SID tiberotestdb
nhncloud  9895  9886  0 14:15 ?        00:00:00 tbsvr_FGWP006  -t NORMAL -SVR_SID tiberotestdb
nhncloud  9896  9886  0 14:15 ?        00:00:00 tbsvr_FGWP007  -t NORMAL -SVR_SID tiberotestdb
nhncloud  9897  9886  0 14:15 ?        00:00:00 tbsvr_FGWP008  -t NORMAL -SVR_SID tiberotestdb
nhncloud  9898  9886  0 14:15 ?        00:00:00 tbsvr_FGWP009  -t NORMAL -SVR_SID tiberotestdb
nhncloud  9899  9886  0 14:15 ?        00:00:00 tbsvr_PEWP000  -t NORMAL -SVR_SID tiberotestdb
nhncloud  9900  9886  0 14:15 ?        00:00:00 tbsvr_PEWP001  -t NORMAL -SVR_SID tiberotestdb
nhncloud  9901  9886  0 14:15 ?        00:00:00 tbsvr_PEWP002  -t NORMAL -SVR_SID tiberotestdb
nhncloud  9902  9886  0 14:15 ?        00:00:00 tbsvr_PEWP003  -t NORMAL -SVR_SID tiberotestdb
nhncloud  9903  9886  0 14:15 ?        00:00:00 tbsvr_PEWP004  -t NORMAL -SVR_SID tiberotestdb
nhncloud  9904  9886  0 14:15 ?        00:00:00 tbsvr_PEWP005  -t NORMAL -SVR_SID tiberotestdb
nhncloud  9905  9886  0 14:15 ?        00:00:00 tbsvr_AGNT     -t NORMAL -SVR_SID tiberotestdb
nhncloud  9906  9886  3 14:15 ?        00:00:01 tbsvr_DBWR     -t NORMAL -SVR_SID tiberotestdb
nhncloud  9907  9886  0 14:15 ?        00:00:00 tbsvr_RCWP     -t NORMAL -SVR_SID tiberotestdb
root     13517  8366  0 14:15 pts/0    00:00:00 grep --color=auto tbsvr
[root@tiberoinstance ~]#
```

설치 로그는 /root/.dbset.log에서 확인이 가능합니다.

```
[root@tiberoinstance ~]# ls -alh
합계 20K
dr-xr-x---.  4 root root  104 10월 17 14:15 .
dr-xr-xr-x. 23 root root 4.0K 10월 17 14:14 ..
-rw-r--r--.  1 root root   18 12월 29  2013 .bash_logout
-rw-r--r--.  1 root root  176 12월 29  2013 .bash_profile
-rw-r--r--.  1 root root  176 12월 29  2013 .bashrc
-rw-r--r--   1 root root 3.6K 10월 17 14:15 .dbset.log
drwxr-----   3 root root   19 10월 17 14:13 .pki
drwx------   2 root root   29 10월 17 14:04 .ssh
```

### Tibero 접속

#### 계정 변경

dbca 명령어로 생성한 OS\_ACCOUNT로 로그인합니다.


```
[root@tiberoinstance ~]# su - nhncloud
마지막 로그인: 목  1월 13 11:34:43 KST 2022 일시 pts/0

 #####   ###  ######    ###         #######  ###  ######   #######  ######   #######  #######  #######   #####   #######  ######   ######
#     #   #   #     #   ###            #      #   #     #  #        #     #  #     #     #     #        #     #     #     #     #  #     #
#         #   #     #   ###            #      #   #     #  #        #     #  #     #     #     #        #           #     #     #  #     #
 #####    #   #     #                  #      #   ######   #####    ######   #     #     #     #####     #####      #     #     #  ######
      #   #   #     #   ###            #      #   #     #  #        #   #    #     #     #     #              #     #     #     #  #     #
#     #   #   #     #   ###            #      #   #     #  #        #    #   #     #     #     #        #     #     #     #     #  #     #
 #####   ###  ######    ###            #     ###  ######   #######  #     #  #######     #     #######   #####      #     ######   ######

[nhncloud@tiberoinstance ~]$

```

#### 접속 확인

```
[nhncloud@tiberoinstance ~]$ tbsql sys/tibero

tbSQL 7

TmaxTibero Corporation Copyright (c) 2020-. All rights reserved.

Connected to Tibero.

SQL> select * FROM v$instance;

INSTANCE_NUMBER INSTANCE_NAME
--------------- ----------------------------------------
DB_NAME
----------------------------------------
HOST_NAME                                                       PARALLEL
--------------------------------------------------------------- --------
   THREAD# VERSION
---------- --------
STARTUP_TIME
--------------------------------------------------------------------------------
STATUS           SHUTDOWN_PENDING
---------------- ----------------
TIP_FILE
--------------------------------------------------------------------------------
              0 tiberotestdb
tiberotestdb
tiberoinstance.novalocal                                      NO
         0 7
2023/10/17
NORMAL           NO
/db/tibero7/config/tiberotestdb.tip


1 row selected.

SQL>
```


### Tibero 기본 계정

Tibero에서 제공하는 기본 계정은 다음과 같습니다.

| 스키마 | 비밀번호 | 설명 |
| --- | --- | --- |
| sys | tibero | SYSTEM 스키마 |
| syscat | syscat | SYSTEM 스키마 |
| sysgis | sysgis | SYSTEM 스키마 |
| outln | outln | SYSTEM 스키마 |
| tibero | tmax | SAMPLE 스키마 DBA 권한 |
| tibero1 | tmax | SAMPLE 스키마 DBA 권한 |

* SYS: 데이터베이스의 관리자 태스크를 수행합니다.
* SYSCAT: DATA DICTIONARY & CATALOGVIEW를 생성합니다.
* SYSGIS: GIS 관련 테이블 생성 및 태스크를 수행하는 계정입니다.
* OUTLN: 동일한 SQL 수행 시 항상 같은 플랜으로 수행될 수 있게 관련 힌트를 저장하는 등의 태스크를 수행합니다.
* TIBERO/TIBERO1: example user이며 DBA 권한을 가지고 있습니다.

## Kafka Instance
> [참고]
> 본 가이드는 Kafka 3.3.1 버전을 기준으로 작성되었습니다.
> 다른 버전을 사용하시는 경우 해당 버전에 맞게 변경해 주십시오.
> 인스턴스 타입은 c1m2(CPU 1core, Memory 2GB) 이상 사양으로 생성해 주십시오.

### Zookeeper, Kafka broker 시작/정지
```
# Zookeeper, Kafka broker 시작(Zookeeper 먼저 시작)
shell> sudo systemctl start zookeeper.service
shell> sudo systemctl start kafka.service

# Zookeeper, Kafka broker 종료(Kafka broker 먼저 종료)
shell> sudo systemctl stop kafka.service
shell> sudo systemctl stop zookeeper.service

# Zookeeper, Kafka broker 재시작
shell> sudo systemctl restart zookeeper.service
shell> sudo systemctl restart kafka.service
```

### Kafka Cluster 설치
- 반드시 신규 인스턴스에 설치합니다.
- 인스턴스는 3대 이상 홀수로 필요하며, 인스턴스 1대에서 설치 스크립트를 수행합니다.
- 인스턴스 1대에 kafka broker, zookeeper node 각 1개씩 같이 구성됩니다.
- 설치 스크립트를 수행하는 인스턴스의 ~ 경로에 타 인스턴스 접속 시 필요한 키 페어(PEM 파일)가 있어야 합니다. 클러스터 인스턴스들의 키 페어는 모두 동일해야 합니다.
- 기본 포트 설치만 지원합니다. 포트 변경이 필요할 경우 클러스터 설치를 완료한 뒤 초기 설정 가이드의 포트 변경을 참고하여 변경합니다.
- 인스턴스 간 Kafka 관련 포트 통신을 위해 아래 보안 그룹 설정을 추가합니다.

보안 그룹 설정
```
방향: 수신
IP 프로토콜: TCP
포트: 22, 9092, 2181, 2888, 3888
```
Hostname, IP 확인 방법
```
# Hostname 확인
shell> hostname

# IP 확인
콘솔 화면
또는 shell> hostname -i
```
Cluster 설치 스크립트 수행 예시(위에서 확인한 hostname, IP 입력)
```
shell> sh ~/.kafka_make_cluster.sh

Enter Cluster Node Count: 3
### 3 is odd number.
Enter Cluster's IP ( Cluster 1 ) : 10.0.0.1
Enter Cluster's HOST_NAME ( Cluster 1 ) : kafka1.novalocal
Enter Cluster's IP ( Cluster 2 ) : 10.0.0.2
Enter Cluster's HOST_NAME ( Cluster 2 ) : kafka2.novalocal
Enter Cluster's IP ( Cluster 3 ) : 10.0.0.3
Enter Cluster's HOST_NAME ( Cluster 3 ) : kafka3.novalocal
10.0.0.1 kafka1.novalocal
10.0.0.2 kafka2.novalocal
10.0.0.3 kafka3.novalocal
Check Cluster Node Info (y/n) y
Enter Pemkey's name: kafka.pem
ls: cannot access /tmp/kafka-logs: No such file or directory
ls: cannot access /tmp/zookeeper: No such file or directory
### kafka1.novalocal ( 10.0.0.1 ), Check if kafka is being used
### kafka1.novalocal ( 10.0.0.1 ), Store node information in the /etc/hosts directory.
### kafka1.novalocal ( 10.0.0.1 ), Modify zookeeper.properties.
### kafka1.novalocal ( 10.0.0.1 ), Modify server.properties.
ls: cannot access /tmp/kafka-logs: No such file or directory
ls: cannot access /tmp/zookeeper: No such file or directory
### kafka2.novalocal ( 10.0.0.2 ), Check if kafka is being used
### kafka2.novalocal ( 10.0.0.2 ), Store node information in the /etc/hosts directory.
### kafka2.novalocal ( 10.0.0.2 ), Modify zookeeper.properties.
### kafka2.novalocal ( 10.0.0.2 ), Modify server.properties.
ls: cannot access /tmp/kafka-logs: No such file or directory
ls: cannot access /tmp/zookeeper: No such file or directory
### kafka3.novalocal ( 10.0.0.3 ), Check if kafka is being used
### kafka3.novalocal ( 10.0.0.3 ), Store node information in the /etc/hosts directory.
### kafka3.novalocal ( 10.0.0.3 ), Modify zookeeper.properties.
### kafka3.novalocal ( 10.0.0.3 ), Modify server.properties.
### kafka1.novalocal ( 10.0.0.1 ), Start Zookeeper, Kafka.
### Zookeeper, Kafka process is running.
### kafka2.novalocal ( 10.0.0.2 ), Start Zookeeper, Kafka.
### Zookeeper, Kafka process is running.
### kafka3.novalocal ( 10.0.0.3 ), Start Zookeeper, Kafka.
### Zookeeper, Kafka process is running.
##### Cluster Installation Complete #####
```

### Kafka 인스턴스 생성 후 초기 설정
#### 포트(port) 변경
최초 설치 후 포트는 Kafka 기본 포트인 9092, Zookeeper 기본 포트인 2181입니다. 보안을 위해 포트를 변경할 것을 권장합니다.

##### 1) ~/kafka/config/zookeeper.properties 파일 수정
~/kafka/config/zookeeper.properties 파일을 열어서 clientPort에 변경할 Zookeeper port를 입력합니다.
```
shell> vi ~/kafka/config/zookeeper.properties

clientPort=변경할 zookeeper port
```
##### 2) ~/kafka/config/server.properties 파일 수정
~/kafka/config/server.properties 파일을 열어서 listeners에 변경할 Kafka port를 입력합니다.

인스턴스 IP 확인 방법
```
콘솔 화면의 Private IP
또는 shell> hostname -i
```
```
shell> vi ~/kafka/config/server.properties

# 주석 해제
listeners=PLAINTEXT://인스턴스 IP:변경할 kafka port

# Zookeeper 포트 변경
zookeeper.connect=인스턴스 IP:변경할 zookeeper port
---> 클러스터인 경우, 각 인스턴스 IP의 Zookeeper port 변경
```

##### 3) Zookeeper, Kafka broker 재시작
```
shell> sudo systemctl stop kafka.service
shell> sudo systemctl stop zookeeper.service

shell> sudo systemctl start zookeeper.service
shell> sudo systemctl start kafka.service
```

##### 4) Zookeeper, Kafka port 변경 확인
변경된 포트가 사용되고 있는지 확인합니다.
```
shell> netstat -ntl | grep [Kafka port]
shell> netstat -ntl | grep [Zookeeper port]
```

### Kafka 토픽 및 데이터 생성/사용

토픽 생성/조회
```
# 인스턴스IP = Private IP / Kafka 기본 port = 9092
# 토픽 생성
shell> ~/kafka/bin/kafka-topics.sh --create --bootstrap-server [인스턴스IP]:[카프카PORT] --topic kafka

# 토픽 리스트 조회
shell> ~/kafka/bin/kafka-topics.sh --list --bootstrap-server [인스턴스IP]:[카프카PORT]

# 토픽 상세 정보 확인
shell> ~/kafka/bin/kafka-topics.sh --describe --bootstrap-server [인스턴스IP]:[카프카PORT] --topic kafka

# 토픽 삭제
shell> ~/kafka/bin/kafka-topics.sh --delete --bootstrap-server [인스턴스IP]:[카프카PORT] --topic kafka
```
데이터 생성/사용
```
# producer 시작
shell> ~/kafka/bin/kafka-console-producer.sh --broker-list [인스턴스IP]:[카프카PORT] --topic kafka

# consumer 시작
shell> ~/kafka/bin/kafka-console-consumer.sh --bootstrap-server [인스턴스IP]:[카프카PORT] --from-beginning --topic kafka
```

## Redis Instance

### Redis 시작/정지
```
# Redis 서비스 시작
shell> sudo systemctl start redis

# Redis 서비스 중지
shell> sudo systemctl stop redis

# Redis 서비스 재시작
shell> sudo systemctl restart redis
```

### Redis 접속
`redis-cli` 커맨드를 이용해 Redis 인스턴스에 접속할 수 있습니다.
```
shell> redis-cli
```

### Redis 인스턴스 생성 후 초기 설정
Redis 인스턴스의 기본 설정 파일은 `~/redis/redis.conf` 입니다. 변경해야 할 파라미터에 대한 설명은 아래와 같습니다.

#### bind
- 기본 값: `127.0.0.1 -::1`
- 변경 값: `<private ip> 127.0.0.1 -::1`

Redis가 사용할 ip에 대한 값입니다. 서버 외부에서 Redis 인스턴스로의 접근을 허용하려면 해당 파라미터에 private ip를 추가해야 합니다. private ip는 `hostname -I` 커맨드로 확인할 수 있습니다.

#### port
- 기본 값: `6379`

포트는 Redis 기본값인 6379입니다. 보안상 포트 변경을 권장합니다. 포트를 변경한 뒤에는 아래 커맨드로 Redis에 접속할 수 있습니다.

```
shell> redis-cli -p <새로운 포트>
```

#### requirepass/masterauth
- 기본 값: `nhncloud`

기본 비밀번호는 `nhncloud`입니다. 보안상 비밀번호 변경을 권장합니다. 복제 연결을 사용할 경우 `requirepass`와 `masterauth`값을 동시에 변경해야 합니다.

### 자동 HA 구성 스크립트
NHN Cloud의 Redis 인스턴스는 자동으로 HA 환경을 구성해 주는 스크립트를 제공합니다. 스크립트는 반드시 **설치 직후의 신규 인스턴스**에서만 사용할 수 있으며, redis.conf에서 설정값을 변경한 경우에는 사용할 수 없습니다.

스크립트를 사용하기 위해서는 다음 설정이 필수적으로 필요합니다.

##### 키 페어 복사
설치 스크립트를 수행하는 인스턴스에 타 인스턴스 접속에 필요한 키 페어(PEM 파일)가 있어야 합니다. 키 페어는 다음과 같이 복사할 수 있습니다.

- centos
```
local> scp -i <키 페어>.pem <키 페어>.pem centos@<floating ip>:/home/centos/
```
- ubuntu
```
local> scp -i <키 페어>.pem <키 페어>.pem ubuntu@<floating ip>:/home/ubuntu/
```

생성한 인스턴스들의 키 페어는 모두 동일해야 합니다.

##### 보안 그룹 설정
Redis 인스턴스간의 통신을 위해 보안 그룹(**Network** > **Security Groups**) 설정이 필요합니다. 아래 규칙으로 보안 그룹을 생성한 뒤 Redis 인스턴스에 적용하세요.

| 방향 | IP 프로토콜| 포트 범위| Ether| 원격|
| --- | --- | --- | --- | --- |
| 수신|TCP | 6379| IPv4| 인스턴스 IP(CIDR)|
| 수신|TCP | 16379| IPv4| 인스턴스 IP(CIDR)|
| 수신|TCP | 26379| IPv4| 인스턴스 IP(CIDR)|

#### Sentinel 자동구성
Sentinel 구성을 위해 3개의 Redis 인스턴스가 필요합니다. 마스터로 사용할 인스턴스에 키 페어를 복사한 뒤 아래와 같이 스크립트를 수행하세요.

```
shell> sh .redis_make_sentinel.sh
```

이후 마스터와 복제본의 private IP를 차례로 입력합니다. 각 인스턴스의 private IP는 `hostname -I` 커맨드로 확인할 수 있습니다.

```
shell> sh .redis_make_sentinel.sh
Enter Master's IP: 192.168.0.33
Enter Replica-1's IP: 192.168.0.27
Enter Replica-2's IP: 192.168.0.97
```

복사해 온 키 페어의 파일명을 입력합니다.
```
shell> Enter Pemkey's name: <키 페어>.pem
```

#### Cluster 자동 구성
Cluster 구성을 위해 6개의 Redis 인스턴스가 필요합니다. 마스터로 사용할 인스턴스에 키 페어를 복사한 뒤 아래와 같이 스크립트를 수행하세요.

```
shell> sh .redis_make_cluster.sh
```

이후 클러스터에 사용할 Redis 인스턴스의 private IP를 차례로 입력합니다. 각 인스턴스의 private IP는 `hostname -I` 커맨드로 확인할 수 있습니다.

```
shell> sh .redis_make_cluster.sh
Enter cluster-1'IP:  192.168.0.79
Enter cluster-2'IP: 192.168.0.10
Enter cluster-3'IP: 192.168.0.33
Enter cluster-4'IP:  192.168.0.116
Enter cluster-5'IP:  192.168.0.91
Enter cluster-6'IP:  192.168.0.32
```

복사해 온 키 페어의 파일명을 입력합니다.

```
shell> Enter Pemkey's name: <키 페어>.pem
```

`yes`를 입력해 클러스터 구성을 완료합니다.
```
>>> Performing hash slots allocation on 6 nodes...
Master[0] -> Slots 0 - 5460
Master[1] -> Slots 5461 - 10922
Master[2] -> Slots 10923 - 16383
Adding replica 192.168.0.91:6379 to 192.168.0.79:6379
Adding replica 192.168.0.32:6379 to 192.168.0.10:6379
Adding replica 192.168.0.116:6379 to 192.168.0.33:6379
M: 0a6ee5bf24141f0058c403d8cc42b349cdc09752 192.168.0.79:6379
   slots:[0-5460] (5461 slots) master
M: b5d078bd7b30ddef650d9a7fa9735e7648efc86f 192.168.0.10:6379
   slots:[5461-10922] (5462 slots) master
M: 0da9b78108b6581bdb90002cbdde3506e9173dd8 192.168.0.33:6379
   slots:[10923-16383] (5461 slots) master
S: 078b4ce014a52588e23577b3fc2dabf408723d68 192.168.0.116:6379
   replicates 0da9b78108b6581bdb90002cbdde3506e9173dd8
S: caaae4ebd3584c0481205e472d6bd0f9dc5c574e 192.168.0.91:6379
   replicates 0a6ee5bf24141f0058c403d8cc42b349cdc09752
S: ab2aa9e37cee48ef8e4237fd63e8301d81193818 192.168.0.32:6379
   replicates b5d078bd7b30ddef650d9a7fa9735e7648efc86f
Can I set the above configuration? (type 'yes' to accept):
```

```
[OK] All nodes agree about slots configuration.
>>> Check for open slots...
>>> Check slots coverage...
[OK] All 16384 slots covered.
```
