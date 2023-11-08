## Compute > Instance > API v2ガイド

APIを使用するにはAPIエンドポイントとトークンなどが必要です。 [API使用準備](/Compute/Compute/ja/identity-api/)を参照してAPIを使用するのに必要な情報を準備します。

インスタンスAPIは`compute`タイプエンドポイントを利用します。正確なエンドポイントはトークン発行レスポンスの`serviceCatalog`を参照します。

| タイプ | リージョン | エンドポイント |
|---|---|---|
| compute | 韓国(パンギョ)リージョン<br>韓国(ピョンチョン)リージョン<br>日本リージョン | https://kr1-api-instance-infrastructure.nhncloudservice.com<br>https://kr2-api-instance-infrastructure.nhncloudservice.com<br>https://jp1-api-instance-infrastructure.nhncloudservice.com |

APIレスポンスにガイドに明示されていないフィールドが表示される場合があります。それらのフィールドは、NHN Cloud内部用途で使用され、事前に告知せずに変更する場合があるため使用しないでください。

## インスタンスタイプ

### タイプリスト表示

```
GET /v2/{tenantId}/flavors
X-Auth-Token: {tokenId}
```

#### リクエスト

このAPIはリクエスト本文を要求しません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tenantId | URL | String | O | テナントID |
| tokenId | Header | String | O | トークンID |
| minDisk | Query | Integer | - | 最小ブロックストレージサイズ(GB)<br>指定したサイズよりブロックストレージサイズが大きいタイプのみ返す |
| minRam | Query | Integer | - | 最小RAMサイズ(MB)<br>指定したサイズよりRAMサイズが大きいタイプのみ返す |

#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
|---|---|---|---|
| flavors | Body | Object | インスタンスタイプリストオブジェクト |
| flavors.id | Body | UUID | インスタンスタイプID |
| flavors.links | Body | Object | インスタンスタイプパスオブジェクト |
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

### タイプリスト詳細表示

```
GET /v2/{tenantId}/flavors/detail
X-Auth-Token: {tokenId}
```

#### リクエスト

このAPIはリクエスト本文を要求しません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tenantId | URL | String | O | テナントID |
| tokenId | Header | String | O | トークンID |
| minDisk | Query | Integer | - | 最小ブロックストレージサイズ(GB)<br>指定したサイズよりブロックストレージサイズが大きいタイプのみ返す |
| minRam | Query | Integer | - | 最小RAMサイズ(MB)<br>指定したサイズよりRAMサイズが大きいタイプのみ返す |

#### レスポンス

| 名前 | 種類 | 形式 | 説明            |
|---|---|---|----------------|
| flavors | Body | Object | インスタンスタイプリストオブジェクト |
| flavors.id | Body | UUID | インスタンスタイプID     |
| flavors.links | Body | Object | インスタンスタイプパスオブジェクト |
| flavors.name | Body | String | インスタンスタイプ名    |
| flavors.ram | Body | Integer | メモリサイズ(MB)     |
| flavors.OS-FLV-DISABLED:disabled | Body | Boolean | 有効/無効         |
| flavors.vcpus | Body | Integer | vCPU数       |
| flavors.extra_specs | Body | Object | 追加仕様オブジェクト      |
| flavors.swap | Body | Integer | スワップ領域サイズ(GB)  |
| flavors.os-flavor-access:is_public | Body | Boolean | 共有有無          |
| flavors.rxtx_factor | Body | Float | ネットワーク送信/受信パケット比率 |
| flavors.OS-FLV-EXT-DATA:ephemeral | Body | Integer | 臨時ブロックストレージサイズ(GB) |
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

## アベイラビリティゾーン

### 可用性リスト表示

```
GET /v2/{tenantId}/os-availability-zone
X-Auth-Token: {tokenId}
```

#### リクエスト
このAPIはリクエスト本文を要求しません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tenantId | URL | String | O | テナントID |
| tokenId | Header | String | O | トークンID |

#### レスポンス
| 名前 | 種類 | 形式 | 説明 |
|---|---|---|---|
| availabilityZoneInfo.hosts | Body | - | アベイラビリティゾーンに属しているホスト情報オブジェクト<br>常にnullと表示 |
| availabilityZoneInfo.zoneName | Body | String | アベイラビリティゾーン名 |
| availabilityZoneInfo.zoneState | Body | Object | アベイラビリティゾーン状態情報オブジェクト |
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

## キーペア

### キーペアリスト表示
```
GET /v2/{tenantId}/os-keypairs
X-Auth-Token: {tokenId}
```

#### リクエスト
このAPIはリクエスト本文を要求しません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tenantId | URL | String | O | テナントID |
| tokenId | Header | String | O | トークンID |

#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
|---|---|---|---|
| keypairs | Body | Array | キーペアオブジェクトリスト |
| keypairs.keypair | Body | Object | キーペアオブジェクト |
| keypairs.keypair.name | Body | String | キーペア名 |
| keypairs.keypair.public_key | Body | String | 公開鍵 |
| keypairs.keypair.fingerprint | Body | String | キーペア指紋 |

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

### キーペア表示
```
GET /v2/{tenantId}/os-keypairs/{keypairName}
X-Auth-Token: {tokenId}
```

#### リクエスト
このAPIはリクエスト本文を要求しません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tenantId | URL | String | O | テナントID |
| keypairName | URL | String | O | キーペア名 |
| tokenId | Header | String | O | トークンID |

#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
|---|---|---|---|
| keypair | Body | Object | キーペアオブジェクトリスト |
| keypair.public_key | Body | String | 公開鍵 |
| keypair.user_id | Body | String | キーペアのオーナーID |
| keypair.name | Body | String | キーペア名 |
| keypair.deleted | Body | Boolean | キーペアが削除されているかどうか |
| keypair.created_at | Body | Datetime | キーペア作成日時<br>`YYYY-MM-DDThh:mm:ss.SSSSSS` |
| keypair.updated_at | Body | Datetime | キーペア修正日時<br>`YYYY-MM-DDThh:mm:ss.SSSSSS` |
| keypair.deleted_at | Body | Datetime | キーペア削除日時<br>`YYYY-MM-DDThh:mm:ss.SSSSSS` |
| keypair.fingerprint | Body | String | キーペア指紋 |
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

### キーペアの作成/登録

```
POST /v2/{tenantId}/os-keypairs
X-Auth-Token: {tokenId}
```

#### リクエスト

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tenantId | URL | String | O | テナントID |
| tokenId | Header | String | O | トークンID |
| keypair | Body | Object | O | キーペアオブジェクト |
| keypair.name | Body | String | O | 作成または登録するキーペア名 |
| keypair.public_key | Body | String | - | 登録する公開鍵。このフィールドが省略されている場合、新しいキーペアを作成します。|

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

#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
|---|---|---|---|
| keypair | Body | Object | キーペアオブジェクト |
| keypair.public_key | Body | String | 公開鍵 |
| keypair.private_key | Body | String | 秘密鍵。新しいキーペアを作成した場合に秘密鍵を返します。|
| keypair.user_id | Body | String | キーペアのオーナーID |
| keypair.name | Body | String | キーペア名 |
| keypair.fingerprint | Body | String | キーペア指紋 |

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

### キーペアを削除する
```
DELETE /v2/{tenantId}/os-keypairs/{keypairName}
X-Auth-Token: {tokenId}
```

#### リクエスト
このAPIはリクエスト本文を要求しません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tenantId | URL | String | O | テナントID |
| keypairName | URL | String | O | キーペア名 |
| tokenId | Header | String | O | トークンID |

#### レスポンス
このAPIはレスポンス本文を返しません。


## インスタンス

### インスタンス状態

インスタンスはさまざまな状態を持ち、状態によって行える動作が決められています。インスタンス状態リストは次のとおりです。

| 状態名              | 説明                                                                                               |
|-------------------|---------------------------------------------------------------------------------------------------|
| `ACTIVE`          | インスタンスがアクティブな状態の場合 |
| `BUILDING`        | インスタンスが作成中の場合 |
| `STOPPED`         | インスタンスが停止している場合 |
| `SHELVED_OFFLOADED` | インスタンスが終了した場合                                                                                     |
| `DELETED`         | インスタンスが削除されている場合 |
| `REBOOT`          | インスタンスを再起動した場合 |
| `HARD_REBOOT`     | インスタンスを強制的に再起動した場合。物理サーバーの電源を落とし、再び入れるのと同じ動作。                                               |
| `RESIZED`         | インスタンスタイプを変更したり、インスタンスを他のホストへ移した場合。<br>インスタンスが停止して、再び起動した状態。                                     |
| `REVERT_RESIZE`   | インスタンスタイプを変更したり、インスタンスを他のホストへ移す過程で失敗した時、元の状態に戻すために復旧する場合                                |
| `VERIFY_RESIZE`   | インスタンスが、タイプ変更またはインスタンスを他のホストへ移す過程を終えてユーザーの承認を待っている場合。<br>NHN Cloudではこの場合、自動的に`ACTIVE`状態に変わる。|
| `ERROR`           | 直前にインスタンスに行った動作が失敗した場合                                                                           |
| `PAUSED`          | インスタンスが一時停止した場合。一時停止したインスタンスはハイパーバイザのメモリに保存される。                                                  |
| `REBUILD`         | インスタンスを作成時、イメージから新たに作り出す状態                                                                  |
| `RESCUED`         | インスタンスを復旧モードで実行中                                                                               |
| `SUSPENDED`       | インスタンスが管理者により最大節電モードになっている場合                                                                   |
| `UNKNOWN`         | インスタンスの状態が不明な場合。`インスタンスがこの状態になった場合、管理者に問い合わせます。`                                          |

### インスタンスリスト表示

```
GET /v2/{tenantId}/servers
X-Auth-Token: {tokenId}
```

#### リクエスト

このAPIはリクエスト本文を要求しません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tenantId | URL | String | O | テナントID |
| tokenId | Header | String | O | トークンID |
| reservation_id | Query | String | - | インスタンス作成予約ID。<br>予約IDを指定すると、同時に作成されたインスタンスリストのみ返す。 |
| changes-since | Query | Datetime | - | 指定された日時以降に変更されたインスタンスリストを返す。`YYYY-MM-DDThh:mm:ss`の形式。|
| image | Query | UUID | - | イメージID<br>指定されたイメージを使用したインスタンスリストを返す |
| flavor | Query | UUID | - | インスタンスタイプID<br>指定されたタイプを使用しているインスタンスリストを返す |
| name | Query | String | - | インスタンス名<br>指定された名前のインスタンスリストを返す。正規表現を使用可能。|
| status | Query | Enum | - | インスタンスの状態<br>指定された状態のインスタンスリストを返す |
| limit | Query | Integer | - | インスタンスリスト数<br>指定された数のインスタンスリストを返す |
| marker | Query | UUID | - | リストの最初のインスタンスUUID<br>ソート基準に従って`marker`に指定されたインスタンスから`limit`数分のインスタンスリストを返す |

#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
|---|---|---|---|
| servers | Body | Object | インスタンスリストオブジェクト |
| id | Body | UUID | インスタンスUUID |
| links | body | Object | インスタンスパスオブジェクト |
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

### インスタンスリスト詳細表示

インスタンスリスト表示と同じように現在テナントに作成されているインスタンスリストを返します。ただし、各インスタンスの詳細な情報が一緒に照会されます。

```
GET /v2/{tenantId}/servers/detail
X-Auth-Token: {tokenId}
```

#### リクエスト

インスタンスリスト表示と同じリクエスト形式です。

#### レスポンス

| 名前 | 種類 | 形式 | 説明                                                                                                                                                                                                       |
|---|---|---|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| servers | body | Object | インスタンスリストオブジェクト                                                                                                                                                                                               |
| status | body | Enum | インスタンスの状態                                                                                                                                                                                                  |
| servers.id | Body | UUID | インスタンスID                                                                                                                                                                                                   |
| servers.name | Body | String | インスタンス名。最大255文字。                                                                                                                                                                                         |
| servers.updated | Body | Datetime | インスタンスの最終修正日時。`YYYY-MM-DDThh:mm:ssZ`形式。                                                                                                                                                                 |
| servers.hostId | Body | String | インスタンスが起動中のホストID                                                                                                                                                                                        |
| servers.addresses | Body | Object | インスタンスIPリストオブジェクト。<br>インスタンスに接続されたポート数分のリストが作成される。                                                                                                                                                             |
| servers.addresses."Network名" | Body | Object | インスタンスに接続されている各Networkのポート情報                                                                                                                                                                                 |
| servers.addresses."Network名".OS-EXT-IPS-MAC:mac_addr | Body | String | インスタンスに接続されたポートのMACアドレス                                                                                                                                                                                     |
| servers.addresses."Network名".version | Body | Integer | インスタンスに接続されたポートのIPバージョン<br>NHN CloudはIPv4のみサポート                                                                                                                                                               |
| servers.addresses."Network名".addr | Body | String | インスタンスに接続されたポートのIPアドレス                                                                                                                                                                                      |
| servers.addresses."Network名".OS-EXT-IPS:type | Body | Enum | ポートのIPアドレスタイプ<br>`fixed`または`floating`のいずれか1つ                                                                                                                                                                |
| servers.links | Body | Object | インスタンスパスオブジェクト                                                                                                                                                                                               |
| servers.key_name | Body | String | インスタンスキーペア名                                                                                                                                                                                              |
| servers.image | Body | Object | インスタンスイメージオブジェクト                                                                                                                                                                                              |
| servers.image.id | Body | UUID | インスタンスイメージID                                                                                                                                                                                               |
| servers.image.links | Body | Object | インスタンスイメージパスオブジェクト                                                                                                                                                                                           |
| servers.OS-EXT-STS:task_state | Body | String | インスタンス作業状態<br>インスタンスに動作を加えた時、動作進行状態を伝える。                                                                                                                                                               |
| servers.OS-EXT-STS:vm_state | Body | String | インスタンスの現在の状態                                                                                                                                                                                               |
| servers.OS-SRV-USG:launched_at | Body | Datetime | インスタンスの最終起動日時<br>`YYYY-MM-DDThh:mm:ss.ssssss`形式                                                                                                                                                        |
| servers.OS-SRV-USG:terminated_at | Body | Datetime | インスタンスの削除日時<br>`YYYY-MM-DDThh:mm:ssZ`形式                                                                                                                                                                  |
| servers.flavor | Body | Object | インスタンスタイプ情報オブジェクト                                                                                                                                                                                            |
| servers.flavor.id | Body | UUID | インスタンスタイプID                                                                                                                                                                                                |
| servers.flavor.links | Body | Object | インスタンスタイプパスオブジェクト                                                                                                                                                                                            |
| servers.security_groups | Body | Object | インスタンスに割り当てられたセキュリティグループリストオブジェクト                                                                                                                                                                                    |
| servers.security_groups.name | Body | String | インスタンスに割り当てられたセキュリティグループ名                                                                                                                                                                                       |
| servers.user_id | Body | String | インスタンスを作成したユーザーID                                                                                                                                                                                          |
| servers.created | Body | Datetime | インスタンス作成日時。`YYYY-MM-DDThh:mm:ssZ`形式                                                                                                                                                                    |
| servers.tenant_id | Body | String | インスタンスが属しているテナントID                                                                                                                                                                                           |
| servers.OS-DCF:diskConfig | Body | Enum | インスタンスブロックストレージパーティション方式。`MANUAL`または`AUTO`のいずれか1つ。<br>**AUTO**：自動的に全ブロックストレージを1つのパーティションに設定<br>**MANUAL**：イメージに指定された通りにパーティションを設定。イメージで設定されたサイズよりブロックストレージのサイズが大きい場合、使用しないまま残す。NHN Cloudは`MANUAL`を使用 |
| servers.os-extended-volumes:volumes_attached | Body | Object | インスタンスに接続された追加ブロックストレージリストオブジェクト                                                                                                                                                                               |
| servers.os-extended-volumes:volumes_attached.id | Body | UUID | インスタンスに接続された追加ブロックストレージID                                                                                                                                                                                   |
| servers.OS-EXT-STS:power_state | Body | Integer | インスタンスの電源の状態<br>- `1`: On<br>- `4`: Off                                                                                                                                                                    |
| servers.metadata | Body | Object | インスタンスメタデータオブジェクト<br>インスタンスメタデータをキーと値のペアで保管                                                                                                                                                                  |

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

### インスタンス表示

```
GET /v2/{tenantId}/servers/{serverId}
X-Auth-Token: {tokenId}
```

#### リクエスト

このAPIはリクエスト本文を要求しません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tenantId | URL | String | O | テナントID |
| serverId | URL | UUID | O | インスタンスID |
| tokenId | Header | String | O | トークンID |

#### レスポンス

| 名前 | 種類 | 形式 | 説明                                                                                                                                                                                                      |
|---|---|---|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| server | body | Object | インスタンスオブジェクト                                                                                                                                                                                                 |
| status | body | Enum | インスタンスの状態                                                                                                                                                                                                 |
| server.id | Body | UUID | インスタンスID                                                                                                                                                                                                  |
| server.name | Body | String | インスタンス名、最大255文字                                                                                                                                                                                        |
| server.updated | Body | Datetime | インスタンスの最終修正日時。`YYYY-MM-DDThh:mm:ssZ`形式。                                                                                                                                                                |
| server.hostId | Body | String | インスタンスが起動中のホストID                                                                                                                                                                                       |
| server.addresses | Body | Object | インスタンスIPリストオブジェクト。<br>インスタンスに接続されたポート数分のリストが作成される。                                                                                                                                                             |
| server.addresses."Network名" | Body | Object | インスタンスに接続された各Networkのポート情報                                                                                                                                                                                |
| server.addresses."Network名".OS-EXT-IPS-MAC:mac_addr | Body | String | インスタンスに接続されたポートのMACアドレス                                                                                                                                                                                    |
| server.addresses."Network名".version | Body | Integer | インスタンスに接続されたポートのIPバージョン<br>NHN CloudはIPv4のみサポート                                                                                                                                                              |
| server.addresses."Network名".addr | Body | String | インスタンスに接続されたポートのIPアドレス                                                                                                                                                                                     |
| server.addresses."Network名".OS-EXT-IPS:type | Body | Enum | ポートのIPアドレスタイプ<br>`fixed`または`floating`のいずれか1つ。                                                                                                                                                               |
| server.links | Body | Object | インスタンスパスオブジェクト                                                                                                                                                                                              |
| server.key_name | Body | String | インスタンスキーペア名                                                                                                                                                                                             |
| server.image | Body | Object | インスタンスイメージオブジェクト                                                                                                                                                                                             |
| server.image.id | Body | UUID | インスタンスイメージID                                                                                                                                                                                              |
| server.image.links | Body | Object | インスタンスイメージパスオブジェクト                                                                                                                                                                                          |
| server.OS-EXT-STS:task_state | Body | String | インスタンス作業状態<br>インスタンスに動作を加えた時、動作進行状態を伝える。                                                                                                                                                              |
| server.OS-EXT-STS:vm_state | Body | String | インスタンスの現在状態                                                                                                                                                                                              |
| server.OS-SRV-USG:launched_at | Body | Datetime | インスタンスの最終起動日時<br>`YYYY-MM-DDThh:mm:ss.ssssss`形式                                                                                                                                                       |
| server.OS-SRV-USG:terminated_at | Body | Datetime | インスタンスの削除日時<br>`YYYY-MM-DDThh:mm:ssZ`形式                                                                                                                                                                 |
| server.flavor | Body | Object | インスタンスタイプ情報オブジェクト                                                                                                                                                                                           |
| server.flavor.id | Body | UUID | インスタンスタイプID                                                                                                                                                                                               |
| server.flavor.links | Body | Object | インスタンスタイプパスオブジェクト                                                                                                                                                                                           |
| server.security_groups | Body | Object | インスタンスに割り当てられたセキュリティグループリストオブジェクト                                                                                                                                                                                   |
| server.security_groups.name | Body | String | インスタンスに割り当てられたセキュリティグループ名                                                                                                                                                                                      |
| server.user_id | Body | String | インスタンスを作成したユーザーID                                                                                                                                                                                         |
| server.created | Body | Datetime | インスタンスの作成日時。`YYYY-MM-DDThh:mm:ssZ`形式                                                                                                                                                                   |
| server.tenant_id | Body | String | インスタンスが属しているテナントID                                                                                                                                                                                          |
| server.OS-DCF:diskConfig | Body | Enum | インスタンスブロックストレージパーティション方式。 `MANUAL`または`AUTO`のいずれか。<br>**AUTO**：自動的にブロックストレージ全体を1つのパーティションに設定<br>**MANUAL**：イメージに指定されたとおりにパーティションを設定。イメージで設定されたサイズよりブロックストレージのサイズが大きい場合、使用せずに残す。NHN Cloudは`MANUAL`を使用 |
| server.os-extended-volumes:volumes_attached | Body | Object | インスタンスに接続された追加ブロックストレージリストオブジェクト                                                                                                                                                                              |
| server.os-extended-volumes:volumes_attached.id | Body | UUID | インスタンスに接続された追加ブロックストレージID                                                                                                                                                                                  |
| server.OS-EXT-STS:power_state | Body | Integer | インスタンスの電源の状態<br>- `1`: On<br>- `4`: Off                                                                                                                                                                   |
| server.metadata | Body | Object | インスタンスメタデータオブジェクト<br>インスタンスメタデータをキーと値のペアで保管                                                                                                                                                                 |
| server.NHN-EXT-ATTR:ephemeral_disk_size | Body | Integer | インスタンスに接続された追加ローカルブロックストレージサイズ                                                                                                                                                                              |

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

### インスタンスを作成する

インスタンスを作成します。

インスタンス作成APIを呼び出し、インスタンス照会でインスタンスの状態を確認します。

* インスタンスの状態が**ACTIVE**になるとインスタンスが正常に作成完了します。
* インスタンスの状態が**BUILDING**から長時間変わらなかったり、**ERROR**の場合は、インスタンス作成引数を確認し、再度作成してください。

* RAMが2GB以上のインスタンスタイプを使用します。

* Windowsインスタンスは2GB以上のRAMが必要です。RAM 2GB以上のインスタンスタイプを使用します。
* 50GB以上のルートブロックストレージが必要です。
* U2タイプはWindowsイメージを使用できません。

ルートブロックストレージサイズは、Linuxは10GB、Windowsは50GBから指定できます。


```
POST /v2/{tenantId}/servers
X-Auth-Token: {tokenId}
```

#### リクエスト

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tenantId | URL | String | O | テナントID |
| tokenId | Header | String | O | トークンID |
| server.security_groups | body | Object | - | セキュリティグループリストオブジェクト<br>省略する場合`default`グループが追加される |
| server.security_groups.name | body | String | - | インスタンスに追加するセキュリティグループ名 |
| server.user_data | body | String | - | インスタンス起動後に実行するスクリプトおよび設定<br>base64エンコーディングされた文字列で65535バイトまで許可 |
| server.availability_zone | body | String | - | インスタンスを作成するアベイラビリティゾーン<br>指定しない場合、任意で選択される |
| server.imageRef | Body | String | O | インスタンスを作成する時に使用するイメージID |
| server.flavorRef | Body | String | O | インスタンスを作成する時に使用するインスタンスタイプID |
| server.networks | Body | Object | O | インスタンスを作成する時に使用するネットワーク情報オブジェクト<br>指定した数のNICが追加される。ネットワークID、サブネットID、ポートID、固定IPの中から1つ指定 |
| server.networks.uuid | Body | UUID | - | インスタンスを作成する時に使用するネットワークID |
| server.networks.subnet | Body | UUID | - | インスタンスを作成する時に使用するネットワークのサブネットID |
| server.networks.port | Body | UUID | - | インスタンスを作成する時に使用するポートID |
| server.networks.fixed_ip | Body | String | - | インスタンスを作成する時に使用する固定IP |
| server.name | Body | String | O | インスタンスの名前<br>英字基準255文字まで許可、ただし、Windowsイメージの場合は15文字以下にする必要がある。 |
| server.metadata | Body | Object | - | インスタンスに追加するメタデータオブジェクト<br>255文字以下のキーと値のペア |
| server.block_device_mapping_v2 | Body | Object | - | インスタンスのブロックストレージ情報オブジェクト<br>**ローカルブロックストレージを使用するU2以外のインスタンスタイプを使用する場合は必ず指定する必要がある。** |
| server.block_device_mapping_v2.uuid | Body | String | - | ブロックストレージの原本ID <br>ルートブロックストレージの場合、必ず起動可能な原本でなければならず、イメージの作成ができないWAF、MS-SQL, MySQLイメージが原本のvolumeやsnapshotは使用できません。<br> `image`を除く原本は作成するインスタンスのアベイラビリティゾーンが同じである必要がある |
| server.block_device_mapping_v2.source_type | Body | Enum | - | 作成するブロックストレージ原本のタイプ<br>`image`：イメージを利用してブロックストレージ作成<br>`volume`：既に作成されたブロックストレージを使用、 destination_typeは必ずvolumeを指定<br>`snapshot`：スナップショットを利用してブロックストレージ作成、 destination_typeは必ずvolumeを指定 |
| server.block_device_mapping_v2.destination_type | Body | Enum | - | インスタンスブロックストレージの位置。インスタンスタイプに応じて別々に設定必要。<br>- `local`：U2インスタンスタイプを利用する場合。<br>- `volume`：U2以外のインスタンスタイプを利用する場合。 |
| server.block_device_mapping_v2.volume_type | Body | String | - | 作成されるブロックストレージのタイプ<br>-`General HDD`：HDDタイプのブロックストレージ<br>-`General SSD`：SSDタイプのブロックストレージ<br> 省略した場合`General HDD`が適用される |
| server.block_device_mapping_v2.delete_on_termination | Body | Boolean | - | インスタンスを削除する時のブロックストレージ処理。デフォルト値は`false`。<br>`true`なら削除、`false`なら維持 |
| server.block_device_mapping_v2.boot_index | Body | Integer | - | 指定したブロックストレージの起動順序<br>-`0`はルートブロックストレージ<br>- それ以外は追加ブロックストレージ<br>サイズが大きいほど起動順序が下がる。 |
| server.key_name | Body | String | O | インスタンスの接続に使用するキーペア |
| server.min_count | Body | Integer | - | 現在のリクエストで作成するインスタンス数の最小値。<br>デフォルト値は1。 |
| server.max_count | Body | Integer | - | 現在のリクエストで作成するインスタンス数の最大値。<br>デフォルト値はmin_count、最大値は10。 |
| server.return_reservation_id | Body | Boolean | - | インスタンス作成リクエスト予約ID。<br>Trueに指定すると、インスタンス作成情報の代わりに予約IDを返す。<br>デフォルト値はFalse |

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

#### レスポンス

| 名前 | 種類 | 形式 | 説明                                                                                                                                                                                                          |
|---|---|---|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| server.security_groups.name | Body | String | 作成したインスタンスのセキュリティグループ名                                                                                                                                                                                          |
| server.OS-DCF:diskConfig | Body | Enum | インスタンスブロックストレージパーティション方式。 `MANUAL`または`AUTO`のいずれか。NHN Cloudでは`MANUAL`に設定されている。<br>**AUTO**：自動的にブロックストレージ全体を1つのパーティションに設定<br>**MANUAL**：イメージに指定されたとおりにパーティションを設定。イメージで設定されたサイズよりブロックストレージのサイズが大きい場合、使用せずに残す。 |
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

### インスタンスを修正する
作成されたインスタンスを修正します。変更できるプロパティは一部の項目に制限されます。

```
PUT /v2/{tenantId}/servers/{serverId}
X-Auth-Token: {tokenId}
```

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

#### レスポンス
インスタンスの表示と同じです。

---

### インスタンスを削除する
作成されたインスタンスを削除します。

```
DELETE /v2/{tenantId}/servers/{serverId}
X-Auth-Token: {tokenId}
```

#### リクエスト
このAPIはリクエスト本文を要求しません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|--|
| tenantId | URL | String | O | テナントID |
| serverId | URL | UUID | O | 削除するインスタンスID |
| tokenId | Header | String | O | トークンID |

#### レスポンス
このAPIはレスポンス本文を返しません。

---

## ブロックストレージ接続管理
### インスタンスに接続されたブロックストレージリスト表示
```
GET /v2/{tenantId}/servers/{serverId}/os-volume_attachments
X-Auth-Token: {tokenId}
```

#### リクエスト
このAPIはリクエスト本文を要求しません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|--|
| tenantId | URL | String | O | テナントID |
| serverId | URL | UUID | O | 変更するインスタンスID |
| tokenId | Header | String | O | トークンID |
| limit | Query | Integer | - | 照会するリストの数 |
| offset | Query | Integer | - | 返されるリストの開始点<br>全てのリストの中からoffset番目のブロックストレージから返す |

#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
|---|---|---|---|
| volumeAttachments | Body | Array | 接続情報オブジェクトリスト |
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

### インスタンスに接続されたブロックストレージ表示
```
GET /v2/{tenantId}/servers/{serverId}/os-volume_attachments/{volumeId}
X-Auth-Token: {tokenId}
```

#### リクエスト
このAPIはリクエスト本文を要求しません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|--|
| tenantId | URL | String | O | テナントID |
| serverId | URL | UUID | O | インスタンスID |
| volumeId | URL | UUID | O | 照会するブロックストレージID |
| tokenId | Header | String | O | トークンID |

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

### インスタンスに追加ブロックストレージを接続する
```
POST /v2/{tenantId}/servers/{serverId}/os-volume_attachments
X-Auth-Token: {tokenId}
```

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

### インスタンスブロックストレージの接続を切る
```
DELETE /v2/{tenantId}/servers/{serverId}/os-volume_attachments/{volumeId}
X-Auth-Token: {tokenId}
```

#### リクエスト
このAPIはリクエスト本文を要求しません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|--|
| tenantId | URL | String | O | テナントID |
| serverId | URL | UUID | O | インスタンスID |
| volumeId | URL | UUID | O | 接続を切るブロックストレージID |
| tokenId | Header | String | O | トークンID |

#### レスポンス
このAPIはレスポンス本文を返しません。

---

## インスタンス追加機能
NHN Cloudは、次のようなインスタンス制御および付加機能を提供します。

* インスタンスの起動、停止、終了、再起動
* インスタンスタイプ変更
* インスタンスイメージ作成
* セキュリティグループの追加および削除

### 停止したインスタンスの起動

停止したインスタンスを再び起動し、状態を**ACTIVE**に変更します。このAPIを呼び出すにはインスタンスの状態が**SHUTOFF**になっている必要があります。

```
POST /v2/{tenantId}/servers/{serverId}/action
X-Auth-Token: {tokenId}
```

#### リクエスト
| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|--|
| tenantId | URL | String | O | テナントID |
| serverId | URL | UUID | O | 変更するインスタンスID |
| tokenId | Header | String | O | トークンID |
| os-start | Body | none | O | インスタンス起動リクエスト |

<details><summary>例</summary>
<p>

```json
{
  "os-start" : null
}
```

</p>
</details>

#### レスポンス
このAPIはレスポンス本文を返しません。

---

### 終了したインスタンスの起動

停止したインスタンスを再起動し、状態を**ACTIVE**に変更します。このAPIを呼び出すには、インスタンスの状態が**SHELVED_OFFLOADED**である必要があります。

```
POST /v2/{tenantId}/servers/{serverId}/action
X-Auth-Token: {tokenId}
```

#### リクエスト
| 名前 | 種類 | 形式 | 必須 | 説明 |
|--|---|---|---|--|
| tenantId | URL | String | O | テナントID |
| serverId | URL | UUID | O | 変更するインスタンスID |
| tokenId | Header | String | O | トークンID |
| unshelve | Body | none | O | インスタンス起動リクエスト |

<details><summary>例</summary>
<p>

```json
{
  "unshelve" : null
}
```

</p>
</details>

#### レスポンス
このAPIはレスポンス本文を返しません。

---

### インスタンス停止

インスタンスを停止し、状態を**SHUTOFF**に変更します。このAPIを呼び出すにはインスタンスの状態が**ACTIVE**または**ERROR**になっている必要があります。

```
POST /v2/{tenantId}/servers/{serverId}/action
X-Auth-Token: {tokenId}
```

#### リクエスト
| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|--|
| tenantId | URL | String | O | テナントID |
| serverId | URL | UUID | O | 変更するインスタンスID |
| tokenId | Header | String | O | トークンID |
| os-stop | Body | none | O | インスタンス停止リクエスト |

<details><summary>例</summary>
<p>

```json
{
  "os-stop" : null
}
```

</p>
</details>

#### レスポンス
このAPIはレスポンス本文を返しません。

---

### インスタンス停止

インスタンスを終了し、状態を**SHELVED_OFFLOADED**に変更します。このAPIを呼び出すためには、インスタンスの状態が**ACTIVE**でなければなりません。

```
POST /v2/{tenantId}/servers/{serverId}/action
X-Auth-Token: {tokenId}
```

#### リクエスト
| 名前 | 種類 | 形式 | 必須 | 説明         |
|---|---|---|---|-------------|
| tenantId | URL | String | O | テナントID      |
| serverId | URL | UUID | O | 変更するインスタンスID |
| tokenId | Header | String | O | トークンID       |
| shelve | Body | none | O | インスタンス停止リクエスト |

<details><summary>例</summary>
<p>

```json
{
  "shelve" : null
}
```

</p>
</details>

#### レスポンス
このAPIはレスポンス本文を返しません。

---

### インスタンス再起動

インスタンスを再起動します。再起動の方法は**SOFT**と**HARD**があります。

* **SOFT**方式：**「優雅な接続停止(Graceful shutdown)」**でインスタンスを停止し、再起動します。インスタンスが**ACTIVE**状態になっている必要があります。
* **HARD**方式：強制停止してインスタンスを再起動します。物理サーバーの電源を落とし、再び入れるのと同じ動作です。インスタンスが次の状態の時のみ強制停止できます。
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

#### リクエスト
| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|--|
| tenantId | URL | String | O | テナントID |
| serverId | URL | UUID | O | 変更するインスタンスID |
| tokenId | Header | String | O | トークンID |
| reboot | Body | Object | O | インスタンス再起動リクエストオブジェクト |
| reboot.type | Body | Enum | O | 再起動方法。**SOFT**または**HARD** |

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

#### レスポンス
このAPIはレスポンス本文を返しません。

---

### インスタンスタイプ変更

インスタンスタイプを変更します。インスタンスが**ACTIVE**または**SHUTOFF**状態の時のみインスタンスタイプを変更できます。インスタンスの状態が**ACTIVE**の場合はインスタンスタイプ変更過程でインスタンスは停止し、再起動します。

使用するイメージやインスタンスタイプによって、変更できるタイプが制限される場合があります。詳細はコンソールユーザーガイドを参照してください。


```
POST /v2/{tenantId}/servers/{serverId}/action
X-Auth-Token: {tokenId}
```

#### リクエスト
| 名前 | 種類 | 形式 | 必須 | 説明                                                                                                                                                                                                                |
|---|---|---|---|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| tenantId | URL | String | O | テナントID                                                                                                                                                                                                             |
| serverId | URL | UUID | O | 変更するインスタンスID                                                                                                                                                                                                        |
| tokenId | Header | String | O | トークンID                                                                                                                                                                                                              |
| resize | Body | Object | O | インスタンスタイプ変更リクエスト                                                                                                                                                                                                     |
| resize.flavorRef | Body | UUID | O | 変更するインスタンスタイプID                                                                                                                                                                                                     |
| resize.OS-DCF:diskConfig | Body | Enum | - | タイプ変更後、ブロックストレージパーティション方式。 `MANUAL`または`AUTO`のいずれか。NHN Cloudでは`MANUAL`に設定されている。<br>**AUTO**：自動的にブロックストレージ全体を1つのパーティションに設定<br>**MANUAL**：イメージに指定されたとおりにパーティションを設定。イメージで設定されたサイズよりブロックストレージのサイズが大きい場合、使用せずに残す。 |

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

#### レスポンス
このAPIはレスポンス本文を返しません。

---

### インスタンスイメージ作成

インスタンスからイメージを作成します。`U2`タイプのインスタンスのみ、このAPIでイメージを作成できます。`U2`タイプ以外のインスタンスイメージを作成するには[ブロックストレージAPI](/Storage/Block Storage/ja/public-api/#_22)を参照します。

インスタンスの状態が**ACTIVE**、**SHUTOFF**、**SUSPENDED**、**PAUSED**の時のみイメージを作成できます。イメージの作成は、データの整合性を保障するためにインスタンスを停止した状態で進行することを推奨します。

イメージの作成に成功すると、イメージの状態が`active`に変わります。イメージの作成が完了したことを確認するにはイメージ照会APIで持続的に状態を確認します。

```
POST /v2/{tenantId}/servers/{serverId}/action
X-Auth-Token: {tokenId}
```

#### リクエスト
| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|--|
| tenantId | URL | String | O | テナントID |
| serverId | URL | UUID | O | 変更するインスタンスID |
| tokenId | Header | String | O | トークンID |
| createImage | Body | Object | O | イメージ作成リクエスト |
| createImage.name | Body | String | O | 作成するイメージの名前 |
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


#### レスポンス

このAPIはレスポンス本文を返しません。作成されたイメージはレスポンスヘッダの`Location`で確認します。

| 名前 | 種類 | 形式 | 説明 |
|--|--|--|--|
| Location | Header | String | 作成したイメージURL |

---

### セキュリティグループ追加

インスタンスにセキュリティグループを追加します。追加したセキュリティグループはインスタンスのすべてのポートに適用されます。

```
POST /v2/{tenantId}/servers/{serverId}/action
X-Auth-Token: {tokenId}
```

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


#### レスポンス
このAPIはレスポンス本文を返しません。

---

### セキュリティグループ削除

インスタンスからセキュリティグループを削除します。インスタンスのすべてのポートから指定したセキュリティグループが削除されます。

```
POST /v2/{tenantId}/servers/{serverId}/action
X-Auth-Token: {tokenId}
```

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


#### レスポンス
このAPIはレスポンス本文を返しません。
