## Compute > Instance > Overview 

An instance refers a virtual server composed of a virtual CPU, memory, and a default disk: with customer's services or applications installed to it, various NHN Cloud service combinations are applied.  

## Components  

An instance is composed of the following: 

- **Image**: Virtual disk that has an operating system
- **Flavors**: Virtual hardware performance of an instance  
- **Availability Zone** (AZ): Physical location where an instance is to be made 
- **Key Pair**: Key used to access an instance
- **Security Group**: Security setting for an instance network 
- **Network**: Virtual network where an instance is to be connected

Flavors and method of use change depending on the above information. Settings, except image and availability area, may change after an instance is created. Some flavors cannot be changed once an instance is created. For more details on change of instance flavors, refer to [Change of Instance Flavors of User Guide on Console](./console-guide/#change-instance-flavors).

### Image 

Image is a virtual disk that has an operating system. NHN Cloud currently supports CentOS, Debian, Ubuntu, and Windows. For more information about the versions we support, refer to [NHN Cloud Service](https://toast.com/service/compute/instance).

All images are configured for optimal execution under virtual hardware of an instance and are safe to use as they have been authenticated by NHN Cloud security. For more details, refer to [Overview of Image](/Compute/Image/en/overview/).

### Flavors 

NHN Cloud provides instance flavors in various types to suit for customer's needs. Depending on the service to run or feature of an application, instances with appropriate flavors can be created.  Flavors can be easily modified on a web console .

| Type   | Description                              |
| ------ | ---------------------------------------- |
| m2 Type | A balanced flavors between CPU and memory. <br />Recommended when performance requirements of a service or an application are not clear. |
| c2 Type | CPU performance is set highly: applied for an web application server or an analysis system requiring high-performance calculation. |
| r2 Type | Recommended when memory usage volume is high, compared to other resources. Generally applied to memory database or cache server. |
| t2 Type | Low-cost instance. Recommended for the servers with low workloads. |
| u2 Type | The cheapest instance. Recommended for servers with low workloads. <br>Using local disks poses a risk of low stability compared to other instances, but it is more affordable. <br>This type of instance does not ensure I/O performance. |
| x1 Type | The flavor supports high-end CPU and memory. It can be applied to services or applications that require high performances. |

### Availability Zone 

NHN Cloud divides the whole system into many availability zones to prepare against potential failure owing to physical hardware issues.  Each availability zone has its own storage system, network switch, raised floor, and power devices. Failure occurred within an availability zone does not affect other zones, so as to raise availability of a whole service. If an instance is built throughout many availability zones, then service availability should raise even more.   

Following features exist between different availability zones.

- Network communication is available between instances that are dispersed and created in many availability zones, and costs for network usage are not charged. 
- Block storage can be shared between instances created in a same availability zone, but not between different availability zones. 
- Floating IP can be shared between different availability zones. If one zone has a failure, the floating IP can be relocated to another zone, so as to minimize failure duration. 


### Key Pair 

Key pair is a pair of a [PKI](https://en.wikipedia.org/wiki/Public_key_infrastructure)-based open SSH key and a private key. To access an instance created in NHN Cloud, key pair is required, instead of ID/PW authentication based on keyboard strokes which is vulnerable to security attacks. Using the private key of a key pair, user encodes and sends login information to an instance to get access authentication, and then safely access the instance. On how to access instances using key pair, refer to [How to Access Instances](#how-to-access-instances). 

Key pairs can be newly created in the NHN Cloud console when an instance is created, or may be registered as made by customer. For how to register key pairs, refer to [Getting Key Pair from Console Guide](./console-guide/#key-pair_1). 

> [Caution]
> When a key pair is newly created, its private key is downloaded. As a private key is not issued no more than once, be sure to store downloaded private keys in a safe disk or USB drive. When a private key is exposed, anyone can access instances, so a cautious approach is required.  

### Security Group 

Security Group is a virtual firewall that determines network traffic delivered to an instance. For more details on Security Group, refer to [Overview of VPC](/Network/VPC/en/overview/). 

> [Note]
> Security groups are designed to ignore all inbound network traffic. Before accessing instance with SSH, configure to open SSH port to the security group that has the instance.  

### Network 

For external communication, an instance must be connected to at least one network defined in VPC. Cannot access an instance which is not connected to a network. To create or change a network, refer to [Overview of VPC](/Network/VPC/en/overview/). 

## Charge 

Instance shall be charged as follows: 

* Charging instances begin from the moment they are created.
* Default instance disk is charged by the block storage charging criteria,
* Closed instances are charged by default instance disk and block storage.


## How to Access Instances 

### How to Access Linux Instances 

Use ssh client to access Linux instances. Access is not available if ssh access port (default 22) is not open to an instance security group. Refer to [Overview of VPC](/Network/VPC/en/overview/) on how to allow ssh access. If a floating IP is not assigned to an instance, access from outside of NHN Cloud is unavailable. Refer to [Overview of VPC](/Network/VPC/en/overview/) on how to assign a floating IP.  

#### How to Access Linux Instances with ssh Client for Mac or Linux 

Generally, Mac or Linux has ssh client by default. Use a private key of key pair as below to access from ssh client: 

Access for CentOS instances: 

	$ ssh -i my_private_key.pem centos@<instance IP>

For Ubuntu instances: 

	$ ssh -i my_private_key.pem ubuntu@<instance IP>

And for Debian instances:  

	$ ssh -i my_private_key.pem debian@<instance IP>

#### How to Access Linux Instances with PuTTY ssh Client for Windows 

PuTTY ssh client is a widely used ssh client program for Windows.  Install [PuTTY](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html).

Access from Windows to Linux instances via PuTTY ssh client requires three stages: 

* Change private key of key pair to that of PuTTY 
* Register private key for PuTTY
* Access instances with PuTTY 


##### 1. Change Private Key of Key Pair to That of PuTTY

In PuTTY, private key of key pair should be changed into the private key format of PuTTY. To convert keys, use puttygen which is installed along with PuTTY.  

![이미지1](http://static.toastoven.net/prod_instance/putty-ssh-001-en.png)

Set RSA for the key format at Parameters at the bottom, and enter 2048 bits, which is default, for the Key Bits to Create. Click **Load** next to **Load an existing private key file** to import the key file of a key pair.  

![이미지2](http://static.toastoven.net/prod_instance/putty002-en.png)

Click **Save private key** next to **Save the generated key** and save the changed private key of a key pair, for PuTTY.  Try saving private key with Encrypted Phrases empty, and a message will show, "*Are you sure you want to save this key without a passphrase to protect it?*". To save more safely, set an encrypted phrase before saving.  

> [Caution]
> To set automatic login to an instance, encrypted phrases should not be used. When encrypted phrases are in use, directly enter password to a private key to login. 

##### 2. Register Private Key for PuTTY to PuTTY

Private keys for PuTTY can be registered in the next two methods: 

* By registering an authenticated private key file in PuTTY
* By registering an authenticated private key file at pageant (PuTTY's authentication agent)

**A. Registering Authenticated Private Key File in PuTTY**


Execute PuTTY and select  **Connection > SSH > Auth** from the category on the left. Register a private key for PuTTY in the **Private key file for authentication** on **Authenticated parameters** on the right.  (in reference of the screen shot below )

![이미지3](http://static.toastoven.net/prod_instance/putty005-en.png)

If you save access information after private key is registered, there is no need to re-register private key files every time: the method is described as below. 

**B. Registering authenticated private key files in pageant (PuTTY authentication agent)**


Run pageant, which is installed along with puTTY, and icons as below are created in the Windows tray. Right-click to select **Add Key** and add a private key for PuTTY. 

![이미지4](http://static.toastoven.net/prod_instance/putty006.png)

To confirm the key added, select **View Keys**. If successful, the added key is displayed as below. 

![이미지5](http://static.toastoven.net/prod_instance/putty008-en.png)

Pageant, once executed, remains in the Windows tray and runs, and there is no need to execute it at every access to an instance: unless, however, your Windows starts anew. 

##### 3. Access

Now that the PuTTY private key is registered well, execute Putty. 

![이미지6](http://static.toastoven.net/prod_instance/putty009-en.png)

**Host Name** of default access information for CentOS is: 

	centos@<Instance IP>

For Ubuntu,

	ubuntu@<Instance IP>

And for Debian,

	debian@<Instance IP>

Specify 22 for the port, and SSH for the format of connection. 

If all information is right, save the session. Enter the session name to save in lower fields of **Load, save, or delete stored session** and click **Save**. Otherwise, private key setting registered in 2-A cannot be maintained.  

Now click **Open** to access to instance. 


### How to Access Windows Instances 

To access a Windows server, select a Windows instance to access from NHN Cloud console. Click **Check Password** in the **Instance Access** tab from screen details at the bottom to check password set in the Windows server. 

![Access to Windows Instance](http://static.toastoven.net/prod_instance/windows-login-en.png)

Private key of key pair required to check password is not transmitted to the server but used only to decrypt password at the browser. 

Click **Connect** next to **Check Password**, receive the rdp file containing setting for remote desktop access, and execute, to get access to Windows server.  `Administrator` is the ID for the Windows server and use the password confirmed in the NHN Cloud console. 
