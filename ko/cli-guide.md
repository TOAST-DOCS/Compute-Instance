# OpenStack CLI 사용자 가이드

## Compute > Instance > CLI 가이드

이 문서는 OpenStack CLI(Command Line Interface)를 통해 NHN Cloud를 사용하는 방법을 설명합니다.

## 개요

OpenStack CLI는 NHN Cloud의 다양한 서비스에 접근할 수 있는 명령줄 도구입니다. 이 가이드를 통해 다음 서비스들을 CLI로 관리할 수 있습니다:

- **Compute**: 인스턴스, 이미지, 키페어 정책
- **Network**: VPC, 서브넷, 라우터, 보안 그룹
- **Storage**: 블록 스토리지, 스냅샷, 오브젝트 스토리지

## 사전 준비

### 1. Python 환경 설정

OpenStack CLI는 Python을 기반으로 동작합니다. Python 3.9 이상 버전을 권장합니다.

```bash
# Python 버전 확인
python3 --version

# virtualenv를 통한 격리된 환경 구성
virtualenv .venv --python=python3.9
source .venv/bin/activate  # Linux/macOS
# 또는
.venv\Scripts\activate     # Windows
```

### 2. OpenStack CLI 클라이언트 설치

#### 통합 클라이언트 설치 (권장)

```bash
pip install python-openstackclient
```

#### 개별 서비스 클라이언트 설치

필요한 서비스만 선택적으로 설치할 수 있습니다:

```bash
# Compute 서비스 (인스턴스, 이미지 등)
pip install python-novaclient

# Network 서비스 (네트워크, 보안그룹 등)
pip install python-neutronclient

# Storage 서비스 (블록 스토리지)
pip install python-cinderclient

# Image 서비스 (이미지 관리)
pip install python-glanceclient

# Orchestration 서비스 (Heat 템플릿)
pip install python-heatclient

# Object Storage 서비스
pip install python-swiftclient
```

### 3. 설치 확인

```bash
openstack --version
```

## 인증 설정

NHN Cloud에서 OpenStack CLI를 사용하기 위해서는 인증 정보를 설정해야 합니다. 두 가지 방법을 제공합니다.

### 방법 1: clouds.yaml 파일 사용 (권장)

#### 1.1 API 엔드포인트 설정

NHN Cloud 콘솔에서 API 엔드포인트를 설정합니다:

1. NHN Cloud 콘솔에 로그인
2. **API 엔드포인트 설정** 메뉴 접근
3. API 비밀번호 설정
4. 테넌트 ID와 사용자 UUID 확인

#### 1.2 clouds.yaml 파일 생성

OpenStack CLI는 다음 위치에서 `clouds.yaml` 파일을 찾습니다:
- 현재 디렉토리
- `~/.config/openstack/`
- `/etc/openstack/`

`~/.config/openstack/clouds.yaml` 파일을 생성하고 다음과 같이 설정합니다:

```yaml
clouds:
  nhn_cloud:
    auth:
      auth_url: https://kr1-api-identity.infrastructure.cloud.toast.com
      username: {사용자_UUID}
      password: {API_비밀번호}
      project_name: c_{프로젝트_ID}
      project_domain_name: default
      user_domain_name: default
    region_name: KR1
    identity_api_version: 3
    volume_api_version: 3
    image_api_version: 2
    interface: public
```

#### 1.3 설정 확인

```bash
# 클라우드 설정 확인
openstack --os-cloud nhn_cloud token issue

# alias 설정 (선택사항)
echo 'alias nhncloud="openstack --os-cloud nhn_cloud"' >> ~/.bashrc
source ~/.bashrc

# alias 사용
nhncloud token issue
```

### 방법 2: 환경변수 사용

#### 2.1 keystonerc 파일 생성

```bash
touch ~/keystonerc_nhncloud
```

#### 2.2 환경변수 설정

`~/keystonerc_nhncloud` 파일에 다음 내용을 추가합니다:

```bash
# 기본 도메인 설정
export OS_PROJECT_DOMAIN_ID=default
export OS_USER_DOMAIN_ID=default

# 프로젝트 정보
export OS_PROJECT_NAME=c_{프로젝트_ID}
export OS_TENANT_NAME=c_{프로젝트_ID}

# 사용자 정보
export OS_USERNAME={사용자_UUID}
export OS_PASSWORD={API_비밀번호}

# 인증 URL
export OS_AUTH_URL=https://kr1-api-identity.infrastructure.cloud.toast.com

# API 버전
export OS_IDENTITY_API_VERSION=3
export OS_VOLUME_API_VERSION=3
export OS_IMAGE_API_VERSION=2

# 리전 설정
export OS_REGION_NAME=KR1
export OS_INTERFACE=public
```

#### 2.3 환경변수 적용

```bash
source ~/keystonerc_nhncloud

# 설정 확인
openstack token issue
```

### 리전별 설정

NHN Cloud는 여러 리전을 제공합니다. 리전에 따라 `OS_REGION_NAME`을 다르게 설정해야 합니다:

| 리전 | OS_REGION_NAME | 설명 |
|------|----------------|------|
| 한국(판교) | KR1 | 기본 리전 |
| 한국(평촌) | KR2 | 보조 리전 |

## 기본 CLI 명령어

### 도움말 및 버전 확인

```bash
# 전체 도움말
openstack --help

# 특정 명령어 도움말
openstack server --help

# 버전 확인
openstack --version
```

### 인증 토큰 관리

```bash
# 토큰 발급
openstack token issue

# 토큰 정보 확인
openstack token show

# 토큰 삭제
openstack token revoke
```

## Compute 서비스 관리

### 인스턴스 관리

#### 인스턴스 목록 조회

```bash
# 기본 인스턴스 목록
openstack server list

# 상세 정보 포함
openstack server list --long

# 특정 상태의 인스턴스만 조회
openstack server list --status ACTIVE

# 특정 이름 패턴으로 조회
openstack server list --name "web-*"
```

#### 인스턴스 상세 정보 조회

```bash
# 특정 인스턴스 정보
openstack server show {인스턴스_ID}

# 인스턴스 콘솔 URL 조회
openstack console url show {인스턴스_ID}

# 인스턴스 로그 조회
openstack console log show {인스턴스_ID}
```

#### 인스턴스 생성

```bash
# 기본 인스턴스 생성
openstack server create \
  --image {이미지_ID} \
  --flavor {플레이버_ID} \
  --network {네트워크_ID} \
  --key-name {키페어_이름} \
  --security-group {보안그룹_이름} \
  {인스턴스_이름}

# 상세 옵션을 포함한 인스턴스 생성
openstack server create \
  --image {이미지_ID} \
  --flavor {플레이버_ID} \
  --network {네트워크_ID} \
  --key-name {키페어_이름} \
  --security-group {보안그룹_이름} \
  --availability-zone {가용영역} \
  --user-data {사용자데이터_파일} \
  --property key=value \
  {인스턴스_이름}
```

#### 인스턴스 상태 관리

```bash
# 인스턴스 시작
openstack server start {인스턴스_ID}

# 인스턴스 중지
openstack server stop {인스턴스_ID}

# 인스턴스 재부팅
openstack server reboot {인스턴스_ID}

# 강제 재부팅
openstack server reboot --hard {인스턴스_ID}

# 인스턴스 일시중지
openstack server pause {인스턴스_ID}

# 인스턴스 일시중지 해제
openstack server unpause {인스턴스_ID}

# 인스턴스 종료 (삭제)
openstack server delete {인스턴스_ID}
```

#### 인스턴스 수정

```bash
# 인스턴스 이름 변경
openstack server set --name {새_이름} {인스턴스_ID}

# 메타데이터 추가
openstack server set --property key=value {인스턴스_ID}

# 메타데이터 삭제
openstack server unset --property key {인스턴스_ID}
```

### 이미지 관리

#### 이미지 목록 조회

```bash
# 모든 이미지 조회
openstack image list

# 공개 이미지만 조회
openstack image list --public

# 특정 상태의 이미지 조회
openstack image list --status active
```

#### 이미지 상세 정보

```bash
# 이미지 상세 정보
openstack image show {이미지_ID}

# 이미지 메타데이터 조회
openstack image show --property key {이미지_ID}
```

#### 이미지 생성

```bash
# 인스턴스에서 이미지 생성
openstack server image create --name {이미지_이름} {인스턴스_ID}

# 외부 이미지 업로드
openstack image create \
  --disk-format qcow2 \
  --container-format bare \
  --file {이미지_파일} \
  {이미지_이름}
```

#### 이미지 삭제

```bash
openstack image delete {이미지_ID}
```

### 플레이버 관리

#### 플레이버 목록 조회

```bash
# 모든 플레이버 조회
openstack flavor list

# 상세 정보 포함
openstack flavor list --long
```

#### 플레이버 상세 정보

```bash
openstack flavor show {플레이버_ID}
```

### 키페어 관리

#### 키페어 목록 조회

```bash
openstack keypair list
```

#### 키페어 생성

```bash
# 새 키페어 생성
openstack keypair create {키페어_이름}

# 기존 공개키 등록
openstack keypair create --public-key {공개키_파일} {키페어_이름}
```

#### 키페어 삭제

```bash
openstack keypair delete {키페어_이름}
```

### 플레이스먼트 정책 관리

#### 플레이스먼트 정책 생성

```bash
# anti-affinity 정책 생성
openstack server group create \
  --policy anti-affinity \
  {정책_이름}
```

#### 플레이스먼트 정책 목록

```bash
openstack server group list
```

#### 플레이스먼트 정책 삭제

```bash
openstack server group delete {정책_ID}
```

## Network 서비스 관리

### 네트워크 관리

#### 네트워크 목록 조회

```bash
# 모든 네트워크 조회
openstack network list

# 외부 네트워크만 조회
openstack network list --external

# 특정 프로젝트의 네트워크 조회
openstack network list --project {프로젝트_ID}
```

#### 네트워크 생성

```bash
# 기본 네트워크 생성
openstack network create {네트워크_이름}

# 외부 네트워크 생성
openstack network create \
  --external \
  --provider-network-type flat \
  --provider-physical-network physnet1 \
  {네트워크_이름}
```

#### 네트워크 삭제

```bash
openstack network delete {네트워크_ID}
```

### 서브넷 관리

#### 서브넷 목록 조회

```bash
openstack subnet list
```

#### 서브넷 생성

```bash
openstack subnet create \
  --network {네트워크_ID} \
  --subnet-range {CIDR} \
  --gateway {게이트웨이_IP} \
  --dns-nameserver {DNS_서버} \
  {서브넷_이름}
```

#### 서브넷 삭제

```bash
openstack subnet delete {서브넷_ID}
```

### 라우터 관리

#### 라우터 목록 조회

```bash
openstack router list
```

#### 라우터 생성

```bash
openstack router create {라우터_이름}
```

#### 라우터에 서브넷 연결

```bash
openstack router add subnet {라우터_ID} {서브넷_ID}
```

#### 라우터에서 서브넷 분리

```bash
openstack router remove subnet {라우터_ID} {서브넷_ID}
```

#### 라우터 삭제

```bash
openstack router delete {라우터_ID}
```

### 보안 그룹 관리

#### 보안 그룹 목록 조회

```bash
openstack security group list
```

#### 보안 그룹 생성

```bash
openstack security group create {보안그룹_이름}
```

#### 보안 그룹 규칙 추가

```bash
# 인바운드 규칙 추가
openstack security group rule create \
  --protocol {프로토콜} \
  --dst-port {포트} \
  --ingress \
  {보안그룹_ID}

# 아웃바운드 규칙 추가
openstack security group rule create \
  --protocol {프로토콜} \
  --egress \
  {보안그룹_ID}
```

#### 보안 그룹 삭제

```bash
openstack security group delete {보안그룹_ID}
```

## Storage 서비스 관리

### 블록 스토리지 관리

#### 볼륨 목록 조회

```bash
# 모든 볼륨 조회
openstack volume list

# 특정 상태의 볼륨 조회
openstack volume list --status available
```

#### 볼륨 생성

```bash
# 기본 볼륨 생성
openstack volume create \
  --size {크기_GB} \
  {볼륨_이름}

# 특정 볼륨 타입으로 생성
openstack volume create \
  --size {크기_GB} \
  --type {볼륨_타입} \
  {볼륨_이름}

# 이미지에서 볼륨 생성
openstack volume create \
  --size {크기_GB} \
  --image {이미지_ID} \
  {볼륨_이름}
```

#### 볼륨 연결/분리

```bash
# 인스턴스에 볼륨 연결
openstack server add volume {인스턴스_ID} {볼륨_ID}

# 인스턴스에서 볼륨 분리
openstack server remove volume {인스턴스_ID} {볼륨_ID}
```

#### 볼륨 삭제

```bash
openstack volume delete {볼륨_ID}
```

### 스냅샷 관리

#### 스냅샷 목록 조회

```bash
openstack volume snapshot list
```

#### 스냅샷 생성

```bash
openstack volume snapshot create \
  --volume {볼륨_ID} \
  {스냅샷_이름}
```

#### 스냅샷에서 볼륨 생성

```bash
openstack volume create \
  --snapshot {스냅샷_ID} \
  --size {크기_GB} \
  {볼륨_이름}
```

#### 스냅샷 삭제

```bash
openstack volume snapshot delete {스냅샷_ID}
```

## 고급 사용법

### 배치 작업

```bash
# 여러 인스턴스 동시 생성
for i in {1..5}; do
  openstack server create \
    --image {이미지_ID} \
    --flavor {플레이버_ID} \
    --network {네트워크_ID} \
    --key-name {키페어_이름} \
    "web-server-$i"
done

# 특정 조건의 리소스 일괄 삭제
openstack server list --name "temp-*" -f value -c ID | xargs -I {} openstack server delete {}
```

### 출력 형식 커스터마이징

```bash
# JSON 형식으로 출력
openstack server list -f json

# CSV 형식으로 출력
openstack server list -f csv

# 특정 컬럼만 출력
openstack server list -c ID -c Name -c Status

# 정렬
openstack server list --sort-column Name
```

### 스크립트 예제

#### 인스턴스 백업 스크립트

```bash
#!/bin/bash
# 인스턴스 백업 스크립트

INSTANCE_ID=$1
BACKUP_NAME="backup-$(date +%Y%m%d-%H%M%S)"

if [ -z "$INSTANCE_ID" ]; then
    echo "Usage: $0 <instance_id>"
    exit 1
fi

echo "Creating backup for instance $INSTANCE_ID..."

# 인스턴스 중지
openstack server stop $INSTANCE_ID

# 인스턴스 상태 확인
while [ "$(openstack server show $INSTANCE_ID -f value -c Status)" != "SHUTOFF" ]; do
    echo "Waiting for instance to stop..."
    sleep 10
done

# 이미지 생성
openstack server image create --name $BACKUP_NAME $INSTANCE_ID

# 인스턴스 시작
openstack server start $INSTANCE_ID

echo "Backup completed: $BACKUP_NAME"
```

## 문제 해결

### 일반적인 오류

#### 인증 오류

```bash
# 토큰 만료 시
openstack token issue

# 잘못된 인증 정보 시
# clouds.yaml 또는 환경변수 재확인
```

#### 권한 오류

```bash
# 프로젝트 권한 확인
openstack project list

# 사용자 권한 확인
openstack user show {사용자_ID}
```

### 디버깅

```bash
# 상세 로그 출력
openstack --debug server list

# API 호출 정보 출력
openstack --timing server list

# 버전 정보 확인
openstack --version
```

## 실용적인 사용 예제

### 웹 서버 인프라 구축

```bash
# 1. 네트워크 생성
openstack network create web-network
openstack subnet create \
  --network web-network \
  --subnet-range 192.168.1.0/24 \
  --gateway 192.168.1.1 \
  web-subnet

# 2. 보안 그룹 생성
openstack security group create web-sg
openstack security group rule create \
  --protocol tcp \
  --dst-port 22 \
  --ingress \
  web-sg
openstack security group rule create \
  --protocol tcp \
  --dst-port 80 \
  --ingress \
  web-sg
openstack security group rule create \
  --protocol tcp \
  --dst-port 443 \
  --ingress \
  web-sg

# 3. 키페어 생성
openstack keypair create web-keypair

# 4. 웹 서버 인스턴스 생성
openstack server create \
  --image {웹서버_이미지_ID} \
  --flavor {플레이버_ID} \
  --network web-network \
  --key-name web-keypair \
  --security-group web-sg \
  --user-data web-init.sh \
  web-server-01
```

### 데이터베이스 서버 백업

```bash
# 1. 데이터베이스 서버 중지
openstack server stop db-server-01

# 2. 서버 상태 확인
while [ "$(openstack server show db-server-01 -f value -c Status)" != "SHUTOFF" ]; do
    echo "Waiting for server to stop..."
    sleep 10
done

# 3. 이미지 생성
openstack server image create \
  --name "db-backup-$(date +%Y%m%d)" \
  db-server-01

# 4. 서버 재시작
openstack server start db-server-01
```

### 자동화 스크립트 예제

#### 리소스 정리 스크립트

```bash
#!/bin/bash
# 테스트 리소스 정리 스크립트

echo "Cleaning up test resources..."

# 테스트 인스턴스 삭제
for instance in $(openstack server list --name "test-*" -f value -c ID); do
    echo "Deleting instance: $instance"
    openstack server delete $instance
done

# 테스트 볼륨 삭제
for volume in $(openstack volume list --name "test-*" -f value -c ID); do
    echo "Deleting volume: $volume"
    openstack volume delete $volume
done

# 테스트 네트워크 삭제
for network in $(openstack network list --name "test-*" -f value -c ID); do
    echo "Deleting network: $network"
    openstack network delete $network
done

echo "Cleanup completed!"
```

## 모니터링 및 로깅

### 리소스 사용량 모니터링

```bash
# 인스턴스 목록과 상태 확인
openstack server list --long

# 볼륨 사용량 확인
openstack volume list --long

# 네트워크 사용량 확인
openstack network list --long

# 보안 그룹 규칙 확인
openstack security group rule list {보안그룹_ID}
```

### 로그 분석

```bash
# 인스턴스 콘솔 로그 확인
openstack console log show {인스턴스_ID}

# 상세 디버그 정보 출력
openstack --debug server show {인스턴스_ID}

# API 호출 타이밍 정보
openstack --timing server list
```

## 성능 최적화

### 배치 작업 최적화

```bash
# 병렬 처리로 여러 인스턴스 생성
for i in {1..10}; do
    openstack server create \
      --image {이미지_ID} \
      --flavor {플레이버_ID} \
      --network {네트워크_ID} \
      --key-name {키페어_이름} \
      "batch-server-$i" &
done
wait

# 백그라운드에서 작업 실행
nohup openstack server image create --name backup-$(date +%Y%m%d) {인스턴스_ID} &
```

### 출력 최적화

```bash
# JSON 형식으로 출력하여 파싱 용이
openstack server list -f json | jq '.[] | {id: .ID, name: .Name, status: .Status}'

# CSV 형식으로 스프레드시트 분석
openstack volume list -f csv > volumes.csv

# 특정 컬럼만 출력하여 성능 향상
openstack server list -c ID -c Name -c Status
```

## 참고 자료

- [OpenStack CLI 공식 문서](https://docs.openstack.org/python-openstackclient/ussuri/)
- [NHN Cloud 콘솔 가이드](https://docs.nhncloud.com/ko/nhncloud/ko/overview/)
- [NHN Cloud API 가이드](https://docs.nhncloud.com/ko/nhncloud/ko/public-api/)
- [OpenStack Ussuri 릴리즈 노트](https://docs.openstack.org/releasenotes/python-openstackclient/ussuri.html)

## 지원

CLI 사용 중 문제가 발생하면 다음을 확인해주세요:

1. OpenStack CLI 버전이 NHN Cloud와 호환되는지 확인
2. 인증 정보가 올바른지 확인
3. 네트워크 연결 상태 확인
4. NHN Cloud 기술지원팀에 문의

### 일반적인 문제 해결

| 문제 | 해결 방법 |
|------|-----------|
| 인증 실패 | `openstack token issue`로 토큰 재발급 |
| 권한 오류 | 프로젝트 권한 및 사용자 역할 확인 |
| 네트워크 연결 실패 | 방화벽 설정 및 DNS 확인 |
| 리소스 생성 실패 | 할당량 및 리전 설정 확인 |

### 유용한 명령어 모음

```bash
# 시스템 정보 확인
openstack --version
openstack token issue
openstack project list
openstack user show {사용자_ID}

# 리소스 상태 확인
openstack server list --long
openstack volume list --long
openstack network list --long
openstack security group list

# 도움말
openstack help
openstack server --help
openstack network --help
openstack volume --help
```

