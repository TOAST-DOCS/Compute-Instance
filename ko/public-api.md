## Compute > Instance > API v2 가이드

API를 사용하려면 API 엔드포인트와 토큰 등이 필요합니다. [API 사용 준비](/Compute/Compute/ko/identity-api/)를 참고하여 API 사용에 필요한 정보를 준비합니다.

인스턴스 API는 `compute` 타입 엔드포인트를 이용합니다. 정확한 엔드포인트는 토큰 발급 응답의 `serviceCatalog`를 참조합니다.

| 타입 | 리전 | 엔드포인트 |
|---|---|---|
| compute | 한국(판교) 리전<br>한국(평촌) 리전<br>일본 리전 | https://kr1-api-instance-infrastructure.nhncloudservice.com<br>https://kr2-api-instance-infrastructure.nhncloudservice.com<br>https://jp1-api-instance-infrastructure.nhncloudservice.com |

API 응답에 가이드에 명시되지 않은 필드가 나타날 수 있습니다. 이런 필드는 NHN Cloud 내부 용도로 사용되며 사전 공지 없이 변경될 수 있으므로 사용하지 않습니다.

## 인스턴스 타입

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
| minDisk | Query | Integer | - | 최소 블록 스토리지 크기(GB)<br>지정한 크기보다 블록 스토리지 크기가 큰 타입만 반환 |
| minRam | Query | Integer | - | 최소 RAM 크기(MB)<br>지정한 크기보다 RAM 크기가 큰 타입만 반환 |

#### 응답

| 이름 | 종류 | 형식 | 설명 |
|---|---|---|---|
| flavors | Body | Object | 인스턴스 타입 목록 객체 |
| flavors.id | Body | UUID | 인스턴스 타입 ID |
| flavors.links | Body | Object | 인스턴스 타입 경로 객체 |
| flavors.name | Body | String | 인스턴스 타입 이름 |


<details><summary>예시</summary>
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
| minDisk | Query | Integer | - | 최소 블록 스토리지 크기(GB)<br>지정한 크기보다 블록 스토리지 크기가 큰 타입만 반환 |
| minRam | Query | Integer | - | 최소 RAM 크기(MB)<br>지정한 크기보다 RAM 크기가 큰 타입만 반환 |

#### 응답

| 이름 | 종류 | 형식 | 설명             |
|---|---|---|----------------|
| flavors | Body | Object | 인스턴스 타입 목록 객체  |
| flavors.id | Body | UUID | 인스턴스 타입 ID     |
| flavors.links | Body | Object | 인스턴스 타입 경로 객체  |
| flavors.name | Body | String | 인스턴스 타입 이름     |
| flavors.ram | Body | Integer | 메모리 크기(MB)     |
| flavors.OS-FLV-DISABLED:disabled | Body | Boolean | 활성화 여부         |
| flavors.vcpus | Body | Integer | vCPU 개수        |
| flavors.extra_specs | Body | Object | 추가 사양 객체       |
| flavors.swap | Body | Integer | 스와프 영역 크기(GB)  |
| flavors.os-flavor-access:is_public | Body | Boolean | 공유 여부          |
| flavors.rxtx_factor | Body | Float | 네트워크 송신/수신 패킷 비율 |
| flavors.OS-FLV-EXT-DATA:ephemeral | Body | Integer | 임시 블록 스토리지 크기(GB)     |
| flavors.disk | Body | Integer | 루트 블록 스토리지 크기(GB) |

<details><summary>예시</summary>
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

## 가용성 영역

### 가용성 목록 보기

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

## 키페어

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

---

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
| keypair.user_id | Body | String | 키페어 소유주 ID |
| keypair.name | Body | String | 키페어 이름 |
| keypair.deleted | Body | Boolean | 키페어 삭제 여부 |
| keypair.created_at | Body | Datetime | 키페어 생성 시각<br>`YYYY-MM-DDThh:mm:ss.SSSSSS` |
| keypair.updated_at | Body | Datetime | 키페어 수정 시각<br>`YYYY-MM-DDThh:mm:ss.SSSSSS` |
| keypair.deleted_at | Body | Datetime | 키페어 삭제 시각<br>`YYYY-MM-DDThh:mm:ss.SSSSSS` |
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

---

### 키페어 생성/등록하기

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
| keypair.public_key | Body | String | - | 등록할 공개키. 이 필드가 생략되면 새로운 키페어를 생성합니다. |

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
| keypair | Body | Object | 키페어 객체 |
| keypair.public_key | Body | String | 공개키 |
| keypair.private_key | Body | String | 비밀키, 새로운 키페어를 생성한 경우에 비밀키를 반환합니다. |
| keypair.user_id | Body | String | 키페어 소유주 ID |
| keypair.name | Body | String | 키페어 이름 |
| keypair.fingerprint | Body | String | 키페어 지문 |

<details><summary>예시</summary>
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

### 인스턴스 상태

인스턴스는 다양한 상태를 가지며 상태에 따라 취할 수 있는 동작이 정해져 있습니다. 인스턴스 상태 목록은 다음과 같습니다.

| 상태명               | 설명                                                                                                |
|-------------------|---------------------------------------------------------------------------------------------------|
| `ACTIVE`          | 인스턴스가 활성 상태인 경우                                                                                   |
| `BUILDING`        | 인스턴스가 생성 중인 경우                                                                                    |
| `STOPPED`         | 인스턴스가 중지된 경우                                                                                      |
| `SHELVED_OFFLOADED` | 인스턴스가 종료된 경우                                                                                      |
| `DELETED`         | 인스턴스가 삭제된 경우                                                                                      |
| `REBOOT`          | 인스턴스를 재시작한 경우                                                                                     |
| `HARD_REBOOT`     | 인스턴스를 강제 재시작한 경우<br> 물리 서버의 전원을 내리고 다시 켜는 것과 동일한 동작                                               |
| `RESIZED`         | 인스턴스 타입을 변경하거나 인스턴스를 다른 호스트로 옮기는 경우<br>인스턴스가 중지되었다가 다시 시작된 상태                                     |
| `REVERT_RESIZE`   | 인스턴스 타입을 변경하거나 인스턴스를 다른 호스트로 옮기는 과정에서 실패했을 때 원상태로 돌아가기 위해 복구하는 경우                                 |
| `VERIFY_RESIZE`   | 인스턴스가 타입 변경 또는 인스턴스를 다른 호스트로 옮기는 과정을 마치고 사용자의 승인을 기다리는 경우<br>NHN Cloud에서는  이 경우 자동으로 `ACTIVE` 상태가 됨 |
| `ERROR`           | 직전 인스턴스에 취한 동작이 실패한 경우                                                                            |
| `PAUSED`          | 인스턴스가 일시 정지된 경우<br>일시 정지된 인스턴스는 하이퍼바이저의 메모리에 저장됨                                                  |
| `REBUILD`         | 인스턴스를 생성 당시 이미지로부터 새롭게 만들어내는 상태                                                                   |
| `RESCUED`         | 인스턴스를 복구 모드에서 실행 중                                                                                |
| `SUSPENDED`       | 인스턴스가 관리자에 의해 최대 절전 모드로 진입한 경우                                                                    |
| `UNKNOWN`         | 인스턴스의 상태를 알 수 없는 경우<br>`인스턴스가 이 상태로 진입한 경우 관리자에게 문의합니다.`                                          |

### 인스턴스 목록 보기

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
| reservation_id | Query | String | - | 인스턴스 생성 예약 ID. <br>예약 ID를 지정하면 동시에 생성된 인스턴스 목록만 반환함 |
| changes-since | Query | Datetime | - | 지정된 시각 이후로 변경된 인스턴스 목록을 반환. `YYYY-MM-DDThh:mm:ss`의 형태. |
| image | Query | UUID | - | 이미지 ID<br>지정된 이미지를 사용한 인스턴스 목록을 반환 |
| flavor | Query | UUID | - | 인스턴스 타입 ID<br>지정된 타입을 사용한 인스턴스 목록을 반환 |
| name | Query | String | - | 인스턴스 이름<br>지정된 이름을 가진 인스턴스 목록을 반환, 정규 표현식으로 질의 가능 |
| status | Query | Enum | - | 인스턴스 상태<br>지정된 상태를 가진 인스턴스 목록을 반환 |
| limit | Query | Integer | - | 인스턴스 목록 개수<br>지정된 개수 만큼의 인스턴스 목록을 반환 |
| marker | Query | UUID | - | 목록의 첫번째 인스턴스 UUID<br>정렬 기준에 따라 `marker`로 지정된 인스턴스부터 `limit` 개수 만큼의 인스턴스 목록을 반환 |

#### 응답

| 이름 | 종류 | 형식 | 설명 |
|---|---|---|---|
| servers | Body | Object | 인스턴스 목록 객체 |
| id | Body | UUID | 인스턴스 UUID |
| links | body | Object | 인스턴스 경로 객체 |
| name | body | String | 인스턴스 이름 |

<details><summary>예시</summary>
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

### 인스턴스 목록 상세 보기

인스턴스 목록 보기와 동일하게 현재 테넌트에 생성된 인스턴스 목록을 반환합니다. 단, 인스턴스별 상세한 정보가 같이 조회됩니다.

```
GET /v2/{tenantId}/servers/detail
X-Auth-Token: {tokenId}
```

#### 요청

인스턴스 목록 보기와 동일한 요청 형태입니다.

#### 응답

| 이름 | 종류 | 형식 | 설명                                                                                                                                                                                                        |
|---|---|---|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| servers | body | Object | 인스턴스 목록 객체                                                                                                                                                                                                |
| status | body | Enum | 인스턴스 상태                                                                                                                                                                                                   |
| servers.id | Body | UUID | 인스턴스 ID                                                                                                                                                                                                   |
| servers.name | Body | String | 인스턴스 이름, 최대 255자                                                                                                                                                                                          |
| servers.updated | Body | Datetime | 인스턴스 최종 수정 시각, `YYYY-MM-DDThh:mm:ssZ` 형식                                                                                                                                                                  |
| servers.hostId | Body | String | 인스턴스가 구동 중인 호스트 ID                                                                                                                                                                                        |
| servers.addresses | Body | Object | 인스턴스 IP 목록 객체. <br>인스턴스에 연결된 포트 수 만큼 목록이 생성됨.                                                                                                                                                             |
| servers.addresses."Network 이름" | Body | Object | 인스턴스에 연결된 Network별 포트 정보                                                                                                                                                                                  |
| servers.addresses."Network 이름".OS-EXT-IPS-MAC:mac_addr | Body | String | 인스턴스에 연결된 포트의 MAC 주소                                                                                                                                                                                      |
| servers.addresses."Network 이름".version | Body | Integer | 인스턴스에 연결된 포트의 IP 버전<br>NHN Cloud는 IPv4만 지원                                                                                                                                                                |
| servers.addresses."Network 이름".addr | Body | String | 인스턴스에 연결된 포트의 IP 주소                                                                                                                                                                                       |
| servers.addresses."Network 이름".OS-EXT-IPS:type | Body | Enum | 포트의 IP 주소 타입<br>`fixed` 또는 `floating` 중 하나                                                                                                                                                                |
| servers.links | Body | Object | 인스턴스 경로 객체                                                                                                                                                                                                |
| servers.key_name | Body | String | 인스턴스 키페어 이름                                                                                                                                                                                               |
| servers.image | Body | Object | 인스턴스 이미지 객체                                                                                                                                                                                               |
| servers.image.id | Body | UUID | 인스턴스 이미지 ID                                                                                                                                                                                               |
| servers.image.links | Body | Object | 인스턴스 이미지 경로 객체                                                                                                                                                                                            |
| servers.OS-EXT-STS:task_state | Body | String | 인스턴스 작업 상태<br>인스턴스에 동작을 가했을 때 동작 진행 상태를 알려줌                                                                                                                                                               |
| servers.OS-EXT-STS:vm_state | Body | String | 인스턴스 현재 상태                                                                                                                                                                                                |
| servers.OS-SRV-USG:launched_at | Body | Datetime | 인스턴스 마지막 부팅 시각<br>`YYYY-MM-DDThh:mm:ss.ssssss` 형식                                                                                                                                                         |
| servers.OS-SRV-USG:terminated_at | Body | Datetime | 인스턴스 삭제 시각<br>`YYYY-MM-DDThh:mm:ssZ` 형식                                                                                                                                                                   |
| servers.flavor | Body | Object | 인스턴스 타입 정보 객체                                                                                                                                                                                             |
| servers.flavor.id | Body | UUID | 인스턴스 타입 ID                                                                                                                                                                                                |
| servers.flavor.links | Body | Object | 인스턴스 타입 경로 객체                                                                                                                                                                                             |
| servers.security_groups | Body | Object | 인스턴스에 할당된 보안 그룹 목록 객체                                                                                                                                                                                     |
| servers.security_groups.name | Body | String | 인스턴스에 할당된 보안 그룹 이름                                                                                                                                                                                        |
| servers.user_id | Body | String | 인스턴스를 생성한 사용자 ID                                                                                                                                                                                          |
| servers.created | Body | Datetime | 인스턴스 생성 시각. `YYYY-MM-DDThh:mm:ssZ` 형식                                                                                                                                                                     |
| servers.tenant_id | Body | String | 인스턴스가 속한 테넌트 ID                                                                                                                                                                                           |
| servers.OS-DCF:diskConfig | Body | Enum | 인스턴스 블록 스토리지 파티션 방식으로, `MANUAL` 또는 `AUTO` 중 하나<br>**AUTO**: 자동으로 전체 블록 스토리지를 하나의 파티션으로 설정<br>**MANUAL**: 이미지에 지정된 대로 파티션을 설정. 이미지에서 설정된 크기보다 블록 스토리지의 크기가 더 큰 경우 사용하지 않은 채로 남겨 둠. NHN Cloud는 `MANUAL`를 사용 |
| servers.os-extended-volumes:volumes_attached | Body | Object | 인스턴스에 연결된 추가 블록 스토리지 목록 객체                                                                                                                                                                                |
| servers.os-extended-volumes:volumes_attached.id | Body | UUID | 인스턴스에 연결된 추가 블록 스토리지 ID                                                                                                                                                                                   |
| servers.OS-EXT-STS:power_state | Body | Integer | 인스턴스의 전원 상태<br>- `1`: On<br>- `4`: Off                                                                                                                                                                    |
| servers.metadata | Body | Object | 인스턴스 메타데이터 객체<br>인스턴스 메타데이터를 키-값 쌍으로 보관                                                                                                                                                                   |

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

### 인스턴스 보기

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

| 이름 | 종류 | 형식 | 설명                                                                                                                                                                                                       |
|---|---|---|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| server | body | Object | 인스턴스 객체                                                                                                                                                                                                  |
| status | body | Enum | 인스턴스 상태                                                                                                                                                                                                  |
| server.id | Body | UUID | 인스턴스 ID                                                                                                                                                                                                  |
| server.name | Body | String | 인스턴스 이름, 최대 255자                                                                                                                                                                                         |
| server.updated | Body | Datetime | 인스턴스 최종 수정 시각, `YYYY-MM-DDThh:mm:ssZ` 형식                                                                                                                                                                 |
| server.hostId | Body | String | 인스턴스가 구동 중인 호스트 ID                                                                                                                                                                                       |
| server.addresses | Body | Object | 인스턴스 IP 목록 객체 <br>인스턴스에 연결된 포트 수 만큼 목록이 생성됨                                                                                                                                                              |
| server.addresses."Network 이름" | Body | Object | 인스턴스에 연결된 Network별 포트 정보                                                                                                                                                                                 |
| server.addresses."Network 이름".OS-EXT-IPS-MAC:mac_addr | Body | String | 인스턴스에 연결된 포트의 MAC 주소                                                                                                                                                                                     |
| server.addresses."Network 이름".version | Body | Integer | 인스턴스에 연결된 포트의 IP 버전<br>NHN Cloud는 IPv4만 지원                                                                                                                                                               |
| server.addresses."Network 이름".addr | Body | String | 인스턴스에 연결된 포트의 IP 주소                                                                                                                                                                                      |
| server.addresses."Network 이름".OS-EXT-IPS:type | Body | Enum | 포트의 IP 주소 타입<br>`fixed` 또는 `floating` 중 하나                                                                                                                                                               |
| server.links | Body | Object | 인스턴스 경로 객체                                                                                                                                                                                               |
| server.key_name | Body | String | 인스턴스 키페어 이름                                                                                                                                                                                              |
| server.image | Body | Object | 인스턴스 이미지 객체                                                                                                                                                                                              |
| server.image.id | Body | UUID | 인스턴스 이미지 ID                                                                                                                                                                                              |
| server.image.links | Body | Object | 인스턴스 이미지 경로 객체                                                                                                                                                                                           |
| server.OS-EXT-STS:task_state | Body | String | 인스턴스 작업 상태<br>인스턴스에 동작을 가했을 때 동작 진행 상태를 알림                                                                                                                                                               |
| server.OS-EXT-STS:vm_state | Body | String | 인스턴스 현재 상태                                                                                                                                                                                               |
| server.OS-SRV-USG:launched_at | Body | Datetime | 인스턴스 마지막 부팅 시각<br>`YYYY-MM-DDThh:mm:ss.ssssss` 형식                                                                                                                                                        |
| server.OS-SRV-USG:terminated_at | Body | Datetime | 인스턴스 삭제 시각<br>`YYYY-MM-DDThh:mm:ssZ` 형식                                                                                                                                                                  |
| server.flavor | Body | Object | 인스턴스 타입 정보 객체                                                                                                                                                                                            |
| server.flavor.id | Body | UUID | 인스턴스 타입 ID                                                                                                                                                                                               |
| server.flavor.links | Body | Object | 인스턴스 타입 경로 객체                                                                                                                                                                                            |
| server.security_groups | Body | Object | 인스턴스에 할당된 보안 그룹 목록 객체                                                                                                                                                                                    |
| server.security_groups.name | Body | String | 인스턴스에 할당된 보안 그룹 이름                                                                                                                                                                                       |
| server.user_id | Body | String | 인스턴스를 생성한 사용자 ID                                                                                                                                                                                         |
| server.created | Body | Datetime | 인스턴스 생성 시각, `YYYY-MM-DDThh:mm:ssZ` 형식                                                                                                                                                                    |
| server.tenant_id | Body | String | 인스턴스가 속한 테넌트 ID                                                                                                                                                                                          |
| server.OS-DCF:diskConfig | Body | Enum | 인스턴스 블록 스토리지 파티션 방식. `MANUAL` 또는 `AUTO` 중 하나.<br>**AUTO**: 자동으로 전체 블록 스토리지를 하나의 파티션으로 설정<br>**MANUAL**: 이미지에 지정된 대로 파티션을 설정. 이미지에서 설정된 크기보다 블록 스토리지의 크기가 더 큰 경우 사용하지 않은 채로 남겨 둠. NHN Cloud는 `MANUAL`를 사용 |
| server.os-extended-volumes:volumes_attached | Body | Object | 인스턴스에 연결된 추가 블록 스토리지 목록 객체                                                                                                                                                                               |
| server.os-extended-volumes:volumes_attached.id | Body | UUID | 인스턴스에 연결된 추가 블록 스토리지 ID                                                                                                                                                                                  |
| server.OS-EXT-STS:power_state | Body | Integer | 인스턴스의 전원 상태<br>- `1`: On<br>- `4`: Off                                                                                                                                                                   |
| server.metadata | Body | Object | 인스턴스 메타데이터 객체<br>인스턴스 메타데이터를 키-값 쌍으로 보관                                                                                                                                                                  |
| server.NHN-EXT-ATTR:ephemeral_disk_size | Body | Integer | 인스턴스에 연결된 추가 로컬 블록 스토리지 크기                                                                                                                                                                               |

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

### 인스턴스 생성하기

인스턴스를 생성합니다.

인스턴스 생성 API를 호출한 후에 인스턴스 조회를 통해 인스턴스 상태를 확인합니다.

* 인스턴스의 상태가 **ACTIVE**가 되면 인스턴스가 정상적으로 생성 완료됩니다.
* 인스턴스 상태가 **BUILDING**에서 오래 지속되거나 **ERROR**인 경우, 인스턴스 생성 매개 변수를 확인하고 다시 생성을 시도합니다.

Windows 인스턴스는 안정적인 동작을 위해 다음과 같은 생성 제약 조건이 있습니다.

* RAM이 2GB 이상인 인스턴스 타입을 사용합니다.
* 50GB 이상의 루트 블록 스토리지가 필요합니다.
* U2 타입은 Windows 이미지를 사용할 수 없습니다.

루트 블록 스토리지 크기는 Linux는 10GB, Windows는 50GB부터 지정할 수 있습니다.


```
POST /v2/{tenantId}/servers
X-Auth-Token: {tokenId}
```

#### 요청

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tenantId | URL | String | O | 테넌트 ID |
| tokenId | Header | String | O | 토큰 ID |
| server.security_groups | body | Object | - | 보안 그룹 목록 객체<br>생략할 경우 `default` 그룹이 추가됨 |
| server.security_groups.name | body | String | - | 인스턴스에 추가할 보안 그룹 이름 |
| server.user_data | body | String | - | 인스턴스 부팅 후 실행할 스크립트 및 설정<br>base64 인코딩된 문자열로 65535 바이트까지 허용 |
| server.availability_zone | body | String | - | 인스턴스를 생성할 가용성 영역<br>지정하지 않을 경우 임의로 선택됨 |
| server.imageRef | Body | String | O | 인스턴스를 생성할 때 사용할 이미지 ID |
| server.flavorRef | Body | String | O | 인스턴스를 생성할 때 사용할 인스턴스 타입 ID |
| server.networks | Body | Object | O | 인스턴스를 생성할 때 사용할 네트워크 정보 객체<br>지정한 개수만큼 NIC이 추가되며, 네트워크 ID, 서브넷 ID, 포트 ID, 고정 IP 중 하나로 지정 |
| server.networks.uuid | Body | UUID | - | 인스턴스를 생성할 때 사용할 네트워크 ID |
| server.networks.subnet | Body | UUID | - | 인스턴스를 생성할 때 사용할 네트워크의 서브넷 ID |
| server.networks.port | Body | UUID | - | 인스턴스를 생성할 때 사용할 포트 ID |
| server.networks.fixed_ip | Body | String | - | 인스턴스를 생성할 때 사용할 고정 IP |
| server.name | Body | String | O | 인스턴스의 이름<br>영문자 기준 255자까지 허용되지만, Windows 이미지의 경우 15자 이하여야 함 |
| server.metadata | Body | Object | - | 인스턴스에 추가할 메타데이터 객체<br>최대 길이 255자 이하의 키-값 쌍 |
| server.block_device_mapping_v2 | Body | Object | - | 인스턴스의 블록 스토리지 정보 객체<br>**로컬 블록 스토리지를 사용하는 U2 외의 인스턴스 타입을 사용할 경우 반드시 지정해야 함** |
| server.block_device_mapping_v2.uuid | Body | String | - | 블록 스토리지의 원본 ID <br>루트 블록 스토리지인 경우 반드시 부팅 가능한 원본이어야 하며, 이미지 생성이 불가능한 WAF, MS-SQL, MySQL 이미지가 원본인 volume이나 snapshot은 사용할 수 없음<br> `image`를 제외한 원본은 생성할 인스턴스의 가용성 영역이 같아야 함 |
| server.block_device_mapping_v2.source_type | Body | Enum | - | 생성할 블록 스토리지 원본의 타입<br>`image`: 이미지를 이용해 블록 스토리지 생성<br>`volume`: 기존에 생성된 블록 스토리지로 사용, destination_type은 반드시 volume으로 지정<br>`snapshot`: 스냅숏을 이용해 블록 스토리지 생성, destination_type은 반드시 volume으로 지정 |
| server.block_device_mapping_v2.destination_type | Body | Enum | - | 인스턴스 블록 스토리지의 위치, 인스턴스 타입에 따라 다르게 설정 필요.<br>- `local`: U2 인스턴스 타입을 이용하는 경우<br>- `volume`: U2 외의 인스턴스 타입을 이용하는 경우 |
| server.block_device_mapping_v2.volume_type | Body | String | - | 생성되는 블록 스토리지의 타입<br>-`General HDD`: HDD 타입의 블록 스토리지<br>-`General SSD`: SSD 타입의 블록 스토리지<br> 생략할 경우 `General HDD`로 적용됨 |
| server.block_device_mapping_v2.delete_on_termination | Body | Boolean | - | 인스턴스 삭제 시 블록 스토리지 처리 여부, 기본값은 `false`.<br>`true`면 삭제, `false`면 유지 |
| server.block_device_mapping_v2.boot_index | Body | Integer | - | 지정한 블록 스토리지의 부팅 순서<br>-`0`이면 루트 블록 스토리지<br>- 그 외는 추가 블록 스토리지<br>크기가 클수록 부팅 순서는 낮아짐 |
| server.key_name | Body | String | O | 인스턴스 접속에 사용할 키페어 |
| server.min_count | Body | Integer | - | 현재 요청으로 생성할 인스턴스 개수의 최솟값.<br>기본값은 1. |
| server.max_count | Body | Integer | - | 현재 요청으로 생성할 인스턴스 개수의 최댓값.<br>기본값은 min_count, 최댓값은 10. |
| server.return_reservation_id | Body | Boolean | - | 인스턴스 생성 요청 예약 ID.<br>True로 지정하면 인스턴스 생성 정보 대신 예약 ID를 반환.<br>기본값은 False |

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

| 이름 | 종류 | 형식 | 설명                                                                                                                                                                                                           |
|---|---|---|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| server.security_groups.name | Body | String | 생성한 인스턴스의 보안 그룹 이름                                                                                                                                                                                           |
| server.OS-DCF:diskConfig | Body | Enum | 인스턴스 블록 스토리지 파티션 방식. `MANUAL` 또는 `AUTO` 중 하나. NHN Cloud에서는 `MANUAL`로 설정됨.<br>**AUTO**: 자동으로 전체 블록 스토리지를 하나의 파티션으로 설정<br>**MANUAL**: 이미지에 지정된 대로 파티션을 설정. 이미지에서 설정된 크기보다 블록 스토리지의 크기가 더 큰 경우 사용하지 않은 채로 남겨 둠. |
| server.id | Body | UUID | 생성한 인스턴스의 ID                                                                                                                                                                                                 |

<details><summary>예시</summary>
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

---

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
| serverId | URL | UUID | O | 삭제할 인스턴스 ID |
| tokenId | Header | String | O | 토큰 ID |

#### 응답
이 API는 응답 본문을 반환하지 않습니다.

---

## 블록 스토리지 연결 관리
### 인스턴스에 연결된 블록 스토리지 목록 보기
```
GET /v2/{tenantId}/servers/{serverId}/os-volume_attachments
X-Auth-Token: {tokenId}
```

#### 요청
이 API는 요청 본문을 요구하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|--|
| tenantId | URL | String | O | 테넌트 ID |
| serverId | URL | UUID | O | 변경할 인스턴스 ID |
| tokenId | Header | String | O | 토큰 ID |
| limit | Query | Integer | - | 조회할 목록 개수 |
| offset | Query | Integer | - | 반환할 목록의 시작점<br>전체 목록 중 offset번째 블록 스토리지부터 반환 |

#### 응답

| 이름 | 종류 | 형식 | 설명 |
|---|---|---|---|
| volumeAttachments | Body | Array | 연결 정보 객체 목록 |
| volumeAttachments.device | Body | String | 인스턴스의 블록 스토리지 이름<br>예) `/dev/vdb` |
| volumeAttachments.id | Body | UUID | 연결 정보 ID |
| volumeAttachments.serverId | Body | UUID | 인스턴스 ID |
| volumeAttachments.volumeId | Body | UUID | 블록 스토리지 ID |

<details><summary>예시</summary>
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

### 인스턴스에 연결된 블록 스토리지 보기
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
| volumeId | URL | UUID | O | 조회할 블록 스토리지 ID |
| tokenId | Header | String | O | 토큰 ID |

#### 응답

| 이름 | 종류 | 형식 | 설명 |
|---|---|---|---|
| volumeAttachment | Body | Object | 연결 정보 객체 |
| volumeAttachment.device | Body | String | 인스턴스의 블록 스토리지 이름<br>예) `/dev/vdb` |
| volumeAttachment.id | Body | UUID | 연결 정보 ID |
| volumeAttachment.serverId | Body | UUID | 인스턴스 ID |
| volumeAttachment.volumeId | Body | UUID | 블록 스토리지 ID |

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

---

### 인스턴스에 추가 블록 스토리지 연결하기
```
POST /v2/{tenantId}/servers/{serverId}/os-volume_attachments
X-Auth-Token: {tokenId}
```

#### 요청

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|--|
| tenantId | URL | String | O | 테넌트 ID |
| serverId | URL | UUID | O | 변경할 인스턴스 ID |
| tokenId | Header | String | O | 토큰 ID |
| volumeAttachment | Body | Object | O | 블록 스토리지 연결 요청 객체 |
| volumeAttachment.volumeId | Body | UUID | O | 연결할 블록 스토리지 ID |

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
| volumeAttachment.device | Body | String | 인스턴스의 블록 스토리지 이름<br>예) `/dev/vdb` |
| volumeAttachment.id | Body | UUID | 연결 정보 ID |
| volumeAttachment.serverId | Body | UUID | 인스턴스 ID |
| volumeAttachment.volumeId | Body | UUID | 블록 스토리지 ID |

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

---

### 인스턴스 블록 스토리지 연결 끊기
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
| volumeId | URL | UUID | O | 연결을 끊을 블록 스토리지 ID |
| tokenId | Header | String | O | 토큰 ID |

#### 응답
이 API는 응답 본문을 반환하지 않습니다.

---

## 인스턴스 추가 기능
NHN Cloud는 다음과 같은 인스턴스 제어 및 부가 기능을 제공합니다.

* 인스턴스 시작, 중지, 종료, 재시작
* 인스턴스 타입 변경
* 인스턴스 이미지 생성
* 보안 그룹 추가 및 삭제

### 중지된 인스턴스 시작

중지된 인스턴스를 다시 시작하고 상태를 **ACTIVE**로 변경합니다. 이 API를 호출하려면 인스턴스의 상태가 **SHUTOFF**여야 합니다.

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
| os-start | Body | none | O | 인스턴스 시작 요청 |

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

---

### 종료된 인스턴스 시작

종료된 인스턴스를 다시 시작하고 상태를 **ACTIVE**로 변경합니다. 이 API를 호출하려면 인스턴스의 상태가 **SHELVED_OFFLOADED**여야 합니다.

```
POST /v2/{tenantId}/servers/{serverId}/action
X-Auth-Token: {tokenId}
```

#### 요청
| 이름 | 종류 | 형식 | 필수 | 설명 |
|--|---|---|---|--|
| tenantId | URL | String | O | 테넌트 ID |
| serverId | URL | UUID | O | 변경할 인스턴스 ID |
| tokenId | Header | String | O | 토큰 ID |
| unshelve | Body | none | O | 인스턴스 시작 요청 |

<details><summary>예시</summary>
<p>

```json
{
  "unshelve" : null
}
```

</p>
</details>

#### 응답
이 API는 응답 본문을 반환하지 않습니다.

---

### 인스턴스 중지

인스턴스를 중지하고 상태를 **SHUTOFF**로 변경합니다. 이 API를 호출하려면 인스턴스의 상태가 **ACTIVE** 또는 **ERROR**여야 합니다.

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
| os-stop | Body | none | O | 인스턴스 중지 요청 |

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

---

### 인스턴스 종료

인스턴스를 종료하고 상태를 **SHELVED_OFFLOADED**로 변경합니다. 이 API를 호출하려면 인스턴스의 상태가 **ACTIVE**여야 합니다.

```
POST /v2/{tenantId}/servers/{serverId}/action
X-Auth-Token: {tokenId}
```

#### 요청
| 이름 | 종류 | 형식 | 필수 | 설명          |
|---|---|---|---|-------------|
| tenantId | URL | String | O | 테넌트 ID      |
| serverId | URL | UUID | O | 변경할 인스턴스 ID |
| tokenId | Header | String | O | 토큰 ID       |
| shelve | Body | none | O | 인스턴스 종료 요청  |

<details><summary>예시</summary>
<p>

```json
{
  "shelve" : null
}
```

</p>
</details>

#### 응답
이 API는 응답 본문을 반환하지 않습니다.

---

### 인스턴스 재시작

인스턴스를 재시작합니다. 재시작 방식은 **SOFT**와 **HARD**로 나눌 수 있습니다.

* **SOFT** 방식: **"우아한 연결 중지(Graceful shutdown)"**를 통해 인스턴스를 중지하고 재시작합니다. 인스턴스가 **ACTIVE** 상태여야 합니다.
* **HARD** 방식: 강제 중지 후 인스턴스를 재시작합니다. 물리 서버의 전원을 끄고 다시 켜는 것과 동일한 동작입니다. 인스턴스가 다음 상태일 때만 강제로 중지할 수 있습니다.
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

#### 요청
| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|--|
| tenantId | URL | String | O | 테넌트 ID |
| serverId | URL | UUID | O | 변경할 인스턴스 ID |
| tokenId | Header | String | O | 토큰 ID |
| reboot | Body | Object | O | 인스턴스 재부팅 요청 객체 |
| reboot.type | Body | Enum | O | 재부팅 방식, **SOFT** 또는 **HARD** |

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

---

### 인스턴스 타입 변경

인스턴스 타입을 변경합니다. 인스턴스가 **ACTIVE**이거나 **SHUTOFF** 상태일 때만 인스턴스 타입 변경할 수 있습니다. 인스턴스의 상태가 **ACTIVE**인 경우에는 인스턴스 타입 변경 과정에서 인스턴스는 중지되고 다시 시작됩니다.

사용하는 이미지나 인스턴스 타입에 따라 변경할 수 있는 타입이 제한될 수 있습니다. 자세한 변경 제약 사항은 콘솔 사용자 가이드를 참고합니다.


```
POST /v2/{tenantId}/servers/{serverId}/action
X-Auth-Token: {tokenId}
```

#### 요청
| 이름 | 종류 | 형식 | 필수 | 설명                                                                                                                                                                                                                 |
|---|---|---|---|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| tenantId | URL | String | O | 테넌트 ID                                                                                                                                                                                                             |
| serverId | URL | UUID | O | 변경할 인스턴스 ID                                                                                                                                                                                                        |
| tokenId | Header | String | O | 토큰 ID                                                                                                                                                                                                              |
| resize | Body | Object | O | 인스턴스 타입 변경 요청                                                                                                                                                                                                      |
| resize.flavorRef | Body | UUID | O | 변경할 인스턴스 타입 ID                                                                                                                                                                                                     |
| resize.OS-DCF:diskConfig | Body | Enum | - | 타입 변경 후 루트 블록 스토리지 파티션 방식. `MANUAL` 또는 `AUTO` 중 하나. NHN Cloud에서는 `MANUAL`로 설정됨.<br>**AUTO**: 자동으로 전체 블록 스토리지를 하나의 파티션으로 설정<br>**MANUAL**: 이미지에 지정된 대로 파티션을 설정. 이미지에서 설정된 크기보다 블록 스토리지의 크기가 더 큰 경우 사용하지 않은 채로 남겨 둠. |

<details><summary>예시</summary>
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

#### 응답
이 API는 응답 본문을 반환하지 않습니다.

---

### 인스턴스 이미지 생성

인스턴스로부터 이미지를 생성합니다. `U2` 타입의 인스턴스만 이 API를 통해 이미지를 생성할 수 있습니다. `U2` 타입 이외의 인스턴스 이미지 생성은 [블록 스토리지 API](/Storage/Block Storage/ko/public-api/#_22)를 참고합니다.

인스턴스의 상태가 **ACTIVE**, **SHUTOFF**, **SUSPENDED**, **PAUSED**일 때만 이미지를 생성할 수 있습니다. 이미지 생성은 데이터 정합성을 보장하기 위해 인스턴스를 중지한 상태에서 진행하는 것을 권장합니다.

이미지 생성이 성공하면 이미지 상태가 `active`로 바뀝니다. 이미지 생성이 완료되는 것을 확인하려면 이미지 조회 API를 통해 지속적으로 상태를 확인합니다.

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
| createImage.metadata | Body | Object | - | 생성할 이미지의 메타데이터<br>Key-Value 형태로 기술 |

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

이 API는 응답 본문을 반환하지 않습니다. 생성된 이미지는 응답 헤더의 `Location`으로 확인합니다.

| 이름 | 종류 | 형식 | 설명 |
|--|--|--|--|
| Location | Header | String | 생성한 이미지 URL |

---

### 보안 그룹 추가

인스턴스에 보안 그룹을 추가합니다. 추가한 보안 그룹은 인스턴스의 모든 포트에 적용됩니다.

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

---

### 보안 그룹 삭제

인스턴스에서 보안 그룹을 삭제합니다. 인스턴스의 모든 포트로부터 지정한 보안 그룹이 삭제됩니다.

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
