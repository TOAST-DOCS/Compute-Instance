## Compute > Instance > API Guide

API is currently available only in the Korea region.

The TOAST Compute Instance service provides APIs of the following types:

* [Availability Zone API](#api_2)
* [Instance API](#api_3)
* [Add Instances API](#api_4)
* [Instance Flavors API](#api_5)
* [Key Pair API](#api_6)


## Prerequisites

Using an Instance API requires an appkey and a token. Get an appkey and a token by using [API Endpoint URL](#api-endpoint-url) and [Token API](#api): include the appkey to Endpoint URL and the token to the Request Body.

### API Endpoint URL

APIs of all instances, images, networks (VPC), and block storages must start with the following URL:

	https://api-compute.cloud.toast.com/compute

Always include an appkey to request an API. You may also get an appkey as below:

1. Click **URL & Appkey** on the top of **Compute** in the TOAST console.
2. Copy **Appkey** from **URL & Appkey** dialogue box.

For instance, here is the URL to issue a token:

	POST https://api-compute.cloud.toast.com/compute/v1.0/appkeys/{appkey}/tokens

In the API documents for instances, images, networks and block storages, the URL prefix has been omitted for clear and easy understanding.

### API Response

#### Response HTTP Status Code

Respond with 200 OK to all API requests, and include the response body of the JSON format.

#### Response Body

The response body includes the "header" information by default, from which detailed results can be confirmed. Additional information may be added, depending on the API.

```json
{
    "header" : {
        "isSuccessful" : true,
        "resultCode": 0,
        "resultMessage" : "SUCCESS"
    }
}
```

When an API call is failed, 'isSuccessful' becomes 'false', and the error code will be displayed on the 'resultCode'. For more details, refer to [Error Code](/Compute/Instance/en/error-code/).

### Token API

Token is an authentication key which is required to use API. All APIs must be requested with the **X-Auth-Token** header added.

#### Set API Passwords

To set a password for API, go to Compute > Instance, and click `[Set API Endpoint]`.

1. Click **Set API Endpoint**.
2. Go to **Set API Endpoint > Set API Password**, and enter a password to use.
3. Enter your password and click **Save**.

#### Issue Tokens

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

| Name         | In   | Type   | Optional | Description                              |
| ------------ | ---- | ------ | -------- | ---------------------------------------- |
| TOAST ID     | Body | String | -        | Enter TOAST ID (Email)                   |
| API Password | Body | String | -        | Password saved in the dialog box for **API Endpoint setting** |

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

| Name      | In   | Type   | Description                              |
| --------- | ---- | ------ | ---------------------------------------- |
| Token ID  | Body | String | Token UUID required at the HTTP header (X-Auth-Token) at the request of API UUID |
| Issued at | Body | String | Token issuance time in the format of yyyy-mm-ddTHH:MM:ssZ. e.g) 2017-05-16T02:17:50.166563 |
| Expires   | Body | String | Expiration time of an issued token in the format of yyyy-mm-ddTHH:MM:ssZ. e.g) 2017-05-16T03:17:50Z |
| User ID   | Body | String | UUID of the user with a token issued     |
| Role name | Body | String | Role assigned to the user with a token issued |

#### Retrieve Token Information
##### Method, URL
```
GET /v1.0/appkeys/{appkey}/tokens?id={tokenId}
```

| Name    | In    | Type   | Optional | Description          |
| ------- | ----- | ------ | -------- | -------------------- |
| tokenId | Query | String | -        | Token ID to Retrieve |

##### Request Body
This API does not require a request body.

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

| Name      | In   | Type   | Description                              |
| --------- | ---- | ------ | ---------------------------------------- |
| Token ID  | Body | String | Token UUID required at the HTTP header (X-Auth-Token) at the request of API |
| Issued at | Body | String | Token issuance time in the format of yyyy-mm-ddTHH:MM:ssZ. e.g) 2017-05-16T02:17:50.166563 |
| Expires   | Body | String | Expiration time of an issued token in the format of yyyy-mm-ddTHH:MM:ssZ. e.g) 2017-05-16T03:17:50Z |
| User ID   | Body | String | UUID of the user with a token issued     |
| Role name | Body | String | Role assigned to the user with a token issued |


## Availability Zone API

### Retrieve Availability Zone
Retrieve information on availability zones to create instances and block storages.

#### Method, URL
```
GET /v1.0/appkeys/{appkey}/availability-zones
X-Auth-Token: {tokenId}
```

| Name    | In     | Type   | Optional | Description |
| ------- | ------ | ------ | -------- | ----------- |
| tokenId | Header | String | -        | Token ID    |

#### Request Body
This API does not require a request body.

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

| Name      | In   | Type    | Description        |
| --------- | ---- | ------- | ------------------ |
| Zone Name | Body | String  | Name of AZ         |
| Available | Body | Boolean | Availability of AZ |

## Instance API
Provide functions, such as create/delete instances, retrieve information and connect with block storages.

### Instance Status
An instance has following status while it is created, changed, deleted, or operated:
![[Figure 1] Instance Status Diagram](http://static.toastoven.net/prod_infrastructure/compute/developersguide/img_001.png)


| Status       | Description                      |
| ------------ | -------------------------------- |
| BUILD        | Creating an instance             |
| POWERING_ON  | Starting an instance             |
| ACTIVE       | Operating an instance            |
| POWERING_OFF | Closing an instance              |
| STOP         | Closed an instance               |
| REBOOTING    | Rebooting an instance            |
| RESIZING     | Changing instance flavors |
| MIGRATING    | Migrating an instance            |
| DELETING     | Deleting an instance             |
| ERROR        | In error                         |

### Retrieve Brief Instance Information
Retrieve brief information of a created instance.
#### Method, URL
```
GET /v1.0/appkeys/{appkey}/instances?id={instanceId}
X-Auth-Token: {tokenId}
```

| Name       | In     | Type   | Optional | Description                              |
| ---------- | ------ | ------ | -------- | ---------------------------------------- |
| tokenId    | Header | String | -        | Token ID                                 |
| instanceId | Query  | String | O        | Instance ID to retrieve. If left empty, retrieve brief information of all instances. |

#### Request Body
This API does not require a request body.

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

| Name            | In   | Type   | Description     |
| --------------- | ---- | ------ | --------------- |
| Instance ID     | Body | String | Instance ID     |
| Instance Name   | Body | String | Instance Name   |
| Instance Status | Body | String | Instance Status |

### Retrieve Instance Details
Retrieve detailed information of an instance.
#### Method, URL
```
GET /v1.0/appkeys/{appkey}/instances-detail?id={instanceId}
X-Auth-Token: {tokenId}
```

| Name       | In     | Type   | Optional | Description                              |
| ---------- | ------ | ------ | -------- | ---------------------------------------- |
| tokenId    | Header | String | -        | Token ID                                 |
| instanceId | Query  | String | O        | Instance ID to retrieve: if left empty, retrieve detail information of all instances. |

#### Request Body
This API does not require a request body.

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
                    "size": "{Root Volume Size}",
                    "type": "{Root Volume Type}"
                },
                "attachments" : [
                    {
                        "id" : "{Attached Volume ID}",
                        "name": "{Attached Volume Name}",
                        "size": "{Attached Volume Size}",
                        "type": "{Attached Volume Type}"
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

| Name                 | In   | Type    | Description                              |
| -------------------- | ---- | ------- | ---------------------------------------- |
| MAC Address          | Body | String  | MAC address of NIC                       |
| IP Address           | Body | String  | IP address of NIC                        |
| version              | Body | Integer | IP version (supports IPv4 only)          |
| Floating IP Address  | Body | String  | Floating IP address assigned to NIC      |
| Zone Name            | Body | String  | Name of availability zone                |
| Flavor ID            | Body | String  | Instance flavors ID               |
| Flavor Name          | Body | String  | Name of instance flavors          |
| Flavor CPU           | Body | Integer | Number of CPUs                           |
| Flavor RAM           | Body | Integer | RAM size (MB)                            |
| Status               | Body | String  | Status of instance                       |
| Instance ID          | Body | String  | Instance ID                              |
| Instance Name        | Body | String  | Instance name                            |
| Image ID             | Body | String  | ID of image installed at an instance     |
| metadata             | Body | Object  | User meta data to be installed at an instance: save in the format of "key":"value" |
| PEM Key Name         | Body | String  | Key pair name to be registered at an instance |
| Root Volume Size     | Body | Integer | Size of Instance root block storage (GB) |
| Root Volume Type     | Body | String | Type of Instance root block storage, Select "General HDD" or "General SSD" |
| Attached Volume ID   | Body | String  | ID of attached block storage             |
| Attached Volume Name | Body | String  | Name of attached block storage           |
| Attached Volume Size | Body | Integer | Size of attached block storage (GB)      |
| Attached Volume Type | Body | String | Type of attached block storage, Select "General HDD" or "General SSD" |
| Security Group Name  | Body | String  | Name of a security group registered at an instance |
| Launched Time        | Body | String  | Recent start time of an instance in the format of yyyy-mm-ddTHH:MM:ssZ. e.g) 2017-05-16T02:17:50.166563 |
| Created Time         | Body | String  | Time of instance creation in the format of yyyy-mm-ddTHH:MM:ssZ. e.g) 2017-05-16T02:17:50.166563 |
| Updated Time         | Body | String  | Time of instance update in the format of yyyy-mm-ddTHH:MM:ssZ. e.g) 2017-05-16T02:17:50.166563 |

### Create Instances
Create a new instance.

#### Method, URL
```
POST /v1.0/appkeys/{appkey}/instances
X-Auth-Token: {tokenId}
Content-Type: application/json;charset=UTF-8
```

| Name    | In     | Type   | Optional | Description |
| ------- | ------ | ------ | -------- | ----------- |
| tokenId | Header | String | -        | Token ID    |

#### Request Body
```json
{
    "instance": {
        "name": "{Instance Name}",
        "image": "{Image ID}",
        "flavor": "{Flavor ID}",
        "networks": [
        	{
            	"id": "{Network ID}"
        	}
        ],
        "availabilityZone": "{Availability Zone}",
        "keyName": "{Key Name}",
        "count": "{Count}",
        "volume": {
           "size": "{Volume Size}",
           "type": "{Volume Type}"
        },
        "securityGroups": [
        	{
            	"name": "{Security Group Name}"
        	}
		]
    }
}
```

| Name                | In   | Type    | Optional | Description                              |
| ------------------- | ---- | ------- | -------- | ---------------------------------------- |
| Instance Name       | Body | String  | -        | Name of an instance (allows up to 20 characters for Linux and 12 for Windows, including alphabets, numbers, '-', and '.' only) |
| Image ID            | Body | String  | -        | ID of an image to be installed at an instance. |
| Flavor ID           | Body | String  | -        | Instance flavors ID.              |
| Network ID          | Body | String  | O        | ID of a network to which an instance is to be connected. |
| Subnet ID           | Body | String  | O        | Subnet ID to be connected with instances. <br> Either network ID or subnet ID must be specified. |
| Availability Zone   | Body | String  | -        | Name of availability zone to which an instance is to be created. |
| Key Name            | Body | String  | -        | Name of a key pair to be registered at a instance. |
| Count               | Body | Integer | O        | Number of instances to be concurrently created: between 1 and 10, and no more than 10. |
| Volume Size         | Body | Integer | -        | Size of Instance root block storage (GB), Refer to the [Console Guide ](/Compute/Instance/en/console-guide/#_5) for the available size to create. |
| Volume Type         | Body | String  | O        | Type of Instance root block storage, Select "General HDD" or "General SSD" |
| Security Group Name | Body | String  | -        | Name of a security group to be registered at an instance |

> [Caution]
> * `Volume Size` and `Volume Type` parameters are valid only for c2, m2, r2, or t2 flavors.
> 	* These parameters are not applicable for 'u2' series flavors which use local storage.
> * `Volume Size` must be set to 10 units within a value beteen "minDisk" of selected Image and 1000 GB.
> * `Volume Type` can be `null`, and it be regarded as "General HDD".


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

| Name            | In   | Type   | Description            |
| --------------- | ---- | ------ | ---------------------- |
| Instance ID     | body | String | ID of created instance |
| Instance Name   | body | String | Name of instance       |
| Instance Status | Body | String | Status of instance     |

### Delete Instances
Delete a particular instance.
#### Method, URL
```
DELETE /v1.0/appkeys/{appkey}/instances?id={instanceId}
X-Auth-Token: {tokenId}
```

| Name       | In     | Type   | Optional | Description                     |
| ---------- | ------ | ------ | -------- | ------------------------------- |
| tokenId    | Header | String | -        | Token ID                        |
| instanceId | Query  | String | -        | ID of an instance to be deleted |

### Attach Block Storages
Attach a block storage to an instance.

#### Method, URL
```
POST /v1.0/appkeys/{appkey}/instances/{instanceId}/attachments
X-Auth-Token: {tokenId}
Content-Type: application/json;charset=UTF-8
```

| Name       | In     | Type   | Optional | Description                              |
| ---------- | ------ | ------ | -------- | ---------------------------------------- |
| tokenId    | Header | String | -        | Token ID                                 |
| instanceId | Path   | String | -        | Instance ID to be attached with a block storage |

#### Request Body
```json
{
    "attachment":{
        "volumeId":"{Volume ID}"
    }
}
```

| Name      | In   | Type   | Optional | Description                              |
| --------- | ---- | ------ | -------- | ---------------------------------------- |
| Volume ID | body | String | -        | Block storage ID to be attached to an instance. |

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

| Name           | In   | Type   | Description                              |
| -------------- | ---- | ------ | ---------------------------------------- |
| Device ID      | body | String | Device name registered at an instance, e.g) "/dev/vdc" |
| Attachement ID | body | String | Attachment ID                            |
| Volume ID      | body | String | Block storage ID: required to detach     |

### Detach Block Storages
Detach a block storage which is attached to an instance.

#### Method, URL
```
DELETE /v1.0/appkeys/{appkey}/instances/{instanceId}/attachments?volumeId={volumeId}
X-Auth-Token: {tokenId}
```

| Name       | In     | Type   | Optional | Description      |
| ---------- | ------ | ------ | -------- | ---------------- |
| tokenId    | Header | String | -        | Token ID         |
| instanceId | Path   | String | -        | Instance ID      |
| volumeId   | Path   | String | -        | Block Storage ID |

#### Request body
This request does not require a body.

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

## Add Instances API
Instance control and additional functions are provided as follows:

- Start/stop/restart instances
- Resize instances
- Create images
- Associate/disassociate floating IPs
- Register/remove security groups

### Common
All Add Instance APIs are called with a same method and URL, and can be divided by additional functions at the request body.
#### Method, URL
```
POST /v1.0/appkeys/{appkey}/instances/{instanceId}/action
X-Auth-Token: {tokenId}
Content-Type: application/json;charset=UTF-8
```

| Name       | In     | Type   | Optional | Description                          |
| ---------- | ------ | ------ | -------- | ------------------------------------ |
| tokenId    | Header | String | -        | Token ID                             |
| instanceId | Path   | String | -        | Instance ID for additional functions |

#### Request Body Template
```json
{
    "action": "{Action Name}",
    "parameters" : {
         "{key}": "{value}"
    }
}
```
| Name        | In   | Type   | Optional | Description                              |
| ----------- | ---- | ------ | -------- | ---------------------------------------- |
| Action Name | Body | String | -        | Additional functions to be executed at an instance |
| parameters  | Body | Object | O        | Parameters required to execute additional functions: fill in required values depending on the additional functions. Some functions operate without parameters. |

### Start Instances
Start an instance which has been stopped.
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

### Stop Instances
Stop an instance which is active or has an error.
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

### Restart Instances
Restart an instance, and the method can be specified as below:

- **SOFT**: Execute a graceful shutdown and restart an instance .
- **HARD**: Execute a shutdown, and restart an instance.

#### Request Body

```json
{
    "action" : "reboot",
    "parameters":{
        "type":"{Reboot Type}"
    }
}
```

| Name        | In   | Type   | Optional | Description                              |
| ----------- | ---- | ------ | -------- | ---------------------------------------- |
| Reboot Type | body | String | -        | Restarting method: either `HARD` or `SOFT` |

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

### Change Flavors
Change flavors of an instance.
#### Request Body
```json
{
    "action": "resize",
    "parameters":{
        "flavor":"{Flavor ID}"
    }
}
```

| Name      | In   | Type   | Optional | Description                           |
| --------- | ---- | ------ | -------- | ------------------------------------- |
| Flavor ID | body | String | -        | Instance flavors ID to change. |

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

### Create Images
Create an image from a specified instance. To retrieve the image, go to [Image API](/Compute/Image/en/api-guide/).

Instances that are stopped can be the subject for image creation.

#### Request Body
```json
{
    "action" : "uploadImage",
    "parameters" : {
        "name": "{Image Name}"
    }
}
```

| Name       | In   | Type   | Optional | Description                |
| ---------- | ---- | ------ | -------- | -------------------------- |
| Image Name | body | String | -        | Name of an image to create |

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

| Name               | In   | Type   | Description              |
| ------------------ | ---- | ------ | ------------------------ |
| Created Image ID   | body | String | ID of an image created   |
| Created Image Name | body | String | Name of an image created |

### Associate Floating IPs
Associate a floating IP with an instance.

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

| Name                       | In   | Type   | Optional | Description                              |
| -------------------------- | ---- | ------ | -------- | ---------------------------------------- |
| Floating IP Address        | body | String | -        | Floating IP address to be associated with an instance |
| IP Address of the instance | body | String | -        | IP address of an instance which a floating IP is to be associated with |

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

### Disassociate Floating IPs
Disassociate a floating IP which is associated with an instance.

#### Request Body

```json
{
    "action": "removeFloatingIp",
    "parameters" : {
        "address": "{Floating IP Address}"
    }
}
```

| Name                | In   | Type   | Optional | Description                              |
| ------------------- | ---- | ------ | -------- | ---------------------------------------- |
| Floating IP Address | body | String | -        | Floating IP address which is to be disassociated |

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

### Register Security Groups
Additionally register a security group at an instance.

#### Request Body
```json
{
    "action": "addSecurityGroup",
    "parameters": {
        "name": "{Security Group Name}"
    }
}
```

| Name                | In   | Type   | Optional | Description                              |
| ------------------- | ---- | ------ | -------- | ---------------------------------------- |
| Security Group Name | body | String | -        | Name of a security group to be added to an instance |

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

### Remove Security Groups
Remove a security group which is registered at an instance.

#### Request Body
```json
{
    "action": "removeSecurityGroup",
    "parameters": {
        "name": "{Security Group Name}"
    }
}
```

| Name                | In   | Type   | Optional | Description                              |
| ------------------- | ---- | ------ | -------- | ---------------------------------------- |
| Security Group Name | body | String | -        | Name of a security group to be removed from an instance |

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

## Instance Flavors API
### List Instance Flavors
List instance flavors with detail information.

#### Method, URL
```
GET /v1.0/appkeys/{appkey}/flavors
X-Auth-Token: {tokenID}
```

| Name    | In     | Type   | Optional | Description |
| ------- | ------ | ------ | -------- | ----------- |
| tokenId | Header | String | -        | Token ID    |

#### Request Body
This API does not require a request body.

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

| Name            | In   | Type    | Description                              |
| --------------- | ---- | ------- | ---------------------------------------- |
| Disabled        | Body | Boolean | Whether instance flavors are disabled |
| Ephermeral      | Body | Integer | Temporary size of a disk (GB)            |
| Type            | Body | String  | Type value depending on optimization features of an instance flavors: <br>One of "general, "compute", and "memory". |
| Volume Size     | Body | Integer | Disk size made available with default disk devices (GB)<br>:to deliver this value only for the u2 flavor, for which default disk is made at a fixed size. |
| Max Volume Size | Body | Integer | Maximum disk size made available with default disk devices (GB) |
| Flavor ID       | Body | String  | Instance flavors ID               |
| Flavor Name     | Body | String  | Name of an instance flavors       |
| Is Public       | Body | Boolean | Whether common instance flavors are used |
| RAM             | Body | Integer | Total volume of RAM of an instance flavors (MB) |
| VCPUs           | Body | Integer | Number of virtual CPU cores assigned to an instance |

## Key Pair API
Create, delete, and retrieve key pairs required to access an instance.
### Retrieve Key Pairs
Retrieve key pairs.
#### Method, URL
```
GET /v1.0/appkeys/{appkey}/keypairs?name={keypairName}
X-Auth-Token: {tokenId}
```

| Name        | In     | Type   | Optional | Description                              |
| ----------- | ------ | ------ | -------- | ---------------------------------------- |
| tokenId     | Header | String | -        | Token ID                                 |
| keypairName | Query  | String | O        | Key pair name to retrieve: if left empty, retrieve all key pair information. |

#### Request Body
This API does not require a request body.

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

| Name              | In   | Type     | Description                              |
| ----------------- | ---- | -------- | ---------------------------------------- |
| Keypair Name      | Body | String   | Name of a key pair                       |
| Public Key Value  | Body | String   | Public key value of a key pair           |
| Fingerprint Value | Body | String   | Value of a fingerprint                   |
| Created At        | Body | DateTime | Time of key pair creation: to show only when getting "Keypair Name" specifically. |

### Create and Upload Key Pairs
Create a key pair or upload a key pair created by user.

#### Method, URL
```
POST /v1.0/appkeys/{appkey}/keypairs
X-Auth-Token: {tokenId}
Content-Type: application/json;charset=UTF-8
```

| Name    | In     | Type   | Optional | Description |
| ------- | ------ | ------ | -------- | ----------- |
| tokenId | Header | String | -        | Token ID    |

#### Request Body

```json
{
    "keypair": {
        "name": "{Keypair Name}",
        "publicKey": "{Public Key Value}"
    }
}
```

| Name             | In   | Type   | Optional | Description                              |
| ---------------- | ---- | ------ | -------- | ---------------------------------------- |
| Keypair Name     | Body | String | -        | Name of a key pair                       |
| Public Key Value | Body | String | O        | Public key to upload: when omitted, a new key pair is created, which shall be delivered to Response, along with its private key. |

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

| Name              | In   | Type   | Description                              |
| ----------------- | ---- | ------ | ---------------------------------------- |
| Keypair Name      | Body | String | Name of a key pair                       |
| Public Key Value  | Body | String | Public key of a key pair                 |
| Private Key Value | Body | String | Private key of a key pair: to be omitted for an upload (when "publickey" item is included to the request). |
| Fingerprint value | Body | String | Value of a fingerprint                   |

The key pair can be used, by saving the whole private key value in the .pem file and accessing the instance which is configured to use the key pair. Take cautions not to lose or delete the **created key value as it cannot be retrieved again**, and it is recommended to save it in a supplementary saving device (like USB memory) to prevent leakage.  

### Delete Key Pairs
Delete a specified key pair.
#### Method, URL
```
DELETE /v1.0/appkeys/{appkey}/keypairs?name={keypairName}
X-Auth-Token: {tokenId}
```

| Name        | In     | Type   | Optional | Description                  |
| ----------- | ------ | ------ | -------- | ---------------------------- |
| tokenId     | Header | String | -        | Token ID                     |
| keypairName | Query  | String | -        | Name of a key pair to delete |

#### Request Body
This API does not require a request body.

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
