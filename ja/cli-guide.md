# OpenStack CLIユーザーガイド

## Compute > Instance > CLIガイド

このドキュメントでは、OpenStack CLI(Command Line Interface)を使用してNHN Cloudを利用する方法を説明します。

## 概要

OpenStack CLIは、NHN Cloudのさまざまなサービスにアクセスできるコマンドラインツールです。このガイドを通じて、次のサービスをCLIで管理できます。

- **Compute**: インスタンス、イメージ、キーペアポリシー
- **Network**: VPC、サブネット、ルーター、セキュリティグループ
- **Storage**: ブロックストレージ、スナップショット、オブジェクトストレージ

## 事前準備

### 1. Python環境設定

OpenStack CLIはPythonをベースに動作します。Python 3.9以上のバージョンを推奨します。

```bash
# Python バージョン確認
python3 --version

# virtualenvによる隔離された環境構成
virtualenv .venv --python=python3.9
source .venv/bin/activate  # Linux/macOS
# または
.venv\Scripts\activate     # Windows
```

### 2. OpenStack CLIクライアントインストール

#### 統合クライアントインストール(推奨)

```bash
pip install python-openstackclient
```

#### 個別サービスクライアントインストール

必要なサービスのみ選択的にインストールできます。

```bash
# Compute サービス (インスタンス、イメージなど)
pip install python-novaclient

# Network サービス (ネットワーク、セキュリティグループなど)
pip install python-neutronclient

# Storage サービス (ブロックストレージ)
pip install python-cinderclient

# Image サービス (イメージ管理)
pip install python-glanceclient

# Orchestration サービス (Heat テンプレート)
pip install python-heatclient

# Object Storage サービス
pip install python-swiftclient
```

### 3. インストール確認

```bash
openstack --version
```

## 認証設定

NHN CloudでOpenStack CLIを使用するには、認証情報を設定する必要があります。2つの方法を提供します。

### 方法1: clouds.yamlファイル使用(推奨)

#### 1.1 APIエンドポイント設定

NHN CloudコンソールでAPIエンドポイントを設定します。

1. NHN Cloudコンソールにログイン
2. **APIエンドポイント設定**メニューにアクセス
3. APIパスワード設定
4. テナントIDとユーザーUUIDを確認

#### 1.2 clouds.yamlファイル作成

OpenStack CLIは次の場所で`clouds.yaml`ファイルを検索します。
- 現在のディレクトリ
- `~/.config/openstack/`
- `/etc/openstack/`

`~/.config/openstack/clouds.yaml`ファイルを作成し、次のように設定します。

```yaml
clouds:
  nhn_cloud:
    auth:
      auth_url: https://kr1-api-identity.infrastructure.cloud.toast.com
      username: {사용자_UUID}
      password: {API_비밀번호}
      project_name: c_{프로젝트_ID}
      project_domain_name: default
      user_domain_name: default
    region_name: KR1
    identity_api_version: 3
    volume_api_version: 3
    image_api_version: 2
    interface: public
```

#### 1.3 設定確認

```bash
# クラウド設定確認
openstack --os-cloud nhn_cloud token issue

# alias設定(オプション)
echo 'alias nhncloud="openstack --os-cloud nhn_cloud"' >> ~/.bashrc
source ~/.bashrc

# alias使用
nhncloud token issue
```

### 方法2: 環境変数使用

#### 2.1 keystonercファイル作成

```bash
touch ~/keystonerc_nhncloud
```

#### 2.2 環境変数設定

`~/keystonerc_nhncloud`ファイルに次の内容を追加します。

```bash
# 기본 도메인 설정
export OS_PROJECT_DOMAIN_ID=default
export OS_USER_DOMAIN_ID=default

# 프로젝트 정보
export OS_PROJECT_NAME=c_{프로젝트_ID}
export OS_TENANT_NAME=c_{프로젝트_ID}

# 사용자 정보
export OS_USERNAME={사용자_UUID}
export OS_PASSWORD={API_비밀번호}

# 인증 URL
export OS_AUTH_URL=https://kr1-api-identity.infrastructure.cloud.toast.com

# API 버전
export OS_IDENTITY_API_VERSION=3
export OS_VOLUME_API_VERSION=3
export OS_IMAGE_API_VERSION=2

# 리전 설정
export OS_REGION_NAME=KR1
export OS_INTERFACE=public
```

#### 2.3 環境変数適用

```bash
source ~/keystonerc_nhncloud

# 設定確認
openstack token issue
```

### リージョン別設定

NHN Cloudは複数のリージョンを提供します。リージョンに応じて`OS_REGION_NAME`を異なる値に設定する必要があります。

| リージョン | OS_REGION_NAME | 説明 |
|------|----------------|------|
| 韓国(パンギョ) | KR1 | デフォルトリージョン |
| 韓国(ピョンチョン) | KR2 | 補助リージョン |

## 基本CLIコマンド

### ヘルプおよびバージョン確認

```bash
# 全体ヘルプ
openstack --help

# 特定コマンドのヘルプ
openstack server --help

# バージョン確認
openstack --version
```

### 認証トークン管理

```bash
# トークン発行
openstack token issue

# トークン情報確認
openstack token show

# トークン削除
openstack token revoke
```

## Computeサービス管理

### インスタンス管理

#### インスタンス一覧照会

```bash
# 基本インスタンス一覧
openstack server list

# 詳細情報を含む
openstack server list --long

# 特定ステータスのインスタンスのみ照会
openstack server list --status ACTIVE

# 特定の名前パターンで照会
openstack server list --name "web-*"
```

#### インスタンス詳細情報照会

```bash
# 特定インスタンス情報
openstack server show {인스턴스_ID}

# インスタンスコンソールURL照会
openstack console url show {인스턴스_ID}

# インスタンスログ照会
openstack console log show {인스턴스_ID}
```

#### インスタンス作成

```bash
# 基本インスタンス作成
openstack server create \
  --image {이미지_ID} \
  --flavor {플레이버_ID} \
  --network {네트워크_ID} \
  --key-name {키페어_이름} \
  --security-group {보안그룹_이름} \
  {인스턴스_이름}

# 詳細オプションを含むインスタンス作成
openstack server create \
  --image {이미지_ID} \
  --flavor {플레이버_ID} \
  --network {네트워크_ID} \
  --key-name {키페어_이름} \
  --security-group {보안그룹_이름} \
  --availability-zone {가용영역} \
  --user-data {사용자데이터_파일} \
  --property key=value \
  {인스턴스_이름}
```

#### インスタンスステータス管理

```bash
# インスタンス開始
openstack server start {인스턴스_ID}

# インスタンス停止
openstack server stop {인스턴스_ID}

# インスタンス再起動
openstack server reboot {인스턴스_ID}

# 強制再起動
openstack server reboot --hard {인스턴스_ID}

# インスタンス一時停止
openstack server pause {인스턴스_ID}

# インスタンス一時停止解除
openstack server unpause {인스턴스_ID}

# インスタンス終了(削除)
openstack server delete {인스턴스_ID}
```

#### インスタンス変更

```bash
# インスタンス名変更
openstack server set --name {새_이름} {인스턴스_ID}

# メタデータ追加
openstack server set --property key=value {인스턴스_ID}

# メタデータ削除
openstack server unset --property key {인스턴스_ID}
```

### イメージ管理

#### イメージ一覧照会

```bash
# すべてのイメージ照会
openstack image list

# パブリックイメージのみ照会
openstack image list --public

# 特定ステータスのイメージ照会
openstack image list --status active
```

#### イメージ詳細情報

```bash
# イメージ詳細情報
openstack image show {이미지_ID}

# イメージメタデータ照会
openstack image show --property key {이미지_ID}
```

#### イメージ作成

```bash
# インスタンスからイメージ作成
openstack server image create --name {이미지_이름} {인스턴스_ID}

# 外部イメージアップロード
openstack image create \
  --disk-format qcow2 \
  --container-format bare \
  --file {이미지_파일} \
  {이미지_이름}
```

#### イメージ削除

```bash
openstack image delete {이미지_ID}
```

### フレーバー管理

#### フレーバー一覧照会

```bash
# すべてのフレーバー照会
openstack flavor list

# 詳細情報を含む
openstack flavor list --long
```

#### フレーバー詳細情報

```bash
openstack flavor show {플레이버_ID}
```

### キーペア管理

#### キーペア一覧照会

```bash
openstack keypair list
```

#### キーペア作成

```bash
# 新しいキーペア作成
openstack keypair create {키페어_이름}

# 既存の公開鍵登録
openstack keypair create --public-key {공개키_파일} {키페어_이름}
```

#### キーペア削除

```bash
openstack keypair delete {키페어_이름}
```

### プレイスメントポリシー管理

#### プレイスメントポリシー作成

```bash
# anti-affinityポリシー作成
openstack server group create \
  --policy anti-affinity \
  {정책_이름}
```

#### プレイスメントポリシー一覧

```bash
openstack server group list
```

#### プレイスメントポリシー削除

```bash
openstack server group delete {정책_ID}
```

## Networkサービス管理

### ネットワーク管理

#### ネットワーク一覧照会

```bash
# すべてのネットワーク照会
openstack network list

# 外部ネットワークのみ照会
openstack network list --external

# 特定プロジェクトのネットワーク照会
openstack network list --project {프로젝트_ID}
```

#### ネットワーク作成

```bash
# 基本ネットワーク作成
openstack network create {네트워크_이름}

# 外部ネットワーク作成
openstack network create \
  --external \
  --provider-network-type flat \
  --provider-physical-network physnet1 \
  {네트워크_이름}
```

#### ネットワーク削除

```bash
openstack network delete {네트워크_ID}
```

### サブネット管理

#### サブネット一覧照会

```bash
openstack subnet list
```

#### サブネット作成

```bash
openstack subnet create \
  --network {네트워크_ID} \
  --subnet-range {CIDR} \
  --gateway {게이트웨이_IP} \
  --dns-nameserver {DNS_서버} \
  {서브넷_이름}
```

#### サブネット削除

```bash
openstack subnet delete {서브넷_ID}
```

### ルーター管理

#### ルーター一覧照会

```bash
openstack router list
```

#### ルーター作成

```bash
openstack router create {라우터_이름}
```

#### ルーターにサブネット接続

```bash
openstack router add subnet {라우터_ID} {서브넷_ID}
```

#### ルーターからサブネット分離

```bash
openstack router remove subnet {라우터_ID} {서브넷_ID}
```

#### ルーター削除

```bash
openstack router delete {라우터_ID}
```

### セキュリティグループ管理

#### セキュリティグループ一覧照会

```bash
openstack security group list
```

#### セキュリティグループ作成

```bash
openstack security group create {보안그룹_이름}
```

#### セキュリティグループルール追加

```bash
# インバウンドルール追加
openstack security group rule create \
  --protocol {프로토콜} \
  --dst-port {포트} \
  --ingress \
  {보안그룹_ID}

# アウトバウンドルール追加
openstack security group rule create \
  --protocol {프로토콜} \
  --egress \
  {보안그룹_ID}
```

#### セキュリティグループ削除

```bash
openstack security group delete {보안그룹_ID}
```

## Storageサービス管理

### ブロックストレージ管理

#### ボリューム一覧照会

```bash
# すべてのボリューム照会
openstack volume list

# 特定ステータスのボリューム照会
openstack volume list --status available
```

#### ボリューム作成

```bash
# 基本ボリューム作成
openstack volume create \
  --size {크기_GB} \
  {볼륨_이름}

# 特定ボリュームタイプで作成
openstack volume create \
  --size {크기_GB} \
  --type {볼륨_타입} \
  {볼륨_이름}

# イメージからボリューム作成
openstack volume create \
  --size {크기_GB} \
  --image {이미지_ID} \
  {볼륨_이름}
```

#### ボリュームの接続/分離

```bash
# インスタンスにボリューム接続
openstack server add volume {인스턴스_ID} {볼륨_ID}

# インスタンスからボリューム分離
openstack server remove volume {인스턴스_ID} {볼륨_ID}
```

#### ボリューム削除

```bash
openstack volume delete {볼륨_ID}
```

### スナップショット管理

#### スナップショット一覧照会

```bash
openstack volume snapshot list
```

#### スナップショット作成

```bash
openstack volume snapshot create \
  --volume {볼륨_ID} \
  {스냅샷_이름}
```

#### スナップショットからボリューム作成

```bash
openstack volume create \
  --snapshot {스냅샷_ID} \
  --size {크기_GB} \
  {볼륨_이름}
```

#### スナップショット削除

```bash
openstack volume snapshot delete {스냅샷_ID}
```

## 高度な使用方法

### バッチ作業

```bash
# 複数インスタンスの同時作成
for i in {1..5}; do
  openstack server create \
    --image {이미지_ID} \
    --flavor {플레이버_ID} \
    --network {네트워크_ID} \
    --key-name {키페어_이름} \
    "web-server-$i"
done

# 特定条件のリソース一括削除
openstack server list --name "temp-*" -f value -c ID | xargs -I {} openstack server delete {}
```

### 出力形式のカスタマイズ

```bash
# JSON形式で出力
openstack server list -f json

# CSV形式で出力
openstack server list -f csv

# 特定カラムのみ出力
openstack server list -c ID -c Name -c Status

# ソート
openstack server list --sort-column Name
```

### スクリプト例

#### インスタンスバックアップスクリプト

```bash
#!/bin/bash
# インスタンスバックアップスクリプト

INSTANCE_ID=$1
BACKUP_NAME="backup-$(date +%Y%m%d-%H%M%S)"

if [ -z "$INSTANCE_ID" ]; then
    echo "Usage: $0 <instance_id>"
    exit 1
fi

echo "Creating backup for instance $INSTANCE_ID..."

# インスタンス停止
openstack server stop $INSTANCE_ID

# インスタンスステータス確認
while [ "$(openstack server show $INSTANCE_ID -f value -c Status)" != "SHUTOFF" ]; do
    echo "Waiting for instance to stop..."
    sleep 10
done

# イメージ作成
openstack server image create --name $BACKUP_NAME $INSTANCE_ID

# インスタンス開始
openstack server start $INSTANCE_ID

echo "Backup completed: $BACKUP_NAME"
```

## トラブルシューティング

### 一般的なエラー

#### 認証エラー

```bash
# トークン有効期限切れの場合
openstack token issue

# 認証情報が正しくない場合
# clouds.yamlまたは環境変数を再確認
```

#### 権限エラー

```bash
# プロジェクト権限確認
openstack project list

# ユーザー権限確認
openstack user show {사용자_ID}
```

### デバッグ

```bash
# 詳細ログ出力
openstack --debug server list

# API呼び出し情報出力
openstack --timing server list

# バージョン情報確認
openstack --version
```

## 実用的な使用例

### Webサーバーインフラ構築

```bash
# 1. ネットワーク作成
openstack network create web-network
openstack subnet create \
  --network web-network \
  --subnet-range 192.168.1.0/24 \
  --gateway 192.168.1.1 \
  web-subnet

# 2. セキュリティグループ作成
openstack security group create web-sg
openstack security group rule create \
  --protocol tcp \
  --dst-port 22 \
  --ingress \
  web-sg
openstack security group rule create \
  --protocol tcp \
  --dst-port 80 \
  --ingress \
  web-sg
openstack security group rule create \
  --protocol tcp \
  --dst-port 443 \
  --ingress \
  web-sg

# 3. キーペア作成
openstack keypair create web-keypair

# 4. WebサーバーInstances作成
openstack server create \
  --image {웹서버_이미지_ID} \
  --flavor {플레이버_ID} \
  --network web-network \
  --key-name web-keypair \
  --security-group web-sg \
  --user-data web-init.sh \
  web-server-01
```

### データベースサーバーバックアップ

```bash
# 1. データベースサーバー停止
openstack server stop db-server-01

# 2. サーバーステータス確認
while [ "$(openstack server show db-server-01 -f value -c Status)" != "SHUTOFF" ]; do
    echo "Waiting for server to stop..."
    sleep 10
done

# 3. イメージ作成
openstack server image create \
  --name "db-backup-$(date +%Y%m%d)" \
  db-server-01

# 4. サーバー再起動
openstack server start db-server-01
```

### 自動化スクリプト例

#### リソースクリーンアップスクリプト

```bash
#!/bin/bash
# テストリソースクリーンアップスクリプト

echo "Cleaning up test resources..."

# テストインスタンス削除
for instance in $(openstack server list --name "test-*" -f value -c ID); do
    echo "Deleting instance: $instance"
    openstack server delete $instance
done

# テストボリューム削除
for volume in $(openstack volume list --name "test-*" -f value -c ID); do
    echo "Deleting volume: $volume"
    openstack volume delete $volume
done

# テストネットワーク削除
for network in $(openstack network list --name "test-*" -f value -c ID); do
    echo "Deleting network: $network"
    openstack network delete $network
done

echo "Cleanup completed!"
```

## モニタリングおよびロギング

### リソース使用量モニタリング

```bash
# インスタンス一覧とステータス確認
openstack server list --long

# ボリューム使用量確認
openstack volume list --long

# ネットワーク使用量確認
openstack network list --long

# セキュリティグループルール確認
openstack security group rule list {보안그룹_ID}
```

### ログ分析

```bash
# インスタンスコンソールログ確認
openstack console log show {인스턴스_ID}

# 詳細デバッグ情報出力
openstack --debug server show {인스턴스_ID}

# API呼び出しタイミング情報
openstack --timing server list
```

## パフォーマンス最適化

### バッチ作業最適化

```bash
# 並列処理で複数インスタンス作成
for i in {1..10}; do
    openstack server create \
      --image {이미지_ID} \
      --flavor {플레이버_ID} \
      --network {네트워크_ID} \
      --key-name {키페어_이름} \
      "batch-server-$i" &
done
wait

# バックグラウンドで作業実行
nohup openstack server image create --name backup-$(date +%Y%m%d) {인스턴스_ID} &
```

### 出力最適化

```bash
# JSON形式で出力してパース容易化
openstack server list -f json | jq '.[] | {id: .ID, name: .Name, status: .Status}'

# CSV形式でスプレッドシート分析
openstack volume list -f csv > volumes.csv

# 特定カラムのみ出力してパフォーマンス向上
openstack server list -c ID -c Name -c Status
```

## 参考資料

- [OpenStack CLI公式ドキュメント](https://docs.openstack.org/python-openstackclient/ussuri/)
- [NHN Cloudコンソールガイド](https://docs.nhncloud.com/ja/nhncloud/ko/overview/)
- [NHN Cloud APIガイド](https://docs.nhncloud.com/ja/nhncloud/ko/public-api/)
- [OpenStack Ussuriリリースノート](https://docs.openstack.org/releasenotes/python-openstackclient/ussuri.html)

## サポート

CLI使用中に問題が発生した場合は、次の点を確認してください。

1. OpenStack CLIバージョンがNHN Cloudと互換性があるか確認
2. 認証情報が正しいか確認
3. ネットワーク接続状態を確認
4. NHN Cloud技術サポートチームにお問い合わせ

### 一般的な問題解決

| 問題 | 解決方法 |
|------|-----------|
| 認証失敗 | `openstack token issue`でトークン再発行 |
| 権限エラー | プロジェクト権限およびユーザーロールを確認 |
| ネットワーク接続失敗 | ファイアウォール設定およびDNSを確認 |
| リソース作成失敗 | クォータおよびリージョン設定を確認 |

### 便利なコマンド集

```bash
# システム情報確認
openstack --version
openstack token issue
openstack project list
openstack user show {사용자_ID}

# リソースステータス確認
openstack server list --long
openstack volume list --long
openstack network list --long
openstack security group list

# ヘルプ
openstack help
openstack server --help
openstack network --help
openstack volume --help
```