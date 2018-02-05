## Compute > Instance > 서드파티 사용 가이드 > Terraform 사용 가이드

이 문서는 TOAST 환경에서 Terraform을 이용해 인스턴스를 관리하는 방법을 설명합니다.

## Terraform
Terraform은 인프라를 손쉽게 구축하고, 안전하게 변경하고, 효율적으로 인프라의 형상을 관리 할 수 있는 오픈 소스 도구입니다. 주요 특징은 다음과 같습니다.

* **Infrastructure as Code**
    * 인프라를 코드로 정의하여 생산성과 투명성을 높일 수 있습니다.
    * 정의한 코드를 쉽게 공유할 수 있어 효율적으로 협업을 진행할 수 있습니다.
* **Execution Plan**
    * 변경 계획과 변경 적용을 분리하여 변경 내용을 적용할 때 발생할 수 있는 실수를 줄일 수 있습니다.
* **Resource Graph**
    * 사소한 변경이 인프라 전체에 어떤 영향을 미칠지 미리 확인할 수 있습니다.
    * 종속성 그래프를 작성하여 이 그래프를 바탕으로 계획을 세우고, 이 계획을 적용했을 때 변경되는 인프라 상태를 확인할 수 있습니다.
* **Change Automation**
    * 여러 장소에 같은 구성의 인프라를 구축하고 변경할 수 있도록 자동화할 수 있습니다.
    * 인프라를 구축하는데 드는 시간을 절약할 수 있고, 실수도 줄일 수 있습니다.

Terraform은 주요 공급자들의 거의 모든 솔루션을 지원합니다.

* AWS, BareMetal, Bitbucket, Chef, Cloudflare, Docker, GitHub, Google Cloud, Grafana, InfluxDB, Heroku, Microsoft Azure, MySQL, OpenStack, PostgreSQL 등


## Terraform 설치
[Terraform 다운로드 페이지](https://www.terraform.io/downloads.html)에서 로컬 PC의 OS에 맞는 파일을 다운로드합니다. 파일의 압축을 해제하고 원하는 경로에 넣은 다음 환경 설정에 해당 경로를 추가하면 설치가 완료됩니다.

다음은 설치 예시입니다.

```
$ wget https://releases.hashicorp.com/terraform/0.11.1/terraform_0.11.1_linux_amd64.zip
$ unzip terraform_0.11.1_linux_amd64.zip
$ export PATH="${PATH}:$(pwd)"
$ terraform - v
Terraform v0.10.5

Your version of Terraform is out of date! The latest version
is 0.11.1. You can update by downloading from www.terraform.io
```

> [참고]
> 이 예시에서는 `export` 명령을 이용해 경로를 설정했기 때문에 터미널을 닫으면 설정한 경로가 사라집니다.
> `.bashrc` 또는 `.bash_profile`과 같은 사용자 프로파일에서 경로를 설정하도록 하면 항구적으로 사용할 수 있습니다.

아무런 파라미터 없이 Terraform을 실행하면 간단한 사용 방법을 볼 수 있습니다.

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

## TOAST 환경에서 사용

TOAST 환경에서 TerraForm을 이용하여 인스턴스를 생성, 추가, 변경, 삭제하는 방법에 대해 예시와 함께 알아보겠습니다.

> [참고]
> 아래 예시의 모든 데이터는 실제 정보가 아닙니다. 반드시 정확한 정보로 수정하시기 바랍니다.

### Terraform 초기화

Terraform을 사용하기 전에 다음과 같이 공급자 설정 파일을 구성해야 합니다.

```
$ vi provider.tf
# Configure the OpenStack Provider
provider "openstack" {
  user_name   = "terraform-guide@nhnent.com"
  tenant_id   = "75a4c0a12fd84edeb68965d320d17129"
  password    = "4DLr-s229-u76u"
  auth_url    = "https://api-gw.cloud.toast.com/infrastructure/identity/v2.0"
  region      = "RegionOne"
}
```

* **provider**
    * 공급자 이름을 명시해야 합니다.
    * TOAST는 OpenStack으로 구축되어 있으므로 공급자 이름은 **_openstack_**입니다.
* **user_name**
    * User Access Key ID 또는 TOAST 계정 ID입니다.
* **tenant_id**
    * 테넌트 ID는 TOAST 콘솔의 **_Compute > Instance > Management_** 메뉴에서 **_API Endpoint 설정_** 버튼을 클릭해 확인할 수 있습니다.
* **password**
    * password는 **API 보안 설정** 메뉴에서 설정할 수 있습니다.
    * API 준비 가이드의 [토큰 API](/Compute/Instance/ko/api-guide/#api) 항목을 참조하십시오.
* **auth_url**
    * auth_url은 `` 입니다.
* **region**
    * 한국 리젼은 **RegionOne**을 사용합니다.


구성한 공급자 설정 파일이 있는 경로에서 `init` 명령을 이용해 Terraform을 초기화합니다.

```
$ terraform init
../terraform init

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

### 인스턴스 생성

인스턴스를 생성하려면 다음과 같이 .tf 파일에 생성할 인스턴스에 대한 정보를 입력해야 합니다.

```
$ vi terraform-instance-01.tf
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

* **resource**
    * 리소스 유형과 리소스 이름으로 구성합니다.
    * TOAST는 OpenStack으로 구축되어 있으므로 리소스 유형은 **openstack_compute_instance_v2** 입니다.
    * 리소스 이름은 생성할 인스턴스의 이름입니다.
* **name**
    * 생성할 인스턴스의 이름입니다.
* **region**
    * 공급자 설정 파일에 기재한 내용과 같아야 합니다.
* **flavor_id**
    * 생성할 인스턴스의 사양 ID 입니다.
    * TOAST에서 제공하는 공개 API 중 [인스턴스 사양 목록 조회 API](/Compute/Instance/ko/api-guide/#_18)를 통해 조회할 수 있습니다.
* **key_pair**
    * 인스턴스 접속에 사용할 키페어 이름입니다.
    * TOAST 콘솔의 **_Compute > Instance > Key Pair_** 메뉴에서 새로 생성하거나, 이미 가지고 있는 키페어를 등록할 수 있습니다. 콘솔 사용 가이드의 [키페어](/Compute/Instance/ko/console-guide/#_7) 항목을 참조하십시오.    
* **network**
    * 인스턴스에 연결할 VPC 이름과 uuid를 입력합니다.
    * TOAST 콘솔의 **_Network > VPC > Management_** 메뉴에서 연결할 VPC를 선택하면, 하단 상세정보 화면에서 이름과 uuid를 확인할 수 있습니다.
* **security_groups**
    * 인스턴스에서 사용할 보안 그룹의 이름입니다.
    * 콤마(,)로 구분하여 하나 이상의 보안 그룹을 지정할 수 있습니다.
    * TOAST 콘솔의 **_Network > VPC > Security Groups_** 메뉴에서 사용할 보안 그룹을 선택하면, 하단 상세정보 화면에서 정보를 확인할 수 있습니다.
* **block_device**
    * 인스턴스에 사용할 이미지 또는 블록 스토리지 정보와 디스크 용량을 설정합니다.
    * uuid
        * TOAST 콘솔의 **_Compute > Images_** 메뉴에서 사용할 이미지를 선택하면 하단 상세정보 화면에서 정보를 확인할 수 있습니다.
    * source_type
        * 이미지를 이용해 인스턴스를 생성한다면 source_type은 **image**입니다.
    * destination_type
        * 블록 디바이스를 인스턴스의 디스크로 사용한다면 destination_type은 **volume**입니다.
    * boot_index
        * 블록 디바이스를 인스턴스의 부트 디스크로 사용한다면 boot index는 **0**입니다.
    * volume_size
        * 생성할 인스턴스에서 사용할 디스크의 용량을 설정합니다.
        * 최소 20GB에서 최대 1,000GB까지 설정할 수 있습니다.
        * 인스턴스 사양에 따라 설정할 수 있는 용량이 다릅니다. 콘솔 사용 가이드의 [인스턴스 생성 > 사양](/Compute/Instance/ko/console-guide/#_4) 항목을 참조하십시오.
    * delete_on_termination
        * 이 옵션이 true로 설정되어 있으면 인스턴스를 삭제할 때 블록 디바이스도 함께 삭제됩니다.


.tf 파일들이 있는 경로에서 `plan` 명령을 실행하면 Terraform이 .tf 파일들을 로드해 설정이 올바른지 확인하고 자체 DB와 비교하여 플랜을 생성합니다. 플랜 생성을 완료하면 플랜을 유형별로 집계하여 보기 좋게 출력합니다.

```
./terraform plan
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


`apply` 명령을 실행하면 플랜을 적용하여 인스턴스를 생성합니다. 그리고 플랜 변경 이력을 기록하는 자체 DB파일(terraform.tfstate)을 생성합니다.

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

$ ls
provider.tf               tc-instance-01.tf         terraform.tfstate         terraform.tfstate.backup
```

생성한 인스턴스는 TOAST 콘솔의 **_Compute > Instance > Management_** 메뉴에서 확인할 수 있습니다.

### 인스턴스 추가 생성

인스턴스 추가는 생성과 같은 방법으로 .tf 파일을 만들고 플랜을 적용합니다. 여러 개의 .tf 파일을 만들고 한꺼번에 적용해도 됩니다.

### 인스턴스 변경

인스턴스를 변경할 .tf 파일을 열어 원하는 정보를 수정하고 플랜을 적용합니다.

변경할 수 있는 사양은 제한적입니다. 디스크를 새로 추가하거나, 인스턴스에 연결한 보안그룹과 VPC를 제거하거나 교체할 수 있습니다. 부트 디스크의 용량을 변경하면 기존의 인스턴스는 삭제되고 새로운 인스턴스가 생성됩니다. 인스턴스 사양은 인스턴스가 종료된 상태에만 변경할 수 있습니다.

아래 예시는 보안 그룹을 하나 더 추가한 내용입니다.

```
$ vi terraform-instance-01.tf
resource "openstack_compute_instance_v2" "terraform-instance-01" {
  ...
  security_groups = ["default", "terraform-sg"]
  ...
}
```

플랜을 로딩하면 변경된 보안 그룹 정보를 정리하여 출력해 줍니다.

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

플랜을 적용하면 인스턴스에 새로운 보안 그룹이 추가됩니다.

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

### 인스턴스 삭제

인스턴스 생성에서 사용했던 .tf 파일을 삭제하고 플랜을 적용하면 인스턴스가 삭제됩니다.

플랜을 로딩하면 리소스 설정을 삭제했기 때문에 삭제된 플랜이 1건이 있음을 보여줍니다.

```
$ rm tc-instance-01.tf
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

플랜을 적용하면 인스턴스가 삭제됩니다.

```
$ terraform apply
openstack_compute_instance_v2.terraform-test-01: Refreshing state... (ID: f4d135bc-6a70-4c4d-b645-931570c9f6b1)
openstack_compute_instance_v2.terraform-test-01: Destroying... (ID: f4d135bc-6a70-4c4d-b645-931570c9f6b1)
openstack_compute_instance_v2.terraform-test-01: Still destroying... (ID: f4d135bc-6a70-4c4d-b645-931570c9f6b1, 10s elapsed)
openstack_compute_instance_v2.terraform-test-01: Destruction complete after 11s
```

## HCL

Terraform 설정 파일은 HCL(HashiCorp Configuration Language)을 사용합니다. HCL은 Terraform 형식(`.tf`)과 JSON 형식(`.tf.json`)을 사용합니다. 손으로 작성할 때는 Terraform 형식이 더 편리합니다.

지정한 폴더에 `.tf`, `.tf.json`을 넣어두면 TerraForm이 알파벳 순서로 로드합니다. 변수나 리소스의 정의 순서는 상관이 없습니다.

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

## References
Terraform Documentation - [https://www.terraform.io/docs/providers/index.html](https://www.terraform.io/docs/providers/index.html)
