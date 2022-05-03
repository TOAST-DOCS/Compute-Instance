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

## PostgreSQL Instance

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

## Tibero Instance

## JEUS Instance

## WebtoB Instance
