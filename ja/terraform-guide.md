## サードパーティー使用ガイド > Terraform使用ガイド
この文書はTerraformでNHN Cloudを使用する方法を説明します。

## Terraform
Terraformはインフラを簡単に構築し、安全に変更し、効率的にインフラの形状を管理できるオープンソースのツールです。Terraformの主な特徴は次のとおりです。

* **Infrastructure as Code**
    * インフラをコードで定義して生産性と透明性を高めることができます。
    * 定義したコードを簡単に共有でき、効率的に協業できます。
* **Execution Plan**
    * 変更計画と変更適用を分離して変更内容を適用する時に発生しうる失敗をへらすことができます。
* **Resource Graph**
    * 些細な変更がインフラ全体にどんな影響を与えるかを事前に確認できます。
    *従属性グラフを作成し、このグラフを元に計画を立て、この計画を適用した時に変更されるインフラの状態を確認できます。
* **Change Automation**
    * 複数の場所に同じ構成のインフラを構築し、変更できるように自動化できます。
    * インフラを構築するのにかかる時間を節約することができ、失敗も減らすことができます。


#### Resourcesサポート

* Compute
    * nhncloud_compute_instance_v2
    * nhncloud_compute_volume_attach_v2
    * nhncloud_compute_keypair_v2    
* Network
    * nhncloud_lb_loadbalancer_v2
    * nhncloud_lb_listener_v2
    * nhncloud_lb_pool_v2
    * nhncloud_lb_member_v2
    * nhncloud_lb_monitor_v2
    * nhncloud_networking_floatingip_v2
    * nhncloud_networking_floatingip_associate_v2
    * nhncloud_networking_port_v2
    * nhncloud_networking_vpc_v2
    * nhncloud_networking_vpcsubnet_v2
    * nhncloud_networking_routingtable_v2
    * nhncloud_networking_routingtable_attach_gateway_v2    
    * nhncloud_networking_secgroup_v2
    * nhncloud_networking_secgroup_rule_v2
    * nhncloud_keymanager_secret_v1
    * nhncloud_keymanager_container_v1
* Block Storage
    * nhncloud_blockstorage_volume_v2
* Object Storage
    * nhncloud_objectstorage_container_v1
    * nhncloud_objectstorage_object_v1

#### Data sourcesサポート

* nhncloud_images_image_v2
* nhncloud_blockstorage_volume_v2
* nhncloud_compute_flavor_v2
* nhncloud_compute_keypair_v2
* nhncloud_blockstorage_snapshot_v2
* nhncloud_networking_vpc_v2
* nhncloud_networking_vpcsubnet_v2
* nhncloud_networking_routingtable_v2
* nhncloud_networking_secgroup_v2
* nhncloud_keymanager_secret_v1
* nhncloud_keymanager_container_v1


### 注意

* **下記例のすべてのデータは実際の情報ではありません。必ず正確な情報に修正して使用します。**
* **下記の例はすべてTerraform 0.12.24を利用しました。**


## Terraformインストール
[Terraformダウンロードページ](https://www.terraform.io/downloads.html)でローカルPCのOSに合ったファイルをダウンロードします。ファイルの圧縮を解凍し、任意の場所に入れた後、次の環境設定に該当パスを追加するとインストールが完了します。

次はインストール例です。

```
$ wget https://releases.hashicorp.com/terraform/0.12.24/terraform_0.12.24_linux_amd64.zip
$ unzip terraform_0.12.24_linux_amd64.zip
$ export PATH="${PATH}:$(pwd)"
$ terraform -v
Terraform v0.12.24
```

## Terraform NHN Cloud provider提供

Terraform NHN Cloud providerは次のような**OS/アーキテクチャ**の互換性を提供し、リンクからバイナリファイルをダウンロードできます。
現在提供するTerraform NHN Cloud providerのバージョンは**1.0.2**です。

* macOS / AMD64
  * [1.0.0](https://static.toastoven.net/prod_cloud_terraform_provider/darwin_amd64/terraform-provider-nhncloud_v1.0.0)
  * [1.0.1](https://static.toastoven.net/prod_cloud_terraform_provider/darwin_amd64/terraform-provider-nhncloud_v1.0.1)
  * [1.0.2](https://static.toastoven.net/prod_cloud_terraform_provider/darwin_amd64/terraform-provider-nhncloud_v1.0.2)
* macOS / Apple silicon
  * [1.0.0](https://static.toastoven.net/prod_cloud_terraform_provider/darwin_arm64/terraform-provider-nhncloud_v1.0.0)
  * [1.0.1](https://static.toastoven.net/prod_cloud_terraform_provider/darwin_arm64/terraform-provider-nhncloud_v1.0.1)
  * [1.0.2](https://static.toastoven.net/prod_cloud_terraform_provider/darwin_arm64/terraform-provider-nhncloud_v1.0.2)
* Linux / AMD64
  * [1.0.0](https://static.toastoven.net/prod_cloud_terraform_provider/linux_amd64/terraform-provider-nhncloud_v1.0.0)
  * [1.0.1](https://static.toastoven.net/prod_cloud_terraform_provider/linux_amd64/terraform-provider-nhncloud_v1.0.1)
  * [1.0.2](https://static.toastoven.net/prod_cloud_terraform_provider/linux_amd64/terraform-provider-nhncloud_v1.0.2)
* Windows / AMD64
  * [1.0.0](https://static.toastoven.net/prod_cloud_terraform_provider/windows_amd64/terraform-provider-nhncloud_v1.0.0)
  * [1.0.1](https://static.toastoven.net/prod_cloud_terraform_provider/windows_amd64/terraform-provider-nhncloud_v1.0.1)
  * [1.0.2](https://static.toastoven.net/prod_cloud_terraform_provider/windows_amd64/terraform-provider-nhncloud_v1.0.2)


### Local provider設定

Local provider設定を通じてTerraform NHN Cloud providerを使用できます。

Local providerを探すためのディレクトリ構造を作成した後、ダウンロードしたバイナリファイルをプラグインのパスに追加します。バイナリファイルには実行権限が必要です。

以下はOSごとのプラグイン基本パスです。より詳しい基本パスの説明は[Terraformサイト](https://developer.hashicorp.com/terraform/cli/config/config-file#provider-installation)の`Implied Local Mirror Directories
`項目を参照してください。

* **Linux / macOS** : `${HOME}/.terraform.d/plugins/terraform.local/local/nhncloud/${version}/${platforms}`
* **Windows** : `%APPDATA%/terraform.d/plugins/terraform.local/local/nhncloud/${version}/${platforms}`

プラグイン基本パス構成ルールについての説明です。

* **version**
    * providerのバージョンです。
* **platforms**
    * パッケージがあるプラットフォームを説明するオブジェクトの配列で、OS識別キーワードとCPUアーキテクチャ識別キーワードで構成されています。
    * **darwin_adm64** : macOS / AMD64
    * **darwin_arm64** : macOS / Apple silicon
    * **linux_amd64** : Linux / AMD64
    * **windows_amd64** : Windows / AMD64

以下は、バイナリダウンロード後、**OS/アーキテクチャ**ごとのプラグイン設定例です。 

**プラグインを設定する際は1.0.2バージョンを使用することを推奨します。**

`macOS / AMD64`プラグインの設定例です。

```
$ mkdir -p $HOME/.terraform.d/plugins/terraform.local/local/nhncloud/1.0.2/darwin_amd64
$ cp terraform-provider-nhncloud_v1.0.2 $HOME/.terraform.d/plugins/terraform.local/local/nhncloud/1.0.2/darwin_amd64
$ chmod +x $HOME/.terraform.d/plugins/terraform.local/local/nhncloud/1.0.2/darwin_amd64/terraform-provider-nhncloud_v1.0.2
```

`macOS / Apple silicon`プラグインの設定例です。

```
$ mkdir -p $HOME/.terraform.d/plugins/terraform.local/local/nhncloud/1.0.2/darwin_arm64
$ cp terraform-provider-nhncloud_v1.0.2 $HOME/.terraform.d/plugins/terraform.local/local/nhncloud/1.0.2/darwin_arm64
$ chmod +x $HOME/.terraform.d/plugins/terraform.local/local/nhncloud/1.0.2/darwin_arm64/terraform-provider-nhncloud_v1.0.2
```

`Linux / AMD64`プラグインの設定例です。

```
$ mkdir -p $HOME/.terraform.d/plugins/terraform.local/local/nhncloud/1.0.2/linux_amd64
$ cp terraform-provider-nhncloud_v1.0.2 $HOME/.terraform.d/plugins/terraform.local/local/nhncloud/1.0.2/linux_amd64
$ chmod +x $HOME/.terraform.d/plugins/terraform.local/local/nhncloud/1.0.2/linux_amd64/terraform-provider-nhncloud_v1.0.2
```

`Windows / AMD64`プラグインの設定例です。

```
$ mkdir -p %APPDATA%/terraform.d/plugins/terraform.local/local/nhncloud/1.0.2/windows_amd64
$ cp terraform-provider-nhncloud_v1.0.2 $HOME/.terraform.d/plugins/terraform.local/local/nhncloud/1.0.2/windows_amd64
$ copy terraform-provider-nhncloud_v1.0.2 %APPDATA%/terraform.d/plugins/terraform.local/local/nhncloud/1.0.2/windows_amd64
```


## Terraformの初期化
Terraformを使用する前に、次のようにプロバイダー設定ファイルを作成します。

プロバイダーファイルの名前は任意で設定可能で、この例では`provider.tf`を使用します。

```
# Define required providers
terraform {
required_version = ">= 1.0.0"
  required_providers {
    nhncloud = {
      source  = "terraform.local/local/nhncloud"
      version = "1.0.2"
    }
  }
}

# Configure the nhncloud Provider
provider "nhncloud" {
  user_name   = "terraform-guide@nhncloud.com"
  tenant_id   = "aaa4c0a12fd84edeb68965d320d17129"
  password    = "difficultpassword"
  auth_url    = "https://api-identity-infrastructure.nhncloudservice.com/v2.0"
  region      = "KR1"
}
```

* **user_name**
    * NHN Cloud IDを使用します。
* **tenant_id**
    * NHN Cloudコンソールの**Compute > Instance > 管理**メニューで**APIエンドポイント設定**ボタンを押してテナントIDを確認します。
* **password**
    * **API Endpoint設定**ウィンドウで保存した**APIパスワード**を使用します。
    * APIパスワードの設定方法は**ユーザーガイド > Compute > Instance > API使用準備**を参照します。
* **auth_url**
    * NHN Cloud身元サービスアドレスを明示します。
    * NHN Cloudコンソールの**Compute > Instance > 管理**メニューで**APIエンドポイント設定**ボタンをクリックして身元サービス(identity) URLを確認します。
* **region**
    * NHN Cloudリソースを管理するリージョン情報を入力します。
    * **KR1**：韓国(パンギョ)リージョン
    * **KR2**：韓国(ピョンチョン)リージョン
    * **JP1**：日本(東京)リージョン

プロバイダー設定ファイルがあるパスで`init`コマンドを利用してTerraformを初期化します。

```
$ ls
provider.tf
$ terraform init
```

### Local providerの交換

新しいバージョンのlocal providerがリリースされた場合、変更するバージョンに[local provider設定](#local-provider)を行った後、`init`コマンドの`--upgrade`オプションでプラグインをアップグレードできます。

```
$ terraform init --upgrade
Initializing the backend...
Initializing provider plugins...
- Finding terraform.local/local/nhncloud versions matching "1.0.2"...
- Installing terraform.local/local/nhncloud v1.0.2...
- Installed terraform.local/local/nhncloud v1.0.2 (unauthenticated)
Terraform has made some changes to the provider dependency selections recorded
in the .terraform.lock.hcl file. Review those changes and commit them to your
version control system if they represent changes you intended to make.
```


## Terraform基本使用方法

Terraformを利用したインフラ構築は、通常下記のようなライフサイクルになります。

1. tfファイル作成
2. 構築計画を確認
3. リソース作成
4. リソース修正
5. リソース削除

まず構築したいインフラの形状をtfファイルに作成します。作成されたtfファイルによる構築計画は、下記のように`plan`コマンドで確認します。

```
$ terraform plan
```

構築計画が問題なければ`apply`コマンドを利用してリソースを作成、修正、削除します。

```
$ terraform apply
```

次のセッションでは、この段階を例を用いて詳しく説明します。

### tfファイル作成

プロバイダー設定ファイルがあるパスにtfファイルを作成します。複数のリソース設定を1つのtfファイルに集めるか、リソースごとに別々のtfファイルでも作成可能です。Terraformは作成された全体tfファイルを一度に読み込んで構築計画を立てます。

下記は`instance.tf`ファイルにインスタンスを作成するリソースを定義したtfファイルの例です。

```
$ ls
instance.tf provider.tf
$ cat instance.tf
resource "nhncloud_compute_instance_v2" "terraform-instance-01" {
  name      = "terraform-instance-01"
  region    = "KR1"
  flavor_id = "da74152c-0167-4ce9-b391-8a88a8ff2754"
  key_pair  = "terraform-keypair"
  network {
    uuid = "00d5b852-cb77-4307-b6be-d81dad24eec1"
  }
  security_groups = ["default"]
  block_device {
    uuid = "6d0993b4-cd6d-4242-b59b-94258f265331"
    source_type = "image"
    destination_type = "volume"
    boot_index = 0
    volume_size = 20
    delete_on_termination = true
  }
}
```


### 構築計画の確認

tfファイルを通して変更されるリソースを`plan`コマンドで確認できます。`plan`コマンドを実行すると、Terraformが.tfファイルをロードして設定が正しいかを確認し、DBと比較してプランを作成します。プラン作成が完了すると、プランをタイプごとに集計して出力します。

```
$ terraform plan
```

作成されたプランが無効な場合、tfファイルを修正し、再度反復して`plan`コマンドを実行します。`plan`コマンドは実際のNHN Cloudリソースを変更しないため、インフラ変更事項を負担なく確認できます。


### リソースを作成する

任意のプランでtfファイルを作成した後、`apply`コマンドでリソースを作成します。

下記は、上で作成した`instance.tf`ファイルを利用して`apply`コマンドを実行した結果の例です。

```
$ terraform apply
...
nhncloud_compute_instance_v2.terraform-instance-01: Creating...
nhncloud_compute_instance_v2.terraform-instance-01: Still creating... [10s elapsed]
nhncloud_compute_instance_v2.terraform-instance-01: Still creating... [20s elapsed]
nhncloud_compute_instance_v2.terraform-instance-01: Still creating... [30s elapsed]
nhncloud_compute_instance_v2.terraform-instance-01: Creation complete after 39s [id=1e846787-04e9-4701-957c-78001b4b7257]

Apply complete! Resources: 1 added, 0 changed, 0 destroyed.
...
```

`apply`コマンドを実行すると、プラン変更履歴を記録するDBファイル(terraform.tfstate)が現在のディレクトリに作成されます。このファイルを削除しないように注意してください。


### リソースを修正する

変更するリソースが定義された`.tf`ファイルを開き、任意の情報を修正し、プランを適用します。変更できる仕様は一部プロパティに制限されます。もし変更できないプロパティを修正した場合、該当リソースは削除後に新たに再び作成されます。

次はインスタンスに`terraform-sg`セキュリティグループをもう1つ追加する例です。上で作成した`instance.tf`ファイルを下記のように修正します。

```
resource "nhncloud_compute_instance_v2" "terraform-instance-01" {
  ...
  security_groups = ["default", "terraform-sg"]
  ...
}
```

構築計画を確認したら、変更されたセキュリティグループ情報を整理して出力します。

```
$ terraform plan
...
Terraform used the selected providers to generate the following execution plan. Resource actions are indicated with the following symbols:
  ~ update in-place

Terraform will perform the following actions:

  # nhncloud_compute_instance_v2.terraform-instance-01 will be updated in-place
  ~ resource "nhncloud_compute_instance_v2" "terraform-instance-01" {
        id                  = "1e846787-04e9-4701-957c-78001b4b7257"
        name                = "terraform-instance-01"
      ~ security_groups     = [
          + "terraform-sg",
            # (1 unchanged element hidden)
        ]
        # (13 unchanged attributes hidden)


        # (2 unchanged blocks hidden)
    }

Plan: 0 to add, 1 to change, 0 to destroy.
```

プランを適用すると、インスタンスに新しいセキュリティグループが追加されます。
```
$ terraform apply
...
nhncloud_compute_instance_v2.terraform-instance-01: Modifying... [id=1e846787-04e9-4701-957c-78001b4b7257]
nhncloud_compute_instance_v2.terraform-instance-01: Modifications complete after 5s [id=1e846787-04e9-4701-957c-78001b4b7257]

Apply complete! Resources: 0 added, 1 changed, 0 destroyed.
```


### リソースを削除する

Terraformで作成したリソースを削除するには、該当の`.tf`ファイルを削除します。

```
$ rm instance.tf
```

構築計画を通して、該当リソースが削除されることを確認できます。

```
$ terraform plan
...
An execution plan has been generated and is shown below.
Resource actions are indicated with the following symbols:
  - destroy

Terraform will perform the following actions:

  # nhncloud_compute_instance_v2.terraform-instance-01 will be destroyed
  - resource "nhncloud_compute_instance_v2" "terraform-instance-01" {
...

Plan: 0 to add, 0 to change, 1 to destroy.
...
```

`apply`コマンドを実行すると、作成されたinstanceが削除されます。

```
$ terraform apply
...
nhncloud_compute_instance_v2.terraform-instance-01: Destroying... [id=1e846787-04e9-4701-957c-78001b4b7257]
nhncloud_compute_instance_v2.terraform-instance-01: Still destroying... [id=1e846787-04e9-4701-957c-78001b4b7257, 10s elapsed]
nhncloud_compute_instance_v2.terraform-instance-01: Destruction complete after 11s

Apply complete! Resources: 0 added, 0 changed, 1 destroyed.
...
```


## Data sources

tfファイルの作成に必要なインスタンスタイプID、イメージIDなどは、コンソールを通して確認するか、Terraformが提供するdata sourcesを利用して取得できます。Data sourcesはtfファイル内に作成し、取得した情報は修正できません。参照のみ可能です。

Data sourcesは、`{data sourcesリソースタイプ}.{data source名前}`で参照します。下記の例では`nhncloud_images_image_v2.ubuntu_1804_20200218`で取得したイメージ情報を参照します。

```
data "nhncloud_images_image_v2" "ubuntu_2004_20201222" {
  name = "Ubuntu Server 18.04.3 LTS (2020.02.18)"
  most_recent = true
}
```

Data sources内で別のdata sourceを参照できます。

```
data "nhncloud_blockstorage_volume_v2" "volume_00"{
  name = "ssd_volume1"
  status = "available"
}

data "nhncloud_blockstorage_snapshot_v2" "my_snapshot" {
  name = "my-snapshot"
  volume_id = data.nhncloud_blockstorage_volume_v2.volume_00.id
  status = "available"
  most_recent = true
}
```

次のセッションでは、NHN Cloudが提供する各種リソースをdata sources機能で取得する方法を説明します。


### イメージ

イメージ情報を取得します。NHN Cloudが提供するパブリックイメージまたは個人イメージをサポートします。

```
data "nhncloud_images_image_v2" "ubuntu_1804_20200218" {
  name = "Ubuntu Server 18.04.3 LTS (2020.02.18)"
  most_recent = true
}

# 同じ名前のイメージの中から、最も古いイメージを照会
data "nhncloud_images_image_v2" "windows2016_20200218" {
  name = "Windows 2016 STD with MS-SQL 2016 Standard (2020.02.18) KO"
  sort_key = "created_at"
  sort_direction = "asc"
  owner = "c289b99209ca4e189095cdecebbd092d"
  tag = "_AVAILABLE_"
}
```
| 名前 | 形式 | 必須 | 説明 |
| ------ | ---- | ---- | --------- |
| name | String | - | 照会するイメージ名<br>イメージ名は、**NHN CloudコンソールCompute > Instanceでインスタンス作成ボタン**を押すとNHN Cloudが提供するイメージリストで確認します。<br>イメージ名はNHN Cloudコンソールに表示された**<イメージ説明>**で作成する必要があります。<br>もし言語項目が存在する場合、上の例のように**「<イメージ説明> <言語>」**形式で入力する必要があります。 |
| size_min | Integer | - | 照会するイメージの最小サイズ(byte) |
| size_max | Integer | - | 照会するイメージの最大サイズ(byte) |
| properties | Object | - | 照会するイメージのプロパティ<br>すべてのプロパティ値が一致するイメージを照会 |
| sort_key | String | - | 特定プロパティを基準に照会したイメージリストをソート<br>デフォルト値は`name` |
| sort_direction | String | - | 照会したイメージリストのソート方向 <br>`asc`：昇順(デフォルト値) <br>`desc`：降順 |
| owner | String | - | 照会するイメージが属しているテナントID |
| tag | String | - | 特定タグが存在するイメージを検索 |
| visibility | String | - | 照会するイメージの表示プロパティ<br>public、private、sharedのうち、いずれか1つの値のみ選択可能<br>省略するとすべての種類のイメージリストを返す |
| most_recent | Boolean | - | `true`：照会したイメージリストのうち、最近作られたイメージ選択<br>`false`：照会された順序でイメージ選択 |
| member_status | String | - | 照会するイメージメンバーの状態 <br>`accepted`、`pending`、`rejected`、`all`のうちいずれか1つ。|


### ブロックストレージ

```
data "nhncloud_blockstorage_volume_v2" "volume_00" {
  name = "ssd_volume1"
  status = "available"
}
```

| 名前 | 形式 | 必須 | 説明 |
| ------ | ---- | ---- | --------- |
| name | String | - | 照会するブロックストレージ名 |
| status | String | - | 照会するブロックストレージの状態 |
| metadata | Object | - | 照会するブロックストレージと関連するメタデータ |


### インスタンスタイプ

インスタンスタイプ名は**NHN CloudコンソールCompute > Instanceでインスタンス作成 > インスタンスタイプ選択ボタン**を押すと確認できます。

```
data "nhncloud_compute_flavor_v2" "m2c2m4"{
  name = "m2.c2m4"
}
```

| 名前 | 形式 | 必須 | 説明 |
| ------ | ---- | ---- | --------- |
| name | String | - | 照会するインスタンスタイプ名 |


### キーペア

```
data "nhncloud_compute_keypair_v2" "my_keypair"{
  name = "my_keypair"
}
```

| 名前  | 形式 | 必須 | 説明       |
| ------ | ---- |----|------------|
| name | String | O  | 照会するキーペア名 |

### スナップショット

```
data "nhncloud_blockstorage_snapshot_v2" "my_snapshot" {
  name = "my-snapshot"
  volume_id = data.nhncloud_blockstorage_volume_v2.volume_00.id
  status = "available"
  most_recent = true
}
```

| 名前 | 形式 | 必須 | 説明 |
| ------ | ---- | ---- | --------- |
| name | String | - | 照会するスナップショットの名前 |
| volume_id | String | - | 照会するスナップショットの原本ブロックストレージID |
| status | String | - | 照会するスナップショットの状態 |
| most_recent | Boolean | - | `true`：照会したスナップショットリストのうち、最近作られたスナップショットを選択<br>`false`：照会された順序でスナップショットを選択 |


### VPC

VPCネットワークのUUIDは、**NHN CloudコンソールNetwork > VPC**でVPCを選択して確認可能です。

```
data "nhncloud_networking_vpc_v2" "default_network" {
  region = "KR1"
  tenant_id = "ba3be1254ab141bcaef674e74630a31f"
  id = "e34fc878-89f6-4d17-a039-3830a0b78346"
  name = "Default Network"
}
```

| 名前 | タイプ | 必須 | 説明       |
| --- | --- |---|------------|
| region | String | - | 照会するVPCが属するリージョン名 |
| tenant\_id | String | - | 照会するVPCが属するテナントID |
| id | String | - | 照会するVPCのID |
| name | String | - | 照会するVPCの名前 |


### VPCサブネット

サブネットIDはNHN Cloudコンソール **Network > サブネット**でサブネットを選択して確認可能です。

```
data "nhncloud_networking_vpcsubnet_v2" "default_subnet" {
  region = "KR1"
  tenant_id = "ba3be1254ab141bcaef674e74630a31f"
  id = "05f6fdc3-641f-48df-b986-773b6489654f"
  name = "Default Network"
  shared = true
}
```

| 名前 | タイプ | 必須 | 説明       |
| --- | --- |---|------------|
| region | String | - | 照会するサブネットが属するリージョン名 |
| tenant\_id | String | - | 照会するサブネットが属するテナントID |
| id | String | - | 照会するサブネットID |
| name | String | - | 照会するサブネットの名前 |
| shared | Bool | - | 照会するサブネットの共有有無 |

### ルーティングテーブル
```
data "nhncloud_networking_routingtable_v2" "default_rt" {
  id = "bf15f6f6-1339-4057-a7fe-5811d39bab18"
}
```

| 名前 | タイプ | 必須 | 説明                 |
| --- | --- |---|---------------------|
| tenant\_id | String | - | 照会するルーティングテーブルが属するテナント ID
| id | String | - | 照会するルーティングテーブルID      |
| name | String | - | 照会するルーティングテーブルの名前


### セキュリティグループ
```
data "nhncloud_networking_secgroup_v2" "default_sg" {
  name = "default"
}
```

| 名前 | タイプ | 必須 | 説明                |
| --- | --- |---|--------------------|
| region | String | - | 照会するセキュリティグループが属するリージョン名 |
| tenant\_id | String | - | 照会するセキュリティグループが属するテナントID |
| name | String | - | 照会するセキュリティグループの名前      |


### シークレット
```
data "nhncloud_keymanager_secret_v1" "secret_01" {
  name      = "terraform_secret_01"
}
```

| 名前 | タイプ | 必須 | 説明              |
| --- | --- |---|------------------|
| region | String | - | 照会するシークレットが属するリージョン名 |
| name | String | - | 照会するシークレットの名前      |


### シークレットコンテナ
```
data "nhncloud_keymanager_container_v1" "container_01" {
  name      = "terraform_container_01"
}
```

| 名前 | タイプ | 必須 | 説明                     |
| --- | --- |---|-------------------------|
| region | String | - | 照会するシークレットコンテナが属するリージョン名 |
| name | String | - | 照会するシークレットコンテナ名        |


## Resources

Terraform resourcesでリソースを作成、修正、削除できます。NHN Cloudでは、Terraformによる次のリソース管理をサポートします。

* インスタンス
* ブロックストレージ
* オブジェクトストレージ
VPC
* Floating IP
* ネットワークポート
* ロードバランサー
* セキュリティグループ

次のセッションでは各リソースを使用する方法を説明します。
### 特記事項

* オブジェクトストレージリソースの使い方は、[ユーザーガイド > Storage > Object Storage > サードパーティツール使用ガイド](https://docs.nhncloud.com/ja/Storage/Object%20Storage/ja/third-party-tools-guide/)を参照してください。


## Resources - インスタンス

### インスタンス作成

```

# ネットワーク追加、ブロックストレージ追加されたインスタンス作成
resource "nhncloud_compute_instance_v2" "tf_instance_02" {
  name      = "tf_instance_02"
  region    = "KR1"
  key_pair  = "terraform-keypair"
  flavor_id = data.nhncloud_compute_flavor_v2.m2c1m2.id
  security_groups = ["default","web"]

  network {
    name = data.nhncloud_networking_vpc_v2.default_network.name
    uuid = data.nhncloud_networking_vpc_v2.default_network.id
  }

  network {
    port = nhncloud_networking_port_v2.port_1.id
  }

  block_device {
    uuid                  = data.nhncloud_images_image_v2.ubuntu_2004_20201222.id
    source_type           = "image"
    destination_type      = "volume"
    boot_index            = 0
    volume_size           = 20
    delete_on_termination = true
  }

  block_device {
    source_type           = "blank"
    destination_type      = "volume"
    boot_index            = 1
    volume_size           = 20
    delete_on_termination = true
  }
}
```
| 名前                                         | 形式     | 必須 | 説明                                                                                                                                                                                          |
|---------------------------------------------|---------|----|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| name                                        | String  | O  | 作成するインスタンスの名前                                                                                                                                                                                |
| region                                      | String  | -  | 作成するインスタンスのリージョン<br>デフォルト値はprovider.tfに設定されたリージョン                                                                                                                                                    |
| flavor_name                                 | String  | -  | 作成するインスタンスのインスタンスタイプ名<br>flavor_idが空の時は必須                                                                                                                                               |
| flavor_id                                   | String  | -  | 作成するインスタンスのインスタンスタイプID<br>flavor_nameが空の時は必須                                                                                                                                             |
| key_pair                                    | String  | -  | インスタンス接続に使用するキーペア名<br>キーペアはNHN Cloudコンソールの**Compute > Instance > Key Pair**メニューで新たに作成するか、<br>すでに持っているキーペアを登録して使用。<br>作成、登録方法は`ユーザーガイド > Compute > Instance > コンソール使用ガイド`を参照。           |
| availability_zone                           | String  | -  | インスタンスを作成するアベイラビリティゾーン<br>指定しない場合、任意のゾーンが選択される<br>ルートブロックストレージのソースタイプが`volume`, `snapshot`の場合、元のブロックストレージのアベイラビリティゾーンと同じに設定する必要があります。同じに設定する必要があります。 |
| network                                     | Object  | -  | 作成するインスタンスに接続するVPCネットワーク情報。<br>コンソールの**Network > VPC > Management**メニューで接続するVPCを選択すると、下部の詳細情報画面でネットワーク名とuuidを確認可。                                                                      |
| network.name                                | String  | -  | VPCネットワーク名。 <br>network.name、network.uuid、network.portのうち、いずれか1つを明示                                                                                                                       |
| network.uuid                                | String  | -  | VPCネットワークID                                                                                                                                                                                  |
| network.port                                | String  | -  | VPCVPCネットワークに接続するポートのID<br>ポートIDを指定する際、要求したセキュリティグループは指定した既存のポートには適用されない。                                                                                                                                                                         |
| security_groups                             | Array   | -  | インスタンスで使用するセキュリティグループの名前リスト <br>コンソールの**Network > VPC > Security Groups**メニューで使用するセキュリティグループを選択すると、下部の詳細情報画面で情報を確認可                                                                            |
| user_data                                   | String  | -  | インスタンスの起動後に実行するスクリプトと設定<br>base64でエンコードされた文字列で65535バイトまで許容。<br>                                                                                                                              |
| block_device                                | Object  | O  | インスタンスのブロックストレージ情報オブジェクト |
| block_device.source_type                    | String  | O  | 作成するブロックストレージ原本のタイプ<br>- `image`:イメージを利用してブロックストレージ作成<br>- `blank`:空のブロックストレージ作成(ルートブロックストレージとして使用できない)<br>- `volume`:既存のブロックストレージを使用<br>- `snapshot`:スナップショットを利用してブロックストレージ作成 |
| block_device.uuid                           | String  | -  | ブロックストレージのソースタイプによって異なる設定が必要<br>- ソースタイプが`image`の場合、イメージIDを設定<br>- ソースタイプが`volume`の場合、既存のブロックストレージIDを設定<br>- ソースタイプが`snapshot`の場合、スナップショットIDを設定<br>- ソースタイプが`blank`の場合、設定不要<br>ルートブロックストレージの場合、必ず起動可能な原本である必要があります。 |
| block_device.boot_index                     | Integer | O  | 指定したブロックストレージの起動順序<br>- `0`の場合はルートブロックストレージ<br>- それ以外は追加ブロックストレージ<br>サイズが大きいほど起動順序は低くなる<br>                                                                                                            |
| block_device.destination_type               | String  | O  | インスタンスブロックストレージの位置                                                                                |
| block_device.volume_size                    | Integer | -  | 作成するブロックストレージサイズ<br>ブロックストレージのソースタイプによって異なる設定が必要<br>- ソースタイプが`volume`の場合、設定不要<br>- ソースタイプが`snapshot`の場合、原本ブロックストレージサイズ以上に設定<br>`GB`単位<br>インスタンスタイプによって作成できるルートブロックストレージのサイズが異なるため、詳細は`ユーザーガイド > Compute > Instance > コンソール使用ガイド > インスタンス作成 > ブロックストレージサイズ`を参照 |
| block_device.volume_type               | Enum    | -  | 作成するブロックストレージのタイプ<br>ブロックストレージのソースタイプが`volume`、`snapshot`の場合は設定不要<br>`ユーザーガイド > Storage > Block Storage > API v2ガイド`で**ブロックストレージタイプリスト表示**レスポンスの`name`を参照 |
| block_device.delete_on_termination          | Boolean | -  | `true`：インスタンス削除時にブロックデバイスも一緒に削除<br>`false`：インスタンス削除時にブロックデバイスは一緒に削除しない                                                                                                                   |
| block_device.nhn_encryption                 | Object  | -  | ブロックストレージ暗号化情報                                                                                                                                                                              |
| block_device.nhn_encryption.skm_appkey      | String  | O  | Secure Key Manager商品のアプリケーションキー                                                                                                                                                                   |
| block_device.nhn_encryption.skm_key_id      | String  | O  | Secure Key ManagerのキーID                                                                                                                                                                     |


### ブロックストレージ接続
```
# インスタンス作成
resource "nhncloud_compute_instance_v2" "tf_instance_01" {
  ...
}

# ブロックストレージ作成
resource "nhncloud_blockstorage_volume_v2" "volume_01" {
  ...
}

# ブロックストレージ接続
resource "nhncloud_compute_volume_attach_v2" "volume_to_instance"{
  instance_id = nhncloud_compute_instance_v2.tf_instance_02.id
  volume_id = nhncloud_blockstorage_volume_v2.volume_01.id
}
```
| 名前 | タイプ | 必須 | 説明 |
| ------ | --- |----| --------- |
| instance_id | String | O  | ブロックストレージを接続する対象インスタンス |
| volume_id | String | O  | 接続するブロックストレージUUID |


### キーペア
```
resource "nhncloud_compute_keypair_v2" "tf_kp_01" {
  name = "tf_kp_01"
}
# public_key指定
resource "nhncloud_compute_keypair_v2" "tf_kp_02" {
  name = "tf_kp_02"
  public_key = "ssh-rsa ... Generated-by-Nova"
}
```

| 名前      | タイプ | 必須 | 説明                           |
|-----------| --- |----|--------------------------------|
| name      | String | O  | 作成するキーペア名                   |
| public_key | String | -  | 登録する公開鍵<br>省略すると、新しい公開鍵を作成 |

> [注意]
> Terraformを通じてキーペアを作成する場合、秘密鍵は状態ファイル(terraform.tfstate)に**暗号化されていない状態**で保存されます。

## Resources - ブロックストレージ

### ブロックストレージ作成
```
# HDDタイプの空ブロックストレージ作成
resource "nhncloud_blockstorage_volume_v2" "volume_01" {
  name = "tf_volume_01"
  size = 10
  availability_zone = "kr-pub-a"
  volume_type = "General HDD"
}

# SSDタイプの空ブロックストレージ作成
resource "nhncloud_blockstorage_volume_v2" "volume_02" {
  name = "tf_volume_02"
  size = 10
  availability_zone = "kr-pub-b"
  volume_type = "General SSD"
}

# スナップショットでブロックストレージ作成
resource "nhncloud_blockstorage_volume_v2" "volume_03" {
  name = "tf_volume_03"
  description = "terraform create volume with snapshot test"
  snapshot_id = data.nhncloud_blockstorage_snapshot_v2.snapshot_01.id
  size = 30
}
```

| 名前               | タイプ     | 必須 | 説明                                                                                                                                                      |
|-------------------|---------|---|----------------------------------------------------------------------------------------------------------------------------------------------------------|
| name              | String  | - | 作成するブロックストレージ名                                                                                                                                          |
| description       | String  | - | ブロックストレージの説明                                                                                                                                              |
| size              | Integer | O | 作成するブロックストレージのサイズ(GB)                                                                                                                                       |
| availability_zone | String  | - | 作成するブロックストレージのアベイラビリティゾーン。値が存在しない場合、任意のアベイラビリティゾーン<br>availability_zoneはコンソール`Storage > Block Storage > 管理`のブロックストレージ作成ボタンを押して表示されるアベイラビリティゾーンで確認できます。 |
| volume_type       | Enum    | -  | ブロックストレージタイプ<br>`ユーザーガイド > Storage > Block Storage > API v2ガイド`の**ブロックストレージタイプリスト表示** レスポンスの`name`参考                                                      |
| snapshot_id       | String  | - | 原本スナップショットID、省略すると空のブロックストレージが作成される                                                                                                                          |
| nhn_encryption                 | Object  | -  | ブロックストレージ暗号化情報                                                                                                                                          |
| nhn_encryption.skm_appkey      | String  | O  | Secure Key Manager商品のアプリケーションキー                                                                                                                                     |
| nhn_encryption.skm_key_id      | String  | O  | Secure Key ManagerのキーID                                                                                                                                       |


### ブロックストレージのインポート

コンソールまたはAPIで作成したブロックストレージを、Terraformでインポートして管理できます。

`.tf`ファイルにインポートするブロックストレージ情報を作成します。
```
resource "nhncloud_blockstorage_volume_v2" "volume_06" {
  name = "volume_06"
  size = 10
}
``` 

`terraform import nhncloud_blockstorage_volume_v2.{name} {bloack storage id}`コマンドでブロックストレージをインポートします。

```
$ terraform import nhncloud_blockstorage_volume_v2.volume_06 10cf5bec-cebb-479b-8408-3ffe3b569a7a
...
nhncloud_blockstorage_volume_v2.volume_06: Importing from ID "10cf5bec-cebb-479b-8408-3ffe3b569a7a"...
nhncloud_blockstorage_volume_v2.volume_06: Import prepared!
  Prepared nhncloud_blockstorage_volume_v2 for import
nhncloud_blockstorage_volume_v2.volume_06: Refreshing state... [id=10cf5bec-cebb-479b-8408-3ffe3b569a7a]

Import successful!
...
```


## Resources - VPC

NHN CloudはTerraformを通して、下記のリソースの作成をサポートします。

* VPC
* VPCサブネット
* ネットワークポート
* Floating IP
* ルーティングテーブル

これ以外のVPCリソースは、コンソールで作成する必要があります。


### VPC作成

指定したIP帯域のVPCを作成します。

```
resource "nhncloud_networking_vpc_v2" "resource-vpc-01" {
  name = "tf-vpc-01"
  cidrv4 = "10.0.0.0/8"
}
```

| 名前 | タイプ | 必須 | 説明   |
| --- | --- |---|--------|
| name | String | O | VPCの名前 |
| cidrv4 | String | O | VPC IP帯域 |
| region | String | - | VPCのリージョン名 |
| tenant\_id | String | - | VPCのtenant ID |


### VPCサブネット作成およびルーティングテーブル接続

指定したVPCにユーザーが指定したIP帯域でサブネットを作成し、作成したサブネットに既存のルーティングテーブルを接続します。
ルーティングテーブルはNHN Cloudコンソールで作成できます。

```
resource "nhncloud_networking_vpcsubnet_v2" "resource-vpcsubnet-01" {
  name      = "tf-vpcsubnet-01"
  vpc_id    = "def56b5e-0f1d-4a31-8005-4d716127f177"
  cidr      = "10.10.10.0/24"
  routingtable_id = "c3ed678d-de8b-4bf7-abea-b7c1118f0828"
}
```

| 名前 | タイプ | 必須 | 説明       |
| --- | --- |---|------------|
| vpc\_id | String | O | サブネットが割り当てられるVPC ID |
| cidr | String | O | サブネットのIP帯域 |
| name | String | O | サブネットの名前  |
| region | String | - | サブネットが割り当てられるリージョン名 |
| tenant\_id | String | - | サブネットが割り当てられるテナントID |
| routingtable\_id | String | - | ルーティングテーブルID |


### ネットワークポート作成

```
resource "nhncloud_networking_port_v2" "port_1" {
  name = "tf_port_1"
  network_id = data.nhncloud_networking_vpc_v2.default_network.id
  admin_state_up = "true"
}
```

| 名前   | 形式 | 必須 | 説明      |
| ------ | ---- |---| --------- |
| name | String | O | 作成するポートの名前 |
| description | String | - | ポートの説明 |
| network_id | String | O | ポートを作成するVPCネットワークID |
| tenant_id | String | - | 作成するポートのテナントID |
| device_id | String | - | 作成されたポートが接続されるデバイスID |
| fixed_ip | Object | - | 作成するポートの固定IP設定情報<br>`no_fixed_ip`プロパティがあってはならない |
| fixed_ip.subent_id | String | O | 固定IPのサブネットID |
| fixed_ip.ip_address | String | - | 設定する固定IPのアドレス |
| no_fixed_ip | Boolean | - | `true`:固定IPがないポート<br>`fixed_ip`プロパティがあってはならない |
| admin_state_up | Boolean | - | 管理者制御状態<br> `true`: 作動<br>`false`:停止 |


### Floating IP作成

```
resource "nhncloud_compute_floatingip_v2" "fip_01" {
  pool = "Public Network"
}
```

| 名前 | 形式 | 必須 | 説明 |
| ------ | --- |---- | --------- |
| pool | String | O | Floating IPを作成するIPプール<br>デフォルト値は`Public Network` |


### Floating IP接続
```
# ネットワークポートの作成
resource "nhncloud_networking_port_v2" "port_1" {
  ...
}

# インスタンス作成
resource "nhncloud_compute_instance_v2" "tf_instance_01" {
...
    network {
    port = nhncloud_networking_port_v2.port_1.id
  }
  ...
}

# Floating IP作成
resource "nhncloud_compute_floatingip_v2" "fip_01" {
  ...
}

# Floating IP接続
resource "nhncloud_compute_floatingip_associate_v2" "fip_associate" {
  floating_ip = nhncloud_compute_floatingip_v2.fip_01.address
  port_id = nhncloud_networking_port_v2.port_1.id
}

```

| 名前         | 形式 | 必須 | 説明                   |
|-------------| --- |---- |-------------------------|
| floating_ip | String | O | 接続するFloating IP           |
| port_id     | String | O | Floating IPを接続するポートUUID |


### ルーティングテーブルの作成
```
resource "nhncloud_networking_vpc_v2" "resource-vpc-01" {
  ...
}
resource "nhncloud_networking_routingtable_v2" "resource-rt-01" {
  name = "resource-rt-01"
  vpc_id = nhncloud_networking_vpc_v2.resource-vpc-01.id
  distributed = false
}
```

| 名前    | タイプ     | 必須 | 説明                                                            |
|--------|---------|----|----------------------------------------------------------------|
| name   | String  | O  | ルーティングテーブルの名前                                                    |
| vpc_id | String  | O  | ルーティングテーブルが属するVPC ID                                             |
| distributed   | Boolean | -  | ルーティングテーブルのルーティング方式 </br>`true`:分散型、`false`:中央集中型(デフォルト値: `true`) |

### ルーティングテーブルにインターネットゲートウェイを接続する

ルーティングテーブルにインターネットゲートウェイを接続します。
インターネットゲートウェイはNHN Cloudコンソールで作成できます。インターネットゲートウェイを作成する方法は[ユーザーガイド](https://docs.nhncloud.com/ko/Network/Internet%20Gateway/ko/console-guide/#_2)を参照してください。

```
resource "nhncloud_networking_routingtable_v2" "resource-rt-01" {
  ...
}
resource "nhncloud_networking_routingtable_attach_gateway_v2" "attach-gw-01" {
  routingtable_id = nhncloud_networking_routingtable_v2.resource-rt-01.id
  gateway_id = "5c7c578a-d199-4672-95d0-1980f996643f"
}
```

| 名前    | タイプ     | 必須 | 説明                                                                                                                     |
|--------|---------|----|-------------------------------------------------------------------------------------------------------------------------|
| routingtable_id   | String  | O  | 修正するルーティングテーブルID                                                                                                          |
| gateway_id | String  | O  | ルーティングテーブルに接続するインターネットゲートウェイのID<br>コンソールの**Network > Internet Gateway**メニューで使用するインターネットゲートウェイを選択すると、下部の詳細情報画面でゲートウェイのIDを確認可能 |

## Resources - ロードバランサー
### ロードバランサー作成

```
resource "nhncloud_lb_loadbalancer_v2" "tf_loadbalancer_01"{
  name = "tf_loadbalancer_01"
  description = "create loadbalancer by terraform."
  vip_subnet_id = data.nhncloud_networking_vpcsubnet_v2.default_subnet.id
  vip_address = "192.168.0.10"  
  admin_state_up = true
}
```
| 名前 | 形式 | 必須 | 説明 |
| ------ | ---- | ---- | --------- |
| name | String | - | ロードバランサーの名前 |
| description | String | - | ロードバランサーの説明 |
| tenant_id | String | - | ロードバランサーが作成されるテナントのID |
| vip_subnet_id | String | O | ロードバランサーが使用するサブネットのUUID |
| vip_address | String | - | ロードバランサーのIP指定 |
| security_group_ids | Object | - | ロードバランサーに適用するセキュリティグループIDリスト<br>**セキュリティグループは必ず名前ではなくIDで指定する必要があります。** |
| admin_state_up | Boolean | - | 管理者制御状態 |
| loadbalancer_type | String | - | ロードバランサータイプ<br>`shared`/`dedicated`使用可能<br>省略した場合、`shared`に設定される |

### リスナー作成

```
# HTTPリスナー
resource "nhncloud_lb_listener_v2" "tf_listener_http_01"{
  name = "tf_listener_01"
  description = "create listener by terraform."
  protocol = "HTTP"
  protocol_port = 80
  loadbalancer_id = nhncloud_lb_loadbalancer_v2.tf_loadbalancer_01.id
  default_pool_id = ""
  connection_limit = 2000
  timeout_client_data = 5000
  timeout_member_connect = 5000
  timeout_member_data = 5000
  timeout_tcp_inspect = 5000
  admin_state_up = true
}

# Terminated HTTPSリスナー
resource "nhncloud_lb_listener_v2" "tf_listener_01"{
  name = "tf_listener_01"
  description = "create listener by terraform."
  protocol = "TERMINATED_HTTPS"
  protocol_port = 443
  loadbalancer_id = nhncloud_lb_loadbalancer_v2.tf_loadbalancer_01.id
  default_pool_id = ""
  connection_limit = 2000
  timeout_client_data = 5000
  timeout_member_connect = 5000
  timeout_member_data = 5000
  timeout_tcp_inspect = 5000
  default_tls_container_ref = "https://kr1-api-key-manager-infrastructure.nhncloudservice.com/v1/containers/3258d456-06f4-48c5-8863-acf9facb26de"
  sni_container_refs = null
  admin_state_up = true
}
```
| 名前 | 形式 | 必須 | 説明 |
| ------ | ---- | ---- | --------- |
| name | String | - | 作成するリスナーの名前 |
| description  | String | - | リスナーの説明 |
| protocol | String | O | 作成するリスナーのプロトコル<br>`TCP`、`HTTP,HTTPS`、`TERMINATED_HTTPS`のうちいずれか1つ |
| protocol_port | Integer | O | 作成するリスナーのポート |
| loadbalancer_id | String | O | 作成するリスナーが接続されるロードバランサーのID |
| default_pool_id | String | - | 作成するリスナーに接続される基本プールID |
| connection_limit  | Integer | - | 作成するリスナーに許可される最大接続数 |
| timeout_client_data  | Integer | - | クライアントが非アクティブの時のtimeout設定(ms) |
| timeout_member_connect  | Integer | - | メンバー接続時のtimeout設定(ms) |
| timeout_member_data | Integer | - | メンバーが非アクティブの時のtimeout時間設定(ms) |
| timeout_tcp_inspect | Integer | - | コンテンツチェックのために追加TCPパケットを待つ時間(ms) |
| default_tls_container_ref | String | - | プロトコルが`TERMINATED_HTTPS`の場合は使用するTLS証明書のパス |
| sni_container_refs | Array | - | SNI証明書のパスリスト |
| insert_headers | String | - | バックエンドメンバーにリクエストを転送する前に追加するヘッダリスト |
| admin_state_up | Boolean | - | 管理者制御状態 |
| keepalive_timeout | Integer | - | リスナーのkeepalive timeout |


### プール作成

```
resource "nhncloud_lb_pool_v2" "tf_pool_01"{
  name = "tf_pool_01"
  description = "create pool by terraform."
  protocol = "HTTP"
  listener_id = nhncloud_lb_listener_v2.tf_listener_01.id
  lb_method = "LEAST_CONNECTIONS"
  persistence{
    type = "APP_COOKIE"
    cookie_name = "testCookie"
  }
  admin_state_up = true
}
```
| 名前 | 形式 | 必須 | 説明 |
| ------ | ---- | ---- | --------- |
| name | String | - | ロードバランサーの名前 |
| description | String | - | プールの説明 |
| protocol | String | O | プロトコル<br>`TCP`、`HTTP`、`HTTPS`、`PROXY`のうちいずれか1つ |
| loadbalancer_id | String | -  | 作成するプールが接続されるロードバランサーID<br>ロードバランサーIDかリスナーIDのいずれかを必ず入力     |
| listener_id | String | -  | 作成するプールが接続されるリスナーID<br>ロードバランサーIDかリスナーIDのいずれかを必ず入力            |
| lb_method | String | O | プールのトラフィックをメンバーに分配するロードバランシング方式<br>`ROUND_ROBIN`、`LEAST_CONNECTIONS`、`SOURCE_IP`のいずれか1つ |
| persistence | Object | - | 作成するプールのセッション持続性 |
| persistence.type | String | O | セッション持続性タイプ<br>`SOURCE_IP`、`HTTP_COOKIE`、`APP_COOKIE`のうちいずれか1つ<br>ロードバランシング方式が`SOURCE_IP`の場合は使用不可。<br>プロトコルが`HTTPS`または`TCP`の場合、HTTP_COOKIEとAPP_COOKIEを使用不可。 |
| persistence.cookie_name | String | - | Cookie名<br>persistence.cookie_nameはセッション持続性タイプがAPP_COOKIEの場合にのみ使用可能。 |
| admin_state_up | Boolean | - | 管理者制御状態 |
| member_port | Integer | - | メンバーの受信ポート<br>トラフィックをこのポートに伝達<br>デフォルト値は`-1` |


### ヘルスモニター作成

```
resource "nhncloud_lb_monitor_v2" "tf_monitor_01"{
  name = "tf_monitor_01"
  pool_id = nhncloud_lb_pool_v2.tf_pool_01.id
  type = "HTTP"
  delay = 20
  timeout = 10
  max_retries = 5
  url_path = "/"
  http_method = "GET"
  expected_codes = "200-202"
  admin_state_up = true
}
```
| 名前 | 形式 | 必須 | 説明 |
| ------ | ---- | ---- | --------- |
| name | String | - | 作成するヘルスモニターの名前 |
| pool_id | String | O | 作成するヘルスモニターが接続されるプールID |
| type | String | O | `TCP`、`HTTP`、`HTTPS`のみサポート |
| delay | Integer | O | ヘルスチェック間隔(秒) |
| timeout | Integer | O | ヘルスチェックレスポンス待機時間(秒)<br>timeoutはdelay値より小さくする必要がある。 |
| max_retries | Integer | O | 最大再試行回数。1～10の値。 |
| url_path | String | - | ヘルスチェックリクエストURL |
| http_method | String | - | ヘルスチェックに使用するHTTP Method<br>デフォルト値はGET|
| expected_codes | String | - | 正常状態とみなすメンバーのHTTP(S)レスポンスコード<br>expected_codesはリスト(`200,201,202`)または範囲指定(`200-202`)も可能。 |
| admin_state_up | Boolean | - | 管理者制御状態 |
| host_header | String | - | ヘルスチェックに使用するホストヘッダのフィールド値<br>ヘルスチェックタイプを`TCP`に設定した場合、このフィールドに設定した値は無視 |
| health_check_port | Integer | - | ヘルスチェックの対象となるメンバーポート |

### メンバー作成

<font color='red'>**(注意)NHN Cloudでメンバー作成時に`subnet_id`を必ず指定します。また`name`はサポートしません。**</font>

```
resource "nhncloud_lb_member_v2" "tf_member_01"{
  pool_id = nhncloud_lb_pool_v2.tf_pool_01.id
  subnet_id = data.nhncloud_networking_vpcsubnet_v2.default_subnet.id
  address = nhncloud_compute_instance_v2.tf_instance_01.access_ip_v4
  protocol_port = 8080
  weight = 4
  admin_state_up = true
}

```
| 名前 | 形式 | 必須 | 説明 |
| ------ | ---- | ---- | --------- |
| pool_id | String | O | 作成するメンバーが属すプールID |
| subnet_id | String | O | 作成するメンバーのサブネットID |
| address | String | O | ロードバランサーからトラフィックを受信するメンバーのIPアドレス |
| protocol_port | Integer | O | トラフィックを受信するメンバーのポート |
| weight | Integer | - | プールで受信する必要があるトラフィックの重み<br>高いほどトラフィックをたくさん受信する。 |
| admin_state_up | Boolean | - | 管理者制御状態 |


### シークレットの作成

```
resource "nhncloud_keymanager_secret_v1" "secret_01" {
  algorithm            = "aes"
  bit_length           = 256
  mode                 = "cbc"
  name                 = "mysecret"
  payload              = "foobar"
  payload_content_type = "text/plain"
  secret_type          = "passphrase"
}
```

| 名前                      | 形式 | 必須 | 説明                                                                                                                                                          |
|--------------------------| ---- |----|--------------------------------------------------------------------------------------------------------------------------------------------------------------|
| name                     | String | -  | シークレットの名前                                                                                                                                                      |
| expiration               | Datetime | -  | 有効期限。ISO8601形式でリクエスト                                                                                                                                        |
| algorithm                | String | -  | 暗号化アルゴリズム                                                                                                                                                    |
| bit_length               | String | -  | 暗号化キーの長さ                                                                                                                                                     |
| mode                     | String | -  | ブロック暗号運用方式                                                                                                                                                 |
| payload                  | String | -  | 暗号化キーペイロード                                                                                                                                                  |
| payload_content_type     | String | -  | 暗号化キーペイロードコンテンツタイプ </br>payloadを入力する時、必ず入力しなければならない </br>サポートするコンテンツタイプリスト: `text/plain`, `application/octet-stream`, `application/pkcs8`, `application/pkix-cert` |
| payload_content_encoding | Enum | -  | 暗号化キーペイロードエンコード方式 </br>payload_content_typeが`text/plain`ではない場合、必ず入力する必要があります。</br> `base64`のみサポート                                                              |
| secret_type              | Enum | -  | シークレットタイプ </br>`symmetric`, `public`, `private`, `passphrase`, `certificate`, `opaque`のいずれか                                                                     |


### シークレットコンテナの作成

```
resource "nhncloud_keymanager_secret_v1" "secret_01" {
...
}
resource "nhncloud_keymanager_container_v1" "container_01" {
  name      = "terraform_container_01"
  type      = "generic"
  secret_refs {
    secret_ref = nhncloud_keymanager_secret_v1.secret_01.secret_ref
  }
}
```

| 名前  | 形式    | 必須 | 説明                                                                                                                                                                                            |
|------|--------|----|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| type | Enum   | O  | コンテナタイプ </br>`generic`, `rsa`, `certificate`のいずれか                                                                                                                                              |
| name | String | -  | コンテナ名                                                                                                                                                                                       |
| secret_refs | Array  | -  | コンテナに登録するシークレットリスト                                                                                                                                                                              |
| secret_refs.secret_ref	 | String | -  | シークレットアドレス                                                                                                                                                                                        |
| secret_refs.name	 | String | -  | コンテナが指定したシークレット名 </br>コンテナタイプが`certificate`の場合：`certificate`, `private_key`, `private_key_passphrase`, `intermediates`に指定 </br>コンテナタイプが`rsa`の場合：`private_key`, `private_key_passphrase`, `public_key`に指定 |


## Resources - セキュリティグループ

### セキュリティグループの作成

```
resource "nhncloud_networking_secgroup_v2" "resource-sg-01" {
  name      = "sg-01"
}
```

| 名前  | 形式    | 必須 | 説明              | 
|------|--------|---|------------------|
| name | String | O | セキュリティグループ名        |
| region | String | - | セキュリティグループが割り当てられるリージョン名 |

### セキュリティルールの作成

```
resource "nhncloud_networking_secgroup_rule_v2" "resource-sg-rule-01" {
  direction         = "ingress"
  ethertype         = "IPv4"
  protocol          = "tcp"
  port_range_min    = 22
  port_range_max    = 22
  remote_ip_prefix  = "0.0.0.0/0"
  security_group_id = data.nhncloud_networking_secgroup_v2.sg-01.id
}
###################### Data Sources ######################
data "nhncloud_networking_secgroup_v2" "sg-01" {
  name = "sg-01"
}
```

| 名前 | 形式   | 必須 | 説明             | 
|------|--------|---|------------------|
| remote_group_id | UUID | - | セキュリティルールの遠隔セキュリティグループID |
| direction | Enum | O | セキュリティルールが適用されるパケットの方向<br>**ingress**, **egress** |
| ethertype | Enum | - | `IPv4`で指定。省略時は`IPv4`で指定 |
| protocol | String | - | セキュリティルールのプロトコル名。省略時は全てのプロトコルに適用。 |
| port_range_max | Integer | - | セキュリティルールのポート範囲最大値 |
| port_range_min | Integer | - | セキュリティルールのポート範囲最小値 |
| security_group_id | UUID | O | セキュリティルールが属するセキュリティグループID |
| remote_ip_prefix | Enum | - | セキュリティルールの宛先IPプレフィックス |
| description | String | - | セキュリティルールの説明 |

## 参考サイト
Terraform Documentation - [https://www.terraform.io/docs/providers/index.html](https://www.terraform.io/docs/providers/index.html)
