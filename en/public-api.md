## Compute > Instance > API v2 Guide

To use the APIs listed in this document, you will need the appropriate API endpoint and token. See [API v2 Preparations](/Compute/Compute/en/identity-api/) to prepare the necessary information for using APIs.

The Instance API uses the `compute` type endpoint. For the exact endpoint, see `serviceCatalog` from the token issue response.

| Type | Region | Endpoint |
|---|---|---|
| compute | Korea (Pangyo) Region<br>Korea (Pyeongchon) Region<br>Japan Region<br>US (California) Region | https://kr1-api-instance-infrastructure.nhncloudservice.com<br>https://kr2-api-instance-infrastructure.nhncloudservice.com<br>https://jp1-api-instance-infrastructure.nhncloudservice.com<br>https://us1-api-instance-infrastructure.nhncloudservice.com |

In each API response, you may find fields that are not specified within this guide. Those fields are for NHN Cloud internal usage, and as such refrain from using them since they may be changed without prior notice.

## Instance Flavors

### List Flavors

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
| minDisk | Query | Integer | - | Minimum block storage size (GB)<br>Returns only flavors with block storage sizes greater than specified value |
| minRam | Query | Integer | - | Minimum RAM Size (MB)<br>Returns only flavors with RAM sizes greater than specified value |

#### Response

| Name | Type | Format | Description |
|---|---|---|---|
| flavors | Body | Object | Instance flavor list object |
| flavors.id | Body | UUID | Instance flavor ID |
| flavors.links | Body | Object | Instance flavor path object |
| flavors.name | Body | String | Instance flavor name |


<details><summary>Example</summary>
<p>

```json
{
  "flavors": [
    {
      "id": "013bea75-8541-4c6f-9abe-a03fee3d74fe",
      "links": [
        {
          "href": "https://kr1-api-instance-infrastructure.nhncloudservice.com/v2/6cdebe3eb0094910bc41f1d42ebe4cb7/flavors/013bea75-8541-4c6f-9abe-a03fee3d74fe",
          "rel": "self"
        },
        {
          "href": "https://kr1-api-instance-infrastructure.nhncloudservice.com/6cdebe3eb0094910bc41f1d42ebe4cb7/flavors/013bea75-8541-4c6f-9abe-a03fee3d74fe",
          "rel": "bookmark"
        }
      ],
      "name": "x1.c32m256"
    },
    {
      "id": "0f19a344-bc66-4228-8cb1-fb9ca82c54f5",
      "links": [
        {
          "href": "https://kr1-api-instance-infrastructure.nhncloudservice.com/v2/6cdebe3eb0094910bc41f1d42ebe4cb7/flavors/0f19a344-bc66-4228-8cb1-fb9ca82c54f5",
          "rel": "self"
        },
        {
          "href": "https://kr1-api-instance-infrastructure.nhncloudservice.com/6cdebe3eb0094910bc41f1d42ebe4cb7/flavors/0f19a344-bc66-4228-8cb1-fb9ca82c54f5",
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

### List Flavors with Details

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
| minDisk | Query | Integer | - | Minimum block storage size (GB)<br/>Returns only flavors with block storage sizes greater than specified value |
| minRam | Query | Integer | - | Minimum RAM Size (MB)<br/>Returns only flavors with RAM sizes greater than specified value |

#### Response

| Name | Type | Format | Description             |
|---|---|---|----------------|
| flavors | Body | Object | Instance flavor list object  |
| flavors.id | Body | UUID | Instance flavor ID     |
| flavors.links | Body | Object | Instance flavor path object  |
| flavors.name | Body | String | Instance flavor name     |
| flavors.ram | Body | Integer | Memory size (MB)     |
| flavors.OS-FLV-DISABLED:disabled | Body | Boolean | Indicates whether the flavor is enabled         |
| flavors.vcpus | Body | Integer | Number of vCPUs        |
| flavors.extra_specs | Body | Object | Extra specifications object       |
| flavors.swap | Body | Integer | Swap space size (GB)  |
| flavors.os-flavor-access:is_public | Body | Boolean | Indicates whether the flavor is publicly visible          |
| flavors.rxtx_factor | Body | Float | Network transmission packet rate |
| flavors.OS-FLV-EXT-DATA:ephemeral | Body | Integer | Temporary block storage size (GB)     |
| flavors.disk | Body | Integer | Root block storage size (GB) |

<details><summary>Example</summary>
<p>

```json
{
  "flavors": [
    {
      "name": "x1.c32m256",
      "links": [
        {
          "href": "https://kr1-api-instance-infrastructure.nhncloudservice.com/v2/6cdebe3eb0094910bc41f1d42ebe4cb7/flavors/97604802-a090-43fa-a5ce-c7cfd737fbba",
          "rel": "self"
        },
        {
          "href": "https://kr1-api-instance-infrastructure.nhncloudservice.com/6cdebe3eb0094910bc41f1d42ebe4cb7/flavors/97604802-a090-43fa-a5ce-c7cfd737fbba",
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
          "href": "https://kr1-api-instance-infrastructure.nhncloudservice.com/v2/6cdebe3eb0094910bc41f1d42ebe4cb7/flavors/31fa632d-aeec-4f12-8a57-ce9d146228e5",
          "rel": "self"
        },
        {
          "href": "https://kr1-api-instance-infrastructure.nhncloudservice.com/6cdebe3eb0094910bc41f1d42ebe4cb7/flavors/31fa632d-aeec-4f12-8a57-ce9d146228e5",
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

## Availability Zones

### List Availability Zones

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
| availabilityZoneInfo | Body | Object | Availability zone info object |
| availabilityZoneInfo.zoneName | Body | String | Availability zone name |
| availabilityZoneInfo.zoneState | Body | Object | Availability zone state info object |
| availabilityZoneInfo.available | Body | Object | Availability zone state |

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

## Key Pairs

### List Key Pairs
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
| keypairs | Body | Array | List of key pair objects |
| keypairs.keypair | Body | Object | Key pair object |
| keypairs.keypair.name | Body | String | Key pair name |
| keypairs.keypair.public_key | Body | String | Pubic key |
| keypairs.keypair.fingerprint | Body | String | Key pair fingerprint |

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

### Show Key Pair
```
GET /v2/{tenantId}/os-keypairs/{keypairName}
X-Auth-Token: {tokenId}
```

#### Request
This API does not require a request body.

| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| tenantId | URL | String | O | Tenant ID |
| keypairName | URL | String | O | Key pair name |
| tokenId | Header | String | O | Token ID |

#### Response

| Name | Type | Format | Description |
|---|---|---|---|
| keypair | Body | Object | List of key pair objects |
| keypair.public_key | Body | String | Pulbic key |
| keypair.user_id | Body | String | Key pair owner ID |
| keypair.name | Body | String | Key pair name |
| keypair.deleted | Body | Boolean | Indicates whether the key pair has been deleted |
| keypair.created_at | Body | Datetime | Key pair created time<br>`YYYY-MM-DDThh:mm:ss.SSSSSS` |
| keypair.updated_at | Body | Datetime | Key pair updated time<br>`YYYY-MM-DDThh:mm:ss.SSSSSS` |
| keypair.deleted_at | Body | Datetime | Key pair deleted time<br>`YYYY-MM-DDThh:mm:ss.SSSSSS` |
| keypair.fingerprint | Body | String | Key pair fingerprint |
| keypair.id | Body | Integer | Key pair ID |

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

### Create/Register Key Pair

```
POST /v2/{tenantId}/os-keypairs
X-Auth-Token: {tokenId}
```

#### Request

| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| tenantId | URL | String | O | Tenant ID |
| tokenId | Header | String | O | Token ID |
| keypair | Body | Object | O | Key pair object |
| keypair.name | Body | String | O | Key pair name to create or register |
| keypair.public_key | Body | String | - | Public key to register. If left blank, a new key pair is created. |

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
| keypair | Body | Object | Key pair object |
| keypair.public_key | Body | String | Public key |
| keypair.private_key | Body | String | Private key. Visible if a key pair has been newly generated. |
| keypair.user_id | Body | String | Key pair owner ID |
| keypair.name | Body | String | Key pair name |
| keypair.fingerprint | Body | String | Key pair fingerprint |

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

### Delete Key Pair
```
DELETE /v2/{tenantId}/os-keypairs/{keypairName}
X-Auth-Token: {tokenId}
```

#### Request
This API does not require a request body.

| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| tenantId | URL | String | O | Tenant ID |
| keypairName | URL | String | O | Key pair name |
| tokenId | Header | String | O | Token ID |

#### Response
This API does not return a response body.


## Instance

### Instance Status

Instances exist in various statuses, and each status defines its own set of permissible operations. See the following list of instance statuses.

| Status Name              | Description                                                                                                |
|-------------------|---------------------------------------------------------------------------------------------------|
| `ACTIVE` | Instance is activated |
| `BUILD` | Instance is building |
| `DELETED` | Instance is deleted |
| `ERROR` | Previous operation on the instance has failed |
| `HARD_REBOOT` | Instance is forcefully rebooted<br> Same as turning the physical server's power switch off and back on again |
| `MIGRATING` | Instance is migrating<br> This is caused by a real-time migration (moving active instances) |
| `PASSWORD` | Password is being reset on instance |
| `PAUSED` | Instance is paused<br>Paused instances are saved in hypervisor memory |
| `REBOOT` | Instance is in a soft reboot state<br> Reboot command is passed to the virtual machine operating system |
| `REBUILD` | Instance is rebuilt from the original image used for creation |
| `RESCUE` | Instance is running in recovery mode |
| `RESIZE` | Instance is changing flavors or migrating to another host<br>Instance is stopped and restarted |
| `REVERT_RESIZE` | Instance is restored to its original state when a failure occurs while changing flavors or migrating to another host |
| `VERIFY_RESIZE` | Instance is waiting for confirmation after changing flavors or migrating to another host<br>In NHN Cloud, the status is automatically changed to `ACTIVE`. |
| `SHELVED_OFFLOADED` | Instance is terminated |
| `SHUTOFF` | Instance is stopped |
| `SUSPENDED` | Instance has entered maximum power saving mode by the administrator |
| `UNKNOWN` | Instance status is unknown<br>`Contact the administrator if the instance is in this status.` | 

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
| reservation_id | Query | String | - | Reservation ID for instance creation. <br>If specified, only returns list of instances that have been created simultaneously |
| changes-since | Query | Datetime | - | Returns list of instances changed since the specified time. `YYYY-MM-DDThh:mm:ss` format. |
| image | Query | UUID | - | Image ID<br>Return list of instances with specified image |
| flavor | Query | UUID | - | Instance flavor ID<br>Return list of instances with specified flavor |
| name | Query | String | - | Instance name<br>Return list of instances with specified name, regex is supported |
| status | Query | Enum | - | Instance status<br>Return list of instances with specified status |
| limit | Query | Integer | - | Number of instances to query<br>Return list with up to specified number of instances |
| marker | Query | UUID | - | UUID of first instance in the list <br>Return list of up to `limit` instances from the instance specified as the `marker`, according to the sort order |

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
          "href": "https://kr1-api-instance-infrastructure.nhncloudservice.com/v2/6cdebe3eb0094910bc41f1d42ebe4cb7/servers/aaf2778b-ea03-4ccc-8b1b-92f4b686c3ec",
          "rel": "self"
        },
        {
          "href": "https://kr1-api-instance-infrastructure.nhncloudservice.com/6cdebe3eb0094910bc41f1d42ebe4cb7/servers/aaf2778b-ea03-4ccc-8b1b-92f4b686c3ec",
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

### List Instances with Details

Return the list of instances created in the current tenant, same as List Instances. However, detailed instance information is returned.

```
GET /v2/{tenantId}/servers/detail
X-Auth-Token: {tokenId}
```

#### Request

The request format is the same as List Instances.

#### Response

| Name | Type | Format | Description                                                                                                                                                                                                        |
|---|---|---|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| servers | body | Object | Instance list object                                                                                                                                                                                                |
| status | body | Enum | Instance Status                                                                                                                                                                                                   |
| servers.id | Body | UUID | Instance ID                                                                                                                                                                                                   |
| servers.name | Body | String | Instance name, max 255 characters                                                                                                                                                                                          |
| servers.updated | Body | Datetime | Last updated time of instance in `YYYY-MM-DDThh:mm:ssZ` format                                                                                                                                                                  |
| servers.hostId | Body | String | ID of host running instance                                                                                                                                                                                        |
| servers.addresses | Body | Object | Instance IP list object. <br>The size of the list is the number of ports attached to the instance.                                                                                                                                                             |
| servers.addresses."Network Name" | Body | Object | Port information of each network associated with instance                                                                                                                                                                                  |
| servers.addresses."Network Name".OS-EXT-IPS-MAC:mac_addr | Body | String | MAC address of port associated with instance                                                                                                                                                                                      |
| servers.addresses."Network Name".version | Body | Integer | IP version of port associated with instance<br>NHN Cloud supports only IPv4                                                                                                                                                                |
| servers.addresses."Network Name".addr | Body | String | IP address of port associated with instance                                                                                                                                                                                       |
| servers.addresses."Network Name".OS-EXT-IPS:type | Body | Enum | IP address type of port<br>Either `fixed` or `floating`                                                                                                                                                                |
| servers.links | Body | Object | Instance path object                                                                                                                                                                                                |
| servers.key_name | Body | String | Instance key pair name                                                                                                                                                                                               |
| servers.image | Body | Object | Instance image object                                                                                                                                                                                               |
| servers.image.id | Body | UUID | Instance image ID                                                                                                                                                                                               |
| servers.image.links | Body | Object | Instance image path object                                                                                                                                                                                            |
| servers.OS-EXT-STS:task_state | Body | String | Instance task status<br>Shows the status of a task operating on an instance                                                                                                                                                               |
| servers.OS-EXT-STS:vm_state | Body | String | Current instance status                                                                                                                                                                                                |
| servers.OS-SRV-USG:launched_at | Body | Datetime | Last instance booted time<br>`YYYY-MM-DDThh:mm:ss.ssssss` format                                                                                                                                                         |
| servers.OS-SRV-USG:terminated_at | Body | Datetime | Instance deleted time<br>`YYYY-MM-DDThh:mm:ssZ` format                                                                                                                                                                   |
| servers.flavor | Body | Object | Instance flavor information object                                                                                                                                                                                             |
| servers.flavor.id | Body | UUID | Instance flavor ID                                                                                                                                                                                                |
| servers.flavor.links | Body | Object | Instance flavor path object                                                                                                                                                                                             |
| servers.security_groups | Body | Object | List object of security groups assigned to instance                                                                                                                                                                                     |
| servers.security_groups.name | Body | String | Name of security group assigned to instance                                                                                                                                                                                        |
| servers.user_id | Body | String | ID of user creating instance                                                                                                                                                                                          |
| servers.created | Body | Datetime | Instance created time. `YYYY-MM-DDThh:mm:ssZ` format                                                                                                                                                                     |
| servers.tenant_id | Body | String | Tenant ID that instance belongs to                                                                                                                                                                                           |
| servers.os-extended-volumes:volumes_attached | Body | Object | List object of additional block storage attached to the instance                                                                                                                                                                                |
| servers.os-extended-volumes:volumes_attached.id | Body | UUID | ID of additional block storage attached to the instance                                                                                                                                                                                   |
| servers.OS-EXT-STS:power_state | Body | Integer | Power state of instance<br>- `1`: On<br>- `4`: Off                                                                                                                                                                    |
| servers.metadata | Body | Object | Instance metadata object<br>Stores instance metadata as key-value pairs                                                                                                                                                                   |
| server.NHN-EXT-ATTR:ephemeral_disk_size | Body | Integer | Size of an additional local block storage attached to the instance                                                                                                                                                                   |
| server.NHN-EXT-ATTR:protect | Body | Boolean | Whether to protect instance deletion                                                                                                                                                                   |

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
          "href": "https://kr1-api-instance-infrastructure.nhncloudservice.com/v2/6cdebe3eb0094910bc41f1d42ebe4cb7/servers/aaf2778b-ea03-4ccc-8b1b-92f4b686c3ec",
          "rel": "self"
        },
        {
          "href": "https://kr1-api-instance-infrastructure.nhncloudservice.com/6cdebe3eb0094910bc41f1d42ebe4cb7/servers/aaf2778b-ea03-4ccc-8b1b-92f4b686c3ec",
          "rel": "bookmark"
        }
      ],
      "key_name": "access-key",
      "image": {
        "id": "8b9f8d47-b89b-45af-b1d6-3f7ce7e06a11",
        "links": [
          {
            "href": "https://kr1-api-instance-infrastructure.nhncloudservice.com/6cdebe3eb0094910bc41f1d42ebe4cb7/images/8b9f8d47-b89b-45af-b1d6-3f7ce7e06a11",
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
            "href": "https://kr1-api-instance-infrastructure.nhncloudservice.com/6cdebe3eb0094910bc41f1d42ebe4cb7/flavors/35a73b57-58a7-434d-aa08-5249aaa95b3e",
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
      },
      "NHN-EXT-ATTR:ephemeral_disk_size": 0,
      "NHN-EXT-ATTR:protect": false
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

| Name | Type | Format | Description                                                                                                                                                                                                       |
|---|---|---|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| server | body | Object | Instance object                                                                                                                                                                                                  |
| status | body | Enum | Instance Status                                                                                                                                                                                                  |
| server.id | Body | UUID | Instance ID                                                                                                                                                                                                  |
| server.name | Body | String | Instance name, max 255 characters                                                                                                                                                                                         |
| server.updated | Body | Datetime | Last updated time of instance in `YYYY-MM-DDThh:mm:ssZ` format                                                                                                                                                                 |
| server.hostId | Body | String | ID of host running instance                                                                                                                                                                                       |
| server.addresses | Body | Object | Instance IP list object. <br>The size of the list is the number of ports attached to the instance.                                                                                                                                                              |
| server.addresses."Network Name" | Body | Object | Port information of each network associated with instance                                                                                                                                                                                 |
| server.addresses."Network Name".OS-EXT-IPS-MAC:mac_addr | Body | String | MAC address of port associated with instance                                                                                                                                                                                     |
| server.addresses."Network Name".version | Body | Integer | IP version of port associated with instance<br>NHN Cloud supports only IPv4                                                                                                                                                               |
| server.addresses."Network Name".addr | Body | String | IP address of port associated with instance                                                                                                                                                                                      |
| server.addresses."Network Name".OS-EXT-IPS:type | Body | Enum | IP address type of port<br>Either `fixed` or `floating`                                                                                                                                                               |
| server.links | Body | Object | Instance path object                                                                                                                                                                                               |
| server.key_name | Body | String | Instance key pair name                                                                                                                                                                                              |
| server.image | Body | Object | Instance image object                                                                                                                                                                                              |
| server.image.id | Body | UUID | Instance image ID                                                                                                                                                                                              |
| server.image.links | Body | Object | Instance image path object                                                                                                                                                                                           |
| server.OS-EXT-STS:task_state | Body | String | Instance task status<br>Shows the status of a task operating on an instance                                                                                                                                                               |
| server.OS-EXT-STS:vm_state | Body | String | Current instance status                                                                                                                                                                                               |
| server.OS-SRV-USG:launched_at | Body | Datetime | Last instance booted time<br>`YYYY-MM-DDThh:mm:ss.ssssss` format                                                                                                                                                        |
| server.OS-SRV-USG:terminated_at | Body | Datetime | Instance deleted time<br>`YYYY-MM-DDThh:mm:ssZ` format                                                                                                                                                                  |
| server.flavor | Body | Object | Instance flavor information object                                                                                                                                                                                            |
| server.flavor.id | Body | UUID | Instance flavor ID                                                                                                                                                                                               |
| server.flavor.links | Body | Object | Instance flavor path object                                                                                                                                                                                            |
| server.security_groups | Body | Object | List object of security groups assigned to instance                                                                                                                                                                                    |
| server.security_groups.name | Body | String | Name of security group assigned to instance                                                                                                                                                                                       |
| server.user_id | Body | String | ID of user creating instance                                                                                                                                                                                         |
| server.created | Body | Datetime | Instance created time. `YYYY-MM-DDThh:mm:ssZ` format                                                                                                                                                                    |
| server.tenant_id | Body | String | Tenant ID that instance belongs to                                                                                                                                                                                          |
| server.os-extended-volumes:volumes_attached | Body | Object | List object of additional block storage attached to the instance                                                                                                                                                                               |
| server.os-extended-volumes:volumes_attached.id | Body | UUID | ID of additional block storage attached to the instance                                                                                                                                                                                  |
| server.OS-EXT-STS:power_state | Body | Integer | Power state of instance<br>- `1`: On<br>- `4`: Off                                                                                                                                                                   |
| server.metadata | Body | Object | Instance metadata object<br>Stores instance metadata as key-value pairs                                                                                                                                                                  |
| server.NHN-EXT-ATTR:ephemeral_disk_size | Body | Integer | Size of an additional local block storage attached to the instance                                                                                                                                                                  |
| server.NHN-EXT-ATTR:protect | Body | Boolean | Whether to protect instance deletion                                                                                                                                                                  |

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
        "href": "https://kr1-api-instance-infrastructure.nhncloudservice.com/v2/6cdebe3eb0094910bc41f1d42ebe4cb7/servers/aaf2778b-ea03-4ccc-8b1b-92f4b686c3ec",
        "rel": "self"
      },
      {
        "href": "https://kr1-api-instance-infrastructure.nhncloudservice.com/6cdebe3eb0094910bc41f1d42ebe4cb7/servers/aaf2778b-ea03-4ccc-8b1b-92f4b686c3ec",
        "rel": "bookmark"
      }
    ],
    "key_name": "access-key",
    "image": {
      "id": "8b9f8d47-b89b-45af-b1d6-3f7ce7e06a11",
      "links": [
        {
          "href": "https://kr1-api-instance-infrastructure.nhncloudservice.com/6cdebe3eb0094910bc41f1d42ebe4cb7/images/8b9f8d47-b89b-45af-b1d6-3f7ce7e06a11",
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
          "href": "https://kr1-api-instance-infrastructure.nhncloudservice.com/6cdebe3eb0094910bc41f1d42ebe4cb7/flavors/35a73b57-58a7-434d-aa08-5249aaa95b3e",
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
    },
    "NHN-EXT-ATTR:ephemeral_disk_size": 0,
    "NHN-EXT-ATTR:protect": false
  }
}
```

</p>
</details>

---

### Create Instance

Create an instance.

After calling the Create Instance API, query the instance and check its status.

* If the status becomes **ACTIVE**, the instance has been created successfully.
* If the status remains in **BUILDING** for a long time or becomes **ERROR**, check parameters used for instance creation and try creating again.

Windows instances have the following additional restrictions that apply to facilitate stable usage.

* Use an instance flavor with at least 2GB RAM capacity.
* Require 50 GB or more of root block storage.
* U2 flavor instances cannot use Windows images.

The root block storage size that can be specified is 10GB for Linux and 50GB for Windows.

You can assign placement policies via scheduler hints when requesting instance creation.


```
POST /v2/{tenantId}/servers
X-Auth-Token: {tokenId}
```

#### Request

| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| tenantId | URL | String | O | Tenant ID |
| tokenId | Header | String | O | Token ID |
| server.security_groups | body | Object | - | List object of security groups<br>If left blank, the `default` group is added. |
| server | body | Object | O | Server Objects |
| server.security_groups.name | body | String | - | **(Conditionally required)** Name of security group to be added to instance |
| server.user_data | body | String | - | Script to be executed or settings to apply after instance boot<br>Allows up to 65535 bytes of base 64-encoded character strings |
| server.availability_zone | body | String | - | Availability zone where instance will be created<br>If left blank, a random zone will be selected<br>If the source type of the root block storage is `volume`, `snapshot`, it must be set to the same availability zone as the source block storage |
| server.imageRef | Body | String | - | Image ID to create instance<br>No configuration required if the source type of the root block storage is `volume`, `snapshot` |
| server.flavorRef | Body | String | O | Instance flavor ID to create instance |
| server.networks | Body | Object | O | Network information object to use when creating instance<br>A NIC is added for each network specified. Specify each network using Network ID, Subnet ID, Port ID, or Fixed IP. |
| server.networks.uuid | Body | UUID | - | **(Conditionally required)** Network ID to create instance |
| server.networks.subnet | Body | UUID | - | **(Conditionally required)** Subnet ID within network to create instance |
| server.networks.port | Body | UUID | - | **(Conditionally required)** Port ID to create instance<br>Security groups requested when specifying a port ID are not applied to existing specified ports |
| server.networks.fixed_ip | Body | String | - | **(Conditionally required)** Fixed IP to create instance |
| server.name | Body | String | O | Instance name<br>Up to 255 alphabetical characters allowed, max 15 characters for Windows images |
| server.metadata | Body | Object | - | Metadata object to add to instance<br>Key-value pairs of max 255 characters |
| server.block_device_mapping_v2 | Body | Object | O | Block storage information object<br>**Must be specified for any instance flavors other than U2 flavor which uses local block storage** |
| server.block_device_mapping_v2.source_type | Body | Enum | O | Source type of block storage to create<br>- `image`: Use an image to create a block storage<br>- `blank`: Create an empty block storage (cannot be used as root block storage)<br>- `volume`: Use previously created block storage<br>- `snapshot`: Create block storage with snapshots |
| server.block_device_mapping_v2.uuid | Body | String | - | **(Conditionally required)** Different settings required for different types of block storage sources<br>- Set the image ID if the source type is `image`<br>- Set an existing created block storage ID if the source type is `volume`<br>- Set `snapshot` ID if the source type is `snapshot`<br>- No settings required if source type is ` blank`<br>For root block storage, it must be a bootable source. |
| server.block_device_mapping_v2.boot_index | Body | Integer | O | Order to boot the specified block storage<br>- If `, root block storage<br>- If not, additional block storage<br>A larger value indicates lower booting priority |
| server.block_device_mapping_v2.destination_type | Body | Enum | O | Requires different settings depending on the location of instance’s block storage or flavor<br>- `local`: For GPU and U2 instance flavors<br>- `volume`: For other instance flavors |
| server.block_device_mapping_v2.volume_type | Body | Enum    | - | **(Conditionally required)** Type of block storage to create<br>No configuration required if the source type of block storage is `volume`, `snapshot`<br>See `Name` from the response of **List Block Storage Types** in the `User Guide > Storage > Block Storage > API v2 guide`. |
| server.block_device_mapping_v2.delete_on_termination | Body | Boolean | - | Indicates whether block storage is deleted when an instance is terminated. Default value is `false`.<br>Delete the volume if `true`, keep the volume if `false`. |
| server.block_device_mapping_v2.volume_size | Body | Integer | - | **(Conditionally required)** Size of block storage to create<br>Different settings required for different types of block storage sources<br>- No configuration required if source type is `volume`<br>- Set equal to or larger than the original block storage size if the source type is `snapshot`<br>GB (unit)<br>Uses the U2 instance type and root block storage is created with the size specified in the U2 instance type and this value will be ignored<br>Different instance types have different sizes of root block storage that can be created. For more details, see `User Guide > Compute > Instance > Console User Guide > Create Instance > Block Storage Size`. |
| server.block_device_mapping_v2.nhn_encryption                   | Body | Object | - | **(Conditionally required)** Block storage encryption information                                                                                                                                                                                        |
| server.block_device_mapping_v2.nhn_encryption.skm_appkey        | Body | String | - | **(Conditionally required)** AppKeys for Secure Key Manager products                                                                                                                                                                              |
| server.block_device_mapping_v2.nhn_encryption.skm_key_id        | Body | String | - | **(Conditionally required)** Symmetric key ID of Secure Key Manager to be used to create encrypted block storage.                                            |               
| server.key_name | Body | String | O | Key pair to access instance |
| server.min_count | Body | Integer | - | Minimum number of instances to create with this request.<br>Default value is 1.<br>Can only be set to `1` if the source type of the block storage is `volume` |
| server.max_count | Body | Integer | - | Maximum number of instances to create with this request.<br>Default value is min_count, max value is 10.<br>Can only be set to `1` if the source type of the block storage is `volume` |
| server.return_reservation_id | Body | Boolean | - | Instance creation request reservation ID.<br>If set to True, reservation ID is returned instead of instance creation information.<br>Default value is False |
| os:scheduler_hints | Body | Object | - | Scheduler Hint Objects |
| os:scheduler_hints.group | Body | String | - | Placement Policy ID |

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
      "source_type": "image",
      "uuid": "9956f822-29c9-4f81-9410-0c392d9c8c24",
      "boot_index": 0,
      "volume_size": 1000,
      "destination_type": "volume",
      "delete_on_termination": 1
    }],
    "security_groups": [{
      "name": "default"
    }]
  },
  "os:scheduler_hints": {
    "group": "f878bd5b-49a7-499f-966e-1eceb21cb06b"
  }
}
```

</p>
</details>

#### Response

| Name | Type | Format | Description                                                                                                                                                                                                           |
|---|---|---|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| server.security_groups.name | Body | String | Security group name of created instance                                                                                                                                                                                           |
| server.id | Body | UUID | Created instance ID                                                                                                                                                                                                 |

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
    "id": "3a005d5b-63cf-4493-bfc6-49db990b5b50",
    "links": [
      {
        "href": "https://kr1-api-instance-infrastructure.nhncloudservice.com/v2/6cdebe3eb0094910bc41f1d42ebe4cb7/servers/3a005d5b-63cf-4493-bfc6-49db990b5b50",
        "rel": "self"
      },
      {
        "href": "https://kr1-api-instance-infrastructure.nhncloudservice.com/6cdebe3eb0094910bc41f1d42ebe4cb7/servers/3a005d5b-63cf-4493-bfc6-49db990b5b50",
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
Modify created instance. Only some attributes are allowed to be modified.

```
PUT /v2/{tenantId}/servers/{serverId}
X-Auth-Token: {tokenId}
```

#### Request

| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| tenantId | URL | String | O | Tenant ID |
| serverId | URL | UUID | O | Modifying instance ID |
| tokenId | Header | String | O | Token ID |
| server | Body | Object | O | Modify instance request object |
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
| serverId | URL | UUID | O | Deleting instance ID |
| tokenId | Header | String | O | Token ID |

#### Response
This API does not return a response body.

---

## Manage Block Storage Attachment
### List additional block storage attached to the instance
```
GET /v2/{tenantId}/servers/{serverId}/os-volume_attachments
X-Auth-Token: {tokenId}
```

#### Request
This API does not require a request body.

| Name | Type | Format | Required | Description |
|---|---|---|---|--|
| tenantId | URL | String | O | Tenant ID |
| serverId | URL | UUID | O | Modifying instance ID |
| tokenId | Header | String | O | Token ID |
| limit | Query | Integer | - | Number of volumes to query |
| offset | Query | Integer | - | Start point of returned list<br>Return block storage starting from offset of the entire list |

#### Response

| Name | Type | Format | Description |
|---|---|---|---|
| volumeAttachments | Body | Array | List of attachment information objects |
| volumeAttachments.device | Body | String | Block storage name<br>e.g.) `/dev/vdb` |
| volumeAttachments.id | Body | UUID | Attachment information ID |
| volumeAttachments.serverId | Body | UUID | Instance ID |
| volumeAttachments.volumeId | Body | UUID | Block storage ID |

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

### List additional block storage attached to the instance
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
| volumeId | URL | UUID | O | ID of block storage to query |
| tokenId | Header | String | O | Token ID |

#### Response

| Name | Type | Format | Description |
|---|---|---|---|
| volumeAttachment | Body | Object | Attachment information object |
| volumeAttachment.device | Body | String | Block storage name<br>e.g.) `/dev/vdb` |
| volumeAttachment.id | Body | UUID | Attachment information ID |
| volumeAttachment.serverId | Body | UUID | Instance ID |
| volumeAttachment.volumeId | Body | UUID | Block storage ID |

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

### Attach additional block storage to the instance
```
POST /v2/{tenantId}/servers/{serverId}/os-volume_attachments
X-Auth-Token: {tokenId}
```

#### Request

| Name | Type | Format | Required | Description |
|---|---|---|---|--|
| tenantId | URL | String | O | Tenant ID |
| serverId | URL | UUID | O | Modifying instance ID |
| tokenId | Header | String | O | Token ID |
| volumeAttachment | Body | Object | O | Object to request block storage attachment |
| volumeAttachment.volumeId | Body | UUID | O | ID of block storage to attach |

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
| volumeAttachment.device | Body | String | Block storage name<br>e.g.) `/dev/vdb` |
| volumeAttachment.id | Body | UUID | Attachment information ID |
| volumeAttachment.serverId | Body | UUID | Instance ID |
| volumeAttachment.volumeId | Body | UUID | Block storage ID |

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

### Detach block storage from the instance
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
| volumeId | URL | UUID | O | ID of block storage to detach |
| tokenId | Header | String | O | Token ID |

#### Response
This API does not return a response body.

---

## Additional Instance Features
NHN Cloud provides the following additional features to handle instances.

* Start, Stop, Terminate, and Restart Instance
* Change Instance Flavor
* Create Instance Image
* Add/Delete Security Group

### Start Stopped Instance

Restart a stopped instance and change its status to **ACTIVE**. To call this API, the instance status must be **SHUTOFF**.

```
POST /v2/{tenantId}/servers/{serverId}/action
X-Auth-Token: {tokenId}
```

#### Request
| Name | Type | Format | Required | Description |
|---|---|---|---|--|
| tenantId | URL | String | O | Tenant ID |
| serverId | URL | UUID | O | Modifying instance ID |
| tokenId | Header | String | O | Token ID |
| os-start | Body | none | O | Instance start request |

<details><summary>Example</summary>
<p>

```json
{
  "os-start" : null
}
```

</p>
</details>

#### Response
This API does not return a response body.

---

### Start Terminated Instance

Restart a terminated instance and changes its status to **ACTIVE**. To call this API, the instance's state must be **SHELVED_OFFLOADED**.

```
POST /v2/{tenantId}/servers/{serverId}/action
X-Auth-Token: {tokenId}
```

#### Request
| Name | Type | Format | Required | Description |
|--|---|---|---|--|
| tenantId | URL | String | O | Tenant ID |
| serverId | URL | UUID | O | Modifying instance ID |
| tokenId | Header | String | O | Token ID |
| unshelve | Body | none | O | Instance start request |

<details><summary>Example</summary>
<p>

```json
{
  "unshelve" : null
}
```

</p>
</details>

#### Response
This API does not return a response body.

---

### Stop Instance

Stop instance and change its status to **SHUTOFF**. To call this API, the instance status must be either **ACTIVE** or **ERROR**.

```
POST /v2/{tenantId}/servers/{serverId}/action
X-Auth-Token: {tokenId}
```

#### Request
| Name | Type | Format | Required | Description |
|---|---|---|---|--|
| tenantId | URL | String | O | Tenant ID |
| serverId | URL | UUID | O | Modifying instance ID |
| tokenId | Header | String | O | Token ID |
| os-stop | Body | none | O | Instance stop request |

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

## Terminate Instance

Terminate the instance and change its status to **SHELVED_OFFLOADED**. The instance's status must be **ACTIVE** to call this API.

```
POST /v2/{tenantId}/servers/{serverId}/action
X-Auth-Token: {tokenId}
```

#### Request
| Name | Type | Format | Required | Description          |
|---|---|---|---|-------------|
| tenantId | URL | String | O | Tenant ID      |
| serverId | URL | UUID | O | Modifying instance ID |
| tokenId | Header | String | O | Token ID       |
| shelve | Body | none | O | Request to terminate instance  |

<details><summary>Example</summary>
<p>

```json
{
  "shelve" : null
}
```

</p>
</details>

#### Response
This API does not return a response body.

---

### Restart Instance

Restart an instance. An instance can be restarted using either a **SOFT** restart or a **HARD** restart.

* **SOFT**: An instance is stopped via **"Graceful Shutdown"** and restarted. The instance must be in **ACTIVE** status.
* **HARD**: Forcefully stop the instance and restart it. This works the same way as turning the power switch of a physical server off and on again. An instance can only be forcefully stopped when it is in one of these statuses.
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
| serverId | URL | UUID | O | Modifying instance ID |
| tokenId | Header | String | O | Token ID |
| reboot | Body | Object | O | Instance reboot request object |
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

### Change Instance Flavor

Change the flavor of an instance. Flavors can only be changed when an instance is **ACTIVE** or **SHUTOFF**. If an instance is **ACTIVE**, the instance is stopped and restarted while changing flavors.

Depending on the current image and flavor you are using, you may be restricted from changing to some flavors. For more details, see the Console Guide.


```
POST /v2/{tenantId}/servers/{serverId}/action
X-Auth-Token: {tokenId}
```

#### Request
| Name | Type | Format | Required | Description                                                                                                                                                                                                                 |
|---|---|---|---|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| tenantId | URL | String | O | Tenant ID                                                                                                                                                                                                             |
| serverId | URL | UUID | O | Modifying instance ID                                                                                                                                                                                                        |
| tokenId | Header | String | O | Token ID                                                                                                                                                                                                              |
| resize | Body | Object | O | Instance flavor change request                                                                                                                                                                                                      |
| resize.flavorRef | Body | UUID | O | New instance flavor ID                                                                                                                                                                                                     |

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

Create an image from an instance. Only `U2` flavor instances can create images via this API. To create images of non-`U2` flavor instances, see [Block Storage API\](/Storage/Block Storage/ko/public-api/#_22).

Images can only be created when an instance is **ACTIVE**, **SHUTOFF**, **SUSPENDED**, or **PAUSED**. It is recommended to stop instances before creating images to ensure data integrity.

When an image is successfully created, the image status becomes `active`. To check if an image is successfully created, use the Get Image API to continuously check its status.

> [Caution]
> The size of the created image may be larger than the actual usage of the root block storage.

```
POST /v2/{tenantId}/servers/{serverId}/action
X-Auth-Token: {tokenId}
```

#### Request
| Name | Type | Format | Required | Description |
|---|---|---|---|--|
| tenantId | URL | String | O | Tenant ID |
| serverId | URL | UUID | O | Modifying instance ID |
| tokenId | Header | String | O | Token ID |
| createImage | Body | Object | O | Image create request |
| createImage.name | Body | String | O | Name of image to create |
| createImage.metadata | Body | Object | - | Metadata of image to create<br>Written in Key-Value format |

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

This API does not return a response body. Check the `Location` response header for the created image.

| Name | Type | Format | Description |
|--|--|--|--|
| Location | Header | String | Created image URL |

---

### Add Security Group

Add a security group to an instance. The added security group is applied to all ports of the instance.

```
POST /v2/{tenantId}/servers/{serverId}/action
X-Auth-Token: {tokenId}
```

#### Request
| Name | Type | Format | Required | Description |
|---|---|---|---|--|
| tenantId | URL | String | O | Tenant ID |
| serverId | URL | UUID | O | Modifying instance ID |
| tokenId | Header | String | O | Token ID |
| addSecurityGroup | Body | Object | O | Add security group request object |
| addSecurityGroup.name | Body | String | O | Name of security group to add |

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

Delete a security group from an instance. The specified security group is deleted from all ports of the instance.

```
POST /v2/{tenantId}/servers/{serverId}/action
X-Auth-Token: {tokenId}
```

#### Request
| Name | Type | Format | Required | Description |
|---|---|---|---|--|
| tenantId | URL | String | O | Tenant ID |
| serverId | URL | UUID | O | Modifying instance ID |
| tokenId | Header | String | O | Token ID |
| removeSecurityGroup | Body | Object | O | Delete security group request object |
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


## Instance Metadata

The values of the instance metadata determine the content of the instance details screen on the **Compute > Instance** service page in the console. The contents by instance metadata are as follows

| Instance Metadata     | Content                                           |
|----------------|----------------------------------------------|
| os_distro      | Name of the **OS** in **Basic Information**<br>Used in combination with os_version |
| os_version     | Version of the **OS** in **Basic Information**<br>Used in combination with os_distro  |
| image_name     | **Image name** for **Basic Information**                        |
| os_type      | **Access Information** format                                 |
| login_username | Username in **Access Information**                            |

> [Caution] Changing and deleting instance metadata can affect related services and features, and you are responsible for the consequences.

### View a List of Instance Metadata

```
GET /v2/{tenantId}/servers/{serverId}/metadata
X-Auth-Token: {tokenId}
```

#### Request
This API does not require a request body.

| Name       | Type | Format | Required | Description                                               |
|----------|---|---|---|--------------------------------------------------|
| tenantId | URL | String | O | Tenant ID                                           |
| serverId | URL | UUID | O | Instance ID                                          |
| tokenId  | Header | String | O | Token ID                                            |

#### Response

| Name       | Type | Format | Description                                               |
|----------|---|---|--------------------------------------------------|
| metadata | Body | Object | Metadata objects to create or modify on the instance<br>Key-value pairs of max 255 characters |

<details><summary>Example</summary>
<p>

```json
{
    "metadata": {
        "os_distro": "ubuntu",
        "description": "Ubuntu Server 20.04.6 LTS (2023.11.21)",
        "volume_size": "20",
        "project_domain": "NORMAL",
        "monitoring_agent": "sysmon",
        "image_name": "Ubuntu Server 20.04.6 LTS (2023.11.21)",
        "os_version": "Server 20.04 LTS",
        "os_architecture": "amd64",
        "login_username": "ubuntu",
        "os_type": "linux",
        "tc_env": "sysmon,dfeac7db42a192a73959d5646117af58"
    }
}
```

</p>
</details>


### View Instance Metadata

```
GET /v2/{tenantId}/servers/{serverId}/metadata/{key}
X-Auth-Token: {tokenId}
```

#### Request
This API does not require a request body.

| Name       | Type | Format | Required | Description                       |
|----------|---|---|---|--------------------------|
| tenantId | URL | String | O | Tenant ID                   |
| serverId | URL | UUID | O | Instance ID                  |
| key      | URL | String | O | Key for metadata to create or modify on the instance |
| tokenId  | Header | String | O | Token ID                    |

#### Response

| Name   | Type | Format | Description                                               |
|------|---|---|--------------------------------------------------|
| meta | Body | Object | Metadata objects to create or modify on the instance<br>Key-value pairs of max 255 characters |

<details><summary>Example</summary>
<p>

```json
{
    "meta": {
        "os_version": "Server 20.04 LTS"
    }
}
```

</p>
</details>

### Create/Modify Instance Metadata

Create or modify metadata for the instance.
If the key you are requesting matches an existing key, change the key-value to the requested value.

```
PUT /v2/{tenantId}/servers/{serverId}/metadata/{key}
X-Auth-Token: {tokenId}
```

#### Request
| Name       | Type | Format | Required | Description                                               |
|----------|---|---|---|--------------------------------------------------|
| tenantId | URL | String | O | Tenant ID                                           |
| serverId | URL | UUID | O | Instance ID                                          |
| key      | URL | String | O | Key for metadata to create or modify on the instance                         |
| tokenId  | Header | String | O | Token ID                                            |
| meta     | Body | Object | O | Metadata objects to create or modify on the instance<br>Key-value pairs of max 255 characters |

<details>
<summary>Example</summary>
<p>

```json
{
    "meta": {
        "os_version": "Server 20.04 LTS"
    }
}
```

</p>
</details>


#### Response

| Name   | Type | Format | Description                                               |
|------|---|---|--------------------------------------------------|
| meta | Body | Object | Metadata objects to create or modify on the instance<br>Key-value pairs of max 255 characters |

<details><summary>Example</summary>
<p>

```json
{
    "meta": {
        "os_version": "Server 20.04 LTS"
    }
}
```

</p>
</details>


### Delete Instance Metadata

Delete metadata for instances that match the key you're requesting.

```
DELETE /v2/{tenantId}/servers/{serverId}/metadata/{key}
X-Auth-Token: {tokenId}
```

#### Request
This API does not require a request body.

| Name       | Type | Format | Required | Description                  |
|----------|---|---|---|---------------------|
| tenantId | URL | String | O | Tenant ID              |
| serverId | URL | UUID | O | Instance ID             |
| key      | URL | String | O | The key to the metadata you want to delete from the instance |
| tokenId  | Header | String | O | Token ID               |

#### Response
This API does not return a response body.


## Placement Policy

### Create a Placement policy

Creates a placement policy.
Provides only the `anti-affinity` placement policy type for distributed placement.

```
POST /v2/{tenantId}/os-server-groups
X-Auth-Token: {tokenId}
```

#### Request
| Name | Type | Format | Required | Description |
|-----|-----|-----|-----|-----|
| tenantId | URL | String | O | Tenant ID |
| tokenId | Header | String | O | Token ID |
| server_group | Body | Object | O | Placement policy object |
| server_group.name | Body | String | O | Placement policy name |
| server_group.policies | Body | Array | O | Placement policy type<br>`Only anti-affinity` can be set |

<details>
<summary>Example</summary>
<p>

```json
{
    "server_group": {
        "name": "policy-test1",
        "policies": [
            "anti-affinity"            
        ]
    }
}
```

</p>
</details>

#### Response

| Name | Type | Format | Description |
|-----|-----|-----|-----|
| server_group | Body | Object | Placement policy object |
| server_group.id | Body | String | Placement policy ID |
| server_group.name | Body | String | Placement policy name |
| server_group.policies | Body | Array | Placement policy type |
| server_group.members | Body | Array | List of instance IDs assigned to the placement policy |
| server_group.metadata | Body | Object | Placement policy metadata object<br>Always displayed as an empty value |

<details><summary>Example</summary>
<p>

```json
{
    "server_group": {
        "id": "11f5a850-9ecc-4895-af77-de6ea471b65a",
        "name": "policy-test1",
        "policies": [
            "anti-affinity"
        ],
        "members": [],
        "metadata": {}
    }
}
```

</p>
</details>

### View the list of Placement policies

```
GET /v2/{tenantId}/os-server-groups
X-Auth-Token: {tokenId}
```

#### Request

This API does not require a request body.

| Name | Type | Format | Required | Description |
|-----|-----|-----|-----|-----|
| tenantId | URL | String | O | Tenant ID |
| tokenId | Header | String | O | Token ID |

#### Response

| Name | Type | Format | Description |
|-----|-----|-----|-----|
| server_groups | Body | Array | List of placement policy objects |
| server_groups.id | Body | String | Placement policy ID |
| server_groups.name | Body | String | Placement policy name |
| server_groups.policies | Body | Array | Placement policy type |
| server_groups.members | Body | Array | List of instance IDs assigned to the placement policy |
| server_groups.metadata | Body | Object | Placement policy metadata object<br>Always displayed as an empty value |

<details><summary>Example</summary>
<p>

```json
{
    "server_groups": [
        {
            "id": "11f5a850-9ecc-4895-af77-de6ea471b65a",
            "name": "policy-test1",
            "policies": [
                "anti-affinity"
            ],
            "members": [
                "c040455d-6495-4628-ad81-ade79cf7b8d6",
                "524e7d81-f373-43a0-b2ff-0a15f8255bb5"            
            ],
            "metadata": {}
        },
        {
            "id": "f947c657-cbe0-4bf2-a2aa-59d198f8e096",
            "name": "policy-test2",
            "policies": [
                "anti-affinity"
            ],
            "members": [],
            "metadata": {}
        }
    ]
}
```

</p>
</details>

### View Placement Policies

```
GET /v2/{tenantId}/os-server-groups/{servergroupId}
X-Auth-Token: {tokenId}
```

#### Request

This API does not require a request body.

| Name | Type | Format | Required | Description |
|-----|-----|-----|-----|-----|
| tenantId | URL | String | O | Tenant ID |
| servergroupId | URL | String | O | Placement policy ID |
| tokenId | Header | String | O | Token ID |

#### Response

| Name | Type | Format | Description |
|-----|-----|-----|-----|
| server_group | Body | Object | Placement policy object |
| server_group.id | Body | String | Placement policy ID |
| server_group.name | Body | String | Placement policy name |
| server_group.policies | Body | Array | Placement policy type |
| server_group.members | Body | Array | List of instance IDs assigned to the placement policy |
| server_group.metadata | Body | Object | Placement policy metadata object<br>Always displayed as an empty value |

<details><summary>Example</summary>
<p>

```json
{
    "server_group": {
        "id": "11f5a850-9ecc-4895-af77-de6ea471b65a",
        "name": "policy-test1",
        "policies": [
            "anti-affinity"
        ],
        "members": [
            "c040455d-6495-4628-ad81-ade79cf7b8d6",
            "524e7d81-f373-43a0-b2ff-0a15f8255bb5"            
        ],
        "metadata": {}
    }
}
```

</p>
</details>

### Deleting a Placement policy

```
DELETE /v2/{tenantId}/os-server-groups/{servergroupId}
X-Auth-Token: {tokenId}
```

#### Request

This API does not require a request body.

| Name | Type | Format | Required | Description |
|-----|-----|-----|-----|-----|
| tenantId | URL | String | O | Tenant ID |
| servergroupId | URL | String | O | Placement policy ID |
| tokenId | Header | String | O | Token ID |

#### Response

This API does not return a response body.
