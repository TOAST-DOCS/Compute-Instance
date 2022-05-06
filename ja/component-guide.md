## Compute > Instance > インストールコンポーネントガイド


## MS-SQL Instance

## MySQL Instance

## PostgreSQL Instance

## CUBRID Instance

## MariaDB Instance

## Tibero Instance

## JEUS Instance

基本提供されるイメージにはCentOS 7.8 with JEUS8Fix1(Domain Administrator Server 2022.03.22), CentOS 7.8 with JEUS8Fix1(Managed Server 2022.03.22)が含まれます。
Domain Administrator Serverをインストールするには、JEUS8Fix1(Domain Administrator Server 2022.03.22)イメージを使用します。
Managed Serverをインストールするには、CentOS 7.8 with JEUS8Fix1(Managed Server 2022.03.22)イメージを使用します。

JEUSは`~/apps/jeus8にインストールされます。

インストール時、以下のプロパティで設定されます。

| 区分 | デフォルト値 |
| --- | --- |
| ドメイン名 | jeus_domain |
| WebAdminポート | 9736 |
| Adminサーバー名 | adminServer |
| AdminユーザーID | administrator |
| Adminユーザーパスワード | jeusadmin |
| ノードマネージャ | java |


## 起動確認

JEUSの設定や制御を行うにはノードマネージャを起動した後、WebAdminまたはjeusadminから制御する必要があります。

### インスタンス接続

インスタンスの作成が完了したら、SSHを使用してインスタンスにアクセスします。
インスタンスにFloating IPが接続されていて、セキュリティグループでTCPポート22(SSH)が許可されている必要があります。

### ノードマネージャ起動

シェルに接続してstartNodeManagerコマンドでノードマネージャを実行します。
ノードマネージャ同士の通信が必要なため、セキュリティグループに基本ポート7730の許可ルールを追加する必要があります。

### JEUS起動

Domain Administrator ServerはstartDomainAdminServerコマンドで実行します。

```
startDomainAdminServer -uadministrator -pjeusadmin
```

### JEUS WebAdmin

次のようにWebAdminを実行します。

1. DASがインストールされたインスタンスにFloating IPを設定します。
2. 該当インスタンスのセキュリティグループに9736ポートの許可ルールを追加します。
3. Webブラウザでhttp://{Floating IP}:9736/webadminに接続するとWebAdmin画面を確認できます。


## WebtoB Instance

基本提供されるイメージはWebtoB5Fix4 with CentOS 7.8です。
WebtoBは`~/apps/webtob`にインストールされます。


### 起動確認

#### インスタンス接続

インスタンスの作成が完了したら、SSHを使用してインスタンスにアクセスします。
インスタンスにFloating IPが接続されていて、セキュリティグループでTCPポート22(SSH)が許可されている必要があります。

#### 環境設定ファイルのコンパイル

wscflコマンドを利用して設定ファイルをコンパイルします。

```
wscfl -i http.m
```

#### WebtoB起動

wsbootを利用してWebtoBを起動します。

```
wsboot
```

wsadminを利用して状態の確認や制御を行うことができます。
