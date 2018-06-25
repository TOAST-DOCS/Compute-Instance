## Compute > Instance > 오류 코드

Response Body에는 "header" 정보가 기본으로 포함되어 있습니다.
API 호출이 실패하면 `isSuccessful`이 `false`가 되며, 오류 코드가 `resultCode`에 표시됩니다. 

```json
{
    "header" : {
        "isSuccessful" : true,
        "resultCode": 0,
        "resultMessage" : "SUCCESS"
    }
}
```

`resultCode`는 아래와 같이 정의되어 있습니다.

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

