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
shell> systemctl start postgresql-13

#postgresql 서비스 중지
shell> systemctl stop postgresql-13

#postgresql 서비스 재시작
shell> systemctl restart postgresql-13
```

### PostgreSQL 접속

이미지 생성 후 초기에는 아래와 같이 접속합니다.
<br>
```
#postgres로 계정 전환 후 접속
shell> su - postgres
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

shell> systemctl restart postgresql-13


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

shell> systemctl restart postgresql-13


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

shell> systemctl restart postgresql-13
```

### PostgreSQL 디렉터리 설명

PostgreSQL 디렉터리 및 파일 설명은 아래와 같습니다.

| 이름 | 설명 |
| --- | --- |
| postgresql.cnf | /var/lib/pgsql/{version}/data/postgresql.cnf |
| initdb.log | PostgreSQL 데이터베이스 클러스터 생성 log - /var/lib/pgsql/{version}/initdb.log |
| DATADIR | PostgreSQL 데이터 파일 경로 - /var/lib/pgsql/{version}/data/ |
| LOG | PostgreSQL log 파일 경로 - /var/lib/pgsql/{version}/data/log/\*.log |
