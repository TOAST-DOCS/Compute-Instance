## Compute > Instance > API 가이드

API는 현재 한국 리전에서만 사용할 수 있습니다.

TOAST Compute Instance 서비스는 다음 종류의 API를 제공합니다.

* [가용성 영역 API](#api_2)
* [인스턴스 API](#api_3)
* [인스턴스 추가 기능 API](#api_4)
* [인스턴스 타입 API](#api_5)
* [키페어 API](#api_6)

## 사전 준비

인스턴스 API를 사용하려면 앱키와 토큰이 필요합니다. [API Endpoint URL](#api-endpoint-url)과 [토큰 API](#api)를 이용하여 앱키와 토큰을 준비합니다. 앱키는 API Endpoint URL에, 토큰은 Request Body에 포함하여 사용합니다.

### API Endpoint URL

모든 인스턴스, 이미지, 네트워크(VPC), 블록 스토리지 API는 다음 URL을 접두사로 사용해야 합니다.

	https://api-compute.cloud.toast.com/compute

API를 요청할 때는 항상 앱키를 포함해 요청해야 합니다. 앱키는 아래와 같이 발급받을 수 있습니다.

1. TOAST 콘솔 **Compute** 페이지 위쪽에서 **URL & Appkey**를 클릭합니다.
2. **URL & Appkey** 대화 상자에서 **Appkey** 값을 복사해 사용합니다.

예를 들어, 토큰 발급 URL은 다음과 같습니다.

	POST https://api-compute.cloud.toast.com/compute/v1.0/appkeys/{appkey}/tokens

인스턴스, 이미지, 네트워크, 블록 스토리지 API 문서에서는 간결하고 보기 쉽게 표기하기 위하여 API URL 접두사를 생략했습니다.

### API Response

#### Response HTTP Status Code

모든 API 요청에 200 OK로 응답하며, JSON 형태의 Response Body를 포함합니다.

#### Response Body

Response Body에는 "header" 정보가 기본으로 포함되어 있으며, 이를 통해 자세한 응답 결과를 확인할 수 있습니다. API에 따라 "header" 외 추가적인 정보가 포함될 수 있습니다.

```json
{
    "header" : {
        "isSuccessful" : true,
        "resultCode": 0,
        "resultMessage" : "SUCCESS"
    }
}
```

API 호출이 실패하면 `isSuccessful`이 `false`가 되며, 오류 코드가 `resultCode`에 표시됩니다. 자세한 오류 코드는 [오류 코드](/Compute/Instance/ko/error-code/)를 참고합니다.

### 토큰 API

토큰은 API 사용을 위해 필수로 발급받아야 하는 인증키이며, 모든 API는 Request에 **X-Auth-Token** Header를 추가하여 요청해야 합니다.

#### API 비밀번호 설정

API 비밀번호는 **Compute > Instance** 서비스 페이지의 **API Endpoint 설정** 버튼을 클릭해 설정할 수 있습니다.

1. **API Endpoint 설정** 버튼을 클릭합니다.
2. **API Endpoint 설정** 아래 **API 비밀번호 설정**에 토큰 발급 시 사용할 비밀번호를 입력합니다.
3. 비밀번호를 입력한 후 **저장** 버튼을 클릭합니다.

#### Token 발급

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
| TOAST ID | Body | String | - | TOAST 계정 ID(Email) 입력 |
| API Password | Body | String | - | **API Endpoint 설정**에서 저장한 비밀번호 |

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
| Token ID | Body | String | API 요청 시 HTTP 헤더(X-Auth-Token)에 써야 할 UUID |
| Issued at | Body | String | 토큰 발급 시간. yyyy-mm-ddTHH:MM:ssZ의 형태. 예) 2017-05-16T02:17:50.166563 |
| Expires | Body | String | 발급한 Token의 만료 시간. yyyy-mm-ddTHH:MM:ssZ의 형태. 예) 2017-05-16T03:17:50Z |
| User ID | Body | String | 토큰을 발급받은 사용자의 UUID |
| Role name | Body | String | 토큰을 발급받은 사용자에게 부여된 Role |

#### Token 정보 조회
##### Method, URL
```
GET /v1.0/appkeys/{appkey}/tokens?id={tokenId}
```

|  Name | In | Type | Optional |Description |
|--|--|--|--|--|
| tokenId | Query | String | - | 조회할 Token ID |

##### Request Body
이 API는 Request Body가 필요 없습니다.

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
| Token ID | Body | String | API 요청 시 HTTP 헤더(X-Auth-Token)에 써야 할 토큰 UUID |
| Issued at | Body | String | 토큰 발급 시간. yyyy-mm-ddTHH:MM:ssZ의 형태. 예) 2017-05-16T02:17:50.166563 |
| Expires | Body | String | 발급한 Token의 만료 시간. yyyy-mm-ddTHH:MM:ssZ의 형태. 예) 2017-05-16T03:17:50Z |
| User ID | Body | String | 토큰을 발급받은 사용자의 UUID |
| Role name | Body | String | 토큰을 발급받은 사용자에게 부여된 Role |


## 가용성 영역 API

### 가용성 영역 조회
인스턴스, 블록 스토리지를 생성할 수 있는 가용성 영역 정보를 조회합니다.

#### Method, URL
```
GET /v1.0/appkeys/{appkey}/availability-zones
X-Auth-Token: {tokenId}
```

|  Name | In | Type | Optional | Description |
|--|--|--|--|--|
| tokenId | Header | String | - | 토큰 ID |

#### Request Body
이 API는 Request Body가 필요 없습니다.

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
| Zone Name | Body | String | 가용성 영역 이름 |
| Available | Body | Boolean | 가용성 영역 가용 여부 |

## 인스턴스 API
인스턴스 생성, 삭제, 정보 조회 및 블록 스토리지 연결 관리 기능을 제공합니다.

### 인스턴스 상태
인스턴스는 생성, 변경, 삭제, 운영 중 다음과 같은 상태를 갖습니다.
![[그림 1] 인스턴스 Status Diagram](http://static.toastoven.net/prod_infrastructure/compute/developersguide/img_001.png)


| 상태 | 설명 |
| --- | --- |
| BUILD | 인스턴스 생성 중 |
| POWERING_ON | 인스턴스 부팅 중 |
| STOP | 인스턴스 종료 상태 |
| ACTIVE | 인스턴스 구동 중 |
| POWERING_OFF | 인스턴스 종료 중 |
| REBOOTING | 인스턴스 리부팅 중 |
| DELETING | 인스턴스 삭제 중 |
| RESIZING | 인스턴스 타입(flavor) 변경 작업 중 |
| MIGRATING | 인스턴스 마이그레이션 작업 중 |
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
이 API는 Request Body가 필요 없습니다.

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
| Instance Name | Body | String | 인스턴스 이름 |
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
| tokenId | Header | String | - | 토큰 ID. |
| instanceId | Query | String | O | 조회할 인스턴스의 ID. 생략하면 모든 인스턴스들의 상세 정보를 조회합니다. |

#### Request Body
이 API는 Request Body가 필요 없습니다.

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

|  Name | In | Type | Description |
|--|--|--|--|
| MAC Address | Body | String | NIC의 MAC 주소 |
| IP Address | Body | String | NIC의 IP 주소 |
| version | Body | Integer | IP 버전(IPv4만 지원) |
| Floating IP Address | Body | String | NIC에 할당된 플로팅 IP 주소 |
| Zone Name | Body | String | 가용성 영역 이름 |
| Flavor ID | Body | String | 인스턴스 타입 ID |
| Flavor Name | Body | String | 인스턴스 타입 이름 |
| Flavor CPU | Body | Integer | CPU 개수 |
| Flavor RAM | Body | Integer | RAM 크기(MB) |
| Status | Body | String | 인스턴스의 상태 |
| Instance ID | Body | String | 인스턴스 ID |
| Instance Name | Body | String | 인스턴스 이름 |
| Image ID | Body | String | 인스턴스에 설치된 이미지 ID |
| metadata | Body | Object | 인스턴스에 설정할 사용자 메타데이터로, "key": "value" 형태로 저장 |
| PEM Key Name | Body | String | 인스턴스에 등록할 키페어 이름 |
| Root Volume Size | Body | Integer | 인스턴스 기본 블록 스토리지 장치 크기(GB) |
| Root Volume Type | Body | String | 인스턴스 기본 블록 스토리지 장치 종류. "General HDD" 또는 "General SSD" 중 하나. |
| Attached Volume ID | Body | String | 추가 블록 스토리지 ID |
| Attached Volume Name | Body | String | 추가 블록 스토리지 이름 |
| Attached Volume Size | Body | Integer | 추가 블록 스토리지 크기 (GB) |
| Attached Volume Type | Body | String | 추가 블록 스토리지 장치 종류. "General HDD" 또는 "General SSD" 중 하나. |
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
              "id": "{Network ID}",
              "subnetId": "{Subnet ID}"
        	}
        ],
        "availabilityZone": "{Availability Zone}",
        "keyName": "{Key Name}",
        "count": "{Count}",
        "volume": {
           "size": "{Volume Size}",
           "type": "{Volume Type}",
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
| Instance Name | Body | String | - | 인스턴스 이름 (리눅스의 경우 최대 20자, Windows의 경우 최대 12자, 영문자와 숫자, '-', '.' 만 가능) |
| Image ID | Body | String | - | 인스턴스에 설치할 이미지 ID |
| Flavor ID | Body | String | - | 인스턴스 사양 ID |
| Network ID | Body | String | O | 인스턴스가 연결될 네트워크 ID |
| Subnet ID | Body | String | O | 인스턴스가 연결될 서브넷 ID<br>네트워크 ID 또는 서브넷 ID 중 하나는 반드시 지정되어야 합니다. |
| Availability Zone | Body | String | - | 인스턴스가 생성될 가용성 영역 이름 |
| Key Name | Body | String | - | 인스턴스에 등록할 키페어 이름 |
| Count | Body | Integer | O | 동시 생성할 인스턴스의 개수, 최대 10개로 제한, 1~10 범위. 생략 시 1대 생성  |
| Volume Size | Body | Integer | O | 인스턴스의 기본 블록 스토리지 크기, 생성 가능한 크기는 [콘솔 가이드](/Compute/Instance/ko/console-guide/#_5)를 참조 |
| Volume Type | Body | String | O | 인스턴스의 기본 블록 스토리지 종류, "General HDD" 또는 "General SSD" 중 하나. |
| Security Group Name | Body | String | - | 인스턴스에 등록할 보안 그룹 이름 |

> [주의]
> * `Volume Size`, `Volume Type` 파라미터는 c2, m2, r2, t2 타입에서만 적용됩니다.
> 	* 이 파라미터는 범용 블록 스토리지가 아닌 로컬 디스크를 사용하는 u2 계열 타입에서는 사용할 수 없습니다.
> * `Volume Size`는 **사용할 이미지의 "minDisk" 값 ~ 1000의 범위 내에서 10 단위**로 설정되어야 합니다.
> * `Volume Type`은 `null`로 지정할 수 있으며, 이 경우 "General HDD"로 지정됩니다.


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
| Instance Name | body | String | 인스턴스 이름 |
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
| Volume ID | body | String | - | 인스턴스에 연결할 블록 스토리지 ID |

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
| Device ID | body | String | 인스턴스에 등록된 장치 이름. 예) "/dev/vdc" |
| Attachement ID | body | String | 연결 ID.|
| Volume ID | body | String | 블록 스토리지 ID. 연결 해제 시 필요. |

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
이 API는 Request Body가 필요 없습니다.

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
다음과 같은 인스턴스 제어 및 부가 기능을 제공합니다.

- 인스턴스 시작, 정지, 재시작
- 인스턴스 타입 변경(resize)
- 인스턴스 이미지 생성
- 플로팅 IP 연결, 해제
- 보안 그룹 등록, 해제

### 공통
모든 인스턴스 추가 기능 API는 동일한 Method, URL로 호출하며, Request Body로 각 추가 기능을 구분합니다.
#### Method, URL
```
POST /v1.0/appkeys/{appkey}/instances/{instanceId}/action
X-Auth-Token: {tokenId}
Content-Type: application/json;charset=UTF-8
```

|  Name | In | Type | Optional | Description |
|--|--|--|--|--|
| tokenId | Header | String| - | 토큰 ID |
| instanceId | Path | String | - | 추가 기능을 수행할 인스턴스 ID |

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
| Action Name | Body | String | - | 인스턴스에서 실행할 추가 기능 |
| parameters | Body | Object| O | 추가 기능 수행에 필요한 파라미터. 추가 기능에 따라 필요한 값을 기재합니다. 일부 추가 기능은 파라미터 없이 동작합니다. |

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
동작 중(ACTIVE) 또는 오류(ERROR) 상태의 인스턴스를 정지합니다.
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

- **SOFT**: 정상 종료(graceful shutdown) 후 인스턴스를 재시작합니다.
- **HARD**: 강제 종료(shutdown) 후 인스턴스를 재시작합니다.

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
| Reboot Type | body | String | - | 리부팅 방식. `HARD` 또는 `SOFT`. |

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

### 인스턴스 타입 변경
인스턴스의 타입을 변경합니다.
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
|  Flavor ID | body | String | - | 변경할 인스턴스 타입 ID |

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
지정한 인스턴스로부터 이미지를 생성합니다. 생성된 이미지는 [이미지 API](/Compute/Image/ko/api-guide/)로 조회할 수 있습니다.

이미지 생성 대상이 되는 인스턴스는 STOP 상태여야 합니다.

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
| Image Name | body | String | - | 생성할 이미지 이름 |

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
인스턴스에 보안 그룹을 추가합니다.

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

## 인스턴스 타입 API
### 인스턴스 타입 목록 조회
인스턴스 타입의 목록 및 상세 정보를 조회합니다.

#### Method, URL
```
GET /v1.0/appkeys/{appkey}/flavors
X-Auth-Token: {tokenID}
```

|  Name | In | Type | Optional | Description |
|--|--|--|--|--|
| tokenId | Header | String| - | 토큰 ID |

#### Request Body
이 API는 Request Body가 필요 없습니다.

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
| Disabled | Body | Boolean | 인스턴스 타입 비활성화 여부. |
| Ephermeral | Body | Integer | 임시 디스크 크기(GB). |
| Type | Body | String | 인스턴스 타입 최적화 특성에 따라 구분되는 Type값.<br>"general, "compute", "memory" 중의 하나. |
| Volume Size | Body | Integer | 인스턴스 생성 시 기본 디스크 장치로 만들어지는 디스크 크기(GB).<br>기본 디스크가 고정된 크기로 만들어지는 u2 타입의 경우에만 이 값이 전달됩니다. |
| Max Volume Size | Body | Integer | 기본 디스크 장치로 만들 수 있는 최대 디스크 크기(GB). |
| Flavor ID | Body | String | 인스턴스 타입 ID. |
| Flavor Name | Body | String | 인스턴스 타입 이름. |
| Is Public | Body | Boolean | 공용 인스턴스 타입 여부. |
| RAM | Body | Integer | 인스턴스 타입이 갖는 RAM 총량(MB). |
| VCPUs | Body | Integer | 인스턴스에 할당되는 가상 CPU 코어 개수. |

## 키페어 API
인스턴스 접근에 필요한 키페어를 생성, 삭제, 조회하는 기능을 제공합니다.
### 키페어 조회
키페어를 조회합니다.
#### Method, URL
```
GET /v1.0/appkeys/{appkey}/keypairs?name={keypairName}
X-Auth-Token: {tokenId}
```

|  Name | In | Type | Optional | Description |
|--|--|--|--|--|
| tokenId | Header | String | - |토큰 ID. |
| keypairName | Query | String | O | 조회할 키페어 이름. 없으면 모든 키페어 정보를 조회합니다. |

#### Request Body
이 API는 Request Body가 필요 없습니다.

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
| Keypair Name | Body | String | 키페어 이름. |
| Public Key Value | Body | String | 키페어의 공개 키 값. |
| Fingerprint Value | Body | String | 핑거프린트(fingerprint) 값. |
| Created At | Body | DateTime | 키페어 생성 시간. "Keypair Name"을 지정한 단건 조회 시에만 노출됩니다. |

### 키페어 생성과 업로드
키페어를 생성하거나 사용자가 직접 생성한 키페어를 업로드합니다.

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
| Public Key Value | Body | String | O | 업로드할 공개 키. 생략 시 새로운 키페어가 만들어지며, 만들어진 키페어의 개인 키가 Response에 함께 전달됩니다. |

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
| Private Key Value | Body | String | 키페어의 개인 키. 키페어 업로드(Request에 "publickey" 항목을 포함)인 경우 생략됩니다. |
| Fingerprint value | Body | String | Fingerprint 값 |

생성된 Private Key Value는 전문을 .pem 파일로 저장 후 해당 Keypair를 사용하도록 설정된 인스턴스에 접근 시 사용할 수 있습니다. **생성된 Private Key Value는 다시 조회할 수 없으므로** 분실 또는 삭제되지 않도록 잘 보관해야 하며, 유출 방지를 위해 가급적 보조 저장매체(USB메모리)에 관리하는 것이 좋습니다.

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
이 API는 Request Body가 필요 없습니다.

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
