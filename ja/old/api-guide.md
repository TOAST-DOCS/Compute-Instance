## Compute > Instance > APIガイド

APIは現在、韓国リージョンでのみ使用できます。

NHN Cloud Compute Instanceサービスは次のAPIを提供します。

* [アベイラビリティーゾーンAPI](#api_2)
* [インスタンスAPI](#api_3)
* [インスタンス追加機能API](#api_4)
* [インスタンスタイプAPI](#api_5)
* [キーペアAPI](#api_6)

## 事前準備

インスタンスAPIを使用するにはアプリケーションキーとトークンが必要です。 [API Endpoint URL](#api-endpoint-url)と[トークンAPI](#api)を利用してアプリケーションキーとトークンを準備します。アプリケーションキーはAPI Endpoint URLに、トークンはRequest Bodyに含めて使用します。

### API Endpoint URL

全てのインスタンス、イメージ、ネットワーク(VPC)、ブロックストレージAPIは次のURLから始まるものを使用する必要があります。

	https：//api-compute.cloud.toast.com/compute

APIをリクエストする時は、常にアプリケーションキーを含めてリクエストする必要があります。アプリケーションキーは下記のように発行を受けることができます。

1. NHN Cloudコンソール**Compute**ページ上側で**URL & Appkey**をクリックします。
2. **URL & Appkey**ダイアログボックスで**Appkey**の値をコピーして使用します。

例えば、トークン発行URLは次のとおりです。

	POST https：//api-compute.cloud.toast.com/compute/v1.0/appkeys/{appkey}/tokens

インスタンス、イメージ、ネットワーク、ブロックストレージAPI文書では簡潔で見やすく表記するためにAPI URLの始めの部分を省略しました。

### API Response

#### Response HTTP Status Code

全てのAPIリクエストに200 OKで応答し、JSON形式のResponse Bodyを含めます。

#### Response Body

Response Bodyには「header」情報が基本的に含まれており、これにより詳細な応答結果を確認できます。APIによって「header」以外の追加情報が含まれる場合があります。

```json
{
    "header" : {
        "isSuccessful" : true,
        "resultCode" : 0,
        "resultMessage" : "SUCCESS"
    }
}
```

API呼び出しが失敗すると、「isSuccessful」が「false」になり、エラーコードが「resultCode」に表示されます。詳細なエラーコードは[エラーコード](/Compute/Instance/ja/error-code/)を参照してください。

### トークンAPI

トークンはAPIを使用するために必ず発行されなければならない認証キーで、全てのAPIはRequestに**X-Auth-Token** Headerを追加してリクエストする必要があります。

#### APIパスワード設定

APIパスワードは**Compute > Instance**サービスページの**API Endpoint設定**ボタンをクリックして設定できます。

1. **API Endpoint設定**ボタンをクリックします。
2. **API Endpoint設定**の下にある**APIパスワード設定**に、トークン発行時に使用するパスワードを入力します。
3. パスワードを入力した後、 **保存**ボタンをクリックします。

#### Token発行

##### Method、URL
```
POST /v1.0/appkeys/{appkey}/tokens
Content-Type： application/json;charset=UTF-8
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
| NHN Cloud ID | Body | String | - | NHN CloudアカウントID(Email)入力 |
| API Password | Body | String | - | **API Endpoint設定**で保存したパスワード |

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
| Token ID | Body | String | APIリクエスト時にHTTPヘッダー(X-Auth-Token)に記入する必要があるUUID |
| Issued at | Body | String |トークン発行時間。 yyyy-mm-ddTHH：MM：ssZの形式。例) 2017-05-16T02：17：50.166563 |
| Expires | Body | String |発行したTokenの満了時間。 yyyy-mm-ddTHH：MM：ssZの形式。例) 2017-05-16T03：17：50Z |
| User ID | Body | String |トークンを発行されたユーザーのUUID |
| Role name | Body | String |トークンを発行されたユーザーに付与されたRole |

#### Token情報照会
##### Method、URL
```
GET /v1.0/appkeys/{appkey}/tokens?id={tokenId}
```

|  Name | In | Type | Optional |Description |
|--|--|--|--|--|
| tokenId | Query | String | - |照会するToken ID |

##### Request Body
このAPIはRequest Bodyが必要ありません。

##### Response Body
```json
{
    "header" : {
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
| Token ID | Body | String | APIリクエスト時、 HTTPヘッダー(X-Auth-Token)に記入する必要があるトークンUUID |
| Issued at | Body | String |トークン発行時間。 yyyy-mm-ddTHH：MM：ssZの形式。例) 2017-05-16T02：17：50.166563 |
| Expires | Body | String |発行したTokenの満了時間。 yyyy-mm-ddTHH：MM：ssZの形式。例) 2017-05-16T03：17：50Z |
| User ID | Body | String |トーークンを発行されたユーザーのUUID |
| Role name | Body | String |トークンを発行されたユーザーに付与されたRole |


## アベイラビリティーゾーンAPI

### アベイラビリティーゾーン照会
インスタンス、ブロックストレージを生成できるアベイラビリティーゾーン情報を照会します。

#### Method、URL
```
GET /v1.0/appkeys/{appkey}/availability-zones
X-Auth-Token： {tokenId}
```

|  Name | In | Type | Optional | Description |
|--|--|--|--|--|
| tokenId | Header | String | - | トークンID |

#### Request Body
このAPIはRequest Bodyが必要ありません。

#### Response Body
```json
{
    "header" : {
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
| Zone Name | Body | String | アベイラビリティーゾーン名 |
| Available | Body | Boolean | アベイラビリティーゾーンを可用可否 |

## インスタンスAPI
インスタンス生成、削除、情報照会およびブロックストレージ接続管理機能を提供します。

### インスタンス状態
インスタンスは生成、変更、削除、運営中は次の状態になります。
![[図1]インスタンスStatus Diagram](http://static.toastoven.net/prod_infrastructure/compute/developersguide/img_001.png)


| 状態 | 説明 |
| --- | --- |
| BUILD | インスタンス生成中 |
| POWERING_ON | インスタンス起動中 |
| STOP | インスタンス終了状態 |
| ACTIVE | インスタンス駆動中 |
| POWERING_OFF | インスタンス終了中 |
| REBOOTING | インスタンス再起動中 |
| DELETING | インスタンス削除中 |
| RESIZING | インスタンスタイプ(flavor)変更作業中 |
| MIGRATING | インスタンスマイグレーション作業中 |
| ERROR | エラー状態 |

### インスタンス情報簡略照会
生成されているインスタンスの簡潔な情報を照会します。
#### Method、URL
```
GET /v1.0/appkeys/{appkey}/instances?id={instanceId}
X-Auth-Token： {tokenId}
```

|  Name | In | Type | Optional |Description |
|--|--|--|--|--|
| tokenId | Header | String | - | トークンID |
| instanceId | Query | String | O | 照会するインスタンスID。記載しない場合、全てのインスタンスの簡略情報を照会します。 |

#### Request Body
このAPIはRequest Bodyが必要ありません。

#### Response Body
```json
{
    "header" : {
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
| Instance ID | Body | String | インスタンスID |
| Instance Name | Body | String | インスタンス名 |
| Instance Status | Body | String | インスタンスの状態 |

### インスタンス削除照会
インスタンスの詳細な情報を照会します。
#### Method、URL
```
GET /v1.0/appkeys/{appkey}/instances-detail?id={instanceId}
X-Auth-Token： {tokenId}
```

|  Name | In | Type | Optional |Description |
|--|--|--|--|--|
| tokenId | Header | String | - | トークンID. |
| instanceId | Query | String | O | 照会するインスタンスのID。省略すると全てのインスタンスの詳細情報を照会します。 |

#### Request Body
このAPIはRequest Bodyが必要ありません。

#### Response Body
```json
{
    "header" : {
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
| MAC Address | Body | String | NICのMACアドレス |
| IP Address | Body | String | NICのIPアドレス |
| version | Body | Integer | IPバージョン(IPv4のみサポート) |
| Floating IP Address | Body | String | NICに割り当てられたFloating IPアドレス |
| Zone Name | Body | String | アベイラビリティーゾーン名 |
| Flavor ID | Body | String | インスタンスタイプ(flavor) ID |
| Flavor Name | Body | String | インスタンスタイプ(flavor) 名 |
| Flavor CPU | Body | Integer | CPU個数 |
| Flavor RAM | Body | Integer | RAMサイズ(MB) |
| Status | Body | String | インスタンスの状態 |
| Instance ID | Body | String | インスタンスID |
| Instance Name | Body | String | インスタンス名 |
| Image ID | Body | String | インスタンスにインストールされたイメージID |
| metadata | Body | Object | インスタンスに設定するユーザーメタデータで、"key"： "value"形式で保存 |
| PEM Key Name | Body | String | インスタンスに登録するキーペア名 |
| Root Volume Size | Body | Integer | インスタンス基本ブロックストレージディスクサイズ(GB) |
| Root Volume Type | Body | String | インスタンス基本ブロックストレージディスクの種類。 "General HDD"または"General SSD"のどちらか。|
| Attached Volume ID | Body | String | 追加ブロックストレージID |
| Attached Volume Name | Body | String | 追加ブロックストレージ名 |
| Attached Volume Size | Body | Integer | 追加ブロックストレージサイズ(GB) |
| Attached Volume Type | Body | String | 追加ブロックストレージディスクの種類。 "General HDD"または"General SSD"のどちらか。 |
| Security Group Name | Body | String | インスタンスに登録されたセキュリティーグループの名前 |
| Launched Time | Body | String | インスタンスが最近起動した時刻yyyy-mm-ddTHH：MM：ssZの形式。例) 2017-05-16T02：17：50.166563 |
| Created Time | Body | String | インスタンス生成時刻。 yyyy-mm-ddTHH：MM：ssZの形式。例) 2017-05-16T02：17：50.166563 |
| Updated Time | Body | String | インスタンス修正時刻。 yyyy-mm-ddTHH：MM：ssZの形式。例) 2017-05-16T02：17：50.166563 |

### インスタンス生成
新たなインスタンスを生成します。

#### Method、URL
```
POST /v1.0/appkeys/{appkey}/instances
X-Auth-Token： {tokenId}
Content-Type： application/json;charset=UTF-8
```

|  Name | In | Type | Optional | Description |
|--|--|--|--|--|
| tokenId | Header | String | - | トークンID |

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
| Instance Name | Body | String | - | インスタンス名(Linuxの場合、最大20文字、Windowsの場合は最大12文字、英数字、'-'、'.'のみ可能) |
| Image ID | Body | String | - | インスタンスにインストールするイメージID |
| Flavor ID | Body | String | - | インスタンスタイプ(flavor) ID |
| Network ID | Body | String | O | インスタンスが接続するネットワークID |
| Subnet ID | Body | String | O | インスタンスが接続されるサブネットID<br>ネットワークIDまたはサブネットIDのどちらかを指定する必要あります。 |
| Availability Zone | Body | String | - | インスタンスが生成されるアベイラビリティーゾーン名 |
| Key Name | Body | String | - | インスタンスに登録するキーペア名 |
| Count | Body | Integer | O | 同時生成するインスタンスの個数、最大10個に制限、1～10の範囲。省略すると1個生成 |
| Volume Size | Body | Integer | O | インスタンスの基本ブロックストレージサイズ、生成可能なサイズは[コンソールガイド](/Compute/Instance/ja/console-guide/#_5)を参照 |
| Volume Type | Body | String | O | インスタンスの基本ブロックストレージの種類、"General HDD"または"General SSD"のどちらか |
| Security Group Name | Body | String | - | インスタンスに登録するセキュリティーグループの名前 |

> [注意]
> * `Volume Size`、`Volume Type`パラメータはc2、m2、r2、t2仕様でのみ適用されます。
> 	*このパラメータは汎用ブロックストレージではないローカルディスクを使用するu2系列の仕様では使用できません。
> * `Volume Size`は**使用するイメージの"minDisk"値～1000の範囲内で10単位**で設定される必要があります。
> * `Volume Type`は`null`に指定することができ、この場合"General HDD"に指定されます。


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
| Instance ID | body | String |生成されたインスタンスID |
| Instance Name | body | String | インスタンス名 |
| Instance Status | Body | String | インスタンスの状態 |

### インスタンス削除
特定インスタンスを削除します。
#### Method、URL
```
DELETE /v1.0/appkeys/{appkey}/instances?id={instanceId}
X-Auth-Token： {tokenId}
```

|  Name | In | Type | Optional | Description |
|--|--|--|--|--|
| tokenId | Header | String | - | トークンID |
| instanceId | Query | String | - | 削除するインスタンスID |

### ブロックストレージ接続
インスタンスにブロックストレージを接続します。

#### Method、URL
```
POST /v1.0/appkeys/{appkey}/instances/{instanceId}/attachments
X-Auth-Token： {tokenId}
Content-Type： application/json;charset=UTF-8
```

|  Name | In | Type | Optional | Description |
|--|--|--|--|--|
| tokenId | Header | String | - | トークンID |
| instanceId | Path | String | - | ブロックストレージを接続するインスタンスID |

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
| Volume ID | body | String | - | インスタンスに接続するブロックストレージID |

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
| Device ID | body | String | インスタンスに登録されたデバイス名。例) "/dev/vdc" |
| Attachement ID | body | String | 接続ID.|
| Volume ID | body | String | ブロックストレージID。接続解除する際に必要。 |

### ブロックストレージ接続解除
インスタンスに接続されているブロックストレージ接続を解除します。

#### Method、URL
```
DELETE /v1.0/appkeys/{appkey}/instances/{instanceId}/attachments?volumeId={volumeId}
X-Auth-Token： {tokenId}
```

|  Name | In | Type | Optional | Description |
|--|--|--|--|--|
| tokenId | Header | String| - | トークンID |
| instanceId | Path | String | - | インスタンスID |
| volumeId | Path | String | - | ブロックストレージID |

#### Request body
このAPIはRequest Bodyが必要ありません。

#### Response Body

```json
{
    "header" : {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    }
}
```

## インスタンス追加機能API
次のようなインスタンス制御および付加機能を提供します。

- インスタンス起動、停止、再起動
- インスタンスタイプ変更(resize)
- インスタンスイメージ生成
- Floating IP接続、解除
- セキュリティーグループ登録、解除

### 共通
全てのインスタンス追加機能APIは同じMethod、URLで呼び出し、Request Bodyで各追加機能を区分します。
#### Method、URL
```
POST /v1.0/appkeys/{appkey}/instances/{instanceId}/action
X-Auth-Token： {tokenId}
Content-Type： application/json;charset=UTF-8
```

|  Name | In | Type | Optional | Description |
|--|--|--|--|--|
| tokenId | Header | String| - | トークンID |
| instanceId | Path | String | - | 追加機能を実行するインスタンスID |

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
| Action Name | Body | String | - | インスタンスで実行する追加機能 |
| parameters | Body | Object| O | 追加機能実行に必要なパラメータ。追加機能に応じて必要な値を記載します。一部追加機能はパラメータなしで動作します。|

### インスタンス起動
停止(STOP)状態のインスタンスを起動します。
#### Request Body
```json
{
    "action" : "start"
}
```

#### Response Body
```json
{
    "header" : {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    }
}
```

### インスタンス停止
動作中(ACTIVE)またはエラー(ERROR)状態のインスタンスを停止します。
#### Request Body
```json
{
    "action" : "stop"
}
```

#### Response Body

```json
{
    "header" : {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    }
}
```

### インスタンス再起動
インスタンスを再起動します。以下のように再起動方式を指定できます。

- **SOFT**:正常終了(graceful shutdown)後、インスタンスを再起動します。
- **HARD**:強制終了(shutdown)後、インスタンスを再起動します。

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
| Reboot Type | body | String | - | 再起動方式。 `HARD`または`SOFT`. |

#### Response Body
```json
{
    "header" : {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    }
}
```

### インスタンスタイプ変更
インスタンスタイプを変更します。
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
|  Flavor ID | body | String | - | 変更するインスタンスタイプ(flavor) ID |

#### Response Body
```json
{
    "header" : {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    }
}
```

### イメージ作成
指定したインスタンスからイメージを作成します。作成されたイメージは[イメージAPI](/Compute/Image/ja/api-guide/)で照会できます。

イメージ作成対象のインスタンスはSTOP状態である必要があります。

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
| Image Name | body | String | - | 作成するイメージ名 |

#### Response Body
```json
{
    "header" : {
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
| Created Image ID | body | String | 作成されたイメージID |
| Created Image Name | body | String |作成されたイメージ名 |

### Floating IP接続
Floating IPをインスタンスに接続します。

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
| Floating IP Address | body | String | - | インスタンスに接続するFloating IPアドレス |
| IP Address of the instance | body | String | - | Floating IPを接続するインスタンスのIPアドレス |

#### Response Body

```json
{
    "header" : {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    }
}
```

### Floating IP接続解除
インスタンスに接続されているFloating IPを接続解除します。

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
| Floating IP Address | body | String | - | 接続を解除するFloating IPアドレス |

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

### セキュリティーグループ登録
インスタンスにセキュリティーグループを追加します。

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
| Security Group Name | body | String | - | インスタンスに追加するセキュリティーグループ名 |

#### Response Body

```json
{
    "header" : {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    }
}
```

### セキュリティーグループ削除
インスタンスに登録されているセキュリティーグループを削除します。

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
| Security Group Name | body | String | - | インスタンスから削除するセキュリティーグループ名 |

#### Response Body
```json
{
    "header" : {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    }
}
```

## インスタンスタイプAPI
### インスタンスタイプのリスト照会
インスタンスタイプのリストおよび詳細情報を照会します。

#### Method、URL
```
GET /v1.0/appkeys/{appkey}/flavors
X-Auth-Token： {tokenID}
```

|  Name | In | Type | Optional | Description |
|--|--|--|--|--|
| tokenId | Header | String| - | トークンID |

#### Request Body
このAPIはRequest Bodyが必要ありません。

#### Response Body
```json
{
    "header" : {
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
| Disabled | Body | Boolean | インスタンスタイプの無効化の可否。 |
| Ephermeral | Body | Integer | 臨時ディスクサイズ(GB)。|
| Type | Body | String | インスタンスタイプが最適化される特性に応じての区分。<br>"general、"compute"、"memory"のいずれか。 |
| Volume Size | Body | Integer | インスタンス生成時、基本ディスクデバイスに作られるディスクサイズ(GB).<br>基本ディスクが固定サイズで作られるu2仕様の場合にのみこの値が渡されます。 |
| Max Volume Size | Body | Integer | 基本ディスクデバイスで作成できる最大ディスクサイズ(GB). |
| Flavor ID | Body | String | インスタンスタイプ(flavor)ID。 |
| Flavor Name | Body | String | インスタンスタイプの名前。 |
| Is Public | Body | Boolean | 共用インスタンスタイプかどうか。 |
| RAM | Body | Integer | インスタンスタイプが持つRAM総量(MB)。 |
| VCPUs | Body | Integer | インスタンスに割り当てられる仮想CPUコア個数。 |

## キーペアAPI
インスタンスのアクセスに必要なキーペアを生成、削除、照会する機能を提供します。
### キーペア照会
キーペアを照会します。
#### Method、URL
```
GET /v1.0/appkeys/{appkey}/keypairs?name={keypairName}
X-Auth-Token： {tokenId}
```

|  Name | In | Type | Optional | Description |
|--|--|--|--|--|
| tokenId | Header | String | - |トークンID. |
| keypairName | Query | String | O | 照会するキーペアの名前。入力しなければ全てのキーペア情報を照会します。 |

#### Request Body
このAPIはRequest Bodyが必要ありません。

#### Response Body

```json
{
    "header" : {
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
| Keypair Name | Body | String | キーペアの名前。 |
| Public Key Value | Body | String | キーペアの公開鍵の値。 |
| Fingerprint Value | Body | String | フィンガープリント(fingerprint)の値。 |
| Created At | Body | DateTime | キーペア生成時間。 "Keypair Name"を指定した1件の照会の時にのみ表示されます。 |

### キーペア生成とアップロード
キーペアを生成したりユーザーが直接生成したキーペアをアップロードします。

#### Method、URL
```
POST /v1.0/appkeys/{appkey}/keypairs
X-Auth-Token： {tokenId}
Content-Type： application/json;charset=UTF-8
```

|  Name | In | Type | Optional | Description |
|--|--|--|--|--|
| tokenId | Header | String | - | トークンID |

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
| Keypair Name | Body | String | - | キーペアの名前 |
| Public Key Value | Body | String | O | アップロードする公開鍵。省略すると新たなキーペアが作成され、作成されたキーペアの秘密鍵がResponseに一緒に渡されます。 |

#### Response Body

```json
{
    "header" : {
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
| Keypair Name | Body | String | キーペアの名前 |
| Public Key Value | Body | String | キーペアの公開鍵 |
| Private Key Value | Body | String | キーペアの秘密鍵。キーペアアップロード(Requestに"publickey"項目を含む)の場合は省略されます。|
| Fingerprint value | Body | String | フィンガープリント(fingerprint)の値 |

生成されたPrivate Key Valueは全文を.pemファイルで保存した後、該当のキーペアが設定されたインスタンスへのアクセス時に使用できます。 **生成されたPrivate Key Valueは1度しか照会できないため**紛失または削除されないよう、しっかりと保管し、流出防止のためになるべく補助記憶装置(USBフラッシュメモリ)で管理するのが良いでしょう。

### キーペア削除
指定したキーペアを削除します。
#### Method、URL
```
DELETE /v1.0/appkeys/{appkey}/keypairs?name={keypairName}
X-Auth-Token： {tokenId}
```

|  Name | In | Type | Optional | Description |
|--|--|--|--|--|
| tokenId | Header | String | - | トークンID |
| keypairName | Query | String | - | 削除するキーペアの名前 |

#### Request Body
このAPIはRequest Bodyが必要ありません。

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
