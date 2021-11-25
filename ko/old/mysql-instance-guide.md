## Compute > Instance > 특화된 인스턴스 사용 가이드 > MySQL Instance 가이드
## MySQL version

MySQL version은 다음과 같이 2가지 종류가 제공됩니다.

* MySQL Community Server 5.6.38
    * mysql-community-server-5.6.38-2.el6.x86_64
* MySQL Community Server 5.7.20
    * mysql-community-server-5.7.20-1.el6.x86_64

## MySQL 시작/정지 방법

```
#mysql 서비스 시작
shell> service mysqld start

#mysql 서비스 중지
shell> service mysqld stop

#mysql 서비스 재시작
shell> service mysqld restart
```

## MySQL 접속

이미지 생성 후 초기에는 아래와 같이 접속합니다.

```
shell> mysql -uroot
```

## MySQL 이미지 생성 후 초기 설정

### 1\. 비밀번호 변경

초기 설치 후 MySQL ROOT 계정 비밀번호는 지정되어 있지 않습니다. 그러므로 설치 후 반드시 바로 비밀번를 변경해야 합니다.

* MySQL 5.6 버전 비밀번호 변경

```
SET PASSWORD [FOR user] = password_option

mysql> set password=password('비밀번호');
```

* MySQL 5.7 버전 비밀번호 변경

```
ALTER USER USER() IDENTIFIED BY 'auth_string';

mysql> ALTER USER 'root'@'localhost' IDENTIFIED BY '새로운 비밀번호';
```

MySQL 기본 validate\_password\_policy는 아래와 같습니다\.

* validate\_password\_policy=MEDIUM
* 기**본 8자 이상, 숫자, 소문자, 대문자, 특수문자**를 포함해야 함

### 2\. 포트(port) 변경

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

## my.cnf 설명

my.cnf 의 기본 경로는 /etc/my.cnf 이고 TOAST 권장 변수(variable)가 설정되어 있으며, 내용은 아래와 같습니다.

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

## MySQL 디렉터리 설명

MySQL 디렉터리 및 파일 설명은 아래와 같습니다.

| 이름 | 설명 |
| --- | --- |
| my.cnf | /etc/my.cnf |
| DATADIR | MySQL 데이터 파일 경로  - /var/lib/mysql/ |
| ERROR_LOG | MySQL error_log 파일 경로  - /var/log/mysqld.log |
| SLOW_LOG | MySQL Slow Query 파일 경로 -  <span style="color:#333333">/var/lib/mysql/*slow.log</span> |
