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

## MariaDB Instance

## Tibero Instance

## JEUS Instance

## WebtoB Instance
