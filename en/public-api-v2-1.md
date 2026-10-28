<!-- machine_translated: true -->

<!-- pre-align:aligned sig=92796381842e -->

<a id="compute-instance-api-v2-1-guide"></a>

## Compute > Instance > API v2.1 Guide { #compute-instance-api-v2-1-guide }

Instance uses the IaaS token for authentication/authorization when making API calls. The IaaS token is the authentication token used by the NHN Cloud's OpenStack-based infrastructure service (IaaS). For more information on IaaS token issuance and usage, see [IaaS token](/nhncloud/en/public-api/iaas-token).

The Instance API uses the `compute` type endpoint. For the exact endpoint, see `serviceCatalog` from the token issue response.

| Type | Region | Endpoint |
|---|---|---|
| compute | Korea (Pangyo) region<br>Korea (Pyeongchon) region<br>Korea (Gwangju) region<br>Japan region | https://kr1-api-instance-infrastructure.nhncloudservice.com<br>https://kr2-api-instance-infrastructure.nhncloudservice.com<br>https://kr3-api-instance-infrastructure.nhncloudservice.com<br>https://jp1-api-instance-infrastructure.nhncloudservice.com |

In each API response, you may find fields that are not specified within this guide. Those fields are for NHN Cloud internal usage, and as such refrain from using them since they may be changed without prior notice.

<a id="instance-flavors"></a>

## Instance Flavors { #instance-flavors }

<a id="list-flavors"></a>
### List Flavors { #list-flavors }

```
GET /v2.1/flavors
X-Auth-Token: {tokenId}
```

<a id="list-flavors-request"></a>
#### Request

This API does not require a request body.

| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| tokenId | Header | String | O | Token ID |
| minDisk | Query | Integer | - | Minimum block storage size (GB)<br>Returns only flavors with a block storage size larger than the specified size |
| minRam | Query | Integer | - | Minimum RAM size (MB)<br>Returns only flavors with a RAM size larger than the specified size |

<a id="list-flavors-response"></a>
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
          "href": "https://kr1-api-instance-infrastructure.nhncloudservice.com/v2.1/flavors/013bea75-8541-4c6f-9abe-a03fee3d74fe",
          "rel": "self"
        },
        {
          "href": "https://kr1-api-instance-infrastructure.nhncloudservice.com/flavors/013bea75-8541-4c6f-9abe-a03fee3d74fe",
          "rel": "bookmark"
        }
      ],
      "name": "x1.c32m256"
    },
    {
      "id": "0f19a344-bc66-4228-8cb1-fb9ca82c54f5",
      "links": [
        {
          "href": "https://kr1-api-instance-infrastructure.nhncloudservice.com/v2.1/flavors/0f19a344-bc66-4228-8cb1-fb9ca82c54f5",
          "rel": "self"
        },
        {
          "href": "https://kr1-api-instance-infrastructure.nhncloudservice.com/flavors/0f19a344-bc66-4228-8cb1-fb9ca82c54f5",
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

<a id="list-flavors-with-details"></a>
### List Flavors with Details { #list-flavors-with-details }

```
GET /v2.1/flavors/detail
X-Auth-Token: {tokenId}
```

<a id="list-flavors-with-details-request"></a>
#### Request

This API does not require a request body.

| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| tokenId | Header | String | O | Token ID |
| minDisk | Query | Integer | - | Minimum block storage size (GB)<br>Returns only flavors with a block storage size larger than the specified size |
| minRam | Query | Integer | - | Minimum RAM size (MB)<br>Returns only flavors with a RAM size larger than the specified size |

<a id="list-flavors-with-details-response"></a>
#### Response

| Name | Type | Format | Description             |
|---|---|---|----------------|
| flavors | Body | Object | Instance flavor list object  |
| flavors.id | Body | UUID | Instance flavor ID     |
| flavors.links | Body | Object | Instance flavor path object  |
| flavors.name | Body | String | Instance flavor name     |
| flavors.ram | Body | Integer | Memory size (MB)     |
| flavors.OS-FLV-DISABLED:disabled | Body | Boolean | Whether activated         |
| flavors.vcpus | Body | Integer | Number of vCPUs        |
| flavors.extra_specs | Body | Object | Extra specifications object       |
| flavors.swap | Body | Integer | Swap space size (MB)  |
| flavors.os-flavor-access:is_public | Body | Boolean | Whether shared          |
| flavors.rxtx_factor | Body | Float | Network transmit/receive packet ratio |
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
          "href": "https://kr1-api-instance-infrastructure.nhncloudservice.com/v2.1/flavors/97604802-a090-43fa-a5ce-c7cfd737fbba",
          "rel": "self"
        },
        {
          "href": "https://kr1-api-instance-infrastructure.nhncloudservice.com/flavors/97604802-a090-43fa-a5ce-c7cfd737fbba",
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
          "href": "https://kr1-api-instance-infrastructure.nhncloudservice.com/v2.1/flavors/31fa632d-aeec-4f12-8a57-ce9d146228e5",
          "rel": "self"
        },
        {
          "href": "https://kr1-api-instance-infrastructure.nhncloudservice.com/flavors/31fa632d-aeec-4f12-8a57-ce9d146228e5",
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

<a id="availability-zones"></a>

## Availability Zones { #availability-zones }

<a id="list-availability-zones"></a>
### List Availability Zones { #list-availability-zones }

```
GET /v2.1/os-availability-zone
X-Auth-Token: {tokenId}
```

<a id="list-availability-zones-request"></a>
#### Request
This API does not require a request body.

| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| tokenId | Header | String | O | Token ID |

<a id="list-availability-zones-response"></a>
#### Response
| Name | Type | Format | Description |
|---|---|---|---|
| availabilityZoneInfo | Body | Object | Availability zone information object |
| availabilityZoneInfo.zoneName | Body | String | Availability zone name |
| availabilityZoneInfo.zoneState | Body | Object | Availability zone state information object |
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

<a id="key-pairs"></a>

## Key Pairs { #key-pairs }

<a id="list-key-pairs"></a>
### List Key Pairs { #list-key-pairs }
```
GET /v2.1/os-keypairs
X-Auth-Token: {tokenId}
```

<a id="list-key-pairs-request"></a>
#### Request
This API does not require a request body.

| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| tokenId | Header | String | O | Token ID |

<a id="list-key-pairs-response"></a>
#### Response

| Name | Type | Format | Description |
|---|---|---|---|
| keypairs | Body | Array | List of key pair objects |
| keypairs.keypair | Body | Object | Key pair object |
| keypairs.keypair.name | Body | String | Key pair name |
| keypairs.keypair.public_key | Body | String | Public key |
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

<a id="show-key-pair"></a>
### Get Key Pair { #show-key-pair }
```
GET /v2.1/os-keypairs/{keypairName}
X-Auth-Token: {tokenId}
```

<a id="show-key-pair-request"></a>
#### Request
This API does not require a request body.

| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| keypairName | URL | String | O | Key pair name |
| tokenId | Header | String | O | Token ID |

<a id="show-key-pair-response"></a>
#### Response

| Name | Type | Format | Description |
|---|---|---|---|
| keypair | Body | Object | List of key pair objects |
| keypair.public_key | Body | String | Public key |
| keypair.user_id | Body | String | Key pair owner ID |
| keypair.name | Body | String | Key pair name |
| keypair.deleted | Body | Boolean | Whether the key pair is deleted |
| keypair.created_at | Body | Datetime | Time when the key pair was created<br>`YYYY-MM-DDThh:mm:ss.SSSSSS` |
| keypair.updated_at | Body | Datetime | Time when the key pair was last updated<br>`YYYY-MM-DDThh:mm:ss.SSSSSS` |
| keypair.deleted_at | Body | Datetime | Time when the key pair was deleted<br>`YYYY-MM-DDThh:mm:ss.SSSSSS` |
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

<a id="createregister-key-pair"></a>
### Create/Register Key Pair { #createregister-key-pair }

```
POST /v2.1/os-keypairs
X-Auth-Token: {tokenId}
```

<a id="createregister-key-pair-request"></a>
#### Request

| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| tokenId | Header | String | O | Token ID |
| keypair | Body | Object | O | Key pair object |
| keypair.name | Body | String | O | Name of the key pair to create or register |
| keypair.public_key | Body | String | - | Public key to register. If this field is omitted, a new key pair is generated. |

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

<a id="createregister-key-pair-response"></a>
#### Response

| Name | Type | Format | Description |
|---|---|---|---|
| keypair | Body | Object | Key pair object |
| keypair.public_key | Body | String | Public key |
| keypair.private_key | Body | String | Private key. Returns the private key when a new key pair is generated. |
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

<a id="delete-key-pair"></a>
### Delete Key Pair { #delete-key-pair }
```
DELETE /v2.1/os-keypairs/{keypairName}
X-Auth-Token: {tokenId}
```

<a id="delete-key-pair-request"></a>
#### Request
This API does not require a request body.

| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| keypairName | URL | String | O | Key pair name |
| tokenId | Header | String | O | Token ID |

<a id="delete-key-pair-response"></a>
#### Response
This API does not return a response body.


<a id="instance"></a>

## Instance { #instance }

<a id="instance-status"></a>

### Instance Status { #instance-status }

Instances exist in various statuses, and each status defines its own set of permissible operations. See the following list of instance statuses.

| Status | Description |
|---|---|
| `ACTIVE` | Instance is active |
| `BUILD` | Instance is being created |
| `DELETED` | Instance has been deleted |
| `ERROR` | The last action taken on the instance failed |
| `HARD_REBOOT` | Instance is being forcibly restarted<br>Equivalent to powering off and powering on a physical server |
| `MIGRATING` | Instance is being migrated<br>This occurs due to live migration (moving an active instance) operations |
| `PASSWORD` | Password is being reset on the instance |
| `PAUSED` | Instance is paused<br>A paused instance is stored in the hypervisor's memory |
| `REBOOT` | Instance is in a soft reboot state<br>A reboot command is sent to the virtual machine's operating system |
| `REBUILD` | Instance is being rebuilt from the image used at creation |
| `RESCUE` | Instance is running in rescue mode |
| `RESIZE` | Instance flavor is being changed or the instance is being migrated to another host<br>The instance has been stopped and restarted |
| `REVERT_RESIZE` | Instance is restored to its original state when a failure occurs while changing flavors or migrating to another host |
| `VERIFY_RESIZE` | Instance is waiting for confirmation after changing flavors or migrating to another host<br>In NHN Cloud, the instance automatically transitions to `ACTIVE` status in this case |
| `SHELVED_OFFLOADED` | Instance has been shut down |
| `SHUTOFF` | Instance is stopped |
| `SUSPENDED` | Instance has been put into hibernation by an administrator |
| `UNKNOWN` | The instance status is unknown<br>`If the instance enters this status, contact the administrator.` | 

<a id="list-instances"></a>

### List Instances { #list-instances }

```
GET /v2.1/servers
X-Auth-Token: {tokenId}
```

<a id="list-instances-request"></a>
#### Request

This API does not require a request body.

| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| tokenId | Header | String | O | Token ID |
| reservation_id | Query | String | - | Instance creation reservation ID. <br>If a reservation ID is specified, only the list of instances created simultaneously is returned. |
| changes-since | Query | Datetime | - | Returns list of instances changed since the specified time. `YYYY-MM-DDThh:mm:ss` format. |
| image | Query | UUID | - | Image ID<br>Returns the list of instances using the specified image |
| flavor | Query | UUID | - | Instance flavor ID<br>Returns the list of instances using the specified flavor |
| name | Query | String | - | Instance name<br>Return list of instances with specified name, regex is supported |
| status | Query | Enum | - | Instance status<br>Returns the list of instances with the specified status |
| limit | Query | Integer | - | Number of instances in the list<br>Returns the list with the specified number of instances |
| marker | Query | UUID | - | UUID of the first instance in the list<br>Return list of up to `limit` instances from the instance specified as the `marker`, according to the sort order |

<a id="list-instances-response"></a>
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
          "href": "https://kr1-api-instance-infrastructure.nhncloudservice.com/v2.1/servers/aaf2778b-ea03-4ccc-8b1b-92f4b686c3ec",
          "rel": "self"
        },
        {
          "href": "https://kr1-api-instance-infrastructure.nhncloudservice.com/servers/aaf2778b-ea03-4ccc-8b1b-92f4b686c3ec",
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

<a id="list-instances-with-details"></a>

### List Instances with Details { #list-instances-with-details }

Return the list of instances created in the current tenant, same as List Instances. However, detailed instance information is returned.

```
GET /v2.1/servers/detail
X-Auth-Token: {tokenId}
```

<a id="list-instances-with-details-request"></a>

#### Request

The request format is the same as List Instances.

<a id="list-instances-with-details-response"></a>

#### Response

| Name | Type | Format | Description                                                                                                                                                                                                        |
|---|---|---|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| servers | Body | Object | Instance list object                                                                                                                                                                                                |
| status | Body | Enum | Instance status                                                                                                                                                                                                   |
| servers.id | Body | UUID | Instance ID                                                                                                                                                                                                   |
| servers.name | Body | String | Instance name, up to 255 characters                                                                                                                                                                                          |
| servers.updated | Body | Datetime | Last updated time of instance in `YYYY-MM-DDThh:mm:ssZ` format                                                                                                                                                                  |
| servers.hostId | Body | String | Host ID on which the instance is running                                                                                                                                                                                        |
| servers.addresses | Body | Object | Instance IP list object. <br>The size of the list is the number of ports attached to the instance.                                                                                                                                                             |
| servers.addresses."Network name" | Body | Object | Port information for each network attached to the instance                                                                                                                                                                                                  |
| servers.addresses."Network name".OS-EXT-IPS-MAC:mac_addr | Body | String | MAC address of the port attached to the instance                                                                                                                                                                                                      |
| servers.addresses."Network name".version | Body | Integer | IP version of the port attached to the instance<br>NHN Cloud supports only IPv4                                                                                                                                                                |
| servers.addresses."Network name".addr | Body | String | IP address of the port attached to the instance                                                                                                                                                                                                       |
| servers.addresses."Network name".OS-EXT-IPS:type | Body | Enum | IP address type of the port<br>One of `fixed` or `floating`                                                                                                                                                                |
| servers.links | Body | Object | Instance path object                                                                                                                                                                                                |
| servers.key_name | Body | String | Instance key pair name                                                                                                                                                                                               |
| servers.image | Body | Object | Instance image object                                                                                                                                                                                               |
| servers.image.id | Body | UUID | Instance image ID                                                                                                                                                                                               |
| servers.image.links | Body | Object | Instance image path object                                                                                                                                                                                            |
| servers.OS-EXT-STS:task_state | Body | String | Instance task state<br>Shows the status of a task operating on an instance                                                                                                                                                               |
| servers.OS-EXT-STS:vm_state | Body | String | Current instance state                                                                                                                                                                                                |
| servers.OS-SRV-USG:launched_at | Body | Datetime | Last boot time of the instance<br>`YYYY-MM-DDThh:mm:ss.ssssss` format                                                                                                                                                         |
| servers.OS-SRV-USG:terminated_at | Body | Datetime | Instance deletion time<br>`YYYY-MM-DDThh:mm:ssZ` format                                                                                                                                                                   |
| servers.flavor | Body | Object | Instance flavor information object                                                                                                                                                                                             |
| servers.flavor.original_name | Body | String | Instance flavor name                                                                                                                                                                                               |
| servers.flavor.vcpus | Body | Integer | Number of vCPUs                                                                                                                                                                                                 |
| servers.flavor.ram | Body | Integer | Memory size (MB)                                                                                                                                                                                              |
| servers.flavor.disk | Body | Integer | Root block storage size (GB)                                                                                                                                                                                        |
| servers.flavor.ephemeral | Body | Integer | Temporary block storage size (GB)                                                                                                                                                                                        |
| servers.flavor.swap | Body | Integer | Swap space size (MB)                                                                                                                                                                                            |
| servers.flavor.extra_specs | Body | Object | Extra specifications object                                                                                                                                                                                                 |
| servers.security_groups | Body | Object | List object of security groups assigned to the instance                                                                                                                                                                                                     |
| servers.security_groups.name | Body | String | Name of the security group assigned to the instance                                                                                                                                                                                                        |
| servers.user_id | Body | String | ID of the user who created the instance                                                                                                                                                                                          |
| servers.created | Body | Datetime | Instance created time. `YYYY-MM-DDThh:mm:ssZ` format                                                                                                                                                                     |
| servers.tenant_id | Body | String | Tenant ID to which the instance belongs                                                                                                                                                                                                           |
| servers.os-extended-volumes:volumes_attached | Body | Object | List object of additional block storage attached to the instance                                                                                                                                                                                                |
| servers.os-extended-volumes:volumes_attached.id | Body | UUID | ID of the additional block storage attached to the instance                                                                                                                                                                                                   |
| servers.OS-EXT-STS:power_state | Body | Integer | Power state of the instance<br>- `1`: On<br>- `4`: Off                                                                                                                                                                    |
| servers.metadata | Body | Object | Instance metadata object<br>Stores instance metadata as key-value pairs                                                                                                                                                                   |
| server.NHN-EXT-ATTR:ephemeral_disk_size | Body | Integer | Size of an additional local block storage attached to the instance                                                                                                                                                                   |
| server.NHN-EXT-ATTR:protect | Body | Boolean | Whether to enable deletion protection for the instance                                                                                                                                                                   |

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
          "href": "https://kr1-api-instance-infrastructure.nhncloudservice.com/v2.1/servers/aaf2778b-ea03-4ccc-8b1b-92f4b686c3ec",
          "rel": "self"
        },
        {
          "href": "https://kr1-api-instance-infrastructure.nhncloudservice.com/servers/aaf2778b-ea03-4ccc-8b1b-92f4b686c3ec",
          "rel": "bookmark"
        }
      ],
      "key_name": "access-key",
      "image": {
        "id": "8b9f8d47-b89b-45af-b1d6-3f7ce7e06a11",
        "links": [
          {
            "href": "https://kr1-api-instance-infrastructure.nhncloudservice.com/images/8b9f8d47-b89b-45af-b1d6-3f7ce7e06a11",
            "rel": "bookmark"
          }
        ]
      },
      "OS-EXT-STS:task_state": null,
      "OS-EXT-STS:vm_state": "active",
      "OS-SRV-USG:launched_at": "2020-02-25T01:22:23.000000",
      "flavor": {
        "vcpus": 32,
        "ram": 131072,
        "disk": 0,
        "ephemeral": 0,
        "swap": 0,
        "original_name": "x1.c32m128",
        "extra_specs": {
          "flavor_type": "performance"
        }
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

<a id="get-instance"></a>

### Get Instance { #get-instance }

```
GET /v2.1/servers/{serverId}
X-Auth-Token: {tokenId}
```

<a id="get-instance-request"></a>

#### Request

This API does not require a request body.

| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| serverId | URL | UUID | O | Instance ID |
| tokenId | Header | String | O | Token ID |

<a id="get-instance-response"></a>

#### Response

| Name | Type | Format | Description                                                                                                                                                                                                       |
|---|---|---|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| server | Body | Object | Instance object                                                                                                                                                                                                  |
| status | Body | Enum | Instance status                                                                                                                                                                                                  |
| server.id | Body | UUID | Instance ID                                                                                                                                                                                                  |
| server.name | Body | String | Instance name, up to 255 characters                                                                                                                                                                                         |
| server.updated | Body | Datetime | Last updated time of instance in `YYYY-MM-DDThh:mm:ssZ` format                                                                                                                                                                 |
| server.hostId | Body | String | ID of the host on which the instance is running                                                                                                                                                                                       |
| server.addresses | Body | Object | Instance IP list object <br>The size of the list is the number of ports attached to the instance                                                                                                                                                              |
| server.addresses."Network name" | Body | Object | Port information per network attached to the instance                                                                                                                                                                                 |
| server.addresses."Network name".OS-EXT-IPS-MAC:mac_addr | Body | String | MAC address of the port attached to the instance                                                                                                                                                                                     |
| server.addresses."Network name".version | Body | Integer | IP version of the port attached to the instance<br>NHN Cloud supports only IPv4                                                                                                                                               |
| server.addresses."Network name".addr | Body | String | IP address of the port attached to the instance                                                                                                                                                                                      |
| server.addresses."Network name".OS-EXT-IPS:type | Body | Enum | IP address type of the port<br>One of `fixed` or `floating`                                                                                                                                               |
| server.links | Body | Object | Instance path object                                                                                                                                                                                               |
| server.key_name | Body | String | Instance key pair name                                                                                                                                                                                              |
| server.image | Body | Object | Instance image object                                                                                                                                                                                              |
| server.image.id | Body | UUID | Instance image ID                                                                                                                                                                                              |
| server.image.links | Body | Object | Instance image path object                                                                                                                                                                                           |
| server.OS-EXT-STS:task_state | Body | String | Instance task state<br>Shows the status of a task operating on an instance                                                                                                                                               |
| server.OS-EXT-STS:vm_state | Body | String | Current instance state                                                                                                                                                                                               |
| server.OS-SRV-USG:launched_at | Body | Datetime | Last boot time of the instance<br>`YYYY-MM-DDThh:mm:ss.ssssss` format                                                                                                                                                        |
| server.OS-SRV-USG:terminated_at | Body | Datetime | Instance deletion time<br>`YYYY-MM-DDThh:mm:ssZ` format                                                                                                                                                                  |
| server.flavor | Body | Object | Instance flavor information object                                                                                                                                                                                            |
| server.flavor.original_name | Body | String | Instance flavor name                                                                                                                                                                                              |
| server.flavor.vcpus | Body | Integer | Number of vCPUs                                                                                                                                                                                                |
| server.flavor.ram | Body | Integer | Memory size (MB)                                                                                                                                                                                             |
| server.flavor.disk | Body | Integer | Root block storage size (GB)                                                                                                                                                                                       |
| server.flavor.ephemeral | Body | Integer | Temporary block storage size (GB)                                                                                                                                                                                       |
| server.flavor.swap | Body | Integer | Swap space size (MB)                                                                                                                                                                                           |
| server.flavor.extra_specs | Body | Object | Extra specifications object                                                                                                                                                                                                |
| server.security_groups | Body | Object | List object of security groups assigned to the instance                                                                                                                                                                                    |
| server.security_groups.name | Body | String | Name of the security group assigned to the instance                                                                                                                                                                                       |
| server.user_id | Body | String | ID of the user who created the instance                                                                                                                                                                                         |
| server.created | Body | Datetime | Instance created time. `YYYY-MM-DDThh:mm:ssZ` format                                                                                                                                                                    |
| server.tenant_id | Body | String | ID of the tenant that the instance belongs to                                                                                                                                                                                          |
| server.os-extended-volumes:volumes_attached | Body | Object | List object of additional block storage attached to the instance                                                                                                                                                                               |
| server.os-extended-volumes:volumes_attached.id | Body | UUID | ID of additional block storage attached to the instance                                                                                                                                                                                  |
| server.OS-EXT-STS:power_state | Body | Integer | Power state of the instance<br>- `1`: On<br>- `4`: Off                                                                                                                                                                   |
| server.metadata | Body | Object | Instance metadata object<br>Stores instance metadata as key-value pairs                                                                                                                                                                  |
| server.NHN-EXT-ATTR:ephemeral_disk_size | Body | Integer | Size of an additional local block storage attached to the instance                                                                                                                                                                  |
| server.NHN-EXT-ATTR:protect | Body | Boolean | Whether to protect the instance from deletion                                                                                                                                                                  |

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
        "href": "https://kr1-api-instance-infrastructure.nhncloudservice.com/v2.1/servers/aaf2778b-ea03-4ccc-8b1b-92f4b686c3ec",
        "rel": "self"
      },
      {
        "href": "https://kr1-api-instance-infrastructure.nhncloudservice.com/servers/aaf2778b-ea03-4ccc-8b1b-92f4b686c3ec",
        "rel": "bookmark"
      }
    ],
    "key_name": "access-key",
    "image": {
      "id": "8b9f8d47-b89b-45af-b1d6-3f7ce7e06a11",
      "links": [
        {
          "href": "https://kr1-api-instance-infrastructure.nhncloudservice.com/images/8b9f8d47-b89b-45af-b1d6-3f7ce7e06a11",
          "rel": "bookmark"
        }
      ]
    },
    "OS-EXT-STS:task_state": null,
    "OS-EXT-STS:vm_state": "active",
    "OS-SRV-USG:launched_at": "2020-02-25T01:22:23.000000",
    "flavor": {
      "vcpus": 32,
      "ram": 131072,
      "disk": 0,
      "ephemeral": 0,
      "swap": 0,
      "original_name": "x1.c32m128",
      "extra_specs": {
        "flavor_type": "performance"
      }
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

<a id="create-instance"></a>

### Create Instance { #create-instance }

Creates an instance.

After calling the instance creation API, check the instance status by retrieving the instance.

* When the instance status becomes **ACTIVE**, the instance has been successfully created.
* If the status remains in **BUILDING** for a long time or becomes **ERROR**, check parameters used for instance creation and try creating again.

Windows instances have the following creation constraints for stable operation:

* Use an instance flavor with at least 2 GB of RAM.
* At least 50 GB of root block storage is required.
* The U2 type cannot use Windows images.

The root block storage size can be set to a minimum of 10 GB for Linux and 50 GB for Windows.

When creating an instance, you can assign a placement policy through scheduler hints.



```
POST /v2.1/servers
X-Auth-Token: {tokenId}
```

<a id="create-instance-request"></a>
#### Request

| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| tokenId | Header | String | O | Token ID |
| server | body | Object | O | Server object |
| server.security_groups | body | Object | - | Security group list object<br>If omitted, the `default` group is added |
| server.security_groups.name | body | String | - | **(Conditionally required)** Name of the security group to add to the instance |
| server.user_data | body | String | - | Script and settings to run after instance boot<br>Base64-encoded string; up to 65,535 bytes are allowed |
| server.availability_zone | body | String | - | Availability zone in which to create the instance<br>If not specified, one is selected at random<br>If the source type of the root block storage is `volume` or `snapshot`, it must be set to the same availability zone as the source block storage |
| server.imageRef | Body | String | - | Image ID to use when creating the instance<br>Not required if the source type of the root block storage is `volume` or `snapshot` |
| server.flavorRef | Body | String | O | Instance flavor ID to use when creating the instance |
| server.networks | Body | Object | O | Network information object to use when creating the instance<br>A NIC is added for each network specified. Specify each network using Network ID, Subnet ID, Port ID, or Fixed IP |
| server.networks.uuid | Body | UUID | - | **(Conditionally required)** Network ID to use when creating the instance |
| server.networks.subnet | Body | UUID | - | **(Conditionally required)** Subnet ID of the network to use when creating the instance |
| server.networks.port | Body | UUID | - | **(Conditionally required)** Port ID to use when creating the instance<br>If a port ID is specified, the requested security groups are not applied to the specified existing port |
| server.networks.fixed_ip | Body | String | - | **(Conditionally required)** Fixed IP to use when creating the instance |
| server.name | Body | String | O | Name of the instance<br>Up to 255 characters in English; must be 15 characters or fewer for Windows images |
| server.metadata | Body | Object | - | Metadata object to add to the instance<br>Key-value pairs with a maximum length of 255 characters each |
| server.block_device_mapping_v2 | Body | Object | O | Block storage information object for the instance |
| server.block_device_mapping_v2.source_type | Body | Enum | O | Source type of the block storage to create<br>- `image`: Creates block storage from an image<br>- `blank`: Creates empty block storage (cannot be used as root block storage)<br>- `volume`: Uses existing block storage<br>- `snapshot`: Creates block storage from a snapshot |
| server.block_device_mapping_v2.uuid | Body | String | - | **(Conditionally required)** Must be set differently depending on the source type of the block storage<br>- If the source type is `image`: set the image ID<br>- If the source type is `volume`: set the ID of existing block storage<br>- If the source type is `snapshot`: set the snapshot ID<br>- If the source type is `blank`: not required<br>If this is the root block storage, the source must be bootable |
| server.block_device_mapping_v2.boot_index | Body | Integer | O | Boot order of the specified block storage<br>- `0` indicates root block storage<br>- Any other value indicates additional block storage<br>A higher value means a lower boot priority |
| server.block_device_mapping_v2.destination_type | Body | Enum | O | Location of the instance's block storage; must be set differently depending on the instance flavor.<br>- `local`: For GPU instances or U2 instance flavors<br>- `volume`: For all other instance flavors |
| server.block_device_mapping_v2.volume_type | Body | Enum    | - | **(Conditionally required)** Type of block storage to create<br>Not required if the source type of the block storage is `volume` or `snapshot`<br>See the `name` in the **List Block Storage Types** response in `User Guide > Storage > Block Storage > API v2 Guide` |
| server.block_device_mapping_v2.delete_on_termination | Body | Boolean | - | Whether to delete the block storage when the instance is deleted; the default is `false`.<br>`true` deletes it, `false` retains it |
| server.block_device_mapping_v2.volume_size | Body | Integer | - | **(Conditionally required)** Size of the block storage to create<br>Must be set differently depending on the source type of the block storage<br>- Not required if the source type is `volume`<br>- If the source type is `snapshot`, set to a value equal to or greater than the size of the source block storage<br>In `GB`<br>Uses the U2 instance type and root block storage is created with the size specified in the U2 instance type and this value will be ignored<br>Different instance types have different sizes of root block storage that can be created. For more details, see `User Guide > Compute > Instance > Console User Guide > Create Instance > Block Storage Size` |
| server.block_device_mapping_v2.nhn_encryption                   | Body | Object | - | **(Conditionally required)** Encryption information for the block storage                                                                                                                                                                                        |
| server.block_device_mapping_v2.nhn_encryption.skm_appkey        | Body | String | - | **(Conditionally required)** Appkey of the Secure Key Manager service                                                                                                                                                                              |
| server.block_device_mapping_v2.nhn_encryption.skm_key_id        | Body | String | - | **(Conditionally required)** Symmetric key ID in Secure Key Manager to use when creating encrypted block storage                                                                                                                                  |
| server.key_name | Body | String | O | Key pair to use to access the instance |
| server.min_count | Body | Integer | - | Minimum number of instances to create with the current request.<br>The default is 1.<br>Can only be set to `1` if the source type of the block storage is `volume` |
| server.max_count | Body | Integer | - | Maximum number of instances to create with the current request.<br>The default is min_count and the maximum is 10.<br>Can only be set to `1` if the source type of the block storage is `volume` |
| server.return_reservation_id | Body | Boolean | - | Reservation ID for the instance creation request.<br>If set to `True`, a reservation ID is returned instead of instance creation information.<br>The default is `False` |
| os:scheduler_hints | Body | Object | - | Scheduler hints object |
| os:scheduler_hints.group | Body | String | - | Placement policy ID |

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

<a id="create-instance-response"></a>
#### Response

| Name | Type | Format | Description                                                                                                                                                                                                           |
|---|---|---|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| server.security_groups.name | Body | String | Name of the security group of the created instance                                                                                                                                                                                                           |
| server.id | Body | UUID | ID of the created instance                                                                                                                                                                                                 |

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
        "href": "https://kr1-api-instance-infrastructure.nhncloudservice.com/v2.1/servers/3a005d5b-63cf-4493-bfc6-49db990b5b50",
        "rel": "self"
      },
      {
        "href": "https://kr1-api-instance-infrastructure.nhncloudservice.com/servers/3a005d5b-63cf-4493-bfc6-49db990b5b50",
        "rel": "bookmark"
      }
    ]
  }
}
```

</p>
</details>

---

<a id="modify-instance"></a>

### Modify Instance { #modify-instance }
Modifies a created instance. Modifiable attributes are limited to certain properties.

```
PUT /v2.1/servers/{serverId}
X-Auth-Token: {tokenId}
```

<a id="modify-instance-request"></a>
#### Request

| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| serverId | URL | UUID | O | ID of the instance to modify |
| tokenId | Header | String | O | Token ID |
| server | Body | Object | O | Instance modification request object |
| server.name | Body | String | - | New name of the instance |

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

<a id="modify-instance-response"></a>
#### Response
Same as Get Instance.

---

<a id="delete-instance"></a>

### Delete Instance { #delete-instance }
Deletes a created instance.

```
DELETE /v2.1/servers/{serverId}
X-Auth-Token: {tokenId}
```

<a id="delete-instance-request"></a>
#### Request
This API does not require a request body.

| Name | Type | Format | Required | Description |
|---|---|---|---|--|
| serverId | URL | UUID | O | ID of the instance to delete |
| tokenId | Header | String | O | Token ID |

<a id="delete-instance-response"></a>
#### Response
This API does not return a response body.

---

<a id="manage-block-storage-attachment"></a>

## Manage Block Storage Attachment { #manage-block-storage-attachment }

<a id="list-additional-block-storage-attached-to-the-instance"></a>
### List Additional Block Storage Attached to the Instance { #list-additional-block-storage-attached-to-the-instance }
```
GET /v2.1/servers/{serverId}/os-volume_attachments
X-Auth-Token: {tokenId}
```

<a id="list-additional-block-storage-attached-to-the-instance-request"></a>
#### Request
This API does not require a request body.

| Name | Type | Format | Required | Description |
|---|---|---|---|--|
| serverId | URL | UUID | O | ID of the instance to modify |
| tokenId | Header | String | O | Token ID |
| limit | Query | Integer | - | Number of items to retrieve |
| offset | Query | Integer | - | Starting point of the list to return<br>Return block storage starting from offset of the entire list |

<a id="list-additional-block-storage-attached-to-the-instance-response"></a>
#### Response

| Name | Type | Format | Description |
|---|---|---|---|
| volumeAttachments | Body | Array | List of attachment information objects |
| volumeAttachments.device | Body | String | Block storage name on the instance<br>Example: `/dev/vdb` |
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

<a id="show-additional-block-storage-attached-to-the-instance"></a>
### Show Additional Block Storage Attached to the Instance { #show-additional-block-storage-attached-to-the-instance }
```
GET /v2.1/servers/{serverId}/os-volume_attachments/{volumeId}
X-Auth-Token: {tokenId}
```

<a id="show-additional-block-storage-attached-to-the-instance-request"></a>
#### Request
This API does not require a request body.

| Name | Type | Format | Required | Description |
|---|---|---|---|--|
| serverId | URL | UUID | O | Instance ID |
| volumeId | URL | UUID | O | ID of the block storage to retrieve |
| tokenId | Header | String | O | Token ID |

<a id="show-additional-block-storage-attached-to-the-instance-response"></a>
#### Response

| Name | Type | Format | Description |
|---|---|---|---|
| volumeAttachment | Body | Object | Attachment information object |
| volumeAttachment.device | Body | String | Block storage name on the instance<br>Example: `/dev/vdb` |
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

<a id="attach-additional-block-storage-to-the-instance"></a>
### Attach Additional Block Storage to the Instance { #attach-additional-block-storage-to-the-instance }
```
POST /v2.1/servers/{serverId}/os-volume_attachments
X-Auth-Token: {tokenId}
```

<a id="attach-additional-block-storage-to-the-instance-request"></a>
#### Request

| Name | Type | Format | Required | Description |
|---|---|---|---|--|
| serverId | URL | UUID | O | ID of the instance to modify |
| tokenId | Header | String | O | Token ID |
| volumeAttachment | Body | Object | O | Object to request block storage attachment |
| volumeAttachment.volumeId | Body | UUID | O | ID of the block storage to attach |

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

<a id="attach-additional-block-storage-to-the-instance-response"></a>
#### Response

| Name | Type | Format | Description |
|---|---|---|---|
| volumeAttachment | Body | Object | Attachment information object |
| volumeAttachment.device | Body | String | Block storage name on the instance<br>Example: `/dev/vdb` |
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

<a id="detach-block-storage-from-the-instance"></a>
### Detach Block Storage from the Instance { #detach-block-storage-from-the-instance }
```
DELETE /v2.1/servers/{serverId}/os-volume_attachments/{volumeId}
X-Auth-Token: {tokenId}
```

<a id="detach-block-storage-from-the-instance-request"></a>
#### Request
This API does not require a request body.

| Name | Type | Format | Required | Description |
|---|---|---|---|--|
| serverId | URL | UUID | O | Instance ID |
| volumeId | URL | UUID | O | ID of block storage to detach |
| tokenId | Header | String | O | Token ID |

<a id="detach-block-storage-from-the-instance-response"></a>
#### Response
This API does not return a response body.

---

<a id="additional-instance-features"></a>

## Additional Instance Features { #additional-instance-features }
NHN Cloud provides the following instance control and additional features:

* Start, stop, terminate, and restart instances
* Change instance flavor
* Create instance images
* Add and delete security groups

<a id="start-stopped-instance"></a>

### Start Stopped Instance { #start-stopped-instance }

Restart a stopped instance and change its status to **ACTIVE**. To call this API, the instance status must be **SHUTOFF**.

```
POST /v2.1/servers/{serverId}/action
X-Auth-Token: {tokenId}
```

<a id="start-stopped-instance-request"></a>
#### Request
| Name | Type | Format | Required | Description |
|---|---|---|---|--|
| serverId | URL | UUID | O | ID of the instance to change |
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

<a id="start-stopped-instance-response"></a>
#### Response
This API does not return a response body.

---

<a id="start-terminated-instance"></a>

### Start Terminated Instance { #start-terminated-instance }

Restart a terminated instance and changes its status to **ACTIVE**. To call this API, the instance's state must be **SHELVED_OFFLOADED**.

```
POST /v2.1/servers/{serverId}/action
X-Auth-Token: {tokenId}
```

<a id="start-terminated-instance-request"></a>
#### Request
| Name | Type | Format | Required | Description |
|--|---|---|---|--|
| serverId | URL | UUID | O | ID of the instance to change |
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

<a id="start-terminated-instance-response"></a>
#### Response
This API does not return a response body.

---

<a id="stop-instance"></a>

### Stop Instance { #stop-instance }

Stop instance and change its status to **SHUTOFF**. To call this API, the instance status must be either **ACTIVE** or **ERROR**.

```
POST /v2.1/servers/{serverId}/action
X-Auth-Token: {tokenId}
```

<a id="stop-instance-request"></a>
#### Request
| Name | Type | Format | Required | Description |
|---|---|---|---|--|
| serverId | URL | UUID | O | ID of the instance to change |
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

<a id="stop-instance-response"></a>
#### Response
This API does not return a response body.

---

<a id="terminate-instance"></a>

### Terminate Instance { #terminate-instance }

Terminate an instance and change its status to **SHELVED_OFFLOADED**. To call this API, the instance status must be **ACTIVE**.

```
POST /v2.1/servers/{serverId}/action
X-Auth-Token: {tokenId}
```

<a id="terminate-instance-request"></a>
#### Request
| Name | Type | Format | Required | Description |
|---|---|---|---|-------------|
| serverId | URL | UUID | O | ID of the instance to change |
| tokenId | Header | String | O | Token ID |
| shelve | Body | none | O | Instance termination request |

<details><summary>Example</summary>
<p>

```json
{
  "shelve" : null
}
```

</p>
</details>

<a id="terminate-instance-response"></a>
#### Response
This API does not return a response body.

---

<a id="restart-instance"></a>

### Restart Instance { #restart-instance }

Restart an instance. The restart method can be either **SOFT** or **HARD**.

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
POST /v2.1/servers/{serverId}/action
X-Auth-Token: {tokenId}
```

<a id="restart-instance-request"></a>
#### Request
| Name | Type | Format | Required | Description |
|---|---|---|---|--|
| serverId | URL | UUID | O | ID of the instance to change |
| tokenId | Header | String | O | Token ID |
| reboot | Body | Object | O | Instance reboot request object |
| reboot.type | Body | Enum | O | Reboot method, **SOFT** or **HARD** |

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

<a id="restart-instance-response"></a>
#### Response
This API does not return a response body.

---

<a id="change-instance-flavor"></a>

### Change Instance Flavor { #change-instance-flavor }

Change the flavor of an instance. Flavors can only be changed when an instance is **ACTIVE** or **SHUTOFF**. If an instance is **ACTIVE**, the instance is stopped and restarted while changing flavors.

Depending on the current image and flavor you are using, you may be restricted from changing to some flavors. For more details, see the Console Guide.


```
POST /v2.1/servers/{serverId}/action
X-Auth-Token: {tokenId}
```

<a id="change-instance-flavor-request"></a>
#### Request
| Name | Type | Format | Required | Description |
|---|---|---|---|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| serverId | URL | UUID | O | ID of the instance to change |
| tokenId | Header | String | O | Token ID |
| resize | Body | Object | O | Instance flavor change request |
| resize.flavorRef | Body | UUID | O | ID of the instance flavor to change to |

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

<a id="change-instance-flavor-response"></a>
#### Response
This API does not return a response body.

---

<a id="create-instance-image"></a>

### Create Instance Image { #create-instance-image }

Create an image from an instance. Only `U2` flavor instances can create images via this API. To create images of non-`U2` flavor instances, see [Block Storage API](/Storage/Block Storage/en/public-api/#create-image-with-block-storage).

Images can only be created when an instance is **ACTIVE**, **SHUTOFF**, **SUSPENDED**, or **PAUSED**. It is recommended to stop instances before creating images to ensure data integrity.

When an image is successfully created, the image status becomes `active`. To check if an image is successfully created, use the Get Image API to continuously check its status.

> [Caution]
> The size of the created image may be larger than the actual usage of the root block storage.

```
POST /v2.1/servers/{serverId}/action
X-Auth-Token: {tokenId}
```

<a id="create-instance-image-request"></a>
#### Request
| Name | Type | Format | Required | Description |
|---|---|---|---|--|
| serverId | URL | UUID | O | ID of the instance to change |
| tokenId | Header | String | O | Token ID |
| createImage | Body | Object | O | Image creation request |
| createImage.name | Body | String | O | Name of the image to create |
| createImage.metadata | Body | Object | - | Metadata of the image to create<br>Specified in key-value format |

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


<a id="create-instance-image-response"></a>
#### Response

| Name | Type | Format | Description |
|--|--|--|--|
| image_id | Body | UUID | ID of the created image |

<details><summary>Example</summary>
<p>

```json
{
  "image_id": "7a0e6e4f-9d4a-4f2c-9b6f-3f08b9d2a5c1"
}
```

</p>
</details>

---

<a id="add-security-group"></a>

### Add Security Group { #add-security-group }

Add a security group to an instance. The added security group is applied to all ports of the instance.

```
POST /v2.1/servers/{serverId}/action
X-Auth-Token: {tokenId}
```

<a id="add-security-group-request"></a>
#### Request
| Name | Type | Format | Required | Description |
|---|---|---|---|--|
| serverId | URL | UUID | O | ID of the instance to change |
| tokenId | Header | String | O | Token ID |
| addSecurityGroup | Body | Object | O | Security group addition request object |
| addSecurityGroup.name | Body | String | O | Name of the security group to add |

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


<a id="add-security-group-response"></a>
#### Response
This API does not return a response body.

---

<a id="delete-security-group"></a>

### Delete Security Group { #delete-security-group }

Delete a security group from an instance. The specified security group is deleted from all ports of the instance.

```
POST /v2.1/servers/{serverId}/action
X-Auth-Token: {tokenId}
```

<a id="delete-security-group-request"></a>
#### Request
| Name | Type | Format | Required | Description |
|---|---|---|---|--|
| serverId | URL | UUID | O | ID of the instance to change |
| tokenId | Header | String | O | Token ID |
| removeSecurityGroup | Body | Object | O | Security group deletion request object |
| removeSecurityGroup.name | Body | String | O | Name of the security group to delete |

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


<a id="delete-security-group-response"></a>
#### Response
This API does not return a response body.


<a id="instance-metadata"></a>

## Instance Metadata { #instance-metadata }

Instance metadata values determine the content of the instance details screen on the **Compute > Instance** page in the Console. The details of each instance metadata are as follows:

| Instance Metadata | Description |
|----------------|----------------------------------------------|
| os_distro      | Name of **OS** under **Basic Information**<br>Used in combination with os_version |
| os_version     | Version of **OS** under **Basic Information**<br>Used in combination with os_distro |
| image_name     | **Image name** under **Basic Information** |
| os_type        | Format of **Connection Information** |
| login_username | Username under **Connection Information** |

> [Caution] Modifying or deleting instance metadata may affect related services and features, and you are responsible for the consequences.

<a id="view-a-list-of-instance-metadata"></a>

### List Instance Metadata { #view-a-list-of-instance-metadata }

```
GET /v2.1/servers/{serverId}/metadata
X-Auth-Token: {tokenId}
```

<a id="view-a-list-of-instance-metadata-request"></a>

#### Request
This API does not require a request body.

| Name | Type | Format | Required | Description |
|----------|---|---|---|--------------------------------------------------|
| serverId | URL | UUID | O | Instance ID |
| tokenId  | Header | String | O | Token ID |

<a id="view-a-list-of-instance-metadata-response"></a>

#### Response

| Name | Type | Format | Description |
|----------|---|---|--------------------------------------------------|
| metadata | Body | Object | Metadata object to create or modify for the instance<br>Key-value pairs of max 255 characters |

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


<a id="view-instance-metadata"></a>

### Get Instance Metadata { #view-instance-metadata }

```
GET /v2.1/servers/{serverId}/metadata/{key}
X-Auth-Token: {tokenId}
```

<a id="view-instance-metadata-request"></a>

#### Request
This API does not require a request body.

| Name | Type | Format | Required | Description |
|----------|---|---|---|--------------------------|
| serverId | URL | UUID | O | Instance ID |
| key      | URL | String | O | Key of the metadata to create or modify for the instance |
| tokenId  | Header | String | O | Token ID |

<a id="view-instance-metadata-response"></a>

#### Response

| Name | Type | Format | Description |
|------|---|---|--------------------------------------------------|
| meta | Body | Object | Metadata object to create or modify for the instance<br>Key-value pairs of max 255 characters |

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

<a id="createmodify-instance-metadata"></a>

### Create/Modify Instance Metadata { #createmodify-instance-metadata }

Creates or modifies the metadata of an instance.
If the requested key matches an existing key, the key-value pair is updated to the requested value.

```
PUT /v2.1/servers/{serverId}/metadata/{key}
X-Auth-Token: {tokenId}
```

<a id="createmodify-instance-metadata-request"></a>

#### Request
| Name | Type | Format | Required | Description |
|----------|---|---|---|--------------------------------------------------|
| serverId | URL | UUID | O | Instance ID |
| key      | URL | String | O | Key of the metadata to create or modify for the instance |
| tokenId  | Header | String | O | Token ID |
| meta     | Body | Object | O | Metadata object to create or modify for the instance<br>Key-value pairs of max 255 characters |

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


<a id="createmodify-instance-metadata-response"></a>

#### Response

| Name | Type | Format | Description |
|------|---|---|--------------------------------------------------|
| meta | Body | Object | Metadata object to create or modify for the instance<br>Key-value pairs of max 255 characters |

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


<a id="delete-instance-metadata"></a>

### Delete Instance Metadata { #delete-instance-metadata }

Deletes the metadata of the instance that matches the requested key.

```
DELETE /v2.1/servers/{serverId}/metadata/{key}
X-Auth-Token: {tokenId}
```

<a id="delete-instance-metadata-request"></a>

#### Request
This API does not require a request body.

| Name | Type | Format | Required | Description |
|----------|---|---|---|---------------------|
| serverId | URL | UUID | O | Instance ID |
| key      | URL | String | O | Key of the metadata to delete from the instance |
| tokenId  | Header | String | O | Token ID |

<a id="delete-instance-metadata-response"></a>

#### Response
This API does not return a response body.


<a id="placement-policy"></a>

## Placement Policy { #placement-policy }

<a id="create-a-placement-policy"></a>
### Create a Placement Policy { #create-a-placement-policy }

Creates a placement policy.
Only the `anti-affinity` placement policy type for distributed placement is provided.

```
POST /v2.1/os-server-groups
X-Auth-Token: {tokenId}
```

<a id="create-a-placement-policy-request"></a>
#### Request
| Name | Type | Format | Required | Description |
|-----|-----|-----|-----|-----|
| tokenId | Header | String | O | Token ID |
| server_group | Body | Object | O | Placement policy object |
| server_group.name | Body | String | O | Placement policy name |
| server_group.policies | Body | Array | O | Placement policy type<br>Only `anti-affinity` can be set |

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

<a id="create-a-placement-policy-response"></a>
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

<a id="view-the-list-of-placement-policies"></a>
### View the List of Placement Policies { #view-the-list-of-placement-policies }

```
GET /v2.1/os-server-groups
X-Auth-Token: {tokenId}
```

<a id="view-the-list-of-placement-policies-request"></a>
#### Request

This API does not require a request body.

| Name | Type | Format | Required | Description |
|-----|-----|-----|-----|-----|
| tokenId | Header | String | O | Token ID |

<a id="view-the-list-of-placement-policies-response"></a>
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

<a id="view-placement-policies"></a>
### View Placement Policies { #view-placement-policies }

```
GET /v2.1/os-server-groups/{servergroupId}
X-Auth-Token: {tokenId}
```

<a id="view-placement-policies-request"></a>
#### Request

This API does not require a request body.

| Name | Type | Format | Required | Description |
|-----|-----|-----|-----|-----|
| servergroupId | URL | String | O | Placement policy ID |
| tokenId | Header | String | O | Token ID |

<a id="view-placement-policies-response"></a>
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

<a id="deleting-a-placement-policy"></a>
### Delete a Placement Policy { #deleting-a-placement-policy }

```
DELETE /v2.1/os-server-groups/{servergroupId}
X-Auth-Token: {tokenId}
```

<a id="deleting-a-placement-policy-request"></a>
#### Request

This API does not require a request body.

| Name | Type | Format | Required | Description |
|-----|-----|-----|-----|-----|
| servergroupId | URL | String | O | Placement policy ID |
| tokenId | Header | String | O | Token ID |

<a id="deleting-a-placement-policy-response"></a>
#### Response

This API does not return a response body.