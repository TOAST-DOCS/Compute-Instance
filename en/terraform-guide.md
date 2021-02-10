## Third Party User Guide > Terraform User Guide 
This document describes how to use NHN Cloud with Terraform. 

## Terraform
 Terraform is an open-source tool to easily build and safely change infrastructure, and also to efficiently manage infrastructure configuration. Find out the main features of Terraform as follows:  

* **Infrastructure as Code**
    * With infrastructure defined in codes, productivity and transparency can be raised.   
    * Sharing defined codes is easy enough to allow efficient collaboration. 
* **Execution Plan**
    * Change plan is separated from application so that mistakes from change application can be minimized. 
* **Resource Graph**
    * You can predict how a small change might impact the entire infrastructure. 
    *  Map out a dependency graph to plan and to check expected change of infrastructure.  
* **Change Automation**
    * Automation is applied to build and change infrastructure of same configuration in many locations. 
    * Time and mistakes can be saved while building infrastructure. 

NHN Cloud supports data sources and resources described as below with Terraform OpenStack Provider. For more details regarding Terraform OpenStack Provider and its features, see [Terraform website for OpenStack Provider](https://www.terraform.io/docs/providers/openstack/index.html)

#### Support of Resources 

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

#### Support of Data Sources 

* openstack_images_image_v2
* openstack_blockstorage_volume_v2
* openstack_compute_flavor_v2
* openstack_blockstorage_snapshot_v2
* openstack_networking_network_v2
* openstack_networking_subnet_v2

### Note

* **All data in the below example are not real. Replace them with precise data before application.**
* **All examples below are made with Terraform 0.12.24.**


## Terraform Installation 
Go to [Download Terraform](https://www.terraform.io/downloads.html) and download files for a local operating system. Decompress the files and send them to an appropriate path, and add the following path to the environment setup, and the installation is complete.  

See the following example for installation.

```
$ wget https://releases.hashicorp.com/terraform/0.12.24/terraform_0.12.24_linux_amd64.zip
$ unzip terraform_0.12.24_linux_amd64.zip
$ export PATH="${PATH}:$(pwd)"
$ terraform -v
Terraform v0.12.24
```


## Terraform Initialization 
Before using Terraform, create supplier configuration files like below. 

Name of the supplier file can be randomly configured, which is set as `provider.tf` in this example.    

```
# Configure the OpenStack Provider
provider "openstack" {
  user_name   = "terraform-guide@nhnent.com"
  tenant_id   = "aaa4c0a12fd84edeb68965d320d17129"
  password    = "difficultpassword"
  auth_url    = "https://api-identity.infrastructure.cloud.toast.com/v2.0"
  region      = "KR1"
}
```
* **user_name**
    * Use NHN Cloud ID. 
* **tenant_id**
    * From **Compute > Instance > Management** on NHN Cloud Console, click **API Endpoint Setting** to check Tenant ID. 
* **password**
    * Use **API Password** saved for **API Endpoint Setting**.
    * Regarding how to set API passwords, see **User Guide > Compute > Instance > Preparing for APIs**.
* **auth_url**
    * Specify the address of NHN Cloud identification service.  
    * From **Compute > Instance > Management** on NHN Cloud console, click **API Endpoint Setting** to check Identity URL.  
* **region**
    * Enter the region to manage NHN Cloud resources.
    * **KR1**: Korea (Pangyo) Region 
    * **KR2**: Korea (Pyeongchon) Region 
    * **JP1**:  Japan (Tokyo) Region 

On the path including supplier configuration files,  use the `init` command to initialize Terraform. 

```
$ ls
provider.tf
$ terraform init
```

## Terraform Usage 

Infrastructure buildup with Terraform has the following life cycle: 

1. Create TF Files
2. Confirm Buildup Plan 
3. Create Resources
4. Modify Resources
5. Delete Resources

Create infrastructure configuration to build on a tf file. Apply the  `plan` command like below, to confirm the buildup plan according to the tf file.  

```
$ terraform plan
```

If well planned, command with `apply` to create, modify, or delete resources. 

```
$ terraform apply
```

See the following section to learn more details on each phase with examples. 

### Create TF Files 

Create a tf file to the path with supplier configuration files. You may collect many resource settings for a single tf file, or write for each tf file per resource. Terraform reads the entire tf files all at once to set up a buildup plan.  

See the below example on tf files in which resources creating instances are defined in the `instance.tf` file.

```
$ ls
instance.tf provider.tf
$ cat instance.tf
resource "openstack_compute_instance_v2" "terraform-instance-01" {
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

### Confirm Buildup Plan  

With the `plan` command, check resources for a change in tf files. By executing the `plan` command, Terraform loads .tf files to check if the setting is correct, and compare it with its own database and create a plan. When the plan is completely created, collect each type of plan for a neat output.    

```
$ terraform plan
```

If a new plan is invalid, correct tf files and repeat the process, and execute the `plan` command. Since the `plan` command requires no change of actual NHN Cloud resources, you can check infrastructure changes any time. 

### Create Resources 

After a tf file is created with a plan of choice, execute  `apply` to create resources. 

See the result example of executing the `apply` command by using the `instance.tf` file created in the above.  

```
$ terraform apply
...
openstack_compute_instance_v2.terraform-instance-01: Creating...
openstack_compute_instance_v2.terraform-instance-01: Still creating... [10s elapsed]
...
openstack_compute_instance_v2.terraform-instance-01: Still creating... [50s elapsed]
openstack_compute_instance_v2.terraform-instance-01: Creation complete after 53s [id=8a8c5516-6762-4592-97ab-db8d3af629e6]

Apply complete! Resources: 1 added, 0 changed, 0 destroyed.
...
```

After the `apply` command is executed, the database file (terraform.tfstate) recording the history of plan changes is created in the current directory. Take cautions for not deleting this file.  

### Modify Resources 

Open the  `.tf` file in which resources for change are defined, modify information, and apply the plan. Specificiation changes are restricted only to some attributes. If there is a change in an attribute which cannot be changed, the resource is deleted and created anew.   

Below example shows adding one more`terraform-sg` security group to an instance. Modify the  `instance.tf`  file created in the above, like follows. 

```
resource "openstack_compute_instance_v2" "terraform-instance-01" {
  ...
  security_groups = ["default", "terraform-sg"]
  ...
}
```

Check the buildup plan, and the changed security group information comes as an output. 

```
$ terraform plan
...
An execution plan has been generated and is shown below.
Resource actions are indicated with the following symbols:
  ~ update in-place

Terraform will perform the following actions:

  ~ openstack_compute_instance_v2.terraform-instance-01
      security_groups.#:          "1" => "2"
      security_groups.3814588639: "default" => "default"
      security_groups.4051241745: "" => "terraform-sg"


Plan: 0 to add, 1 to change, 0 to destroy.
...
```

With the plan applied, a new security group is added to instance. 
```
$ terraform apply
...
openstack_compute_instance_v2.terraform-instance-01: Refreshing state... (ID: 4d135bc-6a70-4c4d-b645-931570c9f6b1)
openstack_compute_instance_v2.terraform-instance-01: Modifying... (ID: 4d135bc-6a70-4c4d-b645-931570c9f6b1)
  security_groups.#:          "1" => "2"
  security_groups.3814588639: "default" => "default"
  security_groups.4051241745: "" => "terraform-sg"
openstack_compute_instance_v2.terraform-instance-01: Modifications complete after 1s (ID: 4d135bc-6a70-4c4d-b645-931570c9f6b1)

Apply complete! Resources: 0 added, 1 changed, 0 destroyed.
```

### Delete Resources

To delete resources created with Terraform, delete corresponding `.tf`  files. 

```
$ rm instance.tf
```

In the buildup plan, confirm the resources have been deleted. 

```
$ terraform plan
...
An execution plan has been generated and is shown below.
Resource actions are indicated with the following symbols:
  - destroy

Terraform will perform the following actions:

  - openstack_compute_instance_v2.terraform-test-01


Plan: 0 to add, 0 to change, 1 to destroy.
...
```

With the `apply` command, created instances are deleted.  

```
$ terraform apply
...
openstack_compute_instance_v2.terraform-test-01: Refreshing state... (ID: f4d135bc-6a70-4c4d-b645-931570c9f6b1)
openstack_compute_instance_v2.terraform-test-01: Destroying... (ID: f4d135bc-6a70-4c4d-b645-931570c9f6b1)
openstack_compute_instance_v2.terraform-test-01: Still destroying... (ID: f4d135bc-6a70-4c4d-b645-931570c9f6b1, 10s elapsed)
openstack_compute_instance_v2.terraform-test-01: Destruction complete after 11s
...
```

## Data Sources

You can find Instance Type ID or Image ID required to create tf files on the console, or import them by using data sources provided by Terraform. Data sources must be written within tf files, and imported data cannot be modified but are used only for reference. 

Get a reference of data sources in `{data sources type}.{data source name}`. In the below example, refer to the image information imported to `openstack_images_image_v2.ubuntu_1804_20200218`.

```
data "openstack_images_image_v2" "ubuntu_1804_20200218" {
  name = "Ubuntu Server 18.04.3 LTS (2020.02.18)"
  most_recent = true
}
```

It is available to get a reference of other data sources within a data source. 

```
data "openstack_blockstorage_volume_v2" "volume_00"{
  name = "ssd_volume1"
  status = "available"
}

data "openstack_blockstorage_snapshot_v2" "my_snapshot" {
  name = "my-snapshot"
  volume_id = data.openstack_blockstorage_volume_v2.volume_00.id
  status = "available"
  most_recent = true
}
```

For more details on the usage of data sources, read `Data Sources` from the [Terraform Website](https://www.terraform.io/docs/providers/openstack/index.html).


The next section describes how to import NHN Cloud resources to data resources.  

### Image

Imports image information. NHN Cloud common images as well as personal images are supported.  

```
data "openstack_images_image_v2" "ubuntu_1804_20200218" {
  name = "Ubuntu Server 18.04.3 LTS (2020.02.18)"
  most_recent = true
}

# Query the oldest image from same-name images 
data "openstack_images_image_v2" "windows2016_20200218" {
  name = "Windows 2016 STD with MS-SQL 2016 Standard (2020.02.18) KO"
  sort_key = "created_at"
  sort_direction = "asc"
  owner = "c289b99209ca4e189095cdecebbd092d"
  tag = "_AVAILABLE_"
}
```
| Name | Format | Required | Description |
| ------ | ---- | ---- | --------- |
| name | String | - | Name of image to query<br>To check image name, go to **Compute > Instance** on NHN Cloud console and click **Create Instance**. <br>Image name must be created as **< Image Description >** which shows on NHN Cloud console<br>If the language item exists, follow the format as **"< Image Description > < Language >"** like the above example. |
| size_min | Integer | - | Minimum size of image to query (bytes) |
| size_max | Integer | - | Maximum size of image to query (bytes) |
| properties | Object | - | Attributes of image to query <br>Query image in which all attributes coincide |
| sort_key | String | - | Sort the list of images queried by particular attribute <br>Default is `name` |
| sort_direction | String | - | Sorting direction of the list of queried images <br>`asc`: Ascending order (Default) <br>`desc`: Descending order |
| owner | String | - | ID of tenant which includes the image to query |
| tag | String | - | Search image in which a particular tag exists |
| visibility | String | - | Show attribute of image to query <br>Select only one out of public, private, and shared <br>If left blank, all types of image list are returned |
| most_recent | Boolean | - | `true`: Select the most recently created image from the list of queried images <br>`false`: Select images in the queried order |
| member_status | String | - | Status of image member to query <br>One of`accepted`,`pending`, 'rejected', and`all` |

### Block Storage

```
data "openstack_blockstorage_volume_v2" "volume_00" {
  name = "ssd_volume1"
  status = "available"
}
```

| Name | Format | Required | Description |
| ------ | ---- | ---- | --------- |
| name | String | - | Name of block storage to query |
| status | String | - | Status of block storage to query |
| metadata | Object | - | Metadata related with block storage to query |

### Instance Type

To check name of an instance type, go to NHN Cloud Console and click **Create Instance > Instance Type** from **Compute > Instance**. 

```
data "openstack_compute_flavor_v2" "u2c2m4"{
  name = "u2.c2m4"
}
```

| Name | Format | Required | Descrition |
| ------ | ---- | ---- | --------- |
| name | String | - | Name of instance type to query |


### Snapshot

```
data "openstack_blockstorage_snapshot_v2" "my_snapshot" {
  name = "my-snapshot"
  volume_id = data.openstack_blockstorage_volume_v2.volume_00.id
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
data "openstack_networking_network_v2" "default_network" {
  name="Default Network"
  network_id = "00d5b852-cb77-4307-b6be-d81dad24eec1"
}
```

| Name | Format | Required | Description |
| ------ | ---- | ---- | --------- |
| name | String | - | Name of VPC network to query |
| network_id | String | - | UUID of VPC network to query |


### Subnet

To check subnet ID, go to NHN Cloud console and select a subnet from **Network > VPC > Subnet**. 

```
data "openstack_networking_subnet_v2" "default_subnet" {
  name = "Default Network"
  subnet_id = "756af037-54f3-4aa2-8c22-56c9da055553"
  network_id = data.openstack_networking_network_v2.default_network.network_id
}
```
| Name | Format | Required | Description |
| ------ | ---- | ---- | --------- |
| name | String | - | Name of subnet to query |
| subnet_id | String | - | UUID of subnet to query |
| network_id | String | - | UUID of network to which subnet to query is included |


## Resources

You may create, modify, or delete resources with Terraform Resources. With Terraform, NHN Cloud supports the following to manage resources:  

* Instance 
* Block Storage
* Floating IP 
* Network Port
* Load Balancer

The following sections describe how to use each resource.  

## Resources - Instance

### Create Instance 

```
# Create u2 Instance
resource "openstack_compute_instance_v2" "tf_instance_01"{
  name = "tf_instance_01"
  region    = "KR1"
  key_pair  = "terraform-keypair"
  image_id = data.openstack_images_image_v2.centos_610_20200218.id
  flavor_id = data.openstack_compute_flavor_v2.u2c1m2.id
  security_groups = ["default"]
  availability_zone = "kr-pub-a"

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

# Instance types other than u2 
# Create instances with network or block storage added 
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
```
| Name | Format | Required | Description |
| ------ | ---- | ---- | --------- |
| name | String | O | Name of instance to create |
| region | String | - | Region of instance to create <br>Default is the region setting at  provider.tf |
| flavor_name | String | - | Name of instance type of instance to create <br>Required if flavor_id is empty |
| flavor_id | String | - | ID of instance type of instance to create <br>Required if flavor_name is empty |
| image_name | String | - | Name of image to create an instance <br>Required if image_id is empty <br>Available only when the instance type is U2 |
| image_id | String | - | Image ID to create an instance <br>Required if image_name is empty <br>Available only when the instance type is U2 |
| key_pair | String | - | Keypair name to access instance<br>You may create a new keypair from **Compute > Instance > Key Pair** on NHN Cloud console, <br>or register an existing keypair<br>See  `User Guide > Compute > Instance > Console User Guide` for more details |
| availability_zone | String | - | Availability zone of an instance to create |
| network | Object | - | VPC network information to be attached to an instance to create.<br>On console, go to **Network > VPC > Management** and select VPC to be attached, and find the name and UUID of network at the bottom. |
| network.name | String | - | Name of VPC network <br>Must specify one of network.name, network.uuid, and network.port |
| network.uuid | String | - | ID of VPC network |
| network.port | String | - | ID of a port to be attached to VPC network |
| security_groups | Array | - | List of the security group names for instance  <br>Select a security group from **Network > VPC > Security Groups** on the console, and check detail information at the bottom of the page. |
| user_data | String | - | Script and setting to be executed after instance booting<br>Allows up to 65535 bytes with character strings encoded in base64 <br> |
| block_device | Object | - | Information object of image or block storage to be applied for an instance |
| block_device.uuid | String | - | ID of original block storage |<br> The original must be bootable for a root volume, and a volume or snapshot that has WAF, from which images cannot be created, or MS-SQL images as the original, are not available. <br> The original excluding `image` must have the same availability zone with an instance to create.|
| block_device.source_type | String | O | Type of original block storage to create<br>`image`: Create block storage by using image <br>`volume`: Use previously-created volume, with the destination_type specified by volume <br>`snapshot`: Use snapshot to create a block storage, with the destination_type specified by volume |
| block_device.destination_type | String | - | Requires different settings for each location or type of instance volume <br>`local`: For U2 instance type <br>`volume`: For other instances than U2 |
| block_device.boot_index | Integer | - | Booting order of specified volumes<br>Root volume for 0 <br>Additional volumes for others<br>The bigger the number, the lower the booting on the priority<br> |
| block_device.volume_size | Integer | - | Disk size for instance to create <br>Available from 20GB to 2,000GB (required, if the instance type is U2) <br>Since each instance type allows different volume size, see `User Guide > Compute > Console User Guide for Instance` |
| block_device.delete_on_termination | Boolean | - | `true`: Delete block device along with instance <br>`false`: Delete instance, but not block instance |

### Attach Block Storage
```
# Create Instance
resource "openstack_compute_instance_v2" "tf_instance_01" {
  ...
}

# Create Block Storage
resource "openstack_blockstorage_volume_v2" "volume_01" {
  ...
}

# Attach Block Storage 
resource "openstack_compute_volume_attach_v2" "volume_to_instance"{
  instance_id = openstack_compute_instance_v2.tf_instance_02.id
  volume_id = openstack_blockstorage_volume_v2.volume_01.id
}
```
| Name | Type | Required | Description |
| ------ | --- |---- | --------- |
| instance_id | String | - | Instance of a target to be attached with block storage |
| volume_id | String | - | UUID of block storage to be attached |


## Resources - Block Storage

### Create Block Storage
```
# Create HDD-type Empty Block Storage 
resource "openstack_blockstorage_volume_v2" "volume_01" {
  name = "tf_volume_01"
  size = 10
  availability_zone = "kr-pub-a"
  volume_type = "General HDD"
}

# Create SSD-type Empty Block Storage 
resource "openstack_blockstorage_volume_v2" "volume_02" {
  name = "tf_volume_02"
  size = 10
  availability_zone = "kr-pub-b"
  volume_type = "General SSD"
}

# Create Block Storage with Snapshot 
resource "openstack_blockstorage_volume_v2" "volume_03" {
  name = "tf_volume_03"
  description = "terraform create volume with snapshot test"
  snapshot_id = data.openstack_blockstorage_snapshot_v2.snapshot_01.id
  size = 30
}
```

| Name | Type | Required | Description |
| ------ | --- |---- | --------- |
| name | String | O | Name of block storage to create |
| description | String | - | Description of block storage |
| size | Integer | - | Size of block storage to create (GB) |
| availability_zone | String | - | Availability zone of a block storage to create; random zone, if value does not exist. <br>To check availability_zone, go to `Storage > Block Storage > Management` of the console and click **Create Block Storage**. |
| volume_type | String | - | Type of block storage <br>`General HDD`: HDD block storage (default) <br>`General SSD`: SSD block storage |


### Import Block Storage

Import block storage created on console or via API to Terraform.  

Create block storage information to be imported onto `.tf` file. 

```
resource "openstack_blockstorage_volume_v2" "volume_06" {
  name = "volume_06"
  size = 10
}
```

With the command of `terraform import openstack_blockstorage_volume_v2.{name} {block storage id}`, import block storage. 

```
$ terraform import openstack_blockstorage_volume_v2.volume_06 10cf5bec-cebb-479b-8408-3ffe3b569a7a
...
openstack_blockstorage_volume_v2.volume_06: Importing from ID "10cf5bec-cebb-479b-8408-3ffe3b569a7a"...
openstack_blockstorage_volume_v2.volume_06: Import prepared!
  Prepared openstack_blockstorage_volume_v2 for import
openstack_blockstorage_volume_v2.volume_06: Refreshing state... [id=10cf5bec-cebb-479b-8408-3ffe3b569a7a]

Import successful!
...
```


## Resources - VPC

NHN Cloud supports creating the following resources with Terraform: 

* Floating IP 
* Network port 

Other VPC resources must be created on console. 

### Create Floating IP

```
resource "openstack_compute_floatingip_v2" "fip_01" {
  pool = "Public Network"
}
```

| Name | Format | Required | Description |
| ------ | --- |---- | --------- |
| pool | String | O | IP pool to create a floating IP <br>From `Network > Floating IP` on console, click`Create Floating IP` and check IP pool. |


### Associate Floating IP
```
# Create Instance
resource "openstack_compute_instance_v2" "tf_instance_01" {
  ...
}

# Create Floating IP
resource "openstack_compute_floatingip_v2" "fip_01" {
  ...
}

# Associate Floating IP
resource "openstack_compute_floatingip_associate_v2" "fip_associate" {
  floating_ip = openstack_compute_floatingip_v2.fip_01.address
  instance_id = openstack_compute_instance_v2.tf_instance_01.id
}

```
| Name | Format | Required | Description |
| ------ | --- |---- | --------- |
| floating_ip | String | O | Floating IP to associate |
| instance_id | String | O | UUID of instance for a target to be associated with floating IP |
| fixed_ip | String | - | Fixed IP of a target to be associated with floating IP |
| wait_until_associated | Boolean | - | `true`: Poll a target instance until floating IP is associated <br>`false`: Do not wait until floating IP is associated (default) |

### Create Network Port

```
resource "openstack_networking_port_v2" "port_1" {
  name = "tf_port_1"
  network_id = data.openstack_networking_network_v2.default_network.id
  admin_state_up = "true"
}
```

| Name | Format | Required | Description |
| ------ | ---- | ---- | --------- |
| name | String | O | Name of port to create |
| description | String | O | Description of port |
| network_id | String | O | Network ID of VPC to create a port |
| tenant_id | String | O | Tenant ID of port to create |
| device_id | String | - | ID of device to be attached to a created port |
| fixed_ip | Object | - | Setting information of fixed IP of a port to create <br>Must not include the`no_fixed_ip` attribute |
| fixed_ip.subent_id | String | O | Subnet ID of fixed IP |
| fixed_ip.ip_address | String | - | Address of fixed IP to configure |
| no_fixed_ip | Boolean | - | `true`: Port without fixed IP<br>Must not include the`fixed_ip` attribute |
| admin_state_up | Boolean | - | Administrator control status <br> `true`: Running <br>`false`: Suspended |




## Resources - Load Balancer 
### Create Load Balancer

```
resource "openstack_lb_loadbalancer_v2" "tf_loadbalancer_01"{
  name = "tf_loadbalancer_01"
  description = "create loadbalancer by terraform."
  vip_subnet_id = data.openstack_networking_subnet_v2.default_subnet.id
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
| vip_address | String | - | IP specified by load balancer |
| security_group_ids | Object | - | List of security group IDs to be applied for load balancer <br>**Security groups must be specified by ID, not by name** |
| admin_state_up | Boolean | - | Administrator control status |


### Create Listener 

```
# HTTP Listener
resource "openstack_lb_listener_v2" "tf_listener_http_01"{
  name = "tf_listener_01"
  description = "create listener by terraform."
  protocol = "HTTP"
  protocol_port = 80
  loadbalancer_id = openstack_lb_loadbalancer_v2.tf_loadbalancer_01.id
  default_pool_id = ""
  connection_limit = 2000
  timeout_client_data = 5000
  timeout_member_connect = 5000
  timeout_member_data = 5000
  timeout_tcp_inspect = 5000
  admin_state_up = true
}

# Terminated HTTPS Listener
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
  default_tls_container_ref = "https://kr1-api-key-manager.infrastructure.cloud.toast.com/v1/containers/3258d456-06f4-48c5-8863-acf9facb26de"
  sni_container_refs = null
  admin_state_up = true
}
```
| Name | Format | Required | Description |
| ------ | ---- | ---- | --------- |
| name | String | - | Name of listener to create |
| description  | String | - | Description of listener |
| protocol | String | O | Listener protocol to create <br>One of`TCP`, `HTTP,HTTPS`, and `TERMINATED_HTTPS` |
| protocol_port | Integer | O | Listener port to create |
| loadbalancer_id | String | O | ID of load balancer to be connected with listener to create |
| default_pool_id | String | - | ID of default pool to be connected with listener to create |
| connection_limit  | Integer | - | Maximum connection count allowed to listener to create |
| timeout_client_data  | Integer | - | Timeout setting when client is disabled (ms) |
| timeout_member_connect  | Integer | - | Timeout setting when member is connected (ms) |
| timeout_member_data | Integer | - | Timeout setting when member is disabled (ms) |
| timeout_tcp_inspect | Integer | - | Timeout for additional TCP packet to inspect content (ms) |
| default_tls_container_ref | String | - | Path of TLC certificate to be enabled when the protocol is`TERMINATED_HTTPS` |
| sni_container_refs | Array | - | List of SNI certificate paths |
| insert_headers | String | - | List of headers to be added before request is sent to a backend member |
| admin_state_up | Boolean | - | Administrator control status |

### Create Pool

<font color='red'>**(Caution) NHN Cloud does not support for specifying `loadbalancer_id` .**</font>

```
resource "openstack_lb_pool_v2" "tf_pool_01"{
  name = "tf_pool_01"
  description = "create pool by terraform."
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
| Name | Format | Required | Description |
| ------ | ---- | ---- | --------- |
| name | String | - | Load balancer name |
| description | String | - | Pool description |
| protocol | String | O | Protocol <br>One of `TCP`, `HTTP`, `HTTPS`, and `PROXY` |
| listener_id | String | O | Listener ID with which a pool to create is to be associated |
| lb_method | String | O | Load balancing method to distribute pool traffic to members <br>One of `ROUND_ROBIN`,`LEAST_CONNECTIONS`, and`SOURCE_IP` |
| persistence | Object | - | Session persistence of a pool to create |
| persistence.type | String | O | Session persistence type<br>One of`SOURCE_IP`, `HTTP_COOKIE`, and `APP_COOKIE` <br>Unavailable if the load balancing type is `SOURCE_IP`<br>HTTP_COOKIE and APP_COOKIE are unavailable if the protocol is  `HTTPS` or`TCP` |
| persistence.cookie_name | String | - | Name of cookie <br>persistence.cookie_name is available only when the session persistence type is APP_COOKIE |
| admin_state_up | Boolean | - | Administrator control status |


### Create Health Monitor 

```
resource "openstack_lb_monitor_v2" "tf_monitor_01"{
  name = "tf_monitor_01"
  pool_id = openstack_lb_pool_v2.tf_pool_01.id
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
| pool_id | String | O | Pool ID to be connected with health monitor to create |
| type | String | O | Support`TCP`, `HTTP`, and `HTTPS` only |
| delay | Integer | O | Interval of status check |
| timeout | Integer | O | Timeout for status check (seconds)<br>Timeout must have smaller value than delay |
| max_retries | Integer | O | Number of maximum retries, between 1 and 10 |
| url_path | String | - | URL requesting of status checks |
| http_method | String | - | HTTP method to check status<br>Default is GET |
| expected_codes | String | - | HTTP response code of members to be considered in normal status <br/>expected_codes is available as list (`200,201,202`), or range (`200-202`) |
| admin_state_up | Boolean | - | Administrator control status |


### Create Member

<font color='red'>**(Caution) `subnet_id` must be specified when NHN Cloud creates a member. Also note that`name` is not supported. **</font>

```
resource "openstack_lb_member_v2" "tf_member_01"{
  pool_id = openstack_lb_pool_v2.tf_pool_01.id
  subnet_id = data.openstack_networking_subnet_v2.default_subnet.id
  address = openstack_compute_instance_v2.tf_instance_01.access_ip_v4
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
