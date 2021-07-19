## Compute > Instance > API v2 Guide

To enable APIs, API endpoint and token are required. Prepare information required to enable an API, in reference of  [Preparing for APIs](/Compute/Compute/ko/identity-api/)

Use `compute`-type endpoint for Instance API. For more details, see `serviceCatalog` from response of token issuance. 

| Type | Region | Endpoint |
|---|---|---|
| compute | Korea (Pangyo) <br>Japan | https://kr1-api-instance.infrastructure.cloud.toast.com<br>https://jp1-api-instance.infrastructure.cloud.toast.com |

In API response, you may find fields that are not specified in the guide. Refrain from using them because such fields are only for the NHN Cloud internal usage and might be changed without previous notice. 

## Instance Type

### List Types 

```
GET /v2/{tenantId}/flavors
X-Auth-Token: {tokenId}
```

#### Request

This API does not require a request body. 

| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| tenantId | URL | String | O | Tenant ID |
| tokenId | Header | String | O | Token ID |
| minDisk | Query | Integer | - | Minimum Disk Size (GB)<br>Return only bigger-size disk types than specified |
| minRam | Query | Integer | - | Minimum RAM Size (MB)<br>Return only bigger RAM-size types than specified |

#### Response

| Name | Type | Format | Description |
|---|---|---|---|
| flavors | Body | Object | List object of instance type |
| flavors.id | Body | UUID | Instance type ID |
| flavors.links | Body | Object | Path object of instance type |
| flavors.name | Body | String | Instance type name |

<details><summary>Example</summary>
<p>


```json
{
  "flavors": [
    {
      "id": "013bea75-8541-4c6f-9abe-a03fee3d74fe",
      "links": [
        {
          "href": "https://kr1-api-instance.infrastructure.cloud.toast.com/v2/6cdebe3eb0094910bc41f1d42ebe4cb7/flavors/013bea75-8541-4c6f-9abe-a03fee3d74fe",
          "rel": "self"
        },
        {
          "href": "https://kr1-api-instance.infrastructure.cloud.toast.com/6cdebe3eb0094910bc41f1d42ebe4cb7/flavors/013bea75-8541-4c6f-9abe-a03fee3d74fe",
          "rel": "bookmark"
        }
      ],
      "name": "x1.c32m256"
    },
    {
      "id": "0f19a344-bc66-4228-8cb1-fb9ca82c54f5",
      "links": [
        {
          "href": "https://kr1-api-instance.infrastructure.cloud.toast.com/v2/6cdebe3eb0094910bc41f1d42ebe4cb7/flavors/0f19a344-bc66-4228-8cb1-fb9ca82c54f5",
          "rel": "self"
        },
        {
          "href": "https://kr1-api-instance.infrastructure.cloud.toast.com/6cdebe3eb0094910bc41f1d42ebe4cb7/flavors/0f19a344-bc66-4228-8cb1-fb9ca82c54f5",
          "rel": "bookmark"
        }
      ],
      "name": "x1.c32m128"
    }
  ]
}
```

</p>
</details>

---

### List Type Details 

```
GET /v2/{tenantId}/flavors/detail
X-Auth-Token: {tokenId}
```

#### Request	

This API does not require a request body. 

| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| tenantId | URL | String | O | Tenant ID |
| tokenId | Header | String | O | Token ID |
| minDisk | Query | Integer | - | Minimum Disk Size (GB)<br>Return only bigger-size disk types than specified |
| minRam | Query | Integer | - | Minimum RAM Size (MB)<br>Return bigger RAM-size types than specified |
| limit | Query | Integer | - | Number of types on the list <br>Return the list of types as many as specified |
| marker | Query | UUID | - | ID of the first type on the list <br/>Return the list of instances as much as `limit` from the `marker` instance, according to the sorting order |

#### Response

| Name | Type | Format | Description |
|---|---|---|---|
| flavors | Body | Object | List object of instance type |
| flavors.id | Body | UUID | Instance type ID |
| flavors.links | Body | Object | Path object of instance type |
| flavors.name | Body | String | Name of instance type |
| flavors.ram | Body | Integer | Memory size (MB) |
| flavors.OS-FLV-DISABLED:disabled | Body | Boolean | Enable or not |
| flavors.vcpus | Body | Integer | Number of vCPUs |
| flavors.extra_specs | Body | Object | Extra specification object |
| flavors.swap | Body | Integer | Swap space size (GB) |
| flavors.os-flavor-access:is_public | Body | Boolean | Go public or not |
| flavors.rxtx_factor | Body | Float | Network transmission packet rate |
| flavors.OS-FLV-EXT-DATA:ephemeral | Body | Integer | Temporary Volume Size (GB) |
| flavors.disk | Body | Integer | Default disk size (GB) |

<details><summary>Example</summary>
<p>


```json
{
  "flavors": [
    {
      "name": "x1.c32m256",
      "links": [
        {
          "href": "https://kr1-api-instance.infrastructure.cloud.toast.com/v2/6cdebe3eb0094910bc41f1d42ebe4cb7/flavors/97604802-a090-43fa-a5ce-c7cfd737fbba",
          "rel": "self"
        },
        {
          "href": "https://kr1-api-instance.infrastructure.cloud.toast.com/6cdebe3eb0094910bc41f1d42ebe4cb7/flavors/97604802-a090-43fa-a5ce-c7cfd737fbba",
          "rel": "bookmark"
        }
      ],
      "ram": 262144,
      "OS-FLV-DISABLED:disabled": false,
      "vcpus": 32,
      "extra_specs": {
        "flavor_type": "performance"
      },
      "swap": "",
      "os-flavor-access:is_public": true,
      "rxtx_factor": 1.0,
      "OS-FLV-EXT-DATA:ephemeral": 0,
      "disk": 0,
      "id": "97604802-a090-43fa-a5ce-c7cfd737fbba"
    },
    {
      "name": "x1.c32m128",
      "links": [
        {
          "href": "https://kr1-api-instance.infrastructure.cloud.toast.com/v2/6cdebe3eb0094910bc41f1d42ebe4cb7/flavors/31fa632d-aeec-4f12-8a57-ce9d146228e5",
          "rel": "self"
        },
        {
          "href": "https://kr1-api-instance.infrastructure.cloud.toast.com/6cdebe3eb0094910bc41f1d42ebe4cb7/flavors/31fa632d-aeec-4f12-8a57-ce9d146228e5",
          "rel": "bookmark"
        }
      ],
      "ram": 131072,
      "OS-FLV-DISABLED:disabled": false,
      "vcpus": 32,
      "extra_specs": {
        "flavor_type": "performance"
      },
      "swap": "",
      "os-flavor-access:is_public": true,
      "rxtx_factor": 1.0,
      "OS-FLV-EXT-DATA:ephemeral": 0,
      "disk": 0,
      "id": "31fa632d-aeec-4f12-8a57-ce9d146228e5"
    }
  ]
}
```

</p>
</details>

---

## Availability Zone

### List Availability 

```
GET /v2/{tenantId}/os-availability-zone
X-Auth-Token: {tokenId}
```

#### Request
This API does not require a request body. 

| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| tenantId | URL | String | O | Tenant ID |
| tokenId | Header | String | O | Token ID |

#### Response
| Name | Type | Format | Description |
|---|---|---|---|
| availabilityZoneInfo | Body | Object | Information object of availability zone |
| availabilityZoneInfo.zoneName | Body | String | Name of availability zone |
| availabilityZoneInfo.zoneState | Body | Object | Information object of availability zone status |
| availabilityZoneInfo.available | Body | Object | Availability zone status |

<details><summary>Example</summary>
<p>


```json
{
    "availabilityZoneInfo": [
      {
        "zoneState": {
          "available": true
        },
        "zoneName": "kr-pub-a"
      },
      {
        "zoneState": {
          "available": true
        },
        "zoneName": "kr-pub-b"
      }
    ]
}
```

</p>
</details>

---

## Keypair

### List Keypairs
```
GET /v2/{tenantId}/os-keypairs
X-Auth-Token: {tokenId}
```

#### Request
This API does not require a request body. 

| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| tenantId | URL | String | O | Tenant ID |
| tokenId | Header | String | O | Token ID |

#### Response

| Name | Type | Format | Description |
|---|---|---|---|
| keypairs | Body | Array | List of keypair objects |
| keypairs.keypair | Body | Object | Keypair object |
| keypairs.keypair.name | Body | String | Keypair name |
| keypairs.keypair.public_key | Body | String | Pubic key |
| keypairs.keypair.fingerprint | Body | String | Keypair fingerprint |

<details><summary>Example</summary>
<p>


```json
{
  "keypairs": [
    {
      "keypair": {
        "public_key": "ssh-rsa ... Generated-by-Nova",
        "name": "keypair",
        "fingerprint": "SHA256:..."
      }
    }
  ]
}
```

</p>
</details>

---

### Get Keypair
```
GET /v2/{tenantId}/os-keypairs/{keypairName}
X-Auth-Token: {tokenId}
```

#### Request
This API does not require a request body. 

| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| tenantId | URL | String | O | Tenant ID |
| keypairName | URL | String | O | Keypair Name |
| tokenId | Header | String | O | Token ID |

#### Response	

| Name | Type | Format | Description |
|---|---|---|---|
| keypair | Body | Object | List of keypair objects |
| keypair.public_key | Body | String | Public key |
| keypair.user_id | Body | String | Keypair owner ID |
| keypair.name | Body | String | Keypair name |
| keypair.deleted | Body | Boolean | Delete keypair or not |
| keypair.created_at | Body | Datetime | Keypair created time <br>`YYYY-MM-DDThh:mm:ss.SSSSSS` |
| keypair.updated_at | Body | Datetime | Keypair updated time<br>`YYYY-MM-DDThh:mm:ss.SSSSSS` |
| keypair.deleted_at | Body | Datetime | Keypair deleted time<br>`YYYY-MM-DDThh:mm:ss.SSSSSS` |
| keypair.fingerprint | Body | String | Keypair fingerprint |
| keypair.id | Body | Integer | Keypair ID |

<details><summary>Example</summary>
<p>


```json
{
  "keypair": {
    "public_key": "ssh-rsa ... Generated-by-Nova",
    "user_id": "826a1213b3f746829515486965690dfe",
    "name": "keypair",
    "deleted": false,
    "created_at": "2020-02-07T03:46:48.000000",
    "updated_at": null,
    "fingerprint": "SHA256:...",
    "deleted_at": null,
    "id": 51
  }
}
```

</p>
</details>

---

### Create/Register Keypair

```
POST /v2/{tenantId}/os-keypairs
X-Auth-Token: {tokenId}
```

#### Request		

| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| tenantId | URL | String | O | Tenant ID |
| tokenId | Header | String | O | Token ID |
| keypair | Body | Object | O | Keypair object |
| keypair.name | Body | String | O | Keypair name to create or register |
| keypair.public_key | Body | String | - | Public key to register: if left blank, a new keypair is created. |

<details><summary>Example</summary>
<p>


```json
{
    "keypair": {
        "name": "keypair-d20a3d59-9433-4b79-8726-20b431d89c78",
        "public_key": "ssh-rsa ... Generated-by-Nova"
    }
}
```

</p>
</details>

#### Response

| Name | Type | Format | Description |
|---|---|---|---|
| keypair | Body | Object | Keypair object |
| keypair.public_key | Body | String | Public key |
| keypair.private_key | Body | String | Secret key: to be returned, if a new keypair is created. |
| keypair.user_id | Body | String | Keypair owner ID |
| keypair.name | Body | String | Keypair name |
| keypair.fingerprint | Body | String | Keypair fingerprint |

<details><summary>Example</summary>
<p>


```json
{
    "keypair": {
        "fingerprint": "SHA256:+EZoD ... /DKiGnY4zf5tYrcix0",
        "name": "keypair",
        "public_key": "ssh-rsa ... Generated-by-Nova",
        "user_id": "436f727b7c9142f896ddd56be591dd7f"
    }
}
```

</p>
</details>

---

### Delete Keypair
```
DELETE /v2/{tenantId}/os-keypairs/{keypairName}
X-Auth-Token: {tokenId}
```

#### Request
This API does not require a request body. 

| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| tenantId | URL | String | O | Tenant ID |
| keypairName | URL | String | O | Keypair name |
| tokenId | Header | String | O | Token ID |

#### Response
This API does not return a response body. 


## Instance

### Status

Instances exist in various statuses, and each status defines operations. See the list of instance statuses like below:

| Status Name | Description |
|--|--|
| `ACTIVE` | Instance is activated |
| `BUILDING` | Instance is under buildup |
| `STOPPED`| Instance is closed |
| `DELETED`| Instance is deleted |
| `REBOOT`| Instance is rebooted |
| `HARD_REBOOT`| Instance is forced for a reboot <br>Operates the same as turning off and turning on the physical server again |
| `RESIZED`| Instance type is changed or instance is moved to another host <br>Instance is closed and restarted |
| `REVERT_RESIZE`| Instance is restored to the original status, when it failed to change instance type or move to another host |
| `VERIFY_RESIZE`| Instance is waiting to be approved after its type is changed or it is moved to another host <br>The status is automatically changed to `ACTIVE`. |
| `ERROR`| Operation on the previous instance has failed |
| `PAUSED`| Instance is paused<br>Paused instance is saved in the memory of hypervisor |
| `REBUILD`| Instance is newly built from the image when created |
| `RESCUED`| Instance is executed in the recovery mode. |
| `SUSPENDED`| Instance has entered in the maximum saving mode by administrator |
| `UNKNOWN`| Instance status is unknown <br>`Contact administraotr if the instance has entered into this status.` |

### List Instances

```
GET /v2/{tenantId}/servers
X-Auth-Token: {tokenId}
```

#### Request

This API does not require a request body. 

| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| tenantId | URL | String | O | Tenant ID |
| tokenId | Header | String | O | Token ID |
| reservation_id | Query | String | - | Reservation ID for instance creation. <br>With reservation ID specified, return only such list of instances that are simultaneously created |
| changes-since | Query | Datetime | - | Return instance list changed after specified time: in the format of `YYYY-MM-DDThh:mm:ss`. |
| image | Query | UUID | - | Image ID<br>Return instance list using specified images |
| flavor | Query | UUID | - | Instance type ID<br>Return instance list using specified types |
| name | Query | String | - | Instance name<br>Return the list of instances with specified names, to be questioned in the regex format |
| status | Query | Enum | - | Instance status<br>Return the list of instances with specified status |
| limit | Query | Integer | - | Instance count on the list <br>Return the list of instances as many as specified |
| marker | Query | UUID | - | First instance UUID on the list <br>Return the list of instance as many as `limit` from the instance specified as the `marker`, according to sorting criteria |

#### Response

| Name | Type | Format | Description |
|---|---|---|---|
| servers | Body | Object | Instance list object |
| id | Body | UUID | Instance UUID |
| links | body | Object | Instance path object |
| name | body | String | Instance name |

<details><summary>Example</summary>
<p>


```json
{
  "servers": [
    {
      "id": "aaf2778b-ea03-4ccc-8b1b-92f4b686c3ec",
      "links": [
        {
          "href": "https://kr1-api-instance.infrastructure.cloud.toast.com/v2/6cdebe3eb0094910bc41f1d42ebe4cb7/servers/aaf2778b-ea03-4ccc-8b1b-92f4b686c3ec",
          "rel": "self"
        },
        {
          "href": "https://kr1-api-instance.infrastructure.cloud.toast.com/6cdebe3eb0094910bc41f1d42ebe4cb7/servers/aaf2778b-ea03-4ccc-8b1b-92f4b686c3ec",
          "rel": "bookmark"
        }
      ],
      "name": "Web-Server"
    }
  ]
}
```

</p>
</details>

---

### List Instance Details 

Return the list of instances created in the current tenant, same as List Instances. However, detail instance information is queried as well. 

```
GET /v2/{tenantId}/servers/detail
X-Auth-Token: {tokenId}
```

#### Request

The request format is same as List Instances. 

#### Response

| Name | Type | Format | Description |
|---|---|---|---|
| servers | body | Object | Instance list object |
| status | body | Enum | Instance status |
| servers.id | Body | UUID | Instance ID |
| servers.name | Body | String | Instance name, no more than 255 letters |
| servers.updated | Body | Datetime | Last updated time of instance in the  `YYYY-MM-DDThh:mm:ssZ` format |
| servers.hostId | Body | String | ID of host running instance |
| servers.addresses | Body | Object | List object of instance IP. <br>The list is created as many as ports attached to instance. |
| servers.addresses."Network Name" | Body | Object | Port information of each network associated with instance |
| servers.addresses."Network Name".OS-EXT-IPS-MAC:mac_addr | Body | String | MAC address of port associated with instance |
| servers.addresses."Network Name".version | Body | Integer | IP version of port associated with instance <br>NHN Cloud supports only IPv4 |
| servers.addresses."Network Name".addr | Body | String | IP address of port associated with instance |
| servers.addresses."Network Name".OS-EXT-IPS:type | Body | Enum | IP address type of port <br>Either `fixed` or `floating` |
| servers.links | Body | Object | Instance path object |
| servers.key_name | Body | String | Instance keypair name |
| servers.image | Body | Object | Instance image object |
| servers.image.id | Body | UUID | Instance image ID |
| servers.image.links | Body | Object | Path object of instance image |
| servers.OS-EXT-STS:task_state | Body | String | Instance operation status <br>Notifies the progress of an operation for instance |
| servers.OS-EXT-STS:vm_state | Body | String | Current instance status |
| servers.OS-SRV-USG:launched_at | Body | Datetime | Last instance booted time <br>In the`YYYY-MM-DDThh:mm:ss.ssssss` format |
| servers.OS-SRV-USG:terminated_at | Body | Datetime | Instance deleted time<br>In the`YYYY-MM-DDThh:mm:ssZ` format |
| servers.flavor | Body | Object | Information object of instance type |
| servers.flavor.id | Body | UUID | Instance type ID |
| servers.flavor.links | Body | Object | Path object of instance type |
| servers.security_groups | Body | Object | List object of security group assigned to instance |
| servers.security_groups.name | Body | String | Name of security group assigned to instance |
| servers.user_id | Body | String | ID of user creating instance |
| servers.created | Body | Datetime | Instance created time. In the `YYYY-MM-DDThh:mm:ssZ` format |
| servers.tenant_id | Body | String | ID of tenant that includes instance |
| servers.OS-DCF:diskConfig | Body | Enum | Disk partition method of instance, either `MANUAL` or `AUTO` <br>**AUTO**: Automatically set the entire disk as a partition <br>**MANUAL**: Set partition as specified for each image. If a disk size is larger than the image setting, leave it unused. NHN Cloud adopts `MANUAL`. |
| servers.os-extended-volumes:volumes_attached | Body | Object | List object of additional volume attached to instance |
| servers.os-extended-volumes:volumes_attached.id | Body | UUID | ID of additional volume attached to instance |
| servers.OS-EXT-STS:power_state | Body | Integer | Power state of instance<br>- `1`: On<br>- `4`: Off |
| servers.metadata | Body | Object | Instance metadata object<br>Store instance metadata in the key-value pair |

<details><summary>Example</summary>
<p>


```json
{
  "servers": [
    {
      "status": "ACTIVE",
      "updated": "2020-02-25T01:22:24Z",
      "hostId": "078d06f898889699f8731d030812e43d2c417edb2cf641dda598c7bd",
      "addresses": {
        "vpc2": [
          {
            "OS-EXT-IPS-MAC:mac_addr": "fa:16:3e:54:a7:64",
            "version": 4,
            "addr": "172.16.0.40",
            "OS-EXT-IPS:type": "fixed"
          }
        ]
      },
      "links": [
        {
          "href": "https://kr1-api-instance.infrastructure.cloud.toast.com/v2/6cdebe3eb0094910bc41f1d42ebe4cb7/servers/aaf2778b-ea03-4ccc-8b1b-92f4b686c3ec",
          "rel": "self"
        },
        {
          "href": "https://kr1-api-instance.infrastructure.cloud.toast.com/6cdebe3eb0094910bc41f1d42ebe4cb7/servers/aaf2778b-ea03-4ccc-8b1b-92f4b686c3ec",
          "rel": "bookmark"
        }
      ],
      "key_name": "access-key",
      "image": {
        "id": "8b9f8d47-b89b-45af-b1d6-3f7ce7e06a11",
        "links": [
          {
            "href": "https://kr1-api-instance.infrastructure.cloud.toast.com/6cdebe3eb0094910bc41f1d42ebe4cb7/images/8b9f8d47-b89b-45af-b1d6-3f7ce7e06a11",
            "rel": "bookmark"
          }
        ]
      },
      "OS-EXT-STS:task_state": null,
      "OS-EXT-STS:vm_state": "active",
      "OS-SRV-USG:launched_at": "2020-02-25T01:22:23.000000",
      "flavor": {
        "id": "35a73b57-58a7-434d-aa08-5249aaa95b3e",
        "links": [
          {
            "href": "https://kr1-api-instance.infrastructure.cloud.toast.com/6cdebe3eb0094910bc41f1d42ebe4cb7/flavors/35a73b57-58a7-434d-aa08-5249aaa95b3e",
            "rel": "bookmark"
          }
        ]
      },
      "id": "aaf2778b-ea03-4ccc-8b1b-92f4b686c3ec",
      "security_groups": [
        {
          "name": "default"
        }
      ],
      "OS-SRV-USG:terminated_at": null,
      "OS-EXT-AZ:availability_zone": "kr-pub-b",
      "user_id": "b6ab578c20c94306ac1f41ffc4415b29",
      "name": "Web-Server",
      "created": "2020-02-25T01:15:46Z",
      "tenant_id": "6cdebe3eb0094910bc41f1d42ebe4cb7",
      "OS-DCF:diskConfig": "MANUAL",
      "os-extended-volumes:volumes_attached": [
        {
          "id": "90712f4f-2faa-4e4f-8eb1-9313a8595570"
        }
      ],
      "accessIPv4": "",
      "accessIPv6": "",
      "progress": 0,
      "OS-EXT-STS:power_state": 1,
      "config_drive": "",
      "metadata": {
        "os_distro": "Windows",
        "description": "Windows 2012 R2 STD (2020.02.18)",
        "os_version": "2012 R2 STD",
        "project_domain": "NORMAL",
        "hypervisor_type": "qemu",
        "monitoring_agent": "sysmon",
        "image_name": "Windows 2012 R2 STD (2020.02.18) EN",
        "volume_size": "50",
        "os_architecture": "amd64",
        "login_username": "Administrator",
        "os_type": "Windows",
        "tc_env": "sysmon"
      }
    }
  ]
}
```

</p>
</details>

---

### Get Instance

```
GET /v2/{tenantId}/servers/{serverId}
X-Auth-Token: {tokenId}
```

#### Request

This API does not require a request body. 

| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| tenantId | URL | String | O | Tenant ID |
| serverId | URL | UUID | O | Instance ID |
| tokenId | Header | String | O | Token ID |

#### Response

| Name | Type | Format | Description |
|---|---|---|---|
| server | body | Object | Instance object |
| status | body | Enum | Instance status |
| server.id | Body | UUID | Instance ID |
| server.name | Body | String | Instance name, no more than 255 letters |
| server.updated | Body | Datetime | Last updated time of instance, in the `YYYY-MM-DDThh:mm:ssZ` format |
| server.hostId | Body | String | ID of host running instance |
| server.addresses | Body | Object | List object of instance IP <br>List is created as many as ports attached to instance |
| server.addresses."Network Name" | Body | Object | Port information of each network associated with instance |
| server.addresses."Network Name".OS-EXT-IPS-MAC:mac_addr | Body | String | MAC address of port associated with instance |
| server.addresses."Network Name".version | Body | Integer | IP version of port associated with instance <br>NHN Cloud supports only IPv4 |
| server.addresses."Network Name".addr | Body | String | IP address of port associated with instance |
| server.addresses."Network Name".OS-EXT-IPS:type | Body | Enum | IP address type of port <br>Either `fixed` or `floating` |
| server.links | Body | Object | Instance path object |
| server.key_name | Body | String | Instance keypair name |
| server.image | Body | Object | Instance image object |
| server.image.id | Body | UUID | Instance image ID |
| server.image.links | Body | Object | Path object of instance image |
| server.OS-EXT-STS:task_state | Body | String | Instance operation status<br>Notifies the progress of an operation for instance |
| server.OS-EXT-STS:vm_state | Body | String | Current instance status |
| server.OS-SRV-USG:launched_at | Body | Datetime | Last instance booted time <br>In the`YYYY-MM-DDThh:mm:ss.ssssss` format |
| server.OS-SRV-USG:terminated_at | Body | Datetime | Instance deleted time<br>In the`YYYY-MM-DDThh:mm:ssZ` format |
| server.flavor | Body | Object | Information object of instance type |
| server.flavor.id | Body | UUID | Instance type ID |
| server.flavor.links | Body | Object | Path object of instance type |
| server.security_groups | Body | Object | List object of security group assigned to instance |
| server.security_groups.name | Body | String | Name of security group assigned to instance |
| server.user_id | Body | String | ID of user creating instance |
| server.created | Body | Datetime | Instance created time, in the `YYYY-MM-DDThh:mm:ssZ` format |
| server.tenant_id | Body | String | ID of tenant including instance |
| server.OS-DCF:diskConfig | Body | Enum | Disk partition method of instance, either `MANUAL` or `AUTO` <br/>**AUTO**: Automatically set the entire disk as a partition 
**MANUAL**: Set partition as specified for each image. If a disk size is larger than the image setting, leave it unused. Adopts `MANUAL` for NHN Cloud. |
| server.os-extended-volumes:volumes_attached | Body | Object | List object of additional volume attached to instance |
| server.os-extended-volumes:volumes_attached.id | Body | UUID | ID of additional volume attached to instance |
| server.OS-EXT-STS:power_state | Body | Integer | Power state of instance<br>- `1`: On<br>- `4`: Off |
| server.metadata | Body | Object | Instance metadata object<br/>Store instance metadata in the key-value pair |

<details><summary>Example</summary>
<p>


```json
{
  "server": {
    "status": "ACTIVE",
    "updated": "2020-02-25T01:22:24Z",
    "hostId": "078d06f898889699f8731d030812e43d2c417edb2cf641dda598c7bd",
    "addresses": {
      "vpc2": [
        {
          "OS-EXT-IPS-MAC:mac_addr": "fa:16:3e:54:a7:64",
          "version": 4,
          "addr": "172.16.0.40",
          "OS-EXT-IPS:type": "fixed"
        }
      ]
    },
    "links": [
      {
        "href": "https://kr1-api-instance.infrastructure.cloud.toast.com/v2/6cdebe3eb0094910bc41f1d42ebe4cb7/servers/aaf2778b-ea03-4ccc-8b1b-92f4b686c3ec",
        "rel": "self"
      },
      {
        "href": "https://kr1-api-instance.infrastructure.cloud.toast.com/6cdebe3eb0094910bc41f1d42ebe4cb7/servers/aaf2778b-ea03-4ccc-8b1b-92f4b686c3ec",
        "rel": "bookmark"
      }
    ],
    "key_name": "access-key",
    "image": {
      "id": "8b9f8d47-b89b-45af-b1d6-3f7ce7e06a11",
      "links": [
        {
          "href": "https://kr1-api-instance.infrastructure.cloud.toast.com/6cdebe3eb0094910bc41f1d42ebe4cb7/images/8b9f8d47-b89b-45af-b1d6-3f7ce7e06a11",
          "rel": "bookmark"
        }
      ]
    },
    "OS-EXT-STS:task_state": null,
    "OS-EXT-STS:vm_state": "active",
    "OS-SRV-USG:launched_at": "2020-02-25T01:22:23.000000",
    "flavor": {
      "id": "35a73b57-58a7-434d-aa08-5249aaa95b3e",
      "links": [
        {
          "href": "https://kr1-api-instance.infrastructure.cloud.toast.com/6cdebe3eb0094910bc41f1d42ebe4cb7/flavors/35a73b57-58a7-434d-aa08-5249aaa95b3e",
          "rel": "bookmark"
        }
      ]
    },
    "id": "aaf2778b-ea03-4ccc-8b1b-92f4b686c3ec",
    "security_groups": [
      {
        "name": "default"
      }
    ],
    "OS-SRV-USG:terminated_at": null,
    "OS-EXT-AZ:availability_zone": "kr-pub-b",
    "user_id": "b6ab578c20c94306ac1f41ffc4415b29",
    "name": "Web-Server",
    "created": "2020-02-25T01:15:46Z",
    "tenant_id": "6cdebe3eb0094910bc41f1d42ebe4cb7",
    "OS-DCF:diskConfig": "MANUAL",
    "os-extended-volumes:volumes_attached": [
      {
        "id": "90712f4f-2faa-4e4f-8eb1-9313a8595570"
      }
    ],
    "accessIPv4": "",
    "accessIPv6": "",
    "progress": 0,
    "OS-EXT-STS:power_state": 1,
    "config_drive": "",
    "metadata": {
      "os_distro": "Windows",
      "description": "Windows 2012 R2 STD (2020.02.18)",
      "os_version": "2012 R2 STD",
      "project_domain": "NORMAL",
      "hypervisor_type": "qemu",
      "monitoring_agent": "sysmon",
      "image_name": "Windows 2012 R2 STD (2020.02.18) EN",
      "volume_size": "50",
      "os_architecture": "amd64",
      "login_username": "Administrator",
      "os_type": "Windows",
      "tc_env": "sysmon"
    }
  }
}
```

</p>
</details>

---

### Create Instance

Create an instance. 

Call Create Instances API and query instance to check its status. 

* When the status becomes **ACTIVE**, the instance is normally created. 
* When the instance remains **BUILDING** for a long time or is in **ERROR**, check parameter for instance creation and try creation again. 

For Window instances, follow the restrictions as below for stable operations:  

* Use an instance type with larger than 2GB RAM capacity.  
* Default disk must be larger than 50GB. 
* U2-type instances cannot use Windows images.  

By default, disk size of Linux is 10GB while that of Windows can be specified between 50GB and 1TB. 


```
POST /v2/{tenantId}/servers
X-Auth-Token: {tokenId}
```

#### Request

| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| tenantId | URL | String | O | Tenant ID |
| tokenId | Header | String | O | Token ID |
| server.security_groups | body | Object | - | List object of security group<br>If left blank, the `default` group is added. |
| server.security_groups.name | body | String | - | Name of security group to be added to an instance |
| server.user_data | body | String | - | Script to be executed after instance booting, and setting <br>Allows up to 65535 bytes of base 64-encoded character strings |
| server.availability_zone | body | String | - | Availability area to create instances<br>Without specified, a random area is to be selected. |
| server.imageRef | Body | String | O | Image ID to create instances |
| server.flavorRef | Body | String | O | ID of instance type to be adopted to create instances |
| server.networks | Body | Object | O | Network information object to create instances <br>NIC is added as many as specified, by one out of Network ID, Subnet ID, Port ID, or Fixed IP. |
| server.networks.uuid | Body | UUID | - | Network ID to create instances |
| server.networks.subnet | Body | UUID | - | Subnet ID of network to create instances |
| server.networks.port | Body | UUID | - | Port ID to create instances |
| server.networks.fixed_ip | Body | String | - | Fixed IP to create instances |
| server.name | Body | String | O | Instance name<br>Allows up to 255 letters for English, but less than 15 letters for Windows images |
| server.metadata | Body | Object | - | Metadata object to be added to an instance<br>Key-value pairs with less than 255 letters |
| server.block_device_mapping_v2 | Body | Object | - | Block storage information object of instance<br> **Must be specified for any instance types other than U2 on local disks**|
| server.block_device_mapping_v2.uuid | Body | String | - | Original ID of block storage <br> The original must be bootable for a root volume, and a volume or snapshot that has WAF, from which images cannot be created, or MS-SQL images as the original, are not available. <br> The original excluding `image` must have the same availability zone with an instance to create. |
| server.block_device_mapping_v2.source_type | Body | Enum | - | Type of block storage original to create <br>`image`: Use image to create a block storage <br>`volume`: Use previously-created volume, with the destination_type specified by volume <br>`snapshot`: Use snapshot to create a block storage, with the destination_type specified by volume |
| server.block_device_mapping_v2.destination_type | Body | Enum | - | Requires different settings for each location or type of instance volume. <br>- `local`: For U2-instance types<br>- `volume`: For other instances types |
| server.block_device_mapping_v2.delete_on_termination | Body | Boolean | - | Process volume when instance is deleted, with  `false` as default <br>With`true`, delete; with`false`, remain |
| server.block_device_mapping_v2.boot_index | Body | Integer | - | Booting order of specified volumes<br>- Root volume for`0`<br>- Additional volumes for others<br>The larger, the lower on the booting order |
| server.key_name | Body | String | O | Keypair to access instances |
| server.min_count | Body | Integer | - | Minimum value of instance count to be created at the current request.<br>Default is 1. |
| server.max_count | Body | Integer | - | Maximum value of instance count to be created at the current request. <br>Default is min_count,  with 10 at the max. |
| server.return_reservation_id | Body | Boolean | - | Request reservation ID for instance creation.<br>With True, reservation ID is returned instead of instance creation information.<br>Default is False |

<details><summary>Example</summary>
<p>


```json
{
  "server": {
    "name": "DB-Master",
    "imageRef": "9956f822-29c9-4f81-9410-0c392d9c8c24",
    "flavorRef": "a4b6a0f7-aeff-4d78-a8d5-7de9f007012d",
    "networks": [{
      "subnet": "b83863ff-0355-4c73-8c10-0bdf66a69aab"
    }],
    "availability_zone": "kr-pub-a",
    "key_name": "access-key",
    "max_count": 1,
    "min_count": 1,
    "block_device_mapping_v2": [{
      "uuid": "9956f822-29c9-4f81-9410-0c392d9c8c24",
      "boot_index": 0,
      "volume_size": 1000,
      "device_name": "vda",
      "source_type": "image",
      "destination_type": "volume",
      "delete_on_termination": 1
    }],
    "security_groups": [{
      "name": "default"
    }]
  }
}
```

</p>
</details>

#### Response

| Name | Type | Format | Description |
|---|---|---|---|
| server.security_groups.name | Body | String | Security group name of created instance |
| server.OS-DCF:diskConfig | Body | Enum | Disk partition method of instance, either `MANUAL` or `AUTO`. Adopts`MANUAL` for NHN Cloud. <br/>**AUTO**: Automatically set the entire disk as a partition 
**MANUAL**: Set partition as specified for each image. If a disk size is larger than the image setting, leave it unused. |
| server.id | Body | UUID | ID of created instance |

<details><summary>Example</summary>
<p>


```json
{
  "server": {
    "security_groups": [
      {
        "name": "default"
      }
    ],
    "OS-DCF:diskConfig": "MANUAL",
    "id": "3a005d5b-63cf-4493-bfc6-49db990b5b50",
    "links": [
      {
        "href": "https://kr1-api-instance.infrastructure.cloud.toast.com/v2/6cdebe3eb0094910bc41f1d42ebe4cb7/servers/3a005d5b-63cf-4493-bfc6-49db990b5b50",
        "rel": "self"
      },
      {
        "href": "https://kr1-api-instance.infrastructure.cloud.toast.com/6cdebe3eb0094910bc41f1d42ebe4cb7/servers/3a005d5b-63cf-4493-bfc6-49db990b5b50",
        "rel": "bookmark"
      }
    ]
  }
}
```

</p>
</details>

---

### Modify Instance 
Modify created instance. Attributes available for change are restricted to some items. 

```
PUT /v2/{tenantId}/servers/{serverId}
X-Auth-Token: {tokenId}
```

#### Request

| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| tenantId | URL | String | O | Tenant ID |
| serverId | URL | UUID | O | Instance ID to change |
| tokenId | Header | String | O | Token ID |
| server | Body | Object | O | Object requesting of changing instances |
| server.name | Body | String | - | New instance name |

<details><summary>Example</summary>
<p>


```json
{
    "server": {
        "name": "new-server-test"
    }
}
```

</p>
</details>

#### Response
Same as Get Instance.

---

### Delete Instance 
Delete a created instance. 

```
DELETE /v2/{tenantId}/servers/{serverId}
X-Auth-Token: {tokenId}
```

#### Request
This API does not require a request body. 

| Name | Type | Format | Required | Description |
|---|---|---|---|--|
| tenantId | URL | String | O | Tenant ID |
| serverId | URL | UUID | O | Instance ID to delete |
| tokenId | Header | String | O | Token ID |

#### Response
This API does not return a response body. 

---

## Volume Attachment 
### List Volumes Attached to Instance
```
GET /v2/{tenantId}/servers/{serverId}/os-volume_attachments
X-Auth-Token: {tokenId}
```

#### Request
This API doesnot require a request body. 

| Name | Type | Format | Required | Description |
|---|---|---|---|--|
| tenantId | URL | String | O | Tenant ID |
| serverId | URL | UUID | O | Instance ID to change |
| tokenId | Header | String | O | Token ID |
| limit | Query | Integer | - | List count to query |
| offset | Query | Integer | - | Start point of list to return<br>Return from the offset volume from the entire list |

#### Response

| Name | Type | Format | Description |
|---|---|---|---|
| volumeAttachments | Body | Array | List of attachment information objects |
| volumeAttachments.device | Body | String | Volume name of instance<br>e.g.) `/dev/vdb` |
| volumeAttachments.id | Body | UUID | Attachment information ID |
| volumeAttachments.serverId | Body | UUID | Instance ID |
| volumeAttachments.volumeId | Body | UUID | Volume ID |

<details><summary>Example</summary>
<p>


```json
{
    "volumeAttachments": [
        {
            "device": "/dev/vda",
            "id": "227cc671-f30b-4488-96fd-7d0bf13648d8",
            "serverId": "4b293d31-ebd5-4a7f-be03-874b90021e54",
            "volumeId": "227cc671-f30b-4488-96fd-7d0bf13648d8"
        },
        {
            "device": "/dev/vdb",
            "id": "a07f71dc-8151-4e7d-a0cc-cd24a3f11113",
            "serverId": "4b293d31-ebd5-4a7f-be03-874b90021e54",
            "volumeId": "a07f71dc-8151-4e7d-a0cc-cd24a3f11113"
        }
    ]
}
```

</p>
</details>

---

### Get Volume Attached to Instance 
```
GET /v2/{tenantId}/servers/{serverId}/os-volume_attachments/{volumeId}
X-Auth-Token: {tokenId}
```

#### Request
This API does not require a request body. 

| Name | Type | Format | Required | Description |
|---|---|---|---|--|
| tenantId | URL | String | O | Tenant ID |
| serverId | URL | UUID | O | Instance ID |
| volumeId | URL | UUID | O | Volume ID to query |
| tokenId | Header | String | O | Token ID |

#### Response

| Name | Type | Format | Description |
|---|---|---|---|
| volumeAttachment | Body | Object | Attachment information object |
| volumeAttachment.device | Body | String | Volume name of instance<br>e.g.) `/dev/vdb` |
| volumeAttachment.id | Body | UUID | Attachment information ID |
| volumeAttachment.serverId | Body | UUID | Instance ID |
| volumeAttachment.volumeId | Body | UUID | Volume ID |

<details><summary>Example</summary>
<p>


```json
{
    "volumeAttachment": {
        "device": "/dev/sdb",
        "id": "a07f71dc-8151-4e7d-a0cc-cd24a3f11113",
        "serverId": "1ad6852e-6605-4510-b639-d0bff864b49a",
        "volumeId": "a07f71dc-8151-4e7d-a0cc-cd24a3f11113"
    }
}
```

</p>
</details>

---

### Attach More Volumes to Instance 
```
POST /v2/{tenantId}/servers/{serverId}/os-volume_attachments
X-Auth-Token: {tokenId}
```

#### Request

| Name | Type | Format | Required | Description |
|---|---|---|---|--|
| tenantId | URL | String | O | Tenant ID |
| serverId | URL | UUID | O | Instance ID to change |
| tokenId | Header | String | O | Token ID |
| volumeAttachment | Body | Object | O | Object requesting of volume attachment |
| volumeAttachment.volumeId | Body | UUID | O | Volume ID to attach |

<details><summary>Example</summary>
<p>


```json
{
  "volumeAttachment": {
      "volumeId": "a07f71dc-8151-4e7d-a0cc-cd24a3f11113"
  }
}
```

</p>
</details>

#### Response

| Name | Type | Format | Description |
|---|---|---|---|
| volumeAttachment | Body | Object | Attachment information object |
| volumeAttachment.device | Body | String | Volume name of instance<br>e.g.) `/dev/vdb` |
| volumeAttachment.id | Body | UUID | Attachment information ID |
| volumeAttachment.serverId | Body | UUID | Instancne ID |
| volumeAttachment.volumeId | Body | UUID | Volume ID |

<details><summary>Example</summary>
<p>


```json
{
    "volumeAttachment": {
        "device": "/dev/vdc",
        "id": "227cc671-f30b-4488-96fd-7d0bf13648d8",
        "serverId": "4b293d31-ebd5-4a7f-be03-874b90021e54",
        "volumeId": "227cc671-f30b-4488-96fd-7d0bf13648d8"
    }
}
```

</p>
</details>

---

### Detach Volume from Instance
```
DELETE /v2/{tenantId}/servers/{serverId}/os-volume_attachments/{volumeId}
X-Auth-Token: {tokenId}
```

#### Request
This API does not require a request body. 

| Name | Type | Format | Required | Description |
|---|---|---|---|--|
| tenantId | URL | String | O | Tenant ID |
| serverId | URL | UUID | O | Instance ID |
| volumeId | URL | UUID | O | Volume ID to detach |
| tokenId | Header | String | O | Token ID |

#### Response
This API does not return a response body. 

---

## Additional Features 
NHN Cloud provides the following features regarding instance control with additional features: 

* Start, Close, and Restart Instance 
* Change Instance Type
* Create Instance Image
* Add/Delete Security Group 

### Start Instance

Restart a closed instance and change the status into **ACTIVE**. To call this API, the instance status must be **SHUTOFF**. 

```
POST /v2/{tenantId}/servers/{serverId}/action
X-Auth-Token: {tokenId}
```

#### Request
| Name | Type | Format | Required | Description |
|---|---|---|---|--|
| tenantId | URL | String | O | Tenant ID |
| serverId | URL | UUID | O | Instance ID to change |
| tokenId | Header | String | O | Token ID |
| os-start | Body | none | O | Request of starting instances |

<details><summary>Example</summary>
<p>


```json
{
  "os-start" : null
}
```

</p>
</details>

---

#### Response
This API does not return a response body.  

### Suspend Instance

Stop instance and change its status to **SHUTOFF**. To call this API, the instance must be either **ACTIVE** or **ERROR**. 

```
POST /v2/{tenantId}/servers/{serverId}/action
X-Auth-Token: {tokenId}
```

#### Request
| Name | Type | Format | Required | Description |
|---|---|---|---|--|
| tenantId | URL | String | O | Tenant ID |
| serverId | URL | UUID | O | Instance ID to change |
| tokenId | Header | String | O | Token ID |
| os-stop | Body | none | O | Request to suspend instance |

<details><summary>Example</summary>
<p>


```json
{
  "os-stop" : null
}
```

</p>
</details>

#### Response
This API does not return a response body. 

---

### Restart Instance

Restart an instance, by using **SOFT** or **HARD** method. 

* **SOFT**: With **"Graceful Shutdown"**, close the instance and restart it. The instance must be **ACTIVE**. 
* **HARD**: Force to close the instance and restart it. It works the same way as turning off and on again a physical server. Forced closure is available only on the following statuses: 
    * **ACTIVE**
    * **ERROR**
    * **HARD_REBOOT**
    * **PAUSED**
    * **REBOOT**
    * **SHUTOFF**
    * **SUSPENDED**

```
POST /v2/{tenantId}/servers/{serverId}/action
X-Auth-Token: {tokenId}
```

#### Request
| Name | Type | Format | Required | Description |
|---|---|---|---|--|
| tenantId | URL | String | O | Tenant ID |
| serverId | URL | UUID | O | Instance ID to change |
| tokenId | Header | String | O | Token ID |
| reboot | Body | Object | O | Object requesting of rebooting instance |
| reboot.type | Body | Enum | O | Reboot type: **SOFT** or **HARD** |

<details><summary>Example</summary>
<p>


```json
{
  "reboot" : {
    "type": "SOFT"
  }
}
```

</p>
</details>

#### Response
This API does not return a response body. 

---

### Change Instance Type 

Change the type of an instance. Types can be changed only for **ACTIVE** or **SHUTOFF** instances. When the instance is **ACTIVE**, the instance is closed and restarted while the type is changed.

Type change may be restricted depending on the image or instance type. For more details, see Console User Guide. 


```
POST /v2/{tenantId}/servers/{serverId}/action
X-Auth-Token: {tokenId}
```

#### Request
| Name | Type | Format | Required | Description |
|---|---|---|---|--|
| tenantId | URL | String | O | Tenant ID |
| serverId | URL | UUID | O | Instance ID to change |
| tokenId | Header | String | O | Token ID |
| resize | Body | Object | O | Request of changing instance type |
| resize.flavorRef | Body | UUID | O | Instance type ID to change |
| resize.OS-DCF:diskConfig | Body | Enum | - | Disk partition method of instance, either `MANUAL` or `AUTO`. Adopts`MANUAL` for NHN Cloud. <br/>**AUTO**: Automatically set the entire disk as a partition 
**MANUAL**: Set partition as specified for each image. If a disk size is larger than the image setting, leave it unused. |

<details><summary>Example</summary>
<p>


```json
{
  "resize" : {
    "flavorRef": "b5f1c148-732c-417d-9d1b-1dffca105dbe"
  }
}
```

</p>
</details>

#### Response
This API does not return a response body. 

---

### Create Instance Image

Create an image from instance. Only `U2`-type instance images can be created via this API. To create images of other types of instances, see [Block Storage API](/Storage/Block Storage/ko/public-api/#_22). 

Images can be created only when the instance is **ACTIVE**, **SHUTOFF**, **SUSPENDED**, or **PAUSED**. It is recommended to close instances before creating images so as to ensure data integrity. 

When an image is successfully created, the image status is turned to `active`. To check if an image is fully created, use Query Image API to find status steadily.  

```
POST /v2/{tenantId}/servers/{serverId}/action
X-Auth-Token: {tokenId}
```

#### Request
| Name | Type | Format | Required | Description |
|---|---|---|---|--|
| tenantId | URL | String | O | Tenant ID |
| serverId | URL | UUID | O | Instance ID to change |
| tokenId | Header | String | O | Token ID |
| createImage | Body | Object | O | Request of creating image |
| createImage.name | Body | String | O | Name of image to create |
| createImage.metadata | Body | Object | - | Metadata of image to create<br>Described in the Key-Value format |

<details><summary>Example</summary>
<p>


```json
{
  "createImage" : {
      "name" : "foo-image",
      "metadata": {
          "meta_var": "meta_val"
      }
  }
}
```

</p>
</details>


#### Response

This API does not return a response body. Check created image from `Location` of the response header.  

| Name | Type | Format | Description |
|--|--|--|--|
| Location | Header | String | URL of created image |

---

### Add Security Group

Add a security group to instance. The added group shall be applied to all ports of an instance. 

```
POST /v2/{tenantId}/servers/{serverId}/action
X-Auth-Token: {tokenId}
```

#### Request
| Name | Type | Format | Required | Description |
|---|---|---|---|--|
| tenantId | URL | String | O | Tenant ID |
| serverId | URL | UUID | O | Instance ID to change |
| tokenId | Header | String | O | Token ID |
| addSecurityGroup | Body | Object | O | Object requesting of adding security groups |
| addSecurityGroup.name | Body | String | O | Name of security group to be added |

<details><summary>Example</summary>
<p>


```json
{
    "addSecurityGroup": {
        "name": "test"
    }
}
```

</p>
</details>


#### Response
This API does not return a response body. 

---

### Delete Security Group

Delete security group from instance. Delete specified security group from all ports. 

```
POST /v2/{tenantId}/servers/{serverId}/action
X-Auth-Token: {tokenId}
```

#### Request
| Name | Type | Format | Required | Description |
|---|---|---|---|--|
| tenantId | URL | String | O | Tenant ID |
| serverId | URL | UUID | O | Instance ID to change |
| tokenId | Header | String | O | Token ID |
| removeSecurityGroup | Body | Object | O | Object requesting of deleting security group |
| removeSecurityGroup.name | Body | String | O | Name of security group to delete |

<details><summary>Example</summary>
<p>


```json
{
    "removeSecurityGroup": {
        "name": "test"
    }
}
```

</p>
</details>


#### Response
This API does not return a response body. 
