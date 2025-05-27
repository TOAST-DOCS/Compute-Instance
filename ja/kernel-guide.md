## Compute > Instance > カーネルバージョンアップガイド

> [注意] 
> カーネルアップデートの際、OSが破損したり、起動に失敗する可能性があり、その結果に対する責任はユーザーにあります。

## Rocky Linux 8

### カーネルバージョン確認

現在インストールされているカーネルバージョンを確認します。

```
[root@rocky810 ~]# uname -r
4.18.0-553.8.1.el8_10.x86_64
```

### 基本リポジトリの設定

システムアーキテクチャとRocky Linuxのバージョンに合った基本リポジトリを変更します。

```
[baseos]
name=Rocky Linux $releasever - BaseOS
mirrorlist=https://mirrors.rockylinux.org/mirrorlist?arch=$basearch&repo=BaseOS-$releasever
#baseurl=http://dl.rockylinux.org/$contentdir/$releasever/BaseOS/$basearch/os/
gpgcheck=1
enabled=1
countme=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-rockyofficial
```

* **name**
  * リポジトリの名前であり、変更しても問題ありません。
* **mirrorlist**
  * パッケージをダウンロードできるミラーサーバーリストを提供するURLを指定します。
  * システムのアーキテクチャ(`$basearch`)とRocky Linuxバージョン(`$releasever`)に合ったミラーサーバのリストを取得します。
* **baseurl**
  * パッケージをダウンロードする基本URLを指定します。このURLは単一のサーバーを指し、そのサーバーから直接パッケージをダウンロードします。
* **gpgcheck**
  * GPG(GNU Privacy Guard)キーを含むリポジトリのURLまたはパスを設定します。GPGキーは、rpmパッケージを認証するために使用する暗号化署名です。

> [参考]
> **mirrorlist**と **baseurl**の両方が設定されている場合は、**mirrorlist**が優先的に適用され、**baseurl**は代替オプションとして動作します。

### アップデート前のキャッシュの削除

既存のダウンロードされたパッケージのメタデータが保存されているキャッシュを削除します。

```
[root@rocky810 ~]# rm -rf /var/cache/dnf
```

### カーネルインストール

#### バージョンを指定してカーネルをインストール

> [参考]
> Rocky Linuxパッケージ8.8以下のバージョンはサポートしません。

```
[root@rocky810 ~]# dnf --releasever=8.10 list kernel*

Installed Packages
kernel.x86_64                                                                                4.18.0-477.27.1.el8_8                                                             @baseos
kernel-core.x86_64                                                                           4.18.0-477.27.1.el8_8                                                             @baseos
kernel-modules.x86_64                                                                        4.18.0-477.27.1.el8_8                                                             @baseos
kernel-tools.x86_64                                                                          4.18.0-477.27.1.el8_8                                                             @baseos
kernel-tools-libs.x86_64                                                                     4.18.0-477.27.1.el8_8                                                             @baseos
Available Packages
kernel.x86_64                                                                                4.18.0-553.16.1.el8_10                                                            baseos
kernel-abi-stablelists.noarch                                                                4.18.0-553.16.1.el8_10                                                            baseos
kernel-core.x86_64                                                                           4.18.0-553.16.1.el8_10                                                            baseos
kernel-cross-headers.x86_64                                                                  4.18.0-553.16.1.el8_10                                                            baseos
kernel-debug.x86_64                                                                          4.18.0-553.16.1.el8_10                                                            baseos
kernel-debug-core.x86_64                                                                     4.18.0-553.16.1.el8_10                                                            baseos
kernel-debug-devel.x86_64                                                                    4.18.0-553.16.1.el8_10                                                            baseos
kernel-debug-modules.x86_64                                                                  4.18.0-553.16.1.el8_10                                                            baseos
kernel-debug-modules-extra.x86_64                                                            4.18.0-553.16.1.el8_10                                                            baseos
kernel-devel.x86_64                                                                          4.18.0-553.16.1.el8_10                                                            baseos
kernel-doc.noarch                                                                            4.18.0-553.16.1.el8_10                                                            baseos
kernel-headers.x86_64                                                                        4.18.0-553.16.1.el8_10                                                            baseos
kernel-modules.x86_64                                                                        4.18.0-553.16.1.el8_10                                                            baseos
kernel-modules-extra.x86_64                                                                  4.18.0-553.16.1.el8_10                                                            baseos
kernel-rpm-macros.noarch                                                                     131-1.el8                                                                         appstream
kernel-tools.x86_64                                                                          4.18.0-553.16.1.el8_10                                                            baseos
kernel-tools-libs.x86_64                                                                     4.18.0-553.16.1.el8_10                                                            baseos
kernelshark.x86_64
```

#### バージョンを指定せずにカーネルをインストール
バージョンを指定しない場合、majorバージョンの最新バージョンを基準にパッケージを検索します。

```
[root@rocky810 ~]# dnf list kernel*

kernel.x86_64                                                                                4.18.0-477.27.1.el8_8                                                             @baseos
kernel-core.x86_64                                                                           4.18.0-477.27.1.el8_8                                                             @baseos
kernel-modules.x86_64                                                                        4.18.0-477.27.1.el8_8                                                             @baseos
kernel-tools.x86_64                                                                          4.18.0-477.27.1.el8_8                                                             @baseos
kernel-tools-libs.x86_64                                                                     4.18.0-477.27.1.el8_8                                                             @baseos
Available Packages
kernel.x86_64                                                                                4.18.0-553.16.1.el8_10                                                            baseos
kernel-abi-stablelists.noarch                                                                4.18.0-553.16.1.el8_10                                                            baseos
kernel-core.x86_64                                                                           4.18.0-553.16.1.el8_10                                                            baseos
kernel-cross-headers.x86_64                                                                  4.18.0-553.16.1.el8_10                                                            baseos
kernel-debug.x86_64                                                                          4.18.0-553.16.1.el8_10                                                            baseos
kernel-debug-core.x86_64                                                                     4.18.0-553.16.1.el8_10                                                            baseos
kernel-debug-devel.x86_64                                                                    4.18.0-553.16.1.el8_10                                                            baseos
kernel-debug-modules.x86_64                                                                  4.18.0-553.16.1.el8_10                                                            baseos
kernel-debug-modules-extra.x86_64                                                            4.18.0-553.16.1.el8_10                                                            baseos
kernel-devel.x86_64                                                                          4.18.0-553.16.1.el8_10                                                            baseos
kernel-doc.noarch                                                                            4.18.0-553.16.1.el8_10                                                            baseos
kernel-headers.x86_64                                                                        4.18.0-553.16.1.el8_10                                                            baseos
kernel-modules.x86_64                                                                        4.18.0-553.16.1.el8_10                                                            baseos
kernel-modules-extra.x86_64                                                                  4.18.0-553.16.1.el8_10                                                            baseos
kernel-rpm-macros.noarch                                                                     131-1.el8                                                                         appstream
kernel-tools.x86_64                                                                          4.18.0-553.16.1.el8_10                                                            baseos
kernel-tools-libs.x86_64                                                                     4.18.0-553.16.1.el8_10                                                            baseos
kernelshark.x86_64
```


#### 最新カーネルインストール
バージョンを指定しない場合、最新バージョンでインストールします。

カーネルをインストールすると依存関係パッケージである**kernel-core**と **kernel-modules**も一緒にインストールします。

```
[root@rocky810 ~]# dnf install kernel
Last metadata expiration check: 0:21:59 ago on Tue 10 Sep 2024 04:44:04 PM KST.
Dependencies resolved.
========================================================================================================================================================================================
 Package                                       Architecture                          Version                                                Repository                             Size
========================================================================================================================================================================================
Installing:
 kernel                                        x86_64                                4.18.0-553.16.1.el8_10                                 baseos                                 10 M
Installing dependencies:
 kernel-core                                   x86_64                                4.18.0-553.16.1.el8_10                                 baseos                                 43 M
 kernel-modules                                x86_64                                4.18.0-553.16.1.el8_10                                 baseos                                 36 M

Transaction Summary
========================================================================================================================================================================================
Install  3 Packages

Total download size: 90 M
Installed size: 96 M
Is this ok [y/N]: y

...
...
...

Installed:
  kernel-4.18.0-553.16.1.el8_10.x86_64            kernel-core-4.18.0-553.16.1.el8_10.x86_64            kernel-modules-4.18.0-553.16.1.el8_10.x86_64

Complete!
```


#### パッケージのインストール確認

カーネルパッケージが正常にインストールされたか確認します。

```
[root@rocky810 ~]# dnf list installed | grep -iE "kernel.*4.18.0-553.16"
kernel.x86_64                         4.18.0-553.16.1.el8_10                    @baseos
kernel-core.x86_64                    4.18.0-553.16.1.el8_10                    @baseos
kernel-modules.x86_64                 4.18.0-553.16.1.el8_10                    @baseos
```

### OS再起動

カーネルアップデートを適用するためにOSを再起動します。

```
[root@rocky810 ~]# sync; reboot
```

### <span style="color:#e11d21;">**[選択]**</span> GRUB2ブートローダの設定ファイル作成
システムのブートメニューをアップデートして、新しくインストールされたカーネルやその他のブート項目を反映します。

dnf, yumは自動的にGRUB2設定ファイルをアップデートします。

```
[root@rocky810 ~]# grub2-mkconfig -o /etc/grub2.cfg
```

#### カーネルアップデート確認

カーネルバージョンが正常にアップデートされたか確認します。

```
[root@rocky810 ~]# uname -r
4.18.0-553.16.1.el8_10.x86_64
```

### カーネル起動順序の変更

複数のカーネルがインストールされている場合、希望のカーネルで起動できるように起動順序を変更します。

#### Rocky 8.10未満のバージョン

##### 基本カーネルの確認

現在基本設定されているカーネルを確認します。

```
[root@rocky810 ~]# grubby --default-kernel
/boot/vmlinuz-4.18.0-553.16.1.el8_10.x86_64
```

##### 現在インストールされているカーネルリスト

現在インストールされているカーネルリストを確認します。

```
[root@rocky810 ~]# grubby --info=ALL | grep ^kernel | grep -v rescue
kernel="/boot/vmlinuz-4.18.0-553.el8_10.x86_64"
kernel="/boot/vmlinuz-4.18.0-553.16.1.el8_10.x86_64"
kernel="/boot/vmlinuz-4.18.0-553.8.1.el8_10.x86_64"
```

##### 基本カーネル変更

現在インストールされているカーネルリストの1つを選択して基本カーネルを変更します。

```
[root@rocky810 ~]# grubby --set-default="/boot/vmlinuz-4.18.0-553.8.1.el8_10.x86_64"
The default is /boot/loader/entries/ea5b6e1e7bc09da25505ebb3a26a8bf4-4.18.0-553.8.1.el8_10.x86_64.conf with index 4 and kernel /boot/vmlinuz-4.18.0-553.8.1.el8_10.x86_64
[root@rocky810 ~]# grubby --default-kernel
/boot/vmlinuz-4.18.0-553.8.1.el8_10.x86_64
```

##### OS再起動

起動順序の変更を適用するためにOSを再起動します。

```
[root@rocky810 ~]# sync; reboot
```

#### Rocky 8.10以上バージョン

現在Rocky 8.10公式イメージでgrubbyコマンドでカーネルを変更できない問題があるので、下記のシェルスクリプトを使います。

```bash
#!/bin/bash

result=1
kernel_list=(` rpm -qa | grep ^kernel-[0-9] | sort `)

echo -e "\n# Choose the kernel you want from the list below. #"
echo "------------------------------------------------"
for index in ${!kernel_list[@]}; do
    echo "Num: $index,  ${kernel_list[$index]}"
done
echo "------------------------------------------------"
read -p "index: " kernel_index

for index in ${!kernel_list[@]}; do
    if [[ "$index" -eq "$kernel_index" ]]; then
        sed -i "s/GRUB_DEFAULT=[^ ]*/GRUB_DEFAULT=$kernel_index/" /etc/default/grub && result=0
        grub2-mkconfig -o /etc/grub2.cfg
    fi
done

if [[ "$result" -eq "1" ]]; then
    echo "[Error] Please choose only from the indexes in the list"
fi
```

##### スクリプトの使い方

シェルスクリプト実行後、出力されるカーネルリストの中で起動するカーネルの番号を入力します。

```
[root@rocky810 ~]# bash kernel_select.sh

# Choose the kernel you want from the list below. #
------------------------------------------------
Num: 0,  kernel-4.18.0-553.16.1.el8_10.x86_64
Num: 1,  kernel-4.18.0-553.8.1.el8_10.x86_64
Num: 2,  kernel-4.18.0-553.el8_10.x86_64
------------------------------------------------
index: 0
Generating grub configuration file ...
done
```

##### OS再起動

起動順序の変更を適用するためにOSを再起動します。

```
[root@rocky810 ~]# sync; reboot
```

## Rocky Linux 9

### カーネルバージョン確認

現在インストールされているカーネルバージョンを確認します。

```
[rocky@rocky95 ~]$ uname -r
5.14.0-503.14.1.el9_5.x86_64
```

### 基本リポジトリの設定

システムアーキテクチャとRocky Linuxのバージョンに合った基本リポジトリを変更します。

```
[baseos]
name=Rocky Linux $releasever - BaseOS
mirrorlist=https://mirrors.rockylinux.org/mirrorlist?arch=$basearch&repo=BaseOS-$releasever$rltype
#baseurl=http://dl.rockylinux.org/$contentdir/$releasever/BaseOS/$basearch/os/
gpgcheck=1
enabled=1
countme=1
metadata_expire=6h
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-Rocky-9
```

* **name**
  * リポジトリの名前であり、変更しても問題ありません。
* **mirrorlist**
  * パッケージをダウンロードできるミラーサーバーリストを提供するURLを指定します。
  * システムのアーキテクチャ(`$basearch`)とRocky Linuxバージョン(`$releasever`)に合ったミラーサーバのリストを取得します。
* **baseurl**
  * パッケージをダウンロードする基本URLを指定します。このURLは単一のサーバーを指し、そのサーバーから直接パッケージをダウンロードします。
* **gpgcheck**
  * GPG(GNU Privacy Guard)キーを含むリポジトリのURLまたはパスを設定します。GPGキーは、rpmパッケージを認証するために使用する暗号化署名です。
* **enabled**
  * このリポジトリを有効にするかどうかを設定します。
  * 値が1の場合は有効、0の場合は無効です。
* **countme**
  * Rocky Linux使用統計を収集する機能です。
  * 値が1の場合、Rocky Linuxプロジェクトにどれだけのユーザーがいるか把握できます。
* **metadata_expire**
  * dnfがリポジトリのメタデータ(パッケージリストなど)を特定の時間(例では6時間)周期で更新するように設定します。
* **gpgkey**
  * パッケージ署名を検証するGPGキーファイルのパスです。

> [参考]
> **mirrorlist**と **baseurl**の両方が設定されている場合は、**mirrorlist**が優先的に適用され、**baseurl**は代替オプションとして動作します。

### アップデート前のキャッシュの削除

既存のダウンロードされたパッケージのメタデータが保存されているキャッシュを削除します。


```
[root@rocky95 ~]# rm -rf /var/cache/dnf
```
### カーネルインストール

#### バージョンを指定してカーネルをインストール

> [参考]
> 現在Rocky Linux 9パッケージは9.5バージョンのみ使用可能です。


```
[root@rocky95 ~]# dnf --releasever=9.5 list kernel*

Rocky Linux 9.5 - BaseOS                                                                                           1.7 MB/s | 2.3 MB     00:01
Rocky Linux 9.5 - AppStream                                                                                        7.2 MB/s | 8.6 MB     00:01
Rocky Linux 9.5 - Extras                                                                                            12 kB/s |  16 kB     00:01
Installed Packages
kernel.x86_64                                                              5.14.0-503.14.1.el9_5                                          @baseos
kernel.x86_64                                                              5.14.0-503.22.1.el9_5                                          @baseos
kernel-core.x86_64                                                         5.14.0-503.14.1.el9_5                                          @baseos
kernel-core.x86_64                                                         5.14.0-503.22.1.el9_5                                          @baseos
kernel-modules.x86_64                                                      5.14.0-503.14.1.el9_5                                          @baseos
kernel-modules.x86_64                                                      5.14.0-503.22.1.el9_5                                          @baseos
kernel-modules-core.x86_64                                                 5.14.0-503.14.1.el9_5                                          @baseos
kernel-modules-core.x86_64                                                 5.14.0-503.22.1.el9_5                                          @baseos
kernel-tools.x86_64                                                        5.14.0-503.22.1.el9_5                                          @baseos
kernel-tools-libs.x86_64                                                   5.14.0-503.22.1.el9_5                                          @baseos
Available Packages
kernel.x86_64                                                              5.14.0-503.23.2.el9_5                                          baseos
kernel-abi-stablelists.noarch                                              5.14.0-503.23.2.el9_5                                          baseos
kernel-core.x86_64                                                         5.14.0-503.23.2.el9_5                                          baseos
kernel-debug.x86_64                                                        5.14.0-503.23.2.el9_5                                          baseos
kernel-debug-core.x86_64                                                   5.14.0-503.23.2.el9_5                                          baseos
kernel-debug-devel.x86_64                                                  5.14.0-503.23.2.el9_5                                          appstream
kernel-debug-devel-matched.x86_64                                          5.14.0-503.23.2.el9_5                                          appstream
kernel-debug-modules.x86_64                                                5.14.0-503.23.2.el9_5                                          baseos
kernel-debug-modules-core.x86_64                                           5.14.0-503.23.2.el9_5                                          baseos
kernel-debug-modules-extra.x86_64                                          5.14.0-503.23.2.el9_5                                          baseos
kernel-debug-uki-virt.x86_64                                               5.14.0-503.23.2.el9_5                                          baseos
kernel-devel.x86_64                                                        5.14.0-503.23.2.el9_5                                          appstream
kernel-devel-matched.x86_64                                                5.14.0-503.23.2.el9_5                                          appstream
kernel-doc.noarch                                                          5.14.0-503.23.2.el9_5                                          appstream
kernel-headers.x86_64                                                      5.14.0-503.23.2.el9_5                                          appstream
kernel-modules.x86_64                                                      5.14.0-503.23.2.el9_5                                          baseos
kernel-modules-core.x86_64                                                 5.14.0-503.23.2.el9_5                                          baseos
kernel-modules-extra.x86_64                                                5.14.0-503.23.2.el9_5                                          baseos
kernel-rpm-macros.noarch                                                   185-13.el9                                                     appstream
kernel-srpm-macros.noarch                                                  1.0-13.el9                                                     appstream
kernel-tools.x86_64                                                        5.14.0-503.23.2.el9_5                                          baseos
kernel-tools-libs.x86_64                                                   5.14.0-503.23.2.el9_5                                          baseos
kernel-uki-virt.x86_64                                                     5.14.0-503.23.2.el9_5                                          baseos
kernel-uki-virt-addons.x86_64                                              5.14.0-503.23.2.el9_5                                          baseos
kernelshark.x86_64                                                         1:1.2-10.el9                                                   appstream
```

#### バージョンを指定せずにカーネルをインストール
バージョンを指定しない場合、majorバージョンの最新バージョンを基準にパッケージを検索します。

```
[root@rocky95 ~]# dnf list kernel*

Rocky Linux 9 - BaseOS                                                                                             2.2 MB/s | 2.3 MB     00:01
Rocky Linux 9 - AppStream                                                                                          7.8 MB/s | 8.6 MB     00:01
Rocky Linux 9 - Extras                                                                                              12 kB/s |  16 kB     00:01
Installed Packages
kernel.x86_64                                                              5.14.0-503.14.1.el9_5                                          @baseos
kernel.x86_64                                                              5.14.0-503.22.1.el9_5                                          @baseos
kernel-core.x86_64                                                         5.14.0-503.14.1.el9_5                                          @baseos
kernel-core.x86_64                                                         5.14.0-503.22.1.el9_5                                          @baseos
kernel-modules.x86_64                                                      5.14.0-503.14.1.el9_5                                          @baseos
kernel-modules.x86_64                                                      5.14.0-503.22.1.el9_5                                          @baseos
kernel-modules-core.x86_64                                                 5.14.0-503.14.1.el9_5                                          @baseos
kernel-modules-core.x86_64                                                 5.14.0-503.22.1.el9_5                                          @baseos
kernel-tools.x86_64                                                        5.14.0-503.22.1.el9_5                                          @baseos
kernel-tools-libs.x86_64                                                   5.14.0-503.22.1.el9_5                                          @baseos
Available Packages
kernel.x86_64                                                              5.14.0-503.23.2.el9_5                                          baseos
kernel-abi-stablelists.noarch                                              5.14.0-503.23.2.el9_5                                          baseos
kernel-core.x86_64                                                         5.14.0-503.23.2.el9_5                                          baseos
kernel-debug.x86_64                                                        5.14.0-503.23.2.el9_5                                          baseos
kernel-debug-core.x86_64                                                   5.14.0-503.23.2.el9_5                                          baseos
kernel-debug-devel.x86_64                                                  5.14.0-503.23.2.el9_5                                          appstream
kernel-debug-devel-matched.x86_64                                          5.14.0-503.23.2.el9_5                                          appstream
kernel-debug-modules.x86_64                                                5.14.0-503.23.2.el9_5                                          baseos
kernel-debug-modules-core.x86_64                                           5.14.0-503.23.2.el9_5                                          baseos
kernel-debug-modules-extra.x86_64                                          5.14.0-503.23.2.el9_5                                          baseos
kernel-debug-uki-virt.x86_64                                               5.14.0-503.23.2.el9_5                                          baseos
kernel-devel.x86_64                                                        5.14.0-503.23.2.el9_5                                          appstream
kernel-devel-matched.x86_64                                                5.14.0-503.23.2.el9_5                                          appstream
kernel-doc.noarch                                                          5.14.0-503.23.2.el9_5                                          appstream
kernel-headers.x86_64                                                      5.14.0-503.23.2.el9_5                                          appstream
kernel-modules.x86_64                                                      5.14.0-503.23.2.el9_5                                          baseos
kernel-modules-core.x86_64                                                 5.14.0-503.23.2.el9_5                                          baseos
kernel-modules-extra.x86_64                                                5.14.0-503.23.2.el9_5                                          baseos
kernel-rpm-macros.noarch                                                   185-13.el9                                                     appstream
kernel-srpm-macros.noarch                                                  1.0-13.el9                                                     appstream
kernel-tools.x86_64                                                        5.14.0-503.23.2.el9_5                                          baseos
kernel-tools-libs.x86_64                                                   5.14.0-503.23.2.el9_5                                          baseos
kernel-uki-virt.x86_64                                                     5.14.0-503.23.2.el9_5                                          baseos
kernel-uki-virt-addons.x86_64                                              5.14.0-503.23.2.el9_5                                          baseos
kernelshark.x86_64                                                         1:1.2-10.el9                                                   appstream
```

#### 最新カーネルインストール
バージョンを指定しない場合、最新バージョンでインストールします。

カーネルをインストールすると依存関係パッケージである**kernel-core**と **kernel-modules**も一緒にインストールします。

```
[root@rocky95 ~]# dnf install kernel
Last metadata expiration check: 0:00:47 ago on Wed 19 Feb 2025 02:26:50 PM KST.
Dependencies resolved.
===================================================================================================================================================
 Package                                  Architecture                Version                                    Repository                   Size
===================================================================================================================================================
Installing:
 kernel                                   x86_64                      5.14.0-503.23.2.el9_5                      baseos                      2.0 M
 kernel-core                              x86_64                      5.14.0-503.23.2.el9_5                      baseos                       18 M
Installing dependencies:
 kernel-modules                           x86_64                      5.14.0-503.23.2.el9_5                      baseos                       36 M
 kernel-modules-core                      x86_64                      5.14.0-503.23.2.el9_5                      baseos                       30 M

Transaction Summary
===================================================================================================================================================
Install  4 Packages

Total download size: 86 M
Installed size: 126 M
Is this ok [y/N]: y
Downloading Packages:
(1/4): kernel-core-5.14.0-503.23.2.el9_5.x86_64.rpm                                                                 14 MB/s |  18 MB     00:01
(2/4): kernel-5.14.0-503.23.2.el9_5.x86_64.rpm                                                                      32 MB/s | 2.0 MB     00:00
(3/4): kernel-modules-core-5.14.0-503.23.2.el9_5.x86_64.rpm                                                         14 MB/s |  30 MB     00:02
(4/4): kernel-modules-5.14.0-503.23.2.el9_5.x86_64.rpm                                                              12 MB/s |  36 MB     00:03
---------------------------------------------------------------------------------------------------------------------------------------------------
Total                                                                                                               24 MB/s |  86 MB     00:03
Running transaction check
Transaction check succeeded.
Running transaction test
Transaction test succeeded.
Running transaction
  Preparing        :                                                                                                                           1/1
  Installing       : kernel-core-5.14.0-503.23.2.el9_5.x86_64                                                                                  1/4
  Running scriptlet: kernel-core-5.14.0-503.23.2.el9_5.x86_64                                                                                  1/4
  Installing       : kernel-modules-core-5.14.0-503.23.2.el9_5.x86_64                                                                          2/4
  Installing       : kernel-modules-5.14.0-503.23.2.el9_5.x86_64                                                                               3/4
  Running scriptlet: kernel-modules-5.14.0-503.23.2.el9_5.x86_64                                                                               3/4
  Installing       : kernel-5.14.0-503.23.2.el9_5.x86_64                                                                                       4/4
  Running scriptlet: kernel-core-5.14.0-503.23.2.el9_5.x86_64                                                                                  4/4
  Running scriptlet: kernel-modules-core-5.14.0-503.23.2.el9_5.x86_64                                                                          4/4
  Running scriptlet: kernel-modules-5.14.0-503.23.2.el9_5.x86_64                                                                               4/4
  Running scriptlet: kernel-5.14.0-503.23.2.el9_5.x86_64                                                                                       4/4
  Verifying        : kernel-modules-core-5.14.0-503.23.2.el9_5.x86_64                                                                          1/4
  Verifying        : kernel-modules-5.14.0-503.23.2.el9_5.x86_64                                                                               2/4
  Verifying        : kernel-core-5.14.0-503.23.2.el9_5.x86_64                                                                                  3/4
  Verifying        : kernel-5.14.0-503.23.2.el9_5.x86_64                                                                                       4/4

Installed:
  kernel-5.14.0-503.23.2.el9_5.x86_64                  kernel-core-5.14.0-503.23.2.el9_5.x86_64     kernel-modules-5.14.0-503.23.2.el9_5.x86_64
  kernel-modules-core-5.14.0-503.23.2.el9_5.x86_64

Complete!
```



#### パッケージのインストール確認

カーネルパッケージが正常にインストールされたか確認します。

```
[root@rocky95 ~]# dnf list installed | grep -i "5.14.0-503.23.2"
kernel.x86_64                          5.14.0-503.23.2.el9_5          @baseos
kernel-core.x86_64                     5.14.0-503.23.2.el9_5          @baseos
kernel-modules.x86_64                  5.14.0-503.23.2.el9_5          @baseos
kernel-modules-core.x86_64             5.14.0-503.23.2.el9_5          @baseos
```

### OS再起動

カーネルアップデートを適用するためにOSを再起動します。

```
[root@rocky95 ~]# sync; reboot
```

### <span style="color:#e11d21;">**[選択]**</span> GRUB2ブートローダの設定ファイル作成
システムのブートメニューをアップデートして、新しくインストールされたカーネルやその他のブート項目を反映します。

dnf, yumは自動的にGRUB2設定ファイルをアップデートします。

```
[root@rocky95 ~]# grub2-mkconfig -o /etc/grub2.cfg
```

#### カーネルアップデート確認

カーネルバージョンが正常にアップデートされたか確認します。

```
[root@rocky810 ~]# uname -r
4.18.0-553.16.1.el8_10.x86_64
```


### カーネル起動順序の変更

複数のカーネルがインストールされている場合、希望のカーネルで起動できるように起動順序を変更します。

##### 基本カーネルの確認

現在基本設定されているカーネルを確認します。

```
[root@rocky95 ~]# grubby --default-kernel
/boot/vmlinuz-5.14.0-503.22.1.el9_5.x86_64
```

##### 現在インストールされているカーネルリスト

現在インストールされているカーネルリストを確認します。

```
[root@rocky95 ~]# grubby --info=ALL | grep ^kernel | grep -v rescue
kernel="/boot/vmlinuz-5.14.0-503.14.1.el9_5.x86_64"
kernel="/boot/vmlinuz-5.14.0-503.22.1.el9_5.x86_64"
```

##### 基本カーネル変更

現在インストールされているカーネルリストの1つを選択して基本カーネルを変更します。

```
[root@rocky95 ~]# grubby --set-default="/boot/vmlinuz-5.14.0-503.14.1.el9_5.x86_64"
The default is /boot/loader/entries/858382f092494811bf89e090de079ab1-5.14.0-503.14.1.el9_5.x86_64.conf with index 0 and kernel /boot/vmlinuz-5.14.0-503.14.1.el9_5.x86_64
[root@rocky95 ~]# grubby --default-kernel
/boot/vmlinuz-5.14.0-503.14.1.el9_5.x86_64
[root@rocky95 ~]# sync; reboot
```

##### OS再起動

起動順序の変更を適用するためにOSを再起動します。

```
[root@rocky810 ~]# sync; reboot
```
