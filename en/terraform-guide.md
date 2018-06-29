## Compute > Instance > Third party Guide > Terraform Guide

This document describes how to manage instances for TOAST services by using Terraform .

## Terraform
Terraform is an open-source tool which can easily build, safely change, and efficiently manage configuration of infrastructure. Main features are as below: 

* **Infrastructure as Code**
    * Elevate productivity and transparency by defining infrastructure in codes. 
    * Easily share defined codes so as to collaborate more efficiently. 
* **Execution Plan**
    * Separate change plan from application and minimize mistakes that may occur when change is applied. 
* **Resource Graph**
    * Predict how a minor change affects the whole infrastructure in advance. 
    * Create a dependency graph to plan and check status of infrastructure changes when this plan applies. 
* **Change Automation**
    * Automation is available to build and change infrastructure of same configuration in many locations. 
    * Save time to build infrastructure, while minimizing mistakes.  

Terraform supports almost all solutions of major providers: 

* AWS, BareMetal, Bitbucket, Chef, Cloudflare, Docker, GitHub, Google Cloud, Grafana, InfluxDB, Heroku, Microsoft Azure, MySQL, OpenStack, PostgreSQL, and more


## Install Terraform 
Download files for the operating system on your local PC from [Download Terraform](https://www.terraform.io/downloads.html). Decompress and put them in the route you choose and add the route to the environment setting, and it is done. 

Here is an example for installation: 

```
$ wget https://releases.hashicorp.com/terraform/0.11.1/terraform_0.11.1_linux_amd64.zip
$ unzip terraform_0.11.1_linux_amd64.zip
$ export PATH="${PATH}:$(pwd)"
$ terraform - v
Terraform v0.10.5

Your version of Terraform is out of date! The latest version
is 0.11.1. You can update by downloading from www.terraform.io
```

> [Note]
> The route for this example has been set with `export`  command, and the route is gone if the terminal is closed. 
> If routes are set by user profiles, such as `.bashrc` or `.bash_profile`, usage becomes eternal. 

By executing Terraform without any parameters, simple usage is available. 

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

## Usage for TOAST Environment  

Here's how to create, add, and remove instances by using Terraform in the TOAST environment with examples. 

> [Note]
> All data of below examples are not real: replace them with correct data for your own usage. 

### Initialize Terraform  

Before using Terraform, provider files must be configured as below: 

```
$ vi provider.tf
# Configure the OpenStack Provider
provider "openstack" {
  user_name   = "terraform-guide@nhnent.com"
  tenant_id   = "75a4c0a12fd84edeb68965d320d17129"
  password    = "kGzBDD9psmgeH4ji"
  auth_url    = "https://api-gw.cloud.toast.com/infrastructure/identity/v2.0"
  region      = "RegionOne"
}
```

* **provider**
    * Specify the provider's name. 
    * The provider of TOAST is named **_openstack_** as it is built on OpenStack. 
* **user_name**
    * Use  **User Access Key ID**(or TOAST account ID) which can be issued from **API Security Setting**.
* **tenant_id**
    * Click **API Endpoint Setting** from **_Compute > Instance > Management_** in the TOAST console to check tenant ID. 
* **password**
    * Use **Secret Access Key** which can be issued from **API Security Setting** 
* **auth_url**
    * The auth_url is ``.
* **region**
    * Korea uses **RegionOne**.

> [Note]
> To issue User Access Key ID and Secret Access Key, refer to [Token API](/Compute/Instance/en/api-guide/#api) in the API preparation guide. 


Command `init`  to initialize Terraform from the route that has configured supplier setting file.  

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

### Create Instances 

To create an instance, data input is required to the instance to be created in the .tf file. 

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
    * Configure the type and name of a resource.
    * As TOAST is built on OpenStack, the resource type is  **openstack_compute_instance_v2**. 
    * The resource name is the name of the instance to create. 
* **name**
    * Name of an instance to create. 
* **region**
    * Must coincide with what is described in provider setting file 
* **flavor_id**
    * Flavors ID of an instance to create.
    * Can be retrieved through [Retrieve List of Instance Flavors API](/Compute/Instance/en/api-guide/#_18) among open APIs provided by TOAST
* **key_pair**
    * Name of a key pair applied to access instance. 
    * You can newly create or register your own key pairs in  **_Compute > Instance > Key Pair_** in the TOAST console. Refer to  [Key Pair](/Compute/Instance/en/console-guide/#_7) in the console user guide. 
* **network**
    * Enter VPC name and uuid to be connected to an instance. 
    * Select VPC to connect in **_Network > VPC > Management_**, and check the name and uuid at the bottom of details. 
* **security_groups**
    * Name of a security group to be applied for an instance. 
    * Specify more than one security groups delimited by comma (,). 
    * Select a security group to use in **_Network > VPC > Security Groups_**, and check information at the bottom of details. 
* **block_device**
    *  Set image or block storage information and disk volume to be applied for an instance. 
    *  uuid
        * Select an image to use in **_Compute > Images_** and check information at the bottom of details. 
    *  source_type
        * If an instance is created with image, the source_type is **image**.
    *  destination_type
        * If block device is used as an instance disk, the destination_type is **volume**.
    *  boot_index
        * If block device is used as an instance boot disk, the boot index is **0**. 
    *  volume_size
        * Set disk volume to use for an instance to be created. 
        * Setting is available between 20GB and 1,000GB. 
        * Volume setting depends on instance flavors: refer to [Create Instances > Flavors](/Compute/Instance/en/console-guide/#_4) in the console user guide.  
    *  delete_on_termination
        * If this option is set true, block device shall be deleted along with an instance destruction. 


Terraform, when `plan` is run on the route containing .tf files, loads the .tf files to see if setting is right and creates a plan in comparison of its own DB. The plan, when completed, is sorted out by type and printed in good display.   

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


When `apply` is executed, apply the plan to create an instance. And, its own Db file (terraform.tfstate) to record history of plan changes is to be created. 

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

To check created instances, go to **_Compute > Instance > Management_** in the TOAST console. 

### Create More Instances 

Creating more instances follows the same method of instance creation, like creating .tf files and applying the plan. Many .tf files can be created and applied all at once. 

### Change Instances

Open the .tf files of which instances need to change, and modify information and apply the plan. 

Change of flavors is limited: add new disks, remove or replace security groups and VPCs connected to an instance. By changing volume of a boot disk, a new instance is created in place of an existing instance. 

Example as below describes adding one more security group. 

```
$ vi terraform-instance-01.tf
resource "openstack_compute_instance_v2" "terraform-instance-01" {
  ...
  security_groups = ["default", "terraform-sg"]
  ...
}
```

Load the plan, and information of changed security group is organized and printed. 

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

Apply the plan, and a new security group is added to the instance. 

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

### Destroy Instances 

Delete the .tf files used to create instances and apply plan, and the instance is destroyed. 

Load the plan and it shows one plan is destroyed as resource setting has been destroyed. 

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

Apply the plan and instance is destroyed. 

```
$ terraform apply
openstack_compute_instance_v2.terraform-test-01: Refreshing state... (ID: f4d135bc-6a70-4c4d-b645-931570c9f6b1)
openstack_compute_instance_v2.terraform-test-01: Destroying... (ID: f4d135bc-6a70-4c4d-b645-931570c9f6b1)
openstack_compute_instance_v2.terraform-test-01: Still destroying... (ID: f4d135bc-6a70-4c4d-b645-931570c9f6b1, 10s elapsed)
openstack_compute_instance_v2.terraform-test-01: Destruction complete after 11s
```

## HCL

For Terraform configuration, use HashiCorp Configuration Language, or HCL, which uses Terraform format (`.tf`) and JSON format (`.tf.json`).

Put  `.tf`, `.tf.json` to a specified folder, and TerraForm loads files in the alphabetical order: definition order of variables or resources does not count. 

You can also use Overrides, which is to override other settings: name a file ending with   `override` or `_override`. Override files are loaded last in the alphabetical order after other files are loaded, to override settings.  

### HCL Grammar 

* **Footnotes**  

**#**, **//, and** **/\* \*/** are available.

* **Value Allocation**  

The **key = value** format is applied. 
Character strings, numbers, boolean, lists, and maps are all available. 

* **Character String** 

Use double quotes. To use many lines of character strings, put a string between  `<<EOF`and `EOF` , in the format of [Here document of Unix Shell](https://en.wikipedia.org/wiki/Here_document). 

```
description = <<EOF
...CharacterStringCharacterStringCharacterString...
...CharacterStringCharacterStringCharacterString...
...CharacterStringCharacterStringCharacterString...
EOF
```

* **Resource** 

To declare resources, use **resource**: the format, defined by Terraform, needs to be specified depending on the provider. 

```
resource "openstack_compute_instance_v2" "web" {
    ...
}
```

* **Provider**

To declare providers, use **provider**: the prefix of resource format that is specified in resource declaration refers to the provider format.  

```
# Resource Format: openstack_compute_instance_v2
provider "openstack" {
    ...
}

# Resource Format: aws_instance
provider "aws" {
    ...
}
```

* **Data Source**

Data Source refers to such data that is to be imported from provider: comprised of type and name, and the keyword is **data**.

```
data "type" "name" {
    ...
}
```

* **Variable**

To declare variables, use **variable** as the keyword: definition is not required as the format can be inferred. 

```
variable "name" {
    type = "type"
    default = {
        key = value
    }
    description = "description"
}
```

* **Module**

With **module**, resource groups that were defined previously can be imported to module: supports GitHub and Bitbucket. 

```
module "name" {
    source = "url"
    ...
}
```

## References
Terraform Documentation - [https://www.terraform.io/docs/providers/index.html](https://www.terraform.io/docs/providers/index.html)
