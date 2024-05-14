## Compute > Instance >错误代码

Response Body里默认包含了 "header" 信息。
如API调用失败，则`isSuccessful`变为 `false`。错误代码显示在 `resultCode`。 

```json
{
    "header" : {
        "isSuccessful" : true,
        "resultCode": 0,
        "resultMessage" : "SUCCESS"
    }
}
```

`resultCode`定义如下。

| isSuccessful | resultCode | resultMessage | Description |
| --- | --- | --- | --- |
| true | 0 | SUCCESS | 处理成功。根据 API，它包含了 "header" 和其他信息。
| false  | -1 | FAIL [: detail description] | 验证模块联动失败或无法执行所提交的请求，请参考detail description 内容后再重试。
| false | -2 | UNKNOWN EXCEPTION | 如发生未知错误，请联系客户中心。
| false | -3 | Permission denied [: detail description] | 没有权限， Appkey或是令牌ID的请求， 请参考detail description内容。
| false | -4 | Invalid parameters [: detail description] | 在调用API时， Request URL或是Body中有无效值，或是输入错误类型的值，请参考detail description内容重试。
| false | -5 | Nonexistent [: detail description] | 访问资源不存在，请参考detail description内容。
| false | -6 | Failed to query internal db [: detail description] | 数据查询失败，请联系客户中心。
| false | -7 | Not supported [: detail description] |不支持的功能请求，请参考detail description内容。
| false | -8 | JSON parsing error [: detail description] | Request Body的 JSON解析失败，需确认 Request Body。
| false | 10400~10499| 与实例API相关的错误信息 | resultCode的后三位是HTTP Status Code，请参考 resultMessage内容后重试。
| false | 11400~11499| 与API相关的错误信息 | resultCode的后三位是HTTP Status Code，请参考 resultMessage内容后重试。
| false | 12400~12499| 与可用区域API相关的错误信息 | resultCode的后三位是 HTTP Status Code，请参考 resultMessage内容后重试。
| false | 13400~13499| 与密钥API相关的错误信息 | resultCo的后三位是HTTP Status Code，请参考 resultMessage内容后重试。 
| false | 20001 | No available IP | 没有可分配的IP，请联系客户中心。
| false | 20004 | No accessible network | 网络查询失败，网络ID无效或不存在
| false | 20400~20499| 与网络API相关的错误信息 | resultCode的后三位是 HTTP Status Code，请参考 resultMessage内容后重试。
| false | 21400~21499| 与子网API相关的错误信息 | resultCode的后三位是HTTP Status Code，请参考 resultMessage内容后重试。
| false | 22400~22499| 与弹性IP API相关的错误信息 | resultCode的后三位是 HTTP Status Code，请参考 resultMessage内容后重试。
| false | 23400~23499| 与负载均衡 API相关的错误信息 | resultCode的后三位是HTTP Status Code，请参考 resultMessage内容后重试
| false | 24400~24499| 与安全组API相关的错误信息 | resultCode的后三位是HTTP Status Code，请参考 resultMessage内容后重试。
| false | 30400~30499| 与镜像API 相关的错误信息 | resultCode的后三位是HTTP Status Code，请参考 resultMessage内容后重试
| false | 40400~40499| 与卷API 相关的错误信息| resultCode的后三位是HTTP Status Code，请参考 resultMessage内容后重试。
| false | 50400~50499| 与令牌토큰 API相关的错误信息 | resultCode的后三位是 HTTP Status Code，请参考 resultMessage内容后重试。

