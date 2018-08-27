## Compute > Instance > 第三方使用指南 > Terraform使用指南

本文档介绍了在TOAST环境中如何通过Terraform管理实例。

## Terraform
Terraform是一个开源工具，可以轻松构建，安全地更改基础架构，并有效的进行基础设施的管理。其主要功能包括：

* **Infrastructure as Code**
    * 可定义基础设施的代码，提高生产力和透明度。
    * 可轻松共享定义的代码，并有效协作。
* **Execution Plan**
    * 通过更新计划和更新应用的分离，可减少更新时可能发生的错误。
* **Resource Graph**
    * 可提前预知微小的更改操作对整个基础设施产生的影响。
    * 可创建依赖关系的拓扑图，并根据此依赖关系制定计划，同时可查看执行计划过程中，基础设施的变化情况。
* **Change Automation**
    * 可在多个位置自动部署，并可以批量更改相同配置的基础设施。
    * 可节省部署基础设施所消耗的时间，也可减少在此过程中容易出现的错误。

Terraform支持当前主流提供商的几乎所有解决方案。

* AWS, BareMetal, Bitbucket, Chef, Cloudflare, Docker, GitHub, Google Cloud, Grafana, InfluxDB, Heroku, Microsoft Azure, MySQL, OpenStack, PostgreSQL 등


## 安装Terraform
从[Terraform下载页面](https://www.terraform.io/downloads.html)下载本地电脑操作系统所支持的文件。解压文件，将其放在目标路径中，然后将该路径添加到环境变量中完成安装。

以下是安装示例。

```
$ wget https://releases.hashicorp.com/terraform/0.11.1/terraform_0.11.1_linux_amd64.zip
$ unzip terraform_0.11.1_linux_amd64.zip
$ export PATH="${PATH}:$(pwd)"
$ terraform - v
Terraform v0.10.5

Your version of Terraform is out of date! The latest version
is 0.11.1. You can update by downloading from www.terraform.io
```

> [参考]
> 在此示例中，因路径是通过使用`export`命令设置的，因此，如果关闭终端，路径也将随之消失。
> 如果在`.bashrc`或`.bash_profile`等用户配置文件中设置路径，则该路径可永久有效。

如果您在无任何参数的情况下运行Terraform，可以查看其简单用法。

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

## 在TOAST环境中使用

我们来看一下如何在TOAST环境中使用TerraForm创建、添加、更改和删除实例。

> [参考]
> 以下示例中的所有数据都不是真实数据。请务必更改为正确的数据。

### Terraform初始化

在使用Terraform之前，必须按照以下方式部署提供商的配置文件。

```
$ vi provider.tf
# Configure the OpenStack Provider
provider "openstack" {
  user_name   = "terraform-guide@nhnent.com"
  tenant_id   = "75a4c0a12fd84edeb68965d320d17129"
  password    = "kGzBDD9psmgeH4ji"
  auth_url    = "https://api-gw.cloud.toast.com/terraform/identity/v2.0"
  region      = "RegionOne"
}
```

* **provider**
    * 必须指定提供商名称。
    * 由于TOAST是使用OpenStack构建的，因此提供商名应为**openstack**。
* **user_name**
    * 使用TOAST ID。
* **tenant_id**
    * 在TOAST控制台的**Compute > Instance > Management**菜单中，单击**API Endpoint设置**按钮，可查看客户ID。
* **password**
    * 使用[设置API Endpoint]窗口中保存的API密码。
* **auth_url**
    * 使用Terraform需要auth_url，获取auth_url请联系客服咨询。
* **region**
    * 韩国地区使用**RegionOne**。

> [参考]
> 设置API密码请参考API指南的[令牌API](/Compute/Instance/zh/api-guide/#api)项。


请在提供商配置文件的部署路径下，使用`init`命令对Terraform进行初始化。

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

### 创建实例

如果要创建实例，您必须按照以下方式，在.tf文件中输入要创建的实例信息。

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
    * 由资源类型和资源名称组成。
    * TOAST使用OpenStack搭建，故，其资源类型应为**openstack_compute_instance_v2**。
    * 资源名称即为要创建的实例名称。
* **name**
    * 要创建的实例的名称。
* **region**
    * 必须与提供商配置文件中的内容一致。
* **flavor_id**
    * 要创建的实例的规格ID。
    * 可以通过TOAST提供的开源API中的[实例规格条目查询API](/Compute/Instance/zh/api-guide/#_18)查询。
* **key_pair**
    * 是指用于访问实例的密钥对名称。
    * 您可以在TOAST控制台的**Compute > Instance > Key Pair**菜单中，新建或注册现有的密钥对。详细说明请参考控制台使用指南之[密钥对](/Compute/Instance/zh/console-guide/#_7)。    
* **network**
    * 输入要连接到实例的VPC名称和uuid。
    * 在TOAST控制台的**Network > VPC > Management**菜单中选择要连接的VPC之后，可在下方详情页面查看到名称与uuid。
* **security_groups**
    * 是指用于实例的安全组的名称。
    * 可使用逗号(,)，指定1个以上的安全组。
    * 在TOAST控制台的**Network > VPC > Security Groups**菜单中选择要使用的安全组，可在下方详情页面中查看到信息。
* **block_device**
    * 设置要在实例中使用的镜像或块存储信息以及磁盘容量信息。
    * uuid
        * 在TOAST控制台的**Compute > Images**菜单中选择要使用的镜像，可在下方详情页面中查看到信息。
    * source_type
        * 如果使用镜像创建实例，此时source_type为**image**。
    * destination_type
        * 如果将块设备作为实例磁盘使用，则destination_type为**volume**。
    * boot_index
        * 如果使用块设备作为实例的引导盘使用，则boot index为**0**。
    * volume_size
        * 为将要创建的实例设置磁盘容量。
        * 最小值可设置为20GB，最大值可设置为1,000GB。
        * 可根据实例配置，设置不同的容量。详细说明请参考控制台使用指南之[创建实例 > 规格](/Compute/Instance/zh/console-guide/#_4)。
    * delete_on_termination
        * 如果将此选项设置为true，则删除实例时块设备也将一并被删除。


如果在.tf文件所在的路径下执行`plan`命令，Terraform就会加载.tf文件，并确认其设置是否正确。同时还将与自己的数据库进行对比，并创建计划。 待计划创建完毕后会按照类型进行汇总并以较直观的方式显示出来。

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


执行`apply`命令后会按照计划创建实例。并生成自己的DB文件(terraform.tfstate)，以此记录计划的更改历史。

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

您可以在TOAST控制台的**Compute > Instance > Management**菜单中找到已创建的实例。

### 添加实例

与创建实例方法相同。即，先创建.tf文件后发布计划即可。可同时创建多个.tf文件一并使用。

### 更改实例

打开要更改的实例.tf文件，修改目标信息，并重新应用计划。

可更改的配置有限。您可以添加一个新磁盘，或删除/替换连接到实例的安全组和VPC。如果更改引导盘的容量，现有实例将会被删除，同时会生成一个新实例。实例配置只能在实例关闭后更改。

以下为新增一个安全组的事例。

```
$ vi terraform-instance-01.tf
resource "openstack_compute_instance_v2" "terraform-instance-01" {
  ...
  security_groups = ["default", "terraform-sg"]
  ...
}
```

加载计划时，会显示已更改的安全组信息。

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

执行计划时，会向实例添加新的安全组。

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

### 删除实例

如果删除在创建实例过程中所使用的.tf文件，再执行计划，则该实例将被删除。

如果资源被删除，加载计划时页面上就会显示存在1个已删除的计划。

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

执行计划时，将删除该实例。

```
$ terraform apply
openstack_compute_instance_v2.terraform-test-01: Refreshing state... (ID: f4d135bc-6a70-4c4d-b645-931570c9f6b1)
openstack_compute_instance_v2.terraform-test-01: Destroying... (ID: f4d135bc-6a70-4c4d-b645-931570c9f6b1)
openstack_compute_instance_v2.terraform-test-01: Still destroying... (ID: f4d135bc-6a70-4c4d-b645-931570c9f6b1, 10s elapsed)
openstack_compute_instance_v2.terraform-test-01: Destruction complete after 11s
```

## HCL

Terraform配置文件使用HCL(HashiCorp Configuration Language)。HCL使用Terraform格式(`.tf`)和JSON格式(`tf.json`)。

如果在指定的文件夹中保存`.tf`, `.tf.json`，则TerraForm会按字母顺序加载。变量或资源的定义顺序无关紧要。

您可以使用覆盖文件将其它设置覆盖。文件名以`override`或者`_override`结尾即可。在完成其它配置文件加载时，覆盖文件将按照字母顺序依次进行覆盖。

### HCL语法

* **注释**

可以使用**#**, **//**, **/\* \*/**。

* **分配值**

使用**key = value**格式。值可以是字符串、数字、布尔值、列表或地图。

* **字符串**

使用双引号。当使用多行字符串时，必须以[Unix shell的Here document](https://en.wikipedia.org/wiki/Here_document)格式在`<<EOF`, `EOF`中间插入字符串。

```
description = <<EOF
... 字符串字符串字符串字符串...
... 字符串字符串字符串字符串...
...字符串字符串字符串字符串...
EOF
```

* **资源**

声明资源时，请使用**resource**关键字，并根据不同的提供商指定由Terraform定义的不同资源类型。

```
resource "openstack_compute_instance_v2" "web" {
    ...
}
```

* **提供商**

声明提供商时，请使用**provider**关键字。声明资源时，指定的资源类型前缀即为提供商类型。

```
# 资源类型: openstack_compute_instance_v2
provider "openstack" {
    ...
}

# 资源类型: aws_instance
provider "aws" {
    ...
}
```

* **数据来源**

从提供商处获取的数据称之为数据来源。它使用**data**关键字，由类型(type)和名称(name)组成。

```
data "type" "name" {
    ...
}
```

* **变量**

声明变量时，请使用**variable**关键字。因为会自行推断格式，故无需定义。

```
variable "name" {
    type = "type"
    default = {
        key = value
    }
    description = "description"
}
```

* **模块**

使用**module**关键字，可以将之前定义的资源组导入模块中使用。支持GitHub, Bitbucket等。

```
module "name" {
    source = "url"
    ...
}
```

## 参考网站
Terraform Documentation - [https://www.terraform.io/docs/providers/index.html](https://www.terraform.io/docs/providers/index.html)
