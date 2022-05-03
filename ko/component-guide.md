## Compute > Instance > 설치 구성 요소 가이드


## MS-SQL Instance

## MySQL Instance

## PostgreSQL Instance

## CUBRID Instance
### 인스턴스 접근 
인스턴스 생성 완료 후 SSH를 사용하여 인스턴스에 접근합니다.
인스턴스에 Floating IP가 연결되어 있어야 하며 보안 그룹에서 TCP 포트 22(SSH)가 허용되어야 합니다.

![cubrid_instance_image_3.jpg](http://static.toastoven.net/prod_cubrid_instance/cubrid_instance_image_3.jpg)

SSH 클라이언트와 설정한 키페어를 이용해 인스턴스에 접속합니다.
SSH 연결에 대한 자세한 가이드는 [SSH 연결 가이드](https://docs.toast.com/ko/Compute/Instance/ko/overview/#linux)를 참고하시기 바랍니다.

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

## Tibero Instance

## JEUS Instance

## WebtoB Instance
