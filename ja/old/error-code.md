## Compute > Instance > エラーコード

Response Bodyには"header"情報が基本的に含まれています。
API呼び出しが失敗すると`isSuccessful`が`false`になり、エラーコードが`resultCode`に表示されます。 

```json
{
    "header" : {
        "isSuccessful" : true,
        "resultCode": 0,
        "resultMessage" : "SUCCESS"
    }
}
```

`resultCode`は下記のように定義されています。

| isSuccessful | resultCode | resultMessage | Description |
| --- | --- | --- | --- |
| true | 0 | SUCCESS | 処理成功。 APIに応じて"header"の他に追加情報を含む |
| false  | -1 | FAIL [: detail description] | 認証モジュール連携失敗またはリクエストを実行できない状態の場合。detail descriptionの内容を確認して対策を実施したら再試行が可能 |
| false | -2 | UNKNOWN EXCEPTION | 予期せぬエラー発生。担当者に問い合わせが必要 |
| false | -3 | Permission denied [: detail description] | 権限がないユーザー、 AppKey、またはトークンIDによるリクエスト。 detail description内容参照 |
| false | -4 | Invalid parameters [: detail description] | API呼び出し時、Request URLまたはBodyに必要な値がないか、誤った形式の値が入力されている場合。detail descriptionの内容を確認して対策を実施したら再試行が可能 |
| false | -5 | Nonexistent [: detail description] | 存在しないリソースにアクセスした場合、detail description内容参照 |
| false | -6 | Failed to query internal db [: detail description] | データベースクエリが失敗した場合。担当者に問い合わせが必要 |
| false | -7 | Not supported [: detail description] | サポートしていない機能をリクエスト。 detail description内容参照 |
| false | -8 | JSON parsing error [: detail description] | Request BodyのJSONパーシング失敗。 Request Body確認必要 |
| false | 10400~10499| インスタンスAPI関連エラーメッセージ | resultCodeの後ろの3桁はHTTPステータスコードで、 resultMessageの内容に応じて対策を実施したら再試行が可能 |
| false | 11400~11499| API関連エラーメッセージ | resultCodeの後ろの3桁はHTTPステータスコードで、resultMessageの内容に応じて対策を実施したら再試行が可能 |
| false | 12400~12499| アベイラビリティーゾーンAPI関連エラーメッセージ | resultCodeの後ろの3桁はHTTPステータスコードで、 resultMessageの内容に応じて対策を実施したら再試行が可能 |
| false | 13400~13499| キーペアAPI関連エラーメッセージ | resultCodeの後ろの3桁はHTTPステータスコードで、 resultMessageの内容に応じて対策を実施したら再試行が可能 |
| false | 20001 | No available IP | 割り当て可能なIPがない。担当者に問い合わせが必要 |
| false | 20004 | No accessible network | ネットワーク照会失敗。誤ったネットワークIDで照会、またはアクセス可能なネットワークがない。 |
| false | 20400~20499| ネットワークAPI関連エラーメッセージ | resultCodeの後ろの3桁はHTTPステータスコードで、 resultMessageの内容に応じて対策を実施したら再試行が可能 |
| false | 21400~21499| サブネットAPI関連エラーメッセージ | resultCodeの後ろの3桁はHTTPステータスコードで、 resultMessageの内容に応じて対策を実施したら再試行が可能 |
| false | 22400~22499| Floating IP API関連エラーメッセージ | resultCodeの後ろの3桁はHTTPステータスコードで、 resultMessageの内容に応じて対策を実施したら再試行が可能 |
| false | 23400~23499| ロードバランサーAPI関連エラーメッセージ | resultCodeの後ろの3桁はHTTPステータスコードで、 resultMessageの内容に応じて対策を実施したら再試行が可能 |
| false | 24400~24499| セキュリティーグループAPI関連エラーメッセージ | resultCodeの後ろの3桁はHTTPステータスコードで、 resultMessageの内容に応じて対策を実施したら再試行が可能 |
| false | 30400~30499| イメージAPI関連エラーメッセージ | resultCodeの後ろの3桁はHTTPステータスコードで、 resultMessageの内容に応じて対策を実施したら再試行が可能 |
| false | 40400~40499| ボリュームAPI関連エラーメッセージ | resultCodeの後ろの3桁はHTTPステータスコードで、 resultMessageの内容に応じて対策を実施したら再試行が可能 |
| false | 50400~50499| トークンAPI関連エラーメッセージ | resultCodeの後ろの3桁はHTTPステータスコードで、 resultMessageの内容に応じて対策を実施したら再試行が可能 |

