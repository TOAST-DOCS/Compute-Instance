## Compute > Instance > 問題解決ガイド

NHN Cloudの使用時に問題が発生した場合、それを解決する方法を説明します。

<h3>現在NHN Cloudで基本提供するOSバージョン以外のバージョンを使用したいです。個人イメージをアップロードして使用できますか？</h3>

NHN Cloudで提供するOSバージョンのみ利用できます。個人イメージのアップロードはサポートしません。
個人OSイメージを使用するにはNHN Cloudが提供するイメージでインスタンスを作成した後、**イメージ作成**機能を利用してください。
<br>

<h3>インスタンスに接続すると、「Permissions 0644 for '/Users/username/.ssh/your-key.pem' are too open.」というメッセージが表示されて接続できません。</h3>

インスタンス接続に使用するキーペアの秘密鍵(PEMキー)の権限が正しくないため生じる問題です。
下記のように秘密鍵ファイルの権限を調整します。

    $ chmod 600 your-key.pem

<br>

<h3>CentOSインスタンスでどうやってroot権限を取得しますか？</h3>

CentOSインスタンスでroot権限を取得するには、次のように`sudo`コマンドを利用します。

    $ sudo su

<br>

<h3>個人イメージを作り、インスタンスを作成して起動しましたがマウント(mount)エラーが発生します。</h3>

2つ以上のブロックストレージを使用するインスタンスでイメージを作成し、作成したイメージでインスタンスを作って起動すると上記のような問題が発生します。

2つ以上のブロックストレージを使用するインスタンスは、基本ディスク以外のディスクを`/etc/fstab`ファイルに設定します。イメージ作成時にこのファイルも複製されるため、新しいインスタンスが起動する時、`/etc/fstab`ファイルが参照するブロックストレージがなくてマウントエラーが発生します。

この問題を解消するには、`/etc/fstab`ファイルで基本ディスク以外のブロックストレージ設定をコメント処理してイメージを作成する必要があります。
<br>

<h3>SSH接続が遅すぎます</h3>

インスタンスが属すセキュリティグループの送信部分でDNSをブロックした場合に発生します。DNS送信ができるようにセキュリティグループを調整します。
<br>

<h3>「Could not resolve the host」メッセージが表示され、yumなどを使用できません。</h3>

インスタンスが属すセキュリティグループの送信部分でDNSをブロックした場合に発生します。DNS送信ができるようにセキュリティグループを調整します。
<br>

<h3>CentOS 6.xインスタンスでパッケージアップデートに失敗します。</h3>

次のように`yum repository`ファイルを修正して使用します。
公式サポートが終了したOSは追加アップデートがサポートされないため、上位バージョンOSの使用を推奨します。
```
$ sudo vi /etc/yum.repos.d/CentOS-Base.repo
...
[base]
name=CentOS-$releasever - Base
#mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=os&infra=$infra&cc=$cc
#baseurl=http://mirror.centos.org/centos/$releasever/os/$basearch/
baseurl=https://vault.centos.org/6.10/os/$basearch/
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6

#released updates
[updates]
name=CentOS-$releasever - Updates
#mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=updates&infra=$infra&cc=$cc
#baseurl=http://mirror.centos.org/centos/$releasever/updates/$basearch/
baseurl=https://vault.centos.org/6.10/updates/$basearch/
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6

#additional packages that may be useful
[extras]
name=CentOS-$releasever - Extras
#mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=extras&infra=$infra&cc=$cc
#baseurl=http://mirror.centos.org/centos/$releasever/extras/$basearch/
baseurl=https://vault.centos.org/6.10/extras/$basearch/
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6
...

$ sudo yum clean all
$ sudo yum repolist
```
