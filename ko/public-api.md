## Compute API

## API 버전

### 버전 목록 보기

TOAST 기본 인프라 서비스 Compute API에서 지원하는 버전 목록을 확인할 수 있습니다.

```
GET /
```

#### 요청

이 API는 요청 본문을 요구하지 않습니다.

#### 응답

<details><summary>예시</summary>
<p>

```json
{
  "versions": {
    "values": [
      {
        "status": "stable",
        "updated": "2015-03-30T00:00:00Z",
        "media-types": [
          {
            "base": "application/json",
            "type": "application/vnd.openstack.identity-v3+json"
          }
        ],
        "id": "v3.4",
        "links": [
          {
            "href": "https://api-identity.cloud.toast.com/v3/",
            "rel": "self"
          }
        ]
      },
      {
        "status": "stable",
        "updated": "2014-04-17T00:00:00Z",
        "media-types": [
          {
            "base": "application/json",
            "type": "application/vnd.openstack.identity-v2.0+json"
          }
        ],
        "id": "v2.0",
        "links": [
          {
            "href": "https://api-identity.cloud.toast.com/v2.0/",
            "rel": "self"
          },
          {
            "href": "http://docs.openstack.org/",
            "type": "text/html",
            "rel": "describedby"
          }
        ]
      }
    ]
  }
}
```

</p>
</details>

## 인스턴스 타입

인스턴스 타입은 가상 머신의 vCPU, RAM 및 디스크 크기에 대한 정의라고 할 수 있습니다.

### 타입 목록 보기

```
GET /v2/{tenantId}/flavors
X-Auth-Token: {tokenId}
```

#### 요청

이 API는 요청 본문을 요구하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tenantId | URL | String | O | 테넌트 ID |
| tokenId | Header | String | O | 토큰 ID |
| minDisk | Query | Integer | - | 최소 디스크 크기<br>지정한 크기보다 디스크 크기가 큰 타입만 반환 |
| minRam | Query | Integer | - | 최소 RAM 크기<br>지정한 크기보다 RAM 크기가 큰 타입만 반환 |
| limit | Query | Integer | - | 타입 목록 갯수<br>지정된 갯수 만큼의 타입 목록을 반환 |
| marker | Query | UUID | - | 목록의 첫번째 타입 ID<br/>정렬 기준에 따라 `marker`로 지정된 인스턴스부터 <br>`limit` 갯수 만큼의 인스턴스 목록을 반환 |

#### 응답

| 이름 | 종류 | 형식 | 설명 |
|---|---|---|---|
| flavors | Body | Object | 타입 목록 객체 |
| flavors.id | Body | UUID | 타입 ID |
| flavors.links | Body | Object | 타입 경로 객체 |
| flavors.name | Body | String | 타입 이름 |


<details><summary>예시</summary>
<p>

```json
{
  "flavors": [
    {
      "id": "013bea75-8541-4c6f-9abe-a03fee3d74fe",
      "links": [
        {
          "href": "https://kr1-api-compute.cloud.toast.com/v2/6cdebe3eb0094910bc41f1d42ebe4cb7/flavors/013bea75-8541-4c6f-9abe-a03fee3d74fe",
          "rel": "self"
        },
        {
          "href": "https://kr1-api-compute.cloud.toast.com/6cdebe3eb0094910bc41f1d42ebe4cb7/flavors/013bea75-8541-4c6f-9abe-a03fee3d74fe",
          "rel": "bookmark"
        }
      ],
      "name": "x1.c32m256"
    },
    {
      "id": "0f19a344-bc66-4228-8cb1-fb9ca82c54f5",
      "links": [
        {
          "href": "https://kr1-api-compute.cloud.toast.com/v2/6cdebe3eb0094910bc41f1d42ebe4cb7/flavors/0f19a344-bc66-4228-8cb1-fb9ca82c54f5",
          "rel": "self"
        },
        {
          "href": "https://kr1-api-compute.cloud.toast.com/6cdebe3eb0094910bc41f1d42ebe4cb7/flavors/0f19a344-bc66-4228-8cb1-fb9ca82c54f5",
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

### 타입 목록 상세 보기

```
GET /v2/{tenantId}/flavors/detail
X-Auth-Token: {tokenId}
```

#### 요청

이 API는 요청 본문을 요구하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tenantId | URL | String | O | 테넌트 ID |
| tokenId | Header | String | O | 토큰 ID |
| minDisk | Query | Integer | - | 최소 디스크 크기<br>지정한 크기보다 디스크 크기가 큰 타입만 반환 |
| minRam | Query | Integer | - | 최소 RAM 크기<br>지정한 크기보다 RAM 크기가 큰 타입만 반환 |
| limit | Query | Integer | - | 타입 목록 갯수<br>지정된 갯수 만큼의 타입 목록을 반환 |
| marker | Query | UUID | - | 목록의 첫번째 타입 ID<br/>정렬 기준에 따라 `marker`로 지정된 인스턴스부터 <br>`limit` 갯수 만큼의 인스턴스 목록을 반환 |

#### 응답

| 이름 | 종류 | 형식 | 설명 |
|---|---|---|---|
| flavors | Body | Object | 타입 목록 객체 |
| flavors.id | Body | UUID | 타입 ID |
| flavors.links | Body | Object | 타입 경로 객체 |
| flavors.name | Body | String | 타입 이름 |
| flavors.ram | Body | Integer | 메모리 (MB) |
| flavors.OS-FLV-DISABLED:disabled | Body | Boolean | 타입 활성화 여부 |
| flavors.vcpus | Body | Integer | 타입 vCPU 갯수 |
| flavors.extra_specs | Body | Object | 타입 추가 사양 객체 |
| flavors.swap | Body | Integer | 스왑 영역 크기 (GB) |
| flavors.os-flavor-access:is_public | Body | Boolean | 타입 공유 여부 |
| flavors.rxtx_factor | Body | Float | 네트워크 송신/수신 패킷 비율 |
| flavors.OS-FLV-EXT-DATA:ephemeral | Body | Integer | 임시 볼륨 크기 (GB) |
| flavors.disk | Body | Integer | 기본 디스크 크기 (GB) |

<details><summary>예시</summary>
<p>

```json
{
  "flavors": [
    {
      "name": "x1.c32m256",
      "links": [
        {
          "href": "https://kr1-api-compute.cloud.toast.com/v2/6cdebe3eb0094910bc41f1d42ebe4cb7/flavors/97604802-a090-43fa-a5ce-c7cfd737fbba",
          "rel": "self"
        },
        {
          "href": "https://kr1-api-compute.cloud.toast.com/6cdebe3eb0094910bc41f1d42ebe4cb7/flavors/97604802-a090-43fa-a5ce-c7cfd737fbba",
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
          "href": "https://kr1-api-compute.cloud.toast.com/v2/6cdebe3eb0094910bc41f1d42ebe4cb7/flavors/31fa632d-aeec-4f12-8a57-ce9d146228e5",
          "rel": "self"
        },
        {
          "href": "https://kr1-api-compute.cloud.toast.com/6cdebe3eb0094910bc41f1d42ebe4cb7/flavors/31fa632d-aeec-4f12-8a57-ce9d146228e5",
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

## 가용성 영역

인스턴스를 물리적으로 분리된 가용성 영역에 둠으로써 서비스의 가용성을 높일 수 있습니다.

### 가용성 목록 보기

사용할 수 있는 가용성 영역 목록을 반환합니다.

```
GET /v2/{tenantId}/os-availability-zone
X-Auth-Token: {tokenId}
```

#### 요청
이 API는 요청 본문을 요구하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tenantId | URL | String | O | 테넌트 ID |
| tokenId | Header | String | O | 토큰 ID |

#### 응답
| 이름 | 종류 | 형식 | 설명 |
|---|---|---|---|
| availabilityZoneInfo | Body | Object | 가용성 영역 정보 객체 |
| availabilityZoneInfo.hosts | Body | - | 가용성 영역에 속한 호스트 정보 객체<br>일반 사용자에게는 null로 표시 |
| availabilityZoneInfo.zoneName | Body | String | 가용성 영역 이름 |
| availabilityZoneInfo.zoneState | Body | Object | 가용성 영역 상태 정보 객체 |
| availabilityZoneInfo.available | Body | Object | 가용성 영역 상태 |

<details><summary>예시</summary>
<p>

```json
{
    "availabilityZoneInfo": [
      {
        "zoneState": {
          "available": true
        },
        "hosts": null,
        "zoneName": "kr-pub-a"
      },
      {
        "zoneState": {
          "available": true
        },
        "hosts": null,
        "zoneName": "kr-pub-b"
      }
    ]
}
```

</p>
</details>

## 키페어
키페어는 인스턴스에 접근할 때 사용하는 접근키 입니다. API를 통해 이 키페어를 관리할 수 있습니다.

### 키페어 목록 보기
```
GET /v2/{tenantId}/os-keypairs
X-Auth-Token: {tokenId}
```

#### 요청
이 API는 요청 본문을 요구하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tenantId | URL | String | O | 테넌트 ID |
| tokenId | Header | String | O | 토큰 ID |

#### 응답

| 이름 | 종류 | 형식 | 설명 |
|---|---|---|---|
| keypairs | Body | Array | 키페어 객체 목록 |
| keypairs.keypair | Body | Object | 키페어 객체 |
| keypairs.keypair.name | Body | String | 키페어 이름 |
| keypairs.keypair.public_key | Body | String | 공개키 |
| keypairs.keypair.fingerprint | Body | String | 키페어 지문 |

<details><summary>예시</summary>
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

### 키페어 보기
```
GET /v2/{tenantId}/os-keypairs/{keypairName}
X-Auth-Token: {tokenId}
```

#### 요청
이 API는 요청 본문을 요구하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tenantId | URL | String | O | 테넌트 ID |
| keypairName | URL | String | O | 키페어 이름 |
| tokenId | Header | String | O | 토큰 ID |

#### 응답

| 이름 | 종류 | 형식 | 설명 |
|---|---|---|---|
| keypair | Body | Object | 키페어 객체 목록 |
| keypair.public_key | Body | String | 공개키 |
| keypair.user_id | Body | String | 키 소유주 ID |
| keypair.name | Body | String | 키페어 이름 |
| keypair.deleted | Body | Boolean | 키페어 삭제 여부 |
| keypair.created_at | Body | Datetime | 키페어 생성 시각<br>`CCYY-MM-DDThh:mm:ss.SSSSSS` |
| keypair.updated_at | Body | Datetime | 키페어 수정 시각<br>`CCYY-MM-DDThh:mm:ss.SSSSSS` |
| keypair.deleted_at | Body | Datetime | 키페어 삭제 시각<br>`CCYY-MM-DDThh:mm:ss.SSSSSS` |
| keypair.fingerprint | Body | String | 키페어 지문 |
| keypair.id | Body | Integer | 키페어 ID |

<details><summary>예시</summary>
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

### 키페어 생성/등록하기
새로 키페어를 생성하거나 공개키를 추가하여 키페어를 등록할 수 있습니다.

```
POST /v2/{tenantId}/os-keypairs
X-Auth-Token: {tokenId}
```

#### 요청

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tenantId | URL | String | O | 테넌트 ID |
| tokenId | Header | String | O | 토큰 ID |
| keypair | Body | Object | O | 키페어 객체 |
| keypair.name | Body | String | O | 생성 또는 등록할 키페어 이름 |
| keypair.public_key | Body | String | - | 공개키 |

<details><summary>예시</summary>
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

#### 응답

| 이름 | 종류 | 형식 | 설명 |
|---|---|---|---|
| keypair | Body | Object | 키페어 객체 목록 |
| keypair.public_key | Body | String | 공개키 |
| keypair.user_id | Body | String | 키 소유주 ID |
| keypair.name | Body | String | 키페어 이름 |
| keypair.fingerprint | Body | String | 키페어 지문 |

<details><summary>예시</summary>
<p>

```json
{
    "keypair": {
        "fingerprint": "1e:2c:9b:56:79:4b:45:77:f9:ca:7a:98:2c:b0:d5:3c",
        "name": "keypair",
        "public_key": "ssh-rsa ... Generated-by-Nova",
        "user_id": "fake"
    }
}
```

</p>
</details>

### 키페어 삭제하기
```
DELETE /v2/{tenantId}/os-keypairs/{keypairName}
X-Auth-Token: {tokenId}
```

#### 요청
이 API는 요청 본문을 요구하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tenantId | URL | String | O | 테넌트 ID |
| keypairName | URL | String | O | 키페어 이름 |
| tokenId | Header | String | O | 토큰 ID |

#### 응답
이 API는 응답 본문을 반환하지 않습니다.


## 인스턴스
인스턴스 서비스 API에서 가장 기본이 되는 리소스 입니다.

### 인스턴스 상태

인스턴스는 다양한 상태를 가지며, 상태에 따라 취할 수 있는 동작이 정해져 있습니다. 가능한 상태 목록은 다음과 같습니다.

| 상태 명 | 설명 |
|--|--|
| `ACTIVE` | 인스턴스가 활성 상태인 경우 |
| `BUILDING` | 인스턴스가 생성 중인 경우 |
| `DELETED`| 인스턴스가 삭제된 경우 |
| `ERROR`| 직전 인스턴스가 취한 동작이 실패한 경우 |
| `HARD_REBOOT`| 인스턴스를 강제 재시작한 경우.<br>강제 재시작하면 마치 물리 서버의 전원 케이블을 뽑았다가 다시 연결하고 켜는 것과 유사한 효과를 냄 |
| `PAUSED`| 인스턴스가 일시정지된 경우. 일시정지된 인스턴스는 하이퍼바이저의 메모리에 저장됨 |
| `REBOOT`| 인스턴스를 재시작한 경우 |
| `REBUILD`| 인스턴스를 생성 당시 이미지로부터 새롭게 만들어내는 경우 |
| `RESCUED`| 인스턴스를 복구 모드에서 실행한 경우 |
| `RESIZED`| 인스턴스 타입을 변경하거나 다른 호스트로 옮기는 경우.<br>이 때 인스턴스는 순간적으로 종료되었다가 다시 켜지게 됨 |
| `REVERT_RESIZE`| 인스턴스 타입을 변경하거나 다른 호스트로 옮겨가는 과정에서 실패했을 때,<br>원 상태로 돌아가기 위해 복구하는 경우 |
| `VERIFY_RESIZE`| 인스턴스가 타입 변경 또는 다른 호스트로 옮기는 과정을 마치고 사용자의 승인을 기다리는 경우.<br>TOAST 기본 인프라 서비스의 인스턴스는 이 경우 1초 뒤 자동으로 `ACTIVE` 상태로 전이함 |
| `STOPPED`| 인스턴스가 종료된 경우 |
| `SUSPENDED`| 인스턴스가 관리자에 의해 최대 절전 모드로 진입한 경우 |
| `UNKNOWN`| 인스턴스의 상태를 알 수 없는 경우.<br>관리하는 인스턴스가 이 상태로 진입한 경우 즉시 관리자에게 문의할 것 |

### 인스턴스 목록 보기

현재 테넌트에 생성된 인스턴스 목록을 가져옵니다.

```
GET /v2/{tenantId}/servers
X-Auth-Token: {tokenId}
```

#### 요청

이 API는 요청 본문을 요구하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tenantId | URL | String | O | 테넌트 ID |
| tokenId | Header | String | O | 토큰 ID |
| changes-since | Query | Datetime | - | 마지막 인스턴스 변경 시각<br>지정된 시각 이후로 변경된 인스턴스 목록을 반환 |
| image | Query | UUID | - | 이미지 ID<br>지정된 이미지를 사용한 인스턴스 목록을 반환 |
| flavor | Query | UUID | - | 인스턴스 타입 ID<br>지정된 타입을 사용한 인스턴스 목록을 반환 |
| name | Query | String | - | 인스턴스 이름<br>지정된 이름을 가진 인스턴스 목록을 반환<br>정규 표현식으로 질의 가능 |
| status | Query | Enum | - | 인스턴스 상태<br>지정된 상태를 가진 인스턴스 목록을 반환 |
| limit | Query | Integer | - | 인스턴스 목록 갯수<br>지정된 갯수 만큼의 인스턴스 목록을 반환 |
| marker | Query | UUID | - | 목록의 첫번째 인스턴스 UUID<br>정렬 기준에 따라 `marker`로 지정된 인스턴스부터<br>`limit` 갯수 만큼의 인스턴스 목록을 반환 |

#### 응답

| 이름 | 종류 | 형식 | 설명 |
|---|---|---|---|
| servers | Body | Object | 인스턴스 목록 객체 |
| id | Body | UUID | 인스턴스 UUID |
| links | body | Object | 인스턴스 경로 객체 |

<details><summary>예시</summary>
<p>

```json
{
  "servers": [
    {
      "id": "aaf2778b-ea03-4ccc-8b1b-92f4b686c3ec",
      "links": [
        {
          "href": "https://kr1-api-compute.cloud.toast.com/v2/6cdebe3eb0094910bc41f1d42ebe4cb7/servers/aaf2778b-ea03-4ccc-8b1b-92f4b686c3ec",
          "rel": "self"
        },
        {
          "href": "https://kr1-api-compute.cloud.toast.com/6cdebe3eb0094910bc41f1d42ebe4cb7/servers/aaf2778b-ea03-4ccc-8b1b-92f4b686c3ec",
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

### 인스턴스 목록 상세 보기

앞서 인스턴스 목록 보기와 비슷하게 현재 테넌트에 생성된 인스턴스 목록을 반환합니다.
단, 인스턴스별 상세한 정보를 같이 조회할 수 있습니다.

#### 요청

이 API는 요청 본문을 요구하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tenantId | URL | String | O | 테넌트 ID |
| tokenId | Header | String | O | 토큰 ID |
| changes-since | Query | Datetime | - | 마지막 인스턴스 변경 시각<br>지정된 시각 이후로 변경된 인스턴스 목록을 반환 |
| image | Query | UUID | - | 이미지 ID<br>지정된 이미지를 사용한 인스턴스 목록을 반환 |
| flavor | Query | UUID | - | 인스턴스 타입 ID<br>지정된 타입을 사용한 인스턴스 목록을 반환 |
| name | Query | String | - | 인스턴스 이름<br>지정된 이름을 가진 인스턴스 목록을 반환 정규 표현식으로 질의 가능 |
| status | Query | Enum | - | 인스턴스 상태<br>지정된 상태를 가진 인스턴스 목록을 반환 |
| limit | Query | Integer | - | 인스턴스 목록 갯수<br> 지정된 갯수 만큼의 인스턴스 목록을 반환 |
| marker | Query | UUID | - | 목록의 첫번째 인스턴스 UUID<br>정렬 기준에 따라 `marker`로 지정된 인스턴스부터 `limit` 갯수 만큼의 인스턴스 목록을 반환 |

#### 응답

| 이름 | 종류 | 형식 | 설명 |
|---|---|---|---|
| servers | body | Object | 인스턴스 목록 객체 |
| status | body | Enum | [인스턴스 상태](#_9) 참조 |
| servers.updated | Body | Datetime | 인스턴스의 최종 수정 시각<br>yyyy-mm-ddTHH:MM:SSZ 형식 |
| servers.hostId | Body | String | 인스턴스가 구동 중인 호스트 구분자 |
| servers.addresses | Body | Object | 인스턴스 IP 목록 객체<br>인스턴스에 연결된 포트 수 만큼 목록이 생성됨 |
| servers.addresses."Network 이름" | Body | Object | 인스턴스에 연결된 Network별 포트 정보 |
| servers.addresses."Network 이름".OS-EXT-IPS-MAC:mac_addr | Body | String | 인스턴스에 연결된 포트의 MAC 주소 |
| servers.addresses."Network 이름".version | Body | Integer | 인스턴스에 연결된 포트의 IP 버전<br>TOAST의 경우 오직 IPv4만 지원 |
| servers.addresses."Network 이름".addr | Body | String | 인스턴스에 연결된 포트의 IP 주소 |
| servers.addresses."Network 이름".OS-EXT-IPS:type | Body | Enum | 포트의 IP 주소 타입<br>`fixed` 또는 `floating` 중 하나 |
| servers.links | Body | Object | 인스턴스 경로 객체 |
| servers.key_name | Body | String | 인스턴스의 접속키 이름 |
| servers.image | Body | Object | 인스턴스의 이미지 정보 객체 |
| servers.image.id | Body | UUID | 인스턴스의 이미지 ID |
| servers.image.links | Body | Object | 인스턴스의 이미지 경로 객체 |
| servers.OS-EXT-STS:task_state | Body | String | 인스턴스의 작업 상태<br>인스턴스에 동작을 가했을 때 동작 진행 상태를 알려줌 |
| servers.OS-EXT-STS:vm_state | Body | String | 인스턴스의 구동 상태<br>인스턴스의 현재 상태를 알려줌 |
| servers.OS-SRV-USG:launched_at | Body | Datetime | 인스턴스의 마지막 부팅 시각<br>yyyy-mm-ddTHH:MM:SS.ssssss 형식 |
| servers.flavor | Body | Object | 인스턴스 타입 정보 객체 |
| servers.flavor.id | Body | UUID | 인스턴스 타입 ID |
| servers.flavor.links | Body | Object | 인스턴스 타입 경로 객체 |
| servers.id | Body | UUID | 인스턴스의 ID |
| servers.security_groups | Body | Object | 인스턴스에 할당된 보안 그룹 목록 객체 |
| servers.security_groups.name | Body | String | 인스턴스에 할당된 보안 그룹 이름 |
| servers.OS-SRV-USG:terminated_at | Body | Datetime | 인스턴스의 삭제 시각<br>yyyy-mm-ddTHH:MM:SSZ 형식 |
| servers.user_id | Body | String | 인스턴스를 생성한 사용자 ID |
| servers.name | Body | String | 인스턴스의 이름<br>최대 255자 제한 |
| servers.created | Body | Datetime | 인스턴스 생성 시각<br>yyy-mm-ddTHH:MM:SSZ 형식 |
| servers.tenant_id | Body | String | 인스턴스가 속한 테넌트 ID |
| servers.OS-DCF:diskConfig | Body | Enum | 인스턴스의 디스크 설정 모드<br>`MANUAL` 또는 `AUTO` 중 하나<br>TOAST에서는 `MANUAL`만 사용 |
| servers.os-extended-volumes:volumes_attached | Body | Object | 인스턴스에 연결된 추가 볼륨 목록 객체 |
| servers.os-extended-volumes:volumes_attached.id | Body | UUID | 인스턴스에 연결된 추가 볼륨 ID |
| servers.OS-EXT-STS:power_state | Body | Integer | 인스턴스의 전원 상태<br>- 1: On<br>- 4: Off |
| servers.metadata | Body | Object | 인스턴스의 메타데이터 객체<br>인스턴스의 기타 정보를 키-값 쌍으로 보관 |

<details><summary>예시</summary>
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
          "href": "https://kr1-api-compute.cloud.toast.com/v2/6cdebe3eb0094910bc41f1d42ebe4cb7/servers/aaf2778b-ea03-4ccc-8b1b-92f4b686c3ec",
          "rel": "self"
        },
        {
          "href": "https://kr1-api-compute.cloud.toast.com/6cdebe3eb0094910bc41f1d42ebe4cb7/servers/aaf2778b-ea03-4ccc-8b1b-92f4b686c3ec",
          "rel": "bookmark"
        }
      ],
      "key_name": "access-key",
      "image": {
        "id": "8b9f8d47-b89b-45af-b1d6-3f7ce7e06a11",
        "links": [
          {
            "href": "https://kr1-api-compute.cloud.toast.com/6cdebe3eb0094910bc41f1d42ebe4cb7/images/8b9f8d47-b89b-45af-b1d6-3f7ce7e06a11",
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
            "href": "https://kr1-api-compute.cloud.toast.com/6cdebe3eb0094910bc41f1d42ebe4cb7/flavors/35a73b57-58a7-434d-aa08-5249aaa95b3e",
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

### 인스턴스 보기

지정한 인스턴스의 상세 정보를 반환한다.

```
GET /v2/{tenantId}/servers/{serverId}
X-Auth-Token: {tokenId}
```

#### 요청

이 API는 요청 본문을 요구하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tenantId | URL | String | O | 테넌트 ID |
| serverId | URL | UUID | O | 인스턴스 ID |
| tokenId | Header | String | O | 토큰 ID |

#### 응답

| 이름 | 종류 | 형식 | 설명 |
|--|--|--|--|
| servers.status | body | Enum | [인스턴스 상태](#_9) 참조 |
| servers.updated | body | Datetime | 인스턴스의 최종 수정 시각<br>yyyy-mm-ddTHH:MM:SSZ 형식 |
| servers.hostId | body | String | 인스턴스가 구동 중인 호스트 구분자 |
| servers.addresses | body | Object | 인스턴스 IP 목록 객체 인스턴스에 연결된 포트 수 만큼 목록이 생성됨 |
| servers.addresses."Network 이름" | body | Object | 인스턴스에 연결된 Network별 포트 정보 |
| servers.addresses."Network 이름".OS-EXT-IPS-MAC:mac_addr | body | String | 인스턴스에 연결된 포트의 MAC 주소 |
| servers.addresses."Network 이름".version | body | Integer | 인스턴스에 연결된 포트의 IP 버전<br>TOAST의 경우 오직 IPv4만 지원 |
| servers.addresses."Network 이름".addr | body | String | 인스턴스에 연결된 포트의 IP 주소 |
| servers.addresses."Network 이름".OS-EXT-IPS:type | body | Enum | 포트의 IP 주소 타입<br> `fixed` 또는 `floating` 중 하나 |
| servers.key_name | body | String | 인스턴스의 접속키 이름 |
| servers.image  | body | Object | 인스턴스의 이미지 정보 객체 |
| servers.image.id | body | UUID | 인스턴스의 이미지 ID |
| servers.OS-EXT-STS:task_state | body | String | 인스턴스의 작업 상태<br>인스턴스에 동작을 가했을 때 동작 진행 상태를 알려줌 |
| servers.OS-EXT-STS:vm_state | body | String | 인스턴스의 구동 상태<br>인스턴스의 현재 상태를 알려줌 |
| servers.OS-SRV-USG:launched_at | body | Datetime | 인스턴스의 마지막 부팅 시각<br>yyyy-mm-ddTHH:MM:SS.ssssss 형식 |
| servers.flavor | body | Object | 인스턴스 타입 정보 객체 |
| servers.flavor.id | body | UUID | 인스턴스 타입 ID |
| servers.flavor.links | body | Object | 인스턴스 타입 경로 객체 |
| servers.id | body | UUID | 인스턴스의 ID |
| servers.security_groups | body | Object | 인스턴스에 할당된 보안 그룹 목록 객체 |
| servers.security_groups.name | body | String | 인스턴스에 할당된 보안 그룹 이름 |
| servers.OS-SRV-USG:terminated_at | body | Datetime | 인스턴스의 삭제 시각<br>yyyy-mm-ddTHH:MM:SSZ 형식 |
| servers.user_id | body | String | 인스턴스를 생성한 사용자 ID |
| server.name | body | String | 인스턴스의 이름 최대 255자 제한 |
| servers.created | body | Datetime | 인스턴스 생성 시각<br>yyy-mm-ddTHH:MM:SSZ 형식 |
| servers.tenant_id | body | String | 인스턴스가 속한 테넌트 ID |
| servers.OS-DCF:diskConfig | body | Enum | 인스턴스의 디스크 설정 모드<br>`MANUAL` 또는 `AUTO` 중 하나 TOAST에서는 `MANUAL`만 사용 |
| servers.os-extended-volumes:volumes_attached | body | Object | 인스턴스에 연결된 추가 볼륨 목록 객체 |
| servers.os-extended-volumes:volumes_attached.id | body | UUID | 인스턴스에 연결된 추가 볼륨 ID |
| servers.OS-EXT-STS:power_state | body | Integer | 인스턴스의 전원 상태 - 1: On - 4: Off |
| servers.metadata | body | Object | 인스턴스의 메타데이터 객체<br>인스턴스의 기타 정보를 키-값 쌍으로 보관  |

<details><summary>예시</summary>
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
        "href": "http://nova.iaas.tcc1.cloud.toastoven.net:8774/v2/6cdebe3eb0094910bc41f1d42ebe4cb7/servers/aaf2778b-ea03-4ccc-8b1b-92f4b686c3ec",
        "rel": "self"
      },
      {
        "href": "http://nova.iaas.tcc1.cloud.toastoven.net:8774/6cdebe3eb0094910bc41f1d42ebe4cb7/servers/aaf2778b-ea03-4ccc-8b1b-92f4b686c3ec",
        "rel": "bookmark"
      }
    ],
    "key_name": "access-key",
    "image": {
      "id": "8b9f8d47-b89b-45af-b1d6-3f7ce7e06a11",
      "links": [
        {
          "href": "http://nova.iaas.tcc1.cloud.toastoven.net:8774/6cdebe3eb0094910bc41f1d42ebe4cb7/images/8b9f8d47-b89b-45af-b1d6-3f7ce7e06a11",
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
          "href": "http://nova.iaas.tcc1.cloud.toastoven.net:8774/6cdebe3eb0094910bc41f1d42ebe4cb7/flavors/35a73b57-58a7-434d-aa08-5249aaa95b3e",
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

### 인스턴스 생성하기

인스턴스를 생성합니다. 생성 API가 호출되었다고 해서 즉시 인스턴스가 생성되는 것은 아닙니다.
인스턴스 단건 조회 API를 통해 인스턴스의 `status` 필드를 관찰하여 `ACTIVE` 상태로 전이하는 것을 확인하셔야 합니다.
인스턴스의 `status`가 `BUILDING` 상태 또는 다른 `ERROR` 상태로 전이되는 경우 잘못된 매개변수를 넣지는 않았는지 확인 바랍니다.

```
POST /v2/{tenantId}/servers
X-Auth-Token: {tokenId}
```

#### 요청

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tenantId | URL | String | O | 테넌트 ID |
| tokenId | Header | String | O | 토큰 ID |
| server.security_groups | body | Object | - | 보안 그룹 목록 객체<br> 지정하지 않을 경우 `default` 그룹만 추가 |
| server.security_groups.name | body | String | - | 인스턴스에 추가할 보안 그룹 이름 |
| server.user_data | body | String | - | 인스턴스 부팅 후 실행할 스크립트 및 설정<br>base64 인코딩된 문자열로 65535자 이하까지 허용 |
| server.availability_zone | body | String | - | 인스턴스를 생성할 가용 영역<br>지정하지 않을 경우 임의로 선택 |
| server.imageRef | Body | String | O | 인스턴스를 생성할 때 사용할 이미지 ID |
| server.flavorRef | Body | String | O | 인스턴스를 생성할 때 사용할 타입 ID |
| server.networks | Body | Object | O | 인스턴스를 생성할 때 사용할 네트워크 정보 객체<br>지정한 갯수만큼 NIC이 추가됨<br>네트워크 ID, 포트 ID, 고정 IP 중 하나만 지정 |
| server.networks.uuid | Body | UUID | - | 인스턴스를 생성할 때 사용할 네트워크 ID |
| server.networks.port | Body | UUID | - | 인스턴스를 생성할 때 사용할 포트 ID |
| server.networks.fixed_ip | Body | String | - | 인스턴스를 생성할 때 사용할 고정 IP |
| server.name | Body | String | O | 생성할 인스턴스의 이름<br>영문자 기준 255자까지 허용<br>단 Windows 이미지의 경우 15자 이하이어야 함 |
| server.metadata | Body | Object | - | 인스턴스에 추가할 메타데이터 객체<br>최대 길이 255자 이하의 키-값 쌍 |
| server.personality | Body | Object | - | 인스턴스에 추가할 파일 정보 객체 |
| server.personality.path | Body | String | - | 인스턴스에 추가할 파일 경로 |
| server.personality.content | Body | String | - | 인스턴스에 추가할 파일 내용<br>Base64 인코딩된 문자열<br>인코딩 전 기준으로 65535자 이하까지 허용 |
| server.block_device_mapping_v2 | Body | Object | - | 인스턴스의 블록 스토리지 정보 객체<br>TOAST의 m/c/r/t/x 타입의 스펙을 사용할 경우<br>반드시 지정해야 함 |
| server.block_device_mapping_v2.source_type | Body | Enum | - | 생성할 인스턴스의 볼륨 원형 타입<br>- `blank`: 빈 볼륨을 생성<br>- `snapshot`: 스냅샷 기반 볼륨 생성<br>- `volume`: 다른 볼륨 기반 새 볼륨 생성<br>- `image`: 이미지 기반 볼륨 생성 |
| server.block_device_mapping_v2.destination_type | Body | Enum | - | 생성할 인스턴스의 볼륨의 위치<br>- `local`: 호스트의 볼륨<br>- `volume`: 호스트 간 공유 가능한 NAS |
| server.block_device_mapping_v2.delete_on_termination | Body | Boolean | - | 인스턴스 삭제시 볼륨 처리 여부<br>`true`이면 삭제, `false`이면 유지 |
| server.block_device_mapping_v2.boot_index | Body | Integer | - | 지정한 볼륨의 부팅 순서<br>-`0` 이면 루트 볼륨<br>- 그 외는 추가 볼륨<br>크기가 클 수록 부팅 순서는 낮아짐 |
| server.key_name | Body | String | O | 생성할 인스턴스의 접근 키 |

<details><summary>예시</summary>
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

#### 응답

| 이름 | 종류 | 형식 | 설명 |
|---|---|---|---|
| server.security_groups.name | Body | String | 생성한 인스턴스의 보안 그룹 이름 |
| server.OS-DCF:diskConfig | Body | Enum | `AUTO` 또는 `MANUAL` 중 하나 |
| server.id | Body | UUID | 생성한 인스턴스의 ID |

#### 예시

<details><summary>펼쳐 보기</summary>
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
        "href": "https://kr1-api-compute.cloud.toast.com/v2/6cdebe3eb0094910bc41f1d42ebe4cb7/servers/3a005d5b-63cf-4493-bfc6-49db990b5b50",
        "rel": "self"
      },
      {
        "href": "https://kr1-api-compute.cloud.toast.com/6cdebe3eb0094910bc41f1d42ebe4cb7/servers/3a005d5b-63cf-4493-bfc6-49db990b5b50",
        "rel": "bookmark"
      }
    ]
  }
}
```

</p>
</details>

> Windows 이미지는 2GB 이상의 RAM과 50GB 이상의 기본 디스크가 필요로 합니다. 또한 U 타입은 Windows 이미지를 사용하실 수 없습니다.
>
> 기본 디스크의 크기는 최소 10GB (Linux 계열)/50GB (Windows 계열) 부터 1TB까지 지정가능합니다.

### 인스턴스 수정하기
생성된 인스턴스를 수정합니다. 변경할 수 있는 속성은 일부 항목으로 제한됩니다.

```
PUT /v2/{tenantId}/servers/{serverId}
X-Auth-Token: {tokenId}
```

#### 요청

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tenantId | URL | String | O | 테넌트 ID |
| serverId | URL | UUID | O | 변경할 인스턴스 ID |
| tokenId | Header | String | O | 토큰 ID |
| server | Body | Object | O | 인스턴스 변경 요청 객체 |
| server.name | Body | String | - | 인스턴스의 새로운 이름 |

<details><summary>예시</summary>
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

#### 응답
인스턴스 보기와 동일합니다.

### 인스턴스 삭제하기
생성된 인스턴스를 삭제합니다.

```
DELETE /v2/{tenantId}/servers/{serverId}
X-Auth-Token: {tokenId}
```

#### 요청
이 API는 요청 본문을 요구하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|--|
| tenantId | URL | String | O | 테넌트 ID |
| serverId | URL | UUID | O | 변경할 인스턴스 ID |
| tokenId | Header | String | O | 토큰 ID |

#### 응답
이 API는 응답 본문을 반환하지 않습니다.


## 인스턴스 볼륨 연결 관리
### 연결된 인스턴스 볼륨 목록 보기
```
GET /v2/{tenantId}/servers/{server_id}/os-volume_attachments
X-Auth-Token: {tokenId}
```

#### 요청
이 API는 요청 본문을 요구하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|--|
| tenantId | URL | String | O | 테넌트 ID |
| serverId | URL | UUID | O | 변경할 인스턴스 ID |
| tokenId | Header | String | O | 토큰 ID |
| limit | Query | Integer | - | 조회할 목록 갯수 |
| offset | Query | Integer | - | 반환될 목록의 시작점<br>전체 목록 중 offset 번째 인스턴스부터 반환 |

#### 응답

| 이름 | 종류 | 형식 | 설명 |
|---|---|---|---|
| volumeAttachments | Body | Array | 연결 정보 객체 목록 |
| volumeAttachments.device | Body | String | 인스턴스의 볼륨 이름<br>예) `/dev/vdb` |
| volumeAttachments.id | Body | UUID | 연결 정보 ID |
| volumeAttachments.serverId | Body | UUID | 인스턴스 ID |
| volumeAttachments.volumeId | Body | UUID | 볼륨 ID |

<details><summary>예시</summary>
<p>

```json
{
    "volumeAttachments": [
        {
            "device": "/dev/vdc",
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

### 연결된 인스턴스 볼륨 보기
```
GET /v2/{tenantId}/servers/{serverId}/os-volume_attachments/{volumeId}
X-Auth-Token: {tokenId}
```

#### 요청
이 API는 요청 본문을 요구하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|--|
| tenantId | URL | String | O | 테넌트 ID |
| serverId | URL | UUID | O | 인스턴스 ID |
| volumeId | URL | UUID | O | 조회할 볼륨 ID |
| tokenId | Header | String | O | 토큰 ID |

#### 응답

| 이름 | 종류 | 형식 | 설명 |
|---|---|---|---|
| volumeAttachment | Body | Object | 연결 정보 객체 |
| volumeAttachment.device | Body | String | 인스턴스의 볼륨 이름<br>예) `/dev/vdb` |
| volumeAttachment.id | Body | UUID | 연결 정보 ID |
| volumeAttachment.serverId | Body | UUID | 인스턴스 ID |
| volumeAttachment.volumeId | Body | UUID | 볼륨 ID |

<details><summary>예시</summary>
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

### 인스턴스 볼륨 연결하기
```
POST /v2/{tenantId}/servers/{serverId}/os-volume_attachments/{volume_id}
X-Auth-Token: {tokenId}
```

#### 요청

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|--|
| tenantId | URL | String | O | 테넌트 ID |
| serverId | URL | UUID | O | 변경할 인스턴스 ID |
| tokenId | Header | String | O | 토큰 ID |
| volumeAttachment | Body | Object | O | 볼륨 연결 요청 객체 |
| volumeAttachment.volumeId | Body | UUID | O | 연결할 볼륨 ID |

<details><summary>예시</summary>
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

#### 응답

| 이름 | 종류 | 형식 | 설명 |
|---|---|---|---|
| volumeAttachment | Body | Object | 연결 정보 객체 |
| volumeAttachment.device | Body | String | 인스턴스의 볼륨 이름<br>예) `/dev/vdb` |
| volumeAttachment.id | Body | UUID | 연결 정보 ID |
| volumeAttachment.serverId | Body | UUID | 인스턴스 ID |
| volumeAttachment.volumeId | Body | UUID | 볼륨 ID |

<details><summary>예시</summary>
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

### 인스턴스 볼륨 연결 끊기
```
DELETE /v2/{tenantId}/servers/{serverId}/os-volume_attachments/{volumeId}
X-Auth-Token: {tokenId}
```

#### 요청
이 API는 요청 본문을 요구하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|--|
| tenantId | URL | String | O | 테넌트 ID |
| serverId | URL | UUID | O | 인스턴스 ID |
| volumeId | URL | UUID | O | 연결을 끊을 볼륨 ID |
| tokenId | Header | String | O | 토큰 ID |

#### 응답
이 API는 응답 본문을 반환하지 않습니다.


## 인스턴스 추가기능
TOAST에서는 다음과 같은 인스턴스 제어 및 부가 기능을 제공합니다.
* 인스턴스 시작, 정지, 재시작
* 인스턴스 타입 변경
* 인스턴스 이미지 생성
* 플로팅 IP 연결 및 해제
* 보안 그룹 추가 및 삭제

### 인스턴스 시작
정지된 인스턴스를 다시 시작하고 상태를 **ACTIVE** 로 변경합니다.
이 API를 호출하려면 인스턴스의 상태가 **SHUTOFF** 이어야 합니다.

```
POST /v2/{tenantId}/servers/{serverId}/action
X-Auth-Token: {tokenId}
```

#### 요청
| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|--|
| tenantId | URL | String | O | 테넌트 ID |
| serverId | URL | UUID | O | 변경할 인스턴스 ID |
| tokenId | Header | String | O | 토큰 ID |
| os-start | Body | - | O | 인스턴스 시작 요청 |

<details><summary>예시</summary>
<p>

```json
{
  "os-start" : null
}
```

</p>
</details>

#### 응답
이 API는 응답 본문을 반환하지 않습니다.

### 인스턴스 종료
정지된 인스턴스를 종료하고 상태를 **SHUTOFF** 로 변경합니다.
이 API를 호출하려면 인스턴스의 상태가 **ACTIVE** 또는 **ERROR** 이어야 합니다.

```
POST /v2/{tenantId}/servers/{serverId}/action
X-Auth-Token: {tokenId}
```

#### 요청
| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|--|
| tenantId | URL | String | O | 테넌트 ID |
| serverId | URL | UUID | O | 변경할 인스턴스 ID |
| tokenId | Header | String | O | 토큰 ID |
| os-stop | Body | - | O | 인스턴스 종료 요청 |

<details><summary>예시</summary>
<p>

```json
{
  "os-stop" : null
}
```

</p>
</details>


#### 응답
이 API는 응답 본문을 반환하지 않습니다.

### 인스턴스 재시작
인스턴스를 재시작 합니다. 재시작 방식은 **SOFT** 와 **HARD** 로 나눌 수 있습니다.

* **SOFT** 방식: 정상 종료 후 인스턴스를 재시작합니다. 인스턴스 상태가 **ACTIVE** 일때만 할 수 있습니다.

* **HARD** 방식: 강제 종료 후 인스턴스를 재시작합니다. 인스턴스 상태가 다음 상태일때만 할 수 있습니다.
  * **ACTIVE**
  * **ERROR**
  * **HARD_REBOOT**
  * **PAUSED**
  * **REBOOT**
  * **SHUTOFF**
  * **SUSPENDED**

이 API를 호출하려면 인스턴스의 상태가 **ACTIVE** 또는 **ERROR** 이어야 합니다.

```
POST /v2/{tenantId}/servers/{serverId}/action
X-Auth-Token: {tokenId}
```

#### 요청
| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|--|
| tenantId | URL | String | O | 테넌트 ID |
| serverId | URL | UUID | O | 변경할 인스턴스 ID |
| tokenId | Header | String | O | 토큰 ID |
| reboot | Body | Object | O | 인스턴스 재부팅 요청 객체 |
| reboot.type | Body | Enum | O | 재부팅 방식<br>**SOFT**, **HARD** |

<details><summary>예시</summary>
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

#### 응답
이 API는 응답 본문을 반환하지 않습니다.

### 인스턴스 타입 변경
지정된 인스턴스 타입을 변경합니다. 사용하는 이미지나 인스턴스 타입에 따라 변경할 수 있는 타입이 제한될 수 있습니다.

```
POST /v2/{tenantId}/servers/{serverId}/action
X-Auth-Token: {tokenId}
```

#### 요청
| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|--|
| tenantId | URL | String | O | 테넌트 ID |
| serverId | URL | UUID | O | 변경할 인스턴스 ID |
| tokenId | Header | String | O | 토큰 ID |
| resize | Body | Object | O | 인스턴스 타입 변경 요청 |
| resize.flavorRef | Body | UUID | O | 변경할 인스턴스 타입 ID |
| resize.OS-DCF:diskConfig | Body | Enum | - | 타입 변경 후 기본 디스크 파티션 방식 지정<br>**AUTO**: 자동으로 전체 디스크를 하나의 파티션으로 설정<br>**MANUAL**: 이미지에 지정된 대로 파티션을 설정. 이미지에서 설정된 크기보다 디스크의 크기가 더 큰 경우 사용하지 않은 채로 남겨둠. |

> TOAST에서 제공하는 이미지를 사용하는 경우 `diskConfig` 값은 항상 **AUTO** 로 지정해야 합니다.

<details><summary>예시</summary>
<p>

```json
{
  "resize" : {
    "flavorRef": "UUID",
    "OS-DCF:diskConfig": "AUTO"
  }
}
```

</p>
</details>

#### 응답
이 API는 응답 본문을 반환하지 않습니다.

### 인스턴스 이미지 생성
지정한 인스턴스로부터 이미지를 생성합니다.
인스턴스의 상태가 **ACTIVE**, **SHUTOFF**, **SUSPENDED**, **PAUSED** 일때만 이미지를 생성할 수 있습니다.
오직 `u2` 타입의 인스턴스만 이 API를 통해 이미지를 생성할 수 있습니다.

이미지 생성이 성공하면 이미지의 상태가 `active` 로 전이합니다.
이미지 생성이 완료되는 것을 확인하기 위해서는 이미지 조회 API를 통해 지속적으로 상태를 확인해야 합니다.

```
POST /v2/{tenantId}/servers/{serverId}/action
X-Auth-Token: {tokenId}
```

#### 요청
| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|--|
| tenantId | URL | String | O | 테넌트 ID |
| serverId | URL | UUID | O | 변경할 인스턴스 ID |
| tokenId | Header | String | O | 토큰 ID |
| createImage | Body | Object | O | 이미지 생성 요청 |
| createImage.name | Body | String | O | 생성할 이미지 이름 |
| createImage.metadata | Body | Object | O | 생성할 이미지의 메타데이터<br>Key-Value 형태로 기술 |

<details><summary>예시</summary>
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


#### 응답

| 이름 | 종류 | 형식 | 설명 |
|--|--|--|--|
| Location | Header | String | 생성한 이미지 URL |
| image_id | Body | UUID | 생성한 이미지 ID |

<details><summary>예시</summary>
<p>

```json
{
    "image_id": "0e7761dd-ee98-41f0-ba35-05994e446431"
}
```

</p>
</details>


### 보안 그룹 추가
지정한 인스턴스에 보안 그룹을 추가합니다. 추가한 보안 그룹은 인스턴스의 모든 포트에 적용됩니다.

```
POST /v2/{tenantId}/servers/{serverId}/action
X-Auth-Token: {tokenId}
```

#### 요청
| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|--|
| tenantId | URL | String | O | 테넌트 ID |
| serverId | URL | UUID | O | 변경할 인스턴스 ID |
| tokenId | Header | String | O | 토큰 ID |
| addSecurityGroup | Body | Object | O | 보안 그룹 추가 요청 객체 |
| addSecurityGroup.name | Body | String | O | 추가할 보안 그룹 이름 |

<details><summary>예시</summary>
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


#### 응답
이 API는 응답 본문을 반환하지 않습니다.

### 보안 그룹 삭제
지정한 인스턴스에서 보안 그룹을 제거합니다. 인스턴스의 모든 포트로부터 지정한 보안 그룹이 제거됩니다.

```
POST /v2/{tenantId}/servers/{serverId}/action
X-Auth-Token: {tokenId}
```

#### 요청
| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|--|
| tenantId | URL | String | O | 테넌트 ID |
| serverId | URL | UUID | O | 변경할 인스턴스 ID |
| tokenId | Header | String | O | 토큰 ID |
| removeSecurityGroup | Body | Object | O | 보안 그룹 삭제 요청 객체 |
| removeSecurityGroup.name | Body | String | O | 삭제할 보안 그룹 이름 |

<details><summary>예시</summary>
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


#### 응답
이 API는 응답 본문을 반환하지 않습니다.
