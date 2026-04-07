# OpenStack CLI User Guide

## Compute > Instance > CLI Guide

This document describes how to use NHN Cloud through the OpenStack CLI (Command Line Interface).

## Overview

The OpenStack CLI is a command-line tool that provides access to various NHN Cloud services. Through this guide, you can manage the following services via CLI:

- **Compute**: Instances, images, Key Pair policies
- **Network**: VPC, subnets, routers, Security Groups
- **Storage**: Block Storage, snapshots, Object Storage

## Prerequisites

### 1. Python Environment Setup

The OpenStack CLI runs on Python. Python 3.9 or later is recommended.

```bash
# Python 버전 확인
python3 --version

# virtualenv를 통한 격리된 환경 구성
virtualenv .venv --python=python3.9
source .venv/bin/activate  # Linux/macOS
# 또는
.venv\Scripts\activate     # Windows
```

### 2. Installing the OpenStack CLI Client

#### Installing the Unified Client (Recommended)

```bash
pip install python-openstackclient
```

#### Installing Individual Service Clients

You can selectively install only the services you need:

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

### 3. Verifying the Installation

```bash
openstack --version
```

## Authentication Settings

To use the OpenStack CLI with NHN Cloud, you need to configure authentication information. Two methods are provided.

### Method 1: Using the clouds.yaml File (Recommended)

#### 1.1 Setting the API Endpoint

Configure the API Endpoint in the NHN Cloud Console:

1. Log in to the NHN Cloud Console
2. Navigate to the **API Endpoint Settings** menu
3. Set the API password
4. Check the tenant ID and user UUID

#### 1.2 Creating the clouds.yaml File

The OpenStack CLI looks for the `clouds.yaml` file in the following locations:
- Current directory
- `~/.config/openstack/`
- `/etc/openstack/`

Create the `~/.config/openstack/clouds.yaml` file and configure it as follows:

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

#### 1.3 Verifying the Configuration

```bash
# 클라우드 설정 확인
openstack --os-cloud nhn_cloud token issue

# alias 설정 (선택사항)
echo 'alias nhncloud="openstack --os-cloud nhn_cloud"' >> ~/.bashrc
source ~/.bashrc

# alias 사용
nhncloud token issue
```

### Method 2: Using Environment Variables

#### 2.1 Creating the keystonerc File

```bash
touch ~/keystonerc_nhncloud
```

#### 2.2 Configuring Environment Variables

Add the following content to the `~/keystonerc_nhncloud` file:

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

#### 2.3 Applying the Environment Variables

```bash
source ~/keystonerc_nhncloud

# 설정 확인
openstack token issue
```

### Region-Specific Settings

NHN Cloud provides multiple regions. You must set `OS_REGION_NAME` differently depending on the region:

| Region | OS_REGION_NAME | Description |
|------|----------------|------|
| Korea (Pangyo) | KR1 | Default region |
| Korea (Pyeongchon) | KR2 | Secondary region |

## Basic CLI Commands

### Help and Version Check

```bash
# 전체 도움말
openstack --help

# 특정 명령어 도움말
openstack server --help

# 버전 확인
openstack --version
```

### Authentication Token Management

```bash
# 토큰 발급
openstack token issue

# 토큰 정보 확인
openstack token show

# 토큰 삭제
openstack token revoke
```

## Compute Service Management

### Instance Management

#### Retrieve Instance List

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

#### Retrieve Instance Details

```bash
# 특정 인스턴스 정보
openstack server show {인스턴스_ID}

# 인스턴스 콘솔 URL 조회
openstack console url show {인스턴스_ID}

# 인스턴스 로그 조회
openstack console log show {인스턴스_ID}
```

#### Create Instances

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

#### Manage Instance Status

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

#### Modify Instances

```bash
# 인스턴스 이름 변경
openstack server set --name {새_이름} {인스턴스_ID}

# 메타데이터 추가
openstack server set --property key=value {인스턴스_ID}

# 메타데이터 삭제
openstack server unset --property key {인스턴스_ID}
```

### Image Management

#### Retrieve Image List

```bash
# 모든 이미지 조회
openstack image list

# 공개 이미지만 조회
openstack image list --public

# 특정 상태의 이미지 조회
openstack image list --status active
```

#### Image Details

```bash
# 이미지 상세 정보
openstack image show {이미지_ID}

# 이미지 메타데이터 조회
openstack image show --property key {이미지_ID}
```

#### Create Images

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

#### Delete Images

```bash
openstack image delete {이미지_ID}
```

### Flavor Management

#### Retrieve Flavor List

```bash
# 모든 플레이버 조회
openstack flavor list

# 상세 정보 포함
openstack flavor list --long
```

#### Flavor Details

```bash
openstack flavor show {플레이버_ID}
```

### Key Pair Management

#### Retrieve Key Pair List

```bash
openstack keypair list
```

#### Create Key Pairs

```bash
# 새 키페어 생성
openstack keypair create {키페어_이름}

# 기존 공개키 등록
openstack keypair create --public-key {공개키_파일} {키페어_이름}
```

#### Delete Key Pairs

```bash
openstack keypair delete {키페어_이름}
```

### Placement Policy Management

#### Create Placement Policies

```bash
# anti-affinity 정책 생성
openstack server group create \
  --policy anti-affinity \
  {정책_이름}
```

#### Placement Policy List

```bash
openstack server group list
```

#### Delete Placement Policies

```bash
openstack server group delete {정책_ID}
```

## Network Service Management

### Network Management

#### Retrieve Network List

```bash
# 모든 네트워크 조회
openstack network list

# 외부 네트워크만 조회
openstack network list --external

# 특정 프로젝트의 네트워크 조회
openstack network list --project {프로젝트_ID}
```

#### Create Networks

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

#### Delete Networks

```bash
openstack network delete {네트워크_ID}
```

### Subnet Management

#### Retrieve Subnet List

```bash
openstack subnet list
```

#### Create Subnets

```bash
openstack subnet create \
  --network {네트워크_ID} \
  --subnet-range {CIDR} \
  --gateway {게이트웨이_IP} \
  --dns-nameserver {DNS_서버} \
  {서브넷_이름}
```

#### Delete Subnets

```bash
openstack subnet delete {서브넷_ID}
```

### Router Management

#### Retrieve Router List

```bash
openstack router list
```

#### Create Routers

```bash
openstack router create {라우터_이름}
```

#### Attach Subnet to Router

```bash
openstack router add subnet {라우터_ID} {서브넷_ID}
```

#### Detach Subnet from Router

```bash
openstack router remove subnet {라우터_ID} {서브넷_ID}
```

#### Delete Routers

```bash
openstack router delete {라우터_ID}
```

### Security Group Management

#### Retrieve Security Group List

```bash
openstack security group list
```

#### Create Security Groups

```bash
openstack security group create {보안그룹_이름}
```

#### Add Security Group Rules

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

#### Delete Security Groups

```bash
openstack security group delete {보안그룹_ID}
```

## Storage Service Management

### Block Storage Management

#### Retrieve Volume List

```bash
# 모든 볼륨 조회
openstack volume list

# 특정 상태의 볼륨 조회
openstack volume list --status available
```

#### Create Volumes

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

#### Attach/Detach Volumes

```bash
# 인스턴스에 볼륨 연결
openstack server add volume {인스턴스_ID} {볼륨_ID}

# 인스턴스에서 볼륨 분리
openstack server remove volume {인스턴스_ID} {볼륨_ID}
```

#### Delete Volumes

```bash
openstack volume delete {볼륨_ID}
```

### Snapshot Management

#### Retrieve Snapshot List

```bash
openstack volume snapshot list
```

#### Create Snapshots

```bash
openstack volume snapshot create \
  --volume {볼륨_ID} \
  {스냅샷_이름}
```

#### Create Volumes from Snapshots

```bash
openstack volume create \
  --snapshot {스냅샷_ID} \
  --size {크기_GB} \
  {볼륨_이름}
```

#### Delete Snapshots

```bash
openstack volume snapshot delete {스냅샷_ID}
```

## Advanced Usage

### Batch Operations

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

### Customizing Output Format

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

### Script Examples

#### Instance Backup Script

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

## Troubleshooting

### Common Errors

#### Authentication Errors

```bash
# 토큰 만료 시
openstack token issue

# 잘못된 인증 정보 시
# clouds.yaml 또는 환경변수 재확인
```

#### Permission Errors

```bash
# 프로젝트 권한 확인
openstack project list

# 사용자 권한 확인
openstack user show {사용자_ID}
```

### Debugging

```bash
# 상세 로그 출력
openstack --debug server list

# API 호출 정보 출력
openstack --timing server list

# 버전 정보 확인
openstack --version
```

## Practical Usage Examples

### Building Web Server Infrastructure

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

### Database Server Backup

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

### Automation Script Examples

#### Resource Cleanup Script

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

## Monitoring and Logging

### Resource Usage Monitoring

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

### Log Analysis

```bash
# 인스턴스 콘솔 로그 확인
openstack console log show {인스턴스_ID}

# 상세 디버그 정보 출력
openstack --debug server show {인스턴스_ID}

# API 호출 타이밍 정보
openstack --timing server list
```

## Performance Optimization

### Batch Operation Optimization

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

### Output Optimization

```bash
# JSON 형식으로 출력하여 파싱 용이
openstack server list -f json | jq '.[] | {id: .ID, name: .Name, status: .Status}'

# CSV 형식으로 스프레드시트 분석
openstack volume list -f csv > volumes.csv

# 특정 컬럼만 출력하여 성능 향상
openstack server list -c ID -c Name -c Status
```

## References

- [OpenStack CLI Official Documentation](https://docs.openstack.org/python-openstackclient/ussuri/)
- [NHN Cloud Console Guide](https://docs.nhncloud.com/en/nhncloud/ko/overview/)
- [NHN Cloud API Guide](https://docs.nhncloud.com/en/nhncloud/ko/public-api/)
- [OpenStack Ussuri Release Notes](https://docs.openstack.org/releasenotes/python-openstackclient/ussuri.html)

## Support

If you encounter issues while using the CLI, please check the following:

1. Verify that the OpenStack CLI version is compatible with NHN Cloud
2. Verify that the authentication information is correct
3. Check the network connection status
4. Contact the NHN Cloud technical support team

### Common Troubleshooting

| Issue | Solution |
|------|-----------|
| Authentication failure | Reissue token with `openstack token issue` |
| Permission error | Check project permissions and user roles |
| Network connection failure | Check firewall settings and DNS |
| Resource creation failure | Check quotas and region settings |

### Useful Command Collection

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