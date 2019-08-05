## Compute > Instance > API向导

TOAST Compute Instance服务提供以下类型的API。

* [可用区API](#api_2)
* [实例API](#api_3)
* [实例附加功能API](#api_4)
* [实例规格API](#api_5)
* [密钥对API](#api_6)


## 事前准备

如果要使用实例 API，您需要一个appkey和一个令牌。通过[API Endpoint URL](#api-endpoint-url)和[令牌API](#api)准备appkey和令牌。Appkey包含在API Endpoint URL中，令牌包含在Request Body中。

### API Endpoint URL

所有实例，镜像，网络（VPC）和块存储API都应使用以下URL作为前缀。

	https://api-compute.cloud.toast.com/compute

请求API时，必须将appkey包含在您的请求中。您可以获得一个APK密钥，如下所示。

1. 在TOAST控制台**计算**页面上方单击**URL & Appkey**。
2. 在**URL & Appkey**对话框中复制并使用**Appkey**值。

例如，令牌发布URL如下。

	POST https://api-compute.cloud.toast.com/compute/v1.0/appkeys/{appkey}/tokens

在实例，镜像，网络和块存储API文档中，我们省略了API URL前缀，以呈现简洁，易于直观的效果。

### API Response

#### Response HTTP Status Code

针对所有API请求，都均以200 OK应答，并包含JSON格式的Response Body。

#### Response Body

Response Body默认包含"header"信息，因此，您可以看到详细的响应结果。不同API，可以包含"header"以外的其它信息。

```json
{
    "header" : {
        "isSuccessful" : true,
        "resultCode": 0,
        "resultMessage" : "SUCCESS"
    }
}
```

如果API调用失败，`isSuccessful`变为`false`，错误代码将显示在`resultCode`中。详细的错误代码请参考[错误代码](/Compute/Instance/zh/error-code/)。

### 令牌API

如果要使用API，就必须获取令牌，进行身份认证。所有API都必须要在请求中添加**X-Auth-Token** Header进行请求。

#### API密码设置

在**Compute > Instance**服务页面单击**API Endpoint设置**按钮设置API密码。

1. 单击**API Endpoint设置**按钮。
2. 在**API Endpoint设置**下方的**API密码设置**中输入用于获取令牌的密码。
3. 输入密码后单击**保存**按钮。

#### 发布令牌

##### Method, URL
```
POST /v1.0/appkeys/{appkey}/tokens
Content-Type: application/json;charset=UTF-8
```


##### Request Body
```json
{
	"auth" : {
    	"username" : "{TOAST ID}",
        "password" : "{API Password}"
    }
}
```

| Name | In | Type | Optional | Description |
| -- | -- | -- | -- | -- |
| TOAST ID | Body | String | - | 输入TOAST账号ID(Email) |
| API Password | Body | String | - | 在**API Endpoint设置**保存的密码 |

##### Response Body
```json
{
    "header" : {
        "isSuccessful" :  true,
        "resultCode" :  0,
        "resultMessage" :  "SUCCESS"
    },
    "access" : {
        "token" : {
            "expires" :  "{Expires}",
            "id" :  "{Token ID}",
            "issued_at" :  "{Issued at}"
        },
        "user" : {
            "id" :  "{User ID}",
            "roles" : [
                {
                    "name" :  "{Role name}"
                }
            ]
        }
    }
}
```

| Name | In | Type | Description |
| -- | -- | -- | -- |
| Token ID | Body | String | 在发出API请求时，需要写入HTTP Header(X-Auth-Token)的UUID |
| Issued at | Body | String | 令牌发布时间。yyyy-mm-ddTHH:MM:ssZ格式。 例如) 2017-05-16T02:17:50.166563 |
| Expires | Body | String | 所发布令牌的到期时间。yyyy-mm-ddTHH:MM:ssZ格式。例如) 2017-05-16T03:17:50Z |
| User ID | Body | String | 获取令牌的用户的UUID |
| Role name | Body | String | 获取令牌的用户被赋予的角色 |

#### 令牌信息查询
##### Method, URL
```
GET /v1.0/appkeys/{appkey}/tokens?id={tokenId}
```

|  Name | In | Type | Optional |Description |
|--|--|--|--|--|
| tokenId | Query | String | - | 要查询的令牌ID |

##### Request Body
此API无需Request Body。

##### Response Body
```json
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    },
    "access": {
        "token": {
            "expires": "{Expires}",
            "id": "{Token ID}",
            "issued_at": "{Issued at}"
        },
        "user": {
            "id": "{User ID}",
            "roles": [
                {
                    "name": "{Role name}"
                }
            ]
        }
    }
}
```

| Name | In | Type | Description |
| -- | -- | -- | -- |
| Token ID | Body | String | 发出API请求时，用于HTTP Header(X-Auth-Token)的令牌UUID |
| Issued at | Body | String | 令牌发布时间。yyyy-mm-ddTHH:MM:ssZ格式。例如) 2017-05-16T02:17:50.166563 |
| Expires | Body | String | 所发布的令牌到期时间。yyyy-mm-ddTHH:MM:ssZ格式。例如) 2017-05-16T03:17:50Z |
| User ID | Body | String | 获取令牌的用户的UUID |
| Role name | Body | String | 获取令牌的用户被赋予的角色 |


## 可用区API

### 可用区查询
查询可以创建实例、块存储的可用区信息。

#### Method, URL
```
GET /v1.0/appkeys/{appkey}/availability-zones
X-Auth-Token: {tokenId}
```

|  Name | In | Type | Optional | Description |
|--|--|--|--|--|
| tokenId | Header | String | - | 令牌ID |

#### Request Body
此API无需请求Request Body。

#### Response Body
```json
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    },
    "zones": [
        {
            "zoneName": "{Zone Name}",
            "zoneState": {
                "available": "{Available}"
            }
        }
    ]
}
```

|  Name | In | Type | Description |
|--|--|--|--|
| Zone Name | Body | String | 可用区名称 |
| Available | Body | Boolean | 可用区是否可用|

## 实例API
提供实例创建、删除、信息查询及块存储连接管理功能。

### 实例状态
实例在创建、修改、删除、运营期间具有以下状态。
![[图1] 实例状态 Diagram](http://static.toastoven.net/prod_infrastructure/compute/developersguide/img_001.png)


| 状态 | 描述 |
| --- | --- |
| BUILD | 实例创建中 |
| POWERING_ON | 实例启动中 |
| STOP | 实例已关闭 |
| ACTIVE | 实例运行中 |
| POWERING_OFF | 实例停止中 |
| REBOOTING | 实例重启中 |
| DELETING | 实例删除中 |
| RESIZING | 更改实例规格(flavor)中 |
| MIGRATING | 实例迁移中 |
| ERROR | 错误状态 |

### 实例摘要查询
查看已创建实例的摘要信息。
#### Method, URL
```
GET /v1.0/appkeys/{appkey}/instances?id={instanceId}
X-Auth-Token: {tokenId}
```

|  Name | In | Type | Optional |Description |
|--|--|--|--|--|
| tokenId | Header | String | - | 令牌ID |
| instanceId | Query | String | O | 要查询的实例ID。如果未指定，则显示所有实例的摘要信息。 |

#### Request Body
此API无需Request Body。

#### Response Body
```json
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    },
    "instances": [
        {
            "id": "{Instance ID}",
            "name": "{Instance Name}",
            "status": "{Instance Status}"
        }
    ]
}
```

|  Name | In | Type | Description |
|--|--|--|--|
| Instance ID | Body | String | 实例ID |
| Instance Name | Body | String | 实例名称 |
| Instance Status | Body | String | 实例状态 |

### 实例详细查询
查询实例的详细信息。
#### Method, URL
```
GET /v1.0/appkeys/{appkey}/instances-detail?id={instanceId}
X-Auth-Token: {tokenId}
```

|  Name | In | Type | Optional |Description |
|--|--|--|--|--|
| tokenId | Header | String | - | 令牌ID. |
| instanceId | Query | String | O | 要查询的实例ID。如果省略，则查询所有实例的详细信息。|

#### Request Body
此API无需Request Body。

#### Response Body
```json
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    },
    "instances": [
        {
            "addresses": [
                {
                    "macAddress": "{MAC Address}",
                    "ipAddress": "{IP Address}",
                    "version": "{IP Version}",
                    "floatingIpAddress": "{Floating IP Address}"
                }
            ],
            "availabilityZone": "{Zone Name}",
            "flavor": {
                "id": "{Flavor ID}",
                "name": "{Flavor Name}",
                "cpu": "{Flavor CPU}",
                "ram": "{Flavor RAM}"
            },
            "status": "{Status}",
            "id": "{Instance ID}",
            "name": "{Instance Name}",
            "image": "{Image ID}",
            "metadata": {
                "{key}": "{value}"
            },
            "keyName": "{PEM Key Name}",
            "volumes": {
                "root" : {
                    "size" : "{Root Volume Size}"
                },
                "attachments" : [
                    {
                        "id" : "{Attached Volume ID}",
                        "name": "{Attached Volume Name}",
                        "size": "{Attached Volume Size}"
                    }
                ]
            },
            "securityGroups": [
                {
                    "name": "{Security Group Name}"
                }
            ],
            "launchedAt": "{Launched Time}",
            "createdAt": "{Created Time}",
            "updatedAt": "{Updated Time}"
        }
    ]
}
```

|  Name | In | Type | Description |
|--|--|--|--|
| MAC Address | Body | String | NIC的MAC地址 |
| IP Address | Body | String | NIC的IP地址 |
| version | Body | Integer | IP版本(仅支持IPv4) |
| Floating IP Address | Body | String | 分配至NIC的弹性IP地址 |
| Zone Name | Body | String | 可用区名称 |
| Flavor ID | Body | String | 实例配置ID |
| Flavor Name | Body | String | 实例配置名称 |
| Flavor CPU | Body | Integer | CPU个数 |
| Flavor RAM | Body | Integer | RAM大小(MB) |
| Status | Body | String | 实例状态 |
| Instance ID | Body | String | 实例ID |
| Instance Name | Body | String | 实例名称 |
| Image ID | Body | String | 实例使用的镜像ID |
| metadata | Body | Object | 保存为"key": "value"格式，用作实例中要设置的用户元数据 |
| PEM Key Name | Body | String | 要在实例上注册的密钥对的名称 |
| Root Volume Size | Body | Integer | 实例默认系统盘大小(GB) |
| Attached Volume ID | Body | String | 附加块存储 ID |
| Attached Volume Name | Body | String | 附加块存储名称 |
| Attached Volume Size | Body | Integer | 附加块存储大小(GB) |
| Security Group Name | Body | String | 在实例中注册的安全组名称 |
| Launched Time | Body | String | 实例最近启动时间。yyyy-mm-ddTHH:MM:ssZ格式。例如) 2017-05-16T02:17:50.166563 |
| Created Time | Body | String | 实例创建时间。yyyy-mm-ddTHH:MM:ssZ格式。例如) 2017-05-16T02:17:50.166563 |
| Updated Time | Body | String | 实例修改时间。yyyy-mm-ddTHH:MM:ssZ格式。例如) 2017-05-16T02:17:50.166563 |

### 创建实例
新建一个实例。

#### Method, URL
```
POST /v1.0/appkeys/{appkey}/instances
X-Auth-Token: {tokenId}
Content-Type: application/json;charset=UTF-8
```

|  Name | In | Type | Optional | Description |
|--|--|--|--|--|
| tokenId | Header | String | - | 令牌ID |

#### Request Body
```json
{
    "instance": {
        "name": "{Instance Name}",
        "image": "{Image ID}",
        "flavor": "{Flavor ID}",
        "networks": [
        	{
              "id": "{Network ID}",
              "subnetId": "{Subnet ID}"
        	}
        ],
        "availabilityZone": "{Availability Zone}",
        "keyName": "{Key Name}",
        "count": "{Count}",
        "volume": {
           "size": "{Volume Size}"
        },
        "securityGroups": [
        	{
            	"name": "{Security Group Name}"
        	}
		]
    }
}
```

|  Name | In | Type | Optional | Description |
|--|--|--|--|--|
| Instance Name | Body | String | - | 实例名称(Linux最多20个字符，Windows最多12字符，只能为英文字母和数字，'-'，'.') |
| Image ID | Body | String | - | 实例使用的镜像ID |
| Flavor ID | Body | String | - | 实例配置ID |
| Network ID | Body | String | O | 实例要连接的网络ID |
| Subnet ID | Body | String | O | 连接实例的子网ID<br>必须指定网络ID或子网ID中的一个。|
| Availability Zone | Body | String | - | 要创建实例的可用区名称 |
| Key Name | Body | String | - | 要在实例中注册的密钥对名称 |
| Count | Body | Integer | 0 | 可同时创建的实例个数，最多可创建10个，1~10范围。省略时生成1个 |
| Volume Size | Body | Integer | 0 | 实例默认系统盘大小，大小请参考[控制台向导](/Compute/Instance/zh/console-guide/#_5) |
| Security Group Name | Body | String | - | 要在实例中注册的安全组的名称 |

* **使用带有"volumeSize"参数值的规格(u2.\*)创建实例时**
	* 应省略"instance.volume.size"参数。
	* 生成规格中定义的卷大小固定的默认系统盘。
* **使用不带有"volumeSize"参数值的规格创建实例时**
	* 必须记载"instance.volume.size"参数。
	* **在目标镜像"minDisk"值 ~ 1000范围内以10为单位**设置，则会按照所设置的大小生成默认系统盘。


#### Response Body
```json
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    },
    "instance": {
        "id": "{Instance ID}",
        "name": "{Instance Name}",
        "status": "{Instance Status}"
    }
}
```

| Name | In | Type | Description |
|--|--|--|--|
| Instance ID | body | String |已创建的实例的ID |
| Instance Name | body | String | 实例名称 |
| Instance Status | Body | String | 实例状态 |

### 删除实例
删除指定的实例。
#### Method, URL
```
DELETE /v1.0/appkeys/{appkey}/instances?id={instanceId}
X-Auth-Token: {tokenId}
```

|  Name | In | Type | Optional | Description |
|--|--|--|--|--|
| tokenId | Header | String | - | 令牌ID |
| instanceId | Query | String | - | 要删除的实例ID |

### 连接块存储
将块存储连接到实例。

#### Method, URL
```
POST /v1.0/appkeys/{appkey}/instances/{instanceId}/attachments
X-Auth-Token: {tokenId}
Content-Type: application/json;charset=UTF-8
```

|  Name | In | Type | Optional | Description |
|--|--|--|--|--|
| tokenId | Header | String | - | 令牌ID |
| instanceId | Path | String | - | 用于连接块存储的实例ID |

#### Request Body
```json
{
    "attachment":{
        "volumeId":"{Volume ID}"
    }
}
```

|  Name | In | Type | Optional | Description |
|--|--|--|--|--|
| Volume ID | body | String | - | 用于连接实例的块存储ID |

#### Response Body

```json
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    },
    "attachment": {
        "device": "{Device ID}",
        "id": "{Attachment ID}",
        "volumeId": "{Volume ID}"
    }
}
```

|  Name | In | Type | Description |
|--|--|--|--|
| Device ID | body | String | 在实例上注册的设备名称。例如) "/dev/vdc" |
| Attachement ID | body | String | 连接ID。|
| Volume ID | body | String | 块存储ID。卸载时需要。|

### 卸载块存储
对挂载在实例上的块存储进行卸载。

#### Method, URL
```
DELETE /v1.0/appkeys/{appkey}/instances/{instanceId}/attachments?volumeId={volumeId}
X-Auth-Token: {tokenId}
```

|  Name | In | Type | Optional | Description |
|--|--|--|--|--|
| tokenId | Header | String| - | 令牌ID |
| instanceId | Path | String | - | 实例ID |
| volumeId | Path | String | - | 块存储ID |

#### Request body
此API无需Request Body。

#### Response Body

```json
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    }
}
```

## 实例附加功能API
提供以下实例控件及附加功能。

- 启动，停止和重启实例
- 更改实例规格(resize)
- 创建实例镜像
- 绑定，解绑弹性IP
- 添加，移出安全组

### 共通
所有实例的附加功能API都调用相同的方法，URL，并将每个附加功能与Request Body区分开来。
#### Method, URL
```
POST /v1.0/appkeys/{appkey}/instances/{instanceId}/action
X-Auth-Token: {tokenId}
Content-Type: application/json;charset=UTF-8
```

|  Name | In | Type | Optional | Description |
|--|--|--|--|--|
| tokenId | Header | String| - | 令牌ID |
| instanceId | Path | String | - | 用于执行附加功能的实例ID |

#### Request Body Template
```json
{
    "action": "{Action Name}",
    "parameters" : {
         "{key}": "{value}"
    }
}
```
|  Name | In | Type | Optional | Description |
|--|--|--|--|--|
| Action Name | Body | String | - | 要在实例上运行的附加功能 |
| parameters | Body | Object| O | 执行附加功能所需的参数。根据附加功能填写所需的值。一些附加功能可以在无参数的情况下运行。|

### 启动实例
启动处于停止(STOP)状态的实例。
#### Request Body
```json
{
    "action" : "start"
}
```

#### Response Body
```json
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    }
}
```

### 停止实例
停止处于运行中(ACTIVE)或故障中(ERROR)的实例。
#### Request Body
```json
{
    "action" : "stop"
}
```

#### Response Body

```json
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    }
}
```

### 重启实例
重启实例。您可以指定以下重启方式。

- **SOFT**: 正常关闭(graceful shutdown)后，重启实例。
- **HARD**: 强制关闭(shutdown)后，重启实例。

#### Request Body

```json
{
    "action" : "reboot",
    "parameters":{
        "type":"{Reboot Type}"
    }
}
```

|  Name | In | Type | Optional | Description |
|--|--|--|--|--|
| Reboot Type | body | String | - | 重启方法。`HARD` 或 `SOFT`. |

#### Response Body
```json
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    }
}
```

### 更改实例配置
更改实例的配置。
#### Request Body
```json
{
    "action": "resize",
    "parameters":{
        "flavor":"{Flavor ID}"
    }
}
```

|  Name | In | Type | Optional | Description |
|--|--|--|--|--|
|  Flavor ID | body | String | - | 要更改配置的实例ID |

#### Response Body
```json
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    }
}
```

### 创建镜像
使用指定的实例创建镜像。创建的镜像可以通过[镜像API](/Compute/Image/zh/api-guide/)查询。

若要使用实例创建自定义镜像，则该实例必须处于STOP状态。

#### Request Body
```json
{
    "action" : "uploadImage",
    "parameters" : {
        "name": "{Image Name}"
    }
}
```

|  Name | In | Type | Optional | Description |
|--|--|--|--|--|
| Image Name | body | String | - | 要创建的镜像名称 |

#### Response Body
```json
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    },
    "createdImage": {
        "id" : "{Created Image ID}",
        "name" : "{Created Image Name}"
    }
}
```

|  Name | In | Type | Description |
|--|--|--|--|
| Created Image ID | body | String | 创建的镜像ID |
| Created Image Name | body | String | 创建的镜像名称 |

### 连接弹性IP
将弹性IP绑定到实例。

#### Request Body
```json
{
    "action": "addFloatingIp",
    "parameters": {
        "address": "{Floating IP Address}",
        "ipAddress": "{IP Address of the instance}"
    }
}
```

|  Name | In | Type | Optional | Description |
|--|--|--|--|--|
| Floating IP Address | body | String | - | 要绑定到实例的弹性IP地址 |
| IP Address of the instance | body | String | - | 要绑定弹性IP的实例的IP地址 |

#### Response Body

```json
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    }
}
```

### 解绑弹性IP
实例解绑弹性IP。

#### Request Body

```json
{
    "action": "removeFloatingIp",
    "parameters" : {
        "address": "{Floating IP Address}"
    }
}
```

|  Name | In | Type | Optional | Description |
|--|--|--|--|--|
| Floating IP Address | body | String | - | 要解绑的弹性IP地址 |

#### Response Body

```json
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    }
}
```

### 注册安全组
将实例添加到安全组。

#### Request Body
```json
{
    "action": "addSecurityGroup",
    "parameters": {
        "name": "{Security Group Name}"
    }
}
```

|  Name | In | Type | Optional | Description |
|--|--|--|--|--|
| Security Group Name | body | String | - | 要添加实例的安全组的名称 |

#### Response Body

```json
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    }
}
```

### 删除安全组
删除已添加的安全组。

#### Request Body
```json
{
    "action": "removeSecurityGroup",
    "parameters": {
        "name": "{Security Group Name}"
    }
}
```

|  Name | In | Type | Optional | Description |
|--|--|--|--|--|
| Security Group Name | body | String | - | 实例中作为删除对象的安全组的名称(要删除的安全组名称)|

#### Response Body
```json
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    }
}
```

## 实例规格API
### 查看实例规格列表
查看实例配置列表及详细信息。

#### Method, URL
```
GET /v1.0/appkeys/{appkey}/flavors
X-Auth-Token: {tokenID}
```

|  Name | In | Type | Optional | Description |
|--|--|--|--|--|
| tokenId | Header | String| - | 令牌ID |

#### Request Body
此API无需Request Body。

#### Response Body
```json
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    },
    "flavors": [
        {
            "disabled": "{Disabled}",
            "ephemeral": "{Ephermeral}",
            "type": "{Type}",
            "volumeSize": "{Volume Size}",
            "id": "{Flavor ID}",
            "name": "{Flavor Name}",
            "isPublic": "{Is Public}",
            "ram": "{RAM}",
            "vcpus": "{VCPUs}"
        }
    ]
}
```

|  Name | In | Type | Description |
|--|--|--|--|
| Disabled | Body | Boolean | 是否禁用实例规格。|
| Ephermeral | Body | Integer | 临时磁盘容量(GB). |
| Type | Body | String | 根据实例规格特性区分的Type值。<br>"general, "compute", "memory"中之一。|
| Volume Size | Body | Integer | 在实例创建期间作为默认系统盘创建的磁盘容量(GB)。<br>仅在固定大小的u2配置下传递该值。|
| Max Volume Size | Body | Integer | 可作为默认系统盘的最大磁盘容量(GB)。|
| Flavor ID | Body | String | 实例配置ID。|
| Flavor Name | Body | String | 实例配置名称。|
| Is Public | Body | Boolean | 是否为共用实例配置。|
| RAM | Body | Integer | 实例配置中RAM总容量(MB)。 |
| VCPUs | Body | Integer | 分配给实例的虚拟CPU核心数。|

## 密钥对API
为访问实例提供创建、删除、查询密钥对的功能。
### 查看密钥对
查看密钥对。
#### Method, URL
```
GET /v1.0/appkeys/{appkey}/keypairs?name={keypairName}
X-Auth-Token: {tokenId}
```

|  Name | In | Type | Optional | Description |
|--|--|--|--|--|
| tokenId | Header | String | - |令牌ID |
| keypairName | Query | String | O | 要查询的密钥对名称。如果没有，则查询所有密钥对信息。|

#### Request Body
此API无需Request Body。

#### Response Body

```json
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    },
    "keypairs": [
        {
            "name": "{Keypair Name}",
            "publicKey": "{Public Key Value}",
            "fingerprint": "{Fingerprint Value}",
       	    "createdAt": "{Created At}"
        }
    ]
}
```

|  Name | In | Type | Description |
|--|--|--|--|
| Keypair Name | Body | String | 密钥对名称 |
| Public Key Value | Body | String | 密钥对的公钥值 |
| Fingerprint Value | Body | String | 指纹(fingerprint)值 |
| Created At | Body | DateTime | 密钥对创建时间。 "Keypair Name"只有在查看指定的单个项目时才会显示"Keypair Name"。|

### 创建和上传密钥对
创建密钥对或上传用户自定义的密钥对。

#### Method, URL
```
POST /v1.0/appkeys/{appkey}/keypairs
X-Auth-Token: {tokenId}
Content-Type: application/json;charset=UTF-8
```

|  Name | In | Type | Optional | Description |
|--|--|--|--|--|
| tokenId | Header | String | - | 令牌ID |

#### Request Body

```json
{
    "keypair": {
        "name": "{Keypair Name}",
        "publicKey": "{Public Key Value}"
    }
}
```

| Name | In | Type | Optional | Description |
| --- | --- | --- | --- | --- |
| Keypair Name | Body | String | - | 密钥对名称 |
| Public Key Value | Body | String | O | 要上传的公钥。 如果省略，则会创建新密钥对，并将新密钥对的私钥一起发送至Response。|

#### Response Body

```json
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    },
    "keypair": {
        "name": "{Keypair Name}",
        "publicKey": "{Public Key Value}",
        "privateKey": "{Private Key Value}",
        "fingerprint": "{Fingerprint Value}"
    }
}
```

| Name | In | Type | Description |
| --- | --- | --- | --- |
| Keypair Name | Body | String | 密钥对名称 |
| Public Key Value | Body | String | 密钥对的公钥 |
| Private Key Value | Body | String | 密钥对的私钥。密钥对上传(在Request中包含"publickey")，省略此操作。|
| Fingerprint value | Body | String | Fingerprint值 |

在将文本另存为.pem文件后，生成的Private Key Value可用于访问已设置为使用密钥对的实例。因**无法再次查询生成的Private Key Value**， 应妥善保存，以免丢失或被删除。建议尽可能在辅助存储介质(USB存储器)中对其进行管理，以防止泄露。

### 删除密钥对
删除指定的密钥对
#### Method, URL
```
DELETE /v1.0/appkeys/{appkey}/keypairs?name={keypairName}
X-Auth-Token: {tokenId}
```

|  Name | In | Type | Optional | Description |
|--|--|--|--|--|
| tokenId | Header | String | - | 令牌ID |
| keypairName | Query | String | - | 要删除的密钥对的名称 |

#### Request Body
此API无需Request Body。

#### Response Body
```json
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    }
}
```
