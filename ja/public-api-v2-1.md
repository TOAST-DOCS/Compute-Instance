<!-- machine_translated: true -->

<!-- pre-align:aligned sig=92796381842e -->

<a id="compute-instance-api-v2-1-guide"></a>

## Compute > Instance > API v2.1ガイド { #compute-instance-api-v2-1-guide }

InstanceはAPI呼び出し時の認証/認可のためにIaaSトークンを使用します。IaaSトークンは、NHN CloudのOpenStackベースのインフラサービス(IaaS)で使用する認証トークンです。IaaSトークンの発行および使用に関する詳細については、[IaaSトークン](/nhncloud/ja/public-api/iaas-token)を参照してください。

インスタンスAPIは`compute`タイプのエンドポイントを使用します。正確なエンドポイントは、トークン発行レスポンスの`serviceCatalog`を参照してください。

| タイプ | リージョン | エンドポイント |
|---|---|---|
| compute | 韓国(パンギョ)リージョン<br>韓国(ピョンチョン)リージョン<br>韓国(光州)リージョン<br>日本リージョン | https://kr1-api-instance-infrastructure.nhncloudservice.com<br>https://kr2-api-instance-infrastructure.nhncloudservice.com<br>https://kr3-api-instance-infrastructure.nhncloudservice.com<br>https://jp1-api-instance-infrastructure.nhncloudservice.com |

APIレスポンスには、ガイドに記載されていないフィールドが含まれる場合があります。これらのフィールドはNHN Cloud内部での使用を目的としており、事前の告知なしに変更される可能性があるため、使用しないでください。

<a id="instance-flavors"></a>

## インスタンスタイプ { #instance-flavors }

<a id="list-flavors"></a>
### タイプ一覧の表示 { #list-flavors }

```
GET /v2.1/{tenantId}/flavors
X-Auth-Token: {tokenId}
```

<a id="list-flavors-request"></a>
#### リクエスト

このAPIはリクエスト本文を必要としません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tenantId | URL | String | O | テナントID |
| tokenId | Header | String | O | トークンID |
| minDisk | Query | Integer | - | 最小ブロックストレージサイズ(GB)<br>指定したサイズより大きいブロックストレージサイズのタイプのみ返却 |
| minRam | Query | Integer | - | 最小RAMサイズ(MB)<br>指定したサイズより大きいRAMサイズのタイプのみ返却 |

<a id="list-flavors-response"></a>
#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
|---|---|---|---|
| flavors | Body | Object | インスタンスタイプ一覧オブジェクト |
| flavors.id | Body | UUID | インスタンスタイプID |
| flavors.links | Body | Object | インスタンスタイプのパスオブジェクト |
| flavors.name | Body | String | インスタンスタイプ名 |


<details><summary>例</summary>
<p>

```json
{
  "flavors": [
    {
      "id": "013bea75-8541-4c6f-9abe-a03fee3d74fe",
      "links": [
        {
          "href": "https://kr1-api-instance-infrastructure.nhncloudservice.com/v2.1/6cdebe3eb0094910bc41f1d42ebe4cb7/flavors/013bea75-8541-4c6f-9abe-a03fee3d74fe",
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
          "href": "https://kr1-api-instance-infrastructure.nhncloudservice.com/v2.1/6cdebe3eb0094910bc41f1d42ebe4cb7/flavors/0f19a344-bc66-4228-8cb1-fb9ca82c54f5",
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

<a id="list-flavors-with-details"></a>
### タイプ一覧の詳細表示 { #list-flavors-with-details }

```
GET /v2.1/{tenantId}/flavors/detail
X-Auth-Token: {tokenId}
```

<a id="list-flavors-with-details-request"></a>
#### リクエスト

このAPIはリクエスト本文を必要としません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tenantId | URL | String | O | テナントID |
| tokenId | Header | String | O | トークンID |
| minDisk | Query | Integer | - | 最小ブロックストレージサイズ(GB)<br>指定したサイズより大きいブロックストレージサイズのタイプのみ返却 |
| minRam | Query | Integer | - | 最小RAMサイズ(MB)<br>指定したサイズより大きいRAMサイズのタイプのみ返却 |

<a id="list-flavors-with-details-response"></a>
#### レスポンス

| 名前 | 種類 | 形式 | 説明             |
|---|---|---|----------------|
| flavors | Body | Object | インスタンスタイプ一覧オブジェクト  |
| flavors.id | Body | UUID | インスタンスタイプID     |
| flavors.links | Body | Object | インスタンスタイプのパスオブジェクト  |
| flavors.name | Body | String | インスタンスタイプ名     |
| flavors.ram | Body | Integer | メモリサイズ(MB)     |
| flavors.OS-FLV-DISABLED:disabled | Body | Boolean | 有効かどうか         |
| flavors.vcpus | Body | Integer | vCPU数        |
| flavors.extra_specs | Body | Object | 追加仕様オブジェクト       |
| flavors.swap | Body | Integer | スワップ領域サイズ(MB)  |
| flavors.os-flavor-access:is_public | Body | Boolean | 共有かどうか          |
| flavors.rxtx_factor | Body | Float | ネットワーク送受信パケット比率 |
| flavors.OS-FLV-EXT-DATA:ephemeral | Body | Integer | 臨時ブロックストレージサイズ(GB)     |
| flavors.disk | Body | Integer | ルートブロックストレージサイズ(GB) |

<details><summary>例</summary>
<p>

```json
{
  "flavors": [
    {
      "name": "x1.c32m256",
      "links": [
        {
          "href": "https://kr1-api-instance-infrastructure.nhncloudservice.com/v2.1/6cdebe3eb0094910bc41f1d42ebe4cb7/flavors/97604802-a090-43fa-a5ce-c7cfd737fbba",
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
          "href": "https://kr1-api-instance-infrastructure.nhncloudservice.com/v2.1/6cdebe3eb0094910bc41f1d42ebe4cb7/flavors/31fa632d-aeec-4f12-8a57-ce9d146228e5",
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

<a id="availability-zones"></a>

## アベイラビリティゾーン { #availability-zones }

<a id="list-availability-zones"></a>
### アベイラビリティゾーン一覧の表示 { #list-availability-zones }

```
GET /v2.1/{tenantId}/os-availability-zone
X-Auth-Token: {tokenId}
```

<a id="list-availability-zones-request"></a>
#### リクエスト
このAPIはリクエスト本文を必要としません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tenantId | URL | String | O | テナントID |
| tokenId | Header | String | O | トークンID |

<a id="list-availability-zones-response"></a>
#### レスポンス
| 名前 | 種類 | 形式 | 説明 |
|---|---|---|---|
| availabilityZoneInfo | Body | Object | アベイラビリティゾーン情報オブジェクト |
| availabilityZoneInfo.zoneName | Body | String | アベイラビリティゾーン名 |
| availabilityZoneInfo.zoneState | Body | Object | アベイラビリティゾーンの状態情報オブジェクト |
| availabilityZoneInfo.available | Body | Object | アベイラビリティゾーンの状態 |

<details><summary>例</summary>
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

<a id="key-pairs"></a>

## キーペア { #key-pairs }

<a id="list-key-pairs"></a>
### キーペア一覧の表示 { #list-key-pairs }
```
GET /v2.1/{tenantId}/os-keypairs
X-Auth-Token: {tokenId}
```

<a id="list-key-pairs-request"></a>
#### リクエスト
このAPIはリクエスト本文を必要としません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tenantId | URL | String | O | テナントID |
| tokenId | Header | String | O | トークンID |

<a id="list-key-pairs-response"></a>
#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
|---|---|---|---|
| keypairs | Body | Array | キーペアオブジェクト一覧 |
| keypairs.keypair | Body | Object | キーペアオブジェクト |
| keypairs.keypair.name | Body | String | キーペア名 |
| keypairs.keypair.public_key | Body | String | 公開鍵 |
| keypairs.keypair.fingerprint | Body | String | キーペアの指紋 |

<details><summary>例</summary>
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

<a id="show-key-pair"></a>
### キーペアの表示 { #show-key-pair }
```
GET /v2.1/{tenantId}/os-keypairs/{keypairName}
X-Auth-Token: {tokenId}
```

<a id="show-key-pair-request"></a>
#### リクエスト
このAPIはリクエスト本文を必要としません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tenantId | URL | String | O | テナントID |
| keypairName | URL | String | O | キーペア名 |
| tokenId | Header | String | O | トークンID |

<a id="show-key-pair-response"></a>
#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
|---|---|---|---|
| keypair | Body | Object | キーペアオブジェクト一覧 |
| keypair.public_key | Body | String | 公開鍵 |
| keypair.user_id | Body | String | キーペア所有者ID |
| keypair.name | Body | String | キーペア名 |
| keypair.deleted | Body | Boolean | キーペアの削除かどうか |
| keypair.created_at | Body | Datetime | キーペア作成日時<br>`YYYY-MM-DDThh:mm:ss.SSSSSS` |
| keypair.updated_at | Body | Datetime | キーペア更新日時<br>`YYYY-MM-DDThh:mm:ss.SSSSSS` |
| keypair.deleted_at | Body | Datetime | キーペア削除日時<br>`YYYY-MM-DDThh:mm:ss.SSSSSS` |
| keypair.fingerprint | Body | String | キーペアの指紋 |
| keypair.id | Body | Integer | キーペアID |

<details><summary>例</summary>
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

<a id="createregister-key-pair"></a>
### キーペアの作成/登録 { #createregister-key-pair }

```
POST /v2.1/{tenantId}/os-keypairs
X-Auth-Token: {tokenId}
```

<a id="createregister-key-pair-request"></a>
#### リクエスト

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tenantId | URL | String | O | テナントID |
| tokenId | Header | String | O | トークンID |
| keypair | Body | Object | O | キーペアオブジェクト |
| keypair.name | Body | String | O | 作成または登録するキーペア名 |
| keypair.public_key | Body | String | - | 登録する公開鍵。このフィールドを省略すると、新しいキーペアを作成します。 |

<details><summary>例</summary>
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

<a id="createregister-key-pair-response"></a>
#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
|---|---|---|---|
| keypair | Body | Object | キーペアオブジェクト |
| keypair.public_key | Body | String | 公開鍵 |
| keypair.private_key | Body | String | 秘密鍵。新しいキーペアを作成した場合に秘密鍵を返します。 |
| keypair.user_id | Body | String | キーペア所有者ID |
| keypair.name | Body | String | キーペア名 |
| keypair.fingerprint | Body | String | キーペアの指紋 |

<details><summary>例</summary>
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

<a id="delete-key-pair"></a>
### キーペアの削除 { #delete-key-pair }
```
DELETE /v2.1/{tenantId}/os-keypairs/{keypairName}
X-Auth-Token: {tokenId}
```

<a id="delete-key-pair-request"></a>
#### リクエスト
このAPIはリクエスト本文を必要としません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tenantId | URL | String | O | テナントID |
| keypairName | URL | String | O | キーペア名 |
| tokenId | Header | String | O | トークンID |

<a id="delete-key-pair-response"></a>
#### レスポンス
このAPIはレスポンス本文を返しません。


<a id="instance"></a>

## インスタンス { #instance }

<a id="instance-status"></a>

### インスタンスの状態 { #instance-status }

インスタンスはさまざまな状態を持ち、状態に応じて取ることができる操作が定められています。インスタンスの状態一覧は次のとおりです。

| 状態名              | 説明                                                                                                |
|-------------------|---------------------------------------------------------------------------------------------------|
| `ACTIVE` | インスタンスがアクティブな状態の場合 |
| `BUILD` | インスタンスが作成中の場合 |
| `DELETED` | インスタンスが削除された場合 |
| `ERROR` | 直前にインスタンスへ行った操作が失敗した場合 |
| `HARD_REBOOT` | インスタンスを強制再起動した場合<br>物理サーバーの電源をオフにして再度オンにする操作と同じ動作 |
| `MIGRATING` | インスタンスがマイグレーション中の場合<br>これはライブマイグレーション(アクティブインスタンスの移動)作業により発生します |
| `PASSWORD` | インスタンスでパスワードをリセット中の場合 |
| `PAUSED` | インスタンスが一時停止された場合<br>一時停止されたインスタンスはハイパーバイザーのメモリに保存されます |
| `REBOOT` | インスタンスがソフト再起動状態の場合<br>再起動コマンドが仮想マシンのOSに送信されます |
| `REBUILD` | インスタンスを作成時のイメージから新たに作り直す状態 |
| `RESCUE` | インスタンスをレスキューモードで実行中の場合 |
| `RESIZE` | インスタンスタイプを変更するか、インスタンスを別のホストに移動する場合<br>インスタンスが停止してから再起動された状態 |
| `REVERT_RESIZE` | インスタンスタイプの変更またはインスタンスを別のホストへの移動が失敗した際に、元の状態に戻すために復元する場合 |
| `VERIFY_RESIZE` | インスタンスのタイプ変更またはインスタンスを別のホストへの移動が完了し、ユーザーの承認を待つ場合<br>NHN Cloudではこの場合、自動的に`ACTIVE`状態になります |
| `SHELVED_OFFLOADED` | インスタンスが終了した場合 |
| `SHUTOFF` | インスタンスが停止した場合 |
| `SUSPENDED` | インスタンスが管理者によって休止状態に入った場合 |
| `UNKNOWN` | インスタンスの状態が不明な場合<br>`インスタンスがこの状態になった場合は、管理者にお問い合わせください。` |

<a id="list-instances"></a>

### インスタンス一覧の表示 { #list-instances }

```
GET /v2.1/{tenantId}/servers
X-Auth-Token: {tokenId}
```

<a id="list-instances-request"></a>
#### リクエスト

このAPIはリクエスト本文を必要としません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tenantId | URL | String | O | テナントID |
| tokenId | Header | String | O | トークンID |
| reservation_id | Query | String | - | インスタンス作成予約ID。<br>予約IDを指定すると、同時に作成されたインスタンス一覧のみを返します |
| changes-since | Query | Datetime | - | 指定した時刻以降に変更されたインスタンス一覧を返します。`YYYY-MM-DDThh:mm:ss`の形式。 |
| image | Query | UUID | - | イメージID<br>指定されたイメージを使用したインスタンス一覧を返します |
| flavor | Query | UUID | - | インスタンスタイプID<br>指定されたタイプを使用したインスタンス一覧を返します |
| name | Query | String | - | インスタンス名<br>指定された名前を持つインスタンス一覧を返します。正規表現で照会可能 |
| status | Query | Enum | - | インスタンスの状態<br>指定された状態を持つインスタンス一覧を返します |
| limit | Query | Integer | - | インスタンス一覧数<br>指定された件数分のインスタンス一覧を返します |
| marker | Query | UUID | - | 一覧の最初のインスタンスUUID<br>ソート基準に従い、`marker`で指定されたインスタンスから`limit`件数分のインスタンス一覧を返します |

<a id="list-instances-response"></a>
#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
|---|---|---|---|
| servers | Body | Object | インスタンス一覧オブジェクト |
| id | Body | UUID | インスタンスUUID |
| links | body | Object | インスタンスのパスオブジェクト |
| name | body | String | インスタンス名 |

<details><summary>例</summary>
<p>

```json
{
  "servers": [
    {
      "id": "aaf2778b-ea03-4ccc-8b1b-92f4b686c3ec",
      "links": [
        {
          "href": "https://kr1-api-instance-infrastructure.nhncloudservice.com/v2.1/6cdebe3eb0094910bc41f1d42ebe4cb7/servers/aaf2778b-ea03-4ccc-8b1b-92f4b686c3ec",
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

<a id="list-instances-with-details"></a>

### インスタンス一覧の詳細表示 { #list-instances-with-details }

インスタンス一覧の表示と同様に、現在のテナントに作成されたインスタンス一覧を返します。ただし、インスタンスごとの詳細情報も合わせて照会されます。

```
GET /v2.1/{tenantId}/servers/detail
X-Auth-Token: {tokenId}
```

<a id="list-instances-with-details-request"></a>

#### リクエスト

インスタンス一覧の表示と同じリクエスト形式です。

<a id="list-instances-with-details-response"></a>

#### レスポンス

| 名前 | 種類 | 形式 | 説明                                                                                                                                                                                                        |
|---|---|---|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| servers | body | Object | インスタンス一覧オブジェクト                                                                                                                                                                                                |
| status | body | Enum | インスタンスの状態                                                                                                                                                                                                   |
| servers.id | Body | UUID | インスタンスID                                                                                                                                                                                                   |
| servers.name | Body | String | インスタンス名、最大255文字                                                                                                                                                                                          |
| servers.updated | Body | Datetime | インスタンスの最終更新日時、`YYYY-MM-DDThh:mm:ssZ`形式                                                                                                                                                                  |
| servers.hostId | Body | String | インスタンスが稼働しているホストID                                                                                                                                                                                        |
| servers.addresses | Body | Object | インスタンスのIPリストオブジェクト。<br>インスタンスに接続されているポート数分のリストが生成されます。                                                                                                                                                             |
| servers.addresses."Network名" | Body | Object | インスタンスに接続されたNetworkごとのポート情報                                                                                                                                                                                  |
| servers.addresses."Network名".OS-EXT-IPS-MAC:mac_addr | Body | String | インスタンスに接続されたポートのMACアドレス                                                                                                                                                                                      |
| servers.addresses."Network名".version | Body | Integer | インスタンスに接続されたポートのIPバージョン<br>NHN CloudはIPv4のみサポート                                                                                                                                                |
| servers.addresses."Network名".addr | Body | String | インスタンスに接続されたポートのIPアドレス                                                                                                                                                                                       |
| servers.addresses."Network名".OS-EXT-IPS:type | Body | Enum | ポートのIPアドレスタイプ<br>`fixed`または`floating`のいずれか                                                                                                                                                                |
| servers.links | Body | Object | インスタンスのパスオブジェクト                                                                                                                                                                                                |
| servers.key_name | Body | String | インスタンスのキーペア名                                                                                                                                                                                               |
| servers.image | Body | Object | インスタンスのイメージオブジェクト                                                                                                                                                                                               |
| servers.image.id | Body | UUID | インスタンスのイメージID                                                                                                                                                                                               |
| servers.image.links | Body | Object | インスタンスのイメージパスオブジェクト                                                                                                                                                                                            |
| servers.OS-EXT-STS:task_state | Body | String | インスタンスの作業状態<br>インスタンスに操作を加えた際の処理進行状態を示します                                                                                                                                                               |
| servers.OS-EXT-STS:vm_state | Body | String | インスタンスの現在の状態                                                                                                                                                                                                |
| servers.OS-SRV-USG:launched_at | Body | Datetime | インスタンスの最終起動日時<br>`YYYY-MM-DDThh:mm:ss.ssssss`形式                                                                                                                                                         |
| servers.OS-SRV-USG:terminated_at | Body | Datetime | インスタンスの削除日時<br>`YYYY-MM-DDThh:mm:ssZ`形式                                                                                                                                                                   |
| servers.flavor | Body | Object | インスタンスタイプ情報オブジェクト                                                                                                                                                                                             |
| servers.flavor.original_name | Body | String | インスタンスタイプ名                                                                                                                                                                                               |
| servers.flavor.vcpus | Body | Integer | vCPU数                                                                                                                                                                                                 |
| servers.flavor.ram | Body | Integer | メモリサイズ(MB)                                                                                                                                                                                              |
| servers.flavor.disk | Body | Integer | ルートブロックストレージサイズ(GB)                                                                                                                                                                                        |
| servers.flavor.ephemeral | Body | Integer | 臨時ブロックストレージサイズ(GB)                                                                                                                                                                                        |
| servers.flavor.swap | Body | Integer | スワップ領域サイズ(MB)                                                                                                                                                                                            |
| servers.flavor.extra_specs | Body | Object | 追加仕様オブジェクト                                                                                                                                                                                                 |
| servers.security_groups | Body | Object | インスタンスに割り当てられたセキュリティグループリストオブジェクト                                                                                                                                                                                     |
| servers.security_groups.name | Body | String | インスタンスに割り当てられたセキュリティグループ名                                                                                                                                                                                        |
| servers.user_id | Body | String | インスタンスを作成したユーザーID                                                                                                                                                                                          |
| servers.created | Body | Datetime | インスタンス作成日時。`YYYY-MM-DDThh:mm:ssZ`形式                                                                                                                                                                     |
| servers.tenant_id | Body | String | インスタンスが属するテナントID                                                                                                                                                                                           |
| servers.os-extended-volumes:volumes_attached | Body | Object | インスタンスに接続された追加ブロックストレージリストオブジェクト                                                                                                                                                                                |
| servers.os-extended-volumes:volumes_attached.id | Body | UUID | インスタンスに接続された追加ブロックストレージID                                                                                                                                                                                   |
| servers.OS-EXT-STS:power_state | Body | Integer | インスタンスの電源の状態<br>- `1`: On<br>- `4`: Off                                                                                                                                                                    |
| servers.metadata | Body | Object | インスタンスのメタデータオブジェクト<br>インスタンスのメタデータをキーと値のペアで保管します                                                                                                                                                                   |
| server.NHN-EXT-ATTR:ephemeral_disk_size | Body | Integer | インスタンスに接続された追加ローカルブロックストレージサイズ                                                                                                                                                                   |
| server.NHN-EXT-ATTR:protect | Body | Boolean | インスタンスの削除保護かどうか                                                                                                                                                                   |

<details><summary>例</summary>
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
          "href": "https://kr1-api-instance-infrastructure.nhncloudservice.com/v2.1/6cdebe3eb0094910bc41f1d42ebe4cb7/servers/aaf2778b-ea03-4ccc-8b1b-92f4b686c3ec",
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
        "vcpus": 32,
        "ram": 131072,
        "disk": 0,
        "ephemeral": 0,
        "swap": 0,
        "original_name": "x1.c32m128",
        "extra_specs": {
          "flavor_type": "performance"
        }
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
      },
      "NHN-EXT-ATTR:ephemeral_disk_size": 0,
      "NHN-EXT-ATTR:protect": false
    }
  ]
}
```

</p>
</details>

---

<a id="get-instance"></a>

### インスタンスの表示 { #get-instance }

```
GET /v2.1/{tenantId}/servers/{serverId}
X-Auth-Token: {tokenId}
```

<a id="get-instance-request"></a>

#### リクエスト

このAPIはリクエスト本文を必要としません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tenantId | URL | String | O | テナントID |
| serverId | URL | UUID | O | インスタンスID |
| tokenId | Header | String | O | トークンID |

<a id="get-instance-response"></a>

#### レスポンス

| 名前 | 種類 | 形式 | 説明                                                                                                                                                                                                       |
|---|---|---|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| server | body | Object | インスタンスオブジェクト                                                                                                                                                                                                  |
| status | body | Enum | インスタンスの状態                                                                                                                                                                                                  |
| server.id | Body | UUID | インスタンスID                                                                                                                                                                                                  |
| server.name | Body | String | インスタンス名、最大255文字                                                                                                                                                                                         |
| server.updated | Body | Datetime | インスタンスの最終更新日時、`YYYY-MM-DDThh:mm:ssZ`形式                                                                                                                                                                 |
| server.hostId | Body | String | インスタンスが稼働中のホストID                                                                                                                                                                                       |
| server.addresses | Body | Object | インスタンスIPリストオブジェクト<br>インスタンスに接続されたポート数だけリストが生成されます                                                                                                                                                              |
| server.addresses."Network 이름" | Body | Object | インスタンスに接続されたネットワーク別ポート情報                                                                                                                                                                                 |
| server.addresses."Network 이름".OS-EXT-IPS-MAC:mac_addr | Body | String | インスタンスに接続されたポートのMACアドレス                                                                                                                                                                                     |
| server.addresses."Network 이름".version | Body | Integer | インスタンスに接続されたポートのIPバージョン<br>NHN CloudはIPv4のみ対応                                                                                                                                               |
| server.addresses."Network 이름".addr | Body | String | インスタンスに接続されたポートのIPアドレス                                                                                                                                                                                      |
| server.addresses."Network 이름".OS-EXT-IPS:type | Body | Enum | ポートのIPアドレスタイプ<br>`fixed`または`floating`のいずれか                                                                                                                                               |
| server.links | Body | Object | インスタンスパスオブジェクト                                                                                                                                                                                               |
| server.key_name | Body | String | インスタンスキーペア名                                                                                                                                                                                              |
| server.image | Body | Object | インスタンスイメージオブジェクト                                                                                                                                                                                              |
| server.image.id | Body | UUID | インスタンスイメージID                                                                                                                                                                                              |
| server.image.links | Body | Object | インスタンスイメージパスオブジェクト                                                                                                                                                                                           |
| server.OS-EXT-STS:task_state | Body | String | インスタンスの作業状態<br>インスタンスに操作を加えたときの動作進行状態を通知します                                                                                                                                                               |
| server.OS-EXT-STS:vm_state | Body | String | インスタンスの現在の状態                                                                                                                                                                                               |
| server.OS-SRV-USG:launched_at | Body | Datetime | インスタンスの最終起動日時<br>`YYYY-MM-DDThh:mm:ss.ssssss`形式                                                                                                                                                        |
| server.OS-SRV-USG:terminated_at | Body | Datetime | インスタンスの削除日時<br>`YYYY-MM-DDThh:mm:ssZ`形式                                                                                                                                                                  |
| server.flavor | Body | Object | インスタンスタイプ情報オブジェクト                                                                                                                                                                                            |
| server.flavor.original_name | Body | String | インスタンスタイプ名                                                                                                                                                                                              |
| server.flavor.vcpus | Body | Integer | vCPU数                                                                                                                                                                                                |
| server.flavor.ram | Body | Integer | メモリサイズ(MB)                                                                                                                                                                             |
| server.flavor.disk | Body | Integer | ルートブロックストレージサイズ(GB)                                                                                                                                                                                       |
| server.flavor.ephemeral | Body | Integer | 臨時ブロックストレージサイズ(GB)                                                                                                                                                                                       |
| server.flavor.swap | Body | Integer | スワップ領域サイズ(MB)                                                                                                                                                                           |
| server.flavor.extra_specs | Body | Object | 追加仕様オブジェクト                                                                                                                                                                                                |
| server.security_groups | Body | Object | インスタンスに割り当てられたセキュリティグループリストオブジェクト                                                                                                                                                                                    |
| server.security_groups.name | Body | String | インスタンスに割り当てられたセキュリティグループ名                                                                                                                                                                                       |
| server.user_id | Body | String | インスタンスを作成したユーザーID                                                                                                                                                                                         |
| server.created | Body | Datetime | インスタンスの作成日時、`YYYY-MM-DDThh:mm:ssZ`形式                                                                                                                                                                    |
| server.tenant_id | Body | String | インスタンスが属するテナントID                                                                                                                                                                                          |
| server.os-extended-volumes:volumes_attached | Body | Object | インスタンスに接続された追加ブロックストレージリストオブジェクト                                                                                                                                                                               |
| server.os-extended-volumes:volumes_attached.id | Body | UUID | インスタンスに接続された追加ブロックストレージID                                                                                                                                                                                  |
| server.OS-EXT-STS:power_state | Body | Integer | インスタンスの電源の状態<br>- `1`: On<br>- `4`: Off                                                                                                                                                                   |
| server.metadata | Body | Object | インスタンスメタデータオブジェクト<br>インスタンスメタデータをキーと値のペアで保管します                                                                                                                                                                  |
| server.NHN-EXT-ATTR:ephemeral_disk_size | Body | Integer | インスタンスに接続された追加ローカルブロックストレージのサイズ                                                                                                                                                                  |
| server.NHN-EXT-ATTR:protect | Body | Boolean | インスタンスの削除保護かどうか                                                                                                                                                                  |

<details><summary>例</summary>
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
        "href": "https://kr1-api-instance-infrastructure.nhncloudservice.com/v2.1/6cdebe3eb0094910bc41f1d42ebe4cb7/servers/aaf2778b-ea03-4ccc-8b1b-92f4b686c3ec",
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
      "vcpus": 32,
      "ram": 131072,
      "disk": 0,
      "ephemeral": 0,
      "swap": 0,
      "original_name": "x1.c32m128",
      "extra_specs": {
        "flavor_type": "performance"
      }
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
    },
    "NHN-EXT-ATTR:ephemeral_disk_size": 0,
    "NHN-EXT-ATTR:protect": false
  }
}
```

</p>
</details>

---

<a id="create-instance"></a>

### インスタンスを作成する { #create-instance }

インスタンスを作成します。

インスタンス作成APIを呼び出した後、インスタンスの照会でインスタンスの状態を確認します。

* インスタンスの状態が**ACTIVE**になると、インスタンスが正常に作成完了します。
* インスタンスの状態が**BUILDING**のまま長時間続く場合、または**ERROR**の場合は、インスタンス作成のパラメータを確認して再度作成を試みます。

Windowsインスタンスは、安定した動作のために次の作成制約条件があります。

* RAMが2GB以上のインスタンスタイプを使用します。
* 50GB以上のルートブロックストレージが必要です。
* U2タイプはWindowsイメージを使用することはできません。

ルートブロックストレージのサイズは、Linuxは10GB、Windowsは50GBから指定できます。

インスタンス作成リクエスト時に、スケジューラヒントを通じて配置ポリシーを割り当てることができます。



```
POST /v2.1/{tenantId}/servers
X-Auth-Token: {tokenId}
```

<a id="create-instance-request"></a>
#### リクエスト

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tenantId | URL | String | O | テナントID |
| tokenId | Header | String | O | トークンID |
| server | body | Object | O | サーバーオブジェクト |
| server.security_groups | body | Object | - | セキュリティグループリストオブジェクト<br>省略した場合、`default`グループが追加されます |
| server.security_groups.name | body | String | - | **(条件付き必須)** インスタンスに追加するセキュリティグループ名 |
| server.user_data | body | String | - | インスタンス起動後に実行するスクリプトおよび設定<br>base64エンコードされた文字列で65535バイトまで許可 |
| server.availability_zone | body | String | - | インスタンスを作成するアベイラビリティゾーン<br>指定しない場合はランダムに選択されます<br>ルートブロックストレージのソースタイプが`volume`、`snapshot`の場合は、元のブロックストレージのアベイラビリティゾーンと同じ設定が必要 |
| server.imageRef | Body | String | - | インスタンスの作成時に使用するイメージID<br>ルートブロックストレージのソースタイプが`volume`、`snapshot`の場合は設定不要 |
| server.flavorRef | Body | String | O | インスタンスの作成時に使用するインスタンスタイプID |
| server.networks | Body | Object | O | インスタンスの作成時に使用するネットワーク情報オブジェクト<br>指定した数だけNICが追加され、ネットワークID、サブネットID、ポートID、固定IPのいずれかで指定 |
| server.networks.uuid | Body | UUID | - | **(条件付き必須)** インスタンスの作成時に使用するネットワークID |
| server.networks.subnet | Body | UUID | - | **(条件付き必須)** インスタンスの作成時に使用するネットワークのサブネットID |
| server.networks.port | Body | UUID | - | **(条件付き必須)** インスタンスの作成時に使用するポートID<br>ポートIDを指定した場合、リクエストしたセキュリティグループは指定した既存のポートに適用されません |
| server.networks.fixed_ip | Body | String | - | **(条件付き必須)** インスタンスの作成時に使用する固定IP |
| server.name | Body | String | O | インスタンスの名前<br>英字基準で255文字まで許可されますが、Windowsイメージの場合は15文字以下である必要があります |
| server.metadata | Body | Object | - | インスタンスに追加するメタデータオブジェクト<br>最大長255文字以下のキーと値のペア |
| server.block_device_mapping_v2 | Body | Object | O | インスタンスのブロックストレージ情報オブジェクト |
| server.block_device_mapping_v2.source_type | Body | Enum | O | 作成するブロックストレージ元のタイプ<br>- `image`: イメージを使用してブロックストレージを作成<br>- `blank`: 空のブロックストレージを作成(ルートブロックストレージとして使用不可)<br>- `volume`: 既存のブロックストレージを使用<br>- `snapshot`: スナップショットを使用してブロックストレージを作成 |
| server.block_device_mapping_v2.uuid | Body | String | - | **(条件付き必須)** ブロックストレージのソースタイプに応じて異なる設定が必要<br>- ソースタイプが`image`の場合はイメージIDを設定<br>- ソースタイプが`volume`の場合は既存のブロックストレージIDを設定<br>- ソースタイプが`snapshot`の場合はスナップショットIDを設定<br>- ソースタイプが`blank`の場合は設定不要<br>ルートブロックストレージの場合は必ず起動可能な元である必要があります |
| server.block_device_mapping_v2.boot_index | Body | Integer | O | 指定したブロックストレージの起動順序<br>-`0`の場合はルートブロックストレージ<br>- それ以外は追加ブロックストレージ<br>値が大きいほど起動順序は低くなります |
| server.block_device_mapping_v2.destination_type | Body | Enum | O | インスタンスブロックストレージの配置場所。インスタンスタイプに応じて異なる設定が必要です。<br>- `local`: GPUインスタンス、U2インスタンスタイプを使用する場合<br>- `volume`: その他のインスタンスタイプを使用する場合 |
| server.block_device_mapping_v2.volume_type | Body | Enum    | - | **(条件付き必須)** 作成するブロックストレージのタイプ<br>ブロックストレージのソースタイプが`volume`、`snapshot`の場合は設定不要<br>`ユーザーガイド > Storage > Block Storage > API v2ガイド`の**ブロックストレージタイプ一覧表示**レスポンスの`name`を参照してください |
| server.block_device_mapping_v2.delete_on_termination | Body | Boolean | - | インスタンス削除時のブロックストレージ処理可否。デフォルト値は`false`です。<br>`true`の場合は削除、`false`の場合は維持 |
| server.block_device_mapping_v2.volume_size | Body | Integer | - | **(条件付き必須)** 作成するブロックストレージのサイズ<br>ブロックストレージのソースタイプに応じて異なる設定が必要<br>- ソースタイプが`volume`の場合は設定不要<br>- ソースタイプが`snapshot`の場合は元のブロックストレージサイズ以上に設定<br>`GB`単位<br>U2インスタンスタイプを使用してルートブロックストレージを作成する場合は、U2インスタンスタイプに指定されたサイズで作成され、この値は無視されます<br>インスタンスタイプに応じて作成できるルートブロックストレージのサイズが異なるため、詳細については`ユーザーガイド > Compute > Instance > コンソール使用ガイド > インスタンスの作成 > ブロックストレージサイズ`を参照してください |
| server.block_device_mapping_v2.nhn_encryption                   | Body | Object | - | **(条件付き必須)** ブロックストレージの暗号化情報                                                                                                                                                                                        |
| server.block_device_mapping_v2.nhn_encryption.skm_appkey        | Body | String | - | **(条件付き必須)** Secure Key Managerサービスのアプリキー                                                                                                                                                                              |
| server.block_device_mapping_v2.nhn_encryption.skm_key_id        | Body | String | - | **(条件付き必須)** 暗号化ブロックストレージの作成に使用するSecure Key Managerの対称キーID                                                                                                                                  |
| server.key_name | Body | String | O | インスタンスへの接続に使用するキーペア |
| server.min_count | Body | Integer | - | 現在のリクエストで作成するインスタンス数の最小値。<br>デフォルト値は1。<br>ブロックストレージのソースタイプが`volume`の場合は`1`のみ設定可能 |
| server.max_count | Body | Integer | - | 現在のリクエストで作成するインスタンス数の最大値。<br>デフォルト値はmin_count、最大値は10。<br>ブロックストレージのソースタイプが`volume`の場合は`1`のみ設定可能 |
| server.return_reservation_id | Body | Boolean | - | インスタンス作成リクエストの予約ID。<br>Trueを指定するとインスタンス作成情報の代わりに予約IDを返します。<br>デフォルト値はFalse |
| os:scheduler_hints | Body | Object | - | スケジューラヒントオブジェクト |
| os:scheduler_hints.group | Body | String | - | 配置ポリシーID |

<details><summary>例</summary>
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
      "source_type": "image",
      "uuid": "9956f822-29c9-4f81-9410-0c392d9c8c24",
      "boot_index": 0,
      "volume_size": 1000,
      "destination_type": "volume",
      "delete_on_termination": 1
    }],
    "security_groups": [{
      "name": "default"
    }]
  },
  "os:scheduler_hints": {
    "group": "f878bd5b-49a7-499f-966e-1eceb21cb06b"
  }
}
```

</p>
</details>

<a id="create-instance-response"></a>
#### レスポンス

| 名前 | 種類 | 形式 | 説明                                                                                                                                                                                                           |
|---|---|---|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| server.security_groups.name | Body | String | 作成したインスタンスのセキュリティグループ名                                                                                                                                                                                           |
| server.id | Body | UUID | 作成したインスタンスのID                                                                                                                                                                                                 |

<details><summary>例</summary>
<p>

```json
{
  "server": {
    "security_groups": [
      {
        "name": "default"
      }
    ],
    "id": "3a005d5b-63cf-4493-bfc6-49db990b5b50",
    "links": [
      {
        "href": "https://kr1-api-instance-infrastructure.nhncloudservice.com/v2.1/6cdebe3eb0094910bc41f1d42ebe4cb7/servers/3a005d5b-63cf-4493-bfc6-49db990b5b50",
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

<a id="modify-instance"></a>

### インスタンスを修正する { #modify-instance }
作成されたインスタンスを修正します。変更できる属性は一部の項目に制限されます。

```
PUT /v2.1/{tenantId}/servers/{serverId}
X-Auth-Token: {tokenId}
```

<a id="modify-instance-request"></a>
#### リクエスト

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tenantId | URL | String | O | テナントID |
| serverId | URL | UUID | O | 変更するインスタンスID |
| tokenId | Header | String | O | トークンID |
| server | Body | Object | O | インスタンス変更リクエストオブジェクト |
| server.name | Body | String | - | インスタンスの新しい名前 |

<details><summary>例</summary>
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

<a id="modify-instance-response"></a>
#### レスポンス
インスタンスの表示と同じです。

---

<a id="delete-instance"></a>

### インスタンスを削除する { #delete-instance }
作成されたインスタンスを削除します。

```
DELETE /v2.1/{tenantId}/servers/{serverId}
X-Auth-Token: {tokenId}
```

<a id="delete-instance-request"></a>
#### リクエスト
このAPIはリクエスト本文を要求しません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|--|
| tenantId | URL | String | O | テナントID |
| serverId | URL | UUID | O | 削除するインスタンスID |
| tokenId | Header | String | O | トークンID |

<a id="delete-instance-response"></a>
#### レスポンス
このAPIはレスポンス本文を返しません。

---

<a id="manage-block-storage-attachment"></a>

## ブロックストレージ接続管理 { #manage-block-storage-attachment }

<a id="list-additional-block-storage-attached-to-the-instance"></a>
### インスタンスに接続されたブロックストレージ一覧の表示 { #list-additional-block-storage-attached-to-the-instance }
```
GET /v2.1/{tenantId}/servers/{serverId}/os-volume_attachments
X-Auth-Token: {tokenId}
```

<a id="list-additional-block-storage-attached-to-the-instance-request"></a>
#### リクエスト
このAPIはリクエスト本文を必要としません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|--|
| tenantId | URL | String | O | テナントID |
| serverId | URL | UUID | O | 変更するインスタンスID |
| tokenId | Header | String | O | トークンID |
| limit | Query | Integer | - | 照会するリストの件数 |
| offset | Query | Integer | - | 返却するリストの開始点<br>全リストのうち、offset番目のブロックストレージから返却 |

<a id="list-additional-block-storage-attached-to-the-instance-response"></a>
#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
|---|---|---|---|
| volumeAttachments | Body | Array | 接続情報オブジェクトの一覧 |
| volumeAttachments.device | Body | String | インスタンスのブロックストレージ名<br>例) `/dev/vdb` |
| volumeAttachments.id | Body | UUID | 接続情報ID |
| volumeAttachments.serverId | Body | UUID | インスタンスID |
| volumeAttachments.volumeId | Body | UUID | ブロックストレージID |

<details><summary>例</summary>
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

<a id="show-additional-block-storage-attached-to-the-instance"></a>
### インスタンスに接続されたブロックストレージの表示 { #show-additional-block-storage-attached-to-the-instance }
```
GET /v2.1/{tenantId}/servers/{serverId}/os-volume_attachments/{volumeId}
X-Auth-Token: {tokenId}
```

<a id="show-additional-block-storage-attached-to-the-instance-request"></a>
#### リクエスト
このAPIはリクエスト本文を必要としません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|--|
| tenantId | URL | String | O | テナントID |
| serverId | URL | UUID | O | インスタンスID |
| volumeId | URL | UUID | O | 照会するブロックストレージID |
| tokenId | Header | String | O | トークンID |

<a id="show-additional-block-storage-attached-to-the-instance-response"></a>
#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
|---|---|---|---|
| volumeAttachment | Body | Object | 接続情報オブジェクト |
| volumeAttachment.device | Body | String | インスタンスのブロックストレージ名<br>例) `/dev/vdb` |
| volumeAttachment.id | Body | UUID | 接続情報ID |
| volumeAttachment.serverId | Body | UUID | インスタンスID |
| volumeAttachment.volumeId | Body | UUID | ブロックストレージID |

<details><summary>例</summary>
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

<a id="attach-additional-block-storage-to-the-instance"></a>
### インスタンスへの追加ブロックストレージの接続 { #attach-additional-block-storage-to-the-instance }
```
POST /v2.1/{tenantId}/servers/{serverId}/os-volume_attachments
X-Auth-Token: {tokenId}
```

<a id="attach-additional-block-storage-to-the-instance-request"></a>
#### リクエスト

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|--|
| tenantId | URL | String | O | テナントID |
| serverId | URL | UUID | O | 変更するインスタンスID |
| tokenId | Header | String | O | トークンID |
| volumeAttachment | Body | Object | O | ブロックストレージ接続リクエストオブジェクト |
| volumeAttachment.volumeId | Body | UUID | O | 接続するブロックストレージID |

<details><summary>例</summary>
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

<a id="attach-additional-block-storage-to-the-instance-response"></a>
#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
|---|---|---|---|
| volumeAttachment | Body | Object | 接続情報オブジェクト |
| volumeAttachment.device | Body | String | インスタンスのブロックストレージ名<br>例) `/dev/vdb` |
| volumeAttachment.id | Body | UUID | 接続情報ID |
| volumeAttachment.serverId | Body | UUID | インスタンスID |
| volumeAttachment.volumeId | Body | UUID | ブロックストレージID |

<details><summary>例</summary>
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

<a id="detach-block-storage-from-the-instance"></a>
### インスタンスのブロックストレージの接続解除 { #detach-block-storage-from-the-instance }
```
DELETE /v2.1/{tenantId}/servers/{serverId}/os-volume_attachments/{volumeId}
X-Auth-Token: {tokenId}
```

<a id="detach-block-storage-from-the-instance-request"></a>
#### リクエスト
このAPIはリクエスト本文を必要としません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|--|
| tenantId | URL | String | O | テナントID |
| serverId | URL | UUID | O | インスタンスID |
| volumeId | URL | UUID | O | 接続を解除するブロックストレージID |
| tokenId | Header | String | O | トークンID |

<a id="detach-block-storage-from-the-instance-response"></a>
#### レスポンス
このAPIはレスポンス本文を返しません。

---

<a id="additional-instance-features"></a>

## インスタンスの追加機能 { #additional-instance-features }
NHN Cloudは次のインスタンス制御および追加機能を提供します。

* インスタンスの起動、停止、終了、再起動
* インスタンスタイプの変更
* インスタンスイメージの作成
* セキュリティグループの追加および削除

<a id="start-stopped-instance"></a>
### 停止したインスタンスの起動 { #start-stopped-instance }

停止したインスタンスを再起動し、状態を**ACTIVE**に変更します。このAPIを呼び出すには、インスタンスの状態が**SHUTOFF**である必要があります。

```
POST /v2.1/{tenantId}/servers/{serverId}/action
X-Auth-Token: {tokenId}
```

<a id="start-stopped-instance-request"></a>
#### リクエスト
| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|--|
| tenantId | URL | String | O | テナントID |
| serverId | URL | UUID | O | 変更するインスタンスID |
| tokenId | Header | String | O | トークンID |
| os-start | Body | none | O | インスタンスの起動リクエスト |

<details><summary>例</summary>
<p>

```json
{
  "os-start" : null
}
```

</p>
</details>

<a id="start-stopped-instance-response"></a>
#### レスポンス
このAPIはレスポンス本文を返しません。

---

<a id="start-terminated-instance"></a>
### 終了したインスタンスの起動 { #start-terminated-instance }

終了したインスタンスを再起動し、状態を**ACTIVE**に変更します。このAPIを呼び出すには、インスタンスの状態が**SHELVED_OFFLOADED**である必要があります。

```
POST /v2.1/{tenantId}/servers/{serverId}/action
X-Auth-Token: {tokenId}
```

<a id="start-terminated-instance-request"></a>
#### リクエスト
| 名前 | 種類 | 形式 | 必須 | 説明 |
|--|---|---|---|--|
| tenantId | URL | String | O | テナントID |
| serverId | URL | UUID | O | 変更するインスタンスID |
| tokenId | Header | String | O | トークンID |
| unshelve | Body | none | O | インスタンスの起動リクエスト |

<details><summary>例</summary>
<p>

```json
{
  "unshelve" : null
}
```

</p>
</details>

<a id="start-terminated-instance-response"></a>
#### レスポンス
このAPIはレスポンス本文を返しません。

---

<a id="stop-instance"></a>
### インスタンスの停止 { #stop-instance }

インスタンスを停止し、状態を**SHUTOFF**に変更します。このAPIを呼び出すには、インスタンスの状態が**ACTIVE**または**ERROR**である必要があります。

```
POST /v2.1/{tenantId}/servers/{serverId}/action
X-Auth-Token: {tokenId}
```

<a id="stop-instance-request"></a>
#### リクエスト
| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|--|
| tenantId | URL | String | O | テナントID |
| serverId | URL | UUID | O | 変更するインスタンスID |
| tokenId | Header | String | O | トークンID |
| os-stop | Body | none | O | インスタンスの停止リクエスト |

<details><summary>例</summary>
<p>

```json
{
  "os-stop" : null
}
```

</p>
</details>

<a id="stop-instance-response"></a>
#### レスポンス
このAPIはレスポンス本文を返しません。

---

<a id="terminate-instance"></a>
### インスタンスの終了 { #terminate-instance }

インスタンスを終了し、状態を**SHELVED_OFFLOADED**に変更します。このAPIを呼び出すには、インスタンスの状態が**ACTIVE**である必要があります。

```
POST /v2.1/{tenantId}/servers/{serverId}/action
X-Auth-Token: {tokenId}
```

<a id="terminate-instance-request"></a>
#### リクエスト
| 名前 | 種類 | 形式 | 必須 | 説明          |
|---|---|---|---|-------------|
| tenantId | URL | String | O | テナントID      |
| serverId | URL | UUID | O | 変更するインスタンスID |
| tokenId | Header | String | O | トークンID       |
| shelve | Body | none | O | インスタンスの終了リクエスト  |

<details><summary>例</summary>
<p>

```json
{
  "shelve" : null
}
```

</p>
</details>

<a id="terminate-instance-response"></a>
#### レスポンス
このAPIはレスポンス本文を返しません。

---

<a id="restart-instance"></a>
### インスタンスの再起動 { #restart-instance }

インスタンスを再起動します。再起動方式は**SOFT**と**HARD**に分けられます。

* **SOFT**方式: **「グレースフルシャットダウン(Graceful shutdown)」**を通じてインスタンスを停止し、再起動します。インスタンスが**ACTIVE**状態である必要があります。
* **HARD**方式: 強制停止後にインスタンスを再起動します。物理サーバーの電源をオフにして再びオンにするのと同じ動作です。インスタンスが次の状態のときのみ強制停止できます。
    * **ACTIVE**
    * **ERROR**
    * **HARD_REBOOT**
    * **PAUSED**
    * **REBOOT**
    * **SHUTOFF**
    * **SUSPENDED**

```
POST /v2.1/{tenantId}/servers/{serverId}/action
X-Auth-Token: {tokenId}
```

<a id="restart-instance-request"></a>
#### リクエスト
| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|--|
| tenantId | URL | String | O | テナントID |
| serverId | URL | UUID | O | 変更するインスタンスID |
| tokenId | Header | String | O | トークンID |
| reboot | Body | Object | O | インスタンス再起動リクエストオブジェクト |
| reboot.type | Body | Enum | O | 再起動方式、**SOFT**または**HARD** |

<details><summary>例</summary>
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

<a id="restart-instance-response"></a>
#### レスポンス
このAPIはレスポンス本文を返しません。

---

<a id="change-instance-flavor"></a>
### インスタンスタイプの変更 { #change-instance-flavor }

インスタンスタイプを変更します。インスタンスが**ACTIVE**または**SHUTOFF**状態のときのみインスタンスタイプを変更できます。インスタンスの状態が**ACTIVE**の場合、インスタンスタイプの変更過程でインスタンスは停止され、再起動されます。

使用するイメージやインスタンスタイプによって、変更できるタイプが制限される場合があります。詳細な変更制約事項については、コンソールユーザーガイドを参照してください。


```
POST /v2.1/{tenantId}/servers/{serverId}/action
X-Auth-Token: {tokenId}
```

<a id="change-instance-flavor-request"></a>
#### リクエスト
| 名前 | 種類 | 形式 | 必須 | 説明                                                                                                                                                                                                                 |
|---|---|---|---|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| tenantId | URL | String | O | テナントID                                                                                                                                                                                                             |
| serverId | URL | UUID | O | 変更するインスタンスID                                                                                                                                                                                                        |
| tokenId | Header | String | O | トークンID                                                                                                                                                                                                              |
| resize | Body | Object | O | インスタンスタイプ変更リクエスト                                                                                                                                                                                                      |
| resize.flavorRef | Body | UUID | O | 変更するインスタンスタイプID                                                                                                                                                                                                     |

<details><summary>例</summary>
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

<a id="change-instance-flavor-response"></a>
#### レスポンス
このAPIはレスポンス本文を返しません。

---

<a id="create-instance-image"></a>
### インスタンスイメージの作成 { #create-instance-image }

インスタンスからイメージを作成します。`U2`タイプのインスタンスのみ、このAPIを通じてイメージを作成できます。`U2`タイプ以外のインスタンスイメージの作成は[ブロックストレージAPI](/Storage/Block Storage/ja/public-api/#create-image-with-block-storage)を参照してください。

インスタンスの状態が**ACTIVE**、**SHUTOFF**、**SUSPENDED**、**PAUSED**のときのみイメージを作成できます。データの整合性を保証するために、インスタンスを停止した状態でイメージを作成することを推奨します。

イメージの作成が成功すると、イメージの状態が`active`に変わります。イメージの作成が完了したことを確認するには、イメージ照会APIを通じて継続的に状態を確認します。

> [注意]
> 作成されたイメージのサイズは、ルートブロックストレージの実際の使用量より大きくなる場合があります。

```
POST /v2.1/{tenantId}/servers/{serverId}/action
X-Auth-Token: {tokenId}
```

<a id="create-instance-image-request"></a>
#### リクエスト
| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|--|
| tenantId | URL | String | O | テナントID |
| serverId | URL | UUID | O | 変更するインスタンスID |
| tokenId | Header | String | O | トークンID |
| createImage | Body | Object | O | イメージ作成リクエスト |
| createImage.name | Body | String | O | 作成するイメージ名 |
| createImage.metadata | Body | Object | - | 作成するイメージのメタデータ<br>Key-Value形式で記述 |

<details><summary>例</summary>
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


<a id="create-instance-image-response"></a>
#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
|--|--|--|--|
| image_id | Body | UUID | 作成したイメージID |

<details><summary>例</summary>
<p>

```json
{
  "image_id": "7a0e6e4f-9d4a-4f2c-9b6f-3f08b9d2a5c1"
}
```

</p>
</details>

---

<a id="add-security-group"></a>
### セキュリティグループの追加 { #add-security-group }

インスタンスにセキュリティグループを追加します。追加したセキュリティグループは、インスタンスのすべてのポートに適用されます。

```
POST /v2.1/{tenantId}/servers/{serverId}/action
X-Auth-Token: {tokenId}
```

<a id="add-security-group-request"></a>
#### リクエスト
| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|--|
| tenantId | URL | String | O | テナントID |
| serverId | URL | UUID | O | 変更するインスタンスID |
| tokenId | Header | String | O | トークンID |
| addSecurityGroup | Body | Object | O | セキュリティグループ追加リクエストオブジェクト |
| addSecurityGroup.name | Body | String | O | 追加するセキュリティグループ名 |

<details><summary>例</summary>
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


<a id="add-security-group-response"></a>
#### レスポンス
このAPIはレスポンス本文を返しません。

---

<a id="delete-security-group"></a>
### セキュリティグループの削除 { #delete-security-group }

インスタンスからセキュリティグループを削除します。インスタンスのすべてのポートから指定したセキュリティグループが削除されます。

```
POST /v2.1/{tenantId}/servers/{serverId}/action
X-Auth-Token: {tokenId}
```

<a id="delete-security-group-request"></a>
#### リクエスト
| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|--|
| tenantId | URL | String | O | テナントID |
| serverId | URL | UUID | O | 変更するインスタンスID |
| tokenId | Header | String | O | トークンID |
| removeSecurityGroup | Body | Object | O | セキュリティグループ削除リクエストオブジェクト |
| removeSecurityGroup.name | Body | String | O | 削除するセキュリティグループ名 |

<details><summary>例</summary>
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


<a id="delete-security-group-response"></a>
#### レスポンス
このAPIはレスポンス本文を返しません。


<a id="instance-metadata"></a>

## インスタンスメタデータ { #instance-metadata }

インスタンスメタデータの値に応じて、コンソールの**Compute > Instance**サービスページのインスタンス詳細情報画面の内容が決まります。インスタンスメタデータごとの内容は次のとおりです。

| インスタンスメタデータ | 内容                                           |
|----------------|----------------------------------------------|
| os_distro      | **基本情報**の**OS**の名前<br>os_versionと組み合わせて使用 |
| os_version     | **基本情報**の**OS**のバージョン<br>os_distroと組み合わせて使用  |
| image_name     | **基本情報**の**イメージ名**                        |
| os_type      | **接続情報**の形式                                 |
| login_username | **接続情報**のユーザー名                            |

> [注意] インスタンスメタデータを変更または削除すると、関連するサービスおよび機能に影響が生じる可能性があります。その結果に対する責任はユーザーにあります。

<a id="view-a-list-of-instance-metadata"></a>
### インスタンスメタデータ一覧の表示 { #view-a-list-of-instance-metadata }

```
GET /v2.1/{tenantId}/servers/{serverId}/metadata
X-Auth-Token: {tokenId}
```

<a id="view-a-list-of-instance-metadata-request"></a>
#### リクエスト
このAPIはリクエスト本文を必要としません。

| 名前       | 種類 | 形式 | 必須 | 説明                                               |
|----------|---|---|---|--------------------------------------------------|
| tenantId | URL | String | O | テナントID                                           |
| serverId | URL | UUID | O | インスタンスID                                          |
| tokenId  | Header | String | O | トークンID                                            |

<a id="view-a-list-of-instance-metadata-response"></a>
#### レスポンス

| 名前       | 種類 | 形式 | 説明                                               |
|----------|---|---|--------------------------------------------------|
| metadata | Body | Object | インスタンスに作成または更新するメタデータオブジェクト<br>最大255文字以下のキーと値のペア |

<details><summary>例</summary>
<p>

```json
{
    "metadata": {
        "os_distro": "ubuntu",
        "description": "Ubuntu Server 20.04.6 LTS (2023.11.21)",
        "volume_size": "20",
        "project_domain": "NORMAL",
        "monitoring_agent": "sysmon",
        "image_name": "Ubuntu Server 20.04.6 LTS (2023.11.21)",
        "os_version": "Server 20.04 LTS",
        "os_architecture": "amd64",
        "login_username": "ubuntu",
        "os_type": "linux",
        "tc_env": "sysmon,dfeac7db42a192a73959d5646117af58"
    }
}
```

</p>
</details>


<a id="view-instance-metadata"></a>
### インスタンスメタデータの表示 { #view-instance-metadata }

```
GET /v2.1/{tenantId}/servers/{serverId}/metadata/{key}
X-Auth-Token: {tokenId}
```

<a id="view-instance-metadata-request"></a>
#### リクエスト
このAPIはリクエスト本文を必要としません。

| 名前       | 種類 | 形式 | 必須 | 説明                       |
|----------|---|---|---|--------------------------|
| tenantId | URL | String | O | テナントID                   |
| serverId | URL | UUID | O | インスタンスID                  |
| key      | URL | String | O | インスタンスに作成または更新するメタデータのキー |
| tokenId  | Header | String | O | トークンID                    |

<a id="view-instance-metadata-response"></a>
#### レスポンス

| 名前   | 種類 | 形式 | 説明                                               |
|------|---|---|--------------------------------------------------|
| meta | Body | Object | インスタンスに作成または更新するメタデータオブジェクト<br>最大255文字以下のキーと値のペア |

<details><summary>例</summary>
<p>

```json
{
    "meta": {
        "os_version": "Server 20.04 LTS"
    }
}
```

</p>
</details>

<a id="createmodify-instance-metadata"></a>
### インスタンスメタデータの作成/更新 { #createmodify-instance-metadata }

インスタンスのメタデータを作成または更新します。
リクエストするキーが既存のキーと一致する場合は、キーと値をリクエストの値に変更します。

```
PUT /v2.1/{tenantId}/servers/{serverId}/metadata/{key}
X-Auth-Token: {tokenId}
```

<a id="createmodify-instance-metadata-request"></a>
#### リクエスト
| 名前       | 種類 | 形式 | 必須 | 説明                                               |
|----------|---|---|---|--------------------------------------------------|
| tenantId | URL | String | O | テナントID                                           |
| serverId | URL | UUID | O | インスタンスID                                          |
| key      | URL | String | O | インスタンスに作成または更新するメタデータのキー                         |
| tokenId  | Header | String | O | トークンID                                            |
| meta     | Body | Object | O | インスタンスに作成または更新するメタデータオブジェクト<br>最大255文字以下のキーと値のペア |

<details>
<summary>例</summary>
<p>

```json
{
    "meta": {
        "os_version": "Server 20.04 LTS"
    }
}
```

</p>
</details>


<a id="createmodify-instance-metadata-response"></a>
#### レスポンス

| 名前   | 種類 | 形式 | 説明                                               |
|------|---|---|--------------------------------------------------|
| meta | Body | Object | インスタンスに作成または更新するメタデータオブジェクト<br>最大255文字以下のキーと値のペア |

<details><summary>例</summary>
<p>

```json
{
    "meta": {
        "os_version": "Server 20.04 LTS"
    }
}
```

</p>
</details>


<a id="delete-instance-metadata"></a>
### インスタンスメタデータの削除 { #delete-instance-metadata }

リクエストするキーと一致するインスタンスのメタデータを削除します。

```
DELETE /v2.1/{tenantId}/servers/{serverId}/metadata/{key}
X-Auth-Token: {tokenId}
```

<a id="delete-instance-metadata-request"></a>
#### リクエスト
このAPIはリクエスト本文を必要としません。

| 名前       | 種類 | 形式 | 必須 | 説明                  |
|----------|---|---|---|---------------------|
| tenantId | URL | String | O | テナントID              |
| serverId | URL | UUID | O | インスタンスID             |
| key      | URL | String | O | インスタンスから削除するメタデータのキー |
| tokenId  | Header | String | O | トークンID               |

<a id="delete-instance-metadata-response"></a>
#### レスポンス
このAPIはレスポンス本文を返しません。


<a id="placement-policy"></a>

## 配置ポリシー { #placement-policy }

<a id="create-a-placement-policy"></a>
### 配置ポリシーの作成 { #create-a-placement-policy }

配置ポリシーを作成します。
分散配置のための`anti-affinity`配置ポリシータイプのみ提供します。

```
POST /v2.1/{tenantId}/os-server-groups
X-Auth-Token: {tokenId}
```

<a id="create-a-placement-policy-request"></a>
#### リクエスト
| 名前 | 種類 | 形式 | 必須 | 説明 |
|-----|-----|-----|-----|-----|
| tenantId | URL | String | O | テナントID |
| tokenId | Header | String | O | トークンID |
| server_group | Body | Object | O | 配置ポリシーオブジェクト |
| server_group.name | Body | String | O | 配置ポリシー名 |
| server_group.policies | Body | Array | O | 配置ポリシータイプ<br>`anti-affinity`のみ設定可 |

<details>
<summary>例</summary>
<p>

```json
{
    "server_group": {
        "name": "policy-test1",
        "policies": [
            "anti-affinity"            
        ]
    }
}
```

</p>
</details>

<a id="create-a-placement-policy-response"></a>
#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
|-----|-----|-----|-----|
| server_group | Body | Object | 配置ポリシーオブジェクト |
| server_group.id | Body | String | 配置ポリシーID |
| server_group.name | Body | String | 配置ポリシー名 |
| server_group.policies | Body | Array | 配置ポリシータイプ |
| server_group.members | Body | Array | 配置ポリシーに割り当てられたインスタンスIDの一覧 |
| server_group.metadata | Body | Object | 配置ポリシーメタデータオブジェクト<br>常に空の値で表示されます |

<details><summary>例</summary>
<p>

```json
{
    "server_group": {
        "id": "11f5a850-9ecc-4895-af77-de6ea471b65a",
        "name": "policy-test1",
        "policies": [
            "anti-affinity"
        ],
        "members": [],
        "metadata": {}
    }
}
```

</p>
</details>

<a id="view-the-list-of-placement-policies"></a>
### 配置ポリシー一覧の表示 { #view-the-list-of-placement-policies }

```
GET /v2.1/{tenantId}/os-server-groups
X-Auth-Token: {tokenId}
```

<a id="view-the-list-of-placement-policies-request"></a>
#### リクエスト

このAPIはリクエスト本文を必要としません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
|-----|-----|-----|-----|-----|
| tenantId | URL | String | O | テナントID |
| tokenId | Header | String | O | トークンID |

<a id="view-the-list-of-placement-policies-response"></a>
#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
|-----|-----|-----|-----|
| server_groups | Body | Array | 配置ポリシーオブジェクトの一覧 |
| server_groups.id | Body | String | 配置ポリシーID |
| server_groups.name | Body | String | 配置ポリシー名 |
| server_groups.policies | Body | Array | 配置ポリシータイプ |
| server_groups.members | Body | Array | 配置ポリシーに割り当てられたインスタンスIDの一覧 |
| server_groups.metadata | Body | Object | 配置ポリシーメタデータオブジェクト<br>常に空の値で表示されます |

<details><summary>例</summary>
<p>

```json
{
    "server_groups": [
        {
            "id": "11f5a850-9ecc-4895-af77-de6ea471b65a",
            "name": "policy-test1",
            "policies": [
                "anti-affinity"
            ],
            "members": [
                "c040455d-6495-4628-ad81-ade79cf7b8d6",
                "524e7d81-f373-43a0-b2ff-0a15f8255bb5"            
            ],
            "metadata": {}
        },
        {
            "id": "f947c657-cbe0-4bf2-a2aa-59d198f8e096",
            "name": "policy-test2",
            "policies": [
                "anti-affinity"
            ],
            "members": [],
            "metadata": {}
        }
    ]
}
```

</p>
</details>

<a id="view-placement-policies"></a>
### 配置ポリシーの表示 { #view-placement-policies }

```
GET /v2.1/{tenantId}/os-server-groups/{servergroupId}
X-Auth-Token: {tokenId}
```

<a id="view-placement-policies-request"></a>
#### リクエスト

このAPIはリクエスト本文を必要としません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
|-----|-----|-----|-----|-----|
| tenantId | URL | String | O | テナントID |
| servergroupId | URL | String | O | 配置ポリシーID |
| tokenId | Header | String | O | トークンID |

<a id="view-placement-policies-response"></a>
#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
|-----|-----|-----|-----|
| server_group | Body | Object | 配置ポリシーオブジェクト |
| server_group.id | Body | String | 配置ポリシーID |
| server_group.name | Body | String | 配置ポリシー名 |
| server_group.policies | Body | Array | 配置ポリシータイプ |
| server_group.members | Body | Array | 配置ポリシーに割り当てられたインスタンスIDの一覧 |
| server_group.metadata | Body | Object | 配置ポリシーメタデータオブジェクト<br>常に空の値で表示されます |

<details><summary>例</summary>
<p>

```json
{
    "server_group": {
        "id": "11f5a850-9ecc-4895-af77-de6ea471b65a",
        "name": "policy-test1",
        "policies": [
            "anti-affinity"
        ],
        "members": [
            "c040455d-6495-4628-ad81-ade79cf7b8d6",
            "524e7d81-f373-43a0-b2ff-0a15f8255bb5"            
        ],
        "metadata": {}
    }
}
```

</p>
</details>

<a id="deleting-a-placement-policy"></a>
### 配置ポリシーの削除 { #deleting-a-placement-policy }

```
DELETE /v2.1/{tenantId}/os-server-groups/{servergroupId}
X-Auth-Token: {tokenId}
```

<a id="deleting-a-placement-policy-request"></a>
#### リクエスト

このAPIはリクエスト本文を必要としません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
|-----|-----|-----|-----|-----|
| tenantId | URL | String | O | テナントID |
| servergroupId | URL | String | O | 配置ポリシーID |
| tokenId | Header | String | O | トークンID |

<a id="deleting-a-placement-policy-response"></a>
#### レスポンス

このAPIはレスポンス本文を返しません。