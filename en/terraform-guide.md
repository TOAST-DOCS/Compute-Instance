## Third Party User Guide > Terraform User Guide
This document describes how to use NHN Cloud with Terraform.

## Terraform
Terraform is an open-source tool that lets you easily build and safely change infrastructure, and also efficiently manage configuration of infrastructure. The main features of Terraform are as follows:

* **Infrastructure as Code**
    * You can increase the productivity and transparency by defining infrastructure as code.
    * You can easily share the defined code for efficient collaboration.
* **Execution Plan**
    * By separating change planning and change execution, you can reduce the potential for mistakes when making changes.
* **Resource Graph**
    * You can see in advance how minor changes will affect the entire infrastructure.
    * By creating a dependency graph, you can make a plan based on the graph and see how your infrastructure changes when you apply the plan.
* **Change Automation**
    * You can automate the process so that infrastructure with the same configuration can be built and changed in multiple locations.
    * You can save time to build infrastructure and reduce mistakes.


#### Supported Resources

* Compute
    * nhncloud_compute_instance_v2
    * nhncloud_compute_volume_attach_v2
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
* Storage
    * nhncloud_blockstorage_volume_v2

#### Supported Data Sources

* nhncloud_images_image_v2
* nhncloud_blockstorage_volume_v2
* nhncloud_compute_flavor_v2
* nhncloud_blockstorage_snapshot_v2
* nhncloud_networking_vpc_v2
* nhncloud_networking_vpcsubnet_v2

### Note

* **The version of the Terraform used in the examples below is 1.0.0.**
* **The name and number of the components including the version can be changed, so make sure you check the information before use.**

## Terraform Installation
Go to [Download Terraform](https://www.terraform.io/downloads.html) and download the file suitable for the operating system of your local PC. Decompress the file to an appropriate path and add the path to your environment setting, and the installation is complete.

See the following example for installation.

```
$ wget https://releases.hashicorp.com/terraform/1.0.0/terraform_1.0.0_linux_amd64.zip
$ unzip terraform_1.0.0_linux_amd64.zip
$ export PATH="${PATH}:$(pwd)"
$ terraform -v
Terraform v1.0.0
```

## Terraform NHN Cloud provider provided

Terraform NHN Cloud provider provides the following **operating system/architecture** compatibility, and you can download the binary file through the link.
Terraform NHN Cloud provider version currently provided is **1.0.0**.

* [macOS / AMD64](https://static.toastoven.net/prod_cloud_terraform_provider/darwin_amd64/terraform-provider-nhncloud_v1.0.0)
* [macOS / Apple silicon](https://static.toastoven.net/prod_cloud_terraform_provider/darwin_arm64/terraform-provider-nhncloud_v1.0.0)
* [Linux / AMD64](https://static.toastoven.net/prod_cloud_terraform_provider/linux_amd64/terraform-provider-nhncloud_v1.0.0)
* [Windows / AMD64](https://static.toastoven.net/prod_cloud_terraform_provider/windows_amd64/terraform-provider-nhncloud_v1.0.0)

### Local provider setup

Terraform NHN Cloud provider can be used through local provider setting.

After creating a directory structure to find a local provider, add the downloaded binary file to the plugin path. The binary file must have execute permissions.

The following is the default plugin path according to the operating system. For more information on default routes, see `Implied Local Mirror Directories` on 
the [Terraform site](https://developer.hashicorp.com/terraform/cli/config/config-file#provider-installation).

* **Linux / macOS** : `${HOME}/.terraform.d/plugins/terraform.local/local/nhncloud/${version}/${platforms}`
* **Windows** : `%APPDATA%/terraform.d/plugins/terraform.local/local/nhncloud/${version}/${platforms}`

Describes path configuration rules for default plugin path.

* **version**
    * The version of the provider.
* **platforms**
    * An array of objects describing the platform containing the package, consisting of an operating system identifying keyword and a CPU architecture identifying keyword.
    * **darwin_adm64** : macOS / AMD64
    * **darwin_arm64** : macOS / Apple silicon
    * **linux_amd64** : Linux / AMD64
    * **windows_amd64** : Windows / AMD64

The following is an example of plugin setting based on **operating system/architecture** after binary download. 

**It is recommended to use version 1.0.0 when configuring the plugin.**

This is an example of `macOS / AMD64` plugin settings.

```
$ mkdir -p $HOME/.terraform.d/plugins/terraform.local/local/nhncloud/1.0.0/darwin_amd64
$ cp terraform-provider-nhncloud_v1.0.0 $HOME/.terraform.d/plugins/terraform.local/local/nhncloud/1.0.0/darwin_amd64
$ chmod +x $HOME/.terraform.d/plugins/terraform.local/local/nhncloud/1.0.0/darwin_amd64/terraform-provider-nhncloud_v1.0.0
```

This is an example of `macOS / Apple silicon` plugin settings.

```
$ mkdir -p $HOME/.terraform.d/plugins/terraform.local/local/nhncloud/1.0.0/darwin_arm64
$ cp terraform-provider-nhncloud_v1.0.0 $HOME/.terraform.d/plugins/terraform.local/local/nhncloud/1.0.0/darwin_arm64
$ chmod +x $HOME/.terraform.d/plugins/terraform.local/local/nhncloud/1.0.0/darwin_arm64/terraform-provider-nhncloud_v1.0.0
```

This is an example of `Linux / AMD64` plugin settings.

```
$ mkdir -p $HOME/.terraform.d/plugins/terraform.local/local/nhncloud/1.0.0/linux_amd64
$ cp terraform-provider-nhncloud_v1.0.0 $HOME/.terraform.d/plugins/terraform.local/local/nhncloud/1.0.0/linux_amd64
$ chmod +x $HOME/.terraform.d/plugins/terraform.local/local/nhncloud/1.0.0/linux_amd64/terraform-provider-nhncloud_v1.0.0
```

This is an example of `Windows / AMD64` plugin settings.

```
$ mkdir -p %APPDATA%/terraform.d/plugins/terraform.local/local/nhncloud/1.0.0/windows_amd64
$ copy terraform-provider-nhncloud_v1.0.0 %APPDATA%/terraform.d/plugins/terraform.local/local/nhncloud/1.0.0/windows_amd64
```

## Terraform Initialization
Before using Terraform, create a provider configuration file as follows.

The name of the provider file can be set randomly. This example uses `provider.tf` as the filename.

```
# Define required providers
terraform {
required_version = ">= 1.0.0"
  required_providers {
    nhncloud = {
      source  = "terraform.local/local/nhncloud"
      version = "1.0.0"
    }
  }
}

# Configure the nhncloud Provider
provider "nhncloud" {
  user_name   = "terraform-guide@nhncloud.com"
  tenant_id   = "aaa4c0a12fd84edeb68965d320d17129"
  password    = "difficultpassword"
  auth_url    = "https://api-identity-infrastructure.nhncloudservice.com/v2.0"
  region      = "KR1"
}
```
* **user_name**
    * Use the NHN Cloud ID.
* **tenant_id**
    * From **Compute > Instance > Management** on NHN Cloud console, click **Set API Endpoint** to check the Tenant ID.
* **password**
    * Use **API Password** that you saved in **Set API Endpoint**.
    * Regarding how to set API passwords, see **User Guide > Compute > Instance > API Preparations**.
* **auth_url**
    * Specify the address of the NHN Cloud identification service.
    * From **Compute > Instance > Management** on NHN Cloud console, click **Set API Endpoint** to check Identity URL.
* **region**
    * Enter the region to manage NHN Cloud resources.
    * **KR1**: Korea (Pangyo) Region
    * **KR2**: Korea (Pyeongchon) Region
    * **JP1**: Japan (Tokyo) Region

On the path where the provider configuration file is located, use the `init` command to initialize Terraform.

```
$ ls
provider.tf
$ terraform init
```

## Terraform Usage

Building infrastructure with Terraform has the following life cycle:

1. Create tf Files
2. Check the Execution plan
3. Create Resources
4. Modify Resources
5. Delete Resources

First, write the configuration of infrastructure to build on a tf file. To check the execution plan according to the tf file, use the `plan` command like below.

```
$ terraform plan
```

If the execution plan is confirmed, use the `apply` command to create, modify, or delete resources.

```
$ terraform apply
```

The following sections describe these steps in more details with examples.

### Create tf Files

Create a tf file in the path including the provider configuration file. You can aggregate multiple resource settings in a single tf file, or create separate tf files for each resource. Terraform reads the entire tf files all at once to set up an execution plan.

The following example shows an `instance.tf` file that defines a resource creating an instance.

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

### Check the Execution plan

You can use the `plan` command to check resources that will be changed in tf files. When you run the `plan` command, Terraform loads .tf files, checks if the configuration is correct, and compares the configuration with its own database and create a plan. When it finishes creating the plan, Terraform aggregates the plan by type and prints out the result in an organized form.

```
$ terraform plan
```

If the created plan requires modification, correct tf files and execute the `plan` command again. The `plan` command does not change the actual NHN Cloud resources, so you can check the infrastructure changes without any burden.

### Create Resources

After creating tf files with the plan you need, create resources with the `apply` command.

The following example shows the result of executing the `apply` command using the `instance.tf` file created above.

```
$ terraform apply
...
nhncloud_compute_instance_v2.terraform-instance-01: Creating...
nhncloud_compute_instance_v2.terraform-instance-01: Still creating... [10s elapsed]
nhncloud_compute_instance_v2.terraform-instance-01: Still creating... [20s elapsed]
nhncloud_compute_instance_v2.terraform-instance-01: Still creating... [30s elapsed]
nhncloud_compute_instance_v2.terraform-instance-01: Creation complete after 39s [id=1e846787-04e9-4701-957c-78001b4b7257]

Apply complete! Resources: 1 added, 0 changed, 0 destroyed.
```

After the `apply` command is executed, Terraform's own database file (terraform.tfstate) that records the history of plan changes is created in the current directory. Be careful not to delete this file.

### Modify Resources

Open the `.tf` file in which resources to change are defined, modify information, and apply the plan. Specification changes are restricted only to some attributes. If there is a change in an attribute which cannot be changed, the resource is deleted and newly created.

The following shows an example of adding one more `terraform-sg` security group to an instance, where the `instance.tf` file created above is modified as follows.

```
resource "nhncloud_compute_instance_v2" "terraform-instance-01" {
  ...
  security_groups = ["default", "terraform-sg"]
  ...
}
```

Check the execution plan, and the changed security group information is printed out in an organized form.

```
$ terraform plan
...
Terraform used the selected providers to generate the following execution plan. Resource actions are indicated with the following symbols:
  ~ update in-place

Terraform will perform the following actions:

  # nhncloud_compute_instance_v2.terraform-instance-01 will be updated in-place
  ~ resource "nhncloud_compute_instance_v2" "terraform-instance-01" {
        id                  = "1e846787-04e9-4701-957c-78001b4b7257"
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

When you apply the plan, a new security group is added to the instance.
```
$ terraform apply
...
nhncloud_compute_instance_v2.terraform-instance-01: Modifying... [id=1e846787-04e9-4701-957c-78001b4b7257]
nhncloud_compute_instance_v2.terraform-instance-01: Modifications complete after 5s [id=1e846787-04e9-4701-957c-78001b4b7257]

Apply complete! Resources: 0 added, 1 changed, 0 destroyed.
```

### Delete Resources

To delete a resource created with Terraform, delete the corresponding `.tf`  file.

```
$ rm instance.tf
```

In the execution plan, you can see that the resources will be deleted.

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

Running the `apply` command deletes the created instance.

```
$ terraform apply
...
nhncloud_compute_instance_v2.terraform-instance-01: Destroying... [id=1e846787-04e9-4701-957c-78001b4b7257]
nhncloud_compute_instance_v2.terraform-instance-01: Still destroying... [id=1e846787-04e9-4701-957c-78001b4b7257, 10s elapsed]
nhncloud_compute_instance_v2.terraform-instance-01: Destruction complete after 11s

Apply complete! Resources: 0 added, 0 changed, 1 destroyed.
```

## Data Sources

You can find Flavor ID or Image ID required to create tf files on the console, or import them by using data sources provided by Terraform. Data sources must be written within tf files, and imported data cannot be modified but are used only for reference. Image names are subject to change because NHN Cloud updates them on a regular basis. Refer to console and specify the exact name of the image to use.

Data sources are referenced as `{data sources type}.{data source name}`. In the below example, the imported image information is referenced with `nhncloud_images_image_v2.ubuntu_2004_20201222`.

```
data "nhncloud_images_image_v2" "ubuntu_2004_20201222" {
  name = "Ubuntu Server 20.04.1 LTS (2020.12.22)"
  most_recent = true
}
```

It is possible to reference another data source within data sources.

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

The following sections describe how to import various resources provided by NHN Cloud by using the data resources feature.

### Image

Imports image information. NHN Cloud's public images as well as private images are supported.

```
data "nhncloud_images_image_v2" "ubuntu_2004_20201222" {
  name = "Ubuntu Server 20.04.1 LTS (2020.12.22)"
  most_recent = true
}

# Query the oldest image from images with the same name
data "nhncloud_images_image_v2" "windows2016_20200218" {
  name = "Windows 2019 STD with MS-SQL 2019 Standard (2020.12.22) KO"
  sort_key = "created_at"
  sort_direction = "asc"
  owner = "c289b99209ca4e189095cdecebbd092d"
  tag = "_AVAILABLE_"
}
```
| Name | Format | Required | Description |
| ------ | ---- | ---- | --------- |
| name | String | - | Name of image to query<br>To check the image name, go to **Compute > Instance** on NHN Cloud console and click **Create Instance**. The information can be found on the list of images provided by NHN Cloud.<br>The image name must be entered as **<Image Description>** which shows up on NHN Cloud console.<br>If the Language item exists, follow the format **"<Image Description> <Language>"** like the above example. |
| size_min | Integer | - | Minimum size of image to query (bytes) |
| size_max | Integer | - | Maximum size of image to query (bytes) |
| properties | Object | - | Attributes of image to query<br>Images that match all attributes are queried. |
| sort_key | String | - | Sort the list of images queried by particular attributes <br>The default is `name` |
| sort_direction | String | - | Sorting order of the list of queried images <br>`asc`: Ascending order (Default) <br>`desc`: Descending order |
| owner | String | - | ID of tenant which includes the image to query |
| tag | String | - | Search images with a particular tag |
| visibility | String | - | Visibility of image to query <br>Select only one among public, private, and shared <br>If omitted, the list with all types of images is returned |
| most_recent | Boolean | - | `true`: Select the most recently created image from the list of queried images <br>`false`: Select images in the queried order |
| member_status | String | - | Status of image member to query <br>One among `accepted`,`pending`, `rejected`, and `all` |

### Block Storage

```
data "nhncloud_blockstorage_volume_v2" "volume_00" {
  name = "ssd_volume1"
  status = "available"
}
```

| Name | Format | Required | Description |
| ------ | ---- | ---- | --------- |
| name | String | - | Name of block storage to query |
| status | String | - | Status of block storage to query |
| metadata | Object | - | Metadata related to block storage to query |

### Instance Flavor

To check name of a flavor, go to **Compute > Instance** on NHN Cloud console and click **Create Instance > Select Flavor**.

```
data "nhncloud_compute_flavor_v2" "u2c2m4"{
  name = "u2.c2m4"
}
```

| Name | Format | Required | Description |
| ------ | ---- | ---- | --------- |
| name | String | - | Name of flavor to query |


### Snapshot

```
data "nhncloud_blockstorage_snapshot_v2" "my_snapshot" {
  name = "my-snapshot"
  volume_id = data.nhncloud_blockstorage_volume_v2.volume_00.id
  status = "available"
  most_recent = true
}
```

| Name | Format | Required | Description |
| ------ | ---- | ---- | --------- |
| name | String | - | Name of snapshot to query |
| volume_id | String | - | ID of original block storage of snapshot to query |
| status | String | - | Status of snapshot to query |
| most_recent | Boolean | - | `true`: Select the most recently created snapshot from the queried snapshot list <br>`false`: Select snapshots in the queried order |


### VPC

To check UUID of VPC network, go to NHN Cloud console and select VPC from **Network > VPC**.

```
data "nhncloud_networking_vpc_v2" "default_network" {
  region = "KR1"
  tenant_id = "ba3be1254ab141bcaef674e74630a31f"
  id = "e34fc878-89f6-4d17-a039-3830a0b78346"
  name = "Default Network"
}
```

| Name | Format | Required | Description |
| ------ | ---- | ---- | --------- |
| region     | String | - | Region name that VPC to query belongs to |
| tenant\_id | String | - | Tenant ID that VPC to query belongs to |
| id | String | - | VPC ID to query |
| name | String | - | VPC name to query |

### VPC Subnet

To check subnet ID, go to NHN Cloud console and select a subnet from **Network > VPC > Subnet**.

```
data "nhncloud_networking_vpcsubnet_v2" "default_subnet" {
  region = "KR1"
  tenant_id = "ba3be1254ab141bcaef674e74630a31f"
  id = "05f6fdc3-641f-48df-b986-773b6489654f"
  name = "Default Network"
  shared = true
}
```

| Name | Type | Required | Description         |
| --- | --- |---|------------|
| region | String | - | Region name that subnet to query belongs to |
| tenant\_id | String | - | Tenant ID that subnet to query belongs to |
| id | String | - | Subnet ID to query |
| name | String | - | Subnet name to query |
| shared | Bool | - | Whether to share subnet to query |

## Resources

You can create, modify, or delete resources with Terraform resources. NHN Cloud supports management of the following resources using Terraform:

* Instance
* Block storage
* VPC
* Floating IP
* Network port
* Load balancer

The following sections describe how to use each resource.

## Resources - Instance

### Create Instance

```
# Create u2 Instance
resource "nhncloud_compute_instance_v2" "tf_instance_01"{
  name = "tf_instance_01"
  region    = "KR1"
  key_pair  = "terraform-keypair"
  image_id = data.nhncloud_images_image_v2.ubuntu_2004_20201222.id
  flavor_id = data.nhncloud_compute_flavor_v2.u2c2m4.id
  security_groups = ["default"]
  availability_zone = "kr-pub-a"

  network {
    name = data.nhncloud_networking_vpc_v2.default_network.name
    uuid = data.nhncloud_networking_vpc_v2.default_network.id
  }

  block_device {
    uuid = data.nhncloud_images_image_v2.ubuntu_2004_20201222.id
    source_type = "image"
    destination_type = "local"
    boot_index = 0
    delete_on_termination = true
    volume_size = 30
  }
}

# Flavors other than u2
# Create instance with network and block storage added
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
    uuid                  = data.nhncloud_images_image_v2.ubuntu_2004_20201222.id
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
| Name    | Format | Required  | Description                                                                                                                                                                                           |
| ------ | ---- | ---- |----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| name | String | O | Name of instance to create                                                                                                                                                                                 |
| region | String | - | Region of instance to create<br>The default is the region configured in provider.tf                                                                                                                                                     |
| flavor_name | String | - | Flavor name of instance to create<br>Required if flavor_id is empty                                                                                                                                                |
| flavor_id | String | - | Flavor ID of instance to create<br>Required if flavor_name is empty                                                                                                                                              |
| image_name | String | - | Image name to use for creating an instance<br>Required if image_id is empty<br>Available only when the flavor is U2                                                                                                                        |
| image_id | String | - | Image ID to use for creating an instance<br>Required if image_name is empty<br>Available only when the flavor is U2                                                                                                                      |
| key_pair | String | - | Key pair name to use for accessing the instance<br>You can create a new key pair from **Compute > Instance > Key Pairs** on NHN Cloud console,<br>or register an existing key pair<br>See `User Guide > Compute > Instance > Console User Guide` for more details            |
| availability_zone | String | - | Availability zone of an instance to create                                                                                                                                                                             |
| network | Object | - | VPC network information to be attached to an instance to create.<br>Go to **Network > VPC > Management**  on the console, select VPC to be attached, and check the network name and UUID at the bottom.                                                                       |
| network.name | String | - | Name of VPC network <br>One among network.name, network.uuid, and network.port must be specified.                                                                                                                        |
| network.uuid | String | - | ID of VPC network                                                                                                                                                                                  |
| network.port | String | - | ID of a port to be attached to VPC network                                                                                                                                                                         |
| security_groups | Array | - | List of the security group names for instance <br>Select a security group from **Network > VPC > Security Groups** on the console, and check detailed information at the bottom of the page.                                                                             |
| user_data | String | - | 	Script to be executed after instance booting and its configuration<br>Base64-encoded string, which allows up to 65535 bytes<br>                                                                                                                              |
| block_device | Object | - | Information object of image or block storage to be used for an instance                                                                                                                                                               |
| block_device.uuid | String | - | ID of original block storage <br>The block storage must be a bootable source if used as the root block storage. Volumes or snapshots which cannot be used to create images, such as those with WAF, MS-SQL images as the source, cannot be used.<br> The original other than `image` must have the same availability zone for the instance to create.                            |
| block_device.source_type | String | O | Type of original block storage to create<br>`image`: Use an image to create a block storage<br>`volume`: Use the existing block storage, with the destination_type set to volume<br>`snapshot`: Use a snapshot to create a block storage, with the destination_type set to volume |
| block_device.destination_type | String | - | Requires different settings depending on the location of instance’s block storage or flavor<br>`local`: For U2 flavor<br>`volume`: For flavors other than U2                                                                                  |
| block_device.boot_index | Integer | - | Order to boot the specified block storage<br>- If , root block storage<br>- If not, additional block storage<br>The higher the number, the lower the booting priority<br>                                                                                                            |
| block_device.volume_size | Integer | - | Block storage size for instance to create<br>Available from 20GB to 2,000GB (required if the flavor is U2)<br>Since each flavor allows different volume size, see `User Guide > Compute > Instance Console User Guide`                        |
| block_device.delete_on_termination | Boolean | - | `true`: When deleting an instance, delete a block device<br>`false`: When deleting an instance, do not delete a block device                                                                                                                   |

### Attach Block Storage
```
# Create Instance
resource "nhncloud_compute_instance_v2" "tf_instance_01" {
  ...
}

# Create Block Storage
resource "nhncloud_blockstorage_volume_v2" "volume_01" {
  ...
}

# Attach Block Storage
resource "nhncloud_compute_volume_attach_v2" "volume_to_instance"{
  instance_id = nhncloud_compute_instance_v2.tf_instance_02.id
  volume_id = nhncloud_blockstorage_volume_v2.volume_01.id
  vendor_options {
    ignore_volume_confirmation = true
  }
}
```
| Name | Type | Required | Description |
| ------ | --- |---------| --------- |
| instance_id | String | O       | Target instance to attach the block storage |
| volume_id | String | O       | UUID of block storage to be attached |


## Resources - Block Storage

### Create Block Storage
```
# Create HDD-type Empty Block Storage
resource "nhncloud_blockstorage_volume_v2" "volume_01" {
  name = "tf_volume_01"
  size = 10
  availability_zone = "kr-pub-a"
  volume_type = "General HDD"
}

# Create SSD-type Empty Block Storage
resource "nhncloud_blockstorage_volume_v2" "volume_02" {
  name = "tf_volume_02"
  size = 10
  availability_zone = "kr-pub-b"
  volume_type = "General SSD"
}

# Create Block Storage with Snapshot
resource "nhncloud_blockstorage_volume_v2" "volume_03" {
  name = "tf_volume_03"
  description = "terraform create volume with snapshot test"
  snapshot_id = data.nhncloud_blockstorage_snapshot_v2.snapshot_01.id
  size = 30
}
```

| Name | Type | Required | Description |
| ------ | --- |----------| --------- |
| name | String | -        | Name of block storage to create |
| description | String | -        | Description of block storage |
| size | Integer | O        | Size of block storage to create (GB) |
| availability_zone | String | -        | Availability zone of a block storage to create. If the value does not exist, random availability zone is used. <br>To check availability_zone, go to `Storage > Block Storage > Management` on the console and click **Create Block Storage**. |
| volume_type | String | -        | Type of block storage <br>`General HDD`: HDD block storage (default) <br>`General SSD`: SSD block storage |


### Import Block Storage

You can import a block storage created on console or via API to Terraform and manage the block storage.

On the `.tf` file, write the information of block storage to import.
```
resource "nhncloud_blockstorage_volume_v2" "volume_06" {
  name = "volume_06"
  size = 10
}
```

Import the block storage with the command `terraform import nhncloud_blockstorage_volume_v2.{name} {block_storage_id}`.

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

NHN Cloud supports creation of the following resources with Terraform:

* VPC
* VPC Subnet
* Network Port
* Floating IP

Other VPC resources must be created in the console.

### Create VPC

Create a VPC with the specified IP range.

```
resource "nhncloud_networking_vpc_v2" "resource-vpc-01" {
  name = "tf-vpc-01"
  cidrv4 = "10.0.0.0/8"
}
```

| Name | Type | Required | Description     |
| --- | --- |---|--------|
| name | String | O | VPC name |
| cidrv4 | String | O | VPC IP range |
| region | String | - | VPC region name |
| tenant\_id | String | - | VPC tenant ID |



### Create VPC Subnet and Attach Routing Table

Create a subnet with the specified IP range in the specified VPC, and attach the existing routing table to the created subnet.
Routing tables can only be created in the NHN Cloud console.

```
resource "nhncloud_networking_vpcsubnet_v2" "resource-vpcsubnet-01" {
  name      = "tf-vpcsubnet-01"
  vpc_id    = "def56b5e-0f1d-4a31-8005-4d716127f177"
  cidr      = "10.10.10.0/24"
  routingtable_id = "c3ed678d-de8b-4bf7-abea-b7c1118f0828"
}
```

| Name | Type | Required | Description         |
| --- | --- |---|------------|
| vpc\_id | String | O | VPC ID to which subnet is assigned |
| cidr | String | O | IP range of subnet |
| name | String | O | Name of subnet    |
| region | String | - | Name of region to which subnet is assigned |
| tenant\_id | String | - | Tenant ID to which subnet is assigned |
| routingtable\_id | String | - | Routing table ID |


### Create Network Port

```
resource "nhncloud_networking_port_v2" "port_1" {
  name = "tf_port_1"
  network_id = data.nhncloud_networking_vpc_v2.default_network.id
  admin_state_up = "true"
}
```

| Name    | Type | Required | Description       |
| ------ | ---- |---| --------- |
| name | String | O | Port name to create |
| description | String | - | Port description |
| network_id | String | O | ID of VPC network to create a port |
| tenant_id | String | - | Tenant ID of the port to create |
| device_id | String | - | Device ID to which the created port will be connected |
| fixed_ip | Object | - | Setting information of fixed IP of a port to create<br>Must not include the `no_fixed_ip` attribute |
| fixed_ip.subent_id | String | O | Subnet ID of a fixed IP |
| fixed_ip.ip_address | String | - | Address of fixed IP to configure |
| no_fixed_ip | Boolean | - | `true`: Port without fixed IP<br>Must not include the `fixed_ip` attribute |
| admin_state_up | Boolean | - | Administrator control status<br> `true`: Running<br>`false`: Suspended |


### Create Floating IP

```
resource "nhncloud_networking_floatingip_v2" "fip_01" {
  pool = "Public Network"
}
```

| Name | Format | Required | Description |
| ------ | --- |---- | --------- |
| pool | String | O | IP pool to create a floating IP <br>From `Network > Floating IP` on console, click `Create Floating IP` and check the IP pool. |


### Associate Floating IP
```
# Create Network Port
resource "nhncloud_networking_port_v2" "port_1" {
  ...
}

# Create Instance
resource "nhncloud_compute_instance_v2" "tf_instance_01" {
    ...
    network {
    port = nhncloud_networking_port_v2.port_1.id
  }
  ...
}

# Create Floating IP
resource "nhncloud_networking_floatingip_v2" "fip_01" {
  ...
}

# Associate Floating IP
resource "nhncloud_networking_floatingip_associate_v2" "fip_associate" {
  floating_ip = nhncloud_networking_floatingip_v2.fip_01.address
  port_id = nhncloud_networking_port_v2.port_1.id
}

```

| Name          | Format | Required  | Description                    |
|-------------| --- |---- |-------------------------|
| floating_ip | String | O | Floating IP to associate           |
| port_id     | String | O | UUID of port to be associated with floating IP |


## Resources - Load Balancer
### Create Load Balancer

```
resource "nhncloud_lb_loadbalancer_v2" "tf_loadbalancer_01"{
  name = "tf_loadbalancer_01"
  description = "create loadbalancer by terraform."
  vip_subnet_id = data.nhncloud_networking_vpcsubnet_v2.default_subnet.id
  vip_address = "192.168.0.10"  
  admin_state_up = true
}
```
| Name | Format | Required | Description |
| ------ | ---- | ---- | --------- |
| name | String | - | Name of load balancer |
| description | String | - | Description of load balancer |
| tenant_id | String | - | Tenant ID to which load balancer is to be created |
| vip_subnet_id | String | O | Subnet UUID to be used by load balancer |
| vip_address | String | - | IP address of load balancer |
| security_group_ids | Object | - | List of security group IDs to be applied for load balancer <br>**Security groups must be specified by ID, not by name** |
| admin_state_up | Boolean | - | Administrator control status |


### Create Listener

```
# HTTP Listener
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

# Terminated HTTPS Listener
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
  default_tls_container_ref = "https://kr1-api-key-manager-infrastructure.nhncloudservice.com/v1/containers/3258d456-06f4-48c5-8863-acf9facb26de"
  sni_container_refs = null
  admin_state_up = true
}
```
| Name | Format | Required | Description |
| ------ | ---- | ---- | --------- |
| name | String | - | Name of listener to create |
| description  | String | - | Description of listener |
| protocol | String | O | Protocol of listener to create <br>One among `TCP`, `HTTP,HTTPS`, and `TERMINATED_HTTPS` |
| protocol_port | Integer | O | Port of listener to create |
| loadbalancer_id | String | O | ID of load balancer to be connected with listener to create |
| default_pool_id | String | - | ID of the default pool to be connected with listener to create |
| connection_limit  | Integer | - | Maximum connection count allowed for listener to create |
| timeout_client_data  | Integer | - | Timeout setting when client is inactive (ms) |
| timeout_member_connect  | Integer | - | Timeout setting when member is connected (ms) |
| timeout_member_data | Integer | - | Timeout setting when member is inactive (ms) |
| timeout_tcp_inspect | Integer | - | Timeout to wait for additional TCP packets for content inspection (ms) |
| default_tls_container_ref | String | - | Path of TLC certificate to be used when the protocol is `TERMINATED_HTTPS` |
| sni_container_refs | Array | - | List of SNI certificate paths |
| insert_headers | String | - | List of headers to be added before request is sent to a backend member |
| admin_state_up | Boolean | - | Administrator control status |

### Create Pool

<font color='red'>**(Caution) NHN Cloud does not support specifying `loadbalancer_id` when creating a pool.**</font>

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
| Name | Format | Required | Description |
| ------ | ---- | ---- | --------- |
| name | String | - | Load balancer name |
| description | String | - | Pool description |
| protocol | String | O | Protocol <br>One among `TCP`, `HTTP`, `HTTPS`, and `PROXY` |
| listener_id | String | O | ID of listener with which a pool to create is associated |
| lb_method | String | O | Load balancing method to distribute pool traffic to members <br>One among `ROUND_ROBIN`,`LEAST_CONNECTIONS`, and `SOURCE_IP` |
| persistence | Object | - | Session persistence of a pool to create |
| persistence.type | String | O | Session persistence type<br>One among `SOURCE_IP`, `HTTP_COOKIE`, and `APP_COOKIE` <br>Unavailable if the load balancing method is `SOURCE_IP`<br>HTTP_COOKIE and APP_COOKIE are unavailable if the protocol is `HTTPS` or `TCP` |
| persistence.cookie_name | String | - | Name of cookie <br>persistence.cookie_name is available only when the session persistence type is APP_COOKIE |
| admin_state_up | Boolean | - | Administrator control status |


### Create Health Monitor

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
| Name | Format | Required | Description |
| ------ | ---- | ---- | --------- |
| name | String | - | Name of health monitor to create |
| pool_id | String | O | ID of pool to be connected with health monitor to create |
| type | String | O | Support `TCP`, `HTTP`, and `HTTPS` only |
| delay | Integer | O | Interval of status check |
| timeout | Integer | O | Timeout for status check (seconds)<br>Timeout must have smaller value than delay |
| max_retries | Integer | O | Number of maximum retries, between 1 and 10 |
| url_path | String | - | Request URL for status check |
| http_method | String | - | HTTP method to use for status check<br>The default is GET |
| expected_codes | String | - | HTTP(S) response code of members to be considered as normal status <br/>expected_codes can be set as list (`200,201,202`) or range (`200-202`) |
| admin_state_up | Boolean | - | Administrator control status |


### Create Member

<font color='red'>**(Caution) `subnet_id` must be specified when you create a member in NHN Cloud. Also note that `name` is not supported. **</font>

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
| Name | Format | Required | Description |
| ------ | ---- | ---- | --------- |
| pool_id | String | O | ID of pool including member to create |
| subnet_id | String | O | Subnet ID of member to create |
| address | String | O | IP address of member to receive traffic from load balancer |
| protocol_port | Integer | O | Port of member to receive traffic |
| weight | Integer | - | Weight of traffic to receive from the pool <br>The higher the weight, the more traffic you receive. |
| admin_state_up | Boolean | - | Administrator control status |


## Reference
Terraform Documentation - [https://www.terraform.io/docs/providers/index.html](https://www.terraform.io/docs/providers/index.html)
