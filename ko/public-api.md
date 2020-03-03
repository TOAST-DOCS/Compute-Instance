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

* 응답 코드
    * 200, 300

* 매개변수

> 작성 중

* 예시

```json
{
    "versions": [
        {
            "id": "v2.0",
            "links": [
                {
                    "href": "http://openstack.example.com/v2/",
                    "rel": "self"
                }
            ],
            "status": "SUPPORTED",
            "version": "",
            "min_version": "",
            "updated": "2011-01-21T11:33:21Z"
        },
        {
            "id": "v2.1",
            "links": [
                {
                    "href": "http://openstack.example.com/v2.1/",
                    "rel": "self"
                }
            ],
            "status": "CURRENT",
            "version": "2.3",
            "min_version": "2.1",
            "updated": "2013-07-23T11:33:21Z"
        }
    ]
}
```


## 인스턴스 타입

인스턴스 타입은 가상 머신의 vCPU, RAM 및 디스크 크기에 대한 정의라고 할 수 있습니다.

### 타입 목록 보기

```
GET /v2/{tenantId}/flavors
X-Auth-Token: {tokenId}
```

#### 요청

이 API는 요청 본문을 요구하지 않습니다.

| 이름     | 종류    | 속성    | 필수     | 설명                                                         |
| -------- | ------ | ------- | -------- | ------------------------------------------------------------ |
| tokenId  | Header | String  | O        | 토큰 ID                                                      |
| tenantId | URI    | String  | O        | 테넌트 ID                                                    |
| minDisk  | Query  | Integer | -        | 최소 디스크 크기<br>지정한 크기보다 디스크 크기가 큰 타입만 반환 |
| minRam   | Query  | Integer | -        | 최소 RAM 크기<br>지정한 크기보다 RAM 크기가 큰 타입만 반환   |
| limit    | Query  | Integer | -        | 타입 목록 갯수<br>지정된 갯수 만큼의 타입 목록을 반환        |
| marker   | Query  | UUID    | -        | 목록의 첫번째 타입 ID<br/>정렬 기준에 따라 `marker`로 지정된 서버부터 `limit` 갯수 만큼의 서버 목록을 반환 |

#### 응답

| 이름 | 종류 | 속성 | 설명 |
|---|---|---|---|
| X-Compute-Request-ID | Header | String | API 호출마다 할당되는 구분자<br/>Request ID와 관련된 로그를 검색할 때 사용 |
| flavors | Body | Object | 타입 목록 객체 |
| id | Body | UUID | 타입 ID |
| links | Body | Object | 타입 경로 객체 |
| name | Body | String | 타입 이름 |

#### 예시

<details><summary>펼쳐 보기</summary>
<p>

```json
{
    "flavors": [
        {
            "id": "1",
            "links": [
                {
                    "href": "http://openstack.example.com/v2/openstack/flavors/1",
                    "rel": "self"
                },
                {
                    "href": "http://openstack.example.com/openstack/flavors/1",
                    "rel": "bookmark"
                }
            ],
            "name": "m1.tiny"
        },
        {
            "id": "2",
            "links": [
                {
                    "href": "http://openstack.example.com/v2/openstack/flavors/2",
                    "rel": "self"
                },
                {
                    "href": "http://openstack.example.com/openstack/flavors/2",
                    "rel": "bookmark"
                }
            ],
            "name": "m1.small"
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

| 이름 | 종류 | 속성 | 필수 | 설명 |
|---|---|---|---|---|
| tokenId | Header | String | O | 토큰 ID |
| tenantId | URI | String | O | 테넌트 ID |

#### 응답
| 이름 | 종류 | 속성 | 설명 |
|---|---|---|---|
| availabilityZoneInfo | Body | Object | 가용성 영역 정보 객체 |
| hosts | Body | - | 가용성 영역에 속한 호스트 정보 객체<br>일반 사용자에게는 null로 표시 |
| zoneName | Body | String | 가용성 영역 이름 |
| zoneState | Body | Object | 가용성 영역 상태 정보 객체 |
| available | Body | Object | 가용성 영역 상태 |

#### 예시
<details><summary>펼쳐 보기</summary>
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

| 이름 | 종류 | 속성 | 필수 | 설명 |
|---|---|---|---|---|
| tokenId | Header | String | O | 토큰 ID |
| tenantId | URI | String | O | 테넌트 ID |

#### 응답

| 이름 | 종류 | 속성 | 설명 |
|---|---|---|---|
| keypairs | Body | Array | 키페어 객체 목록 |
| keypairs.keypair | Body | Object | 키페어 객체 |
| keypairs.keypair.name | Body | String | 키페어 이름 |
| keypairs.keypair.public_key | Body | String | 공개키 |
| keypairs.keypair.fingerprint | Body | String | 키페어 지문 |

#### 예시
<details><summary>펼쳐 보기</summary>
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

| 이름 | 종류 | 속성 | 필수 | 설명 |
|---|---|---|---|---|
| tokenId | Header | String | O | 토큰 ID |
| tenantId | URI | String | O | 테넌트 ID |
| keypairName | URI | String | O | 키페어 이름 |

#### 응답

| 이름 | 종류 | 속성 | 설명 |
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

#### 예시
<details><summary>펼쳐 보기</summary>
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

| 이름 | 종류 | 속성 | 필수 | 설명 |
|---|---|---|---|---|
| tokenId | Header | String | O | 토큰 ID |
| tenantId | URI | String | O | 테넌트 ID |
| keypair | Body | Object | O | 키페어 객체 |
| keypair.name | Body | String | O | 생성 또는 등록할 키페어 이름 |
| keypair.public_key | Body | String | - | 공개키 |

#### 예시
<details><summary>펼쳐 보기</summary>
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

| 이름 | 종류 | 속성 | 설명 |
|---|---|---|---|
| keypair | Body | Object | 키페어 객체 목록 |
| keypair.public_key | Body | String | 공개키 |
| keypair.user_id | Body | String | 키 소유주 ID |
| keypair.name | Body | String | 키페어 이름 |
| keypair.fingerprint | Body | String | 키페어 지문 |

#### 예시
<details><summary>펼쳐 보기</summary>
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

| 이름 | 종류 | 속성 | 필수 | 설명 |
|---|---|---|---|---|
| tokenId | Header | String | O | 토큰 ID |
| tenantId | URI | String | O | 테넌트 ID |
| keypairName | URI | String | O | 키페어 이름 |

#### 응답
이 API는 응답 본문을 반환하지 않습니다.


## 서버

TOAST 기본 인프라 서비스의 인스턴스는 내부적으로 **서버** 라는 이름으로 통용됩니다. 인스턴스 서비스 API에서 가장 기본이 되는 리소스 입니다.

### 서버 상태

서버는 다양한 상태를 가지며, 상태에 따라 취할 수 있는 동작이 정해져 있습니다. 가능한 상태 목록은 다음과 같습니다.

| 상태 명 | 설명 |
|--|--|
| `ACTIVE` | 서버가 활성 상태인 경우 |
| `BUILDING` | 서버가 생성 중인 경우 |
| `DELETED`| 서버가 삭제된 경우 |
| `ERROR`| 직전 서버가 취한 동작이 실패한 경우 |
| `HARD_REBOOT`| 서버를 강제 재시작한 경우. 강제 재시작하면 마치 물리 서버의 전원 케이블을 뽑았다가 다시 연결하고 켜는 것과 유사한 효과를 냄 |
| `PAUSED`| 서버가 일시정지된 경우. 일시정지된 서버는 하이퍼바이저의 메모리에 저장됨 |
| `REBOOT`| 서버를 재시작한 경우 |
| `REBUILD`| 서버를 생성 당시 이미지로부터 새롭게 만들어내는 경우 |
| `RESCUED`| 서버를 복구 모드에서 실행한 경우 |
| `RESIZED`| 서버 타입을 변경하거나 다른 호스트로 옮기는 경우. 이 때 서버는 순간적으로 종료되었다가 다시 켜지게 됨 |
| `REVERT_RESIZE`| 서버 타입을 변경하거나 다른 호스트로 옮겨가는 과정에서 실패했을 때, 원 상태로 돌아가기 위해 복구하는 경우 |
| `VERIFY_RESIZE`| 서버가 타입 변경 또는 다른 호스트로 옮기는 과정을 마치고 사용자의 승인을 기다리는 경우. TOAST 기본 인프라 서비스의 인스턴스는 이 경우 1초 뒤 자동으로 `ACTIVE` 상태로 전이함 |
| `STOPPED`| 서버가 종료된 경우 |
| `SUSPENDED`| 서버가 관리자에 의해 최대 절전 모드로 진입한 경우 |
| `UNKNOWN`| 서버의 상태를 알 수 없는 경우. 관리하는 서버가 이 상태로 진입한 경우 즉시 관리자에게 문의할 것 |

### 서버 목록 보기

현재 테넌트에 생성된 서버 목록을 가져옵니다.

```
GET /v2/{tenantId}/servers
X-Auth-Token: {tokenId}
```

#### 요청

이 API는 요청 본문을 요구하지 않습니다.

| 이름 | 종류 | 속성 | 필수 | 설명 |
| ------------- | ------ | -------- | -------- | ------------------------------------------------------------ |
| tokenId       | Header | String   | O        | 토큰 ID                                                      |
| tenantId      | URI    | String   | O        | 테넌트 ID                                                    |
| changes-since | Query  | Datetime | -        | 마지막 서버 변경 시각<br>지정된 시각 이후로 변경된 서버 목록을 반환 |
| image         | Query  | UUID     | -        | 이미지 ID<br>지정된 이미지를 사용한 서버 목록을 반환         |
| flavor        | Query  | UUID     | -        | 서버 타입 ID<br>지정된 타입을 사용한 서버 목록을 반환        |
| name          | Query  | String   | -        | 서버 이름<br>지정된 이름을 가진 서버 목록을 반환<br>정규 표현식으로 질의 가능 |
| status        | Query  | Enum     | -        | 서버 상태<br>지정된 상태를 가진 서버 목록을 반환             |
| limit         | Query  | Integer  | -        | 서버 목록 갯수<br>지정된 갯수 만큼의 서버 목록을 반환        |
| marker        | Query  | UUID     | -        | 목록의 첫번째 서버 UUID<br>정렬 기준에 따라 `marker`로 지정된 서버부터 `limit` 갯수 만큼의 서버 목록을 반환 |

#### 응답

| 이름 | 종류 | 속성 | 설명 |
| ---------------------------- | ------ | ------ | ------------------------------------------------------------ |
| X-Compute-Request-ID | Header | String | API 호출마다 할당되는 구분자<br>Request ID와 관련된 로그를 검색할 때 사용 |
| servers | Body | Object | 서버 목록 객체 |
| id | Body | UUID | 서버 UUID |
| links | body | Object | 서버 경로 객체 |

### 서버 목록 상세 보기

앞서 서버 목록 보기와 비슷하게 현재 테넌트에 생성된 서버 목록을 반환한다. 단, 서버별 상세한 정보를 같이 조회할 수 있다.

#### 요청

이 API는 요청 본문을 요구하지 않습니다.

| 이름 | 종류 | 속성 | 필수 | 설명 |
|---|---|---|---|---|
| tokenId | Header | String | O | 토큰 ID |
| tenantId | URI | String | O | 테넌트 ID |
| changes-since | Query | Datetime | - | 마지막 서버 변경 시각<br>지정된 시각 이후로 변경된 서버 목록을 반환 |
| image | Query | UUID | - | 이미지 ID<br>지정된 이미지를 사용한 서버 목록을 반환 |
| flavor | Query | UUID | - | 서버 타입 ID<br>지정된 타입을 사용한 서버 목록을 반환 |
| name | Query | String | - | 서버 이름<br>지정된 이름을 가진 서버 목록을 반환 정규 표현식으로 질의 가능 |
| status | Query | Enum | - | 서버 상태<br>지정된 상태를 가진 서버 목록을 반환 |
| limit | Query | Integer | - | 서버 목록 갯수<br> 지정된 갯수 만큼의 서버 목록을 반환 |
| marker | Query | UUID | - | 목록의 첫번째 서버 UUID<br>정렬 기준에 따라 `marker`로 지정된 서버부터 `limit` 갯수 만큼의 서버 목록을 반환 |

#### 응답

| 이름 | 종류 | 속성 | 설명 |
|---|---|---|---|
| X-Compute-Request-ID | Header | String | API 호출마다 할당되는 구분자<br>Request ID와 관련된 로그를 검색할 때 사용 |
| servers | body | Object | 서버 목록 객체 |
| status | body | Enum | [서버 상태](#_9) 참조 |
| servers.updated | Body | Datetime | 서버의 최종 수정 시각<br>yyyy-mm-ddTHH:MM:SSZ 형식 |
| servers.hostId | Body | String | 서버가 구동 중인 호스트 구분자 |
| servers.addresses | Body | Object | 서버 IP 목록 객체<br>서버에 연결된 포트 수 만큼 목록이 생성됨 |
| servers.addresses."Network 이름" | Body | Object | 서버에 연결된 Network별 포트 정보 |
| servers.addresses."Network 이름".OS-EXT-IPS-MAC:mac_addr | Body | String | 서버에 연결된 포트의 MAC 주소 |
| servers.addresses."Network 이름".version | Body | Integer | 서버에 연결된 포트의 IP 버전<br>TOAST의 경우 오직 IPv4만 지원 |
| servers.addresses."Network 이름".addr | Body | String | 서버에 연결된 포트의 IP 주소 |
| servers.addresses."Network 이름".OS-EXT-IPS:type | Body | Enum | 포트의 IP 주소 타입<br>`fixed` 또는 `floating` 중 하나 |
| servers.links | Body | Object | 서버 경로 객체 |
| servers.key_name | Body | String | 서버의 접속키 이름 |
| servers.image | Body | Object | 서버의 이미지 정보 객체 |
| servers.image.id | Body | UUID | 서버의 이미지 ID |
| servers.image.links | Body | Object | 서버의 이미지 경로 객체 |
| servers.OS-EXT-STS:task_state | Body | String | 서버의 작업 상태<br>서버에 동작을 가했을 때 동작 진행 상태를 알려줌 |
| servers.OS-EXT-STS:vm_state | Body | String | 서버의 구동 상태<br>서버의 현재 상태를 알려줌 |
| servers.OS-SRV-USG:launched_at | Body | Datetime | 서버의 마지막 부팅 시각<br>yyyy-mm-ddTHH:MM:SS.ssssss 형식 |
| servers.flavor | Body | Object | 서버 타입 정보 객체 |
| servers.flavor.id | Body | UUID | 서버 타입 ID |
| servers.flavor.links | Body | Object | 서버 타입 경로 객체 |
| servers.id | Body | UUID | 서버의 ID |
| servers.security_groups | Body | Object | 서버에 할당된 보안 그룹 목록 객체 |
| servers.security_groups.name | Body | String | 서버에 할당된 보안 그룹 이름 |
| servers.OS-SRV-USG:terminated_at | Body | Datetime | 서버의 삭제 시각<br>yyyy-mm-ddTHH:MM:SSZ 형식 |
| servers.user_id | Body | String | 서버를 생성한 사용자 ID |
| servers.name | Body | String | 서버의 이름<br>최대 255자 제한 |
| servers.created | Body | Datetime | 서버 생성 시각<br>yyy-mm-ddTHH:MM:SSZ 형식 |
| servers.tenant_id | Body | String | 서버가 속한 테넌트 ID |
| servers.OS-DCF:diskConfig | Body | Enum | 서버의 디스크 설정 모드<br>`MANUAL` 또는 `AUTO` 중 하나<br>TOAST에서는 `MANUAL`만 사용 |
| servers.os-extended-volumes:volumes_attached | Body | Object | 서버에 연결된 추가 볼륨 목록 객체 |
| servers.os-extended-volumes:volumes_attached.id | Body | UUID | 서버에 연결된 추가 볼륨 ID |
| servers.OS-EXT-STS:power_state | Body | Integer | 서버의 전원 상태<br>- 1: On<br>- 4: Off |
| servers.metadata | Body | Object | 서버의 메타데이터 객체<br>서버의 기타 정보를 키-값 쌍으로 보관 |

#### 예시

<details><summary>펼쳐 보기</summary>
<p>

```json
{
  "servers": [
    {
      "status": "ACTIVE",
      "updated": "2020-01-17T02:02:39Z",
      "hostId": "183dd3f951dc6a5f6f68c9041ab8aa63dc0e15dd08f8f816b45e21b3",
      "addresses": {
        "Default Network": [
          {
            "OS-EXT-IPS-MAC:mac_addr": "fa:16:3e:15:40:e7",
            "version": 4,
            "addr": "192.168.0.181",
            "OS-EXT-IPS:type": "fixed"
          },
          {
            "OS-EXT-IPS-MAC:mac_addr": "fa:16:3e:15:40:e7",
            "version": 4,
            "addr": "133.186.144.46",
            "OS-EXT-IPS:type": "floating"
          }
        ]
      },
      "links": [
        {
          "href": "http://nova.iaas.tcc1.cloud.toastoven.net:8774/v2/6cdebe3eb0094910bc41f1d42ebe4cb7/servers/64eded6f-ac50-492f-a720-e454c05ab5df",
          "rel": "self"
        },
        {
          "href": "http://nova.iaas.tcc1.cloud.toastoven.net:8774/6cdebe3eb0094910bc41f1d42ebe4cb7/servers/64eded6f-ac50-492f-a720-e454c05ab5df",
          "rel": "bookmark"
        }
      ],
      "key_name": "really",
      "image": {
        "id": "eaf0b7ea-be90-45b8-8086-93374bbb4e72",
        "links": [
          {
            "href": "http://nova.iaas.tcc1.cloud.toastoven.net:8774/6cdebe3eb0094910bc41f1d42ebe4cb7/images/eaf0b7ea-be90-45b8-8086-93374bbb4e72",
            "rel": "bookmark"
          }
        ]
      },
      "OS-EXT-STS:task_state": null,
      "OS-EXT-STS:vm_state": "active",
      "OS-SRV-USG:launched_at": "2020-01-17T02:02:38.000000",
      "flavor": {
        "id": "edc79d63-98c3-4b77-a2d4-482d70e6b554",
        "links": [
          {
            "href": "http://nova.iaas.tcc1.cloud.toastoven.net:8774/6cdebe3eb0094910bc41f1d42ebe4cb7/flavors/edc79d63-98c3-4b77-a2d4-482d70e6b554",
            "rel": "bookmark"
          }
        ]
      },
      "id": "64eded6f-ac50-492f-a720-e454c05ab5df",
      "security_groups": [
        {
          "name": "default"
        }
      ],
      "OS-SRV-USG:terminated_at": null,
      "OS-EXT-AZ:availability_zone": "kr-pub-a",
      "user_id": "0942d2bbc6464adfa70f2ad4d04a37ef",
      "name": "YG-Win2016-2",
      "created": "2020-01-17T01:56:17Z",
      "tenant_id": "6cdebe3eb0094910bc41f1d42ebe4cb7",
      "OS-DCF:diskConfig": "MANUAL",
      "os-extended-volumes:volumes_attached": [
        {
          "id": "e94796b3-a5fe-43ca-a872-82709ef60a9c"
        }
      ],
      "accessIPv4": "",
      "accessIPv6": "",
      "progress": 0,
      "OS-EXT-STS:power_state": 1,
      "config_drive": "",
      "metadata": {
        "os_distro": "Windows",
        "description": "Windows 2016 STD (2019.10.22)",
        "os_version": "2016 STD",
        "hypervisor_type": "qemu",
        "monitoring_agent": "sysmon",
        "image_name": "Windows 2016 STD (2019.10.22)",
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

### 서버 보기

지정한 서버의 상세 정보를 반환한다.

```
GET /v2/{tenantId}/servers/{serverId}
X-Auth-Token: {tokenId}
```

#### 요청

이 API는 요청 본문을 요구하지 않습니다.

| 이름 | 종류 | 속성 | 필수 | 설명 |
|---|---|---|---|---|
| tokenId | Header | String | O | 토큰 ID |
| tenantId | URI | String | O | 테넌트 ID |
| serverId | URI | UUID | O | 인스턴스 ID |

#### 응답

| 이름 | 종류 | 속성 | 설명 |
|--|--|--|--|
| servers.status | body | Enum | [서버 상태](#_9) 참조 |
| servers.updated | body | Datetime | 서버의 최종 수정 시각<br>yyyy-mm-ddTHH:MM:SSZ 형식 |
| servers.hostId | body | String | 서버가 구동 중인 호스트 구분자 |
| servers.addresses | body | Object | 서버 IP 목록 객체 서버에 연결된 포트 수 만큼 목록이 생성됨 |
| servers.addresses."Network 이름" | body | Object | 서버에 연결된 Network별 포트 정보 |
| servers.addresses."Network 이름".OS-EXT-IPS-MAC:mac_addr | body | String | 서버에 연결된 포트의 MAC 주소 |
| servers.addresses."Network 이름".version | body | Integer | 서버에 연결된 포트의 IP 버전<br>TOAST의 경우 오직 IPv4만 지원 |
| servers.addresses."Network 이름".addr | body | String | 서버에 연결된 포트의 IP 주소 |
| servers.addresses."Network 이름".OS-EXT-IPS:type | body | Enum | 포트의 IP 주소 타입<br> `fixed` 또는 `floating` 중 하나 |
| servers.key_name | body | String | 서버의 접속키 이름 |
| servers.image  | body | Object | 서버의 이미지 정보 객체 |
| servers.image.id | body | UUID | 서버의 이미지 ID |
| servers.OS-EXT-STS:task_state | body | String | 서버의 작업 상태<br>서버에 동작을 가했을 때 동작 진행 상태를 알려줌 |
| servers.OS-EXT-STS:vm_state | body | String | 서버의 구동 상태<br>서버의 현재 상태를 알려줌 |
| servers.OS-SRV-USG:launched_at | body | Datetime | 서버의 마지막 부팅 시각<br>yyyy-mm-ddTHH:MM:SS.ssssss 형식 |
| servers.flavor | body | Object | 서버 타입 정보 객체 |
| servers.flavor.id | body | UUID | 서버 타입 ID |
| servers.flavor.links | body | Object | 서버 타입 경로 객체 |
| servers.id | body | UUID | 서버의 ID |
| servers.security_groups | body | Object | 서버에 할당된 보안 그룹 목록 객체 |
| servers.security_groups.name | body | String | 서버에 할당된 보안 그룹 이름 |
| servers.OS-SRV-USG:terminated_at | body | Datetime | 서버의 삭제 시각<br>yyyy-mm-ddTHH:MM:SSZ 형식 |
| servers.user_id | body | String | 서버를 생성한 사용자 ID |
| server.name | body | String | 서버의 이름 최대 255자 제한 |
| servers.created | body | Datetime | 서버 생성 시각<br>yyy-mm-ddTHH:MM:SSZ 형식 |
| servers.tenant_id | body | String | 서버가 속한 테넌트 ID |
| servers.OS-DCF:diskConfig | body | Enum | 서버의 디스크 설정 모드<br>`MANUAL` 또는 `AUTO` 중 하나 TOAST에서는 `MANUAL`만 사용 |
| servers.os-extended-volumes:volumes_attached | body | Object | 서버에 연결된 추가 볼륨 목록 객체 |
| servers.os-extended-volumes:volumes_attached.id | body | UUID | 서버에 연결된 추가 볼륨 ID |
| servers.OS-EXT-STS:power_state | body | Integer | 서버의 전원 상태 - 1: On - 4: Off |
| servers.metadata | body | Object | 서버의 메타데이터 객체<br>서버의 기타 정보를 키-값 쌍으로 보관  |

#### 예시

<details><summary>펼쳐 보기</summary>
<p>

```json
{
  "server": {
    "status": "ACTIVE",
    "updated": "2020-01-17T02:02:39Z",
    "hostId": "183dd3f951dc6a5f6f68c9041ab8aa63dc0e15dd08f8f816b45e21b3",
    "addresses": {
      "Default Network": [
        {
          "OS-EXT-IPS-MAC:mac_addr": "fa:16:3e:15:40:e7",
          "version": 4,
          "addr": "192.168.0.181",
          "OS-EXT-IPS:type": "fixed"
        },
        {
          "OS-EXT-IPS-MAC:mac_addr": "fa:16:3e:15:40:e7",
          "version": 4,
          "addr": "133.186.144.46",
          "OS-EXT-IPS:type": "floating"
        }
      ]
    },
    "links": [
      {
        "href": "http://nova.iaas.tcc1.cloud.toastoven.net:8774/v2/6cdebe3eb0094910bc41f1d42ebe4cb7/servers/64eded6f-ac50-492f-a720-e454c05ab5df",
        "rel": "self"
      },
      {
        "href": "http://nova.iaas.tcc1.cloud.toastoven.net:8774/6cdebe3eb0094910bc41f1d42ebe4cb7/servers/64eded6f-ac50-492f-a720-e454c05ab5df",
        "rel": "bookmark"
      }
    ],
    "key_name": "really",
    "image": {
      "id": "eaf0b7ea-be90-45b8-8086-93374bbb4e72",
      "links": [
        {
          "href": "http://nova.iaas.tcc1.cloud.toastoven.net:8774/6cdebe3eb0094910bc41f1d42ebe4cb7/images/eaf0b7ea-be90-45b8-8086-93374bbb4e72",
          "rel": "bookmark"
        }
      ]
    },
    "OS-EXT-STS:task_state": null,
    "OS-EXT-STS:vm_state": "active",
    "OS-SRV-USG:launched_at": "2020-01-17T02:02:38.000000",
    "flavor": {
      "id": "edc79d63-98c3-4b77-a2d4-482d70e6b554",
      "links": [
        {
          "href": "http://nova.iaas.tcc1.cloud.toastoven.net:8774/6cdebe3eb0094910bc41f1d42ebe4cb7/flavors/edc79d63-98c3-4b77-a2d4-482d70e6b554",
          "rel": "bookmark"
        }
      ]
    },
    "id": "64eded6f-ac50-492f-a720-e454c05ab5df",
    "security_groups": [
      {
        "name": "default"
      }
    ],
    "OS-SRV-USG:terminated_at": null,
    "OS-EXT-AZ:availability_zone": "kr-pub-a",
    "user_id": "0942d2bbc6464adfa70f2ad4d04a37ef",
    "name": "YG-Win2016-2",
    "created": "2020-01-17T01:56:17Z",
    "tenant_id": "6cdebe3eb0094910bc41f1d42ebe4cb7",
    "OS-DCF:diskConfig": "MANUAL",
    "os-extended-volumes:volumes_attached": [
      {
        "id": "e94796b3-a5fe-43ca-a872-82709ef60a9c"
      }
    ],
    "accessIPv4": "",
    "accessIPv6": "",
    "progress": 0,
    "OS-EXT-STS:power_state": 1,
    "config_drive": "",
    "metadata": {
      "os_distro": "Windows",
      "description": "Windows 2016 STD (2019.10.22)",
      "os_version": "2016 STD",
      "hypervisor_type": "qemu",
      "monitoring_agent": "sysmon",
      "image_name": "Windows 2016 STD (2019.10.22)",
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

### 서버 생성하기

인스턴스를 생성합니다. 생성 API가 호출되었다고 해서 즉시 인스턴스가 생성되는 것은 아닙니다. 서버 단건 조회 API를 통해 서버의 `status` 필드를 관찰하여 `ACTIVE` 상태로 전이하는 것을 확인하셔야 합니다. 서버의 `status`가 `BUILDING` 상태 또는 다른 `ERROR` 상태로 전이되는 경우 잘못된 매개변수를 넣지는 않았는지 확인 바랍니다.

```
POST /v2/{tenantId}/servers
X-Auth-Token: {tokenId}
```

#### 요청

| 이름 | 종류 | 속성 | 필수 | 설명 |
|---|---|---|---|---|
| tokenId | Header | String | O | 토큰 ID |
| tenantId | URI | String | O | 테넌트 ID |
| server.security_groups | body | Object | - | 보안 그룹 목록 객체<br> 지정하지 않을 경우 `default` 그룹만 추가 |
| server.security_groups.name | body | String | - | 서버에 추가할 보안 그룹 이름 |
| server.user_data | body | String | - | 서버 부팅 후 실행할 스크립트 및 설정<br>base64 인코딩된 문자열로 65535자 이하까지 허용 |
| server.availability_zone | body | String | - | 서버를 생성할 가용 영역<br>지정하지 않을 경우 임의로 선택 |
| server.imageRef | Body | String | O | 서버를 생성할 때 사용할 이미지 ID |
| server.flavorRef | Body | String | O | 서버를 생성할 때 사용할 타입 ID |
| server.networks | Body | Object | O | 서버를 생성할 때 사용할 네트워크 정보 객체<br>지정한 갯수만큼 NIC이 추가됨<br>네트워크 ID, 포트 ID, 고정 IP 중 하나만 지정 |
| server.networks.uuid | Body | UUID | - | 서버를 생성할 때 사용할 네트워크 ID |
| server.networks.port | Body | UUID | - | 서버를 생성할 때 사용할 포트 ID |
| server.networks.fixed_ip | Body | String | - | 서버를 생성할 때 사용할 고정 IP |
| server.name | Body | String | O | 생성할 서버의 이름<br>영문자 기준 255자까지 허용<br>단 Windows 이미지의 경우 15자 이하이어야 함 |
| server.metadata | Body | Object | - | 서버에 추가할 메타데이터 객체<br>최대 길이 255자 이하의 키-값 쌍 |
| server.personality | Body | Object | - | 서버에 추가할 파일 정보 객체 |
| server.personality.path | Body | String | - | 서버에 추가할 파일 경로 |
| server.personality.content | Body | String | - | 서버에 추가할 파일 내용<br>Base64 인코딩된 문자열<br>인코딩 전 기준으로 65535자 이하까지 허용 |
| server.block_device_mapping_v2 | Body | Object | - | 서버의 블록 스토리지 정보 객체<br>TOAST의 m/c/r/t/x 타입의 스펙을 사용할 경우<br>반드시 지정해야 함 |
| server.block_device_mapping_v2.source_type | Body | Enum | - | 생성할 서버의 볼륨 원형 타입<br>- `blank`: 빈 볼륨을 생성<br>- `snapshot`: 스냅샷 기반 볼륨 생성<br>- `volume`: 다른 볼륨 기반 새 볼륨 생성<br>- `image`: 이미지 기반 볼륨 생성 |
| server.block_device_mapping_v2.destination_type | Body | Enum | - | 생성할 서버의 볼륨의 위치<br>- `local`: 호스트의 볼륨<br>- `volume`: 호스트 간 공유 가능한 NAS |
| server.block_device_mapping_v2.delete_on_termination | Body | Boolean | - | 서버 삭제시 볼륨 처리 여부<br>`true`이면 삭제, `false`이면 유지 |
| server.block_device_mapping_v2.boot_index | Body | Integer | - | 지정한 볼륨의 부팅 순서<br>-`0`이면 루트 볼륨<br>- 그 외는 추가 볼륨<br>크기가 클 수록 부팅 순서는 낮아짐 |
| server.key_name | Body | String | O | 생성할 서버의 접근 키 |

#### 응답 예시

<details><summary>펼쳐 보기</summary>
<p>

```json
{
  "server": {
    "name": "BIG.IMAGE",
    "imageRef": "9956f822-29c9-4f81-9410-0c392d9c8c24",
    "flavorRef": "a4b6a0f7-aeff-4d78-a8d5-7de9f007012d",
    "networks": [{
      "subnet": "b83863ff-0355-4c73-8c10-0bdf66a69aab"
    }],
    "metadata": {
      "login_username": "root",
      "os_architecture": "amd64",
      "hypervisor_type": "qemu",
      "os_distro": "CentOS",
      "os_type": "linux",
      "os_version": "7.1",
      "volume_size": "1000"
    },
    "availability_zone": "kr-pub-a",
    "key_name": "tcc_key",
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

```json
{
  "server": {
    "name": "web.instance",
    "imageRef": "576f91b3-da1a-4ca9-bedc-8e57b3ae0772",
    "flavorRef": "fc08ea44-77a2-40bd-9273-54ebcc742d15",
    "networks": [{
      "subnet": "ae99e027-4c82-4e16-a102-4c9948e7af2d"
    }],
    "metadata": {
      "login_username": "cirros",
      "os_architecture": "amd64",
      "os_distro": "Cirros",
      "os_type": "linux",
      "os_version": "0.4.0"
    },
    "block_device_mapping_v2": [{
      "boot_index": 1,
      "volume_size" : 20,
      "source_type" : "blank",
      "destination_type" : "volume"
    }],
    "availability_zone": "kr-pub-a",
    "key_name": "tcc-alpha-key",
    "max_count": 1,
    "min_count": 1,
    "security_groups": [{
      "name": "default"
    }]
  }
}
```

</p>
</details>


#### 응답

| Name                                 | In   | Type   | Description                  |
| ------------------------------------ | ---- | ------ | ---------------------------- |
| server.security_groups.name | Body | String | 생성한 서버의 보안 그룹 이름 |
| server.OS-DCF:diskConfig | Body | Enum | `AUTO` 또는 `MANUAL` 중 하나 |
| server.id | Body | UUID | 생성한 서버의 ID |

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
        "href": "http://nova.iaas.tcc1.cloud.toastoven.net:8774/v2/6cdebe3eb0094910bc41f1d42ebe4cb7/servers/3a005d5b-63cf-4493-bfc6-49db990b5b50",
        "rel": "self"
      },
      {
        "href": "http://nova.iaas.tcc1.cloud.toastoven.net:8774/6cdebe3eb0094910bc41f1d42ebe4cb7/servers/3a005d5b-63cf-4493-bfc6-49db990b5b50",
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

### 서버 수정하기

생성된 서버를 수정합니다. 변경할 수 있는 속성은 일부 항목으로 제한됩니다.

```
PUT /v2/{tenantId}/servers/{serverId}
X-Auth-Token: {tokenId}
```

#### 요청

| 이름 | 종류 | 속성 | 필수 | 설명 |
|---|---|---|---|---|
| tokenId | Header | String | O | 토큰 ID |
| tenantId | URI | String | O | 테넌트 ID |
| serverId | URI | UUID | O | 변경할 서버 ID |
| server.name | Body | String | - | 서버의 새로운 이름 |
| server.accessIPv4 | String | - | 서버의 새로운 고정 IP |

#### 예시

<details><summary>펼쳐 보기</summary>
<p>

```json
{
    "server": {
        "name": "new-server-test"
    }
}
```

```json
{
    "server": {
        "accessIPv4": "67.23.10.132"
    }
}
```

</p>
</details>

#### 응답
서버 보기와 동일합니다.

### 서버 삭제하기

생성된 서버를 삭제합니다.

```
DELETE /v2/{tenantId}/servers/{serverId}
X-Auth-Token: {tokenId}
```

#### 요청
이 API는 요청 본문을 요구하지 않습니다.

| 이름 | 종류 | 속성 | 필수 | 설명 |
|---|---|---|---|--|
| tokenId | Header | String | O | 토큰 ID |
| tenantId | URI | String | O | 테넌트 ID |
| serverId | URI | UUID | O | 변경할 서버 ID |

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

| 이름 | 종류 | 속성 | 필수 | 설명 |
|---|---|---|---|--|
| tokenId | Header | String | O | 토큰 ID |
| tenantId | URI | String | O | 테넌트 ID |
| serverId | URI | UUID | O | 변경할 서버 ID |
| limit | Query | Integer | - | 조회할 목록 갯수 |
| offset | Query | Integer | - | 반환될 목록의 시작점<br>전체 목록 중 offset 번째 인스턴스부터 반환 |

#### 응답

| 이름 | 종류 | 속성 | 설명 |
|---|---|---|---|
| volumeAttachments | Body | Array | 연결 정보 객체 목록 |
| volumeAttachments.device | Body | String | 인스턴스의 볼륨 이름<br>예) `/dev/vdb` |
| volumeAttachments.id | Body | UUID | 연결 정보 ID |
| volumeAttachments.serverId | Body | UUID | 인스턴스 ID |
| volumeAttachments.volumeId | Body | UUID | 볼륨 ID |

#### 예시
<details><summary>펼쳐 보기</summary>
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

| 이름 | 종류 | 속성 | 필수 | 설명 |
|---|---|---|---|--|
| tokenId | Header | String | O | 토큰 ID |
| tenantId | URI | String | O | 테넌트 ID |
| serverId | URI | UUID | O | 서버 ID |
| volumeId | URI | UUID | O | 조회할 볼륨 ID |

#### 응답

| 이름 | 종류 | 속성 | 설명 |
|---|---|---|---|
| volumeAttachment | Body | Object | 연결 정보 객체 |
| volumeAttachment.device | Body | String | 인스턴스의 볼륨 이름<br>예) `/dev/vdb` |
| volumeAttachment.id | Body | UUID | 연결 정보 ID |
| volumeAttachment.serverId | Body | UUID | 인스턴스 ID |
| volumeAttachment.volumeId | Body | UUID | 볼륨 ID |

#### 예시
<details><summary>펼쳐 보기</summary>
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

| 이름 | 종류 | 속성 | 필수 | 설명 |
|---|---|---|---|--|
| tokenId | Header | String | O | 토큰 ID |
| tenantId | URI | String | O | 테넌트 ID |
| serverId | URI | UUID | O | 변경할 서버 ID |
| volumeAttachment | Body | Object | O | 볼륨 연결 요청 객체 |
| volumeAttachment.volumeId | Body | UUID | O | 연결할 볼륨 ID |

#### 예시
<details><summary>펼쳐 보기</summary>
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

| 이름 | 종류 | 속성 | 설명 |
|---|---|---|---|
| volumeAttachment | Body | Object | 연결 정보 객체 |
| volumeAttachment.device | Body | String | 인스턴스의 볼륨 이름<br>예) `/dev/vdb` |
| volumeAttachment.id | Body | UUID | 연결 정보 ID |
| volumeAttachment.serverId | Body | UUID | 인스턴스 ID |
| volumeAttachment.volumeId | Body | UUID | 볼륨 ID |

#### 예시
<details><summary>펼쳐 보기</summary>
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

| 이름 | 종류 | 속성 | 필수 | 설명 |
|---|---|---|---|--|
| tokenId | Header | String | O | 토큰 ID |
| tenantId | URI | String | O | 테넌트 ID |
| serverId | URI | UUID | O | 서버 ID |
| volumeId | URI | UUID | O | 연결을 끊을 볼륨 ID |

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
| 이름 | 종류 | 속성 | 필수 | 설명 |
|---|---|---|---|--|
| tokenId | Header | String | O | 토큰 ID |
| tenantId | URI | String | O | 테넌트 ID |
| serverId | URI | UUID | O | 변경할 서버 ID |
| os-start | Body | - | O | 인스턴스 시작 요청 |

#### 예시
<details><summary>펼쳐 보기</summary>
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
| 이름 | 종류 | 속성 | 필수 | 설명 |
|---|---|---|---|--|
| tokenId | Header | String | O | 토큰 ID |
| tenantId | URI | String | O | 테넌트 ID |
| serverId | URI | UUID | O | 변경할 서버 ID |
| os-stop | Body | - | O | 인스턴스 종료 요청 |

#### 예시
<details><summary>펼쳐 보기</summary>
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
| 이름 | 종류 | 속성 | 필수 | 설명 |
|---|---|---|---|--|
| tokenId | Header | String | O | 토큰 ID |
| tenantId | URI | String | O | 테넌트 ID |
| serverId | URI | UUID | O | 변경할 서버 ID |
| reboot | Body | Object | O | 인스턴스 재부팅 요청 객체 |
| reboot.type | Body | Enum | O | 재부팅 방식<br>**SOFT**, **HARD** |

#### 예시
<details><summary>펼쳐 보기</summary>
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
| 이름 | 종류 | 속성 | 필수 | 설명 |
|---|---|---|---|--|
| tokenId | Header | String | O | 토큰 ID |
| tenantId | URI | String | O | 테넌트 ID |
| serverId | URI | UUID | O | 변경할 서버 ID |
| resize | Body | Object | O | 인스턴스 타입 변경 요청 |
| resize.flavorRef | Body | UUID | O | 변경할 인스턴스 타입 ID |
| resize.OS-DCF:diskConfig | Body | Enum | - | 타입 변경 후 기본 디스크 파티션 방식 지정<br>**AUTO**: 자동으로 전체 디스크를 하나의 파티션으로 설정<br>**MANUAL**: 이미지에 지정된 대로 파티션을 설정. 이미지에서 설정된 크기보다 디스크의 크기가 더 큰 경우 사용하지 않은 채로 남겨둠. |

> TOAST에서 제공하는 이미지를 사용하는 경우 `diskConfig` 값은 항상 **AUTO** 로 지정해야 합니다.

#### 예시
<details><summary>펼쳐 보기</summary>
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
| 이름 | 종류 | 속성 | 필수 | 설명 |
|---|---|---|---|--|
| tokenId | Header | String | O | 토큰 ID |
| tenantId | URI | String | O | 테넌트 ID |
| serverId | URI | UUID | O | 변경할 서버 ID |
| createImage | Body | Object | O | 이미지 생성 요청 |
| createImage.name | Body | String | O | 생성할 이미지 이름 |
| createImage.metadata | Body | Object | O | 생성할 이미지의 메타데이터<br>Key-Value 형태로 기술 |

#### 예시
<details><summary>펼쳐 보기</summary>
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

| 이름 | 종류 | 속성 | 설명 |
|--|--|--|--|
| Location | Header | String | 생성한 이미지 URL |
| image_id | Body | UUID | 생성한 이미지 ID |

#### 예시
<details><summary>펼쳐 보기</summary>
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
| 이름 | 종류 | 속성 | 필수 | 설명 |
|---|---|---|---|--|
| tokenId | Header | String | O | 토큰 ID |
| tenantId | URI | String | O | 테넌트 ID |
| serverId | URI | UUID | O | 변경할 서버 ID |
| addSecurityGroup | Body | Object | O | 보안 그룹 추가 요청 객체 |
| addSecurityGroup.name | Body | String | O | 추가할 보안 그룹 이름 |

#### 예시
<details><summary>펼쳐 보기</summary>
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
| 이름 | 종류 | 속성 | 필수 | 설명 |
|---|---|---|---|--|
| tokenId | Header | String | O | 토큰 ID |
| tenantId | URI | String | O | 테넌트 ID |
| serverId | URI | UUID | O | 변경할 서버 ID |
| removeSecurityGroup | Body | Object | O | 보안 그룹 삭제 요청 객체 |
| removeSecurityGroup.name | Body | String | O | 삭제할 보안 그룹 이름 |

#### 예시
<details><summary>펼쳐 보기</summary>
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
