## Compute > Instance > 特化したインスタンス使用ガイド > MySQL Instanceガイド
## MySQL version

MySQL versionは次の2種類が提供されます。

* MySQL Community Server 5.6.38
    * mysql-community-server-5.6.38-2.el6.x86_64
* MySQL Community Server 5.7.20
    * mysql-community-server-5.7.20-1.el6.x86_64

## MySQL開始/停止方法

```
#mysqlサービス開始
shell> service mysqld start

#mysqlサービス中止
shell> service mysqld stop

#mysqlサービス再起動
shell> service mysqld restart
```

## MySQL接続

イメージ生成後、最初は下記のように接続します。

```
shell> mysql -uroot
```

## MySQLイメージ生成後、初期設定

### 1\.パスワード変更

初期インストール後、 MySQL ROOTアカウントパスワードは指定されていません。したがってインストール後、必ずすぐにパスワードの変更をする必要があります。

* MySQL 5.6バージョンパスワード変更

```
SET PASSWORD [FOR user] = password_option

mysql> set password=password('パスワード');
```

* MySQL 5.7バージョンパスワード変更

```
ALTER USER USER() IDENTIFIED BY 'auth_string';

mysql> ALTER USER 'root'@'localhost' IDENTIFIED BY '新しいパスワード';
```

MySQL基本validate\_password\_policyは下記のとおりです。

* validate\_password\_policy=MEDIUM
* 基本**8文字以上で、アルファベット(大文字/小文字)、数字、特殊文字**を含める必要があります。

### 2\. ポート(port)変更

提供されるイメージポートはMySQLの基本ポートである3306です。セキュリティー上、ポート変更を推奨します。

```
shell> vi /etc/my.cnf


#my.cnfファイルに使用するポートを明示します。

port =使用するポート名


#viエディタ保存


#mysqlサービス再起動


shell> service mysqld restart


#変更されたポートに下記のように接続


shell> mysql -uroot -P[変更されたポート番号]
```

## my.cnf説明

my.cnfの基本パスは /etc/my.cnfで、NHN Cloud推奨変数(variable)が設定されており、内容は以下のとおりです。

| 名前 | 説明 |
| --- | --- |
| default\_storage\_engine | 基本ストレージエンジン(stroage engine)を指定します。 InnoDBで指定され、 Online-DDLとトランザクション(transaction)を使用できます。 |
| expire\_logs\_days | binlog設定で蓄積されるログ保存日を設定します。基本3日に指定されています。 |
| innodb\_log\_file\_size | トランザクション(transaction)のredo logを保存するログファイルのサイズを指定します。 <br><br>実際の運営環境では256MB以上を推奨します。現在512MBに設定されています。設定値を修正した際は、DB再起動が必要です。 |
| innodb\_file\_per\_table | テーブルが削除またはTRUNCATEされる時、テーブルのスペースがOSにすぐに返還されます。 |
| innodb\_log\_files\_in\_group | innodb\_log\_fileファイルの個数を設定し、循環的\(circular\)に使われます。最低2個以上で構成されます。 |
| log_timestamps | MySQL 5.7の基本log時間はUTCで表示されます。したがってログ時間をSYSTEMローカル時間に変更します。 |
| slow\_query\_log | slow\_query logオプションを使用します。 long\_query\_timeに基づく基本10秒以上のクエリーはslow\_query\_logに記録されます。 |
| sysdate-is-now | sysdateの場合、 replicationでsysdate()使用されたSQL文は複製時、マスターとスレーブ間の時間が変わる問題があり、sysdate()とnow()の関数を同一に適用します。 |

## MySQLディレクトリ説明

MySQLディレクトリおよびファイルの説明は下記のとおりです。

| 名前 | 説明 |
| --- | --- |
| my.cnf | /etc/my.cnf |
| DATADIR | MySQLデータファイルパス - /var/lib/mysql/ |
| ERROR_LOG | MySQL error_logファイルパス - /var/log/mysqld.log |
| SLOW_LOG | MySQL Slow Queryファイルパス -  <span style="color:#333333">/var/lib/mysql/*slow.log</span> |

