## Compute > Instance > Console Guide

## Create Instances

### Image

Select an image with the operating system you need: select either from public images of TOAST, user images, or shared images.  

Instance flavors vary depending on the image you choose, so it is recommended to choose an image first before creating an instance. 

| OS                               | Disk      | Memory   |
| -------------------------------- | --------- | -------- |
| Linux <br>CentOS, Ubuntu, Debian | Over 20GB | Over 1GB |
| Windows                          | Over 50GB | Over 2GB |
 

### Availability Zone 

Unless specified, the availability zone shall be set randomly. Depending on the availability zone, the block storage is determined that the instance can use. If the block storage exists in a particular availability zone, then choose the zone.  

> [Note] VPC resources are available in all availability zones.

For more details of Availability Zone, refer to [Availibility Zone of Overview of Instance](/Compute/Instance/en/overview/#_4).

### Flavors 

Flavors can be selected depending on virtual hardware performance. Nevertheless, such selection may be limited depending on the performance that image requires. For more details, refer to [Overview of Instances](./overview). 

Instance flavors may be changed in the TOAST console after created: from higher to lower specs, or vice versa. Some flavors cannot be changed, so refer to [Change of Instance Flavors](./console-guide/#_14)

> [Caution] Default instance disk cannot be changed with Change of Instance Flavors. 

### Device Size 

Determines default disk size of an instance. 

- Size of a default disk cannot change once an instance is created. When there's a shortage of volume, add block storages for further usage. 
- Device size must be larger than the minimum requirement of an image. 

Default disk size depends on the instance flavors. 

| Flavors | Supportive Device Size |
| ----------------| -------------------------- |
| Type u2 | Fixed at 20 ~ 100GB: vary by flavors    |
| Types t2, m2, c2, r2, and x1 | 20 ~ 1000 GB               |

>[Note] As it is charged by the size of a device, making a large size regardless of usage may be inefficient. It is recommended to add block storages, depending on further needs. 


### Volume Type
Determines default disk type of an instance.

- Chooose one of **HDD** or **SSD** based on your performance requirement. This changes the price of the instance.
- Can not change the volume type of the instance once created.

### Number of Instances 

This feature applies when creating many instances with the same image, availability zone, flavors, device size, key pair, and network setting. Instance name is created with numbers attached to each name, like `-1`, and `-2`. For instance, creating two instances named `my-instance` shall result in  `my-instance-1 `and `my-instance-2`. 

When many instances are created for a random availability zone, each instance shall be created in a random availability zone. For instance, if two instances are created with a random availability zone, two may be created in the same zone, or in different zones. In case all instances need to be created in a same zone, select a particular zone before creation. 

### Key Pair

Use an old key pair or create a new key pair. To register old key pairs,refer to [Import Key Pair (Windows)](./console-guide/#_16) for Windows users, and [Import Key Pair (Mac and Linux)](./console-guide/#_17) for Mac and Linux users. 

### Security Group

Specify a security group where an instance is to be included. One instance can be included to many security groups, and in such case: 

- Can communicate via network with all instances included to each security group. Take cautious approach when specifying a security group for an instance with sensitive data from which unintentional access from other instances should be prevented. 
- Rules of each security group are combined and applied for the instance's external communication. 

For more details, refer to [Overview of VPC](/Network/VPC/en/overview/)

### Network 

Select a subnet to be connected to an instance, among those defined in VPC. Every time a subnet is selected, a network interface is created to be connected to the subnet. The order of selected subnets may be changed to alter network interfaces, in which case, the first network interface  (`eth0`) shall be set as the default gateway. 

For more details on creating and managing network, refer to [Overview of VPC](/Network/VPC/en/overview/).

### Floating IP

After instance is created, specify whether to use floating IP. When it is enabled, create and associate a new floating IP to the first network interface. Note that the first network interface must be connected to a subnet where Internet gateway is configured.

Floating IP can be managed under Instance > Management, or Instance > Floating IP. For more floating IP details, see [VPC Console Guide](/Network/VPC/ko/console-guide/).

### Additional Block Storage

After instance is created, specify whether to attach block storage. When it is enabled, create a new block storage, apart from a basic disk, and attach it to an instance. Name, storage type, and size of it can be specified, like in a basic disk.

Leave the basic disk only for OS purposes, and use additional disk to store frequently-used application programs or data. Then, it gets easier to migrate or copy, by attaching/detaching block storage or via snapshots. In addition, when an instance has faults, detach the additional disk and associate it to another instance, so as to recover service easily.

Block storage can be managed under Instances > Block Storage. For more details on block storage, see [Block Storage Guide](/Storage/Block%20Storage/ko/overview/).

### Scheduled Script

After instance is created, specify a script to execute. Scheduled script is executed, after instance is first booted, followed by the initiazliation process, including network configuration. TOAST Scheduled Script is executed by automated tools, such as cloud-init (Linux), Cloudbase-init (Windows), which are embedded in official images.

> [Caution]
> Scheduled script is executed at the user's authority for root (Linux)/Administrator (Windows).

#### Linux
The first line of a scheduled script must start with  `#!`.
```
#!/bin/bash
...
```

To properly run a scheduled script, check log files within an instance. Find log outputs from standard output/error device of a script on `/var/log/cloud-init-output.log`.

#### Windows

Windows images support both Batch and Powershell, for the scheduled script type. Each type is delimited by specified indicator of the first line.

* Batch Script
```
rem cmd
...
```

* PowerShell Script
```
#ps1_sysnative
...
```

To use both Batch and Powershell script, describe as below:

* EC2 format
```
<script>
...
</script>
<powershell>
...
</powershell>
```

Find logs of a scheduled script at `C:\Program Files\Cloudbase Solutions\Cloudbase-Init\log\cloudbase-init`.

For more details regarding scheduled scripts, see guides for [cloud-init](https://cloudinit.readthedocs.io/en/latest/topics/format.html) or [Cloudbase-init](https://cloudbase-init.readthedocs.io/en/latest/userdata.html).


## Further Functions of Instances 

### Create Image 

Create an image from default disk of an instance. It is recommended to close instances before creating an image, so as to ensure data integrity. 

Even when a default disk has no additional space, image can be created but normal usage is impossible as initialization to use the image in other instances is unavailable. Before creating an image, empty at least 100KB of space.   

Image, once created, is registered as Private Image at Compute > Image. The image can be used to create an instance which has the same disk as the original instance. 

### Associate/Disassociate Floating IP

Floating IP can be associated or disassociated, regardless of the instance status. When there is no available floating IP, create one by clicking **Create**. Or go to **Network > VPC > Floating IP** to create a floating IP.  

For more details, refer to [Overview of VPC](/Network/VPC/en/overview/). 

### Modify Security Group

Security groups of an instance can be modified, regardless of instance status: modified security groups are immediately applied. 

For more details, refer to [Security Group](./console-guide/#_8) and [Overview of VPC](/Network/VPC/en/overview/). 

### Modify Network Interface 

Network interfaces may be added or deleted, regardless of instance status. When a network interface is associated with a floating IP, the floating IP is disassociated along with interface deleted. Once disassociated, a floating IP cannot be automatically associated even by adding the same network interface again: add network interface first and associate the the floating IP again.  

### Change Instance Flavors 

Change of instance flavors is enabled when an instance is closed. If an instance is currently executed, click **Close Instances** in **Further Functions** and close the instance.   

Instance flavors are allowed to change, depending on the current flavors.  

* Instances with types m2, c2, r2, t2, and x1 can be changed to have flavors of types m2, c2, r2, t2, and x1. 
* Instances with types m2, c2, r2, t2, and x1 cannot be changed to have flavors of type u2. 
* Type u2 cannot change flavors after created: not even to those of the same u2 type. 

If an instance changes its flavors, Change and Confirm Change are followed. When all tasks are completed, the VM status changes into **Shutoff**, and you can start an instance by clicking **Start Instance** in **Further Functions**. 

> [Note] Default disk size of an instance cannot be changed. If an instance has no sufficient disk space, add a block storage: for detailed method, refer to [Overview of Block Storage](/Storage/Block%20Storage/en/overview/). 

Instances shall be charged by changed flavors, as of the time of change. 

## Key Pair

### Import Key Pairs (Windows)

To create key pair, use Puttygen, which is installed along with PuTTY ssh client, and register it to TOAST. Install [PuTTY](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html).

Run puttygen. 
![이미지1](http://static.toastoven.net/prod_instance/putty-ssh-001-en.png)
Select RSA (or SSH-2 RSA in the older version of puttygen) in **Parameters**. Press **Generate**, in **Actions**. To create a key, continue to move the mouse in an empty space.  

When a key is created, public key file shows as below. Copy and paste the whole content of public key to **Public Key of Import Key Pairs:** register key pair. 
![이미지1](http://static.toastoven.net/prod_instance/putty-ssh-002-en.png)
Click **Save private key** of **Actions** and save the private key. Try saving private key with Encrypted Phrases empty, and a message will show, "*Want to save this key without protected by encrypted phrases?"*. To save more safely, set an encrypted phrase before saving.

> [Caution]
To set automatic login to an instance, encrypted phrases should not be used. When encrypted phrases are in use, directly enter password to a private key to login. 

Key pair, once registered, can be used to create an instance, and its private key is required to access an instance. On how to access instances, refer to [Overview of Instances](./overview/#_9). 

Like key pairs created from TOAST, these key pairs need to be cautiously managed as their private keys, when exposed, may be abused by anyone to access instances. 



### Import Key Pairs (Mac and Linux)

Key pairs created by `ssh-keygen` of Mac or Linux can be registered for usage. Key pair is created with the following command:

	$ ssh-keygen -t rsa -f my_key.key

Passwords for key pair may or may not be set, although it is recommended to set passwords for high-level of security. Inside the file with  `.pub` added in the key pair name, lies a key pair public key. 

	$ cat my_key.key.pub
	ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCnnUAe36txQqk8J7VzbNuYKVQQ3gbNoClndHMX49OD+1Rw5xrDFLUKQqxbBDtlNMoA9tKBZNrQBpKr1kFEtvMIj1HPkH9ocb4MbuoVVjpkIhixbKMMJPDQ4JQJxaifsjR59YsZyDAp0aXZp+o+OB97P3S4AKPY2kQR0JdSr30+6Av6smf+3mZceAE4abzklfbyWT5slP1im/wfYEPO3QBEDl/0JbmTjKWPYI6QnbwnPRHS63SJ+Kd2QeYQYJCadv7X4mXnw81qEIWq/dx1SQkGDTNgR7lnN2ApFlU5EZcow69z6tiCr0hlyigwjGooMg3wTZvcSlYcVeTzZ755RArd ...

Copy the whole content to **Open Key** of **Import Key Pairs:** to register a key pair. 

Key pair, once registered, can be used to create an instance, and its private key is required to access an instance. On how to access instances, refer to [How to Access Instances](./overview/#_9).   

Like key pairs created from TOAST, these key pairs need to be cautiously managed as their private keys, when exposed, may be abused by anyone to access instances. 

## Appendix 1. Change of Windows Language Packs 

TOAST Cloud primarily supports English for Windows display. You may change your language preference, as follows:  

1. Go to **START > Control Panel > Clock, Language, and Region > Add a language**.

![이미지1](http://static.toastoven.net/prod_instance/windows1.png)

2. Select **Change your language preferences > Add a language**.

![이미지1](http://static.toastoven.net/prod_instance/windows2.png)

3. Choose a language in **Add a language** and click **Add**.

![이미지1](http://static.toastoven.net/prod_instance/windows3.png)

4. Check the language pack just added. 

![이미지1](http://static.toastoven.net/prod_instance/windows4.png)

5. Download and install the language pack. 

![이미지1](http://static.toastoven.net/prod_instance/windows5.png)

6. Download and install updates. 

![이미지1](http://static.toastoven.net/prod_instance/windows6.png)

7. To change the installed language pack, double-click the selected language or select **Options**.

![이미지1](http://static.toastoven.net/prod_instance/windows7.png)

8. Choose **Make this the primary language** for Windows display language.

![이미지1](http://static.toastoven.net/prod_instance/windows8.png)

9. To apply the change of primary language, click **Log off now**.

![이미지1](http://static.toastoven.net/prod_instance/windows9.png)

10. Log in again, and you can see Windows is displayed by the language pack of your choice.

![이미지1](http://static.toastoven.net/prod_instance/windows10.png)

## Appendix 2. Change of Windows Routing 

Windows routing for TOAST Cloud can be changed as follows:  


* Press **Windows key + R** to open an execution window, and enter`cmd` and execute to open a command prompt window.  
  Enter route command.  
* Print current configuration: route print
* Add: route add "Destination" mask "subnet" "gateway" metric "Metric value" if "Interface number"
* Change: route change "Destination" mask "subnet" "gateway" metric "Metric value" if "Interface number"
* Delete: route delete "Destination" mask "Destination subnet" "gateway" metric "Metric value" if "Interface number"
  * Option : -p (specify as permanent route)

Description
![이미지1](http://static.toastoven.net/prod_instance/windows_route1.png)

* Metric Value: The lower, the higher on the priority
* Interface Number: Available on route print (in the red box above)
* Permanent Route: Use the -p option, because otherwise, route configuration returns to default at the system reboot (in the blue box above)

Example 1 - Setting external communication for particular interfaces only 
* Use command for route change to modify metrics on interface route to which external communication is not wanted, or not enter default gateway information on the fixed IP setting. 

* How to Modify Metric
  * Increase metrics of an interface 

  $ route change 0.0.0.0 mask 0.0.0.0 172.16.5.1 metric 10 if 14 -p
  ![이미지1](http://static.toastoven.net/prod_instance/windows_route2.png)
  How to Set Fixed IP

  * Enter ipconfig /all to find IP information.
    ![이미지1](http://static.toastoven.net/prod_instance/windows_route3.png)
  * Enter IP information, excluding default gateway, on the window for IP settings. 
    ![이미지1](http://static.toastoven.net/prod_instance/windows_route4.png)
  * Check route print.
    ![이미지1](http://static.toastoven.net/prod_instance/windows_route5.png)
    Example 2 - Setting route for a particular bandwidth
  * Command route add to set route for a particular bandwidth.

  $ route add 172.16.0.0 mask 255.255.0.0 172.16.5.1 metric 1 if 14 -p
  ![이미지1](http://static.toastoven.net/prod_instance/windows_route6.png)
  Example 3 - Removing a particular route 

  * Remove specified route, by using route delete. 

  $ route delete 172.16.0.0 mask 255.255.0.0 172.16.5.1
  ![이미지1](http://static.toastoven.net/prod_instance/windows_route7.png)

## Appendix 3. Change of System Locale 

You can change system locale  for TOAST Cloud on Windows as follows. 

* Go to **Windows Key > Control Panel > Clock and Country**.

![이미지1](http://static.toastoven.net/prod_instance/win_locale1.png)

* Select the **Country or Region** item. 

![이미지1](http://static.toastoven.net/prod_instance/win_locale2.png)

* On the **Administrative Option** tab, click **Change System Locale**.

![이미지1](http://static.toastoven.net/prod_instance/win_locale3.png)

* Select a system locale to change. 

![이미지1](http://static.toastoven.net/prod_instance/win_locale4.png)

* Restart the system to apply the change. 

![이미지1](http://static.toastoven.net/prod_instance/win_locale5.png)