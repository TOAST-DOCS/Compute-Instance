## Compute > Instance > インストールコンポーネントガイド

## NAT Instance
NATインスタンスは、プライベートネットワークインスタンスから特定IPアドレス帯域にインターネットアクセスできるようにするインスタンスです。
韓国(パンギョ)、韓国(ピョンチョン)リージョンでのみ提供する機能です。

### 主な機能
* インターネットゲートウェイが接続されていないプライベートネットワークのインスタンスがNATインスタンスを介してインターネットにアクセスできます。
* NATインスタンスのFloating IPをソースIPに変更してインターネットにアクセスします。
* NATインスタンスまで転送されたパケットは、NATインスタンスのサブネットに接続されたルーティングテーブルのルート設定に基づいてパケットを転送します。
* NATインスタンスのサブネットに接続されたルーティングテーブルには、該当NATインスタンスをゲートウェイにするルート設定を追加してはいけません。
* インターネット上から始まった流入トラフィックは、プライベートネットワークのインスタンスが受信できません。
* NATインスタンスを介してインターネットアクセスを許可する宛先IPアドレス帯域を指定できます。
* NATインスタンスに接続して他のインスタンスに接続できます。
* セキュリティグループを設定できます。
* ネットワークACLを設定できます。
* 二重化をサポートしません。
* NATインスタンスは、ネットワークインターフェイス設定でネットワークソース/対象確認を必ず「無効」に設定する必要があります。
* NATインスタンスでプライベートイメージを作成する場合、機能が正常に動作しない場合があります。
* NATインスタンスは、1つのネットワークインタフェースでのみNAT機能を提供します。

> [参考] NATゲートウェイとの違い
>
> | 区分 | NATゲートウェイ | NATインスタンス |
> |--|--|--|
> |可用性| 二重化をサポート | 二重化をサポートしない |
> |メンテナンス|NHN Cloudで管理| ユーザーが直接管理|
> |セキュリティグループ|設定不可| 設定可|
> |ネットワークACL| 設定可 | 設定可|
> |SSH|使用不可| 使用可|

### ソース/対象確認設定
NATインスタンスが正常に動作するには、ネットワークインターフェイス設定でネットワークソース/対象確認を無効化する必要があります。

### ルート設定
NATインスタンスをルートゲートウェイに指定します。 NATインスタンスまで転送されたパケットは、NATインスタンスのサブネットに接続されたルーティングテーブルのルート設定に基づいてパケットを転送します。

### 設定注意事項
* NATインスタンスは、1つのネットワークインタフェースのみ使用することを推奨します。 NATインスタンスに複数のネットワークインタフェースを接続しても、1つのインタフェース(eth0)だけがNAT機能を持つことができます。
* NATインスタンスのサブネットに接続されたルーティングテーブルには、該当NATインスタンスをゲートウェイにするルート設定を追加してはいけません。
* NATインスタンスサブネットとNATインスタンスをゲートウェイとして使用するインスタンスのサブネットを分離し、別々のルーティングテーブルを使用することを強く推奨します。
* もし、NATインスタンスサブネットとNATインスタンスをゲートウェイとして使用するインスタンスのサブネットが同じか、同じルーティングテーブルに接続されている時にNATインスタンスをゲートウェイにするルート設定を追加しなければいけない場合は、該当NATインスタンスには必ずFloating IPを設定する必要があります。また、対象CIDRはIP Prefix 0 (/0)を指定する必要があります。 これ以外の設定をすると、通信ができず、そのルーティングテーブルを使用するすべてのインスタンスの通信にも影響を与える場合があります。

> [参考] NATインスタンスのルーティング設定
>
> * NATインスタンスのサブネットがルーティングテーブル1に接続されていて、NATインスタンスをゲートウェイとして使用するインスタンスがルーティングテーブル2に接続されている場合
>     * ルーティングテーブル2のルート設定で、特定CIDR(例：8.8.8.8/32)に対してNATインスタンスをゲートウェイとして指定できます。
>     * ルーティングテーブル1のルート設定にはNATインスタンスをゲートウェイとして指定してはいけません。ただし、NATインスタンスがFloating IPに接続されている場合は、例外的にIP prefix 0 (/0)を対象CIDRに設定できます。
> * NATインスタンスサブネットとNATインスタンスをゲートウェイとして使用するインスタンスのサブネットがルーティングテーブル1に一緒に接続されている場合
>     * NATインスタンスがFloating IPに接続されている場合、ルート対象CIDRにIP Prefix 0 (/0)を設定できます。
>     * 上の設定の他には、ルーティングテーブル1のルーティング設定にNATインスタンスをゲートウェイとして指定してはいけません。


## MS-SQL Instance
インスタンス作成完了後、RDP(リモートデスクトップププロトコル)を通じてインスタンスにアクセスします。
インスタンスにFloating IPが接続されている必要があり、セキュリティーグループでTCPポート3389(RDP)が許可されている必要があります。
**+ パスワード確認** ボタンをクリックし、インスタンス作成時に設定したキーペアを使用してパスワードを確認します。

![mssqlinstance_02_201812](https://static.toastoven.net/prod_ms_sql/mssqlinstance_02_201812_en.png)

**接続** ボタンをクリックし、.rdpファイルをダウンロードした後に、獲得したパスワードを使用してインスタンスに接続します。

### MS-SQLイメージ作成後の初期設定

#### 1. SQL認証モード設定

サーバーの基本認証モードが「Windows認証モード」になっています。
MS-SQLのデータベースアカウントを使用するためにSQL認証モードに変更する必要があります。

Microsoft SQL Server Management Studioを実行して、インスタンス名でオブジェクトに接続します。

![mssqlinstance_03_201812](https://static.toastoven.net/prod_ms_sql/mssqlinstance_03_201812_en.png)

1. オブジェクトを右クリックします。
2. メニューで **プロパティ**を選択します。
3. サーバープロパティウィンドウで **セキュリティー** メニューを選択します。
4. **サーバー認証** 方式を「SQL ServerおよびWindows認証モード」に変更します。

※ SQL認証モード設定後、適用するためにMS-SQLサービスを再起動する必要があります。

#### 2. MS-SQLサービスポート変更

MS-SQLのデフォルトのサービスポート1433は、広く認知されているポートなので、セキュリティー脆弱性になることがあります。
次のポートに変更することを推奨します。
※ Expressの場合、デフォルトのポートが指定されていません。

SQL Server構成管理者を実行します。

![mssqlinstance_04_201812](https://static.toastoven.net/prod_ms_sql/mssqlinstance_04_201812_en.png)

1. 左のチェンネルで **SQL Serverネットワーク構成**の下位項目 **MSSQLSERVERに対するプロトコル**を選択します。
2. プロトコル名の中から **TCP/IP**を右クリックします。
3. メニューで **プロパティ**を選択します。
4. **IPアドレス** タブをクリックします。
5. ドロップダウンメニューの中から **IP ALL**を選択し、別のポート番号に変更します。

※ MS-SQLサービスポート変更後、適用のためにMS-SQLサービスを再起動する必要があります。

#### 3. 外部からのMS-SQLデータベース接続許容設定

外部からMS-SQLデータベースに接続するために、 **Network > Security Group** でMS-SQLサービスポートをSecurity Groupsに追加する必要があります。
Security Groupsに追加する時、接続を許可するMS-SQLサービスポート(基本ポート：1433)および遠隔IPを登録します。

### データボリューム割り当て

MS-SQLのデータ/ログファイル(MDF/LDF)、バックアップファイルは別途のBlock Storageの使用を推奨します。
Block Storageを作成するには、**Compute > Instance > Block Storage** タブで + Block Storage作成ボタンをクリックします。

![mssqlinstance_05_201812](https://static.toastoven.net/prod_ms_sql/mssqlinstance_05_201812_en.png)

Block Storage作成時、Volumeタイプは性能を考慮して「汎用SSD」の使用を推奨します。

![mssqlinstance_06_201812](https://static.toastoven.net/prod_ms_sql/mssqlinstance_06_201812_en.png)

Block Storage作成完了後、Storageを選択し、 **接続管理** ボタンをクリックしてインスタンスに接続します。

<br/>

RDPでインスタンスに接続し、 **コンピュータ管理**を実行して **保存場所 > ディスクの管理**に移動します。

![mssqlinstance_07_201812](https://static.toastoven.net/prod_ms_sql/mssqlinstance_07_201812_en.png)

接続されたBlock Storageが探知されたことを確認できます。使用するには先にディスク初期化を実行する必要があります。
1. **ディスク1** ブロックを右クリックした後、**ディスク初期化**をクリックします。
2. パーティション形式選択後、 **確認** ボタンをクリックします。

<br/>

初期化完了後、ディスクボリュームを作成します。

![mssqlinstance_08_201812](https://static.toastoven.net/prod_ms_sql/mssqlinstance_08_201812_en.png)

割り当てられていないディスクを右クリックし、 **新しいシンプルボリューム**をクリックして新しいシンプルボリュームウィザードを進行します。

<br/>

Microsoft SQL Server Management Studioサーバープロパティのデータベース設定で、データベース基本位置を作成したボリュームのディレクトリに変更します。

![mssqlinstance_09_201812](https://static.toastoven.net/prod_ms_sql/mssqlinstance_09_201812_en.png)

※ MS-SQLデータベース基本位置の変更後、適用のためにMS-SQLサービスを再起動する必要があります。

### MS-SQLサービス再起動
MS-SQLの設定変更時、MS-SQLサービスの再起動が必要な場合があります。
変更設定を適用するためにMS-SQLサービスを再起動します。

SQL Server構成管理者の **SQL Server構成管理者(ローカル) > SQL Serverサービス > SQL Server(MSSQLSERVER)** を選択後、右クリックして表示されるメニューにある「再起動」からMS-SQLサービスを再起動します。

![mssqlinstance_10_201812](https://static.toastoven.net/prod_ms_sql/mssqlinstance_10_201812_en.png)

### MS-SQLサービス自動実行確認/設定
MS-SQLのサービスが、OS起動時に自動で起動するように設定されているかを確認します。

SQL Server構成管理者のSQL Server構成管理者(ローカル) > SQL Serverサービスで「起動モード」を確認できます。

![mssqlinstance_11_201812](https://static.toastoven.net/prod_ms_sql/mssqlinstance_11_201812_en.png)

**SQL SERVER (MSSSQLSERVER)** および **SQL Serverエージェント(MSSQLSERVER)** などのサービス起動モードが **自動**ではない場合：
1. サービスを右クリックした後、 **プロパティ**を選択します。
2. **サービス**タブで **General > 起動モード**を **自動**に変更します。

> [参考]
> MS-SQL Instanceのリリース状況は、[インスタンスリリースノート](/Compute/Compute/ko/release-notes/)を参照してください。


## MySQL Instance
### MySQL起動/停止方法

```
#mysqlサービス起動
shell> service mysqld start

#mysqlサービス停止
shell> service mysqld stop

#mysqlサービス再起動
shell> service mysqld restart
```

### MySQL接続

イメージ作成後、最初は下記のように接続します。

```
shell> mysql -uroot
```

### MySQLインスタンス作成後の初期設定

#### 1\.パスワード設定

初期インストール後、MySQL ROOTアカウントパスワードは指定されていません。したがってインストール後、すぐにパスワードを設定する必要があります。

```
mysql> ALTER USER USER() IDENTIFIED BY '新しいパスワード';
```

MySQL基本validate\_password\_policyは下記の通りです。

* validate\_password\_policy=MEDIUM
* 基本**8文字以上、数字、大文字、小文字、特殊文字**を含める必要がある

#### 2\.ポート(port)変更

提供されるイメージポートはMySQL基本ポートの3306です。セキュリティー上、ポートの変更を推奨します。

```
shell> vi /etc/my.cnf


#my.cnfファイルに使用するポートを明示します。

port =使用するポート名


#vi エディタ保存


#mysqlサービス再起動


shell> service mysqld restart


#変更されたポートに下記のように接続


shell> mysql -uroot -P[変更されたポート番号]
```

### my.cnf説明

my.cnfのデフォルトのパスは /etc/my.cnf で、NHN Cloud推奨変数(variable)が設定されています。内容は下記の通りです。

| 名前 | 説明 |
| --- | --- |
| default\_storage\_engine | 基本ストレージエンジン(stroage engine)を指定します。InnoDBが指定され、Online-DDLとトランザクション(transaction)を使用できます。 |
| expire\_logs\_days | binlog設定で、 ログを保存する日数を設定します。デフォルトで3日に指定されています。 |
| innodb\_log\_file\_size | トランザクション(transaction)のredo logを保存するログファイルのサイズを指定します。<br><br>実際の運営環境では256MB以上を推奨しており、現在512MBに設定されています。設定値を修正した時は、DBの再起動が必要です。 |
| innodb\_file\_per\_table | テーブルが削除されたりTRUNCATEされる時、テーブルスペースがOSにすぐに返却されます。 |
| innodb\_log\_files\_in\_group | innodb\_log\_fileファイルの個数を設定し、循環的\(circular\)に使用されます。最小2個以上で構成されます。 |
| log_timestamps | MySQL 5.7の基本log時間はUTCで表示されます。したがってログ時間をSYSTEMローカル時間に変更します。 |
| slow\_query\_log | slow\_query logオプションを使用します。 long\_query\_timeによる基本10秒以上のクエリーはslow\_query\_logに記録されます。 |
| sysdate-is-now | sysdateの場合、replicationでsysdate()を使用したSQL文は、複製時にマスターとスレーブの間の時間が異なる問題があり、sysdate()とnow()の関数を同一に適用します。 |

### MySQLディレクトリ説明

MySQLディレクトリおよびファイル説明は下記の通りです。

| 名前 | 説明 |
| --- | --- |
| my.cnf | /etc/my.cnf |
| DATADIR | MySQLデータファイルのパス  - /var/lib/mysql/ |
| ERROR_LOG | MySQL error_logファイルのパス  - /var/log/mysqld.log |
| SLOW_LOG | MySQL Slow Queryファイルのパス -  <span style="color:#333333">/var/lib/mysql/*slow.log</span> |


> MySQL Instanceのリリース状況は[インスタンスリリースノート](/Compute/Compute/ja/release-notes/)を参照してください。


## PostgreSQL Instance
### PostgreSQL開始/停止方法

```
#postgresqlサービス開始
shell> sudo systemctl start postgresql-13

#postgresqlサービス中止
shell> sudo systemctl stop postgresql-13

#postgresqlサービス再起動
shell> sudo systemctl restart postgresql-13
```

### PostgreSQL接続

イメージ作成後、最初は下記のように接続します。
<br>
```
#postgresにアカウント切り替え後、接続
shell> sudo su - postgres
shell> psql
```

### PostgreSQLインスタンス作成後、初期設定

#### 1\. ポート\(port\)変更

提供されるイメージポートはPostgreSQL基本ポート5432です。セキュリティ上、ポートの変更を推奨します。
<br>
```
shell> vi /var/lib/pgsql/13/data/postgresql.conf


#postgresql.confファイルに使用するポートを明記します。

port =使用するポート名


#viエディタ保存


#postgresqlサービス再起動

shell> sudo systemctl restart postgresql-13


#変更されたポートに下記のように接続

shell> psql -p[変更されたポート番号]
```

#### 2\. サーバーログタイムゾーン変更

サーバーログに記録される基本時間帯がUTCに設定されています。SYSTEMローカル時間と同じタイムゾーンに変更することを推奨します。
<br>
```
shell> vi /var/lib/pgsql/13/data/postgresql.conf


#postgresql.confファイルに使用するタイムゾーンを明記します。

log_timezone =使用するタイムゾーン


#viエディタ保存


#postgresqlサービス再起動

shell> sudo systemctl restart postgresql-13


#postgresql接続

shell> psql


#変更した設定を確認

postgres=# SHOW log_timezone;
```

#### 3\. publicスキーマ権限の削除

基本的にすべてのユーザーにpublicスキーマに対するCREATEおよびUSAGE権限を付与しているため、データベースに接続できるユーザーはpublicスキーマからオブジェクトを作成できます。すべてのユーザーがpublicスキーマからオブジェクトを作成できないように権限を削除すことを推奨します。
<br>
```
#postgresql接続

shell> psql


#権限削除コマンド実行

postgres=# REVOKE CREATE ON SCHEMA public FROM PUBLIC;
```

#### 4\. 遠隔接続許可

ローカルホスト以外の接続を許可するにはlisten_addresses変数とクライアント認証設定ファイルを変更する必要があります。
<br>
```
shell> vi /var/lib/pgsql/13/data/postgresql.conf


#postgresql.confファイルに許可するアドレスを明記します。
#IPv4アドレスを全て許可する場合0.0.0.0
#IPv6アドレスを全て許可する場合::
#すべてのアドレスを許可する場合 *

listen_addresses =許可するアドレス


#viエディタ保存


shell> vi /var/lib/pgsql/13/data/pg_hba.conf


#IPアドレス形式ごとにクライアント認証制御
#古いクライアントライブラリはscram-sha-256方式がサポートされていないため、md5に変更必要

# TYPE  DATABASE        USER            ADDRESS                 METHOD
# IPv4 local connections:
host    all             all             127.0.0.1/32            scram-sha-256
host    許可DB           許可ユーザー         許可アドレス                  scram-sha-256
# IPv6 local connections:
host    all             all             ::1/128                 scram-sha-256
host    許可DB           許可ユーザー         許可アドレス                  scram-sha-256


#postgresqlサービス再起動

shell> sudo systemctl restart postgresql-13
```

### PostgreSQLディレクトリ説明

PostgreSQLディレクトリおよびファイルの説明は下記のとおりです。

| 名前 | 説明 |
| --- | --- |
| postgresql.cnf | /var/lib/pgsql/{version}/data/postgresql.cnf |
| initdb.log | PostgreSQLデータベースクラスター作成log - /var/lib/pgsql/{version}/initdb.log |
| DATADIR | PostgreSQLデータファイルパス - /var/lib/pgsql/{version}/data/ |
| LOG | PostgreSQL logファイルパス - /var/lib/pgsql/{version}/data/log/\*.log |

## CUBRID Instance
### CUBRIDサービスの起動/停止方法

“cubrid” LinuxアカウントにログインしてCUBRIDサービスを次のように開始または終了できます。
```
# CUBRIDサービス/サーバーの起動
shell> sudo su - cubrid
shell> cubrid service start
shell> cubrid server start demodb

# CUBRIDサービス/サーバーの終了
shell> sudo su - cubrid
shell> cubrid server stop demodb
shell> cubrid service stop

# CUBRIDサービス/サーバーの再起動
shell> sudo su - cubrid
shell> cubrid server restart demodb
shell> cubrid service restart

# CUBRIDブローカーの開始/終了/再起動
shell> sudo su - cubrid
shell> cubrid broker start
shell> cubrid broker stop
shell> cubrid broker restart
```

### CUBRID接続

イメージ作成後、最初は以下のように接続します。
```
shell> sudo su - cubrid
shell> csql -u dba demodb@localhost
```

### CUBRIDインスタンス作成後の初期設定

#### 1\. パスワード設定

初期インストール後、CUBRID dbaアカウントのパスワードは指定されていません。そのため、インストール後に必ずパスワードを設定する必要があります。
```
shell> csql -u dba -c "ALTER USER dba PASSWORD 'new_password'" demodb@localhost
```

#### 2\. ブローカーポート\(port\)の変更

**query_editor**のブローカーポートはデフォルト値が**30000**に設定され、**broker1**のブローカーポートはデフォルト値が**33000**に設定されます。
セキュリティ上、ポートの変更を推奨します。

###### 1)ブローカーファイルの修正

以下のファイルを開き、以下のように変更するポートアドレスを入力します。
```
shell> vi /opt/cubrid/conf/cubrid_broker.conf

[%query_editor]
BROKER_PORT             =[変更するportアドレス]

[%BROKER1]
BROKER_PORT             =[変更するportアドレス]
```

###### 2)ブローカーの再起動

ポートの変更を適用するためにブローカーを再起動します。
```
shell> cubrid broker restart
```

#### 3\. マネージャサーバーポート\(port\)変更

マネージャサーバーポートはデフォルト値が **8001**に設定されます。 
セキュリティ上、ポート変更を推奨します。

###### 1)  cm.confファイルの修正

以下のファイルを開き、次のように変更するポートアドレスを入力します。
```
shell> vi /opt/cubrid/conf/cm.conf

cm_port =[変更するportアドレス]
```

###### 2)マネージャサーバーの再起動

ポートの変更を適用するためにマネージャを再起動します。
```
shell> cubrid manager stop
shell> cubrid manager start
```

### CUBRIDディレクトリの説明

CUBRIDディレクトリおよびファイルの説明は次のとおりです。

| 名前 | 説明 |
| --- | --- |
| database.txt | CUBRIDデータベース位置情報ファイルパス - /opt/cubrid/databases |
| CONF PATH | CUBRIDサーバー、ブローカー、マネージャ環境変数ファイルパス - /opt/cubrid/conf |
| LOG PATH | CUBRIDプロセスログファイルパス - /opt/cubrid/log |
| SQL\_LOG | CUBRID SQL Queryファイルパス /opt/cubrid/log/broker/sql\_log |
| ERROR\_LOG | CUBRID ERROR SQL Queryファイルパス - /opt/cubrid/log/broker/error\_log |
| SLOW\_LOG | CUBRID Slow Queryファイルパス - /opt/cubrid/log/broker/sql\_log |

#### cubrid.conf説明

サーバー設定用ファイルです。運営するデータベースのメモリ、同時ユーザー数に応じたスレッド数、ブローカーとサーバー間の通信ポートなどを設定可能です。

| 名前 | 説明 |
| --- | --- |
| service  | CUBRIDサービス開始時に自動的に開始するプロセスを登録するパラメータです。 <br>デフォルトでserver、broker、managerプロセスが登録されています。 |
| cubrid\_port\_id | マスタープロセスが使用するポートです。 |
| max\_clients | 1つデータベースサーバープロセスが同時に接続できるクライアントの最大数です。 |
| data\_buffer\_size | データベースサーバーがメモリ内にキャッシュするデータバッファのサイズを設定するためのパラメータです。 <br>必要なメモリサイズがシステムメモリの2/3以内になるように設定することを推奨します。 |

#### broker.confの説明

ブローカー設定ファイルです。運営するブローカーが使用するポート、応用サーバー(CAS)数、SQL LOGなどを設定可能です。

| 名前 | 説明 |
| --- | --- |
| BROKER\_PORT | ブローカーが使用するポートです。実際のJDBCなどのドライバーで表示されるポートは該当ブローカーのポートです。 |
| MAX\_NUM\_APPL\_SERVER | 該当ブローカーに同時接続できるCASの最大数を設定するパラメータです。 |
| MIN\_NUM\_APPL\_SERVER | 該当ブローカーへの接続リクエストがなくてもデフォルトで待機しているCASプロセスの最小数を設定するパラメータです。 |
| LOG\_DIR | SQLログが保存されるディレクトリを指定するパラメータです。 |
| ERROR\_LOG\_DIR | ブローカーのエラーログが保存されるディレクトリを指定するパラメータです。 |

#### cm.confの説明

CUBRIDマネージャ設定ファイルです。運営するマネージャサーバープロセスが使用するポート、モニタリング収集サイクルなどを設定可能です。

| 名前 | 説明 |
| --- | --- |
| cm\_port | マネージャサーバープロセスが使用するポートです。 |
| cm\_process\_monitor\_interval | モニタリング情報の収集サイクルです。 |
| support\_mon\_statistic | 累積モニタリングを使用するかどうかを設定するパラメータです。 |
| server\_long\_query\_time | サーバーの診断項目のうちslow\_query項目を設定する場合、何秒以上を遅いクエリと判別するかを決定するパラメータです。 |


## MariaDB Instance
### MariaDB 起動/停止方法

``` sh
# MariaDBサービスの開始
shell> sudo systemctl start mariadb.service

# MariaDBサービスの終了
shell> sudo systemctl stop mariadb.service

# MariaDBサービスの再起動
shell> sudo systemctl restart mariadb.service
```

### MariaDB接続

イメージ作成後、最初は以下のように接続します。

``` sh
shell> sudo mysql -u root
```

パスワード変更後は以下のように接続します。

``` sh
shell> mysql -u root -p
Enter password:
```

### MariaDBインスタンス作成後の初期設定

#### 1\. パスワード設定

初期インストール後、MariaDB rootアカウントパスワードは指定されていません。そのため、インストール後に必ずパスワードを設定する必要があります。

```
SET PASSWORD [FOR user] = password_option

MariaDB> SET PASSWORD = PASSWORD('パスワード');
```

#### 2\. ポート\(port\)の変更

初期インストール後のポートはMariaDBのデフォルトポートである3306です。セキュリティ上、ポートの変更を推奨します。

##### 1) `/etc/my.cnf.d/servfer.cnf`ファイルの修正

`/etc/my.cnf.d/server.cnf`ファイルを開き、[mariadb]の下に以下のように変更するポートアドレスを入力します。

```
shell> sudo vi /etc/my.cnf.d/server.cnf
```

```
[mariadb]
port=[変更するportアドレス]
```

##### 2)インスタンスの再起動
ポートの変更が適用されるようにインスタンスを再起動します。
```
sudo systemctl restart mariadb.service
```

## Tibero Instance

### Tibero Instance作成

#### 最小推奨仕様

- ルートブロックストレージ
    - 高速化のためにSSDを推奨し、root disk fullが発生しないように**50GB以上**に設定することを推奨します。

- 最小推奨仕様: 4vCore/8GB
    - **推奨仕様未満の場合、DBMSのインストールが制限される場合があります。**

#### 追加ブロックストレージ

- ルートボリューム以外の追加ボリュームを作成します。
    - TMI(Tibero Machine Image)は追加ボリューム150GBを必要とするため、**追加ブロックストレージ150G以上**を必ず設定する必要があります

### インスタンス接続

- インスタンスの作成が完了したら、SSHを使用してインスタンスにアクセスします。
- インスタンスにFloating IPが接続されていて、セキュリティグループでTCPポート22(SSH)が許可されている必要があります。
- SSHクライアントと設定したキーペアを利用してインスタンスに接続します。
- SSH接続の詳細については[SSH接続ガイド](https://docs.toast.com/ko/Compute/Instance/ko/overview/#linux)<span style="color:#313338">を参照してください。</span>

### TMIインストール


rootアカウントで /rootパスからdbcaコマンドを実行します。
```
$ ./dbca OS_ACCOUNT DB_NAME DB_CHARACTERSET DB_TYPE DB_PORT
```

| No | 項目 | 引数値 |
| :---: | --- | --- |
| 1 | OS\_ACCOUNT | Tiberoが動作するOSのアカウント |
| 2 | DB\_NAME | Tiberoで使用されるDB\_NAME(= SID) |
| 3 | DB\_CHARACTERSET | Tiberoで使用するDB文字コード |
| 4 | DB\_TYPE | Tibero Type指定(16vCore以下: SE/16vCore超過: CE) |
| 5 | DB\_PORT | Tiberoで使用するサービスIPのポート |

##### Tibero 7 Cloud Standard Edition
```
[centos@tiberoinstance ~]$ sudo su root
[root@tiberoinstance centos]# cd
[root@tiberoinstance ~]# pwd
/root
[root@tiberoinstance ~]# ./dbca nhncloud tiberotestdb utf8 SE 8639
```

##### Tibero 7 Cloud Enterprise Edition
```
[centos@tiberoinstance ~]$ sudo su root
[root@tiberoinstance centos]# cd
[root@tiberoinstance ~]# pwd
/root
[root@tiberoinstance ~]# ./dbca nhncloud tiberotestdb utf8 CE 8639
```

#### インストール完了

dbcaコマンドを実行すると進行状況が表示され、nomountモードで データベースが作成されます。所要時間は10分以下です。 
完了すると下記のように出力されます。

```
SQL>
System altered.

SQL>
System altered.

SQL> Disconnected.
[root@tiberoinstance ~]#
```


#### 動作確認とインストールログの確認

Tiberoが動作していることを確認します。

```
[root@tiberoinstance ~]# ps -ef |grep tbsvr
nhncloud  9886     1  1 14:14 ?        00:00:00 tbsvr          -t NORMAL -SVR_SID tiberotestdb
nhncloud  9888  9886  0 14:15 ?        00:00:00 tbsvr_MGWP     -t NORMAL -SVR_SID tiberotestdb
nhncloud  9889  9886 36 14:15 ?        00:00:21 tbsvr_FGWP000  -t NORMAL -SVR_SID tiberotestdb
nhncloud  9890  9886  0 14:15 ?        00:00:00 tbsvr_FGWP001  -t NORMAL -SVR_SID tiberotestdb
nhncloud  9891  9886  0 14:15 ?        00:00:00 tbsvr_FGWP002  -t NORMAL -SVR_SID tiberotestdb
nhncloud  9892  9886  0 14:15 ?        00:00:00 tbsvr_FGWP003  -t NORMAL -SVR_SID tiberotestdb
nhncloud  9893  9886  0 14:15 ?        00:00:00 tbsvr_FGWP004  -t NORMAL -SVR_SID tiberotestdb
nhncloud  9894  9886  0 14:15 ?        00:00:00 tbsvr_FGWP005  -t NORMAL -SVR_SID tiberotestdb
nhncloud  9895  9886  0 14:15 ?        00:00:00 tbsvr_FGWP006  -t NORMAL -SVR_SID tiberotestdb
nhncloud  9896  9886  0 14:15 ?        00:00:00 tbsvr_FGWP007  -t NORMAL -SVR_SID tiberotestdb
nhncloud  9897  9886  0 14:15 ?        00:00:00 tbsvr_FGWP008  -t NORMAL -SVR_SID tiberotestdb
nhncloud  9898  9886  0 14:15 ?        00:00:00 tbsvr_FGWP009  -t NORMAL -SVR_SID tiberotestdb
nhncloud  9899  9886  0 14:15 ?        00:00:00 tbsvr_PEWP000  -t NORMAL -SVR_SID tiberotestdb
nhncloud  9900  9886  0 14:15 ?        00:00:00 tbsvr_PEWP001  -t NORMAL -SVR_SID tiberotestdb
nhncloud  9901  9886  0 14:15 ?        00:00:00 tbsvr_PEWP002  -t NORMAL -SVR_SID tiberotestdb
nhncloud  9902  9886  0 14:15 ?        00:00:00 tbsvr_PEWP003  -t NORMAL -SVR_SID tiberotestdb
nhncloud  9903  9886  0 14:15 ?        00:00:00 tbsvr_PEWP004  -t NORMAL -SVR_SID tiberotestdb
nhncloud  9904  9886  0 14:15 ?        00:00:00 tbsvr_PEWP005  -t NORMAL -SVR_SID tiberotestdb
nhncloud  9905  9886  0 14:15 ?        00:00:00 tbsvr_AGNT     -t NORMAL -SVR_SID tiberotestdb
nhncloud  9906  9886  3 14:15 ?        00:00:01 tbsvr_DBWR     -t NORMAL -SVR_SID tiberotestdb
nhncloud  9907  9886  0 14:15 ?        00:00:00 tbsvr_RCWP     -t NORMAL -SVR_SID tiberotestdb
root     13517  8366  0 14:15 pts/0    00:00:00 grep --color=auto tbsvr
[root@tiberoinstance ~]#
```

インストールログは /root/.dbset.logで確認できます。

```
[root@tiberoinstance ~]# ls -alh
合計20K
dr-xr-x---.  4 root root  104 10月17 14:15 .
dr-xr-xr-x. 23 root root 4.0K 10月17 14:14 ..
-rw-r--r--.  1 root root   18 12月29  2013 .bash_logout
-rw-r--r--.  1 root root  176 12月29  2013 .bash_profile
-rw-r--r--.  1 root root  176 12月29  2013 .bashrc
-rw-r--r--   1 root root 3.6K 10月17 14:15 .dbset.log
drwxr-----   3 root root   19 10月17 14:13 .pki
drwx------   2 root root   29 10月17 14:04 .ssh
```

### Tibero接続


#### アカウント変更

dbcaコマンドで作成したOS\_ACCOUNTにログインします。


```
[root@tiberoinstance ~]# su - nhncloud
最終ログイン：1月13(木) 11:34:43 KST 2022日時pts/0

 #####   ###  ######    ###         #######  ###  ######   #######  ######   #######  #######  #######   #####   #######  ######   ######
#     #   #   #     #   ###            #      #   #     #  #        #     #  #     #     #     #        #     #     #     #     #  #     #
#         #   #     #   ###            #      #   #     #  #        #     #  #     #     #     #        #           #     #     #  #     #
 #####    #   #     #                  #      #   ######   #####    ######   #     #     #     #####     #####      #     #     #  ######
      #   #   #     #   ###            #      #   #     #  #        #   #    #     #     #     #              #     #     #     #  #     #
#     #   #   #     #   ###            #      #   #     #  #        #    #   #     #     #     #        #     #     #     #     #  #     #
 #####   ###  ######    ###            #     ###  ######   #######  #     #  #######     #     #######   #####      #     ######   ######

[nhncloud@tiberoinstance ~]$

```

#### 接続確認

```
[nhncloud@tiberoinstance ~]$ tbsql sys/tibero

tbSQL 7

TmaxTibero Corporation Copyright (c) 2020-. All rights reserved.

Connected to Tibero.

SQL> select * FROM v$instance;

INSTANCE_NUMBER INSTANCE_NAME
--------------- ----------------------------------------
DB_NAME
----------------------------------------
HOST_NAME                                                       PARALLEL
--------------------------------------------------------------- --------
   THREAD# VERSION
---------- --------
STARTUP_TIME
--------------------------------------------------------------------------------
STATUS           SHUTDOWN_PENDING
---------------- ----------------
TIP_FILE
--------------------------------------------------------------------------------
              0 tiberotestdb
tiberotestdb
tiberoinstance.novalocal                                      NO
         0 7
2023/10/17
NORMAL           NO
/db/tibero7/config/tiberotestdb.tip


1 row selected.

SQL>
```


### Tibero基本アカウント

Tiberoで提供する基本アカウントは次のとおりです。

| スキーマ | パスワード | 説明 |
| --- | --- | --- |
| sys | tibero | SYSTEMスキーマ |
| syscat | syscat | SYSTEMスキーマ |
| sysgis | sysgis | SYSTEMスキーマ |
| outln | outln | SYSTEMスキーマ |
| tibero | tmax | SAMPLEスキーマDBA権限 |
| tibero1 | tmax | SAMPLEスキーマDBA権限 |

* SYS：データベースの管理者タスクを実行します。
* SYSCAT：DATA DICTIONARY & CATALOGVIEWを作成します。
* SYSGIS：GIS関連テーブルの作成およびタスクを実行するアカウントです。
* OUTLN：同じSQLを実行するときに常に同じプランで実行できるように関連ヒントを保存するなどのタスクを実行します。
* TIBERO/TIBERO1：example userであり、DBA権限を持っています。

## Kafka Instance
> [参考]
> このガイドはKafka 3.3.1バージョンを基準に作成されました。
> 他のバージョンを使用する場合は、該当のバージョンに合わせて変更してください。
> インスタンスタイプはc1m2(CPU 1core、Memory 2GB)以上の仕様で作成してください。

### Zookeeper、Kafka broker起動/停止
```
# Zookeeper、Kafka broker起動(Zookeeperを先に起動)
shell> sudo systemctl start zookeeper.service
shell> sudo systemctl start kafka.service
# Zookeeper、Kafka broker終了(Kafka brokerを先に終了)
shell> sudo systemctl stop kafka.service
shell> sudo systemctl stop zookeeper.service
# Zookeeper, Kafka broker再起動
shell> sudo systemctl restart zookeeper.service
shell> sudo systemctl restart kafka.service
```

### Kafka Clusterインストール
- 必ず新規インスタンスにインストールします。
- インスタンスは3台以上、奇数で必要です。インスタンス1台でインストールスクリプトを実行します。
- インスタンス1台にkafka broker、zookeeper nodeが各1つずつ構成されます。
- インストールスクリプトを実行するインスタンスの ~ パスに他のインスタンスに接続する時に必要なキーペア(PEMファイル)が必要です。クラスタインスタンスのキーペアはすべて同じでなければなりません。
- デフォルトポートのインストールのみサポートします。ポートの変更が必要な場合はクラスタインストールを完了してから初期設定ガイドのポート変更を参考にして変更します。
- インスタンス間のKafka関連ポート通信のために、以下のセキュリティグループ設定を追加します。

セキュリティグループ設定
```
方向：受信
IPプロトコル： TCP
ポート： 22, 9092, 2181, 2888, 3888
```
Hostname、IPの確認方法
```
# Hostname確認
shell> hostname
# IP確認
コンソール画面
またはshell> hostname -i
```
Clusterインストールスクリプト実行例(上で確認したhostname、IPを入力)
```
shell> sh ~/.kafka_make_cluster.sh
Enter Cluster Node Count: 3
### 3 is odd number.
Enter Cluster's IP ( Cluster 1 ) : 10.0.0.1
Enter Cluster's HOST_NAME ( Cluster 1 ) : kafka1.novalocal
Enter Cluster's IP ( Cluster 2 ) : 10.0.0.2
Enter Cluster's HOST_NAME ( Cluster 2 ) : kafka2.novalocal
Enter Cluster's IP ( Cluster 3 ) : 10.0.0.3
Enter Cluster's HOST_NAME ( Cluster 3 ) : kafka3.novalocal
10.0.0.1 kafka1.novalocal
10.0.0.2 kafka2.novalocal
10.0.0.3 kafka3.novalocal
Check Cluster Node Info (y/n) y
Enter Pemkey's name: kafka.pem
ls: cannot access /tmp/kafka-logs: No such file or directory
ls: cannot access /tmp/zookeeper: No such file or directory
### kafka1.novalocal ( 10.0.0.1 ), Check if kafka is being used
### kafka1.novalocal ( 10.0.0.1 ), Store node information in the /etc/hosts directory.
### kafka1.novalocal ( 10.0.0.1 ), Modify zookeeper.properties.
### kafka1.novalocal ( 10.0.0.1 ), Modify server.properties.
ls: cannot access /tmp/kafka-logs: No such file or directory
ls: cannot access /tmp/zookeeper: No such file or directory
### kafka2.novalocal ( 10.0.0.2 ), Check if kafka is being used
### kafka2.novalocal ( 10.0.0.2 ), Store node information in the /etc/hosts directory.
### kafka2.novalocal ( 10.0.0.2 ), Modify zookeeper.properties.
### kafka2.novalocal ( 10.0.0.2 ), Modify server.properties.
ls: cannot access /tmp/kafka-logs: No such file or directory
ls: cannot access /tmp/zookeeper: No such file or directory
### kafka3.novalocal ( 10.0.0.3 ), Check if kafka is being used
### kafka3.novalocal ( 10.0.0.3 ), Store node information in the /etc/hosts directory.
### kafka3.novalocal ( 10.0.0.3 ), Modify zookeeper.properties.
### kafka3.novalocal ( 10.0.0.3 ), Modify server.properties.
### kafka1.novalocal ( 10.0.0.1 ), Start Zookeeper, Kafka.
### Zookeeper, Kafka process is running.
### kafka2.novalocal ( 10.0.0.2 ), Start Zookeeper, Kafka.
### Zookeeper, Kafka process is running.
### kafka3.novalocal ( 10.0.0.3 ), Start Zookeeper, Kafka.
### Zookeeper, Kafka process is running.
##### Cluster Installation Complete #####
```

### Kafkaインスタンス作成後の初期設定
#### ポート(port)変更
最初のインストール後、ポートはKafkaデフォルトポート9092、Zookeeperデフォルトポート2181です。セキュリティのためにポートを変更することを推奨します。

##### 1) ~/kafka/config/zookeeper.propertiesファイル修正
~/kafka/config/zookeeper.propertiesファイルを開いてclientPortに変更するZookeeper portを入力します。
```
shell> vi ~/kafka/config/zookeeper.properties
clientPort=変更するzookeeper port
```
##### 2) ~/kafka/config/server.propertiesファイル修正
~/kafka/config/server.propertiesファイルを開いてlistenersに変更するKafka portを入力します。

インスタンスIPの確認方法
```
コンソール画面のPrivate IP
またはshell> hostname -i
```
```
shell> vi ~/kafka/config/server.properties

# コメント解除
listeners=PLAINTEXT://インスタンスIP：変更するkafka port

# Zookeeperポート変更
zookeeper.connect=インスタンスIP：変更するzookeeper port
---> クラスタの場合、各インスタンスIPのZookeeper portを変更
```

##### 3) Zookeeper、Kafka brokerの再起動
```
shell> sudo systemctl stop kafka.service
shell> sudo systemctl stop zookeeper.service
shell> sudo systemctl start zookeeper.service
shell> sudo systemctl start kafka.service
```

##### 4) Zookeeper、Kafka port変更確認
変更されたポートが使用されていることを確認します。
```
shell> netstat -ntl | grep [Kafka port]
shell> netstat -ntl | grep [Zookeeper port]
```

### Kafkaトピックおよびデータ作成/使用

トピックの作成/照会
```
# インスタンスIP = Private IP / Kafka基本port = 9092
# トピック作成
shell> ~/kafka/bin/kafka-topics.sh --create --bootstrap-server [インスタンスIP]:[Kafka PORT] --topic kafka
# トピックリスト照会
shell> ~/kafka/bin/kafka-topics.sh --list --bootstrap-server [インスタンスIP]:[Kafka PORT]
# トピック詳細情報確認
shell> ~/kafka/bin/kafka-topics.sh --describe --bootstrap-server [インスタンスIP]:[Kafka PORT] --topic kafka
# トピック削除
shell> ~/kafka/bin/kafka-topics.sh --delete --bootstrap-server [インスタンスIP]:[Kafka PORT] --topic kafka
```
データ作成/使用
```
# producer起動
shell> ~/kafka/bin/kafka-console-producer.sh --broker-list [インスタンスIP]:[Kafka PORT] --topic kafka
# consumer起動
shell> ~/kafka/bin/kafka-console-consumer.sh --bootstrap-server [インスタンスIP]:[Kafka PORT] --from-beginning --topic kafka
```

## Redis Instance

### Redis起動/停止
```
# Redisサービスの起動
shell> sudo systemctl start redis

# Redisサービスの停止
shell> sudo systemctl stop redis

# Redisサービスの再起動
shell> sudo systemctl restart redis
```

### Redis接続
`redis-cli`コマンドを利用してRedisインスタンスに接続できます。
```
shell> redis-cli
```

### Redisインスタンス作成後の初期設定
Redisインスタンスの基本設定ファイルは`~/redis/redis.conf`です。変更が必要なパラメータの説明は次のとおりです。

#### bind
- 基本値：`127.0.0.1 -::1`
- 変更値：`<private ip> 127.0.0.1 -::1`

Redisが使用するipの値です。サーバー外部からRedisインスタンスへのアクセスを許可するには該当パラメータにprivate ipを追加する必要があります。 private ipは`hostname -I`コマンドで確認できます。

#### port
- 基本値：`6379`

ポートはRedisデフォルト値である6379です。セキュリティ上、ポートを変更することを推奨します。ポートを変更した後は、以下のコマンドでRedisに接続できます。

```
shell> redis-cli -p <新しいポート>
```

#### requirepass/masterauth
- 基本値：`nhncloud`

基本パスワードは`nhncloud`です。セキュリティ上、パスワードを変更することを推奨します。複製接続を使用する場合、`requirepass`と`masterauth`値を同時に変更する必要があります。

### 自動HA構成スクリプト
NHN CloudのRedisインスタンスは自動的にHA環境を構成するスクリプトを提供します。スクリプトは必ず**インストール直後の新規インスタンス**でのみ使用することができ、redis.confで設定値を変更した場合には使用できません。

スクリプトを使用するには次の設定が必ず必要です。

##### キーペアコピー
インストールスクリプトを実行するインスタンスに他のインスタンス接続に必要なキーペア(PEMファイル)が必要です。キーペアは次のようにコピーできます。
- centos
```
local> scp -i <キーペア>.pem <キーペア>.pem centos@<floating ip>:/home/centos/
```
- ubuntu
```
local> scp -i <キーペア>.pem <キーペア>.pem ubuntu@<floating ip>:/home/ubuntu/
```
作成したインスタンスのキーペアは、すべて同じである必要があります。

##### セキュリティグループ設定
Redisインスタンス間の通信のためにセキュリティグループ(**Network** > **Security Groups**)設定が必要です。以下のルールでセキュリティグループを作成し、Redisインスタンスに適用してください。

| 方向 | IPプロトコル| ポート範囲| Ether| 遠隔|
| --- | --- | --- | --- | --- |
| 受信|TCP | 6379| IPv4| インスタンスIP(CIDR)|
| 受信|TCP | 16379| IPv4| インスタンスIP(CIDR)|
| 受信|TCP | 26379| IPv4| インスタンスIP(CIDR)|

#### Sentinel自動構成
Sentinel構成のために3つのRedisインスタンスが必要です。マスターとして使用するインスタンスにキーペアをコピーし、以下のようにスクリプトを実行してください。

```
shell> sh .redis_make_sentinel.sh
```

その後、マスターとレプリカのprivate IPを順番に入力します。各インスタンスのprivate IPは`hostname -I`コマンドで確認できます。

```
shell> sh .redis_make_sentinel.sh
Enter Master's IP: 192.168.0.33
Enter Replica-1's IP: 192.168.0.27
Enter Replica-2's IP: 192.168.0.97
```

コピーしたキーペアのファイル名を入力します。
```
shell> Enter Pemkey's name: <キーペア>.pem
```

#### Cluster自動構成
Cluster構成のために6つのRedisインスタンスが必要です。マスターとして使用するインスタンスにキーペアをコピーし、以下のようにスクリプトを実行してください。

```
shell> sh .redis_make_cluster.sh
```

その後、クラスタに使用するRedisインスタンスのprivate IPを順番に入力します。各インスタンスのprivate IPは`hostname -I`コマンドで確認できます。

```
shell> sh .redis_make_cluster.sh
Enter cluster-1'IP:  192.168.0.79
Enter cluster-2'IP: 192.168.0.10
Enter cluster-3'IP: 192.168.0.33
Enter cluster-4'IP:  192.168.0.116
Enter cluster-5'IP:  192.168.0.91
Enter cluster-6'IP:  192.168.0.32
```

コピーしたキーペアのファイル名を入力します。

```
shell> Enter Pemkey's name: <キーペア>.pem
```

`yes`を入力し、クラスタ構成を完了します。
```
>>> Performing hash slots allocation on 6 nodes...
Master[0] -> Slots 0 - 5460
Master[1] -> Slots 5461 - 10922
Master[2] -> Slots 10923 - 16383
Adding replica 192.168.0.91:6379 to 192.168.0.79:6379
Adding replica 192.168.0.32:6379 to 192.168.0.10:6379
Adding replica 192.168.0.116:6379 to 192.168.0.33:6379
M: 0a6ee5bf24141f0058c403d8cc42b349cdc09752 192.168.0.79:6379
   slots:[0-5460] (5461 slots) master
M: b5d078bd7b30ddef650d9a7fa9735e7648efc86f 192.168.0.10:6379
   slots:[5461-10922] (5462 slots) master
M: 0da9b78108b6581bdb90002cbdde3506e9173dd8 192.168.0.33:6379
   slots:[10923-16383] (5461 slots) master
S: 078b4ce014a52588e23577b3fc2dabf408723d68 192.168.0.116:6379
   replicates 0da9b78108b6581bdb90002cbdde3506e9173dd8
S: caaae4ebd3584c0481205e472d6bd0f9dc5c574e 192.168.0.91:6379
   replicates 0a6ee5bf24141f0058c403d8cc42b349cdc09752
S: ab2aa9e37cee48ef8e4237fd63e8301d81193818 192.168.0.32:6379
   replicates b5d078bd7b30ddef650d9a7fa9735e7648efc86f
Can I set the above configuration? (type 'yes' to accept):
```

```
[OK] All nodes agree about slots configuration.
>>> Check for open slots...
>>> Check slots coverage...
[OK] All 16384 slots covered.
```
