## Compute > Instance > API 가이드

TOAST Compute Instance서비스에서는 다음 종류의 API를 제공합니다. 

* 가용성 영역 API
* 인스턴스 API
* 인스턴스 추가기능 API
* 인스턴스 사양 API
* 키페어 API


## 사전 준비

인스턴스 API를 사용하려면 토큰 발급과 같은 사전 준비가 필요합니다. [API 사용 준비](/ko/Infrastructure%20Common/ko/api-common/)를 참조합니다.

## 가용성 영역 API

### 가용성 영역 조회
인스턴스, 블록 스토리지를 생성할 수 있는 AZ 정보를 조회합니다.

#### Method, URL
```
GET /v1.0/appkeys/{appkey}/availability-zones
X-Auth-Token: {tokenId}
```

|  Name | In | Type | Optional | Description |
|--|--|--|--|--|
| tokenId | Header | String | - | 토큰 ID |

#### Request Body
이 API는 request body를 필요로 하지 않습니다.

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
| Zone Name | Body | String | AZ 이름 |
| Available | Body | Boolean | AZ 가용 여부 |

## 인스턴스 API
인스턴스 생성, 삭제, 정보 조회 및 블록 스토리지 연결관리 기능을 제공합니다.

### 인스턴스 상태
인스턴스는 생성, 변경, 삭제, 운영 중 다음과 같은 상태를 갖습니다.
![[그림 1] 인스턴스 Status Diagram](http://static.toastoven.net/prod_infrastructure/compute/developersguide/img_001.png)


| 상태 | 설명 |
| --- | --- |
| BUILD | 인스턴스 생성 중 |
| POWERING_ON | 인스턴스 부팅 중 |
| ACTIVE | 인스턴스 구동 중 |
| POWERING_OFF | 인스턴스 종료 중 |
| STOP | 인스턴스 종료 상태 |
| REBOOTING | 인스턴스 리부팅 중 |
| RESIZING | 인스턴스 사양 변경 작업 중 |
| MIGRATING | 인스턴스 Migration 작업 중 |
| DELETING | 인스턴스 삭제 중 |
| ERROR | 오류 상태 |

### 인스턴스 정보 간략 조회
생성되어 있는 인스턴스의 간략한 정보를 조회합니다.
#### Method, URL
```
GET /v1.0/appkeys/{appkey}/instances?id={instanceId}
X-Auth-Token: {tokenId}
```

|  Name | In | Type | Optional |Description |
|--|--|--|--|--|
| tokenId | Header | String | - | 토큰 ID |
| instanceId | Query | String | O | 조회할 인스턴스 ID. 기재하지 않을 경우 모든 인스턴스들의 간략 정보를 조회합니다. |

#### Request Body
이 API는 Request Body를 필요로 하지 않습니다.

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
| Instance ID | Body | String | 인스턴스 ID |
| Instance Name | Body | String | 인스턴스 이름 (리눅스의 경우 최대 255자, 윈도우의 경우 최대 12자) |
| Instance Status | Body | String | 인스턴스의 상태 |

### 인스턴스 상세 조회
인스턴스의 상세한 정보를 조회합니다.
#### Method, URL
```
GET /v1.0/appkeys/{appkey}/instances-detail?id={instanceId}
X-Auth-Token: {tokenId}
```

|  Name | In | Type | Optional |Description |
|--|--|--|--|--|
| tokenId | Header | String | - | 토큰 ID |
| instanceId | Query | String | O | 조회할 인스턴스의 ID. 생략 시 모든 인스턴스들의 상세 정보를 조회합니다. |

#### Request Body
이 API는 Request Body를 필요로 하지 않습니다.

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
                "key": "value"
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
| MAC Address | Body | String | NIC의 MAC 주소 |
| IP Address | Body | String | NIC의 IP 주소 |
| version | Body | Integer | IP 버전. (IPv4만 지원) |
| Floating IP Address | Body | String | NIC에 할당된 플로팅 IP 주소 |
| Zone Name | Body | String | 가용성 영역 이름 |
| Flavor ID | Body | String | 인스턴스 사양 ID |
| Flavor Name | Body | String | 인스턴스 사양 이름 |
| Flavor CPU | Body | Integer | CPU 개수 |
| Flavor RAM | Body | Integer | RAM 크기 (MB) |
| Status | Body | String | 인스턴스의 상태 |
| Instance ID | Body | String | 인스턴스 ID |
| Instance Name | Body | String | 인스턴스 이름 (리눅스의 경우 최대 255자, 윈도우의 경우 최대 12자) |
| Image ID | Body | String | 인스턴스에 설치된 이미지 ID |
| metadata | Body | Object | 인스턴스에 설정할 사용자 메타데이터, "key": "value" 형태로 저장 |
| PEM Key Name | Body | String | 인스턴스에 등록할 키페어 이름 |
| Root Volume Size | Body | Integer | 인스턴스 기본 디스크 장치 크기 (GB) |
| Attached Volume ID | Body | String | 추가 블록 스토리지 ID |
| Attached Volume Name | Body | String | 추가 블록 스토리지 이름 |
| Attached Volume Size | Body | Integer | 추가 블록 스토리지 크기 (GB) |
| Security Group Name | Body | String | 인스턴스에 등록된 보안 그룹의 이름 |
| Launched Time | Body | String | 인스턴스 최근 부팅 시각. yyyy-mm-ddTHH:MM:ssZ의 형태. 예) 2017-05-16T02:17:50.166563 |
| Created Time | Body | String | 인스턴스 생성 시각. yyyy-mm-ddTHH:MM:ssZ의 형태. 예) 2017-05-16T02:17:50.166563 |
| Updated Time | Body | String | 인스턴스 수정 시각. yyyy-mm-ddTHH:MM:ssZ의 형태. 예) 2017-05-16T02:17:50.166563 |

### 인스턴스 생성
새로운 인스턴스를 생성합니다.

#### Method, URL
```
POST /v1.0/appkeys/{appkey}/instances
X-Auth-Token: {tokenId}
Content-Type: application/json;charset=UTF-8
```

|  Name | In | Type | Optional | Description |
|--|--|--|--|--|
| tokenId | Header | String | - | 토큰 ID |

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
        "volumes": [
        	{
            	"size": "{Volume Size}"
        	}
        ],
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
| Instance Name | Body | String | - | 인스턴스 이름 (리눅스의 경우 최대 255자, 윈도우의 경우 최대 12자) |
| Image ID | Body | String | - | 인스턴스에 설치할 이미지 ID. [이미지 API](/ko/Compute/Image/ko/api-guide/) 참조 |
| Flavor ID | Body | String | - | 인스턴스 사양 ID. |
| Network ID | Body | String | - | 인스턴스가 연결될 네트워크 ID. |
| Availability Zone | Body | String | - | 인스턴스가 생성될 가용성 영역 이름. |
| Key Name | Body | String | - | 인스턴스에 등록할 키페어 이름. |
| Count | Body | Integer | - | 동시 생성할 인스턴스의 개수, 최대 10개로 제한 |
| Volume Size | Body | Integer | - | 인스턴스의 기본 디스크 크기, 생성 가능한 크기는 [콘솔 가이드](./console-guide)를 참조. |
| Security Group Name | Body | String | - | 인스턴스에 등록할 보안 그룹 이름 |

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
| Instance ID | body | String |생성된 인스턴스 ID |
| Instance Name | body | String | 인스턴스 이름 (리눅스의 경우 최대 255자, 윈도우의 경우 최대 12자) |
| Instance Status | Body | String | 인스턴스의 상태 |

### 인스턴스 삭제
특정 인스턴스를 삭제합니다.
#### Method, URL
```
DELETE /v1.0/appkeys/{appkey}/instances?id={instanceId}
X-Auth-Token: {tokenId}
```

|  Name | In | Type | Optional | Description |
|--|--|--|--|--|
| tokenId | Header | String | - | 토큰 ID |
| instanceId | Query | String | - | 삭제할 인스턴스 ID |

### 블록 스토리지 연결
인스턴스에 블록 스토리지를 연결합니다.

#### Method, URL
```
POST /v1.0/appkeys/{appkey}/instances/{instanceId}/attachments
X-Auth-Token: {tokenId}
Content-Type: application/json;charset=UTF-8
```

|  Name | In | Type | Optional | Description |
|--|--|--|--|--|
| tokenId | Header | String | - | 토큰 ID |
| instanceId | Path | String | - | 블록 스토리지를 연결할 인스턴스 ID |

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
| Volume ID | body | String | - | 인스턴스에 연결할 블록 스토리지 ID. |

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
| Device ID | body | String | 인스턴스에 등록된 장치 이름, 예) "/dev/vdc" |
| Attachement ID | body | String | Attachment ID |
| Volume ID | body | String | 블록 스토리지 ID, 연결 해제 시 필요 |

### 블록 스토리지 연결 해제
인스턴스에 연결되어 있는 블록 스토리지 연결을 해제합니다.

#### Method, URL
```
DELETE /v1.0/appkeys/{appkey}/instances/{instanceId}/attachments?volumeId={volumeId}
X-Auth-Token: {tokenId}
```

|  Name | In | Type | Optional | Description |
|--|--|--|--|--|
| tokenId | Header | String| - | 토큰 ID |
| instanceId | Path | String | - | 인스턴스 ID |
| volumeId | Path | String | - | 블록 스토리지 ID |

#### Request body
이 Request는 Body를 필요로 하지 않습니다.

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

## 인스턴스 추가기능 API
다음과 같은 인스턴스 제어 및 부가기능을 제공합니다.

- 인스턴스 시작/정지/재시작
- 인스턴스 사양 변경(Resize)
- 인스턴스 이미지 생성
- 플로팅 IP 연결/해제
- 보안 그룹 등록/해제

### 공통
모든 인스턴스 Action API는 동일한 Method, URL로 호출하며, Request Body로 각 Action을 구분합니다.
#### Method, URL
```
POST /v1.0/appkeys/{appkey}/instances/{instanceId}/action
X-Auth-Token: {tokenId}
Content-Type: application/json;charset=UTF-8
```

|  Name | In | Type | Optional | Description |
|--|--|--|--|--|
| tokenId | Header | String| - | 토큰 ID |
| instanceId | Path | String | - | Action을 수행할 인스턴스 ID |

#### Request Body Template
```json
{
    "action": "Action Name",
    "parameters" : {
    	"key": "value"
    }
}
```
|  Name | In | Type | Optional | Description |
|--|--|--|--|--|
| Action Name | Body | String | - | 인스턴스에서 실행할 Action명 |
| parameters | Body | Object| O | Action 수행에 필요한 파라미터. Action에 따라 필요한 값을 기재합니다. 일부 action은 파라미터 없이 동작합니다. |

### 인스턴스 시작
정지(STOP) 상태의 인스턴스를 시작합니다.
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

### 인스턴스 정지
동작중(ACTIVE) 또는 오류(ERROR) 상태의 인스턴스를 정지합니다.
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

### 인스턴스 리부팅
인스턴스를 리부팅합니다. 아래와 같은 리부팅 방식을 지정할 수 있습니다.

 - `SOFT`: Graceful Shutdown 수행 후 인스턴스를 재시작합니다.
 - `HARD`: 강제 Shutdown 수행 후 인스턴스를 재시작합니다.

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
| Reboot Type | body | String | - | Reboot 타입. `HARD` or `SOFT` |

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

### 인스턴스 사양 변경
인스턴스의 사양을 변경합니다.
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
|  Flavor ID | body | String | - | 변경할 인스턴스 사양 ID. |

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

### 이미지 생성
지정한 인스턴스로 부터 이미지를 생성합니다. 생성된 이미지는 [이미지 API](/ko/Compute/Image/ko/api-guide/)로 조회할 수 있습니다.
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
| ImageName | body | String | - | 생성할 이미지 이름 |

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
| Created Image ID | body | String | 생성된 이미지 ID |
| Created Image Name | body | String | 생성된 이미지 이름 |

### 플로팅 IP 연결
플로팅 IP를 인스턴스에 연결합니다.

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
| Floating IP Address | body | String | - | 인스턴스에 연결할 플로팅 IP 주소 |
| IP Address of the instance | body | String | - | 플로팅 IP를 연결할 인스턴스의 IP 주소 |

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

### 플로팅 IP 연결 해제
인스턴스에 연결되어 있는 플로팅 IP를 연결 해제합니다.

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
| Floating IP Address | body | String | - | 연결을 해제할 플로팅 IP 주소 |

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

### 보안 그룹 등록
인스턴스에 보안 그룹을 추가 등록합니다.

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
| Security Group Name | body | String | - | 인스턴스에 추가할 보안 그룹 이름 |

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

### 보안 그룹 제거
인스턴스에 등록되어 있는 보안 그룹을 제거합니다.

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
| Security Group Name | body | String | - | 인스턴스에서 제거할 보안 그룹 이름 |

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

## 인스턴스 사양 API
### 인스턴스 사양 목록 조회
인스턴스 사양의 목록 및 상세 정보를 조회합니다.

#### Method, URL
```
GET /v1.0/appkeys/{appkey}/flavors
X-Auth-Token: {tokenID}
```

|  Name | In | Type | Optional | Description |
|--|--|--|--|--|
| tokenId | Header | String| - | 토큰 ID |

#### Request Body
이 API는 Request Body를 필요로 하지 않습니다.

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
            "minVolumeSize": "{Min Volume Size}",
            "maxVolumeSize": "{Max Volume Size}",
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
| Disabled | Body | Boolean | 인스턴스 사양 비활성화 여부 |
| Ephermeral | Body | Integer | 임시 디스크 사이즈, GB |
| Type | Body | String | 인스턴스 사양 최적화 특성에 따라 구분되는 Type값. "general, "compute", "memory" |
| Min Volume Size | Body | Integer | 기본 디스크 장치로 만들 수 있는 최소 디스크 크기. GB |
| Max Volume Size | Body | Integer | 기본 디스크 장치로 만들 수 있는 최대 디스크 크기. GB |
| Flavor ID | Body | String | 인스턴스 사양 ID |
| Flavor Name | Body | String | 인스턴스 사양 이름 |
| Is Public | Body | Boolean | 공용 인스턴스 사양 여부 |
| RAM | Body | Integer | 인스턴스 사양이 갖는 RAM 총량. MB |
| VCPUs | Body | Integer | 인스턴스에 할당되는 가상 CPU 코어 개수 |

## 키페어 API
인스턴스 접근에 필요한 키페어에 대한 생성, 삭제, 조회 기능을 제공합니다.
### 키페어 조회
키페어를 조회합니다.
#### Method, URL
```
GET /v1.0/appkeys/{appkey}/keypairs?name={keypairName}
X-Auth-Token: {tokenId}
```

|  Name | In | Type | Optional | Description |
|--|--|--|--|--|
| tokenId | Header | String | - |토큰 ID |
| keypairName | Query | String | O | 조회할 키페어 이름. 기재하지 않을 경우 모든 키페어 정보를 조회합니다. |

#### Request Body
이 API는 request body를 필요로 하지 않습니다.

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
| Keypair Name | Body | String | 키페어 이름 |
| Public Key Value | Body | String | 키페어의 공개 키 값 |
| Fingerprint Value | Body | String | Fingerprint 값 |
| Created At | Body | DateTime | 키페어 생성 시간. "Keypair Name"을 지정한 단건 조회 시에만 노출됩니다. |

### 키페어 생성과 업로드
키페어를 생성하거나 사용자가 생성한 키페어를 업로드 합니다.

#### Method, URL
```
POST /v1.0/appkeys/{appkey}/keypairs
X-Auth-Token: {tokenId}
Content-Type: application/json;charset=UTF-8
```

|  Name | In | Type | Optional | Description |
|--|--|--|--|--|
| tokenId | Header | String | - | 토큰 ID |

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
| Keypair Name | Body | String | - | 키페어 이름 |
| Public Key Value | Body | String | O | 업로드할 공개 키. 생략 시 새로운 키페어가 만들어지며, 만들어진 키페어의 개인 키가 Response로 전달됩니다. |

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
| Keypair Name | Body | String | 키페어 이름 |
| Public Key Value | Body | String | 키페어의 공개 키 |
| Private Key Value | Body | String | 키페어의 개인 키. 키페어 업로드인 경우 생략됩니다. |
| Fingerprint value | Body | String | Fingerprint 값 |

### 키페어 삭제
지정한 키페어를 삭제합니다.
#### Method, URL
```
DELETE /v1.0/appkeys/{appkey}/keypairs?name={keypairName}
X-Auth-Token: {tokenId}
```

|  Name | In | Type | Optional | Description |
|--|--|--|--|--|
| tokenId | Header | String | - | 토큰 ID |
| keypairName | Query | String | - | 삭제할 키페어 이름 |

#### Request Body
이 API는 Request Body를 필요로 하지 않습니다.

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
