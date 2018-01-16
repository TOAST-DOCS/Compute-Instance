## Compute > API 가이드

TOAST Compute 서비스에서는 다음 종류의 API를 제공합니다. 

* Token API
* 가용성 영역 API
* 인스턴스 API
* 인스턴스 사양 API
* 키페어 API


## 사전 준비

??? 배껴올 것!


## 기본 정보
### API Endpoint URL
```
https://api-compute.cloud.toast.com/compute/v1.0/appkeys/{appkey}
```

| Name | In | Type | Description |
| -- | -- | -- | -- |
| appkey | Path Variable | String | 상품 이용시 발급받은 앱키 |

### API Response
#### Response HTTP Status Code
모든 API 요청에 대해 200 OK로 응답하며, JSON 형태의 Response Body를 포함합니다.

#### Response Body
Response Body에는 "header" 정보가 기본으로 포함되어 있으며, 이를 통해 자세한 응답 결과를 확인할 수 있습니다.
API에 따라 "header" 외 추가적인 정보가 포함될 수 있습니다.

```json
{
    "header" : {
        "isSuccessful" : true,
        "resultCode": 0,
        "resultMessage" : "SUCCESS"
    }
}
```

Response body "header"의 `resultCode`의 의미는 아래와 같이 정의되어 있습니다.

| isSuccessful | resultCode | resultMessage | Description |
| --- | --- | --- | --- |
| true | 0 | SUCCESS | 처리 성공. API에 따라 "header" 외 추가 정보 포함 |
| false  | -1 | FAIL [: detail description] | 인증 모듈 연동 실패 또는 요청을 수행할 수 없는 상태인 경우. detail description 내용에 따라 조치 후 재시도 가능 |
| false | -2 | UNKNOWN EXCEPTION | 예기치 못한 오류 발생. 담당자 문의 필요 |
| false | -3 | Permission denied [: detail description] | 권한이 없는 사용자, AppKey, 또는 토큰 ID에 의한 요청. detail description 내용 참조 |
| false | -4 | Invalid parameters [: detail description] | API 호출 시 Request URL 또는 Body에 필요한 값이 없거나, 잘못된 형식의 값이 입력된 경우. detail description 내용에 따라 조치 후 재시도 가능 |
| false | -5 | Nonexistent [: detail description] | 존재하지 않는 자원에 접근한 경우. detail description 내용 참조 |
| false | -6 | Failed to query internal db [: detail description] | 데이터베이스 질의가 실패한 경우. 담당자 문의 필요 |
| false | -7 | Not supported [: detail description] | 지원하지 않는 기능 요청. detail description 내용 참조 |
| false | -8 | JSON parsing error [: detail description] | Request Body의 JSON 파싱 실패. Request Body 확인 필요 |
| false | 10400~10499| 인스턴스 API 관련 에러 메시지 | resultCode의 뒤 세자리는 HTTP Status Code이며, resultMessage 내용에 따라 조치 후 재시도 가능 |
| false | 11400~11499| API 관련 에러 메시지 | resultCode의 뒤 세자리는 HTTP Status Code이며, resultMessage 내용에 따라 조치 후 재시도 가능 |
| false | 12400~12499| 가용성 영역 API 관련 에러 메시지 | resultCode의 뒤 세자리는 HTTP Status Code이며, resultMessage 내용에 따라 조치 후 재시도 가능 |
| false | 13400~13499| 키페어 API 관련 에러 메시지 | resultCode의 뒤 세자리는 HTTP Status Code이며, resultMessage 내용에 따라 조치 후 재시도 가능 |
| false | 20001 | No available IP | 할당 가능한 IP가 없음. 담당자 문의 필요 |
| false | 20004 | No accessible network | 네트워크 조회 실패. 잘못된 네트워크 ID로 조회, 또는 접근 가능한 네트워크가 없음. |
| false | 20400~20499| 네트워크 API 관련 에러 메시지 | resultCode의 뒤 세자리는 HTTP Status Code이며, resultMessage 내용에 따라 조치 후 재시도 가능 |
| false | 21400~21499| 서브넷 API 관련 에러 메시지 | resultCode의 뒤 세자리는 HTTP Status Code이며, resultMessage 내용에 따라 조치 후 재시도 가능 |
| false | 22400~22499| 플로팅 IP API 관련 에러 메시지 | resultCode의 뒤 세자리는 HTTP Status Code이며, resultMessage 내용에 따라 조치 후 재시도 가능 |
| false | 23400~23499| 로드 밸런서 API 관련 에러 메시지 | resultCode의 뒤 세자리는 HTTP Status Code이며, resultMessage 내용에 따라 조치 후 재시도 가능 |
| false | 24400~24499| 보안 그룹 API 관련 에러 메시지 | resultCode의 뒤 세자리는 HTTP Status Code이며, resultMessage 내용에 따라 조치 후 재시도 가능 |
| false | 30400~30499| 이미지 API 관련 에러 메시지 | resultCode의 뒤 세자리는 HTTP Status Code이며, resultMessage 내용에 따라 조치 후 재시도 가능 |
| false | 40400~40499| 볼륨 API 관련 에러 메시지 | resultCode의 뒤 세자리는 HTTP Status Code이며, resultMessage 내용에 따라 조치 후 재시도 가능 |
| false | 50400~50499| 토큰 API 관련 에러 메시지 | resultCode의 뒤 세자리는 HTTP Status Code이며, resultMessage 내용에 따라 조치 후 재시도 가능 |

## 토큰 (업데이트필요)
**토큰** 은 Compute 서비스의 RESTful API 사용을 위해 발급받아야 하는 인증키이며, 이후 모든 API 요청 시 Request에 **X-Auth-Token** Header로 입력하여 요청해야 합니다.
### 토큰 API
#### 토큰 발급
##### Method, URL
```
POST /v1.0/appkeys/{appkey}/tokens
Content-Type: application/json;charset=UTF-8
```


##### Request Body
```json
{
	"auth" : {
    	"username" : "{User Name}",
        "password" : "{API Password}"
    }
}
```

| Name | In | Type | Optional | Description |
| -- | -- | -- | -- | -- |
| User Name | Body | String | - | TOAST 사용자 계정 ID |
| API Password | Body | String | - | API 패스워드 |

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
| Token ID | Body | String | API 요청 시 HTTP 헤더(X-Auth-Token) 에 기재해야 하는 UUID |
| Issued at | Body | String | 토큰 발급 시간. yyyy-mm-ddTHH:MM:ssZ의 형태. 예) 2017-05-16T02:17:50.166563 |
| Expires | Body | String | 발급한 Token의 만료 시간. yyyy-mm-ddTHH:MM:ssZ의 형태. 예) 2017-05-16T03:17:50Z |
| User ID | Body | String | 토큰을 발급받은 사용자의 UUID |
| Role name | Body | String | 토큰을 발급받은 사용자에게 부여된 Role |

#### 토큰 정보 조회
##### Method, URL
```
GET /v1.0/appkeys/{appkey}/tokens?id={tokenId}
```

|  Name | In | Type | Optional |Description |
|--|--|--|--|--|
| tokenId | Query | String | - | 조회할 토큰 ID |

##### Request Body
Request Body 없이 API 요청합니다.

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
| Token ID | Body | String | API 요청 시 HTTP 헤더(X-Auth-Token) 에 기재해야 하는 UUID |
| Issued at | Body | String | 토큰 발급 시간. yyyy-mm-ddTHH:MM:ssZ의 형태. 예) 2017-05-16T02:17:50.166563 |
| Expires | Body | String | 발급한 Token의 만료 시간. yyyy-mm-ddTHH:MM:ssZ의 형태. 예) 2017-05-16T03:17:50Z |
| User ID | Body | String | 토큰을 발급받은 사용자의 UUID | 
| Role name | Body | String | 토큰을 발급받은 사용자에게 부여된 Role |

## 가용성 영역 (AZ, Availability Zone)
### 가용성 영역 API
#### 가용성 영역 조회
인스턴스, 블록 스토리지를 생성할 수 있는 AZ 정보를 조회합니다.

##### Method, URL
```
GET /v1.0/appkeys/{appkey}/availability-zones
X-Auth-Token: {tokenId}
```

|  Name | In | Type | Optional | Description |
|--|--|--|--|--|
| tokenId | Header | String | - | 토큰 ID |

##### Request Body
이 API는 request body를 필요로 하지 않습니다.

##### Response Body
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

## 인스턴스
### 인스턴스 API
인스턴스 생성, 삭제, 정보 조회 및 블록 스토리지 연결관리 기능을 제공합니다.

#### 인스턴스 상태
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

#### 인스턴스 정보 간략 조회
생성되어 있는 인스턴스의 간략한 정보를 조회합니다.
##### Method, URL
```
GET /v1.0/appkeys/{appkey}/instances?id={instanceId}
X-Auth-Token: {tokenId}
```

|  Name | In | Type | Optional |Description |
|--|--|--|--|--|
| tokenId | Header | String | - | 토큰 ID |
| instanceId | Query | String | O | 조회할 인스턴스 식별자. 기재하지 않을 경우 모든 인스턴스들의 간략 정보를 조회합니다. |

##### Request Body
이 API는 Request Body를 필요로 하지 않습니다.

##### Response Body
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
| Instance ID | Body | String | 인스턴스 식별자 |
| Instance Name | Body | String | 인스턴스 이름 (리눅스의 경우 최대 255자, 윈도우의 경우 최대 12자) |
| Instance Status | Body | String | 인스턴스의 상태 |

#### 인스턴스 상세 조회
인스턴스의 상세한 정보를 조회합니다.
##### Method, URL
```
GET /v1.0/appkeys/{appkey}/instances-detail?id={instanceId}
X-Auth-Token: {tokenId}
```

|  Name | In | Type | Optional |Description |
|--|--|--|--|--|
| tokenId | Header | String | - | 토큰 ID |
| instanceId | Query | String | O | 조회할 인스턴스의 식별자. 생략 시 모든 인스턴스들의 상세 정보를 조회합니다. |

##### Request Body
이 API는 Request Body를 필요로 하지 않습니다.

##### Response Body
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
| version | Body | Integer | IP 버전. TOAST에서는 IP v4만 지원 |
| Floating IP Address | Body | String | NIC에 할당된 플로팅 IP 주소 |
| Zone Name | Body | String | 가용성 영역 |
| Flavor ID | Body | String | 인스턴스 사양 식별자 |
| Flavor Name | Body | String | 인스턴스 사양 이름 |
| Flavor CPU | Body | Integer | CPU 개수 |
| Flavor RAM | Body | Integer | RAM 크기, MB 단위 |
| Status | Body | String | 인스턴스의 상태 |
| Instance ID | Body | String | 인스턴스 ID |
| Instance Name | Body | String | 인스턴스 이름 (리눅스의 경우 최대 255자, 윈도우의 경우 최대 12자) |
| Image ID | Body | String | 인스턴스에 설치된 이미지 ID |
| metadata | Body | Object | 인스턴스에 설정할 사용자 메타데이터, "key": "value" 형태로 저장 |
| PEM Key Name | Body | String | 인스턴스에 등록할 키페어 이름 |
| Root Volume Size | Body | Integer | 인스턴스 기본 디스크 장치 크기, GB 단위 |
| Attached Volume ID | Body | String | 추가 블록 스토리지 식별자 |
| Attached Volume Name | Body | String | 추가 블록 스토리지 이름 |
| Attached Volume Size | Body | Integer | 추가 블록 스토리지 크기, GB 단위 |
| Security Group Name | Body | String | 인스턴스에 등록된 보안 그룹의 이름 |
| Launched Time | Body | String | 인스턴스 최근 부팅 시각. yyyy-mm-ddTHH:MM:ssZ의 형태. 예) 2017-05-16T02:17:50.166563 |
| Created Time | Body | String | 인스턴스 생성 시각. yyyy-mm-ddTHH:MM:ssZ의 형태. 예) 2017-05-16T02:17:50.166563 |
| Updated Time | Body | String | 인스턴스 수정 시각. yyyy-mm-ddTHH:MM:ssZ의 형태. 예) 2017-05-16T02:17:50.166563 |

#### 인스턴스 생성
새로운 인스턴스를 생성합니다.

##### Method, URL
```
POST /v1.0/appkeys/{appkey}/instances
X-Auth-Token: {tokenId}
Content-Type: application/json;charset=UTF-8
```

|  Name | In | Type | Optional | Description |
|--|--|--|--|--|
| tokenId | Header | String | - | 토큰 ID |

##### Request Body
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
| Image ID | Body | String | - | 인스턴스에 설치할 이미지 식별자. [Image API](/ko/Compute/Image/ko/api-guide/) 참조 |
| Flavor ID | Body | String | - | 인스턴스 사양 식별자. 인스턴스 사양 API 참조|
| Network ID | Body | String | - | 인스턴스가 연결될 네트워크 식별자. 네트워크 API 참조|
| Availability Zone | Body | String | - | 인스턴스가 생성될 가용성 영역 이름. 가용성 API 참조 |
| Key Name | Body | String | - | 인스턴스에 등록할 키페어 이름. 키페어 API 참조|
| Count | Body | Integer | - | 동시 생성할 인스턴스의 개수, 최대 10개로 제한 |
| Volume Size | Body | Integer | - | 인스턴스의 기본 디스크 크기, 생성 가능한 크기는 [개요](./overview)를 참조. |
| Security Group Name | Body | String | - | 인스턴스에 등록할 보안 그룹 이름 |

##### Response Body
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
| Instance ID | body | String |생성된 인스턴스 식별자 |
| Instance Name | body | String | 인스턴스 이름 (리눅스의 경우 최대 255자, 윈도우의 경우 최대 12자) |
| Instance Status | Body | String | 인스턴스의 상태 |

#### 인스턴스 삭제
특정 인스턴스를 삭제합니다.
##### Method, URL
```
DELETE /v1.0/appkeys/{appkey}/instances?id={instanceId}
X-Auth-Token: {tokenId}
```

|  Name | In | Type | Optional | Description |
|--|--|--|--|--|
| tokenId | Header | String | - | 토큰 ID |
| instanceId | Query | String | - | 삭제할 인스턴스 식별자 |

#### 블록 스토리지 연결
인스턴스에 블록 스토리지를 연결합니다.

##### Method, URL
```
POST /v1.0/appkeys/{appkey}/instances/{instanceId}/attachments
X-Auth-Token: {tokenId}
Content-Type: application/json;charset=UTF-8
```

|  Name | In | Type | Optional | Description |
|--|--|--|--|--|
| tokenId | Header | String | - | 토큰 ID |
| instanceId | Path | String | - | 블록 스토리지를 연결할 인스턴스 ID |

##### Request Body
```json
{
    "attachment":{
        "volumeId":"{Volume ID}"
    }
}
```

|  Name | In | Type | Optional | Description |
|--|--|--|--|--|
| Volume ID | body | String | - | 인스턴스에 연결할 블록 스토리지 식별자. |

##### Response Body

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
| Attachement ID | body | String | Attachment 식별자 |
| Volume ID | body | String | 블록 스토리지 식별자, 연결 해제 시 필요 |

#### 블록 스토리지 연결 해제
인스턴스에 연결되어 있는 블록 스토리지 연결을 해제합니다.

##### Method, URL
```
DELETE /v1.0/appkeys/{appkey}/instances/{instanceId}/attachments?volumeId={volumeId}
X-Auth-Token: {tokenId}
```

|  Name | In | Type | Optional | Description |
|--|--|--|--|--|
| tokenId | Header | String| - | 토큰 ID |
| instanceId | Path | String | - | 인스턴스 식별자 |
| volumeId | Path | String | - | 블록 스토리지 식별자 |

##### Request body
이 Request는 Body를 필요로 하지 않습니다.

##### Response Body

```json
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    }
}
```

### 인스턴스 추가기능 API
다음과 같은 인스턴스 제어 및 부가기능을 제공합니다.

- 인스턴스 시작/정지/재시작
- 인스턴스 사양 변경(Resize)
- 인스턴스 이미지 생성
- 플로팅 IP 연결/해제
- 보안 그룹 등록/해제

#### 공통
모든 인스턴스 Action API는 동일한 Method, URL로 호출하며, Request Body로 각 Action을 구분합니다.
##### Method, URL
```
POST /v1.0/appkeys/{appkey}/instances/{instanceId}/action
X-Auth-Token: {tokenId}
Content-Type: application/json;charset=UTF-8
```

|  Name | In | Type | Optional | Description |
|--|--|--|--|--|
| tokenId | Header | String| - | 토큰 ID |
| instanceId | Path | String | - | Action을 수행할 인스턴스 식별자 |

##### Request Body Template
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
| parameters | Body | Object| O | Action 수행에 필요한 파라미터. Action에 따라 필요한 값을 기재하거나 없을 수 있습니다. |

#### 인스턴스 시작
정지(STOP) 상태의 인스턴스를 시작합니다.
##### Request Body
```json
{
    "action" : "start"
}
```

##### Response Body 
```json
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    }
}
```

#### 인스턴스 정지
동작중(ACTIVE) 또는 오류(ERROR) 상태의 인스턴스를 정지합니다.
##### Request Body
```json
{
    "action" : "stop"
}
```

##### Response Body

```json
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    }
}
```

#### 인스턴스 리부팅
인스턴스를 리부팅합니다. 다음과 같은 리부팅 방식을 지정할 수 있습니다.

 - SOFT - Graceful Shutdown 수행 후 인스턴스를 재시작합니다.
 - HARD - 강제 Shutdown 수행 후 인스턴스를 재시작합니다.

##### Request Body

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
| Reboot Type | body | String | - | Reboot 타입. "HARD" or "SOFT" |

##### Response Body
```json
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    }
}
```

#### 인스턴스 사양 변경
인스턴스의 사양을 변경합니다.
##### Request Body
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
|  Flavor ID | body | String | - | 변경할 인스턴스 사양 식별자. 인스턴스 사양 API 참조 |

##### Response Body
```json
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    }
}
```

#### 이미지 생성
지정한 인스턴스로 부터 이미지를 생성합니다. 생성된 이미지는 [이미지 API](/ko/Compute/Image/ko/api-guide/)로 조회할 수 있습니다.
##### Request Body
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

##### Response Body
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
| Created Image ID | body | String | 생성된 이미지 식별자 |
| Created Image Name | body | String | 생성된 이미지 이름 |

#### 플로팅 IP 연결
플로팅 IP를 인스턴스에 연결합니다.

##### Request Body
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

##### Response Body

```json
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    }
}
```

#### 플로팅 IP 연결 해제
인스턴스에 연결되어 있는 플로팅 IP를 연결 해제합니다.

##### Request Body

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

##### Response Body

```json
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    }
}
```

#### 보안 그룹 등록
인스턴스에 보안 그룹을 추가 등록합니다.

##### Request Body
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

##### Response Body

```json
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    }
}
```

#### 보안 그룹 해제
인스턴스에 등록되어 있는 보안 그룹을 제거합니다.

##### Request Body
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

##### Response Body
```json
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    }
}
```

### 인스턴스 사양 API
#### 인스턴스 사양 목록 조회
인스턴스 사양의 목록 및 상세 정보를 조회합니다.

##### Method, URL
```
GET /v1.0/appkeys/{appkey}/flavors
X-Auth-Token: {tokenID}
```

|  Name | In | Type | Optional | Description |
|--|--|--|--|--|
| tokenId | Header | String| - | 토큰 ID |

##### Request Body
이 API는 Request Body를 필요로 하지 않습니다.

##### Response Body
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
| Flavor ID | Body | String | 인스턴스 사양 식별자 |
| Flavor Name | Body | String | 인스턴스 사양 이름 |
| Is Public | Body | Boolean | 공용 인스턴스 사양 여부 |
| RAM | Body | Integer | 인스턴스 사양이 갖는 RAM 총량. MB |
| VCPUs | Body | Integer | 인스턴스에 할당되는 가상 CPU 코어 개수 |

### 키페어 API
인스턴스 접근에 필요한 키페어에 대한 생성, 삭제, 조회 기능을 제공합니다.
#### 키페어 조회
키페어를 조회합니다.
##### Method, URL
```
GET /v1.0/appkeys/{appkey}/keypairs?name={keypairName}
X-Auth-Token: {tokenId}
```

|  Name | In | Type | Optional | Description |
|--|--|--|--|--|
| tokenId | Header | String | - |토큰 ID |
| keypairName | Query | String | O | 조회할 키페어 이름. 기재하지 않을 경우 모든 키페어 정보를 조회합니다. |

##### Request Body
이 API는 request body를 필요로 하지 않습니다.

##### Response Body

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
| Public Key Value | Body | String | 키페어의 Public Key 값 |
| Fingerprint Value | Body | String | Fingerprint 값 |
| Created At | Body | DateTime | 키페어 생성 시간. "Keypair Name"을 지정한 단건 조회 시에만 노출됩니다. |

#### 키페어 생성과 업로드
키페어를 생성하거나 사용자가 생성한 키페어를 업로드 합니다.

##### Method, URL
```
POST /v1.0/appkeys/{appkey}/keypairs
X-Auth-Token: {tokenId}
Content-Type: application/json;charset=UTF-8
```

|  Name | In | Type | Optional | Description |
|--|--|--|--|--|
| tokenId | Header | String | - | 토큰 ID |

##### Request Body

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
| Public Key Value | Body | String | O | 업로드할 공개키. 생략 시 새로운 키페어가 만들어지며, 만들어진 키페어의 개인키가 Response로 전달됩니다. |

##### Response Body

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
| Public Key Value | Body | String | 키페어의 공개키 |
| Private Key Value | Body | String | 키페어의 개인키. 키페어 업로드인 경우 생략됩니다. |
| Fingerprint value | Body | String | Fingerprint 값 |

#### 키페어 삭제
지정한 키페어를 삭제합니다.
##### Method, URL
```
DELETE /v1.0/appkeys/{appkey}/keypairs?name={keypairName}
X-Auth-Token: {tokenId}
```

|  Name | In | Type | Optional | Description |
|--|--|--|--|--|
| tokenId | Header | String | - | 토큰 ID |
| keypairName | Query | String | - | 삭제할 키페어 이름 |

##### Request Body
이 API는 Request Body를 필요로 하지 않습니다.

##### Response Body
```json
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    }
}
```
