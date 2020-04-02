## 서드파티 사용 가이드 > Terraform 사용 가이드
TOAST 리소스는 TerraForm을 통해 생성, 추가, 변경, 삭제할 수 있습니다. 
이 문서에서는 Terraform을 이용해 TOAST 리소스를 관리하는 방법을 설명합니다.

다음은 TOAST가 지원하는 TerraForm 기능 목록입니다.

* Management TOAST resource
    * Compute
        * openstack_compute_instance_v2
        * openstack_compute_volume_attach_v2
    * Network
        * openstack_lb_loadbalancer_v2
        * openstack_lb_listener_v2
        * openstack_lb_pool_v2
        * openstack_lb_member_v2
        * openstack_lb_monitor_v2
        * openstack_compute_floatingip_v2
        * openstack_compute_floatingip_associate_v2
        * openstack_networking_port_v2
    * Storage
        * openstack_blockstorage_volume_v2

* Reference TOAST resource data
    * openstack_images_image_v2
    * openstack_blockstorage_volume_v2
    * openstack_compute_flavor_v2
    * openstack_blockstorage_snapshot_v2
    * openstack_networking_network_v2
    * openstack_networking_subnet_v2

> [참고]
> 목록 이외의 기능 사용시 TOAST는 정상 동작을 보장하지 않습니다. 
> 아래 예시의 모든 데이터는 실제 정보가 아닙니다. 반드시 정확한 정보로 수정하시기 바랍니다.

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

Terraform은 주요 공급자들의 거의 모든 솔루션을 지원합니다.

* AWS, BareMetal, Bitbucket, Chef, Cloudflare, Docker, GitHub, Google Cloud, Grafana, InfluxDB, Heroku, Microsoft Azure, MySQL, OpenStack, PostgreSQL 등

## Terraform 설치
[Terraform 다운로드 페이지](https://www.terraform.io/downloads.html)에서 로컬 PC의 운영체제에 맞는 파일을 다운로드합니다. 파일의 압축을 해제하고 원하는 경로에 넣은 다음 환경 설정에 해당 경로를 추가하면 설치가 완료됩니다.

다음은 설치 예시입니다.

```
$ wget https://releases.hashicorp.com/terraform/0.11.1/terraform_0.11.1_linux_amd64.zip
$ unzip terraform_0.11.1_linux_amd64.zip
$ export PATH="${PATH}:$(pwd)"
$ terraform -v
Terraform v0.10.5

Your version of Terraform is out of date! The latest version
is 0.11.1. You can update by downloading from www.terraform.io
```

> [참고]
> 이 예시에서는 `export` 명령을 이용해 경로를 설정했기 때문에 터미널을 닫으면 설정한 경로가 사라집니다.
> `.bashrc` 또는 `.bash_profile`과 같은 사용자 프로파일에서 경로를 설정하도록 하면 계속 사용할 수 있습니다.

아무런 파라미터 없이 Terraform을 실행하면 간단한 사용법을 볼 수 있습니다.

```
$ terraform
Usage: terraform [--version] [--help] <command> [args]

The available commands for execution are listed below.
The most common, useful commands are shown first, followed by
less common or more advanced commands. If you're just getting
started with Terraform, stick with the common commands. For the
other commands, please read the help and docs before usage.

Common commands:
    apply              Builds or changes infrastructure
    console            Interactive console for Terraform interpolations
    destroy            Destroy Terraform-managed infrastructure
    env                Workspace management
    fmt                Rewrites config files to canonical format
    get                Download and install modules for the configuration
    graph              Create a visual graph of Terraform resources
    import             Import existing infrastructure into Terraform
    init               Initialize a Terraform working directory
    output             Read an output from a state file
    plan               Generate and show an execution plan
    providers          Prints a tree of the providers used in the configuration
    push               Upload this Terraform module to Atlas to run
    refresh            Update local state file against real resources
    show               Inspect Terraform state or plan
    taint              Manually mark a resource for recreation
    untaint            Manually unmark a resource as tainted
    validate           Validates the Terraform files
    version            Prints the Terraform version
    workspace          Workspace management

All other commands:
    debug              Debug output management (experimental)
    force-unlock       Manually unlock the terraform state
    state              Advanced state management
```

## Terraform 초기화
Terraform을 사용하기 전에 다음과 같이 공급자 설정 파일을 구성해야 합니다.

```
$ vi provider.tf

# Configure the OpenStack Provider
provider "openstack" {
  user_name   = "terraform-guide@nhnent.com"
  tenant_id   = "75a4c0a12fd84edeb68965d320d17129"
  password    = "kGzBDD9psmgeH4ji"
  auth_url    = "https://api-gw.cloud.toast.com/terraform/identity/v2.0"
  region      = "KR1"
}
```
* **provider**
    * 공급자 이름을 명시해야 합니다.
    * TOAST는 OpenStack으로 구축되어 있으므로 공급자 이름은 **openstack**입니다.
* **user_name**
    * TOAST ID를 사용합니다.
* **tenant_id**
    * TOAST 콘솔의 **Compute > Instance > 관리** 메뉴에서 **API 엔드포인트 설정** 버튼을 클릭해 테넌트 ID를 확인할 수 있습니다.
* **password**
    * **API Endpoint 설정** 창에서 저장한 **API 비밀번호**를 사용합니다.
* **auth_url**
    * Terraform을 사용하기 위한 auth_url 
    * TOAST 콘솔의 **Compute > Instance > 관리** 메뉴에서 **API 엔드포인트 설정** 버튼을 클릭해 신원 서비스(identity) URL을 확인할 수 있습니다.
* **region**
    * TOAST 리소스를 관리할 리전 정보를 입력합니다. 
    * `KR1`: 한국(판교) 리전
    * `KR2`: 한국(평촌) 리전
    * `JP1`: 일본(도쿄) 리전
    * `US1`: 미국(캘리포니아) 리전
    
> [참고]
> API 비밀번호 설정 방법은 **API 가이드**의 [토큰 API](/Compute/Instance/ko/api-guide/#api) 항목을 참고합니다.

구성한 공급자 설정 파일이 있는 경로에서 `init` 명령을 이용해 Terraform을 초기화합니다.

```
$ terraform init

Initializing provider plugins...
- Checking for available provider plugins on https://releases.hashicorp.com...
- Downloading plugin for provider "openstack" (1.1.0)...

The following providers do not have any version constraints in configuration,
so the latest version was installed.

To prevent automatic upgrades to new major versions that may contain breaking
changes, it is recommended to add version = "..." constraints to the
corresponding provider blocks in configuration, with the constraint strings
suggested below.

* provider.openstack: version = "~> 1.1"

Terraform has been successfully initialized!

You may now begin working with Terraform. Try running "terraform plan" to see
any changes that are required for your infrastructure. All Terraform commands
should now work.

If you ever set or change modules or backend configuration for Terraform,
rerun this command to reinitialize your working directory. If you forget, other
commands will detect it and remind you to do so if necessary.
```

## Terraform 사용하기
Terraform 초기화 과정을 마쳤으면 `.tf` 확장자 파일을 작성하고 Terraform을 실행 할 수 있습니다.

### Terraform 구축 계획 확인
Terraform을 실행 하기전 작성한 `.tf` 확장자 파일을 통해 Terraform 실행시 TOAST 리소스의 변동사항을 확인 할 수 있습니다.
.tf 파일들이 있는 경로에서 `plan` 명령을 실행하면 Terraform이 .tf 파일들을 로드해 설정이 올바른지 확인하고 자체 DB와 비교하여 플랜을 생성합니다. 
플랜 생성을 완료하면 플랜을 유형별로 집계하여 보기 좋게 출력합니다.
```
$ terraform plan
Refreshing Terraform state in-memory prior to plan...
The refreshed state will be used to calculate this plan, but will not be
persisted to local or remote state storage.


------------------------------------------------------------------------

An execution plan has been generated and is shown below.
Resource actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  + openstack_compute_instance_v2.terraform-test-01
      id:                                   <computed>
      access_ip_v4:                         <computed>
      access_ip_v6:                         <computed>
      all_metadata.%:                       <computed>
      availability_zone:                    <computed>
      block_device.#:                       "1"
      block_device.0.boot_index:            "0"
      block_device.0.delete_on_termination: "true"
      block_device.0.destination_type:      "volume"
      block_device.0.source_type:           "image"
      block_device.0.uuid:                  "6d0993b4-cd6d-4242-b59b-94258f265331"
      block_device.0.volume_size:           "20"
      flavor_id:                            "da74152c-0167-4ce9-b391-8a88a8ff2754"
      flavor_name:                          <computed>
      force_delete:                         "false"
      image_id:                             <computed>
      image_name:                           <computed>
      key_pair:                             "tf-test"
      name:                                 "terraform-test-01"
      network.#:                            "1"
      network.0.access_network:             "false"
      network.0.fixed_ip_v4:                <computed>
      network.0.fixed_ip_v6:                <computed>
      network.0.floating_ip:                <computed>
      network.0.mac:                        <computed>
      network.0.name:                       "Default Network"
      network.0.port:                       <computed>
      network.0.uuid:                       "00d5b852-cb77-4307-b6be-d81dad24eec1"
      region:                               "RegionOne"
      security_groups.#:                    "1"
      security_groups.3814588639:           "default"
      stop_before_destroy:                  "false"


Plan: 1 to add, 0 to change, 0 to destroy.

------------------------------------------------------------------------

Note: You didn't specify an "-out" parameter to save this plan, so Terraform
can't guarantee that exactly these actions will be performed if
"terraform apply" is subsequently run.
```

### Terraform 실행하기
구축 계획을 확인하여 문제가 없다면, `apply` 명령을 통해 구축 계획을 적용하여 TOAST 리소스를 생성, 삭제, 변경을 할 수 있습니다.

#### TOAST 리소스 생성하기
TOAST 리소스 생성을 위해 아래 가이드를 참고하여 `.tf`에 리소스를 추가하고 Terraform을 실행하면 새로운 TOAST 리소스를 생성할 수 있습니다.
```
$ vi instance.tf

# Create a web server
resource "openstack_compute_instance_v2" "terraform-instance-01" {
  name      = "terraform-instance-01"
  region    = "RegionOne"
  flavor_id = "da74152c-0167-4ce9-b391-8a88a8ff2754"
  key_pair  = "terraform-keypair"
  network {
    uuid = "00d5b852-cb77-4307-b6be-d81dad24eec1"
    name = "Default Network"
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

```
$ terraform apply
openstack_compute_instance_v2.terraform-test-01: Creating...
  access_ip_v4:                         "" => "<computed>"
  ...
  stop_before_destroy:                  "" => "false"
openstack_compute_instance_v2.terraform-test-01: Still creating... (10s elapsed)
openstack_compute_instance_v2.terraform-test-01: Still creating... (20s elapsed)
openstack_compute_instance_v2.terraform-test-01: Still creating... (30s elapsed)
openstack_compute_instance_v2.terraform-test-01: Still creating... (40s elapsed)
openstack_compute_instance_v2.terraform-test-01: Still creating... (50s elapsed)
openstack_compute_instance_v2.terraform-test-01: Still creating... (1m0s elapsed)
openstack_compute_instance_v2.terraform-test-01: Still creating... (1m10s elapsed)
openstack_compute_instance_v2.terraform-test-01: Creation complete after 1m12s (ID: f4d135bc-6a70-4c4d-b645-931570c9f6b1)

Apply complete! Resources: 1 added, 0 changed, 0 destroyed.
```

처음 `apply` 명령이 실행하면 플랜 변경 이력을 기록하는 자체 DB파일(terraform.tfstate)이 생성됩니다.
```
$ ls
provider.tf               tc-instance-01.tf         terraform.tfstate         terraform.tfstate.backup
```

#### TOAST 리소스 수정하기
변경할 TOAST 리소스가 정의된 `.tf` 파일을 열어 원하는 정보를 수정하고 플랜을 적용합니다.
변경할 수 있는 사양은 제한적입니다. 만약 변경을 할 수 없는 정보를 수정하면 해당 TOAST 리소스는 삭제 후 작성한 정보에 맞게 새롭게 다시 생성됩니다.

다음은 인스턴스에 보안 그룹을 하나 더 추가하는 예시입니다.

1. 보안 그룹 terraform-sg 추가 
```
$ vi terraform-instance-01.tf
resource "openstack_compute_instance_v2" "terraform-instance-01" {
  ...
  security_groups = ["default", "terraform-sg"]
  ...
}
```

2. 플랜을 로딩하면 변경된 보안 그룹 정보를 정리하여 출력합니다.
```
$ terraform plan
Refreshing Terraform state in-memory prior to plan...
The refreshed state will be used to calculate this plan, but will not be
persisted to local or remote state storage.

openstack_compute_instance_v2.terraform-instance-01: Refreshing state... (ID: 4d135bc-6a70-4c4d-b645-931570c9f6b1)

------------------------------------------------------------------------

An execution plan has been generated and is shown below.
Resource actions are indicated with the following symbols:
  ~ update in-place

Terraform will perform the following actions:

  ~ openstack_compute_instance_v2.terraform-instance-01
      security_groups.#:          "1" => "2"
      security_groups.3814588639: "default" => "default"
      security_groups.4051241745: "" => "terraform-sg"


Plan: 0 to add, 1 to change, 0 to destroy.

------------------------------------------------------------------------

Note: You didn't specify an "-out" parameter to save this plan, so Terraform
can't guarantee that exactly these actions will be performed if
"terraform apply" is subsequently run.
```

3. 플랜을 적용하면 인스턴스에 새로운 보안 그룹이 추가됩니다.
```
$ terraform apply
openstack_compute_instance_v2.terraform-instance-01: Refreshing state... (ID: 4d135bc-6a70-4c4d-b645-931570c9f6b1)
openstack_compute_instance_v2.terraform-instance-01: Modifying... (ID: 4d135bc-6a70-4c4d-b645-931570c9f6b1)
  security_groups.#:          "1" => "2"
  security_groups.3814588639: "default" => "default"
  security_groups.4051241745: "" => "terraform-sg"
openstack_compute_instance_v2.terraform-instance-01: Modifications complete after 1s (ID: 4d135bc-6a70-4c4d-b645-931570c9f6b1)

Apply complete! Resources: 0 added, 1 changed, 0 destroyed.
```

#### TOAST 리소스 삭제하기
Terraform을 통해 생성한 TOAST 리소스는 Terraform을 이용하여 손쉽게 삭제가 가능합니다. 

**1. Terraform을 통해 생성한 리소스를 지우기 위해 해당하는 `.tf` 파일을 삭제합니다.**  
```
$ rm tc-instance-01.tf
```

**2. 구축 계획을 통해 해당 리소스가 삭제될 것을 확인 할 수 있습니다.** 
```
$ terraform plan
Refreshing Terraform state in-memory prior to plan...
The refreshed state will be used to calculate this plan, but will not be
persisted to local or remote state storage.

openstack_compute_instance_v2.terraform-test-01: Refreshing state... (ID: 4d135bc-6a70-4c4d-b645-931570c9f6b1)

------------------------------------------------------------------------

An execution plan has been generated and is shown below.
Resource actions are indicated with the following symbols:
  - destroy

Terraform will perform the following actions:

  - openstack_compute_instance_v2.terraform-test-01


Plan: 0 to add, 0 to change, 1 to destroy.

------------------------------------------------------------------------

Note: You didn't specify an "-out" parameter to save this plan, so Terraform
can't guarantee that exactly these actions will be performed if
"terraform apply" is subsequently run.
```

**3. Terraform을 실행하면 해당 리소스는 삭제됩니다.**
```
$ terraform apply
openstack_compute_instance_v2.terraform-test-01: Refreshing state... (ID: f4d135bc-6a70-4c4d-b645-931570c9f6b1)
openstack_compute_instance_v2.terraform-test-01: Destroying... (ID: f4d135bc-6a70-4c4d-b645-931570c9f6b1)
openstack_compute_instance_v2.terraform-test-01: Still destroying... (ID: f4d135bc-6a70-4c4d-b645-931570c9f6b1, 10s elapsed)
openstack_compute_instance_v2.terraform-test-01: Destruction complete after 11s
```

## TOAST 리소스 조회
Terraform은 여러 TOAST 리소스 정보를 조회하여 TOAST 리소스를 관리하기 위해 사용할 수 있습니다.
가져온 TOAST 리소스 정보는 수정을 할 수 없으며 오직 참조만 가능합니다.

### 이미지
인스턴스 생성시 필요한 이미지 정보를 가져옵니다. 
TOAST가 제공하는 공용 이미지 또는 개인 이미지를 지원합니다.

```
$ vi image.tf

data "openstack_images_image_v2" "ubuntu_1804_20200218" {
  name = "Ubuntu Server 18.04.3 LTS (2020.02.18)"
  most_recent = true
}

# 같은 이름의 이미지중 가장 오래된 이미지 조회
data "openstack_images_image_v2" "windows2016_20200218" {
  name = "Windows 2016 STD with MS-SQL 2016 Standard (2020.02.18) KO"
  sort_key = "created_at"
  sort_direction = "asc"
  owner = "c289b99209ca4e189095cdecebbd092d"
  tag = "_AVAILABLE_"
}
```
| 이름    | 형식 | 필수  | 설명       |
| ------ | ---- | ---- | --------- |
| name | String | - | 조회할 이미지 이름 |
| size_min | Integer | - | 조회할 이미지의 최소 크기(byte) |
| size_max | Integer | - | 조회할 이미지의 최대 크기(byte) |
| properties | Object | - | 조회할 이미지의 속성<br>모든 속성 값이 일치하는 이미지가 조회 |
| sort_key | String | - | 특정 속성을 기준으로 조회한 이미지 목록 정렬<br>기본값은 `name` |
| sort_direction | String | - | 조회한 이미지 목록 정렬 방향 <br>`asc`: 오름차순(기본값) <br>`desc`: 내림차순 |
| owner | String | - | 조회할 이미지가 속한 테넌트 ID |
| tag | String | - | 특정 태그가 존재하는 이미지 검색 |
| visibility | String | - | 조회할 이미지의 보여주기 속성<br>public, private, shared 중 하나의 값만 선택 가능<br>생략하면 모든 종류의 이미지 목록을 반환 |
| most_recent | Boolean | - | `true`: 조회한 이미지 목록중 가장 최근에 만들어진 이미지 선택<br>`false`: 조회된 순서로 이미지 선택 | 
| member_status | String | - | 조회할 이미지 멤버 상태 <br>`accepted`,`pending`,`rejected`,`all`중 하나.| 

>[참고]
> * 이미지 이름은 **TOAST 콘솔 Compute > Instance에서 인스턴스 생성 버튼**을 클릭하면 TOAST에서 제공하는 이미지 목록을 통해 확인 가능합니다.
> * 이미지 이름은 TOAST 콘솔에 표시된 **<이미지 설명>**으로 작성해야 합니다.
> * 만약 언어 항목이 존재한다면 위의 예시처럼 **"<이미지 설명> <언어>"** 형식으로 입력해야 합니다.

### 블록 스토리지
```
$ vi block_storage.tf

data "openstack_blockstorage_volume_v2" "volume_00"{
  name = "ssd_volume1"
  status = "available"
}
```
| 이름    | 형식 | 필수  | 설명       |
| ------ | ---- | ---- | --------- |
| name | String | - | 조회할 블록스토리지 이름 |
| status | String | - | 조회할 블록스토리지 상태 |
| metadata | Object | - | 조회할 블록 스토리지와 관련된 메타데이타 |

### 인스턴스 타입
```
vi flavor.tf

#u2.c1m2
data "openstack_compute_flavor_v2" "u2c2m4"{
  name = "u2.c2m4"
}
```
| 이름    | 형식 | 필수  | 설명       |
| ------ | ---- | ---- | --------- |
| name | String | - | 조회할 인스턴스 타입 이름 |

>[참고]
>인스턴스 타입 목록은 **TOAST 콘솔 Compute > Instance에서 인스턴스 생성 > 인스턴스 타입 선택 버튼**을 통해 확인 할 수 있습니다. 


### 스냅숏
```
vi snapshot.tf

data "openstack_blockstorage_snapshot_v2" "my_snapshot" {
  name = "my-snapshot"
  volume_id = data.openstack_blockstorage_volume_v2.volume_00.id
  status = "available"
  most_recent = true
}
```
| 이름    | 형식 | 필수  | 설명       |
| ------ | ---- | ---- | --------- |
| name | String | - | 조회할 스냅숏 이름 |
| volume_id | String | - | 조회할 스냅숏의 원본 블록 스토리지 ID | 
| status | String | - | 조회할 스냅숏의 상태 |
| most_recent | Boolean | - | `true`: 조회한 스냅숏 목록중 가장 최근에 만들어진 스냅숏 선택<br>`false`: 조회된 순서로 스냅숏 선택 |

### VPC
```
vi vpc_network.tf

data "openstack_networking_network_v2" "default_network"{
  name="Default Network"
  network_id = "00d5b852-cb77-4307-b6be-d81dad24eec1"
}
```
| 이름    | 형식 | 필수  | 설명       |
| ------ | ---- | ---- | --------- |
| name | String | - | 조회할 VPC 네트워크 이름 |
| network_id | String | - | 조회할 VPC 네트워크 UUID |

>[참고]
>VPC 네트워크의 UUID는 **TOAST 콘솔 Network > VPC**에서 VPC를 선택하여 확인 가능합니다.

### 서브넷
```
vi subnet.tf

data "openstack_networking_subnet_v2" "default_subnet"{
  name = "Default Network"
  subnet_id = "756af037-54f3-4aa2-8c22-56c9da055553"
  network_id = data.openstack_networking_network_v2.default_network.network_id
}
```
| 이름    | 형식 | 필수  | 설명       |
| ------ | ---- | ---- | --------- |
| name | String | - | 조회할 서브넷의 이름 |
| subnet_id | String | - | 조히할 서브넷의 UUID |
| network_id | String | - | 조회할 서브넷이 속한 네트워크 UUID |

>[참고]
>서브넷 ID는 **TOAST 콘솔 Network > VPC > 서브넷**에서 서브넷을 선택하여 확인 가능합니다.

## 블록 스토리지 관리
### 블록 스토리지 생성
```
vi block_storage.tf

# HDD 블록 스토리지 생성
resource "openstack_blockstorage_volume_v2" "volume_01" {
  name = "tf_volume_01"
  size = 10
  availability_zone = "kr-pub-a"
  volume_type = "General HDD"
}

# SSD 블록 스토리지 생성
resource "openstack_blockstorage_volume_v2" "volume_02" {
  name = "tf_volume_02"
  size = 10
  availability_zone = "kr-pub-b"
  volume_type = "General SSD"
}

# 스냅숏으로 블록 스토리지 생성
resource "openstack_blockstorage_volume_v2" "volume_03" {
  name = "tf_volume_03"
  description = "terraform create volume with snapshot test"
  snapshot_id = data.openstack_blockstorage_snapshot_v2.snapshot_01.id
  size = 30
}

# 볼륨 복제1 - 볼륨 내용물만 복제(가용성 영역이 다를 수 있음)
resource "openstack_blockstorage_volume_v2" "volume_04" {
  name = "tf_volume_04"
  description = "terraform copy volume test"
  source_replica = openstack_blockstorage_volume_v2.volume_01.id
  size = 10
}

# 볼륨 복제2 - 복제대상 볼륨을 완벽하게 복제(가용성 영역이 같음)
resource "openstack_blockstorage_volume_v2" "volume_05" {
  name = "tf_volume_05"
  description = "terraform copy volume"
  source_vol_id = openstack_blockstorage_volume_v2.volume_01.id
  size = 10
}
```
| 이름    | 타입 | 필수  | 설명       |
| ------ | --- |---- | --------- |
| name | String | - | 생성할 블록 스토리지 이름 |
| description | String | - | 블록 스토리지 설명 |
| size | Integer | - | 생성할 블록 스토리지 크기(GB) |
| source_vol_id | String | - | 복제할 블록 스토리지 ID |
| availability_zone | String | - | 생성할 블록 스토리지의 가용성 영역 |
| volume_type | String | O | 블록 스토리지 타입 (General HDD, General SSD) |

>[참고]
>* availability_zone은 TOAST 콘솔 Storage > Block Storage > 관리의 블록 스토리지 생성 버튼을 눌러서 표시되는 가용성 영역에서 확인할 수 있습니다.

## 네트워크 관리 
### 로드 밸런서 생성
```
$ vi loadbalancer.tf

resource "openstack_lb_loadbalancer_v2" "tf_loadbalancer_01"{
  name = "tf_loadbalancer_01"
  description = "create loadbalancer by terraform."
  vip_subnet_id = data.openstack_networking_subnet_v2.default_subnet.id
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
| security_group_ids | Object | - | 로드 벨런서에 적용 할 보안 그룹 ID 목록 |
| admin_state_up | Boolean | - | 관리자 제어 상태 |

>[참고]
>보안 그룹은 반드시 이름이 아닌 ID로 지정해야 합니다. 

### 리스너 생성
```
$ vi listener.tf 

resource "openstack_lb_listener_v2" "tf_listener_01"{
  name = "tf_listener_01"
  description = "create listener by terraform."
  protocol = "TERMINATED_HTTPS"
  protocol_port = 443
  loadbalancer_id = openstack_lb_loadbalancer_v2.tf_loadbalancer_01.id
  default_pool_id = ""
  connection_limit = 2000
  timeout_client_data = 5000
  timeout_member_connect = 5000
  timeout_member_data = 5000
  timeout_tcp_inspect = 5000
  default_tls_container_ref = "http://iaas.tcc1.cloud.toastoven.net/key-manager/v1/containers/3258d456-06f4-48c5-8863-acf9facb26de"
  sni_container_refs = null
  admin_state_up = true
}
```
| 이름    | 형식 | 필수  | 설명       |
| ------ | ---- | ---- | --------- |
| name | String | - | 생성할 리스너의 이름 |
| description  | String | - | 리스너 설명 |
| tenant_id | String | - | 생성할 리스너의 테넌트 ID |
| protocol | String | O | 생성할 리스너의 프로토콜<br>`TCP`, `HTTP,HTTPS`, `TERMINATED_HTTPS` 중 하나 |
| protocol_port | Integer | O | 생성할 리스너의 포트 |
| loadbalancer_id | String | O | 생성할 리스너가 연결될 로드 밸런서 ID |
| default_pool_id | String | - | 생성할 리스너에 연결될 기본 풀 ID |
| connection_limit  | Integer | - | 생성할 리스너에 허용되는 최대 연결 수 |
| timeout_client_data  | Integer | - | 클라이언트 비활동시 timeout 설정 (ms) |
| timeout_member_connect  | Integer | - | 멤버 연결시 timeout 설정 (ms) |
| timeout_member_data | Integer | - | 멤버 비활동시 timeout 시간 설정 (ms) |
| timeout_tcp_inspect | Integer | - | 콘텐츠 검사를 위해 추가 TCP 패킷을 기다리는 시간 (ms) |
| default_tls_container_ref | String | - | 프로토콜이 `TERMINATED_HTTPS`인 경우 사용할 기본인프라상품 key-manager의 TLS 인증서 경로 |
| sni_container_refs | Array | - | 기본인프라상품 key-manager의 SNI 인증서 경로 목록 |
| insert_headers | String | - | 백엔드 멤버에게 요청을 전송하기 전에 추가해줄 헤더 목록 |
| admin_state_up | Boolean | - | 관리자 제어 상태 |

### 풀 생성
```
$ vi pool.tf 

resource "openstack_lb_pool_v2" "tf_pool_01"{
  name = "tf_pool_01"
  description = "create pool by terraform."
  tenant_id = openstack_lb_loadbalancer_v2.tf_loadbalancer_01.tenant_id
  protocol = "HTTP"
  listener_id = openstack_lb_listener_v2.tf_listener_01.id
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
| name | String | - | 로드밸런서 이름 |
| description | String | - | 풀 설명 |
| tenant_id | String | - | 생성할 풀의 테넌트 ID |
| protocol | String | O | 프로토콜<br>`TCP`, `HTTP`, `HTTPS`, `PROXY`중 하나 |
| listener_id | String | O | 생성할 풀이 연결될 리스너 ID |
| lb_method | String | O | 풀의 트래픽을 멤버에게 분배하는 로드밸런싱 방식<br>`ROUND_ROBIN`,`LEAST_CONNECTIONS`,`SOURCE_IP`중 하나 |
| persistence | Object | - | 생성할 풀의 세션 지속성 |
| persistence.type | String | O | 세션 지속성 타입<br>`SOURCE_IP`, `HTTP_COOKIE`, `APP_COOKIE`중 하나 |
| persistence.cookie_name | String | - | 쿠키 이름 |
| admin_state_up | Boolean | - | 관리자 제어 상태 |

>[참고]
>* Terraform 공식 문서와 달리 TOAST에서는 `loadbalancer_id`를 지원하지 않습니다.
>* persistence.type은 로드밸런싱 방식이 `SOURCE_IP`인 경우 사용 할 수 없습니다.
>* persistence.type은 프로토콜이 `HTTPS`이거나 `TCP`인 경우 HTTP_COOKIE와 APP_COOKIE를 사용할 수 없습니다.
>* persistence.cookie_name은 세션 지속성 타입이 APP_COOKIE인 경우에만 사용 가능합니다.

### 헬스 모니터 생성
```
$ vi health_monitor.tf 

resource "openstack_lb_monitor_v2" "tf_monitor_01"{
  name = "tf_monitor_01"
  tenant_id = openstack_lb_pool_v2.tf_pool_01.tenant_id
  pool_id = openstack_lb_pool_v2.tf_pool_01.id
  type = "PING" 
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
| tenant_id | String | - | 생성할 헬스 모니터의 테넌트 ID |
| pool_id | String | O | 생성할 헬스 모니터가 연결될 풀 ID |
| type | String | O | `TCP`, `HTTP`, `HTTPS` |
| delay | Integer | O | 상태 확인 간격(초) |
| timeout | Integer | O | 상태 확인 응답 대기 시간 (초)|
| max_retries | Integer | O | 최대 재시도 횟수 |
| url_path | String | - | 상태 확인 요청 URL |
| http_method | String | - | 상태 확인에 사용할 HTTP Method<br>기본값은 GET|
| expected_codes | String | - | 정상 상태로 간주할 멤버의 HTTP(S) 응답 코드 |
| admin_state_up | Boolean | - | 관리자 제어 상태 |

>[참고]
>* expected_codes는 목록(`200,201,202`)이나 범위 지정(`200-202`)도 가능합니다. 
>* timeout은 delay 값 보다 작아야 합니다. 
>* 입력 가능한 max_retries 값의 범위는 1~10 입니다.

### 멤버 생성
```
$ vi member.tf 

resource "openstack_lb_member_v2" "tf_member_01"{
  pool_id = openstack_lb_pool_v2.tf_pool_01.id
  subnet_id = data.openstack_networking_subnet_v2.default_subnet.id
  tenant_id = openstack_lb_pool_v2.tf_pool_01.tenant_id
  address = openstack_compute_instance_v2.tf_instance_01.access_ip_v4
  protocol_port = 8080
  weight = 4
  admin_state_up = true
}

```
| 이름    | 형식 | 필수  | 설명       |
| ------ | ---- | ---- | --------- |
| pool_id | String | O | 생성할 멤버가 속한 풀 ID |
| subnet_id | String | O | 생성할 멤버의 서브넷 ID |
| tenant_id | String | - | 생성할 멤버의 테넌트 ID |
| address | String | O | 로드밸런서에서 트래픽을 수신할 멤버의 IP 주소 |
| protocol_port | Integer | O | 트래픽을 수신할 멤버의 포트 |
| weight | Integer | - | 풀에서 받아야하는 트래픽의 가중치<br>높을 수록 트래픽을 많이 받는다. |
| admin_state_up | Boolean | - | 관리자 제어 상태 |

>[참고]
>* TOAST에서는 `name` 속성을 지원하지 않습니다.
>* Terraform 공식 문서에서는 `subnet_id` 속성이 선택사항으로 나와있으나, TOAST에서는 **필수적으로 명시**해주어야 합니다.

### 플로팅 IP 생성
```
$ vi floating_ip.tf

resource "openstack_compute_floatingip_v2" "fip_01" {
  pool = "Public Network"
}
```
| 이름    | 형식 | 필수  | 설명       |
| ------ | --- |---- | --------- |
| pool | String | O | 플로팅 IP를 생성할 IP풀 |

>[참고]
> * Network > Floating IP에서 플로팅 IP 생성 버튼을 클릭해서 표시되는 IP 풀에서 확인할 수 있습니다.
 

### 플로팅 IP 연결
```
$ vi instance.tf

# 인스턴스 생성
resource "openstack_compute_instance_v2" "tf_instance_01" {
  ...
}

# 플로팅 IP 생성
resource "openstack_compute_floatingip_v2" "fip_01" {
  ...
}

# 플로팅 IP 연결
resource "openstack_compute_floatingip_associate_v2" "fip_associate" {
  floating_ip = openstack_compute_floatingip_v2.fip_01.address
  instance_id = openstack_compute_instance_v2.tf_instance_01.id
}
```
| 이름    | 형식 | 필수  | 설명       |
| ------ | --- |---- | --------- |
| floating_ip | String | O | 연결할 플로팅 IP |
| instance_id | String | O | 플로팅 IP를 연결할 대상 인스턴스 UUID |
| fixed_ip | String | - | 플로팅 IP를 연결할 대상의 고정 IP |
| wait_until_associated | Boolean | - | `true`: 플로팅 IP를 연결될때 까지 대상 인스턴스를 폴링 `false`: 플로팅 IP를 연결될때 까지 대기하지 않음(가본값) |

### 네트워크 포트 생성
```
$ vi port.tf

resource "openstack_networking_port_v2" "port_1" {
  name = "tf_port_1"
  network_id = data.openstack_networking_network_v2.default_network.id
  admin_state_up = "true"
}
```
| 이름    | 형식 | 필수  | 설명       |
| ------ | ---- | ---- | --------- |
| name | String | O | 생성할 포트의 이름 |
| description | String | O | 포트 설명 |
| network_id | String | O | 포트를 연결할 VPC 네트워크 ID |
| tenant_id | String | O | 생성할 포트의 테넌트 ID |
| device_id | String | - | 생성된 포트가 연결될 장치의 ID |
| fixed_ip | Object | - | 생성할 포트의 고정 IP 설정 정보<br>`no_fixed_ip`속성이 없어야 함 |
| fixed_ip.subent_id | String | O | 고정 IP의 서브넷 ID |
| fixed_ip.ip_address | String | - | 설정할 고정 IP의 주소 |
| no_fixed_ip | Boolean | - | `true`: 고정 IP가 없는 포트<br>`fixed_ip`속성이 없어야 함 |
| admin_state_up | Boolean | - | 관리자 제어 상태<br> `true`: 작동<br>`false`: 중지 |

## 인스턴스 관리
### 인스턴스 생성
```
$ vi instance.tf

# u2 인스턴스 생성 
resource "openstack_compute_instance_v2" "tf_instance_01"{
  name = "tf_instance_01"
  region    = "KR1"
  key_pair  = "terraform-keypair"
  image_id = data.openstack_images_image_v2.centos_610_20200218.id
  flavor_id = data.openstack_compute_flavor_v2.u2c1m2.id
  security_groups = ["default","ssh"]

  network {
    name = data.openstack_networking_network_v2.default_network.name
    uuid = data.openstack_networking_network_v2.default_network.id
  }

  block_device {
    uuid = data.openstack_images_image_v2.centos_610_20200218.id
    source_type = "image"
    destination_type = "local"
    boot_index = 0
    delete_on_termination = true
    volume_size = 30
  }
}

# m2 인스턴스 생성 
resource "openstack_compute_instance_v2" "tf_instance_02" {
  name      = "tf_instance_02"
  region    = "KR1"
  key_pair  = "terraform-keypair"
  flavor_id = data.openstack_compute_flavor_v2.m2c1m2.id
  security_groups = ["default","web"]

  network {
    name = data.openstack_networking_network_v2.default_network.name
    uuid = data.openstack_networking_network_v2.default_network.id
  }

  network {
    port = openstack_networking_port_v2.port_1.id
  }

  block_device {
    uuid                  = data.openstack_images_image_v2.centos_610_20200218.id
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

resource "openstack_compute_instance_v2" "terraform-instance-01" {
  name      = "terraform-instance-01"
  region    = "KR1"
  flavor_id = "da74152c-0167-4ce9-b391-8a88a8ff2754"
  key_pair  = "terraform-keypair"
  network {
    uuid = "00d5b852-cb77-4307-b6be-d81dad24eec1"
    name = "Default Network"
  }
  security_groups = ["default","web"]
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
| 이름    | 형식 | 필수  | 설명       |
| ------ | ---- | ---- | --------- |
| name | String | O | 생성할 인스턴스의 이름 |
| region | String | - | 생성할 인스턴스의 리전<br>기본값은 provider.tf에 설정된 region |
| flavor_name | String | - | 생성할 인스턴스 타입의 이름<br>flavor_id가 비어있을 때 필수 |
| flavor_id | String | - | 생성할 인스턴스 타입 ID<br>flavor_name이 비어있을 때 필수 |
| image_name | String | - | 인스턴스 생성시 사용할 이미지 이름<br>image_id가 비어있을 때 필수<br>인스턴스 타입이 U2일 때만 사용가능 |
| image_id | String | - | 인스턴스 생성시 사용할 이미지 ID<br>image_name이 비어있을 때 필수<br>인스턴스 타입이 U2일 때만 사용가능 |
| key_pair | String | - | 인스턴스 접속에 사용할 키페어 이름 |
| network | Object | - | 생성할 인스턴스에 연결할 VPC 네트워크 정보 |
| network.name | String | - | VPC 네트워크 이름 |
| network.uuid | String | - | VPC 네트워크 ID |
| network.port | String | - | VPC 네트워크에 연결할 포트의 ID |
| security_groups | Array | - | 인스턴스에서 사용할 보안 그룹의 이름 목록 |
| user_data | String | - | 	인스턴스 부팅 후 실행할 스크립트 및 설정<br>base64 인코딩된 문자열로 65535 바이트까지 허용<br> |
| block_device | Object | - | 인스턴스에 사용할 이미지 또는 블록 스토리지 정보 객체 |
| block_device.uuid | String | - | 생성할 블록 스토리지의 원본의 ID |
| block_device.source_type | String | O | 생성할 블록 디바이스 원본의 타입<br>`image`: 이미지를 통행 블록 스토리지 생성<br>`blank`: 추가 볼륨 생성시 빈 블록 디바이스 생성 |
| block_device.destination_type | String | - | 인스턴스 볼륨의 위치, 인스턴스 타입에 따라 다르게 설정 필요<br>`local`: U2 인스턴스 타입을 이용하는 경우<br>`volume`: U2 외의 인스턴스 타입을 이용하는 경우 |
| block_device.boot_index | Integer | - | 지정한 볼륨의 부팅 순서<br>0 이면 루트 볼륨<br>그 외는 추가 볼륨<br>숫자가 클 수록 부팅 순서는 낮아짐<br> |
| block_device.volume_size | Integer | - | 생성할 인스턴스에서 사용할 디스크의 용량<br>최소 20GB에서 최대 2,000GB까지 설정 가능<br>만약 인스턴스 타입이 U2일시 필수 입력 |
| block_device.delete_on_termination | Boolean | - | `true`: 인스턴스 삭제시 블록 디바이스도 함께 삭제<br>`false`: 인스턴스 삭제시 블록 디바이스는 함께 삭제하지 않음 |


>[참고]
>* 키페어는 TOAST 콘솔의 **Compute > Instance > Key Pair** 메뉴에서 새로 생성하거나, 이미 가지고 있는 키페어를 등록할 수 있습니다. 자세한 설명은 콘솔 사용 가이드의 [키페어](/Compute/Instance/ko/console-guide/#_7) 항목을 참고합니다.
>* VPC 네트워크는 TOAST 콘솔의 **Network > VPC > Management** 메뉴에서 연결할 VPC를 선택하면, 하단 상세 정보 화면에서 이름과 uuid를 확인할 수 있습니다.
>* flavor_id는 TOAST에서 제공하는 공개 API 중 [인스턴스 타입 목록 조회 API](/Compute/Instance/ko/api-guide/#_18)를 통해 조회할 수 있습니다.
>* 보안 그룹은 TOAST 콘솔의 **Network > VPC > Security Groups** 메뉴에서 사용할 보안 그룹을 선택하면, 하단 상세 정보 화면에서 정보를 확인할 수 있습니다.
>* 인스턴스 타입에 따라 설정할 수 있는 volume_size가 다릅니다. 자세한 설명은 콘솔 사용 가이드의 [인스턴스 생성 > 타입](/Compute/Instance/ko/console-guide/#flavor) 항목을 참고합니다.

### 블록 스토리지 연결
```
vi volume_attach.tf

# 인스턴스 생성
resource "openstack_compute_instance_v2" "tf_instance_01" {
  ...
}

# 블록 스토리지 생성
resource "openstack_blockstorage_volume_v2" "volume_01" {
  ...
}

# 블록 스토리지 연결
resource "openstack_compute_volume_attach_v2" "volume_to_instance"{
  instance_id = openstack_compute_instance_v2.tf_instance_02.id
  volume_id = openstack_blockstorage_volume_v2.volume_01.id
}
```
| 이름    | 타입 | 필수  | 설명       |
| ------ | --- |---- | --------- |
| instance_id | String | - | 블록 스토리지를 연결할 대상 인스턴스 |
| volume_id | String | - | 연결할 블록 스토리지 UUID |

## HCL

Terraform 설정 파일은 HCL(HashiCorp Configuration Language)을 사용합니다. HCL은 Terraform 형식(`.tf`)과 JSON 형식(`tf.json`)을 사용합니다.

지정한 폴더에 `.tf`, `.tf.json`을 저장하면 TerraForm이 알파벳 순서로 로드합니다. 변수나 리소스의 정의 순서는 상관이 없습니다.

그 외에 다른 설정을 덮어쓰기 위한 오버라이드 파일을 사용할 수 있습니다. 파일명을 `override` 또는 `_override`로 끝나도록 하면 됩니다. 오버라이드 파일은 다른 설정 파일들의 로딩이 다 끝나면 알파벳순으로 로드하여 설정들을 덮어씁니다.

### HCL 문법

* **주석**

**#**, **//**, **/\* \*/**을 사용할 수 있습니다.

* **값 할당**

**key = value** 형태를 사용합니다. 값은 문자열, 숫자, 불리언, 리스트, 맵을 모두 사용할 수 있습니다.

* **문자열**

큰따옴표를 사용합니다. 여러 줄의 문자열을 사용할 때는 [유닉스 셸의 Here document](https://en.wikipedia.org/wiki/Here_document) 형식으로 `<<EOF`, `EOF` 사이에 문자열을 넣어야 합니다.

```
description = <<EOF
...문자열문자열문자열문자열...
...문자열문자열문자열문자열...
...문자열문자열문자열문자열...
EOF
```

* **자원**

자원을 선언할 때는 **resource** 키워드를 사용하며 공급자에 따라 Terraform이 정의해 둔 자원 유형을 명시해야 합니다.

```
resource "openstack_compute_instance_v2" "web" {
    ...
}
```

* **공급자**

공급자를 선언할 때는 **provider** 키워드를 사용합니다. 자원을 선언할 때 명시한 자원 유형의 접두사가 공급자 유형입니다.

```
# 자원 유형: openstack_compute_instance_v2
provider "openstack" {
    ...
}

# 자원 유형: aws_instance
provider "aws" {
    ...
}
```

* **데이터 소스**

공급자로부터 가져올 데이터를 데이터 소스라고 합니다. **data** 키워드를 사용하며, 유형(type)과 이름(name)으로 구성합니다.

```
data "type" "name" {
    ...
}
```

* **변수**

변수를 선언할 때는 **variable** 키워드를 사용합니다. 형식을 추론하기 때문에 정의하지 않아도 무방합니다.

```
variable "name" {
    type = "type"
    default = {
        key = value
    }
    description = "description"
}
```

* **모듈**

**module** 키워드를 사용하면 기존에 정의한 리소스 그룹을 모듈로 가져와 사용할 수 있습니다. GitHub, Bitbucket 등을 지원합니다.

```
module "name" {
    source = "url"
    ...
}
```

## 참고 사이트
Terraform Documentation - [https://www.terraform.io/docs/providers/index.html](https://www.terraform.io/docs/providers/index.html)
