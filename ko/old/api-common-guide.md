## Reference > API 준비 가이드

Object Storage를 제외한 Compute, Network, Storage API를 사용하기 위해 필요한 작업입니다.

## API Endpoint URL

	https://api-compute.cloud.toast.com/compute/v1.0/appkeys/{appkey}

| Name   | In            | Type   | Description     |
| ------ | ------------- | ------ | --------------- |
| appkey | Path Variable | String | 상품 이용 시 발급받은 앱키 |

앱키는 다음과 같은 방법으로 발급받을 수 있습니다.

1. TOAST 콘솔 **Compute** 페이지 위쪽에서 **URL & Appkey**를 클릭합니다.
2. **URL & Appkey** 대화 상자에서 **Appkey** 값을 복사해 사용합니다.


### API Response
##### Response HTTP Status Code
모든 API 요청에 200 OK로 응답하며, JSON 형태의 Response Body를 포함합니다.

##### Response Body
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


Response body "header"의 `resultCode`는 다음과 같습니다.

| isSuccessful | resultCode  | resultMessage                            | Description                              |
| ------------ | ----------- | ---------------------------------------- | ---------------------------------------- |
| true         | 0           | SUCCESS                                  | 처리 성공. API에 따라 "header" 외 추가 정보 포함.      |
| false        | -1          | FAIL [: detail description]              | 인증 모듈 연동 실패 또는 요청을 수행할 수 없는 상태인 경우. detail description 내용에 따라 조치 후 재시도 가능. |
| false        | -2          | UNKNOWN EXCEPTION                        | 예기치 못한 오류 발생. 담당자 문의 필요.                 |
| false        | -3          | Permission denied [: detail description] | 권한이 없는 사용자, Appkey 또는 토큰 ID에 의한 요청. detail description 내용 참고. |
| false        | -4          | Invalid parameters [: detail description] | API 호출 시 Request URL 또는 Body에 필요한 값이 없거나, 잘못된 형식의 값이 입력된 경우. detail description 내용에 따라 조치 후 재시도 가능. |
| false        | -5          | Nonexistent [: detail description]       | 존재하지 않는 자원에 접근한 경우. detail description 내용 참고. |
| false        | -6          | Failed to query internal db [: detail description] | 데이터베이스 질의가 실패한 경우. 담당자 문의 필요.            |
| false        | -7          | Not supported [: detail description]     | 지원하지 않는 기능 요청. detail description 내용 참고. |
| false        | -8          | JSON parsing error [: detail description] | Request Body의 JSON 파싱 실패. Request Body 확인 필요. |
| false        | 10400~10499 | 인스턴스 API 관련 오류 메시지                       | resultCode의 뒤 세 자리는 HTTP Status Code이며, resultMessage 내용에 따라 조치 후 재시도 가능. |
| false        | 11400~11499 | API 관련 오류 메시지                            | resultCode의 뒤 세 자리는 HTTP Status Code이며, resultMessage 내용에 따라 조치 후 재시도 가능. |
| false        | 12400~12499 | 가용성 영역 API 관련 오류 메시지                     | resultCode의 뒤 세 자리는 HTTP Status Code이며, resultMessage 내용에 따라 조치 후 재시도 가능. |
| false        | 13400~13499 | 키페어 API 관련 오류 메시지                        | resultCode의 뒤 세자리는 HTTP Status Code이며, resultMessage 내용에 따라 조치 후 재시도 가능 |
| false        | 20001       | No available IP                          | 할당 가능한 IP가 없음. 담당자 문의 필요.                |
| false        | 20004       | No accessible network                    | 네트워크 조회 실패. 잘못된 네트워크 ID로 조회, 또는 접근 가능한 네트워크가 없음. |
| false        | 20400~20499 | 네트워크 API 관련 오류 메시지                       | resultCode의 뒤 세 자리는 HTTP Status Code이며, resultMessage 내용에 따라 조치 후 재시도 가능. |
| false        | 21400~21499 | 서브넷 API 관련 오류 메시지                        | resultCode의 뒤 세 자리는 HTTP Status Code이며, resultMessage 내용에 따라 조치 후 재시도 가능. |
| false        | 22400~22499 | 플로팅 IP API 관련 오류 메시지                     | resultCode의 뒤 세 자리는 HTTP Status Code이며, resultMessage 내용에 따라 조치 후 재시도 가능. |
| false        | 23400~23499 | 로드 밸런서 API 관련 오류 메시지                     | resultCode의 뒤 세 자리는 HTTP Status Code이며, resultMessage 내용에 따라 조치 후 재시도 가능. |
| false        | 24400~24499 | 보안 그룹 API 관련 오류 메시지                      | resultCode의 뒤 세 자리는 HTTP Status Code이며, resultMessage 내용에 따라 조치 후 재시도 가능. |
| false        | 30400~30499 | 이미지 API 관련 오류 메시지                        | resultCode의 뒤 세 자리는 HTTP Status Code이며, resultMessage 내용에 따라 조치 후 재시도 가능. |
| false        | 40400~40499 | 볼륨 API 관련 오류 메시지                         | resultCode의 뒤 세 자리는 HTTP Status Code이며, resultMessage 내용에 따라 조치 후 재시도 가능. |
| false        | 50400~50499 | 토큰 API 관련 오류 메시지                         | resultCode의 뒤 세 자리는 HTTP Status Code이며, resultMessage 내용에 따라 조치 후 재시도 가능. |


## 토큰 API

**토큰**은 API 사용을 위해 필수로 발급받아야 하는 인증 키이며, 모든 API는 Request에 **X-Auth-Token** Header를 추가하여 요청해야 합니다.

### API 보안 설정

인증 토큰을 발급받으려면 User Access Key ID와 Secret Access Key가 필요합니다.

1. 웹 콘솔 오른쪽 위의 계정을 클릭합니다.
2. 메뉴에서 **API 보안 설정**을 클릭합니다.
3. **API 보안 설정**에서 **User Access Key ID 발급** 버튼을 클릭합니다.
4. User Access Key ID와 Secret Access Key가 발급됩니다.

### 토큰(token) 발급

#### Method, URL
```
POST /v1.0/appkeys/{appkey}/tokens
Content-Type: application/json;charset=UTF-8
```


#### Request Body
```json
{
	"auth" : {
    	"username" : "{User Access Key ID}",
        "password" : "{Secret Access Key}"
    }
}
```

| Name               | In   | Type   | Optional | Description                        |
| ------------------ | ---- | ------ | -------- | ---------------------------------- |
| User Access Key ID | Body | String | -        | API 보안 설정에서 생성한 User Access Key ID |
| Secret Access Key  | Body | String | -        | API 보안 설정에서 발급받은 Secret Access Key |

#### Response Body
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
| Token ID  | Body | String | API 요청 시 HTTP 헤더(X-Auth-Token)에 써야 할 UUID |
| Issued at | Body | String | 토큰 발급 시간. yyyy-mm-ddTHH:MM:ssZ의 형태. 예) 2017-05-16T02:17:50.166563 |
| Expires   | Body | String | 발급한 Token의 만료 시간. yyyy-mm-ddTHH:MM:ssZ의 형태. 예) 2017-05-16T03:17:50Z |
| User ID   | Body | String | 토큰을 발급받은 사용자의 UUID                       |
| Role name | Body | String | 토큰을 발급받은 사용자에게 부여된 Role                  |

### 토큰 정보 조회
#### Method, URL
```
GET /v1.0/appkeys/{appkey}/tokens?id={tokenId}
```

| Name    | In    | Type   | Optional | Description  |
| ------- | ----- | ------ | -------- | ------------ |
| tokenId | Query | String | -        | 조회할 Token ID |

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
| Token ID  | Body | String | API 요청 시 HTTP 헤더(X-Auth-Token)에 써야 할 토큰 UUID |
| Issued at | Body | String | 토큰 발급 시간. yyyy-mm-ddTHH:MM:ssZ의 형태. 예) 2017-05-16T02:17:50.166563 |
| Expires   | Body | String | 발급한 Token의 만료 시간. yyyy-mm-ddTHH:MM:ssZ의 형태. 예) 2017-05-16T03:17:50Z |
| User ID   | Body | String | 토큰을 발급받은 사용자의 UUID                       |
| Role name | Body | String | 토큰을 발급받은 사용자에게 부여된 Role                  |
