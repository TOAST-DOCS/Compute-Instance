## Compute > Instance > Error Code

Response Body includes "header" information by default. 
When API call fails, `isSuccessful` becomes `false` , with the error code displayed on `resultCode`.

```json
{
    "header" : {
        "isSuccessful" : true,
        "resultCode": 0,
        "resultMessage" : "SUCCESS"
    }
}
```

`resultCode`is defined as below: 

| isSuccessful | resultCode | resultMessage | Description |
| --- | --- | --- | --- |
| true | 0 | SUCCESS | Processing is successful. Additional information other than "header" may be added, depending on the API. |
| false  | -1 | FAIL [: detail description] | Authentication module has failed to interface or cannot request. Retry is available after measures taken, depending on detail description. |
| false | -2 | UNKNOWN EXCEPTION | Unexpected error occurred. Contact Administrator. |
| false | -3 | Permission denied [: detail description] | Requested by unauthorized use, Appkey, or token ID. Refer to detail description. |
| false | -4 | Invalid parameters [: detail description] | No valid data is available in Request URL or Body when API is called, or invalid value is entered. Retry is available after measures taken, depending on detail description. |
| false | -5 | Nonexistent [: detail description] | Accessed nonexistent resources. Refer to detail description. |
| false | -6 | Failed to query internal db [: detail description] | Failed to query database. Contact Administrator. |
| false | -7 | Not supported [: detail description] | Requested for unsupported functions. Refer to detail description |
| false | -8 | JSON parsing error [: detail description] | Json parsing of the Request Body failed. Check Request Body. |
| false | 10400~10499| Error message related to instance API | The last three digits of resultCode represent HTTP Status Code, and retry is available after measures taken in accordance with resultMessage. |
| false | 11400~11499| Error message related to API | The last three digits of resultCode represent HTTP Status Code, and retry is available after measures taken in accordance with resultMessage. |
| false | 12400~12499| Error message related to availability area API | The last three digits of resultCode represent HTTP Status Code, and retry is available after measures taken in accordance with resultMessage. |
| false | 13400~13499| Error message related to key pair API | The last three digits of resultCode represent HTTP Status Code, and retry is available after measures taken in accordance with resultMessage. |
| false | 20001 | No available IP | No available IP to allocate. Contact Administrator. |
| false | 20004 | No accessible network | Failed to retrieve network. Retrieved by a wrong network ID, or no network is accessible. |
| false | 20400~20499| Error message related to network API | The last three digits of resultCode represent HTTP Status Code, and retry is available after measures taken in accordance with resultMessage. |
| false | 21400~21499| Error message related to subnet API | The last three digits of resultCode represent HTTP Status Code, and retry is available after measures taken in accordance with resultMessage. |
| false | 22400~22499| Error message related to floating IP API | The last three digits of resultCode represent HTTP Status Code, and retry is available after measures taken in accordance with resultMessage. |
| false | 23400~23499| Error message related to load balancer API | The last three digits of resultCode represent HTTP Status Code, and retry is available after measures taken in accordance with resultMessage. |
| false | 24400~24499| Error message related to security group API | The last three digits of resultCode represent HTTP Status Code, and retry is available after measures taken in accordance with resultMessage. |
| false | 30400~30499| Error message related to image API | The last three digits of resultCode represent HTTP Status Code, and retry is available after measures taken in accordance with resultMessage. |
| false | 40400~40499| Error message related to volume API | The last three digits of resultCode represent HTTP Status Code, and retry is available after measures taken in accordance with resultMessage. |
| false | 50400~50499| Error message related to token API | The last three digits of resultCode represent HTTP Status Code, and retry is available after measures taken in accordance with resultMessage. |

