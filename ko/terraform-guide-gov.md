## 서드파티 사용 가이드 > Terraform 사용 가이드
이 문서는 Terraform으로 NHN Cloud를 사용하는 방법을 설명합니다.

## Terraform
Terraform은 인프라를 손쉽게 구축하고 안전하게 변경하고, 효율적으로 인프라의 형상을 관리할 수 있는 오픈 소스 도구입니다. Terraform의 주요 특징은 다음과 같습니다.

* **Infrastructure as Code**
    * 인프라를 코드로 정의하여 생산성과 투명성을 높일 수 있습니다.
    * 정의한 코드를 쉽게 공유할 수 있어 효율적으로 협업할 수 있습니다.
* **Execution Plan**
    * 변경 계획과 변경 적용을 분리하여 변경 내용을 적용할 때 발생할 수 있는 실수를 줄일 수 있습니다.
* **Resource Graph**
    * 사소한 변경이 인프라 전체에 어떤 영향을 미칠지 미리 확인할 수 있습니다.
    * 종속성 그래프를 작성하여 이 그래프를 바탕으로 계획을 세우고, 이 계획을 적용했을 때 변경되는 인프라 상태를 확인할 수 있습니다.
* **Change Automation**
    * 여러 장소에 같은 구성의 인프라를 구축하고 변경할 수 있도록 자동화할 수 있습니다.
    * 인프라를 구축하는 데 드는 시간을 절약할 수 있고, 실수도 줄일 수 있습니다.


#### Resources 지원

* Compute
    * nhncloud_compute_instance_v2
    * nhncloud_compute_volume_attach_v2
    * nhncloud_compute_keypair_v2
* Network
    * nhncloud_lb_loadbalancer_v2
    * nhncloud_lb_listener_v2
    * nhncloud_lb_pool_v2
    * nhncloud_lb_member_v2
    * nhncloud_lb_monitor_v2
    * nhncloud_networking_floatingip_v2
    * nhncloud_networking_floatingip_associate_v2
    * nhncloud_networking_port_v2
    * nhncloud_networking_vpc_v2
    * nhncloud_networking_vpcsubnet_v2
    * nhncloud_networking_routingtable_v2
    * nhncloud_networking_routingtable_attach_gateway_v2
    * nhncloud_networking_secgroup_v2
    * nhncloud_networking_secgroup_rule_v2
    * nhncloud_keymanager_secret_v1
    * nhncloud_keymanager_container_v1
* Block Storage
    * nhncloud_blockstorage_volume_v2
* Object Storage
    * nhncloud_objectstorage_container_v1
    * nhncloud_objectstorage_object_v1

#### Data sources 지원

* nhncloud_images_image_v2
* nhncloud_blockstorage_volume_v2
* nhncloud_compute_flavor_v2
* nhncloud_compute_keypair_v2
* nhncloud_blockstorage_snapshot_v2
* nhncloud_networking_vpc_v2
* nhncloud_networking_vpcsubnet_v2
* nhncloud_networking_routingtable_v2
* nhncloud_networking_secgroup_v2
* nhncloud_keymanager_secret_v1
* nhncloud_keymanager_container_v1


### 알아두기

* **아래 예시에 사용된 Terraform 버전은 1.0.0입니다.**
* **버전을 포함한 구성요소의 이름과 숫자는 변경될 수 있으니, 확인 후 사용하시기 바랍니다.**


## Terraform 설치
[Terraform 다운로드 페이지](https://www.terraform.io/downloads.html)에서 로컬 PC의 운영체제에 맞는 파일을 다운로드합니다. 파일의 압축을 해제하고 원하는 경로에 넣은 다음 환경 설정에 해당 경로를 추가하면 설치가 완료됩니다.

다음은 설치 예시입니다.

```
$ wget https://releases.hashicorp.com/terraform/1.0.0/terraform_1.0.0_linux_amd64.zip
$ unzip terraform_1.0.0_linux_amd64.zip
$ export PATH="${PATH}:$(pwd)"
$ terraform -v
Terraform v1.0.0
```


## Terraform provider 제공

NHN Cloud는 HashiCorp사의 공식 파트너로서 [Terraform Registry](https://registry.terraform.io/providers/nhn-cloud/nhncloud/latest)를 통해 Terraform provider를 제공합니다.


## Terraform 초기화
Terraform을 사용하기 전에 다음과 같이 공급자 설정 파일을 생성합니다.

공급자 파일 이름은 임의로 설정 가능하며, 이 예제에서는 `provider.tf`를 사용합니다.

```
# Define required providers
terraform {
required_version = ">= 1.0.0"
  required_providers {
    nhncloud = {
      source  = "nhn-cloud/nhncloud"
      version = "1.0.2"
    }
  }
}

# Configure the nhncloud Provider
provider "nhncloud" {
  user_name   = "terraform-guide@nhncloud.com"
  tenant_id   = "aaa4c0a12fd84edeb68965d320d17129"
  password    = "difficultpassword"
  auth_url    = "https://api-identity-infrastructure.gov-nhncloudservice.com/v2.0"
  region      = "KR1"
}
```
* **user_name**
    * NHN Cloud ID를 사용합니다.
* **tenant_id**
    * NHN Cloud 콘솔의 **Compute > Instance > 관리** 메뉴에서 **API 엔드포인트 설정** 버튼을 클릭해 테넌트 ID를 확인합니다.
* **password**
    * **API Endpoint 설정** 대화 상자에서 저장한 **API 비밀번호**를 사용합니다.
    * API 비밀번호 설정 방법은 **사용자 가이드 > Compute > Instance > API 사용 준비**를 참고합니다.
* **auth_url**
    * NHN Cloud 신원 서비스 주소를 명시합니다.
    * NHN Cloud 콘솔의 **Compute > Instance > 관리** 메뉴에서 **API 엔드포인트 설정** 버튼을 클릭해 신원 서비스(identity) URL을 확인합니다.
* **region**
    * NHN Cloud 리소스를 관리할 리전 정보를 입력합니다.
    * **KR1**: 한국(판교) 리전

공급자 설정 파일이 있는 경로에서 `init` 명령을 이용해 Terraform을 초기화합니다.

```
$ ls
provider.tf
$ terraform init
```


## Terraform 기본 사용법

Terraform을 이용한 인프라 구축은 보통 아래와 같은 수명 주기(라이프 사이클)를 가집니다.

1. tf 파일 작성
2. 구축 계획 확인
3. 리소스 생성
4. 리소스 수정
5. 리소스 삭제

먼저 구축할 인프라 형상을 tf 파일에 작성합니다. 작성된 tf 파일에 따른 구축 계획은 아래와 같이 `plan` 명령으로 확인합니다.

```
$ terraform plan
```

구축 계획이 문제가 없다면 `apply` 명령을 이용하여 리소스를 생성, 수정, 삭제합니다.

```
$ terraform apply
```

다음 섹션에서는 이 단계들을 예제와 함께 더 자세히 설명합니다.


### tf 파일 작성

공급자 설정 파일이 있는 경로에 tf 파일을 작성합니다. 여러 리소스 설정을 하나의 tf 파일에 모아두거나, 리소스별로 별도의 tf 파일로도 작성 가능합니다. Terraform은 작성된 전체 tf 파일을 한번에 읽어서 구축 계획을 수립합니다.

아래는 `instance.tf` 파일에 인스턴스를 생성하는 리소스를 정의한 tf 파일 예제입니다.

```
$ ls
instance.tf provider.tf
$ cat instance.tf
resource "nhncloud_compute_instance_v2" "terraform-instance-01" {
  name      = "terraform-instance-01"
  region    = "KR1"
  flavor_id = "da74152c-0167-4ce9-b391-8a88a8ff2754"
  key_pair  = "terraform-keypair"
  network {
    uuid = "00d5b852-cb77-4307-b6be-d81dad24eec1"
  }
  security_groups = ["default"]
  block_device {
    uuid = "6d0993b4-cd6d-4242-b59b-94258f265331"
    source_type = "image"
    destination_type = "volume"
    boot_index = 0
    volume_size = 20
    delete_on_termination = true
  }
}
```

### 구축 계획 확인

tf 파일에서 변경될 리소스를 `plan` 명령으로 확인할 수 있습니다. `plan` 명령을 실행하면 Terraform이 .tf 파일들을 로드해 설정이 올바른지 확인하고 자체 DB와 비교하여 플랜을 생성합니다. 플랜 생성을 완료하면 플랜을 유형별로 집계하여 보기 좋게 출력합니다.

```
$ terraform plan
```

생성된 플랜이 잘못되었다면 tf 파일을 수정하고 다시 반복하여 `plan` 명령을 실행합니다. `plan` 명령은 실제 NHN Cloud 리소스를 변경하지 않으므로 인프라 변경 사항을 부담없이 확인할 수 있습니다.


### 리소스 생성하기

원하는 플랜으로 tf 파일을 작성한 후에, `apply` 명령으로 리소스를 생성합니다.

아래는 위에서 작성한 `instance.tf` 파일을 이용해 `apply` 명령을 수행한 결과 예제입니다.

```
$ terraform apply
...
nhncloud_compute_instance_v2.terraform-instance-01: Creating...
nhncloud_compute_instance_v2.terraform-instance-01: Still creating... [10s elapsed]
...
nhncloud_compute_instance_v2.terraform-instance-01: Still creating... [50s elapsed]
nhncloud_compute_instance_v2.terraform-instance-01: Creation complete after 53s [id=8a8c5516-6762-4592-97ab-db8d3af629e6]

Apply complete! Resources: 1 added, 0 changed, 0 destroyed.
...
```

`apply` 명령이 실행하면 플랜 변경 이력을 기록하는 자체 DB 파일(terraform.tfstate)이 현재 디렉터리에 생성됩니다. 이 파일을 삭제하지 않도록 주의합니다.


### 리소스 수정하기

변경할 리소스가 정의된 `.tf` 파일을 열어 원하는 정보를 수정하고 플랜을 적용합니다. 변경할 수 있는 사양은 일부 속성으로 제한됩니다. 만약 변경할 수 없는 속성을 수정하면 해당 리소스는 삭제 후 새롭게 다시 생성됩니다.

다음은 인스턴스에 `terraform-sg` 보안 그룹을 하나 더 추가하는 예시입니다. 위에서 작성한 `instance.tf` 파일을 아래와 같이 수정합니다.

```
resource "nhncloud_compute_instance_v2" "terraform-instance-01" {
  ...
  security_groups = ["default", "terraform-sg"]
  ...
}
```

구축 계획을 확인하면 변경된 보안 그룹 정보를 정리하여 출력합니다.

```
$ terraform plan
...
Terraform used the selected providers to generate the following execution plan. Resource actions are indicated with the following symbols:
  ~ update in-place

Terraform will perform the following actions:

  # nhncloud_compute_instance_v2.terraform-instance-01 will be updated in-place
  ~ resource "nhncloud_compute_instance_v2" "terraform-instance-01" {
        id                  = "8a8c5516-6762-4592-97ab-db8d3af629e6"
        name                = "terraform-instance-01"
      ~ security_groups     = [
          + "terraform-sg",
            # (1 unchanged element hidden)
        ]
        # (13 unchanged attributes hidden)


        # (2 unchanged blocks hidden)
    }

Plan: 0 to add, 1 to change, 0 to destroy.
```

플랜을 적용하면 인스턴스에 새로운 보안 그룹이 추가됩니다.
```
$ terraform apply
...
nhncloud_compute_instance_v2.terraform-instance-01: Modifying... [id=8a8c5516-6762-4592-97ab-db8d3af629e6]
nhncloud_compute_instance_v2.terraform-instance-01: Modifications complete after 5s [id=8a8c5516-6762-4592-97ab-db8d3af629e6]

Apply complete! Resources: 0 added, 1 changed, 0 destroyed.
```


### 리소스 삭제하기

Terraform으로 생성한 리소스를 지우기 위해 해당하는 `.tf` 파일을 삭제합니다.

```
$ rm instance.tf
```

구축 계획에서 해당 리소스가 삭제될 것을 확인할 수 있습니다.

```
$ terraform plan
...
Terraform used the selected providers to generate the following execution plan. Resource actions are indicated with the following symbols:
  - destroy

Terraform will perform the following actions:

  # nhncloud_compute_instance_v2.terraform-instance-01 will be destroyed
  - resource "nhncloud_compute_instance_v2" "terraform-instance-01" {
...

Plan: 0 to add, 0 to change, 1 to destroy.
...
```

`apply` 명령으로 생성된 instance가 삭제됩니다.

```
$ terraform apply
...
nhncloud_compute_instance_v2.terraform-instance-01: Destroying... [id=8a8c5516-6762-4592-97ab-db8d3af629e6]
nhncloud_compute_instance_v2.terraform-instance-01: Still destroying... [id=8a8c5516-6762-4592-97ab-db8d3af629e6, 10s elapsed]
nhncloud_compute_instance_v2.terraform-instance-01: Destruction complete after 11s

Apply complete! Resources: 0 added, 0 changed, 1 destroyed.
```


## Data sources

tf 파일 작성에 필요한 인스턴스 타입 ID, 이미지 ID 등은 콘솔에서 확인하거나, Terraform이 제공하는 data sources를 이용하여 가져올 수 있습니다. Data sources는 tf 파일 안에 작성하며, 가져온 정보는 수정할 수 없고 오직 참조만 가능합니다. NHN Cloud는 주기적으로 이미지를 업데이트하므로 이미지 이름이 변경될 수 있습니다. 사용하고자 하는 정확한 이미지 이름은 콘솔을 참조하여 명시합니다.

Data sources는 `{data sources 자원 유형}.{data source 이름}`으로 참조합니다. 아래 예제에서는 `nhncloud_images_image_v2.ubuntu_1804_20200218`로 가져온 이미지 정보를 참조합니다.

```
data "nhncloud_images_image_v2" "ubuntu_1804_20200218" {
  name = "Ubuntu Server 18.04.3 LTS (2020.02.18)"
  most_recent = true
}
```

Data sources 안에서 다른 data source를 참조할 수 있습니다.

```
data "nhncloud_blockstorage_volume_v2" "volume_00"{
  name = "ssd_volume1"
  status = "available"
}

data "nhncloud_blockstorage_snapshot_v2" "my_snapshot" {
  name = "my-snapshot"
  volume_id = data.nhncloud_blockstorage_volume_v2.volume_00.id
  status = "available"
  most_recent = true
}
```

다음 섹션에서는 NHN Cloud가 제공하는 각종 리소스를 data sources 기능으로 가져오는 방법을 설명합니다.


### 이미지

이미지 정보를 가져옵니다. NHN Cloud가 제공하는 공용 이미지 또는 개인 이미지를 지원합니다.

```
data "nhncloud_images_image_v2" "ubuntu_1804_20200218" {
  name = "Ubuntu Server 18.04.3 LTS (2020.02.18)"
  most_recent = true
}

# 같은 이름의 이미지 중 가장 오래된 이미지 조회
data "nhncloud_images_image_v2" "windows2016_20200218" {
  name = "Windows 2016 STD with MS-SQL 2016 Standard (2020.02.18) KO"
  sort_key = "created_at"
  sort_direction = "asc"
  owner = "c289b99209ca4e189095cdecebbd092d"
  tag = "_AVAILABLE_"
}
```
| 이름    | 형식 | 필수  | 설명       |
| ------ | ---- | ---- | --------- |
| name | String | - | 조회할 이미지 이름<br>이미지 이름은 NHN Cloud 콘솔 **Compute > Instance에서 인스턴스 생성 버튼**을 클릭하면 NHN Cloud에서 제공하는 이미지 목록에서 확인합니다.<br>이미지 이름은 NHN Cloud 콘솔에 표시된 **<이미지 설명>**으로 작성해야 함<br>만약 언어 항목이 존재한다면 위의 예시처럼 **"<이미지 설명> <언어>"** 형식으로 입력해야 함 |
| size_min | Integer | - | 조회할 이미지의 최소 크기(바이트) |
| size_max | Integer | - | 조회할 이미지의 최대 크기(바이트) |
| properties | Object | - | 조회할 이미지의 속성<br>모든 속성 값이 일치하는 이미지 조회 |
| sort_key | String | - | 특정 속성을 기준으로 조회한 이미지 목록 정렬<br>기본값은 `name` |
| sort_direction | String | - | 조회한 이미지 목록 정렬 방향 <br>`asc`: 오름차순(기본값) <br>`desc`: 내림차순 |
| owner | String | - | 조회할 이미지가 속한 테넌트 ID |
| tag | String | - | 특정 태그가 존재하는 이미지 검색 |
| visibility | String | - | 조회할 이미지의 보여주기 속성<br>public, private, shared 중 하나의 값만 선택 가능<br>생략하면 모든 종류의 이미지 목록을 반환 |
| most_recent | Boolean | - | `true`: 조회한 이미지 목록 중 가장 최근에 생성된 이미지 선택<br>`false`: 조회된 순서로 이미지 선택 |
| member_status | String | - | 조회할 이미지 멤버 상태 <br>`accepted`,`pending`,`rejected`,`all` 중 하나|


### 블록 스토리지

```
data "nhncloud_blockstorage_volume_v2" "volume_00" {
  name = "ssd_volume1"
  status = "available"
}
```

| 이름    | 형식 | 필수  | 설명       |
| ------ | ---- | ---- | --------- |
| name | String | - | 조회할 블록 스토리지 이름 |
| status | String | - | 조회할 블록 스토리지 상태 |
| metadata | Object | - | 조회할 블록 스토리지와 관련된 메타데이터 |


### 인스턴스 타입

인스턴스 타입 이름은 NHN Cloud 콘솔 **Compute > Instance**에서 **인스턴스 생성 > 인스턴스 타입 선택** 버튼을 클릭해 확인할 수 있습니다.

```
data "nhncloud_compute_flavor_v2" "m2c2m4"{
  name = "m2.c2m4"
}
```

| 이름    | 형식 | 필수  | 설명       |
| ------ | ---- | ---- | --------- |
| name | String | - | 조회할 인스턴스 타입 이름 |


### 키페어

```
data "nhncloud_compute_keypair_v2" "my_keypair"{
  name = "my_keypair"
}
```

| 이름    | 형식 | 필수 | 설명         |
| ------ | ---- |----|------------|
| name | String | O  | 조회할 키페어 이름 |


### 스냅숏

```
data "nhncloud_blockstorage_snapshot_v2" "my_snapshot" {
  name = "my-snapshot"
  volume_id = data.nhncloud_blockstorage_volume_v2.volume_00.id
  status = "available"
  most_recent = true
}
```

| 이름    | 형식 | 필수  | 설명       |
| ------ | ---- | ---- | --------- |
| name | String | - | 조회할 스냅숏 이름 |
| volume_id | String | - | 조회할 스냅숏의 원본 블록 스토리지 ID |
| status | String | - | 조회할 스냅숏의 상태 |
| most_recent | Boolean | - | `true`: 조회한 스냅숏 목록 중 가장 최근에 만들어진 스냅숏 선택<br>`false`: 조회된 순서로 스냅숏 선택 |


### VPC

VPC 네트워크의 UUID는 NHN Cloud 콘솔 **Network > VPC**에서 VPC를 선택하여 확인 가능합니다.

```
data "nhncloud_networking_vpc_v2" "default_network" {
  region = "KR1"
  tenant_id = "ba3be1254ab141bcaef674e74630a31f"
  id = "e34fc878-89f6-4d17-a039-3830a0b78346"
  name = "Default Network"
}
```

| 이름 | 타입 | 필수 | 설명         |
| --- | --- |---|------------|
| region | String | - | 조회할 VPC가 속한 리전 이름 |
| tenant\_id | String | - | 조회할 VPC가 속한 테넌트 ID |
| id | String | - | 조회할 VPC의 ID |
| name | String | - | 조회할 VPC 이름 |


### VPC 서브넷

서브넷 ID는 NHN Cloud 콘솔 **Network > 서브넷**에서 서브넷을 선택하여 확인 가능합니다.

```
data "nhncloud_networking_vpcsubnet_v2" "default_subnet" {
  region = "KR1"
  tenant_id = "ba3be1254ab141bcaef674e74630a31f"
  id = "05f6fdc3-641f-48df-b986-773b6489654f"
  name = "Default Network"
  shared = true
}
```

| 이름 | 타입 | 필수 | 설명         |
| --- | --- |---|------------|
| region | String | - | 조회할 서브넷이 속한 리전 이름 |
| tenant\_id | String | - | 조회할 서브넷이 속한 테넌트 ID |
| id | String | - | 조회할 서브넷 ID |
| name | String | - | 조회할 서브넷 이름 |
| shared | Bool | - | 조회할 서브넷의 공유 여부 |


### 라우팅 테이블
```
data "nhncloud_networking_routingtable_v2" "default_rt" {
  id = "bf15f6f6-1339-4057-a7fe-5811d39bab18"
}
```

| 이름 | 타입 | 필수 | 설명                  |
| --- | --- |---|---------------------|
| tenant\_id | String | - | 조회할 라우팅 테이블이 속한 테넌트 ID |
| id | String | - | 조회할 라우팅 테이블 ID      |
| name | String | - | 조회할 라우팅 테이블 이름   |


### 보안 그룹
```
data "nhncloud_networking_secgroup_v2" "default_sg" {
  name = "default"
}
```

| 이름 | 타입 | 필수 | 설명                 |
| --- | --- |---|--------------------|
| region | String | - | 조회할 보안 그룹이 속한 리전 이름 |
| tenant\_id | String | - | 조회할 보안 그룹이 속한 테넌트 ID |
| name | String | - | 조회할 보안 그룹 이름       |


### 시크릿
```
data "nhncloud_keymanager_secret_v1" "secret_01" {
  name      = "terraform_secret_01"
}
```

| 이름 | 타입 | 필수 | 설명               |
| --- | --- |---|------------------|
| region | String | - | 조회할 시크릿이 속한 리전 이름 |
| name | String | - | 조회할 시크릿 이름       |


### 시크릿 컨테이너
```
data "nhncloud_keymanager_container_v1" "container_01" {
  name      = "terraform_container_01"
}
```

| 이름 | 타입 | 필수 | 설명                      |
| --- | --- |---|-------------------------|
| region | String | - | 조회할 시크릿 컨테이너가 속한 리전 이름  |
| name | String | - | 조회할 시크릿 컨테이너 이름         |


## Resources

Terraform resources를 통해 리소스를 생성, 수정, 삭제할 수 있습니다. NHN Cloud에서는 Terraform을 통해 다음 리소스 관리를 지원합니다.

* 인스턴스
* 블록 스토리지
* 오브젝트 스토리지
* VPC
* 플로팅 IP
* 네트워크 포트
* 로드 밸런서
* 보안 그룹

다음 섹션에는 각 리소스를 사용하는 방법을 설명합니다.

### 알아두기

* 오브젝트 스토리지 리소스 사용법은 [사용자 가이드 > Storage > Object Storage > 서드 파티 도구 사용 가이드](https://docs.gov-nhncloud.com/ko/Storage/Object%20Storage/ko/third-party-tools-guide/)를 참고하십시오.

## Resources - 인스턴스

### 인스턴스 생성

```
# 네트워크 추가, 블록 스토리지 추가된 인스턴스 생성
resource "nhncloud_compute_instance_v2" "tf_instance_02" {
  name      = "tf_instance_02"
  region    = "KR1"
  key_pair  = "terraform-keypair"
  flavor_id = data.nhncloud_compute_flavor_v2.m2c1m2.id
  security_groups = ["default","web"]

  network {
    name = data.nhncloud_networking_vpc_v2.default_network.name
    uuid = data.nhncloud_networking_vpc_v2.default_network.id
  }

  network {
    port = nhncloud_networking_port_v2.port_1.id
  }

  block_device {
    uuid                  = data.nhncloud_images_image_v2.centos_610_20200218.id
    source_type           = "image"
    destination_type      = "volume"
    boot_index            = 0
    volume_size           = 20
    delete_on_termination = true
  }

  block_device {
    source_type           = "blank"
    destination_type      = "volume"
    boot_index            = 1
    volume_size           = 20
    delete_on_termination = true
  }
}
```

| 이름                                          | 형식      | 필수 | 설명                                                                                                                                                                                           |
|---------------------------------------------|---------|----|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| name                                        | String  | O  | 생성할 인스턴스의 이름                                                                                                                                                                                 |
| region                                      | String  | -  | 생성할 인스턴스의 리전<br>기본값은 provider.tf에 설정된 리전                                                                                                                                                     |
| flavor_name                                 | String  | -  | 생성할 인스턴스의 인스턴스 타입 이름<br>flavor_id가 비어 있을 때 필수                                                                                                                                                |
| flavor_id                                   | String  | -  | 생성할 인스턴스의 인스턴스 타입 ID<br>flavor_name이 비어 있을 때 필수                                                                                                                                              |
| key_pair                                    | String  | -  | 인스턴스 접속에 사용할 키페어 이름<br>키페어는 NHN Cloud 콘솔의 **Compute > Instance > Key Pair** 메뉴에서 새로 생성하거나,<br>이미 가지고 있는 키페어를 등록하여 사용<br>생성, 등록 방법은 `사용자 가이드 > Compute > Instance > 콘솔 사용 가이드`를 참고            |
| availability_zone                           | String  | -  | 인스턴스를 생성할 가용성 영역<br>지정하지 않을 경우 임의로 선택됨<br>루트 블록 스토리지의 소스 타입이 `volume`, `snapshot`인 경우 원본 블록 스토리지의 가용성 영역과 동일하게 설정 필요 |
| network                                     | Object  | -  | 생성할 인스턴스에 연결할 VPC 네트워크 정보.<br>콘솔의 **Network > VPC > Management** 메뉴에서 연결할 VPC를 선택하면 하단 상세 정보 화면에서 네트워크 이름과 UUID를 확인 가능                                                                       |
| network.name                                | String  | -  | VPC 네트워크 이름 <br>network.name, network.uuid, network.port 중 하나는 반드시 명시                                                                                                                        |
| network.uuid                                | String  | -  | VPC 네트워크 ID                                                                                                                                                                                  |
| network.port                                | String  | -  | VPC 네트워크에 연결할 포트의 ID<br>포트 ID 지정 시 요청한 보안 그룹은 지정한 기존 포트에 적용되지 않음                                                                                                                                                                         |
| security_groups                             | Array   | -  | 인스턴스에서 사용할 보안 그룹의 이름 목록 <br>콘솔의 **Network > VPC > Security Groups** 메뉴에서 사용할 보안 그룹을 선택하면, 하단 상세 정보 화면에서 정보 확인 가능                                                                             |
| user_data                                   | String  | -  | 인스턴스 부팅 후 실행할 스크립트 및 설정<br>base64 인코딩된 문자열로 65535 바이트까지 허용<br>                                                                                                                              |
| block_device                                | Object  | O  | 인스턴스의 블록 스토리지 정보 객체 |
| block_device.source_type                    | String  | O  | 생성할 블록 스토리지 원본의 타입<br>- `image`: 이미지를 이용해 블록 스토리지 생성<br>- `blank`: 빈 블록 스토리지 생성(루트 블록 스토리지로 사용할 수 없음)<br>- `volume`: 기존에 생성된 블록 스토리지를 사용<br>- `snapshot`: 스냅숏을 이용해 블록 스토리지 생성 |
| block_device.uuid                           | String  | -  | 블록 스토리지의 소스 타입에 따라 다르게 설정 필요<br>- 소스 타입이 `image`인 경우 블록 스토리지의 원본 이미지 ID 를 설정<br>- 소스 타입이 `volume`인 경우 블록 스토리지 ID 를 설정<br>- 소스 타입이 `snapshot`인 경우 스냅숏 ID 를 설정<br>루트 블록 스토리지인 경우 반드시 부팅 가능한 원본이어야 함 |
| block_device.boot_index                     | Integer | O  | 지정한 블록 스토리지의 부팅 순서<br>- `0`이면 루트 블록 스토리지<br>- 그 외는 추가 블록 스토리지<br>크기가 클수록 부팅 순서는 낮아짐<br>                                                                                                            |
| block_device.destination_type               | String  | O  | 인스턴스 블록 스토리지의 위치<br>`volume`만 지원                                                                                                                                                |
| block_device.volume_size                    | Integer | -  | 생성할 블록 스토리지 크기<br>블록 스토리지의 소스 타입에 따라 다르게 설정 필요<br>- 소스 타입이 `volume`인 경우 설정 불필요<br>- 소스 타입이 `snapshot`인 경우 원본 블록 스토리지 크기 보다 같거나 크게 설정<br>`GB` 단위<br>인스턴스 타입에 따라 생성할 수 있는 루트 블록 스토리지의 크기가 다르므로 자세한 내용은 `사용자 가이드 > Compute > Instance > 콘솔 사용 가이드 > 인스턴스 생성 > 블록 스토리지 크기`를 참고 |
| block_device.volume_type               | Enum    | -  | 생성할 블록 스토리지의 타입<br>블록 스토리지의 소스 타입이 `volume`, `snapshot`인 경우 설정 불필요<br>`사용자 가이드 > Storage > Block Storage > API v2 가이드`에서 **블록 스토리지 타입 목록 보기** 응답의 `name` 참고 |
| block_device.delete_on_termination          | Boolean | -  | `true`: 인스턴스 삭제 시 블록 디바이스도 함께 삭제<br>`false`: 인스턴스 삭제 시 블록 디바이스는 함께 삭제하지 않음                                                                                                                   |
| block_device.nhn_encryption                 | Object  | -  | 블록 스토리지 암호화 정보                                                                                                                                                                               |
| block_device.nhn_encryption.skm_appkey      | String  | O  | Secure Key Manager 상품의 앱키                                                                                                                                                                    |
| block_device.nhn_encryption.skm_key_id      | String  | O  | Secure Key Manager의 키 ID                                                                                                                                                                     |


### 블록 스토리지 연결
```
# 인스턴스 생성
resource "nhncloud_compute_instance_v2" "tf_instance_01" {
  ...
}

# 블록 스토리지 생성
resource "nhncloud_blockstorage_volume_v2" "volume_01" {
  ...
}

# 블록 스토리지 연결
resource "nhncloud_compute_volume_attach_v2" "volume_to_instance"{
  instance_id = nhncloud_compute_instance_v2.tf_instance_02.id
  volume_id = nhncloud_blockstorage_volume_v2.volume_01.id
}
```
| 이름    | 타입 | 필수 | 설명       |
| ------ | --- |----| --------- |
| instance_id | String | O  | 블록 스토리지를 연결할 대상 인스턴스 |
| volume_id | String | O  | 연결할 블록 스토리지 UUID |


### 키페어
```
resource "nhncloud_compute_keypair_v2" "tf_kp_01" {
  name = "tf_kp_01"
}

# public_key 지정
resource "nhncloud_compute_keypair_v2" "tf_kp_02" {
  name = "tf_kp_02"
  public_key = "ssh-rsa ... Generated-by-Nova"
}
```

| 이름        | 타입 | 필수 | 설명                             |
|-----------| --- |----|--------------------------------|
| name      | String | O  | 생성할 키페어 이름                     |
| public_key | String | -  | 등록할 공개키<br>생략하면 새로운 공개키를 생성 |


## Resources - 블록 스토리지

### 블록 스토리지 생성
```
# HDD 타입의 빈 블록 스토리지 생성
resource "nhncloud_blockstorage_volume_v2" "volume_01" {
  name = "tf_volume_01"
  size = 10
  availability_zone = "kr-pub-a"
  volume_type = "General HDD"
}

# SSD 타입의 빈 블록 스토리지 생성
resource "nhncloud_blockstorage_volume_v2" "volume_02" {
  name = "tf_volume_02"
  size = 10
  availability_zone = "kr-pub-b"
  volume_type = "General SSD"
}

# 스냅숏으로 블록 스토리지 생성
resource "nhncloud_blockstorage_volume_v2" "volume_03" {
  name = "tf_volume_03"
  description = "terraform create volume with snapshot test"
  snapshot_id = data.nhncloud_blockstorage_snapshot_v2.snapshot_01.id
  size = 30
}
```

| 이름                | 타입      | 필수 | 설명                                                                                                                                                       |
|-------------------|---------|---|----------------------------------------------------------------------------------------------------------------------------------------------------------|
| name              | String  | - | 생성할 블록 스토리지 이름                                                                                                                                           |
| description       | String  | - | 블록 스토리지 설명                                                                                                                                               |
| size              | Integer | O | 생성할 블록 스토리지 크기(GB)                                                                                                                                       |
| availability_zone | String  | - | 생성할 블록 스토리지의 가용성 영역, 값이 존재하지 않을 경우 임의의 가용성 영역<br>availability_zone은 콘솔 `Storage > Block Storage > 관리`의 **블록 스토리지 생성** 버튼을 클릭하면 표시되는 가용성 영역에서 확인할 수 있습니다. |
| volume_type       | Enum    | -  | 블록 스토리지 타입<br>`사용자 가이드 > Storage > Block Storage > API v2 가이드` 의 **블록 스토리지 타입 목록 보기** 응답의 `name` 참고                                                       |
| snapshot_id       | String  | - | 원본 스냅숏 ID, 생략하면 빈 블록 스토리지가 생성됨                                                                                                                           |
| nhn_encryption                 | Object  | -  | 블록 스토리지 암호화 정보                                                                                                                                           |
| nhn_encryption.skm_appkey      | String  | O  | Secure Key Manager 상품의 앱키                                                                                                                                      |
| nhn_encryption.skm_key_id      | String  | O  | Secure Key Manager의 키 ID                                                                                                                                       |


### 블록 스토리지 불러오기

콘솔 또는 API를 통해 생성한 블록 스토리지를 Terraform으로 불러와 관리할 수 있습니다.

`.tf` 파일에 불러올 블록 스토리지 정보를 작성합니다.
```
resource "nhncloud_blockstorage_volume_v2" "volume_06" {
  name = "volume_06"
  size = 10
}
```

`terraform import nhncloud_blockstorage_volume_v2.{name} {block_storage_id}` 명령으로 블록 스토리지를 불러옵니다.

```
$ terraform import nhncloud_blockstorage_volume_v2.volume_06 10cf5bec-cebb-479b-8408-3ffe3b569a7a
...
nhncloud_blockstorage_volume_v2.volume_06: Importing from ID "10cf5bec-cebb-479b-8408-3ffe3b569a7a"...
nhncloud_blockstorage_volume_v2.volume_06: Import prepared!
  Prepared nhncloud_blockstorage_volume_v2 for import
nhncloud_blockstorage_volume_v2.volume_06: Refreshing state... [id=10cf5bec-cebb-479b-8408-3ffe3b569a7a]

Import successful!
...
```


## Resources - VPC

NHN Cloud는 Terraform으로 아래 자원에 대한 생성을 지원합니다.

* VPC
* VPC 서브넷
* 네트워크 포트
* 플로팅 IP
* 라우팅 테이블

이외의 VPC 자원은 콘솔에서 생성해야 합니다.


### VPC 생성

지정한 IP 대역의 VPC를 생성합니다.

```
resource "nhncloud_networking_vpc_v2" "resource-vpc-01" {
  name = "tf-vpc-01"
  cidrv4 = "10.0.0.0/8"
}
```

| 이름 | 타입 | 필수 | 설명     |
| --- | --- |---|--------|
| name | String | O | VPC 이름 |
| cidrv4 | String | O | VPC IP 대역 |
| region | String | - | VPC의 리전 이름 |
| tenant\_id | String | - | VPC의 tenant ID |


### VPC 서브넷 생성 및 라우팅 테이블 연결

지정한 VPC에 사용자가 지정한 IP 대역으로 서브넷을 생성하며, 생성한 서브넷에 기존 라우팅 테이블을 연결합니다.
라우팅 테이블은 NHN Cloud 콘솔에서 생성할 수 있습니다.

```
resource "nhncloud_networking_vpcsubnet_v2" "resource-vpcsubnet-01" {
  name      = "tf-vpcsubnet-01"
  vpc_id    = "def56b5e-0f1d-4a31-8005-4d716127f177"
  cidr      = "10.10.10.0/24"
  routingtable_id = "c3ed678d-de8b-4bf7-abea-b7c1118f0828"
}
```

| 이름 | 타입 | 필수 | 설명         |
| --- | --- |---|------------|
| vpc\_id | String | O | 서브넷이 할당될 VPC ID |
| cidr | String | O | 서브넷의 IP 대역 |
| name | String | O | 서브넷의 이름    |
| region | String | - | 서브넷이 할당될 리전 이름 |
| tenant\_id | String | - | 서브넷이 할당될 테넌트 ID |
| routingtable\_id | String | - | 라우팅 테이블 ID |


### 네트워크 포트 생성

```
resource "nhncloud_networking_port_v2" "port_1" {
  name = "tf_port_1"
  network_id = data.nhncloud_networking_vpc_v2.default_network.id
  admin_state_up = "true"
}
```

| 이름    | 형식 | 필수 | 설명       |
| ------ | ---- |----| --------- |
| name | String | O  | 생성할 포트의 이름 |
| description | String | -  | 포트 설명 |
| network_id | String | O  | 포트를 생성할 VPC 네트워크 ID |
| tenant_id | String | -  | 생성할 포트의 테넌트 ID |
| device_id | String | -  | 생성된 포트가 연결될 장치 ID |
| fixed_ip | Object | -  | 생성할 포트의 고정 IP 설정 정보<br>`no_fixed_ip` 속성이 없어야 함 |
| fixed_ip.subent_id | String | O  | 고정 IP의 서브넷 ID |
| fixed_ip.ip_address | String | -  | 설정할 고정 IP의 주소 |
| no_fixed_ip | Boolean | -  | `true`: 고정 IP가 없는 포트<br>`fixed_ip` 속성이 없어야 함 |
| admin_state_up | Boolean | -  | 관리자 제어 상태<br> `true`: 작동<br>`false`: 중지 |


### 플로팅 IP 생성

```
resource "nhncloud_networking_floatingip_v2" "fip_01" {
  pool = "Public Network"
}
```

| 이름    | 형식 | 필수  | 설명       |
| ------ | --- |---- | --------- |
| pool | String | O | 플로팅 IP를 생성할 IP 풀<br>기본값은 `Public Network` |


### 플로팅 IP 연결
```
# 네트워크 포트 생성
resource "nhncloud_networking_port_v2" "port_1" {
  ...
}

# 인스턴스 생성
resource "nhncloud_compute_instance_v2" "tf_instance_01" {
  ...
    network {
    port = nhncloud_networking_port_v2.port_1.id
  }
  ...
}

# 플로팅 IP 생성
resource "nhncloud_networking_floatingip_v2" "fip_01" {
  ...
}

# 플로팅 IP 연결
resource "nhncloud_networking_floatingip_associate_v2" "fip_associate" {
  floating_ip = nhncloud_networking_floatingip_v2.fip_01.address
  port_id = nhncloud_networking_port_v2.port_1.id
}

```

| 이름          | 형식 | 필수  | 설명                    |
|-------------| --- |---- |-------------------------|
| floating_ip | String | O | 연결할 플로팅 IP           |
| port_id     | String | O | 플로팅 IP를 연결할 포트 UUID |


### 라우팅 테이블 생성
```
resource "nhncloud_networking_vpc_v2" "resource-vpc-01" {
  ...
}

resource "nhncloud_networking_routingtable_v2" "resource-rt-01" {
  name = "resource-rt-01"
  vpc_id = nhncloud_networking_vpc_v2.resource-vpc-01.id
  distributed = false
}
```

| 이름     | 타입      | 필수 | 설명                                                             |
|--------|---------|----|----------------------------------------------------------------|
| name   | String  | O  | 라우팅 테이블 이름                                                     |
| vpc_id | String  | O  | 라우팅 테이블이 속할 VPC ID                                             |
| distributed   | Boolean | -  | 라우팅 테이블의 라우팅 방식 </br>`true`: 분산형, `false`: 중앙 집중형(기본값: `true`) |

### 라우팅 테이블에 인터넷 게이트웨이 연결하기

라우팅 테이블에 인터넷 게이트웨이를 연결합니다.
인터넷 게이트웨이는 NHN Cloud 콘솔에서 생성할 수 있습니다. 인터넷 게이트웨이를 생성하는 방법은 [사용자 가이드](https://docs.nhncloud.com/ko/Network/Internet%20Gateway/ko/console-guide/#_2)를 참고하세요.
```
resource "nhncloud_networking_routingtable_v2" "resource-rt-01" {
  ...
}

resource "nhncloud_networking_routingtable_attach_gateway_v2" "attach-gw-01" {
  routingtable_id = nhncloud_networking_routingtable_v2.resource-rt-01.id
  gateway_id = "5c7c578a-d199-4672-95d0-1980f996643f"
}
```

| 이름     | 타입      | 필수 | 설명                                                                                                                      |
|--------|---------|----|-------------------------------------------------------------------------------------------------------------------------|
| routingtable_id   | String  | O  | 수정할 라우팅 테이블 ID                                                                                                          |
| gateway_id | String  | O  | 라우팅 테이블에 연결할 인터넷 게이트웨이의 ID<br>콘솔의 **Network > Internet Gateway** 메뉴에서 사용할 인터넷 게이트웨이를 선택하면 하단 상세 정보 화면에서 게이트웨이의 ID 확인 가능 |


## Resources - 로드 밸런서
### 로드 밸런서 생성

```
resource "nhncloud_lb_loadbalancer_v2" "tf_loadbalancer_01"{
  name = "tf_loadbalancer_01"
  description = "create loadbalancer by terraform."
  vip_subnet_id = data.nhncloud_networking_vpcsubnet_v2.default_subnet.id
  vip_address = "192.168.0.10"  
  admin_state_up = true
}
```
| 이름    | 형식 | 필수  | 설명       |
| ------ | ---- | ---- | --------- |
| name | String | - | 로드 밸런서 이름 |
| description | String | - | 로드 밸런서 설명 |
| tenant_id | String | - | 로드 밸런서가 생성될 테넌트 ID |
| vip_subnet_id | String | O | 로드 밸런서가 사용할 서브넷 UUID |
| vip_address | String | - | 로드 밸런서의 IP 지정 |
| security_group_ids | Object | - | 로드 벨런서에 적용할 보안 그룹 ID 목록<br>**보안 그룹은 반드시 이름이 아닌 ID로 지정해야 함** |
| admin_state_up | Boolean | - | 관리자 제어 상태 |
| loadbalancer_type | String | - | 로드 밸런서 타입<br>`shared`/`dedicated` 사용 가능<br>생략할 경우 `shared`로 설정됨 |

### 리스너 생성

```
# HTTP 리스너
resource "nhncloud_lb_listener_v2" "tf_listener_http_01"{
  name = "tf_listener_01"
  description = "create listener by terraform."
  protocol = "HTTP"
  protocol_port = 80
  loadbalancer_id = nhncloud_lb_loadbalancer_v2.tf_loadbalancer_01.id
  default_pool_id = ""
  connection_limit = 2000
  timeout_client_data = 5000
  timeout_member_connect = 5000
  timeout_member_data = 5000
  timeout_tcp_inspect = 5000
  admin_state_up = true
}

# Terminated HTTPS 리스너
resource "nhncloud_lb_listener_v2" "tf_listener_01"{
  name = "tf_listener_01"
  description = "create listener by terraform."
  protocol = "TERMINATED_HTTPS"
  protocol_port = 443
  loadbalancer_id = nhncloud_lb_loadbalancer_v2.tf_loadbalancer_01.id
  default_pool_id = ""
  connection_limit = 2000
  timeout_client_data = 5000
  timeout_member_connect = 5000
  timeout_member_data = 5000
  timeout_tcp_inspect = 5000
  default_tls_container_ref = "https://kr1-api-key-manager-infrastructure.gov-nhncloudservice.com/v1/containers/3258d456-06f4-48c5-8863-acf9facb26de"
  sni_container_refs = null
  admin_state_up = true
}
```
| 이름    | 형식 | 필수  | 설명       |
| ------ | ---- | ---- | --------- |
| name | String | - | 생성할 리스너 이름 |
| description  | String | - | 리스너 설명 |
| protocol | String | O | 생성할 리스너 프로토콜<br>`TCP`, `HTTP,HTTPS`, `TERMINATED_HTTPS` 중 하나 |
| protocol_port | Integer | O | 생성할 리스너 포트 |
| loadbalancer_id | String | O | 생성할 리스너가 연결될 로드 밸런서 ID |
| default_pool_id | String | - | 생성할 리스너에 연결될 기본 풀 ID |
| connection_limit  | Integer | - | 생성할 리스너에 허용되는 최대 연결 수 |
| timeout_client_data  | Integer | - | 클라이언트 비활동 시 타임아웃 설정(ms) |
| timeout_member_connect  | Integer | - | 멤버 연결 시 타임아웃 설정(ms) |
| timeout_member_data | Integer | - | 멤버 비활동 시 타임아웃 시간 설정(ms) |
| timeout_tcp_inspect | Integer | - | 콘텐츠 검사를 위해 추가 TCP 패킷을 기다리는 시간(ms) |
| default_tls_container_ref | String | - | 프로토콜이 `TERMINATED_HTTPS`인 경우 사용할 TLS 인증서 경로 |
| sni_container_refs | Array | - | SNI 인증서 경로 목록 |
| insert_headers | String | - | 백엔드 멤버에게 요청을 전송하기 전에 추가할 헤더 목록 |
| admin_state_up | Boolean | - | 관리자 제어 상태 |
| keepalive_timeout | Integer | - | 리스너의 keepalive timeout |


### 풀 생성

```
resource "nhncloud_lb_pool_v2" "tf_pool_01"{
  name = "tf_pool_01"
  description = "create pool by terraform."
  protocol = "HTTP"
  listener_id = nhncloud_lb_listener_v2.tf_listener_01.id
  lb_method = "LEAST_CONNECTIONS"
  persistence{
    type = "APP_COOKIE"
    cookie_name = "testCookie"
  }
  admin_state_up = true
}
```
| 이름    | 형식 | 필수  | 설명       |
| ------ | ---- | ---- | --------- |
| name | String | - | 로드 밸런서 이름 |
| description | String | - | 풀 설명 |
| protocol | String | O | 프로토콜<br>`TCP`, `HTTP`, `HTTPS`, `PROXY` 중 하나 |
| loadbalancer_id | String | -  | 생성할 풀이 연결될 로드밸런서 ID<br>로드밸런서 ID나 리스너 ID 중 하나는 필수로 입력       |
| listener_id | String | -  | 생성할 풀이 연결될 리스너 ID<br>로드밸런서 ID나 리스너 ID 중 하나는 필수로 입력              |
| lb_method | String | O | 풀의 트래픽을 멤버에게 분배하는 로드 밸런싱 방식<br>`ROUND_ROBIN`,`LEAST_CONNECTIONS`,`SOURCE_IP` 중 하나 |
| persistence | Object | - | 생성할 풀의 세션 지속성 |
| persistence.type | String | O | 세션 지속성 타입<br>`SOURCE_IP`, `HTTP_COOKIE`, `APP_COOKIE` 중 하나<br>로드 밸런싱 방식이 `SOURCE_IP`인 경우 사용 할 수 없음<br>프로토콜이 `HTTPS`이거나 `TCP`인 경우 HTTP_COOKIE와 APP_COOKIE를 사용할 수 없음 |
| persistence.cookie_name | String | - | 쿠키 이름<br>persistence.cookie_name은 세션 지속성 타입이 APP_COOKIE인 경우에만 사용 가능 |
| admin_state_up | Boolean | - | 관리자 제어 상태 |
| member_port | Integer | - | 멤버의 수신 포트<br>트래픽을 이 포트로 전달<br>기본 값은 `-1` |


### 헬스 모니터 생성

```
resource "nhncloud_lb_monitor_v2" "tf_monitor_01"{
  name = "tf_monitor_01"
  pool_id = nhncloud_lb_pool_v2.tf_pool_01.id
  type = "HTTP"
  delay = 20
  timeout = 10
  max_retries = 5
  url_path = "/"
  http_method = "GET"
  expected_codes = "200-202"
  admin_state_up = true
}
```
| 이름    | 형식 | 필수  | 설명       |
| ------ | ---- | ---- | --------- |
| name | String | - | 생성할 헬스 모니터의 이름 |
| pool_id | String | O | 생성할 헬스 모니터가 연결될 풀 ID |
| type | String | O | `TCP`, `HTTP`, `HTTPS`만 지원 |
| delay | Integer | O | 상태 확인 간격(초) |
| timeout | Integer | O | 상태 확인 응답 대기 시간(초)<br> timeout은 delay 값보다 작아야 함 |
| max_retries | Integer | O | 최대 재시도 횟수로 1~10 사이의 값 |
| url_path | String | - | 상태 확인 요청 URL |
| http_method | String | - | 상태 확인에 사용할 HTTP 메서드<br>기본값은 GET|
| expected_codes | String | - | 정상 상태로 간주할 멤버의 HTTP(S) 응답 코드<br>expected_codes는 목록(`200,201,202`)이나 범위 지정(`200-202`)도 가능 |
| admin_state_up | Boolean | - | 관리자 제어 상태 |
| host_header | String | - | 상태 확인에 사용할 호스트 헤더의 필드값<br>상태 확인 타입을 `TCP`로 설정한 경우 이 필드에 설정한 값은 무시 |
| health_check_port | Integer | - | 헬스 체크의 대상이 되는 멤버 포트 |

### 멤버 생성

<font color='red'>**(주의) NHN Cloud에서 멤버 생성 시에 `subnet_id`를 필수로 지정합니다. 또한 `name`은 지원하지 않습니다.**</font>

```
resource "nhncloud_lb_member_v2" "tf_member_01"{
  pool_id = nhncloud_lb_pool_v2.tf_pool_01.id
  subnet_id = data.nhncloud_networking_vpcsubnet_v2.default_subnet.id
  address = nhncloud_compute_instance_v2.tf_instance_01.access_ip_v4
  protocol_port = 8080
  weight = 4
  admin_state_up = true
}

```
| 이름    | 형식 | 필수  | 설명       |
| ------ | ---- | ---- | --------- |
| pool_id | String | O | 생성할 멤버가 속한 풀 ID |
| subnet_id | String | O | 생성할 멤버의 서브넷 ID |
| address | String | O | 로드 밸런서에서 트래픽을 수신할 멤버의 IP 주소 |
| protocol_port | Integer | O | 트래픽을 수신할 멤버의 포트 |
| weight | Integer | - | 풀에서 받아야 하는 트래픽의 가중치<br>높을수록 트래픽을 많이 받음 |
| admin_state_up | Boolean | - | 관리자 제어 상태 |


### 시크릿 생성

```
resource "nhncloud_keymanager_secret_v1" "secret_01" {
  algorithm            = "aes"
  bit_length           = 256
  mode                 = "cbc"
  name                 = "mysecret"
  payload              = "foobar"
  payload_content_type = "text/plain"
  secret_type          = "passphrase"
}
```

| 이름                       | 형식 | 필수 | 설명                                                                                                                                                           |
|--------------------------| ---- |----|--------------------------------------------------------------------------------------------------------------------------------------------------------------|
| name                     | String | -  | 시크릿 이름                                                                                                                                                       |
| expiration               | Datetime | -  | 만료일. ISO8601 형식으로 요청                                                                                                                                         |
| algorithm                | String | -  | 암호화 알고리즘                                                                                                                                                     |
| bit_length               | String | -  | 암호화 키 길이                                                                                                                                                     |
| mode                     | String | -  | 블록 암호 운용 방식                                                                                                                                                  |
| payload                  | String | -  | 암호화 키 페이로드                                                                                                                                                   |
| payload_content_type     | String | -  | 암호화 키 페이로드 콘텐츠 타입 </br>payload를 입력할 시 필수로 입력해야 함 </br>지원하는 콘텐츠 타입 목록: `text/plain`, `application/octet-stream`, `application/pkcs8`, `application/pkix-cert` |
| payload_content_encoding | Enum | -  | 암호화 키 페이로드 인코딩 방식 </br>payload_content_type이 `text/plain`이 아닌 경우 필수로 입력해야 함 </br> `base64`만 지원                                                               |
| secret_type              | Enum | -  | 시크릿 타입 </br>`symmetric`, `public`, `private`, `passphrase`, `certificate`, `opaque` 중 하나                                                                     |


### 시크릿 컨테이너 생성

```
resource "nhncloud_keymanager_secret_v1" "secret_01" {
...
}

resource "nhncloud_keymanager_container_v1" "container_01" {
  name      = "terraform_container_01"
  type      = "generic"
  secret_refs {
    secret_ref = nhncloud_keymanager_secret_v1.secret_01.secret_ref
  }
}
```

| 이름   | 형식     | 필수 | 설명                                                                                                                                                                                             |
|------|--------|----|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| type | Enum   | O  | 컨테이너 타입 </br>`generic`, `rsa`, `certificate` 중 하나                                                                                                                                              |
| name | String | -  | 컨테이너 이름                                                                                                                                                                                        |
| secret_refs | Array  | -  | 컨테이너에 등록할 시크릿 목록                                                                                                                                                                               |
| secret_refs.secret_ref	 | String | -  | 시크릿 주소                                                                                                                                                                                         |
| secret_refs.name	 | String | -  | 컨테이너가 지정한 시크릿 이름 </br>컨테이너 타입이 `certificate`인 경우: `certificate`, `private_key`, `private_key_passphrase`, `intermediates`로 지정 </br>컨테이너 타입이 `rsa`인 경우: `private_key`, `private_key_passphrase`, `public_key`로 지정 |


## Resources - 보안 그룹

### 보안 그룹 생성

```
resource "nhncloud_networking_secgroup_v2" "resource-sg-01" {
  name      = "sg-01"
}
```

| 이름   | 형식     | 필수 | 설명               | 
|------|--------|---|------------------|
| name | String | O | 보안 그룹 이름         |
| region | String | - | 보안 그룹이 할당될 리전 이름 |

### 보안 규칙 생성

```
resource "nhncloud_networking_secgroup_rule_v2" "resource-sg-rule-01" {
  direction         = "ingress"
  ethertype         = "IPv4"
  protocol          = "tcp"
  port_range_min    = 22
  port_range_max    = 22
  remote_ip_prefix  = "0.0.0.0/0"
  security_group_id = data.nhncloud_networking_secgroup_v2.sg-01.id
}

###################### Data Sources ######################

data "nhncloud_networking_secgroup_v2" "sg-01" {
  name = "sg-01"
}
```

| 이름   | 형식     | 필수 | 설명               | 
|------|--------|---|------------------|
| remote_group_id | UUID | - | 보안 규칙의 원격 보안 그룹 ID |
| direction | Enum | O | 보안 규칙이 적용되는 패킷 방향<br>**ingress**, **egress** |
| ethertype | Enum | - | `IPv4`로 지정. 생략 시 `IPv4`로 지정 |
| protocol | String | - | 보안 규칙의 프로토콜 이름. 생략 시에 모든 프로토콜에 적용. |
| port_range_max | Integer | - | 보안 규칙의 포트 범위 최댓값 |
| port_range_min | Integer | - | 보안 규칙의 포트 범위 최솟값 |
| security_group_id | UUID | O | 보안 규칙이 속한 보안 그룹 ID |
| remote_ip_prefix | Enum | - | 보안 규칙의 목적지 IP 접두사 |
| description | String | - | 보안 규칙 설명 |

## 참고 사이트
Terraform Documentation - [https://www.terraform.io/docs/providers/index.html](https://www.terraform.io/docs/providers/index.html)
