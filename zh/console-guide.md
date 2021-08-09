## Compute > Instance > 控制台使用指南

## 创建实例

### 镜像

请选择安装操作系统所需的镜像。镜像可以从NHN Cloud提供的公共镜像、已创建的用户自定义镜像、共享镜像中选择。

根据您使用的镜像，实例规格（flavor）会有所不同，因此创建实例时请先选择镜像后再进行后面的操作。

| 操作系统                        | 磁盘     | 内存   |
| -------------------------------- | ---------- | -------- |
| Linux<br>CentOS, Ubuntu, Debian | 20GB以上 | 1GB以上 |
| Windows                           | 50GB以上 | 2GB以上 |


### 可用区域(availability zone)

如果未明确设置可用区域，则将其设置为任意区域。根据可用区域决定此实例可用的块存储。如果要使用的块存储存在于特定的可用区域中，请将其设置为可用区域后使用。

> [参考] VPC的资源可以在所有可用区域中使用。

有关可用性区域的详细说明请参考[实例概述中的可用区域](./overview/#availability-zone)。

### 规格(flavor)

您可以根据虚拟硬件的性能来选择各种规格。但是，根据镜像要求的虚拟硬件性能，可选择的规格会受到限制。有关详细说明请参考[实例概述](./overview)。

实例在创建后，可以在NHN Cloud控制台更改实例规格。可以从高规格降配为低规格，也可以从低规格升配为高规格。但部分规格是无法更改的，有关详细说明请参考[实例规格的更改](./console-guide/#_15)。 
> [注意] 实例的默认系统盘无法通过规格更改来变更。

### 磁盘空间

确定实例的默认系统盘大小。

- 创建实例后无法更改默认系统盘的大小。如果实例的磁盘容量不足，则需添加块存储。
- 磁盘的空间至少是镜像所需的最小容量。

实例的默认系统盘大小取决于实例的规格。

| 规格            | 支持的设备大小         |
| ----------------| -------------------------- |
| u2 类型           | 固定为20G，无法更改 |
| t2, m2, c2, r2, x1 类型 | 20 ~ 2000GB               |

>[参考] 根据磁盘空间的大小决定实际支付金额。因此磁盘的容量不要设置太大。建议您根据需求添加块存储。

### 卷类型

确定实例默认的磁盘类型。

- 选择**HDD** 或 **SSD**，类型不同，价格和性能也将不同。
- 确定选择后，无法更改磁盘类型。

### 实例数

创建多个镜像、可用区域、规格、磁盘容量、秘钥、网络设置相同的实例时使用实例数。此时的实例名称在设置的名称后面加数字`-1`, `-2`。例如，创建2个名为`my-instance`的实例，则会创建`my-instance-1`, `my-instance-2`的实例。一次可以创建的最多实例数为10个。
如果在任意的可用区域创建了多个实例，每个实例则创建在任意的可用区域。例如，2个实例创建在任意的可用区域时，此2个实例可能会在同一个可用领域，也可能在不同的可用区域。因此，所有实例需要在同一个可用区域时，请选择特定的可用区域后创建。

### 密钥对

您可以使用现有的密钥对或创建新的密钥对后使用。现有秘钥的注册方法，Windows用户请参考 [导入密钥对 (Windows用户)](./console-guide/#windows_1)、Mac及Linux 用户请参考[导入密钥对(Mac、Linux用户)](./console-guide/#maclinux)。

### 网络

在VPC定义的子网中选择连接实例的子网。每当选择一个子网时，都会在实例中创建连接到该子网的网络接口。您也可以更换被选择的子网的顺序来更改网络接口。此时，第一个网络接口(`eth0`)默认为网关。

网络创建及管理相关详细说明请参考[VPC概述](/Network/VPC/zh/overview/)。

### 浮动IP

创建实例后，指定是否使用浮动IP。若选择使用浮动IP，新建浮动IP并连接至第一个网络接口。此时，第一个网络接口必须连接至设置了互联网网关的子网。

浮动IP管理也可在实例 > 管理页面或实例 > 浮动IP页面内设置。有关浮动IP的更详细的信息参考[VPC控制台使用指南](/Network/VPC/zh/console-guide/)。

### 安全组

指定实例要所属的安全组。一个实例可以属于多个安全组。如果实例属于多个安全组时请注意以下事项。

- 属于各个安全组的所有实例都可以相互进行网络通信。对应具有敏感数据的实例，应防止其他实例的恶意访问，请慎重指定安全组。
- 聚合各安全组的所有规则并将其应用于该实例的外部通信。

安全组的详细说明请参考[VPC概述](/Network/VPC/zh/overview/)。

### 附加块存储

创建实例后，指定是否连接附加块存储。若选择使用附加块存储，创建与基本磁盘分开的新的块存储并连接至实例。与基本磁盘一样，创建附加磁盘时，可指定名称、存储类型、大小。

若仅将基本磁盘用于OS，且把经常使用的应用程序或数据保存在附加磁盘中的话，通过块存储连接/断开连接或快照功能，可轻松地迁移或复制。此外，发生实例问题时，仅断开与附加磁盘的连接后，再连接至其他实例，可轻松恢复服务。

也可以在实例 > 块存储页面管理块存储。有关块存储的更详细的说明参考[块存储指南](/Storage/Block%20Storage/zh/overview/)。

### 调度脚本

创建实例后指定要执行的脚本。调度脚本在完成实例的首次启动并结束网络设置等初始化过程后执行。NHN Cloud的调度脚本利用官方镜像内置的cloud-init (Linux)、Cloudbase-init (Windows)等自动化工具执行。

> [注意]
> 调度脚本以root (Linux)/Administrator (Windows)用户权限执行。

#### Linux
调度脚本的第一行必须以 `#!` 开头。
```
#!/bin/bash
...
```

为了正常运行调度脚本，应确认实例内部的日志文件。脚本中向标准输出/错误设备输出的日志可在“/var/log/cloud-init-output.log”中确认。

#### Windows

Windows镜像支持Batch脚本格式、Powershell脚本格式的调度脚本格式。各格式按照第一行列出的指令区分。

* Batch脚本
```
rem cmd
...
```

* PowerShell脚本
```
#ps1_sysnative
...
```

若欲同时使用Batch脚本及PowerShell脚本，按如下方式操作。

* EC2 format
```
<script>
...
</script>
<powershell>
...
</powershell>
```

调度脚本的日志可在`C:\Program Files\Cloudbase Solutions\Cloudbase-Init\log\cloudbase-init`中确认。

有关调度脚本的更详细的说明参考[cloud-init](https://cloudinit.readthedocs.io/en/latest/topics/format.html)或[Cloudbase-init](https://cloudbase-init.readthedocs.io/en/latest/userdata.html)指南。


## 实例添加功能

### 创建镜像

从实例的默认磁盘创建镜像。为保证数据整合性，建议在终止实例的状态下创建镜像。

实例的默认系统盘中没有空间的情况下也可以创建镜像，但是无法为了将镜像用于其他实例而进行初始化。在创建镜像之前请确保实例具有至少100KB的可用空间。

创建的镜像在**Compute > Image**中注册为 Private镜像。使用已注册的镜像可以创建与原始实例磁盘相同的实例。

### 弹性IP绑定和解绑

不管实例的状态如何，您都可以绑定和解绑弹性IP。 如果没有可用的弹性IP或没有您所需要的弹性IP，则可以通过点击**创建**按钮创建弹性IP后绑定。或者，您也可以在 **Network > VPC > Floating IP**中创建弹性IP。

有关弹性IP相关的详细说明请参考[VPC概述](/Network/VPC/zh/overview/)。

### 更改安全组

不管实例的状态如何，您都可以更改实例的安全组，更改后可立即应用。

有关安全组的详细说明请参考[安全组](./console-guide/#_8)及[VPC 概述](/Network/VPC/zh/overview/)。

### 更改网络接口
不管实例的状态如何，您都可以添加或删除实例的网络接口。删除网路接口时，如果弹性IP绑定了接口，弹性IP也会解绑。解绑过的弹性IP，即使再次添加相同的网络接口也不会再次自动绑定，因此添加网路接口后需要再次绑定弹性IP。

### 更改实例规格

更改实例规格前要关闭实例。如果实例正在运行，请点击**追加功能**的**关闭实例**。

根据当前的规格，可更改的实例规格不同。

* m2, c2, r2, t2, x1类型的实例可以更改为m2, c2, r2, t2, x1类型的实例规格。
* m2, c2, r2, t2, x1类型的实例无法更改为u2类型的实例规格。
* u2类型与I类型相同，创建后无法更改规格。也无法更改为同样u2类型的实例规格。

更改实例规格时需要进行更改及验证更改操作。当所有的操作结束后VM状态更改为**Shutoff**状态，点击**追加功能**的 **Start instance**按钮即可启动实例。

> [参考]实例的默认系统盘大小无法更改。如果实例的磁盘空间不足，请添加块存储。有关块存储的添加方法请参考[块存储概述](/Storage/Block%20Storage/zh/overview/)。

付费时根据实例的更改时间为基准，按照更改后的规格来支付。

## 密钥对

### 导入密钥对(Windows用户)

安装PuTTY SSH客户端的同时会自动安装puttygen程序，利用此程序可以创建密钥对并将其注册到NHN Cloud后使用。

设置[PuTTY](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html)或运用韩语补丁的[iPuTTY](https://github.com/iPuTTY/iPuTTY/releases/tag/l0.70i)。 

启动puttygen。

![图1](http://static.toastoven.net/prod_instance/putty-ssh-001-en.png)

在**变量**中 选择**RSA**(或旧版本puttygen中的SSH-2 RSA)。点击 **操作** 的**创建**按钮。继续在空白区域移动鼠标以生成秘钥。

创建秘钥后，公钥文件内容如下所示。之后将整个公钥内容粘贴到**导入密钥对**中的 **公钥:** 字段以注册密钥对。

![图1](http://static.toastoven.net/prod_instance/putty-ssh-002-en.png)

点击**操作**中的**保存私密**按钮以保存私密。如果秘钥密码字段为空，点击保存时会出现 **确定不设置秘钥密码保护吗?**的提示。因此为了更安全地使用转换后的私钥，请设置私钥密码后保存。

> [注意]
如果想自动登录到实例，请不要使用私钥密码。如果使用了私钥密码，则必须在登录时手动输入秘钥的密码。

已注册的密钥对可以在创建实例时使用。访问实例时则需要此密钥对的私钥进行访问。有关实例的访问方法请参考[实例概述](./overview/#_5)。

与在NHN Cloud创建的密钥对一样，利用此方式创建的密钥对的私钥如果一旦泄露，任何人都可以利用泄露的私钥访问实例，因此请慎重管理私钥。



### 导入密钥对(Mac、Linux用户)

利用Mac或Linux的`ssh-keygen`来创建的密钥对可以在NHN Cloud上注册并使用。使用以下命令创建密钥对。

	$ ssh-keygen -t rsa -f my_key.key

您可以设置密钥对的密码，但不设置也不影响使用。为提高安全性，建议您设置密码。您输入的密钥对名称会自动追加.pub为扩展名,该文件里有公钥.
	$ cat my_key.key.pub
	ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCnnUAe36txQqk8J7VzbNuYKVQQ3gbNoClndHMX49OD+1Rw5xrDFLUKQqxbBDtlNMoA9tKBZNrQBpKr1kFEtvMIj1HPkH9ocb4MbuoVVjpkIhixbKMMJPDQ4JQJxaifsjR59YsZyDAp0aXZp+o+OB97P3S4AKPY2kQR0JdSr30+6Av6smf+3mZceAE4abzklfbyWT5slP1im/wfYEPO3QBEDl/0JbmTjKWPYI6QnbwnPRHS63SJ+Kd2QeYQYJCadv7X4mXnw81qEIWq/dx1SQkGDTNgR7lnN2ApFlU5EZcow69z6tiCr0hlyigwjGooMg3wTZvcSlYcVeTzZ755RArd ...

将整个内容粘贴到**导入密钥对**的**公钥:**字段中以注册密钥对。

已注册的密钥对可以在创建实例时使用。访问实例时则需要此密钥对的私钥进行访问。实例的访问方法请参考[实例概述](./overview/#_5)。

与在NHN Cloud创建的密钥对一样，利用这种方式创建的密钥对的私钥如果一旦泄露，任何人都可以利用泄露的私钥访问实例，因此请慎重管理私钥。

## 附录 1.更改Windows语言包

NHN Cloud云Windows图像默认提供英文版。默认使用其他语言的方法如下。

1.选择**START > Control Panel > Clock, Language, and Region > Add a language**。

![이미지1](http://static.toastoven.net/prod_instance/windows1.png)

2.选择**更改语言默认设置 > 添加语言**。

![이미지1](http://static.toastoven.net/prod_instance/windows2.png)

3.在**添加语言(Add a language)**中选择想要使用的语言，单击**添加(Add)**。

![이미지1](http://static.toastoven.net/prod_instance/windows3.png)

4.确认添加的语言包。

![이미지1](http://static.toastoven.net/prod_instance/windows4.png)

5.下载并安装添加的语言包。

![이미지1](http://static.toastoven.net/prod_instance/windows5.png)

6.下载并安装更新。

![이미지1](http://static.toastoven.net/prod_instance/windows6.png)

7.若欲更改安装的语言包，双击选择的语言，或选择**选项(Options)**。

![이미지1](http://static.toastoven.net/prod_instance/windows7.png)

8.在语言选项中选择**设置为默认语言**。

![이미지1](http://static.toastoven.net/prod_instance/windows8.png)

9.若欲应用默认语言设置，单击**现在注销(Log off now)**。

![이미지1](http://static.toastoven.net/prod_instance/windows9.png)

10.再次登录即可确认已更改为用户选择的语言包。

![이미지1](http://static.toastoven.net/prod_instance/windows10.png)

## 附录 2.更改Windows路由

在NHN Cloud云Windows中更改路由的方法如下。


* 按**Windows键 + R**打开运行窗口后输入`cmd`并执行，打开命令提示符窗口。
  输入Route命令。
* 输出当前设置：route print
* 添加：route add “目的地” mask “subnet” “gateway” metric “Metric值” if “Interface号"
* 更改：route change “目的地” mask “subnet” “gateway” metric “Metric值” if “Interface号"
* 删除：route delete “目的地” mask “目的地subnet” “gateway” metric “Metric值” if “Interface号"
  * 选项：-p（指定永久路径）

说明
![이미지1](http://static.toastoven.net/prod_instance/windows_route1.png)
* Metric值：值越小，优先顺序越高
* Interface号：可在route print中确认（上图中红色边框）
* 永久路径：若不使用-p选项，重启系统时设置的路径会初始化，因此使用（上图中蓝色边框）

案例1 - 仅特定接口设置外部通信
* 有使用route change命令更改不需要外部通信的接口路径的metric，或不在固定IP设置中输入网关信息的方法等。

* Metric更改方法
  * 增加接口的metric

  $ route change 0.0.0.0 mask 0.0.0.0 172.16.5.1 metric 10 if 14 -p
  ![이미지1](http://static.toastoven.net/prod_instance/windows_route2.png)
  固定IP设置方法
  * 输入ipconfig /all，确认IP信息。
    ![이미지1](http://static.toastoven.net/prod_instance/windows_route3.png)
  * 利用确认的IP信息，在IP设置窗口中输入，不包括默认网关。
    ![이미지1](http://static.toastoven.net/prod_instance/windows_route4.png)
  * 利用route print确认。
    ![이미지1](http://static.toastoven.net/prod_instance/windows_route5.png)
    案例2 - 设置特定段的路径
  * 利用route add命令设置指定段的路径。

  $ route add 172.16.0.0 mask 255.255.0.0 172.16.5.1 metric 1 if 14 -p
  ![이미지1](http://static.toastoven.net/prod_instance/windows_route6.png)
  案例3 - 删除特定路径

  * 使用route delete删除指定的路径。

  $ route delete 172.16.0.0 mask 255.255.0.0 172.16.5.1
  ![이미지1](http://static.toastoven.net/prod_instance/windows_route7.png)

## 附录 3.更改系统Locale

在NHN Cloud云Windows中更改系统Locale的方法如下。

* 选择**Windows键 > 控制面板 > 时间及国家**。

![이미지1](http://static.toastoven.net/prod_instance/win_locale1.png)

* 选择**国家或地区**。

![이미지1](http://static.toastoven.net/prod_instance/win_locale2.png)

* 在**管理员选项**选项卡中单击**更改系统Locale**。

![이미지1](http://static.toastoven.net/prod_instance/win_locale3.png)

* 选择要更改的系统Locale。

![이미지1](http://static.toastoven.net/prod_instance/win_locale4.png)

* 若欲应用，重启系统。

![이미지1](http://static.toastoven.net/prod_instance/win_locale5.png)

## 附录 4.用于维护管理程序的实例重启指南
NHN Cloud定期升级管理程序软件，提升基本基础设施服务的安全性及稳定性。
驱动中的实例应通过重启从维护对象管理程序移动至完成维护的管理程序。

若欲重启实例，应通过控制台使用实例名前生成的**! 重启**按钮。
`实例无法通过控制台中的实例重启或操作系统的重启功能移动至其他管理程序。`
请按照以下指南使用控制台中的重启功能。

移动至被指定为维护对象的实例所在的项目。

**1. 确认维护对象实例。**

实例名前有**! 重启**按钮的实例是维护对象实例。
将鼠标光标放在**! 重启**按钮上，可确认详细的维护日程。
![实例维护图像1](http://static.toastoven.net/prod_instance/instance_p_migration_zh_1.png)    

**2.禁用或结束维护对象实例中驱动中的应用程序**

禁用或结束维护对象实例中驱动中的应用程序时，应采取措施避免影响服务。 
不可避免地影响服务时，联系NHN Cloud客服中心，将为您介绍合适的措施。

**3.单击维护对象实例名旁生成的[! 重启]按钮。**

![实例维护图像2](http://static.toastoven.net/prod_instance/instance_p_migration_zh_2.png)

**4.当询问是否重启实例的窗口跳出时，单击[确定]按钮。**

![实例维护图像3](http://static.toastoven.net/prod_instance/instance_p_migration_zh_3.png)

**5.实例状态显示灯变为绿色，等待至[! 重启]按钮消失。**

若实例状态显示灯未改变或**! 重启**按钮未禁用，请进行“刷新”。


实例重启过程中，无法对相应实例进行任何操作。
若实例重启未正常完成，将自动向管理员报告，NHN Cloud会另行联系您。
