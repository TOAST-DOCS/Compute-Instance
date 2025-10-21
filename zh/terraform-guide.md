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

#### Supported Data Sources

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

## Terraform provider provided

NHN Cloud is an official partner of HashiCorp and offers Terraform provider through [Terraform Registry](https://registry.terraform.io/providers/nhn-cloud/nhncloud/latest).

## Terraform Initialization
Before using Terraform, create a provider configuration file as follows.

The name of the provider file can be set randomly. This example uses `provider.tf` as the filename.

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
data "nhncloud_compute_flavor_v2" "m2c2m4"{
  name = "m2.c2m4"
}
```

| Name | Format | Required | Description |
| ------ | ---- | ---- | --------- |
| name | String | - | Name of flavor to query |


### Key Pair

```
data "nhncloud_compute_keypair_v2" "my_keypair"{
  name = "my_keypair"
}
```

| Name    | Format | Required | Description         |
| ------ | ---- |----|------------|
| name | String | O  | Key pair name to query |


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


### Routing Table
```
data "nhncloud_networking_routingtable_v2" "default_rt" {
  id = "bf15f6f6-1339-4057-a7fe-5811d39bab18"
}
```

| Name | Format | Required | Description                  |
| --- | --- |---|---------------------|
| tenant_id | String | - | Tenant ID to which routing table to query is included |
| id | String | - | Routing table ID to query      |
| name | String | - | Routing table name to query   |


### Security Group
```
data "nhncloud_networking_secgroup_v2" "default_sg" {
  name = "default"
}
```

| Name | Format | Required | Description                 |
| --- | --- |---|--------------------|
| region | String | - | Region name that security group to query belongs to |
| tenant_id | String | - | Tenant ID that security group to query belongs to |
| name | String | - | Security group name to query       |


### Secret
```
data "nhncloud_keymanager_secret_v1" "secret_01" {
  name      = "terraform_secret_01"
}
```

| Name | Format | Required | Description               |
| --- | --- |---|------------------|
| region | String | - | Region name that secret to query belongs to |
| name | String | - | Secret name to query       |


### Secret Container
```
data "nhncloud_keymanager_container_v1" "container_01" {
  name      = "terraform_container_01"
}
```

| Name | Format | Required | Description                      |
| --- | --- |---|-------------------------|
| region | String | - | Region name to which the secret container you want to look up belongs.  |
| name | String | - | Secret container name to query         |


## Resources

You can create, modify, or delete resources with Terraform resources. NHN Cloud supports management of the following resources using Terraform:

* Instance
* Block storage
* Object storage
* VPC
* Floating IP
* Network port
* Load balancer
* Security group

The following sections describe how to use each resource.


### Note

* For how to use Object Storage, see [User Guide > Storage > Object Storage > Third-Party Tools Usage Guide](https://docs.nhncloud.com/en/Storage/Object%20Storage/en/third-party-tools-guide/).

## Resources - Instance

### Create Instance

```
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

| Name                                          | Format      | Required | Description                                                                                                                                                                                           |
|---------------------------------------------|---------|----|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| name                                        | String  | O  | Name of instance to create                                                                                                                                                                                 |
| region                                      | String  | -  | Region of instance to create<br>The default is the region configured in provider.tf                                                                                                                                                     |
| flavor_name                                 | String  | -  | Flavor name of instance to create<br>Required if flavor_id is empty                                                                                                                                                |
| flavor_id                                   | String  | -  | Flavor ID of instance to create<br>Required if flavor_name is empty                                                                                                                                              |
| key_pair                                    | String  | -  | Key pair name to use for accessing the instance<br>You can create a new key pair from **Compute > Instance > Key Pairs** on NHN Cloud console,<br>or register an existing key pair<br>See `User Guide > Compute > Instance > Console User Guide` for more details            |
| availability_zone                           | String  | -  | Availability zone where instance will be created<br>If left blank, a random zone will be selected<br>If the source type of the root block storage is `volume`, `snapshot`, it must be set to the same availability zone as the source block storage |
| network                                     | Object  | -  | VPC network information to be attached to an instance to create.<br>Go to **Network > VPC > Management** on the console, select VPC to be attached, and check the network name and UUID at the bottom.                                                                       |
| network.name                                | String  | -  | Name of VPC network <br>One among network.name, network.uuid, and network.port must be specified.                                                                                                                        |
| network.uuid                                | String  | -  | ID of VPC network                                                                                                                                                                                  |
| network.port                                | String  | -  | ID of a port to be attached to VPC network<br>Security groups requested when specifying a port ID are not applied to existing specified ports                                           |
| security_groups                             | Array   | -  | List of the security group names for instance <br>Select a security group from **Network > VPC > Security Groups** on the console, and check detailed information at the bottom of the page.                                                                             |
| user_data                                   | String  | -  | Script to be executed after instance booting and its configuration<br>Base64-encoded string, which allows up to 65535 bytes<br>                                                                                                                              |
| block_device                                | Object  | O  | Block storage information object |
| block_device.source_type                    | String  | O  | Type of original block storage to create<br>- `image`: Use an image to create a block storage<br>- `blank`: Create an empty block storage (cannot be used as root block storage)<br>- `volume`: Use previously created block storage<br>- `snapshot`: Create block storage with snapshots |
| block_device.uuid                           | String  | -  | Different settings required for different types of block storage sources<br>- Set the image ID if the source type is `image`<br>- Set an existing created block storage ID if the source type is `volume`<br>- Set `snapshot` ID if the source type is `snapshot`<br>- No settings required if source type is ` blank`<br>For root block storage, it must be a bootable source. |
| block_device.boot_index                     | Integer | O  | Order to boot the specified block storage<br>- If `0`, root block storage<br>- If not, additional block storage<br>A larger value indicates lower booting priority<br>                                                                                                            |
| block_device.destination_type               | String  | O  |Location of instance block storage<br>Supports `volume` only                                                     |
| block_device.volume_size                    | Integer | -  | Size of block storage to create<br>Different settings required for different types of block storage sources<br>- No settings required if source type is `volume`<br>- Set equal to or larger than the original block storage size if the source type is `snapshot`<br>GB (unit)<br>Different instance types have different sizes of root block storage that can be created. For more details, see `User Guide > Compute > Instance > Console User Guide > Create Instance > Block Storage Size`. |
| block_device.volume_type               | Enum    | -  | Type of Block Storage to Create<br>No configuration required if the source type of the block storage is `volume`, `snapshot`<br>See `Name` from the response of **List Block Storage Types** in the `User Guide > Storage > Block Storage > API v2 guide`. |
| block_device.delete_on_termination          | Boolean | -  | `true`: When deleting an instance, delete a block device<br>`false`: When deleting an instance, do not delete a block device                                                                                                                   |
| block_device.nhn_encryption                 | Object  | -  | About block storage encryption                                                                                                                                                                               |
| block_device.nhn_encryption.skm_appkey      | String  | O  | AppKeys for Secure Key Manager products                                                                                                                                                                    |
| block_device.nhn_encryption.skm_key_id      | String  | O  | Key ID in Secure Key Manager                                                                                                                                                                     |


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


### Key Pair
```
resource "nhncloud_compute_keypair_v2" "tf_kp_01" {
  name = "tf_kp_01"
}

# specify public_key
resource "nhncloud_compute_keypair_v2" "tf_kp_02" {
  name = "tf_kp_02"
  public_key = "ssh-rsa ... Generated-by-Nova"
}
```

| Name        | Format | Required | Description                             |
|-----------| --- |----|--------------------------------|
| name      | String | O  | Name of the key pair to create                     |
| public_key | String | -  | Public key to register<br>If omitted, create a new public key |

> [Caution]
> When you create a key pair through Terraform, the private key is stored **unencrypted** in the state file (terraform.tfstate).


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

| Name                | Format      | Required | Description                                                                                                                                                       |
|-------------------|---------|---|----------------------------------------------------------------------------------------------------------------------------------------------------------|
| name              | String  | - | Name of block storage to create                                                                                                                                           |
| description       | String  | - | Description of block storage                                                                                                                                               |
| size              | Integer | O | Size of block storage to create (GB)                                                                                                                                       |
| availability_zone | String  | - | Availability zone of a block storage to create. If the value does not exist, random availability zone is used.<br>To check availability_zone, go to `Storage > Block Storage > Management` on the console and click **Create Block Storage**. |
| volume_type       | Enum    | -  | Type of block storage<br>See `Name` from the response of **List Block Storage Types** in the `User Guide > Storage > Block Storage > API v2 guide`.                                                       |
| snapshot_id       | String  | - | Original snapshot ID, if omitted, empty block storage is created                                                                                                                           |
| nhn_encryption                 | Object  | -  | About block storage encryption                                                                                                                                           |
| nhn_encryption.skm_appkey      | String  | O  | AppKeys for Secure Key Manager products                                                                                                                                      |
| nhn_encryption.skm_key_id      | String  | O  | Key ID in Secure Key Manager                                                                                                                                       |


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
* Routing Table

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


### Create Routing Table
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

| Name     | Format      | Required | Description                                                             |
|--------|---------|----|----------------------------------------------------------------|
| name   | String  | O  | Routing table name                                                     |
| vpc_id | String  | O  | VPC ID to which the routing table belongs                                             |
| distributed   | Boolean | -  | Routing method of routing table </br>`true`: decentralized, `false`: centralized (default: `true`) |

### Associate Internet Gateway with Routing Table

Associate an Internet gateway to the routing table.
You can create an Internet gateway in the NHN Cloud console. For information on how to create an Internet gateway, see the [User Guide](https://docs.nhncloud.com/ko/Network/Internet%20Gateway/ko/console-guide/#_2).

```
resource "nhncloud_networking_routingtable_v2" "resource-rt-01" {
  ...
}

resource "nhncloud_networking_routingtable_attach_gateway_v2" "attach-gw-01" {
  routingtable_id = nhncloud_networking_routingtable_v2.resource-rt-01.id
  gateway_id = "5c7c578a-d199-4672-95d0-1980f996643f"
}
```

| Name     | Format      | Required | Description                                                                                                                      |
|--------|---------|----|-------------------------------------------------------------------------------------------------------------------------|
| routingtable_id   | String  | O  | Routing table ID to modify                                                                                                          |
| gateway_id | String  | O  | Internet gateway ID to be associated with routing table<br>In the console, select the Internet gateway you want to use from the **Network > Internet Gateway** menu, and you can see the ID of the gateway in the details screen below. |

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
| loadbalancer_type | String | - | Load Balancer Type<br>`shared/dedicated` available<br>Set to `shared` if omitted |

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
| keepalive_timeout | Integer | - | The listener's keepalive timeout |


### Create Pool


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
| loadbalancer_id | String | -  | The load balancer ID that the pool to be created will be connected to<br>At least one of the load balancer ID or listener ID is required       |
| listener_id | String | -  | ID of listener with which a pool to create is associated<br>At least one of the load balancer ID or listener ID is required              |
| lb_method | String | O | Load balancing method to distribute pool traffic to members <br>One among `ROUND_ROBIN`,`LEAST_CONNECTIONS`, and `SOURCE_IP` |
| persistence | Object | - | Session persistence of a pool to create |
| persistence.type | String | O | Session persistence type<br>One among `SOURCE_IP`, `HTTP_COOKIE`, and `APP_COOKIE` <br>Unavailable if the load balancing method is `SOURCE_IP`<br>HTTP_COOKIE and APP_COOKIE are unavailable if the protocol is `HTTPS` or `TCP` |
| persistence.cookie_name | String | - | Name of cookie <br>persistence.cookie_name is available only when the session persistence type is APP_COOKIE |
| admin_state_up | Boolean | - | Administrator control status |
| member_port | Integer | - | The member's listening port<br>Forward traffic to this port<br>The default value is `-1` |


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
| host_header | String | - | Field values in the host header to use for status checking<br>If you set the health check type to `TCP`, the value you set in this field is ignored. |
| health_check_port | Integer | - | Member port targeted by health check |


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


### Create a Secret

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

| Name                       | Format | Required | Description                                                                                                                                                           |
|--------------------------| ---- |----|--------------------------------------------------------------------------------------------------------------------------------------------------------------|
| name                     | String | -  | Secret name                                                                                                                                                       |
| expiration               | Datetime | -  | Expiration date. Request in ISO8601 format                                                                                                                                         |
| algorithm                | String | -  | Encryption algorithm                                                                                                                                                     |
| bit_length               | String | -  | Encryption key length                                                                                                                                                     |
| mode                     | String | -  | How block encryption work                                                                                                                                                  |
| payload                  | String | -  | Encryption key payload                                                                                                                                                   |
| payload_content_type     | String | -  | Encryption key payload content type </br>Required when entering a payload </br>List of supported content types: `text/plain`, `application/octet-stream`, `application/pkcs8`, `application/pkix-cert` |
| payload_content_encoding | Enum | -  | Encoding encryption key payload </br>Required if payload_content_type is not `text/plain` </br> Only supports `base64`                                                               |
| secret_type              | Enum | -  | Secret type </br>One of the following: `symmetric`, `public`, `private`, `passphrase`, `certificate`, `opaque`                                                                      |


### Create Secret Container

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

| Name   | Format     | Required | Description                                                                                                                                                                                             |
|------|--------|----|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| type | Enum   | O  | Container type </br>One of `generic`, `rsa`, `certificate`                                                                                                                                               |
| name | String | -  | Container name                                                                                                                                                                                        |
| secret_refs | Array  | -  | List of secrets to register in the container                                                                                                                                                                               |
| secret_refs.secret_ref	 | String | -  | Secret address                                                                                                                                                                                         |
| secret_refs.name	 | String | -  | The secret name specified by the container </br>If container type is `certificate`: Specify `as` `certificate`, `private_key`, `private_key_passphrase`, and `intermediates` </br>If container type is `rsa`: Specify `as` `private_key`, `private_key_passphrase`, and `public_key` |


## Resources - Security Groups

### Create a Security Group

```
resource "nhncloud_networking_secgroup_v2" "resource-sg-01" {
  name      = "sg-01"
}
```

| Name   | Format     | Required | Description               | 
|------|--------|---|------------------|
| name | String | O | Security group name         |
| region | String | - | Name of region to which security group is assigned |


### Create a Security Rule

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

| Name   | Format     | Required | Description               | 
|------|--------|---|------------------|
| remote_group_id | UUID | - | Remote security group ID of security rules |
| direction | Enum | O | Packet direction to which security rules apply<br>**ingress**, **egress** |
| ethertype | Enum | - | Specify as `IPv4`. Specify as `IPv4`when omitted |
| protocol | String | - | Protocol name of the security rule. Applied to all protocols if omitted. |
| port_range_max | Integer | - | Maximum port range of the security rule |
| port_range_min | Integer | - | Minimum port range of the security rule |
| security_group_id | UUID | O | Security group ID containing the security rule |
| remote_ip_prefix | Enum | - | Destination IP prefix of the security rule |
| description | String | - | Security rule description |

## Reference
Terraform Documentation - [https://www.terraform.io/docs/providers/index.html](https://www.terraform.io/docs/providers/index.html)
