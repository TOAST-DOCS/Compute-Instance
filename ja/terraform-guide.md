## Compute > Instance > サードパーティ使用ガイド > Terraform使用ガイド

この文書ではTOAST環境でTerraformを利用してインスタンスを管理する方法を説明します。

## Terraform
Terraformはインフラの簡単な構築、安全な変更、そして効率的にインフラの形状を管理できるオープンソースのツールです。Terraformの主な特徴は次のとおりです。

* **Infrastructure as Code**
    * インフラをコードで定義し、生産性と透明性を高めることができます。
    * 定義したコードを簡単に共有でき、効率的に協業することができます。
* **Execution Plan**
    * 変更計画と変更適用を分離して、変更内容を適用する時に発生しうるミスを減らすことができます。
* **Resource Graph**
    * ささいな変更がインフラ全体にどのような影響を与えるか、あらかじめ確認できます。
    * 従属性グラフを作成し、このグラフを元に計画を立て、この計画を適用した時に変更されるインフラ状態を確認できます。
* **Change Automation**
    * 複数の場所に同じ構成のインフラを構築し変更できるように自動化することができます。
    * インフラを構築するのにかかる時間を節約でき、ミスを減らすことができます。

Terraformは主要プロバイダーのほぼ全てのソリューションをサポートします。

* AWS、 BareMetal、 Bitbucket、 Chef、 Cloudflare、 Docker、 GitHub、 Google Cloud、 Grafana、 InfluxDB、 Heroku、 Microsoft Azure、 MySQL、 OpenStack、 PostgreSQLなど


## Terraformインストール
[Terraformダウンロードページ](https://www.terraform.io/downloads.html)からローカルPCのオペレーションシステムに合ったファイルをダウンロードします。ファイルを解凍し、希望する場所に入れた後、環境設定に該当のパスを追加したらインストールが完了します。

次はインストール例です。

```
$ wget https://releases.hashicorp.com/terraform/0.11.1/terraform_0.11.1_linux_amd64.zip
$ unzip terraform_0.11.1_linux_amd64.zip
$ export PATH="${PATH}:$(pwd)"
$ terraform - v
Terraform v0.10.5

Your version of Terraform is out of date! The latest version
is 0.11.1. You can update by downloading from www.terraform.io
```

> [参考]
> この例では`export`コマンドを利用してパスを設定したため、ターミナルを閉じると設定したパスが消えます。
> `.bashrc`または`.bash_%file`のようなユーザープロファイルからパスを設定するようにすると、継続して使用できます。

パラメータなしでTerraformを実行すると、簡単な使用方法が表示されます。

```
$ terraform
Usage: terraform [--version] [--help] <command> [args]

The available commands for execution are listed below.
The most common、 useful commands are shown first、 followed by
less common or more advanced commands. If you're just getting
started with Terraform、 stick with the common commands. For the
other commands、 please read the help and docs before usage.

Common commands:
    apply              Builds or changes infrastructure
    console            Interactive console for Terraform interpolations
    destroy            Destroy Terraform-managed infrastructure
    env                Workspace management
    fmt                Rewrites config files to canonical format
    get                Download and install modules for the configuration
    graph              Create a visual graph of Terraform resources
    import             Import existing infrastructure into Terraform
    init               Initialize a Terraform working directory
    output             Read an output from a state file
    plan               Generate and show an execution plan
    providers          Prints a tree of the providers used in the configuration
    push               Upload this Terraform module to Atlas to run
    refresh            Update local state file against real resources
    show               Inspect Terraform state or plan
    taint              Manually mark a resource for recreation
    untaint            Manually unmark a resource as tainted
    validate           Validates the Terraform files
    version            Prints the Terraform version
    workspace          Workspace management

All other commands:
    debug              Debug output management (experimental)
    force-unlock       Manually unlock the terraform state
    state              Advanced state management
```

## TOAST環境で使用

TOAST環境でTerraFormを利用してインスタンスを生成、追加、変更、削除する方法を、例を挙げて説明します。

> [参考]
> 下記の例のデータは全て実際の情報ではありません。必ず正確な情報に修正してください。

### Terraform初期化

Terraformを使用する前に、次のようにプロバイダー設定ファイルを構成する必要があります。

```
$ vi provider.tf
# Configure the OpenStack Provider
provider "openstack" {
  user_name   = "terraform-guide@nhnent.com"
  tenant_id   = "75a4c0a12fd84edeb68965d320d17129"
  password    = "kGzBDD9psmgeH4ji"
  auth_url    = "https://api-gw.cloud.toast.com/terraform/identity/v2.0"
  region      = "RegionOne"
}
```

* **provider**
    * プロバイダー名を明示する必要があります。
    * TOASTはOpenStacで構築されているので、プロバイダー名は**openstack**です。
* **user_name**
    * **APIセキュリティー設定** メニューで発行できる**User Access Key ID**(またはTOASTアカウントID)を使用します。
* **tenant_id**
    * TOASTコンソールの**Compute > Instance > Management** メニューで**API Endpoint設定** ボタンをクリックしてテナントIDを確認できます。
* **password**
    * **APIセキュリティー設定** メニューから発行できる**Secret Access Key**を使用します。
* **auth_url**
    * auth_urlは``です。
* **region**
    * 韓国リージョンは**RegionOne**を使用します。

> [参考]
> User Access Key IDとSecret Access Keyの発行はAPI準備ガイドの[トークンAPI](/Compute/Instance/ja/api-guide/#api)項目を参照してください。


構成したプロバイダー設定ファイルがあるパスで`init`コマンドを利用してTerraformを初期化します。

```
$ terraform init

Initializing provider plugins...
- Checking for available provider plugins on https://releases.hashicorp.com...
- Downloading plugin for provider "openstack" (1.1.0)...

The following providers do not have any version constraints in configuration、
so the latest version was installed.

To prevent automatic upgrades to new major versions that may contain breaking
changes、 it is recommended to add version = "..." constraints to the
corresponding provider blocks in configuration、 with the constraint strings
suggested below.

* provider.openstack: version = "~> 1.1"

Terraform has been successfully initialized!

You may now begin working with Terraform. Try running "terraform plan" to see
any changes that are required for your infrastructure. All Terraform commands
should now work.

If you ever set or change modules or backend configuration for Terraform、
rerun this command to reinitialize your working directory. If you forget、 other
commands will detect it and remind you to do so if necessary.
```

### インスタンス生成

インスタンスを生成するには、次のように.tfファイルに生成するインスタンス情報を入力する必要があります。

```
$ vi terraform-instance-01.tf
# Create a web server
resource "openstack_compute_instance_v2" "terraform-instance-01" {
  name      = "terraform-instance-01"
  region    = "RegionOne"
  flavor_id = "da74152c-0167-4ce9-b391-8a88a8ff2754"
  key_pair  = "terraform-keypair"
  network {
    uuid = "00d5b852-cb77-4307-b6be-d81dad24eec1"
    name = "Default Network"
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

* **resource**
    * リソースタイプとリソース名で構成されています。
    * TOASTはOpenStackで構築されているので、リソースタイプは**openstack_compute_instance_v2**です。
    * リソース名は生成するインスタンスの名前です。
* **name**
    * 生成するインスタンスの名前です。
* **region**
    * プロバイダー設定ファイルに入力した内容と同じである必要があります。
* **flavor_id**
    * 生成するインスタンスの仕様IDです。
    * TOASTから提供する公開APIのうち[インスタンス仕様リスト照会API](/Compute/Instance/ja/api-guide/#_18)を通して照会できます。
* **key_pair**
    * インスタンス接続に使用するキーペアの名前です。
    * TOASTコンソールの**Compute > Instance > Key Pair** メニューで新たに作成するか、既に持っているキーペアを登録することができます。詳細についてはコンソール使用ガイドの[キーペア](/Compute/Instance/ja/console-guide/#_7)項目を参照してください。    
* **network**
    * インスタンスに接続するVPCの名前とuuidを入力します。
    * TOASTコンソールの**Network > VPC > Management** メニューで接続するVPCを選択すると、下段の詳細情報画面で名前とuuidを確認できます。
* **security_groups**
    * インスタンスで使用するセキュリティーグループの名前です。
    * カンマ(,)で区分し、1つ以上のセキュリティーグループを指定できます。
    * TOASTコンソールの**Network > VPC > Security Groups** メニューで使用するセキュリティーグループを選択すると、下段の詳細情報画面で情報を確認できます。
* **block_device**
    * インスタンスに使用するイメージまたはブロックストレージ情報とディスク容量を設定します。
    * uuid
        * TOASTコンソールの**Compute > Images** メニューで使用するイメージを選択すると、下段の詳細情報画面で情報を確認できます。
    * source_type
        * イメージを利用してインスタンスを生成するなら、source_typeは**image**です。
    * destination_type
        * ブロックデバイスをインスタンスのディスクに使用するならdestination_typeは**volume**です。
    * boot_index
        * ブロックデバイスをインスタンスのブートディスクに使用するならboot indexは**0**です。
    * volume_size
        * 生成するインスタンスで使用するディスクの容量を設定します。
        * 最小20GBから最大1,000GBまで設定できます。
        * インスタンス仕様によって設定できる容量が異なります。詳細についてはコンソール使用ガイドの[インスタンス生成 > 仕様](/Compute/Instance/ja/console-guide/#_4)項目を参照してください。
    * delete_on_termination
        * このオプションがtrueに設定されていると、インスタンスを削除する時、ブロックデバイスも一緒に削除されます。


.tfファイルがあるパスで`plan`コマンドを実行すると、Terraformが.tfファイルをロードし、設定が正しいか確認して自DBと比較してプランを作成します。プラン作成が完了すると、プランをタイプ別に集計して見やすく表示します。

```
./terraform plan
Refreshing Terraform state in-memory prior to plan...
The refreshed state will be used to calculate this plan、 but will not be
persisted to local or remote state storage.


------------------------------------------------------------------------

An execution plan has been generated and is shown below.
Resource actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  + openstack_compute_instance_v2.terraform-test-01
      id:                                   <computed>
      access_ip_v4:                         <computed>
      access_ip_v6:                         <computed>
      all_metadata.%:                       <computed>
      availability_zone:                    <computed>
      block_device.#:                       "1"
      block_device.0.boot_index:            "0"
      block_device.0.delete_on_termination: "true"
      block_device.0.destination_type:      "volume"
      block_device.0.source_type:           "image"
      block_device.0.uuid:                  "6d0993b4-cd6d-4242-b59b-94258f265331"
      block_device.0.volume_size:           "20"
      flavor_id:                            "da74152c-0167-4ce9-b391-8a88a8ff2754"
      flavor_name:                          <computed>
      force_delete:                         "false"
      image_id:                             <computed>
      image_name:                           <computed>
      key_pair:                             "tf-test"
      name:                                 "terraform-test-01"
      network.#:                            "1"
      network.0.access_network:             "false"
      network.0.fixed_ip_v4:                <computed>
      network.0.fixed_ip_v6:                <computed>
      network.0.floating_ip:                <computed>
      network.0.mac:                        <computed>
      network.0.name:                       "Default Network"
      network.0.port:                       <computed>
      network.0.uuid:                       "00d5b852-cb77-4307-b6be-d81dad24eec1"
      region:                               "RegionOne"
      security_groups.#:                    "1"
      security_groups.3814588639:           "default"
      stop_before_destroy:                  "false"


Plan: 1 to add、 0 to change、 0 to destroy.

------------------------------------------------------------------------

Note: You didn't specify an "-out" parameter to save this plan、 so Terraform
can't guarantee that exactly these actions will be performed if
"terraform apply" is subsequently run.
```


`apply`コマンドを実行すると、プランを適用してインスタンスを生成します。そしてプラン変更履歴を記録する自DBファイル(terraform.tfstate)を生成します。

```
$ terraform apply
openstack_compute_instance_v2.terraform-test-01: Creating...
  access_ip_v4:                         "" => "<computed>"
  ...
  stop_before_destroy:                  "" => "false"
openstack_compute_instance_v2.terraform-test-01: Still creating... (10s elapsed)
openstack_compute_instance_v2.terraform-test-01: Still creating... (20s elapsed)
openstack_compute_instance_v2.terraform-test-01: Still creating... (30s elapsed)
openstack_compute_instance_v2.terraform-test-01: Still creating... (40s elapsed)
openstack_compute_instance_v2.terraform-test-01: Still creating... (50s elapsed)
openstack_compute_instance_v2.terraform-test-01: Still creating... (1m0s elapsed)
openstack_compute_instance_v2.terraform-test-01: Still creating... (1m10s elapsed)
openstack_compute_instance_v2.terraform-test-01: Creation complete after 1m12s (ID: f4d135bc-6a70-4c4d-b645-931570c9f6b1)

Apply complete! Resources: 1 added、 0 changed、 0 destroyed.

$ ls
provider.tf               tc-instance-01.tf         terraform.tfstate         terraform.tfstate.backup
```

生成したインスタンスはTOASTコンソールの**Compute > Instance > Management** メニューで確認できます。

### インスタンス追加生成

インスタンス追加は生成と同じ方法で.tfファイルを作成してプランを適用します。複数の.tfファイルを作成していっぺんに適用することもできます。

### インスタンス変更

インスタンスを変更する.tfファイルを開いて情報を修正してプランを適用します。

変更できる仕様は限定的です。ディスクの新規追加や、インスタンスに接続したセキュリティーグループとVPCの削除や交換ができます。ブートディスクの容量を変更すると、既存のインスタンスは削除され、新たなインスタンスが生成されます。インスタンスの仕様はインスタンスが終了した状態でのみ変更できます。

下の例はセキュリティーグループを一つ追加したものです。

```
$ vi terraform-instance-01.tf
resource "openstack_compute_instance_v2" "terraform-instance-01" {
  ...
  security_groups = ["default"、 "terraform-sg"]
  ...
}
```

プランをローディングすると変更されたセキュリティーグループ情報を整理して表示します。

```
$ terraform plan
Refreshing Terraform state in-memory prior to plan...
The refreshed state will be used to calculate this plan、 but will not be
persisted to local or remote state storage.

openstack_compute_instance_v2.terraform-instance-01: Refreshing state... (ID: 4d135bc-6a70-4c4d-b645-931570c9f6b1)

------------------------------------------------------------------------

An execution plan has been generated and is shown below.
Resource actions are indicated with the following symbols:
  ~ update in-place

Terraform will perform the following actions:

  ~ openstack_compute_instance_v2.terraform-instance-01
      security_groups.#:          "1" => "2"
      security_groups.3814588639: "default" => "default"
      security_groups.4051241745: "" => "terraform-sg"


Plan: 0 to add、 1 to change、 0 to destroy.

------------------------------------------------------------------------

Note: You didn't specify an "-out" parameter to save this plan、 so Terraform
can't guarantee that exactly these actions will be performed if
"terraform apply" is subsequently run.
```

プランを適用すると、インスタンスに新たなセキュリティーグループが追加されます。

```
$ terraform apply
openstack_compute_instance_v2.terraform-instance-01: Refreshing state... (ID: 4d135bc-6a70-4c4d-b645-931570c9f6b1)
openstack_compute_instance_v2.terraform-instance-01: Modifying... (ID: 4d135bc-6a70-4c4d-b645-931570c9f6b1)
  security_groups.#:          "1" => "2"
  security_groups.3814588639: "default" => "default"
  security_groups.4051241745: "" => "terraform-sg"
openstack_compute_instance_v2.terraform-instance-01: Modifications complete after 1s (ID: 4d135bc-6a70-4c4d-b645-931570c9f6b1)

Apply complete! Resources: 0 added、 1 changed、 0 destroyed.
```

### インスタンス削除

インスタンス生成で使用していた.tfファイルを削除してプランを適用すると、インスタンスが削除されます。

プランをローディングすると、リソース設定を削除したため削除されたプランが1件あります、と表示されます。

```
$ rm tc-instance-01.tf
$ terraform plan
Refreshing Terraform state in-memory prior to plan...
The refreshed state will be used to calculate this plan、 but will not be
persisted to local or remote state storage.

openstack_compute_instance_v2.terraform-test-01: Refreshing state... (ID: 4d135bc-6a70-4c4d-b645-931570c9f6b1)

------------------------------------------------------------------------

An execution plan has been generated and is shown below.
Resource actions are indicated with the following symbols:
  - destroy

Terraform will perform the following actions:

  - openstack_compute_instance_v2.terraform-test-01


Plan: 0 to add、 0 to change、 1 to destroy.

------------------------------------------------------------------------

Note: You didn't specify an "-out" parameter to save this plan、 so Terraform
can't guarantee that exactly these actions will be performed if
"terraform apply" is subsequently run.
```

プランを適用するとインスタンスが削除されます。

```
$ terraform apply
openstack_compute_instance_v2.terraform-test-01: Refreshing state... (ID: f4d135bc-6a70-4c4d-b645-931570c9f6b1)
openstack_compute_instance_v2.terraform-test-01: Destroying... (ID: f4d135bc-6a70-4c4d-b645-931570c9f6b1)
openstack_compute_instance_v2.terraform-test-01: Still destroying... (ID: f4d135bc-6a70-4c4d-b645-931570c9f6b1、 10s elapsed)
openstack_compute_instance_v2.terraform-test-01: Destruction complete after 11s
```

## HCL

Terraform設定ファイルはHCL(HashiCorp Configuration Language)を使用します。 HCLはTerraform形式(`.tf`)とJSON形式(`tf.json`)を使用します。

指定したフォルダに`.tf`、 `.tf.json`を保存するとTerraFormがアルファベット順にロードします。変数やリソースの定義順序は関係ありません。

そのほか、他の設定を上書きするためのオーバーライドファイルを使用することができます。ファイル名は`override`または`_override`で終わるようにします。オーバーライドファイルは他の設定ファイルのローディングが全て終わるとアルファベット順にロードして設定を上書きします。

### HCL文法

* **コメント**

**#**、 **//**、 **/\* \*/**を使用できます。

* **値の代入**

**key = value** 形式を使用します。値は文字列、数字、Boolean、リスト、マップを全て使用できます。

* **文字列**

二重引用符を使用します。複数行の文字列を使用する時は[UNIXシェルのHere document](https://en.wikipedia.org/wiki/Here_document)形式で`<<EOF`、 `EOF`の間に文字列を入力する必要があります。

```
description = <<EOF
...文字列文字列文字列文字列...
...文字列文字列文字列文字列...
...文字列文字列文字列文字列...
EOF
```

* **リソース**

リソースを宣言する時は**resource** キーワードを使用し、プロバイダーに応じてTerraformが定義しておいたリソースタイプを明示する必要があります。

```
resource "openstack_compute_instance_v2" "web" {
    ...
}
```

* **プロバイダー**

プロバイダーを宣言する時は**provider** キーワードを使用します。リソースを宣言する時、明示したリソースタイプの接頭辞がプロバイダータイプです。

```
# リソースタイプ: openstack_compute_instance_v2
provider "openstack" {
    ...
}

# リソースタイプ: aws_instance
provider "aws" {
    ...
}
```

* **データソース**

プロバイダーから持ってくるデータをデータソースと呼びます。**data** キーワードを使用し、タイプ(type)と名前(name)で構成されます。

```
data "type" "name" {
    ...
}
```

* **変数**

変数を宣言する時は**variable** キーワードを使用します。推論するため定義しなくても構いません。

```
variable "name" {
    type = "type"
    default = {
        key = value
    }
    description = "description"
}
```

* **モジュール**

**module** キーワードを使用すると、定義したリソースグループをモジュールに持ってきて使用することができます。 GitHub、 Bitbucketなどをサポートしています。

```
module "name" {
    source = "url"
    ...
}
```

## 参考サイト
Terraform Documentation - [https://www.terraform.io/docs/providers/index.html](https://www.terraform.io/docs/providers/index.html)

