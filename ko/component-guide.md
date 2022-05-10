## Compute > Instance > 설치 구성 요소 가이드

## NAT Instance
NAT 인스턴스는 프라이빗 네트워크 인스턴스에서 특정 IP 주소 대역에 대해 인터넷에 액세스할 수 있게 하는 인스턴스입니다.
한국(평촌) 리전에서만 제공하는 기능입니다.

### 주요 기능
* 인터넷 게이트웨이가 연결되지 않은 프라이빗 네트워크의 인스턴스가 NAT 인스턴스를 통해 인터넷에 액세스할 수 있습니다.
* NAT 인스턴스의 플로팅 IP를 소스 IP로 변경하여 인터넷에 액세스합니다.
* NAT 인스턴스까지 전달된 패킷은 NAT 인스턴스의 서브넷에 연결된 라우팅 테이블의 라우트 설정에 따라 패킷을 전달합니다.
* NAT 인스턴스의 서브넷에 연결된 라우팅 테이블에는 해당 NAT 인스턴스를 게이트웨이로하는 라우트 설정을 추가하면 안 됩니다.
* 인터넷상에서 시작한 유입 트래픽은 프라이빗 네트워크의 인스턴스가 수신하지 못합니다.
* NAT 인스턴스를 통해서 인터넷 액세스를 허용할 목적지 IP 주소 대역을 지정할 수 있습니다.
* NAT 인스턴스에 접속하여 다른 인스턴스로 접속할 수 있습니다.
* 보안 그룹을 설정할 수 있습니다.
* 네트워크 ACL을 설정할 수 있습니다.
* 이중화를 지원하지 않습니다.
* NAT 인스턴스는 네트워크 인터페이스 설정에서 네트워크 소스/대상 확인을 반드시 비활성화로 설정해야 합니다.
* NAT 인스턴스로 개인 이미지를 생성하는 경우 기능이 정상적으로 동작하지 않을 수 있습니다.

> [참고] NAT 게이트웨이와의 차이
>
> | 구분 | NAT 게이트웨이 | NAT 인스턴스 |
> |--|--|--|
> |가용성| 이중화 지원 | 이중화 지원 안 함 |
> |유지 관리|NHN Cloud에서 관리| 사용자가 직접 관리|
> |보안 그룹|설정 불가| 설정 가능|액
> |네트워크 ACL| 설정 가능 | 설정 가능|
> |SSH|사용 불가| 사용 가능|

### 소스/대상 확인 설정
NAT 인스턴스가 정상적으로 동작하려면 네트워크 인터페이스 설정에서 네트워크 소스/대상 확인을 비활성화해야 합니다.

### 라우트 설정
NAT 인스턴스를 라우트 게이트웨이로 지정합니다. NAT 인스턴스까지 전달된 패킷은 NAT 인스턴스의 서브넷에 연결된 라우팅 테이블의 라우트 설정에 따라 패킷을 전달합니다.

### 설정 주의 사항
* NAT 인스턴스의 서브넷에 연결된 라우팅 테이블에는 해당 NAT 인스턴스를 게이트웨이로 하는 라우트 설정을 추가하지 않아야 합니다. 
* NAT 인스턴스 서브넷과 NAT 인스턴스를 게이트웨이로 사용할 인스턴스들의 서브넷을 분리하고, 서로 다른 라우팅 테이블을 사용할 것을 강력히 권고합니다.
* 만약, NAT 인스턴스 서브넷과 NAT 인스턴스를 게이트웨이로 사용할 인스턴스들의 서브넷이 동일하거나 같은 라우팅 테이블에 연결되어 있을 때, NAT 인스턴스를 게이트웨이로 하는 라우트 설정을 추가해야 한다면, 해당 NAT 인스턴스에는 반드시 플로팅 IP를 설정해야 합니다. 또한, 대상 CIDR은 IP Prefix 0 (/0)으로만 지정해야 합니다. 이외 다른 설정을 하면 통신이 되지 않으며, 그 라우팅 테이블을 사용하는 모든 인스턴스들의 통신에도 영향을 줄 수 있습니다.

> [참고] NAT 인스턴스에 대한 라우팅 설정
>
> * NAT 인스턴스의 서브넷이 라우팅 테이블 1에 연결되어 있고 NAT 인스턴스를 게이트웨이로 사용할 인스턴스들이 라우팅 테이블 2에 연결되어 있는 경우
>     * 라우팅 테이블 2의 라우트 설정에 특정 CIDR(예: 8.8.8.8/32)에 대해서 NAT 인스턴스를 게이트웨이로 지정할 수 있습니다.
>     * 라우팅 테이블 1의 라우트 설정에는 NAT 인스턴스를 게이트웨이로 지정하면 안 됩니다. 다만, NAT 인스턴스가 플로팅 IP에 연결된 경우 예외적으로 IP prefix 0 (/0)을 대상 CIDR로 설정할 수 있습니다.
> * NAT 인스턴스 서브넷과 NAT 인스턴스를 게이트웨이로 사용할 인스턴스들의 서브넷이 라우팅 테이블 1에 같이 연결되어 있는 경우
>     * NAT 인스턴스가 플로팅 IP에 연결되어 있다면 라우트 대상 CIDR에 IP Prefix 0 (/0)을 설정할 수 있습니다.
>     * 위 설정 이외에는, 라우팅 테이블 1의 라우팅 설정에 NAT 인스턴스를 게이트웨이로 지정하면 안 됩니다.


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

초기 설치 후 MySQL ROOT 계정 비밀번호는 지정되어 있지 않습니다. 그러므로 설치 후 반드시 바로 비밀번호를 설정해야 합니다.

* MySQL 5.6 버전 비밀번호 설정

```
SET PASSWORD [FOR user] = password_option

mysql> set password=password('비밀번호');
```

* MySQL 5.7 버전 비밀번호 설정

```
ALTER USER USER() IDENTIFIED BY 'auth_string';

mysql> ALTER USER 'root'@'localhost' IDENTIFIED BY '새로운 비밀번호';
```

MySQL 기본 validate\_password\_policy는 아래와 같습니다\.

* validate\_password\_policy=MEDIUM
* 기**본 8자 이상, 숫자, 소문자, 대문자, 특수문자**를 포함해야 함

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
shell> mysql -u root
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

## JEUS Instance

기본 제공되는 이미지에는 CentOS 7.8 with JEUS8Fix1(Domain Administrator Server 2022.03.22), CentOS 7.8 with JEUS8Fix1(Managed Server 2022.03.22)이 포함됩니다.
Domain Administrator Server를 설치하기 위해서는 CentOS 7.8 with JEUS8Fix1(Domain Administrator Server 2022.03.22) 이미지를 사용합니다.
Managed Server를 설치하기 위해서는 CentOS 7.8 with JEUS8Fix1(Managed Server 2022.03.22) 이미지를 사용합니다.

JEUS 는 `~/apps/jeus8`에 설치됩니다.

설치 시 아래 속성들로 설정됩니다.

| 구분 | 기본값 |
| --- | --- |
| 도메인 이름 | jeus_domain |
| WebAdmin 포트 | 9736 |
| 어드민 서버 이름 | adminServer |
| 어드민 유저 아이디 | administrator |
| 어드민 유저 비밀번호 | jeusadmin |
| 노드 매니저 | java |


### 기동 확인

JEUS를 설정하거나 제어하려면 노드 매니저를 기동한 후 WebAdmin이나 jeusadmin을 통해서 제어해야 합니다.

### 인스턴스 접속

인스턴스 생성 완료 후 SSH를 사용하여 인스턴스에 접근합니다.
인스턴스에 플로팅 IP가 연결되어 있어야 하며 보안 그룹에서 TCP 포트 22(SSH)가 허용되어야 합니다.

### 노드 매니저 기동

셸로 접속하여 startNodeManager 명령어로 노드 매니저를 실행합니다.
노드 매니저끼리 통신이 필요하므로 보안 그룹에 기본 포트인 7730에 대한 허용 규칙을 추가해야 합니다.

### JEUS 기동

Domain Administrator Server는 startDomainAdminServer 명령어로 실행합니다.

```
startDomainAdminServer -uadministrator -pjeusadmin
```

### JEUS WebAdmin

다음과 같이 WebAdmin을 실행합니다.

1. DAS가 설치된 인스턴스에 플로팅 IP를 설정합니다.
2. 해당 인스턴스의 보안 그룹에 9736 포트에 대한 허용 규칙을 추가합니다.
3. 웹 브라우저에서 http://{플로팅 IP}:9736/webadmin으로 접속하면 WebAdmin 화면을 볼 수 있습니다.



## WebtoB Instance

기본 제공되는 이미지는 CentOS 7.8 with WebtoB5Fix4 (2022.03.22)입니다.
WebtoB 는 `~/apps/webtob` 에 설치됩니다.

### 기동 확인

#### 인스턴스 접속

인스턴스 생성 완료 후 SSH를 사용하여 인스턴스에 접근합니다.
인스턴스에 플로팅 IP가 연결되어 있어야 하며 보안 그룹에서 TCP 포트 22(SSH)가 허용되어야 합니다.

#### 환경 설정 파일 컴파일

wscfl 명령어를 이용하여 설정 파일을 컴파일합니다.

```
wscfl -i http.m
```

#### WebtoB 기동

wsboot를 이용하여 WebtoB를 기동합니다.

```
wsboot
```

wsadmin을 이용하여 상태를 확인하거나 제어할 수 있습니다.
